#2017-09-20
At a Kafka cluster with high load, when one broker is shutdown and brought back in a couple of minutes, some messages got lost. The error was

> java.util.concurrent.ExecutionException: org.apache.kafka.common.errors.TimeoutException: Expiring 1 record(s) for test2-0: 42084 ms has passed since batch creation plus linger time

My guess, the problem is **auto.leader.rebalance.enable** config. By default, it is true. A background thread tries to balance leader distribution through the cluster.

- https://issues.apache.org/jira/browse/KAFKA-4084
- https://blog.imaginea.com/how-to-rebalance-topics-in-kafka-cluster/

**Solution**: Disable auto leader rebalance. When all replicas are in sync, run  **kafka-preferred-replica-election.sh** tool manually. Note that this tool tries to rebalance all partitions if no topic partition list is given. If rebalancing leaders for all partitions causes problem, do it incrementally.

- https://cwiki.apache.org/confluence/display/KAFKA/Replication+tools


> Written with [StackEdit](https://stackedit.io/).