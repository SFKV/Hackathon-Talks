# Copyright (c) 2017-2020 Forschungszentrum Juelich GmbH
# This code is licensed under MIT license (see the LICENSE file for details)
#
# This Makefile intentionally rebuilds everytime everything.

PDFLATEX		?= pdflatex
XELATEX			?= xelatex
LUALATEX		?= lualatex
LATEX_FLAGS		?= -halt-on-error -interaction=nonstopmode

export TEXINPUTS	:= $(CURDIR)/../:$(CURDIR)/src//:$(TEXINPUTS)

cmd%pdflatex		 = $(PDFLATEX) $(LATEX_FLAGS)
cmd%xelatex		 = $(XELATEX) $(LATEX_FLAGS)
cmd%lualatex		 = $(LUALATEX) $(LATEX_FLAGS)

engines			 = pdflatex xelatex lualatex
targets			 = minimal minimal_handout tutorial $(other)
other			:= $(patsubst %.tex,%,$(wildcard *.tex))

%/minimal: src
	@mkdir -p $(@D)
	cd $(@D) && $(cmd%$*) ../../tutorial/minimal.tex

%/minimal_handout: src
	@mkdir -p $(@D)
	cd $(@D) && $(cmd%$*) ../../tutorial/minimal_handout.tex

%/tutorial: src %/minimal %/minimal_handout
	@mkdir -p $(@D)
	cd $(@D) && $(cmd%$*) -jobname tutorial '\makeatletter\def\input@path{{../../tutorial/}}\makeatother\input{./tutorial.tex}'

define other-target
%/$1: src
	@mkdir -p $$(@D)
	cd $$(@D) && $$(cmd%$$*) ../$1.tex
endef
$(foreach o,$(other),$(eval $(call other-target,$o)))

all: $(engines)

$(foreach engine,$(engines),$(eval $(engine): $(patsubst %,$(engine)/%,$(targets))))

src:
	mkdir -p $@
	cd $@ && ln -s ../../*.sty ../../*.pdf .

clean:
	$(RM) -r src
	$(RM) -r $(engines)

# update the tutorial.pdf that is shipped with the theme
update-tutorial: lualatex/tutorial
	cp lualatex/tutorial.pdf ../tutorial
