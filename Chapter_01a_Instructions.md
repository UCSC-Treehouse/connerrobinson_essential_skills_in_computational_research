# Chapter 1a, RStudio and GitHub


In this first section of Chapter 1 you will create and work on a new
branch on GitHub, create a **Quarto document**, and have a brief
introduction to plotting. Then, you will **push** your Chapter 1a work
to a Chapter-1a branch, for your mentor’s review.

------------------------------------------------------------------------

## Prerequisites

So, where are you going to store all of this new data analysis? That’s
the job of Quarto notebooks. For each chapter you work through, you will
create a new quarto document *and* a new GitHub branch where you will
push your corresponding chapters’ work to.

**Create Your First GitHub Branch**

1.  Before you return to RStudio to create your first quarto notebook,
    you want to create a new branch on GitHub where you can push that
    chapter’s work.

- In **GitHub Desktop**, click Current Branch –\> New Branch

<img src="Images/create_new_ch_1a_branch.png" 
     alt="GitHub Desktop screen where 'Current Branch' is selected and 'New Branch' button is visible. Both are circled in red." 
     style="width: 55%;">

- Name the branch ‘chapter-01a’ and click ‘Create Branch’

<img src="Images/name_new_branch_ch_1a.png" 
     alt="GitHub Desktop screen upon creation of new branch, a name 'chapter-01a' is given and 'Create Branch' button is visible in blue" 
     style="width: 55%;">

**Create Your First Quarto Notebook:**

1.  Now, you can return to RStudio by launching the .Rproj file in your
    new directory

<img src="Images/launch_new_project.png" 
     alt="Screenshot of project folder with .Rproj file button" 
     style="width: 55%;">

2.  Install Packages and Enable Reproducibility

You have already initialized this project with renv, ensuring
consistency of your package versions, and thus consistency when sharing
or reproducing your work. renv takes and saves a “photo” or “snapshot”
of your current packages and versions and restores this exact setup when
reopening or sharing the project. You will do this step each time you
install or update a package. So, to start, let’s install three packages.

In your **R Console**, install the ‘tidyverse’, ‘palmerpenguins’, and
‘ggthemes’ packages:

``` r
# install core packages using renv
renv::install(c("tidyverse", "palmerpenguins", "ggthemes"))
```

This installation is using renv. Like many things in R, there is **more
than one way to achieve the same end goal**. ‘renv::install()’
automatically includes a snapshot of the packages and versions. But, if
you just run ‘install.packages()’, you can always run ‘renv::snapshot()’
to save the current packages and versions afterward. This is always done
in your R Console.

3.  Next, create a new Quarto document (.qmd file) where you can work on
    the examples, exercises, and type any notes you may want.

- File –\> New File –\> Quarto Document…

<img src="Images/create_new_quarto_file.png" 
     alt="RStudio screen showing button navigation to create a new quarto document. 'File', 'New file', and Quarto document...' are all circled in red."
     style="width: 55%;">

4.  Name your Quarto document

- Title file ‘Chapter 1a’
- **deselect** ‘Use visual markdown editor’
- Click ‘Create Empty Document’

<img src="Images/name_new_quarto_file.png" 
     alt="RStudio screen upon creating new quarto document: title box changed to 'Chapter 1a', 'Use visual markdown editor' is deselected, and 'Create Empty Document' button is present and circled in red."
     style="width: 55%;">

You will see something like the following…

<img src="Images/ch_1a_quarto_file.png" 
     alt="RStudio screen with newly created 'Chapter 1a' quarto document. The file is blank besides the YAML header with 'title: Chapter 1a' and 'format: html'."
     style="width: 55%;">

5.  Change the YAML Header

The section at the top of your document, enclosed by ‘—’, is called the
YAML header. Currently, it specifies ‘format: html’, which renders your
.qmd file as an HTML document. When you “render” the document, Quarto
combines your code, text, and raw data into a finished document.
Changing the header of the document to ‘format: gfm’ ensures that the
output is a markdown file. We want to render to a gfm (GitHub Flavored
Markdown), specifically, as it is optimized for viewing on GitHub.

<img src="Images/change_yaml_header.png" 
     alt="Cropped screenshot of top left corner of RStudio screen. The YAML header has the correct 'Chapter 1a' title but the output format has been changed to 'format: gfm'."
     style="width: 55%;">

6.  Load Libraries from the Installed Packages.

Installing new packages does not mean they are ready to use just yet.
Next, we need to load the libraries from the packages into our new
project. Unlike installing packages, you need to load the libraries each
new session.

**Load Libraries**

Thus far you have been working in your Console. You will now switch to
writing commands in your own **Quarto document**! But don’t worry, your
Console will still appear at the bottom of your RStudio screen. To
create a new code chunk, click the green “+C” button near the top of
your RStudio page. (**Hint**: Make sure your cursor is outside of the
YAML header!)

<img src="Images/create_code_chunk.png" 
     alt="RStudio screen navigated to the top by the green '+C' button, which is circled in red. This will create a new code chunk."
     style="width: 55%;">

(**Hint**: If you click the right side of the button, by the down arrow,
you will notice there are many types of code you can select. For the
purpose of the following examples and exercises, you will be using R.)

Copy the following command into a new code chunk to load the `tidyverse`
library.

``` r
# load tidyverse library (you will use this in a lot of your data analysis!)
library(tidyverse)
```

To run a command, click the “Run” button on the top right of your
RStudio screen, to the right of the “Insert a new code chunk” button. A
dropdown box will appear, click “Run Current Chunk” to run the
**entire** chunk.

