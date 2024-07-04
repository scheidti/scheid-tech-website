# scheid.tech Website

Repository for my personal blog website, powered by [Hugo](https://gohugo.io/) - a fast and modern static site generator.

## Getting Started

To run this blog locally, you'll need to have Hugo installed on your machine. If you haven't installed Hugo yet, follow the instructions on the [official Hugo website](https://gohugo.io/getting-started/installing/).

### Installation

1. Clone the repository to your local machine:

```bash
git clone https://github.com/scheidti/scheid-tech-website.git
cd scheid-tech-website
```

2. Start the Hugo server:

```bash
hugo server
```

This will start the Hugo server locally. Now, you can open your browser and navigate to `http://localhost:1313` to see the blog running.

### Generate icon font

To generate an icon font from SVG files located in the project, follow these steps. This process uses Docker to run `fontcustom`, a tool that compiles SVGs into font files, ensuring you don't need to install any additional dependencies on your system.

```bash
cd ./assets/icons
docker run -i -t -v $(pwd):/app/project telor/fontcustom-worker fontcustom compile -n icons
```

This process will generate font files and a CSS file in the specified output directory, which you can then include in your web project to use the icons.

## Licence

The content of this project itself is licensed under the [Creative Commons Attribution 4.0 International](https://creativecommons.org/licenses/by/4.0/) license, and the underlying source code used to format and display that content is licensed under the [MIT license](./LICENCE).

### Icons

The icons in the assets directory are from [Pictogrammers.com](https://pictogrammers.com/library/mdi/) and licensed under the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt).