## Abstract

We describe a system called Overton, whose main design goal is to support
engineers in building, monitoring, and improving production machine learning
systems. Key challenges engineers face are monitoring fine-grained quality,
diagnosing errors in sophisticated applications, and handling contradictory or
incomplete supervision data. Overton automates the life cycle of model
construction, deployment, and monitoring by providing a set of novel
high-level, declarative abstractions. Overton's vision is to shift developers
to these higher-level tasks instead of lower-level machine learning tasks. In
fact, using Overton, engineers can build deep-learning-based applications
without writing any code in frameworks like TensorFlow. For over a year,
Overton has been used in production to support multiple applications in both
near-real-time applications and back-of-house processing. In that time,
Overton-based applications have answered billions of queries in multiple
languages and processed trillions of records reducing errors 1.7 -- 2.8x versus
production systems.
