---
title: 'Hızlı başlangıç: form tanıyıcı istemci kitaplığı'
titleSuffix: Azure Cognitive Services
description: Özel belgelerinizden anahtar/değer çiftlerini ve tablo verilerini çıkaran bir form işleme uygulaması oluşturmak için, form tanıyıcı istemci Kitaplığı ' nı kullanın.
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
ms.openlocfilehash: d099feff76d74f358a7d7958fb10406a5b8dc188
ms.sourcegitcommit: e46f9981626751f129926a2dae327a729228216e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98029041"
---
# <a name="quickstart-use-the-form-recognizer-client-library"></a>Hızlı başlangıç: form tanıyıcı istemci kitaplığını kullanma

Seçtiğiniz dili kullanarak form tanıyıcısı ile çalışmaya başlayın. Azure form tanıyıcı, makine öğrenimi teknolojisini kullanarak otomatik veri işleme yazılımı oluşturmanıza imkan tanıyan bir bilişsel hizmettir. Formdan metin, anahtar/değer çiftleri ve tablo verilerini belirleyip ayıklayın &mdash; . hizmet, özgün dosyadaki ilişkileri içeren yapılandırılmış verileri çıktı olarak verir. SDK paketini yüklemek için bu adımları izleyin ve temel görevler için örnek kodu deneyin. Form tanıyıcı istemci kitaplığı şu anda form tanıyıcı hizmeti 'nin v 2.0 'ı hedefliyor.

Form tanıyıcı istemci kitaplığını kullanarak şunları yapın:

* [Form içeriğini tanı](#recognize-form-content)
* [Alındıları tanı](#recognize-receipts)
* [İş kartlarını tanıma](#recognize-business-cards)
* [Faturaları tanıma](#recognize-invoices)
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
