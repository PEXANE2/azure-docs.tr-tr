---
title: Sunucu boyutları
description: Ayrılabilen farklı sunucu boyutlarını açıklar
author: florianborn71
ms.author: flborn
ms.date: 05/28/2020
ms.topic: reference
ms.openlocfilehash: b9479c2ab5b63440a03bd74d2503930108a49091
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88511194"
---
# <a name="server-sizes"></a>Sunucu boyutları

Azure uzaktan Işleme iki sunucu yapılandırmasında kullanılabilir: `Standard` ve `Premium` .

## <a name="polygon-limits"></a>Çokgen limitleri

`Standard`Boyut sunucusuyla uzaktan işleme, 20.000.000 poligonun en büyük sahne boyutuna sahiptir. Boyutla uzaktan Işleme `Premium` , sabit bir en yüksek değeri zorlamaz, ancak içeriğiniz hizmetin işleme yeteneklerini aşarsa performans düşebilir.

' Standart ' sunucu boyutundaki oluşturucunun bu sınırlamaya rastması durumunda, işlemeyi bir dama tahtası arka planına geçirir:

![Tahtası](media/checkerboard.png)

## <a name="specify-the-server-size"></a>Sunucu boyutunu belirtin

İstenen sunucu yapılandırması türü, işleme oturumu başlatma sırasında belirtilmelidir. Çalışan bir oturum içinde değiştirilemez. Aşağıdaki kod örnekleri, sunucu boyutunun belirtilmesi gereken yeri gösterir:

```cs
async void CreateRenderingSession(AzureFrontend frontend)
{
    RenderingSessionCreationParams sessionCreationParams = new RenderingSessionCreationParams();
    sessionCreationParams.Size = RenderingSessionVmSize.Standard; // or  RenderingSessionVmSize.Premium

    AzureSession session = await frontend.CreateNewRenderingSessionAsync(sessionCreationParams).AsTask();
}
```

```cpp
void CreateRenderingSession(ApiHandle<AzureFrontend> frontend)
{
    RenderingSessionCreationParams sessionCreationParams;
    sessionCreationParams.Size = RenderingSessionVmSize::Standard; // or  RenderingSessionVmSize::Premium

    if (auto createSessionAsync = frontend->CreateNewRenderingSessionAsync(sessionCreationParams))
    {
        // ...
    }
}
```

[Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)için, istenen sunucu boyutu dosyanın içinde belirtilmelidir `arrconfig.json` :

```json
{
  "accountSettings": {
    ...
  },
  "renderingSessionSettings": {
    "vmSize": "<standard or premium>",
    ...
  },
```

### <a name="how-the-renderer-evaluates-the-number-of-polygons"></a>Oluşturucunun çokgenler sayısını değerlendirme

Kısıtlama testi için kabul edilen çokgenler sayısı, gerçekten işleyiciye geçirilen çokgenler sayısıdır. Bu geometri genellikle örneklenmiş tüm modellerin toplamıdır, ancak özel durumlar da vardır. Aşağıdaki geometri **dahil değildir**:
* Frustum görünümü dışındaki model örnekleri yüklendi.
* [Hiyerarşik durum geçersiz kılma bileşeni](../overview/features/override-hierarchical-state.md)kullanılarak görünmez durumuna geçiş yapan modeller veya model parçalar.

Buna uygun olarak, `standard` bir çokgen sayısı ile birden çok modeli yükleyen, her bir modelin sınırına yakın olan bir uygulamayı yazmak mümkündür. Uygulama aynı anda yalnızca tek bir model gösteriyorsa, dama tahtası tetiklenmez.

### <a name="how-to-determine-the-number-of-polygons"></a>Çokgenler sayısını belirleme

Yapılandırma boyutunun Bütçe sınırına katkıda bulunan bir modelin veya sahnenin çokgenler sayısını belirlemenin iki yolu vardır `standard` :
* Model dönüştürme tarafında, [dönüştürme çıkışı json dosyasını](../how-tos/conversion/get-information.md)alın ve `numFaces` [ *ınputstatistics* bölümündeki](../how-tos/conversion/get-information.md#the-inputstatistics-section) girişi kontrol edin.
* Uygulamanız dinamik içerikle uğraşıyorsa, işlenen çokgenler sayısı, çalışma zamanı sırasında dinamik olarak sorgulanabilir. Bir [performans değerlendirmesi sorgusu](../overview/features/performance-queries.md#performance-assessment-queries) kullanın ve `polygonsRendered` yapı içindeki üyeyi kontrol edin `FrameStatistics` . `polygonsRendered`Bu alan, `bad` oluşturucunun Çokgen sınırlamasını okuması durumunda olarak ayarlanır. Bu zaman uyumsuz sorgudan sonra, kullanıcı eyleminin alınacağından emin olmak için, dama tahtası arka planı her zaman bir gecikmeyle birlikte gönderilir. Kullanıcı eylemi, örneğin model örneklerini gizleyebilir veya silebilir.

## <a name="pricing"></a>Fiyatlandırma

Her bir yapılandırma türü için fiyatlandırma hakkında ayrıntılı bir döküm için, [Uzaktan işleme fiyatlandırma](https://azure.microsoft.com/pricing/details/remote-rendering) sayfasına bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
* [Model dönüştürme](../how-tos/conversion/model-conversion.md)

