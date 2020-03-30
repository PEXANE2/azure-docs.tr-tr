---
title: Azure Güvenlik Merkezi'nde güvenlik önerileri
description: Bu belge, Azure Güvenlik Merkezi'ndeki önerilerin Azure kaynaklarınızı korumanıza ve güvenlik ilkelerine uymanıza nasıl yardımcı olduğunu size iletiyor.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/29/2019
ms.author: memildin
ms.openlocfilehash: 408b0f020be72b8e6b10dd6c97298afda1b91360
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79245310"
---
# <a name="security-recommendations-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde güvenlik önerileri 
Bu konu, Azure kaynaklarınızı korumanıza yardımcı olmak için Azure Güvenlik Merkezi'ndeki önerileri nasıl görüntülayacağınızı ve anlayacağınızı açıklar.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belge adım adım bir kılavuz değildir.
>

## <a name="what-are-security-recommendations"></a>Güvenlik önerileri nelerdir?

Öneriler, kaynaklarınızı güvence altına almak için gerçekleştirdiğiniz eylemlerdir.

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için Azure kaynaklarınızın güvenlik durumunu düzenli aralıklarla analiz ediyor. Daha sonra bunları kaldırmak için nasıl öneriler sağlar.

Her öneri size şunları sağlar:

- Tavsiye edilen şeyin kısa bir açıklaması.
- Tavsiyeyi uygulamak için yerine getirilen düzeltme adımları. <!-- In some cases, Quick Fix remediation is available. -->
- Hangi kaynakların onlar hakkında önerilen eylemi gerçekleştirmenize ihtiyacı vardır.
- Bu öneriyi uygularsanız Güvenli Puan ınızın artacağı miktar olan **Güvenli Puan etkisi.**

## <a name="monitor-recommendations"></a>Önerileri izleyin<a name="monitor-recommendations"></a>

Güvenlik Merkezi, olası güvenlik açıklarını belirlemek için kaynaklarınızın güvenlik durumunu analiz eder. **Genel Bakış** altındaki **Öneriler** döşemesi, Güvenlik Merkezi tarafından tanımlanan toplam öneri sayısını gösterir.

![Güvenlik merkezine genel bakış](./media/security-center-recommendations/asc-overview.png)

1. **Genel Bakış**altında **Öneriler döşemesini** seçin. **Öneriler** listesi açılır.

      ![Önerileri görüntüleme](./media/security-center-recommendations/view-recommendations.png)

    Önerilere filtre uygulayabilirsiniz. Önerilere filtre uygulayabilmek için **Öneriler** bıçağındaki **Filtre'yi** seçin. **Filtre** bıçağı açılır ve görmek istediğiniz önem ve durum değerlerini seçersiniz.

   * **ÖNERİLER**: Tavsiye.
   * **GÜVENLİ PUAN Etkİsİ**: Güvenlik Merkezi tarafından güvenlik önerilerinizi kullanarak ve her önerinin ne kadar önemli olduğunu belirlemek için gelişmiş algoritmalar uygulayan bir puan. Daha fazla bilgi için [Güvenli Puan hesaplaması'na](security-center-secure-score.md#secure-score-calculation)bakın.
   * **KAYNAK**: Bu önerinin uygulandığı kaynakları listeler.
   * **STATUS BARS**: Bu özel önerinin önem derecesini açıklar:
       * **Yüksek (Kırmızı)**: Anlamlı bir kaynakla (uygulama, VM veya ağ güvenlik grubu gibi) bir güvenlik açığı vardır ve dikkat gerektirir.
       * **Orta (Turuncu)**: Bir güvenlik açığı vardır ve onu ortadan kaldırmak veya bir işlemi tamamlamak için kritik olmayan veya ek adımlar gereklidir.
       * **Düşük (Mavi)**: Ele alınması gereken ancak hemen dikkat gerektirmeyen bir güvenlik açığı vardır. (Varsayılan olarak, düşük öneriler sunulmaz, ancak görmek istiyorsanız düşük önerilere filtre uygulayabilirsiniz.) 
       * **Sağlıklı (Yeşil)**:
       * **Mevcut Değil (Gri)**:

1. Her önerinin ayrıntılarını görüntülemek için öneriye tıklayın.

    ![Öneri ayrıntıları](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Azure kaynakları için [klasik ve Kaynak Yöneticisi dağıtım modellerine](../azure-classic-rm.md) bakın.
 
## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Güvenlik Merkezi'nde güvenlik önerileri tanıtıldı. Önerileri nasıl düzelteceklerini öğrenmek için:

* [İyileştirici öneriler](security-center-remediate-recommendations.md) — Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini nasıl yapılandırıştırılacak öğrenin.
