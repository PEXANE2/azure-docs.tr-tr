---
title: Sistem durumu için Metin Analizi kullanma
titleSuffix: Azure Cognitive Services
description: Sağlık bilgilerini, sistem durumu için Metin Analizi ile yapılandırılmamış klinik metinden nasıl ayıklayacağınızı ve etiketleyeceğinizi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 12/02/2020
ms.author: aahi
ms.custom: references_regions
ms.openlocfilehash: 27af0ffdeb254dbb671a0618d0e9973336a7f529
ms.sourcegitcommit: 65a4f2a297639811426a4f27c918ac8b10750d81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96559023"
---
# <a name="how-to-use-text-analytics-for-health-preview"></a>Nasıl yapılır: sistem durumu için Metin Analizi kullanma (Önizleme)

> [!IMPORTANT] 
> Sistem durumu Metin Analizi, "olduğu gıbı" ve "tüm hatalarıyla bırlıkte" sağlanmış bir önizleme özelliğidir. Bu nedenle, **sistem durumu için metin analizi (Önizleme) herhangi bir üretim kullanımı için uygulanmamalıdır veya dağıtılmamalıdır.** Sağlık durumu için Metin Analizi, bir tıbbi cihaz, klinik destek, tanılama aracı ya da tanılama, sağlama, azaltma, işleme veya önlemeye yönelik başka bir teknoloji veya Microsoft tarafından bu amaçla kullanılması amaçlanan bir lisans ya da hak sağlanmaz. Bu özellik, profesyonel tıp önerisi veya sağlık görüşlerine, tanılama, işleme ya da sağlık uzmanı 'nın klinik yargılarına veya bu şekilde kullanılmamalıdır. Müşteri, sistem durumu için Metin Analizi herhangi bir kullanım işleminden yalnızca sorumludur. Microsoft, sağlık veya yetenek ile bağlantılı olarak belirtilen herhangi bir malzeme için Metin Analizi, herhangi bir kişinin sistem durumunu veya tıbbi gereksinimlerini karşılamasını garanti etmez. 


Sistem durumu Metin Analizi, Doctor 'ın notları, disşarj özetleri, klinik belgeler ve elektronik sistem durumu kayıtları gibi yapılandırılmamış metinlerden ilgili tıbbi bilgileri çıkaran ve etiketleyen Metin Analizi API'si hizmetinin bir özelliğidir.  Bu hizmeti kullanmanın iki yolu vardır: 

* Web tabanlı API (zaman uyumsuz) 
* Bir Docker kapsayıcısı (zaman uyumlu)   