<img src="Images/run_current_chunk.png" 
     alt="RStudio screen navigated to the top right 'Run' button and 'Run current chunk' button, both circled in red."
     style="width: 55%;">

You will see the following output:

<img src="Images/load_library_output.png" 
     alt="RStudio screen when running command to load 'library(tidyverse)' with output."
     style="width: 55%;">

Now that you know how to run an entire chunk, let’s see how to run
**selected lines** of your chunk. Copy the next two commands into a new
chunk. Select both lines, click “Run”, and “Run Selected Line(s)”.

``` r
# load palmerpenguins library (includes an example dataset)
library(palmerpenguins)
# load ggthemes library (offers colorblind safe color palette)
library(ggthemes)
```

<img src="Images/run_selected_lines.png" 
     alt="RStudio screen when running selected lines of a command."
     style="width: 55%;">

AMAZING work! Now, let’s *really* get into it.

------------------------------------------------------------------------

## Background Info

- A **data frame** is a rectangular collection of **variables** (in the
  columns) and **observations** (in the rows). In the context of
  **palmerpenguins**, a variable refers to an attribute of all the
  penguins, and an observation refers to all the attributes of a single
  penguin.
- In the **tidyverse**, we use special data frames called **tibbles**

------------------------------------------------------------------------

## Creating a ggplot

**end goal**:

<img src="Images/penguin_flipper_to_mass_ggplot2.png" 
     alt="Scatterplot showing positive correlation between penguin flipper length and body mass across three species"
     style="width: 55%;">

**To begin**: Create a plot with the function ggplot(), which you will
add **layers** to using different **arguments**. The first argument of
ggplot() is the dataset to be used in the graph: ggplot(data = penguins)
creates an empty graph that is primed to display the penguins dataset.
Run the following command (and all example and exercise commands) the
same way you ran the library commands above.

``` r
ggplot(data = penguins)
```

Now we can tell ggplot() how we want to visualize our penguins data. Our
next argument is mapping, where we define how the variables in our
dataset are mapped to visual properties (ie **aesthetics**) of the plot.
The mapping argument is always defined in the aes() function, and the x
and y arguments of aes() specify which variables to map to the x and y
axes. Let’s map flipper length to the x axis and body mass to the y
axis. (*as you type in variable names, you might notice them populate…
select the correct name and press tab to autofill*)

``` r
ggplot(
  data = penguins,
  mapping = aes(x = flipper_length_mm, y = body_mass_g)
)
```

But how will our data be displayed in this now structured graph? We will
explore different visualizations of your data in the following section.

------------------------------------------------------------------------

## Render Your New Work

By now you should have four code chunks, two to load libraries and two
to create a plot. While a `.qmd` document (the file you are working in
now) can be viewed on GitHub, it is best used to view raw code. Instead,
you will “render” your document to a markdown document, in this case a
gfm, since we specified that in our YAML header.

Render your Chapter 1a work by clicking the “Render” button by the blue
right arrow located on the top of your RStudio screen.

<img src="Images/render_document.png" 
     alt="RStudio screen at the top of the page with the 'Render' button with the blue arrow circled in red."
     style="width: 55%;">

------------------------------------------------------------------------

Now, to wrap up Chapter 1a, you will learn to use GitHub to make your
work viewable to others (and also reproducible)! You will push your
Chapter 1a work to GitHub, creating a pull request in the process. This
pull request will allow a mentor to review your work each chapter.

Remember that Chapter 1a branch you created? After you save your Quarto
notebook, you are going to push all of your new Chapter 1a changes to
that branch on GitHub, so your work will be visible on the
UCSC-Treehouse organization.

Lastly, you will request your mentor as a ‘reviewer’, so they can check
over your work *before* you officially merge it with the main branch.

Navigate back to **GitHub Desktop**… You should see your new changes
highlighted.

1.  Push new Chapter 1a changes to chapter-01a branch (Look to the
    bottom left of your screen and write a summary)

<img src="Images/push_ch_1a_to_ch_1a_branch.png" 
     alt="Cropped image of bottom left corner of GitHub Desktop screen where description 'Create and complete chapter 1a' is given and blue 'Commit 1 file to chapter-01a' button is circled in red at the bottom."
     style="width: 55%;">

2.  Publish the new chapter-01a branch

<img src="Images/publish_ch_1a_branch.png" 
     alt="GitHub Desktop screen where blue 'Publish branch' button is present and circled in red."
     style="width: 55%;">

3.  Create a pull request

<img src="Images/create_pull_request.png" 
     alt="GitHub Desktop screen after committing changes to new branch, prompted to click 'Create Pull Request' button in blue."
     style="width: 55%;">

You will be relocated to the **GitHub browser**.

1.  Add your mentor as a reviewer (**Note**: here I use ‘hbeale’ but
    make sure you are adding *your* mentor’s GitHub id)

<img src="Images/add_reviewer_to_pull_request.png" 
     alt="GitHub browser screen upon creating a pull request. On the right-hand side, a reviewer is added. The example shows 'hbeale' as reviewer, circled in red."
     style="width: 55%;">

2.  Click “Create pull request” and you’re done!

------------------------------------------------------------------------

**NEXT UP:** [Chapter
1b](https://github.com/UCSC-Treehouse/Essential-skills-for-Treehouse-computational-research/blob/main/Chapter-Instructions/Chapter_01b_Instructions.md)
