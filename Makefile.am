###############################################################################
## Makefile for Finnish--Irish
###############################################################################

LANG1=fin
LANG2=gle
PREFIX1=$(LANG1)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)

BASENAME=apertium-$(PREFIX1)

TARGETS_MONOLING=$(PREFIX1).automorf.hfst $(PREFIX1).autogen.bin \
				 $(PREFIX2).automorf.bin $(PREFIX2).autogen.hfst 
TARGETS_TRANSFER=$(PREFIX2).t1x.bin $(PREFIX1).t1x.bin  \
				 $(PREFIX1).autobil.bin $(PREFIX2).autobil.bin \
				 $(PREFIX1).t2x.bin $(PREFIX1).t3x.bin \
				 $(PREFIX2).t2x.bin $(PREFIX2).t3x.bin \
				 $(PREFIX1).rlx.bin $(PREFIX2).rlx.bin \
				 $(PREFIX1).autolex.bin $(PREFIX2).autolex.bin
TARGETS_COMMON=$(TARGETS_MONOLING) $(TARGETS_TRANSFER)

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

###############################################################################
## fin transducer
###############################################################################

# currently we copy stuff from another dir
$(PREFIX1).automorf.hfst: $(AP_LIB1)/$(LANG1).automorf.hfst
	cp -v -f $< $@

$(PREFIX1).automorf.bin: $(AP_LIB1)/$(LANG1).automorf.bin
	cp -v -f $< $@

$(PREFIX2).autogen.bin: $(AP_LIB1)/$(LANG1).autogen.bin
	cp -v -f $< $@

$(PREFIX2).autogen.hfst: $(AP_LIB1)/$(LANG1).autogen.hfst
	cp -v -f $< $@

###############################################################################
## gle transducer
###############################################################################

$(PREFIX2).automorf.bin: $(AP_LIB2)/$(LANG2).automorf.bin
	cp -v -f $< $@

$(PREFIX1).autogen.bin: $(AP_LIB2)/$(LANG2).autogen.bin
	cp -v -f $< $@


###############################################################################
## Bilingual dictionaries
###############################################################################

$(PREFIX1).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@

$(PREFIX2).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $< 
	lt-comp rl $< $@

###############################################################################
## Disambiguation rules
###############################################################################

$(PREFIX1).rlx.bin: $(BASENAME).$(PREFIX1).rlx
	cg-comp $< $@

$(PREFIX2).rlx.bin: $(BASENAME).$(PREFIX2).rlx
	cg-comp $< $@


###############################################################################
## Lexical selection rules
###############################################################################

$(PREFIX1).autolex.bin: $(BASENAME).$(PREFIX1).lrx
	lrx-comp $< $@

$(PREFIX2).autolex.bin: $(BASENAME).$(PREFIX2).lrx
	lrx-comp $< $@


###############################################################################
## Structural transfer rules
###############################################################################

$(PREFIX1).t1x.bin: $(BASENAME).$(PREFIX1).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

$(PREFIX1).t2x.bin: $(BASENAME).$(PREFIX1).t2x
	apertium-validate-interchunk $<
	apertium-preprocess-transfer $< $@

$(PREFIX1).t3x.bin: $(BASENAME).$(PREFIX1).t3x
	apertium-validate-postchunk $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t1x.bin: $(BASENAME).$(PREFIX2).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t2x.bin: $(BASENAME).$(PREFIX2).t2x
	apertium-validate-interchunk $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t3x.bin: $(BASENAME).$(PREFIX2).t3x
	apertium-validate-postchunk $<
	apertium-preprocess-transfer $< $@


###############################################################################
## Distribution
###############################################################################

EXTRA_DIST=					\
		$(BASENAME).$(PREFIX1).lrx	\
		$(BASENAME).$(PREFIX1).dix	\
		$(BASENAME).$(PREFIX2).lrx	\
		$(BASENAME).$(PREFIX1).t1x	\
		$(BASENAME).$(PREFIX1).t2x	\
		$(BASENAME).$(PREFIX1).t3x	\
		$(BASENAME).$(PREFIX2).t1x	\
		$(BASENAME).$(PREFIX2).t2x	\
		$(BASENAME).$(PREFIX2).t3x	\
		modes.xml

###############################################################################
## Installation
###############################################################################

apertium_fin_gledir=$(prefix)/share/apertium/$(BASENAME)/

# Files which should be installed to $(prefix)/share/apertium/$(BASENAME)/:
apertium_fin_gle_DATA=	\
	$(TARGETS_COMMON)				\
	$(BASENAME).$(PREFIX1).t1x			\
	$(BASENAME).$(PREFIX1).t2x			\
	$(BASENAME).$(PREFIX1).t3x			\
	$(BASENAME).$(PREFIX2).t1x			\
	$(BASENAME).$(PREFIX2).t2x			\
	$(BASENAME).$(PREFIX2).t3x


# All modes are created by the same goal, listing several will just give problems with make -j2
noinst_DATA=modes/$(PREFIX1).mode \
	.deps/$(LANG1).automorf.hfst \
	.deps/$(LANG2).automorf.bin

install-data-local: install-modes

###############################################################################
## Cleanup
###############################################################################

CLEANFILES = $(TARGETS_COMMON)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	echo "house" | apertium -d . -f line fin-gle | tee .test-mt
	@echo "house" > .test-ref
	@diff .test-ref .test-mt
	echo "house" | apertium -d . -f line gle-fin | tee .test-mt
	@echo "house" > .test-ref
	@diff .test-ref .test-mt
	@rm .test-ref .test-mt
