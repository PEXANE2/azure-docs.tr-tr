---
title: 'Hızlı başlangıç: form tanıyıcı istemci kitaplığı veya REST API'
titleSuffix: Azure Cognitive Services
description: Özel belgelerinizden anahtar/değer çiftlerini ve tablo verilerini çıkaran bir form işleme uygulaması oluşturmak için, bir form tanıyıcı istemci kitaplığı veya REST API kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 09/21/2020
ms.author: pafarley
zone_pivot_groups: programming-languages-set-formre
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: Form işleme, otomatik veri işleme
ms.openlocfilehash: c915e7753487eba284d89dbb480f9848e90c1ef1
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98132292"
---
# <a name="quickstart-use-the-form-recognizer-client-library-or-rest-api"></a>Hızlı başlangıç: form tanıyıcı istemci kitaplığı veya REST API kullanın

Seçtiğiniz dili kullanarak form tanıyıcısı ile çalışmaya başlayın. Azure form tanıyıcı, makine öğrenimi teknolojisini kullanarak otomatik veri işleme yazılımı oluşturmanıza imkan tanıyan bir bilişsel hizmettir. Form belgelerinizden metin, anahtar/değer çiftleri, seçim işaretleri, tablo verileri ve daha fazlasını tanımlayıp ayıklayın &mdash; . hizmet, özgün dosyadaki ilişkileri içeren yapılandırılmış verileri çıktı olarak verir. Form tanıyıcıyı REST API veya SDK aracılığıyla kullanabilirsiniz. SDK paketini yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. 

Form tanıyıcısını şu şekilde kullanın:

* [Düzeni çözümle](#analyze-layout)
* [Alındıları analiz etme](#analyze-receipts)
* [İş kartlarını çözümle](#analyze-business-cards)
* [Faturaları analiz etme](#analyze-invoices)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Özel modellerinizi yönetin](#manage-your-custom-models)

::: zone pivot="programming-language-csharp"

[!INCLUDE [C# SDK quickstart](../includes/quickstarts/csharp-sdk.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [Java SDK quickstart](../includes/quickstarts/java-sdk.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [NodeJS SDK quickstart](../includes/quickstarts/javascript-sdk.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [Python SDK quickstart](../includes/quickstarts/python-sdk.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end
