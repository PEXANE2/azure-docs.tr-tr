---
title: VM boyutları
description: Ayrılabilen ayrı VM boyutlarını açıklar
author: florianborn71
ms.author: flborn
ms.date: 05/28/2020
ms.topic: reference
ms.openlocfilehash: aab914caa2647146639aa366f558c80bebcfde54
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84810211"
---
# <a name="vm-sizes"></a>VM boyutları

İşleme hizmeti, Azure 'da ve olarak adlandırılan iki farklı makine türü üzerinde çalışabilir `Standard` `Premium` .

## <a name="polygon-limits"></a>Çokgen limitleri

VM boyutu için **20.000.000 poligonun** sabit sınırlaması vardır `Standard` . Boyut için böyle bir sınırlama yoktur `Premium` .

Standart bir VM 'deki işleyici bu kısıtlamayı geçtiğinde, işlemeyi bir dama tahtası arka planına geçirir:

![Tahtası](media/checkerboard.png)

## <a name="allocate-the-vm"></a>VM 'yi ayır

İstenen VM türü, işleme oturumu başlatma sırasında belirtilmelidir. Çalışan bir oturum içinde değiştirilemez. Aşağıdaki kod örnekleri, VM boyutunun belirtilmesi gereken yeri gösterir:

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

[Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)için, sanal makine boyutu dosyanın içinde belirtilmelidir `arrconfig.json` :

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

Bir modelin veya sahnenin boyut sanal makinesinin Bütçe sınırına katkıda bulunan çokgenler sayısını belirlemenin iki yolu vardır `standard` :
* Model dönüştürme tarafında, [dönüştürme çıkışı json dosyasını](../how-tos/conversion/get-information.md)alın ve `numFaces` [ *ınputstatistics* bölümündeki](../how-tos/conversion/get-information.md#the-inputstatistics-section) girişi kontrol edin.
* Uygulamanız dinamik içerikle uğraşıyorsa, işlenen çokgenler sayısı, çalışma zamanı sırasında dinamik olarak sorgulanabilir. Bir [performans değerlendirmesi sorgusu](../overview/features/performance-queries.md#performance-assessment-queries) kullanın ve `polygonsRendered` yapı içindeki üyeyi kontrol edin `FrameStatistics` . Bu zaman uyumsuz sorgudan sonra, kullanıcı eyleminin alınacağından emin olmak için, dama tahtası arka planı her zaman bir gecikmeyle birlikte gönderilir. Kullanıcı eylemi, örneğin model örneklerini gizleyebilir veya silebilir.

## <a name="pricing"></a>Fiyatlandırma

Her bir sanal makine türü için fiyatlandırma hakkında ayrıntılı bir döküm için, [Uzaktan işleme fiyatlandırma](https://azure.microsoft.com/pricing/details/remote-rendering) sayfasına bakın.

## <a name="next-steps"></a>Sonraki adımlar
* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
* [Model dönüştürme](../how-tos/conversion/model-conversion.md)

