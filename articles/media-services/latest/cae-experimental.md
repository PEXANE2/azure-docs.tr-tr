---
title: İçerik kullanan kodlama için deneysel önayar-Azure | Microsoft Docs
description: Bu makalede, Microsoft Azure Media Services v3 'de içeriğe duyarlı kodlama ele alınmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/05/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9389466b6291542563c068706479bf981c5880da
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896132"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>İçerik algılayan kodlama için deneysel önayar

İçeriği [Uyarlamalı bit hızı akışı](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)ile teslim etmek üzere hazırlamak için videonun birden çok bit hızında (yüksek-düşük) kodlanması gerekir. Hızını düzgün bir şekilde azalmadan emin olmak için bit hızı düşürüldü, bu nedenle videonun çözümlenme olur. Bu, bir çözüm ve bit fiyatları tablosu olarak adlandırılır; buna neden olur. bkz. Media Services [yerleşik kodlama önayarları](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset).

## <a name="overview"></a>Genel Bakış

Tek bir önceden ayarlanmış-tüm videolar yaklaşımının ötesine geçilmesi, Netflix [bloglarını](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2) Aralık 2015 ' de yayımladıktan sonra artar. Bu tarihten sonra, Market 'te içerik duyarlı kodlamaya yönelik birden çok çözüm yayımlanmıştır; genel bakış için [Bu makaleye](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx) bakın. Düşünce, tek tek videonun karmaşıklığına yönelik kodlama merdiveni ' i özelleştirmek veya ayarlamak için içeriğe göz önünde bulundurulmalıdır. Her çözünürlükte, kalitedeki artışın Perceptive olmadığı bir bit hızı vardır: kodlayıcı bu en iyi bit hızı değerinde çalışır. Bir sonraki iyileştirme düzeyi, içeriğe göre çözümlerin seçmesidir. Örneğin, bir PowerPoint sunusunun videosu, 720p 'in altına gitmesinin avantajına sahip değildir. Diğer bir deyişle, Kodlayıcıdaki her bir görüntü için ayarları iyileştirmek üzere kodlayıcı eklenebilir. Netflix, 2018 ' de [böyle bir yaklaşım](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830) ile açıklanmıştır.

Erken 2017 ' de, Microsoft, kaynak videoların kalite ve çözünürlüğündeki değişkenlik sorununu çözmek için [uyarlamalı akış](autogen-bitrate-ladder.md) ön ayarını yayımlamıştır. Müşterilerimiz, bazı Kullanıcı adına, bazı 1080p, 720p ve daha az sayıda SD ve daha düşük çözünürlükte içerik karması içeriyordu. Ayrıca, tüm kaynak içerikleri film veya TV Studios 'den yüksek kaliteli mezzanines. Uyarlamalı akış önceden ayarı, bit hızı el merdivenine, giriş Mezzanine 'nin çözünürlüğü veya Ortalama bit hızını aşmamasını sağlayarak bu sorunları giderir.

Deneysel içeriğe duyarlı kodlama önceden ayarı, bu mekanizmayı genişleterek, kodlayıcının belirli bir çözüm için en iyi bit hızı değerini, ancak kapsamlı hesaplama analizine gerek kalmadan araymasına olanak tanır. Net sonuç, bu yeni önayar, uyarlamalı akış önayarı 'ndan daha düşük bit hızına sahip bir çıkış üretir, ancak daha yüksek bir kalitede olur. [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) ve [vmaf](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion)gibi kalite ölçümlerini kullanarak karşılaştırmayı gösteren aşağıdaki örnek grafiklere bakın. Kaynak, kodlayıcılarını stres amacıyla tasarlanan filmlerden ve TV gösterlerinden gelen yüksek karmaşıklık görüntüleriyle kısa küçük resimleri birleştirerek oluşturulmuştur. Tanım olarak, bu ön ayar içerikten içeriğe farklılık gösteren sonuçlar üretir. Ayrıca, bazı içerikler için bit hızı veya kalite açısından önemli bir azalma olmadığı anlamına gelir.

![PSNR kullanan hız deformasyonu (RD) eğrisi](media/cae-experimental/msrv1.png)

**Şekil 1: yüksek karmaşıklık kaynağı için PSNR ölçümünü kullanarak oran deformasyonu (RD) eğrisi**

![VMAF kullanan hız deformasyonu (RD) eğrisi](media/cae-experimental/msrv2.png)

**Şekil 2: yüksek karmaşıklık kaynağı için VMAF ölçümünü kullanarak oran deformasyonu (RD) eğrisi**

Ön ayar şu anda yüksek karmaşıklık, yüksek kaliteli kaynak videoları (Filmler, TV programları) için ayarlanmıştır. İş, düşük karmaşıklık içeriğine (örneğin, PowerPoint sunuları) ve poorer kaliteli videoların uyarlanmasını sağlayacak şekilde devam etmektedir. Bu önayar Ayrıca, uyarlamalı akış ön ayarıyla aynı çözüm kümesini kullanır. Microsoft, içeriğe göre en düşük çözüm kümesini seçmek için yöntemler üzerinde çalışmaktadır. Aşağıdaki gibi, kodlayıcının girişin düşük kaliteli olduğunu belirleyebildiği (düşük bit hızı nedeniyle birçok sıkıştırma yapıtı), başka bir kaynak içeriği kategorisinin sonuçları vardır. Deneysel ön ayarda, kodlayıcının yalnızca bir çıkış katmanı üretmesine karar verdiğine ve çoğu istemcinin, bu akışı etkilemeden oynatacağından emin olun.

![PSNR kullanan RD eğrisi](media/cae-experimental/msrv3.png)

**Şekil 3: düşük kaliteli giriş için PSNR kullanan RD eğrisi (1080p 'de)**

![VMAF kullanan RD eğrisi](media/cae-experimental/msrv4.png)

**Şekil 4: düşük kaliteli giriş için VMAF kullanan RD eğrisi (1080p 'de)**

## <a name="use-the-experimental-preset"></a>Deneysel önayar kullanma

Bu önayarı kullanan dönüşümler aşağıdaki gibi oluşturulabilir. [Bu](stream-files-tutorial-with-api.md)gibi bir öğretici kullanılıyorsa, kodu aşağıdaki şekilde güncelleştirebilirsiniz:

```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new experimental preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncodingExperimental
      }
   }
};
```

> [!NOTE]
> Temel algoritmaların hala gelişmekte olduğunu bildirmek için burada "deneysel" ön eki kullanılır. Zamana göre zaman içindeki değişiklikler, güçlü bir algoritmaya uyum sağlamak ve çok çeşitli giriş koşullarına uyum sağlamak amacıyla bit hızı merdiveni oluşturmak için kullanılan mantığa göre yapılır. Bu önceden belirlenmiş ayarı kullanan kodlama işleri, çıkış dakikaları temel alınarak faturalandırılır ve çıkış varlığı, DASH ve HLS gibi protokollerde akış uç noktalarımızdan teslim edilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Videolarınızı en iyi duruma getirmeye yönelik bu yeni seçenek hakkında bilgi edindiğinize göre, sizi denemeye davet ediyoruz. Bu makalenin sonundaki bağlantıları kullanarak bize geri bildirim gönderebilir veya <amsved@microsoft.com>doğrudan daha fazla iletişim sağlayabilirsiniz.
