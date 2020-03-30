---
title: B2B konuk kullanıcıözellikleri - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory B2B konuk kullanıcı özellikleri ve durumdavetiye kullanımdan önce ve sonra
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro, seo-update-azuread-jan, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40f5002e361653614c966dc43301afa83eb7b200
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050807"
---
# <a name="properties-of-an-azure-active-directory-b2b-collaboration-user"></a>Azure Active Directory B2B işbirliği kullanıcısının özellikleri

Bu makalede, Azure Etkin Dizini'ndeki (Azure AD) B2B konuk kullanıcı nesnesinin özellikleri ve durumları, davet inden önce ve sonra açıklanmaktadır. Azure AD'nin işletmeden işletmeye (B2B) işbirliği kullanıcısı UserType = Guest'e sahip bir kullanıcıdır. Bu konuk kullanıcı genellikle bir iş ortağı kuruluşundan gelir ve varsayılan olarak davet dizininde sınırlı ayrıcalıklara sahiptir.

Davet eden kuruluşun gereksinimlerine bağlı olarak, bir Azure AD B2B işbirliği kullanıcısı aşağıdaki hesap durumlarından birinde bulunabilir:

- Durum 1: Azure AD'nin harici bir örneğinde yer alan ve davet eden kuruluşta konuk kullanıcı olarak temsil edilen. Bu durumda, B2B kullanıcıdavet kiracıya ait bir Azure AD hesabı kullanarak kaydolur. İş ortağı kuruluş Azure AD kullanmıyorsa, Azure AD'deki konuk kullanıcı yine de oluşturulur. Gereksinimler, davetlerini kullanmaları ve Azure AD'nin e-posta adreslerini doğrulamalarıdır. Bu düzenleme aynı zamanda bir just-in-time (JIT) kira veya bir "viral" kira denir.

   > [!IMPORTANT]
   > **31 Mart 2021'den itibaren Microsoft,** B2B işbirliği senaryoları için yönetilmeyen Azure AD hesapları ve kiracılar oluşturarak artık davetlerin itfasını desteklemez. Hazırlık olarak, müşterilerin tek [seferlik parola kimlik doğrulaması e-postasını](one-time-passcode.md)seçmelerini öneririz. Bu genel önizleme özelliği hakkındaki görüşlerinizi memnuniyetle karşılıyoruz ve işbirliği yapmak için daha fazla yol oluşturmaktan heyecan duyuyoruz.

- Durum 2: Bir Microsoft veya başka bir hesapta yer alan ve ana kuruluşta konuk kullanıcı olarak temsil edilen. Bu durumda, konuk kullanıcı bir Microsoft hesabı veya sosyal hesabı (google.com veya benzer) ile imzalar. Davet edilen kullanıcının kimliği, teklif intihal sırasında davet eden kuruluşun dizininde bir Microsoft hesabı olarak oluşturulur.

- Durum 3: Ev sahibi kuruluşun şirket içi Etkin Dizinine dahil olur ve ev sahibi kuruluşun Azure AD'si ile senkronize edilir. İş ortağı hesaplarını Kullanıcı Türü = Konuk ile Azure AD B2B kullanıcıları olarak bulutla senkronize etmek için Azure AD Connect'i kullanabilirsiniz. Bkz. [Yerel olarak yönetilen iş ortağı hesaplarının bulut kaynaklarına erişimini sağlar.](hybrid-on-premises-to-cloud.md)

- Durum 4: Ev sahibi kuruluşun Azure AD'ında UserType = Konuk ve ana kuruluşun yönettiği kimlik bilgileriyle yer alan.

  ![Dört kullanıcı durumu gösteren diyagram](media/user-properties/redemption-diagram.png)


Şimdi, Azure AD ad işbirliği kullanıcısının Azure AD'de nasıl göründüğünü görelim.

### <a name="before-invitation-redemption"></a>Davet inden önce

