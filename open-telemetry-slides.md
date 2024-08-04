---
title: Services Observability. 
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---
# System Observability

<div class='absolute bottom-2 right-3'>
presented by Igor Udot
</div>

---

# Monitoring vs Observability

<img src="./imgs/open-telemetry-slides/blackwhitebox.png" class="absolute w-130 top-55 left-60"/>

---
layout: center
class: text-center
mdc: true
---

# "Is the system working as expected?"

What is monitoring


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

# The Three Pillars of Observability

<div>

Metrics - Traces - Logs

</div>

<img 
  class="absolute w-100 left-70"
  src='./imgs/open-telemetry-slides/pillars.png'
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

# Traces
Traces are records of the paths taken by requests.

<div v-click='[1]' class="text-center absolute left-25 top-60">
Without tracing, finding the root cause of problems in a distributed system can be challenging.
</div>
<img v-click='[2]' class="w-150 absolute right-50" src='./imgs/open-telemetry-slides/waterfall-trace.svg'>

<img v-click=3 class="w-230 absolute right-5 top-50" src='./imgs/open-telemetry-slides/day-routine.png'>

---

# Logs
Logs are the historical records of your systems. 


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
layout: center
---


  <br/>

  Metrics: <span v-mark.yellow="1">indicate</span> there is a problem
  
  Traces: <span v-mark.yellow="1">identify</span> the source of the problem
  
  Logs: <span v-mark.yellow="1">provide</span> the detail which reveals the root cause of the problem

  <div v-click="2">And they <span v-mark.yellow.circle="3">are correlated together</span>.

  </div>
  
---
layout: center
class: text-center
---
There should be <span v-mark.yellow=0>some ID</span> to help us correlate metrics, traces, and logs.

<div v-click=1> 

# It is a trace ID.
</div>

---
class: text-center
---

<img v-click=[1,2] class="w-40 absolute top-50 left-40" src='./imgs/open-telemetry-slides/ex1/metric_before.png' >

<div v-click=[0] class="absolute top-50 left-60 text-center">

# We got some exception. 
Our alert system indicates that the exception count is equal to 1.
</div>

<img v-click=[2,4] class="w-100 absolute right-5 top-10 left-70" src='./imgs/open-telemetry-slides/ex1/ex1_metrics.png'>

<div v-click=[3] class="absolute top-94 left-34">
<span v-mark.red.circle="3">  There is our trace ID--> </span>
</div>


<img v-click=4 class="w-40 absolute top-50 left-40" src='./imgs/open-telemetry-slides/ex1/metric.png' >

<img v-click=5 class="w-30 absolute top-65 left-82" src='./imgs/open-telemetry-slides/ex1/arg.png' >

<img v-click=5 class="w-38 absolute top-50 left-112" src='./imgs/open-telemetry-slides/ex1/trace_before.png' >


<img v-click=[6] class="w-150 absolute right-5 top-10 left-40" src='./imgs/open-telemetry-slides/ex1/ex1_jaeger.png' >


<img v-click=[7,9] class="w-230 absolute top-10 left-40" src='./imgs/open-telemetry-slides/ex1/ex1_jaeger_in_trace.png' >

<div v-click=[8] class="absolute top-9 left-100">
<span v-mark.red.circle="8">  <-- There is our trace ID </span>
</div>


<img v-click=10 class="w-38 absolute top-50 left-112" src='./imgs/open-telemetry-slides/ex1/trace.png' >
<img v-click=11 class="w-30 absolute top-65 left-152" src='./imgs/open-telemetry-slides/ex1/arg.png' >
<img v-click=11 class="w-38 absolute top-50 left-182" src='./imgs/open-telemetry-slides/ex1/log_before.png' >

<img v-click=[12] class="w-110 absolute  left-65" src='./imgs/open-telemetry-slides/ex1/ex1_grafana.png' >
<img v-click=[13] class="w-150 absolute  left-45" src='./imgs/open-telemetry-slides/ex1/ex1_logs.png' >
<div v-click=[13] class="absolute bottom-15 right-2">
<span v-mark.red.circle="13"><--  There is our trace ID </span>
</div>

<img v-click=14 class="w-38 absolute top-50 left-182" src='./imgs/open-telemetry-slides/ex1/log.png' >

<img v-click=15 class="w-10 absolute top-37 left-55" src='./imgs/open-telemetry-slides/ex1/prom.png' >
<img v-click=16 class="w-40 absolute top-35 left-112" src='./imgs/open-telemetry-slides/ex1/jaeger.png' >
<img v-click=17 class="w-28 absolute top-35 left-188" src='./imgs/open-telemetry-slides/ex1/gl.png' >

