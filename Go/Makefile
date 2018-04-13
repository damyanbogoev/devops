# Self-Documented Makefile https://marmelab.com/blog/2016/02/29/auto-documented-makefile.html

PACKAGE = [package_name]
COMMIT_HASH = `git rev-parse --short HEAD 2>/dev/null`
BUILD_DATE = `date +%FT%T%z`
LDFLAGS = -ldflags "-X $(PACKAGE)/[binary_name].CommitHash=$(COMMIT_HASH) -X $(PACKAGE)/[binary_name].BuildDate=$(BUILD_DATE)"

.PHONY: restore build build-race install check doc fmt lint test test-race help

default: help

restore: ## installs godep and sync the dependencies
	go get github.com/tools/godep
	godep restore

build: restore ## builds the binary
	go build $(LDFLAGS) $(PACKAGE)

build-race: vendor ## builds the binary with race detector enabled
	go build -race $(LDFLAGS) $(PACKAGE)

install: restore ## installs the binary
	go install $(LDFLAGS) $(PACKAGE)

check: restore test ## checks the binary
	@if [ $$? -eq 0 ] ; then echo "NO ISSUES FOUND." ; else echo "ISSUES DETECTED." ; fi

doc: ## extracts and generates documentation for Go programs
	godoc -http=:6060 -index

fmt: ## formats the Go code
	@go fmt `go list ./... | grep -v vendor/`

lint: ## runs go lint
	@golint `go list ./... | grep -v vendor/`

test: ## runs the tests
	go test `go list ./... | grep -v vendor/`

test-race: ## runs the tests
	go test -race `go list ./... | grep -v vendor/`

help:
	@grep -E '^[a-zA-Z0-9_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'