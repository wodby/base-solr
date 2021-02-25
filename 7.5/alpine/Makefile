-include env_make

SOLR_VER ?= 7.5.0

REPO = wodby/base-solr
NAME = base-solr-$(SOLR_VER)

ifeq ($(TAG),)
    TAG := $(SOLR_VER)
endif

PLATFORM ?= linux/amd64

.PHONY: build buildx-build buildx-push test push shell run start stop logs clean release

default: build

build:
	docker build -t $(REPO):$(TAG) ./

buildx-build:
	docker buildx build --platform $(PLATFORM) -t $(REPO):$(TAG) ./

buildx-push:
	docker buildx build --platform $(PLATFORM) --push -t $(REPO):$(TAG) ./

test:
	IMAGE=$(REPO):$(TAG) ./test.sh

push:
	docker push $(REPO):$(TAG)

shell:
	docker run --rm --name $(NAME) -i -t $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(TAG) /bin/bash

run:
	docker run --rm --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(TAG) $(CMD)

start:
	docker run -d --name $(NAME) $(PORTS) $(VOLUMES) $(ENV) $(REPO):$(TAG)

stop:
	docker stop $(NAME)

logs:
	docker logs $(NAME)

clean:
	-docker rm -f $(NAME)

release: build push
