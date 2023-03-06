# Week 2 — Observability and Distributed Tracing

## Required Homework
Observability is important in an application because the more complex an app gets, the harder it is to track, identify and succesfully  mitigate errors. This week involved incorporating Distributed Tracing within the application to help pinpoint issues if needed. This was achieved by instrumenting the backend application to use Open Telemetry (OTEL), with Honeycomb.io being the provider. AWS X-Ray is another service that was instrumented for tracing, and Cloudwatch Logs and Rollbar services were used for error logging. 

The idea is to incorporate these services in the earlier stages of application deployment, to later reap the benefits of as we continue. 

### Instrumenting Honeycomb with OTEL

Honeycomb was instrumented by adding [this](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/5ea5582e9ef7901eb2855ed11ec7e5d07247bf80) to the app.py file. To test functionality, we incorporated 'Spans', which are a single piece of instrumentation from a single location within the code. 

The code to create a mock span for testing, added to the file 'home_activities.py':

```
    with tracer.start_as_current_span("home-activites-mock-data"):
      span = trace.get_current_span()
      now = datetime.now(timezone.utc).astimezone()
      span.set_attribute("app.now", now.isoformat())
```

Once we visited the backend page on a browser, we are able to record a span trace in the Honeycomb logs:

![](assets/Week2-Honeycomb)

### Instrumenting AWS X-Ray 

AWS X-Ray is another application that helps one get a broad view of requests as they travel through the application. To instrument it, we added [this](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/73a5995fe49a537150d40b3b05afb387e5e4eeaf). I ran into some issues, so I checked and ofcourse, there was a mistake.[This](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/e2a68dbf428d509320454e2ebf096486292aa69d) is what fixed the issue. 

Similar to incorporating spans for Honeycomb, we added a ['segment'](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/95418054ac0ce0e05520ee6e7ec688d5c286b241) to record specific tracing for a request. 

### Configuring WatchTower for Cloudwatch Logs

Logging is another pillar of Observability. We made use of Cloudwatch Logs for this purpose and added [this](https://github.com/aki23gup/aws-bootcamp-cruddur-2023/commit/cbf395e0a843fa278049d4ab2919e7a2c30350a5)
to the app.py file. 

This custom logger will send application log data to Cloudwatch logs:

```
@app.after_request
def after_request(response):
    timestamp = strftime('[%Y-%b-%d %H:%M]')
    LOGGER.error('%s %s %s %s %s %s', timestamp, request.remote_addr, request.method, request.scheme, request.full_path,              response.status)
    return response
```
Note: The code is commented out to again, save ourselves from incurring costs. If need be, we can comment out this logger and view functionality.

### Integrating Rollbar for Error Logging

Rollbar is another gre
![]()
