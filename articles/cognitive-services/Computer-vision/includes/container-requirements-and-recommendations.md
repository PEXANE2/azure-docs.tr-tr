---
title: Kapsayıcı gereksinimleri ve önerileri
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: c62044582cb488a5ef2d20b3f407c0865b3994ba
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80877992"
---
> [!NOTE]
> Gereksinimler ve öneriler, 29 satır ve toplam 803 karakter içeren taranmış bir iş harfinin 8 MB 'lik bir görüntüsünü kullanarak saniyede tek bir istek ile kıyaslamayı temel alır.

Aşağıdaki tabloda, her bir okuma kapsayıcısı için kaynakların en düşük ve önerilen ayırması açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |TPS<br>(En düşük, en yüksek)|
|-----------|---------|-------------|--|
| Okuma | 1 çekirdek, 8 GB bellek, 0,24 TPS | 8 çekirdek, 16 GB bellek, 1,17 TPS | 0,24, 1,17 |

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS-saniye başına işlem.

Çekirdek ve bellek, `--cpus` `--memory` `docker run` komutunun bir parçası olarak kullanılan ve ayarlarına karşılık gelir.
