-include $(PVFMM_DIR)/MakeVariables

ifndef CXX_PVFMM
-include ../MakeVariables
endif

ifndef CXXFLAGS_PVFMM
$(error Cannot find file: MakeVariables)
endif

# FC=$(FC_PVFMM) # TODO: for now, FC must be provided by user
# CC=$(CC_PVFMM) # TODO: for now, CC must be provided by user
CXX=$(CXX_PVFMM)
CXXFLAGS=$(CXXFLAGS_PVFMM)
LDLIBS=$(LDLIBS_PVFMM)

RM = rm -f
MKDIRS = mkdir -p

BINDIR = ./bin
SRCDIR = ./src
OBJDIR = ./obj
INCDIR = ./include

TARGET_BIN = \
       $(BINDIR)/example1 \
       $(BINDIR)/example2 \
       $(BINDIR)/fmm_pts \
       $(BINDIR)/fmm_cheb

all : $(TARGET_BIN)

$(BINDIR)/%: $(SRCDIR)/%.f90
	-@$(MKDIRS) $(dir $@)
	$(FC) $(CXXFLAGS) -I$(INCDIR) $^ $(LDLIBS) -o $@

$(BINDIR)/%: $(SRCDIR)/%.c
	-@$(MKDIRS) $(dir $@)
	$(CC) $(CXXFLAGS) -I$(INCDIR) $^ $(LDLIBS) -o $@

ifeq ($(INTEL_OFFLOAD_OK),yes)

$(BINDIR)/%: $(SRCDIR)/%.cpp
	-@$(MKDIRS) $(dir $@)
	$(CXX) $(CXXFLAGS) -no-offload         -I$(INCDIR) $^ $(LDLIBS) -o $@
	$(CXX) $(CXXFLAGS)                     -I$(INCDIR) $^ $(LDLIBS) -o $@_async
	$(CXX) $(CXXFLAGS) -D__DEVICE_SYNC__=1 -I$(INCDIR) $^ $(LDLIBS) -o $@_mic

else

$(BINDIR)/%: $(SRCDIR)/%.cpp
	-@$(MKDIRS) $(dir $@)
	$(CXX) $(CXXFLAGS) -I$(INCDIR) $^ $(LDLIBS) -o $@

endif

clean:
	$(RM) -r $(BINDIR)/* $(OBJDIR)/*
	$(RM) *~ */*~

