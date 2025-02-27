###############################################################################
## Makefile for {{languageName1}}--{{languageName2}}
###############################################################################

LANG1={{languageCode1}}
LANG2={{languageCode2}}
PREFIX1=$(LANG1)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)

BASENAME=apertium-$(PREFIX1)

TARGETS_COMMON =		\{{ifnot_giella1
	$(PREFIX1).automorf.bin	\
ifnot_giella1}}{{if_giella1
	$(PREFIX1).automorf.hfst \
	$(PREFIX1).automorf-untrimmed.hfst \
if_giella1}}{{if_prob1
	$(PREFIX1).prob		\
if_prob1}}{{if_rlx1
	$(PREFIX1).rlx.bin	\
if_rlx1}}{{if_giella1
	$(PREFIX1).mor.rlx.bin  \
	$(PREFIX1).syn.rlx.bin  \
if_giella1}}
	$(PREFIX1).autobil.bin	\
	$(PREFIX1).autolex.bin	\{{ifnot_giella1
	$(PREFIX1).autogen.bin	\
ifnot_giella1}}{{if_giella1
	$(PREFIX1).autogen.hfst \
if_giella1}}{{if_pgen2
	$(PREFIX1).autopgen.bin	\
if_pgen2}}{{if_lsx
	$(PREFIX1).autoseq.bin	\
	$(PREFIX1).revautoseq.bin	\
if_lsx}}{{if_chunk
	$(PREFIX1).t1x.bin	\
	$(PREFIX1).t2x.bin	\
	$(PREFIX1).t3x.bin	\
if_chunk}}{{if_rtx
	$(PREFIX1).rtx.bin	\
if_rtx}}{{ifnot_giella2
	$(PREFIX2).automorf.bin	\
ifnot_giella2}}{{if_giella2
	$(PREFIX2).automorf.hfst \
	$(PREFIX2).automorf-untrimmed.hfst \
if_giella2}}{{if_prob2
	$(PREFIX2).prob		\
if_prob2}}{{if_rlx2
	$(PREFIX2).rlx.bin	\
if_rlx2}}{{if_giella2
	$(PREFIX2).mor.rlx.bin  \
	$(PREFIX2).syn.rlx.bin  \
if_giella2}}
	$(PREFIX2).autobil.bin	\
	$(PREFIX2).autolex.bin	\{{ifnot_giella2
	$(PREFIX2).autogen.bin	\
ifnot_giella2}}{{if_giella2
	$(PREFIX2).autogen.hfst \
if_giella2}}{{if_pgen1
	$(PREFIX2).autopgen.bin	\
if_pgen1}}{{if_lsx
	$(PREFIX2).autoseq.bin	\
	$(PREFIX2).revautoseq.bin	\
if_lsx}}{{if_chunk
	$(PREFIX2).t1x.bin	\
	$(PREFIX2).t2x.bin	\
	$(PREFIX2).t3x.bin
if_chunk}}{{if_rtx
	$(PREFIX2).rtx.bin
if_rtx}}

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

###############################################################################
## Monolingual transducers
###############################################################################

{{if_giella
.deps/%.autobil.prefixes: %.autobil.bin .deps/.d
	lt-print -H $< .deps/$*.autobil.att
	hfst-txt2fst .deps/$*.autobil.att -o .deps/$*.autobil.hfst
	hfst-project -p upper .deps/$*.autobil.hfst -o .deps/$*.autobil.upper                                     # bidix
	echo ' [ ? - %+ ]* ' | hfst-regexp2fst > .deps/$*.any-nonplus.hfst                                        # [^+]*
	hfst-concatenate -1 .deps/$*.autobil.upper -2 .deps/$*.any-nonplus.hfst -o .deps/$*.autobil.nonplussed    # bidix [^+]*
	echo ' %+ ' | hfst-regexp2fst > .deps/$*.single-plus.hfst                                                 # +
	hfst-concatenate -1 .deps/$*.single-plus.hfst -2 .deps/$*.autobil.nonplussed -o .deps/$*.autobil.postplus # + bidix [^+]*
	hfst-repeat -f0 -t1 -i .deps/$*.autobil.postplus -o .deps/$*.autobil.postplus.0,1                         # (+ bidix [^+]*){0,1} -- gives at most one +
	hfst-concatenate -1 .deps/$*.autobil.nonplussed -2 .deps/$*.autobil.postplus.0,1 -o $@                    # bidix [^+]* (+ bidix [^+]*){0,1}
if_giella}}

