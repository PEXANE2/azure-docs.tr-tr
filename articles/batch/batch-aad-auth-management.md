---
title: Toplu İşlem Yönetimi çözümlerinin kimliğini doğrulamak için Azure Active Directory'yi kullanın
description: Toplu İşlem Yönetimi .NET kitaplığını kullanan uygulamalardan kimlik doğrulaması yapmak için Azure Active Directory'yi kullanarak keşfedin.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: labrenne
ms.openlocfilehash: 5c217971bd213c97a2ee31a0a1f513b601d14df9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79472988"
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Active Directory ile Toplu Yönetim çözümlerini doğrula

Azure Toplu İş Yönetimi hizmetini arayan uygulamalar [Azure Etkin Dizin][aad_about] (Azure AD) ile kimlik doğrulaması olur. Azure AD, Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure, müşterilerinin, hizmet yöneticilerinin ve kuruluş kullanıcılarının kimlik doğrulaması için Azure AD'yi kullanır.

Toplu İşlem Yönetimi .NET kitaplığı, Toplu Iş hesapları, hesap anahtarları, uygulamalar ve uygulama paketleriyle çalışmak için türleri ortaya çıkarır. Toplu İşlem Yönetimi .NET kitaplığı bir Azure kaynak sağlayıcısı istemcisidir ve bu kaynakları programlı bir şekilde yönetmek için [Azure Kaynak Yöneticisi][resman_overview] ile birlikte kullanılır. Toplu İşlem Yönetimi .NET kitaplığı ve [Azure Kaynak Yöneticisi][resman_overview]de dahil olmak üzere herhangi bir Azure kaynak sağlayıcısı istemcisi aracılığıyla yapılan istekleri doğrulamak için Azure AD'nin gerçeklemesi gerekir.

Bu makalede, Toplu İşlem Yönetimi .NET kitaplığını kullanan uygulamalardan kimlik doğrulaması yapmak için Azure AD'yi kullanmayı araştırıyoruz. Azure AD'nin, tümleşik kimlik doğrulamasını kullanarak bir abonelik yöneticisinin veya yardımcı yöneticinin kimliğini doğrulamak için nasıl kullanılacağını gösteririz. Toplu İşlem Yönetimi .NET kitaplığıyla Azure AD'yi kullanmak için GitHub'da bulunan [AccountManagement][acct_mgmt_sample] örnek projesini kullanıyoruz.

Toplu İşlem Yönetimi .NET kitaplığını ve Hesap Yönetimi örneğini kullanma hakkında daha fazla bilgi edinmek [için ,.NET için Toplu Yönetim istemci kitaplığı yla Toplu İş hesaplarını ve kotalarını yönet'e](batch-management-dotnet.md)bakın.

## <a name="register-your-application-with-azure-ad"></a>Uygulamanızı Azure AD ile kaydedin

Azure [Active Directory Authentication Library][aad_adal] (ADAL), uygulamalarınızda kullanılmak üzere Azure AD'ye programlı bir arabirim sağlar. Uygulamanızdan ADAL'ı aramak için, başvurunuzu bir Azure AD kiracısına kaydettirmeniz gerekir. Uygulamanızı kaydettirdiğinizde, Azure AD kiracısı içinde bir ad da dahil olmak üzere uygulamanızla ilgili bilgileri Azure AD'ye savurabilirsiniz. Azure AD, uygulamanızı çalışma zamanında Azure AD ile ilişkilendirmek için kullandığınız bir uygulama kimliği sağlar. Uygulama kimliği hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesneleri'ne](../active-directory/develop/app-objects-and-service-principals.md)bakın.

