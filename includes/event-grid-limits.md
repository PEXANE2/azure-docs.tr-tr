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
ms.openlocfilehash: 5992726893b722b0aa46c976a0167793f5ee6bb4
ms.sourcegitcommit: f7d057377d2b1b8ee698579af151bcc0884b32b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82131731"
---
Aşağıdaki sınırlar, olay etki alanları *değil* Azure Event Grid sistem konuları ve özel konular için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel konu (giriş) için yayımlama oranı | Konu başına saniyede 5.000 olay |
| İstekleri Yayımla | saniyede 250 |
| Olay boyutu | 1 MB. İşlemler, 64 KB 'lik artışlarla ücretlendirilir. Bu nedenle, 64 KB üzerindeki olaylar birden çok olaymış gibi işlemler ücretlerine neden olur. Örneğin, 130 KB olan bir olay 3 ayrı olay gibi işlemler için işlem yapmış olur.  |

Aşağıdaki sınırlar yalnızca olay etki alanları için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Olay etki alanı başına konu başlıkları | 100.000 |
| Bir etki alanı içindeki Konu başına olay abonelikleri | 500 |
| Etki alanı kapsamı olay abonelikleri | 50 |
| Olay etki alanı (giriş) için yayımlama oranı | saniye başına 5.000 olay |
| İstekleri Yayımla | saniyede 250 |
| Azure aboneliği başına olay etki alanları | 100 |