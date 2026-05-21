VERSION := v1.3

TOKEN := $(shell awk -F=  '/token=ey/ { printf("%s",$$2) }'  ~/.ansible.cfg)
IMAGE := quay.io/tosmi/ansible-dev-container
CONTAINER := $(IMAGE):$(VERSION)
PLATFORMS := linux/amd64,linux/arm64

all:
	@echo TOKEN: $(TOKEN)
	@echo CONTAINER: $(CONTAINER)

.PHONY: container
container: ## Build the container image
	podman manifest exists $(CONTAINER) || podman manifest create $(CONTAINER)

	podman build \
		--platform $(PLATFORMS) \
		--manifest $(CONTAINER) \
		--build-arg ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_TOKEN=$(TOKEN) \
		--build-arg ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_VALIDATED_TOKEN=$(TOKEN) \
		.
.PHONY: push
push: ## Push the container image to the registry
	podman manifest push $(CONTAINER)


# Help and Documentation
.PHONY: help
help: ## Show this help message
	@echo "Usage: make [target]"
	@echo ""
	@echo "Targets:"
	@awk '/^[a-zA-Z\-_0-9%]+:/ { \
		helpMessage = match($$0, /^([^:]+):[^#]*## (.+)/); \
		if (helpMessage) { \
			helpCommand = substr($$0, 1, index($$0, ":")-1); \
			helpMessage = substr($$0, index($$0, "##") + 3); \
			printf "  %-20s %s\n", helpCommand, helpMessage; \
		} \
	}' $(MAKEFILE_LIST) | sort
