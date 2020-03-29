---
title: Duyarlılık Analizi konteyner örneğini doğrulayın
titleSuffix: Azure Cognitive Services
description: Duyarlılık Analizi konteyner örneğini nasıl doğrulayatılabildiğini öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: 2e201b4ec0d1364ea99b376171efabad65af0a50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70968625"
---
### <a name="verify-the-sentiment-analysis-container-instance"></a>Duyarlılık Analizi konteyner örneğini doğrulayın

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

    Konteynerde paketlenmiş olan model 0 ile 1 arasında değişen bir skor üretir, 0 negatif duygu ve 1 pozitif duygudur.

    Döndürülen JSON yanıtı güncelleştirilmiş metin girişi için duyarlılık içerir:

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
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

Artık yanıt yükü json verilerinin belgesini `id` özgün istek yük belgesiyle `id`ilişkilendirebiliriz. Daha fazla `0.98` puan çok olumlu bir duygu gösterir.