Hesap Yönetimi örnek uygulamasını kaydetmek için, [Uygulamaları Azure Etkin Dizini ile Tümleştirme'de][aad_integrate]Uygulama [Ekleme](../active-directory/develop/quickstart-register-app.md) bölümündeki adımları izleyin. Uygulama türü için **Yerel İstemci Uygulamasını** belirtin. **Redirect URI** için endüstri standardı OAuth 2.0 URI olduğunu. `urn:ietf:wg:oauth:2.0:oob` Ancak, gerçek bir bitiş noktası `http://myaccountmanagementsample`olması gerekmemektedir, çünkü **Redirect URI**için geçerli URI (gibi) belirtebilirsiniz:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

Kayıt işlemini tamamladıktan sonra, başvuru kimliğini ve başvurunuz için listelenen nesne (hizmet sorumlusu) kimliğini görürsünüz.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-the-azure-resource-manager-api-access-to-your-application"></a>Azure Kaynak Yöneticisi API erişimini uygulamanız için kullanma

Ardından, uygulamanıza erişimi Azure Kaynak Yöneticisi API'sine devretmeniz gerekir. Kaynak Yöneticisi API'si için Azure AD tanımlayıcısı **Windows Azure Hizmet Yönetimi API'sidir.**

Azure portalında şu adımları izleyin:

1. Azure portalının sol daki gezinti bölmesinde **Tüm hizmetler'i**seçin , **Uygulama Kayıtları'nı**tıklatın ve **Ekle'yi**tıklatın.
2. Uygulama kayıtları listesinde uygulamanızın adını arayın:

    ![Başvuru adınızı arayın](./media/batch-aad-auth-management/search-app-registration.png)

3. **Ayarlar** bıçağını görüntüleyin. **API Access** **bölümünde, Gerekli izinleri**seçin.
4. Yeni bir gerekli izin eklemek için **Ekle'yi** tıklatın. 
5. 1. **adımda, Windows Azure Hizmet Yönetimi API'sini**girin, sonuçlar listesinden bu API'yi seçin ve **Seç** düğmesini tıklatın.
6. 2. adımda, **kuruluş kullanıcıları olarak Azure klasik dağıtım modeline erişin**yanındaki onay kutusunu seçin ve **Seç** düğmesini tıklatın.
7. **Bitti** düğmesini tıklatın.

**Gerekli İzinler** bıçağı artık başvurunuziçin izinlerin hem ADAL hem de Kaynak Yöneticisi API'lerine verildiğini gösterir. Uygulamanızı Azure AD'ye ilk kaydettiğinizde varsayılan olarak ADAL'a izin verilir.

![Azure Kaynak Yöneticisi API'sine temsilci izinleri](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Azure AD bitiş noktaları

Toplu İşlem Yönetimi çözümlerinizi Azure AD ile doğrulamak için iki tanınmış uç noktaya ihtiyacınız vardır.

- **Azure AD ortak bitiş noktası,** tümleşik kimlik doğrulama durumunda olduğu gibi belirli bir kiracı sağlanmadığında genel bir kimlik bilgisi toplama arabirimi sağlar:

    `https://login.microsoftonline.com/common`

- **Azure Kaynak Yöneticisi bitiş noktası,** Toplu Iş yönetim hizmetindeki istekleri doğrulamak için bir belirteç elde etmek için kullanılır:

    `https://management.core.windows.net/`

Hesap Yönetimi örnek uygulaması, bu uç noktaların sabitlerini tanımlar. Bu sabitleri değişmeden bırakın:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Başvuru kimliğinizi başvurun 

İstemci uygulamanız çalışma zamanında Azure AD'ye erişmek için uygulama kimliğini (istemci kimliği olarak da adlandırılır) kullanır. Uygulamanızı Azure portalına kaydettikten sonra, kayıtlı uygulamanız için Azure AD tarafından sağlanan uygulama kimliğini kullanmak için kodunuzu güncelleştirin. AccountManagement örnek uygulamasında, uygulama kimliğinizi Azure portalından uygun sabite kopyalayın:

```csharp
// Specify the unique identifier (the "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access the Microsoft Graph API. For information
// about registering an application in Azure Active Directory, please see "Register an application with the Microsoft identity platform" here:
// https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app
private const string ClientId = "<application-id>";
```
Ayrıca, kayıt işlemi sırasında belirttiğiniz yeniden yönlendirme URI'yi kopyalayın. Kodunuzda belirtilen yeniden yönlendirme URI, uygulamayı kaydettirdiğinizde sağladığınız yeniden yönlendirme URI ile eşleşmelidir.

```csharp
// The URI to which Azure AD will redirect in response to an OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need to be a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Azure AD kimlik doğrulama belirteci edinme

Hesap Yönetimi örneğini Azure AD kiracısına kaydettirdikten ve örnek kaynak kodunu değerlerinizle güncelleştirdikten sonra, örnek Azure AD'yi kullanarak kimlik doğrulamaya hazırdır. Örneği çalıştırdığınızda, ADAL bir kimlik doğrulama belirteci elde etmeye çalışır. Bu adımda, Microsoft kimlik bilgileriniz için sizi ister: 

```csharp
// Obtain an access token using the "common" AAD resource. This allows the application
// to query AAD for information that lies outside the application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

Kimlik bilgilerinizi sağladıktan sonra, örnek uygulama kimlik doğrulama isteklerini Toplu İşlem yönetimi hizmetine vermeye devam edebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Hesap Yönetimi örnek [uygulamasının][acct_mgmt_sample]çalıştırılmı hakkında daha fazla bilgi [için ,.NET için Toplu Yönetim istemci kitaplığı ile Toplu İş hesaplarını ve kotaları yönet'e](batch-management-dotnet.md)bakın.

Azure AD hakkında daha fazla bilgi edinmek için [Azure Etkin Dizin Belgeleri'ne](https://docs.microsoft.com/azure/active-directory/)bakın. Azure [Kod Örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory) kitaplığında ADAL'ın nasıl kullanılacağını gösteren ayrıntılı örnekler mevcuttur.

Azure AD kullanarak Toplu Iş hizmeti uygulamalarının kimliğini doğrulamak için [Active Directory ile Toplu İş Ii Doğrula hizmet çözümlerini doğrulayın.](batch-aad-auth.md) 


[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Azure Etkin Dizin nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]:../active-directory/develop/authentication-scenarios.md "Azure AD için Kimlik Doğrulama Senaryoları"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Uygulamaları Azure Etkin Dizini ile Tümleştirme"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: https://portal.azure.com
[resman_overview]: ../azure-resource-manager/management/overview.md