> [!VIDEO https://channel9.msdn.com/Shows/AI-Show/Introducing-Text-Analytics-for-Health/player]

## <a name="features"></a>Özellikler

Sistem durumu için Metin Analizi, adlandırılmış varlık tanıma (NER), ilişki ayıklama, varlık olumsuzlama ve Ingilizce metin üzerinde varlık bağlantısı kurarak yapılandırılmamış klinik ve Biyotıp metninde Öngörüler elde etmeyi sağlar.

### <a name="named-entity-recognition"></a>[Adlandırılmış varlık tanıma](#tab/ner)

Adlandırılmış varlık tanıma, bir veya daha fazla anlamsal tür ile ilişkilendirilebilen (örn. tanılama, yollara adı, belirti/işaret veya yaş gibi), yapılandırılmamış metinde belirtilen kelimeleri ve tümceleri algılar.

> [!div class="mx-imgBorder"]
> ![Sağlık durumu](../media/ta-for-health/health-named-entity-recognition.png)

### <a name="relation-extraction"></a>[İlişki ayıklama](#tab/relation-extraction)

İlişki ayıklama metinde bahsedilen kavramlar arasındaki anlamlı bağlantıları tanımlar. Örneğin, bir koşul adı bir saat ile ilişkilendirerek bir "koşul süresi" ilişkisi bulunur. 

> [!div class="mx-imgBorder"]
> ![Sağlık YENIDEN](../media/ta-for-health/health-relation-extraction.png)


### <a name="entity-linking"></a>[Varlık Bağlama](#tab/entity-linking)

Varlık bağlama, metinde bahsedilen adlandırılmış varlıkları, önceden tanımlanmış bir kavram veritabanında bulunan kavramlara ilişkilendirerek ayrı varlıkların ayırt düzeyini ortadan kaldırarak ayırt ediyor. Örneğin, Birleşik Tıbbi dil sistemi (UMLS).

> [!div class="mx-imgBorder"]
> ![Sağlık EL](../media/ta-for-health/health-entity-linking.png)

Sistem durumu Metin Analizi Birleşik Tıbbi dil sistemi ([Umls](https://www.nlm.nih.gov/research/umls/sourcereleasedocs/index.html)) Metaeşanlamlılar bilgi kaynağında bulunan sağlık ve biotıbbi sözcük dağarcıklarını bağlamayı destekler.

### <a name="negation-detection"></a>[Algılama Olumsuzlaştırma](#tab/negation-detection) 

Tıbbi içeriğin anlamı, yanlış tanılanıyor durumunda önemli bir engel olabilecek Olumsuzlaştırma gibi değiştiricilerin büyük ölçüde etkilenir. Sistem durumu Metin Analizi, metinde bahsedilen farklı varlıkların algılanmasını desteklemiyor. 

> [!div class="mx-imgBorder"]
> ![Sistem durumu NEG](../media/ta-for-health/health-negation.png)

---

Desteklenen varlıkların tam listesi için sistem durumu için Metin Analizi tarafından döndürülen [varlık kategorilerine](../named-entity-types.md?tabs=health) bakın.

### <a name="supported-languages-and-regions"></a>Desteklenen diller ve bölgeler

Sistem durumu Metin Analizi yalnızca Ingilizce dil belgelerini destekler. 

Sistem durumu barındırılan Web API 'SI için Metin Analizi Şu anda yalnızca şu bölgelerde kullanılabilir: Batı ABD 2, Doğu ABD 2, Orta ABD, Kuzey Avrupa ve Batı Avrupa.

## <a name="request-access-to-the-public-preview"></a>Genel önizlemeye erişim isteyin

Sistem durumu genel önizlemesi için Metin Analizi erişim istemek için bilişsel [Hizmetler istek formunu](https://aka.ms/csgate) doldurun ve iletin. Sistem durumu kullanımı için Metin Analizi faturalandırılmaz. 

Form, siz, şirketiniz ve kapsayıcısını kullanacağınız kullanıcı senaryosu hakkında bilgi ister. Formu gönderdikten sonra, Azure bilişsel hizmetler ekibi bunu gözden geçirir ve bir karar vererek size e-posta ile gönderilir.

> [!IMPORTANT]
> * Formunda, bir Azure aboneliği KIMLIĞIYLE ilişkili bir e-posta adresi kullanmanız gerekir.
> * Kullandığınız Azure kaynağının, onaylanan Azure abonelik KIMLIĞIYLE oluşturulmuş olması gerekir. 
> * Microsoft 'tan uygulamanızın durumuyla ilgili güncelleştirmeler için e-postanızı (hem gelen kutusu hem de önemsiz klasörler) denetleyin.

## <a name="using-the-docker-container"></a>Docker kapsayıcısını kullanma 

Kendi ortamınızda sistem durumu kapsayıcısı için Metin Analizi çalıştırmak için, [kapsayıcıyı indirmek ve yüklemek üzere aşağıdaki yönergeleri](../how-tos/text-analytics-how-to-install-containers.md?tabs=healthcare)izleyin.

## <a name="using-the-client-library"></a>İstemci kitaplığını kullanma

Metin Analizi istemci kitaplığının en son sürümü, istemci nesnesi kullanarak sistem durumu için Metin Analizi çağırmanızı sağlar. Başvuru belgelerine başvurun ve GitHub 'daki örneklere bakın:
* [C#](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics)
* [Python](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/textanalytics/azure-ai-textanalytics/)
* [Java](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/textanalytics/azure-ai-textanalytics)



## <a name="sending-a-rest-api-request"></a>REST API isteği gönderiliyor 

### <a name="preparation"></a>Hazırlık

Sistem durumu Metin Analizi, üzerinde çalışmaya daha az miktarda metin verdiğiniz zaman daha yüksek kaliteli bir sonuç üretir. Bu, daha büyük metin blokları üzerinde daha iyi sonuç veren anahtar tümceciği ayıklama gibi bazı diğer Metin Analizi özelliklerden daha tersidir. Bu işlemlerden en iyi sonuçları elde etmek için, girişleri uygun şekilde yeniden ayarlamayı göz önünde bulundurun.

Bu biçimde JSON belgelerinize sahip olmanız gerekir: KIMLIK, metin ve dil. 

Belge boyutunun belge başına 5.120 karakter altında olması gerekir. Bir koleksiyonda izin verilen en fazla belge sayısı için kavramlar bölümündeki [veri limitleri](../concepts/data-limits.md?tabs=version-3) makalesine bakın. Koleksiyon, istek gövdesinde gönderilir.

### <a name="structure-the-api-request-for-the-hosted-asynchronous-web-api"></a>Barındırılan zaman uyumsuz Web API 'SI için API isteğini yapılandırma

Hem kapsayıcı hem de barındırılan Web API 'SI için bir POST isteği oluşturmanız gerekir. İstediğiniz bölgedeki barındırılan Web API 'sine hızlı bir şekilde bir POST isteği oluşturmak ve göndermek için [sistem durumu BARıNDıRıLAN API başvurusu için metin analizi](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/Health) [Postman](text-analytics-how-to-call-api.md)'ı, bir kıvrımlı komutu veya **API test konsolunu** kullanabilirsiniz. 

Aşağıda, sistem sağlığı API isteğinin GÖNDERI gövdesinde Metin Analizi eklenmiş bir JSON dosyası örneği verilmiştir:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Subject was administered 100mg remdesivir intravenously over a period of 120 min"
    }
  ]
}
```

### <a name="hosted-asynchronous-web-api-response"></a>Barındırılan zaman uyumsuz Web API yanıtı 

Bu POST isteği, zaman uyumsuz işlem için bir iş göndermek üzere kullanıldığından, yanıt nesnesinde bir metin yok.  Ancak, iş ve çıkış durumunu denetlemek için bir GET isteği oluşturmak üzere yanıt üst bilgilerinde işlem konumu ANAHTARıNıN değeri gereklidir.  POST isteğinin yanıt üstbilgisindeki Operation-location ANAHTARıNıN değerine bir örnek aşağıda verilmiştir:

`https://<your-custom-subdomain>.cognitiveservices.azure.com/text/analytics/v3.1-preview.3/entities/health/jobs/<jobID>`

İş durumunu denetlemek için POST yanıtının Operation-location anahtar üstbilgisi değerindeki URL 'ye bir GET isteği yapın.  Aşağıdaki durumlar, bir işin durumunu yansıtmak için kullanılır: `NotStarted` , `running` ,,,, `succeeded` `failed` `rejected` `cancelling` , ve `cancelled` .  

`NotStarted` `running` Get ISTEĞIYLE aynı URL 'ye BIR Delete http çağrısını içeren veya durumu ile bir işi iptal edebilirsiniz.  [Sistem durumu BARıNDıRıLAN API başvurusu için metin analizi](https://westus2.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v3-1-preview-3/operations/CancelHealthJob), silme çağrısı hakkında daha fazla bilgi bulunabilir.

Bir GET isteğinin yanıtı örneği aşağıda verilmiştir.  Çıkışın, `expirationDateTime` çıktının temizlenmeden sonra geçen süre (örneğin, işin oluşturulduğu zamandan itibaren 24 saat) geçtiğinde, çıktının alınması için kullanılabilir olduğunu lütfen unutmayın.

```json
{
    "jobId": "b672c6f5-7c0d-4783-ba8c-4d0c47213454",
    "lastUpdateDateTime": "2020-11-18T01:45:00Z",
    "createdDateTime": "2020-11-18T01:44:55Z",
    "expirationDateTime": "2020-11-19T01:44:55Z",
    "status": "succeeded",
    "errors": [],
    "results": {
        "documents": [
            {
                "id": "1",
                "entities": [
                    {
                        "offset": 25,
                        "length": 5,
                        "text": "100mg",
                        "category": "Dosage",
                        "confidenceScore": 1.0,
                        "isNegated": false
                    },
                    {
                        "offset": 31,
                        "length": 10,
                        "text": "remdesivir",
                        "category": "MedicationName",
                        "confidenceScore": 1.0,
                        "isNegated": false,
                        "links": [
                            {
                                "dataSource": "UMLS",
                                "id": "C4726677"
                            },
                            {
                                "dataSource": "MSH",
                                "id": "C000606551"
                            },
                            {
                                "dataSource": "NCI",
                                "id": "C152185"
                            },
                            {
                                "dataSource": "NCI_FDA",
                                "id": "3QKI37EEHE"
                            }
                        ]
                    },
                    {
                        "offset": 42,
                        "length": 13,
                        "text": "intravenously",
                        "category": "MedicationRoute",
                        "confidenceScore": 1.0,
                        "isNegated": false
                    },
                    {
                        "offset": 56,
                        "length": 4,
                        "text": "over",
                        "category": "Time",
                        "confidenceScore": 0.87,
                        "isNegated": false
                    },
                    {
                        "offset": 73,
                        "length": 7,
                        "text": "120 min",
                        "category": "Time",
                        "confidenceScore": 0.99,
                        "isNegated": false
                    }
                ],
                "relations": [
                    {
                        "relationType": "DosageOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/0",
                        "target": "#/results/documents/0/entities/1"
                    },
                    {
                        "relationType": "RouteOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/2",
                        "target": "#/results/documents/0/entities/1"
                    },
                    {
                        "relationType": "TimeOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/3",
                        "target": "#/results/documents/0/entities/1"
                    },
                    {
                        "relationType": "TimeOfMedication",
                        "bidirectional": false,
                        "source": "#/results/documents/0/entities/4",
                        "target": "#/results/documents/0/entities/1"
                    }
                ],
                "warnings": []
            }
        ],
        "errors": [],
        "modelVersion": "2020-09-03"
    }
}
```


### <a name="structure-the-api-request-for-the-container"></a>Kapsayıcı için API isteğini yapılandırma

Dağıttığınız kapsayıcıya bir sorgu göndermek için [Postman](text-analytics-how-to-call-api.md) veya örnek kıvrımlı isteği kullanabilirsiniz ve `serverURL` değişkeni uygun değerle değiştirin.  Kapsayıcının URL 'sindeki API sürümünün barındırılan API 'den farklı olduğunu aklınızda yapın.

```bash
curl -X POST 'http://<serverURL>:5000/text/analytics/v3.2-preview.1/entities/health' --header 'Content-Type: application/json' --header 'accept: application/json' --data-binary @example.json

```

Aşağıdaki JSON, sistem durumu API 'SI isteğinin GÖNDERI gövdesinde Metin Analizi eklenmiş bir JSON dosyası örneğidir:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Patient reported itchy sores after swimming in the lake."
    },
    {
      "language": "en",
      "id": "2",
      "text": "Prescribed 50mg benadryl, taken twice daily."
    }
  ]
}
```

### <a name="container-response-body"></a>Kapsayıcı yanıt gövdesi

Aşağıdaki JSON, Kapsayıcılı zaman uyumlu çağrıdan sistem durumu API 'SI yanıt gövdesi için Metin Analizi bir örneğidir:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "id": "0",
                    "offset": 25,
                    "length": 5,
                    "text": "100mg",
                    "category": "Dosage",
                    "confidenceScore": 1.0,
                    "isNegated": false
                },
                {
                    "id": "1",
                    "offset": 31,
                    "length": 10,
                    "text": "remdesivir",
                    "category": "MedicationName",
                    "confidenceScore": 1.0,
                    "isNegated": false,
                    "links": [
                        {
                            "dataSource": "UMLS",
                            "id": "C4726677"
                        },
                        {
                            "dataSource": "MSH",
                            "id": "C000606551"
                        },
                        {
                            "dataSource": "NCI",
                            "id": "C152185"
                        },
                        {
                            "dataSource": "NCI_FDA",
                            "id": "3QKI37EEHE"
                        }
                    ]
                },
                {
                    "id": "2",
                    "offset": 42,
                    "length": 13,
                    "text": "intravenously",
                    "category": "MedicationRoute",
                    "confidenceScore": 1.0,
                    "isNegated": false
                },
                {
                    "id": "3",
                    "offset": 56,
                    "length": 4,
                    "text": "over",
                    "category": "Time",
                    "confidenceScore": 0.87,
                    "isNegated": false
                },
                {
                    "id": "4",
                    "offset": 73,
                    "length": 7,
                    "text": "120 min",
                    "category": "Time",
                    "confidenceScore": 0.99,
                    "isNegated": false
                }
            ],
            "relations": [
                {
                    "relationType": "DosageOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/0",
                    "target": "#/documents/0/entities/1"
                },
                {
                    "relationType": "RouteOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/2",
                    "target": "#/documents/0/entities/1"
                },
                {
                    "relationType": "TimeOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/3",
                    "target": "#/documents/0/entities/1"
                },
                {
                    "relationType": "TimeOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/0/entities/4",
                    "target": "#/documents/0/entities/1"
                }
            ]
        }
    ],
    "errors": [],
    "modelVersion": "2020-09-03"
}
```