<img v-click=18 class="w-30 absolute top-70 left-82" src='./imgs/open-telemetry-slides/ex1/bk_arg.png' >
<img v-click=18 class="w-30 absolute top-70 left-152" src='./imgs/open-telemetry-slides/ex1/bk_arg.png' >
<img v-click=18 class="w-100 absolute top-83 left-82" src='./imgs/open-telemetry-slides/ex1/round_arg.png' >


<img v-click=19 class="w-28 absolute top-55 left-152" src='./imgs/open-telemetry-slides/ex1/opentel.png' >
<img v-click=19 class="w-28 absolute top-55 left-82" src='./imgs/open-telemetry-slides/ex1/opentel.png' >
<img v-click=19 class="w-28 absolute top-85 left-120" src='./imgs/open-telemetry-slides/ex1/opentel.png' >



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
.bg {
  transition: opacity 1200ms ease;
  background-color: white;
}
</style>
<div v-click="[2,7]" class='absolute top-40 w-220 h-200 bg'>
<div class="bg">

````md magic-move {at:3, lines: true}

```python
# tracing
resource = Resource.create(attributes={ #            <-- Labels for recognizing service.
    "service.name": app_name,
})

exporter = OTLPSpanExporter(endpoint=endpoint) #                                  <-- Exporter
processor = BatchSpanProcessor(exporter) #                                        <-- Processor

provider = TracerProvider(active_span_processor=processor, resource=resource) #   <-- Provider
set_tracer_provider(provider)
```
```python
# metrics
resource = Resource.create(attributes={ #            <-- Labels for recognizing service.
    "service.name": app_name,
})
exporter = OTLPMetricExporter(insecure=True) #                                    <-- Exporter
reader = PeriodicExportingMetricReader(exporter, math.inf) #                      <-- Processor

provider = MeterProvider(metric_readers=[reader], resource=resource) #            <-- Provider
set_meter_provider(provider)
```
```python
# logs 
resource = Resource.create(attributes={ #            <-- Labels for recognizing service.
    "service.name": app_name,
})

exporter = OTLPLogExporter(insecure=True) #                                          <-- Exporter
processor = BatchLogRecordProcessor(exporter) #                                      <-- Processor

provider = LoggerProvider(multi_log_record_processor=processor, resource=resource) # <-- Provider
set_logger_provider(provider)

handler = LoggingHandler(level=logging.NOTSET, logger_provider=provider)
# Attach OTLP handler to root logger
logging.getLogger().addHandler(handler)
```
```python
# BUT you can also add a custom logs processor to your code.
def add_open_telemetry_spans(_, __, event_dict) -> dict:
    span = trace.get_current_span() # <-- get span(context) object 
    if not span.is_recording():
        event_dict["span"] = None
        return event_dict

    ctx = span.get_span_context() #   <-- get span(context) attribute
    parent = getattr(span, "parent", None)

    event_dict["span"] = { #          <-- set this context to the logger 
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

</div>
</div>



<arrow v-click="[8,10]" x1="290" y1="220" x2="290" y2="270" color="#76a0ef" width="2" arrowSize="3" />
<arrow v-click="[8,10]" x1="530" y1="220" x2="530" y2="270" color="#76a0ef" width="2" arrowSize="3" />

<div v-click="[8,10]" class='absolute top-45 left-55'> <span v-mark="{ at: 8, color: '#76a0ef', type: 'circle' }">Exporters</span> help us transfer data to data <span v-mark="{ at: 9, color: '#70db70', type: 'circle' }">consumers / receivers.</span> </div>

<arrow v-click="[9]" x1="420" y1="220" x2="420" y2="270" color="#70db70" width="2" arrowSize="3" />
<arrow v-click="[9]" x1="700" y1="220" x2="700" y2="270" color="#70db70" width="2" arrowSize="3" />

<div v-click="10" class='absolute top-45 left-45'> And ideally, it's supposed to use the OTLP protocol for communication. </div>

<arrow v-click="10" x1="350" y1="220" x2="350" y2="270" color="#953" width="2" arrowSize="3" />
<arrow v-click="10" x1="610" y1="220" x2="610" y2="270" color="#953" width="2" arrowSize="3" />

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
<src src="https://miro.medium.com/v2/resize:fit:1400/1*Re85YC9ZNUCniuZEdAsH3A.png" />


---

