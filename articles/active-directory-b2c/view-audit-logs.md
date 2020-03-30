---
title: Denetim günlüklerine erişme ve inceleme
titleSuffix: Azure AD B2C
description: Azure AD B2C denetim günlüklerine programlı olarak ve Azure portalında nasıl erişilir?
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 02/20/2020
ms.author: mimart
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 83086fa2cb96eba423b9111134a0406d7256821f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264225"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Azure AD B2C denetim günlüklerine erişim

Azure Active Directory B2C (Azure AD B2C), B2C kaynakları, verilen belirteçler ve yönetici erişimi hakkında etkinlik bilgileri içeren denetim günlükleri yayar. Bu makalede, denetim günlüklerinde bulunan bilgilere ve Azure AD B2C kiracınız için bu verilere nasıl erişilene ilişkin talimatlara kısa bir genel bakış sunulmaktadır.

Denetim günlüğü olayları yalnızca **yedi gün**saklanır. Daha uzun bir saklama süresi ne gerekiyorsa, aşağıda gösterilen yöntemlerden birini kullanarak günlüklerinizi indirmeyi ve depolamayı planlayın.

> [!NOTE]
> Azure Etkin **Dizininin** **Veya** Azure **AD B2C** sayfalarının Kullanıcılar bölümünde tek tek Azure AD B2C uygulamaları için kullanıcı oturum açma larını Azure portalında göremezsiniz. Oturum açma olayları kullanıcı etkinliğini gösterir, ancak kullanıcının oturum imzaladığı B2C uygulamasıyla ilişkilendirilemez. Bu makalede daha fazla açıklandığı gibi, bunun için denetim günlüklerini kullanmanız gerekir.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Denetim günlükleri B2C kategorisinde mevcut faaliyetlere genel bakış

Denetim günlüklerinde **B2C** kategorisi aşağıdaki türde etkinlikleri içerir:

|Etkinlik türü |Açıklama  |
|---------|---------|
|Yetkilendirme |Bir kullanıcının B2C kaynaklarına erişme yetkisiyle ilgili etkinlikler (örneğin, B2C ilkelerinin listesine erişen bir yönetici).         |
|Dizin |Bir yönetici Azure portalını kullanarak imzaattığında alınan dizin öznitelikleriyle ilgili etkinlikler. |
|Uygulama | B2C uygulamalarında (CRUD) işlemleri oluşturun, okuyun, güncelleyin ve silin. |
|Anahtar |Bir B2C anahtar kabında depolanan anahtarlar üzerinde CRUD işlemleri. |
|Kaynak |B2C kaynaklarında CRUD işlemleri. Örneğin, ilkeler ve kimlik sağlayıcıları.
|Kimlik doğrulaması |Kullanıcı kimlik bilgilerinin ve belirteç vermenin doğrulanması.|

Kullanıcı nesnesi CRUD etkinlikleri için **Çekirdek Dizini** kategorisine bakın.

## <a name="example-activity"></a>Örnek etkinlik

Azure portalındaki bu örnek resim, bir kullanıcı harici bir kimlik sağlayıcısıyla giriş yaptığında yakalanan verileri gösterir, bu durumda, Facebook:

![Azure portalında Denetim Günlüğü Etkinlik Ayrıntıları sayfası örneği](./media/view-audit-logs/audit-logs-example.png)

Etkinlik ayrıntıları paneli aşağıdaki ilgili bilgileri içerir:

|Section|Alan|Açıklama|
|-------|-----|-----------|
| Etkinlik | Adı | Hangi aktivite gerçekleşti. Örneğin, gerçek kullanıcı oturum açma sonucuna varan uygulama için *bir id_token sorun.* |
| Başlatan (Aktör) | ObjectId | Kullanıcının oturum imzaladığı B2C uygulamasının **Nesne Kimliği.** Bu tanımlayıcı Azure portalında görünmez, ancak Microsoft Graph API üzerinden erişilebilir. |
| Başlatan (Aktör) | Spn | Kullanıcının oturum imzaladığı B2C uygulamasının **Başvuru Kimliği.** |
| Hedefler | ObjectId | Oturum açan kullanıcının **Nesne Kimliği.** |
| Ek Ayrıntılar | TenantId | Azure AD B2C kiracısının **kiracı kimliği.** |
| Ek Ayrıntılar | PolicyId | Kullanıcıyı oturum açmada kullanılan kullanıcı akışının (ilke) **İlke Kimliği.** |
| Ek Ayrıntılar | ApplicationID | Kullanıcının oturum imzaladığı B2C uygulamasının **Başvuru Kimliği.** |

## <a name="view-audit-logs-in-the-azure-portal"></a>Azure portalında denetim günlüklerini görüntüleme

Azure portalı, Azure AD B2C kiracınızdaki denetim günlüğü olaylarına erişim sağlar.

