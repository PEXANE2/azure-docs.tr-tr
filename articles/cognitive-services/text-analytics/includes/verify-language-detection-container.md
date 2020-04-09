---
title: Dil Algılama kapsayıcı örneğini doğrulama
titleSuffix: Azure Cognitive Services
description: Dil Algılama kapsayıcı örneğini nasıl doğrulayatılabildiğini öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: dc52586550f89ddae147d79458584331ed984eea
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876479"
---
### <a name="verify-the-language-detection-container-instance"></a>Dil Algılama kapsayıcı örneğini doğrulama

1. Genel **Bakış** sekmesini seçin ve IP adresini kopyalayın.
1. Yeni bir tarayıcı sekmesi açın ve IP adresini girin. Örneğin, girin `http://<IP-address>:5000 (http://55.55.55.55:5000`). Kapsayıcının ana sayfası görüntülenir, bu da kapsayıcının çalıştığını bilmenizi sağlar.

    ![Çalıştığını doğrulamak için kapsayıcı giriş sayfasını görüntüleyin](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Kapsayıcının Swagger sayfasına gitmek için **Servis API Açıklama** bağlantısını seçin.

1. **POST** API'lerden herhangi birini seçin ve **Tazyit'i**seçin. Parametreler, bu örnek girişi içeren görüntülenir:

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

1. **Set showStats** için `true`.

1. Metnin duyarlılığını belirlemek için **Yürüt'ü** seçin.

    Konteynerde paketlenmiş olan model 0 ile 1 arasında değişen bir skor üretir, 0 negatif duygu ve 1 pozitif duygudur.

    Döndürülen JSON yanıtı güncelleştirilmiş metin girişi için duyarlılık içerir:

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

Artık yanıt yükü JSON verilerinin belgelerini, ilgili lerine göre orijinal istek yükü `id`belgeleriyle ilişkilendirebiliriz. Her belge gibi çeşitli istatistikleri içeren `characterCount` `transactionCount`bağımsız olarak işlenir. Ayrıca, elde edilen her `detectedLanguages` belge `name`, `iso6391Name`, `score` ve algılanan her dil için dizi vardır. Birden çok dil algılandığında, `score` en olası dili belirlemek için kullanılır.