include ../../py/mkenv.mk

CROSS = 0

QSTR_DEFS = qstrdefsport.h

include $(TOP)/py/py.mk

CC = emcc -g4
LD = emcc -g4

INC += -I.
INC += -I$(TOP)
INC += -I$(BUILD)

CPP = clang -E

ifdef EMSCRIPTEN
    CPP += -isystem $(EMSCRIPTEN)/system/include/libc -cxx-isystem $(EMSCRIPTEN)/system/include/libcxx
endif

CFLAGS = -m32 -Wall -Werror $(INC) -std=c99 $(COPT)
LDFLAGS = -m32 -Wl,-Map=$@.map,--cref -Wl,--gc-sections

CFLAGS += -O0 -DNDEBUG
CFLAGS += -fdata-sections -ffunction-sections

SRC_LIB = $(addprefix lib/,\
	utils/interrupt_char.c \
	utils/stdout_helpers.c \
	utils/pyexec.c \
	mp-readline/readline.c \
	)

SRC_C = \
	main.c \
	mphalport.c \
	modutime.c \

SRC_QSTR += $(SRC_C)

OBJ = 
OBJ = $(PY_O) 
OBJ += $(addprefix $(BUILD)/, $(SRC_LIB:.c=.o))
OBJ += $(addprefix $(BUILD)/, $(SRC_C:.c=.o))

JSFLAGS = -O0 -s WASM=0

all: $(BUILD)/micropython.js

$(BUILD)/micropython.js: $(OBJ)
	$(ECHO) "LINK $(BUILD)/firmware.js"
	$(Q)emcc $(LDFLAGS) -o $(BUILD)/firmware.js $(OBJ) $(JSFLAGS)
	cat $(BUILD)/firmware.js > $@

min: $(BUILD)/micropython.js
	uglifyjs $< -c -o $(BUILD)/micropython.min.js

test: $(BUILD)/micropython.js $(TOP)/tests/run-tests
	$(eval DIRNAME=ports/$(notdir $(CURDIR)))
	cd $(TOP)/tests && MICROPY_MICROPYTHON=../ports/javascript/node_run.sh ./run-tests

include $(TOP)/py/mkrules.mk
