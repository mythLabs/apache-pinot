https://docs.pinot.apache.org/basics/data-import/pinot-stream-ingestion/import-from-apache-kafka#upload-schema-and-table
---------------------------------------------
docker run \
    --network=pinot-demo_default \
    -v /home/amit/pinot:/home/amit/pinot \
    --name pinot-streaming-table-creation1 \
    apachepinot/pinot:latest AddTable \
    -schemaFile /home/amit/pinot/employee-schema.json \
    -tableConfigFile /home/amit/pinot/employee-table-realtime.json \
    -controllerHost manual-pinot-controller \
    -controllerPort 9000 \
    -exec

------------------------------------------------
kafka-console-consumer.sh \
    --bootstrap-server kafka:9092 \
    --topic quickstart.sampleData \
    --from-beginning

------------------
kafka-topics.sh --list --bootstrap-server kafka:9092


--------------------------

curl -X POST \
     -H "Content-Type: application/json" \
     --data '
     {"name": "mongo-source",
      "config": {
         "connector.class":"com.mongodb.kafka.connect.MongoSourceConnector",
         "connection.uri":"mongodb://mongo1:27017/?replicaSet=rs0",
         "database":"quickstart",
         "collection":"sampleData",
         "publish.full.document.only": true,
         "key.converter":"org.apache.kafka.connect.storage.StringConverter",
         "value.converter":"org.apache.kafka.connect.storage.StringConverter",
         "key.converter.schemas.enable":false,
         "value.converter.schemas.enable":false, 
         "pipeline":"[{\"$match\": {\"operationType\": \"insert\"}}]"
         }
     }
     ' \
     http://connect:8083/connectors -w "\n"

------------
https://www.mongodb.com/docs/kafka-connector/current/quick-start/#send-the-contents-of-a-document-through-your-connectors

-----------------
https://docs.pinot.apache.org/basics/data-import/pinot-stream-ingestion/import-from-apache-kafka
-----------------------
db.sampleData.insertOne({"ID": 100,"firstName": "Natalie","lastName": "Jones","timestamp": 1571900400000})

------------
