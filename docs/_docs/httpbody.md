---
category: documentation
---

# HttpBody message Feature
The [HTTP Body](https://github.com/googleapis/googleapis/blob/master/google/api/httpbody.proto) messages allows a response message to be specified with custom data content and a custom content type header. The values included in the HTTPBody response will be used verbatim in the returned message from the gateway. Make sure you format your response carefully!

## Example Usage
1. Create a mux with the HTTP Body Marshaler as option. 

```golang 
	mux := runtime.NewServeMux(runtime.SetHTTPBodyMarshaler)
```
2. Define your service in gRPC with an httpbody response message

```golang
import "google/api/httpbody.proto";
import "google/api/annotations.proto";
import "google/protobuf/empty.proto";

service HttpBodyExampleService {

 	rpc HelloWorld(google.protobuf.Empty) returns (google.api.HttpBody) {
		option (google.api.http) = {
			get: "/helloworld"
		};
	}	

}
```
3. Generate gRPC and reverse-proxy stubs and implement your service.

## Example service implementation

```golang
func (*HttpBodyExampleService) Helloworld(ctx context.Context, in *empty.Empty) (*httpbody.HttpBody, error) {
	return &httpbody.HttpBody{
		ContentType: "text/html",
		Data:        []byte("Hello World"),
	}, nil
}

```