---
title: include dosyası
description: include dosyası
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 58a741b369231a353a6b8e282a6e604a63a5727d
ms.sourcegitcommit: 7df70220062f1f09738f113f860fad7ab5736e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71210365"
---
**Veri toplama birimi ve bekletme** 

| Katman | Gün başına sınır | Veri saklama | Yorum |
|:---|:---|:---|:---|
| Geçerli GB başına Fiyatlandırma Katmanı<br>(2018 Nisan 'da tanıtılan) | Sınır yok | 30-730 gün | 31 günden daha fazla veri saklama, ek ücretler için kullanılabilir. Azure Izleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski ücretsiz Katmanlar<br>(2016 Nisan 'da tanıtılan) | 500 MB | 7 gün | Çalışma alanınız günde 500 MB sınırına ulaştığında, veri alımı durduruluyor ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır. Azure Güvenlik Merkezi tarafından toplanan verilerin bu 500 MB/gün sınırına dahil edilmediğini ve bu sınırın üzerine toplanmaya devam edileceğini unutmayın.  |
| GB katmanı başına eski tek başına<br>(2016 Nisan 'da tanıtılan) | Sınır yok | 30 ila 730 gün | 31 günden daha fazla veri saklama, ek ücretler için kullanılabilir. Azure Izleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Düğüm başına eski (OMS)<br>(2016 Nisan 'da tanıtılan) | Sınır yok | 30 ila 730 gün | 31 günden daha fazla veri saklama, ek ücretler için kullanılabilir. Azure Izleyici fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski Standart katman | Sınır yok | 30 gün  | Saklama ayarlanamıyor |
| Eski Premium katmanı | Sınır yok | 365 gün  | Saklama ayarlanamıyor |

**Abonelik başına çalışma alanı sayısı.**

| Fiyatlandırma katmanı    | Çalışma alanı sınırı | Açıklamalar
|:---|:---|:---|
| Ücretsiz katman  | 10 | Bu sınır arttırılamıyor. |
| Diğer tüm katmanlar | Sınır yok | Bir kaynak grubu içindeki kaynak sayısıyla ve abonelik başına kaynak grubu sayısına göre sınırlandırılırsınız. |

**Azure portal**

| Category | Sınırlar | Açıklamalar |
|:---|:---|:---|
| Günlük sorgusu tarafından döndürülen en fazla kayıt sayısı | 10,000 | Sorgu kapsamını, zaman aralığını ve sorgudaki filtreleri kullanarak sonuçları küçültün. |


**Veri Toplayıcı API 'SI**

| Category | Sınırlar | Açıklamalar |
|:---|:---|:---|
| Tek gönderi için en büyük boyut | 30 MB | Daha büyük birimleri birden çok gönderime ayırın. |
| Alan değerleri için en büyük boyut  | 32 KB | 32 KB'den daha uzun alanlar kesilir. |

**Arama API 'SI**

| Category | Sınırlar | Açıklamalar |
|:---|:---|:---|
| Tek bir sorguda döndürülen en fazla kayıt sayısı | 500,000 | |
| Döndürülen en fazla veri boyutu | 64.000.000 bayt (~ 61 MIB)| |
| En fazla sorgu çalışma süresi | 10 dakika | Ayrıntılar için bkz. [zaman aşımları](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) .  |
| En yüksek istek hızı | AAD kullanıcısı veya istemci IP adresi başına 30 saniye başına 200 istek | Ayrıntılar için [Hız sınırlarına](https://dev.loganalytics.io/documentation/Using-the-API/Limits) bakın. |

**Genel çalışma alanı sınırları**

| Category | Sınırlar | Açıklamalar |
|:---|:---|:---|
| Tablodaki en fazla sütun         | 500 | |
| Sütun adı için en fazla karakter | 500 | |
| Kapasiteden bölgeler | Batı Orta ABD | Geçici kapasite sınırında olduğundan bu bölgede şu anda yeni bir çalışma alanı oluşturamazsınız. Bu sınır, Ekim 2019 ' ün sonuna kadar planlanmaktadır. |
| Verileri dışarı aktar | Şu anda kullanılamıyor | Verileri toplamak ve dışarı aktarmak için Azure Işlevini veya mantıksal uygulamayı kullanın. | 

**Veri alım oranı**

Azure Izleyici, her ay büyüyen bir hızda çok sayıda müşteriye hizmet veren binlerce müşteriyi sunan yüksek ölçekli bir veri hizmetidir. Varsayılan alma hızı eşiği, çalışma alanı başına **500 MB/dk** olarak ayarlanır. Verileri daha yüksek bir fiyata tek bir çalışma alanına gönderirseniz, bazı veriler bırakılır ve eşik aşılmaya devam edilirken her 6 saatte bir olay, çalışma alanınızda *işlem* tablosuna gönderilir. Alım biriminiz, hız sınırını aşmaya devam ediyorsa veya bir süre önce bu sınıra ulaşmayı bekliyorsanız, bir destek isteği açarak çalışma alanınıza bir artış isteğinde bulunabilir.
 
Çalışma alanınızdaki bu tür bir olay hakkında bildirim almak için, sıfırdan farklı sonuç sayısına sahip uyarı mantığı temeli ile aşağıdaki sorguyu kullanarak bir [günlük uyarı kuralı](../articles/azure-monitor/platform/alerts-log.md) oluşturun.

``` Kusto
Operation
|where OperationCategory == "Ingestion"
|where Detail startswith "The rate of data crossed the threshold"
``` 


>[!NOTE]
>Log Analytics kullanmaya ne kadar süre kullandığınıza bağlı olarak eski fiyatlandırma katmanlarına erişiminiz olabilir. [Eski Log Analytics fiyatlandırma katmanları](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers)hakkında daha fazla bilgi edinin. 