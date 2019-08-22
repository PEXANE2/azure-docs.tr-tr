---
title: Dinamik grup oluşturma ve durumu denetleme-Azure Active Directory | Microsoft Docs
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
ms.openlocfilehash: cb4f9d2f78857231d0ecd81a2538a75b4b8a2f74
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69650287"
---
# <a name="create-a-dynamic-group-and-check-status"></a>Dinamik grup oluşturma ve durumu denetleme

Azure Active Directory (Azure AD) ' de, Kullanıcı veya cihaz özelliklerine göre grup üyeliğini Belirleme kurallarını kullanabilirsiniz. Bu makalede, Azure portal bir dinamik grup için nasıl bir kural ayarlanacağı açıklanır.
Dinamik üyelik güvenlik grupları veya Office 365 grupları için desteklenir. Bir grup üyeliği kuralı uygulandığında, Kullanıcı ve cihaz öznitelikleri üyelik kuralıyla eşleşmeler için değerlendirilir. Bir kullanıcı veya cihaz için bir öznitelik değiştiğinde, kuruluştaki tüm dinamik grup kuralları üyelik değişiklikleri için işlenir. Kullanıcılar ve cihazlar bir grubun koşullarını karşılıyorsa eklenir veya kaldırılır. Güvenlik grupları, cihazlar veya kullanıcılar için kullanılabilir, ancak Office 365 grupları yalnızca Kullanıcı grupları olabilir.

Bir üyelik kuralı için sözdizimi, desteklenen özellikler, işleçler ve değerler örnekleri için bkz. [Azure Active Directory gruplar Için dinamik üyelik kuralları](groups-dynamic-membership.md).

## <a name="to-create-a-group-membership-rule"></a>Bir grup üyeliği kuralı oluşturmak için

1. [Azure AD Yönetim merkezinde](https://aad.portal.azure.com) , Kiracıdaki genel yönetici, Intune Yöneticisi veya Kullanıcı Yöneticisi rolündeki bir hesapla oturum açın.
2. **Grupları**seçin.
3. **Tüm gruplar**' ı seçin ve **Yeni Grup**' u seçin.

   ![Yeni grup eklemek için komutu seçin](./media/groups-create-rule/new-group-creation.png)

4. **Grup** sayfasında, yeni grup için bir ad ve açıklama girin. Kullanıcılar veya cihazlar için bir **üyelik türü** seçin ve ardından **Dinamik sorgu Ekle**' yi seçin. Kural Oluşturucusu en fazla beş ifadeyi destekler. Altıncı veya sonraki bir ifadeyi eklemek için metin kutusunu kullanmanız gerekir.

   ![Dinamik bir grup için üyelik kuralı ekle](./media/groups-create-rule/add-dynamic-group-rule.png)

5. Üyelik sorgunuz için kullanılabilen özel uzantı özelliklerini görmek için
   1. **Özel uzantı özelliklerini al** seçeneğini belirleyin
   2. Uygulama KIMLIĞI ' ni girin ve ardından **özellikleri Yenile**' yi seçin.
6. Kuralı oluşturduktan sonra **Kaydet**' i seçin.
7. Grubu oluşturmak için **Yeni Grup** sayfasında **Oluştur** ' u seçin.

Girdiğiniz kural geçerli değilse, kuralın neden işlenmediği hakkında bir açıklama, portalda bir Azure bildiriminde görüntülenir. Kuralı nasıl düzelteceğinizi anlamak için dikkatle okuyun.

## <a name="turn-on-or-off-welcome-email"></a>Hoş geldiniz e-postasını aç veya kapat

Yeni bir Office 365 Grubu oluşturulduğunda, gruba eklenen kullanıcılar bir hoş geldiniz e-posta bildirimi gönderilir. Daha sonra, bir kullanıcının veya cihazın herhangi bir özniteliği değişirse, kuruluştaki tüm dinamik grup kuralları üyelik değişiklikleri için işlenir. Ayrıca eklenen kullanıcılar da hoş geldiniz bildirimini alır. Bu davranışı [Exchange PowerShell](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps)'de devre dışı bırakabilirsiniz.

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

* &lt;**Tarih ve saat**&gt;: Üyeliğin güncelleştirildiği son zaman.
* **Devam ediyor**: Güncelleştirmeler Şu anda devam ediyor.
* **Bilinmiyor**: Son güncelleştirme saati alınamıyor. Grup yeni olabilir.

Belirli bir grup için üyelik kuralı işlenirken bir hata oluşursa, grubun **Genel Bakış sayfasının** üst kısmında bir uyarı gösterilir. Kiracıdaki tüm gruplar için bekleyen dinamik üyelik güncelleştirmeleri, daha sonra 24 saat boyunca işlenemezse, **tüm grupların**en üstünde bir uyarı gösterilir.

![işlem hata iletisi uyarıları](./media/groups-create-rule/processing-error.png)

Bu makaleler Azure Active Directory gruplar hakkında ek bilgiler sağlar.

* [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
* [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
* [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
* [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
