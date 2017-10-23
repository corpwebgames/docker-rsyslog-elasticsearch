# Docker log driver syslog forward to Elasticsearch
##Introduction
This docker simply accepts syslog messages via TCP or UDP, stores them locally in /var/log/syslog (which can be mapped on the host for persistence) and forwards them to an elastic search endpoint.

Note that this docker does not support linking since the elasticsearch machine is likely to run on another machine.

##Configuration

Environment var | Description
-------------|---------------
ESLOG\_HOST | The endpoint where the ElasticSearch instance is hosted.
ESLOG\_ES\_PORT | The port number where the ElasticSearch hosts the API on.

##Example usage

Start the ElasticSearch docker:

	docker run -d -p 9200:9200 elasticsearch
	
Start this docker:

	docker run -d -e ESLOG_HOST={hostip} -e ESLOG_ES_PORT=9200 -p 5514:514 visity/rsyslog-elasticsearch
	
where {hostip} should be the ip address of the elasticsearch docker host. I used port 5514 so it will not conflict with any locally running syslog instance.

On a docker 1.8 machine (which supports tagging):

	docker run -d --log-driver=syslog --log-opt syslog-address=tcp://{hostip}:5514 --log-opt syslog-tag="redis" redis

	
