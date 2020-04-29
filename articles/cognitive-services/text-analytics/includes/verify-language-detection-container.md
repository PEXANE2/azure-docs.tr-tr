---
title: Dil Algılama kapsayıcı örneğini doğrulama
titleSuffix: Azure Cognitive Services
description: Dil Algılama kapsayıcı örneğini doğrulamayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: dc52586550f89ddae147d79458584331ed984eea
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876479"
---
### <a name="verify-the-language-detection-container-instance"></a>Dil Algılama kapsayıcı örneğini doğrulama

1. **Genel bakış** sekmesini SEÇIN ve IP adresini kopyalayın.
1. Yeni bir tarayıcı sekmesi açın ve IP adresini girin. Örneğin, girin `http://<IP-address>:5000 (http://55.55.55.55:5000`). Kapsayıcının ana sayfası görüntülenir ve bu da kapsayıcının çalıştığını bilmenizi sağlar.

    ![Çalıştığını doğrulamak için kapsayıcı giriş sayfasını görüntüleme](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Kapsayıcının Swagger sayfasına gitmek için **hizmet API 'Si açıklaması** bağlantısını seçin.

1. **Post** API 'lerinden birini seçin ve **dene**' yi seçin. Parametreler, bu örnek girişi içeren görüntülenir:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. **Showstats** öğesini olarak `true`ayarlayın.

1. Metnin yaklaşımını belirlemek için **Yürüt** ' ü seçin.

    Kapsayıcıda paketlenmiş model, 0 ile 1 arasında değişen bir puan üretir; burada 0 negatif yaklaşım ve 1 pozitif yaklaşım olur.

    Döndürülen JSON yanıtı, güncelleştirilmiş metin girişi için yaklaşım içerir:

    ```json
    {
      "documents": [
        {
          "id": "1",
          "detectedLanguages": [
            {
              "name": "English",
              "iso6391Name": "en",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 11,
            "transactionsCount": 1
          }
        },
        {
          "id": "2",
          "detectedLanguages": [
            {
              "name": "French",
              "iso6391Name": "fr",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 21,
            "transactionsCount": 1
          }
        },
        {
          "id": "3",
          "detectedLanguages": [
            {
              "name": "Spanish",
              "iso6391Name": "es",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 65,
            "transactionsCount": 1
          }
        },
        {
          "id": "4",
          "detectedLanguages": [
            {
              "name": "French",
              "iso6391Name": "fr",
              "score": 1
            }
          ],
          "statistics": {
            "charactersCount": 8,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 4,
        "validDocumentsCount": 4,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 4
      }
    }
    ```

Artık yanıt yükünün JSON verilerinin belgelerini, bunlara karşılık gelen `id`özgün istek yükü belgeleriyle ilişkilendirebiliriz. Her belge, `characterCount` ve `transactionCount`gibi çeşitli istatistiklerle bağımsız olarak değerlendirilir. Ayrıca, her `detectedLanguages` elde edilen belge `name`,, ve algılanan her `iso6391Name`dil için `score` ,, ve ile dizi içerir. Birden çok dil algılandığında `score` , en olası dili belirlemede kullanılır.