---
title: Konteyner gereksinimleri ve öneriler
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: c62044582cb488a5ef2d20b3f407c0865b3994ba
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877992"
---
> [!NOTE]
> Gereksinimler ve öneriler, 29 satır ve toplam 803 karakter içeren taranmış bir iş mektubunun 8 MB'lık görüntüsünü kullanarak saniyede tek bir istekiçeren kıyaslamalara dayanır.

Aşağıdaki tabloda, her Okuma kapsayıcısı için en az ve önerilen kaynak tahsisi açıklanmaktadır.

| Kapsayıcı | Minimum | Önerilen |Tps<br>(Minimum, Maksimum)|
|-----------|---------|-------------|--|
| Okuma | 1 çekirdek, 8 GB bellek, 0,24 TPS | 8 çekirdek, 16 GB bellek, 1,17 TPS | 0.24, 1.17 |

* Her çekirdek en az 2,6 gigahertz (GHz) veya daha hızlı olmalıdır.
* TPS - saniyedeki işlemler.

Çekirdek ve bellek, `--cpus` `--memory` komutun bir parçası olarak `docker run` kullanılan ve ayarlara karşılık gelir.
