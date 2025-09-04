---
layout: post
title: Engineering cheat-sheet to burn into my memory
date: 2025-09-03 12:00:00
description:
tags: systems; engineering
featured: false
related_posts: false
---

#### <b>CUDA / GPUs</b>

Keep tensors on GPU and avoid .cpu() / .numpy() unless required

Use mixed precision!
```
FP32 (single precision) -> standard, stable
FP16/BF16 (mixed precision) -> faster, less memory
INT8/FP8 -> quantized inference, big speed/memory gains
```

Free unused tensors:
```python
del tensor;
torch.cuda.empty_cache()
```

PyTorch: torch.profiler, torch.cuda.synchronize() around timers

Watch GPU utilization with nvidia-smi (if it's low, it's data-bound not compute-bound!)


<br>
#### <b>Curl</b>


Basics
```bash
curl https://my-api.com           # simple GET
curl -v https://my-api.com        # verbose output (headers, etc.)
curl -I https://my-api.com        # HEAD request (headers only)
```

HTTP Methods
```bash
curl -X GET https://my-api.com/resource
curl -X POST https://my-api.com/resource
curl -X PUT https://my-api.com/resource/123
curl -X DELETE https://my-api.com/resource/123
```

Send Data
```bash
curl -d "param1=value1&param2=value2" https://my-api.com/form
curl -H "Content-Type: application/json" \
     -d '{"id":"123","ttl":25}' \
     https://my-api.com/users
```



<br>
#### <b>gRPC</b>

gRPC (Google Remote Procedure Call) = an open-source high-performance framework for building distributed systems and APIs.

Uses Protobufs as default interface definition language and data serialization format.

Client calls function inside proto file and gRPC handles serialization, transport (HTTP/2 by default), and deserialization.

HTTP/2 = second major version of HTTP and supports multiplexing (allows multiple signals / data streams to be combined and transmitted simultaneously).


<br>
#### <b>Protobuf</b>

Protocol Buffers (Protobuf) is a language-neutral, platform-neutral, efficient mechanism for serializing structured data, developed by Google

Serialization = turning structured data into bytes for storage / transmission

Deserialization = turning those bytes back into usable objects

Define data schema in a .proto file:
<br>

```
syntax = "proto3";

message Person {
  string name = 1;
  int32 id = 2;
  string email = 3;
}
```

Compile with ```protoc```

Default serialization format in gRPC APIs


<br>
#### <b>AWS s3 CLI</b>

Upload a file:
```bash
aws s3 cp file.txt s3://my-bucket-name/
```

Upload a directory (recursive)
```bash
aws s3 cp myfolder/ s3://my-bucket-name/ --recursive
```

Download a file
```bash
aws s3 cp s3://my-bucket-name/file.txt ./localfile.txt
```

Sync local <-> S3
```bash
aws s3 sync ./localfolder s3://my-bucket-name/folder
aws s3 sync s3://my-bucket-name/folder ./localfolder
```

<br>




