---
title: Verileri silme & dışa aktarma - LUIS
titleSuffix: Azure Cognitive Services
description: Verilerini görüntüleme, dışa aktarma ve silme konusunda tam denetime sahipsiniz. Gizlilik ve uyumluluk sağlamak için müşteri verilerini silin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 04/02/2019
ms.author: diberry
ms.openlocfilehash: 4e3e0d04b0086905b80e26fb4f838c36b5b5545e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273357"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Bilişsel Hizmetlerde Dil Anlayışı (LUIS) müşteri verilerinizi dışa aktarma ve silme

Gizlilik ve uyumluluk sağlamak için müşteri verilerini silin.

## <a name="summary-of-customer-data-request-features"></a>Müşteri veri seçimi özelliklerinin özeti
Dil Anlama Akıllı Hizmet (LUIS) hizmeti çalıştırmak için müşteri içeriğini korur, ancak LUIS kullanıcı görüntüleme, dışa aktarma ve verilerini silme üzerinde tam kontrole sahiptir. Bu LUIS web [portalı](luis-reference-regions.md) veya [LUIS Yazma (programlı olarak da bilinir) API'ler](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f)üzerinden yapılabilir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Müşteri içeriği Microsoft bölgesel Azure depolama alanında şifrelenmiş olarak depolanır ve şunları içerir:

- Kayıt sırasında toplanan kullanıcı hesabı içeriği
- Modelleri oluşturmak için gerekli eğitim verileri
- Modeli geliştirmeye yardımcı olmak için [etkin öğrenme](luis-concept-review-endpoint-utterances.md) tarafından kullanılan günlüğe kaydedilmiş kullanıcı sorguları
  - Kullanıcılar istek, ayrıntıları [burada](troubleshooting.md#how-can-i-disable-the-logging-of-utterances) `&log=false` ekleyerek sorgu günlüğü kapatabilirsiniz

## <a name="deleting-customer-data"></a>Müşteri verilerini silme
LUIS kullanıcıları, LUIS web portalı veya LUIS Authoring (Programlı) API'leri aracılığıyla herhangi bir kullanıcı içeriğini silmek için tam kontrole sahiptir. Aşağıdaki tabloda her ikisine de yardımcı olan bağlantılar görüntülenir:

| | **Kullanıcı Hesabı** | **Uygulama** | **Örnek Söylenme(ler)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **Portal** | [Bağlantı](luis-concept-data-storage.md#delete-an-account) | [Bağlantı](luis-how-to-start-new-app.md#delete-app) | [Bağlantı](luis-concept-data-storage.md#utterances-in-an-intent) | [Aktif öğrenme söyleyişleri](luis-how-to-review-endpoint-utterances.md#disable-active-learning)<br>[Günlüğe Kaydedilmiş Söyleyinatlar](luis-concept-data-storage.md#disable-logging-utterances) |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma
LUIS kullanıcıları portaldaki verileri görüntülemek için tam kontrole sahiptir, ancak luis authoring (Programlı) API'leri aracılığıyla dışa aktarılmalıdır. Aşağıdaki tabloda, LUIS Authoring (Programlı) API'ler aracılığıyla veri dışa aktarmaya yardımcı olan bağlantılar görüntülenir:

| | **Kullanıcı Hesabı** | **Uygulama** | **Söylenme(ler)** | **Son kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **API'ler** | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Bağlantı](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>Aktif öğrenmenin yeri

Etkin [öğrenmeyi](luis-how-to-review-endpoint-utterances.md#enable-active-learning)etkinleştirmek için, kullanıcıların yayınlanan LUIS uç noktalarından alınan günlük lerini aşağıdaki Azure coğrafyalarında saklar:

* [Avrupa](#europe)
* [Avustralya](#australia)
* [Birleşik Devletler](#united-states)

Luis, aktif öğrenme verileri (aşağıda ayrıntılı olarak) [dışında, bölgesel hizmetler için veri depolama uygulamalarını](https://azuredatacentermap.azurewebsites.net/)takip eder.

### <a name="europe"></a>Avrupa

[eu.luis.ai](https://eu.luis.ai) portalı ve Europe Authoring (Programlı API olarak da bilinir) Azure'un Avrupa coğrafyasında barındırılır. eu.luis.ai portalı ve Europe Authoring (Programlı API'ler olarak da bilinir) aşağıdaki Azure coğrafyalarına uç noktaların dağıtılmasını destekler:

* Avrupa
* Fransa
* Birleşik Krallık

Bu Azure coğrafyalarına dağıtılırken, uygulamanızın son kullanıcılarından son noktaya kadar alınan sözcükler, etkin öğrenme için Azure'un Avrupa coğrafyasında depolanır. Aktif öğrenmeyi devre dışı kullanabilirsiniz, [bkz.](luis-how-to-review-endpoint-utterances.md#disable-active-learning) Depolanan açıklamaları yönetmek için [bkz.](luis-how-to-review-endpoint-utterances.md#delete-utterance)

### <a name="australia"></a>Avustralya

[au.luis.ai](https://au.luis.ai) portalı ve Avustralya Yazarlık (Programlı API'ler olarak da bilinir) Azure'un Avustralya coğrafyasında barındırılır. au.luis.ai portalı ve Avustralya Yazarlığı (Programlı API'ler olarak da bilinir) aşağıdaki Azure coğrafyalarına uç noktaların dağıtılmasını destekler:

* Avustralya

Bu Azure coğrafyalarına dağıtılırken, uygulamanızın son kullanıcılarından son noktaya kadar alınan sözcükler, etkin öğrenme için Azure'un Avustralya coğrafyasında depolanır. Aktif öğrenmeyi devre dışı kullanabilirsiniz, [bkz.](luis-how-to-review-endpoint-utterances.md#disable-active-learning) Depolanan açıklamaları yönetmek için [bkz.](luis-how-to-review-endpoint-utterances.md#delete-utterance)

### <a name="united-states"></a>Amerika Birleşik Devletleri

[luis.ai](https://www.luis.ai) portalı ve Abd Yazarlık (Programlı API'ler olarak da bilinir) Azure'un ABD coğrafyasında barındırılır. luis.ai portalı ve Abd Yazarlığı (Programlı API'ler olarak da bilinir) aşağıdaki Azure coğrafyalarına uç noktaların dağıtılmasını destekler:

* Avrupa veya Avustralya yazma bölgeleri tarafından desteklenmeyen Azure coğrafyaları

Bu Azure coğrafyalarına dağıtılırken, uygulamanızın son kullanıcılarından son noktaya kadar alınan sözcükler, etkin öğrenme için Azure'un ABD coğrafyasında depolanır. Aktif öğrenmeyi devre dışı kullanabilirsiniz, [bkz.](luis-how-to-review-endpoint-utterances.md#disable-active-learning) Depolanan açıklamaları yönetmek için [bkz.](luis-how-to-review-endpoint-utterances.md#delete-utterance)


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS bölgeleri referans](./luis-reference-regions.md)
