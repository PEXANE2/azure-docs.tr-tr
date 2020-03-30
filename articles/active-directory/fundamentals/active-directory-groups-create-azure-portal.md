---
title: Temel bir grup oluşturun ve üye ekleyin - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Etkin Dizini'ni kullanarak temel bir grubun nasıl oluşturulacağına ilişkin yönergeler.
services: active-directory
author: msaburnley
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: quickstart
ms.date: 03/01/2019
ms.author: ajburnle
ms.reviewer: krbain
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c551a81788df8d68664abaa03f467a4521222d0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73473479"
---
# <a name="create-a-basic-group-and-add-members-using-azure-active-directory"></a>Azure Etkin Dizini'ni kullanarak temel bir grup oluşturun ve üye ekleyin
Azure Active Directory (Azure AD) portalını kullanarak temel bir grup oluşturabilirsiniz. Bu makalenin amaçları doğrultusunda, kaynak sahibi (yönetici) tarafından tek bir kaynağa temel bir grup eklenir ve bu grup, o kaynağa erişmesi gereken belirli üyeleri (çalışanlar) içerir. Dinamik üyelikler ve kural oluşturma da dahil olmak üzere daha karmaşık senaryolar için bkz. [Azure Active Directory kullanıcı yönetimi belgeleri](../users-groups-roles/index.yml).

## <a name="create-a-basic-group-and-add-members"></a>Temel bir grup oluşturma ve üye ekleme
Temel bir grup oluşturabilir ve aynı anda üyelerinizi ekleyebilirsiniz.

