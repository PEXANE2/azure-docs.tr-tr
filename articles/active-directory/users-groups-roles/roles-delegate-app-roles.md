---
title: Temsilci uygulaması yönetici yönetimi permaları - Azure AD | Microsoft Dokümanlar
description: Azure Active Directory'de uygulama erişim yönetimi için izin verme
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 251bc1c2277f9e43543f95c49d0b730a5a41c3d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253045"
---
# <a name="delegate-app-registration-permissions-in-azure-active-directory"></a>Azure Active Directory'de uygulama kayıt izinlerini temsilciolarak

Bu makalede, uygulama yönetimi gereksinimlerinizi karşılamak için Azure Etkin Dizini'ndeki (Azure AD) özel roller tarafından verilen izinlerin nasıl kullanılacağı açıklanmaktadır. Azure AD'de, Uygulama oluşturma ve yönetim izinlerini aşağıdaki yollarla devredebilirsiniz:

- [Kimlerin uygulama oluşturabileceğini](#restrict-who-can-create-applications) ve oluşturdukları uygulamaları yönetebileceğini kısıtlama. Azure AD'de varsayılan olarak, tüm kullanıcılar uygulama kayıtlarını kaydedebilir ve oluşturdukları uygulamaların tüm yönlerini yönetebilir. Bu, yalnızca belirli kişilere izin vermek için sınırlandırılabilir.
- [Bir uygulamaya bir veya daha fazla sahip atama.](#assign-application-owners) Bu, birisine belirli bir uygulama için Azure AD yapılandırmasının tüm yönlerini yönetme olanağı vermenin basit bir yoludur.
- Tüm uygulamalar için Azure AD'deki yapılandırmayı yönetmeye erişim sağlayan [yerleşik bir yönetim rolü atama.](#assign-built-in-application-admin-roles) Bt uzmanlarına, Azure AD'nin uygulama yapılandırması ile ilgili olmayan diğer bölümlerini yönetmeye erişim vermeden geniş uygulama yapılandırma izinlerini yönetmelerine izin vermenin önerilen yolu budur.
- Çok özel izinleri tanımlayan [özel bir rol oluşturmak](#create-and-assign-a-custom-role-preview) ve bunu sınırlı bir sahip olarak tek bir uygulamanın kapsamına veya dizin kapsamına (tüm uygulamalar) sınırlı bir yönetici olarak atamak.

Yukarıdaki yöntemlerden birini kullanarak iki nedenden dolayı erişim izni vermeyi göz önünde bulundurmak önemlidir. İlk olarak, yönetim görevlerini gerçekleştirme yeteneğini devretmek, genel yönetici yüküne neden olabilir. İkinci olarak, sınırlı izinler kullanmak güvenlik duruşunuzu geliştirir ve yetkisiz erişim potansiyelini azaltır. [Azure Active Directory'de Temsilci yönetiminde](roles-concept-delegation.md)delegasyon konuları ve genel yönergeler tartışılmıştır.

## <a name="restrict-who-can-create-applications"></a>Uygulamaları kimlerin oluşturabileceğini kısıtlama

Azure AD'de varsayılan olarak, tüm kullanıcılar uygulama kayıtlarını kaydedebilir ve oluşturdukları uygulamaların tüm yönlerini yönetebilir. Herkes, şirket verilerine kendi adlarına erişen uygulamalara onay verme yeteneğine de sahiptir. Genel anahtarları 'Hayır' olarak ayarlayarak ve seçilen kullanıcıları Uygulama Geliştiricisi rolüne ekleyerek bu izinleri seçerek vermeyi seçebilirsiniz.

### <a name="to-disable-the-default-ability-to-create-application-registrations-or-consent-to-applications"></a>Uygulama kayıtları oluşturma veya başvurulara onay verme varsayılan yeteneğini devre dışı kılabilir

1. Azure REKLAM kuruluşunuzdaki Genel yönetici rolüne uygun bir hesapla Azure REKLAM kuruluşunuzda oturum açın.
1. Aşağıdakilerden birini veya her ikisini ayarlayın:

    -  [Kuruluşunuz için Kullanıcı ayarları sayfasında,](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings)Kullanıcılar no olarak ayarlanan **uygulamaları kaydedebilirsiniz** ayarlayın. Bu, kullanıcıların uygulama kayıtları oluşturması için varsayılan yeteneği devre dışı bırakılır.
    - Kurumsal [uygulamaların kullanıcı ayarlarında,](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/)Kullanıcılar kendi **adlarına şirket verilerine erişen uygulamaların** No olarak belirlenmesine izin verebilir. Bu, kullanıcıların şirket verilerine kendi adlarına erişen uygulamalara onay vermelerini varsayılan özelliği devre dışı kalacaktır.

### <a name="grant-individual-permissions-to-create-and-consent-to-applications-when-the-default-ability-is-disabled"></a>Varsayılan yetenek devre dışı bırakıldığında uygulamaları oluşturmak ve uygulamaları kabul etmek için bireysel izinler verme

Kullanıcılar başvuru ayarını No olarak **kaydettirdiğinde** uygulama kayıtları oluşturma olanağı sağlamak için Uygulama geliştiricisi rolünü atayın. Bu rol aynı **zamanda, Kullanıcılar kendi adlarına şirket verilerine erişen uygulamalara onay verebildiği** zaman kendi adına rıza gösterme izni de verir. Sistem davranışı olarak, bir kullanıcı yeni bir uygulama kaydı oluşturduğunda, otomatik olarak ilk sahibi olarak eklenir. Sahiplik izinleri, kullanıcıya sahip oldukları bir uygulama kaydının veya kurumsal uygulamanın tüm yönlerini yönetme olanağı sağlar.

## <a name="assign-application-owners"></a>Uygulama sahiplerini atama

Sahiplerini atamak, belirli bir uygulama kaydı veya kurumsal uygulama için Azure AD yapılandırmasının tüm yönlerini yönetme olanağı nı vermenin basit bir yoludur. Sistem davranışı olarak, bir kullanıcı yeni bir uygulama kaydı oluşturduğunda otomatik olarak ilk sahibi olarak eklenir. Sahiplik izinleri, kullanıcıya sahip oldukları bir uygulama kaydının veya kurumsal uygulamanın tüm yönlerini yönetme olanağı sağlar. Orijinal sahibi kaldırılabilir ve ek sahipleri eklenebilir.

### <a name="enterprise-application-owners"></a>Kurumsal uygulama sahipleri

Bir kullanıcı sahibi olarak, tek oturum açma yapılandırması, sağlama ve kullanıcı atamaları gibi kuruluş uygulamasının kuruluşa özgü yapılandırmasını yönetebilir. Sahipler başka kullanıcıları sahip olarak ekleyebilir veya kaldırabilir. Global yöneticilerin aksine, sahipleri yalnızca sahip oldukları kurumsal uygulamaları yönetebilir.

Bazı durumlarda, uygulama galerisinden oluşturulan kurumsal uygulamalar hem kurumsal uygulama hem de uygulama kaydı içerir. Bu doğruysa, kurumsal uygulamaya bir sahip eklemek, sahibi otomatik olarak ilgili uygulama kaydına sahibini bir sahip olarak ekler.

### <a name="to-assign-an-owner-to-an-enterprise-application"></a>Bir işletme uygulamasına bir sahibi atamak için

1. [Azure REKLAM kuruluşunuzda,](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) kuruluş için Uygulama yöneticisi veya Bulut uygulama yöneticisi için uygun bir hesapla oturum açın.
1. Kuruluşun [Uygulama kayıtları sayfasında,](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) uygulama için Genel Bakış sayfasını açmak için bir uygulama seçin.
1. Uygulamanın sahiplerinin listesini görmek için **Sahipleri** seçin.
1. Uygulamaya eklemek için bir veya daha fazla sahip seçmek için **Ekle'yi** seçin.

> [!IMPORTANT]
> Kullanıcılar ve hizmet müdürleri uygulama kayıtlarının sahibi olabilir. Yalnızca kullanıcılar kurumsal uygulamaların sahibi olabilir. Gruplarher ikisinin de sahibi olarak atanamaz.
>
> Sahipler bir uygulamaya kimlik bilgileri ekleyebilir ve bu kimlik bilgilerini uygulamanın kimliğini taklit etmek için kullanabilir. Uygulama sahibinden daha fazla izine sahip olabilir ve bu nedenle, sahibinin kullanıcı veya hizmet sorumlusu olarak erişebildikleri ne kadar ayrıcalıklı bir ayrıcalık tır. Uygulama sahibi, uygulamanın izinlerine bağlı olarak uygulamanın kimliğine bürünürken kullanıcıları veya diğer nesneleri oluşturabilir veya güncelleyebilir.

## <a name="assign-built-in-application-admin-roles"></a>Yerleşik uygulama yöneticisi rollerini atama

Azure AD, tüm uygulamalar için Azure AD'de yapılandırmayı yönetmeye erişim sağlamak için yerleşik yönetici rolleri kümesine sahiptir. Bu roller, BT uzmanlarına, Azure AD'nin uygulama yapılandırması ile ilgili olmayan diğer bölümlerini yönetmeye erişim vermeden geniş uygulama yapılandırma izinlerini yönetmelerine erişim izni vermenin önerilen yoludur.

- Uygulama Yöneticisi: Bu roldeki kullanıcılar kurumsal uygulamaların, uygulama kayıtlarının ve uygulama proxy ayarlarının tüm yönlerini oluşturabilir ve yönetebilir. Bu rol ayrıca, temsilci verilen izinlere ve Microsoft Graph hariç uygulama izinlerine izin verme olanağı da verir. Bu role atanan kullanıcılar, yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenmez.
- Bulut Uygulama Yöneticisi: Bu roldeki kullanıcılar, uygulama proxy'sini yönetme olanağı dışında Uygulama Yöneticisi rolüyle aynı izinlere sahiptir. Bu role atanan kullanıcılar, yeni uygulama kayıtları veya kurumsal uygulamalar oluştururken sahip olarak eklenmez.

Daha fazla bilgi ve bu rollerin açıklamasını görüntülemek için [Kullanılabilir rolleri](directory-assign-admin-roles.md#available-roles)görün.

Uygulama Yöneticisi veya Bulut Uygulama Yöneticisi rollerini atamak [için Azure Active Directory](../fundamentals/active-directory-users-assign-role-azure-portal.md) nasıl yapılacağını kullanıcılara atama rollerindeki yönergeleri izleyin.

> [!IMPORTANT]
> Uygulama Yöneticileri ve Bulut Uygulama Yöneticileri, bir uygulamaya kimlik bilgileri ekleyebilir ve bu kimlik bilgilerini uygulamanın kimliğini taklit etmek için kullanabilir. Uygulama, yönetici rolün izinleri üzerinde ayrıcalık yüksekliği olan izinlere sahip olabilir. Bu roldeki bir yönetici, uygulamanın izinlerine bağlı olarak uygulamayı taklit ederken kullanıcıları veya diğer nesneleri oluşturabilir veya güncelleyebilir.
> Her iki rol de Koşullu Erişim ayarlarını yönetme olanağı sağlar.

## <a name="create-and-assign-a-custom-role-preview"></a>Özel bir rol oluşturma ve atama (önizleme)

Özel roller oluşturma ve özel roller atamaayrı adımlardır:

- [Özel bir *rol tanımı* oluşturun](roles-create-custom.md) ve önceden ayarlanmış bir [listeden izinler ekleyin.](roles-custom-available-permissions.md) Bunlar yerleşik rollerde kullanılan izinlerle aynıdır.
- Özel rolü atamak için [bir *rol ataması* oluşturun.](roles-assign-powershell.md)

Bu ayırma, tek bir rol tanımı oluşturmanıza ve sonra farklı *kapsamlarda*birçok kez atamanıza olanak tanır. Özel bir rol kuruluş genelinde kapsamda atanabilir veya tek bir Azure AD nesnesi varsa kapsamda atanabilir. Nesne kapsamının bir örneği tek bir uygulama kaydıdır. Farklı kapsamlar kullanılarak, aynı rol tanımı Sally'ye kuruluştaki tüm uygulama kayıtları üzerinden atanabilir ve ardından naveen'e yalnızca Contoso Gider Raporları uygulama kaydı üzerinden atanabilir.

Uygulama yönetimini devre delegasyon için özel roller oluştururken ve kullanırken ipuçları:
- Özel roller yalnızca Azure AD portalının en güncel uygulama kayıt bıçaklarına erişim verir. Eski uygulama kayıtları bıçaklarına erişim izni vermezler.
- "Azure AD yönetim portalına erişimi kısıtla" kullanıcı ayarı Evet olarak ayarlandığında özel roller Azure AD portalına erişim vermez.
- Kullanıcının rol atamalarını kullanarak erişesahip olduğu uygulama kayıtları yalnızca Uygulama kayıt sayfasındaki 'Tüm uygulamalar' sekmesinde gösteriş gösterir. Bunlar 'Sahip olunan uygulamalar' sekmesinde görünmüyor.

Özel rollerin temelleri hakkında daha fazla bilgi için, [özel rollere genel bakışın](roles-custom-overview.md)yanı sıra [özel bir rolün](roles-create-custom.md) nasıl oluşturulup oluşturulabildiğini ve [nasıl bir rol atayabildiğinize](roles-assign-powershell.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Başvuru kayıt alt türleri ve izinleri](roles-custom-available-permissions.md)
- [Azure AD yöneticisi rol başvurusu](directory-assign-admin-roles.md)
