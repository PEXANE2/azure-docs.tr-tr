---
title: Z-mücadele azaltma
description: Z-fighting eserlerini hafifletmek için teknikleri açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/06/2020
ms.topic: article
ms.openlocfilehash: bc06deafe3f589fce9a9178fefdb22388254929d
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680459"
---
# <a name="z-fighting-mitigation"></a>Z-mücadele azaltma

İki yüzey çakıştığında, hangisinin diğerinin üzerine işlenmesi gerektiği açık değildir. Sonuç piksel başına bile değişir ve bu da görünüme bağımlı yapılarla sonuçlanır. Sonuç olarak, kamera veya kafes hareket ettiğinde, bu desenler belirgin bir şekilde titreşir. Bu objeye *z-dövüş*denir. AR ve VR uygulamaları için sorun yoğunlaşır, çünkü kafaya monte edilmiş cihazlar doğal olarak her zaman hareket eder. Görüntüleyici rahatsızlığını önlemek için Z-fighting azaltma işlevi Azure Uzaktan İşleme'de kullanılabilir.

## <a name="z-fighting-mitigation-modes"></a>Z-mücadele azaltma modları

|Durum                        | Sonuç                               |
|---------------------------------|:-------------------------------------|
|Düzenli z-mücadele               |![Z-mücadele](./media/zfighting-0.png)|
|Z-mücadele azaltma etkin    |![Z-mücadele](./media/zfighting-1.png)|
|Dama tahtası vurgulama etkin|![Z-mücadele](./media/zfighting-2.png)|

Aşağıdaki kod z-mücadele azaltma sağlar:

``` cs
void EnableZFightingMitigation(AzureSession session, bool highlight)
{
    ZFightingMitigationSettings settings = session.Actions.ZFightingMitigationSettings;

    // enabling z-fighting mitigation
    settings.Enabled = true;

    // enabling checkerboard highlighting of z-fighting potential
    settings.Highlighting = highlight;
}
```

> [!NOTE]
> Z-mücadele azaltma tüm render meshes etkileyen küresel bir ayardır.

## <a name="reasons-for-z-fighting"></a>Z-mücadele nedenleri

Z-mücadele başlıca iki nedenden dolayı olur:

1. yüzeyler kameradan çok uzakta olduğunda, derinlik değerlerinin hassasiyeti düşer ve değerler ayırt edilemez hale gelir
1. bir kafesteki yüzeyler fiziksel olarak çakıştığında

İlk sorun her zaman olabilir ve ortadan kaldırmak zordur. Uygulamanızda bu durumda, *yakın düzlem* mesafesinin *uzak düzlem* mesafesine oranının pratik olduğu kadar düşük olduğundan emin olun. Örneğin, 0,01 ve uzak düzlemde 1000 mesafedeki yakın bir düzlem, yakın düzlemin 0,1 ve uzak düzlemin 20 mesafede olmasından çok daha erken bir sorun yaratacaktır.

İkinci sorun kötü yazılmış içerik için bir göstergedir. Gerçek dünyada, iki nesne aynı anda aynı yerde olamaz. Uygulamaya bağlı olarak, kullanıcılar çakışan yüzeylerin var olup olmadığını ve nerede olduklarını bilmek isteyebilir. Örneğin, gerçek bir dünya inşaatının temelini oluşturan bir binanın CAD sahnesi, fiziksel olarak imkansız yüzey kavşakları içermemelidir. Görsel denetime izin vermek için, animasyonlu bir dama tahtası deseni olarak potansiyel z-mücadele görüntüler vurgulama modu kullanılabilir.

## <a name="limitations"></a>Sınırlamalar

Sağlanan z-mücadele azaltma en iyi çabadır. Tüm z-mücadele kaldırır garantisi yoktur. Ayrıca otomatik olarak diğerine bir yüzey tercih edecektir. Böylece birbirine çok yakın yüzeylervarsa, "yanlış" yüzey üst te sona erer. Sık karşılaşılan bir sorun, metin ve diğer çıkartmaların bir yüzeye uygulanmasıdır. Z-mücadele azaltma ile bu ayrıntıları kolayca sadece kaybolabilir etkin.

## <a name="performance-considerations"></a>Performansla ilgili önemli noktalar

* Z-fighting azaltmayı etkinleştirmek, çok az performans yüküne neden olmaz.
* Ayrıca z-fighting bindirme etkinleştirme önemsiz olmayan bir performans yükü tabi, ancak sahne bağlı olarak değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Oluşturma modları](../../concepts/rendering-modes.md)
* [Geç aşama yeniden projeksiyonu](late-stage-reprojection.md)
