################################################################################
#                        NORTH CAROLINA STATE UNIVERSITY
#
#                               AnyCore Project
#
# AnyCore Copyright (c) 2007-2016 by Rangeen Basu Roy Chowdhury and 
# Eric Rotenberg. All Rights Reserved.
#
# AnyCore is based on FabScalar (c) 2007-2016 by Niket K. Choudhary, 
# Brandon H. Dwiel, and Eric Rotenberg.  All Rights Reserved.
#
# AnyCore is distributed under Berkeley License.
################################################################################

# Add additional flags
#DEFINES    = +define+SIM+USE_VPI+VERIFY
DEFINES    = +define+SIM+USE_VPI+VERIFY+PRINT_EN

CURRENT    = ../../..
CORE_NAME  = src

# Overwrite CONFIG to change the superset configuration.
CONFIG     = StaticConfig5
#CONFIG     = DynamicConfig

# The Verilog source files
PARAMFILE = $(CURRENT)/../$(CORE_NAME)/configs/$(CONFIG).v

FETCH    = $(CURRENT)/../$(CORE_NAME)/fetch/*.sv

DECODE   = $(CURRENT)/../$(CORE_NAME)/decode/*.sv

RENAME   = $(CURRENT)/../$(CORE_NAME)/rename/*.sv

DISPATCH = $(CURRENT)/../$(CORE_NAME)/dispatch/*.sv

ISSUEQ   = $(CURRENT)/../$(CORE_NAME)/issue/*.sv

REGREAD  = $(CURRENT)/../$(CORE_NAME)/regRead/*.sv

EXECUTE  = $(CURRENT)/../$(CORE_NAME)/execute/*.sv

WRITEBK  = $(CURRENT)/../$(CORE_NAME)/writeback/*.sv

LSU      = $(CURRENT)/../$(CORE_NAME)/lsu/*.sv

RETIRE   = $(CURRENT)/../$(CORE_NAME)/retire/*.sv

MISC     = $(PARAMFILE) \
           $(CURRENT)/../$(CORE_NAME)/ISA/SimpleScalar_ISA.v \
           $(CURRENT)/../$(CORE_NAME)/include/structs.svh \
           $(CURRENT)/../$(CORE_NAME)/lib/*.sv

MEM      = $(CURRENT)/../$(CORE_NAME)/rams/*.sv	\
           $(CURRENT)/../$(CORE_NAME)/rams_configurable/*.svh	\
           $(CURRENT)/../$(CORE_NAME)/rams_configurable/*.sv	\

TOP      = $(CURRENT)/../$(CORE_NAME)/fabscalar/*.sv

TESTBENCH	=	$(CURRENT)/../$(CORE_NAME)/testbenches/simulate.sv

#IODINE   = $(CURRENT)/../iodine/*.sv


# Combines all the files
FILES    = $(MISC) $(MEM) $(FETCH) $(DECODE) $(RENAME) $(DISPATCH) \
            $(ISSUEQ) $(REGREAD) $(EXECUTE) $(WRITEBK) $(RETIRE) $(TOP) \
					 	$(LSU) $(IODINE) $(TESTBENCH)


VPI_DIR = $(CURRENT)/functional-sim/vpi
VPI_INCDIR = $(VPI_DIR)/include
VPI_SRCDIR = $(VPI_DIR)/src
VPI_FLAGS  = -loadvpi :initializeSim.initializeSim,readOpcode_calltf.readOpcode_calltf,readOperand_calltf.readOperand_calltf
VPI_FILES = $(VPI_SRCDIR)/initializeSim.cpp \
            $(VPI_SRCDIR)/readOpcode.cpp \
            $(VPI_SRCDIR)/readOperand.cpp \
            $(VPI_SRCDIR)/readUnsignedByte.cpp \
            $(VPI_SRCDIR)/readSignedByte.cpp \
            $(VPI_SRCDIR)/readUnsignedHalf.cpp \
            $(VPI_SRCDIR)/readSignedHalf.cpp \
            $(VPI_SRCDIR)/readWord.cpp \
            $(VPI_SRCDIR)/writeByte.cpp \
            $(VPI_SRCDIR)/writeHalf.cpp \
            $(VPI_SRCDIR)/writeWord.cpp \
            $(VPI_SRCDIR)/getArchRegValue.cpp \
            $(VPI_SRCDIR)/copyMemory.cpp \
            $(VPI_SRCDIR)/getRetireInstPC.cpp \
            $(VPI_SRCDIR)/getArchPC.cpp \
            $(VPI_SRCDIR)/global_vars.cc \
            $(VPI_SRCDIR)/VPI_global_vars.cc \
            $(VPI_SRCDIR)/veri_memory.cc \
            $(VPI_SRCDIR)/read_config.cc \
            $(VPI_SRCDIR)/getPerfectNPC.cpp \
            $(VPI_SRCDIR)/funcsimRunahead.cpp \
            $(VPI_SRCDIR)/handleTrap.cpp \
            $(VPI_SRCDIR)/resumeTrap.cpp \
            $(VPI_SRCDIR)/register_systf.cpp

NCSC_RUNARGS = -access rwc -l run.log -ncsc_runargs "-DSIM_LINUX -I/usr/include -I/usr/local/include -I$(CURRENT)/functional-sim/include -I$(VPI_INCDIR) -L$(CURRENT) -L$(CURRENT)/functional-sim/libss-vpi/lib -lSS_VPI" 

# DPI files for Iodine
DPI_CC     = g++
DPI_DIR    = ../../../iodine
DPI_CFLAGS = -g -m32 -fPIC -shared -I$(DPI_DIR) -I`ncroot`/tools/inca/include
DPI_FILES  = $(DPI_DIR)/iodine.cc \
             $(DPI_DIR)/iodine_dpi.cpp 
IODINE_FLAGS = -sv_root ./ -sv_lib ./iodine_dpi.so +define+IODINE
DEBUG        = +define+PRINT_EN


run_nc:
	clear
	rm -rf *.log results/*
#ifdef CONFIG
#	cp $(CURRENT)/../$(CORE_NAME)/configs/$(CONFIG).v $(CURRENT)/../$(CORE_NAME)/FabScalarParam.v
#	#make clean
#endif
	irun -top worklib.simulate:sv $(DEFINES) $(NCSC_RUNARGS) $(FILES) $(VPI_FILES) $(VPI_FLAGS)

# Prints signals to the results/ directory and runs with Iodine tracing
debug: iodine_dpi.so 
	clear
	rm -rf *.log results/*
#ifdef CONFIG
#	cp $(CURRENT)/../$(CORE_NAME)/configs/$(CONFIG).v $(CURRENT)/../$(CORE_NAME)/FabScalarParam.v
#	#make clean
#endif
	irun -top worklib.simulate:sv $(IODINE_FLAGS) $(DEBUG) $(DEFINES) $(NCSC_RUNARGS) $(FILES) $(VPI_FILES) $(VPI_FLAGS)

# Runs with Iodine tracing
iodine: iodine_dpi.so 
	clear
	rm -rf *.log results/*
#ifdef CONFIG
#	cp $(CURRENT)/../$(CORE_NAME)/configs/$(CONFIG).v $(CURRENT)/../$(CORE_NAME)/FabScalarParam.v
#	#make clean
#endif
	irun -top worklib.simulate:sv $(IODINE_FLAGS) $(DEFINES) $(NCSC_RUNARGS) $(FILES) $(VPI_FILES) $(VPI_FLAGS)


iodine_dpi.so: $(DPI_FILES)
	$(DPI_CC) $(DPI_CFLAGS) -o iodine_dpi.so $(DPI_FILES)

# Runs with the gui
run_nc_g: 
	clear
	rm -rf *.log results/*
#ifdef CONFIG
#	cp $(CURRENT)/../$(CORE_NAME)/configs/$(CONFIG).v $(CURRENT)/../$(CORE_NAME)/FabScalarParam.v
#	#make clean
#endif
	irun -gui -top worklib.simulate:sv $(DEFINES)  $(NCSC_RUNARGS) $(FILES) $(VPI_FILES) $(VPI_FLAGS)

clean:
	rm -rf *.o libvpi.so INCA_libs *.log *.sl work irun.* results/* waves.shm* top outfile .simvision out.* iodine_dpi.so run.log* simvision*
