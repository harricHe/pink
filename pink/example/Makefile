CXX = g++
ifeq ($(__PERF), 1)
	CXXFLAGS = -O0 -g -pg -pipe -fPIC -D__XDEBUG__ -W -Wwrite-strings -Wpointer-arith -Wreorder -Wswitch -Wsign-promo -Wredundant-decls -Wformat -D_GNU_SOURCE -D__STDC_FORMAT_MACROS -std=c++11 -Wno-unused-variable
else
	CXXFLAGS = -O2 -pipe -fPIC -W -Wwrite-strings -Wpointer-arith -Wreorder -Wswitch -Wsign-promo -Wredundant-decls -Wformat -Wall -D_GNU_SOURCE -D__STDC_FORMAT_MACROS -std=c++11 -Wno-unused-variable -Wno-maybe-uninitialized -Wno-unused-parameter
endif
OBJECT = holy_srv holy_srv_chandle dispatch_srv myproto_cli http_server https_server redis_cli bg_thread simple_http_server

ifndef SLASH_PATH
SLASH_PATH = ../third/slash
endif

SRC_DIR = ./

OUTPUT = ./output

LIB_PATH = -L../lib/ \
					 -L$(SLASH_PATH)/slash/lib/

LIBS = -lpink \
			 -lslash \
			 -lprotobuf \
			 -lcrypto \
			 -lssl \
			 -lpthread

INCLUDE_PATH = -I../../ \
							 -I$(SLASH_PATH)/

.PHONY: all clean version


BASE_BOJS := $(wildcard $(SRC_DIR)/*.cc)
BASE_BOJS += $(wildcard $(SRC_DIR)/*.c)
BASE_BOJS += $(wildcard $(SRC_DIR)/*.cpp)
OBJS = $(patsubst %.cc,%.o,$(BASE_BOJS))

all: $(OBJECT)
	rm -rf $(OUTPUT)
	mkdir -p $(OUTPUT)
	mkdir -p $(OUTPUT)/bin
	mkdir -p $(OUTPUT)/log
	cp $(OBJECT) $(OUTPUT)/bin/
	rm -rf $(OBJECT)
	@echo "Success, go, go, go..."


holy_srv: myholy_srv.cc myproto.pb.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

holy_srv_chandle: myholy_srv_chandle.cc myproto.pb.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

dispatch_srv: mydispatch_srv.cc myproto.pb.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

myproto_cli: myproto_cli.cc myproto.pb.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

redis_cli: redis_cli_test.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

bg_thread: bg_thread.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

http_server: http_server.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

https_server: https_server.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS) -D__ENABLE_SSL

simple_http_server: simple_http_server.cc
	$(CXX) $(CXXFLAGS) -o $@ $^ $(INCLUDE_PATH) $(LIB_PATH) $(LIBS)

myproto.pb.cc: myproto.proto
	protoc -I=$(SRC_DIR) --cpp_out=$(SRC_DIR) ./$^

$(OBJS): %.o : %.cc
	$(CXX) $(CXXFLAGS) -c $< -o $@ $(INCLUDE_PATH) 

clean: 
	rm -rf $(OUTPUT)
	rm -rf $(SRC_DIR)/log
	rm -f $(SRC_DIR)/*.o
	rm -rf $(OBJECT)

