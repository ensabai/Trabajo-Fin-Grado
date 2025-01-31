SHELL:= /bin/bash
-include ./aux/Makefile

# BEGIN: FINE TUNE
FILES:= ms 
DUST_EXT:= {aux,bbl,blg,dvi,lof,log,lot,nav,out,Rout,snm,synctex.gz,toc,vrb}
CLEAN_EXT:= {dvi,html,pdf}
# to keep .tex & .log files from 'knitr::knit()', uncomment next line
#.PRECIOUS: %.tex
# END: FINE TUNE 

PDFL_OPTS:= -output-format pdf

.PHONY: default
default:
	make clean;\
	for f in $(FILES); do (make $$f.pdf); done;\
	make dust

.PHONY: help
help:
	@echo "Type 'make' for the default recipe, i.e.: (i) clean, (ii) make all pdf files, and (iii) dust"
	@echo "Type 'make clean' to remove byproduct files named after FILES with extensions in DUST_EXT (see below) and CLEAN_EXT, i.e.:" $(CLEAN_EXT)
	@echo "Type 'make file.pdf' to make a pdf from file.tex or file.Rnw (by knitting it into filename.tex)"
	@echo "Type 'make dust' to remove byproduct files named after FILES with extensions in DUST_EXT, i.e.:" $(DUST_EXT)

%.pdf: %.tex
	pdflatex $(PDFL_OPTS) $*; bibtex $*; pdflatex $(PDFL_OPTS) $*; pdflatex $(PDFL_OPTS) $*

%.tex: %.Rnw
	Rscript --no-save --no-restore --no-site-file --no-init-file -e "knitr::knit(\"$*.Rnw\",quiet=TRUE)" > $*.Rout 2>&1

.PHONY:clean dust
clean: dust
	for f in $(FILES); do (rm -f $(basename $$f).$(CLEAN_EXT)); done;\
	$(foreach f, $(wildcard *.Rnw), rm -f $(basename $f).tex $(basename $f).R)
	rm -f fig/*.*

dust:
	rm -f Sweave.sty
	for f in $(FILES); do (rm -f $(basename $$f).$(DUST_EXT)); done
