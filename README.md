# Concurrency Issues In Java - CharsetDecoder
This is a basic SpringBoot API that includes an intentionally vulnerable CharsetDecoder API. This is for testing purposes, and to showcase the errors/issues this issue can cause.

## Quick Start
1. Start the App:
```bash
mvn spring-boot:run  # Or run via your IDE
```

2. Trigger the Vulnerability:

Use Apache Benchmark (ab) or curl in parallel to spam the vulnerable endpoint:

```bash
ab -n 1000 -c 50 "http://localhost:8080/employees/decode?input=AttackPayload"
```
* -n 1000: 1000 total requests.
* -c 50: 50 concurrent threads.

Option B: Bash Loop with curl (No install needed)
```bash
for i in {1..100}; do curl "http://localhost:8080/employees/decode?input=Test$i" & done
```
* Runs 100 requests in parallel (background jobs).

3. Expected Results:

Application logs: Will show `IllegalStateException` or `MalformedInputException`.
Some responses: May return corrupted strings or HTTP 500 errors.
No OS impact: Your laptop will keep running (just the app may crash).

Example:
```
java.lang.IllegalStateException: Current state = FLUSHED, new state = CODING
```