# -------------------
# {{languageName1}} analysis:
# -------------------
{{if_hfst1
.deps/$(PREFIX1).autobil.prefixes: $(PREFIX1).autobil.bin .deps/.d
	lt-print $(PREFIX1).autobil.bin | sed 's/ /@_SPACE_@/g' > .deps/$(PREFIX1).autobil.att
	hfst-txt2fst -e ε <  .deps/$(PREFIX1).autobil.att > .deps/$(PREFIX1).autobil.hfst
	hfst-project -p upper .deps/$(PREFIX1).autobil.hfst > .deps/$(PREFIX1).autobil.upper
	echo " ?* " | hfst-regexp2fst > .deps/any-symbol.hfst
	hfst-concatenate -1 .deps/$(PREFIX1).autobil.upper -2 .deps/any-symbol.hfst -o .deps/$(PREFIX1).autobil.prefixes

.deps/$(LANG1).automorf.hfst: $(AP_SRC1)/$(LANG1).automorf.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

.deps/$(LANG1).automorf.bin: $(AP_SRC1)/$(LANG1).automorf.att.gz .deps/.d
	@$(ZCAT) $< > $@.att
	lt-comp lr $@.att $@
	@rm $@.att

$(PREFIX1).automorf.bin: .deps/$(LANG1).automorf.hfst .deps/$(PREFIX1).autobil.prefixes
	hfst-compose-intersect -1 .deps/$(LANG1).automorf.hfst -2 .deps/$(PREFIX1).autobil.prefixes -o .deps/$(PREFIX1).automorf.trimmed
	hfst-fst2txt -i .deps/$(PREFIX1).automorf.trimmed -o .deps/$(PREFIX1).automorf.trimmed.att
	lt-comp lr .deps/$(PREFIX1).automorf.trimmed.att $@
if_hfst1}}{{if_lttoolbox1
.deps/$(LANG1).automorf.bin: $(AP_SRC1)/$(LANG1).automorf.bin .deps/.d
	cp $< $@

$(PREFIX1).automorf.bin: .deps/$(LANG1).automorf.bin $(PREFIX1).autobil.bin
	lt-trim $^ $@
if_lttoolbox1}}{{if_giella1
.deps/$(LANG1).automorf.hfst: $(AP_SRC1)/apertium-und.$(LANG1)-und.LR.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

.deps/$(PREFIX1).automorf-trimmed.hfst: .deps/$(LANG1).automorf.hfst .deps/$(PREFIX1).autobil.prefixes
	hfst-compose-intersect -1 $< -2 .deps/$(PREFIX1).autobil.prefixes -o $@

$(PREFIX1).automorf.hfst: .deps/$(PREFIX1).automorf-trimmed.hfst
	hfst-fst2fst -w -i $< -o $@

$(PREFIX1).automorf-untrimmed.hfst: .deps/$(LANG1).automorf.hfst
	hfst-fst2fst -w -i $< -o $@
if_giella1}}

# -------------------
# {{languageName1}} generation:
# ---------------------
{{if_hfst1
$(PREFIX2).autogen.hfst: $(AP_SRC1)/$(LANG1).autogen.hfst
	cp $< $@
if_hfst1}}

{{ifnot_giella1
$(PREFIX2).autogen.bin: $(AP_SRC1)/$(LANG1).autogen.bin
	cp $< $@
ifnot_giella1}}{{if_giella1
.deps/$(LANG1).autogen.hfst: $(AP_SRC1)/apertium-$(LANG1).$(LANG1).RL.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

$(PREFIX2).autogen.hfst: .deps/$(LANG1).autogen.hfst
	hfst-fst2fst -O -i $< -o $@
if_giella1}}

