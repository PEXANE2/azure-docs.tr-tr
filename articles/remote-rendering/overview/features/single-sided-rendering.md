---
title: Tek taraflı görüntüleme
description: Tek taraflı görüntüleme ayarlarını ve kullanım servis taleplerini açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/06/2020
ms.topic: article
ms.openlocfilehash: 34ee5d4978c6476da407cde33598a5713177078e
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80682019"
---
# <a name="single-sided-rendering"></a>Tek taraflı görüntüleme

Çoğu renderer performansı artırmak için [arka yüz toplama](https://en.wikipedia.org/wiki/Back-face_culling) kullanır. Ancak, meshes kesilmiş [düzlemler](cut-planes.md)ile açık kesilir, kullanıcılar genellikle üçgenlerin arka tarafında bakacağız. Eğer bu üçgenler itlaf edilirse, sonuç ikna edici görünmüyor.

Bu sorunu güvenilir bir şekilde önlemenin yolu, üçgenleri *çift taraflı*hale getirmektir. Arka yüz toplama kullanmanın performans etkileri olduğundan, varsayılan olarak Azure Uzaktan İşleme yalnızca kesik bir düzlemle kesişen meshe'ler için çift taraflı işlemeye geçer.

*Tek taraflı görüntüleme* ayarı, bu davranışı özelleştirmenize olanak tanır.

> [!CAUTION]
> Tek taraflı görüntüleme ayarı deneysel bir özelliktir. Gelecekte tekrar kaldırılabilir. Uygulamanızdaki kritik bir sorunu gerçekten çözmediği sürece lütfen varsayılan ayarı değiştirmeyin.

## <a name="prerequisites"></a>Ön koşullar

Tek taraflı işleme ayarı yalnızca `opaqueMaterialDefaultSidedness` ' olarak ayarlanmış seçenekle `SingleSided` [dönüştürülmüş](../../how-tos/conversion/configure-model-conversion.md) olan meshe'ler için bir etkiye sahiptir. Varsayılan olarak bu seçenek `DoubleSided`.

## <a name="single-sided-rendering-setting"></a>Tek taraflı görüntüleme ayarı

Üç farklı mod vardır:

**Normal:** Bu modda, meshes her zaman dönüştürülür olarak işlenir. Bu, `opaqueMaterialDefaultSidedness` `SingleSided` setle dönüştürülmüş meshe'lerin, kesilmiş bir düzlemle kesişseler bile her zaman arka yüz toplama özelliği etkin leştirilmiş olarak işlenecekleri anlamına gelir.

**DynamicDoubleSiding:** Bu modda, kesilmiş bir düzlem bir kafesle kesiştiğinde, otomatik olarak çift taraflı işlemeye geçer. Bu mod varsayılan moddur.

**AlwaysDoubleSided:** Tüm tek taraflı geometriyi her zaman çift taraflı hale getirmek için zorlar. Bu mod çoğunlukla açıkta dır, böylece tek taraflı ve çift taraflı görüntüleme arasındaki performans etkisini kolayca karşılaştırabilirsiniz.

Tek taraflı görüntüleme ayarlarını değiştirme aşağıdaki gibi yapılabilir:

``` cs
void ChangeSingleSidedRendering(AzureSession session)
{
    SingleSidedSettings settings = session.Actions.SingleSidedSettings;

    // Single-sided geometry is rendered as is
    settings.Mode = SingleSidedMode.Normal;

    // Single-sided geometry is always rendered double-sided
    settings.Mode = SingleSidedMode.AlwaysDoubleSided;
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Uçakları kes](cut-planes.md)
* [Model dönüşümyapılandırma](../../how-tos/conversion/configure-model-conversion.md)