### <a name="to-create-a-basic-group-and-add-members"></a>Temel bir grup oluşturmak ve üye eklemek için
1. Dizin için bir Genel yönetici hesabı kullanarak [Azure portalda](https://portal.azure.com) oturum açın.

1. **Azure Active Directory**'yi bulun ve seçin.

    ![Gruplar'ın gösterdiği Azure REKLAM sayfası](media/active-directory-groups-create-azure-portal/search-active-directory.png)

1. Etkin **Dizin** sayfasında **Gruplar'ı** seçin ve ardından **Yeni grup'u**seçin.

    ![Gruplar'ın gösterdiği Azure REKLAM sayfası](media/active-directory-groups-create-azure-portal/group-full-screen.png)

1. Yeni **Grup** sayfasında, gerekli bilgileri doldurun.

    ![Örnek bilgileriyle doldurulmuş şekilde yeni grup sayfası](media/active-directory-groups-create-azure-portal/new-group-blade.png)

   - **Grup türü (gerekli).** Önceden tanımlanmış bir grup türü seçin. Bu içerir: - **Güvenlik**. Bir kullanıcı grubu için paylaşılan kaynaklara üye ve bilgisayar erişimini yönetmek için kullanılır. Örneğin, belirli bir güvenlik ilkesi için bir güvenlik grubu oluşturabilirsiniz. Böylece, her bir üyeye ayrı ayrı izin eklemek zorunda kalmadan aynı anda tüm üyelere bir dizi izin verebilirsiniz. Kaynaklara erişimi yönetme hakkında daha fazla bilgi için bkz. [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md).
               - **Ofis 365**. Üyelerin paylaşılan posta kutusuna, takvime, takvime, dosyalara, SharePoint sitesine vb.’ye erişmesini sağlayarak işbirliği fırsatları sunar. Bu seçenek, kuruluşunuzun dışındaki kişilerin de gruba erişmesini sağlar. Office 365 Grupları hakkında daha fazla bilgi için bkz. [Office 365 Grupları hakkında bilgi edinin](https://support.office.com/article/learn-about-office-365-groups-b565caa1-5c40-40ef-9915-60fdb2d97fa2).

   - **Grup adı (gerekli).** Grup için unutmayacağınız ve mantıklı olan bir ad ekler. Adın başka bir grup için zaten kullanIlip kullanılmadığını belirlemek için bir denetim gerçekleştirilir. Yinelenen adlandırmayı önlemek için ad zaten kullanılıyorsa, grubunuzun adını değiştirmeniz istenir.

   - **Grup e-posta adresi (gereklidir).** Grup için bir e-posta adresi ekleyin veya otomatik olarak doldurulan e-posta adresini saklayın.

   - **Grup açıklaması.** Grubunuza isteğe bağlı bir açıklama ekler.

   - **Üyelik türü (gerekli).** Önceden tanımlanmış bir üyelik türü seçin. Buna şunlar dahildir: - **Atanmış.** Bu grubun üyesi olacak ve benzersiz izinlere sahip olacak şekilde belirli kullanıcıları eklemenize olanak sağlar. Bu makalenin amaçları doğrultusunda, bu seçeneği kullanıyoruz.
          - **Dinamik kullanıcı.** Üyeleri otomatik olarak eklemek ve kaldırmak için dinamik üyelik kurallarını kullanmanıza olanak tanır. Bir üyenin öznitelikleri değişirse sistem, üyenin kural gereksinimlerini karşıladığını mı (eklendiğini) yoksa artık kural gereksinimlerini karşılamadığını mı (kaldırıldığını) görmek amacıyla dizin için dinamik grup kurallarınıza bakar.
          - **Dinamik cihaz.** Otomatik olarak cihazlar eklemek ve kaldırmak için dinamik grup kuralları kullanmanıza olanak sağlar. Bir cihazın öznitelikleri değişirse sistem, cihazın kural gereksinimlerini karşıladığını mı (eklendiğini) yoksa artık kural gereksinimlerini karşılamadığını mı (kaldırıldığını) görmek amacıyla dizin için dinamik grup kurallarınıza bakar.

        >[!Important]
        >Ya cihazlar ya da kullanıcılar için bir dinamik grup oluşturabilirsiniz, her ikisi için oluşturamazsınız. Ayrıca cihaz sahiplerinin özniteliklerine göre de bir cihaz grubu oluşturamazsınız. Cihaz üyeliği kuralları yalnızca cihaz ilişkilendirmesine başvurabilir. Kullanıcılar ve aygıtlar için dinamik bir grup oluşturma hakkında daha fazla bilgi için dinamik [bir grup oluştur ve durumu denetle'yi](../users-groups-roles/groups-create-rule.md)görün.

1. **Oluştur'u**seçin.

    Grubunuz oluşturulur ve üyeler eklemeniz için hazır olur.

1. **Grup** sayfasından **Üyeler** alanını seçin ve sonra **Üyeleri seç** sayfasından grubunuza eklenecek üyeleri aramaya başlayın.

    ![Grup oluşturma işlemi sırasında grubunuz için üyeleri seçme](media/active-directory-groups-create-azure-portal/select-members-create-group.png)

1. Üye ekleme işleminiz bittiğinde **Seç** öğesini seçin.

    **Gruba Genel Bakış** sayfası, şimdi gruba eklenen üye sayısını gösterecek şekilde güncelleştirilir.

    ![Üye sayısı vurgulanmış şekilde Gruba Genel Bakış sayfası](media/active-directory-groups-create-azure-portal/group-overview-blade-number-highlight.png)

## <a name="turn-on-or-off-welcome-email"></a>Karşılama e-postalarını açma veya kapatma

Dinamik veya statik üyelikle yeni bir Office 365 grubu oluşturulduğunda, gruba eklenen tüm kullanıcılara hoş geldiniz bildirimi gönderilir. Bir kullanıcının veya aygıtın herhangi bir öznitelikleri değiştiğinde, kuruluştaki tüm dinamik grup kuralları olası üyelik değişiklikleri için işlenir. Daha sonra eklenen kullanıcılar da hoş geldiniz bildirimi alırsınız. Bu davranışı [Exchange PowerShell'de](https://docs.microsoft.com/powershell/module/exchange/users-and-groups/Set-UnifiedGroup?view=exchange-ps)kapatabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Şimdi bir grup ve en az bir kullanıcı eklediğinize göre artık aşağıdakileri yapabilirsiniz:

- [Grupları ve üyeleri görüntüleme](active-directory-groups-view-azure-portal.md)

- [Grup üyeliğini yönetme](active-directory-groups-membership-azure-portal.md)

- [Bir gruptaki kullanıcılar için dinamik kuralları yönetme](../users-groups-roles/groups-create-rule.md)

- [Grup ayarlarınızı düzenleme](active-directory-groups-settings-azure-portal.md)

- [Grupları kullanarak kaynaklara erişimi yönetme](active-directory-manage-groups.md)

- [Grupları kullanarak SaaS uygulamalarına erişimi yönetme](../users-groups-roles/groups-saasapps.md)

- [PowerShell komutlarını kullanarak grupları yönetme](../users-groups-roles/groups-settings-v2-cmdlets.md)

- [Azure Etkin Dizini'ne Azure aboneliği ni ilişkilendirme veya ekleme](active-directory-how-subscriptions-associated-directory.md)
