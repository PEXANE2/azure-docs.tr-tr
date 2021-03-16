---
title: 'Hızlı başlangıç: form tanıyıcı istemci kitaplığı veya REST API'
titleSuffix: Azure Cognitive Services
description: Özel belgelerinizden anahtar/değer çiftlerini ve tablo verilerini çıkaran bir form işleme uygulaması oluşturmak için, bir form tanıyıcı istemci kitaplığı veya REST API kullanın.
services: cognitive-services
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: forms-recognizer
ms.topic: quickstart
ms.date: 01/29/2021
ms.author: lajanuar
zone_pivot_groups: programming-languages-set-formre
ms.custom: devx-track-js, devx-track-csharp, cog-serv-seo-aug-2020
keywords: Form işleme, otomatik veri işleme
ms.openlocfilehash: 43182892aa80f6226ffb5177d0be2e24d15ead5c
ms.sourcegitcommit: 3ea12ce4f6c142c5a1a2f04d6e329e3456d2bda5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2021
ms.locfileid: "103467235"
---
# <a name="quickstart-use-the-form-recognizer-client-library-or-rest-api"></a>Hızlı başlangıç: form tanıyıcı istemci kitaplığı veya REST API kullanın

Seçtiğiniz geliştirme dilini kullanarak form tanıyıcısı ile çalışmaya başlayın. Azure form tanıyıcı, makine öğrenimi teknolojisini kullanarak otomatik veri işleme yazılımı oluşturmanıza imkan tanıyan bir bilişsel hizmettir. Form belgelerinizden metin, anahtar/değer çiftleri, seçim işaretleri, tablo verileri ve daha fazlasını tanımlayıp ayıklayın &mdash; . hizmet, özgün dosyadaki ilişkileri içeren yapılandırılmış verileri çıktı olarak verir. Form tanıyıcıyı REST API veya SDK aracılığıyla kullanabilirsiniz. SDK paketini yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. 

Form tanıyıcısını şu şekilde kullanın:

* [Düzeni çözümle](#analyze-layout)
* [Faturaları analiz etme](#analyze-invoices)
* [Özel bir modeli eğitme](#train-a-custom-model)
* [Formları özel bir model ile analiz etme](#analyze-forms-with-a-custom-model)
* [Alındıları analiz etme](#analyze-receipts)
* [İş kartlarını çözümle](#analyze-business-cards)
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