### <a name="negation-detection-output"></a>Algılama çıkışının Olumsuzlaştırma

Değilleme algılama kullanılırken, bazı durumlarda tek bir olumsuzlama bir kez birkaç terim ele alabilir. Tanınan bir varlığın değilleme, JSON çıktısında bayrağın Boolean değeri tarafından temsil edilir `isNegated` , örneğin:

```json
{
  "id": "2",
  "offset": 90,
  "length": 10,
  "text": "chest pain",
  "category": "SymptomOrSign",
  "score": 0.9972,
  "isNegated": true,
  "links": [
    {
      "dataSource": "UMLS",
      "id": "C0008031"
    },
    {
      "dataSource": "CHV",
      "id": "0000023593"
    },
    ...
```

### <a name="relation-extraction-output"></a>İlişki ayıklama çıkışı

İlişki ayıklama çıkışı, ilişkinin *kaynağına* ve *hedefine* URI başvuruları içerir. İlişki rolü olan varlıklar `ENTITY` `target` alana atanır. İlişki rolü olan varlıklar `ATTRIBUTE` `source` alana atanır. Kısaltma ilişkileri çift yönlü `source` ve `target` alanları içerir ve `bidirectional` olarak ayarlanır `true` . 

```json
"relations": [
                {
                    "relationType": "DosageOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/1/entities/0",
                    "target": "#/documents/1/entities/1"
                },
                {
                    "relationType": "FrequencyOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/1/entities/2",
                    "target": "#/documents/1/entities/1"
                }
            ]
  },
...
]
```

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Adlandırılmış varlık kategorileri](../named-entity-types.md)
* [Yenilikler](../whats-new.md)
