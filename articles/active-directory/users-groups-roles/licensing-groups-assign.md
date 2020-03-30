---
title: Bir gruba lisans atama - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Active Directory grup lisanslama yoluyla kullanıcılara lisans atama
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: users-groups-roles
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 130ce05e332f4705feb4acd54cbeb25d25a82532
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253071"
---
# <a name="assign-licenses-to-users-by-group-membership-in-azure-active-directory"></a>Azure Active Directory'de grup üyeliğine göre kullanıcılara lisans atama

Bu makale, bir grup kullanıcıya ürün lisansları atamanız ve Azure Active Directory 'de (Azure AD) doğru lisanslı olduklarını doğrulamanız için size yol göstermektedir.

Bu örnekte, kiracı **İk Bölümü**adlı bir güvenlik grubu içerir. Bu grup, insan kaynakları departmanının tüm üyelerini (yaklaşık 1.000 kullanıcı) içerir. Tüm departmana Office 365 Enterprise E3 lisansları atamak istiyorsunuz. Ürüne dahil edilen Yammer Enterprise hizmeti, departman kullanmaya hazır olana kadar geçici olarak devre dışı bırakılmalı. Ayrıca, aynı kullanıcı grubuna Enterprise Mobility + Security lisansları dağıtmak istiyorsunuz.

> [!NOTE]
> Bazı Microsoft hizmetleri tüm konumlarda kullanılamaz. Lisansın bir kullanıcıya atanmadan önce, yöneticinin kullanıcıüzerindeki Kullanım konumu özelliğini belirtilmesi gerekir.
>
> Grup lisans ataması için, kullanım yeri belirtilmeyen kullanıcılar dizinin konumunu devralır. Birden çok konumda kullanıcılarınız varsa, kullanım konumunu her zaman Azure AD'deki kullanıcı oluşturma akışınızın bir parçası olarak ayarlamanızı öneririz (örn. AAD Connect yapılandırması yoluyla) bu da lisans atamasının sonucunun her zaman doğru olmasını ve kullanıcıların almamasını sağlar izin verilmeyen yerlerde ki hizmetler.

## <a name="step-1-assign-the-required-licenses"></a>Adım 1: Gerekli lisansları atama

