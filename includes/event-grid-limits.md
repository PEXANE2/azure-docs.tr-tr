---
title: include dosyası
description: include dosyası
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 05/22/2019
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 3f94481e6a8550479788d92c744327e1dc3b58c4
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "66376912"
---
Aşağıdaki sınırlar, olay etki alanları *değil* Azure Event Grid sistem konuları ve özel konular için geçerlidir.

| Resource | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel konu (giriş) için yayımlama oranı | Konu başına saniyede 5.000 olay |
| İstekleri Yayımla | saniyede 250 |
| Olay boyutu | Genel kullanılabilirlik (GA) için 64 KB desteği. 1 MB desteği şu anda önizleme aşamasındadır. |

Aşağıdaki sınırlar yalnızca olay etki alanları için geçerlidir.

| Resource | Sınır |
| --- | --- |
| Olay etki alanı başına konu başlıkları | Genel Önizleme sırasında 1.000 |
| Bir etki alanı içindeki Konu başına olay abonelikleri | Genel Önizleme sırasında 50 |
| Etki alanı kapsamı olay abonelikleri | Genel Önizleme sırasında 50 |
| Olay etki alanı (giriş) için yayımlama oranı | Genel Önizleme sırasında saniye başına 5.000 olay |
| İstekleri Yayımla | saniyede 250 |