1. [Azure portalında](https://portal.azure.com) oturum açın
1. Azure AD B2C kiracınızı içeren dizine geçin ve ardından **Azure AD B2C'ye**göz atın.
1. Sol menüdeki **Etkinlikler** altında **Denetim günlüklerini**seçin.

Son yedi gün içinde günlüğe kaydedilen etkinlik olaylarının listesi görüntülenir.

![Azure portalında iki etkinlik olayı olan örnek filtre](./media/view-audit-logs/audit-logs-example-filter.png)

Çeşitli filtreleme seçenekleri mevcuttur:

* **Etkinlik Kaynak Türü** - [Kullanılabilir etkinliklere Genel Bakış](#overview-of-activities-available-in-the-b2c-category-of-audit-logs) bölümünde tabloda gösterilen etkinlik türlerine göre filtreuygulayın.
* **Tarih** - Gösterilen etkinliklerin tarih aralığını filtreleyin.

Listede bir satır seçerseniz, olayın etkinlik ayrıntıları görüntülenir.

Virgülle ayrılmış değerler (CSV) dosyasındaki etkinlik olaylarılistesini indirmek için **İndir'i**seçin.

## <a name="get-audit-logs-with-the-azure-ad-reporting-api"></a>Azure AD raporlama API'si ile denetim günlükleri alın

Denetim günlükleri Azure Etkin Diziniçin diğer etkinliklerle aynı ardışık alt işte yayımlanır, böylece [Azure Etkin Dizin raporlama API'si](https://docs.microsoft.com/graph/api/directoryaudit-list)aracılığıyla erişilebilirler. Daha fazla bilgi için bkz: [Azure Etkin Dizin raporlama API'si ile başlayın.](../active-directory/reports-monitoring/concept-reporting-api.md)

### <a name="enable-reporting-api-access"></a>Raporlama API erişimini etkinleştirme

Azure AD raporlama API'sine komut dosyası veya uygulama tabanlı erişime izin vermek için, Azure AD B2C kiracınızda aşağıdaki API izinlerine sahip kayıtlı bir uygulamanız gerekir. Bu izinleri B2C kiracınızda varolan bir uygulama kaydında etkinleştirebilir veya özellikle denetim günlüğü otomasyonu yla kullanılmak üzere yeni bir izin oluşturabilirsiniz.

* Microsoft Graph > Uygulama izinleri > AuditLog > AuditLog.Read.All

Gerekli izinleri olan bir uygulamayı kaydetmek için aşağıdaki makaledeki adımları izleyin:

[Microsoft Graph ile Azure AD B2C'yi yönetme](microsoft-graph-get-started.md)

Uygun izinlere sahip bir uygulamayı kaydettikten sonra, komut dosyasıyla etkinlik etkinliklerini nasıl elde edebileceğinize bir örnek için bu makalenin daha sonraki PowerShell komut dosyası bölümüne bakın.

### <a name="access-the-api"></a>API'ye erişin

API üzerinden Azure AD B2C denetim günlüğü olaylarını indirmek `B2C` için, kategorideki günlüklere filtre uygulayın. Kategoriye göre filtre `filter` uygulayabilmek için, Azure AD raporlama API bitiş noktasını ararken sorgu dizesi parametresini kullanın.

```HTTP
https://graph.microsoft.com/v1.0/auditLogs/directoryAudits?$filter=loggedByService eq 'B2C' and activityDateTime gt 2019-09-10T02:28:17Z
```

### <a name="powershell-script"></a>PowerShell betiği

Aşağıdaki PowerShell komut dosyası, Azure AD raporlama API'sini nasıl sorgulayabildiğini gösteren bir örnek gösterir. API'yi sorguladıktan sonra, günlüğe kaydedilen olayları standart çıktıya yazdırır ve json çıktısını bir dosyaya yazar.

Bu komut dosyasını [Azure Bulut Kabuğu'nda](overview.md)deneyebilirsiniz. Uygulama kimliğiniz, istemci sırrınız ve Azure AD B2C kiracınızın adı ile güncelleştirdiğinden emin olun.

```powershell
# This script requires an application registration that's granted Microsoft Graph API permission
# https://docs.microsoft.com/azure/active-directory-b2c/microsoft-graph-get-started

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's client ID, a GUID
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's client secret
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # Insert your Azure AD B2C tenant domain name

$loginURL       = "https://login.microsoftonline.com"
$resource       = "https://graph.microsoft.com"           # Microsoft Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 through n for number of nextLink pages
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

Makalede daha önce gösterilen örnek etkinlik olayının JSON gösterimi aşağıda verilmiştir:

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

Diğer yönetim görevlerini otomatikleştirebilirsiniz, örneğin, [Microsoft Graph ile Azure AD B2C kullanıcı hesaplarını yönetebilirsiniz.](manage-user-accounts-graph-api.md)
