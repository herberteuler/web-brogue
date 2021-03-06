
SDL_FLAGS = `sdl-config --cflags` `sdl-config --libs`
LIBTCODDIR=src/libtcod-1.5.2
CFLAGS=-IUnBrogueCode -IPlatformCode -Wall -Wno-parentheses ${DEFINES}
RELEASENAME=brogue-1.7.4
LASTTARGET := $(shell ./brogue --target)
CC ?= gcc

ifeq (${LASTTARGET},both)
all : both
else ifeq (${LASTTARGET},curses)
all : curses
else ifeq (${LASTTARGET},tcod)
all : tcod
else ifeq (${LASTTARGET}, web)
all : web
else
all : both
endif

%.o : %.c Makefile UnBrogueCode/Rogue.h UnBrogueCode/IncludeGlobals.h
	$(CC) $(CFLAGS) -o $@ -c $< 

BROGUEFILES=UnBrogueCode/Architect.o \
	UnBrogueCode/Combat.o \
	UnBrogueCode/Dijkstra.o \
	UnBrogueCode/Globals.o \
	UnBrogueCode/IO.o \
	UnBrogueCode/Items.o \
	UnBrogueCode/Light.o \
	UnBrogueCode/Monsters.o \
	UnBrogueCode/Buttons.o \
	UnBrogueCode/Movement.o \
	UnBrogueCode/Recordings.o \
	UnBrogueCode/RogueMain.o \
	UnBrogueCode/Random.o \
	UnBrogueCode/MainMenu.o \
	PlatformCode/main.o \
	PlatformCode/platformdependent.o \
	PlatformCode/curses-platform.o \
	PlatformCode/tcod-platform.o \
	PlatformCode/term.o \
	PlatformCode/web-platform.o

TCOD_DEF = -DBROGUE_TCOD -I$(LIBTCODDIR)/include
TCOD_DEP = ${LIBTCODDIR}
TCOD_LIB = -L. -L${LIBTCODDIR} ${SDL_FLAGS} -ltcod -Wl,-rpath,.

CURSES_DEF = -DBROGUE_CURSES
CURSES_LIB = -lncurses -lm

WEB_DEF = -DBROGUE_WEB
WEB_LIB = -lm

tcoddebug : CFLAGS += -g 
tcoddebug : tcod
tcod : DEPENDENCIES += ${TCOD_DEP}
tcod : DEFINES += ${TCOD_DEF}
tcod : LIBRARIES += ${TCOD_LIB}

cursesdebug : CFLAGS += -g 
cursesdebug : curses
curses : DEFINES = ${CURSES_DEF}
curses : LIBRARIES = ${CURSES_LIB}

both : DEPENDENCIES += ${TCOD_DEP}
both : DEFINES += ${TCOD_DEF} ${CURSES_DEF}
both : LIBRARIES += ${TCOD_LIB} ${CURSES_LIB}
	
webdebug : CFLAGS += -g 
webdebug : web
web : DEFINES += ${WEB_DEF}
web : LIBRARIES += ${WEB_LIB}

ifeq (${LASTTARGET},both)
both : bin/brogue
tcod : clean bin/brogue
curses : clean bin/brogue
web : clean bin/brogue
else ifeq (${LASTTARGET},curses)
curses : bin/brogue
tcod : clean bin/brogue
both : clean bin/brogue
web : clean bin/brogue
else ifeq (${LASTTARGET},tcod)
tcod : bin/brogue
curses : clean bin/brogue
both : clean bin/brogue
web : clean bin/brogue
else ifeq (${LASTTARGET}, web)
web : bin/brogue
tcod : clean bin/brogue
curses : clean bin/brogue
both : clean bin/brogue
else
both : bin/brogue
curses : bin/brogue
tcod : bin/brogue
web : bin/brogue
endif

.PHONY : clean both curses tcod web tar

bin/brogue : ${DEPENDENCIES} ${BROGUEFILES}
	$(CC) -O2 -o bin/brogue ${BROGUEFILES} ${LIBRARIES} -Wl,-rpath,.

clean : 
	rm -f UnBrogueCode/*.o PlatformCode/*.o bin/brogue

${LIBTCODDIR} :
	src/get-libtcod.sh

tar : both
	rm -f ${RELEASENAME}.tar.gz
	tar --transform 's,^,${RELEASENAME}/,' -czf ${RELEASENAME}.tar.gz \
	Makefile \
	brogue \
	$(wildcard *.sh) \
	$(wildcard *.rtf) \
	readme \
	$(wildcard *.txt) \
	bin/brogue \
	bin/keymap \
	bin/icon.bmp \
	bin/brogue-icon.png \
	$(wildcard bin/fonts/*.png) \
	$(wildcard bin/*.so) \
	$(wildcard src/*.sh) \
	$(wildcard UnBrogueCode/*.c) \
	$(wildcard UnBrogueCode/*.h) \
	$(wildcard PlatformCode/*.c) \
	$(wildcard PlatformCode/*.h)

