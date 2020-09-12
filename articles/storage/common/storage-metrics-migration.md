---
title: Depolama Analizi ölçülerden Azure Izleyici ölçümlerine taşıma | Microsoft Docs
description: Depolama Analizi ölçümlerinden (klasik ölçümler) Azure Izleyici 'de ölçümlere nasıl geçiş yapılacağını öğrenin.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 09/04/2020
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.custom: monitoring
ms.openlocfilehash: f0357c153a068b1f857e4595fa17a9ac6bc22104
ms.sourcegitcommit: 206629373b7c2246e909297d69f4fe3728446af5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2020
ms.locfileid: "89500275"
---
# <a name="transition-to-metrics-in-azure-monitor"></a>Azure Izleyici 'de ölçümlere geçiş

**31 ağustos 2023** ' de, *Klasik ölçümler* olarak da bilinen depolama Analizi ölçümler kullanımdan kaldırılacak. Daha fazla bilgi için [resmi duyurusuna](https://azure.microsoft.com/updates/azure-storage-classic-metrics-will-be-retired-on-31-august-2023/)bakın. Klasik ölçümler kullanıyorsanız, bu tarihten önce Azure Izleyici 'de ölçümlere geçiş yaptığınızdan emin olun. Bu makale, geçişi yapmanıza yardımcı olur. 

## <a name="steps-to-complete-the-transition"></a>Geçişi tamamlamaya yönelik adımlar

Azure Izleyici 'de ölçümlere geçiş yapmak için aşağıdaki yaklaşımı öneririz.

1. Azure Izleyici 'de klasik ölçümler ve ölçümler arasındaki bazı [önemli farklılıklar](#key-differences-between-classic-metrics-and-metrics-in-azure-monitor) hakkında bilgi edinin. 

2. Şu anda kullandığınız klasik ölçümlerin bir listesini derleyin.

