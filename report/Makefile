TARGET=report
TEX=xelatex -shell-escape
BIBTEX=bibtex
READER=mupdf

all: view

rebuild output/$(TARGET).pdf: *.tex *.bib output
	cd output && rm -f *.tex *.bib && ln -s ../*.tex ../*.bib .
	cd output && $(TEX) $(TARGET).tex $(BIBTEX) $(TARGET).aux


output:
	mkdir output -p
	cd output && rm -f res && ln -s ../res .

view: output/$(TARGET).pdf
	$(READER) output/$(TARGET).pdf &

clean:
	rm -rf output

# compile twice to get cross reference right
final : *.tex *.bib output
	cd output && rm -f *.tex *.bib && ln -s ../*.tex ../*.bib .
	cd output && $(TEX) $(TARGET).tex && $(BIBTEX) $(TARGET).aux && \
		$(TEX) $(TARGET).tex && $(TEX) $(TARGET).tex
	ps2pdf14 output/$(TARGET).pdf ./$(TARGET).pdf

count:
	@word_count *.tex

dist: output/$(TARGET).pdf
	rm -rf paper
	mkdir paper
	cp output/$(TARGET).pdf paper/
	cp -r screenshots paper/
	7z a -tzip paper.zip paper
	rm -rf paper

.PHONY: all view clean rebuild dist