For QoS testing, it is easier to create a simple “single-shot” version of your publisher that sends one message and exits. This allows you to cleanly demonstrate QoS 0/1 behaviour in screenshots in contrast to the continuous weather-streaming mode.

# Screenshot 1 — QoS 0 (message lost if subscriber is offline)

Steps:

Stop subscriber.

Run: publish_one_message(qos=0)

Start subscriber.

Screenshot:

Publisher terminal shows Published QoS0 message.

Subscriber terminal shows nothing.

This is the textbook behaviour of QoS 0.


# Screenshot 2 — QoS 1 (message delivered if subscriber is online)

Steps:

Start subscriber.

Run: publish_one_message(qos=1)

Screenshot:

Publisher terminal shows Published QoS1 message.

Subscriber shows: Received: hello-qos1.

QoS 0 vs. QoS 1: The difference between QoS 0 and QoS 1 is demonstrated by lost versus delivered messages in controlled single-shot mode.


# Sceenshot 3 - Retained message
For retained-message testing, publish one retained message with a simple “single-shot” publisher and then start the subscriber. If the subscriber prints the message immediately (without a new publish), this provides clear evidence of retained behaviour.

Steps:

Publisher:
Use your publisher, but run it once so it sends a single reading marked as retained.
Ensure the subscriber is not running during this publish.

Screenshot – Publisher evidence

Take one screenshot showing the publisher has successfully published one message,

Subscriber:
Start the subscriber after the retained message exists on the broker.
Do not publish anything new.

Screenshot – Subscriber evidence

Take one screenshot showing the subscriber receives a message immediately on connect
Label it Subscriber – message received instantly due to retained flag.