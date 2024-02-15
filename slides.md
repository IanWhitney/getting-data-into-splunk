theme: Next, 8
footer: Ian Whitney, ASR (OUE)

# Getting Data Into Splunk
## The What and How

---

## Splunk
### What Is It

> software for searching, monitoring, and analyzing machine-generated data

---

## Splunk
### What Do You Use It For

We use it to make sure our applications are healthy
- Alerts
- Dashboards
- Research when things go awry

---

## Splunk
### What Do You Need

- Generate Data
- Get that data in to Splunk

---

## Generate Data
- Log files are a good starting point
- Do not limit yourself to log files

---

## Generate Data
- More structured = better

---

## Generate Data
### Application Logs

- A sensible place to start
- You probably already have these
- Write them in JSON if you can

---

## Generate Data
### Instrumentation

- Many application frameworks let you wrap code to observe code
- These observations can be sent to Splunk!

---

## Generate Data
### Instrumentation

```ruby
ActiveSupport::Notifications.instrument "your_application.magic", {} do |instrumentation|
  # magic happens
end
```

```json
{
    "severity": "INFO",
    "timestamp": 1707750727997393,
    "program": "your_application",
    "environment": "production",
    "version": "0.0.40",
    "entry": {
        "string": null,
        "instrumentation": {
            "name": "your_application.magic",
            "started": 1707750727992772,
            "finished": 1707750727997335,
            "elapsed": 4563
        }
    }
}
```

---

## Generate Data
### Database Queries
- SQL queries can return json
- Which you can then put in to Splunk!

---
## Generate Data
### Database Queries

```sql
SELECT /*json*/
    max(updated_at) as most_recent_update
FROM
    important_application_table
;
```

```json
{
    "most_recent_update": 1707750727992772
}
```

---

## Generate Data
### API Responses
- These usually return JSON
- Put it in Splunk!

---

## Generate Data
### API Responses

```
curl -H "Content-Type: application/json"  -X GET \
"http://127.0.0.1:8088/healthcheck"
```

```json
{
    "isHealthy": true,
    "details": {
        "metastore": {
            "isHealthy": true
        },
        "kafka": {
            "isHealthy": true
        },
        "commandRunner": {
            "isHealthy": true
        }
    },
    "serverState": "READY"
}
```

---

## Generate Data
### Prebuilt

- A lot of data from hosts is already in Splunk
    - `top`
    - `cpu`
    - `vmstat`
    - `syslog`

---

## Ingestion

- Have the Splunk team do it
- Use methods that already exist, `syslog`
- Send it yourself, `HEC`
- Other (secretly also `HEC`)

---

## Ingestion
### Have the Splunk team do it

- Work with the Splunk team
- Format your files a consistent way
- Store your files in a consistent location
- Success!

---

## Ingestion
### Have the Splunk team do it

- Requires fewer changes on your side
- Slower. Splunk team is super busy!
- Can require you to remember to add new hosts to Splunk ingestion

---

## Ingestion
### Use methods that already exist, `syslog`

- Splunk already ingests data host log data
- If you run your process via `systemd` or have Docker log to `journald`, logs end up in Splunk!
- Be sure to add unique and helpful tags

---

## Ingestion
### Use methods that already exist, `syslog`

> example

---

## Ingestion
### Send it yourself, `HEC`

- Get a token from the Splunk team
- Send your data over http

---

## Ingestion
### Send it yourself, `HEC`

> Example

---

## Ingestion
### Other (secretly also `HEC`)

- Boomi
- Kafka
- Etc

--- 

## Ingestion
### Other (secretly also `HEC`)

> Boomi example
