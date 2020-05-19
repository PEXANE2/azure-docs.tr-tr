---
title: include dosyası
description: include dosyası
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 05/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 220759e8ed31c091887bd55f8d12aa4cc03a065f
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83590290"
---
Aşağıdaki sınırlar, olay etki alanları *değil* Azure Event Grid sistem konuları ve özel konular için geçerlidir.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel konu (giriş) için yayımlama oranı | Konu başına saniyede 5.000 olay |
| İstekleri Yayımla | saniyede 250 |
| Olay boyutu | 1 MB. İşlemler, 64 KB 'lik artışlarla ücretlendirilir. Bu nedenle, 64 KB üzerindeki olaylar birden çok olaymış gibi işlemler ücretlerine neden olur. Örneğin, 130 KB olan bir olay 3 ayrı olay gibi işlemler için işlem yapmış olur.  |
| Olay etki alanı başına konu başlıkları | 100.000 |
| Bir etki alanı içindeki Konu başına olay abonelikleri | 500 |
| Etki alanı kapsamı olay abonelikleri | 50 |
| Olay etki alanı (giriş) için yayımlama oranı | saniye başına 5.000 olay |
| Olay etki alanı için yayımlama istekleri | saniyede 250 |
| Azure aboneliği başına olay etki alanları | 100 |
| Konu veya etki alanı başına özel uç nokta bağlantıları | 64 | 
| Konu veya etki alanı başına IP güvenlik duvarı kuralları | 16 | 