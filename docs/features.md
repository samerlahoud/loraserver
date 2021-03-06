# Features

## Device classes

### Class A

LoRa Server has full support for Class-A devices. Received payloads are
de-duplicated (in case they are picked up by multiple gateways), after which
they are forwarded to the [application server](https://docs.loraserver.io/lora-app-server/).
When a receive-window occurs, LoRa Server will poll the [application server](https://docs.loraserver.io/lora-app-server/)
for downlink data. By polling instead of letting the application server push the
data, the application server is able to respect the maximum payload size for the
data-rate used for the downlink transmission.

### Class B

Todo.

### Class C

Todo.

## Confirmed data up / down

Both uplink and downlink confirmed data is handled by LoRa Server. In case of
a downlink (confirmed) payload, LoRa Server will keep the payload in its queue
until it has been acknowledged by the node.

## Node activation

LoRa Server has support for both ABP (activation by personalization) and OTAA
(over the air activation). In case of ABP, the [application server](https://docs.loraserver.io/lora-app-server/)
provisions LoRa Server with a node-session. In case of OTAA, LoRa Server will
call the [application server](https://docs.loraserver.io/lora-app-server/) with
the received join-request and in case of a positive response, it will transmit
the join-accept to the node.

## Network-controller interface

Although a network-controller component is still to be implemented, it is
already possible to receive MAC commands received by the node by
implementing the network-controller interface as specified by the 
[api/nc/nc.proto](https://github.com/brocaar/loraserver/tree/master/api/nc/nc.proto)
file. See also the [api](api.md) documentation.

## Receive windows

Through OTAA and ABP, it is possible to configure which RX window to use for
downlink transmissions. This also includes the parameters like data-rate
(for RX2) and the delay to use.

## Relax frame-counter

A problem with many ABP devices is that after a power-cycle, the frame-counter
of the device is reset. Since this reset is not known by LoRa Server it means
that all payloads with a frame-counter smaller or equal than the known counter
get rejected. In order to work around this issue it is possible to enable
the relax frame-counter mode. Important to know, this compromises security!

## ISM bands

As different regions have have different regulations regarding the license-free
bands, you have to specify the ISM band to operate on when starting LoRa Server.
At this moment the following ISM bands have been implemented:

- AU 915-928
- CN 470-510
- EU 863-870
- US 902-928

For the following ISM bands help is needed to verify the correctness of the
implementation:

-  EU 433 ISM band testers ([issues/49](https://github.com/brocaar/loraserver/issues/49))
-  CN 779-787 ISM band testers ([issues/50](https://github.com/brocaar/loraserver/issues/50))
