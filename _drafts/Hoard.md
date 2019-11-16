---
layout: post
title: "Hoard: A Distributed Data Caching System to Accelerate Deep Learning Training on the Cloud"
categories: [papers]
tags: [storage, filesystem, cache, deep learning]
---

[Hoard: A Distributed Data Caching System to Accelerate Deep Learning Training on the Cloud](https://arxiv.org/abs/1812.00669)

GPU는 여전히 비싼 자원이고, 그러한 자원을 관리하는 사람들이 해야 할 일은 그 자원이 효율적으로 (쉬는 시간이 없이) 사용되게 하는 것이다. 이 논문은 Deep Learning training에 사용되는 GPU 서버들과 training dataset을 공급하는 파일시스템 (혹은 오브젝트 스토리지) 간의 비효율성을 지적하며, 그 해결을 위한 분산 캐시 시스템을 제안한다. 

하나의 GPU 서버 노드 당 4개의 GPU가 있고, 총 4개의 GPU 서버가 있으며, 각 GPU 노드에 두 개의 NVMe 디스크를 넣어서 NFS로의 데이터 액세스를 캐시한 실험에서 AlexNet ImageNet 이미지 분류 벤치마크 (150 GB) 의 경우 캐시가 없을 때에 비해 2.1배의 성능 향상이 있었다고 한다. 단, NFS 로의 네트워크는 10 Gbps 였다. 

