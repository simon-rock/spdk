#
#  BSD LICENSE
#
#  Copyright (c) Intel Corporation.
#  All rights reserved.
#
#  Redistribution and use in source and binary forms, with or without
#  modification, are permitted provided that the following conditions
#  are met:
#
#    * Redistributions of source code must retain the above copyright
#      notice, this list of conditions and the following disclaimer.
#    * Redistributions in binary form must reproduce the above copyright
#      notice, this list of conditions and the following disclaimer in
#      the documentation and/or other materials provided with the
#      distribution.
#    * Neither the name of Intel Corporation nor the names of its
#      contributors may be used to endorse or promote products derived
#      from this software without specific prior written permission.
#
#  THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
#  "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
#  LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
#  A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
#  OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
#  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
#  LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
#  DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
#  THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
#  (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
#  OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#

SPDK_ROOT_DIR := $(abspath $(CURDIR)/../..)
include $(SPDK_ROOT_DIR)/mk/spdk.common.mk
include $(SPDK_ROOT_DIR)/mk/spdk.modules.mk

APP = nvmf_tgt

CFLAGS += $(ENV_CFLAGS)

C_SRCS := conf.c nvmf_tgt.c nvmf_rpc.c

SPDK_LIBS = \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_nvmf.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_nvme.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_event.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_log.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_trace.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_conf.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_util.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_bdev.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_copy.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_rpc.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_jsonrpc.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_json.a \

# These libraries do not expose any external API, only constructors,
# so they must be linked specially to ensure they are not removed.
SPDK_WHOLE_LIBS = \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_app_rpc.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_log_rpc.a \
	$(SPDK_ROOT_DIR)/build/lib/libspdk_bdev_rpc.a \

LIBS += $(BLOCKDEV_MODULES_LINKER_ARGS) \
        $(COPY_MODULES_LINKER_ARGS)

LIBS += $(SPDK_LIBS)

LIBS += -Wl,--whole-archive $(SPDK_WHOLE_LIBS) -Wl,--no-whole-archive

ifeq ($(CONFIG_RDMA),y)
LIBS += -libverbs -lrdmacm
endif

LIBS += $(ENV_LINKER_ARGS)

all : $(APP)

$(APP) : $(OBJS) $(SPDK_LIBS) $(SPDK_WHOLE_LIBS) $(BLOCKDEV_MODULES) $(LINKER_MODULES) $(ENV_LIBS)
	$(LINK_C)

clean :
	$(CLEAN_C) $(APP)

include $(SPDK_ROOT_DIR)/mk/spdk.deps.mk
