---
title: Yaklaşım Analizi kapsayıcısı için Docker Pull
titleSuffix: Azure Cognitive Services
description: Yaklaşım Analizi kapsayıcısı için Docker Pull komutu
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 549ecf940c53267db6b3caa576c4c24db414337a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90906035"
---
#### <a name="docker-pull-for-the-sentiment-analysis-v3-container"></a>Yaklaşım Analizi v3 kapsayıcısı için Docker Pull

Yaklaşım Analizi kapsayıcısı v3 kapsayıcısı, çeşitli dillerde kullanılabilir. Ingilizce kapsayıcısının kapsayıcısını indirmek için aşağıdaki komutu kullanın. 

```
docker pull mcr.microsoft.com/azure-cognitive-services/textanalytics/sentiment:3.0-en
```

Kapsayıcıyı başka bir dile indirmek için `en` aşağıdaki dil kodlarından biriyle değiştirin. 

| Metin Analizi kapsayıcı | Dil kodu |
|--|--|
| İngilizce | `en` |
| İspanyolca | `es` |
| Fransızca | `fr` |
| İtalyanca | `it` |
| Almanca | `de` |
| Çince-Basitleştirilmiş | `zh` |
| Çince-Geleneksel | `zht` |
| Japonca | `ja` |
| Portekizce | `pt` |
| Felemenkçe | `nl` |

Metin Analizi kapsayıcıları için kullanılabilir etiketlerin tam açıklaması için bkz. [Docker Hub](https://go.microsoft.com/fwlink/?linkid=2018654).