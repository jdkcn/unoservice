# LibreOffice PDF generator service

A docker container environment to bundle the execution of `unoconv`,
a command-line utility that uses `LibreOffice` to convert documents 
of various types (such as Word, OpenDocument, etc.) to PDF.

An instance of `LibreOffice` will be run in the background, and used
by multiple requestors to perform the necessary conversions.

To build, run:

```shell
docker build --rm -t unoconv .
```

To start the container, run:

```shell
docker run -it -m 512m \
  --name unoservice -p 3000:3000 \
  --tmpfs /tmp unoconv
```

You can use your custom fonts dir for add extra fonts, run:

```shell
docker run -m 512m -i -t -d -p 3000:3000  \
  --restart=always --name unoservice \
  --tmpfs /tmp \
  -v /data/unoservice/fonts:/usr/share/fonts/truetype/custom unoconv
```

Now, files can be sent to the service:

```shell
curl -o mydoc.pdf -F format=pdf -F 'file=@mydoc.docx' http://localhost:3000/convert
```
