---
title: İçerik duyarlı kodlama için önceden ayarlanmış - Azure Medya Hizmetleri
description: Bu makalede, Microsoft Azure Media Services v3'te içerik duyarlı kodlama açıklanmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 01/24/2020
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 3ea6c4226a59ba020a477cc5811033ff3dc3c2e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76772074"
---
# <a name="use-the-content-aware-encoding-preset-to-find-the-optimal-bitrate-value-for-a-given-resolution"></a>Belirli bir çözünürlük için en uygun bit hızı değerini bulmak için içerik duyarlı kodlama ön ayarını kullanın

Adaptif [bitrate akışı](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)ile teslimat için içerik hazırlamak için, video birden fazla bit hızında (düşük yüksek) kodlanması gerekir. Bu, bitrate düşürüldükçe videonun çözünürlüğü gibi, kalitenin zarif bir şekilde bozulmasını sağlar. Bu tür çoklu bit hızı kodlama, kod çözme merdiveni olarak adlandırılan bir tabloolan çözüm ve bitrates tablosundan yararlanılır, [bkz.](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset)

İşlediğiniz içeriğin farkında olmalı ve kodlama merdivenini tek tek videonun karmaşıklığına göre özelleştirmeli/ayarlamalısınız. Her çözünürlükte, kalitedeki herhangi bir artışın algısal olmadığı bir bithızı vardır – kodlayıcı bu optimum bithızı değeriyle çalışır. Bir sonraki optimizasyon düzeyi, içeriğe dayalı çözünürlükleri seçmektir – örneğin, PowerPoint sunumunun videosu 720p'nin altına inmekten yarar lanamaz. Daha ileri giderek, kodlayıcı video içinde her çekim için ayarları optimize etmek için görevlendirilebilir. 

Microsoft'un [Uyarlanabilir Akış](autogen-bitrate-ladder.md) önceden ayarlanmış kısmen kalite ve kaynak videoların çözümünde değişkenlik sorunu giderir. Müşterilerimiz, bazıları 1080p, diğerleri 720p ve SD ve daha düşük çözünürlükte birkaç içerik çeşitli bir karışımı var. Ayrıca, tüm kaynak içeriği film veya TV stüdyolarından yüksek kaliteli mezzaninler olduğunu. Uyarlanabilir Akış önayı, bithızı merdiveninin çözünürlüğü veya giriş asma katının ortalama bit hızını asla aşmamasını sağlayarak bu sorunları giderır. Ancak, bu ön ayar çözünürlük ve bitrate dışında kaynak özelliklerini incelemez.

## <a name="the-content-aware-encoding"></a>İçerik duyarlı kodlama 

İçerik farkında kodlama ön ayarı, kodlayıcının belirli bir çözünürlük için en uygun bithızı değerini aramasını sağlayan, ancak kapsamlı hesaplamalı analiz gerektirmeden özel bir mantık ekleyerek "uyarlanabilir bitrate akışı" mekanizmasını genişletir. Bu önceden ayarlanmış, GOP uyumlu bir MP4 kümesi üretir. Herhangi bir giriş içeriği göz önüne alındığında, hizmet giriş içeriğinin ilk hafif analizini gerçekleştirir ve sonuçları uyarlanabilir akışla teslim için en uygun katman sayısını, uygun bit hızı ve çözünürlük ayarlarını belirlemek için kullanır. Bu ön ayar, özellikle düşük ve orta karmaşıklıkta videolar için etkilidir, çıktı dosyaları Adaptive Streaming önceden ayarlanmış daha düşük bitrates olacak ama yine de izleyiciler için iyi bir deneyim sunan bir kalitede. Çıkış video ve ses interleaved mp4 dosyaları içerecektir

Aşağıdaki örnek [grafikler, PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) ve [VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion)gibi kalite ölçümleri kullanılarak karşılaştırmayı göstermektedir. Kaynak, kodlayıcıyı vurgulamak amacıyla film ve TV programlarından gelen yüksek karmaşıklıktaki çekimlerin kısa kliplerinin biraraya getirmekle oluşturuldu. Tanım olarak, bu ön ayar içerikten içeriğe değişen sonuçlar üretir – aynı zamanda bazı içerikler için bitrate'de önemli bir azalma veya kalitede iyileşme olmayacağı anlamına da gelir.

![PSNR kullanarak hız bozulması (RD) eğrisi](media/content-aware-encoding/msrv1.png)

**Şekil 1: Yüksek karmaşıklık kaynağı için PSNR metrik kullanılarak hız bozulması (RD) eğrisi**

![VMAF kullanarak hız bozulması (RD) eğrisi](media/content-aware-encoding/msrv2.png)

**Şekil 2: Yüksek karmaşıklık kaynağı için VMAF metrik kullanılarak hız bozulması (RD) eğrisi**

Aşağıda, kodlayıcının girişin düşük kalitede olduğunu (düşük bit hızı nedeniyle birçok sıkıştırma yapıları) olduğunu belirleyebildiği başka bir kaynak içeriği kategorisinin sonuçları verilmiştir. İçeriğe duyarlı önceden ayarlanmış olan kodlayıcının, çoğu istemcinin oyalanmadan akışı oynatabilmesi için yeterince düşük bir bit hızında tek bir çıkış katmanı üretmeye karar verdiğini unutmayın.

![PSNR kullanarak RD eğrisi](media/content-aware-encoding/msrv3.png)

**Şekil 3: Düşük kaliteli giriş için PSNR kullanan RD eğrisi (1080p' de)**

![VMAF kullanarak RD eğrisi](media/content-aware-encoding/msrv4.png)

**Şekil 4: Düşük kaliteli giriş için VMAF kullanan RD eğrisi (1080p'de)**

## <a name="how-to-use-the-content-aware-encoding-preset"></a>İçerik duyarlı kodlama ön ayarını kullanma 

Bu ön aseti aşağıdaki gibi kullanan dönüşümler oluşturabilirsiniz. 

> [!TIP]
> Tranform çıktılarını kullanan öğreticiler için [Sonraki adımlar](#next-steps) bölümüne bakın. Çıktı varlığı, MPEG-DASH ve HLS gibi protokollerde (öğreticilerde gösterildiği gibi) Medya Hizmetleri akış uç noktalarından teslim edilebilir.


```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncoding
      }
   }
};
```

> [!NOTE]
> Ön akümeyi `ContentAwareEncoding` kullanarak işleri kodlama, çıktı dakikaları temel alınarak faturalandırılır. 

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Medya Hizmetleri v3 ile video yükleme, kodlama ve akış](stream-files-tutorial-with-api.md)
* [Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışı - REST](stream-files-tutorial-with-rest.md)
* [Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışlayın - CLI](stream-files-cli-quickstart.md)
* [Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışlayın - .NET](stream-files-dotnet-quickstart.md)
* [Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışı - Node.js](stream-files-nodejs-quickstart.md)
