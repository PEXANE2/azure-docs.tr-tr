---
title: HeadPose özniteliğini kullanma
titleSuffix: Azure Cognitive Services
description: HeadPose özniteliğini kullanarak yüz dikdörtgenini otomatik olarak döndürdüğünüz veya video akışında kafa hareketlerini nasıl algılayıcı kullanacağınızı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 534846044770d66ec5171ad4f61de921d2d5d194
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169798"
---
# <a name="use-the-headpose-attribute"></a>HeadPose özniteliğini kullanma

Bu kılavuzda, bazı önemli senaryoları etkinleştirmek için algılanan bir yüzün HeadPose özniteliğini nasıl kullanabileceğinizi göreceksiniz.

## <a name="rotate-the-face-rectangle"></a>Yüz dikdörtgenini döndürün

Algılanan her yüzle döndürülen yüz dikdörtgeni, görüntüdeki yüzün konumunu ve boyutunu işaretler. Varsayılan olarak, dikdörtgen her zaman görüntüyle hizalanır (kenarları dikey ve yataydır); bu, açılı yüzleri çerçeveleme için verimsiz olabilir. Görüntüdeki yüzleri programlı bir şekilde kırpmak istediğiniz durumlarda, dikdörtgeni kırpmak için döndürebilmek daha iyidir.

[Bilişsel Hizmetler Yüz WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) örnek uygulaması algılanan yüz dikdörtgenleri döndürmek için HeadPose özniteliği ni kullanır.

### <a name="explore-the-sample-code"></a>Örnek kodu keşfedin

HeadPose özniteliğini kullanarak yüz dikdörtgenini programlı bir şekilde döndürebilirsiniz. Yüzleri algılarken bu özniteliği belirtirseniz [(yüzleri nasıl algılayacak gerektiğine](HowtoDetectFacesinImage.md)bakın), daha sonra sorgulayabilirsiniz. [Bilişsel Hizmetler Yüz WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) uygulamasından aşağıdaki **yöntem, Algısal Yüz** nesnelerinin bir listesini alır ve **[Yüz](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** nesnelerinin bir listesini döndürür. **Face** here, güncelleştirilmiş dikdörtgen koordinatları da dahil olmak üzere yüz verilerini depolayan özel bir sınıftır. Yeni değerler **üst,** **sol**, **genişlik**ve **yükseklik**için hesaplanır ve yeni bir alan **FaceAngle** döndürme belirtir.

```csharp
/// <summary>
/// Calculate the rendering face rectangle
/// </summary>
/// <param name="faces">Detected face from service</param>
/// <param name="maxSize">Image rendering size</param>
/// <param name="imageInfo">Image width and height</param>
/// <returns>Face structure for rendering</returns>
public static IEnumerable<Face> CalculateFaceRectangleForRendering(IList<DetectedFace> faces, int maxSize, Tuple<int, int> imageInfo)
{
    var imageWidth = imageInfo.Item1;
    var imageHeight = imageInfo.Item2;
    var ratio = (float)imageWidth / imageHeight;
    int uiWidth = 0;
    int uiHeight = 0;
    if (ratio > 1.0)
    {
        uiWidth = maxSize;
        uiHeight = (int)(maxSize / ratio);
    }
    else
    {
        uiHeight = maxSize;
        uiWidth = (int)(ratio * uiHeight);
    }

    var uiXOffset = (maxSize - uiWidth) / 2;
    var uiYOffset = (maxSize - uiHeight) / 2;
    var scale = (float)uiWidth / imageWidth;

    foreach (var face in faces)
    {
        var left = (int)(face.FaceRectangle.Left * scale + uiXOffset);
        var top = (int)(face.FaceRectangle.Top * scale + uiYOffset);

        // Angle of face rectangles, default value is 0 (not rotated).
        double faceAngle = 0;

        // If head pose attributes have been obtained, re-calculate the left & top (X & Y) positions.
        if (face.FaceAttributes?.HeadPose != null)
        {
            // Head pose's roll value acts directly as the face angle.
            faceAngle = face.FaceAttributes.HeadPose.Roll;
            var angleToPi = Math.Abs((faceAngle / 180) * Math.PI);

            // _____       | / \ |
            // |____|  =>  |/   /|
            //             | \ / |
            // Re-calculate the face rectangle's left & top (X & Y) positions.
            var newLeft = face.FaceRectangle.Left +
                face.FaceRectangle.Width / 2 -
                (face.FaceRectangle.Width * Math.Sin(angleToPi) + face.FaceRectangle.Height * Math.Cos(angleToPi)) / 2;

            var newTop = face.FaceRectangle.Top +
                face.FaceRectangle.Height / 2 -
                (face.FaceRectangle.Height * Math.Sin(angleToPi) + face.FaceRectangle.Width * Math.Cos(angleToPi)) / 2;

            left = (int)(newLeft * scale + uiXOffset);
            top = (int)(newTop * scale + uiYOffset);
        }

        yield return new Face()
        {
            FaceId = face.FaceId?.ToString(),
            Left = left,
            Top = top,
            OriginalLeft = (int)(face.FaceRectangle.Left * scale + uiXOffset),
            OriginalTop = (int)(face.FaceRectangle.Top * scale + uiYOffset),
            Height = (int)(face.FaceRectangle.Height * scale),
            Width = (int)(face.FaceRectangle.Width * scale),
            FaceAngle = faceAngle,
        };
    }
}
```

### <a name="display-the-updated-rectangle"></a>Güncelleştirilmiş dikdörtgeni görüntüleme

Buradan, ekranınızdaki döndürülen **Yüz** nesnelerini kullanabilirsiniz. [FaceDetectionPage.xaml'ın](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) aşağıdaki satırları, yeni dikdörtgenin bu verilerden nasıl işlenir olduğunu gösterir:

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="detect-head-gestures"></a>Kafa hareketlerini algılama

HeadPose değişikliklerini gerçek zamanlı olarak izleyerek kafa sallayarak ve kafa sallayarak kafa hareketlerini algılayabilirsiniz. Bu özelliği özel bir canlılık dedektörü olarak kullanabilirsiniz.

Canlılık algılama, bir öznenin görüntü veya video gösterimi değil, gerçek bir kişi olduğunu belirleme görevidir. Bir baş hareket dedektörü canlılık doğrulamak için bir yol olarak hizmet verebilir, özellikle bir kişinin bir görüntü temsilaksine.

> [!CAUTION]
> Kafa hareketlerini gerçek zamanlı olarak algılamak için Yüz API'sini yüksek oranda (saniyede birden fazla) aramanız gerekir. Serbest katman (f0) aboneliğiniz varsa, bu mümkün olmayacaktır. Ücretli bir katman aboneliğiniz varsa, kafa hareketi algılaması için hızlı API çağrıları yapmanın maliyetini hesapladığınıza emin olun.

Baş hareketi algılamanın çalışma örneği için GitHub'daki [Yüz Kafası Örneği'ne](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceAPIHeadPoseSample) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Döndürülmüş yüz dikdörtgenlerinin çalışma örneği için GitHub'daki [Bilişsel Hizmetler Yüz WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) uygulamasına bakın. Veya kafa hareketlerini algılamak için HeadPose özniteliğini gerçek zamanlı olarak izleyen [Face HeadPose Sample](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) uygulamasına bakın.