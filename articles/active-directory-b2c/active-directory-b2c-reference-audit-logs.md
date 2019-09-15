---
title: Azure Active Directory B2C 'de denetim günlükleri örnekleri ve tanımları
description: Azure AD B2C denetim günlüklerine erişirken kılavuz ve örnekler.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 09/14/2019
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: cbb748e9856b6de9004d57e4393e205ddfcfffb4
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2019
ms.locfileid: "70998843"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Azure AD B2C denetim günlüklerine erişme

Azure Active Directory B2C (Azure AD B2C) B2C kaynakları, belirteçleri verilen belirteçler ve yönetici erişimi hakkındaki etkinlik bilgilerini içeren denetim günlüklerini yayar. Bu makalede, denetim günlüklerinde bulunan bilgilere kısa bir genel bakış ve Azure AD B2C kiracınız için bu verilere nasıl erişecağınız hakkında yönergeler sunulmaktadır.

Denetim günlüğü olayları yalnızca **yedi gün**boyunca tutulur. Daha uzun bir bekletme dönemi istiyorsanız, aşağıda gösterilen yöntemlerden birini kullanarak günlüklerinizi indirmeyi ve depolamayı planlayın.

> [!NOTE]
> Azure portal **Azure Active Directory** veya **Azure AD B2C** sayfaların **Kullanıcılar** bölümü altında bireysel Azure AD B2C uygulamaları için Kullanıcı oturum açma işlemlerini göremezsiniz. Oturum açma olayları Kullanıcı etkinliğini gösterir, ancak kullanıcının oturum açmadığı B2C uygulamasına geri alınamaz. Bu makalenin ilerleyen bölümlerinde açıklandığı gibi, Denetim günlüklerini bu şekilde kullanmanız gerekir.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Denetim günlüklerinin B2C kategorisinde bulunan etkinliklere genel bakış

Denetim günlüklerinde **B2C** kategorisi aşağıdaki etkinlik türlerini içerir:

|Etkinlik türü |Açıklama  |
|---------|---------|
|Authorization |Bir kullanıcının B2C kaynaklarına (örneğin, B2C ilkeleri listesine erişen bir yönetici) erişmesine yönelik yetkilendirmeyle ilgili etkinlikler.         |
|Dizin |Bir yönetici Azure portal kullanarak oturum açtığında alınan dizin öznitelikleriyle ilgili etkinlikler. |
|Uygulama | B2C uygulamalarında oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri. |
|Anahtar |B2C anahtar kapsayıcısında depolanan anahtarlarla CRUD işlemleri. |
|Resource |B2C kaynaklarında CRUD işlemleri. Örneğin, ilkeler ve kimlik sağlayıcıları.
|Authentication |Kullanıcı kimlik bilgilerinin ve belirteç verme 'nin doğrulanması.|

Kullanıcı nesnesi CRUD etkinlikleri için **çekirdek Dizin** kategorisine bakın.

## <a name="example-activity"></a>Örnek etkinlik

Bu örnek Azure portal görüntüsü, bir Kullanıcı bir dış kimlik sağlayıcısıyla oturum açtığında yakalanan verileri gösterir, bu durumda Facebook:

![Azure portal içindeki denetim günlüğü etkinlik ayrıntıları sayfası örneği](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

Etkinlik ayrıntıları paneli aşağıdaki ilgili bilgileri içerir:

|`Section`|Alan|Açıklama|
|-------|-----|-----------|
| Etkinlik | Name | Hangi etkinlik gerçekleşti. Örneğin, *uygulamaya bir id_token*gönderin ve bu, gerçek Kullanıcı oturum açma sonucuna işaret ediyor. |
| Başlatan (aktör) | ObjectId | Kullanıcının oturum açtığı B2C uygulamasının **nesne kimliği** . Bu tanımlayıcı Azure portal görünmez, ancak Microsoft Graph API 'SI aracılığıyla erişilebilir. |
| Başlatan (aktör) | Spn | Kullanıcının oturum açtığı B2C uygulamasının **uygulama kimliği** . |
| Hedef (ler) | ObjectId | Oturum açan kullanıcının **nesne kimliği** . |
| Ek Ayrıntılar | TenantId | Azure AD B2C kiracının **KIRACı kimliği** . |
| Ek Ayrıntılar | `PolicyId` | Kullanıcının oturumu açmak için kullanılan Kullanıcı akışının (ilke) **Ilke kimliği** . |
| Ek Ayrıntılar | ApplicationId | Kullanıcının oturum açtığı B2C uygulamasının **uygulama kimliği** . |

## <a name="view-audit-logs-in-the-azure-portal"></a>Azure portal denetim günlüklerini görüntüleme

Azure portal, Azure AD B2C kiracınızdaki denetim günlüğü olaylarına erişim sağlar.

1. [Azure portalda](https://portal.azure.com) oturum açma
1. Azure AD B2C kiracınızı içeren dizine geçin ve **Azure AD B2C**gidin.
1. Sol menüdeki **Etkinlikler** ' in altında, **Denetim günlükleri**' ni seçin.

Son yedi gün içinde günlüğe kaydedilen etkinlik olaylarının listesi görüntülenir.

![Azure portal iki etkinlik olayına sahip örnek filtre](media/active-directory-b2c-reference-audit-logs/audit-logs-example-filter.png)

Aşağıdakiler dahil olmak üzere çeşitli filtreleme seçenekleri mevcuttur:

* **Etkinlik kaynak türü** - [kullanılabilir etkinlikler bölümüne genel bakış](#overview-of-activities-available-in-the-b2c-category-of-audit-logs) bölümünde tabloda gösterilen etkinlik türlerine göre filtreleyin.
* **Tarih** -gösterilen etkinliklerin tarih aralığını filtreleyin.

Listeden bir satır seçerseniz, olay için etkinlik ayrıntıları görüntülenir.

Etkinlik olaylarının listesini bir virgülle ayrılmış değerler (CSV) dosyasında indirmek için **İndir**' i seçin.

## <a name="get-audit-logs-with-the-azure-ad-reporting-api"></a>Azure AD Raporlama API 'SI ile denetim günlüklerini alın

Denetim günlükleri, Azure Active Directory için diğer etkinliklerle aynı işlem hattına yayımlanır, bu nedenle [Azure Active Directory Raporlama API 'si](https://docs.microsoft.com/graph/api/directoryaudit-list)üzerinden erişilebilirler. Daha fazla bilgi için bkz. [Azure Active Directory Raporlama API 'si ile çalışmaya başlama](../active-directory/reports-monitoring/concept-reporting-api.md).

### <a name="enable-reporting-api-access"></a>Raporlama API 'SI erişimini etkinleştir

Azure AD Raporlama API 'sine betik veya uygulama tabanlı erişime izin vermek için, aşağıdaki API izinleriyle Azure AD B2C kiracınızda kayıtlı bir Azure Active Directory uygulamanız olması gerekir:

* Microsoft Graph
  * Uygulama: Tüm denetim günlüğü verilerini okuyun

Bu izinleri B2C kiracınızda var olan bir uygulama kaydında etkinleştirebilir veya bir özel olarak denetim günlüğü otomasyonu ile kullanmak için yeni bir tane oluşturabilirsiniz.

Yeni bir uygulama oluşturmak, gerekli API izinlerini atamak ve bir istemci parolası oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Uygulamayı kaydet
    1. [Azure Portal](https://portal.azure.com)oturum açın, Azure AD B2C kiracınızı içeren dizine geçin ve **Azure AD B2C**gidin.
    1. Sol menüdeki **Yönet** ' in altında **uygulama kayıtları (eski)** seçeneğini belirleyin.
    1. **Yeni uygulama kaydı** seçin
    1. Uygulama için bir ad girin. Örneğin, *Denetim günlüğü uygulaması*.
    1. **Oturum açma URL**'sinde GEÇERLI bir URL girin. Örneğin, *https://localhost* . Bu uç noktanın erişilebilir olması gerekmez, ancak geçerli bir URL olması gerekir.
    1. **Oluştur**’u seçin.
    1. **Kayıtlı uygulama** sayfasında görüntülenen **uygulama kimliğini** kaydedin. Daha sonraki bir bölümde gösterilen örnek PowerShell betiği gibi Otomasyon betiklerinin kimlik doğrulaması için bu değere ihtiyacınız vardır.
1. API erişim izinleri atama
    1. **Kayıtlı uygulamaya** Genel Bakış sayfasında, **Ayarlar**' ı seçin.
    1. **API erişimi**altında **gerekli izinler**' i seçin.
    1. **Ekle**' yi ve ardından **bir API 'yi seçin**.
    1. **Microsoft Graph**' yi seçin ve ardından öğesini **seçin**.
    1. **Uygulama izinleri**altında **tüm denetim günlüğü verilerini oku**' nı seçin.
    1. **Seç** düğmesini seçin ve **bitti**' yi seçin.
    1. Seçin **izinleri verin**ve ardından **Evet**.
1. İstemci parolası oluştur
    1. **API erişimi**altında **anahtarlar**' ı seçin.
    1. **Anahtar açıklaması** kutusuna anahtar için bir açıklama girin. Örneğin, *Denetim günlüğü anahtarı*.
    1. Bir geçerlilik **süresi**seçin ve ardından **Kaydet**' i seçin.
    1. Anahtarın **değerini**kaydedin. Daha sonraki bir bölümde gösterilen örnek PowerShell betiği gibi Otomasyon betiklerinin kimlik doğrulaması için bu değere ihtiyacınız vardır.

Artık gerekli API erişimi, uygulama KIMLIĞI ve otomasyon betiklerinizde kullanabileceğiniz bir anahtar içeren bir uygulamanız var. Bir komut dosyasıyla etkinlik olaylarını nasıl alabileceğiniz hakkında bir örnek için bu makalenin ilerleyen kısımlarında bulunan PowerShell betiği bölümüne bakın.

### <a name="access-the-api"></a>API 'ye erişme

API aracılığıyla Azure AD B2C denetim günlüğü olaylarını indirmek için, `B2C` kategorideki günlüklere filtre uygulayın. Kategoriye göre filtrelemek için, Azure AD `filter` Raporlama API uç noktasını çağırdığınızda sorgu dizesi parametresini kullanın.

```HTTP
https://graph.microsoft.com/v1.0/auditLogs/directoryAudits?$filter=loggedByService eq 'B2C' and activityDateTime gt 2019-09-10T02:28:17Z
```

### <a name="powershell-script"></a>PowerShell betiği

Aşağıdaki PowerShell betiği, Azure AD Raporlama API 'sinin nasıl sorgulanalınacağını gösteren bir örnek gösterir. API sorgulandıktan sonra, günlüğe kaydedilen olayları standart çıktıya yazdırır, ardından JSON çıkışını bir dosyaya yazar.

Bu betiği [Azure Cloud Shell](../cloud-shell/overview.md)deneyebilirsiniz. Bunu, uygulama KIMLIĞINIZ, anahtarınız ve Azure AD B2C kiracınızın adı ile güncelleştirdiğinizden emin olun.

```powershell
# This script requires the registration of a Web Application in Azure Active Directory:
# https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-reporting-api

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a GUID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client secret/key
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # Insert your Azure AD B2C tenant; for example, contoso.onmicrosoft.com
$loginURL       = "https://login.microsoftonline.com"
$resource       = "https://graph.microsoft.com"           # Microsoft Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = "https://graph.microsoft.com/v1.0/auditLogs/directoryAudits?`$filter=loggedByService eq 'B2C' and activityDateTime gt  " + $7daysago

    # loop through each query page (1 through n)
    Do {
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```

Makalenin önceki kısımlarında gösterilen örnek etkinlik olayının JSON temsili aşağıda verilmiştir:

```JSON
{
    "id": "B2C_DQO3J_4984536",
    "category": "Authentication",
    "correlationId": "00000000-0000-0000-0000-000000000000",
    "result": "success",
    "resultReason": "N/A",
    "activityDisplayName": "Issue an id_token to the application",
    "activityDateTime": "2019-09-14T18:13:17.0618117Z",
    "loggedByService": "B2C",
    "operationType": "",
    "initiatedBy": {
        "user": null,
        "app": {
            "appId": "00000000-0000-0000-0000-000000000000",
            "displayName": null,
            "servicePrincipalId": null,
            "servicePrincipalName": "00000000-0000-0000-0000-000000000000"
        }
    },
    "targetResources": [
        {
            "id": "00000000-0000-0000-0000-000000000000",
            "displayName": null,
            "type": "User",
            "userPrincipalName": null,
            "groupType": null,
            "modifiedProperties": []
        }
    ],
    "additionalDetails": [
        {
            "key": "TenantId",
            "value": "test.onmicrosoft.com"
        },
        {
            "key": "PolicyId",
            "value": "B2C_1A_signup_signin"
        },
        {
            "key": "ApplicationId",
            "value": "00000000-0000-0000-0000-000000000000"
        },
        {
            "key": "Client",
            "value": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36"
        },
        {
            "key": "IdentityProviderName",
            "value": "facebook"
        },
        {
            "key": "IdentityProviderApplicationId",
            "value": "0000000000000000"
        },
        {
            "key": "ClientIpAddress",
            "value": "127.0.0.1"
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Diğer yönetim görevlerini otomatikleştirebilir, örneğin, [.NET ile kullanıcıları yönetebilirsiniz](active-directory-b2c-devquickstarts-graph-dotnet.md).
