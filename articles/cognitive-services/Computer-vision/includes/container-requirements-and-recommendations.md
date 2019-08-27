---
title: Kapsayıcı gereksinimleri ve önerileri.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 8/26/2019
ms.author: dapine
ms.openlocfilehash: 5da088bf0356dd54d98ec5393fd2db8068f9c666
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034481"
---
Aşağıdaki tabloda, her bir Metin Tanıma kapsayıcısı için ayrılacak minimum ve önerilen CPU çekirdekleri ve bellek açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |TPS<br>(En düşük, en yüksek)|
|-----------|---------|-------------|--|
|Metin Tanıma|1 çekirdek, 8 GB bellek, 0,5 TPS|2 çekirdek, 8 GB bellek, 1 TPS|0,5, 1|

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS-saniye başına işlem

Çekirdek ve bellek, `--cpus` `docker run` komutunun bir parçası `--memory` olarak kullanılan ve ayarlarına karşılık gelir.