---
title: Sistem güncelleştirmelerini Azure Güvenlik Merkezi'nde uygulayın | Microsoft Dokümanlar
description: Bu belge, Azure Güvenlik Merkezi önerilerini nasıl uygulayacağınızı gösterir **Sistem güncelleştirmelerini uygulayın** ve **sistem güncelleştirmelerini sonra yeniden başlatın.**
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: memildin
ms.openlocfilehash: 3f27753b0775f44cbdf9d4c478a19e423b8e1f19
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604562"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Azure Güvenlik Merkezi'nde sistem güncelleştirmelerini uygulayın
Azure Güvenlik Merkezi eksik işletim sistemi güncelleştirmeleri için Windows ve Linux sanal makinelerini (VM) ve bilgisayarlarını her gün izler. Güvenlik Merkezi, Windows bilgisayarında hangi hizmetin yapılandırıldığına bağlı olarak Windows Update veya Windows Server Update Services'den (WSUS) kullanılabilir güvenlik ve kritik güncelleştirmeler listesini alır. Güvenlik Merkezi Linux sistemlerinde de en son güncelleştirmeleri denetler. VM'nizde veya bilgisayarınızda eksik sistem güncelleştirmesi varsa, Güvenlik Merkezi sistem güncelleştirmelerini uygulamanızı önerir.

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
Uygula sistem güncelleştirmeleri Güvenlik Merkezi'nde bir öneri olarak sunulur. VM veya bilgisayarınızda bir sistem güncelleştirmesi eksikse, bu öneri **Öneriler** altında ve **Bilgi İşlem**altında görüntülenir.  Öneri nin seçilmesi, **Uygula sistemi güncelleştirmeleri** panosunu açar.

Bu örnekte, **Compute**kullanacağız.

1. Güvenlik Merkezi ana menüsü altında **İşlem'i** seçin.

   ![İşlem'i seçin][1]

2. **Bilgi İşlem**altında, Eksik **sistem güncelleştirmelerini**seçin. **Uygula sistemi güncelleştirmeleri** panosu açılır.

   ![Sistem güncellemeleri panosuuygulama][2]

   Pano üst sağlar:

    - Windows ve Linux VM'lerinin ve bilgisayarların toplam sayısı sistem güncelleştirmelerini eksik.
    - VM'lerinizde ve bilgisayarlarınızda eksik olan toplam kritik güncelleştirme sayısı.
    - VM'lerinizde ve bilgisayarlarınızda eksik olan toplam güvenlik güncelleştirmesi sayısı.

   Pano alt kısmında, VM'lerinizve bilgisayarlarınızda eksik güncelleştirmeler ve eksik güncelleştirmenin önem derecesi listelenir.  Liste aşağıdakileri içerir:

    - AD: Eksik güncelleştirmenin adıdır.
    - №. Bilgisayarların & VM'lerin sayısı: Bu güncelleştirmeeksik toplam VM ve bilgisayar sayısı.
    - DURUM: Önerinin geçerli durumu:

      - Açık: Öneri henüz ele alınmadı.
      - Devam Ediyor: Öneri şu anda bu kaynaklara uygulanıyor; sizin bir eylem yapmanıza gerek yok.
      - Çözüldü: Öneri daha önce tamamlandı. (Sorun çözüldüğünde girdi soluklaşır).

    - ÖNEM DERECESİ: Belirli bir önerinin önem derecesini açıklar:

      - Yüksek: Anlamlı bir kaynakta (uygulama, sanal makine veya ağ güvenlik grubu) güvenlik açığı var ve ilgilenilmesi gerekiyor.
      - Orta: Bir işlemi tamamlamak veya bir güvenlik açığını ortadan kaldırmak için kritik olmayan adımlar veya ek adımlar gerekiyor.
      - Düşük: Bir güvenlik açığının ele alınması gerekiyor ancak hemen ilgilenilmesi gerekmiyor. (Varsayılan olarak, düşük öneriler sunulmaz ancak bunları görüntülemek istiyorsanız düşük öneriler filtresini kullanabilirsiniz.)

3. Ayrıntıları görüntülemek için listede eksik bir güncelleştirme seçin.

   ![Eksik güvenlik güncelleştirmesi][3]

4. Üst şeritteki **Arama** simgesini seçin.  Azure Monitörü, güncelleştirmeyi kaçıran bilgisayarlara filtre uygulanarak arama sorgusunu kaydeder.

   ![Azure Monitor günlükleri arama][4]

5. Daha fazla bilgi için listeden bir bilgisayar seçin. Başka bir arama sonucu yalnızca o bilgisayar için filtre uygulanmış bilgilerle açılır.

    ![Azure Monitor günlükleri arama][5]

## <a name="next-steps"></a>Sonraki adımlar
Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -- Önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md) -- Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtla](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını nasıl yönetip yanıtlaştak yapılacağını öğrenin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) -- İş ortağı çözümlarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) -- Azure güvenliği ve uyumluluğu yla ilgili blog gönderilerini bulun.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
