---
title: 'Hızlı başlangıç: konuşmayı konuşmaya çevirme-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/20/2019
ms.author: yulili
ms.openlocfilehash: 805193c08e57ddc18dfca0a78c6b58cf895baec1
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74817450"
---
Bu hızlı başlangıçta konuşmayı bir dilden başka bir dilde konuşmaya etkileşimli bir şekilde çevirmek için [konuşma SDK 'sını](~/articles/cognitive-services/speech-service/speech-sdk.md) kullanacaksınız. Birkaç önkoşul karşıladıktan sonra, konuşmayı konuşmaya çevirme özelliği yalnızca altı adımdan yararlanır:
> [!div class="checklist"]
> * Abonelik anahtarınızdan ve bölgenizde bir ````SpeechTranslationConfig```` nesnesi oluşturun.
> * Kaynak ve hedef dilleri belirtmek için ````SpeechTranslationConfig```` nesnesini güncelleştirin.
> * Konuşma çıkışı ses adını belirtmek için ````SpeechTranslationConfig```` nesnesini güncelleştirin.
> * Yukarıdaki ````SpeechTranslationConfig```` nesnesini kullanarak bir ````TranslationRecognizer```` nesnesi oluşturun.
> * ````TranslationRecognizer```` nesnesini kullanarak, tek bir söylik için tanıma sürecini başlatın.
> * Döndürülen ````TranslationRecognitionResult```` inceleyin.
