---
title: Azure AD'de hizmet & uygulamalar | Azure
titleSuffix: Microsoft identity platform
description: Azure Active Directory'de uygulama ve hizmet temel nesneleri arasındaki ilişki hakkında bilgi edinin.
author: rwike77
manager: CelesteDG
services: active-directory
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/13/2019
ms.author: ryanwi
ms.custom: aaddev, identityplatformtop40
ms.reviewer: sureshja
ms.openlocfilehash: 19085346fb5797245c9f71911f8178df0a1b742a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263016"
---
# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Azure Active Directory'deki uygulama ve hizmet sorumlusu nesneleri

Bazen, Azure Etkin Dizin (Azure AD) bağlamında kullanıldığında "uygulama" teriminin anlamı yanlış anlaşılabilir. Bu makalede, [çok kiracılı](developer-glossary.md#multi-tenant-application)bir uygulama için kayıt ve onay bir örnek ile, Azure AD uygulama entegrasyonu kavramsal ve somut yönlerini açıklar.

## <a name="overview"></a>Genel Bakış

Azure AD ile tümleşik olan bir uygulamanın yazılım yönünün ötesine geçmesine ilişkin etkileri vardır. "Uygulama", yalnızca uygulama yazılımına değil, aynı zamanda Azure AD kaydına ve çalışma zamanında kimlik doğrulama/yetkilendirme "konuşmaları"ndaki rolüne de atıfta bulunarak kavramsal bir terim olarak sıklıkla kullanılır.

Tanım olarak, bir uygulama bu rollerde çalışabilir:

- [İstemci](developer-glossary.md#client-application) rolü (kaynak tüketen)
- [Kaynak sunucusu](developer-glossary.md#resource-server) rolü (istemcilere API'lerin açığa çıkarılması)
- Hem istemci rolü hem de kaynak sunucusu rolü

[OAuth 2.0 Yetkilendirme Hibesi akışı,](developer-glossary.md#authorization-grant) istemcinin/kaynağın sırasıyla bir kaynağın verilerine erişmesine/korumasına olanak tanıyan konuşma protokolünü tanımlar.

Aşağıdaki bölümlerde, Azure AD uygulama modelinin tasarım zamanı ve çalışma zamanında bir uygulamayı nasıl temsil ettiğinizi görürsünüz.

## <a name="application-registration"></a>Uygulama kaydı

[Azure portalına][AZURE-Portal]bir Azure REKLAM uygulaması kaydettiğinizde, Azure AD kiracınızda iki nesne oluşturulur:

- Bir uygulama nesnesi ve
- Hizmet sorumlusu nesnesi

### <a name="application-object"></a>Uygulama nesnesi

Azure AD uygulaması, uygulamanın "ev" kiracıolarak bilinen, uygulamanın kayıtlı olduğu Azure AD kiracısında bulunan tek ve tek uygulama nesnesi tarafından tanımlanır. Microsoft Graph [Application varlığı,][MS-Graph-App-Entity] bir uygulama nesnesinin özellikleri için şema tanımlar.

### <a name="service-principal-object"></a>Hizmet temel nesnesi

Azure AD kiracıtarafından güvence altına alınarak güvenli kaynaklara erişmek için, erişim gerektiren varlığın bir güvenlik sorumlusu tarafından temsil edilmesi gerekir. Bu, hem kullanıcılar (kullanıcı anabilim) hem de uygulamalar (hizmet sorumlusu) için geçerlidir.

Güvenlik ilkesi, Azure AD kiracısında kullanıcı/uygulama için erişim ilkesini ve izinlerini tanımlar. Bu, oturum açma sırasında kullanıcının/uygulamanın kimlik doğrulaması ve kaynak erişimi sırasında yetkilendirme gibi temel özellikleri sağlar.

Bir uygulamaya kiracıdaki kaynaklara erişim izni verildiğinde (kayıt veya [rıza](developer-glossary.md#consent)üzerine), bir hizmet temel nesnesi oluşturulur. Microsoft Graph [ServicePrincipal varlığı,][MS-Graph-Sp-Entity] bir hizmet ana nesnesinin özellikleri için şema tanımlar.

### <a name="application-and-service-principal-relationship"></a>Uygulama ve hizmet sorumlusu ilişkisi

Uygulama nesnesini, başvurunuzun tüm kiracılar arasında kullanılmak üzere *genel* gösterimi ve hizmet ilkesini belirli bir kiracıda kullanılmak üzere *yerel* temsilcilik olarak düşünün.

Uygulama nesnesi, buna karşılık gelen hizmet sorumlusu nesnelerini oluştururken kullanılmak üzere ortak ve varsayılan özelliklerin *türetildiği* şablon görevi görür. Bu nedenle bir uygulama nesnesinin yazılım uygulamasıyla 1:1 ilişkisi ve karşılık gelen hizmet ana nesnesi(ler) ile 1:01 ilişkisi vardır.

Uygulamanın kullanıldığı her kiracıda bir hizmet ilkesi oluşturularak, oturum açma ve/veya kiracı tarafından güvence altına alınarak kaynaklara erişim için bir kimlik oluşturması sağlanmalıdır. Tek kiracı uygulamasının (ana kiracısında) uygulama kaydı sırasında kullanılmak için oluşturulan ve onaylanan tek bir hizmet sorumlusu vardır. Çok kiracılı bir Web uygulaması/API'si, her kiracıdan bir kullanıcının kullanımına onay verdiği her kiracıda oluşturulan bir hizmet ilkesine de sahiptir.

> [!NOTE]
> Uygulama nesnenizde yaptığınız değişiklikler, uygulamanın yalnızca ev kiracısındaki (kayıtlı olduğu kiracı) hizmet ana nesnesine de yansıtılır. Çok kiracılı uygulamalarda, erişim [Uygulama Erişim Paneli](https://myapps.microsoft.com) aracılığıyla kaldırılıp yeniden verilene kadar, uygulama nesnesindeki değişiklikler hiçbir tüketici kiracısının hizmet ana nesnelerine yansıtılmaz.
>
> Ayrıca, yerel uygulamaların varsayılan olarak çok kiracı olarak kaydolduğunu da unutmayın.

## <a name="example"></a>Örnek

Aşağıdaki diyagram, **İk uygulaması**adı verilen örnek çok kiracılı bir uygulama bağlamında, bir uygulamanın uygulama nesnesi ile ilgili hizmet temel nesneleri arasındaki ilişkiyi göstermektedir. Bu örnek senaryoda üç Azure AD kiracısı vardır:

- **Adatum** - **İk uygulamasını** geliştiren şirket tarafından kullanılan kiracı
- **Contoso** - **İk uygulamasının** tüketicisi olan Contoso organizasyonu tarafından kullanılan kiracı
- **Fabrikam** - **Fabrikam** organizasyonunda kullanılan ve İk uygulamasını da tüketen kiracı

![Uygulama nesnesi ve hizmet ana nesnesi arasındaki ilişki](./media/app-objects-and-service-principals/application-objects-relationship.svg)

Bu örnek senaryoda:

| Adım | Açıklama |
|------|-------------|
| 1    | Uygulamanın ev kiracı uygulama ve hizmet ana nesneleri oluşturma işlemidir. |
| 2    | Contoso ve Fabrikam yöneticileri onayı tamamladığında, şirketinin Azure AD kiracısında bir hizmet temel nesnesi oluşturulur ve yöneticinin verdiği izinleri atanır. Ayrıca İk uygulamasının, kullanıcıların bireysel kullanım için rızasını sağlayacak şekilde yapılandırılabileceğini/tasarlanabileceğini de unutmayın. |
| 3    | İk uygulamasının (Contoso ve Fabrikam) tüketici kiracılarının her birinin kendi hizmet temel nesnesi vardır. Her biri, ilgili yöneticitarafından onaylanan izinler tarafından yönetilen çalışma zamanında uygulamanın bir örneğini kullandıklarını temsil eder. |

## <a name="next-steps"></a>Sonraki adımlar

- Hem uygulama hem de hizmet ana nesnelerini sorgulamak için [Microsoft Graph Explorer'ı](https://developer.microsoft.com/graph/graph-explorer) kullanabilirsiniz.
- Bir uygulamanın uygulama nesnesine, [Azure portalının][AZURE-Portal] uygulama bildirimi düzenleyicisi Microsoft Graph API'sını veya OData [Application varlığı][MS-Graph-App-Entity]tarafından temsil edildiği şekilde Azure [AD PowerShell cmdlets'i](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0)kullanarak erişebilirsiniz.
- Bir uygulamanın hizmet ana nesnesine, OData [ServicePrincipal varlığı][MS-Graph-Sp-Entity]tarafından temsil edildiği şekilde Microsoft Graph API veya [Azure AD PowerShell cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0)aracılığıyla erişebilirsiniz.

<!--Image references-->

<!--Reference style links -->
[MS-Graph-App-Entity]: https://docs.microsoft.com/graph/api/resources/application
[MS-Graph-Sp-Entity]: https://docs.microsoft.com/graph/api/resources/serviceprincipal
[AZURE-Portal]: https://portal.azure.com
