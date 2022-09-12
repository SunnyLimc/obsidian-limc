this lesson is about how to build your system, add your dependencies, or compose a bunch of files

### make

- always try to build the **first** target by default
- only build for **latest** changes (once file is up-to-date since last build, the program will stop)

```shell
# define default target [target: dependencies target (will be run if the file is missing)]
paper.pdf: paper.tex plot-date.png
	# cmds
	pdflatex paper.tex

# a dependency target of above (% refer to any whitespace-separated string)
plot-%.png: %.dat plot.py
	# cmds ($* refer to the substring matched by %)
	# it will generate the tex
	./plot.py -i $*.dat -o $@
```