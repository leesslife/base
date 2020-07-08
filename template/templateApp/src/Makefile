TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# xxxRecord.h will be created from xxxRecord.dbd
DBDINC += xxxRecord

# Install xxxSupport.dbd into <top>/dbd
DBD += xxxSupport.dbd

# Build an IOC support library
LIBRARY_IOC += templateSupport

# Compile and add the code to the support library
templateSupport_SRCS += xxxRecord.c
templateSupport_SRCS += devXxxSoft.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application.
# This is required for Windows DLL builds.
templateSupport_SRCS += dbSubExample.c
templateSupport_SRCS += templateHello.c
templateSupport_SRCS += initTrace.c

templateSupport_LIBS += $(EPICS_BASE_IOC_LIBS)


# Build the IOC application
PROD_IOC = template

# template.dbd will be created and installed
DBD += template.dbd

# template.dbd will include these files:
template_DBD += base.dbd
template_DBD += xxxSupport.dbd
template_DBD += dbSubExample.dbd
template_DBD += templateHello.dbd
template_DBD += initTrace.dbd

# template_registerRecordDeviceDriver.cpp derives from template.dbd
template_SRCS += template_registerRecordDeviceDriver.cpp

# Build the main IOC entry point where needed
template_SRCS_DEFAULT += templateMain.cpp
template_SRCS_vxWorks += -nil-

# Link in the code from our support library
template_LIBS += templateSupport

# To build SNL programs, SNCSEQ must be defined
# in the <top>/configure/RELEASE file
ifneq ($(SNCSEQ),)
    # Build sncExample into templateSupport
    sncExample_SNCFLAGS += +r
    template_DBD += sncExample.dbd
    # A .stt sequence program is *not* pre-processed:
    templateSupport_SRCS += sncExample.stt
    templateSupport_LIBS += seq pv
    template_LIBS += seq pv

    # Build sncProgram as a standalone program
    PROD_HOST += sncProgram
    sncProgram_SNCFLAGS += +m
    # A .st sequence program *is* pre-processed:
    sncProgram_SRCS += sncProgram.st
    sncProgram_LIBS += seq pv
    sncProgram_LIBS += $(EPICS_BASE_HOST_LIBS)
endif

# Finally link IOC to the EPICS Base libraries
template_LIBS += $(EPICS_BASE_IOC_LIBS)

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE

