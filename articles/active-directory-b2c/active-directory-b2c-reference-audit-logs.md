---
title: Azure Active Directory B2C | denetim günlükleri örnekleri ve tanımları | Microsoft Docs
description: Azure AD B2C denetim günlüklerine erişirken kılavuz ve örnekler.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: d8cc67b8e243fb2b97cd1522a850adc63c84428e
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69969623"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Azure AD B2C denetim günlüklerine erişme

Azure Active Directory B2C (Azure AD B2C) B2C kaynakları, verilen belirteçler ve yönetici erişimiyle ilgili etkinlik bilgilerini içeren denetim günlüklerini yayar. Bu makalede, denetim günlükleri ve bu verilere Azure AD B2C kiracınız için nasıl erişileceğini gösteren yönergeler hakkında kısa bir genel bakış sunulmaktadır.

> [!IMPORTANT]
> Denetim günlükleri yalnızca yedi gün boyunca tutulur. Daha uzun bir bekletme dönemi istiyorsanız, aşağıda gösterilen yöntemlerden birini kullanarak günlüklerinizi indirmeyi ve depolamayı planlayın.

> [!NOTE]
> **Azure Active Directory** veya **Azure AD B2C** Blade 'in **Kullanıcılar** bölümünde bireysel Azure AD B2C uygulamaları için Kullanıcı oturum açma işlemlerini göremezsiniz. Oturum açma işlemleri, Kullanıcı etkinliğini gösterir, ancak kullanıcının oturum açmasını sağlayacak B2C uygulamasına geri bağlanamaz. Bu makalenin ilerleyen bölümlerinde açıklandığı gibi, Denetim günlüklerini bu şekilde kullanmanız gerekir.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Denetim günlüklerinin B2C kategorisinde bulunan etkinliklere genel bakış
Denetim günlüklerinde **B2C** kategorisi aşağıdaki etkinlik türlerini içerir:

|Etkinlik türü |Açıklama  |
|---------|---------|
|Authorization |Bir kullanıcının B2C kaynaklarına erişmesi için yetkilendirmesiyle ilgili etkinlikler (örneğin, B2C ilkelerinin listesine erişen bir yönetici)         |
|Dizin |Bir yönetici Azure portal kullanarak oturum açtığında alınan dizin öznitelikleriyle ilgili etkinlikler |
|Uygulama | B2C uygulamalarında CRUD işlemleri |
|Anahtar |B2C anahtar kapsayıcısında depolanan anahtarlarla CRUD işlemleri |
|Resource |B2C kaynaklarında CRUD işlemleri (örneğin, ilkeler ve kimlik sağlayıcıları)
|Authentication |Kullanıcı kimlik bilgilerinin ve belirteç verme 'nin doğrulanması|

> [!NOTE]
> Kullanıcı nesnesi CRUD etkinlikleri için **çekirdek Dizin** kategorisine bakın.

