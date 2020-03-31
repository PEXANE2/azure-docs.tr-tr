---
title: Dinamik bir grup oluşturun veya edinin ve durum alın - Azure AD | Microsoft Dokümanlar
description: Azure portalında bir grup üyelik kuralı oluşturma veya güncelleştirme ve işleme durumunu denetleme.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/07/2020
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d2ed7f27e2145f666f38eec5ddc6c985a4d32138
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266383"
---
# <a name="create-or-update-a-dynamic-group-in-azure-active-directory"></a>Azure Etkin Dizin'de dinamik bir grup oluşturma veya güncelleştirme

Azure Etkin Dizini'nde (Azure AD), kullanıcı veya aygıt özelliklerine göre grup üyeliğini belirlemek için kuralları kullanabilirsiniz. Bu makalede, Azure portalında dinamik bir grup için bir kural nasıl ayarlır.
Dinamik üyelik, güvenlik grupları veya Office 365 grupları için desteklenir. Grup üyeliği kuralı uygulandığında, kullanıcı ve aygıt öznitelikleri üyelik kuralıyla eşleşen eşleşmeler için değerlendirilir. Bir kullanıcı veya aygıt için bir öznitelik değiştiğinde, kuruluştaki tüm dinamik grup kuralları üyelik değişiklikleri için işlenir. Kullanıcılar ve aygıtlar, bir grubun koşullarını karşılıyorsa eklenir veya kaldırılır. Güvenlik grupları aygıtlar veya kullanıcılar için kullanılabilir, ancak Office 365 grupları yalnızca kullanıcı grupları olabilir.

## <a name="rule-builder-in-the-azure-portal"></a>Azure portalında kural oluşturucu

Azure AD, önemli kurallarınızı daha hızlı oluşturmak ve güncelleştirmek için bir kural oluşturucu sağlar. Kural oluşturucu en fazla beş ifade inşaat destekler. Kural oluşturucu, birkaç basit ifadeyle bir kural oluşturmayı kolaylaştırır, ancak her kuralı çoğaltmak için kullanılamaz. Kural oluşturucu oluşturmak istediğiniz kuralı desteklemiyorsa, metin kutusunu kullanabilirsiniz.

Metin kutusunu kullanarak oluşturmanızı önerdiğimiz gelişmiş kurallara veya sözdizimine bazı örnekler aşağıda verilmiştir:

