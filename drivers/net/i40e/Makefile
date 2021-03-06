#   BSD LICENSE
#
#   Copyright(c) 2010-2017 Intel Corporation. All rights reserved.
#   All rights reserved.
#
#   Redistribution and use in source and binary forms, with or without
#   modification, are permitted provided that the following conditions
#   are met:
#
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in
#       the documentation and/or other materials provided with the
#       distribution.
#     * Neither the name of Intel Corporation nor the names of its
#       contributors may be used to endorse or promote products derived
#       from this software without specific prior written permission.
#
#   THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
#   "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
#   LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
#   A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
#   OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
#   SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
#   LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
#   DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
#   THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
#   (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
#   OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

include $(RTE_SDK)/mk/rte.vars.mk

#
# library name
#
LIB = librte_pmd_i40e.a

CFLAGS += -O3
CFLAGS += $(WERROR_FLAGS) -DPF_DRIVER -DVF_DRIVER -DINTEGRATED_VF
CFLAGS += -DX722_A0_SUPPORT

EXPORT_MAP := rte_pmd_i40e_version.map

LIBABIVER := 2

#
# Add extra flags for base driver files (also known as shared code)
# to disable warnings
#
ifeq ($(CONFIG_RTE_TOOLCHAIN_ICC),y)
CFLAGS_BASE_DRIVER = -wd593 -wd188
else ifeq ($(CONFIG_RTE_TOOLCHAIN_CLANG),y)
CFLAGS_BASE_DRIVER += -Wno-sign-compare
CFLAGS_BASE_DRIVER += -Wno-unused-value
CFLAGS_BASE_DRIVER += -Wno-unused-parameter
CFLAGS_BASE_DRIVER += -Wno-strict-aliasing
CFLAGS_BASE_DRIVER += -Wno-format
CFLAGS_BASE_DRIVER += -Wno-missing-field-initializers
CFLAGS_BASE_DRIVER += -Wno-pointer-to-int-cast
CFLAGS_BASE_DRIVER += -Wno-format-nonliteral
CFLAGS_BASE_DRIVER += -Wno-unused-variable
else
CFLAGS_BASE_DRIVER  = -Wno-sign-compare
CFLAGS_BASE_DRIVER += -Wno-unused-value
CFLAGS_BASE_DRIVER += -Wno-unused-parameter
CFLAGS_BASE_DRIVER += -Wno-strict-aliasing
CFLAGS_BASE_DRIVER += -Wno-format
CFLAGS_BASE_DRIVER += -Wno-missing-field-initializers
CFLAGS_BASE_DRIVER += -Wno-pointer-to-int-cast
CFLAGS_BASE_DRIVER += -Wno-format-nonliteral
CFLAGS_BASE_DRIVER += -Wno-format-security
CFLAGS_BASE_DRIVER += -Wno-unused-variable

ifeq ($(shell test $(GCC_VERSION) -ge 44 && echo 1), 1)
CFLAGS_BASE_DRIVER += -Wno-unused-but-set-variable
endif

CFLAGS_i40e_lan_hmc.o += -Wno-error
endif
OBJS_BASE_DRIVER=$(sort $(patsubst %.c,%.o,$(notdir $(wildcard $(SRCDIR)/base/*.c))))
$(foreach obj, $(OBJS_BASE_DRIVER), $(eval CFLAGS_$(obj)+=$(CFLAGS_BASE_DRIVER)))

VPATH += $(SRCDIR)/base

#
# all source are stored in SRCS-y
# base driver is based on the package of dpdk-i40e.2016.04.18.12.tar.gz.
#
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_adminq.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_common.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_diag.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_hmc.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_lan_hmc.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_nvm.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_dcb.c

SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_ethdev.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_rxtx.c
ifeq ($(CONFIG_RTE_ARCH_ARM64),y)
SRCS-$(CONFIG_RTE_LIBRTE_I40E_INC_VECTOR) += i40e_rxtx_vec_neon.c
else ifeq ($(CONFIG_RTE_ARCH_PPC_64),y)
SRCS-$(CONFIG_RTE_LIBRTE_I40E_INC_VECTOR) += i40e_rxtx_vec_altivec.c
else
SRCS-$(CONFIG_RTE_LIBRTE_I40E_INC_VECTOR) += i40e_rxtx_vec_sse.c
endif
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_ethdev_vf.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_pf.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_fdir.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_flow.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += rte_pmd_i40e.c
SRCS-$(CONFIG_RTE_LIBRTE_I40E_PMD) += i40e_tm.c

# install this header file
SYMLINK-$(CONFIG_RTE_LIBRTE_I40E_PMD)-include := rte_pmd_i40e.h

include $(RTE_SDK)/mk/rte.lib.mk
