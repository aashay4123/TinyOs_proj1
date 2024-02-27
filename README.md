# Project Report: Simulation of a Simple Wireless Sensor Network Using TinyOS

## Introduction

Wireless Sensor Networks (WSNs) are crucial for various applications, including environmental monitoring, healthcare, and home automation. Simulating these networks is essential for understanding their behavior and performance under different conditions. This project utilizes TinyOS, a flexible, component-based operating system designed for low-power wireless devices, to simulate a WSN with two nodes. The simplicity of the setup allows for an in-depth examination of basic communication mechanisms in TinyOS.

## Code Overview

The system comprises two nodes: a sensor node responsible for collecting and transmitting data, and a sink node that receives this data. Both nodes operate within the same communication channel, defined by `AM_CHANNEL`.

## Code Implementation

The implementation involves several key files:
- **Makefile**: Specifies the component to be used for the application, `Send_receiveAppC`.
- **Send_receive.h**: Defines the communication channel and timer period.
- **Send_receiveAppC.nc**: Sets up the component connections for the application.
- **Send_receiveC.nc**: Contains the main logic for data transmission and reception.

## Key Components and Interfaces

- **AMSenderC and AMReceiverC**: Used for sending and receiving packets.
- **TimerMilliC**: Manages periodic events for data transmission.
- **LedsC**: Provides visual feedback of the operation (e.g., data reception).

## Simulation Setup

The simulation was set up to test the basic communication between the two nodes, focusing on transmitting a counter value from the sensor node to the sink node at regular intervals defined by `TIMER_PERIOD`.

## Simulation Run

1. **Initialization**: Upon booting, the sensor node starts the active message module (`AMControl.start()`).
2. **Timer Events**: The periodic timer (`Timer0`) triggers the creation and sending of messages containing the node ID and an incrementing counter.
3. **Message Sending**: When the timer fires, the sensor node prepares a message and attempts to send it. If successful, the `busy` flag is set to prevent overlapping send operations.
4. **Message Reception**: The sink node receives messages, extracts the counter value, and updates the LEDs accordingly.

## Problems Encountered

- **Packet Collision**: Due to the simplicity of the communication protocol, packet collisions can occur under higher network loads, leading to data loss.
- **Channel Interference**: Using a single, hard-coded communication channel (`AM_CHANNEL = 3`) can lead to interference in dense networks or when multiple networks operate in proximity.
- **Busy Waiting**: The `busy` flag is used to prevent the node from sending a new message before the previous one is sent. However, this can lead to inefficiencies in scenarios where the node could perform other tasks while waiting.

## Tricks and Solutions

- **Retransmission Strategy**: Implementing a basic retransmission strategy could mitigate the effects of packet collisions.
- **Event-driven Programming**: Making full use of TinyOS's event-driven model could improve efficiency, allowing the node to handle other tasks or enter a low-power state while waiting for the `sendDone` event.
- **Adaptive Channel Selection**: Implementing a scheme for dynamic channel selection could mitigate interference problems.
- **Queue Management**: Introducing a message queue could prevent message loss by storing messages to be sent once the `busy` flag is cleared.

## Conclusion

This project demonstrated the basic capabilities of TinyOS in simulating a wireless sensor network with minimal nodes. Despite its simplicity, the simulation highlighted essential aspects of WSN communication, such as packet transmission, event handling, and potential challenges like data collision and system efficiency. Future work could expand on this foundation by implementing more sophisticated communication protocols, energy management strategies, and exploring the impacts of network scale on performance.
