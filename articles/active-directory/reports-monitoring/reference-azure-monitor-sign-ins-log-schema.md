---
title: Azure Izleyici 'de oturum açma günlüğü şeması | Microsoft Docs
description: Azure Izleyici 'de kullanılmak üzere Azure AD oturum açma günlüğü şemasını açıkla
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
ms.openlocfilehash: 6d7c9713f27643e792ea381e1a2419cbc4b67a99
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82129194"
---
# <a name="interpret-the-azure-ad-sign-in-logs-schema-in-azure-monitor"></a>Azure Izleyici 'de Azure AD oturum açma günlüğü şemasını yorumlama

Bu makalede, Azure Izleyici 'de Azure Active Directory (Azure AD) oturum açma günlüğü şeması açıklanmaktadır. Oturum açma bilgileriyle ilgili bilgilerin çoğu, `records` nesnenin *Özellikler* özniteliği altında sağlanır.


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
| Zaman | Tarih ve saat, UTC olarak. |
| ResourceId | Bu değer eşlenmemiş ve bu alanı güvenle yoksayabilirsiniz.  |
| ThrottledRequests | Oturum açma işlemleri için bu değer her zaman *oturum açma etkinliğidir*. |
| OperationVersion | İstemci tarafından istenen REST API sürümü. |
| Kategori | Oturum açma işlemleri için bu değer her zaman *oturum*açadır. | 
| TenantId | Günlüklerle ilişkili kiracı GUID 'SI. |
| 'I | Oturum açma işleminin sonucu *başarılı* veya *başarısız*olabilir. | 
| ResultSignature | Varsa, oturum açma işlemi için hata kodunu içerir. |
| ResultDescription | Oturum açma işlemi için hata açıklaması sağlar. |
| riskDetail | riskDetail | Riskli bir kullanıcının, oturum açmanın veya risk algılamanın arkasındaki ' neden ' durumunu sağlar. Olası değerler `none`şunlardır:, `adminGeneratedTemporaryPassword`, `userPerformedSecuredPasswordChange` `userPerformedSecuredPasswordReset` `adminConfirmedSigninSafe`,,, `aiConfirmedSigninSafe`, `userPassedMFADrivenByRiskBasedPolicy`, `adminDismissedAllRiskForUser`, `adminConfirmedSigninCompromised`,. `unknownFutureValue` Değer `none` , Kullanıcı veya şimdiye kadar oturum açma işlemleri için hiçbir eylem gerçekleştirilmediği anlamına gelir. <br>**Note:** Bu özelliğe ilişkin ayrıntılar Azure AD Premium P2 lisansı gerektirir. Diğer lisanslar değeri `hidden`döndürür. |
| riskEventTypes | riskEventTypes | Oturum açma ile ilişkili risk algılama türleri. Olası değerler `unlikelyTravel`şunlardır:, `anonymizedIPAddress`, `maliciousIPAddress`, `unfamiliarFeatures`, `malwareInfectedIPAddress`, `suspiciousIPAddress`, `leakedCredentials`, `investigationsThreatIntelligence`, `generic`, ve. `unknownFutureValue` |
| riskLevelAggregated | riskLevel | Toplu risk düzeyi. Olası değerler şunlardır `none`:, `low`, `medium`, `high`, `hidden`, ve. `unknownFutureValue` Değer `hidden` , kullanıcının veya oturum açma Azure AD kimlik koruması için etkinleştirilmemiş anlamına gelir. **Note:** Bu özelliğin ayrıntıları yalnızca Azure AD Premium P2 müşterileri için kullanılabilir. Diğer tüm müşteriler döndürülür `hidden`. |
| riskLevelDuringSignIn | riskLevel | Oturum açma sırasında risk düzeyi. Olası değerler şunlardır `none`:, `low`, `medium`, `high`, `hidden`, ve. `unknownFutureValue` Değer `hidden` , kullanıcının veya oturum açma Azure AD kimlik koruması için etkinleştirilmemiş anlamına gelir. **Note:** Bu özelliğin ayrıntıları yalnızca Azure AD Premium P2 müşterileri için kullanılabilir. Diğer tüm müşteriler döndürülür `hidden`. |
| riskState | riskState | Riskli kullanıcının, oturum açma işleminin veya risk algılamanın durumunu raporlar. Olası `none`değerler şunlardır:, `confirmedSafe`, `remediated`, `dismissed`, `atRisk`, `confirmedCompromised`,. `unknownFutureValue` |
| Ort |  Bu değer eşlenmemiş ve bu alanı güvenle yoksayabilirsiniz. |
| Callerıpaddress | İsteği yapan istemcinin IP adresi. | 
| CorrelationId | İstemci tarafından geçirilen isteğe bağlı GUID. Bu değer, istemci tarafı işlemlerini sunucu tarafı işlemleriyle ilişkilendirmenize yardımcı olabilir ve hizmetleri kapsayan günlükleri izlerken yararlıdır. |
| Kimlik | İsteği yaptığınızda sunulan belirtecin kimliği. Bu bir kullanıcı hesabı, sistem hesabı veya hizmet sorumlusu olabilir. |
| Düzey | İleti türünü sağlar. Denetim için her zaman *bilgilendirme amaçlıdır*. |
| Konum | Oturum açma etkinliğinin konumunu sağlar. |
| Özellikler | Oturum açma işlemleri ile ilişkili tüm özellikleri listeler. Daha fazla bilgi için bkz. [MICROSOFT Graph API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). Bu şema, okunabilirlik için oturum açma kaynağıyla aynı öznitelik adlarını kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure platformu günlükleri hakkında daha fazla bilgi edinin](../../azure-monitor/platform/platform-logs-overview.md)
