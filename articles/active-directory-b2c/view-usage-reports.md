---
title: Kullanım Raporlama API örnekleri ve tanımları
titleSuffix: Azure AD B2C
description: Azure AD B2C kiracı kullanıcıları, kimlik doğrulamaları ve çok faktörlü kimlik doğrulamaları hakkında rapor alma hakkında rehberlik ve örnekler.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: b4486c1459024d60b53292f070dd565d20d10c52
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76847738"
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a>Raporlama API 'SI aracılığıyla Azure AD B2C kullanım raporlarına erişme

Azure Active Directory B2C (Azure AD B2C), Kullanıcı oturum açma ve Azure Multi-Factor Authentication temel alınarak kimlik doğrulaması sağlar. Kimlik sağlayıcıları arasında uygulama ailenizin son kullanıcıları için kimlik doğrulaması sağlanır. Kiracıda kayıtlı kullanıcı sayısını, kaydolmak için kullandıkları sağlayıcıları ve türe göre kimlik doğrulama sayısını bildiğiniz gibi soruları cevaplayabilirsiniz:
* Her bir kimlik sağlayıcısı türünden (örneğin, bir Microsoft veya LinkedIn hesabı) kaç Kullanıcı son 10 gün içinde kayıtlı?
* Son ayda Multi-Factor Authentication kullanan kaç kimlik doğrulama başarıyla tamamlandı?
* Bu ay kaç oturum açma tabanlı kimlik doğrulaması tamamlandı? Gün başına? Uygulama başına mı?
* Azure AD B2C kiracı etkinliklerimin beklenen aylık maliyetini nasıl tahmin edebilirim?

Bu makale, kullanıcı sayısına, faturalandırılabilir oturum açma tabanlı kimlik doğrulamaları ve Multi-Factor Authentication 'a dayalı olarak faturalandırma etkinliğine bağlı raporlara odaklanır.


## <a name="prerequisites"></a>Ön koşullar
Başlamadan önce, [Azure AD Raporlama API 'lerine erişmek Için önkoşulların](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)adımlarını gerçekleştirmeniz gerekir. Bir uygulama oluşturun, onun için bir parola alın ve Azure AD B2C kiracının raporlarına erişim hakları verin. *Bash betiği* ve *Python betiği* örnekleri de burada verilmiştir.

## <a name="powershell-script"></a>PowerShell betiği
Bu betik, `TimeStamp` parametresi ve `ApplicationId` filtresi kullanılarak dört kullanım raporu oluşturmayı gösterir.

```powershell
# This script will require the Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.microsoftonline.com"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from the tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for the report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for the " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a>Kullanım raporu tanımları
* **Tenantusercount**: kiracı 'daki Kullanıcı sayısı, son 30 gün içinde her gün kimlik sağlayıcısı türüne göre. (İsteğe bağlı olarak, bir `TimeStamp` filtresi belirli bir tarihten itibaren geçerli tarihe kadar kullanıcı sayısını sağlar). Rapor şunları sağlar:
  * **Totalusercount**: tüm Kullanıcı nesnelerinin sayısı.
  * **Otherusercount**: Azure Active Directory Kullanıcı sayısı (Azure AD B2C Kullanıcı değil).
  * **Localusercount**: Azure AD B2C kiracısında yerel kimlik bilgileriyle oluşturulan Azure AD B2C Kullanıcı hesabı sayısı.

* **Alternateıdusercount**: dış kimlik sağlayıcılarına (örneğin, Facebook, bir Microsoft hesabı veya başka bir Azure Active Directory kiracıyla) kaydedilen Azure AD B2C Kullanıcı sayısı (`OrgId`olarak da adlandırılır).

* **b2cAuthenticationCountSummary**: son 30 gün içinde günlük faturalandırılabilir kimlik doğrulamalarının, güne ve kimlik doğrulama akışının türüne göre Özet sayısı.

* **b2cAuthenticationCount**: bir zaman aralığı içindeki kimlik doğrulama sayısı. Varsayılan değer son 30 gündür.  (İsteğe bağlı: başlangıç ve bitiş `TimeStamp` parametreleri belirli bir zaman dilimini tanımlar.) Çıktı `StartTimeStamp` (Bu kiracı için en erken etkinlik tarihi) ve `EndTimeStamp` (en son güncelleştirme) içerir.

* **b2cMfaRequestCountSummary**: çok faktörlü kimlik doğrulamaları, güne ve türe (SMS veya Voice) göre günlük sayısının Özeti.


## <a name="limitations"></a>Sınırlamalar
Kullanıcı sayısı verileri her 24 ile 48 saat arasında yenilenir. Kimlik doğrulamaları günde birkaç kez güncelleştirilir. `ApplicationId` filtresi kullanılırken, boş bir rapor yanıtı aşağıdaki koşullardan biri olabilir:
  * Kiracıda uygulama KIMLIĞI yok. Doğru olduğundan emin olun.
  * Uygulama KIMLIĞI var, ancak raporlama döneminde hiçbir veri bulunamadı. Tarih/saat parametrelerinizi gözden geçirin.


## <a name="next-steps"></a>Sonraki adımlar
### <a name="monthly-bill-estimates-for-azure-ad"></a>Azure AD için aylık fatura tahminleri
[En güncel Azure AD B2C fiyatlandırmayla](https://azure.microsoft.com/pricing/details/active-directory-b2c/)birlikte kullanıldığında günlük, haftalık ve aylık Azure tüketimini tahmin edebilirsiniz.  Bir tahmin özellikle, kiracı davranışında yapılan ve genel maliyeti etkileyebilecek değişiklikler için plan yaptığınızda faydalıdır. [Bağlı Azure aboneliğinizdeki](billing.md)gerçek maliyetleri gözden geçirebilirsiniz.

### <a name="options-for-other-output-formats"></a>Diğer çıkış biçimleri için seçenekler
Aşağıdaki kod, JSON 'a çıktı gönderme örnekleri, bir ad değer listesi ve XML:
```powershell
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath name-your-file.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
