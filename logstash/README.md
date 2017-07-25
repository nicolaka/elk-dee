# Extending Logstash

This directory is used to extend the logstash directory for Modernizing Traditional Applications.

## Testing

Many times traditional applications will log to multiple locations. Java applications are often configured to log in this way. It's possible to modernize those applications without changing the logger by leverging Go service templating that Docker provides.

When launching a Tomcat application with the following parameters, Docker will create a volume that contains the container name.

```
docker service create \
    -d \
    --name prod-tomcat \
    --label version=1.5 \
    --label environment=prod \
    --mount type=volume,src="{{.Task.Name}}",dst=/usr/local/tomcat/logs \
    --replicas 3 \
    tomcat:latest
```

That will create output similar to this from logstash when using this repo. The `CONTAINER_NAME` will match the output of the `stdout` stream from the container, making it easy to filter based on your container's logs.

```
{
              "path" => "/log/volumes/prod-tomcat.2.sdfuuijyruyzoi98uwpixl5iv/_data/catalina.2017-07-05.log",
        "@timestamp" => 2017-07-06T13:24:14.875Z,
          "@version" => "1",
              "host" => "logstash",
           "message" => "05-Jul-2017 23:40:50.256 INFO [localhost-startStop-1] org.apache.catalina.startup.HostConfig.deployDirectory Deployment of web application directory [/usr/local/tomcat/webapps/docs] has finished in [47] ms",
    "CONTAINER_NAME" => "prod-tomcat.2.sdfuuijyruyzoi98uwpixl5iv",
         "FILE_NAME" => "catalina.2017-07-05.log"
}
```
