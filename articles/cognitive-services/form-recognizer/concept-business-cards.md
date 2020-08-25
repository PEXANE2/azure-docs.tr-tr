---
title: İş kartları-form tanıyıcı
titleSuffix: Azure Cognitive Services
description: Tanıyıcı API kullanımı ve limitleriyle iş kartı analizine ilişkin kavramları öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: conceptual
ms.date: 08/17/2019
ms.author: pafarley
ms.openlocfilehash: 039f7343bcef64db9ad9eae558cd3e97f3678c59
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88799290"
---
# <a name="business-card-concepts"></a>Kartvizit kavramları

Azure form tanıyıcısı, önceden oluşturulmuş modellerinden birini kullanarak iş kartlarından anahtar değer çiftlerini çözümleyebilir ve ayıklayabilir. İş kartı API 'SI, önemli bilgileri Ingilizce olarak iş kartlarımızdan ayıklamak için Iş kartımızda güçlü optik karakter tanıma (OCR) yeteneklerini birleştirir. Kişisel iletişim bilgilerini, şirket adını, iş başlığını ve daha fazlasını ayıklar. Önceden oluşturulmuş Iş kartı API 'SI, tanıyıcı v 2.1 Preview biçiminde herkese açık bir şekilde sunulmaktadır. 

## <a name="what-does-the-business-card-api-do"></a>Iş kartı API 'SI ne yapar?

Iş kartı API 'SI, iş kartlarından önemli alanları ayıklar ve bunları düzenlenmiş bir JSON yanıtında döndürür.

![Contoso FOTT + JSON çıktısından resim dökümü](./media/business-card-english.jpg)

### <a name="fields-extracted"></a>Ayıklanan alanlar: 
* Kişi adları 
* Ad 
* Soyadı 
* Şirket adları 
* Departmanlar 
* İş başlıkları 
* E-postalar 
* Web Siteleri 
* Adresler 
* Telefon numaraları 
  * Cep telefonları 
  * Yazdırılacağı 
  * İş telefonları 
  * Diğer telefonlar 

Iş kartı API 'SI Ayrıca Iş kartından tanınan tüm metni döndürür. Bu OCR çıkışı JSON yanıtına dahildir.  

### <a name="input-requirements"></a>Giriş gereksinimleri 

[!INCLUDE [input reqs](./includes/input-requirements-receipts.md)]

## <a name="the-analyze-business-card-operation"></a>Iş kartını çözümle işlemi

[Çözümle Iş kartı](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync) , giriş olarak bir iş kartının GÖRÜNTÜSÜNÜ veya PDF 'sini alır ve ilgi ve metin değerlerini ayıklar. Çağrı, adlı bir yanıt üst bilgisi alanı döndürüyor `Operation-Location` . `Operation-Location`Değer, bir sonraki adımda kullanılacak sonuç kimliğini içeren BIR URL 'dir.

|Yanıt üst bilgisi| Sonuç URL 'SI |
|:-----|:----|
|İşlem-konum | `https://cognitiveservice/formrecognizer/v2.1-preview.1/prebuilt/businessCard/analyzeResults/49a36324-fc4b-4387-aa06-090cfbf0064f` |

## <a name="the-get-analyze-business-card-result-operation"></a>Iş kartını çözümle sonucu al işlemi

İkinci adım, [Çözümle Iş kartı sonucunu al](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/GetAnalyzeBusinessCardResult) işlemini çağırmalıdır. Bu işlem, Iş kartını çözümle işlemi tarafından oluşturulan sonuç KIMLIĞINI giriş olarak alır. Aşağıdaki olası değerlere sahip bir **durum** alanı IÇEREN bir JSON yanıtı döndürür. **Başarılı** değerle döndürülünceye kadar bu işlemi tekrarlayarak çağırın. Saniye başına istek (RPS) oranını aşmamak için 3 ile 5 saniye arasında bir Aralık kullanın.

