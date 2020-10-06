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
ms.openlocfilehash: e084256d9c2043d4382ca180ef3178175b301367
ms.sourcegitcommit: a07a01afc9bffa0582519b57aa4967d27adcf91a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91745701"
---
Aşağıdaki sınırlar Azure Event Grid **konular** (sistem, özel ve iş ortağı konuları) için geçerlidir. 

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına özel konular | 100 |
| Konu başına olay abonelikleri | 500 |
| Özel veya iş ortağı konusu (giriş) için yayımlama oranı | 5.000 olay/sn veya 1 MB/sn (hangisi önce karşılanır)<br/>Sistem konuları için uygulanmaz. |
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


