- [Running Haystack using docker-compose](#running-haystack-using-docker-compose)
  * [Allocate memory to docker](#allocate-memory-to-docker)
  * [To start Haystack's traces, trends and service graph](#to-start-haystacks-traces-trends-and-service-graph)
  * [To start Zipkin (tracing) with Haystack's trends and service graph](#to-start-zipkin-tracing-with-haystacks-trends-and-service-graph)
  * [Note on composing components](#note-on-composing-components)

## Running Haystack using docker-compose

### Allocate memory to docker

Please check this [Stackoverflow answer](https://stackoverflow.com/questions/44533319/how-to-assign-more-memory-to-docker-container) 

To run all of haystack and its components, __it is suggested to change the default in docker settings from `2GiB` to `4GiB`__

### To start Haystack's traces, trends and service graph

```
docker-compose -f docker-compose.yml \
               -f traces/docker-compose.yml \
               -f trends/docker-compose.yml \
               -f service-graph/docker-compose \
               -f agent/docker-compose.yml up
```

The command above starts haystack-agent as well.  Give a minute or two for the containers to come up and connect with each other. Haystack's UI will be available at http://localhost:8080 

Finally, one can find a sample spring boot application @  https://github.com/mchandramouli/haystack-springbootsample to send data to Haystack via haystack-agent listening in port 34000. 


### To start Zipkin (tracing) with Haystack's trends and service graph

```
docker-compose -f docker-compose.yml \
               -f zipkin/docker-compose.yml \
               -f trends/docker-compose.yml \
               -f service-graph/docker-compose up
```

The command above starts [Pitchfork](https://github.com/HotelsDotCom/pitchfork) to proxy data to [Zipkin](https://github.com/openzipkin/) and Haystack. 

Give a minute or two for the containers to come up and connect with each other.  Once the stack is up, one can use the sample application @ https://github.com/openzipkin/brave-webmvc-example and send some sample data to see traces (from Zipkin), trends and service-graph in haystack-ui @ http://localhost:8080


### Note on composing components

Note the two commands above add a series of `docker-compose.yml` files. 

* Haystack needs at least one trace provider ( traces/docker-compose.yml or zipkin/docker-compose.yml ) and one trends provider ( trends/docker-compose.yml )
* One can remove service-graph/docker-compose.yml if `service graph` is not required
* Staring the stack just with with base docker-compose.yml, will start core services like kafka, cassandra and elastic-search along with haystack-ui with mock backend
```
docker-compose -f docker-compose.yml up
```

