+++
title = "How to Set Up MinIO as an Artifact Store for ZenML"
description = "This blog post shows how to configure MinIO as an artifact store in ZenML — a data persistence layer for storing artifacts like datasets and models."
date = "2025-08-03"
images = []
tags = ["ZenML", "MinIO", "S3", "MLOps", "AI"]
summary = "This blog post walks you through configuring MinIO as an artifact store in ZenML. The artifact store is a core component of any MLOps stack, acting as a data persistence layer where artifacts such as datasets and models are stored."
outputs = ["html", "markdown"]
[sitemap]
  changefreq = "monthly"
  priority = 0.5
+++

This blog post shows how to configure [MinIO](https://www.min.io/) as an artifact store in [ZenML](https://www.zenml.io/) — a data persistence layer for storing artifacts like datasets and models.

## Requirements

- ZenML instance and Python project (see the [docs](https://docs.zenml.io/getting-started/installation))
- MinIO server
- [MinIO client](https://docs.min.io/community/minio-object-store/reference/minio-mc.html) (mc)

## Steps

1. Create a new bucket and access key in MinIO using the MinIO Client:
   ```bash
   # List existing server aliases
   mc alias list

   # Create a new alias (if it doesn't exist)
   mc alias set myserver https://s3.yourserver.com admin_username admin_password

   # Create a new bucket named "zenml"
   mc mb myserver/zenml

   # Show available policies
   mc admin policy list myserver

   # Create a new access key for your user (used in ZenML)
   mc admin accesskey create myserver admin_username --policy readwrite
   ```

2. Register the new bucket as an artifact store in ZenML:

   ```bash
   # Log in to your ZenML instance
   zenml login https://zenml.yourserver.com

   # Install the ZenML S3 integration (if not already installed)
   zenml integration install s3

   # Create a ZenML secret
   zenml secret create minio_secret \
       --aws_access_key_id='your_generated_access_key' \
       --aws_secret_access_key='your_generated_secret_key'

   # Register a ZenML artifact store
   zenml artifact-store register minio_store -f s3 \
       --path='s3://zenml' \
       --authentication_secret=minio_secret \
       --client_kwargs='{"endpoint_url":"https://s3.yourserver.com","region_name":"your-region"}'
   ```

3. Use the new artifact store in your current ZenML stack:

   ```bash
   zenml stack update -a minio_store
   ```