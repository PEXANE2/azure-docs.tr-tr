---
title: Azure'da Etkin Dizin tabanlı hizmetleri karşılaştırın | Microsoft Dokümanlar
description: Bu genel bakışta, Active Directory Domain Services, Azure Active Directory ve Azure Active Directory Etki Alanı Hizmetleri için farklı kimlik tekliflerini karşılaştırAbilirsiniz.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: overview
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 62826b209b37e5abe5d33455809ba1b894ddb85f
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476456"
---
# <a name="compare-self-managed-active-directory-domain-services-azure-active-directory-and-managed-azure-active-directory-domain-services"></a>Kendi kendini yöneten Active Directory Etki Alanı Hizmetlerini, Azure Etkin Dizini ve yönetilen Azure Etkin Dizin Etki Alanı Hizmetlerini karşılaştırın

Uygulamaları, hizmetleri veya aygıtları merkezi bir kimliğe erişmek sağlamak için Azure'da Active Directory tabanlı hizmetleri kullanmanın üç yaygın yolu vardır. Kimlik çözümlerindeki bu seçim, kuruluşunuzun gereksinimleri için en uygun dizini kullanma esnekliği sağlar. Örneğin, çoğunlukla yalnızca bulut kullanıcıları mobil aygıtları çalıştıran kullanıcıları yönetiyorsanız, kendi Active Directory Domain Services (AD DS) kimlik çözümünüzü oluşturmak ve çalıştırmak mantıklı olmayabilir. Bunun yerine, Azure Etkin Dizini'ni kullanabilirsiniz.

Active Directory tabanlı üç kimlik çözümü ortak bir ad ve teknolojiyi paylaşsa da, farklı müşteri taleplerini karşılayan hizmetler sunmak üzere tasarlanmıştır. Yüksek düzeyde, bu kimlik çözümleri ve özellik kümeleri şunlardır:

* **Active Directory Domain Services (AD DS)** - Kimlik ve kimlik doğrulama, bilgisayar nesnesi yönetimi, grup ilkesi ve güvenler gibi temel özellikleri sağlayan kurumsal kullanıma hazır hafif dizin erişim protokolü (LDAP) sunucusu.
    * AD DS, şirket içi BT ortamına sahip birçok kuruluşta merkezi bir bileşendir ve temel kullanıcı hesabı kimlik doğrulaması ve bilgisayar yönetimi özellikleri sağlar.
* **Azure Etkin Dizin (Azure AD)** - Office 365, Azure portalı veya SaaS uygulamaları gibi kaynaklar için kullanıcı hesabı ve kimlik doğrulama hizmetleri sağlayan bulut tabanlı kimlik ve mobil aygıt yönetimi.
    * Azure AD, bulutta yerel olarak çalışan kullanıcılara tek bir kimlik sağlamak için şirket içi BIR AD DS ortamıyla senkronize edilebilir.
* **Azure Active Directory Domain Services (Azure AD DS)** - Etki alanı birleştirme, grup ilkesi, LDAP ve Kerberos / NTLM kimlik doğrulaması gibi tam uyumlu geleneksel AD DS özelliklerinin bir alt kümesiyle yönetilen etki alanı hizmetleri sağlar.
    * Azure AD DS, şirket içi AD DS ortamıyla senkronize edilebilen Azure AD ile tümleşir. Bu yetenek, merkezi kimlik kullanım kılıflarını, kaldırma ve kaydırma stratejisinin bir parçası olarak Azure'da çalışan geleneksel web uygulamalarına genişletir.

Bu genel bakış makalesi, bu kimlik çözümlerinin birlikte nasıl çalışabileceğini veya kuruluşunuzun gereksinimlerine bağlı olarak bağımsız olarak nasıl kullanılacağını karşılaştırır ve karşılaştırır.

## <a name="azure-ad-ds-and-self-managed-ad-ds"></a>Azure AD DS ve kendi kendini yöneten AD DS

Kerberos veya NTLM gibi geleneksel kimlik doğrulama mekanizmalarına erişilmesi gereken uygulamalarınız ve hizmetleriniz varsa, bulutta Active Directory Domain Services sağlamanın iki yolu vardır:

* Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) kullanarak oluşturduğunuz *yönetilen* bir etki alanı. Microsoft gerekli kaynakları oluşturur ve yönetir.
* Sanal makineler (VM'ler), Windows Server konuk işletim sistemi ve Active Directory Domain Services (AD DS) gibi geleneksel kaynakları kullanarak oluşturduğunuz ve yapılandırdığınız *kendi kendini yöneten* bir etki alanı. Daha sonra bu kaynakları yönetmeye devam e

Azure AD DS ile temel hizmet *bileşenleri, yönetilen* bir etki alanı deneyimi olarak Microsoft tarafından sizin için dağıtılır ve korunur. Sanal Ayarlar, Windows Server OS veya etki alanı denetleyicileri (DCs) gibi bileşenler için AD DS altyapısını dağıtmaz, yönetmez, yamayapmaz ve güvenli hale getiremezsiniz.

Azure AD DS, geleneksel kendi kendini yöneten AD DS ortamına daha küçük bir özellik alt kümesi sağlar ve bu da bazı tasarım ve yönetim karmaşıklığını azaltır. Örneğin, tasarım ve bakım için AD ormanları, etki alanı, siteler ve çoğaltma bağlantıları yoktur. Bulutta çalışan ve Kerberos veya NTLM gibi geleneksel kimlik doğrulama mekanizmalarına erişilmesi gereken uygulamalar ve hizmetler için Azure AD DS, en az miktarda yönetim yüküyle yönetilen bir etki alanı deneyimi sağlar.

Kendi kendini yöneten bir AD DS ortamını dağıtıp çalıştırdığınızda, ilişkili tüm altyapı ve dizin bileşenlerini korumanız gerekir. Kendi kendini yöneten BIR AD DS ortamıyla ek bakım yükü vardır, ancak daha sonra şemayı genişletme veya orman güvenleri oluşturma gibi ek görevler yapabilirsiniz.

Buluttaki uygulamalara ve hizmetlere kimlik sağlayan kendi kendini yöneten bir AD DS ortamı için yaygın dağıtım modelleri şunlardır:

* **Bağımsız bulutad DS** - Azure VM'ler etki alanı denetleyicileri olarak yapılandırılır ve ayrı, yalnızca buluta özel BIR AD DS ortamı oluşturulur. Bu AD DS ortamı şirket içi AD DS ortamıyla bütünleşmez. Bulutta VM'leri oturum etmek ve yönetmek için farklı bir kimlik bilgileri kümesi kullanılır.
* **Kaynak ormanı dağıtımı** - Azure VM'leri etki alanı denetleyicileri olarak yapılandırılır ve varolan bir ormanın parçası olan bir AD DS etki alanı oluşturulur. Daha sonra bir güven ilişkisi şirket içi AD DS ortamına yapılandırılır. Diğer Azure VM'leri bu kaynak ormanına bulutta katılabilir. Kullanıcı kimlik doğrulaması, şirket içi AD DS ortamına vpn / ExpressRoute bağlantısı üzerinden çalışır.
* **Şirket içi etki alanını Azure'a genişletin** - Azure sanal ağı, VPN / ExpressRoute bağlantısını kullanarak şirket içi ağa bağlanır. Azure SANAL M'ler, etki alanı yla şirket içi AD DS ortamına katılmalarını sağlayan bu Azure sanal ağına bağlanır.
    * Alternatif, Azure VM'leri oluşturmak ve bunları şirket içi AD DS etki alanından çoğaltma etki alanı denetleyicileri olarak tanıtmaktır. Bu etki alanı denetleyicileri, şirket içi AD DS ortamına VPN / ExpressRoute bağlantısı üzerinden çoğalır. Şirket içi AD DS etki alanı etkin bir şekilde Azure'a genişletilir.

Aşağıdaki tablo, kuruluşunuz için ihtiyaç duyabileceğiniz bazı özellikleri ve yönetilen bir Azure AD DS etki alanı ile kendi kendini yöneten bir AD DS etki alanı arasındaki farkları özetleyilmektedir:

| **Özellik** | **Azure REKLAM DS** | **Kendi kendini yöneten AD DS** |
| ----------- |:---------------:|:----------------------:|
| **Yönetilen hizmet**                               | **&#x2713;** | **&#x2715;** |
| **Güvenli dağıtımlar**                            | **&#x2713;** | Yönetici dağıtımı güvenli hale |
| **DNS sunucusu**                                    | **&#x2713;** (yönetilen hizmet) |**&#x2713;** |
| **Etki alanı veya Kurumsal yönetici ayrıcalıkları** | **&#x2715;** | **&#x2713;** |
| **Etki alanına katılma**                                   | **&#x2713;** | **&#x2713;** |
| **NTLM ve Kerberos kullanarak etki alanı kimlik doğrulaması** | **&#x2713;** | **&#x2713;** |
| **Kerberos kısıtlı delegasyon**               | Kaynak tabanlı | Kaynak tabanlı & hesap tabanlı|
| **Özel OU yapısı**                           | **&#x2713;** | **&#x2713;** |
| **Grup İlkesi**                                  | **&#x2713;** | **&#x2713;** |
| **Şema uzantıları**                             | **&#x2715;** | **&#x2713;** |
| **AD etki alanı / orman güvenleri**                     | **&#x2713;** (yalnızca tek yönlü giden orman güvenleri) | **&#x2713;** |
| **Güvenli LDAP (LDAPS)**                           | **&#x2713;** | **&#x2713;** |
| **LDAP okuyun**                                     | **&#x2713;** | **&#x2713;** |
| **LDAP yazmak**                                    | **&#x2713;** (yönetilen etki alanı içinde) | **&#x2713;** |
| **Coğrafi dağıtılmış dağıtımlar**                   | **&#x2715;** | **&#x2713;** |

## <a name="azure-ad-ds-and-azure-ad"></a>Azure AD DS ve Azure REKLAM

Azure AD, kuruluş tarafından kullanılan aygıtların kimliğini yönetmenize ve bu aygıtlardan şirket kaynaklarına erişimi denetlemenize olanak tanır. Kullanıcılar ayrıca kişisel cihazlarını (kendi getir (BYO) modelini, aygıta kimlik sağlayan Azure AD'ye kaydedebilirler. Azure AD, bir kullanıcı Azure AD'de kaydolduğunda ve güvenli kaynaklara erişmek için aygıtı kullandığında aygıtın kimliğini doğrular. Cihaz, Microsoft Intune gibi Mobil Aygıt Yönetimi (MDM) yazılımı kullanılarak yönetilebilir. Bu yönetim yeteneği, hassas kaynaklara erişimi yönetilen ve ilke uyumlu aygıtlara sınırlamanızı sağlar.

Geleneksel bilgisayarlar ve dizüstü bilgisayarlar da Azure AD'ye katılabilir. Bu mekanizma, kullanıcıların şirket kimlik bilgilerini kullanarak aygıtta oturum açmalarına izin vermek gibi, Azure AD ile kişisel bir aygıt kaydetmenin aynı avantajlarını sunar.

Azure AD'ye katılan aygıtlar size aşağıdaki avantajları sağlar:

* Azure AD tarafından güvenli uygulamalariçin tek oturum açma (SSO).
* Aygıtlar arasında kullanıcı ayarlarının kurumsal ilke uyumlu dolaşımı.
* Kurumsal kimlik bilgilerini kullanarak Windows Mağazası'na kurumsal iş için erişim.
* İşletmeler için Windows Hello.
* Şirket ilkesine uygun cihazlardan uygulamalara ve kaynaklara kısıtlı erişim.

Aygıtlar, şirket içi AD DS ortamı nı içeren karma bir dağıtımla veya olmadan Azure AD'ye katılabilir. Aşağıdaki tabloda, ortak aygıt sahipliği modelleri ve bunların genellikle bir etki alanına nasıl birleştirileceği özetlenir:

| **Cihaz türü**                                        | **Cihaz platformları**             | **Mechanism**          |
|:----------------------------------------------------------| -------------------------------- | ---------------------- |
| Kişisel cihazlar                                          | Windows 10, iOS, Android, Mac OS | Azure AD kayıtlı    |
| Kuruluşa ait aygıt şirket içi AD DS'ye katılmadı | Windows 10                       | Azure AD'ye katılanlar        |
| Kuruluşa ait aygıt, şirket içi AD DS'ye katıldı  | Windows 10                       | Hibrit Azure AD'ye katılmış |

Azure AD'ye bağlı veya kayıtlı bir aygıtta, kullanıcı kimlik doğrulaması modern OAuth / OpenID Connect tabanlı protokolleri kullanarak gerçekleşir. Bu protokoller internet üzerinden çalışacak şekilde tasarlanmıştır, bu nedenle kullanıcıların kurumsal kaynaklara her yerden eriştiği mobil senaryolar için mükemmeldir.

Azure AD DS'nin birleştirilmiş aygıtlarıyla, uygulamalar kimlik doğrulama için Kerberos ve NTLM protokollerini kullanabilir, bu nedenle kaldırma ve kaydırma stratejisinin bir parçası olarak Azure VM'lerinde çalışmak üzere geçirilen eski uygulamaları destekleyebilir. Aşağıdaki tablo, aygıtların nasıl temsil edildiği ne kadar farklı olduğunu özetler ve kendilerini dizine karşı doğrulayabilir:

| **Nitelik**                      | **Azure AD'ye katılım**                                 | **Azure AD DS-birleştirilmiş**                                                    |
|:--------------------------------| --------------------------------------------------- | ------------------------------------------------------------------------- |
| Cihaz tarafından kontrol            | Azure AD                                            | Azure AD DS yönetilen etki alanı                                                |
| Dizinde temsil | Azure AD dizinindeki aygıt nesneleri            | Azure AD DS yönetilen etki alanındabilgisayar nesneleri                        |
| Kimlik Doğrulaması                  | OAuth / OpenID Connect tabanlı protokoller              | Kerberos ve NTLM protokolleri                                               |
| Yönetim                      | Intune gibi Mobil Cihaz Yönetimi (MDM) yazılımı | Grup İlkesi                                                              |
| Ağ                      | Internet üzerinden çalışır                             | Yönetilen etki alanının dağıtıldığı sanal ağa bağlı veya bu ağa bakılmalıdır |
| Büyük için ...                    | Son kullanıcı mobil veya masaüstü aygıtları                  | Sunucu VM'leri Azure'da dağıtıldı                                              |

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS'yi kullanmaya başlamak için, [Azure portalını kullanarak Bir Azure AD DS yönetilen etki alanı oluşturun.][tutorial-create]

<!-- INTERNAL LINKS -->
[manage-dns]: manage-dns.md
[deploy-kcd]: deploy-kcd.md
[custom-ou]: create-ou.md
[manage-gpos]: manage-group-policy.md
[tutorial-ldaps]: tutorial-configure-ldaps.md
[tutorial-create]: tutorial-create-instance.md
