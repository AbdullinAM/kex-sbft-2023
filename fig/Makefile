
.PHONY: all

%.eps: %.svg
	inkscape -S $*.svg --export-type=eps --export-filename=$*.eps


all: jetbrainsResearch.eps kfg.eps

clean:
	rm *.eps
