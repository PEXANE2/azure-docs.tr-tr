---
title: Görüntüde yüzleri algılayın
titleSuffix: Azure Cognitive Services
description: Bu kılavuzda, belirli bir görüntüden cinsiyet, Age veya poz gibi öznitelikleri ayıklamak için yüz algılamanın nasıl kullanılacağı gösterilmektedir.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: sbowles
ms.custom: devx-track-csharp
ms.openlocfilehash: 500099753ee4fe47f02e7f09d9732b71aa3bae36
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91856374"
---
# <a name="get-face-detection-data"></a>Yüz algılama verilerini al

Bu kılavuzda, belirli bir görüntüden cinsiyet, Age veya poz gibi öznitelikleri ayıklamak için yüz algılamanın nasıl kullanılacağı gösterilmektedir. Bu kılavuzdaki kod parçacıkları, Azure bilişsel hizmetler yüz istemci kitaplığı kullanılarak C# dilinde yazılmıştır. Aynı işlevsellik [REST API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)ile kullanılabilir.

Bu kılavuzda nasıl yapılacağı gösterilmektedir:

- Görüntüdeki yüzlerin konumlarını ve boyutlarını alın.
- Bir görüntüde pupıls, burun ve ağız gibi çeşitli yüzlerin konumlarını alın.
- Bir algılanan yüzün cinsiyeti, yaşını, duygu ve diğer özniteliklerini tahmin edin.

## <a name="setup"></a>Kurulum

Bu kılavuzda [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) `faceClient` , bir yüz abonelik anahtarı ve uç nokta URL 'si olan adlı bir faceclient nesnesi oluşturmuş olduğunuz varsayılmaktadır. Buradan, bu kılavuzda kullanılan [DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet), ya da [Detectwithstreamasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet)' i çağırarak yüz algılama özelliğini kullanabilirsiniz. Bu özelliği ayarlama hakkında yönergeler için hızlı başlangıçlardan birini izleyin.

Bu kılavuz, hangi bağımsız değişkenleri geçebileceğinize ve döndürülen verilerle yapabileceklerinize ilişkin olarak, algılama çağrısının özelliklerine odaklanır. Yalnızca ihtiyacınız olan özellikleri sorgulamanızı öneririz. Her işlemin tamamlanabilmesi için ek süre sürer.

## <a name="get-basic-face-data"></a>Temel yüz verilerini al

Yüzeyleri bulmak ve konumlarını bir görüntüde almak için, [DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet) veya [Detectwithstreamasync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet) yöntemini, _returnceıd_ parametresi **true**olarak ayarlanmış şekilde çağırın. Bu varsayılan ayardır.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="basic1":::

Döndürülen [Detectedface](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) nesnelerini benzersiz kimlikleri ve yüzün piksel koordinatlarını sağlayan bir dikdörtgen için sorgulayabilirsiniz.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="basic2":::

Yüzün konumunu ve boyutlarını ayrıştırma hakkında daha fazla bilgi için bkz. [Facerectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet). Bu dikdörtgen genellikle gözler, eyebro, burun ve ağız içerir. Baş, korun ve Chin en üstü dahil değildir. Yüz dikdörtgenini, tam bir kafa kırpmak veya orta ölçekli bir dikey almak için, belki de fotoğraf KIMLIĞI türünde bir görüntü için dikdörtgeni her yönde genişletebilirsiniz.

## <a name="get-face-landmarks"></a>Yüz çizgilerini al

[Yüz yüzleri](../concepts/face-detection.md#face-landmarks) , bir yüz üzerinde yer alan, pupıls veya burun ipucu gibi kolay bir arama noktası kümesidir. Yüz yer işareti verilerini almak için _detectionModel_ parametresini **detectionModel. Detection01** ve _returncelandiþaretleri_ parametresini **true**olarak ayarlayın.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="landmarks1":::

Aşağıdaki kod, burun ve pupler 'in konumlarını nasıl alacağınızı gösterir:

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="landmarks2":::

Yüzün yönünü doğru şekilde hesaplamak için yüz surtik verilerini de kullanabilirsiniz. Örneğin, yüzün merkezinden bir vektör olarak dönüşü, gözlerinizin merkezine bir vektör olarak tanımlayabilirsiniz. Aşağıdaki kod bu vektörü hesaplar:

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="direction":::

Yüzün yönünü bildiğiniz zaman, dikdörtgen yüzey çerçevesini daha doğru hizalamak için döndürebilirsiniz. Görüntüdeki yüzeyleri kırpmak için, yüzeyleri her zaman sağ görünecek şekilde görüntüyü program aracılığıyla döndürebilirsiniz.

## <a name="get-face-attributes"></a>Yüz özniteliklerini Al

Yüz algılama API 'SI, yüz dikdörtgeni ve yer işaretlerinin yanı sıra bir yüzün çeşitli kavramsal özniteliklerini analiz edebilir. Tam liste için, [yüz nitelikleri](../concepts/face-detection.md#attributes) kavramsal bölümüne bakın.

Yüz özniteliklerini çözümlemek için _detectionModel_ parametresini **detectionModel. Detection01** ve _returnfaceattributes_ parametresini bir [faceattributetype numaralandırma](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) değerleri listesine ayarlayın.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="attributes1":::

Ardından, döndürülen verilere yönelik başvuruları alın ve gereksinimlerinize göre daha fazla işlem yapın.

:::code language="csharp" source="~/cognitive-services-quickstart-code/dotnet/Face/sdk/detect.cs" id="attributes2":::

Özniteliklerin her biri hakkında daha fazla bilgi edinmek için, [yüz algılama ve öznitelikler](../concepts/face-detection.md) kavramsal Kılavuzu ' na bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuzda, çeşitli yüz algılama işlevlerini nasıl kullanacağınızı öğrendiniz. Daha sonra, derinlemesine bir öğreticiyi izleyerek bu özellikleri uygulamanız ile tümleştirin.

- [Öğretici: görüntüde yüz verileri göstermek için WPF uygulaması oluşturma](../Tutorials/FaceAPIinCSharpTutorial.md)

## <a name="related-topics"></a>İlgili konular

- [Başvuru belgeleri (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Başvuru belgeleri (.NET SDK)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/faceapi?view=azure-dotnet)
