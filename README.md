# scheid.tech Website

## Generate icon font

```bash
cd ./assets/icons
docker run -i -t -v $(pwd):/app/project telor/fontcustom-worker fontcustom compile -n icons
```

## Licence

The content of this project itself is licensed under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) license, and the underlying source code used to format and display that content is licensed under the [MIT license](./LICENCE).

### Icons

The icons in the assets directory are from [Pictogrammers.com](https://pictogrammers.com/library/mdi/) and licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt).