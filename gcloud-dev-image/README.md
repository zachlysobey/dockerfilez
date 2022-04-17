# `gcloud-dev-image`

Just something I've been poking on a little.

I know folks have effectively used docker images as portable development environments.

I figured it'd be nice to have something like that while experimenting with K8s in GCP

## Usage

### Build the image

```console
docker build . -t gcloud-dev-image
```

### Run the image

```console
docker run -i -t -v ${PWD}:/workspace gcloud-dev-image /bin/bash
```
