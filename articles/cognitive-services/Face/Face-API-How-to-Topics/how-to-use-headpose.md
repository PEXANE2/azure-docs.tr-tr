---
title: HeadPose özniteliğini kullanma
titleSuffix: Azure Cognitive Services
description: Yüz dikdörtgenini otomatik olarak döndürmek veya bir video akışında baş hareketleri algılamak için Headpoz özniteliğini nasıl kullanacağınızı öğrenin.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 534846044770d66ec5171ad4f61de921d2d5d194
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "76169798"
---
# <a name="use-the-headpose-attribute"></a>HeadPose özniteliğini kullanma

Bu kılavuzda, bazı önemli senaryoları etkinleştirmek için algılanan bir yüzün Headpoz özniteliğini nasıl kullanabileceğinizi göreceksiniz.

## <a name="rotate-the-face-rectangle"></a>Yüz dikdörtgenini döndürme

Algılanan her yüz ile döndürülen yüz dikdörtgeni, görüntüdeki yüzün konumunu ve boyutunu işaretler. Varsayılan olarak, dikdörtgen her zaman görüntüyle hizalanır (kenarları dikey ve yatay olur); Bu, çerçeveleme açılı yüzleri için verimsiz olabilir. Görüntüdeki yüzeyleri program aracılığıyla kırpmak istediğiniz durumlarda, kırpmak için dikdörtgeni döndürebilmek daha iyidir.

Bilişsel [Hizmetler yüz WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) örnek uygulaması, algılanan yüz dikdörtgenlerini döndürmek Için headpoz özniteliğini kullanır.

### <a name="explore-the-sample-code"></a>Örnek kodu keşfet

Baş dikdörtgeni, Headpoz özniteliğini kullanarak programlı bir şekilde döndürebilirsiniz. Yüzeyleri algılamadığında bu özniteliği belirtirseniz (bkz. [yüzleri algılama](HowtoDetectFacesinImage.md)), daha sonra sorgulayabilirsiniz. Bilişsel [Hizmetler yüz WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) uygulamasının aşağıdaki yöntemi, **algılayıcısı geçen yüz** nesnelerinin bir listesini alır ve **[yüz](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** nesnelerinin bir listesini döndürür. Buradaki **yüz** , güncelleştirilmiş dikdörtgen koordinatları dahil yüz verileri depolayan özel bir sınıftır. Yeni değerler üst, **sol**, **Genişlik**ve **Yükseklik**için hesaplanır ve yeni bir alan olan **çok** **yönlü açı** döndürme işlemini belirtir.

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

Buradan, görüntüinizdeki döndürülen **yüzey** nesnelerini kullanabilirsiniz. [Facedetectionpage. xaml](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) tarafından verilen aşağıdaki satırlar, yeni dikdörtgenin bu verilerden nasıl işleneceğini gösterir:

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="detect-head-gestures"></a>Baş hareketleri Algıla

Yayın pozları gerçek zamanlı olarak izleyerek nodding ve Head gerçekleşmesi gibi baş hareketleri tespit edebilirsiniz. Bu özelliği, özel bir özel algılayıcı algılayıcısı olarak kullanabilirsiniz.

Lizleştirme algılama, bir konunun bir görüntü veya video temsili değil gerçek bir kişi olduğunu belirleme görevidir. Bir baş hareket algılayıcısı, özellikle de bir kişinin görüntü gösteriminin aksine, sızma doğrulamanızı sağlamaya yardımcı olmak için bir yol sunar.

> [!CAUTION]
> Baş hareketleri gerçek zamanlı olarak algılamak için, Yüz Tanıma API'si yüksek bir hızda (saniye başına birden fazla) çağırmanız gerekir. Ücretsiz bir katmanlı (F0) aboneliğiniz varsa, bu mümkün olmayacaktır. Ücretli bir aboneliğiniz varsa, baş hareket algılaması için hızlı API çağrıları yapma maliyetlerini hesapladığınızdan emin olun.

Baş hareket algılamasında çalışan bir örnek için GitHub 'daki [yüz Headpoz örneğine](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceAPIHeadPoseSample) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Döndürülmüş yüz dikdörtgenlerinin çalışan bir örneği için GitHub 'da bilişsel [Hizmetler 'ın WPF](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) uygulamasına bakın. Ya da, baş hareketleri algılamak için Headpoz özniteliğini gerçek zamanlı olarak izleyen [yüz Headpoz örnek](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) uygulamasına bakın.