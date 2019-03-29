# Overview

This project is created to provide a simple HTTP Binding for Azure Functions.

# Supported features

The following commands are supported as of now.

## GET

Obviously this is an input binding and can be used like this.

	[FunctionName("MyInputSample")]
	public static void MyInputSample(
		[HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "MyInputSample")]
		HttpRequestMessage req,
		[Http(Url = "%BaseAddress%api/Echo/{Identifier}")] 
		HttpModel httpModel,
		ILogger log)
	{
		log.LogInformation($"Executing MyInputSample function.");
		// Over here you can do something with the HttpModel.
		log.LogInformation($"Executed MyInputSample function.");
	}

The `HttpModel` consists of both the response (`Content`) of the request as well as the `StatusCode`.

	public class HttpModel
	{
		public string Response { get; set; }
		public int ResponseCode { get; set; }
	}

## POST

An output binding which will invoke a HTTP POST to the specified URL with an object.

	[FunctionName("MyOutputPostSample")]
	public static void MyOutputPostSample(
		[HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "MyInputSample")]
		HttpRequestMessage req,
		[HttpCommand(CommandUrl = "%BaseAddress%api/EchoCommand", MediaType = "application/json", HttpMethod = "POST")]
		IAsyncCollector<HttpCommand> httpPostCommandCollector,
		ILogger log)
	{
		var identifier = Guid.NewGuid();
		log.LogInformation($"Executing MyOutputPostSample function.");

		var model = new ModelToSend
		{
			Identifier = identifier.ToString("D")
		};
		httpPostCommandCollector.AddAsync(new HttpCommand(JsonConvert.SerializeObject(model)));

		log.LogInformation($"Executed MyOutputPostSample function.");
	}

## PUT

An output binding which will invoke a HTTP PUT to the specified URL with an object.

	[FunctionName("MyOutputPutSample")]
	public static void MyOutputPutSample(
		[HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = "MyInputSample")]
		HttpRequestMessage req,
		[HttpCommand(CommandUrl = "%BaseAddress%api/EchoCommand", MediaType = "application/json", HttpMethod = "POST")]
		IAsyncCollector<HttpCommand> httpPostCommandCollector,
		ILogger log)
	{
		var identifier = Guid.NewGuid();
		log.LogInformation($"Executing MyOutputPutSample function.");

		var model = new ModelToSend
		{
			Identifier = identifier.ToString("D")
		};
		httpPostCommandCollector.AddAsync(new HttpCommand(JsonConvert.SerializeObject(model)));

		log.LogInformation($"Executed MyOutputPutSample function.");
	}