---
title: "Git, GitHub pages and Zenodo"
---

## Committing changes

In order to create commits, we need to make sure our directory is a git repository. We can check this in any directory by using the terminal. Make your project directory your current directory with `cd` (if you click the **Terminal** tab in Rstudio you will directly in the project directory) and type:

``` bash
git status
```

If your directory is not set up for git it will return something like:

```         
fatal: not a git repository (or any of the parent directories): .git
```

Otherwise, it will give you an overview of files that are or can be staged and committed.

If you followed the instructions, a git repository has been initiated upon creating the project, so you should be able to stage and commit your files.

::: callout-note
If your project directory isn't a git repository yet, initialize it with

``` bash
git init 
```
:::

::: callout-important
## Exercise

Check out the untracked files with `git status` do all these files need to be version-controlled? If not, add the files/directories you don't want to version control to the `.gitignore`
:::

::: {.callout-tip collapse="true"}
## Answer

The directory `_site/` contains the rendered html files. These aren't source files, so don't have to be version controlled. We can add it to the `.gitignore`, which would look like:

``` gitignore
.Rproj.user
.Rhistory
.RData
.Ruserdata

/.quarto/
_site/
```
:::

Now that we're all set for our first commit we can start by staging the files, we could do that one-by-one by typing e.g. `git add index.qmd` and so on. To stage it and start tracking it. However, to stage all changed and untracked files we can use:

``` bash
git add --all
```

::: callout-note
If you want to check beforehand what you will stage, you can do a 'dry run' with:

``` bash
git add --all -n
```

And if you made a mistake and want to unstage a file:

``` bash
git rm --staged file.txt
```
:::

The command `git status` will now return all files in green, which that they are staged. The next step will be the commit itself:

``` bash
git commit -m "first commit"
```

::: callout-note
The more descriptive you make your commits, the easier it is to find back a commit if you e.g. want to undo a change. It is generally advised to write in a tense that describes what the commit does, so e.g.

-   Fixes bug
-   Adds image to main page
-   Adds gene expression heatmap
:::

## Using a remote repository

::: {.callout-warning}
This sub chapter assumes that you can connect with GitHub through https. If you haven't setup a PAT yet, follow the instructions [here](https://happygitwithr.com/https-pat.html) as stated in the [course preparations](course_preparations.qmd).
:::

We will now link our local repository to a remote repository on GitHub. First, we need to create an empty repository on GitHub. Do this by:

-   Navigating to the [GitHub website](https://github.com) and login
-   Click the `+` sign at the top right and click **New repository**
-   Fill out the form:
    -   Choose a repository name. Typically this is the same name as your project directory
    -   We will host a website, so make sure it's public
    -   Leave the rest unchecked, so no README file and no `.gitignore` (these are already in our local repository)
-   Click the green button with `Create repository`

After you've created the empty repository, GitHub is already giving you suggestions what to do. We've already created the commit, and our current branch is the main branch (check it with `git branch`). 

::: {.callout-caution}
If running `git branch` does not return `main` (if so, it's likely to return `master`), change the branch name with:

```sh
git branch -m main
```
:::

Now, the only thing we need to run is:

``` bash
git remote add origin https://github.com/USER/REPOSITORY.git
```

Which tells the local repository to create a new remote connection called `origin` and to connect to the repository url. After that, we'll synchronize with the remote repository with:

``` bash
git push -u origin main
```

Which pushes the `main` branch from the local repository to the remote (named `origin`). The option `-u` makes sure that the `main` branch is used as default branch on the remote repository.

To publish your site, you only need to run two commands. To make sure `_site` contains the latest version of your website:

``` bash
quarto render
```

To create (if it doesn't exist) a branch named `gh-pages` push the website files there, and tell GitHub that the website will be there:

``` bash
quarto publish gh-pages
```

🤝 congrats! You've just published your first website with an analysis report!

## Store it in Zenodo

To store your repository in Zenodo:

- Browse to [zenodo.org](https://zenodo.org) and sign up and use your GitHub account. 
- On the top right, click the drop-down menu and select *GitHub*. 
- All the public repositories in your namespace are in the list. Move the switch at the repository you have just created.
- Now, go to your repository on GitHub and create a release. Do this by selecting *Create a new release* on the right side. 
- Choose a tag that starts with `v` and follows the rules of [semantic versioning](https://semver.org/), so a good first name would be e.g. `v1.0.0`. 
- Choose a release title. This can be anything. The release title of R v4.2.0 is for example `Vigorous Calisthenics`. 
- Click the green button **Publish release**
- Go back to the [GitHub page on Zenodo](https://zenodo.org/account/settings/github/) and click *Sync now* at the top right. 
- There should now be a green bar at your repository, and it received a DOI. You can check out the record by clicking on the repository name

::: {.callout-important}
## Exercise

Off course we want to show off with the DOI we just assigned. In order to do that, click the DOI badge and copy-paste the markdown syntax to `index.qmd` to show it on the main page of your website. Now, stage, commit and push the change. After that, render and publish the website. 
:::

::: {.callout-tip collapse="true"}
## Answer

An example of the top of `index.qmd`:

```markdown
---
title: "quarto-tutorial"
---

[![DOI](https://zenodo.org/badge/657627364.svg)](https://zenodo.org/badge/latestdoi/657627364)
```

After that, we run the commands:

```bash
git add index.qmd
git commit -m "Adds a badge to the website"
git push
```

And the quarto commands:

```bash
quarto render
quarto publish gh-pages
```

:::