|Alan| Tür | Olası değerler |
|:-----|:----:|:----|
|durum | string | notStarted: analiz işlemi başlatılmamış. |
| |  | çalışıyor: analiz işlemi devam ediyor. |
| |  | başarısız: çözümleme işlemi başarısız oldu. |
| |  | başarılı: çözümleme işlemi başarılı oldu. |

**Durum** alanı **başarılı** DEĞERINE sahip olduğunda, JSON yanıtı iş kartı anlama ve metin tanıma sonuçlarını içerir. İş kartı anlama sonucu, her değerin ayıklanan metin, normalleştirilmiş değer, sınırlama kutusu, güvenirlik ve karşılık gelen Word öğelerini içerdiği adlandırılmış alan değerlerinin bir sözlüğü olarak düzenlenmiştir. Metin tanıma sonucu, metin, sınırlayıcı kutusu ve güvenle bilgi içeren bir satır ve sözcük hiyerarşisi olarak düzenlenir.

![örnek iş kartı çıkışı](./media/business-card-results.png)

### <a name="sample-json-output"></a>Örnek JSON çıkışı

Başarılı bir JSON yanıtı örneğine bakın: "readResults" düğümü, tanınan metnin tamamını içerir. Metin sayfaya, sonra satıra, sonra da tek sözcüklere göre düzenlenir. "DocumentResults" düğümü, modelin bulduğu iş kartına özgü değerleri içerir. Burada ad, soyadı, şirket adı ve daha fazlası gibi faydalı anahtar/değer çiftleri bulacaksınız.

```json
{
    "status": "succeeded",
    "createdDateTime": "2020-08-20T17:41:19Z",
    "lastUpdatedDateTime": "2020-08-20T17:41:24Z",
    "analyzeResult": {
        "version": "2.1.0",
        "readResults": [
            {
                "page": 1,
                "angle": -17.0956,
                "width": 4032,
                "height": 3024,
                "unit": "pixel",
                "lines": 
                          {
                        "text": "Dr. Avery Smith",
                        "boundingBox": [
                            419.3,
                            1154.6,
                            1589.6,
                            877.9,
                            1618.9,
                            1001.7,
                            448.6,
                            1278.4
                        ],
                        "words": [
                            {
                                "text": "Dr.",
                                "boundingBox": [
                                    419,
                            ]
    
            }
        ],
        "documentResults": [
            {
                "docType": "prebuilt:businesscard",
                "pageRange": [
                    1,
                    1
                ],
                "fields": {
                    "ContactNames": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "object",
                                "valueObject": {
                                    "FirstName": {
                                        "type": "string",
                                        "valueString": "Avery",
                                        "text": "Avery",
                                        "boundingBox": [
                                            703,
                                            1096,
                                            1134,
                                            989,
                                            1165,
                                            1109,
                                            733,
                                            1206
                                        ],
                                        "page": 1
                                    },
                                    "LastName": {
                                        "type": "string",
                                        "valueString": "Smith",
                                        "text": "Smith",
                                        "boundingBox": [
                                            1186,
                                            976,
                                            1585,
                                            879,
                                            1618,
                                            998,
                                            1218,
                                            1096
                                        ],
                                        "page": 1
                                    }
                                },
                                "text": "Dr. Avery Smith",
                                "boundingBox": [
                                    419.3,
                                    1154.6,
                                    1589.6,
                                    877.9,
                                    1618.9,
                                    1001.7,
                                    448.6,
                                    1278.4
                                ],
                                "confidence": 0.97
                            }
                        ]
                    },
                    "JobTitles": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Senior Researcher",
                                "text": "Senior Researcher",
                                "boundingBox": [
                                    451.8,
                                    1301.9,
                                    1313.5,
                                    1099.9,
                                    1333.8,
                                    1186.7,
                                    472.2,
                                    1388.7
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Departments": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Cloud & Al Department",
                                "text": "Cloud & Al Department",
                                "boundingBox": [
                                    480.1,
                                    1403.3,
                                    1590.5,
                                    1129.6,
                                    1612.6,
                                    1219.6,
                                    502.3,
                                    1493.3
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Emails": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "avery.smith@contoso.com",
                                "text": "avery.smith@contoso.com",
                                "boundingBox": [
                                    2107,
                                    934,
                                    2917,
                                    696,
                                    2935,
                                    764,
                                    2126,
                                    995
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Websites": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "https://www.contoso.com/",
                                "text": "https://www.contoso.com/",
                                "boundingBox": [
                                    2121,
                                    1002,
                                    2992,
                                    755,
                                    3014,
                                    826,
                                    2143,
                                    1077
                                ],
                                "page": 1,
                                "confidence": 0.995
                            }
                        ]
                    },
                    "MobilePhones": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 7911 123456",
                                "boundingBox": [
                                    2434.9,
                                    1033.3,
                                    3072,
                                    836,
                                    3096.2,
                                    914.3,
                                    2459.1,
                                    1111.6
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "OtherPhones": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 20 9876 5432",
                                "boundingBox": [
                                    2473.2,
                                    1115.4,
                                    3139.2,
                                    907.7,
                                    3163.2,
                                    984.7,
                                    2497.2,
                                    1192.4
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Faxes": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "phoneNumber",
                                "text": "+44 (0) 20 6789 2345",
                                "boundingBox": [
                                    2525,
                                    1185.4,
                                    3192.4,
                                    977.9,
                                    3217.9,
                                    1060,
                                    2550.5,
                                    1267.5
                                ],
                                "page": 1,
                                "confidence": 0.99
                            }
                        ]
                    },
                    "Addresses": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "2 Kingdom Street Paddington, London, W2 6BD",
                                "text": "2 Kingdom Street Paddington, London, W2 6BD",
                                "boundingBox": [
                                    1230,
                                    2138,
                                    2535.2,
                                    1678.6,
                                    2614.2,
                                    1903.1,
                                    1309,
                                    2362.5
                                ],
                                "page": 1,
                                "confidence": 0.977
                            }
                        ]
                    },
                    "CompanyNames": {
                        "type": "array",
                        "valueArray": [
                            {
                                "type": "string",
                                "valueString": "Contoso",
                                "text": "Contoso",
                                "boundingBox": [
                                    1152,
                                    1916,
                                    2293,
                                    1552,
                                    2358,
                                    1733,
                                    1219,
                                    2105
                                ],
                                "page": 1,
                                "confidence": 0.97
                            }
                        ]
                    }
                }
            }
        ]
    }
}
```

