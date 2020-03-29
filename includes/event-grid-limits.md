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
ms.openlocfilehash: ee80c04a6365c2cf337c4033a90df8d2993c299d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76845983"
---
Aşağıdaki sınırlar, olay etki alanları *için değil,* Azure Olay Izgara sistemi konuları ve özel konular için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel bir konu (giriş) için yayımlama oranı | Konu başına saniyede 5.000 olay |
| İstekleri yayımlama | Saniyede 250 |
| Olay boyutu | 1 MB (birden fazla 64-KB olay olarak ücretlendirilir) |

Aşağıdaki sınırlar yalnızca olay etki alanları için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Etkinlik etki alanı başına konular | 100.000 |
| Bir etki alanı içinde konu başına olay abonelikleri | 500 |
| Etki alanı kapsamı olay abonelikleri | 50 |
| Olay etki alanı (giriş) için yayımlama oranı | Saniyede 5.000 olay |
| İstekleri yayımlama | Saniyede 250 |
| Azure Aboneliği Başına Etkinlik Etki Alanları | 100 |