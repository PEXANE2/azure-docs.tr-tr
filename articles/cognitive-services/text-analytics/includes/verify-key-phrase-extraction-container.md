---
title: Anahtar Tümcecik Ayıklama kapsayıcı örneğini doğrulama
titleSuffix: Azure Cognitive Services
description: Anahtar Tümcecik Ayıklama kapsayıcı örneğini nasıl doğrulayatılabildiğini öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 5c177517ec18d7526b1cc09da74e35cb5434766d
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876475"
---
### <a name="verify-the-key-phrase-extraction-container-instance"></a>Anahtar Tümcecik Ayıklama kapsayıcı örneğini doğrulama

1. Genel **Bakış** sekmesini seçin ve IP adresini kopyalayın.
1. Yeni bir tarayıcı sekmesi açın ve IP adresini girin. Örneğin, girin `http://<IP-address>:5000 (http://55.55.55.55:5000`). Kapsayıcının ana sayfası görüntülenir, bu da kapsayıcının çalıştığını bilmenizi sağlar.

    ![Çalıştığını doğrulamak için kapsayıcı giriş sayfasını görüntüleyin](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Kapsayıcının Swagger sayfasına gitmek için **Servis API Açıklama** bağlantısını seçin.

1. **POST** API'lerden herhangi birini seçin ve **Tazyit'i**seçin. Parametreler, bu örnek girişi içeren görüntülenir:

    ```json
    {
      "documents": [
        {
          "id": "1",
          "text": "Hello world"
        },
        {
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        },
        {
          "id": "4",
          "text": ":) :( :D"
        }
      ]
    }
    ```

1. Girişi aşağıdaki JSON içeriğiyle değiştirin:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. **Set showStats** için `true`.

1. Metnin duyarlılığını belirlemek için **Yürüt'ü** seçin.

    Kapsayıcıda paketlenmiş model, 0 ile 1 arasında değişen, 0 negatif ve 1 pozitif bir puan üretir.

    Döndürülen JSON yanıtı güncelleştirilmiş metin girişi için duyarlılık içerir:

    ```json
    {
      "documents": [
        {
          "id": "7",
          "keyPhrases": [
            "Great people",
            "great sessions",
            "KubeCon Conference",
            "Barcelona",
            "best conferences"
          ],
          "statistics": {
            "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

Artık yanıt yükü json verilerinin belgesini `id` özgün istek yük belgesiyle `id`ilişkilendirebiliriz. Elde edilen belge, `keyPhrases` ilgili giriş belgesinden ayıklanan anahtar tümceciklerin listesini içeren bir diziye sahiptir. Ayrıca, her bir belge `characterCount` gibi `transactionCount` çeşitli istatistikler vardır.