# Obsidian LaTeX Injection

This is a quick hack of [a quick write-up](https://github.com/thequilo/obsidian-tikz-renderer) of a possible way to render tikz graphics in Obsidian, enabling arbitrary LaTeX code injection.

:warning: This is an experimental plugin! Things may change and break at any time!

## Usage

Code blocks marked by `latex injection` as their language line will have their contents rendered by `pdflatex` and converted to a PNG image by ImageMagick's `convert`. The 'injection' keyword can be replaced by any of the following: `inject`, `render`, `compile`, `run`, `load`.

### Preamble

This plugin supports user-defined preambles. Simply add your preample in this plugin's settings. Here is the preamble that I use:
```latex
\input{/home/alexander/Latex/preamble-obsidian}
```

### Example

The following LaTeX code with some user-defined macros from the imported preamble (e.g. `\blank`)
```latex
\begin{tikzcd}
\Hom_\c D(L\blank,d) \arrow[Rightarrow]{r}{\varphi_d} \arrow[Rightarrow]{d}[swap]{\Hom_\c D(L\blank,h)} & \Hom_\c C(\blank,Rd) \arrow[Rightarrow]{d}{\Hom_\c C(\blank,Rh)}\\
    \Hom_\c D(L\blank,d) \arrow[Rightarrow]{r}[swap]{\varphi_e} & \Hom_\c C(\blank,Re)
\end{tikzcd}
```

is rendered as follows:

![Diagram](output.png "Diagram")


## Installation

### 1. Install LaTeX and ImageMagick's `convert`

 - LaTeX: https://www.latex-project.org/get/
 - ImageMagick: use your OS's native package manager (e.g. `apt install imagemagick` on Ubuntu) or follow the instructions on https://imagemagick.org/script/download.php


### 2. install this plugin

Download the files of this repo and place them in `<path_to_your_vault>/.obsidian/plugins/obsidian-tikz-renderer`. Once that is done, go to the _Community plugins_ section in the Obsidian settings and click on the _Reload plugins_ button appearing besides the _Installed plugins_ section. Then enable the plugin as you would any other.


### 3. Set the latex command correctly

Go to the settings of the LaTeX Injection plugin and check if the render command is correct.

**Linux:** The default should work if the paths are set correctly.

**Windows:** For safety, use the full paths to the executables, e.g., `C:\texlive\2021\bin\win32\pdflatex.exe -interaction=nonstopmode -halt-on-error -shell-escape "{input}" && C:\pdf2svg-windows-1.0\dist-64bits\pdf2svg.exe input.pdf "{output}"`.



## Troubleshooting

**`Error: Command failed: pdflatex (...)`** This means either `pdflatex` failed to compile the LaTeX code or the image conversion failed. You could test your LaTeX code seperately (e.g. by running `pdflatex` directly or using an TeXstudio) or check if the image conversion method is working correctly. The standard `convert` has a security policy issue that causes it to fail, and a resolution can be found in [this StackExchange answer](https://stackoverflow.com/a/59193253/21379986).

**`Error: ENOENT: no such file or directory`** Make sure the image conversion creates `output.png` from `input.pdf`.


## Limitations

**LaTeX blocks don't resize** because the plugin just adds the PNG image. The alternative is to rely on `pdf2svg` and embed some SVG code, but each such SVG code creates a `<g>` group, so when you try to render multiple blocks this way it causes a conflict (since for some reason `<g>` declarations are shared among all `<svg>` blocks).

There are two work-arounds: output larger image files during the conversion process (e.g. using `-density 300` or greater as a `convert` parameter) or adding `\usepackage{scalefnt}` to the LaTeX preamble and inserting `\scalefont{2}` before the rest of the LaTeX code block.


## License

I'm not sure which license the [original work](https://github.com/thequilo/obsidian-tikz-renderer) falls under, so I can't safely assign a license.


## Buy me a coffee

If you spot me in Amsterdam feel free to give me a coffee :grin: