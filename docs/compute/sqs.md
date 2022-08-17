# SQS

- Decouple application tiers with SQS
- Standard queue performs best effort ordering. Ordering is not guaranteed. Unlimited Throughput
- FIFO Queue: Ordering is guaranteed. 300 messages per second. With 10 messages batched per operation (maximum), 3000 messages per second
- Standard: At-least once delivery. FIFO: Exactly once processing.

## FIFO Queues

- FIFO Queue requires **Message Group ID** and **Message Deduplication ID**

## Delay Queue

- Set a delay time before a message is seen.

## Long Polling

- Long polling can reduce costs. Waits for messages to arrive.
- SQS Long polling can be enabled either at the Queue level or API level using WaitTimeSeconds.
- Receive Message Wait Time can be set up to 20 seconds.
