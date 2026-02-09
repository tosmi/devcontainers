VERSION := v1.1

TOKEN := $(shell awk -F=  '/token=ey/ { printf("%s",$$2) }'  ~/.ansible.cfg)
IMAGE := quay.io/tosmi/ansible-dev-container
CONTAINER := $(IMAGE):$(VERSION)

all:
	@echo TOKEN: $(TOKEN)
	@echo CONTAINER: $(CONTAINER)

.PHONY: container
container:
	podman manifest create $(CONTAINER)

	podman build \
		--platform linux/amd64,linux/arm64 \
		--manifest $(CONTAINER) \
		--build-arg ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_TOKEN=$(TOKEN) \
		--build-arg ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_VALIDATED_TOKEN=$(TOKEN) \
		.
.PHONY: push
push:
	podman manifest push $(CONTAINER)
