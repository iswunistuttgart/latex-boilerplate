# LaTeX GitLab Base Project

This project can be used to kick-start a LaTeX-based, git(lab)-powered project.

It is based on many many years of experience from writing LaTeX files and changing the compilation environment.
That also implies it may not be the right thing for you if you're a starter, but it's worth knowing that this project exists ;)

By default, this project gives you the following file and directory structure for your project

* `algorithms/`
* `animations/`
* `appendix/`
* `config/`
* `content/`
* `data/`
* `glossaries/`
* `figures/`
* `images/`
* `listings/`
* `tables/`
* `.latexmkrc-win`
* `.latexmkrc-macos`
* `.gitignore`
* `.gitlab-ci.yml`

Further down, the use of each file/directory will be explained.

What you should keep in mind is that I like to keep my main tex file as empty as possible.
In most cases you will only find some `\input`s or `\include`s inside the main `.tex` file.
Every actual content is contained within subdirectories.

Want to read some advices on writing good LaTeX documents? Check out this [advice for writing LaTeX documents](https://github.com/dspinellis/latex-advice)

## How To

If you want to create a LaTeX document and want to use this boilerplate, simply clone the project and get going:

```bash
git clone https://git.isw.uni-stuttgart.de/projekte/eigenentwicklungen/templates/latex-git-project.git "LaTeX Document"
cd "LaTeX Document"
```

Since your git project now is a clone off of this boilerplate, your origin will point there.
That's not what we want, but we might want to keep the remote to incorporate upstream changes.
As such, we will need to rename the origin remote to upstream and add our own origin remote:

```bash
git remote rename origin upstream
git remote add origin <your new origin URL>
```

That's it, now you can start writing your document, keep track of upstream changes, while also version control your actual document.
How neat is that?

## directories

### algorithms

You are writing a code-heavy document and want to provide pseudo-code or code excerpts?
It's always a good idea to keep content in separate files rather than all within your main document.
This would be the folder for you to keep all algorithm or code snippets in that you may later include using package `minted`, or packages `algorithm` together with `algpseudocode`.

### animations

After having found out, how to create animations using the `animate` package that is able to render both TikZ and psplots as animations, I added this directory.
It is very similar to the `figures/` directory as it is supposed to keep the raw animation files - not their data, this'll belong into `data/`.
Using the `animations/` directory, you can move your animations into standalone files that contain all logic to render the animation, allowing you to have reusable code.


### appendix

As the name suggests, content that is not within your main document but seen as *appended to it* goes into this directory.
Speaking LaTeX, there's actually no difference if you put your appendix into the main `content/` directory (or `content/appendix/`), but I feel your directory is going to be looking cleaner if you physically have actual content separate from your appendix.


### config

Use this directory to put any configuration of your document into.
Be it additional packages you need to load, configuration of loaded packages like `siunitx`, layout adjustments or macro/environment definitions.

If you prefer, keep your different configuration files separate i.e., have one for loading packages, one for macro definitions, one for environment definitions, one for loading TikZ libraries.
You get the idea.

### content

As the name suggests, all your content `tex`-files should go in here.
This is mostly useful when your document organically grows (like a thesis) and you only want to compile the currently edited text.
Putting all your content into the main `tex`-file will be a pain in the long run.
Just want to compile your abstract or introduction? Simply comment out the other contents' `\input` and compile your document.
Simple as that.

How you further break down your content (I'd suggest creating files on a `\subsection` level) is up to you.
Also, prefixing or suffixing your file with the section number might be your thing - but keep in mind that you might rearrange sections in your document and that would require you to also change the filename of the corresponding `tex` files.

### data

It is preferable to have graphs (2D or 3D) available as `tikz` files as they will be rendered as vector graphics and will also pick up the environment's style (font family, font size, font weight, etc.).
To keep markup from content separate (as LaTeX does), we should keep the markup of our figures separate from the data.
Thus, the data used to generate a plot will be put into the `data` directory, while the `tikz` markup is located inside the `figures` directory (see below).
It is recommended to keep the same naming convention inside the `data` and `figures` directory.

### figures

You might ask yourself "why is there a `figures/` and an `images/` directory? Isn't everything a figure at its core?" and you are technically right.
However, I prefer to think of figures as anything that is a data-based plot or generated from actual text (for my documents, this mostly implies TikZ/pgfplots).
This directory contains all these `tikzpicture`s.
Within the `figures/` directory, you can throw everything in at the top level or add additional sublevel directories to split by sections or content.

### glossaries

Any good document requires some glossaries be it acronyms, notation, or a list of symbols used.
The most powerful package is `glossaries`, yet there's nothing more important than a powerful directory hierarchy.
I prefer to have my glossaries in a separate directory rather than within my content directory.
Thus, you may also want to throw all your glossaries into this directory.

### images

In contrast to figures, images are all documents that can be thought of as being pre-compiled.
This includes anything from `png`, `jpeg`, `pdf` to `eps` or `ps` files.
Whenever the object to be drawn does not consist of textual markup, it is considered an image and will be kept in the `images/` directory.

### listings

Some documents may also include source code listings.
These floating environments should be treated much like any other floating environment: separate markup from content.
Your markup of a listing goes into the corresponding `tex`-file inside the `content/` directory while the actual content/data goes into the corresponding `tex`-file inside the `listings/` directory.

### tables

Another floating environment in LaTeX are tables.
These can be quite long at times and take up space in your actual document.
Think of it in the same way as you do for figures/images: you do not copy them into the document but actually include these figures via `\includegraphics{figures/…}`.
Why not do this with tables, too? Also, this allows re-using tables in multiple documents as you only need to copy the table data in one single file and won't ever have to skim through your document finding the table.

### videos

This folder will most likely only be necessary for presentations typeset with LaTeX, however, you may abuse your document PDF to include a video, too.
This folder may be used to store videos (sidenote: think about enabling git LFS on your repository and on files in this directoyr — or for that matter for `*.mp4` or other video formats).

## Files

### .latexmkrc-*

Setting up your LaTeX build environment correctly can be a challenge.
If you are using any of the known IDEs like TeXmaker, TeXstudio, TeXworks, etc., there's many options to configure.
These options can be very simple to set in case you are really just writing a plain document.
However, as soon as you need to add some references/bibliography or want to add complex TikZ/pgfplots, compilation gets more complicated.
Wouldn't it be cool to just have to run one command and wait for everything to compile as needed? C/C++, for example, has some thing called `makefile` that takes care of exactly that, so where's a similar thing for LaTeX? It's inside your `.latexmkrc` file in conjunction with `latexmk`.
If you have a `.latexmkrc` in your project's root directory (and it's configured well), you can simply run `latexmk` inside the project directory from your command window, terminal, git bash, power shell, etc.

Depending on your OS, you want to copy the respective `.latexmkrc-*` file to `.latexmkrc` and start using it.
The main difference is the previewer being set differently on Windows and on macOS: on Windows, `sumatraPDF` is being used by default while macOS has `Previewer.app` set as default.

### .gitignore

It is very important to exclude the right files when working with a LaTeX document.
Since it is quite a repetitive and cumbersome task (even with [gitignore.io](gitignore.io)), we provide a basic `.gitignore` that's geared towards both Windows and UNIX users.
If you are a power LaTeX user, you might want to take a look at the `.gitignore` file and see if it works for your or possibly excludes files that you don't want excluded.

The provided `.gitignore` file contains the following ignore definitions from [gitignore.io](gitignore.io):

* archives
* cvs
* latex
* linux
* osx
* sublimetext
* svn
* tags
* tex
* windows

### .gitlab-ci.yml

A file-repository should generally only consist of the files needed to generate a certain file/document/binary etc.
If transferred to LaTeX this means you wouldn't want your generated `pdf` file tracked - it's a dirty binary file, so it's a no go tracking it.
But then again, everyone wanting to look at your document would need to clone the repository, run `latexmk .` and wait till the final document is generated.
Annoying, right? Luckily there's ways to make this unnecessary by using a `.gitlab-ci.yml` file with GitLab's Continuous Integration (CI) enabled.
If you have correctly configured your GitLab project, you can simply push changes of your files to the remote and it will automatically build the `pdf` for you.
It'll even alert you in case it fails building the file.
How neat is that?

You will need two things for this auto-magic to work: a `.gitlab-ci.yml` file (:checkmark: can be found in this project) and a correctly configure GitLab project.
The latter really is quite simple - just head over to the [GitLab help pages](https://gitlab.com/help/ci/enable_or_disable_ci.md) and follow the step(s).

The provided `.gitlab-ci.yml` is configured to use a docker image of texlive 2018 which ships the most recent version of LaTeX on an ubuntu system.
Besides texlive-2018, `perl`, `ghostscript`, `imagemagick`, and `python-pygments` is installed.
Using pgfplots may require having ghostscript available while wanting to add code listings using the *highly recommended* `minted` package requires `python` and `python-pygments`.
When GitLab CI is enabled and the `.gitlab-ci.yml` is added to the repo, there is only one job being performed: `compile_pdf`.
This job downloads previously mentioned docker image and starts it.
Then, the most recent version of the repository is checked out (in the branch you pushed to) and `latexmk` is executed.
Since this picks up the filename to compile from the `.latexmkrc` file, you ought to make sure that your `tex` file is named accordingly.
The `tex` file used by `latexmk` ought to be called `source.tex` - but you can also change the corresponding line in either `.latexmkrc-macos` or `.latexmkrc-win`.
After successful compilation (you should get an email if your compilation failed showing the error) the compiled files (artifacts, as GitLab calls them) are taken from the root directory matching the file extension `*.pdf`.
The artifacts name is composed of GitLab's project slug and the commit SHA.


## HowTos

* [Typesetting of MATLAB data in LaTeX documents](howtos/matlab-array-typesetting.md)


## Roadmap

[ ] Add support for TikZ/PGF. Not sure how exactly, but maybe at least a simple `externalization` script of some sort


## Further reading

If you need more information, especially on the provided dot-files, I suggest first looking into the files and then looking at the following list of links (in no particular order):

  * [Typesetting beautiful](http://www.inf.ethz.ch/personal/markusp/teaching/guides/guide-tables.pdf) tables [and another](https://tex.stackexchange.com/questions/112343/beautiful-table-samples)
  * [Typesetting mathematics for science and technology according to ISO 31-XI](https://www.tug.org/TUGboat/Articles/tb18-1/tb54becc.pdf)
  * [Is a period after an abbreviation the same as an end of sentence period?](https://tex.stackexchange.com/questions/2229/is-a-period-after-an-abbreviation-the-same-as-an-end-of-sentence-period)
  * [Making pretty graphs in MATLAB](https://blogs.mathworks.com/loren/2007/12/11/making-pretty-graphs/) and then [exporting these figures nicely](https://de.mathworks.com/matlabcentral/fileexchange/23629-export-fig)
  * [Export MATLAB figures to TikZ](https://github.com/matlab2tikz/matlab2tikz) (the best way for proper, publication- and camera-ready figures)
  * [Mathematical Typesetting with LaTeX](https://www.tug.org/~hvoss/PDF/mathmode.pdf) by Herbert Voß
  * [Writing Scientific Documents Using LaTeX](ftp://ftp.dante.de/tex-archive/info/intro-scientific/scidoc.pdf)
  * [American Mathematical Society's (amsmath) Short Math Guide for LaTeX](ftp://ftp.ams.org/pub/tex/doc/amsmath/short-math-guide.pdf)
  * [An essential guide to LaTeX2e usage](http://ctan.math.illinois.edu/info/l2tabu/english/l2tabuen.pdf)
