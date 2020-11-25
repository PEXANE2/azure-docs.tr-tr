---
title: Kapsayıcı gereksinimleri ve önerileri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/23/2020
ms.author: aahi
ms.openlocfilehash: 4697be519eee96778eecdf37f7b358a88ad886c6
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96006925"
---
> [!NOTE]
> Gereksinimler ve öneriler, 29 satır ve toplam 803 karakter içeren taranmış bir iş harfinin 8 MB 'lik bir görüntüsünü kullanarak saniyede tek bir istek ile kıyaslamayı temel alır.

Aşağıdaki tabloda, her bir okuma kapsayıcısı için kaynakların en düşük ve önerilen ayırması açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |
|-----------|---------|-------------|
| Okuma 2,0-Önizleme | 1 çekirdek, 8 GB bellek |  8 çekirdek, 16 GB bellek |
| Okuma 3,2-Önizleme | 8 çekirdek, 16 GB bellek | 8 çekirdek, 24 GB bellek |

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.

Çekirdek ve bellek, `--cpus` `--memory` komutunun bir parçası olarak kullanılan ve ayarlarına karşılık gelir `docker run` .
