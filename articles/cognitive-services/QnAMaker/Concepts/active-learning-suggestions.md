---
title: Aktif öğrenme önerileri - QnA Maker
description: Aktif öğrenme önerileri, soru ve yanıt çiftinize kullanıcı gönderimlerine dayalı alternatif sorular önererek bilgi tabanınızın kalitesini artırmanızı sağlar.
ms.topic: conceptual
ms.date: 03/19/2020
ms.openlocfilehash: edbe06b12fbb97473b28ccca968fd3e7d8366152
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804230"
---
# <a name="active-learning-suggestions"></a>Aktif öğrenme önerileri

_Aktif öğrenme önerileri_ özelliği, soru ve yanıt çiftinize kullanıcı gönderimlerine dayalı alternatif sorular önererek bilgi tabanınızın kalitesini artırmanızı sağlar. Bu önerileri, varolan sorulara ekleyerek veya reddederek gözden geçirin.

Bilgi tabanınız otomatik olarak değişmez. Herhangi bir değişikliğin etkili olması için önerileri kabul etmelisiniz. Bu öneriler soru ekler, ancak varolan soruları değiştirmez veya kaldırmaz.

## <a name="what-is-active-learning"></a>Aktif öğrenme nedir?

QnA Maker örtük ve açık geribildirim ile yeni soru varyasyonları öğrenir.

* [Örtük geri bildirim](#how-qna-makers-implicit-feedback-works) – Ranker, bir kullanıcı sorusunun çok yakın olan puanları içeren birden çok yanıtı olduğunda anlar ve bunu geri bildirim olarak görür. Bunun olması için bir şey yapmana gerek yok.
* [Açık geri bildirim](#how-you-give-explicit-feedback-with-the-train-api) – Puanlarda çok az değişiklik olan birden çok yanıt bilgi tabanından döndürüldüğünde, istemci uygulaması kullanıcıya hangi sorunun doğru soru olduğunu sorar. Kullanıcının açık geri bildirimi [Tren API'si](../How-to/improve-knowledge-base.md#train-api)ile QnA Maker'a gönderilir.

Her iki yöntem de sıralamacıya kümelenmiş benzer sorgularla sağlar.

## <a name="how-active-learning-works"></a>Aktif öğrenme nasıl çalışır?

Aktif öğrenme, QnA Maker tarafından döndürülen en iyi birkaç yanıtın puanlarına göre tetiklenir. Sorguyla eşleşen QnA çiftleri arasındaki puan farkları küçük bir aralıkta yatıyorsa, sorgu olası QnA çiftlerinin her biri için olası bir öneri (alternatif soru olarak) olarak kabul edilir. Belirli bir QnA çifti için önerilen soruyu kabul ettiğinizde, diğer çiftler için reddedilir. Önerileri kabul ettikten sonra, kaydetmek ve tren hatırlamak gerekir.

Etkin öğrenme, uç noktaların makul miktarda ve çeşitli kullanım sorguları aldığı durumlarda mümkün olan en iyi önerileri verir. Her 30 dakikada bir 5 veya daha fazla benzer sorgu kümelendiğinde, QnA Maker kullanıcı tabanlı soruları bilgi bankası tasarımcısına kabul etmesi veya reddetmesi için önerir. Tüm öneriler benzerlik tarafından bir araya toplanır ve alternatif sorular için en iyi öneriler, son kullanıcıların belirli sorgularının sıklığına göre görüntülenir.

Sorular QnA Maker portalında önerildiği nde, bu önerileri gözden geçirmeniz ve reddetmeniz veya reddetmeniz gerekir. Önerileri yönetmek için bir API yok.

## <a name="turn-on-active-learning"></a>Etkin öğrenmeyi açma

Varsayılan olarak, etkin öğrenme **kapalıdır.**
Aktif öğrenmeyi kullanmak için:
* QnA Maker'ın bilgi tabanınız için alternatif sorular toplaması için [aktif öğrenmeyi açmanız](../How-To/use-active-learning.md#turn-on-active-learning-for-alternate-questions) gerekir.
* Önerilen alternatif soruları görmek için, Edit sayfasında [görünüm seçeneklerini kullanın.](../How-To/improve-knowledge-base.md#view-suggested-questions)

## <a name="how-qna-makers-implicit-feedback-works"></a>QnA Maker'ın örtük geri bildirimi nasıl çalışır?

QnA Maker'ın örtük geri bildirimi, skor yakınlığını belirlemek için bir algoritma kullanır ve ardından aktif öğrenme önerileri yapar. Yakınlığı belirlemek için algoritma basit bir hesaplama değildir. Aşağıdaki örnekteki aralıkların düzeltilmesi amaçlanmaz, ancak yalnızca algoritmanın etkisini anlamak için bir kılavuz olarak kullanılmalıdır.

Bir sorunun puanı %80 gibi son derece kendinden emin olduğunda, aktif öğrenme için düşünülen puan aralığı yaklaşık %10'dur. Güven puanı %40 gibi azaldıkça, puan aralığı da yaklaşık %4'lük bir azalma sağlar.

QnA Maker'S generateAnswer bir sorgu aşağıdaki JSON yanıtı, A, B ve C için puanları yakın ve öneri olarak kabul edilecektir.

```json
{
  "activeLearningEnabled": true,
  "answers": [
    {
      "questions": [
        "Q1"
      ],
      "answer": "A1",
      "score": 80,
      "id": 15,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    },
    {
      "questions": [
        "Q2"
      ],
      "answer": "A2",
      "score": 78,
      "id": 16,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    },
    {
      "questions": [
        "Q3"
      ],
      "answer": "A3",
      "score": 75,
      "id": 17,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    },
    {
      "questions": [
        "Q4"
      ],
      "answer": "A4",
      "score": 50,
      "id": 18,
      "source": "Editorial",
      "metadata": [
        {
          "name": "topic",
          "value": "value"
        }
      ]
    }
  ]
}
```

QnA Maker hangi cevabın en iyi cevap olduğunu bilmeyecek. En iyi yanıtı seçmek ve yeniden eğitmek için QnA Maker portalının öneri listesini kullanın.


## <a name="how-you-give-explicit-feedback-with-the-train-api"></a>Tren API'si ile nasıl açık geri bildirimde

QnA Maker cevaplardan hangisinin en iyi cevap olduğu konusunda açık geri bildirime ihtiyaç duyar. En iyi yanıtın nasıl belirlendiği size kalmış ve şunları içerebilir:

* Yanıtlardan birini seçerek kullanıcı geri bildirimi.
* Kabul edilebilir bir puan aralığını belirlemek gibi iş mantığı.
* Hem kullanıcı geri bildirimi hem de iş mantığının birleşimi.

Kullanıcı seçtikten sonra QnA Maker'a doğru yanıtı göndermek için [Tren API'sini](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/train) kullanın.

## <a name="next-step"></a>Sonraki adım

> [!div class="nextstepaction"]
> [Bilgi tabanını sorgula](query-knowledge-base.md)