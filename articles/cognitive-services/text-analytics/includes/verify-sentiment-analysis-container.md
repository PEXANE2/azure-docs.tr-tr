---
title: Yaklaşım analizi kapsayıcı örneği doğrulayın
titleSuffix: Azure Cognitive Services
description: Yaklaşım analizi kapsayıcı örneği doğrulamak öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f69d573e9e70a505018e94cca354f363097cc1b8
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229196"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Yaklaşım analizi kapsayıcı örneği doğrulayın

1. Seçin **genel bakış** sekmesini ve IP adresini kopyalayın.
1. Yeni bir tarayıcı sekmesi açın ve IP adresini girin. For example, girin `http://<IP-address>:5000 (http://55.55.55.55:5000`). Kapsayıcının giriş sayfası görüntülenir, kapsayıcı tamamlanamayacağını çalışıyor.

    ![Çalıştığından emin olmak için kapsayıcı giriş sayfasını görüntüle](../media/how-tos/container-instance/swagger-docs-on-container.png).

1. Seçin **hizmet API açıklaması** bağlantı kapsayıcının swagger sayfasına gidin.

1. Birini **POST** API'ler ve select **deneyin**.  Bu örnek giriş dahil olmak üzere parametreler görüntülenir:

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

1. Giriş, aşağıdaki JSON içerikle değiştirin:

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

1. Ayarlama **showStats** true.

1. Seçin **yürütme** metinlerdeki belirlemek için.

    Burada negatif 0 ve 1 pozitif 1, 0-arasında bir puan kapsayıcıda paketlenmiştir modeli oluşturur.

    Döndürülen JSON yanıtı güncelleştirilmiş metin için yaklaşım içerir:

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

Biz belge artık ilişkilendirebilmek `id` yanıt yükünde 's JSON verilerinin özgün istek yükü belgeye `id`. Puanı görüyoruz birden fazla `.98`, kesin pozitif yaklaşım belirten.