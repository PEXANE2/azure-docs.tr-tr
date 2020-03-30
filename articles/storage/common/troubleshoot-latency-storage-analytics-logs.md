---
title: Depolama Analizi günlüklerini kullanarak gecikme sorunlarını giderme
description: Azure Depolama Analizi günlüklerini kullanarak gecikme sorunlarını belirleyin ve sorun giderin ve istemci uygulamasını optimize edin.
author: v-miegge
ms.topic: troubleshooting
ms.author: kartup
manager: dcscontentpm
ms.date: 10/21/2019
ms.service: storage
ms.subservice: common
services: storage
tags: ''
ms.openlocfilehash: 2197a149235c0dca98a24a57549538b2a4cbb1c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74196516"
---
# <a name="troubleshoot-latency-using-storage-analytics-logs"></a>Depolama Analizi günlüklerini kullanarak gecikme sorunlarını giderme

Azure Depolama ile istemci uygulamalarını oluşturmak ve desteklemek için tanılama ve sorun giderme önemli bir beceridir.

Azure uygulamasının dağıtılmış yapısı nedeniyle, hem hataları hem de performans sorunlarını tanılama ve sorun giderme geleneksel ortamlardan daha karmaşık olabilir.

Aşağıdaki adımlar, Azure Depolama Analizi günlüklerini kullanarak gecikme sorunlarını nasıl tanımlayıp sorun gidereceklerini ve istemci uygulamasını optimize edin.

## <a name="recommended-steps"></a>Önerilen adımlar

