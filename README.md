# [Elastic stack (ELK) on Docker documentation](https://github.com/deviantony/docker-elk)

# Customizations for Planning Labs geosearch

## docker compose
1. Mount out ES data into local filesystem at ./data
2. Do not expose ES on ports outside of docker network
3. Expose syslog UDP port in logstash container to enable nginx log consumption via syslog
4. Mount custom ES index template into logstash container

## kibana
1. kibana/config/kibana.yml: add 'basePath' to serve kibana UI at /kibana 

## logstash
1. logstash/pipeline/logstash.conf: ingest nginx logs from syslog; add parsing for nginx access and error logs
2. logstash/outputs: directory of custom templates to use when creating indices in ES; mounted into container in docker-compose.yml
3. logstash/pipeline/logstash.conf: update output to use custom template defined in logstash/outputs (see 2)

## nginx
1. nginx.conf: write out access_log and error_log to syslog, like the example in nginx.conf in this repo
2. nginx/conf.d: update server-specific nginx/conf.d/{server}.conf to include kibana route configuration, from the example nginx.server.conf

	NOTE: supply correct `auth_basic_user_file` file path in kibana nginx config
