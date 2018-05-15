
# Example apps: Real-time data analysis using Google Kubernetes Engine, Redis or PubSub, and BigQuery

This repository contains two related example [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/)
(GKE) apps that show how to build a 'pipeline' to stream data into BigQuery.


The app in the `pubsub` directory uses [Google Cloud PubSub](https://cloud.google.com/pubsub/docs).
<!-- Documentation for this example can be found on the Google Cloud Platform site:
https://cloud.google.com/solutions/real-time/kubernetes-pubsub-bigquery -->

The app in the `redis` subdirectory uses [Redis](http://redis.io/).
<!-- Documentation for this example can be found on the Google Cloud Platform site:
https://cloud.google.com/solutions/real-time-analysis/kubernetes-redis-bigquery -->


## How to use

Specify the GCP settings (pubsub topic, BQ project id, dataset and table names) and number of tweets to process in a k8s configmap named tw-conf:

    $ kubectl create configmap tw-conf --from-literal=PUBSUB_TOPIC=<projects/your-project/topics/your-topic> --from-literal=BATCH_SIZE=50 --from-literal=TOTAL_TWEETS=10000000 --from-literal=NUM_RETRIES=3 --from-literal=PROJECT_ID=<project_id> --from-literal=BQ_DATASET=<bq_dataset> --from-literal=BQ_TABLE=<bq_table>

Specify the settings to your twitter credentials (consulmer key/secret, app token/secret) in a k8s secret names tw-sec:

    $ kubectl create secret generic tw-sec --from-literal=CONSUMERKEY=<consumerkey> --from-literal=CONSUMERSECRET=<consumersecret> --from-literal=ACCESSTOKEN=<accesstoken> --from-literal=ACCESSTOKENSEC=<accesstokensec>

Create pods:

    $ kubectl create -f pubsub/bigquery-controller.yaml
    $ kubectl create -f pubsub/twitter-stream.yaml
