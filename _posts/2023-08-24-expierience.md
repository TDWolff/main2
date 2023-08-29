---
toc: true
comments: true
layout: post
title: My Expierience
description: This Is Going To Explain My Expieriences And My Struggles
type: plans
courses: { csp: {week: 1, categories: [4.A]} }
---

### Week 1
-Throughout week 1 I went in thinking I got this in the bag, I soon realized this is gonna be a bit harder than I thought.
I realized that I was not the best coder in the world, that I was not the great coder my assumptions had lead me to, but I persevered. I pushed my self and soon I understood what I was doing.

### The Beginning
-Starting off strong, I copied the student repo from the [Teacher Repo](https://nighthawkcoders.github.io/teacher/) and followed the instructions to make my website. I didn't have any struggles till I had to use the Make command in terminal. I kept getting an error.
```bash
make: Error 127
```
At the end of the day I left feeling defeated. Code 1 - Torin 0.

### The Fix
-Start of the next day, I got to work figuring out what was going on. I searched high and low, google and in my files to find the cause, and then. I found it. Hidden inside of the MakeFile, I saw it.
```makefile
# Convert .md file, if .ipynb file is newer
$(DESTINATION_DIRECTORY)/%_IPYNB_2_.md: _notebooks/%.ipynb
	@echo "Converting source $< to destination $@"
	@python -c 'import sys; from scripts.convert_notebooks import convert_single_notebook; convert_single_notebook(sys.argv[1])' "$<"
```

The fix was right there staring me in the face. I ran a quick command in terminal to verify my conclusion.
```bash
python3 --version
```
I quickly hopped back into the MakeFile and added 1 number.
```makefile
# Convert .md file, if .ipynb file is newer
$(DESTINATION_DIRECTORY)/%_IPYNB_2_.md: _notebooks/%.ipynb
	@echo "Converting source $< to destination $@"
	@python3 -c 'import sys; from scripts.convert_notebooks import convert_single_notebook; convert_single_notebook(sys.argv[1])' "$<"
```
And......


IT WORKED. I finally could use the make command to make my server! The error was being caused by the fact that the MakeFile didn't know wether to use the Python3 that came pre-installed on my laptop or to use the newly installed python 2.7.16 and all I needed to do was to help it by clarifying to use python3.

