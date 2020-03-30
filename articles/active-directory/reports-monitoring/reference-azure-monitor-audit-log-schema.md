---
title: Azure Etkin Dizin denetim günlüğü şemasını Azure Monitörü'nde yorumlama | Microsoft Dokümanlar
description: Azure Monitörü'nde kullanılmak üzere Azure AD denetim günlüğü şemasını açıklayın
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
ms.openlocfilehash: d9f58b213e50a021651f35112a48d8f74ae59571
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68987951"
---
# <a name="interpret-the-azure-ad-audit-logs-schema-in-azure-monitor-preview"></a>Azure Denetimi şemalarını Azure Monitörü'nde yorumlama (önizleme)

Bu makalede, Azure Monitor'da Azure Etkin Dizin (Azure AD) denetim günlüğü şeması açıklanmaktadır. Her bir günlük girişi metin olarak depolanır ve aşağıdaki iki örnekte gösterildiği gibi JSON blob olarak biçimlendirilir: 

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-17T00:14:31.2585575Z", 
        "operationName": "Change password (self-service)",
        "operationVersion": "1.0",
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "resultDescription": "None", 
        "durationMs": "-1", 
        "correlationId": "60d5e89a-b890-413f-9e25-a047734afe9f", 
        "identity": "sreens@wingtiptoysonline.com", 
        "Level": "Informational", 
        "location": "WUS", 
        "properties": { 
            "identityType": "UPN", 
            "operationType": "Update", 
            "additionalDetails": "None", 
            "additionalTargets": "", 
            "targetUpdatedProperties": "", 
            "targetResourceType": "UPN__TenantContextID__PUID__ObjectID__ObjectClass", 
            "targetResourceName": "sreens@wingtiptoysonline.com__bf85dc9d-cb43-44a4-80c4-469e8c58249e__1003BFFD9FEB17DB__7a408bdd-7d97-4574-8511-dd747b56465d__User", 
            "auditEventCategory": "UserManagement" 
        } 
    } 
    ] 
} 
```

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-18T19:47:43.0368859Z", 
        "operationName": "Update service principal.", 
        "operationVersion": "1.0", 
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "durationMs": "-1", 
        "callerIpAddress": "<null>", 
        "correlationId": "14916c7a-5a7d-44e8-9b06-74b49efb08ee", 
        "identity": "NA", 
        "Level": "Informational", 
        "properties": { 
            "identityType": "NA", 
            "operationType": "Update", 
            "additionalDetails": {}, 
            "additionalTargets": "", 
            "targetUpdatedProperties": [ 
            { 
                "Name": "Included Updated Properties", 
                "OldValue": null, 
                "NewValue": "" 
            }, 
            { 
                "Name": "TargetId.ServicePrincipalNames", 
                "OldValue": null, 
                "NewValue": "http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29" 
            } 
            ], 
        "targetResourceType": "Other__ObjectID__ObjectClass__Name__AppId__SPN", 
        "targetResourceName": "ServicePrincipal_ea70a262-4da3-440a-b396-9734ddfd9df2__ea70a262-4da3-440a-b396-9734ddfd9df2__ServicePrincipal__Salesforce__cd3ed3de-93ee-400b-8b19-b61ef44a0f29__http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29", 
        "auditEventCategory": "ApplicationManagement" 
      } 
    } 
    ] 
} 
```

