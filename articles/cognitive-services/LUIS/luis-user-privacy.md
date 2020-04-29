---
title: Dışarı aktarma & verileri silme-LUSıS
titleSuffix: Azure Cognitive Services
description: Verilerini görüntüleme, dışarı aktarma ve silme üzerinde tam denetime sahip olursunuz. Gizlilik ve uyumluluk sağlamak için müşteri verilerini silin.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78273357"
---
# <a name="export-and-delete-your-customer-data-in-language-understanding-luis-in-cognitive-services"></a>Bilişsel hizmetler 'de müşteri verilerinizi Language Understanding (LUSıS) olarak dışa ve silme

Gizlilik ve uyumluluk sağlamak için müşteri verilerini silin.

## <a name="summary-of-customer-data-request-features"></a>Müşteri verileri istek özelliklerinin Özeti
Language Understanding Intelligent Service (LUSıS), hizmeti çalıştırmak için müşteri içeriğini korur, ancak LUO kullanıcısı verilerini görüntüleme, dışarı aktarma ve silme üzerinde tam denetime sahiptir. Bu, LUıS Web [portalı](luis-reference-regions.md) veya [LUIS yazma (programlı olarak da bilinir) API 'leri](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f)aracılığıyla yapılabilir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

Müşteri içeriği, Microsoft bölgesel Azure depolama 'da şifrelenmiş olarak depolanır ve şunları içerir:

- Kayıt sırasında toplanan kullanıcı hesabı içeriği
- Modelleri derlemek için gereken eğitim verileri
- Modeli iyileştirmenize yardımcı olmak için [etkin öğrenme](luis-concept-review-endpoint-utterances.md) tarafından kullanılan günlüğe kaydedilmiş kullanıcı sorguları
  - Kullanıcılar isteğe ekleyerek `&log=false` sorgu günlüğünü kapatabilir ve [buradaki ayrıntıları burada](troubleshooting.md#how-can-i-disable-the-logging-of-utterances) bulabilirsiniz

## <a name="deleting-customer-data"></a>Müşteri verilerini silme
LUıS kullanıcıları, LUıS Web portalı ya da LUıS yazma (programlı olarak da bilinir) API 'Leri aracılığıyla herhangi bir Kullanıcı içeriğini silmek için tam denetime sahiptir. Aşağıdaki tabloda her ikisiyle yardımcı olan bağlantılar görüntülenmektedir:

| | **Kullanıcı Hesabı** | **Uygulama** | **Örnek söyleyler** | **Son Kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **Portal** | [Bağlantısının](luis-concept-data-storage.md#delete-an-account) | [Bağlantısının](luis-how-to-start-new-app.md#delete-app) | [Bağlantısının](luis-concept-data-storage.md#utterances-in-an-intent) | [Etkin öğrenimi](luis-how-to-review-endpoint-utterances.md#disable-active-learning)<br>[Günlüğe kaydedilen Utterslar](luis-concept-data-storage.md#disable-logging-utterances) |
| **API'ler** | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c4c) | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c39) | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0b) | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/58b6f32139e2bb139ce823c9) |


## <a name="exporting-customer-data"></a>Müşteri verilerini dışa aktarma
LUıS kullanıcıları portalda verileri görüntülemek için tam denetime sahiptir, ancak bu, LUıS yazma (programlı olarak da bilinir) API 'Leri aracılığıyla verilmelidir. Aşağıdaki tabloda, LUıS yazma (programlı olarak da bilinir) API 'Leri aracılığıyla veri dışarı aktarmaları ile ilgili bağlantılar gösterilmektedir:

| | **Kullanıcı Hesabı** | **Uygulama** | **Utterance (s)** | **Son Kullanıcı sorguları** |
| --- | --- | --- | --- | --- |
| **API'ler** | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c48) | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c0a) | [Bağlantısının](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c36) |

## <a name="location-of-active-learning"></a>Etkin öğrenimi konumu

[Etkin öğrenimi](luis-how-to-review-endpoint-utterances.md#enable-active-learning)etkinleştirmek için, yayımlanan Luo uç noktalarında alınan kullanıcıların günlüğe kaydettiği, aşağıdaki Azure coğrafi bölgelerde depolanır:

* [Avrupa](#europe)
* [Avustralya](#australia)
* [Birleşik Devletler](#united-states)

Etkin öğrenme verileri (aşağıda ayrıntılı) dışında, Lua, [Bölgesel hizmetler için veri depolama uygulamalarına](https://azuredatacentermap.azurewebsites.net/)uyar.

### <a name="europe"></a>Avrupa

[Eu.Luis.ai](https://eu.luis.ai) portalı ve Avrupa yazma (programlı API 'ler olarak da bilinir), Azure 'un Avrupa coğrafi konumunda barındırılır. Eu.luis.ai portalı ve Avrupa yazma (programlı API 'Ler olarak da bilinir), uç noktaların aşağıdaki Azure coğrafi noktalara dağıtımını destekler:

* Avrupa
* Fransa
* Birleşik Krallık

Bu Azure coğrafi noktalara dağıtım yaparken, uygulamanızın son kullanıcılarından gelen uç nokta tarafından alınan utmalslar, etkin öğrenme için Azure Avrupa coğrafi konumunda depolanır. Etkin öğrenmeyi devre dışı bırakabilirsiniz, bkz. [etkin öğrenmeyi devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning)bırakma. Depolanan söyleymeleri yönetmek için bkz. [söylenişi 'i silme](luis-how-to-review-endpoint-utterances.md#delete-utterance).

### <a name="australia"></a>Avustralya

[Au.Luis.ai](https://au.luis.ai) portalı ve Avustralya yazma (programlı API 'ler olarak da bilinir), Azure 'un Avustralya coğrafi konumunda barındırılır. Au.luis.ai portalı ve Avustralya yazma (programlı API 'Ler olarak da bilinir), uç noktaların aşağıdaki Azure coğrafi noktalara dağıtımını destekler:

* Avustralya

Bu Azure coğrafi noktalara dağıtım yaparken, uygulamanızın son kullanıcılarından gelen uç nokta tarafından alınan araslar, etkin öğrenme için Azure 'ın Avustralya Coğrafya 'da depolanır. Etkin öğrenmeyi devre dışı bırakabilirsiniz, bkz. [etkin öğrenmeyi devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning)bırakma. Depolanan söyleymeleri yönetmek için bkz. [söylenişi 'i silme](luis-how-to-review-endpoint-utterances.md#delete-utterance).

### <a name="united-states"></a>Amerika Birleşik Devletleri

[Luis.ai](https://www.luis.ai) portalı ve Birleşik Devletler yazma (programlı API 'ler olarak da bilinir), Azure 'un Birleşik Devletler Coğrafya içinde barındırılır. Luis.ai portalı ve Birleşik Devletler yazma (programlı API 'Ler olarak da bilinir), uç noktaların aşağıdaki Azure coğrafi noktalara dağıtımını destekler:

* Avrupa veya Avustralya yazma bölgeleri tarafından desteklenmeyen Azure coğrafi bölgeler

Bu Azure coğrafi noktalara dağıtım yaparken, uygulamanızın son kullanıcılarından gelen uç nokta tarafından alınan araslar, etkin öğrenme için Azure Birleşik Devletler Coğrafya 'da depolanır. Etkin öğrenmeyi devre dışı bırakabilirsiniz, bkz. [etkin öğrenmeyi devre dışı](luis-how-to-review-endpoint-utterances.md#disable-active-learning)bırakma. Depolanan söyleymeleri yönetmek için bkz. [söylenişi 'i silme](luis-how-to-review-endpoint-utterances.md#delete-utterance).


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUıN bölgeleri başvurusu](./luis-reference-regions.md)
