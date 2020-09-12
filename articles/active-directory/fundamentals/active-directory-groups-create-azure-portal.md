---
title: Temel Grup oluşturma ve üye ekleme-Azure Active Directory | Microsoft Docs
description: Azure Active Directory kullanarak temel Grup oluşturma yönergeleri.
services: active-directory
author: ajburnle
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: how-to
ms.date: 06/05/2020
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18, contperfq4
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3dcbd8618dc0f2bae2eacc9ced67869d8209286a
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89565533"
---
# <a name="create-a-basic-group-and-add-members-using-azure-active-directory"></a>Azure Active Directory kullanarak temel Grup oluşturma ve üye ekleme
Azure Active Directory (Azure AD) portalını kullanarak temel bir grup oluşturabilirsiniz. Bu makalenin amaçları doğrultusunda, kaynak sahibi (yönetici) tarafından tek bir kaynağa temel bir grup eklenir ve bu grup, o kaynağa erişmesi gereken belirli üyeleri (çalışanlar) içerir. Dinamik üyelikler ve kural oluşturma da dahil olmak üzere daha karmaşık senaryolar için bkz. [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).

## <a name="group-and-membership-types"></a>Grup ve Üyelik türleri
Birkaç grup ve üyelik türü vardır. Aşağıdaki bilgiler, bir grup oluştururken hangi seçeneklere izin vereceğinize karar vermenize yardımcı olmak üzere her grup ve üyelik türünü ve bunların neden kullanıldığını açıklamaktadır.

### <a name="group-types"></a>Grup türleri:
- **Güvenlik**. Bir kullanıcı grubu için paylaşılan kaynaklara üye ve bilgisayar erişimini yönetmek için kullanılır. Örneğin, belirli bir güvenlik ilkesi için bir güvenlik grubu oluşturabilirsiniz. Böylece, her üyeye ayrı ayrı izin eklemek zorunda kalmadan aynı anda tüm üyelere bir dizi izin verebilirsiniz. Bir güvenlik grubu, sahipleri olarak kullanıcıları, cihazları, grupları ve hizmet sorumlularını üye, kullanıcılar ve hizmet sorumluları olarak içerebilir. Kaynaklara erişimi yönetme hakkında daha fazla bilgi için bkz. [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md).
- **Microsoft 365**. Üyelerin paylaşılan posta kutusuna, takvime, takvime, dosyalara, SharePoint sitesine vb.’ye erişmesini sağlayarak işbirliği fırsatları sunar. Bu seçenek, kuruluşunuzun dışındaki kişilerin de gruba erişmesini sağlar. Bir Microsoft 365 Grubu yalnızca üyeleri olarak kullanıcıları içerebilir. Hem kullanıcılar hem de hizmet sorumluları Microsoft 365 grubunun sahipleri olabilir. Microsoft 365 grupları hakkında daha fazla bilgi için bkz. [Microsoft 365 grupları hakkında bilgi edinin](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2).

### <a name="membership-types"></a>Üyelik türleri:
- **Atanan.** Bu grubun üyesi olacak ve benzersiz izinlere sahip olacak şekilde belirli kullanıcıları eklemenize olanak sağlar. Bu makalenin amaçları doğrultusunda, bu seçeneği kullanıyoruz.
- **Dinamik Kullanıcı.** , Üyeleri otomatik olarak eklemek ve kaldırmak için dinamik üyelik kuralları kullanmanıza olanak sağlar. Bir üyenin öznitelikleri değişirse sistem, üyenin kural gereksinimlerini karşıladığını mı (eklendiğini) yoksa artık kural gereksinimlerini karşılamadığını mı (kaldırıldığını) görmek amacıyla dizin için dinamik grup kurallarınıza bakar.
- **Dinamik cihaz.** Otomatik olarak cihazlar eklemek ve kaldırmak için dinamik grup kuralları kullanmanıza olanak sağlar. Bir cihazın öznitelikleri değişirse sistem, cihazın kural gereksinimlerini karşıladığını mı (eklendiğini) yoksa artık kural gereksinimlerini karşılamadığını mı (kaldırıldığını) görmek amacıyla dizin için dinamik grup kurallarınıza bakar.

    > [!IMPORTANT]
    > Ya cihazlar ya da kullanıcılar için bir dinamik grup oluşturabilirsiniz, her ikisi için oluşturamazsınız. Ayrıca cihaz sahiplerinin özniteliklerine göre de bir cihaz grubu oluşturamazsınız. Cihaz üyeliği kuralları yalnızca cihaz ilişkilendirmesine başvurabilir. Kullanıcılar ve cihazlar için dinamik bir grup oluşturma hakkında daha fazla bilgi için bkz. [dinamik grup oluşturma ve durumu denetleme](../users-groups-roles/groups-create-rule.md)

