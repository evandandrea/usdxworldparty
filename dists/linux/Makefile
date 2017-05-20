ARCH = $(shell uname -m)
VERSION = $(shell cat ../../VERSION)
REV = $(shell git rev-parse --short HEAD)

.PHONY: default build-deps build run-local compress clean distclean upload

default: build

build-deps: prefix/built_libs
prefix/built_libs:
	@./dl.sh
	./build-deps.sh | tee build-deps.log

run: output/ultrastardx
	cd output && LD_LIBRARY_PATH=lib ./usdxworldparty

build: output/usdxworldparty
output/usdxworldparty output/VERSION: prefix/built_libs
	@./build.sh | tee build.log
	@mkdir -p output/data/songs
	@cp -v launch.sh output/
	@cp -v ../../LICENSE output/
	@cp -v ../../game/LICENSE.* output/
	echo -e "Version: $(VERSION)\nRevison: $(REV)\nBuild date: `date -u +%FT%TZ`" > output/VERSION

compress: UsdxWorldParty-$(REV)-$(ARCH).tar.xz
UsdxWorldParty-$(REV)-$(ARCH).tar.xz: output/usdxworldparty output/VERSION
	rm -f "./UsdxWorldParty-$(REV)-$(ARCH).tar.xz"
	cd output && tar cJf "../UsdxWorldParty-$(REV)-$(ARCH).tar.xz" .

upload: UsdxWorldParty-$(REV)-$(ARCH).tar.xz
	curl --upload-file './UsdxWorldParty-$(REV)-$(ARCH).tar.xz' "https://transfer.sh/UsdxWorldParty-$(REV)-$(ARCH).tar.xz"

clean:
	rm -rf output UltraStarDeluxe*.tar.xz

distclean: clean
	rm -rf deps prefix
