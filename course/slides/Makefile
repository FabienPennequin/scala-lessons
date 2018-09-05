PUBLISH_TO=~/sites/julienrf.github.com/scala-lessons/course
IMAGES=$(wildcard images/*.png)
SOURCES=$(wildcard content/en/*.md)

all: $(SOURCES) Makefile
	yarn install
	pandoc -t revealjs -s -o index.html --standalone --slide-level=2 -V revealjs-url=./node_modules/reveal.js/ -V theme:black --number-sections $(SOURCES)

images: $(IMAGES)
	cp $(IMAGES) $(PUBLISH_TO)

publish: all images
	cp index.html $(PUBLISH_TO)

pdf: $(SOURCES)
	pandoc -o scala-course.pdf $(SOURCES)