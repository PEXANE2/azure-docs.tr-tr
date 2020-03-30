---
title: Ticari Pazar'daki Azure uygulamaları için Onboarding API
description: Microsoft İş Ortağı Merkezi'ndeki ticari pazardaki Azure uygulamaları için API ön koşulları.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/10/2019
ms.openlocfilehash: cc4d56058ce3985ec3a1d9124ef4ec73ff6be1a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80279768"
---
# <a name="api-for-onboarding-azure-apps-in-partner-center"></a>İş Ortağı Merkezi'nde Azure uygulamalarına binme için API

Azure tekliflerini programlı bir şekilde sorgulamak, gönderiler oluşturmak ve yayımlamak için *İş Ortağı Merkezi gönderme API'sini* kullanın.  Bu API, hesabınız birçok teklifi yönetiyorsa ve bu teklifler için gönderim işlemini otomatikleştirmek ve optimize etmek istiyorsanız yararlıdır.

## <a name="api-prerequisites"></a>API ön koşullar

Azure Ürünleri Için İş Ortağı Merkezi API'sini kullanmak için ihtiyacınız olan birkaç programlı varlık vardır: 

- Bir Azure Etkin Dizin uygulaması.
- bir Azure Etkin Dizin (Azure AD) erişim jetonu.

### <a name="step-1-complete-prerequisites-for-using-the-partner-center-submission-api"></a>Adım 1: İş Ortağı Merkezi gönderme API'sini kullanmak için tam ön koşullar

İş Ortağı Merkezi gönderme API'sini aramak için kod yazmaya başlamadan önce, aşağıdaki ön koşulları tamamladığınızdan emin olun.

- Sizin (veya kuruluşunuz) bir Azure REKLAM dizinine sahip olmalısınız ve dizin için [Global yönetici](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) iznine sahip olmalısınız. Microsoft'un Office 365 veya diğer iş hizmetlerini zaten kullanıyorsanız, Azure REKLAM dizininiz zaten var. Aksi takdirde, [İş Ortağı Merkezi'nde](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) ek ücret ödemeden yeni bir Azure REKLAMı oluşturabilirsiniz.

- Bir [Azure REKLAM uygulamasını İş Ortağı Merkezi hesabınızla ilişkilendirmeli](https://docs.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services#associate-an-azure-ad-application-with-your-windows-partner-center-account) ve kiracı kimliğinizi, istemci kimliğinizi ve anahtarınızı edinmeniz gerekir. Microsoft Mağazası gönderim API'sine yapılan aramalarda kullanacağınız bir Azure AD erişim jetonunu elde etmek için bu değerlere ihtiyacınız vardır.

#### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Azure REKLAM uygulamasını İş Ortağı Merkezi hesabınızla ilişkilendirme

Microsoft Mağazası gönderim API'sini kullanmak için, bir Azure REKLAM uygulamasını İş Ortağı Merkezi hesabınızla ilişkilendirmeniz, uygulama için kiracı kimliğini ve istemci kimliğini almanız ve bir anahtar oluşturmanız gerekir. Azure AD uygulaması, İş Ortağı Merkezi gönderim API'sini aramak istediğiniz uygulamayı veya hizmeti temsil eder. API'ye geçtiğiniz bir Azure AD erişim jetonunu elde etmek için kiracı kimliğine, istemci kimliğine ve anahtarına ihtiyacınız var.

>[!Note]
>Bu görevi yalnızca bir kez gerçekleştirmeniz gerekir. Kiracı kimliği, istemci kimliği ve anahtarı aldıktan sonra, yeni bir Azure AD erişim jetonu oluşturmak için ihtiyacınız olan her zaman bunları yeniden kullanabilirsiniz.

1. İş Ortağı [Merkezi'nde, kuruluşunuzun İş Ortağı Merkezi hesabını kuruluşunuzun Azure REKLAM diziniyle ilişkilendirin.](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center)
1. Ardından, İş Ortağı Merkezi'nin **Hesap ayarları** bölümündeki **Kullanıcılar** sayfasından, İş Ortağı Merkezi hesabınıza yönelik gönderilere erişmek için kullanacağınız uygulamayı veya hizmeti temsil eden Azure AD [uygulamasını ekleyin.](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account) Bu uygulamayı **Yönetici** rolünü atadığınızdan emin olun. Uygulama Azure REKLAM dizininizde henüz yoksa, İş [Ortağı Merkezi'nde yeni bir Azure AD uygulaması oluşturabilirsiniz.](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)
1. **Kullanıcılar** sayfasına dönün, uygulama ayarlarına gitmek için Azure REKLAM uygulamanızın adını tıklatın ve **Kiracı Kimliği** ve **İstemci Kimliği** değerlerini kopyalayın.
1. **Yeni anahtar ekle'yi**tıklatın. Aşağıdaki **ekranda, Anahtar** değerini kopyalayın. Bu sayfadan ayrıldıktan sonra bu bilgilere bir daha erişemezsiniz. Daha fazla bilgi için azure [AD uygulaması için Yönet tuşlarına](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys)bakın.

### <a name="step-2-obtain-an-azure-ad-access-token"></a>Adım 2: Azure AD erişim jetonu edinin

İş Ortağı Merkezi gönderme API'sindeki yöntemlerden herhangi birini aramadan önce, öncelikle API'deki her yöntemin **Yetkilendirme** üstbilgisine geçtiğiniz bir Azure AD erişim belirteci edinmeniz gerekir. Bir erişim jetonu aldıktan sonra, süresi dolmadan önce kullanmak için 60 dakikanız vardır. Belirteç süresi dolduktan sonra, belirteci yenileyebilir, böylece ileride API'ye yapılan aramalarda kullanmaya devam edebilirsiniz.

Erişim jetonuna ulaşmak için, bitiş noktasına bir `HTTP POST` gönderme yapmak için `https://login.microsoftonline.com/<tenant_id>/oauth2/token` Müşteri Kimlik Bilgilerini Kullanarak [Hizmet Çağrılarına Hizmet](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) yönergelerini izleyin. Örnek bir istek aşağıda veda edebilirsiniz:

JSONCopy
```Json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource= https://api.partner.microsoft.com
```

client_id *tenant_id* `POST URI` ve *client_secret* parametrelerindeki *client_secret* tenant_id değeri için, önceki bölümde İş Merkezi'nden aldığınız kiracı kimliğini, istemci kimliğini ve başvurunuzun anahtarını belirtin. *Kaynak* parametresi için `https://api.partner.microsoft.com`.

### <a name="step-3-use-the-microsoft-store-submission-api"></a>Adım 3: Microsoft Mağazası gönderme API'sini kullanma

Azure AD erişim jetonuna sahip olduktan sonra, İş Ortağı Merkezi gönderme API'sında yöntemleri arayabilirsiniz. Gönderiler oluşturmak veya güncellemek için, genellikle İş Ortağı Merkezi gönderim API'sinde belirli bir sırada birden çok yöntemi çağırırsınız. Her senaryo ve her yöntemin sözdizimi hakkında bilgi için Yutma API swagger bakın.

https://apidocs.microsoft.com/services/partneringestion/
