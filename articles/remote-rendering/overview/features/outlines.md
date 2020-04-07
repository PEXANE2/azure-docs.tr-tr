---
title: Anahat oluşturma
description: Seçim anahattı oluşturmanın nasıl yapılacağını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 8b52dbe8cd12e51c42677ce37acbd57ad551ec50
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680836"
---
# <a name="outline-rendering"></a>Anahat oluşturma

Seçili [nesneler, Hiyerarşik durum geçersiz kılma bileşeni](../../overview/features/override-hierarchical-state.md)üzerinden anahat oluşturma ekleyerek görsel olarak vurgulanabilir. Bu bölümde, anahat oluşturma için genel parametrelerin istemci API'sı aracılığıyla nasıl değiştirildeğiştiği açıklanmaktadır.

Anahat özellikleri genel bir ayardır. Anahat oluşturmayı kullanan tüm nesneler aynı ayarı kullanır - nesne başına anahat rengi kullanmak mümkün değildir.

## <a name="parameters-for-outlinesettings"></a>Parametreler için`OutlineSettings`

Sınıf, `OutlineSettings` genel anahat özellikleriyle ilgili ayarları tutar. Aşağıdaki üyeleri ortaya çıkarır:

| Parametre      | Tür    | Açıklama                                             |
|----------------|---------|---------------------------------------------------------|
| `Color`          | Color4Ub | Anahat çizmek için kullanılan renk. Alfa kısmı yoksayılır.         |
| `PulseRateHz`    | float   | Anahattın saniyede salınım hızı|
| `PulseIntensity` | float   | Anahat darbe etkisinin yoğunluğu. Hiçbir zonklama için 0,0 ve tam nabız için 1,0 arasında olmalıdır. Yoğunluk örtülü olarak `MinOpacity = 1.0 - PulseIntensity`anahat minimum opaklık ayarlar. |

![Anahatlar](./media/outlines.png) Parametreyi `color` sarıdan (solda) macenta (ortada) `pulseIntensity` ve 0'dan 0,8'e (sağda) değiştirmenin etkisini tanımlar.

## <a name="example"></a>Örnek

Aşağıdaki kod, API üzerinden anahat parametrelerini ayarlamak için bir örnek gösterir:

``` cs
void SetOutlineParameters(AzureSession session)
{
    OutlineSettings outlineSettings = session.Actions.OutlineSettings;
    outlineSettings.Color = new Color4Ub(255, 255, 0, 255);
    outlineSettings.PulseRateHz = 2.0f;
    outlineSettings.PulseIntensity = 0.5f;
}
```

## <a name="performance"></a>Performans

Anahat oluşturma, performans oluşturma üzerinde önemli bir etkiye sahip olabilir. Bu etki, belirli bir çerçeve için seçili ve seçilmemiş nesneler arasındaki ekran-uzay uzamsal ilişkisine göre değişir.

## <a name="next-steps"></a>Sonraki adımlar

* [Hiyerarşik durum geçersiz kılma bileşeni](../../overview/features/override-hierarchical-state.md)
