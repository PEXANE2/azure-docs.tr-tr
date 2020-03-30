---
title: 'Azure AD Connect eşitlemesi: Yanlışlıkla silmeleri önleme | Microsoft Dokümanlar'
description: Bu konu, Azure AD Connect'te yanlışlıkla silmeleri önleme (yanlışlıkla silmeleri önleme) özelliğini açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 6b852cb4-2850-40a1-8280-8724081601f7
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 48f3109b4c87e25444629ca25411894eab8a9d56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71827136"
---
# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect Eşitleme: Yanlışlıkla Silmeleri Engelleme
Bu konu, Azure AD Connect'te yanlışlıkla silmeleri önleme (yanlışlıkla silmeleri önleme) özelliğini açıklar.

Azure AD Connect'i yüklerken, yanlışlıkla silinmelerin varsayılan olarak etkinleştirilmesini ve 500'den fazla silme özelliğine sahip bir dış ayıltMaya izin vermeyecek şekilde yapılandırılmayı önleyin. Bu özellik, birçok kullanıcıyı ve nesneyi etkileyebilecek yanlışlıkla gerçekleştirilen yapılandırma değişikliklerinden ve şirket içi dizin değişikliklerinden koruma sağlamak için tasarlanmıştır.

## <a name="what-is-prevent-accidental-deletes"></a>Yanlışlıkla siler önlemek nedir
Birçok silme gördüğünüzde sık karşılaşılan senaryolar şunlardır:

* Tüm [OU](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) veya [etki alanının](how-to-connect-sync-configure-filtering.md#domain-based-filtering) seçilmediği yerlerde [filtreleme](how-to-connect-sync-configure-filtering.md) de değişiklikler.
* Bir kuruluş birimi içindeki tüm nesnelerin silinmesi.
* Bir kuruluş biriminin yeniden adlandırılması nedeniyle içindeki tüm nesnelerin eşitleme kapsamı dışında olarak değerlendirilmesi.

Azure Active Directory Connect ile yüklenen AD Sync `Enable-ADSyncExportDeletionThreshold`modülünün bir parçası olan PowerShell kullanılarak 500 nesnenin varsayılan değeri değiştirilebilir. Bu değeri kuruluşunuzun boyutuna uyacak şekilde yapılandırmanız gerekir. Eşitleme zamanlayıcısı her 30 dakikada çalıştığından, değer 30 dakika içinde görülen silme sayısıdır.

Azure AD'ye dışa aktarılamayacak kadar çok silme varsa, dışa aktarma durur ve aşağıdaki gibi bir e-posta alırsınız:

![Yanlışlıkla e-postayı silmeyi önleme](./media/how-to-connect-sync-feature-prevent-accidental-deletes/email.png)

> *Merhaba (teknik temas). (Time) kimlik eşitleme hizmeti, silme sayısının yapılandırılan silme eşiğini (kuruluş adı) aştığını algıladı. Bu Kimlik eşitleme çalışmasında silinmesi için toplam (sayı) nesne gönderildi. Bu, (sayı) nesnelerin yapılandırılan silme eşik değerini karşıladı veya aştı. Devam etmeden önce bu silme işlemlerinin yapılması gerektiğini doğrulamanızı istiyoruz. Bu e-posta iletisinde listelenen hata hakkında daha fazla bilgi için lütfen önleme yanlışlıkla silme lere bakın.*
>
> 

Dışa Aktarma profili `stopped-deletion-threshold-exceeded` için **Eşitleme Hizmet Yöneticisi** Kullanıcı Arabirimi'ne baktığınızda durumu da görebilirsiniz.
![Yanlışlıkla eşitleme Hizmeti Yöneticisi Kullanıcı UI silmeleri önleme](./media/how-to-connect-sync-feature-prevent-accidental-deletes/syncservicemanager.png)

Bu beklenmeyen bir durumsa, araştırın ve düzeltici eylemlerde bulundu. Hangi nesnelerin silinmek üzere olduğunu görmek için aşağıdakileri yapın:

1. **Eşitleme Hizmetini** Başlat Menüsünden Başlatın.
2. **Konektörler'e**gidin.
3. Azure Etkin Dizin türüne sahip **Bağlayıcı'yı**seçin.
4. **Sağdaki Eylemler** **altında, Arama Bağlayıcısı Alanı'nı**seçin.
5. **Kapsam**altındaki açılır pencerede, **Çünkü'i seçin** ve geçmişte bir zaman seçin. **Ara**’ya tıklayın. Bu sayfa, silinmek üzere olan tüm nesnelerin görünümünü sağlar. Her öğeyi tıklatarak, nesne hakkında ek bilgi alabilirsiniz. Kılavuzda görünür olmak için ek öznitelikler eklemek için **Sütun Ayarı'nı** da tıklatabilirsiniz.

![Bağlantı Alanı Ara](./media/how-to-connect-sync-feature-prevent-accidental-deletes/searchcs.png)

[!NOTE] Tüm silmelerin istendiğinden emin değilseniz ve daha güvenli bir rotadan geçmek istiyorsanız. PowerShell cmdlet: `Enable-ADSyncExportDeletionThreshold` İstenmeyen silmelere izin verebilecek eşiği devre dışı bırakmak yerine yeni bir eşik ayarlamak için kullanabilirsiniz. 

## <a name="if-all-deletes-are-desired"></a>Tüm silmeler isteniyorsa
Tüm silmeler isteniyorsa, aşağıdakileri yapın:

1. Geçerli silme eşiğini almak için PowerShell cmdlet'i `Get-ADSyncExportDeletionThreshold`çalıştırın. Azure AD Global Administrator hesabı ve parolası sağlayın. Varsayılan değer 500'dür.
2. Bu korumayı geçici olarak devre dışı bırakmak ve bu silmelerin geçmesine izin vermek için PowerShell cmdlet'i çalıştırın: `Disable-ADSyncExportDeletionThreshold`. Azure AD Global Administrator hesabı ve parolası sağlayın.
   ![Kimlik Bilgileri](./media/how-to-connect-sync-feature-prevent-accidental-deletes/credentials.png)
3. Azure Etkin Dizin Bağlayıcısı hala seçiliyken, eylem **Çalıştır'ı** seçin ve **Dışa Aktar'ı**seçin.
4. Korumayı yeniden etkinleştirmek için PowerShell cmdlet'i çalıştırın: `Enable-ADSyncExportDeletionThreshold -DeletionThreshold 500`. 500'i, geçerli silme eşiğini alırken fark ettiğiniz değerle değiştirin. Azure AD Global Administrator hesabı ve parolası sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Eşitlemeyi anlama ve özelleştirme](how-to-connect-sync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