# -------------------
# {{languageName2}} analysis:
# -------------------
{{if_hfst2
.deps/$(PREFIX2).autobil.prefixes: $(PREFIX2).autobil.bin .deps/.d
	lt-print $(PREFIX2).autobil.bin | sed 's/ /@_SPACE_@/g' > .deps/$(PREFIX2).autobil.att
	hfst-txt2fst -e ε <  .deps/$(PREFIX2).autobil.att > .deps/$(PREFIX2).autobil.hfst
	hfst-project -p upper .deps/$(PREFIX2).autobil.hfst > .deps/$(PREFIX2).autobil.upper
	echo " ?* " | hfst-regexp2fst > .deps/any-symbol.hfst
	hfst-concatenate -1 .deps/$(PREFIX2).autobil.upper -2 .deps/any-symbol.hfst -o .deps/$(PREFIX2).autobil.prefixes

.deps/$(LANG2).automorf.hfst: $(AP_SRC2)/$(LANG2).automorf.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

.deps/$(LANG2).automorf.bin: $(AP_SRC2)/$(LANG2).automorf.att.gz .deps/.d
	@$(ZCAT) $< > $@.att
	lt-comp lr $@.att $@
	@rm $@.att

$(PREFIX2).automorf.bin: .deps/$(LANG2).automorf.hfst .deps/$(PREFIX2).autobil.prefixes
	hfst-compose-intersect -1 .deps/$(LANG2).automorf.hfst -2 .deps/$(PREFIX2).autobil.prefixes -o .deps/$(PREFIX2).automorf.trimmed
	hfst-fst2txt -i .deps/$(PREFIX2).automorf.trimmed -o .deps/$(PREFIX2).automorf.trimmed.att
	lt-comp lr .deps/$(PREFIX2).automorf.trimmed.att $@
if_hfst2}}{{if_lttoolbox2
.deps/$(LANG2).automorf.bin: $(AP_SRC2)/$(LANG2).automorf.bin .deps/.d
	cp $< $@

$(PREFIX2).automorf.bin: .deps/$(LANG2).automorf.bin $(PREFIX2).autobil.bin
	lt-trim $^ $@
if_lttoolbox2}}{{if_giella2
.deps/$(LANG2).automorf.hfst: $(AP_SRC2)/apertium-und.$(LANG2)-und.LR.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

.deps/$(PREFIX2).automorf-trimmed.hfst: .deps/$(LANG2).automorf.hfst .deps/$(PREFIX2).autobil.prefixes
	hfst-compose-intersect -1 $< -2 .deps/$(PREFIX2).autobil.prefixes -o $@

$(PREFIX2).automorf.hfst: .deps/$(PREFIX2).automorf-trimmed.hfst
	hfst-fst2fst -w -i $< -o $@

$(PREFIX2).automorf-untrimmed.hfst: .deps/$(LANG2).automorf.hfst
	hfst-fst2fst -w -i $< -o $@
if_giella2}}

# -------------------
# {{languageName2}} generation:
# ---------------------
{{if_hfst2
$(PREFIX1).autogen.hfst: $(AP_SRC2)/$(LANG2).autogen.hfst
	cp $< $@
if_hfst2}}

{{ifnot_giella2
$(PREFIX1).autogen.bin: $(AP_SRC2)/$(LANG2).autogen.bin
	cp $< $@
ifnot_giella2}}{{if_giella2
.deps/$(LANG2).autogen.hfst: $(AP_SRC2)/apertium-$(LANG2).$(LANG2).RL.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

$(PREFIX1).autogen.hfst: .deps/$(LANG2).autogen.hfst
	hfst-fst2fst -O -i $< -o $@
if_giella2}}

###############################################################################
## Tagger models
###############################################################################

$(PREFIX1).prob: $(AP_SRC1)/$(LANG1).prob
	cp $< $@

$(PREFIX2).prob: $(AP_SRC2)/$(LANG2).prob
	cp $< $@

###############################################################################
## Disambiguation rules
###############################################################################

{{ifnot_giella1
$(PREFIX1).rlx.bin: $(AP_SRC1)/apertium-$(LANG1).$(LANG1).rlx
	cg-comp $< $@
ifnot_giella1}}{{if_giella1
$(PREFIX1).mor.rlx.bin: $(AP_SRC1)/$(LANG1).mor.rlx.bin gt2apertium.cg3r
	cg-relabel $^ $@

$(PREFIX1).syn.rlx.bin: $(AP_SRC1)/$(LANG1).syn.rlx.bin gt2apertium.cg3r
	cg-relabel $^ $@
if_giella1}}

