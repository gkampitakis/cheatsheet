<h1 align="center">Kafka</h1>

## Commands

- Read Messages of a topic

```bash
kafka-console-consumer --bootstrap-server
```

- List all topics

```bash
kafka-topics --list --zookeeper <zookeeper-host>
```

- Update number of partitions in a topic

```bash
kafka-topics --zookeeper <zookeeper-host> --alter --topic <topic> --partitions 6
```

- Get number of partitions in a topic

```bash
kafka-topics --describe --bootstrap-server <server-host> --topic <topic>
```

- Describe a consumer group

```bash
kafka-consumer-groups --bootstrap-server <server-host> --describe --group <group-name>
```

- Get all topics

```bash
kafka-topics --list --bootstrap-server <server-host>
```

- Get all consumers

```bash
kafka-consumer-groups --bootstrap-server <server-host> --list
```

- Get number of messages in a topic (without consuming them)

```bash
kafka-run-class kafka.tools.GetOffsetShell --broker-list <broker-list> --topic <topic> --time -1 --offsets 1 | awk -F  ":" '{sum += $3} END {print sum}'
```

- Updating a topic's config

```bash
kafka-configs --zookeeper <zookeper-host> --alter --entity-type topics --entity-name <topic> --add-config cleanup.policy=compact
```

- Check the oldest message

```bash
kafka-console-consumer --bootstrap-server <server-host> --topic <topic> --max-messages 1 --from-beginning
```

- Create topic

```bash
kafka-topics --zookeeper <zookeepeer-host> --create --replication-factor 1 --partitions 3 --topic <topic>
```

- Delete a topic

```bash
kafka-topics --delete --zookeeper <zookeper-host> --topic <topic>
```
