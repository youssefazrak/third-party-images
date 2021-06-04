FROM golang:1.16 as builder

RUN go get github.com/sigstore/cosign@v0.5.0

RUN cd /go/pkg/mod/github.com/sigstore/cosign\@v0.5.0 &&\
    make cosign



FROM alpine:3.13.5

# Commit details

ARG commit
ENV IMAGE_COMMIT=$commit
LABEL io.kyma-project.third-party-images.commit=$commit

LABEL source=git@github.com:kyma-project/third-party-images.git

ENV KUBECTL_VERSION="v1.19.7"

RUN apk --no-cache upgrade &&\
    apk --no-cache add openssl coreutils curl bash jq grep inotify-tools &&\
    wget https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl -O /usr/local/bin/kubectl &&\
    chmod +x /usr/local/bin/kubectl

COPY --from=builder /go/pkg/mod/github.com/sigstore/cosign\@v0.5.0/cosign usr/local/bin/cosign

ENTRYPOINT ["/bin/bash"]
