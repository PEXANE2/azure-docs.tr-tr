---
title: Operasyon API'si alın | Azure Marketi
description: Teklifteki tüm işlemleri alır veya belirtilen operationId için belirli bir işlem alır.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
ms.date: 04/08/2020
ms.author: dsindona
ms.openlocfilehash: 93b2ca700a987b86aedfdae55d58540c8ffe84ed
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255882"
---
# <a name="retrieve-operations"></a>İşlemleri alma

> [!NOTE]
> Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu İş Ortağı Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için [Bulut İş Ortağı Portalı API Başvurusu'nda](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) listelenen değişiklikleri gözden geçirin.

Teklifteki tüm işlemleri alır veya belirtilen operationId için belirli bir işlem alır. İstemci, çalışan işlemleri filtrelemek için sorgu parametrelerini kullanabilir.

``` https

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/submissions/?api-version=2017-10-31&status=<filteredStatus>

  GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/operations/<operationId>?api-version=2017-10-31

```


## <a name="uri-parameters"></a>URI parametreleri

|  **Adı**          |      **Açıklama**                                                                                           | **Veri türü** |
|  ----------------  |     --------------------------------------------------------------------------------------------------------   |  -----------  |
|  publisherId       |  Yayımcı tanımlayıcısı, örneğin`Contoso`                                                                   |  Dize       |
|  offerId           |  Teklif tanımlayıcısı                                                                                              |  Dize       |
|  operationId       |  Teklifteki işlemi benzersiz olarak tanımlayan GUID. OperationId bu API kullanılarak alınabilir ve [ayrıca Yayımlama teklifi](./cloud-partner-portal-api-publish-offer.md) API'si gibi uzun süren herhangi bir işlem için yanıtın HTTP üstbilgisinde döndürülür.  |   Guid   |
|  api-sürümü       | API'nin en son sürümü |    Tarih      |
|  |  |  |

## <a name="header"></a>Üst bilgi


|  **Adı**          |  **Değer**           |
|  ---------------   | -------------------- |
|  İçerik Türü      | `application/json`   |
|  Yetkilendirme     | `Bearer YOUR_TOKEN`  |
|  |  |


## <a name="body-example"></a>Gövde örneği

### <a name="response"></a>Yanıt

#### <a name="get-operations"></a>GET işlemleri

``` json
    [
        {
            "id": "5a63deb5-925b-4ee0-938b-7c86fbf287c5",
            "offerId": "56615b67-2185-49fe-80d2-c4ddf77bb2e8",
            "offerVersion": 1,
            "offerTypeId": "microsoft-azure-virtualmachines",
            "publisherId": "contoso",
            "submissionType": "publish",
            "submissionState": "running",
            "publishingVersion": 2,
            "slot": "staging",
            "version": 636576975611768314,
            "definition": {
                "metadata": {
                    "emails": "jdoe@contoso.com"
                }
            },
            "changedTime": "2018-03-26T21:46:01.179948Z"
        }
    ]
```

#### <a name="get-operation"></a>GET işlemi

``` json
    [
        {
            "status" : "running",
            "messages" : [],
            "publishingVersion" : 2,
            "offerVersion" : 1,
            "cancellationRequestState": "canCancel",
            "steps": [
                        {
                            "estimatedTimeFrame": "< 15 min",
                            "id": "displaydummycertify",
                            "stepName": "Validate Pre-Requisites",
                            "description": "Offer settings provided are validated",
                            "status": "complete",
                            "messages": 
                            [
                                {
                                    "messageHtml": "Step completed.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:36.500052Z"
                                }
                            ],
                            "progressPercentage": 100
                        },
                        {
                            "estimatedTimeFrame": "< 5 day",
                            "id": "displaycertify",
                            "stepName": "Certification",
                            "description": "Your offer is analyzed by our certification systems for issues.",
                            "status": "blocked",
                            "messages": 
                            [
                                {
                                    "messageHtml": "No virtual machine image was found for the plan contoso.",
                                    "level": "error",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                },
                                {
                                    "messageHtml": "This step has not started yet.",
                                    "level": "information",
                                    "timestamp": "2017-03-28T19:50:39.5506018Z"
                                }
                            ],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 day",
                            "id": "displayprovision",
                            "stepName": "Provisioning",
                            "description": "Your virtual machine is being replicated in our production systems.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "< 1 hour",
                            "id": "displaypackage",
                            "stepName": "Packaging and Lead Generation Registration",
                            "description": "Your virtual machine is packaged for being shown to your customers. Additionally, we hookup our lead generation systems to send leads for your offer.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "id": "publisher-signoff",
                            "stepName": "Publisher signoff",
                            "description": "Offer is available to preview. Ensure that everything looks good before making your offer live.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        },
                        {
                            "estimatedTimeFrame": "~2-5 days",
                            "id": "live",
                            "stepName": "Live",
                            "description": "Offer is publicly visible and is available for purchase.",
                            "status": "notStarted",
                            "messages": [],
                            "progressPercentage": 0
                        }
                    ],
                "previewLinks": [],
                "liveLinks": [],
            }
        }
    ]
```

### <a name="response-body-properties"></a>Yanıt gövde özellikleri

|  **Adı**                    |  **Açıklama**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
|  id                          | İşlemi benzersiz olarak tanımlayan GUID                                                       |
|  submissionType              | Teklif için bildirilen işlem türünü tanımlar, örneğin`Publish/GoLive`      |
|  createdDateTime             | Operasyonun oluşturulduğu UTC tarih saati                                                       |
|  lastActionDateTime          | Operasyonda son güncelleştirmenin yapıldığı UTC tarih saati                                       |
|  durum                      | Operasyonun durumu, ya `not started` \| `running` \| `failed` \| `completed`. Aynı anda yalnızca `running` bir işlem durumu olabilir. |
|  error                       | Başarısız işlemler için hata iletisi                                                               |
|  |  |

### <a name="response-step-properties"></a>Yanıt adım özellikleri

|  **Adı**                    |  **Açıklama**                                                                                  |
|  --------------------        |  ------------------------------------------------------------------------------------------------ |
| tahminiTimeFrame | Bu işlemin tahmini süresi |
| id | Adım işlemi için benzersiz tanımlayıcı |
| açıklama | Adımın açıklaması |
| stepName | Adım için dostça isim |
| durum | Adımın durumu, ya `notStarted` \| `running` \| `failed` \|`completed` |
| sayısı | Adım sırasında karşılaşılan bildirimler veya uyarılar. Dize dizileri |
| progressPercentage | Adımın ilerlemesini gösteren 0'dan 100'e kadar bir yarım |
| | |

### <a name="response-status-codes"></a>Yanıt durum kodları

| **Kod**  |   **Açıklama**                                                                                  |
|  -------- |   -------------------------------------------------------------------------------------------------|
|  200      | `OK`- İstek başarıyla işlendi ve talep edilen işlem(ler) iade edildi.        |
|  400      | `Bad/Malformed request`- Hata yanıt gövdesi daha fazla bilgi içerebilir.                    |
|  403      | `Forbidden`- İstemcinin belirtilen ad alanına erişimi yoktur.                          |
|  404      | `Not found`- Belirtilen varlık yok.                                                 |
|  |  |
