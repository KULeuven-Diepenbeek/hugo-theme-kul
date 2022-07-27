

# Hugo KU Leuven Campus Diepenbeek Theme

This repository contains a theme for [Hugo](https://gohugo.io/), based on [hugo-theme-learn](https://learn.netlify.app/en/), which in turn is based on [Grav Learn Theme](https://learn.getgrav.org/).

Take a look at the theme in action:

- https://kuleuven-diepenbeek.github.io/ses-course/
- https://kuleuven-diepenbeek.github.io/appdev-course/
- https://kuleuven-diepenbeek.github.io/osc-course/
- ...


## Installation

**Pre-requirement**: you'll need at least Hugo `0.8+` (the **extended** edition). Download at https://gohugo.io/getting-started/quick-start/ 

Add the Hugo binary to your local `$PATH` (e.g. `/usr/local/bin`). 

If you create a **NEW** course, in the root of your course repository folder:


```shell
git submodule add https://github.com/KULeuven-Diepenbeek/hugo-theme-kul themes/hugo-theme-kul/
```

This will create a folder `hugo-theme-kul` in `themes`.

If you checkout an **existing** course with a submodule, execute the following:

```shell
git submodule sync
git submodule update --init --recursive
```

This will fill the folder `hugo-theme-kul` which is otherwise empty.


To select the theme, in your `config.toml`, set `theme = "hugo-theme-kul"`. Menus and other options can be configured there as well. See the [hugo-theme-learn documentation](https://learn.netlify.app/en/)


Here's a quick video on how to get it up and running, including creating a new Hugo website:

[![](https://cdn.loom.com/sessions/thumbnails/bc09f573e76c48af90c6843144f011e2-with-play.gif)](https://www.loom.com/share/bc09f573e76c48af90c6843144f011e2)

In short, from scratch:

1. `hugo new site mycourse`
2. `cd mycourse && git init`
3. `git submodule add https://github.com/KULeuven-Diepenbeek/hugo-theme-kul themes/hugo-theme-kul/`
4. Edit the `config.toml` file to add `theme = 'hugo-theme-kul'`
5. Add your contents in the `/contents` folder and test at `http://localhost:1313` with `hugo serve`!
6. Generate HTML files: `hugo` -- output in `/docs`. Upload wherever you want. 


## Usage


See the [hugo-theme-learn documentation](https://learn.netlify.app/en/). Everything available in that theme is also present here. 

Also included: [Mathjax Support](https://www.mathjax.org/) out-of-the-box.

### Notice blocks

![](/images/noticeblock.jpg)

Different options in `notice` possible: warning, info, ...

### Code blocks

It is possible to provide multiple blocks to create a switcher. The following code:

```
<div class="devselect">

`kt
class ArnieLookalike : IllBeBack {
    override fun doBackFlip(): Boolean = false
}

class StuntmanArnie : IllBeBack {
    override fun doBackFlip(): Boolean = true
}
`

`java
public class ArnieLookalike implements IllBeBack {
    @Override
    public boolean doBackFlip() {
        return false;
    }
}

public class StuntmanArnie implements IllBeBack {
    @Override
    public boolean doBackFlip() {
        return true;
    }
}
`

</div>
```

(Where one backtick equals three) produces the following block:

![](/images/codeblock.jpg)

### Structured blocks (Mermaid)


See https://mermaid-js.github.io/mermaid/#/ for syntax and documentation. This is seamlessly integrated into the theme.

The following mermaid block:

```
{{<mermaid>}}
graph LR;
    A{*age} -->|after first assignment| B[young<br/>10]
    A -.-> |after second assignment| C[old<br/>80]
{{< /mermaid >}}
```

Produces the following visualization:

![](/images/mermaid.jpg)


### Generating a ONE-PAGE PDF

1. Install the `website2pdf` node plugin (`npm install website2pdf`)
2. Run the local Hugo server (`hugo serve`)
3. In a separate terminal, run the website2pdf tool by providing the correct sitemap URL: `npx website2pdf --sitemapUrl "http://localhost:1313/appdev-course/sitemap.xml`
4. Paste every generated PDF file together using ImageMagick: `convert -density 100 $(ls *pdf) ../output.pdf`.

Done!


## License

Licensed under MIT, the same license as the theme is built upon. See `LICENSE.md`.


## Auto-committing generated HTML files


You can use GitHub workflows to kickstart Hugo to generate and commit the `/docs` folder in case you employ GitHub pages as the course homepage. 

This is an example workflow, adapted from the "appdev-course" repository. Save in `.github/workflows/main.yml`:

```
name: CI

    branches: [ main ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
        with:
          submodules: recursive

      - name: Generate hugo
        run: |
          cd $GITHUB_WORKSPACE
          ./hugo

      - name: commit generated hugo stuff
        uses: stefanzweifel/git-auto-commit-action@v4
        with:
          commit_message: Hugo regen
```

This requires you to **Check in the "hugo" Linux x65 binary** into the root (or otherwise) folder of your repository! 
