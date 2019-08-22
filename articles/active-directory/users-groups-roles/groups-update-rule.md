---
title: Dinamik bir grup kuralını güncelleştirme ve yönetme ve üyelik sorunlarını giderme-Azure Active Directory | Microsoft Docs
description: Azure portal bir grup üyeliği kuralı oluşturma, durumu denetleme.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/12/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ce3bce5e2656efea0a4fc3d7aa6be46f1e6926ec
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69657352"
---
# <a name="update-a-dynamic-group-to-manage-membership-in-azure-active-directory"></a>Azure Active Directory üyeliğini yönetmek için dinamik bir grubu güncelleştirme

Azure Active Directory (Azure AD) ' de, Kullanıcı veya cihaz özelliklerine göre grup üyeliğini Belirleme kurallarını kullanabilirsiniz. Bu makalede, Azure portal bir dinamik grup için nasıl bir kural ayarlanacağı açıklanır.
Dinamik üyelik güvenlik grupları veya Office 365 grupları için desteklenir. Bir grup üyeliği kuralı uygulandığında, Kullanıcı ve cihaz öznitelikleri üyelik kuralıyla eşleşmeler için değerlendirilir. Bir kullanıcı veya cihaz için bir öznitelik değiştiğinde, kuruluştaki tüm dinamik grup kuralları üyelik değişiklikleri için işlenir. Kullanıcılar ve cihazlar bir grubun koşullarını karşılıyorsa eklenir veya kaldırılır.

Bir üyelik kuralı için sözdizimi, desteklenen özellikler, işleçler ve değerler örnekleri için bkz. [Azure Active Directory gruplar Için dinamik üyelik kuralları](groups-dynamic-membership.md).

## <a name="to-update-a-group-membership-rule"></a>Bir grup üyeliği kuralını güncelleştirmek için

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) , Kiracıdaki genel yönetici, Intune Yöneticisi veya Kullanıcı Yöneticisi rolündeki bir hesapla oturum açın.
1. **Gruplar** > **tüm gruplar**' ı seçin.
1. Profilini açmak için bir grup seçin.
1. Grubun profil sayfasında **dinamik üyelik kuralları**' nı seçin. Kural Oluşturucusu en fazla beş ifadeyi destekler. Altıncı veya sonraki bir ifadeyi eklemek için metin kutusunu kullanmanız gerekir.

   ![Dinamik bir grup için üyelik kuralı ekle](./media/groups-update-rule/update-dynamic-group-rule.png)

1. Üyelik kuralınız için kullanılabilen özel uzantı özelliklerini görmek için:
   1. **Özel uzantı özelliklerini al** seçeneğini belirleyin
   2. Uygulama KIMLIĞI ' ni girin ve ardından **özellikleri Yenile**' yi seçin.
1. Kuralı güncelleştirdikten sonra **Kaydet**' i seçin.

Girdiğiniz kural geçerli değilse, kuralın neden işlenmediği hakkında bir açıklama, portalda bir Azure bildiriminde görüntülenir. Kuralı nasıl düzelteceğinizi anlamak için dikkatle okuyun.

## <a name="check-processing-status-for-a-rule"></a>Bir kural için işleme durumunu denetleme

Grubun **genel bakış** sayfasında Üyelik işleme durumu ve son güncelleştirilme tarihi ' ni görebilirsiniz.
  
  ![dinamik grup durumunu görüntüleme](./media/groups-create-rule/group-status.png)

**Üyelik işleme** durumu için aşağıdaki durum iletileri görüntülenebilir:

* **Değerlendiriliyor**:  Grup değişikliği alındı ve güncelleştirmeler değerlendiriliyor.
* **İşleme**: Güncelleştirmeler işleniyor.
* **Güncelleştirme Tamam**: İşlem tamamlandı ve geçerli tüm güncelleştirmeler yapıldı.
* **İşlem hatası**:  Üyelik kuralı değerlendirilirken bir hata nedeniyle işlem tamamlanamadı.
* **Güncelleştirme duraklatıldı**: Dinamik üyelik kuralı güncelleştirmeleri yönetici tarafından duraklatıldı. MembershipRuleProcessingState, "duraklatıldı" olarak ayarlandı.

**Üyelik son güncelleştirilme** durumu için aşağıdaki durum iletileri görüntülenebilir:

* **Tarih ve saat**: Üyeliğin güncelleştirildiği son zaman.
* **Devam ediyor**: Güncelleştirmeler Şu anda devam ediyor.
* **Bilinmiyor**: Son güncelleştirme saati alınamıyor. Grup yeni olabilir.

Belirli bir grup için üyelik kuralı işlenirken bir hata oluşursa, grubun **Genel Bakış sayfasının** üst kısmında bir uyarı gösterilir. Kiracıdaki tüm gruplar için bekleyen dinamik üyelik güncelleştirmeleri, daha sonra 24 saat boyunca işlenemezse, **tüm grupların**en üstünde bir uyarı gösterilir.

![işlem hata iletisi uyarıları](./media/groups-create-rule/processing-error.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleler, Azure AD 'de dinamik gruplarla çalışma hakkında ek bilgiler sağlar.

* Dinamik kural yapısına yönelik kapsamlı bir başvuru için bkz. [dinamik üyelik kuralı sözdizimi](groups-dynamic-membership.md).
* [Statik bir üyelik grubu oluşturun ve üye ekleyin](../fundamentals/active-directory-groups-create-azure-portal.md).
