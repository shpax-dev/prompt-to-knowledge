# Network Reliability and RPC

A distributed system spans machines and communication links. A timeout can mean a request was lost, the response was lost, a peer failed, or the system is merely slow. One participant cannot infer a remote operation's outcome from silence alone.

## UDP and TCP

UDP provides datagrams without guaranteeing delivery, ordering, or duplicate suppression. An application can add its own reliability protocol. A checksum alone does not make a message reliably delivered.

TCP provides a reliable, ordered byte-stream abstraction while the connection works. Segments can arrive out of order at the receiver's network stack; the byte stream delivered to the application remains ordered. TCP sequence numbers count bytes, while simple classroom diagrams may number abstract messages.

Acknowledgments, sequence numbers, retransmission, buffering, and adaptive timeouts build the reliable transport. Very short timeouts can create unnecessary retransmissions and overhead. Backoff helps avoid worsening overload.

Reliable delivery does not by itself guarantee exactly-once execution of a remote application action. Retrying a non-idempotent operation after a lost acknowledgment can repeat the effect unless the application protocol handles duplicates.

## Remote procedure calls

Client and server stubs marshal arguments into a shared wire representation, transmit the request, unmarshal it, invoke the procedure, and serialize the result. Serialization addresses representation differences such as byte ordering; simply copying local pointers does not make them meaningful on another machine.

An RPC looks syntactically like a function call but adds latency and partial-failure semantics. Classify operations as idempotent or non-idempotent when deciding whether retries are safe.

Source: `CS537 - Final Review.pdf`, PDF pp. 54-59; `FA24 - exam.pdf`, pp. 3 and 13, questions 28 and 52. Related: [[review/advanced/distributed-file-systems|remote file access]].
