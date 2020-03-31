---
title: Azure Monitör'de oturum açma günlüğü şeması | Microsoft Dokümanlar
description: Azure Monitörü'nde kullanılmak üzere Azure AD oturum açma şeasını açıklayın
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5525f2f8ab4ef83ba9c3aeeff945bc9d875600d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75748671"
---
# <a name="interpret-the-azure-ad-sign-in-logs-schema-in-azure-monitor"></a>Azure Monitor'da Azure AD oturum açma şemalarını yorumlama

Bu makalede, Azure Monitor'da Azure Etkin Dizin (Azure AD) oturum açma şeması açıklanmaktadır. Oturum açma larla ilgili bilgilerin çoğu `records` nesnenin *Özellikler* özniteliği altında sağlanır.


```json
{ 
    "time": "2019-03-12T16:02:15.5522137Z", 
    "resourceId": "/tenants/<TENANT ID>/providers/Microsoft.aadiam",
    "operationName": "Sign-in activity", 
    "operationVersion": "1.0", 
    "category": "SignInLogs", 
    "tenantId": "<TENANT ID>", 
    "resultType": "50140", 
    "resultSignature": "None", 
    "resultDescription": "This error occurred due to 'Keep me signed in' interrupt when the user was signing-in.", 
    "durationMs": 0, 
    "callerIpAddress": "<CALLER IP ADDRESS>", 
    "correlationId": "a75a10bd-c126-486b-9742-c03110d36262", 
    "identity": "Timothy Perkins", 
    "Level": 4, 
    "location": "US", 
    "properties": 
        {
            "id":"0231f922-93fa-4005-bb11-b344eca03c01",
            "createdDateTime":"2019-03-12T16:02:15.5522137+00:00",
            "userDisplayName":"Timothy Perkins",
            "userPrincipalName":"<USER PRINCIPAL NAME>",
            "userId":"<USER ID>",
            "appId":"<APPLICATION ID>",
            "appDisplayName":"Azure Portal",
            "ipAddress":"<IP ADDRESS>",
            "status":
            {
                "errorCode":50140,
                "failureReason":"This error occurred due to 'Keep me signed in' interrupt when the user was signing-in."
            },
            "clientAppUsed":"Browser",
            "deviceDetail":
            {
                "operatingSystem":"Windows 10",
                "browser":"Chrome 72.0.3626"
            },
            "location":
                {
                    "city":"Bellevue",
                    "state":"Washington",
                    "countryOrRegion":"US",
                    "geoCoordinates":
                    {
                        "latitude":45,
                        "longitude":122
                    }
                },
            "correlationId":"a75a10bd-c126-486b-9742-c03110d36262",
            "conditionalAccessStatus":"notApplied",
            "appliedConditionalAccessPolicies":
            [
                {
                    "id":"ae11ffaa-9879-44e0-972c-7538fd5c4d1a",
                    "displayName":"Hr app access policy",
                    "enforcedGrantControls":
                    [
                        "Mfa"
                    ],
                    "enforcedSessionControls":
                    [
                    ],
                    "result":"notApplied"
                },
                {
                    "id":"b915a70b-2eee-47b6-85b6-ff4f4a66256d",
                    "displayName":"MFA for all but global support access",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"830f27fa-67a8-461f-8791-635b7225caf1",
                    "displayName":"Header Based Application Control",
                    "enforcedGrantControls":["Mfa"],
                    "enforcedSessionControls":[],
                    "result":"notApplied"
                },
                {
                    "id":"8ed8d7f7-0a2e-437b-b512-9e47bed562e6",
                    "displayName":"MFA for everyones",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"52924e0f-798b-4afd-8c42-49055c7d6395",
                    "displayName":"Device compliant",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
             ],
            "isInteractive":true,
            "tokenIssuerType":"AzureAD",
            "authenticationProcessingDetails":[],
            "networkLocationDetails":[],
            "processingTimeInMilliseconds":0,
            "riskDetail":"hidden",
            "riskLevelAggregated":"hidden",
            "riskLevelDuringSignIn":"hidden",
            "riskState":"none",
            "riskEventTypes":[],
            "resourceDisplayName":"windows azure service management api",
            "resourceId":"797f4846-ba00-4fd7-ba43-dac1f8f63013",
            "authenticationMethodsUsed":[]
        }
}
```


## <a name="field-descriptions"></a>Alan açıklamaları