3. [Azure izleyici 'deki hangi ölçümlerin](#metrics-mapping-between-old-metrics-and-new-metrics) , kullanmakta olduğunuz ölçümlerle aynı verileri sunmakta olduğunu belirler. 
   
4. Ölçüm verilerini görüntülemek için [grafikler](https://docs.microsoft.com/learn/modules/gather-metrics-blob-storage/2-viewing-blob-metrics-in-azure-portal) veya [panolar](https://docs.microsoft.com/learn/modules/gather-metrics-blob-storage/4-using-dashboards-in-the-azure-portal) oluşturun.

   > [!NOTE]
   > Azure Izleyici 'de ölçümler varsayılan olarak etkindir, bu nedenle ölçümleri yakalamaya başlamak için yapmanız gereken bir şey yoktur. Bununla birlikte, bu ölçümleri görüntülemek için grafikler veya panolar oluşturmanız gerekir. 
 
5. Klasik depolama ölçümlerini temel alan uyarı kuralları oluşturduysanız, Azure Izleyici 'de ölçümleri temel alan [Uyarı kuralları oluşturun](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview) . 

6. Azure Izleyici 'de tüm ölçümlerinizi görebildikten sonra, klasik günlüğe kaydetmeyi devre dışı bırakabilirsiniz. 

<a id="key-differences-between-classic-metrics-and-metrics-in-azure-monitor"></a>

## <a name="classic-metrics-vs-metrics-in-azure-monitor"></a>Azure Izleyici 'de, klasik ölçümler ve ölçümler karşılaştırması

Bu bölümde, bu iki ölçüm platformu arasındaki bazı önemli farklılıklar açıklanmaktadır.

Temel fark, ölçümlerin yönetilme yönleridir. Klasik ölçümler Azure depolama tarafından yönetilir, ancak Azure Izleyici ölçümleri Azure Izleyici tarafından yönetilir. Klasik ölçümler sayesinde, Azure Storage ölçüm değerleri toplar, bunları toplar ve depolama hesabında bulunan tablolarda depolar. Azure Izleyici ölçümleri sayesinde Azure depolama, ölçüm verilerini Azure Izleyici arka ucuna gönderir. Azure Izleyici, Azure portal verilerin yanı sıra alınan verileri de içeren Birleşik bir izleme deneyimi sağlar. 

Ölçüm desteği olarak, klasik ölçümler yalnızca Azure Blob depolama için **Kapasite** ölçümleri sağlar. Azure Izleyici ölçümleri blob, tablo, dosya, kuyruk ve Premium Depolama için kapasite ölçümleri sağlar. Klasik ölçümler blob, tablo, Azure dosyası ve kuyruk depolama üzerinde **işlem** ölçümleri sağlar. Azure Izleyici ölçümleri ilgili listeye Premium depolama ekleyin.

Hesabınızdaki etkinlik bir ölçümü tetikleyemezse, klasik ölçümler söz konusu ölçüm için sıfır (0) değerini gösterir. Azure Izleyici 'de ölçümler, verileri tamamen yok sayacak ve bu da temizleyici raporlara yol açar. Örneğin, klasik ölçümler ile sunucu zaman aşımı hatası bildirilmezse, `ServerTimeoutError` ölçümler tablosundaki değer 0 olarak ayarlanır. Boyut eşittir ile ölçüm değerini sorgulayıp Azure Izleyici hiçbir veri döndürmez `Transactions` `ResponseType` `ServerTimeoutError` . 

Azure Izleyici 'de ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçümler](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics).

<a id="metrics-mapping-between-old-metrics-and-new-metrics"></a>

## <a name="map-classic-metrics-to-metrics-in-azure-monitor"></a>Klasik ölçümleri Azure Izleyici 'de ölçümlerle eşleme

 Azure Izleyici 'deki hangi ölçümlerin, kullanmakta olduğunuz ölçümlerle aynı verileri sunmakta olduğunu belirlemek için bu tabloları kullanın. 

**Kapasite ölçümleri**

| Klasik ölçüm | Azure Izleyici 'de ölçüm |
| ------------------- | ----------------- |
| `Capacity`            | `BlobCapacity` Boyut `BlobType` değerine eşit `BlockBlob` veya `PageBlob` |
| `ObjectCount`        | `BlobCount` Boyut `BlobType` değerine eşit `BlockBlob` veya `PageBlob` |
| `ContainerCount`      | `ContainerCount` |

> [!NOTE]
> Klasik ölçümler olarak kullanılamayan çeşitli yeni kapasite ölçümleri de mevcuttur. Listenin tamamını görüntülemek için bkz. [ölçümler](../common/monitor-storage-reference.md#metrics).

**İşlem ölçümleri**

| Klasik ölçüm | Azure Izleyici 'de ölçüm |
| ------------------- | ----------------- |
| `AnonymousAuthorizationError` | Boyutu `ResponseType` `AuthorizationError` ve boyutu `Authentication` eşit olan işlemler `Anonymous` |
| `AnonymousClientOtherError` | Boyutu `ResponseType` `ClientOtherError` ve boyutu `Authentication` eşit olan işlemler `Anonymous` |
| `AnonymousClientTimeoutError` | Boyutu `ResponseType` `ClientTimeoutError` ve boyutu `Authentication` eşit olan işlemler `Anonymous` |
| `AnonymousNetworkError` | Boyutu `ResponseType` `NetworkError` ve boyutu `Authentication` eşit olan işlemler `Anonymous` |
| `AnonymousServerOtherError` | Boyutu `ResponseType` `ServerOtherError` ve boyutu `Authentication` eşit olan işlemler `Anonymous` |
| `AnonymousServerTimeoutError` | Boyutu `ResponseType` `ServerTimeoutError` ve boyutu `Authentication` eşit olan işlemler `Anonymous` |
| `AnonymousSuccess` | Boyutu `ResponseType` `Success` ve boyutu `Authentication` eşit olan işlemler `Anonymous` |
| `AnonymousThrottlingError` | Boyutu veya ile eşit boyuta sahip işlemler `ResponseType` `ClientThrottlingError` `ServerBusyError` `Authentication``Anonymous` |
| `AuthorizationError` | Boyutu `ResponseType` Şuna eşit olan işlemler `AuthorizationError` |
| `Availability` | `Availability` |
| `AverageE2ELatency` | `SuccessE2ELatency` |
| `AverageServerLatency` | `SuccessServerLatency` |
| `ClientOtherError` | Boyutu `ResponseType` Şuna eşit olan işlemler `ClientOtherError` |
| `ClientTimeoutError` | Boyutu `ResponseType` Şuna eşit olan işlemler `ClientTimeoutError` |
| `NetworkError` | Boyutu `ResponseType` Şuna eşit olan işlemler `NetworkError` |
| `PercentAuthorizationError` | Boyutu `ResponseType` Şuna eşit olan işlemler `AuthorizationError` |
| `PercentClientOtherError` | Boyutu `ResponseType` Şuna eşit olan işlemler `ClientOtherError` |
| `PercentNetworkError` | Boyutu `ResponseType` Şuna eşit olan işlemler `NetworkError` |
| `PercentServerOtherError` | Boyutu `ResponseType` Şuna eşit olan işlemler `ServerOtherError` |
| `PercentSuccess` | Boyutu `ResponseType` Şuna eşit olan işlemler `Success` |
| `PercentThrottlingError` | Boyutu `ResponseType` veya değerine eşit olan işlemler `ClientThrottlingError``ServerBusyError` |
| `PercentTimeoutError` | Boyutu `ResponseType` eşit `ServerTimeoutError` veya `ResponseType` buna eşit olan işlemler `ClientTimeoutError` |
| `SASAuthorizationError` | Boyutu `ResponseType` `AuthorizationError` ve boyutu `Authentication` eşit olan işlemler `SAS` |
| `SASClientOtherError` | Boyutu `ResponseType` `ClientOtherError` ve boyutu `Authentication` eşit olan işlemler `SAS` |
| `SASClientTimeoutError` | Boyutu `ResponseType` `ClientTimeoutError` ve boyutu `Authentication` eşit olan işlemler `SAS` |
| `SASNetworkError` | Boyutu `ResponseType` `NetworkError` ve boyutu `Authentication` eşit olan işlemler `SAS` |
| `SASServerOtherError` | Boyutu `ResponseType` `ServerOtherError` ve boyutu `Authentication` eşit olan işlemler `SAS` |
| `SASServerTimeoutError` | Boyutu `ResponseType` `ServerTimeoutError` ve boyutu `Authentication` eşit olan işlemler `SAS` |
| `SASSuccess` | Boyutu `ResponseType` `Success` ve boyutu `Authentication` eşit olan işlemler `SAS` |
| `SASThrottlingError` | Boyutu veya ile eşit boyuta sahip işlemler `ResponseType` `ClientThrottlingError` `ServerBusyError` `Authentication``SAS` |
| `ServerOtherError` | Boyutu `ResponseType` Şuna eşit olan işlemler `ServerOtherError` |
| `ServerTimeoutError` | Boyutu `ResponseType` Şuna eşit olan işlemler `ServerTimeoutError` |
| `Success` | Boyutu `ResponseType` Şuna eşit olan işlemler `Success` |
| `ThrottlingError` | `Transactions` Boyut `ResponseType` değerine eşit `ClientThrottlingError` veya `ServerBusyError`|
| `TotalBillableRequests` | `Transactions` |
| `TotalEgress` | `Egress` |
| `TotalIngress` | `Ingress` |
| `TotalRequests` | `Transactions` |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Azure Izleyici 'de depolama ölçümleri](./storage-metrics-in-azure-monitor.md)
