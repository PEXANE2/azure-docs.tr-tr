---
title: Azure Etkin Dizini ile Azure Toplu İş hizmetlerinin doğrulamasını doğrulayın
description: Toplu İşlem, Toplu İşlem hizmetinden kimlik doğrulaması için Azure AD'yi destekler. Kimlik doğrulaması nın iki şekilde nasıl yapılacağını öğrenin.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
tags: ''
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 01/28/2020
ms.author: labrenne
ms.openlocfilehash: f56c05f64086ac2e98e69d6b21fae7a0a63b5006
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77019528"
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Active Directory ile Toplu servis çözümlerini doğrula

Azure Toplu İşlem, [Azure Etkin Dizin][aad_about] (Azure AD) ile kimlik doğrulamasını destekler. Azure AD, Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Azure, müşterilerinin, hizmet yöneticilerinin ve kuruluş kullanıcılarının kimliğini doğrulamak için Azure AD'yi kullanır.

Azure Toplu İş ile Azure AD kimlik doğrulaması kullanırken, kimlik doğrulaması iki şekilde yapabilirsiniz:

- Uygulama yla etkileşimde olan bir kullanıcının kimliğini doğrulamak için **tümleşik kimlik doğrulaması** kullanarak. Tümleşik kimlik doğrulaması kullanan bir uygulama, kullanıcının kimlik bilgilerini toplar ve Toplu Iş kaynaklarına erişimi doğrulamak için bu kimlik bilgilerini kullanır.
- Gözetimsiz bir uygulamanın kimliğini doğrulamak için bir **hizmet sorumlusu** kullanarak. Hizmet ilkesi, çalışma zamanında kaynaklara erişirken uygulamayı temsil etmek için uygulamanın ilke ve izinlerini tanımlar.