{{ifnot_giella2
$(PREFIX2).rlx.bin: $(AP_SRC2)/apertium-$(LANG2).$(LANG2).rlx
	cg-comp $< $@
ifnot_giella2}}{{if_giella2
$(PREFIX2).mor.rlx.bin: $(AP_SRC2)/$(LANG2).mor.rlx.bin gt2apertium.cg3r
	cg-relabel $^ $@

$(PREFIX2).syn.rlx.bin: $(AP_SRC2)/$(LANG2).syn.rlx.bin gt2apertium.cg3r
	cg-relabel $^ $@
if_giella2}}

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
## Lexical selection rules
###############################################################################

$(PREFIX1).autolex.bin: $(BASENAME).$(PREFIX1).lrx
	lrx-comp $< $@

$(PREFIX2).autolex.bin: $(BASENAME).$(PREFIX2).lrx
	lrx-comp $< $@
{{if_lsx
$(PREFIX1).autoseq.bin: $(BASENAME).$(PREFIX1).lsx
	lsx-comp lr $< $@

$(PREFIX2).autoseq.bin: $(BASENAME).$(PREFIX2).lsx
	lsx-comp lr $< $@

$(PREFIX1).revautoseq.bin: $(BASENAME).$(PREFIX2).lsx
	lsx-comp rl $< $@

$(PREFIX2).revautoseq.bin: $(BASENAME).$(PREFIX1).lsx
	lsx-comp rl $< $@
if_lsx}}

###############################################################################
## Structural transfer rules
###############################################################################

{{if_chunk
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
if_chunk}}{{if_rtx
$(PREFIX1).rtx.bin: $(BASENAME).$(PREFIX1).rtx
	rtx-comp $< $@

$(PREFIX2).rtx.bin: $(BASENAME).$(PREFIX2).rtx
	rtx-comp $< $@
if_rtx}}

###############################################################################
## Postgeneration dictionaries
###############################################################################

$(PREFIX1).autopgen.bin: $(AP_SRC2)/$(LANG2).autopgen.bin
	cp $< $@

$(PREFIX2).autopgen.bin: $(AP_SRC1)/$(LANG1).autopgen.bin
	cp $< $@

###############################################################################
## Distribution
###############################################################################

EXTRA_DIST=					\
		$(BASENAME).$(PREFIX1).lrx	\
		$(BASENAME).$(PREFIX1).dix	\
		$(BASENAME).$(PREFIX2).lrx	\{{if_chunk
		$(BASENAME).$(PREFIX1).t1x	\
		$(BASENAME).$(PREFIX1).t2x	\
		$(BASENAME).$(PREFIX1).t3x	\
		$(BASENAME).$(PREFIX2).t1x	\
		$(BASENAME).$(PREFIX2).t2x	\
		$(BASENAME).$(PREFIX2).t3x	\
if_chunk}}{{if_rtx
		$(BASENAME).$(PREFIX1).rtx	\
		$(BASENAME).$(PREFIX2).rtx	\
if_rtx}}{{if_anaphora
		$(BASENAME).$(PREFIX1).arx	\
		$(BASENAME).$(PREFIX2).arx	\
if_anaphora}}{{if_lsx
		$(BASENAME).$(PREFIX1).lsx	\
		$(BASENAME).$(PREFIX2).lsx	\
if_lsx}}
		modes.xml

###############################################################################
## Installation
###############################################################################

apertium_{{languageCode1}}_{{languageCode2}}dir=$(prefix)/share/apertium/$(BASENAME)/

# Files which should be installed to $(prefix)/share/apertium/$(BASENAME)/:
apertium_{{languageCode1}}_{{languageCode2}}_DATA=	\{{if_anaphora
	$(BASENAME).$(PREFIX1).arx			\
	$(BASENAME).$(PREFIX2).arx			\
if_anaphora}}{{if_chunk
	$(TARGETS_COMMON)				\
	$(BASENAME).$(PREFIX1).t1x			\
	$(BASENAME).$(PREFIX1).t2x			\
	$(BASENAME).$(PREFIX1).t3x			\
	$(BASENAME).$(PREFIX2).t1x			\
	$(BASENAME).$(PREFIX2).t2x			\
	$(BASENAME).$(PREFIX2).t3x
if_chunk}}{{ifnot_chunk
	$(TARGETS_COMMON)
ifnot_chunk}}


# All modes are created by the same goal, listing several will just give problems with make -j2
noinst_DATA=modes/$(PREFIX1).mode \
	.deps/$(LANG1).automorf.bin \
	.deps/$(LANG2).automorf.bin

install-data-local: install-modes
uninstall-local: uninstall-modes

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
	apertium-regtest test
