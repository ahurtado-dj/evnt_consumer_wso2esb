===============================
WSO2-ESB-CONSUMER
===============================

# seguir las instrucciones de : https://docs.wso2.com/display/ESB490/Kafka+Inbound+Protocol
# dentro de ellas: 
- copiar librerias de <KAFKA_HOME>/lib  a <ESB_HOME>/repository/components/lib
	kafka_2.11-0.8.2.2.jar
	metrics-core-2.2.0.jar
	scala-library-2.11.5.jar
	scala-parser-combinators_2.11-1.0.2.jar
	zkclient-0.3.jar
	zookeeper-3.4.6.jar
# crear mediacion acorde al ejemplo
# iniciar wso2 esb

crear ServiceBus/sequence: 


<sequence name="Usuarios_sync_KakfaConsumerSEQ" xmlns="http://ws.apache.org/ns/synapse">
  <log level="full"/>
  <drop/>
</sequence>

crear ServiceBus/inboundEnpoint

<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                 name="Usuarios_sync_KakfaConsumerEP"
                 sequence="Usuarios_sync_KakfaConsumerSEQ"
                 onError="fault"
                 protocol="kafka"
                 suspend="false">
   <parameters>
      <parameter name="interval">1000</parameter> 
      <parameter name="sequential">false</parameter>
      <parameter name="coordination">false</parameter>
      <parameter name="zookeeper.connect">192.168.90.30:2181</parameter>
      <parameter name="group.id">test-group</parameter>

      <parameter name="consumer.type">highlevel</parameter>
      <parameter name="content.type">application/json</parameter>

      <parameter name="topics">my-topic-test</parameter>
   </parameters>
</inboundEndpoint>

#configuracion simple
<inboundEndpoint xmlns="http://ws.apache.org/ns/synapse"
                 name="Usuarios_sync_KakfaConsumerEP"
                 sequence="Usuarios_sync_KakfaConsumerSEQ"
                 onError="fault"
                 protocol="kafka"
                 suspend="false">
   <parameters>
      <parameter name="interval">1000</parameter> 
      <parameter name="sequential">false</parameter>
      <parameter name="coordination">false</parameter>
      <parameter name="zookeeper.connect">192.168.90.30:2181</parameter>
      <parameter name="group.id">test-group</parameter>
      <parameter name="consumer.type">simple</parameter>
      <parameter name="content.type">application/json</parameter>

      <parameter name="simple.topic">my-topic-test</parameter> 
      <parameter name="simple.brokers">192.168.90.30</parameter>
      <parameter name="simple.port">9092</parameter>
      <parameter name="simple.partition">0</parameter>
      <parameter name="simple.max.messages.to.read">5</parameter>
   </parameters>
</inboundEndpoint>



# purgar el topico kafka
rm -rf /tmp/kafka-logs/newTopic-*