Python ve REST API kullanarak iş kartı verilerinin ayıklanmasını uygulamak için [iş kartı verilerini](./QuickStarts/python-business-cards.md) ayıklama hızlı başlangıcını izleyin.

## <a name="customer-scenarios"></a>Müşteri senaryoları  

Iş kartı API 'siyle ayıklanan veriler, çeşitli görevleri gerçekleştirmek için kullanılabilir. Bu kişi bilgisinin ayıklanması, istemciye yönelik rollerdeki diğer roller için otomatik olarak zaman kazandırır. Müşterilerimizin Iş kartı API 'SI ile yaptığımız örneklere birkaç örnek aşağıda verilmiştir:

* Iş kartlarından kişi bilgilerini ayıklayın ve hızla telefon kişileri oluşturun. 
* İş kartı görüntülerini kullanarak otomatik olarak iletişim oluşturmak için CRM ile tümleştirin. 
* Satış fırsatlarını takip edin.  
* Mevcut iş kartı görüntülerinden toplu olarak kişi bilgilerini ayıklayın. 

Iş kartı API 'SI de [Aibuilder Iş kartı işleme özelliğini](https://docs.microsoft.com/ai-builder/prebuilt-business-card)güçlendirir.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanmaya başlamak için hızlı başlangıcı izleyin [API Python hızlı başlangıç](./quickstarts/python-business-cards.md)
- [Form tanıyıcı](https://westcentralus.dev.cognitive.microsoft.com/docs/services/form-recognizer-api-v2-1-preview-1/operations/AnalyzeBusinessCardAsync) hakkında bilgi edinin REST API
- [Form tanıyıcı](overview.md) hakkında daha fazla bilgi


