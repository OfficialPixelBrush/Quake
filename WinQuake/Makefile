# Modern Linux Makefile for Quake
# Supports debug and release builds

# Compiler
CC := gcc
CFLAGS_BASE := -Dstricmp=strcasecmp -Wall -Wextra
LDFLAGS := -lm

# Directories
BUILD_DEBUG_DIR := debug
BUILD_RELEASE_DIR := release
BIN_DIR := $(BUILD_DEBUG_DIR)/bin

# SDL/GL/X11 (adjust if using Mesa or system GL)
GL_CFLAGS := -DGLQUAKE -I/usr/include
GL_LDFLAGS := -lGL -lX11 -lXext -ldl -lXxf86dga -lXxf86vm

# Source files (Linux only)
SOURCES := \
    cl_demo.c cl_input.c cl_main.c cl_parse.c cl_tent.c \
    chase.c cmd.c common.c console.c crc.c cvar.c \
    draw.c d_edge.c d_fill.c d_init.c d_modech.c \
    d_part.c d_polyse.c d_scan.c d_sky.c d_sprite.c \
    d_surf.c d_vars.c d_zpoint.c host.c host_cmd.c \
    keys.c menu.c mathlib.c model.c net_dgrm.c \
    net_loop.c net_main.c net_vcr.c net_udp.c net_bsd.c \
    nonintel.c pr_cmds.c pr_edict.c pr_exec.c r_aclip.c \
    r_alias.c r_bsp.c r_light.c r_draw.c r_efrag.c \
    r_edge.c r_misc.c r_main.c r_sky.c r_sprite.c \
    r_surf.c r_part.c r_vars.c screen.c sbar.c \
    sv_main.c sv_phys.c sv_move.c sv_user.c zone.c \
    view.c wad.c world.c cd_linux.c sys_linux.c \
    snd_dma.c snd_mem.c snd_mix.c snd_linux.c vid_vga.c

OBJS_DEBUG := $(patsubst %.c,$(BUILD_DEBUG_DIR)/%.o,$(filter %.c,$(SOURCES))) \
              $(patsubst %.s,$(BUILD_DEBUG_DIR)/%.o,$(filter %.s,$(SOURCES)))

# Rules
all: build_debug build_release

build_debug: CFLAGS := $(CFLAGS_BASE) -O0 -g
build_debug: $(BUILD_DEBUG_DIR)/bin/squake

build_release: CFLAGS := $(CFLAGS_BASE) -O2 -funroll-loops -ffast-math
build_release: BIN_DIR := $(BUILD_RELEASE_DIR)/bin
build_release: $(BUILD_RELEASE_DIR)/bin/squake

# Create directories
$(BUILD_DEBUG_DIR)/%.o: %.c
	@mkdir -p $(dir $@)
	$(CC) $(CFLAGS) -c $< -o $@

$(BUILD_DEBUG_DIR)/%.o: %.s
	@mkdir -p $(dir $@)
	$(CC) -c $< -o $@

# Link the binaries
$(BUILD_DEBUG_DIR)/bin/squake: $(OBJS_DEBUG)
	@mkdir -p $(dir $@)
	$(CC) $(CFLAGS) $^ -o $@ $(LDFLAGS)

$(BUILD_RELEASE_DIR)/bin/squake: $(OBJS_DEBUG)
	@mkdir -p $(dir $@)
	$(CC) $(CFLAGS) $^ -o $@ $(LDFLAGS)

# GLQuake build
build_debug_gl: CFLAGS := $(CFLAGS_BASE) -O0 -g $(GL_CFLAGS)
build_debug_gl: $(BUILD_DEBUG_DIR)/bin/glquake

$(BUILD_DEBUG_DIR)/bin/glquake: $(OBJS_DEBUG)
	@mkdir -p $(dir $@)
	$(CC) $(CFLAGS) $^ -o $@ $(GL_LDFLAGS) $(LDFLAGS)

# Clean
clean:
	rm -rf $(BUILD_DEBUG_DIR) $(BUILD_RELEASE_DIR)

.PHONY: all build_debug build_release clean build_debug_gl