State 1 ve State 2 hesapları, konuk kullanıcıların kendi kimlik bilgilerini kullanarak işbirliği yapmaya davet etmenin sonucudur. Davet ilk olarak konuk kullanıcıya gönderildiğinde, dizininizde bir hesap oluşturulur. Kimlik doğrulaması konuk kullanıcının kimlik sağlayıcısı tarafından gerçekleştirildığından, bu hesapla ilişkili herhangi bir kimlik belgesi yoktur. Dizininizdeki konuk kullanıcı hesabının **Kaynak** özelliği **Davet edilen kullanıcı**olarak ayarlanır. 

![Teklif intifası öncesi kullanıcı özelliklerini gösteren ekran görüntüsü](media/user-properties/before-redemption.png)

### <a name="after-invitation-redemption"></a>Davet inden sonra

Konuk kullanıcı daveti kabul ettikten **sonra, Kaynak** özelliği konuk kullanıcının kimlik sağlayıcısına göre güncelleştirilir.

Devlet 1'deki konuk kullanıcılar için **Kaynak** **Dış Azure Etkin Dizini'dir.**

![Teklif intifası sonrası Devlet 1 konuk kullanıcı](media/user-properties/after-redemption-state1.png)

Devlet 2'deki konuk kullanıcılar için **Kaynak** **Microsoft Hesabı'dır.**

![Teklif inden sonra Devlet 2 konuk kullanıcı](media/user-properties/after-redemption-state2.png)

Devlet 3 ve Durum 4'teki konuk kullanıcılar için **Kaynak** özelliği, bir sonraki bölümde açıklandığı gibi **Azure Etkin Dizini** veya **Windows Server Etkin Dizini**olarak ayarlanır.

## <a name="key-properties-of-the-azure-ad-b2b-collaboration-user"></a>Azure AD B2B işbirliği kullanıcısının temel özellikleri
### <a name="usertype"></a>UserType
Bu özellik, kullanıcının ana bilgisayar kirasıyla ilişkisini gösterir. Bu özellik iki değere sahip olabilir:
- Üye: Bu değer, ana kuruluş çalışanını ve kuruluşun bordrosundaki bir kullanıcıyı gösterir. Örneğin, bu kullanıcı yalnızca dahili sitelere erişebilmek için bekliyor. Bu kullanıcı harici bir ortak çalışan olarak kabul edilmez.

- Konuk: Bu değer, şirket için dahili olarak kabul edilen harici bir ortak, ortak veya müşteri gibi bir kullanıcıyı gösterir. Böyle bir kullanıcının bir CEO'nun dahili notlarını alması veya şirket avantajlarından yararlanması bekfensi değildir.

  > [!NOTE]
  > UserType'ın kullanıcının nasıl imzaladığı, kullanıcının dizin rolü ve benzeri hiçbir ilişkisi yoktur. Bu özellik yalnızca kullanıcının ana bilgisayar kuruluşuyla ilişkisini gösterir ve kuruluşun bu özelliğe bağlı ilkeleri uygulamasına izin verir.

### <a name="source"></a>Kaynak
Bu özellik, kullanıcının nasıl imzaladığını gösterir.

- Davet Edilen Kullanıcı: Bu kullanıcı davet edildi, ancak henüz bir davet kullanmadı.

- Dış Azure Etkin Dizin: Bu kullanıcı harici bir kuruluşta yer almaktadır ve diğer kuruluşa ait bir Azure AD hesabı kullanarak kimlik doğrulaması yapmaktadır. Bu tür oturum açma, Durum 1'e karşılık gelir.

- Microsoft hesabı: Bu kullanıcı bir Microsoft hesabına aittir ve bir Microsoft hesabı kullanarak kimlik doğrulaması yapılır. Bu tür oturum açma, Durum 2'ye karşılık gelir.

- Windows Server Active Directory: Bu kullanıcı, bu kuruluşa ait şirket içi Active Directory'den oturum açılır. Bu tür oturum açma, Durum 3'e karşılık gelir.

- Azure Etkin Dizin: Bu kullanıcı, bu kuruluşa ait bir Azure REKLAM hesabı kullanarak kimlik doğrulaması sağlar. Bu tür oturum açma, Durum 4'e karşılık gelir.
  > [!NOTE]
  > Kaynak ve UserType bağımsız özelliklerdir. Kaynak değeri UserType için belirli bir değer anlamına gelmez.

