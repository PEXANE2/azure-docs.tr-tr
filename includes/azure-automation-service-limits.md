---
title: include dosyası
description: include dosyası
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 12/13/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: d700dfcf5a7b6e9ada2a755335689ffa571e4c3d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334916"
---
#### <a name="process-automation"></a>Süreç otomasyonu

| Kaynak | Sınır |Notlar|
| --- | --- |---|
| Azure Otomasyon hesabı başına her 30 saniyede bir gönderilebilen maksimum yeni iş sayısı (zamanlanmamış işler) |100 |Bu sınıra ulaşıldığında, bir iş oluşturmak için sonraki istekler başarısız olursa. İstemci bir hata yanıtı alır.|
| Otomasyon hesabı başına aynı zaman diliminde maksimum eşzamanlı çalışan iş sayısı (zamanlanmamış işler) |200 |Bu sınıra ulaşıldığında, bir iş oluşturmak için sonraki istekler başarısız olursa. İstemci bir hata yanıtı alır.|
| 30 günlük bir yuvarlanma süresi için iş meta verilerinin maksimum depolama boyutu | 10 GB (yaklaşık 4 milyon iş)|Bu sınıra ulaşıldığında, bir iş oluşturmak için sonraki istekler başarısız olursa. |
| Maksimum iş akışı sınırı|1 MB|Tek bir akış 1 MB'dan büyük olamaz.|
| Otomasyon hesabı başına her 30 saniyede bir içe aktarılabilen maksimum modül sayısı |5 ||
| Bir modülün maksimum boyutu |100 MB ||
| İş çalıştırma süresi, Ücretsiz katman |Takvim ayı başına abonelik başına 500 dakika ||
| Kum havuzu başına izin verilen maksimum disk alanı miktarı<sup>1</sup> |1 GB |Yalnızca Azure kum kutuları için geçerlidir.|
| Bir kum havuzuna verilen maksimum bellek miktarı<sup>1</sup> |400 MB |Yalnızca Azure kum kutuları için geçerlidir.|
| Kum havuzu başına izin verilen maksimum ağ soketi sayısı<sup>1</sup> |1000 |Yalnızca Azure kum kutuları için geçerlidir.|
| Runbook<sup>1</sup> başına izin verilen maksimum çalışma süresi |3 saat |Yalnızca Azure kum kutuları için geçerlidir.|
| Abonelikteki maksimum Otomasyon hesabı sayısı |Sınır yok ||
| Otomasyon Hesabı Başına Maksimum Karma İşçi Grubu Sayısı|4.000||
|Tek bir Karma Runbook Çalışanı üzerinde çalıştırılabilen maksimum eşzamanlı iş sayısı|50 ||
| Maksimum runbook iş parametresi boyutu   | 512 kilobit||
| Maksimum runbook parametreleri   | 50|50 parametre sınırına ulaşırsanız, json veya XML dizesini bir parametreye geçirebilir ve runbook ile ayrıştabilirsiniz.|
| Maksimum webhook yük boyutu |  512 kilobit|
| İş verilerinin tutulduğu maksimum gün|30 gün|
| Maksimum PowerShell iş akışı durumu boyutu |5 MB| İş akışını kontrol ederken PowerShell iş akışı runbook'ları için geçerlidir.|

<sup>1.1.2</sup> Kum havuzu, birden çok iş tarafından kullanılabilen paylaşılan bir ortamdır. Aynı kum havuzunu kullanan işler, kum havuzunun kaynak sınırlamalarına bağlıdır.

#### <a name="change-tracking-and-inventory"></a>Değişiklik İzleme ve Stok

Aşağıdaki tablo, değişiklik izleme için makine başına izlenen madde sınırlarını gösterir.

| **Kaynak** | **Sınırı**| **Notlar** |
|---|---|---|
|Dosya|500||
|Kayıt Defteri|250||
|Windows yazılımı|250|Yazılım güncelleştirmelerini içermez.|
|Linux paketleri|1,250||
|Hizmetler|250||
|Daemon|250||

#### <a name="update-management"></a>Güncelleştirme Yönetimi

Aşağıdaki tabloda Güncelleştirme Yönetimi için sınırlar gösterilmektedir.

| **Kaynak** | **Sınırı**| **Notlar** |
|---|---|---|
|Güncelleştirme dağıtımı başına makine sayısı|1000||