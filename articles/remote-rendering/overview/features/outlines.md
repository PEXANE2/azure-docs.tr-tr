---
title: Ana hattı işleme
description: Seçim ana hattı işlemenin nasıl yapılacağını açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 4f3889a0ba121cb9a3167c1f6ac95f0bed280539
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83759022"
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
| `PulseIntensity` | float   | Ana hat Pulse efektinin yoğunluğu. Tam pulun için 0,0 arasında bir pulun ve 1,0 olmaması gerekir. Şiddette, ana hattın en düşük opaklığını örtülü olarak belirler `MinOpacity = 1.0 - PulseIntensity` . |

![](./media/outlines.png) `color` Parametrenin sarıdan (solda) macenta (orta) ve `pulseIntensity` 0 ile 0,8 (sağ) arasında değiştirilmesini açıklar.

## <a name="example"></a>Örnek

Aşağıdaki kod, API aracılığıyla ana hat parametrelerini ayarlamak için bir örnek gösterir:

```cs
void SetOutlineParameters(AzureSession session)
{
    OutlineSettings outlineSettings = session.Actions.OutlineSettings;
    outlineSettings.Color = new Color4Ub(255, 255, 0, 255);
    outlineSettings.PulseRateHz = 2.0f;
    outlineSettings.PulseIntensity = 0.5f;
}
```

```cpp
void SetOutlineParameters(ApiHandle<AzureSession> session)
{
    ApiHandle<OutlineSettings> outlineSettings = *session->Actions()->OutlineSettings();
    Color4Ub outlineColor;
    outlineColor.channels = { 255, 255, 0, 255 };
    outlineSettings->Color(outlineColor);
    outlineSettings->PulseRateHz(2.0f);
    outlineSettings->PulseIntensity(0.5f);
}
```

## <a name="performance"></a>Performans

Ana hat işleme, işleme performansı üzerinde önemli bir etkiye sahip olabilir. Bu etki, belirli bir çerçeve için seçili ve seçili olmayan nesneler arasındaki ekran alanı açısından uzamsal ilişkiye göre farklılık gösterir.

## <a name="next-steps"></a>Sonraki adımlar

* [Hiyerarşik durum geçersiz kılma bileşeni](../../overview/features/override-hierarchical-state.md)
