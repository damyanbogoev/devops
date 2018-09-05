# Self-Documented Makefile https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html

ndef = $(if $(value $(1)),,$(error $(1) argument not set))

.PHONY: build run post help

default: help

build: ## builds drafts (include content marked as draft)
	hugo -D -v

run: ## starts hugo webserver, which watches hugo files for changes, rebuilds them and serves the site (include content marked as draft)
	hugo server -D --disableFastRender

post: ## creates a new post (make post name=new_post_name)
	$(call ndef,name)
	@hugo new post/${name}.md

deploy: ## deploys the latest changes on damyanon.net
	@echo "place your deploy command here"

help:
	@grep -E '^[a-zA-Z0-9_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'