+++
title = "MinIO als Artifact Store in ZenML konfigurieren"
description = "Hier wird gezeigt, wie sich MinIO als Artifact Store in ZenML konfigurieren lässt und in der MLOps-Pipeline als Speicherort für Artefakte wie Datasets und KI-Modelle genutzt werden kann."
date = "2025-08-03"
images = []
tags = ["ZenML", "MinIO", "S3", "MLOps", "AI"]
summary = "Hier wird gezeigt, wie sich MinIO als Artifact Store in ZenML konfigurieren lässt und in der MLOps-Pipeline als Speicherort für Artefakte wie Datasets und KI-Modelle genutzt werden kann."
outputs = ["html", "markdown"]
[sitemap]
  changefreq = "monthly"
  priority = 0.5
+++

Hier wird gezeigt, wie sich [MinIO](https://www.min.io/) als Artifact Store in [ZenML](https://www.zenml.io/) einrichten lässt und in der MLOps-Pipeline als Speicherort für Artefakte wie Datasets und KI-Modelle genutzt werden kann.

## Voraussetzungen

- ZenML-Instanz und Python-Projekt ([Dokumentation](https://docs.zenml.io/getting-started/installation))
- MinIO-Server
- [MinIO-Client](https://docs.min.io/community/minio-object-store/reference/minio-mc.html) (mc)

## Schritte

1. Erstelle mit dem MinIO Client einen neuen Bucket und zugehörigen Access Key:
   ```bash
   # Server-Aliase auflisten
   mc alias list

   # Neuen Alias erstellen (falls noch nicht vorhanden)
   mc alias set myserver https://s3.yourserver.com admin_username admin_password

   # Einen neuen Bucket namens "zenml" erstellen
   mc mb myserver/zenml

   # Vorhandene Policies anzeigen
   mc admin policy list myserver

   # Neuen Access Key für Benutzer erstellen (wird in ZenML verwendet)
   mc admin accesskey create myserver admin_username --policy readwrite
   ```

2. Registriere den neuen Bucket als Artifact Store in ZenML:

   ```bash
   # Bei ZenML-Instanz anmelden
   zenml login https://zenml.yourserver.com

   # Die ZenML S3-Integration installieren (falls nicht vorhanden)
   zenml integration install s3

   # ZenML-Secret erstellen
   zenml secret create minio_secret \
       --aws_access_key_id='your_generated_access_key' \
       --aws_secret_access_key='your_generated_secret_key'

   # ZenML Artifact Store registrieren
   zenml artifact-store register minio_store -f s3 \
       --path='s3://zenml' \
       --authentication_secret=minio_secret \
       --client_kwargs='{"endpoint_url":"https://s3.yourserver.com","region_name":"your-region"}'
   ```

3. Verwende den neuen Artifact Store in deinem aktuellen ZenML-Stack:

   ```bash
   zenml stack update -a minio_store
   ```