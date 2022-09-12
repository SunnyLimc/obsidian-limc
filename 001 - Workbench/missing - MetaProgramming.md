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

### version

- versions are given due to people can reliably depend on it
- sematic versioning
	- major version, backward **incompatibility**, removing func or rename it
	- minor version, add something to library (set patch no to 0), the minimum version support for a specific new feature 
	- patch version, full back compatibility, external interface remain no changes
- lock file - indicate with version of dependencies currently been used
	- auditable, reproduceable

### continual integrate

- Travis CI
- Azure pipeline
- GitHub action

- coverage testing

- depend bot
	- version badge on README. md

### testing

- test suite, the whole tools and configs of test
- unit test - small test, self contained test, only test for one feature 
- micro test - only test very particular things
- integration test - large test, test interaction of subsystem if it's work good
- regression test - test thing broken in the past to make sure you **won't reintroduce** that bug in future feature
- mocking test - simulate an action and verify it