Demo of Jupyter running [jupyter-scala](https://github.com/alexarchambault/jupyter-scala)

Build with
```
docker build -t docxs/scala-notebook scala
```

Run with
```
docker run -it --rm -p 8888:8888 -h sandbox docxs/scala-notebook -bash
```
