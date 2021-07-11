# makefile

.SUFFIXES: md pdf

%.pdf : %.md
	pandoc -f gfm -o $@ $<

clean: 
	rm *.pdf

