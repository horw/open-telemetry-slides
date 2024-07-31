---
title: Services Observability. 
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---
# Services Observability

<div class='absolute bottom-2 right-3'>
presented by Igor Udot
</div>
---
layout: center
class: text-center
mdc: true
---



# "Why is the system behaving this way?"

What is observability

---

# Timeline
https://www.crowdstrike.com/blog/observability-redefined/

- 1960: Observability and Control Theory 
- 2013: The Twitter Observability Team Describes Its Mission
- 2016: The (Four) Pillars of Observability at Twitter
- 2017: The Three Pillars of Observability
- 2018: The Year of Observability
- 2019: Passionate Debate Ensues
- 2020: Observability (Re)defined

Observability is something you 
<span v-mark.red="1">
either have, or you don’t.
</span>


---
layout: center 
---

# The Three Pillars of Observability

<div>

Metrics - Logs - Tracing

</div>
<img 
  v-click
  class="w-150 "
  src='https://www.atatus.com/blog/content/images/size/w1000/2023/02/observability.png'
/>

---

# Metrics
Metrics are a numerical representation of data.

<div v-click.hide>


- Performance metrics
- Resource utilization metrics
- Error rates metrics
- Availability metrics
</div>

<div v-after
  class='absolute top-40 w-200'
>
  <img
  src='https://last9.ghost.io/content/images/size/w1000/2023/05/image-28.png'>
</div>
---

# Logs
Log files are the historical records of your systems. 


````md magic-move {lines: true}
```json
{"level":"info","msg":"Something noteworthy happened!","time":"2019-12-09T16:18:21+01:00"}
{"level":"warning","msg":"You should probably take a look at this.","time":"2019-12-09T16:18:21+01:00"}
{"level":"error","msg":"Something failed but I'm not quitting.","time":"2019-12-09T16:18:21+01:00"}
```

```md
A simple print statement such as `Hello, I reached here` can help in debugging a lot at times...
```
````

---

# Traces
Records the paths taken by requests.

Without tracing, finding the root cause of performance problems in a distributed system can be challenging.

<img class="w-150 absolute right-50" src='https://opentelemetry.io/img/waterfall-trace.svg'>

---
layout: center
---


  <br/>

  Metrics: <span v-mark.yellow="1">indicate</span> there is a problem
  
  Traces: <span v-mark.yellow="1">identify</span> the source of the problem
  
  Logs: <span v-mark.yellow="1">provide</span> the detail which reveals the root cause of the problem

---

# OpenTelemery
https://opentelemetry.io/docs/what-is-opentelemetry/

OpenTelemetry is an Observability framework and toolkit designed to <span v-mark.circle.green="1">create</span> and <span v-mark.circle.green="1">manage</span> telemetry data such as <span v-mark.circle.green="1"> traces, metrics, and logs</span>. Crucially, OpenTelemetry is vendor- and tool-agnostic, meaning that it can be used with a broad variety of Observability backends, including open source tools like Jaeger and Prometheus, as well as commercial offerings.

OpenTelemetry<span v-mark.circle.red="2"> is not an observability backend</span> like Jaeger, Prometheus, or other commercial vendors. OpenTelemetry is focused on the generation, collection, management, and export of telemetry. A major goal of OpenTelemetry is that you can easily instrument your applications or systems, no matter their language, infrastructure, or runtime environment. Crucially, the<span v-mark.circle.red="2"> storage and visualization </span>of telemetry is intentionally left to other tools.
---

# Workflow example
<img v-click="0" src='https://dt-cdn.net/images/otel-otlp-colector-9745904d35.svg' class='absolute top-60 left-20 w-200'>

<arrow v-click="[1,7]" x1="140" y1="220" x2="140" y2="270" color="#953" width="2" arrowSize="3" />
<div v-click="[1,7]" class='absolute top-40 left-20'> We should instrument our app <br/> to make it possible to send OTLP.</div>

<style>
.slidev-vclick-target {
  transition: opacity 1500ms ease;
}
.bg {
  transition: opacity 500ms ease, visibility 500ms ease;
  background-color: white;
}
</style>
<div v-click="[2,6]" class='absolute top-40 w-220 h-200 bg'>


````md magic-move {at:3, lines: true}

```python
# tracing
resource = Resource.create(attributes={
    "service.name": app_name,
})

tracer = TracerProvider(resource=resource)
tracer.add_span_processor(BatchSpanProcessor(
    OTLPSpanExporter(endpoint=endpoint))
)

trace.set_tracer_provider(tracer)
```
```python
# metrics
resource = Resource.create(attributes={
    "service.name": app_name,
})
exporter = OTLPMetricExporter(insecure=True)
reader = PeriodicExportingMetricReader(exporter, math.inf)

provider = MeterProvider(metric_readers=[reader], resource=resource)
set_meter_provider(provider)
```
```python
# logs
def add_open_telemetry_spans(_, __, event_dict) -> dict:
    span = trace.get_current_span()
    if not span.is_recording():
        event_dict["span"] = None
        return event_dict

    ctx = span.get_span_context()
    parent = getattr(span, "parent", None)

    event_dict["span"] = {
        "span_id": hex(ctx.span_id),
        "trace_id": hex(ctx.trace_id),
        "parent_span_id": None if not parent else hex(parent.span_id),
    }

    return event_dict
```
```python
# instrument libs
LoggingInstrumentor().instrument(set_logging_format=True)
HTTPXClientInstrumentor().instrument()
...
FastAPIInstrumentor.instrument_app(app, tracer_provider=tracer)
```
````
<style>
.slidev-vclick-target {
  transition: opacity 100ms ease;
}
</style>
</div>



<arrow v-click="[7]" x1="290" y1="220" x2="290" y2="270" color="#953" width="2" arrowSize="3" />
<arrow v-click="[7]" x1="530" y1="220" x2="530" y2="270" color="#953" width="2" arrowSize="3" />

<div v-click="[7]" class='absolute top-45 left-45'> Exporters help us transfer data to data consumers / receivers. </div>

<div v-click="9" class='absolute top-45 left-45'> And ideally, it's supposed to use the OTLP protocol for communication. </div>

<arrow v-click="9" x1="350" y1="220" x2="350" y2="270" color="#953" width="2" arrowSize="3" />
<arrow v-click="9" x1="610" y1="220" x2="610" y2="270" color="#953" width="2" arrowSize="3" />

---

# OpenTelemetry Protocol
https://opentelemetry.io/docs/specs/otlp/<br/>

The OpenTelemetry Protocol (OTLP) specification describes the encoding, transport, and delivery mechanism of telemetry data between telemetry sources, intermediate nodes such as collectors and telemetry backends.



---

# OpenTelemetry Collector


<img v-click="0" class='absolute left-30 h-100 ' src='https://miro.medium.com/v2/resize:fit:1400/1*R9WOnbtcwJaKmh7lbtaNHQ.png'>

<arrow v-click="1" x1="290" y1="120" x2="290" y2="160" color="#953" width="2" arrowSize="3" />
<arrow v-click="2" x1="500" y1="120" x2="500" y2="285" color="#953" width="2" arrowSize="3" />
<arrow v-click="3" x1="700" y1="120" x2="700" y2="160" color="#953" width="2" arrowSize="3" />


---

# System Example
<!-- <img src='https://grafana.com/media/blog/lambda-traces/Lambda-traces-2.png'> -->

<div class="pt-12">
  <span @click="$slidev.nav.go(1)" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>


---

