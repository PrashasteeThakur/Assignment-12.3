# Assignment-12.3

Explain in detail.
1.What is meant by FlumeNG ? 

Ans.Apache Flume is a distributed, reliable, and available system for efficiently collecting, aggregating and moving large amounts of log data from many different sources to a centralized data store. Flume is currently undergoing incubation at The Apache Software Foundation. Flume NG is work related to new major revision of Flume.Components in FlumeNG are similar to Flume but some new concepts are introduced in FlumeNG.These concepts help in simplifying the architecture, implementation, configuration and deployment of Flume.

Flow Pipeline
A flow in Flume NG starts from the client. The client transmits the event to it’s next hop destination. This destination is an agent. More precisely, the destination is a source operating within the agent. The source receiving this event will then deliver it to one or more channels. The channels that receive the event are drained by one or more sinks operating within the same agent. If the sink is a regular sink, it will forward the event to it’s next-hop destination which will be another agent. If instead it is a terminal sink, it will forward the event to it’s final destination. Channels allow the decoupling of sources from sinks using the familiar producer-consumer model of data exchange. This allows sources and sinks to have different performance and runtime characteristics and yet be able to effectively use the physical resources available to the system.

Reliability and Failure Handling
Flume NG uses channel-based transactions to guarantee reliable message delivery. When a message moves from one agent to another, two transactions are started, one on the agent that delivers the event and the other on the agent that receives the event. In order for the sending agent to commit it’s transaction, it must receive success indication from the receiving agent. The receiving agent only returns a success indication if it’s own transaction commits properly first. This ensures guaranteed delivery semantics between the hops that the flow makes.
This mechanism also forms the basis for failure handling in Flume NG. When a flow that passes through many different agents encounters a communication failure on any leg of the flow, the affected events start getting buffered at the last unaffected agent in the flow. If the failure is not resolved on time, this may lead to the failure of the last unaffected agent, which then would force the agent before it to start buffering the events. Eventually if the failure occurs when the client transmits the event to its first-hop destination, the failure will be reported back to the client which can then allow the application generating the events to take appropriate action.
On the other hand, if the failure is resolved before the first-hop agent fails, the buffered events in various agents downstream will start draining towards their destination. Eventually the flow will be restored to its original characteristic throughput levels.


2.Can Flume provides 100 % reliability to the data flow? 

Ans.Yes.it provides end to end reliability of the flow.By default flume uses a transactional approach in the data flow.sources and sinks encapsulated in a transactional repository provided by channels.These channels are responsible to pass reliably from end to end in the flow.So it provide 100% reliability to the data flow.


3.Can Flume can distributes data to multiple destinations? 

Ans.Yes, It supports multiplexing flow.The event flows from one source to multiple channels and multiple destinations.It is achieved by defining a flow multiplexer.


4.Explain about the different channel types in Flume. And which channel type is faster?

Ans.The 3 different built in channel types available in Flume are-
a.MEMORY Channel – Events are read from the source into memory and passed to the sink.
b.JDBC Channel – JDBC Channel stores the events in an embedded Derby database.
c.FILE Channel– File Channel writes the contents to a file on the file system after reading the event from a source. The file is deleted only  after the contents are successfully delivered to the sink.
MEMORY Channel is the fastest channel among the three however has the risk of data loss. The channel that you choose completely depends on the nature of the big data application and the value of each event.