1. Azure AD [**yönetici merkezinde**](https://aad.portal.azure.com) lisans yöneticisi hesabıyla oturum açın. Lisansları yönetmek için hesabın bir lisans yöneticisi, kullanıcı yöneticisi veya genel yönetici olması gerekir.

1. Kiracıdaki tüm lisanslanabilir ürünleri görebileceğiniz ve yönetebileceğiniz bir sayfa açmak için **Lisanslar'ı** seçin.

1. **Tüm ürünler**altında, ürün adlarını seçerek hem Office 365 Enterprise E5 hem de Enterprise Mobility + Security E3'ü seçin. Atamayı başlatmak için sayfanın üst kısmında **Ata'yı** seçin.

   ![Lisans atamak için ürünleri seçme](./media/licensing-groups-assign/licenses-all-products-assign.png)
  
1. Lisans **Atla** sayfasında, kullanıcıların ve grupların listesini açmak için **Kullanıcılar ve gruplar** seçin.

1. Bir kullanıcı veya grup seçin ve seçiminizi onaylamak için sayfanın altındaki **Seç** düğmesini kullanın.

1. Lisans **Atama** sayfasında, daha önce seçtiğimiz iki üründe yer alan tüm hizmet planlarını görüntüleyen **Atama seçeneklerini**tıklatın. **Yammer Enterprise'ı** bulun ve bu hizmeti ürün lisansından devre dışı bırakabilmek için **kapatın.** **Lisans seçeneklerinin**altındaki **Tamam'ı** tıklatarak onaylayın.

   ![lisanslar için hizmet planlarını seçin](./media/licensing-groups-assign/assignment-options.png)
  
1. Atamayı tamamlamak için, **Lisans Ata** sayfasında, sayfanın altındaki **Atama'yı** tıklatın.

1. Sağ üst köşede, işlemin durumunu ve sonucunu gösteren bir bildirim görüntülenir. Gruba atama tamamlanamadıysa (örneğin, gruptaki önceden varolan lisanslar nedeniyle), hatanın ayrıntılarını görüntülemek için bildirimi tıklatın.

Bir gruba lisans atadığında, Azure AD bu grubun tüm varolan üyelerini işler. Bu işlem, grubun boyutuna göre değişen, biraz zaman alabilir. Bir sonraki adımda, işlemin tamamlanıp tamamlanamayacağına nasıl doğruvereceği ve sorunları çözmek için daha fazla dikkat gerekip gerekmedi

## <a name="step-2-verify-that-the-initial-assignment-has-finished"></a>Adım 2: İlk atamanın tamamladığını doğrulama

1. Azure **Etkin Dizin** > **Gruplarına**gidin. Lisansların atandığı grubu seçin.

1. Grup sayfasında **Lisanslar'ı**seçin. Bu, lisansların kullanıcılara tam olarak atanmış olup olmadığını ve bakmanız gereken hatalar olup olmadığını hızlı bir şekilde onaylamanızı sağlar. Aşağıdaki bilgiler kullanılabilir:

   - Şu anda gruba atanan hizmet lisansları. Etkinleştirilen belirli hizmetleri göstermek ve değişiklik yapmak için bir giriş seçin.

   - Değişiklikler işleniyorsa veya tüm kullanıcı üyeleri için işlem tamamlanmışsa kullanılabilir olan en son lisans değişikliklerinin durum güncelleştirmeleri.

   - Hata durumunda olan kullanıcı lisansı atamaları hakkında bilgi.

   ![lisans hataları ve lisans durumu](./media/licensing-groups-assign/assignment-errors.png)

1. **Azure Etkin Dizin** > **Kullanıcıları ve grup** > *adı* > **Denetim günlükleri**altında lisans işleme hakkında daha ayrıntılı bilgi bakın. Aşağıdaki etkinlikleri kontrol edin:

   - Etkinlik: `Start applying group based license to users`. Sistem gruptaki lisans atama değişikliğini aldığında ve tüm kullanıcı üyelerine uygulamaya başladığında bu günlüğe kaydedilir. Yapılan değişiklik hakkında bilgi içerir.

   - Etkinlik: `Finish applying group based license to users`. Sistem gruptaki tüm kullanıcıları işlemeyi bitirdiğinde bu günlüğe kaydedilir. Kaç kullanıcının başarıyla işlendiğinin ve kaç kullanıcıya grup lisansı atanamadığının bir özetini içerir.

   Denetim günlüklerinin grup tabanlı lisanslama tarafından yapılan değişiklikleri analiz etmek için nasıl kullanAbileceği hakkında daha fazla bilgi edinmek için [bu bölümü okuyun.](licensing-group-advanced.md#use-audit-logs-to-monitor-group-based-licensing-activity)

## <a name="step-3-check-for-license-problems-and-resolve-them"></a>Adım 3: Lisans sorunlarını denetleyin ve bunları çözün

1. Azure **Etkin Dizin** > **Grupları'na**gidin ve lisansların atandığı grubu bulun.
1. Grup sayfasında **Lisanslar'ı**seçin. Sayfanın üstündeki bildirim, lisansların atanamayacağı 10 kullanıcı olduğunu gösterir. Bu grup için lisans hatası durumundaki tüm kullanıcıların listesini görmek için açın.
1. **Başarısız atamalar** sütunu, her iki ürün lisansının da kullanıcılara atanamayacağını söyler. Hata sütununun **en üst nedeni,** hatanın nedenini içerir. Bu durumda, **çakışan hizmet planları.**

   ![atanamayan lisanslar](./media/licensing-groups-assign/failed-assignments.png)

1. Kullanıcının **Lisanslar** sayfasını açmak için bir kullanıcı seçin. Bu sayfa, şu anda kullanıcıya atanan tüm lisansları gösterir. Bu örnekte, kullanıcı **Kiosk kullanıcıları** grubundan devralınan Office 365 Enterprise E1 lisansına sahiptir. Bu, sistemin **İk Departmanı** grubundan uygulamaya çalıştığı E3 lisansıyla çelişiyor. Sonuç olarak, bu gruptaki lisansların hiçbiri kullanıcıya atanmadı.

   ![Bir kullanıcı için tüm lisans çakışmalarını görüntüleme](./media/licensing-groups-assign/user-licence-conflicting-service-plans.png)

1. Bu çakışmayı çözmek için kullanıcıyı **Kiosk kullanıcıları** grubundan kaldırın. Azure AD değişikliği işledikten **sonra, İk Departmanı** lisansları doğru şekilde atanır.

## <a name="next-steps"></a>Sonraki adımlar

Grupları kullanarak lisans ataması için özellik kümesi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Active Directory'de grup tabanlı lisanslama nedir?](/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)
- [Azure Active Directory'de grubun lisans sorunlarını tanımlama ve çözme](licensing-groups-resolve-problems.md)
- [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](licensing-groups-migrate-users.md)
- [Azure Active Directory'de grup tabanlı lisanslama yı kullanarak kullanıcıların ürün lisansları arasında geçiş ilertirme](licensing-groups-change-licenses.md)
- [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](../active-directory-licensing-group-advanced.md)
- [Azure Active Directory'de grup tabanlı lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
