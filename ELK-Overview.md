# Elasticsearch-Logstash-Kibana {ELK} : Overview


---

`ELK` is a collection of different open source modules like `Elasticsearch`, `Logstash`, `Kibana`. This helps to any user to collect, analyze and visualize data in realtime. Each modules fits based on your use case and environment.

> `Elasticsearch` is a NoSQL database that is based on the Lucene search engine. `Logstash` is a log pipeline tool that accepts inputs from various sources, executes different transformations, and exports the data to various targets. `Kibana` is a visualization layer that works on top of Elasticsearch.


### Why ELK?

#### Challenges :

- **No Consistency** (It's difficult to be a jack-of-all trades)

	- Difficulty in logging for each application, system, device
	- Interpreting the various types logs
	- Variation in format makes it challenging to search
	- Many types of time formats

- **No Centralization** (Simply put, log data is everywhere)

	- Logs in many locations on various servers
	- SSH + GREP doesn’t scale 

- **Accessibility of Log Data** (Much of the data is difficult to locate and manage)

	- Access is often difficult
	- High expertise to mine data
	- Logs can be difficult to find
	- Immense size of Log Data

#### Solution :

> The ELK Stack is popular because it fulfills a need in the log analytics space. Splunk’s enterprise software has long been the market leader, but its numerous functionalities are increasingly not worth the expensive price — especially for smaller companies such as products and tech startups.
>
<br>
> The ELK stack can help you manage each of these challenges, and more. ELK is best for time-series data-anything with a time stamp-such as you'll find in most web server logs, transaction logs, and stock data listings. To be intelligible, these logs usually need substantial clean-up.

### What is ELK?

#### Elasticsearch :

Elasticsearch is a search server based on Lucene. It provides a distributed, multi tenant-capable full-text search engine with a RESTful web interface and schema-free JSON documents. Enables real-time ability to search and store data.

#### Logstash :

Logstash is a tool for log data intake, processing, and output. It helps to centralize data processing of all types of logs. Multiple plugins for different functionalities.

It consist mainly 3 components :

- Input : Passing logs to process them into machine understandable format
- Filters : Set of conditionals to perform specific action on a event 
- Output : Decision maker for processed events/logs

#### Kibana :

Kibana is a powerful front-end dashboard for visualizing indexed information from elasticsearch. It's a flexible analytics & visualization platform and useful for instant sharing and embedding of dashboards. Capable of providing data in the form of charts, graphs, counts,etc in realtime.


### Basic ELK Setup 

![Basic ELK Setup](images/ELK_basic_setup.png)


