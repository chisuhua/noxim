##### LIBRARIES CONFIGURATION #####

SYSTEMC := ${SYSTEMC_HOME}
YAML    := libs/yaml-cpp


##### DIRECTORIES #####

#SYSTEMC_LIBS := $(wildcard $(SYSTEMC)/lib-*)
SYSTEMC_LIBS := $(SYSTEMC)/lib

SRCDIR  := ../src
SUBDIRS := $(filter %/,$(wildcard $(SRCDIR)/*/))
OBJDIR  := ./build

SRCS := $(wildcard $(SRCDIR)/*.cpp) $(wildcard $(addsuffix *.cpp,$(SUBDIRS)))
OBJS := $(subst $(SRCDIR),$(OBJDIR),$(SRCS:.cpp=.o))

MODULE := noxim


##### COMPILER OPTIONS #####

CXX      := g++
OPT      := -O3
# to enable logs with LOG macro, uncomment "-g DDEBUG" in the line below
# and recompile everythin (make clean)
#DEBUG    :=  -g -DDEBUG
OTHER    := -Wall -DSC_NO_WRITE_CHECK --std=c++17 # -Wno-deprecated
CXXFLAGS := $(OPT) $(OTHER) $(DEBUG)

INCDIR := -I$(SRCDIR) -isystem $(SYSTEMC)/include -I$(YAML)/include
LIBDIR := -L$(SRCDIR) -L$(SYSTEMC_LIBS) -L$(YAML)/lib

LIBS := -lsystemc -lm -lyaml-cpp 

SPACE := $(subst ,, )
VPATH := $(SRCDIR):$(subst $(SPACE),:,$(SUBDIRS))

##### RULES #####

$(MODULE): $(OBJS)
	$(CXX) $(LIBDIR) $^ $(LIBS) -o $@ 2>&1 | c++filt

$(OBJDIR)/%.o: %.cpp
	@mkdir -p $(dir $@)
	$(CXX) $(CXXFLAGS) $(INCDIR) -c $< -o $@

clean:
	rm -f $(OBJS) $(MODULE) *~

depend: 
	makedepend $(SRCS) -Y -f- 2>/dev/null | sed 's_$(SRCDIR)_$(OBJDIR)_' > Makefile.deps

-include Makefile.deps
