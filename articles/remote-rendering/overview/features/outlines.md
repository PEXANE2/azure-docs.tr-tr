---
title: Ana hattı işleme
description: Seçim ana hattı işlemenin nasıl yapılacağını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 8b52dbe8cd12e51c42677ce37acbd57ad551ec50
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80680836"
---
# <a name="outline-rendering"></a>Ana hattı işleme

Seçilen nesneler, [hiyerarşik durum geçersiz kılma bileşeni](../../overview/features/override-hierarchical-state.md)aracılığıyla ana hat işleme eklenerek görsel olarak vurgulanabilir. Bu bölümde, ana hat işleme için genel parametrelerin istemci API 'SI aracılığıyla nasıl değiştirildiği açıklanmaktadır.

Ana hat özellikleri genel bir ayardır. Ana hat işleme kullanan tüm nesneler aynı ayarı kullanır; nesne başına ana hat rengi kullanmak mümkün değildir.

## <a name="parameters-for-outlinesettings"></a>İçin parametreler`OutlineSettings`

Sınıf `OutlineSettings` , genel anahat özellikleriyle ilgili ayarları barındırır. Aşağıdaki üyeleri kullanıma sunar:

| Parametre      | Tür    | Açıklama                                             |
|----------------|---------|---------------------------------------------------------|
| `Color`          | Color4Ub | Anahattı çizmek için kullanılan renk. Alfa bölümü yok sayılır.         |
| `PulseRateHz`    | float   | Ana hattın saniyedeki kaç saniyede ele alındığı oran|
| `PulseIntensity` | float   | Ana hat Pulse efektinin yoğunluğu. Tam pulun için 0,0 arasında bir pulun ve 1,0 olmaması gerekir. Şiddette, ana hattın en düşük opaklığını örtülü olarak `MinOpacity = 1.0 - PulseIntensity`belirler. |

![Parametrenin sarıdan (solda) macenta (orta) ve](./media/outlines.png) `pulseIntensity` 0 ile 0,8 (sağ) arasında değiştirilmesini açıklar. `color`

## <a name="example"></a>Örnek

Aşağıdaki kod, API aracılığıyla ana hat parametrelerini ayarlamak için bir örnek gösterir:

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

Ana hat işleme, işleme performansı üzerinde önemli bir etkiye sahip olabilir. Bu etki, belirli bir çerçeve için seçili ve seçili olmayan nesneler arasındaki ekran alanı açısından uzamsal ilişkiye göre farklılık gösterir.

## <a name="next-steps"></a>Sonraki adımlar

* [Hiyerarşik durum geçersiz kılma bileşeni](../../overview/features/override-hierarchical-state.md)
