# makefile

.SUFFIXES: md pdf

%.pdf : %.md
	pandoc -f gfm -o $@ $<

%.html : %.md
	pandoc -f gfm --css SSI3-MELD-report.css --standalone -o $@ $<

