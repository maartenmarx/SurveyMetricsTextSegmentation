# Link naar de overleaf

https://www.overleaf.com/4753118495ttgjnnbcgkty

## Command voor pandoc conversion

``pandoc -f markdown-auto_identifiers -t latex survey_text_segmentaiton_metrics.md -o main.tex --standalone --variable title="Metrics for Text Segmentation: A Survey" --variable --author="Maarten Marx, Ruben van Heusden" --toc --variable date="February 2023" --citeproc --bibliography=PSS.bib --natbib``
