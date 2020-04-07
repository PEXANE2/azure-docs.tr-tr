---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 02/08/2020
ms.author: diberry
ms.openlocfilehash: dd44d9cb01ff072d89afeb4efc4a59071c621315
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758350"
---
Bu Postman tabanlı quickstart bilgi tabanından bir cevap alma ile size yol.

## <a name="prerequisites"></a>Ön koşullar

* Son [**Postacı.**](https://www.getpostman.com/)
* Sahip olmalısınız.
    * [QnA Maker hizmeti](../How-To/set-up-qnamaker-service-azure.md)
    * Hızlı başlangıç tan oluşturulmuş [sorular ve yanıtlar içeren](../Quickstarts/add-question-metadata-portal.md) eğitimli ve yayınlanmış bir bilgi tabanı meta veriler ve Chit sohbet ile yapılandırılır.

> [!NOTE]
> Bilgi tabanınızdan bir soruya yanıt vermeye hazır olduğunuzda, bilgi tabanınızı [eğitmeli](../Quickstarts/create-publish-knowledge-base.md#save-and-train) ve [yayınlamanız](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) gerekir. Bilgi tabanınız yayımlandığında, Yanıt oluşturmak için **Yayımla** sayfası HTTP istek ayarlarını görüntüler. **Postacı** sekmesi, yanıt oluşturmak için gereken ayarları gösterir.

## <a name="set-up-postman-for-requests"></a>İstekler için Postacı'yı ayarlama

Bu hızlı başlatma Postman **POST** isteği için aynı ayarları kullanır sonra post gövde JSON için sorgulamak için çalışıyoruz ne dayalı hizmete gönderilen yapılandırır.

Postman yapılandırmak için bu yordamı kullanın, sonra POST gövde JSON yapılandırmak için sonraki her bölümü okuyun.

1. Bilgi tabanının **Ayarlar** sayfasından, bilgi tabanından yanıt oluşturmak için kullanılan yapılandırmayı görmek için **Postacı** sekmesini seçin. Postacı'da kullanmak için aşağıdaki bilgileri kopyalayın.

    |Adı|Ayar|Amaç ve değer|
    |--|--|--|
    |`POST`| `/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer`|Bu, URL için HTTP yöntemi ve rotasıdır.|
    |`Host`|`https://diberry-qna-s0-s.azurewebsites.net/qnamaker`|Bu URL'nin ana bilgisayarıdır. Tam generateAnswer URL'sini almak için Ana Bilgisayar ve Posta değerlerini birleştirin.|
    |`Authorization`|`EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`|İsteğinizi Azure'a yetkilendirmek için üstbilgi değeri. |
    |`Content-type`|`application/json`|İçeriğiniz için üstbilgi değeri.|
    ||`{"question":"<Your question>"}`|Bir JSON nesnesi olarak POST isteğinin gövdesi. Bu değer, sorgunun ne yapacağına bağlı olarak aşağıdaki her bölümde değişecektir.|

1. Postman'ı açın ve yayınlanmış bilgi tabanı ayarlarınızla yeni bir temel **POST** isteği oluşturun. Aşağıdaki bölümlerde, sorguyu bilgi tabanınıza değiştirmek için POST gövdesi JSON'u değiştirin.

## <a name="use-metadata-to-filter-answer"></a>Yanıta filtre lemek için meta verileri kullanma

Önceki hızlı başlatmada, iki farklı soruyu ayırt etmek için iki QnA çiftine meta veriler eklenmiştir. Filtreyi yalnızca ilgili QnA çiftiyle sınırlamak için sorguya meta verileri ekleyin.

1. Postman'da, yalnızca json sorgusunu `strictFilters` değiştirerek özelliği ad/değer çiftinin ''in' ile birlikte `service:qna_maker`eklemesini Vücut JSON olmalıdır:

    ```json
    {
        'question':'size',
        'strictFilters': [
            {
                'name':'service','value':'qna_maker'
            }
        ]
    }
    ```

    Soru sadece tek bir `size`kelime, iki soru ve cevap kümeleri de döndürebilir. Dizi, `strictFilters` yanıtı sadece `qna_maker` yanıtlara indirgeyecek şekilde söyler.

1. Yanıt, yalnızca filtre ölçütlerini karşılayan yanıtı içerir.

    Aşağıdaki yanıt okunabilirlik için biçimlendirilmiştir:

    ```JSON
    {
        "answers": [
            {
                "questions": [
                    "How large a knowledge base can I create?",
                    "What is the max size of a knowledge base?",
                    "How many GB of data can a knowledge base hold?"
                ],
                "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.",
                "score": 68.76,
                "id": 3,
                "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting",
                "metadata": [
                    {
                        "name": "link_in_answer",
                        "value": "true"
                    },
                    {
                        "name": "service",
                        "value": "qna_maker"
                    }
                ],
                "context": {
                    "isContextOnly": false,
                    "prompts": []
                }
            }
        ],
        "debugInfo": null
    }
    ```

    Arama terimini karşılamayan ancak filtreyi karşılayan bir soru ve yanıt kümesi varsa, döndürülmez. Bunun yerine, `No good match found in KB.` genel yanıt döndürülür.

## <a name="use-debug-query-property"></a>Hata ayıklama sorgusu özelliğini kullanma

Hata ayıklama bilgileri, döndürülen yanıtın nasıl belirlendiğini anlamanıza yardımcı olur. Yararlı olsa da, gerekli değildir. Hata ayıklama bilgileriyle yanıt oluşturmak `debug` için aşağıdaki özelliği ekleyin:

1. Postman olarak, mülkiyet ekleyerek sadece vücut `debug` JSON değiştirin. JSON olmalıdır:

    ```json
    {
        'question':'size',
        'Debug': {
            'Enable':true
        }

    }
    ```

1. Yanıt, yanıtla ilgili bilgileri içerir. Aşağıdaki JSON çıkışında, bazı hata ayıklama ayrıntıları elips ile değiştirilmiştir.

    ```console
    {
        "answers": [
            {
                "questions": [
                    "How do I share a knowledge base with others?"
                ],
                "answer": "Sharing works at the level of a QnA Maker service, that is, all knowledge bases in the service will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base) how to collaborate on a knowledge base.",
                "score": 56.07,
                "id": 5,
                "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting",
                "metadata": [],
                "context": {
                    "isContextOnly": false,
                    "prompts": []
                }
            }
        ],
        "debugInfo": {
            "userQuery": {
                "question": "How do I programmatically update my Knowledge Base?",
                "top": 1,
                "userId": null,
                "strictFilters": [],
                "isTest": false,
                "debug": {
                    "enable": true,
                    "recordL1SearchLatency": false,
                    "mockQnaL1Content": null
                },
                "rankerType": 0,
                "context": null,
                "qnaId": 0,
                "scoreThreshold": 0.0
            },
            "rankerInfo": {
                "specialFuzzyQuery": "how do i programmatically~6 update my knowledge base",
                "synonyms": "what s...",
                "rankerLanguage": "English",
                "rankerFileName": "https://qnamakerstore.blob.core.windows.net/qnamakerdata/rankers/ranker-English.ini",
                "rankersDirectory": "D:\\home\\site\\wwwroot\\Data\\QnAMaker\\rd0003ffa60fc45.24.0\\RankerData\\Rankers",
                "allQnAsfeatureValues": {
                    "WordnetSimilarity": {
                        "5": 0.54706300120043716,...
                    },
                    ...
                },
                "rankerVersion": "V2",
                "rankerModelType": "TreeEnsemble",
                "rankerType": 0,
                "indexResultsCount": 25,
                "reRankerResultsCount": 1
            },
            "runtimeVersion": "5.24.0",
            "indexDebugInfo": {
                "indexDefinition": {
                    "name": "064a4112-bd65-42e8-b01d-141c4c9cd09e",
                    "fields": [...
                    ],
                    "scoringProfiles": [],
                    "defaultScoringProfile": null,
                    "corsOptions": null,
                    "suggesters": [],
                    "analyzers": [],
                    "tokenizers": [],
                    "tokenFilters": [],
                    "charFilters": [],
                    "@odata.etag": "\"0x8D7A920EA5EE6FE\""
                },
                "qnaCount": 117,
                "parameters": {},
                "azureSearchResult": {
                    "continuationToken": null,
                    "@odata.count": null,
                    "@search.coverage": null,
                    "@search.facets": null,
                    "@search.nextPageParameters": null,
                    "value": [...],
                    "@odata.nextLink": null
                }
            },
            "l1SearchLatencyInMs": 0,
            "qnaL1Results": {...}
        },
        "activeLearningEnabled": true
    }
    ```

## <a name="use-test-knowledge-base"></a>Test bilgi tabanını kullanma

Eğer test bilgi tabanından bir cevap almak `isTest` istiyorsanız, vücut özelliğini kullanın.

Postman olarak, mülkiyet ekleyerek sadece vücut `isTest` JSON değiştirin. JSON olmalıdır:

```json
{
    'question':'size',
    'isTest': true
}
```

JSON yanıtı, yayımlanmış bilgi bankası sorgusuyla aynı şema kullanır.

> [!NOTE]
> Test ve yayımlanmış bilgi tabanları tamamen aynıysa, test dizini kaynaktaki tüm bilgi tabanları arasında paylaşıldığından bazı küçük farklılıklar olabilir.

## <a name="query-for-a-chit-chat-answer"></a>Chit-chat yanıtı için sorgu

1. Postman'da, yalnızca json gövdesini kullanıcıdan gelen bir konuşma sonu deyimiolarak değiştirin. JSON olmalıdır:

    ```json
    {
        'question':'thank you'
    }
    ```

1. Yanıt, skoru ve yanıtı içerir.

    ```json
    {
      "answers": [
          {
              "questions": [
                  "I thank you",
                  "Oh, thank you",
                  "My sincere thanks",
                  "My humblest thanks to you",
                  "Marvelous, thanks",
                  "Marvelous, thank you kindly",
                  "Marvelous, thank you",
                  "Many thanks to you",
                  "Many thanks",
                  "Kthx",
                  "I'm grateful, thanks",
                  "Ahh, thanks",
                  "I'm grateful for that, thank you",
                  "Perfecto, thanks",
                  "I appreciate you",
                  "I appreciate that",
                  "I appreciate it",
                  "I am very thankful for that",
                  "How kind, thank you",
                  "Great, thanks",
                  "Great, thank you",
                  "Gracias",
                  "Gotcha, thanks",
                  "Gotcha, thank you",
                  "Awesome thanks!",
                  "I'm grateful for that, thank you kindly",
                  "thank you pal",
                  "Wonderful, thank you!",
                  "Wonderful, thank you very much",
                  "Why thank you",
                  "Thx",
                  "Thnx",
                  "That's very kind",
                  "That's great, thanks",
                  "That is lovely, thanks",
                  "That is awesome, thanks!",
                  "Thanks bot",
                  "Thanks a lot",
                  "Okay, thanks!",
                  "Thank you so much",
                  "Perfect, thanks",
                  "Thank you my friend",
                  "Thank you kindly",
                  "Thank you for that",
                  "Thank you bot",
                  "Thank you",
                  "Right on, thanks very much",
                  "Right on, thanks a lot",
                  "Radical, thanks",
                  "Rad, thanks",
                  "Rad thank you",
                  "Wonderful, thanks!",
                  "Thanks"
              ],
              "answer": "You're welcome.",
              "score": 100.0,
              "id": 75,
              "source": "qna_chitchat_Professional.tsv",
              "metadata": [
                  {
                      "name": "editorial",
                      "value": "chitchat"
                  }
              ],
              "context": {
                  "isContextOnly": false,
                  "prompts": []
              }
          }
      ],
      "debugInfo": null,
      "activeLearningEnabled": true
    }
    ```

    `Thank you` sorusu bir genel konuşma sorusuyla tam olarak eşleştiği için Soru-Cevap Oluşturma tam olarak emindir ve 100 puan döndürür. QnA Maker ayrıca ilgili tüm soruların yanı sıra Chit-chat meta veri etiketi bilgilerini içeren meta veri özelliğini de iade etti.

## <a name="use-threshold-and-default-answer"></a>Eşik ve varsayılan yanıtı kullanma

Yanıt için minimum bir eşik isteyebilirsiniz. Eşik karşılanmazsa, varsayılan yanıt döndürülür.

1. Postman'da, yalnızca json gövdesini kullanıcıdan gelen bir konuşma sonu deyimiolarak değiştirin. JSON olmalıdır:

    ```json
    {
        'question':'size',
        'scoreThreshold':80.00
    }
    ```

    Sorunun puanı %71 olduğundan bilgi tabanı bu yanıtı bulamamalı ve bunun yerine bilgi tabanını oluşturduğunuzda sağladığınız varsayılan yanıtı döndürmelidir.

    Puan ve cevap da dahil olmak üzere döndürülen JSON yanıtı:

    ```json
    {
        "answers": [
            {
                "questions": [],
                "answer": "No good match found in KB.",
                "score": 0.0,
                "id": -1,
                "source": null,
                "metadata": []
            }
        ],
        "debugInfo": null,
        "activeLearningEnabled": true
    }
    ```

    QnA Maker bir `0`puan döndü , hangi güven anlamına gelir. Ayrıca varsayılan yanıtı döndürdü.

1. Eşik değerini %60'a değiştirin ve sorguyu yeniden isteyin:

    ```json
    {
        'question':'size',
        'scoreThreshold':60.00
    }
    ```

    İade edilen JSON cevabı buldu.

    ```json
    {
        "answers": [
            {
                "questions": [
                    "How large a knowledge base can I create?",
                    "What is the max size of a knowledge base?",
                    "How many GB of data can a knowledge base hold?"
                ],
                "answer": "The size of the knowledge base depends on the SKU of Azure search you choose when creating the QnA Maker service. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/tutorials/choosing-capacity-qnamaker-deployment) for more details.",
                "score": 71.1,
                "id": 3,
                "source": "https://docs.microsoft.com/azure/cognitive-services/qnamaker/troubleshooting",
                "metadata": [
                    {
                        "name": "link_in_answer",
                        "value": "true"
                    },
                    {
                        "name": "server",
                        "value": "qna_maker"
                    }
                ],
                "context": {
                    "isContextOnly": false,
                    "prompts": []
                }
            }
        ],
        "debugInfo": null,
        "activeLearningEnabled": true
    }
    ```