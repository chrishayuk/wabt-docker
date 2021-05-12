# Introduction
This is a docker image for converting webassembly .wat files to .wasm files

# building the image
to build the image, type in

```
docker build -t quay.io/chrishayuk/wat2wasm .
```

# running the image
to run the images, type

```
docker run -v /Users/chrishayuk/chris-source/working/wabt-docker/wat2wasm/data:/data wat2wasm /data/input-wat/math.wat

```