## <a name="can-azure-ad-b2b-users-be-added-as-members-instead-of-guests"></a>Azure AD B2B kullanıcıları misafir yerine üye olarak eklenebilir mi?
Genellikle, bir Azure AD B2B kullanıcısı ve konuk kullanıcı eş anlamlıdır. Bu nedenle, varsayılan olarak UserType = Guest'e sahip bir kullanıcı olarak bir Azure AD B2B işbirliği kullanıcısı eklenir. Ancak, bazı durumlarda, ortak kuruluş ana kuruluş da ait olduğu daha büyük bir kuruluşun üyesidir. Bu durumda, ana bilgisayar kuruluşu ortak kuruluştaki kullanıcıları konuk yerine üye olarak ele almak isteyebilir. Ortak kuruluştan bir kullanıcıyı üye olarak ana kuruluşa eklemek veya davet etmek için Azure AD B2B Invitation Manager API'lerini kullanın.

## <a name="filter-for-guest-users-in-the-directory"></a>Dizindeki konuk kullanıcılar için filtre

![Konuk kullanıcılar için filtreyi gösteren ekran görüntüsü](media/user-properties/filter-guest-users.png)

## <a name="convert-usertype"></a>Kullanıcı Türünü Dönüştür
PowerShell'i kullanarak UserType'ı Üyeden Misafir'e dönüştürmek mümkündür. Ancak, UserType özelliği kullanıcının kuruluşla ilişkisini temsil eder. Bu nedenle, bu özelliği yalnızca kullanıcının kuruluşla ilişkisi değişirse değiştirmeniz gerekir. Kullanıcı ilişkisi değişirse, kullanıcı ana adı (UPN) değişmeli mi? Kullanıcı aynı kaynaklara erişmeye devam etmeli mi? Posta kutusu atanmalı mı? PowerShell'i atomik bir etkinlik olarak kullanarak UserType'ı değiştirmenizi önermiyoruz. Ayrıca, bu özelliğin PowerShell kullanılarak değişmez hale gelmesi durumunda, bu değere bağımlılık yapmanızı önermiyoruz.

## <a name="remove-guest-user-limitations"></a>Konuk kullanıcı sınırlamalarını kaldırma
Konuk kullanıcılarınıza daha yüksek ayrıcalıklar vermek istediğiniz durumlar olabilir. Herhangi bir role konuk kullanıcı ekleyebilir ve hatta bir kullanıcıya üyelerle aynı ayrıcalıkları vermek için dizindeki varsayılan konuk kullanıcı kısıtlamalarını kaldırabilirsiniz.

Şirket dizinindeki konuk kullanıcının üye kullanıcıyla aynı izinlere sahip olması için varsayılan sınırlamaları kapatmak mümkündür.

![Kullanıcı ayarlarında Harici kullanıcılar seçeneğini gösteren ekran görüntüsü](media/user-properties/remove-guest-limitations.png)

## <a name="can-i-make-guest-users-visible-in-the-exchange-global-address-list"></a>Konuk kullanıcıları Exchange Global Adres Listesi'nde görünür hale getirebilir miyim?
Evet. Varsayılan olarak, konuk nesneler kuruluşunuzun genel adres listesinde görünmez, ancak bunları görünür kılmak için Azure Active Directory PowerShell'i kullanabilirsiniz. Ayrıntılar için bkz: Konuk nesneleri genel adres listesinde görünür [Manage guest access in Office 365 Groups](https://docs.microsoft.com/office365/admin/create-groups/manage-guest-access-in-groups?redirectSourcePath=%252fen-us%252farticle%252fmanage-guest-access-in-office-365-groups-9de497a9-2f5c-43d6-ae18-767f2e6fe6e0&view=o365-worldwide#add-guests-to-the-global-address-list) **yapabilir miyim?** 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [B2B işbirliği kullanıcı belirteçleri](user-token.md)
* [B2B işbirliği kullanıcı iddiaları eşleme](claims-mapping.md)
