# Arazzo 1.1.0 Protocol & Workflow Examples

This repository serves as a working collection of examples to support the evaluation and design of AsyncAPI support in Arazzo 1.1.0.

Each example demonstrates how API workflows can span synchronous and asynchronous systems using Arazzo's declarative syntax. It includes:

 - Realistic OpenAPI and AsyncAPI documents
 - Composed Arazzo workflow definitions (.arazzo.yaml)
 - Support for both send and receive step types, across diverse protocols
 - Use of new Arazzo features including `timeout`, `correlationId`, `action`, and `dependsOn`

These examples are intended to validate protocol-level fit, discover modeling patterns, and shape spec guidance and tooling implementation strategies.


## Initial Arazzo Async API Example Coverage

| Protocol                                                       | `send` | `receive` | Arazzo Fit    | Notes                                                                |
| -------------------------------------------------------------- | ------ | --------- | ------------- | -------------------------------------------------------------------- |
| [**Kafka**](./v1.1.0-prep/async-api-examples/kafka/)           | ✅      | ✅         | ✅ | Well-supported pub-sub; structured JSON or binary payloads possible. |
| [**AMQP**](./v1.1.0-prep/async-api-examples/amqp/)             | ✅      | ✅         | ✅ | Strong JSON alignment via RabbitMQ; works well with events.          |
| [**MQTT / MQTT 5**](./v1.1.0-prep/async-api-examples/mqtt/)    | ✅      | ✅         | ✅ | Great for lightweight telemetry and IoT messaging.                   |
| [**NATS**](./v1.1.0-prep/async-api-examples/nats/)             | ✅      | ✅         | ✅ | Simple and fast messaging with subject routing.                      |
| [**WebSocket**](./v1.1.0-prep/async-api-examples/websocket/)   | ✅      | ✅         | ✅ | Bi-directional real-time channels; suited for chat, UX events.       |
| [**HTTP (webhooks)**](v1.1.0-prep/async-api-examples/http-webhooks/) | ❌ (*)      | ✅         | ✅ | Receive-only event triggers; modeled using AsyncAPI or OpenAPI. In Arazzo, send can be used when the workflow acts as the webhook client (e.g. issuing a POST to an external webhook URL).      |
| [**SNS**](./v1.1.0-prep/async-api-examples/sns/)               | ✅      | ✅         | ✅ | Supports send and receive; receive typically via subscriber integration (e.g. SQS, Lambda) |
| [**SQS**](./v1.1.0-prep/async-api-examples/sqs/)               | ✅      | ✅         | ⚠️ Partial     | Supports send and receive; receive typically via polling. Commonly paired with SNS.              |
| [**SSE**](./v1.1.0-prep/async-api-examples/sse/)             | ❌      | ✅         | ⚠️ Partial     | One-way streaming from server to client over HTTP.                   |

**Notes**

 - Media-type flexibility: Arazzo supports any contentType, including application/xml, application/octet-stream, etc. Binary messages are fine if they do not require selectors.
 - If introspection (e.g. for `successCriteria` or `outputs`) is needed, then JSON Schema or structured payloads are **required**.
 - Tooling should provide warnings if selector logic is used with non-structured content.

## Possible Future Arazzo Coverage

Recommendation would be to consider based on community feedback or vendor extension success

| Protocol                     | `send` | `receive` | Arazzo Fit | Notes                                                         |
| ---------------------------- | ------ | --------- | ---------- | ------------------------------------------------------------- |
| **STOMP**                    | ✅      | ✅         | ⚠️ Partial | Text-based protocol; JSON support varies.                     |
| **Solace**                   | ✅      | ✅         | ⚠️ Partial | Proprietary brokers; support varies by use.                   |
| **Redis Pub/Sub**            | ✅      | ✅         | ⚠️ Partial | Requires runtime normalization for selector support.          |
| **Google Pub/Sub**           | ✅      | ✅         | ⚠️ Partial | Works well with JSON, but schema registration needed.         |
| **Pulsar**                   | ✅      | ✅         | ⚠️ Partial | Advanced streaming; ensure messages are introspectable.       |
| **Mercure**                  | ❌      | ✅         | ⚠️ Partial | Push-focused pub-sub for browser apps.                        |


## Not supported

 - gRPC
 - Avro / Protobuf / Thrift

