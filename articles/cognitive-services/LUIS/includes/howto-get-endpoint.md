---
title: include dosyası
description: include dosyası
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.date: 02/14/2020
ms.subservice: language-understanding
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 5b1b361778de145a5e32a07bb0164ff2293d9a1a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77279479"
---
**Azure Kaynakları** sayfasında (sol menü) **Yönet** bölümünde Örnek **Sorgu** URL'sini kopyalayın ve ardından yeni bir tarayıcı sekmesine yapıştırın.

Endpoint URL'si, APP-ID ve KEY-ID'nin yerini alan kendi uygulama kimliğiniz ve bitiş noktası anahtarınızla aşağıdaki biçime benzer:

```console
https://westus.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```