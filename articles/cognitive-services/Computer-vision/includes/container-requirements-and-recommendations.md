---
title: Konteyner gereksinimleri ve öneriler
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/20/2019
ms.author: dapine
ms.openlocfilehash: 2fa97a3a18741f506e10e5e4d61da286db3f744c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73481744"
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
