LATEX := pdflatex
BIBTEX := bibtex
LATEX_OPTS := -output-dir build
SOURCES := white_paper.tex 
ALL_TEX_FILES := $(shell find . -iname '*.tex')
OBJECTS := $(SOURCES:.tex=.pdf)
VECTOR_SOURCES := $(shell find vectors -iname '*.svg')
VECTOR_OBJECTS := $(VECTOR_SOURCES:.svg=.pdf_tex)
INKSCAPE_CMD := inkscape -D -z

all: $(VECTOR_OBJECTS) $(OBJECTS) 

vectors: $(VECTOR_OBJECTS)

%.pdf: %.tex
	mkdir -p ./build
	$(LATEX) $(LATEX_OPTS) $< 
	$(BIBTEX) build/$(<:.tex=.aux)
	$(LATEX) $(LATEX_OPTS) $< 2> /dev/null
	$(LATEX) $(LATEX_OPTS) $< 2> /dev/null
	ln -rsf ./build/$(<:.tex=.log) $(<:.tex=.log) # For latex-suite to check errors
	cp build/$(notdir $@) $(dir $@)/$(notdir $@)

%.pdf_tex: %.svg
	$(INKSCAPE_CMD) --file=$< --export-pdf=$(<:.svg=.pdf) --export-latex
	# BSD sed must have a backup file in -i mode. On GNU sed it's optional.
	sed -i.bak 's|$(notdir $(<:.svg=.pdf))|$(<:.svg=.pdf)|g' $@
	rm -f $@.bak

clean:
	rm -rf build/*
	rm -f $(OBJECTS)
	rm -f $(VECTOR_OBJECTS)
	rm -f *.log

setup:
	mkdir build

spellcheck:
	# Need to install aspell & english dict for this to work
	@echo "Have you committed? ispell will mutate the files (press ctrl+c to cancel) "
	@read x
	for f in $(ALL_TEX_FILES); do \
		aspell -x -d british check $$f; \
	done

pdf : all
