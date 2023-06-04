
CANARD_BASE=../..

CC=gcc
CFLAGS = -g -Wall -Idsdl_generated/include -I$(CANARD_BASE) -I$(CANARD_BASE)/drivers/socketcan

LIBS=$(CANARD_BASE)/canard.c

# add socketcan driver for linux
LIBS+=$(CANARD_BASE)/drivers/socketcan/socketcan.c

# add in generated code
LIBS+=dsdl_generated/src/uavcan.protocol.NodeStatus.c
LIBS+=dsdl_generated/src/uavcan.protocol.GetNodeInfo_res.c
LIBS+=dsdl_generated/src/uavcan.equipment.esc.RawCommand.c
LIBS+=dsdl_generated/src/uavcan.equipment.esc.Status.c
LIBS+=dsdl_generated/src/uavcan.protocol.dynamic_node_id.Allocation.c

all: esc_node

DSDL:
	git clone --depth 1 https://github.com/DroneCAN/DSDL

dronecan_dsdlc:
	git clone --depth 1 https://github.com/DroneCAN/dronecan_dsdlc

dsdl_generated: DSDL dronecan_dsdlc
	python3 dronecan_dsdlc/dronecan_dsdlc.py -O dsdl_generated DSDL/dronecan DSDL/uavcan DSDL/com DSDL/ardupilot

esc_node: dsdl_generated esc_node.c $(LIBS)
	$(CC) -o esc_node esc_node.c $(LIBS) $(CFLAGS)

clean:
	rm -rf esc_node DSDL dsdl_generated dronecan_dsdlc