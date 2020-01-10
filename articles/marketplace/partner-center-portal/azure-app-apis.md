---
title: Ticari Market 'te Azure uygulamaları için ekleme API 'SI
description: Microsoft Iş Ortağı Merkezi 'nde ticari Market 'teki Azure uygulamaları için API önkoşulları.
author: qianw211
manager: evansma
ms.author: v-qiwe
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/10/2019
ms.openlocfilehash: e0904169924aed7aeb1df5297a3a20f14cf7c6ae
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480612"
---
# <a name="api-for-onboarding-azure-apps-in-partner-center"></a>Azure uygulamalarını Iş Ortağı Merkezi 'ne ekleme API 'SI

Program aracılığıyla sorgulamak, teslimleri oluşturmak ve Azure tekliflerini yayımlamak için *Iş Ortağı Merkezi gönderme API* 'sini kullanın.  Bu API, hesabınız birçok teklifi yönetirse ve bu teklifler için gönderme işlemini otomatikleştirmek ve iyileştirmek istiyorsanız yararlıdır.

## <a name="api-prerequisites"></a>API önkoşulları

Azure ürünleri için Iş Ortağı Merkezi API 'sini kullanmak üzere ihtiyacınız olan birkaç programlı varlık vardır: 

- bir Azure Active Directory uygulaması.
- Azure Active Directory (Azure AD) erişim belirteci.

### <a name="step-1-complete-prerequisites-for-using-the-partner-center-submission-api"></a>1\. Adım: Iş Ortağı Merkezi gönderme API 'SI kullanımı için tüm önkoşulları

Iş Ortağı Merkezi gönderme API 'sini çağırmak için kod yazmaya başlamadan önce, aşağıdaki önkoşulları tamamladığınızdan emin olun.

- Siz (veya kuruluşunuzun) bir Azure AD dizinine sahip olmanız ve dizin için [genel yönetici](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) izninizin olması gerekir. Office 365 veya Microsoft 'un diğer iş hizmetlerini zaten kullanıyorsanız Azure AD dizininiz zaten var. Aksi takdirde, ek ücret ödemeden [Iş Ortağı Merkezi 'nde yeni bir Azure ad oluşturabilirsiniz](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) .

- [Bir Azure AD uygulamasını Iş Ortağı Merkezi hesabınızla ilişkilendirmeli](https://docs.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services#associate-an-azure-ad-application-with-your-windows-partner-center-account) ve kiracı kimliğinizi, istemci kimliğini ve anahtarınızı edinmeniz gerekir. Microsoft Store gönderme API 'SI çağrılarında kullanacağınız bir Azure AD erişim belirteci edinmek için bu değerlere ihtiyacınız vardır.

#### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Azure AD uygulamasını Iş Ortağı Merkezi hesabınızla ilişkilendirme

Microsoft Store gönderimi API 'sini kullanmak için, Iş Ortağı Merkezi hesabınızla bir Azure AD uygulaması ilişkilendirmeniz, uygulamanın kiracı KIMLIĞINI ve istemci KIMLIĞINI almanız ve bir anahtar oluşturmanız gerekir. Azure AD uygulaması, Iş Ortağı Merkezi gönderme API 'sini çağırmak istediğiniz uygulamayı veya hizmeti temsil eder. API 'ye geçirdiğiniz bir Azure AD erişim belirteci edinmek için kiracı KIMLIĞI, istemci KIMLIĞI ve anahtarı gereklidir.

>[!Note]
>Bu görevi yalnızca bir kez gerçekleştirmeniz gerekir. Kiracı KIMLIĞI, istemci KIMLIĞI ve anahtarı aldıktan sonra, yeni bir Azure AD erişim belirteci oluşturmak için gereken her seferinde bunları yeniden kullanabilirsiniz.

1. Iş Ortağı Merkezi 'nde [kuruluşunuzun Iş Ortağı Merkezi hesabını kuruluşunuzun Azure AD diziniyle ilişkilendirin](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center).
1. Ardından, Iş Ortağı Merkezi 'nin **Hesap ayarları** bölümündeki **Kullanıcılar** sayfasında, iş ortağı merkezi hesabınıza yönelik Gönderimlere erişmek için kullanacağınız uygulamayı veya HIZMETI temsil eden [Azure AD uygulamasını ekleyin](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account) . Bu uygulamayı **yönetici** rolüne atadığınızdan emin olun. Uygulama henüz Azure AD dizininizde yoksa, [Iş Ortağı Merkezi 'nde yeni bir Azure AD uygulaması oluşturabilirsiniz](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).
1. **Kullanıcılar** sayfasına dönün, uygulama ayarlarına gitmek IÇIN Azure AD uygulamanızın adına tıklayın ve **kiracı KIMLIĞI** ile **istemci kimliği** değerlerini kopyalayın.
1. **Yeni anahtar Ekle**' ye tıklayın. Aşağıdaki ekranda, **anahtar** değerini kopyalayın. Bu sayfadan ayrıldıktan sonra bu bilgilere bir daha erişemeyeceksiniz. Daha fazla bilgi için bkz. [Azure AD uygulaması için anahtarları yönetme](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys).

### <a name="step-2-obtain-an-azure-ad-access-token"></a>2\. Adım: Azure AD erişim belirteci alma

Iş Ortağı Merkezi gönderme API 'sindeki yöntemlerden herhangi birini çağırabilmeniz için önce API 'deki her yöntemin **Yetkilendirme** üstbilgisine geçirdiğiniz BIR Azure AD erişim belirteci edinmeniz gerekir. Erişim belirteci elde ettikten sonra, süresi dolmadan önce kullanmanız 60 dakika sürer. Belirtecin süresi dolduktan sonra, API 'yi sonraki çağrılarında kullanmaya devam edebilmeniz için belirteci yenileyebilirsiniz.

Erişim belirtecini almak için, `https://login.microsoftonline.com/<tenant_id>/oauth2/token` uç noktasına bir `HTTP POST` göndermek üzere [Istemci kimlik bilgilerini kullanarak çağrılara hizmet vermek Için hizmet](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) talimatlarını izleyin. Örnek bir istek aşağıda verilmiştir:

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

`POST URI` ve *client_id* ve *client_secret* parametrelerinin *tenant_id* değeri Için, önceki bölümde iş ortağı merkezi 'nden ALDıĞıNıZ uygulamanızın Kiracı kimliğini, istemci kimliğini ve anahtarını belirtin. *Kaynak* parametresi için `https://api.partner.microsoft.com`belirtmeniz gerekir.

### <a name="step-3-use-the-microsoft-store-submission-api"></a>3\. Adım: Microsoft Store gönderimi API 'sini kullanma

Bir Azure AD erişim belirteciniz olduktan sonra, Iş Ortağı Merkezi gönderme API 'sindeki yöntemleri çağırabilirsiniz. Gönderimler oluşturmak veya güncelleştirmek için genellikle Iş Ortağı Merkezi gönderme API 'sinde belirli bir sırada birden çok yöntemi çağırabilirsiniz. Her senaryo ve her yöntemin sözdizimi hakkında daha fazla bilgi için bkz. alma API 'SI Swagger.

https://apidocs.microsoft.com/services/partneringestion/
