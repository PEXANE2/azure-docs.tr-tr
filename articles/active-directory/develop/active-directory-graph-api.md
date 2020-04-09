---
title: Azure Active Directory Graph API'si
description: REST API uç noktaları aracılığıyla Azure AD'ye programlı erişim sağlayan Azure AD Graph API için genel bakış ve hızlı başlangıç kılavuzu.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/26/2019
ms.author: ryanwi
ms.reviewer: dkershaw, sureshja
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 73cdac1a372b42df5a8f52ea09f04ecc40031698
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885727"
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API'si

> [!IMPORTANT]
> Azure Active Directory (Azure AD) kaynaklarına erişmek için Azure AD Graph API yerine [Microsoft Graph'ı](https://developer.microsoft.com/graph) kullanmanızı şiddetle öneririz. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Grafiği API'sinin hala uygun olabileceği çok sınırlı sayıda senaryo vardır; daha fazla bilgi için [Microsoft Graph veya Azure AD Graph](https://developer.microsoft.com/office/blogs/microsoft-graph-or-azure-ad-graph/) blog gönderisine bakın ve Azure AD Grafiği uygulamalarını Microsoft [Graph'a geçirin.](https://docs.microsoft.com/graph/migrate-azure-ad-graph-overview)

Bu makale, Azure AD Grafiği API'si için geçerlidir. Microsoft Graph API ile ilgili benzer bilgiler için [bkz.](https://docs.microsoft.com/graph/use-the-api)

Azure Active Directory Graph API, REST API uç noktaları üzerinden Azure AD’ye program aracılığıyla erişim sağlar. Uygulamalar, dizin verileri ve nesneler üzerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini gerçekleştirmek için Azure AD Graph API'sını kullanabilir. Örneğin, Azure AD Grafiği API'si bir kullanıcı nesnesi için aşağıdaki yaygın işlemleri destekler:

* Dizinde yeni bir kullanıcı oluşturma
* Kullanıcının grupları gibi ayrıntılı özelliklerini alma
* Bir kullanıcının konumu ve telefon numarası gibi özelliklerini güncelleştirme veya parolalarını değiştirme
* Rol tabanlı erişim için kullanıcının grup üyeliğini kontrol edin
* Bir kullanıcının hesabını devre dışı veya tamamen silme

Ayrıca, gruplar ve uygulamalar gibi diğer nesneler üzerinde benzer işlemler gerçekleştirebilirsiniz. Azure AD Grafiği API'sini bir dizin üzerinde aramak için uygulamanızın Azure AD'ye kayıtlı olması gerekir. Uygulamanız ayrıca Azure AD Grafiği API'sine erişim izni verilmelidir. Bu erişim normalde bir kullanıcı veya yönetici onay akışı aracılığıyla elde edilir.

Azure Etkin Dizin Grafiği API'sini kullanmaya başlamak için [Azure AD Grafiği API hızlı başlatma kılavuzuna](active-directory-graph-api-quickstart.md)bakın veya [etkileşimli Azure AD Grafiği API başvuru belgelerini](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)görüntüleyin.

## <a name="features"></a>Özellikler

Azure AD Grafiği API'sı aşağıdaki özellikleri sağlar:

* **REST API Uç Noktaları**: Azure AD Graph API, standart HTTP istekleri kullanılarak erişilen uç noktalardan oluşan bir restful hizmetidir. Azure AD Graph API, istek ler ve yanıtlar için XML veya Javascript Nesne Gösterimi (JSON) içerik türlerini destekler. Daha fazla bilgi için Azure [AD Graph REST API başvurusuna](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)bakın.
* **Azure AD ile kimlik doğrulama**: Azure AD Grafiği API'sine yapılan her istek, isteğin Yetkilendirme üstbilgisine bir JSON Web Belirteci (JWT) ekleyerek doğrulanmalıdır. Bu belirteç, Azure AD'nin belirteç bitiş noktasına istekte bulunarak ve geçerli kimlik bilgileri sağlayarak elde edilir. OAuth 2.0 istemci kimlik bilgileri akışını veya Grafik'i aramak için bir belirteç elde etmek için yetkilendirme kodu hibe akışını kullanabilirsiniz. Daha fazla bilgi için Azure [AD'de OAuth 2.0.](https://msdn.microsoft.com/library/azure/dn645545.aspx)
* **Rol Tabanlı Yetkilendirme (RBAC)**: Azure AD Grafiği API'sinde RBAC gerçekleştirmek için güvenlik grupları kullanılır. Örneğin, bir kullanıcının belirli bir kaynağa erişimi olup olmadığını belirlemek istiyorsanız, uygulama doğru veya yanlış döndüren [Denetim grubu üyeliğini (geçişli)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) işlemi arayabilir.
* **Diferansiyel Sorgu**: Diferansiyel sorgu, Azure AD Grafiği API'sine sık sık sorgu yapmak zorunda kalmadan iki dönem arasındaki dizindeki değişiklikleri izlemenize olanak tanır. Bu istek türü yalnızca önceki diferansiyel sorgu isteği ve geçerli istek arasında yapılan değişiklikleri döndürecek. Daha fazla bilgi için Azure [AD Grafiği API diferansiyel sorgusuna](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query)bakın.
* **Dizin Uzantıları**: Harici bir veri deposuna gerek kalmadan dizin nesnelerine özel özellikler ekleyebilirsiniz. Örneğin, uygulamanız her kullanıcı için bir Skype Kimliği özelliği gerektiriyorsa, yeni özelliği dizine kaydedebilirsiniz ve her kullanıcı nesnesinde kullanılabilir. Daha fazla bilgi için Azure [AD Graph API dizini şeması uzantılarına](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions)bakın.
* **İzin kapsamları tarafından güvenli**: Azure AD Grafiği API'si, OAuth 2.0'ı kullanarak Azure AD verilerine güvenli erişim sağlayan izin kapsamlarını ortaya çıkarır. Çeşitli istemci uygulama türlerini destekler:
  
  * oturum açmış kullanıcıdan (temsilci) yetkilendirme yoluyla verilere yetki verilen kullanıcı arabirimleri
  * oturum açmış bir kullanıcı olmadan arka planda çalışan hizmet/daemon uygulamaları mevcut ve uygulama tanımlı rol tabanlı erişim kontrolü kullanmak
    
    Hem temsilci hem de uygulama izinleri Azure AD Graph API tarafından ortaya çıkarılan bir ayrıcalığı temsil eder ve [Azure portalındaki](https://portal.azure.com)uygulama kaydı izinleri özellikleri aracılığıyla istemci uygulamaları tarafından istenebilir. [Azure AD Graph API izin kapsamları,](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) istemci uygulamanız tarafından kullanılabilecek bilgiler sağlar.

## <a name="scenarios"></a>Senaryolar

Azure AD Grafiği API'sı birçok uygulama senaryosuna olanak tanır. Aşağıdaki senaryolar en yaygın olanıdır:

* **İş Hattı (Tek Kiracı) Uygulaması**: Bu senaryoda, bir kurumsal geliştirici Office 365 aboneliği olan bir kuruluş için çalışır. Geliştirici, bir kullanıcıya lisans atama gibi görevleri gerçekleştirmek için Azure AD ile etkileşimde olan bir web uygulaması oluşturuyor. Bu görev Azure AD Grafiği API'sine erişim gerektirir, bu nedenle geliştirici Azure AD'deki tek kiracı uygulamasını kaydeder ve Azure AD Graph API için okuma ve yazma izinlerini yapılandırır. Daha sonra uygulama, Azure AD Grafiği API'sini aramak üzere bir belirteç edinmek için kendi kimlik bilgilerini veya şu anda oturum açma kullanıcısının kimlik bilgilerini kullanacak şekilde yapılandırılır.
* **Hizmet Uygulaması Olarak Yazılım (Multi-Kiracı)**: Bu senaryoda, bağımsız bir yazılım satıcısı (ISV), Azure AD kullanan diğer kuruluşlar için kullanıcı yönetimi özellikleri sağlayan barındırılan çok kiracılı bir web uygulaması geliştiriyor. Bu özellikler dizin nesnelerine erişim gerektirir, bu nedenle uygulamanın Azure AD Grafiği API'sini araması gerekir. Geliştirici uygulamayı Azure AD'de kaydeder, Azure AD Graph API için okuma ve yazma izinleri gerektirecek şekilde yapılandırır ve diğer kuruluşların uygulamayı kendi dizinde kullanmaya izin verebilmesi için dış erişim sağlar. Başka bir kuruluştaki bir kullanıcı uygulamayı ilk kez doğruladığında, uygulamanın istediği izinlerle birlikte bir onay iletişim kutusu gösterilir. İzin vermek, daha sonra uygulamaya istenen izinleri kullanıcıdizinindeki Azure AD Graph API'sine verir. İzin çerçevesi hakkında daha fazla bilgi için, [bkz.](consent-framework.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Etkin Dizin Grafiği API'sini kullanmaya başlamak için aşağıdaki konulara bakın:

* [Azure AD Grafiği API hızlı başlatma kılavuzu](active-directory-graph-api-quickstart.md)
* [Azure AD Grafiği REST belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
