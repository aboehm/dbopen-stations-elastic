# Import Deutsche Bahn Station Dataset into ELK

## Data source

Deutsche Bahn AG published in January 2016 the first time a list of station including

* ID number
* Name
* Geo Coordinates (Longitude, Latitude)
* Type of connection (longdistance/ regional/ private regional)
* Operation spot

The data set can be downloaded under http://data.deutschebahn.com/datasets/haltestellen/.

## Software

With the following files, you can import the data into ELK architecture (Elaticsearch, Logstash, Kibana). Documention and software will be distrubted under https://elastic.co.

## Example

For this example elasticsearch, logstash and kibana running on the same machine without any password protections.

### Template

Bevor the import procedure, a template must be placed so elaticsearch use the correct data types. Install the template through

```
curl -XPOST http://localhost:9200/_template/db_open_stations?pretty \
  --data-binary @db_open_stations.template.json
```
Elastisearch should respond

```{ "acknowledged" : true }```

### Import

Start import through logstash configuration. 

```
curl -q http://data.deutschebahn.com/datasets/haltestellen/D_Bahnhof_2016_01_alle.csv |\
  /opt/logstash/bin/logstash -f db_open_stations.conf

```

Different import methods are specified in db_open_stations.conf. The data will be placed in indices db_open_stations-*.

### Visualize

After some configuarion in Kibana, a map can be displayed.

![Map](/example_map.jpg)
