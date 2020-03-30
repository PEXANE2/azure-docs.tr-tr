---
title: Azure Depolama ölçümleri geçişi | Microsoft Dokümanlar
description: Eski ölçümleri Azure Monitor tarafından yönetilen yeni ölçümlere nasıl geçirteceklerini öğrenin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 03/30/2018
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 537369c9466b1083723642ec9e93fcdf25056c5e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68855333"
---
# <a name="azure-storage-metrics-migration"></a>Azure Depolama ölçümleri geçiş

Azure'daki monitör deneyimini birleştirme stratejisiyle uyumlu olan Azure Depolama, ölçümleri Azure Monitor platformuna entegre eder. Gelecekte, eski ölçümlerin hizmeti Azure ilkesine dayalı bir erken bildirimle sona erer. Eski depolama ölçümlerine güveniyorsanız, metrik bilgilerinizi korumak için hizmet bitiş tarihinden önce geçiş yapmanız gerekir.

Bu makalede, eski ölçümlerden yeni ölçümlere nasıl geçiş yapılacağını gösterir.

## <a name="understand-old-metrics-that-are-managed-by-azure-storage"></a>Azure Depolama tarafından yönetilen eski ölçümleri anlama

Azure Depolama eski metrik değerleri toplar ve bunları aynı depolama hesabı içinde $Metric tablolarda toplar ve depolar. Bir izleme grafiği oluşturmak için Azure portalını kullanabilirsiniz. Ayrıca, şemaya dayalı $Metric tablolarından gelen verileri okumak için Azure Depolama SDK'larını da kullanabilirsiniz. Daha fazla bilgi için Bkz. [Depolama Analizi.](./storage-analytics.md)

Eski ölçümler yalnızca Azure Blob depolamada kapasite ölçümleri sağlar. Eski ölçümler Blob depolama, Tablo depolama, Azure Dosyaları ve Sıra depolama işlem ölçümleri sağlar.

Eski ölçümler düz bir şemada tasarlanmıştır. Tasarımı tetikleyen trafik desenleri olmadığında sıfır metrik değer elde edin. Örneğin, canlı trafikten depolama hesabına sunucu zaman alabilme hatası almasanız bile **ServerTimeoutError** değeri $Metric tablolarında 0 olarak ayarlanır.

## <a name="understand-new-metrics-managed-by-azure-monitor"></a>Azure İzleyici tarafından yönetilen yeni ölçümleri anlama

Yeni depolama ölçümleri için Azure Depolama, metrik verileri Azure Monitor'un arka ucuna yayır. Azure Monitor, portaldan gelen verilerin yanı sıra veri alımı da dahil olmak üzere birleşik bir izleme deneyimi sağlar. Daha fazla bilgi için bu [makaleye](../../monitoring-and-diagnostics/monitoring-overview-metrics.md)başvurabilirsiniz.

Yeni ölçümler Blob, Tablo, Dosya, Kuyruk ve premium depolamada kapasite ölçümleri ve işlem ölçümleri sağlar.

Çok boyutlu, Azure Monitor'un sağladığı özelliklerden biridir. Azure Depolama, yeni metrik şema tanımlamada tasarımı benimser. Ölçümlerde desteklenen boyutlar için Azure Depolama ölçümlerinde ayrıntıları [Azure Monitor'da](./storage-metrics-in-azure-monitor.md)bulabilirsiniz. Çok boyutlu tasarım, hem yutma işleminden hem de ölçümlerin depolanmasından elde edilen kapasitede maliyet verimliliği sağlar. Sonuç olarak, trafiğiniz ilgili ölçümleri tetiklemediyse, ilgili metrik veriler oluşturulmayacaktır. Örneğin, trafiğiniz herhangi bir sunucu zaman atlama hatası tetiklemediyse, Azure Monitor, **ServerTimeoutError'a**eşit dimension **ResponseType** ile metrik **İşlemlerin** değerini sorgularken herhangi bir veri döndürmez.

