# CS106A Summer 2017 Website

This is a pre-compiled, static website for the Summer 2017 offering of
Stanford's CS106A introductory programming course.  This repo is structured as
follows:

```
bottle/ - the source files for Bottle, the library used to compile templates
templates/ - the uncompiled Bottle templates
WWW/ - the output folder containing compiled pages and other static resources
compile.py - the template compilation script
```

## Editing
Within `templates`, there are subfolders for exam pages, assignment pages, and
subparts of various pages.  Here are the relevant files for various tasks you
may want to do:

- **Posting announcements:** add a `<div class="well">` element to
`templates/parts/announcements.html` containing your announcement.  See existing
announcements, as well as `templates/parts/announcements-old.html`, for example
formatting.

- **Adding non-section handouts:** add the handout to `WWW/handouts/`, with the
naming style `NUMBER-DASHED_NAME.extension`.  E.g. `1-General-Information.pdf`.
Any handouts in this folder will be automatically listed in the "Handouts"
dropdown with the format `NUMBER - SEPARATED_NAME`.
E.g. `1 - General Information`.

- **Adding section handouts:** add the handout to `WWW/section-handouts/` and
update `templates/parts/dropdowns/sectionMaterialsDropdownList.html` to list the
new handout or other materials.

- **Add a new assignment:** add the assignment page template to
`templates/assignments/` and update
`templates/parts/dropdowns/assignmentsDropdownList.html` to list the new
assignment.

- **Change the lecture schedule:** the lecture calendar and the lectures
dropdown are both dynamically generated from the same JSON file,
`schedule.json`.  This JSON file has the following format:

```
{
	"days": ...,
	"weeks": ...
}
```

`days` maps to an array of strings naming each day on which there is lecture.
This is displayed as the header row in the schedule table.  E.g.

```
{
	"days": ["Monday", "Tuesday", "Wednesday", "Thursday"],
	"weeks": ...
}
```

`weeks` is a list containing data for each week.  Each week is itself a list of
objects representing each lecture that week.  Note that each week's length must
match the length of the `days` array.  These lecture objects have the following
keys (whose values are all strings):
```
date (REQUIRED): the date string (e.g. "June 26") to display
title (OPTIONAL): the name of the lecture (or event)
type (OPTIONAL): "HOLIDAY" if a holiday, or "OFF" if no lecture
filename (OPTIONAL): the filename prefix for all material filepaths
code (OPTIONAL): if false, does not show the "Code" link for this day
slides (OPTIONAL): if false, does not show the "Slides" links for this day
notes (OPTIONAL): a list of strings to display in gray on this calendar day,
				 each on its own line.
```

Of note, `filename` is used as follows:
- the lecture PPT is assumed to be at ```{{pathToRoot}}lectures/{{filename}}/{{filename}}.pptx```
- the lecture PDF is assumed to be at ```{{pathToRoot}}lectures/{{filename}}/{{filename}}.pdf```
- the lecture code folder is assumed to be at ```{{pathToRoot}}lectures/{{filename}}/{{filename}}/```
- the lecture code ZIP is assumed to be at ```{{pathToRoot}}lectures/{{filename}}/{{filename}}.zip```

Holidays are grayed out in the calendar and not displayed in the dropdown.  All
information is dynamically displayed from this JSON file for each lecture,
including date, title, links to material, any HWs due, and any reading due;
lectures are also numbered automatically.

All lectures in this file are also displayed in the "Lectures" dropdown in the
navbar, up to the first lecture without a filename.  Dividers are added between
weeks.


## Compiling
To compile all references and links to test locally, from the root directory run

```
python compile.py -t --output_dir myDirectory
```

This compiles all templates in the `templates/` folder and, preserving its
directory structure and filenames, creates all HTML files in the given
output directory.  If no output directory is specified, the files are outputted
to the ```WWW/``` directory.

To compile all references and links to host, from the root directory run

```
python compile.py --output_dir myDirectory
```

This does the same thing as the previous command, but compiles all website URLs
based on the given ```ROOT``` constant in `compile.py` instead of local paths.

You may also change any of the default compilation settings in `compile.py`,
including:

```
ROOT - the root URL for the hosted version of this website
OUTPUT_DIR - the default output directory for compiled files
TEMPLATE_DIR - the directory to compile
ROOT_TEMPLATE - the "meta-template" containing the <head>, footer, etc. inside
which all other templates are rendered.
IGNORE_DIRS - directories within TEMPLATE_DIR that should be ignored
```


## Testing Locally
To host the compiled site locally, from the `WWW/` directory run

`python -m SimpleHTTPServer`

This will host the site on `localhost:8000`.  Make sure to compile using the
`-t` flag beforehand; otherwise the paths will not function properly!

