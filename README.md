# Tensor Bridge

Tensor Bridge is an [OpenAPI (Swagger) Specification](https://github.com/OAI/OpenAPI-Specification) as well as a simple [Connexion](https://github.com/zalando/connexion) wrapper for [TensorFlow Serving](https://github.com/tensorflow/serving).

The specification was obtained by compiling an annotated `tensor_bridge.proto` using [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway).
The result is located in `swagger/tensor_bridge.json`.

## Deprecation notice

TensorFlow now supports [REST API](https://www.tensorflow.org/serving/api_rest) natively as of version [v1.8.0](https://github.com/tensorflow/serving/releases/tag/1.8.0).
Tensor Bridge API is still useful as the exact REST equivalent of the TF gRPC API.
However, we deprecate this project and recommend using the official TF REST API for new developments.


## How is this useful?

The publicly available version of TensorFlow serving works over [gRPC](http://www.grpc.io/).

Now you can use the API to build your own REST service and use JSON to talk to your TensorFlow models. A full example is included in this repo (see `app.py` and `api/`).
If you prefer Go, you can even generate a reverse proxy automatically using [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway).

## Installation

Simply run 

```bash
docker build -t tf-bridge .
```
from the project root.

This will take a while as you are compiling TensorFlow and TensorFlow Serving from source.
(Consider dedicating around 6-8 GB of RAM to Docker)

When the image is created you can start the servers

```bash
docker run -d -p 9001:9001 -p 9000:9000 -e MODEL=mnist tf-bridge
```
Tensor Bridge can be queried at `9001`.

The gRPC endpoint is still available at `9000` for convenience and testing.

You will notice that we use the `MODEL` variable to specify the model. As an example, we included an exported MNIST model in this repo.

To see the Swagger UI go to `http://localhost:9001/ui/`

## Client

There is also a simple client located in `client/mnist_client.py` for testing purposes. Make sure to install the necessary dependencies from `requirements.txt`.

If everything went well, you will shortly get the following output

`Inference error rate: 10.4%`

