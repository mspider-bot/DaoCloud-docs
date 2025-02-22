# RabbitMQ data migration

Data for RabbitMQ includes metadata (RabbitMQ users, vhosts, queues, exchanges, and bindings) and message data, where message data is stored in a separate message store.

Due to business needs, it is required to migrate the data on the `rabbitmq-cluster-a` cluster to the `rabbitmq-cluster-b` cluster.

## Data Migration Steps

!!! info

    Starting with V3.7.0, RabbitMQ stores all message data in the `msg_stores/vhosts` directory, with subdirectories for each vhost.
    Each vhost directory is hash-named and contains a `.vhost` file with the vhost name, so that a particular vhost's message set can be backed up individually.
    Learn about [more information](https://www.rabbitmq.com/backup.html).

RabbitMQ data migration can adopt the following two solutions:

- Option 1: Do not migrate data, switch the production end first, and then switch the consumer end.
- Option 2: Migrate the data first, and then switch the production end and the consumer end at the same time.

### Option One

Without migrating data, switch the production end first, and then switch the consumer end.

#### Operating procedures

1. Switch the message producer to the cluster `rabbitmq-cluster-b`, and stop producing messages to the `rabbitmq-cluster-a` cluster.
2. The consumer consumes the messages in `rabbitmq-cluster-a` and `rabbitmq-cluster-b` clusters at the same time. When all the messages in the `rabbitmq-cluster-a` cluster are consumed, switch the message consumer to `rabbitmq -cluster-b` In the cluster, the data migration is completed.

#### Authentication method

- Viewed on the RabbitMQ Management Web UI page.

    ![view](../images/migrate01.png)

- Call the API to view

    ```shell
    curl -s -u username:password -XGET http://ip:port/api/overview
    ```

    Parameter Description:

    - username: the account of the RabbitMQ Management WebUI using the `rabbitmq-cluster-a` cluster
    - password: the password for the RabbitMQ Management WebUI using the `rabbitmq-cluster-a` cluster
    - ip: the IP address of the RabbitMQ Management WebUI using the `rabbitmq-cluster-a` cluster
    - port: the port number of the RabbitMQ Management WebUI using the `rabbitmq-cluster-a` cluster

- In the Overview view, the number of consumed messages (Ready) and the number of unacknowledged messages (Unacked) are both 0, indicating that the consumption is complete.

    ![Number of messages is 0](../images/migrate02.png)

### Option II

Migrate the data first, and then switch the production end and the consumer end at the same time. Data migration is done with the help of the shove plugin.

> The principle of shovel data migration is to consume the messages in the `rabbitmq-cluster-a` cluster and produce the messages to the `rabbitmq-cluster-b` cluster. After the migration, the messages in the `rabbitmq-cluster-a` cluster are cleared. It is recommended to migrate offline, and the business will be interrupted.

Both `rabbitmq-cluster-a` and `rabbitmq-cluster-b` need to enable the shovel plugin.

![Open plugin](../images/migrate03.png)

Parameter Description:

- Name: Configure the name of the shovel.
- Source: Specify the protocol type, the source cluster address of the connection, and the type of the source.
- Prefech count: Indicates the number of messages in the shovel's internal cache (the cache part from the source cluster to the destination cluster).
- Auto-delete: The default is Never, which means that the cluster message will not be deleted. If it is set to `After initial length transferred`, it will be deleted after the message transfer is completed.
- Destination: Specify the protocol type, the connection target cluster address, and the type of the target end.
- Add forwarding headers: If set to `true`, the x-shoveled header attribute will be added to the forwarded message.
- Reconnect delay: Specify the time to wait before re-establishing the connection when the Shovel link fails, in seconds. If set to 0, there will be no reconnection action, that is, Shovel will stop working when the first connection fails. The default is 5 seconds.
- Acknowledgment mode: Refer to Federation configuration.

    - `no ack` means that no message acknowledgment is required;
    - `on publish` means that Shovel will send each message to the destination and then send a message confirmation to the source;
    - `on confirm` indicates that Shovel will use the publisher confirm mechanism to send a message confirmation to the source after receiving the message confirmation from the destination.

#### Number of messages before migration

![Pre-migration message](../images/migrate04.png)

#### Configure shovel information

![Configure shovel](../images/migrate05.png)

#### shovel running status

![running status](../images/migrate06.png)

When the shovel status is "running", the migration starts. After the data migration is complete, switch the production end and consumer end to the `rabbitmq-cluster-b` cluster to complete the migration process.

#### Number of messages after migration

- rabbitmq-cluster-a cluster message status

    ![cluster message](../images/migrate07.png)

- rabbitmq-cluster-b cluster message status

    ![cluster message](../images/migrate08.png)