| Alan adı | Açıklama |
|------------|-------------|
| Zaman | TARIH ve saat, UTC'de. |
| ResourceId | Bu değer eşlenmemiştir ve bu alanı güvenle yoksayabilirsiniz.  |
| ThrottledRequests | Oturum açmalar için bu değer her zaman *oturum açma etkinliğidir.* |
| OperationVersion | İstemci tarafından istenen REST API sürümü. |
| Kategori | Oturum açmalar için bu değer her zaman *SignIn'dir.* | 
| TenantId | Günlüklerle ilişkili kiracı GUID. |
| Sonuç Türü | Oturum açma işleminin sonucu *Başarı* veya *Başarısızlık*olabilir. | 
| ResultSignature | Oturum açma işlemi için varsa hata kodunu içerir. |
| Sonuç Açıklaması | Oturum açma işlemi için hata açıklamasını sağlar. |
| riskDetay | riskDetay | Riskli bir kullanıcının belirli bir durumunun arkasındaki 'nedeni' sağlar, oturum açma veya risk algılama. Olası değerler `none`şunlardır: , `aiConfirmedSigninSafe`, `userPassedMFADrivenByRiskBasedPolicy` `adminDismissedAllRiskForUser` `userPerformedSecuredPasswordChange` `userPerformedSecuredPasswordReset` `adminConfirmedSigninSafe` `adminGeneratedTemporaryPassword`, `adminConfirmedSigninCompromised` `unknownFutureValue`, , , , , , . Değer, `none` kullanıcı veya oturum açma konusunda şimdiye kadar hiçbir eylem gerçekleştirilmemiştir. <br>**Not:** Bu özelliğin ayrıntıları için Azure AD Premium P2 lisansı gerekir. Diğer lisanslar değeri `hidden`döndürün. |
| riskEventTypes | riskEventTypes | Oturum açma ile ilişkili risk algılama türleri. Olası değerler `unlikelyTravel`şunlardır: , `suspiciousIPAddress`, `leakedCredentials` `investigationsThreatIntelligence` `maliciousIPAddress` `unfamiliarFeatures` `malwareInfectedIPAddress` `anonymizedIPAddress`, `generic`, `unknownFutureValue`, , , , ve . |
| riskLevelAggregated | riskDüzey | Toplu risk düzeyi. Olası değerler şunlardır: `none`, `hidden`, `unknownFutureValue` `low` `medium` `high`, , , ve . Değer, `hidden` kullanıcının veya oturum açmanın Azure AD Kimlik Koruması için etkinleştirilen olmadığı anlamına gelir. **Not:** Bu özelliğin ayrıntıları yalnızca Azure AD Premium P2 müşterileri için kullanılabilir. Diğer tüm müşteriler `hidden`iade edilecektir. |
| riskLevelDuringSignIn | riskDüzey | Oturum açma sırasında risk seviyesi. Olası değerler şunlardır: `none`, `hidden`, `unknownFutureValue` `low` `medium` `high`, , , ve . Değer, `hidden` kullanıcının veya oturum açmanın Azure AD Kimlik Koruması için etkinleştirilen olmadığı anlamına gelir. **Not:** Bu özelliğin ayrıntıları yalnızca Azure AD Premium P2 müşterileri için kullanılabilir. Diğer tüm müşteriler `hidden`iade edilecektir. |
| riskState | riskState | Riskli kullanıcının durumunu, oturum açmadurumunu veya risk tespitini bildirir. Olası değerler şunlardır: `none`, `atRisk` `confirmedCompromised`, `unknownFutureValue` `confirmedSafe` `remediated` `dismissed`, , , , . |
| Sürems |  Bu değer eşlenmemiştir ve bu alanı güvenle yoksayabilirsiniz. |
| ArayanIpAddress | İstekte bulundu müşterinin IP adresi. | 
| CorrelationId | İstemci tarafından geçirilen isteğe bağlı GUID. Bu değer, istemci tarafındaki işlemleri sunucu tarafındaki işlemlerle ilişkilendirmenize yardımcı olabilir ve hizmetleri kapsayan günlükleri takip ederken kullanışlıdır. |
| Kimlik | İstekte bulunduğunuzda sunulan belirtecikimlik. Bir kullanıcı hesabı, sistem hesabı veya hizmet anapara olabilir. |
| Düzey | İleti türünü sağlar. Denetim için, her zaman *Informational'* s . |
| Konum | Oturum açma etkinliğinin konumunu sağlar. |
| Özellikler | Oturum açma ile ilişkili tüm özellikleri listeler. Daha fazla bilgi için [Microsoft Graph API Başvurusu'na](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)bakın. Bu şema, okunabilirlik için oturum açma kaynağıyla aynı öznitelik adlarını kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure tanı günlükleri hakkında daha fazla bilgi edinin](../../azure-monitor/platform/platform-logs-overview.md)
