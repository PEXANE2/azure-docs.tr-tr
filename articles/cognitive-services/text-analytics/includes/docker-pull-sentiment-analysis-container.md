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
ms.openlocfilehash: a3db0e2ffdd4a75f02634ca2227c3c41416d4f65
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83588433"
---
#### <a name="docker-pull-for-the-sentiment-analysis-v3-container"></a>Yaklaşım Analizi v3 kapsayıcısı için Docker Pull

Yaklaşım Analizi kapsayıcısı v3 kapsayıcısı, çeşitli dillerde kullanılabilir. Ingilizce kapsayıcısının kapsayıcısını indirmek için aşağıdaki komutu kullanın. 

```
docker pull mcr.microsoft.com/azure-cognitive-services/sentiment:3.0-en
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