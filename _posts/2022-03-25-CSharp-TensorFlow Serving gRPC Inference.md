---
title: Tensorflow Serving gRPC C# 구현
author: BeomBeomJoJo
date: 2022-03-25 11:33:00 +0800
categories: [C#, .NET6, gRPC]
tags: [C#, .NET6, gRPC]
math: true
mermaid: true
---

## **참조**
* https://developers.google.com/protocol-buffers/docs/csharptutorial
* https://github.com/figroc/tensorflow-serving-client

<br/>

## **목적**
* C#으로 Tensorflow Serving 코드 구현하여 Inference 되는지 POC 진행하였습니다.

<br/>

## **개발 환경**
* **.NET 버전 : .NET 6****
* **개발 도구 : Visual Studio 2022**
* **사용 Nuget Package : Tensorflow-serving-client, Newtonsoft.json**

<br/>

## **C# Tensroflow Serving 프로젝트 구성**
* Tensorflow Serving 프로젝트 구성은 다음과 같이 구성하였습니다.
  * **Example : 1부터 10까지의 mnist 이미지 저장소**
  * **Protos : protocalBuf 저장소**
  * **Utils : Tensorflow Serving 코드 구현에 필요한 Helper 클래스 저장소**
  * **MainService : Main함수 있는 C# 프로젝트**

<br/>

## **Tensorflow Serving gRPC C# 메인 코드**
* C#에서 Tensorflow Serving 을 gRPC로 구현하여 Inference 하는 메인 프로젝트의 코드는 다음과 같습니다.
* 코드 흐름을 간단히 설명 드리도록 하겠습니다.
  1. TensorFlow Serving Server와 통신할 gRPC channel, client 를 설정
  2. 사용 가능한 MNIST 모델 확인
  3. 메타 데이터 input, output 파라미터 값 가져오기
  4. 검증에 필요한 이미지 개수만큼 반복하여, Predict 예측하기
  5. 실제 값과 검증 값 비교하여 출력하기
* 크게 Main 코드의 흐름은 위와 같습니다.

```csharp
using Grpc.Core;
using gRPCTensorflowServing.Utils;
using Tensorflow.Serving;

// 채널 설정 (IP, PORT 는 Tensorflow Serving 서버 정보)
var channel = new Channel("localhost:8500", ChannelCredentials.Insecure);
var client = new PredictionService.PredictionServiceClient(channel);

// 사용 가능한 MNIST 모델 확인
var response = client.GetModelMetadata(new GetModelMetadataRequest()
{
   ModelSpec = new ModelSpec() { Name = ModelMethodClasses.ModelSpecName },
   MetadataField = { ModelMethodClasses.MetadataField }
});

// 메타데이터 input, ouput 값 가져오기
MetadataParsing parse = new();
var webRequest = parse.CreateRequest("http://localhost:8501/v1/models/mnist-model/metadata");
var webResponse = parse.CreateResponse(webRequest);
string data = parse.GetMetadata(webResponse);
string input = parse.GetParameter(data, "inputs"); // input 메타데이터 정보
string output = parse.GetParameter(data, "outputs"); // output 메타데이터 정보

Console.WriteLine($"Model Available: {response.ModelSpec.Name} Ver.{response.ModelSpec.Version}");

// Inference 시작 - 이미지 개수 만큼
for(int number = 0; number < 10; number++)
{
   // Prediction Request 생성
   var request = new PredictRequest()
   {
      ModelSpec = new ModelSpec() { Name = ModelMethodClasses.ModelSpecName, SignatureName = ModelMethodClasses.PredictImages }
   };

   // 이미지 Tensor 추가 [1 - 784]
   var mnistImageName = $"{AppDomain.CurrentDomain.BaseDirectory }/{ModelMethodClasses.ImageFolderDirectory}/{number}.bmp";
   using (Stream stream = new FileStream($"{mnistImageName}", FileMode.Open))
   {
      request.Inputs.Add(input, TensorBuilder.CreateTensorFromImage(stream, 255.0f));
   }

   // 예측 값 가져오기
   var predictResponse = client.Predict(request);

   //Compute Max value from prediction array
   var maxValue = predictResponse.Outputs[output].FloatVal.Max();
   //Get index of predicted value
   var predictedValue = predictResponse.Outputs[output].FloatVal.IndexOf(maxValue);

   Console.WriteLine($"Predict: {number} {(number == predictedValue ? "Y" : "N")}");
   Console.WriteLine($"Result value: {predictedValue}, probability: {maxValue}");
   Console.WriteLine($"All values: {predictResponse.Outputs[output].FloatVal}");
   Console.WriteLine("");
}

channel.ShutdownAsync().Wait();
```

<br/>

## **실행 결과**
* 실행 결과, C#으로 TensorFlow Inference 정상적으로 진행되는 것 확인하였습니다.

```
Model Available: mnist-model Ver.1
Predict: 0 Y
Result value: 0, probability: 0.9551584
All values: [ 0.9551584, 1.4487581e-06, 0.04432098, 0.00039069273, 6.0360883e-10, 2.1820162e-05, 5.5765076e-07, 3.5178113e-05, 3.4397831e-07, 7.049199e-05 ]

Predict: 1 Y
Result value: 1, probability: 0.9978509
All values: [ 1.74891e-07, 0.9978509, 9.433564e-08, 2.7461786e-07, 0.002071152, 3.0861179e-06, 5.2460973e-07, 6.152771e-05, 1.21720295e-05, 2.9181262e-08 ]

Predict: 2 Y
Result value: 2, probability: 0.996099
All values: [ 0.0006271216, 0.00066090975, 0.996099, 0.0005590186, 7.274455e-11, 1.0616196e-06, 6.892166e-07, 2.9520946e-08, 0.0020521132, 2.1713186e-08 ]

Predict: 3 Y
Result value: 3, probability: 0.9757952
All values: [ 1.27248e-08, 1.0377942e-05, 6.3910375e-06, 0.9757952, 2.0217285e-06, 0.02417695, 3.482116e-07, 3.023068e-07, 8.1433245e-06, 2.4138964e-07 ]

Predict: 4 Y
Result value: 4, probability: 0.86466795
All values: [ 0.0061049354, 0.00028904926, 0.025137775, 5.1506267e-05, 0.86466795, 0.013524423, 0.06365003, 0.009256375, 0.017284125, 3.3808414e-05 ]

Predict: 5 Y
Result value: 5, probability: 0.58344865
All values: [ 4.457224e-06, 1.6846321e-05, 5.3268764e-06, 1.2049274e-05, 4.421595e-07, 0.58344865, 0.00022240315, 0.00013144493, 0.22625506, 0.18990324 ]

Predict: 6 N
Result value: 5, probability: 0.9791316
All values: [ 0.00027174217, 1.8444723e-06, 9.016209e-05, 0.012975363, 2.7820413e-06, 0.9791316, 0.0026335707, 0.00040962524, 0.0039110803, 0.0005723025 ]

Predict: 7 N
Result value: 3, probability: 0.9549715
All values: [ 2.2315381e-14, 3.6238573e-05, 0.0007003575, 0.9549715, 1.3756849e-13, 4.2566246e-12, 2.280724e-16, 0.044291973, 1.785482e-09, 1.7748846e-13 ]

Predict: 8 Y
Result value: 8, probability: 0.57427007
All values: [ 0.004614033, 0.016874775, 0.0009916167, 0.030640299, 5.7397654e-05, 0.37145382, 0.00092690485, 5.3614644e-07, 0.57427007, 0.00017059203 ]

Predict: 9 Y
Result value: 9, probability: 0.96771795
All values: [ 3.8007954e-11, 5.7989e-07, 1.9819613e-08, 0.031727865, 0.0004046025, 1.6353757e-05, 5.08366e-11, 0.00013149057, 1.1504455e-06, 0.96771795 ]
```

<br/>
