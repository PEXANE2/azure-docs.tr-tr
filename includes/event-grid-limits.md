---
title: dosya dahil etme
description: dosya dahil etme
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 05/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 7e7a0424e4454639211c6494aab0700e75269361
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83720936"
---
Aşağıdaki sınırlar, olay etki alanları *değil* Azure Event Grid sistem konuları ve özel konular için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel konu (giriş) için yayımlama oranı | Konu başına saniyede 5.000 olay |
| Olay boyutu | 1 MB. İşlemler, 64 KB 'lik artışlarla ücretlendirilir. Bu nedenle, 64 KB üzerindeki olaylar birden çok olaymış gibi işlemler ücretlerine neden olur. Örneğin, 130 KB olan bir olay 3 ayrı olay gibi işlemler için işlem yapmış olur.  |
| Olay etki alanı başına konu başlıkları | 100.000 |
| Bir etki alanı içindeki Konu başına olay abonelikleri | 500 |
| Etki alanı kapsamı olay abonelikleri | 50 |
| Olay etki alanı (giriş) için yayımlama oranı | saniye başına 5.000 olay |
| Azure aboneliği başına olay etki alanları | 100 |
| Konu veya etki alanı başına özel uç nokta bağlantıları | 64 | 
| Konu veya etki alanı başına IP güvenlik duvarı kuralları | 16 | 