Azure AD hakkında daha fazla bilgi edinmek için [Azure Etkin Dizin Belgeleri'ne](https://docs.microsoft.com/azure/active-directory/)bakın.

## <a name="endpoints-for-authentication"></a>Kimlik doğrulama için uç noktalar

Toplu Iş uygulamalarının kimliğini doğrulamak için kodunuza bazı iyi bilinen uç noktaları eklemeniz gerekir.

### <a name="azure-ad-endpoint"></a>Azure AD bitiş noktası

Temel Azure AD yetkilisi bitiş noktası:

`https://login.microsoftonline.com/`

Azure AD ile kimlik doğrulaması yapmak için bu bitiş noktasını kiracı kimliğiyle birlikte (dizin kimliği) kullanırsınız. Kiracı kimliği, kimlik doğrulaması için kullanılacak Azure AD kiracısını tanımlar. Kiracı kimliğini almak için Azure [Etkin Dizininiz için kiracı kimliğini al'da](#get-the-tenant-id-for-your-active-directory)belirtilen adımları izleyin:

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> Bir hizmet ilkesi ni kullanarak kimlik doğrulaması yaptığınızda kiracıya özel bitiş noktası gereklidir. 
> 
> Tümleşik kimlik doğrulaması kullanarak kimlik doğrulaması yaptığınızda kiracıya özel bitiş noktası isteğe bağlıdır, ancak önerilir. Ancak, Azure AD ortak bitiş noktasını da kullanabilirsiniz. Ortak bitiş noktası, belirli bir kiracı sağlanmadığında genel bir kimlik bilgisi toplama arabirimi sağlar. Ortak bitiş `https://login.microsoftonline.com/common`noktası.
>
>

Azure AD bitiş noktaları hakkında daha fazla bilgi için Azure [AD için Kimlik Doğrulama Senaryoları'na][aad_auth_scenarios]bakın.

### <a name="batch-resource-endpoint"></a>Toplu kaynak bitiş noktası

Toplu **Iş hizmetinde** isteklerin kimlik doğrulamasını sağlamak için bir belirteç elde etmek için Azure Toplu Iş son noktasını kullanın:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Başvurunuzu bir kiracıya kaydedin

Azure AD'yi kimlik doğrulaması için kullanmanın ilk adımı, uygulamanızı bir Azure AD kiracısına kaydetmektir. Uygulamanızı kaydetmek, kodunuzdan Azure [Active Directory Authentication Library'yi][aad_adal] (ADAL) aramanızı sağlar. ADAL, uygulamanızdan Azure AD ile kimlik doğrulaması için bir API sağlar. Entegre kimlik doğrulaması veya hizmet ilkesi ni kullanmayı planlayıp planlamadığınıza bakılmaksızın başvurunuzu kaydetmek gereklidir.

Uygulamanızı kaydettirdiğinizde, uygulamanızla ilgili bilgileri Azure AD'ye savurabilirsiniz. Azure AD daha sonra, uygulamanızı çalışma zamanında Azure AD ile ilişkilendirmek için kullandığınız bir uygulama *kimliği (istemci kimliği*olarak da adlandırılır) sağlar. Uygulama kimliği hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesneleri'ne](../active-directory/develop/app-objects-and-service-principals.md)bakın.

Toplu İşlem uygulamanızı kaydetmek için, Uygulamaları Azure [Etkin Dizini ile Tümleştirme'de][aad_integrate] [Uygulama Ekleme](../active-directory/develop/quickstart-register-app.md) bölümündeki adımları izleyin. Başvurunuzu Yerel Uygulama olarak kaydettirürseniz, **Yeniden Yönlendirme URI**için geçerli bir URI belirtebilirsiniz. Gerçek bir bitiş noktası olması gerekmez.

Başvurunuzu kaydettikten sonra başvuru kimliğini görürsünüz:

![Toplu İşlem uygulamanızı Azure AD ile kaydedin](./media/batch-aad-auth/app-registration-data-plane.png)

Azure AD'ye bir uygulama kaydetme hakkında daha fazla bilgi için [Azure AD için Kimlik Doğrulama Senaryoları'na](../active-directory/develop/authentication-scenarios.md)bakın.

## <a name="get-the-tenant-id-for-your-active-directory"></a>Aktif Dizininiz için kiracı kimliğini alın

Kiracı kimliği, uygulamanıza kimlik doğrulama hizmetleri sağlayan Azure AD kiracısını tanımlar. Kiracı kimliğini almak için aşağıdaki adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
1. **Özellikleri**seçin.
1. **Dizin Kimliği**için sağlanan GUID değerini kopyalayın. Bu değer, kiracı kimliği olarak da adlandırılır.

![Dizin kimliğini kopyalama](./media/batch-aad-auth/aad-directory-id.png)

## <a name="use-integrated-authentication"></a>Tümleşik kimlik doğrulamayı kullanma

Tümleşik kimlik doğrulamayla kimlik doğrulaması yapmak için Toplu İşlem hizmeti API'sine bağlanmak için uygulama izni vermeniz gerekir. Bu adım, uygulamanızın Azure AD ile Toplu Iş hizmeti API'sine yapılan çağrıları doğrulamasını sağlar.

Başvurunuzu kaydettikten sonra, Toplu İşlem hizmetine erişimini sağlamak için Azure portalında aşağıdaki adımları izleyin:

1. Azure portalının sol daki gezinti bölmesinde **Tüm hizmetler'i**seçin. **Uygulama Kayıtlarını**seçin.
1. Uygulama kayıtları listesinde uygulamanızın adını arayın:

    ![Başvuru adınızı arayın](./media/batch-aad-auth/search-app-registration.png)

1. Uygulamayı seçin ve **API izinlerini**seçin.
1. **API izinleri** bölümünde, **izin ekle'yi**seçin.
1. **API seç'te**Toplu İş API'yi arayın. API'yi bulana kadar aşağıdaki dizelerden her birini arayın:
    1. **Microsoft Azure Toplu İş**
    1. Batch API'sinin kimliği: **ddbf3205-c6bd-46ae-8127-60eb93363864**.
1. Toplu İş API'yi bulduktan sonra seç ve **Seç'i**seçin.
1. **İzinleri**Seç'te, **Azure Toplu İş Hizmetine Eriş'in** yanındaki onay kutusunu seçin ve ardından İzin **Ekle'yi**seçin.

**API izinleri** bölümü artık Azure AD uygulamanızın hem Microsoft Grafiği'ne hem de Toplu İş hizmeti API'sine erişimi olduğunu gösterir. Uygulamanızı Azure AD'ye ilk kaydettiğinizde izinler Otomatik olarak Microsoft Graph'a verilir.

![Hibe API izinleri](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Hizmet sorumlusu kullanma

Gözetimsiz çalışan bir uygulamanın kimliğini doğrulamak için bir hizmet ilkesi kullanırsınız. Uygulamanızı kaydettikten sonra, bir hizmet ilkesini yapılandırmak için Azure portalında aşağıdaki adımları izleyin:

1. Başvurunuz için bir sır isteyin.
1. Uygulamanıza rol tabanlı erişim denetimi (RBAC) atayın.

### <a name="request-a-secret-for-your-application"></a>Başvurunuz için bir sır isteyin

Uygulamanız bir hizmet ilkesiyle kimlik doğruladığında, hem uygulama kimliğini hem de bir sırrı Azure AD'ye gönderir. Kodunuzda kullanmak üzere gizli anahtarı oluşturmanız ve kopyalamanız gerekir.

Azure portalında şu adımları izleyin:

1. Azure portalının sol daki gezinti bölmesinde **Tüm hizmetler'i**seçin. **Uygulama Kayıtlarını**seçin.
1. Uygulama kayıtları listesinden başvurunuzu seçin.
1. Uygulamayı seçin ve ardından **Sertifikalar & sırları**seçin. **İstemci sırları** bölümünde, **Yeni istemci sırrını**seçin.
1. Bir sır oluşturmak için, gizli için bir açıklama girin. Sonra bir yıl, iki yıl veya hiçbir sona erme sırrı için bir sona erme seçin ...
1. Sırrı oluşturmak ve görüntülemek için **Ekle'yi** seçin. Sayfadan çıktıktan sonra bir daha erişemeyeceğiniz için gizli değeri güvenli bir yere kopyalayın.

    ![Gizli bir anahtar oluşturma](./media/batch-aad-auth/secret-key.png)

### <a name="assign-rbac-to-your-application"></a>Başvurunuza RBAC atama

Bir hizmet sorumlusuyla kimlik doğrulaması yapmak için, RBAC'ı uygulamanıza atamanız gerekir. Şu adımları uygulayın:

1. Azure portalında, uygulamanız tarafından kullanılan Toplu İşlem hesabına gidin.
1. Toplu İş hesabının **Ayarlar** bölümünde **Erişim Denetimi 'ni (IAM)** seçin.
1. Rol **atamaları** sekmesini seçin.
1. **Rol ataması ekle**’yi seçin.
1. **Role** açılır bırakma görevinden, uygulamanız için *Katılımcı* veya *Okuyucu* rolünü seçin. Bu roller hakkında daha fazla bilgi için Azure [portalında Rol Tabanlı Erişim Denetimi'ne başlayın'a](../role-based-access-control/overview.md)bakın.  
1. **Seç** alanına, başvurunuzun adını girin. Uygulamanızı listeden seçin ve sonra **Kaydet'i**seçin.

Uygulamanız artık bir RBAC rolü atanmış erişim denetim ayarlarınızda görünmelidir.

![Uygulamanız için bir RBAC rolü atama](./media/batch-aad-auth/app-rbac-role.png)

### <a name="assign-a-custom-role"></a>Özel bir rolü atama

Özel bir rol, iş, görev ve daha fazlasını göndermek için kullanıcıya ayrıntılı izin verir. Bu, kullanıcıların havuz oluşturma veya düğümleri değiştirme gibi maliyeti etkileyen işlemleri gerçekleştirmesini engelleme olanağı sağlar.

Aşağıdaki RBAC işlemleri için bir Azure REKLAM kullanıcısına, grubuna veya hizmet sorumlusuna izin vermek için özel bir rol kullanabilirsiniz:

- Microsoft.Batch/batchAccounts/pools/write
- Microsoft.Batch/batchAccounts/pools/delete
- Microsoft.Batch/batchAccounts/pools/read
- Microsoft.Batch/batchAccounts/jobSchedules/write
- Microsoft.Batch/batchAccounts/jobSchedules/delete
- Microsoft.Batch/batchAccounts/jobSchedules/read
- Microsoft.Batch/batchAccounts/jobs/write
- Microsoft.Batch/batchAccounts/jobs/delete
- Microsoft.Batch/batchAccounts/jobs/read
- Microsoft.Batch/batchAccounts/certificates/write
- Microsoft.Batch/batchAccounts/certificates/delete
- Microsoft.Batch/batchAccounts/certificates/read
- Microsoft.Batch/batchAccounts/read (okunan işlem için)
- Microsoft.Batch/batchAccounts/listKeys/action (herhangi bir işlem için)

Özel roller, Toplu Iş hesabı kimlik bilgileri (paylaşılan anahtar) için değil, Azure AD tarafından kimlik doğrulanan kullanıcılar içindir. Toplu Iş hesabı kimlik bilgilerinin Toplu İşlem hesabına tam izin verdiğini unutmayın. Ayrıca, otomatik havuz kullanan işlerin havuz düzeyinde izinler gerektirdiğini de unutmayın.

Özel bir rol tanımına örnek aşağıda verilmiştir:

```json
{
 "properties":{
    "roleName":"Azure Batch Custom Job Submitter",
    "type":"CustomRole",
    "description":"Allows a user to submit jobs to Azure Batch but not manage pools",
    "assignableScopes":[
      "/subscriptions/88888888-8888-8888-8888-888888888888"
    ],
    "permissions":[
      {
        "actions":[
          "Microsoft.Batch/*/read",
          "Microsoft.Authorization/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*",
          "Microsoft.Insights/alertRules/*"
        ],
        "notActions":[

        ],
        "dataActions":[
          "Microsoft.Batch/batchAccounts/jobs/*",
          "Microsoft.Batch/batchAccounts/jobSchedules/*"
        ],
        "notDataActions":[

        ]
      }
    ]
  }
}
```

Özel bir rol oluşturma hakkında daha genel bilgi için [Azure kaynakları için Özel rolleri](../role-based-access-control/custom-roles.md)görün.

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>Azure Etkin Dizininiz için kiracı kimliğini alın

Kiracı kimliği, uygulamanıza kimlik doğrulama hizmetleri sağlayan Azure AD kiracısını tanımlar. Kiracı kimliğini almak için aşağıdaki adımları izleyin:

1. Azure portalında Active Directory'nizi seçin.
1. **Özellikleri**seçin.
1. **Dizin Kimliği**için sağlanan GUID değerini kopyalayın. Bu değer, kiracı kimliği olarak da adlandırılır.

![Dizin kimliğini kopyalama](./media/batch-aad-auth/aad-directory-id.png)

## <a name="code-examples"></a>Kod örnekleri

Bu bölümdeki kod örnekleri, tümleşik kimlik doğrulaması kullanarak ve bir hizmet ilkesiyle Azure AD ile nasıl kimlik doğrulaması yapılacağını gösterir. Bu kod örneklerinin çoğu .NET kullanır, ancak kavramlar diğer diller için benzerdir.

> [!NOTE]
> Azure AD kimlik doğrulama belirteci bir saat sonra sona erer. Uzun ömürlü bir **BatchClient** nesnesi kullanırken, her isteğe bağlı olarak adal'dan her zaman geçerli bir belirteç olduğundan emin olmak için bir belirteç almanızı öneririz. 
>
>
> Bunu .NET'te başarmak için, Azure AD'den belirteci alan bir yöntem yazın ve bu yöntemi bir **Toplu İşlemKimlik Bilgileri** nesnesine temsilci olarak geçirin. Geçerli bir belirteç sağlandığından emin olmak için Toplu İşlem hizmetine yapılan her isteğe temsilci yöntemi çağrılır. Varsayılan olarak ADAL belirteçleri önbelleğe alır, bu nedenle yeni bir belirteç yalnızca gerektiğinde Azure AD'den alınır. Azure AD'deki belirteçler hakkında daha fazla bilgi için Azure [AD için Kimlik Doğrulama Senaryoları'na][aad_auth_scenarios]bakın.
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Kod örneği: Toplu İşlem .NET ile Azure AD tümleşik kimlik doğrulamasını kullanma

Toplu Iş .NET'ten tümleşik kimlik doğrulaması yla kimlik doğrulaması yapmak için [Azure Toplu İşlem .NET](https://www.nuget.org/packages/Microsoft.Azure.Batch/) paketine ve [ADAL paketine](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) başvurun.

Kodunuzda `using` aşağıdaki ifadeleri ekleyin:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Kiracı kimliği de dahil olmak üzere kodunuzdaki Azure AD bitiş noktasına başvurun. Kiracı kimliğini almak için Azure [Etkin Dizininiz için kiracı kimliğini al'da](#get-the-tenant-id-for-your-active-directory)belirtilen adımları izleyin:

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Toplu Iş hizmeti kaynağı bitiş noktasına başvurun:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Toplu Iş hesabınıza başvurun:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Uygulamanız için uygulama kimliğini (istemci kimliği) belirtin. Uygulama kimliği, Azure portalındaki uygulama kaydınızdan edinilebilir:

```csharp
private const string ClientId = "<application-id>";
```

Ayrıca, başvurunuzu Yerel Uygulama olarak kaydettiyseniz, belirttiğiniz uri yönlendirmeyi kopyalayın. Kodunuzda belirtilen yeniden yönlendirme URI, uygulamayı kaydettirdiğinizde sağladığınız yeniden yönlendirme URI ile eşleşmelidir:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Azure AD'den kimlik doğrulama belirteci edinmek için bir geri arama yöntemi yazın. Burada gösterilen **GetAuthenticationTokenAsync** geri arama yöntemi, uygulamayla etkileşimde bulunan bir kullanıcının kimliğini doğrulamak için ADAL'ı çağırır. ADAL tarafından sağlanan **AcquireTokenAsync** yöntemi kullanıcıyı kimlik bilgilerini ister ve kullanıcı bunları sağladıktan sonra uygulama devam eder (kimlik bilgilerini zaten önbelleğe almamışsa):

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire the authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Temsilciyi parametre olarak alan bir **BatchTokenCredentials** nesnesi oluştur. **BatchClient** nesnesini açmak için bu kimlik bilgilerini kullanın. Bu **BatchClient** nesnesini Toplu İşlem hizmetine karşı sonraki işlemler için kullanabilirsiniz:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Kod örneği: Toplu İşlem .NET ile Azure REKLAM hizmeti ilkesini kullanma

Toplu İşlem .NET'ten bir servis müdürüyle kimlik doğrulaması yapmak için [Azure Toplu İşlem .NET](https://www.nuget.org/packages/Azure.Batch/) paketine ve [ADAL paketine](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) başvurun.

Kodunuzda `using` aşağıdaki ifadeleri ekleyin:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Kiracı kimliği de dahil olmak üzere kodunuzdaki Azure AD bitiş noktasına başvurun. Bir hizmet sorumlusu kullanırken, kiracıya özel bir bitiş noktası sağlamanız gerekir. Kiracı kimliğini almak için Azure [Etkin Dizininiz için kiracı kimliğini al'da](#get-the-tenant-id-for-your-active-directory)belirtilen adımları izleyin:

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Toplu Iş hizmeti kaynağı bitiş noktasına başvurun:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Toplu Iş hesabınıza başvurun:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Uygulamanız için uygulama kimliğini (istemci kimliği) belirtin. Uygulama kimliği, Azure portalındaki uygulama kaydınızdan edinilebilir:

```csharp
private const string ClientId = "<application-id>";
```

Azure portalından kopyaladığınız gizli anahtarı belirtin:

```csharp
private const string ClientKey = "<secret-key>";
```

Azure AD'den kimlik doğrulama belirteci edinmek için bir geri arama yöntemi yazın. Burada gösterilen **GetAuthenticationTokenAsync** geri arama yöntemi, katılımsız kimlik doğrulama için ADAL'ı çağırır:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Temsilciyi parametre olarak alan bir **BatchTokenCredentials** nesnesi oluştur. **BatchClient** nesnesini açmak için bu kimlik bilgilerini kullanın. Ardından, Toplu İşlem hizmetine karşı sonraki işlemler için bu **BatchClient** nesnesini kullanın:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-python"></a>Kod örneği: Toplu Python ile Azure REKLAM hizmeti ilkesini kullanma

Toplu Python'dan bir servis müdürüyle kimlik doğrulaması yapmak için [azure toplu](https://pypi.org/project/azure-batch/) ve [azure ortak](https://pypi.org/project/azure-common/) modüllerini yükleyin ve başvurun.

```python
from azure.batch import BatchServiceClient
from azure.common.credentials import ServicePrincipalCredentials
```

Bir hizmet sorumlusu kullanırken, kiracı kimliğini sağlamanız gerekir. Kiracı kimliğini almak için Azure [Etkin Dizininiz için kiracı kimliğini al'da](#get-the-tenant-id-for-your-active-directory)belirtilen adımları izleyin:

```python
TENANT_ID = "<tenant-id>"
```

Toplu Iş hizmeti kaynağı bitiş noktasına başvurun:  

```python
RESOURCE = "https://batch.core.windows.net/"
```

Toplu Iş hesabınıza başvurun:

```python
BATCH_ACCOUNT_URL = "https://myaccount.mylocation.batch.azure.com"
```

Uygulamanız için uygulama kimliğini (istemci kimliği) belirtin. Uygulama kimliği, Azure portalındaki uygulama kaydınızdan edinilebilir:

```python
CLIENT_ID = "<application-id>"
```

Azure portalından kopyaladığınız gizli anahtarı belirtin:

```python
SECRET = "<secret-key>"
```

**ServicePrincipalCredentials nesnesi** oluşturma:

```python
credentials = ServicePrincipalCredentials(
    client_id=CLIENT_ID,
    secret=SECRET,
    tenant=TENANT_ID,
    resource=RESOURCE
)
```

**BatchServiceClient** nesnesini açmak için hizmet temel kimlik bilgilerini kullanın. Ardından, Toplu İşlem hizmetine karşı sonraki işlemler için bu **BatchServiceClient** nesnesini kullanın.

```python
    batch_client = BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL
)
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD hakkında daha fazla bilgi edinmek için [Azure Etkin Dizin Belgeleri'ne](https://docs.microsoft.com/azure/active-directory/)bakın. Azure [Kod Örnekleri](https://azure.microsoft.com/resources/samples/?service=active-directory) kitaplığında ADAL'ın nasıl kullanılacağını gösteren ayrıntılı örnekler mevcuttur.

- Hizmet ilkeleri hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesnelerine](../active-directory/develop/app-objects-and-service-principals.md)bakın. Azure portalını kullanarak bir hizmet sorumlusu oluşturmak [için, kaynaklara erişebilen Etkin Dizin uygulaması ve hizmet ilkesi oluşturmak için Kullan portalına](../active-directory/develop/howto-create-service-principal-portal.md)bakın. PowerShell veya Azure CLI ile bir hizmet ilkesi de oluşturabilirsiniz.

- Azure AD'yi kullanarak Toplu İşlem Yönetimi uygulamalarının kimliğini doğrulamak için [Active Directory ile Toplu Yönetim'i Doğrula'ya verme çözümlerine](batch-aad-auth-management.md)bakın.

- Azure AD belirteci kullanılarak kimlik doğrulaması yapılan bir Toplu Iş istemcisi oluşturmanın bir Python örneği için, [Python Script örneğiyle Birlikte Azure Toplu Özel Görüntü Dağıtma](https://github.com/azurebigcompute/recipes/blob/master/Azure%20Batch/CustomImages/CustomImagePython.md) bölümüne bakın.

[aad_about]:../active-directory/fundamentals/active-directory-whatis.md "Azure Etkin Dizin nedir?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Azure AD için Kimlik Doğrulama Senaryoları"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Uygulamaları Azure Etkin Dizini ile Tümleştirme"
[azure_portal]: https://portal.azure.com
