# Kafkaless installer

An ansible playbook for installing [Kafka](https://kafka.apache.org/) and [Kubeless](https://kubeless.io) on a running Openshift instance.

## Why

Kubeless is a Kubernetes-native serverless solution which uses Apache Kafka for messaging under the hood. By default Kubeless runs a Kafka instance for you, but a custom cluster can be used instead (for example, if one already exists in the Kubernetes/Openshift cluster, or if a higher level of configuration is needed). This playbook handles the setup necessary to do this, and uses [Strimzi](https://strimzi.io/) for the Kafka cluster. Strimzi provides a way to run an Apache Kafka cluster on OpenShift and Kubernetes in various deployment configurations.

## Variables

- `kubeless_namespace`: The name of the Kubeless namespace (defaults to kubeless)
- `kafka_namespace`: The name of the Strimzi/Kafka namespace (defaults to strimzi)
- `kafka_cluster_name`: The name of the Strimzi/Kafka cluster (defaults to cluster)

## Kafka triggers

When setting up the Kubeless instance, a trigger controller is also created. This allows functions to be triggered whenever messages are published to given (trigger) topics in Kafka. To do this,

- Create a kubeless kafka trigger. This needs to associate the function that will be called when a topic is published to. For example, a `hello_world` function could be invoked whenever a producer publishes a message to the `hello` topic
- Produce a message to that topic (for testing I usually use [these Go producers/consumers](https://github.com/dimitraz/kafka-on-k8s))
- That's it! The function should be invoked.
