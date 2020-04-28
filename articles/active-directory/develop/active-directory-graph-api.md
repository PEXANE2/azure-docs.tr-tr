---
title: Azure Active Directory Graph API'si
description: Azure AD Graph API için bir genel bakış ve hızlı başlangıç kılavuzu, Azure AD 'ye REST API uç noktalar aracılığıyla programlı erişim sağlar.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80885727"
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API'si

> [!IMPORTANT]
> Azure Active Directory (Azure AD) kaynaklarına erişmek için Azure AD Graph API yerine [Microsoft Graph](https://developer.microsoft.com/graph) kullanmanızı kesinlikle öneririz. Geliştirme çalışmalarımız şu anda Microsoft Graph üzerine yoğunlaşmıştır ve Azure AD Graph API’si için başka bir geliştirme planlanmamaktadır. Azure AD Graph API 'nin hala uygun olabileceği çok sınırlı sayıda senaryo vardır; daha fazla bilgi için bkz. [Microsoft Graph veya Azure AD Graph](https://developer.microsoft.com/office/blogs/microsoft-graph-or-azure-ad-graph/) blog gönderisi ve [Azure ad Graph uygulamalarını Microsoft Graph 'ye geçirme](https://docs.microsoft.com/graph/migrate-azure-ad-graph-overview).

Bu makale, Azure AD Graph API için geçerlidir. Microsoft Graph API ile ilgili benzer bilgiler için, bkz. [MICROSOFT Graph API 'Sini kullanma](https://docs.microsoft.com/graph/use-the-api).

Azure Active Directory Graph API, REST API uç noktaları üzerinden Azure AD’ye program aracılığıyla erişim sağlar. Uygulamalar, dizin verileri ve nesnelerinde oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemlerini gerçekleştirmek için Azure AD Graph API kullanabilir. Örneğin, Azure AD Graph API, bir kullanıcı nesnesi için aşağıdaki yaygın işlemleri destekler:

* Dizinde yeni Kullanıcı oluşturma
* Kullanıcının grupları gibi ayrıntılı özelliklerini alın
* Bir kullanıcının, konumu ve telefon numarası gibi özelliklerini güncelleştirin veya parolalarını değiştirin
* Rol tabanlı erişim için kullanıcının grup üyeliğini denetleme
* Kullanıcının hesabını devre dışı bırakma veya tamamen silme

Ayrıca, gruplar ve uygulamalar gibi diğer nesneler üzerinde de benzer işlemler gerçekleştirebilirsiniz. Azure AD Graph API bir dizinde çağırmak için uygulamanızın Azure AD 'ye kayıtlı olması gerekir. Uygulamanıza Ayrıca Azure AD Graph API erişimi verilmelidir. Bu erişim, normalde bir kullanıcı veya yönetici onay akışı aracılığıyla gerçekleştirilir.

Azure Active Directory Graph API kullanmaya başlamak için [Azure ad Graph API hızlı başlangıç kılavuzuna](active-directory-graph-api-quickstart.md)bakın veya [etkileşimli Azure AD Graph API başvuru belgelerini](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)görüntüleyin.

## <a name="features"></a>Özellikler

Azure AD Graph API aşağıdaki özellikleri sağlar:

* **Uç noktalar REST API**: Azure AD Graph API, standart http istekleri kullanılarak erişilen uç noktalardan oluşan bir restekli hizmettir. Azure AD Graph API, istekler ve yanıtlar için XML veya JavaScript nesne gösterimi (JSON) içerik türlerini destekler. Daha fazla bilgi için bkz. [Azure AD Graph REST API başvurusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Azure AD Ile kimlik doğrulaması**: azure ad Graph API her isteğin, isteğin yetkilendirme üst bilgisine bir JSON Web token (JWT) eklenerek kimlik doğrulaması yapılmalıdır. Bu belirteç, Azure AD 'nin belirteç uç noktasına bir istek yapılarak ve geçerli kimlik bilgileri sağlanarak elde edilir. Grafiği çağırmak için bir belirteç almak üzere OAuth 2,0 istemci kimlik bilgileri akışını veya yetkilendirme kodu verme akışı ' nı kullanabilirsiniz. Daha fazla bilgi için [Azure AD 'de OAuth 2,0](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Rol tabanlı yetkilendirme (RBAC)**: güvenlik grupları, Azure AD Graph API RBAC gerçekleştirmek için kullanılır. Örneğin, bir kullanıcının belirli bir kaynağa erişimi olup olmadığını öğrenmek isterseniz, uygulama, doğru veya yanlış döndüren [denetim grubu üyeliği (geçişli)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/functions-and-actions#checkMemberGroups) işlemini çağırabilir.
* **Değişiklik sorgusu**: fark sorgusu, Azure AD Graph API için sık kullanılan sorgular yapmak zorunda kalmadan iki zaman dilimi arasında bir dizindeki değişiklikleri izlemenize olanak sağlar. Bu tür bir istek yalnızca önceki fark sorgu isteği ve geçerli istek arasında yapılan değişiklikleri döndürür. Daha fazla bilgi için bkz. [Azure AD Graph API fark sorgusu](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Dizin uzantıları**: bir dış veri deposuna gerek duymadan dizin nesnelerine özel özellikler ekleyebilirsiniz. Örneğin, uygulamanız her bir kullanıcı için bir Skype KIMLIĞI özelliği gerektiriyorsa, yeni özelliği dizine kaydedebilir ve her Kullanıcı nesnesinde kullanılabilir hale gelir. Daha fazla bilgi için bkz. [Azure AD Graph API Dizin şeması uzantıları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **İzin kapsamları tarafından güvenli hale getirilmiş**: azure ad Graph API, OAuth 2,0 kullanarak Azure AD verilerine güvenli erişim sağlayan izin kapsamlarını kullanıma sunar. Aşağıdakiler de dahil olmak üzere çeşitli istemci uygulaması türlerini destekler:
  
  * oturum açan kullanıcıdan Yetkilendirme aracılığıyla verilere temsilci erişimi verilen kullanıcı arabirimleri (atanan)
  * oturum açmış bir kullanıcı olmadan arka planda çalışan ve uygulama tanımlı rol tabanlı erişim denetimi kullanan hizmet/Daemon uygulamaları
    
    Hem temsilci hem de uygulama izinleri, Azure AD Graph API tarafından sunulan bir ayrıcalığı temsil eder ve [Azure Portal](https://portal.azure.com)uygulama kayıt izinleri özellikleri aracılığıyla istemci uygulamalar tarafından istenebilir. [Azure AD Graph API izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes) , istemci uygulamanız tarafından kullanılabilen özellikler hakkında bilgiler sağlar.

## <a name="scenarios"></a>Senaryolar

Azure AD Graph API birçok uygulama senaryosuna izin vermez. Aşağıdaki senaryolar en sık kullanılan senaryolardır:

* **Iş kolu (tek kiracılı) uygulama**: Bu senaryoda, bir kurumsal geliştirici Office 365 aboneliğine sahip bir kuruluşta çalışmaktadır. Geliştirici, bir kullanıcıya lisans atama gibi görevleri gerçekleştirmek için Azure AD ile etkileşen bir Web uygulaması oluşturuyor. Bu görevin Azure AD Graph API erişimi olması gerekir, bu nedenle geliştirici Azure AD 'ye tek kiracılı uygulamayı kaydeder ve Azure AD Graph API için okuma ve yazma izinlerini yapılandırır. Ardından, uygulama kendi kimlik bilgilerini veya şu anda oturum açma kullanıcısının Azure AD Graph API çağrısı için bir belirteç almak üzere yapılandırılmıştır.
* **Hizmet uygulaması olarak yazılım (çok kiracılı)**: Bu senaryoda, bağımsız bir yazılım SATıCıSı (ISV), Azure ad kullanan diğer kuruluşlar için kullanıcı yönetim özellikleri sağlayan bir barındırılan çok kiracılı web uygulaması geliştirmektir. Bu özelliklerin dizin nesnelerine erişmesi gerekir, böylece uygulama Azure AD Graph API çağırmalıdır. Geliştirici uygulamayı Azure AD 'ye kaydeder, Azure AD Graph API için okuma ve yazma izinleri gerektirecek şekilde yapılandırır ve diğer kuruluşların bu uygulamayı kendi dizinlerinde kullanmayı kabul edebilmesi için dış erişimi sağlar. Başka bir kuruluştaki bir Kullanıcı uygulamayı ilk kez doğruladığında, uygulamanın istediği izinlerle bir onay iletişim kutusu gösterilir. Onay verilmesi, uygulamayı kullanıcının dizininde Azure AD Graph API için istenen izinleri verecektir. Onay çerçevesi hakkında daha fazla bilgi için bkz. [onay çerçevesine genel bakış](consent-framework.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Active Directory Graph API kullanmaya başlamak için aşağıdaki konulara bakın:

* [Azure AD Graph API hızlı başlangıç kılavuzu](active-directory-graph-api-quickstart.md)
* [Azure AD Graph REST belgeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
