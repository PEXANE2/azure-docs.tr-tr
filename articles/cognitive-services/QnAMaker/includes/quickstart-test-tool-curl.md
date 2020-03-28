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
ms.openlocfilehash: f3a1a33b2fe859839deec587191b3b3a319c0cf8
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77495111"
---
Bu cURL tabanlı quickstart bilgi tabanından bir cevap alma ile size yol.

## <a name="prerequisites"></a>Ön koşullar

* Son [**cURL**](https://curl.haxx.se/).
* Sahip olmalısınız.
    * [QnA Maker hizmeti](../How-To/set-up-qnamaker-service-azure.md)
    * Sorular ve cevaplar ile eğitimli ve yayınlanan bilgi tabanı, önceki [quickstart](../Quickstarts/add-question-metadata-portal.md)itibaren, meta veri ve Chit sohbet ile yapılandırılmış.

> [!NOTE]
> Bilgi tabanınızdan bir soruya yanıt vermeye hazır olduğunuzda, bilgi tabanınızı [eğitmeli](../Quickstarts/create-publish-knowledge-base.md#save-and-train) ve [yayınlamanız](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) gerekir. Bilgi tabanınız yayımlandığında, Yanıt oluşturmak için **Yayımla** sayfası HTTP istek ayarlarını görüntüler. **cURL** sekmesi komut satırı aracından yanıt oluşturmak için gereken ayarları gösterir.

## <a name="use-metadata-to-filter-answer"></a>Yanıta filtre lemek için meta verileri kullanma

Meta verilere dayalı bir yanıt için önceki hızlı sorgu bilgi tabanını kullanın.

1. Bilgi tabanının **Ayarlar** sayfasından, bilgi tabanından yanıt oluşturmak için kullanılan örnek bir cURL komutunu görmek için **CURL** sekmesini seçin.
1. Komutu yeniden yapabilmek için komutu editable ortama (metin dosyası gibi) kopyalayın. Soru değerini, qna kümeleri için `service:qna_maker` filtre olarak kullanılacak şekilde aşağıdaki gibi edin.

    ```bash
    curl -X POST https://replace-with-your-resource-name.azurewebsites.net/qnamaker/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey replace-with-your-endpoint-key" -H "Content-type: application/json" -d "{'top':30, 'question':'size','strictFilters': [{'name':'service','value':'qna_maker'}]}"
    ```

    Soru sadece tek bir `size`kelime, iki QnA setleri de döndürebilir. Dizi, `strictFilters` yanıtı sadece `qna_maker` yanıtlara indirgeyecek şekilde söyler.

1. Yanıt, yalnızca filtre ölçütlerini karşılayan yanıtı içerir. Aşağıdaki cURL yanıtı okunabilirlik için biçimlendirilmiştir:

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

```json
Debug: {Enable:true}
```

1. Daha fazla bilgi görmek için hata ayıklama özelliğini içerecek şekilde cURL komutunu edin.

    ```bash
    curl -X POST https://replace-with-your-resource-name.azurewebsites.net/qnamaker/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey replace-with-your-endpoint-key" -H "Content-type: application/json" -d "{'question':'size', 'Debug':{'Enable':true}}"
    ```

1. Yanıt, yanıtla ilgili bilgileri içerir. Aşağıdaki JSON çıkışında, bazı hata ayıklama ayrıntıları kısalık için elips ile değiştirilmiştir.

    ```console
    {
        "answers": [
            {
                "questions": [
                    "How do I share a knowledge base with others?"
                ],
                "answer": "Sharing works at the level of a QnA Maker service, that is, all knowledge bases in the service will be shared. Read [here](https://docs.microsoft.com/azure/cognitive-services/qnamaker/how-to/collaborate-knowledge-base) to learn how to collaborate on a knowledge base.",
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

Özellik bir boolean değeridir.

```json
isTest:true
```

cURL komutu aşağıdaki gibi görünür:

```bash
curl -X POST https://replace-with-your-resource-name.azurewebsites.net/qnamaker/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey replace-with-your-endpoint-key" -H "Content-type: application/json" -d "{'question':'size', 'IsTest':true}"
```

JSON yanıtı, yayımlanmış bilgi bankası sorgusuyla aynı şema kullanır.

> [!NOTE]
> Test ve yayımlanmış bilgi tabanları tamamen aynıysa, test dizini kaynaktaki tüm bilgi tabanları arasında paylaşıldığından bazı küçük farklılıklar olabilir.

## <a name="use-curl-to-query-for-a-chit-chat-answer"></a>Chit-chat yanıtı için sorgulama kURL'yi kullanın

1. cURL özellikli terminalde, soru gibi `Thank you` kullanıcıdan gelen bir bot konuşma bitiş deyimi kullanın. Ayarlı başka özellik yok.

    ```bash
    curl -X POST https://replace-with-your-resource-name.azurewebsites.net/qnamaker/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey replace-with-your-endpoint-key" -H "Content-type: application/json" -d "{'question':'thank you'}"
    ```

1. CURL komutunu çalıştırın ve puan ve yanıt da dahil olmak üzere JSON yanıtını alın.

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

## <a name="use-curl-with-threshold-and-default-answer"></a>Eşik ve varsayılan yanıt ile cURL kullanma

Yanıt için minimum bir eşik isteyebilirsiniz. Eşik karşılanmazsa, varsayılan yanıt döndürülür.

1. %80 veya daha iyi bir `size` eşikle yanıt istemek için kendi kaynak adınızı, bilgi bankası kimliğinizi ve bitiş noktası anahtarınızı değiştirerek aşağıdaki cURL komutunu kullanın. Sorunun puanı %71 olduğundan bilgi tabanı bu yanıtı bulamamalı ve bunun yerine bilgi tabanını oluşturduğunuzda sağladığınız varsayılan yanıtı döndürmelidir.

    ```bash
    curl -X POST https://replace-with-your-resource-name.azurewebsites.net/qnamaker/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey replace-with-your-endpoint-key" -H "Content-type: application/json" -d "{'question':'size', 'scoreThreshold':80.00}"
    ```

1. CURL komutunu çalıştırın ve puan ve yanıt da dahil olmak üzere JSON yanıtını alın.

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

## <a name="use-curl-with-threshold-and-default-answer"></a>Eşik ve varsayılan yanıt ile cURL kullanma

Yanıt için minimum bir eşik isteyebilirsiniz. Eşik karşılanmazsa, varsayılan yanıt döndürülür.

1. % `threshold` 80 veya daha `size` iyi bir eşik ile bir cevap istemek için özelliği ekleyin. Soru puanı% 71 olduğu için bilgi tabanı bu cevabı bulamamalıdır. Sonuç, bilgi tabanını oluşturduğunuzda sağladığınız varsayılan yanıtı döndürür.

    ```bash
    curl -X POST https://replace-with-your-resource-name.azurewebsites.net/qnamaker/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey replace-with-your-endpoint-key" -H "Content-type: application/json" -d "{'question':'size', 'scoreThreshold':80.00}"
    ```

1. cURL komutunu çalıştırın ve JSON yanıtını alın.

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

    ```bash
    curl -X POST https://replace-with-your-resource-name.azurewebsites.net/qnamaker/knowledgebases/replace-with-your-knowledge-base-id/generateAnswer -H "Authorization: EndpointKey replace-with-your-endpoint-key" -H "Content-type: application/json" -d "{'question':'size', 'scoreThreshold':60.00}"
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