- Beşten fazla ifade içeren kural
- Doğrudan raporlar kuralı
- Ayar [operatörü önceliği](groups-dynamic-membership.md#operator-precedence)
- [Karmaşık ifadeler içeren kurallar;](groups-dynamic-membership.md#rules-with-complex-expressions) örneğin`(user.proxyAddresses -any (_ -contains "contoso"))`

> [!NOTE]
> Kural oluşturucu, metin kutusunda oluşturulmuş bazı kuralları görüntüleyemeyebilir. Kural oluşturucu kuralı görüntüleyemediğinde bir ileti görebilirsiniz. Kural oluşturucu, desteklenen sözdizimini, doğrulamayı veya dinamik grup kurallarının işlenmesini hiçbir şekilde değiştirmez.

![Dinamik bir grup için üyelik kuralı ekleme](./media/groups-create-rule/update-dynamic-group-rule.png)

Üyelik kuralı için sözdizimi, desteklenen özellikler, işleçler ve değerler örnekleri için Azure [Etkin Dizini'ndeki gruplar için Dinamik üyelik kurallarına](groups-dynamic-membership.md)bakın.

## <a name="to-create-a-group-membership-rule"></a>Grup üyeliği kuralı oluşturmak için

1. Azure AD [yönetici merkezinde,](https://aad.portal.azure.com) Kiracı'daki Global yönetici, Intune yöneticisi veya Kullanıcı yöneticisi rolünde bir hesapla oturum açın.
1. **Grupları**arayın ve seçin.
1. **Tüm grupları**seçin ve **Yeni grubu**seçin.

   ![Yeni grup eklemek için komutu seçin](./media/groups-create-rule/create-new-group-azure-active-directory.png)

1. **Grup** sayfasına, yeni grup için bir ad ve açıklama girin. Kullanıcılar veya aygıtlar için **Üyelik türü** seçin ve ardından dinamik **sorgu ekle'yi**seçin. Kural oluşturucu en fazla beş ifadeleri destekler. Beşten fazla ifade eklemek için metin kutusunu kullanmanız gerekir.

   ![Dinamik bir grup için üyelik kuralı ekleme](./media/groups-create-rule/add-dynamic-group-rule.png)

1. Üyelik sorgunuz için kullanılabilen özel uzantı özelliklerini görmek için:
   1. **Özel uzantı özelliklerini al'ı** seçin
   1. Uygulama kimliğini girin ve ardından **özellikleri yenile'yi**seçin.
1. Kuralı oluşturduktan sonra **Kaydet'i**seçin.
1. Grubu oluşturmak için **Yeni grup** sayfasında **Oluştur'u** seçin.

Girdiğiniz kural geçerli değilse, kuralın neden işlenemediğine ilişkin bir açıklama portaldaki bir Azure bildiriminde görüntülenir. Kuralın nasıl düzeltilebildiğini anlamak için dikkatle okuyun.

## <a name="to-update-an-existing-rule"></a>Varolan bir kuralı güncelleştirmek için

1. Azure AD [yönetici merkezinde,](https://aad.portal.azure.com) Kiracı'daki Global yönetici, Grup yöneticisi, Intune yöneticisi veya Kullanıcı yöneticisi rolünde bulunan bir hesapla oturum açın.
1. **Gruplar'ı** > Seçin**Tüm gruplar.**
1. Profilini açmak için bir grup seçin.
1. Grubun profil sayfasında **Dinamik üyelik kurallarını**seçin. Kural oluşturucu en fazla beş ifadeleri destekler. Beşten fazla ifade eklemek için metin kutusunu kullanmanız gerekir.

   ![Dinamik bir grup için üyelik kuralı ekleme](./media/groups-create-rule/update-dynamic-group-rule.png)

1. Üyelik kuralınız için kullanılabilen özel uzantı özelliklerini görmek için:
   1. **Özel uzantı özelliklerini al'ı** seçin
   1. Uygulama kimliğini girin ve ardından **özellikleri yenile'yi**seçin.
1. Kuralı güncelledikten sonra **Kaydet'i**seçin.

## <a name="turn-on-or-off-welcome-email"></a>Karşılama e-postalarını açma veya kapatma

Yeni bir Office 365 grubu oluşturulduğunda, gruba eklenen kullanıcılara hoş geldiniz e-posta bildirimi gönderilir. Daha sonra, bir kullanıcı nın veya aygıtın herhangi bir öznitelikleri değişirse, kuruluştaki tüm dinamik grup kuralları üyelik değişiklikleri için işlenir. Daha sonra eklenen kullanıcılar da hoş geldiniz bildirimi alırsınız. Bu davranışı [Exchange PowerShell'de](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps)kapatabilirsiniz.

## <a name="check-processing-status-for-a-rule"></a>Kural için işleme durumunu denetleme

Grup için **Genel Bakış** sayfasında üyelik işleme durumunu ve son güncelleştirilen tarihi görebilirsiniz.
  
  ![dinamik grup durumunun görüntülenmesi](./media/groups-create-rule/group-status.png)

Üyelik **işleme** durumu için aşağıdaki durum iletileri gösterilebilir:

- **Değerlendirme**: Grup değişikliği alındı ve güncellemeler değerlendiriliyor.
- **İşleme**: Güncelleştirmeler işleniyor.
- **Tamamlandı**: İşlem tamamlandı ve geçerli tüm güncellemeler yapıldı.
- **İşlem hatası**: Üyelik kuralını değerlendiren bir hata nedeniyle işleme tamamlanamadı.
- **Güncelleştirme duraklatıldı**: Dinamik üyelik kuralı güncelleştirmeleri yönetici tarafından duraklatıldı. MembershipRuleProcessingState "Duraklatıldı" olarak ayarlanır.

**Üyelik son güncellenen** durum için aşağıdaki durum iletileri gösterilebilir:

- &lt;**Tarih ve saat**&gt;: Üyeliğin son güncellenmesi.
- **Devam Ediyor**: Güncelleştirmeler şu anda devam ediyor.
- **Bilinmiyor**: Son güncelleştirme zamanı alınamıyor. Grup yeni olabilir.

Belirli bir grup için üyelik kuralını işlerken bir hata oluşursa, grup için **Genel Bakış sayfasının** üst kısmında bir uyarı gösterilir. Kiracıiçindeki tüm gruplar için 24 saatten fazla bekleyen dinamik üyelik güncelleştirmeleri işlenemezse, tüm **grupların**en üstünde bir uyarı gösterilir.

![hata iletisi uyarılarını işleme](./media/groups-create-rule/processing-error.png)

Bu makaleler, Azure Etkin Dizini'ndeki gruplar hakkında ek bilgiler sağlar.

- [Var olan grupları görme](../fundamentals/active-directory-groups-view-azure-portal.md)
- [Yeni grup oluşturma ve üye ekleme](../fundamentals/active-directory-groups-create-azure-portal.md)
- [Bir grubun ayarlarını yönetme](../fundamentals/active-directory-groups-settings-azure-portal.md)
- [Bir grubun üyeliklerini yönetme](../fundamentals/active-directory-groups-membership-azure-portal.md)
- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](groups-dynamic-membership.md)
