# Cross-compiling opus file under mingw

TOOL_PREFIX ?= i686-w64-mingw32

# To build opusfile under mingw, you need to first build:
DEPS = ogg opus ssl

ogg_URL := http://downloads.xiph.org/releases/ogg/libogg-1.3.0.tar.xz
ogg_SHA := 231725029c843492914f24e74085e734bca6f1d6446ac72df39b0c3a9d4bc74b
ogg_URL := http://downloads.xiph.org/releases/ogg/libogg-1.3.1.tar.xz
ogg_SHA := 3a5bad78d81afb78908326d11761c0fb1a0662ee7150b6ad587cc586838cdcfa

opus_URL := http://downloads.xiph.org/releases/opus/opus-1.1-beta.tar.gz
opus_SHA := ec1784287f385aef994b64734aaecae04860e61aa50fc6eef6643fa7e40dd193

ssl_URL := http://www.openssl.org/source/openssl-1.0.1e.tar.gz
ssl_SHA := f74f15e8c8ff11aa3d5bb5f276d202ec18d7246e95f961db76054199c69c1ae3

libopusfile-0.dll: ../unix/Makefile $(DEPS)
	CC=$(TOOL_PREFIX)-gcc \
	RANLIB=$(TOOL_PREFIX)-ranlib \
	PKG_CONFIG_PATH=${PWD}/lib/pkgconfig \
	$(MAKE) -f $<

opusfile: $(DEPS)
	../configure --host=$(TOOL_PREFIX) --prefix=${PWD} \
	  PKG_CONFIG_PATH=${PWD}/lib/pkgconfig
	$(MAKE)

clean:
	$(RM) -r objs

# Generate rules to download and verify each dependency.
define WGET_template =
$(1)_DIR := $$(basename $$(basename $$(notdir $$($(1)_URL))))
 $(1)_DIR: $$(notdir $$($(1)_URL))
	@if test "$$($(1)_SHA)" = "$$$$(sha256sum $$< | cut -f 1 -d ' ')"; \
	then \
	  echo "+ $$@ checksum verified."; \
	else \
	  echo "! $$@ checksum didn't match!"; \
	  $(RM) $$<; exit 1; \
	fi
	tar xf $$<
 $$(notdir $$($(1)_URL)):
	wget $$($(1)_URL)
 $(1): $(1)_BUILD
endef
$(foreach dep,$(DEPS),$(eval $(call WGET_template,$(dep))))

ogg_BUILD: $(ogg_DIR)
	cd $< && ./configure --host=$(TOOL_PREFIX) --prefix=${PWD}
	$(MAKE) -C $< check
	$(MAKE) -C $< install

opus_BUILD: $(opus_DIR)
	cd $< && ./configure --host=$(TOOL_PREFIX) --prefix=${PWD}
	$(MAKE) -C $< check
	$(MAKE) -C $< install

ssl_BUILD: $(ssl_DIR)
	cd $< && ./Configure shared mingw64 no-asm \
	  --prefix=${PWD} \
	  --cross-compile-prefix=$(TOOL_PREFIX)-
	$(MAKE) -C $< depend
	$(MAKE) -C $<
	$(MAKE) -C $< install

# CROSS_COMPILE="i686-w64-mingw32-" ./Configure mingw no-asm no-shared --prefix=$PWD/mingw && make depend && make -j8 && make install


