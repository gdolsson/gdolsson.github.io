---
layout: "post"
title: "Tesseract OCR"
date: "2021-08-09 14:09"
---
[Tesseract](https://github.com/tesseract-ocr) is a free and open source OCR software. A "[tutorial](https://guides.nyu.edu/c.php?g=884390&p=6355032)" of sorts. This is also available as a `brew install tesseract` and then `tesseract image-input.png output-filename -l eng pdf` or switch pdf to txt instead to get plain text output.

Tesseract does not work with PDF files, hence the following can provide a solution to solve this.

`convert -density 300 file.pdf -depth 8 file.tiff`

Use `imagemagick` to create a 300 dpi image at a color depth of 8 bits from file.pdf into a file named file.tiff in the current folder.

`tesseract file.tiff output-filename -l eng pdf`

To do an OCR reading of the PDF file.