## <a name="example-activity"></a>Örnek etkinlik
Aşağıdaki örnekte, bir Kullanıcı bir dış kimlik sağlayıcısıyla oturum açtığında yakalanan veriler gösterilmektedir:  
    ![Azure portal içindeki denetim günlüğü etkinlik ayrıntıları sayfası örneği](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

Etkinlik ayrıntıları paneli aşağıdaki ilgili bilgileri içerir:

|`Section`|Alan|Açıklama|
|-------|-----|-----------|
| Etkinlik | Name | Hangi etkinlik gerçekleşti. Örneğin, "uygulamaya bir id_token verme" (gerçek Kullanıcı oturum açma sonucuna giriş). |
| Başlatan (aktör) | ObjectId | Kullanıcının oturum açtığı B2C uygulamasının **nesne kimliği** (bu tanımlayıcı Azure Portal görünmez, ancak Graph API ile erişilebilir). |
| Başlatan (aktör) | Spn | Kullanıcının oturum açtığı B2C uygulamasının **uygulama kimliği** . |
| Hedef (ler) | ObjectId | Oturum açan kullanıcının **nesne kimliği** . |
| Ek Ayrıntılar | TenantId | Azure AD B2C kiracının **KIRACı kimliği** . |
| Ek Ayrıntılar | `PolicyId` | Kullanıcının oturumu açmak için kullanılan Kullanıcı akışının (ilke) **Ilke kimliği** . |
| Ek Ayrıntılar | ApplicationId | Kullanıcının oturum açtığı B2C uygulamasının **uygulama kimliği** . |

## <a name="accessing-audit-logs-through-the-azure-portal"></a>Azure portal aracılığıyla denetim günlüklerine erişme
1. [Azure Portal](https://portal.azure.com) gidin. B2C dizininizde olduğunuzdan emin olun.
2. Sol taraftaki Sık Kullanılanlar çubuğunda **Azure Active Directory** ' a tıklayın

    ![Sol taraftaki Portal menüsünde Azure Active Directory Düğme vurgulanmış](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. **Etkinlik**' in altında, **Denetim günlükleri** ' ne tıklayın.

    ![Menünün etkinlik bölümünde vurgulanan denetim günlükleri düğmesi](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. **Kategori** Dropbox ' da **B2C** ' yi seçin.
3. **Uygula** ' ya tıklayın

    ![Denetim günlüğü filtresinde kategori ve Uygula düğmesi vurgulanır](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Son yedi gün içinde günlüğe kaydedilen etkinliklerin listesini görürsünüz.
- Yukarıda özetlenen etkinlik türlerine göre filtrelemek için **etkinlik kaynağı türü** açılan listesini kullanın
- Gösterilen etkinliklerin tarih aralığını filtrelemek için **Tarih aralığı** açılan listesini kullanın
- Listede belirli bir satıra tıklarsanız, sağda bulunan bağlamsal bir kutu, etkinlikle ilişkili ek öznitelikleri gösterir
- Etkinlikleri CSV dosyası olarak indirmek için **İndir** 'e tıklayın

> [!NOTE]
> Ayrıca, sol taraftaki Sık Kullanılanlar çubuğunda **Azure Active Directory** yerine **Azure AD B2C** giderek denetim günlüklerini görebilirsiniz. **Etkinlikler**' in altında, benzer filtreleme özellikleri ile aynı günlükleri bulacağınız **Denetim günlükleri**' ne tıklayın.

## <a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Azure AD Raporlama API 'SI aracılığıyla denetim günlüklerine erişme
Denetim günlükleri, Azure Active Directory için diğer etkinliklerle aynı işlem hattına yayımlanır, bu nedenle [Azure Active Directory Raporlama API 'si](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference)üzerinden erişilebilirler.

### <a name="prerequisites"></a>Önkoşullar
Azure AD Raporlama API 'sine kimlik doğrulaması yapmak için önce bir uygulama kaydetmeniz gerekir. [Azure AD Raporlama API 'lerine erişmek Için ön koşullar](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)bölümündeki adımları izlediğinizden emin olun.

### <a name="accessing-the-api"></a>API 'ye erişme
Azure AD B2C denetim günlüklerini API aracılığıyla indirmek için günlükleri **B2C** kategorisine göre filtrelemek isteyeceksiniz. Kategoriye göre filtrelemek için, aşağıda gösterildiği gibi Azure AD Raporlama API uç noktasını çağırırken sorgu dizesi parametresini kullanın:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

### <a name="powershell-script"></a>PowerShell betiği
Aşağıdaki betik, PowerShell 'i kullanarak Azure AD Raporlama API 'sini sorgulama ve sonuçları bir JSON dosyası olarak depolama hakkında bir örnek sağlar:

```powershell
# This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
$loginURL       = "https://login.microsoftonline.com"
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # AAD B2C Tenant; for example, contoso.onmicrosoft.com
$resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&$filter=category eq ''B2C''and activityDate gt ' + $7daysago

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
