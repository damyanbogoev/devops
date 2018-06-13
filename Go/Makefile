# Self-Documented Makefile https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html

BRANCH_NAME = `git rev-parse --abbrev-ref HEAD`
COMMIT_HASH = `git rev-parse --short HEAD 2>/dev/null`
BUILD_DATE = `date +%FT%T%z`
VERSION = $(shell cat version/VERSION)
VERSION_PKG = version_pkg_importpath
LDFLAGS = -ldflags "-X $(VERSION_PKG).Version=$(VERSION) -X $(VERSION_PKG).Revision=$(COMMIT_HASH) -X $(VERSION_PKG).Branch=$(BRANCH_NAME) -X $(VERSION_PKG).BuildDate=$(BUILD_DATE)"
PACKAGE = pkg_importpath
PACKAGES = $(go list ./... | grep -v '/vendor/')

.PHONY: default
default: help

.PHONY: restore
restore: ## installs godep and sync the dependencies
	go get github.com/tools/godep
	godep restore

.PHONY: build
build: restore ## builds the binary
	go build $(LDFLAGS) $(PACKAGE)

.PHONY: build-race
build-race: vendor ## builds the binary with race detector enabled
	go build -race $(LDFLAGS) $(PACKAGE)

.PHONY: install
install: restore ## installs the binary
	go install $(LDFLAGS) $(PACKAGE)

.PHONY: check
check: restore test ## checks the binary
	@if [ $$? -eq 0 ] ; then echo "NO ISSUES FOUND." ; else echo "ISSUES DETECTED." ; fi

.PHONY: doc
doc: ## extracts and generates documentation for Go programs
	godoc -http=:6060 -index

.PHONY: fmt
fmt: ## formats the Go code
	@go fmt $(PACKAGES)

.PHONY: lint
lint: ## runs go lint
	@golint $(PACKAGES)

.PHONY: test
test: ## runs the tests
	go test $(PACKAGES)

.PHONY: test-race
test-race: ## runs the tests
	go test -race $(PACKAGES)

.PHONY: help
help:
	@grep -E '^[a-zA-Z0-9_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'