MAKE = gmake
CC = g++
#clang++

INCDIR =
LIBDIR =
BINDIR = ..
OBJDIR = .obj

GCC_VERSION = $(shell $(CC) --version 2>&1 | grep "(GCC)" | cut -d' ' -f3  | cut -d'.' -f1)
BSD_VERSION = $(shell uname -v 2>&1 | cut -d' ' -f2 | cut -d'.' -f1)
DB_VERSION = 40999
$(shell if [ ! -d $(OBJDIR) ]; then mkdir $(OBJDIR); fi)

TARGET = $(BINDIR)/db_r$(DB_VERSION)

MYFLAGS =  -Wno-long-long -ansi

CFLAGS =  -m32 -Wno-long-long -g -Wall -O2 -pipe -fno-rtti -fno-exceptions -pthread -D_THREAD_SAFE

#ifeq ($(GCC_VERSION), 4)
#CFLAGS += -mtune=i686
#else
#CFLAGS += -mcpu=i686
#endif

# boost
INCDIR += -I../../../Extern/include/boost

LIBDIR += -L../../libthecore/lib -L../../libsql -L../../libpoly -L../../libgame/lib

# Extern
INCDIR += -I../../../Extern/include
LIBDIR += -I../../../Extern/lib

# Project Library
INCDIR += -I/usr/local/include

#ifeq ($(BSD_VERSION), 7)
#INCDIR += -I../../libmysql/7.x-5.1.35
#LIBDIR += -L../../libmysql/7.x-5.1.35
#else
#INCDIR += -I../../libmysql/5.x-5.1.35
#LIBDIR += -L../../libmysql/5.x-5.1.35
#endif
#INCDIR += -I../../libmysql/mysql-5.5.35/include
#LIBDIR += -L../../libmysql/mysql-5.5.35/lib
#INCDIR += -I/usr/ports/databases/mysql51-client/work/mysql-5.1.73/include
#LIBDIR += -L/usr/ports/databases/mysql51-client/work/mysql-5.1.73/libmysql
INCDIR += -I/usr/local/include/mysql
LIBDIR += -L/usr/local/lib/mysql

#LIBS += -lmysqlclient -lz
LIBS += /usr/local/lib/mysql/libmysqlclient.a /usr/lib/libz.a

LIBS += -lthecore -lsql -lpoly -lgame -lm

SRCS =	Config.cpp NetBase.cpp Peer.cpp PeerBase.cpp Main.cpp Lock.cpp DBManager.cpp \
		Cache.cpp LoginData.cpp ClientManager.cpp ClientManagerPlayer.cpp ClientManagerLogin.cpp \
		ClientManagerBoot.cpp ClientManagerParty.cpp ClientManagerGuild.cpp GuildManager.cpp HB.cpp \
		PrivManager.cpp MoneyLog.cpp ItemAwardManager.cpp ClientManagerEventFlag.cpp Marriage.cpp \
		Monarch.cpp BlockCountry.cpp ItemIDRangeManager.cpp ClientManagerHorseName.cpp version.cpp \
		AuctionManager.cpp ProtoReader.cpp CsvReader.cpp 

OBJS = $(SRCS:%.cpp=$(OBJDIR)/%.o)

default: $(TARGET)

$(TARGET): $(OBJS)
	@echo linking ...
	@$(CC) $(CFLAGS) $(LIBDIR) $(OBJS) $(LIBS) -o $(TARGET)
	@touch version.cpp

$(OBJDIR)/%.o: %.cpp
	@echo compile $<
	@$(CC) $(CFLAGS) $(INCDIR) -c $< -o $@

$(OBJDIR)/version.o: version.cpp
	@$(CC) $(CFLAGS) -D__USER__=\"$(USER)\" -D__HOSTNAME__=\"$(HOSTNAME)\" -D__PWD__=\"$(PWD)\" -D__P4_VERSION__=\"$(DB_VERSION)\" -c $< -o $@
	@echo compile $<

$(OBJDIR):
	@mkdir $(OBJDIR)

#gcc42: CC = g++
#	default
#gcc48: CC = g++48
#	default
#clang: CC = clang++
#	default

clean:
	@rm -f $(OBJS) $(BINDIR)/db_r*

dep:
	@touch Depend
	makedepend -fDepend $(INCDIR) -I/usr/include/c++/3.3 -I/usr/include/c++/4.2 -p$(OBJDIR)/ $(SRCS) 2> /dev/null

sinclude Depend