## <a name="create-a-basic-group-and-add-members"></a>Temel bir grup oluşturma ve üye ekleme
Temel bir grup oluşturabilir ve aynı anda üyelerinizi ekleyebilirsiniz. Temel bir grup oluşturmak ve üye eklemek için aşağıdaki yordamı kullanın:

1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

1. **Azure Active Directory**'yi bulun ve seçin.

1. **Active Directory** sayfasında, **gruplar** ' ı seçin ve ardından **Yeni Grup**' u seçin.

    ![Grupları gösteren Azure AD sayfası](media/active-directory-groups-create-azure-portal/group-full-screen.png)

1. **Yeni Grup** bölmesi görüntülenir ve gerekli bilgileri doldurmanız gerekir.

    ![Örnek bilgileriyle doldurulmuş şekilde yeni grup sayfası](media/active-directory-groups-create-azure-portal/new-group-blade.png)

1. Önceden tanımlanmış bir **Grup türü**seçin. Grup türleri hakkında daha fazla bilgi için bkz. [Grup ve Üyelik türleri](#group-types).

1. Bir **Grup adı** oluşturun ve ekleyin. Anımsayacak ve grup için anlamlı bir ad seçin. Adın zaten başka bir grup tarafından kullanımda olup olmadığını belirlemekte bir denetim gerçekleştirilecek. Ad zaten kullanımda ise, yinelenen adlandırmayı önlemek için grubunuzun adını değiştirmeniz istenir.

1. Grup için bir **Grup e-posta adresi** ekleyin veya otomatik olarak doldurulmuş e-posta adresini saklayın.

1. **Grup açıklaması.** Grubunuza isteğe bağlı bir açıklama ekler.

1. Önceden tanımlanmış bir **üyelik türü seçin (gerekli).** Üyelik türleri hakkında daha fazla bilgi için bkz. [Grup ve Üyelik türleri](#membership-types).

1. **Oluştur**’u seçin. Grubunuz oluşturulur ve üyeler eklemeniz için hazır olur.

1. **Grup** sayfasından **Üyeler** alanını seçin ve sonra **Üyeleri seç** sayfasından grubunuza eklenecek üyeleri aramaya başlayın.

    ![Grup oluşturma işlemi sırasında grubunuz için üyeleri seçme](media/active-directory-groups-create-azure-portal/select-members-create-group.png)

1. Üye ekleme işleminiz bittiğinde **Seç** öğesini seçin.

    **Gruba Genel Bakış** sayfası, şimdi gruba eklenen üye sayısını gösterecek şekilde güncelleştirilir.

    ![Üye sayısı vurgulanmış şekilde Gruba Genel Bakış sayfası](media/active-directory-groups-create-azure-portal/group-overview-blade-number-highlight.png)

## <a name="turn-on-or-off-group-welcome-email"></a>Grup hoş geldiniz e-postasını aç veya kapat

Yeni Microsoft 365 Grubu oluşturulduğunda, dinamik veya statik üyelikle, gruba eklenen tüm kullanıcılara bir hoş geldiniz bildirimi gönderilir. Bir kullanıcının veya cihazın herhangi bir özniteliği değiştiğinde, kuruluştaki tüm dinamik grup kuralları olası üyelik değişiklikleri için işlenir. Ayrıca eklenen kullanıcılar da hoş geldiniz bildirimini alır. Bu davranışı [Exchange PowerShell](/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps)'de devre dışı bırakabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

- [Grupları kullanarak SaaS uygulamalarına erişimi yönetme](../users-groups-roles/groups-saasapps.md)
- [PowerShell komutlarını kullanarak grupları yönetme](../users-groups-roles/groups-settings-v2-cmdlets.md)