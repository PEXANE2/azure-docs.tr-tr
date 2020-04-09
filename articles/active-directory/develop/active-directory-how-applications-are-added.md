---
title: Azure AD'ye uygulamalar nasıl ve neden eklenir?
titleSuffix: Microsoft identity platform
description: Bir uygulamanın Azure AD'ye eklenmesi ne anlama gelir ve bu uygulama oraya nasıl gelir?
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: lenalepa, sureshja
ms.openlocfilehash: 01ea22af472877abe34236ec82a7750eccfcdfb9
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80884282"
---
# <a name="how-and-why-applications-are-added-to-azure-ad"></a>Uygulamalar Azure AD'ye nasıl ve neden eklenir?

Azure AD'deki uygulamaların iki temsili vardır:

* [Uygulama nesneleri](app-objects-and-service-principals.md#application-object) - [Özel durumlar](#notes-and-exceptions)olmasına rağmen, uygulama nesneleri bir uygulamanın tanımı olarak kabul edilebilir.
* [Hizmet ilkeleri](app-objects-and-service-principals.md#service-principal-object) - Bir uygulama örneği olarak kabul edilebilir. Hizmet ilkeleri genellikle bir uygulama nesnesi başvurur ve bir uygulama nesnesi dizinler arasında birden çok hizmet ilkeleri tarafından başvurulabilir.

## <a name="what-are-application-objects-and-where-do-they-come-from"></a>Uygulama nesneleri nedir ve nereden gelir?

[Uygulama Kayıtları](https://aka.ms/appregistrations) deneyimi aracılığıyla Azure portalındaki [uygulama nesnelerini](app-objects-and-service-principals.md#application-object) yönetebilirsiniz. Uygulama nesneleri uygulamayı Azure AD'ye açıklar ve uygulamanın tanımı olarak kabul edilerek, hizmetin ayarlarına göre uygulamaya nasıl belirteçler verilebildiğini bilmesine olanak tanır. Uygulama nesnesi, diğer dizinlerde hizmet ilkelerini destekleyen çok kiracılı bir uygulama olsa bile, yalnızca kendi ev dizininde bulunur. Uygulama nesnesi aşağıdakilerden herhangi birini içerebilir (ayrıca burada belirtilmeyen ek bilgiler):

* Ad, logo ve yayıncı
* Uri'leri Yönlendirme
* Sırlar (uygulamanın kimliğini doğrulamak için kullanılan simetrik ve/veya asimetrik tuşlar)
* API bağımlılıkları (OAuth)
* Yayınlanmış API'ler/kaynaklar/kapsamlar (OAuth)
* Uygulama rolleri (RBAC)
* SSO meta verileri ve yapılandırması
* Kullanıcı sağlama meta veri ve yapılandırma
* Proxy meta veri ve yapılandırma

Uygulama nesneleri, şunlardır:

* Azure portalında uygulama kayıtları
* Visual Studio'yu kullanarak yeni bir uygulama oluşturma ve Azure AD kimlik doğrulamasını kullanacak şekilde yapılandırma
* Bir yönetici uygulama galerisinden bir uygulama eklediğinde (bu da bir hizmet ilkesi oluşturur)
* Yeni bir uygulama oluşturmak için Microsoft Graph API veya PowerShell'i kullanma
* Azure'daki ve geliştirici merkezlerindeki API explorer deneyimlerinde çeşitli geliştirici deneyimleri de dahil olmak üzere pek çok kişi

## <a name="what-are-service-principals-and-where-do-they-come-from"></a>Hizmet müdürleri nelerdir ve nereden gelirler?

Azure portalındaki [hizmet ilkelerini](app-objects-and-service-principals.md#service-principal-object) Kurumsal [Uygulamalar](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/) deneyimi aracılığıyla yönetebilirsiniz. Hizmet ilkeleri, Azure AD'ye bağlanan bir uygulamayı yöneten lerdir ve dizininizdeki uygulama örneği olarak kabul edilebilir. Belirli bir uygulama için, en fazla bir uygulama nesnesi (bir "ev" dizininde kayıtlıdır) ve bir veya daha fazla hizmet asıl nesneleri hareket ettiği her dizinde uygulama örneklerini temsil edebilir. 

Hizmet sorumlusu şunları içerebilir:

* Uygulama kimliği özelliği aracılığıyla bir uygulama nesnesine geri başvuru
* Yerel kullanıcı ve grup uygulama rolü atamalarının kayıtları
* Uygulamaya verilen yerel kullanıcı ve yönetici izinlerinin kayıtları
  * Örneğin: uygulamanın belirli bir kullanıcının e-postasına erişme izni
* Koşullu Erişim ilkesi de dahil olmak üzere yerel ilkelerin kayıtları
* Bir uygulama için alternatif yerel ayarların kayıtları
  * Talepler dönüşüm kuralları
  * Öznitelik eşlemeleri (Kullanıcı sağlama)
  * Dizin özel uygulama rolleri (uygulama özel rolleri destekliyorsa)
  * Dizin özel adı veya logosu

Uygulama nesneleri gibi, hizmet ilkeleri de dahil olmak üzere birden çok yol aracılığıyla oluşturulabilir:

* Kullanıcılar Azure AD ile tümleşik bir üçüncü taraf uygulamada oturum açtıklarında
  * Oturum açma sırasında, kullanıcılardan profillerine ve diğer izinlerine erişmek için uygulamaya izin vermeleri istenir. İzin veren ilk kişi, başvuruyu temsil eden bir hizmet yöneticisinin dizine eklenmesine neden olur.
* Kullanıcılar [Office 365](https://products.office.com/) gibi Microsoft çevrimiçi hizmetlerinde oturum açınca
  * Office 365'e abone olduğunuzda veya deneme sürümüne başladığınızda, dizinde Office 365 ile ilişkili tüm işlevleri sunmak için kullanılan çeşitli hizmetleri temsil eden bir veya daha fazla hizmet ilkesi oluşturulur.
  * SharePoint gibi bazı Office 365 hizmetleri, iş akışları da dahil olmak üzere bileşenler arasında güvenli iletişim sağlamak için sürekli olarak hizmet ilkeleri oluşturur.
* Bir yönetici uygulama galerisinden bir uygulama eklediğinde (bu da altta yatan bir uygulama nesnesi oluşturur)
* Azure AD Uygulama [Proxy'sini](/azure/active-directory/manage-apps/application-proxy) kullanmak için uygulama ekleme
* SAML veya parola tek oturum açma (SSO) kullanarak tek oturum için bir uygulama bağlama
* Microsoft Graph API veya PowerShell üzerinden programlanabilir

## <a name="how-are-application-objects-and-service-principals-related-to-each-other"></a>Uygulama nesneleri ve hizmet ilkeleri birbiriyle nasıl ilişkilidir?

Bir uygulamanın ev dizininde, çalıştığı dizinlerin her birinde (uygulamanın ev dizini dahil) bir veya daha fazla hizmet yöneticisi tarafından başvurulan bir uygulama nesnesi vardır.

![Uygulama nesneleri ve hizmet ilkeleri arasındaki ilişkiyi gösterir][apps_service_principals_directory]

Önceki diyagramda, Microsoft uygulamaları yayımlamak için kullandığı dahili (solda gösterilen) iki dizin tutar:

* Microsoft Apps için bir tane (Microsoft hizmetleri dizini)
* Önceden entegre edilmiş üçüncü taraf uygulamaları için bir tane (Uygulama galerisi dizini)

Azure AD ile tümleşen uygulama yayıncılarının/satıcılarının bir yayımlama dizinine sahip olması gerekir (sağda "Bazı SaaS Dizini" olarak gösterilir).

Kendinizi eklediğiniz (diyagramda **App (sizin)** olarak temsil edilen) uygulamalar şunlardır:

* Geliştirdiğiniz uygulamalar (Azure AD ile tümleştirilmiş)
* Tek oturum açma için bağlayacağınız uygulamalar
* Azure AD uygulama proxy'sini kullanarak yayınladığınız uygulamalar

### <a name="notes-and-exceptions"></a>Notlar ve özel durumlar

* Tüm hizmet ilkeleri bir uygulama nesnesini işaret etmez. Azure AD ilk olarak oluşturulduğunda, uygulamalara sağlanan hizmetler daha sınırlıydı ve hizmet ilkesi bir uygulama kimliği oluşturmak için yeterliydi. Özgün hizmet yöneticisi, Windows Server Active Directory hizmet hesabına daha yakın durumdaydı. Bu nedenle, önce bir uygulama nesnesi oluşturmadan Azure AD PowerShell'i kullanmak gibi farklı yollardan hizmet ilkeleri oluşturmak yine de mümkündür. Microsoft Graph API bir hizmet ilkesi oluşturmadan önce bir uygulama nesnesi gerektirir.
* Yukarıda açıklanan bilgilerin tümü şu anda programlı olarak açıklanmaz. Aşağıdakiler yalnızca UI'de kullanılabilir:
  * Talepler dönüşüm kuralları
  * Öznitelik eşlemeleri (Kullanıcı sağlama)
* Hizmet ilkesi ve uygulama nesneleri hakkında daha ayrıntılı bilgi için Microsoft Graph API başvuru belgelerine bakın:
  * [Uygulama](https://docs.microsoft.com/graph/api/resources/application?view=graph-rest-1.0)
  * [Hizmet Müdürü](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta)

## <a name="why-do-applications-integrate-with-azure-ad"></a>Uygulamalar neden Azure AD ile tümleşir?

Uygulamalar, aşağıdakiler de dahil olmak üzere sağladığı hizmetlerden bir veya daha fazlasını yararlanmak için Azure AD'ye eklenir:

* Uygulama kimlik doğrulaması ve yetkilendirme
* Kullanıcı kimlik doğrulaması ve yetkilendirme
* Federasyon veya şifre kullanarak SSO
* Kullanıcı sağlama ve senkronizasyon
* Rol tabanlı erişim denetimi - Uygulamada rol tabanlı yetkilendirme denetimleri gerçekleştirmek için uygulama rollerini tanımlamak için dizini kullanın
* OAuth yetkilendirme hizmetleri - API'lere/kaynaklara erişim yetkisi vermek için Office 365 ve diğer Microsoft uygulamaları tarafından kullanılır
* Uygulama yayıncılığı ve proxy - Özel bir ağdan internete bir uygulama yayınlayın

## <a name="who-has-permission-to-add-applications-to-my-azure-ad-instance"></a>Azure AD örneğime uygulama ekleme izni kimler var?

Yalnızca genel yöneticilerin yapabileceği bazı görevler olsa da (uygulama galerisinden uygulama galerisinden uygulama ekleme ve Uygulama Proxy'sini kullanacak bir uygulama yapılandırma gibi) varsayılan olarak dizininizdeki tüm kullanıcılar geliştirdikleri uygulama nesnelerini kaydetme hakkına sahiptir ve rıza yoluyla örgütsel verilerine hangi uygulamaları paylaştıkları/erişebilecekleri konusunda takdir hakkına sahiptir. Bir kişi, bir uygulamada oturum açan ve onay veren ilk dizindeki ilk kullanıcıysa, bu kiracınızda bir hizmet yöneticisi oluşturur; aksi takdirde, rıza hibe bilgileri mevcut hizmet sorumlusunda saklanır.

Kullanıcıların kaydolmasına ve uygulamalara izin vermelerine izin vermek başlangıçta endişe verici gelebilir, ancak aşağıdakileri göz önünde bulundurun:


* Uygulamalar, uygulamanın dizine kaydedilmesine veya kaydedilmesine gerek kalmadan uzun yıllar boyunca kullanıcı kimlik doğrulaması için Windows Server Active Directory'den yararlanabilmiştir. Şimdi organizasyon tam olarak kaç uygulama dizin kullanıyor ve ne amaçla görünürlüğünü artırmış olacak.
* Bu sorumlulukların kullanıcılara devredilmesi, yönetici odaklı bir uygulama kaydı ve yayımlama işlemine duyulan ihtiyacı yok eder. Active Directory Federation Services (ADFS) ile bir yöneticinin geliştiricileri adına güvenilen bir taraf olarak bir uygulama eklemesi gerekiyordu. Artık geliştiriciler self servis yapabilir.
* Kullanıcıların kuruluş hesaplarını iş amacıyla kullanarak uygulamalara oturum açmaları iyi bir şeydir. Daha sonra kuruluştan ayrılırlarsa, kullandıkları uygulamada otomatik olarak kendi hesabına erişimlerini kaybederler.
* Hangi verilerle paylaşıldıka iyi bir şey. Veriler her zamankinden daha taşınabilir ve hangi verileri hangi uygulamalarla paylaştığını net bir şekilde kaydetmek yararlıdır.
* OAuth için Azure AD kullanan API sahipleri, kullanıcıların uygulamalara tam olarak hangi izinleri verebileceğine ve hangi izinlerin bir yöneticinin kabul etmesini gerektirdiğine karar verir. Yalnızca yöneticiler daha büyük kapsamları ve daha önemli izinleri kabul edebilirken, kullanıcı onayı kullanıcıların kendi verilerine ve yeteneklerine göre kapsama sahiptir.
* Bir kullanıcı bir uygulamanın verilerine erişmesine izin verdiğinde, bir uygulamanın dizine nasıl eklenmiş olduğunu belirlemek için Azure portalındaki Denetim Raporlarını görüntüleyebilmeniz için olay denetlenebilir.

Dizininizdeki kullanıcıların uygulamaları kaydetmesini ve yönetici onayı olmadan uygulamalarda oturum açmalarını engellemek istiyorsanız, bu özellikleri kapatmak için değiştirebileceğiniz iki ayar vardır:

* Kullanıcıların kendi adlarına uygulamalara rıza göstermelerini önlemek için:
  1. Azure portalında, Kurumsal uygulamalar altında [Kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) bölümüne gidin.
  2. Değiştir **Kullanıcılar, kendi adlarına şirket verilerine erişen uygulamalara** **hayır**olarak izin verebilir.
     
     > [!NOTE]
     > Kullanıcı onayBelgesini kapatmaya karar verirseniz, bir yöneticinin kullanıcının kullanması gereken yeni uygulamaları kabul etmesi gerekir.

* Kullanıcıların kendi uygulamalarını kaydettirmelerini önlemek için:
  1. Azure portalında, Azure Etkin Dizini altındaki [Kullanıcı ayarları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/UserSettings) bölümüne gidin
  2. Değiştir Kullanıcılar uygulamaları **No**olarak **kaydedebilir.**

> [!NOTE]
> Microsoft varsayılan yapılandırmayı, kullanıcıların uygulamaları kaydedebilmesi ve uygulamaları kendi adlarına kabul edebilmeleri için kullanır.

<!--Image references-->
[apps_service_principals_directory]:../media/active-directory-how-applications-are-added/HowAppsAreAddedToAAD.jpg