## <a name="metrics-mapping-between-old-metrics-and-new-metrics"></a>Eski ölçümlerle yeni ölçümler arasında ölçüm eşlemesi

Metrik verileri programlı olarak okursanız, programlarınızda yeni metrik şemayı benimsemeniz gerekir. Değişiklikleri daha iyi anlamak için aşağıdaki tabloda listelenen eşleme lere başvurabilirsiniz:

**Kapasite ölçümleri**

| Eski metrik | Yeni metrik |
| ------------------- | ----------------- |
| **Kapasite**            | **BlobType** boyutuile **BlobCapacity** **BlockBlob** veya **PageBlob** eşit |
| **Objectcount**        | **BlobType** boyutu **blockblob veya PageBlob** eşit **PageBlob** **blobCount** |
| **Konteyner Sayısı**      | **Konteyner Sayısı** |

Aşağıdaki ölçümler, eski ölçümlerin desteklemediğinin yeni teklifleridir:
* **Tablo Kapasitesi**
* **Tablo Sayısı**
* **TabloEntityCount**
* **Sıra Kapasitesi**
* **Sıra Sayısı**
* **QueueMessageCount**
* **Dosya Kapasitesi**
* **Dosya Sayısı**
* **FileShareCount**
* **Kullanılmış Kapasite**

**İşlem ölçümleri**

