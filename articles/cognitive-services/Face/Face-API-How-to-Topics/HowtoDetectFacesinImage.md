---
title: Görüntüdeki yüzleri algılama - Yüz
titleSuffix: Azure Cognitive Services
description: Bu kılavuz, cinsiyet, yaş veya belirli bir görüntüden poz gibi öznitelikleri ayıklamak için yüz algılama nasıl kullanılacağını gösterir.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: sbowles
ms.openlocfilehash: 7070cb3bcd1b519828a750cf4ba6caf7ecb34bbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76169887"
---
# <a name="get-face-detection-data"></a>Yüz algılama verilerini alın

Bu kılavuz, cinsiyet, yaş veya belirli bir görüntüden poz gibi öznitelikleri ayıklamak için yüz algılama nasıl kullanılacağını gösterir. Bu kılavuzdaki kod parçacıkları Azure Bilişsel Hizmetler Yüz istemci kitaplığı kullanılarak C# olarak yazılır. Aynı işlevsellik REST [API'si](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)aracılığıyla kullanılabilir.

Bu kılavuz, nasıl gösteriş yapılacağını gösterir:

- Görüntüdeki yüzlerin konumlarını ve boyutlarını alın.
- Bir görüntüde gözbebekleri, burun ve ağız gibi çeşitli yüz simgelerinin konumlarını alın.
- Tespit edilen bir yüzün cinsiyetini, yaşını, duygusunu ve diğer özelliklerini tahmin edin.

## <a name="setup"></a>Kurulum

Bu kılavuz, Face abonelik anahtarı ve bitiş `faceClient`noktası URL'si ile adında bir [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) nesnesi zaten inşa ettiğinizi varsayar. Buradan, bu kılavuzda kullanılan [DetectWithUrlAsync veya DetectWithStreamAsync'i](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet)arayarak yüz algılama özelliğini kullanabilirsiniz. [DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet) Bu özelliğin nasıl ayarlanacağına ilişkin talimatlar için hızlı başlangıçlardan birini izleyin.

Bu kılavuz, hangi bağımsız değişkenleri geçirebileceğiniz ve döndürülen verilerle neler yapabileceğiniz gibi Algıla aramasının özelliklerine odaklanır. Yalnızca ihtiyacınız olan özellikleri sorgulamanızı öneririz. Her işlemin tamamlanması ek zaman alır.

## <a name="get-basic-face-data"></a>Temel yüz verilerini alın

Yüzleri bulmak ve konumlarını bir görüntüde almak için, _returnFaceId_ parametresi ile yöntemi **doğru**olarak ayarlayın. Bu varsayılan ayardır.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

Döndürülen [AlgıYüz](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) nesnelerini benzersiz kimliklerini ve yüzün piksel koordinatlarını veren bir dikdörtgen için sorgulayabilirsiniz.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Yüzün konumunu ve boyutlarını nasıl ayrıştıracakları hakkında bilgi için [FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet)bölümüne bakın. Genellikle, bu dikdörtgen gözleri içerir, kaşlar, burun, ve ağız. Başın, kulakların ve çenenin üst kısmı mutlaka dahil değildir. Tam bir kafa kırpmak veya bir orta çekim portre almak için yüz dikdörtgeni kullanmak için, belki bir fotoğraf kimliği türü görüntü için, her yönde dikdörtgen genişletebilirsiniz.

## <a name="get-face-landmarks"></a>Yüz simgelerini alın

[Yüz işaretleri,](../concepts/face-detection.md#face-landmarks) göz bebekleri veya burnun ucu gibi bir yüzde bulunması kolay noktalar kümesidir. Yüz işareti verilerini almak için _returnFaceLandmarks_ parametresini **doğru**olarak ayarlayın.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

Aşağıdaki kod, burnun ve göz bebeklerinin konumlarını nasıl alabileceğinizi gösterir:

```csharp
foreach (var face in faces)
{
    var landmarks = face.FaceLandmarks;

    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;

    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;

    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
```

Yüzün yönünü doğru hesaplamak için yüz işaretleri verilerini de kullanabilirsiniz. Örneğin, yüzün dönüşünü ağzın merkezinden gözlerin ortasına kadar bir vektör olarak tanımlayabilirsiniz. Aşağıdaki kod bu vektörü hesaplar:

```csharp
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;

var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);

var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;

var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);

Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
```

Yüzün yönünü bildiğinizde, dikdörtgen yüz çerçevesini daha düzgün hizalamak için döndürebilirsiniz. Görüntüde yüz kırpmak için, yüzlerin her zaman dik görünmesi için görüntüyü programlı bir şekilde döndürebilirsiniz.

## <a name="get-face-attributes"></a>Yüz özniteliklerini alma

Yüz dikdörtgenleri ve simgesel işaretlerin yanı sıra, yüz algılama API'sı bir yüzün çeşitli kavramsal özelliklerini analiz edebilir. Tam liste için [Yüz öznitelikleri](../concepts/face-detection.md#attributes) kavramsal bölümüne bakın.

Yüz özniteliklerini çözümlemek için _returnFaceAttributes_ parametresini [FaceAttributeType Enum](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) değerleri listesine ayarlayın.

```csharp
var requiredFaceAttributes = new FaceAttributeType[] {
    FaceAttributeType.Age,
    FaceAttributeType.Gender,
    FaceAttributeType.Smile,
    FaceAttributeType.FacialHair,
    FaceAttributeType.HeadPose,
    FaceAttributeType.Glasses,
    FaceAttributeType.Emotion
};
var faces = await faceClient.DetectWithUrlAsync(imageUrl, true, false, requiredFaceAttributes);
```

Ardından, döndürülen verilere başvuru alın ve ihtiyaçlarınıza göre daha fazla işlem yapın.

```csharp
foreach (var face in faces)
{
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
    var emotion = attributes.Emotion;
}
```

Özniteliklerin her biri hakkında daha fazla bilgi edinmek için [Yüz algılama ve öznitelikler](../concepts/face-detection.md) kavramsal kılavuzuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, yüz algılamanın çeşitli işlevlerini kullanmayı öğrendiniz. Ardından, ayrıntılı bir öğretici izleyerek bu özellikleri uygulamanız içine entegre edin.

- [Öğretici: Görüntüdeki yüz verilerini görüntülemek için bir WPF uygulaması oluşturun](../Tutorials/FaceAPIinCSharpTutorial.md)
- [Öğretici: Resimdeki yüzleri algılamak ve çerçeve içine almak için Android uygulaması oluşturma](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)

## <a name="related-topics"></a>İlgili konular

- [Referans belgeleri (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Referans belgeleri (.NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)