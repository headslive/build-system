PDFLATEX=texi2pdf -q -c -o inside-heads

TEX = 01-heads-main.tex

all: pdf

pdf: ${TEX}
	${PDFLATEX} $<

clean:
	rm -f *.out *.aux *.log *.toc

distclean: clean
	rm -f *.pdf

.PHONY: all pdf clean distclean


