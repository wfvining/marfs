all: marfs_garb


CFLAGS += -Wall -g 

ifdef DEBUG
   CFLAGS += -DUSE_STDOUT
endif

# relative paths to main marfs build
MARFS_FUSE = ../../../../fuse/src
LOGGING = ../../../../common/log/src
MARFS_CONFIG = ../../../../common/configuration/src
UTILITIES_COMMON = ../../common/src


#OBJS = $(MARFS_FUSE)/marfs_base.o $(MARFS_FUSE)/logging.o 
H    = marfs_gc.h

CFLAGS += -I$(MARFS_FUSE)
CFLAGS += -I$(LOGGING)
CFLAGS += -I$(LIBAWS4C)
CFLAGS += -I$(MARFS_CONFIG)
CFLAGS += -I$(UTILITIES_COMMON)
LDFLAGS += -L$(LIBAWS4C)
LDFLAGS += -L$(MARFS_FUSE)
LDFLAGS += -L$(MARFS_CONFIG)
LDFLAGS += -L$(UTILITIES_COMMON)
LIBS += -laws4c -lgpfs -lcurl -lm -lmarfs -lconfig -lutil_comm

#Build regular marfs_gc or debug version
marfs_gc: $(H) $(OBJS)  marfs_gc.c 
	gcc $(CFLAGS) $(LDFLAGS) -o marfs_gc marfs_gc.c $(OBJS) $(LIBS)

marfs_gc_debug: marfs_gc.c marfs_gc.h
	gcc $(CFLAGS) -DUSE_STDOUT $(LDFLAGS) -o marfs_gc marfs_gc.c $(OBJS) $(LIBS)


# From Jeff's marfs makefile.  This make sure an AWS4C path exists
# in order to build.
pre_req:
ifeq ($(LIBAWS4C),)
	$(error $$LIBAWS4C is empty.  Consider 'make ... LIBAWS4C=<DIR>')
else ifeq ($(wildcard $(LIBAWS4C)/libaws4c.a),)
	$(error couldnt find $$LIBAWS4C (in $(LIBAWS4C)/*).  Consider 'make ... LIBAWS4C=<DIR>')
endif

#Dependancy on AWS4C path
marfs_garb: marfs_gc.std
marfs_gc.std: pre_req
	@ $(MAKE) marfs_gc
marfs_gc.dbg: pre_req
	@ $(MAKE) marfs_gc_debug
       
%: %.o
	gcc -o $@ $< $(LDFLAGS) $(LIBS)

%.o: %.c %.h 
	gcc $(CFLAGS) -c -o $@ $<


# Seriously?  Make can't figure this out?
% : %.c
	@ $(MAKE) $*.o
	@ $(MAKE) $*

clean:

	$(RM) *.{o,i}
	$(RM) *~
	$(RM) marfs_gc   
