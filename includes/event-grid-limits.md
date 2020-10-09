---
title: dosya dahil etme
description: dosya dahil etme
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 10/18/2020
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: b884bc72381c98af77f2f49336f3dd5762c68734
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91859179"
---
Aşağıdaki sınırlar Azure Event Grid **konular** (sistem, özel ve iş ortağı konuları) için geçerlidir. 

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel veya iş ortağı konusu (giriş) için yayımlama oranı | 5.000 olay/sn veya 1 MB/sn (hangisi önce karşılanır) |
| Olay boyutu | 1 MB  |
| Konu başına özel uç nokta bağlantıları  | 64 | 
| Konu başına IP güvenlik duvarı kuralları | 16 | 

Azure Event Grid **etki alanları**için aşağıdaki sınırlar geçerlidir. 

| Kaynak | Sınır |
| --- | --- |
| Olay etki alanı başına konu başlıkları | 100.000 |
| Bir etki alanı içindeki Konu başına olay abonelikleri | 500 |
| Etki alanı kapsamı olay abonelikleri | 50 |
| Olay etki alanı (giriş) için yayımlama oranı | 5.000 olay/sn veya 1 MB/sn (hangisi önce karşılanır) |
| Azure aboneliği başına olay etki alanları | 100 |
| Etki alanı başına özel uç nokta bağlantıları | 64 | 
| Etki alanı başına IP güvenlik duvarı kuralları | 16 | 


