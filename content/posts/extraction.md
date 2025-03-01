While building the Voxed app I stumbled upon a rather daunting task. I needed a way to ingest hundreds of file types, and embed their contents for RAG. However, this is easier said than done. Due to file diversity, most file types don't actually provide meaningful information in practice. For example, I tried to upload a PDF my teacher uploaded to canvas into one of my class notebooks on Voxed but the PDF didn't actually contain any OCR extractable text. Instead the text was in images. Along with that there where also diagrams and other visuals that needed to be handled. But enough rambling let's actually explore the solution...

## Embedded images

Often times (especially in education), documents have embedded visuals and diagrams making ingestion difficult.
[https://aidanandrews22.github.io/content/images/extraction/img1.png]
[https://aidanandrews22.github.io/content/images/extraction/img2.png]