| Eski metrik | Yeni metrik |
| ------------------- | ----------------- |
| **Anonim Yetkilendirme Hatası** | Boyut **ResponseType** ile işlemler **Yetkilendirme Hatası** ve Boyut **Kimlik** **Doğrulama** anonim eşit eşit |
| **AnonimClientOtherError** | **ClientOtherError** ve **adsız adsız** boyut **kimlik doğrulamasına** eşit boyut **ResponseType** ile işlemler |
| **AnonimClientTimeoutHatası** | **MüşteriZaman OutError** ve boyut **Kimlik Doğrulama** anonim eşit **Anonymous** boyut **ResponseType** ile işlemler |
| **AnonimAğ Hatası** | **NetworkError** ve **Adsız'a** eşit boyut **Kimlik Doğrulamasına** eşit boyut **ResponseType** ile yapılan işlemler |
| **AnonimServerOtherError** | **ServerOtherError** ve **Adsız'a** eşit boyut **Kimlik Doğrulamasına** eşit boyut **ResponseType** ile işlemler |
| **AnonimServerTimeoutHatası** | **ServerTimeoutError** ve Boyut **Kimlik Doğrulaması** ile birlikte Işlem **YanıtTürü** **Anonim'e** eşittir |
| **Anonim Başarı** | **Boyut** **ResponseType** başarı ve boyut **Kimlik Doğrulama** eşit **Anonim** eşit ile işlemler |
| **AnonimThrottlingHatası** | **ClientThrottlingError** veya **ServerBusyError** ve boyut **Kimlik Doğrulamasına** eşit boyut **ResponseType** ile işlemler **Anonymous'a** eşittir |
| **Yetkilendirme Hatası** | **Yetkilendirme Hatasına** eşit boyut **ResponseType** ile işlemler |
| **Kullanılabilir -lik** | **Kullanılabilir -lik** |
| **AverageE2ELatency** | **SuccessE2ELatency** |
| **AverageServerLatency** | **SuccessServerLatency** |
| **IstemciOtherError** | **ClientOtherError'a** eşit boyut **ResponseType** ile yapılan işlemler |
| **İstemciTimeoutHatası** | Dimension **ResponseType** ile birlikte **istemcitimeoutError** eşit hareketler |
| **Ağ Hatası** | **NetworkError'a** eşit boyut **ResponseType** ile yapılan işlemler |
| **Yüzde Yetkilendirme Hatası** | **Yetkilendirme Hatasına** eşit boyut **ResponseType** ile işlemler |
| **YüzdeistemciOtherError** | **ClientOtherError'a** eşit boyut **ResponseType** ile yapılan işlemler |
| **YüzdeAğ Hatası** | **NetworkError'a** eşit boyut **ResponseType** ile yapılan işlemler |
| **YüzdeServerOtherError** | **ServerOtherError'a** eşit boyut **ResponseType** ile yapılan işlemler |
| **Yüzde Başarı** | **Başarıya** Eşit Boyut **ResponseType** ile işlemler |
| **YüzdeThrottlingHatası** | **ClientThrottlingError** veya **ServerBusyError** boyutuna eşit **YanıtTürü** ile işlemler |
| **YüzdeTimeoutHatası** | **ServerTimeoutError** veya **ResponseType'a** eşit dimension **ResponseType** ile **işlemlerClientTimeoutError'a** eşittir |
| **SASAuthorizationError** | Boyut **ResponseType** ile işlemler **Yetkilendirme Hatası** ve Boyut Kimlik **Doğrulama** **SAS** eşit eşittir |
| **SASClientOtherError** | **SAS'a** eşit **DimensionResponseType** boyutuna sahip işlemlerDiğer **Hata** ve Boyut **Kimlik Doğrulama** |
| **SASClientTimeoutHatası** | SAS'a eşit dimension **ClientTimeoutError** **ResponseType** ve **SAS'a** eşit boyut **Kimlik Doğrulamaile** yapılan işlemler |
| **SASNetworkHatası** | **NetworkError** ve Boyut Kimlik Doğrulamasi'ne eşit **SAS** boyut **ResponseType** ile yapılan işlemler **Authentication** |
| **SASServerOtherError** | **ServerOtherError** ve Boyut **Kimlik Doğrulaması** ile **SAS** eşit boyut **ResponseType** ile işlemler |
| **SASServerTimeoutHatası** | **ServerTimeoutError** ve Boyut **Kimlik Doğrulaması** ile **sas** eşit boyut **ResponseType** ile işlemler |
| **SASSuccess** | **Boyut** **ResponseType** başarı ve boyut **Kimlik Doğrulama** **sas** eşit eşit ile işlemler |
| **SASThrottlingHatası** | Dimension **ResponseType** ile işlem **ClientThrottlingError** veya **ServerBusyError** ve boyut **Kimlik Doğrulama** **SAS** eşit eşit |
| **ServerOtherError** | **ServerOtherError'a** eşit boyut **ResponseType** ile yapılan işlemler |
| **ServerTimeoutHatası** | **ServerTimeoutError'a** eşit boyut **ResponseType** ile yapılan işlemler |
| **Başarı** | **Başarıya** Eşit Boyut **ResponseType** ile işlemler |
| **ThrottlingError** | **ClientThrottlingError** veya **ServerBusyError** boyutuna eşit **YanıtTürü** ile **işlemler**|
| **Toplam Faturalı İstekler** | **İşlemler** |
| **TotalEgress** | **Çıkış** |
| **TotalIngress** | **Giriş** |
| **TotalRequests** | **İşlemler** |

## <a name="faq"></a>SSS

### <a name="how-should-i-migrate-existing-alert-rules"></a>Varolan uyarı kurallarını nasıl geçirmeliyim?

Eski depolama ölçümlerine dayalı klasik uyarı kuralları oluşturduysanız, yeni metrik şemaya dayalı yeni uyarı kuralları oluşturmanız gerekir.

### <a name="is-new-metric-data-stored-in-the-same-storage-account-by-default"></a>Yeni metrik veriler varsayılan olarak aynı depolama hesabında depolanır mı?

Hayır. Metrik verileri bir depolama hesabına arşivlemek için [Azure Monitörü Tanı Ayarı API'sini](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Azure Monitör'de depolama ölçümleri](./storage-metrics-in-azure-monitor.md)
