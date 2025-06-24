ZEPHYR_UBOOT_DIR	:= $(dir $(abspath $(lastword $(MAKEFILE_LIST))))
UBOOT_DIR			:= $(abspath $(ZEPHYR_UBOOT_DIR)/..)
TOPDIR				:= $(abspath $(UBOOT_DIR)/../..)
ZEPHYR_DIR			:= $(TOPDIR)/zephyr
PHTHON				?= python3
NPROC 				?= $(shell nproc)
SDK_VERSION			?= $(shell cat $(ZEPHYR_DIR)/SDK_VERSION)
ARCH 				?= arm
CROSS_COMPILE		?= /home/$(USER)/zephyr-sdk-$(SDK_VERSION)/aarch64-zephyr-elf/bin/aarch64-zephyr-elf-
O					?= $(ZEPHYR_DIR)/build/u-boot
DEFCONFIG			?= rpi_5_defconfig

all: build

phony+=config
config:
	$(MAKE) -C $(UBOOT_DIR) CROSS_COMPILE=$(CROSS_COMPILE) ARCH=$(ARCH) O=$(O) $(DEFCONFIG)

phony+=build
build: config
	$(MAKE) -C $(UBOOT_DIR) CROSS_COMPILE=$(CROSS_COMPILE) ARCH=$(ARCH) O=$(O) -j $(NPROC)

phony+=gen_compiledb
gen_compiledb:
	mkdir -p $(UBOOT_DIR)/.vscode
	$(PHTHON) $(UBOOT_DIR)/scripts/gen_compile_commands.py -d $(O) -o $(UBOOT_DIR)/.vscode/compile_commands.json

phony+=clean
clean:
	rm -rf $(O)

.PHONY: $(phony)
