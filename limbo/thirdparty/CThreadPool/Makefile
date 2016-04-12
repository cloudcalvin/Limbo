# $Id$
# plain simple Makefile to build test

#==========================================================================
#                         Directories and names 
# ==========================================================================

LIMBO_ROOT_DIR = $(realpath ../../..)
TEST_DIR = $(realpath .)
OBJDIR = $(LIMBO_ROOT_DIR)/obj
MKDIR = if [ ! -d $(@D) ]; then mkdir -p $(@D); fi

VPATH = . $(TEST_DIR)

#==========================================================================
#                         Compilation Flags
# ==========================================================================

# default DBG is off
DBG = 0

UNAME_S = $(shell uname -s)
ifeq ($(UNAME_S), Linux)
	-include $(LIMBO_ROOT_DIR)/Unix.mk
endif 
ifeq ($(UNAME_S), Darwin)
	-include $(LIMBO_ROOT_DIR)/Darwin.mk
endif 

ifeq ($(DBG), 1)
	CXXFLAGS = $(CXXFLAGS_DEBUG) -DTHPOOL_DEBUG
else
	CXXFLAGS = $(CXXFLAGS_RELEASE)
endif

#==========================================================================
#                         Special Library
# ==========================================================================

INCLUDE = -I $(LIMBO_ROOT_DIR) -pthread 
LIBS = -lpthread

# ==========================================================================
#                         Standard Setting
# ==========================================================================

SRCS = thpool.c
OBJS = $(SRCS:%.c=$(OBJDIR)/%.o)
DEPS = $(OBJS:%.o=%.d) 	# one dependency file for each source

SRCS_TEST = example.c
OBJS_TEST = $(patsubst $(TEST_DIR)/%.c,$(TEST_DIR)/%.o,$(SRCS_TEST))
DEPS_TEST = $(OBJS_TEST:%.o=%.d)

all: 
	@$(MKDIR)
	$(CC) $(CXXFLAGS) -c -o $(OBJDIR)/thpool.o thpool.c $(INCLUDE)
	$(CC) $(CXXFLAGS) -c -o $(TEST_DIR)/example.o $(TEST_DIR)/example.c $(INCLUDE)
	$(AR) $(ARFLAGS) libthpool.a $(OBJDIR)/thpool.o
	mv libthpool.a $(LIMBO_ROOT_DIR)/lib
	$(CC) $(CXXFLAGS) -o example $(TEST_DIR)/example.o $(LIMBO_ROOT_DIR)/lib/libthpool.a $(LIBS) 

.PHONY: clean
clean: cleandep
	rm -f $(TEST_DIR)/example $(OBJS) $(OBJS_TEST)

.PHONY: cleandep
cleandep:
	rm -f $(DEPS) $(DEPS_TEST)
