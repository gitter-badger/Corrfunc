ROOT_DIR := ../..
INSTALL_HEADERS_DIR := $(ROOT_DIR)/include
INSTALL_LIB_DIR := $(ROOT_DIR)/lib
INSTALL_BIN_DIR := $(ROOT_DIR)/bin
UTILS_DIR := $(ROOT_DIR)/utils
IO_DIR := $(ROOT_DIR)/io

include $(ROOT_DIR)/mocks.options $(ROOT_DIR)/common.mk
OPT:=$(filter-out -DOUTPUT_RPAVG,$(OPT))

LIBNAME:=countpairs_theta_mocks
LIBRARY := lib$(LIBNAME).a
LIBSRC  := countpairs_theta_mocks.c countpairs_theta_mocks_impl_float.c countpairs_theta_mocks_impl_double.c \
           $(UTILS_DIR)/gridlink_mocks_impl_float.c $(UTILS_DIR)/gridlink_mocks_impl_double.c \
           $(UTILS_DIR)/utils.c $(UTILS_DIR)/progressbar.c $(UTILS_DIR)/cpu_features.c 
LIBRARY_HEADERS := $(LIBNAME).h


TARGET := DDtheta_mocks
TARGETS := $(TARGET) wtheta
TARGETSRC := $(LIBSRC) $(TARGET).c $(IO_DIR)/io.c $(IO_DIR)/ftread.c
INCL     := $(LIBNAME).h countpairs_theta_mocks_kernels_float.c countpairs_theta_mocks_kernels_double.c countpairs_theta_mocks_kernels.c.src \
            countpairs_theta_mocks_impl.c.src countpairs_theta_mocks_impl.h.src countpairs_theta_mocks_impl_double.h countpairs_theta_mocks_impl_float.h \
            $(IO_DIR)/io.h $(IO_DIR)/ftread.h $(IO_DIR)/io.h  countpairs_theta_mocks_impl_double.c countpairs_theta_mocks_impl_float.c \
            $(UTILS_DIR)/gridlink_mocks_impl_double.h $(UTILS_DIR)/gridlink_mocks_impl_float.h $(UTILS_DIR)/gridlink_mocks_impl.h.src \
            $(UTILS_DIR)/gridlink_mocks_impl_double.c $(UTILS_DIR)/gridlink_mocks_impl_float.c $(UTILS_DIR)/gridlink_mocks_impl.c.src \
            $(UTILS_DIR)/cellarray_mocks_float.h $(UTILS_DIR)/cellarray_mocks_double.h $(UTILS_DIR)/cellarray_mocks.h.src \
	    $(UTILS_DIR)/progressbar.h $(UTILS_DIR)/cpu_features.h  $(UTILS_DIR)/avx_calls.h  $(UTILS_DIR)/sse_calls.h \
	    $(UTILS_DIR)/utils.h $(UTILS_DIR)/function_precision.h $(UTILS_DIR)/defs.h

TARGETOBJS:=$(TARGETSRC:.c=.o)
LIBOBJS:=$(LIBSRC:.c=.o) 

TARGET2:=wtheta
SRC2:=$(TARGET2).c

all: $(TARGETS) $(TARGETSRC) $(SRC2) $(ROOT_DIR)/mocks.options $(ROOT_DIR)/common.mk Makefile

wtheta: $(SRC2) $(UTILS_DIR)/utils.c 
	$(CC) $(CFLAGS) $(INCLUDE) $^ $(CLINK) -o $@ 

countpairs_theta_mocks_impl_double.o: countpairs_theta_mocks_impl_double.c countpairs_theta_mocks_impl_double.h countpairs_theta_mocks_kernels_double.c $(UTILS_DIR)/gridlink_mocks_impl_double.h $(UTILS_DIR)/cellarray_mocks_double.h
countpairs_theta_mocks_impl_float.o: countpairs_theta_mocks_impl_float.c countpairs_theta_mocks_impl_float.h countpairs_theta_mocks_kernels_float.c $(UTILS_DIR)/gridlink_mocks_impl_float.h $(UTILS_DIR)/cellarray_mocks_float.h
countpairs_theta_mocks.o:countpairs_theta_mocks.c countpairs_theta_mocks_impl_float.h countpairs_theta_mocks_impl_double.h

libs:lib
lib: $(LIBRARY)

install: $(INSTALL_BIN_DIR)/$(TARGET) $(INSTALL_BIN_DIR)/wtheta $(INSTALL_LIB_DIR)/$(LIBRARY) $(INSTALL_HEADERS_DIR)/$(LIBRARY_HEADERS)

tests:
	$(MAKE) -C ../tests wtheta

clean:
	$(RM) $(TARGETS) $(TARGETOBJS) $(LIBRARY) countpairs_theta_mocks_kernels_float.c countpairs_theta_mocks_kernels_double.c countpairs_theta_mocks_impl_double.[ch] countpairs_theta_mocks_impl_float.[ch]
	$(RM) -R *.dSYM

distclean:clean | $(INSTALL_LIB_DIR) $(INSTALL_HEADERS_DIR) $(INSTALL_BIN_DIR)
	cd $(INSTALL_LIB_DIR) && $(RM) $(LIBRARY)
	cd $(INSTALL_HEADERS_DIR) && $(RM) $(LIBRARY_HEADERS)
	cd $(INSTALL_BIN_DIR) && $(RM) $(TARGETS)

include $(ROOT_DIR)/rules.mk