```json
{
    "records": [
    {
        "time": "2018-12-10T00:03:46.6161822Z",
        "resourceId": "/tenants/7918d4b5-0442-4a97-be2d-36f9f9962ece/providers/Microsoft.aadiam",
        "operationName": "Update policy",
        "operationVersion": "1.0",
        "category": "AuditLogs",
        "tenantId": "7918d4b5-0442-4a97-be2d-36f9f9962ece",
        "resultSignature": "None",
        "durationMs": 0,
        "callerIpAddress": "<null>",
        "correlationId": "192298c1-0994-4dd6-b05a-a6c5984c31cb",
        "identity": "MS-PIM",
        "level": "Informational",
        "properties": {
            "id": "Directory_VNXV4_28148892",
            "category": "Policy",
            "correlationId": "192298c1-0994-4dd6-b05a-a6c5984c31cb",
            "result": 0,
            "resultReason": "",
            "activityDisplayName": "Update policy",
            "activityDateTime": "2018-12-10T00:03:46.6161822+00:00",
            "loggedByService": "Core Directory",
            "operationType": "Update",
            "initiatedBy": {},
            "targetResources": [
            {
                "id": "5e7a8ae7-165d-44a4-a4f4-6141f8c8ef40",
                "displayName": "Default Policy",
                "type": "Policy",
                "modifiedProperties": []
            }
            ],
            "additionalDetails": []
        }
    }
    ]
}

```

## <a name="field-and-property-descriptions"></a>Alan ve özellik açıklamaları

| Alan adı | Açıklama |
|------------|-------------|
| time       | Tarih ve saat (UTC). |
| operationName | Operasyonun adı. |
| operationVersion | İstemci tarafından istenen REST API sürümü. |
| category | Şu anda *Denetim* desteklenen tek değerdir. |
| tenantId | Günlüklerle ilişkili kiracı GUID. |
| resultType | İşlemin sonucu. Sonuç *Başarı* veya *Başarısızlık*olabilir. |
| resultSignature |  Bu alan eşlenmemiş ve güvenli bir şekilde yok sayabilirsiniz. | 
| resultDescription | Varsa, sonucun ek bir açıklaması. | 
| durationMs |  Bu alan eşlenmemiş ve güvenli bir şekilde yok sayabilirsiniz. |
| callerIpAddress | İstekte bulundu müşterinin IP adresi. | 
| correlationId | İstemci tarafından geçirilen isteğe bağlı bir GUID. İstemci tarafı işlemlerini sunucu tarafındaki işlemlerle ilişkilendirmenize yardımcı olabilir ve hizmetleri kapsayan günlükleri takip ederken kullanışlıdır. |
| identity | İstekte bulunduğunuzda sunulan belirtecikimlik. Kimlik bir kullanıcı hesabı, sistem hesabı veya hizmet sorumlusu olabilir. |
| düzey | İleti türü. Denetim günlükleri için, düzey her zaman *Bilgilendirme*dir. |
| location | Veri merkezinin konumu. |
| properties | Denetim günlüğüyle ilgili desteklenen özellikleri listeler. Daha fazla bilgi için sonraki tabloya bakın. | 

<br>

| Özellik adı | Açıklama |
|---------------|-------------|
| DenetimEventKategori | Denetim olayının türü. *Bu Kullanıcı Yönetimi,* *Uygulama Yönetimi*veya başka bir tür olabilir.|
| Kimlik Türü | Türü *Uygulama* veya *Kullanıcı*olabilir. |
| İşlem Türü | Türü *Ekle*, *Güncelleştir*, *Sil*olabilir . veya *Diğer*. |
| Hedef Kaynak Türü | İşlemin gerçekleştirildiğini hedef kaynak türünü belirtir. Tür *Uygulama*, *Kullanıcı*, *Rol*, *Politika* olabilir | 
| Hedef Kaynak Adı | Hedef kaynağın adı. Bir uygulama adı, bir rol adı, kullanıcı anaadı veya bir hizmet ana adı olabilir. |
| ek Hedefler | Belirli işlemler için ek özellikleri listeler. Örneğin, bir güncelleştirme işlemi için, eski değerler ve yeni değerler *hedefGüncelLeştirilmiş Özellikler*altında listelenir. | 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Monitor'da oturum açma günlüklerini şema yorumlama](reference-azure-monitor-sign-ins-log-schema.md)
* [Azure tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Sık sorulan sorular ve bilinen sorunlar](concept-activity-logs-azure-monitor.md#frequently-asked-questions)