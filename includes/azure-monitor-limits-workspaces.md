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
ms.openlocfilehash: 97f265d76ac70891e9cefc0ef6651e439706ed23
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80334748"
---
**Veri toplama hacmi ve saklama** 

| Katman | Günlük limit | Veri saklama | Açıklama |
|:---|:---|:---|:---|
| Geçerli GB başına fiyatlandırma katmanı<br>(Nisan 2018'de tanıtıldı) | Sınır yok | 30 - 730 gün arası | Ek ücretler için 31 günden fazla veri saklama kullanılabilir. Azure Monitör fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski Ücretsiz katmanlar<br>(Nisan 2016'da tanıtıldı) | 500 MB | 7 gün | Çalışma alanınız günlük 500 MB sınırına ulaştığında, veri alımı bir sonraki günün başında durur ve devam eder. Bir gün için UTC temel alınır. Azure Güvenlik Merkezi tarafından toplanan verilerin günlük 500 MB sınırına dahil olmadığını ve bu sınırın üzerinde toplanmaya devam edeceğini unutmayın.  |
| GB başına eski Bağımsız katman<br>(Nisan 2016'da tanıtıldı) | Sınır yok | 30 ila 730 gün | Ek ücretler için 31 günden fazla veri saklama kullanılabilir. Azure Monitör fiyatlandırması hakkında daha fazla bilgi edinin. |
| Düğüm Başına Eski (OMS)<br>(Nisan 2016'da tanıtıldı) | Sınır yok | 30 ila 730 gün | Ek ücretler için 31 günden fazla veri saklama kullanılabilir. Azure Monitör fiyatlandırması hakkında daha fazla bilgi edinin. |
| Eski Standart katman | Sınır yok | 30 gün  | Bekletme ayarlanamıyor |
| Eski Premium katmanı | Sınır yok | 365 gün  | Bekletme ayarlanamıyor |

**Abonelik başına çalışma alanı sayısı.**

| Fiyatlandırma katmanı    | Çalışma alanı sınırı | Yorumlar
|:---|:---|:---|
| Ücretsiz katmanı  | 10 | Bu sınır artırılamaz. |
| Diğer tüm katmanlar | Sınır yok | Kaynak grubundaki kaynak sayısı ve abonelik başına kaynak grupları nın sayısı yla sınırlısınız. |

**Azure portalında**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Günlük sorgusu tarafından döndürülen maksimum kayıtlar | 10,000 | Sorgudaki sorgu kapsamı, zaman aralığı ve filtreleri kullanarak sonuçları azaltın. |


**Veri Toplayıcı API'si**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Tek bir gönderi için maksimum boyut | 30 MB | Daha büyük birimleri birden çok dire bölün. |
| Alan değerleri için maksimum boyut  | 32 KB | 32 KB'den daha uzun alanlar kesilir. |

**Arama API’si**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Tek bir sorguda döndürülen maksimum kayıtlar | 500.000 | |
| Döndürülen verilerin maksimum boyutu | 64.000.000 bayt (~61 MiB)| |
| Maksimum sorgu çalışma süresi | 10 dakika | Ayrıntılar için [Zaman Zaman Ları'na](https://dev.loganalytics.io/documentation/Using-the-API/Timeouts) bakın.  |
| Maksimum istek oranı | AAD kullanıcı veya istemci IP adresi başına 30 saniyede 200 istek | Ayrıntılar için [Fiyat sınırlarına](https://dev.loganalytics.io/documentation/Using-the-API/Limits) bakın. |

**Genel çalışma alanı sınırları**

| Kategori | Sınır | Yorumlar |
|:---|:---|:---|
| Tablodaki maksimum sütunlar         | 500 | |
| Sütun adı için maksimum karakterler | 500 | |
| Veri dışarı aktarma | Şu anda kullanılamıyor | Verileri toplamak ve dışa aktarmak için Azure İşlevi veya Mantık Uygulaması'nı kullanın. | 

**Veri alma hacmi oranı**


Azure Monitor, her ay terabaytlarce veri gönderen binlerce müşteriye artan bir hızla hizmet veren yüksek ölçekli bir veri hizmetidir. [Tanılama ayarlarını](../articles/azure-monitor/platform/diagnostic-settings.md) kullanarak Azure kaynaklarından gönderilen veriler için varsayılan alma hacmi oranı sınırı çalışma alanı başına yaklaşık **6 GB/dk'dır.** Bu, günlük uzunluğuna ve sıkıştırma oranına bağlı olarak veri türleri arasında değişebildiği için yaklaşık bir değerdir. Bu sınır aracılardan veya Veri Toplayıcı [API'sinden](../articles/azure-monitor/platform/data-collector-api.md)gönderilen veriler için geçerli değildir.

Verileri tek bir çalışma alanına daha yüksek bir hızda gönderirseniz, bazı veriler bırakılır ve eşik aşılması devam ederken çalışma alanınızdaki *Çalışma* tablosuna her 6 saatte bir olay gönderilir. Yutma hacminiz fiyat sınırını aşmaya devam ederse veya yakın bir zamanda bu sayıya ulaşmayı bekliyorsanız, bir destek isteği açarak çalışma alanınızda bir artış talep edebilirsiniz.
 
Çalışma alanınızda böyle bir olaydan haberdar olmak için, sıfırdan daha fazla sonuç sayısına ilişkin uyarı mantığı tabanına sahip aşağıdaki sorguyu kullanarak bir [günlük uyarı kuralı](../articles/azure-monitor/platform/alerts-log.md) oluşturun.

``` Kusto
Operation
|where OperationCategory == "Ingestion"
|where Detail startswith "The rate of data crossed the threshold"
``` 


>[!NOTE]
>Log Analytics'i ne kadar süredir kullandığınıza bağlı olarak, eski fiyatlandırma katmanlarına erişiminiz olabilir. [Log Analytics eski fiyatlandırma katmanları](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#legacy-pricing-tiers)hakkında daha fazla bilgi edinin. 