---
title: "Hızlı başlangıç: BLOB depolama 'da depolanan konuşmayı tanıma-konuşma hizmeti"
titleSuffix: Azure Cognitive Services
description: Daha sonra belirlenecek
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 10/28/2019
ms.author: erhopf
zone_pivot_groups: programming-languages-set-two
ms.openlocfilehash: ef61139056ad194d89e2a67cb4ac1b1414a3380b
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74828836"
---
Bu hızlı başlangıçta, bir [SAS blobu](https://aka.ms/ignite2019/speech/placeholder)içinde depolanan konuşmayı tanımak Için [toplu iş dökümü REST API](../../../batch-transcription.md) kullanacaksınız. Birkaç önkoşulu karşıladıktan sonra, REST API kullanarak konuşmayı tanıma yalnızca birkaç adım sürer:
> [!div class="checklist"]
> * Konuşma konuşmaya başlamak için JSON isteğini konuşma hizmetine gönderin.
> * Döküm durumunu denetleyin.
> * Tamamlandığında, döküm sonuçlarını indirin.