1. Depolama [Analizi günlüklerini indirin.](https://docs.microsoft.com/azure/storage/common/storage-analytics-logging#download-storage-logging-log-data)

2. Ham biçim günlüklerini tabular biçime dönüştürmek için aşağıdaki PowerShell komut dosyasını kullanın:

   ```Powershell
   $Columns = 
        (   "version-number",
            "request-start-time",
            "operation-type",
            "request-status",
            "http-status-code",
            "end-to-end-latency-in-ms",
            "server-latency-in-ms",
            "authentication-type",
            "requester-account-name",
            "owner-account-name",
            "service-type",
            "request-url",
            "requested-object-key",
            "request-id-header",
            "operation-count",
            "requester-ip-address",
            "request-version-header",
            "request-header-size",
            "request-packet-size",
            "response-header-size",
            "response-packet-size",
            "request-content-length",
            "request-md5",
            "server-md5",
            "etag-identifier",
            "last-modified-time",
            "conditions-used",
            "user-agent-header",
            "referrer-header",
            "client-request-id"
        )

   $logs = Import-Csv “REPLACE THIS WITH FILE PATH” -Delimiter ";" -Header $Columns

   $logs | Out-GridView -Title "Storage Analytic Log Parser"
   ```

3. Komut dosyası, aşağıda gösterildiği gibi bilgileri sütunlara göre filtreleyebileceğiniz bir GUI penceresi başlatacaktır.

   ![Depolama Analitik Günlük Parser Penceresi](media/troubleshoot-latency-storage-analytics-logs/storage-analytic-log-parser-window.png)
 
4. Günlük girişlerini "işlem türüne" göre daraltın ve sorunun zaman dilimi sırasında oluşturulan günlük girişini arayın.

   ![İşlem türü günlük girişleri](media/troubleshoot-latency-storage-analytics-logs/operation-type.png)

5. Sorunun oluştuğu süre boyunca aşağıdaki değerler önemlidir:

   * İşlem Tipi = GetBlob
   * istek durumu = SASNetworkError
   * Uç-Uç-Gecikme-In-Ms = 8453
   * Sunucu-Gecikme-In-Ms = 391

   Uç-Uç Gecikme si aşağıdaki denklem kullanılarak hesaplanır:

   * Uçuca Gecikme = Sunucu Gecikmesi + İstemci Gecikmesi

   Günlük girişini kullanarak İstemci Gecikmesini hesaplayın:

   * İstemci Gecikmesi = Uç-Uç Gecikme - Sunucu Gecikmesi

          * Example: 8453 – 391 = 8062ms

   Aşağıdaki tablo, yüksek gecikme gecikmesi OperationType ve RequestStatus sonuçları hakkında bilgi sağlar:

   |   |İstek Durumu=<br>Başarılı|İstek Durumu=<br>(SAS) Ağ Hatası|Öneri|
   |---|---|---|---|
   |GetBlob|Evet|Hayır|[**GetBlob Operasyonu:** İstek Durumu = Başarı](#getblob-operation-requeststatus--success)|
   |GetBlob|Hayır|Evet|[**GetBlob Operasyonu:** İstek Durumu = (SAS)Ağ Hatası](#getblob-operation-requeststatus--sasnetworkerror)|
   |PutBlob|Evet|Hayır|[**Devreye Sok:** İstek Durumu = Başarı](#put-operation-requeststatus--success)|
   |PutBlob|Hayır|Evet|[**Devreye Sok:** İstek Durumu = (SAS)Ağ Hatası](#put-operation-requeststatus--sasnetworkerror)|

## <a name="status-results"></a>Durum sonuçları

### <a name="getblob-operation-requeststatus--success"></a>GetBlob Operasyonu: RequestStatus = Başarı

"Önerilen adımlar" bölümünün 5.

* Uçuca Gecikme
* Sunucu Gecikmesi
* İstemci Gecikmesi

**İstek Durumu = Başarı**ile **GetBlob Işleminde,** Max **Time** **İstemci-Gecikme**içinde harcanıyorsa, bu, Azure Depolama'nın istemciye veri yazmak için büyük miktarda zaman harcadığını gösterir. Bu gecikme, Istemci Tarafı Sorununu gösterir.

**Öneri:**

* Müvekkilinizin kodunu araştırın.
* İstemcinin ağ bağlantısı sorunlarını araştırmak için Wireshark, Microsoft İleti Çözümleyicisi veya Tcping'i kullanın. 

### <a name="getblob-operation-requeststatus--sasnetworkerror"></a>GetBlob İşlemi: İstek Durumu = (SAS)NetworkError

"Önerilen adımlar" bölümünün 5.

* Uçuca Gecikme
* Sunucu Gecikmesi
* İstemci Gecikmesi

**RequestStatus = (SAS)NetworkError**ile **getblob işleminde,** **Max Time** **Client-Latency'de**harcanıyorsa, en yaygın sorun istemcinin depolama hizmetinde bir zaman aşımı süresi dolmadan önce bağlantısını kesmesidir.

**Öneri:**

* İstemcinin depolama hizmetiyle bağlantısını neden ve ne zaman kestiğini anlamak için istemcinizdeki kodu araştırın.
* İstemcinin ağ bağlantısı sorunlarını araştırmak için Wireshark, Microsoft İleti Çözümleyicisi veya Tcping'i kullanın. 

### <a name="put-operation-requeststatus--success"></a>İşleme Koyula: RequestStatus = Başarı

"Önerilen adımlar" bölümünün 5.

* Uçuca Gecikme
* Sunucu Gecikmesi
* İstemci Gecikmesi

İstek Durumu ile **Bir Put İşleminde** **= Başarı**, Max **Time** **İstemci-Gecikme**süresi içinde harcanmışsa, bu, Istemcinin Azure Depolama'ya veri göndermesinin daha fazla zaman aldığını gösterir. Bu gecikme, Istemci Tarafı Sorununu gösterir.

**Öneri:**

* Müvekkilinizin kodunu araştırın.
* İstemcinin ağ bağlantısı sorunlarını araştırmak için Wireshark, Microsoft İleti Çözümleyicisi veya Tcping'i kullanın. 

### <a name="put-operation-requeststatus--sasnetworkerror"></a>İşlemE Koy: RequestStatus = (SAS)NetworkError

"Önerilen adımlar" bölümünün 5.

* Uçuca Gecikme
* Sunucu Gecikmesi
* İstemci Gecikmesi

**RequestStatus = (SAS)NetworkError**ile bir **PutBlob Işleminde,** **Max Time** **Client-Latency'de**harcanıyorsa, en yaygın sorun istemcinin depolama hizmetinde bir zaman aşımı süresi dolmadan önce bağlantısını kesmesidir.

**Öneri:**

* İstemcinin depolama hizmetiyle bağlantısını neden ve ne zaman kestiğini anlamak için istemcinizdeki kodu araştırın.
* İstemcinin ağ bağlantısı sorunlarını araştırmak için Wireshark, Microsoft İleti Çözümleyicisi veya Tcping'i kullanın.

