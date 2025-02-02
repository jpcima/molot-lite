CC=g++
LD=$(CC)
CFLAGS=-O2 -g -fvisibility=hidden
INSTALL_DIR = /usr/local/lib/lv2
ifndef ARCHNAME
OEXT=.o
SOEXT=.so
ARCCFLAG=
ARCLFLAG=
DLLNAME=Molot_Stereo_Lite
else
DLLNAME=so
OEXT=_$(ARCHNAME).o
SOEXT=.$(ARCHNAME)
ifeq ($(ARCHNAME),x86_64)
ARCCFLAG=-m64
else
ARCCFLAG=-m32
endif
ARCLFLAG=-melf_$(ARCHNAME)
endif
ifdef LV2CREATE
UNAME := $(shell uname -m)
COPYNAME=so
else
UNAME=so
COPYNAME=Molot_Stereo_Lite
endif
BUNDLE=Molot_Stereo_Lite.lv2
OBJECTS=Molot_Stereo_Lite$(OEXT) moloteng$(OEXT)
all: $(DLLNAME)$(SOEXT) ladspa$(SOEXT)

Molot_Stereo_Lite$(OEXT): Molot_Stereo_Lite.cpp Molot_Stereo_Lite.h plugin.h
	$(CC) $(ARCCFLAG) -Wall -fPIC $(CFLAGS) -o Molot_Stereo_Lite$(OEXT) -c Molot_Stereo_Lite.cpp

plugin$(OEXT): plugin.cpp Molot_Stereo_Lite.h plugin.h
	$(CC) $(ARCCFLAG) -Wall -fPIC $(CFLAGS) -o plugin$(OEXT) -c plugin.cpp

moloteng$(OEXT): moloteng.cpp plugin.h Molot_Stereo_Lite.h moloteng.h
	$(CC) $(ARCCFLAG) -Wall -fPIC $(CFLAGS) -o moloteng$(OEXT) -c moloteng.cpp

$(DLLNAME)$(SOEXT): $(OBJECTS) plugin$(OEXT)
	$(LD) $(ARCLFLAG) -o $(DLLNAME)$(SOEXT) plugin$(OEXT) $(OBJECTS) -shared

ladspa$(OEXT): ladspa.cpp Molot_Stereo_Lite.h plugin.h
	$(CC) $(ARCCFLAG) -Wall -fPIC $(CFLAGS) -o ladspa$(OEXT) -c ladspa.cpp

ladspa$(SOEXT): $(OBJECTS) ladspa$(OEXT)
	$(LD) $(ARCLFLAG) -o ladspa$(SOEXT) ladspa$(OEXT) $(OBJECTS) -shared

install:
	mkdir -p $(INSTALL_DIR)/$(BUNDLE)
	cp -f manifest.ttl $(INSTALL_DIR)/$(BUNDLE)/manifest.ttl
	cp -f Molot_Stereo_Lite.ttl $(INSTALL_DIR)/$(BUNDLE)/Molot_Stereo_Lite.ttl
	cp -f $(COPYNAME).$(UNAME) $(INSTALL_DIR)/$(BUNDLE)/Molot_Stereo_Lite.so
	chmod 775 $(INSTALL_DIR)/$(BUNDLE)/*.ttl

uninstall:
	rm -rf $(INSTALL_DIR)/$(BUNDLE)

clean:
	rm -f *.o *.so
