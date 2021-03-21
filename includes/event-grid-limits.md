---
title: include dosyası
description: include dosyası
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: include
ms.date: 02/17/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 2f52e8a89ec9dd78a1951836053cb2c698310bbd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100645561"
---
Aşağıdaki sınırlar Azure Event Grid **konular** (sistem, özel ve iş ortağı konuları) için geçerlidir. 

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel veya iş ortağı konusu (giriş) için yayımlama oranı | 5.000 olay/sn veya 5 MB/sn (hangisi önce karşılanır) |
| Olay boyutu | 1 MB  |
| Konu başına özel uç nokta bağlantıları  | 64 | 
| Konu başına IP güvenlik duvarı kuralları | 16 | 

Azure Event Grid **etki alanları** için aşağıdaki sınırlar geçerlidir. 

| Kaynak | Sınır |
| --- | --- |
| Olay etki alanı başına konu başlıkları | 100.000 |
| Bir etki alanı içindeki Konu başına olay abonelikleri | 500 |
| Etki alanı kapsamı olay abonelikleri | 50 |
| Olay etki alanı (giriş) için yayımlama oranı | 5.000 olay/sn veya 5 MB/sn (hangisi önce karşılanır) |
| Azure aboneliği başına olay etki alanları | 100 |
| Etki alanı başına özel uç nokta bağlantıları | 64 | 
| Etki alanı başına IP güvenlik duvarı kuralları | 16 | 


