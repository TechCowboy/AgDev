# Copyright (C) 2015-2024 CE Programming
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU Lesser General Public
# License as published by the Free Software Foundation; either
# version 3 of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
# Lesser General Public License for more details.
#
# You should have received a copy of the GNU Lesser General Public License
# along with this program; if not, write to the Free Software Foundation,
# Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.

include $(CURDIR)/../common.mk

BUILD_SRC := $(patsubst %,build/%.src,$(wildcard *.c *.cpp))

EZCFLAGS := -S -ffreestanding -Wall -Wextra -Oz
EZCFLAGS += -D_EZ80 -isystem ../libc/include -isystem ../libc -isystem ../agon/include/agon -isystem ../agon/include -isystem ../fileioc -mllvm -profile-guided-section-prefix=false
EZCXXFLAGS := $(EZCFLAGS) -fno-exceptions -fno-rtti
EZCXXFLAGS += -isystem ../libc/include

WILDCARD_SRC = $(wildcard *.src) $(BUILD_SRC)
WILDCARD_H = $(wildcard include/*.h)
WILDCARD_AGON_H = $(wildcard include/agon/*.h)
WILDCARD_HW_H = $(wildcard include/sys/*.h)
WILDCARD_TI_H = $(wildcard include/ti/*.h)

all: $(BUILD_SRC)

build/%.c.src: %.c
	$(Q)$(call MKDIR,build)
	$(Q)$(EZCC) $(EZCFLAGS) $< -o $@

build/%.cpp.src: %.cpp
	$(Q)$(call MKDIR,build)
	$(Q)$(EZCC) $(EZCXXFLAGS) $< -o $@

clean:
	$(Q)$(call RMDIR,build)

install: $(addprefix install-,$(TARGETS))
	$(Q)$(call MKDIR,$(INSTALL_H))
	$(Q)$(call MKDIR,$(INSTALL_AGON))
	$(Q)$(call MKDIR,$(INSTALL_AGON_H))
	$(Q)$(call MKDIR,$(INSTALL_TI_H))
	$(Q)$(call MKDIR,$(INSTALL_HW_H))
	$(Q)$(call COPY,$(foreach file,$(call NATIVEPATH,$(WILDCARD_H)),$(call QUOTE_ARG,$(file))),$(INSTALL_H))
	$(Q)$(call COPY,$(foreach file,$(call NATIVEPATH,$(WILDCARD_HW_H)),$(call QUOTE_ARG,$(file))),$(INSTALL_HW_H))
	$(Q)$(call COPY,$(foreach file,$(call NATIVEPATH,$(WILDCARD_TI_H)),$(call QUOTE_ARG,$(file))),$(INSTALL_TI_H))
	$(Q)$(call COPY,$(call NATIVEPATH,$(WILDCARD_SRC)),$(INSTALL_AGON))
	$(Q)$(call COPY,$(foreach file,$(call NATIVEPATH,$(WILDCARD_AGON_H)),$(call QUOTE_ARG,$(file))),$(INSTALL_AGON_H))

.PHONY: all install clean