---
title: Azure Service Fabric performans Izleme
description: Azure Service Fabric kümelerini izleme ve tanılamaya yönelik performans sayaçları hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 30b9b8393007033a7c2e6798cd57d9cf0128820d
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75464709"
---
# <a name="performance-metrics"></a>Performans ölçümleri

Kümenizin performansını ve üzerinde çalışan uygulamaları anlamak için ölçümlerin toplanması gerekir. Service Fabric kümeler için aşağıdaki performans sayaçlarını toplamayı öneririz.

## <a name="nodes"></a>Düğümler

Kümenizdeki makineler için, her makinede yükü daha iyi anlamak ve uygun küme ölçekleme kararları almak için aşağıdaki performans sayaçlarını toplamayı göz önünde bulundurun.

| Sayaç kategorisi | Sayaç adı |
| --- | --- |
| Mantıksal Disk | Mantıksal Disk Boş Alan |
| FizikselDisk (disk başına) | Ortalama Disk okuma sırası uzunluğu |
| FizikselDisk (disk başına) | Ortalama Disk yazma sırası uzunluğu |
| FizikselDisk (disk başına) | Ortalama Disk sn/okuma |
| FizikselDisk (disk başına) | Ortalama Disk sn/yazma |
| FizikselDisk (disk başına) | Disk Okuma/sn |
| FizikselDisk (disk başına) | Disk Okuma Bayt/sn |
| FizikselDisk (disk başına) | Disk Yazma/sn |
| FizikselDisk (disk başına) | Disk Yazma Bayt/sn |
| Hafıza | Kullanılabilir MBayt |
| PagingFile | Kullanım yüzdesi |
| İşlemci (Toplam) | % İşlemci zamanı |
| İşlem (hizmet başına) | % İşlemci zamanı |
| İşlem (hizmet başına) | KIMLIK Işlemi |
| İşlem (hizmet başına) | Özel Baytlar |
| İşlem (hizmet başına) | İş Parçacığı Sayısı |
| İşlem (hizmet başına) | Sanal Bayt Sayısı |
| İşlem (hizmet başına) | Çalışma Kümesi |
| İşlem (hizmet başına) | Çalışma kümesi-özel |
| Ağ arabirimi (tüm örnekler) | Bayt recd |
| Ağ arabirimi (tüm örnekler) | Gönderilen Bayt |
| Ağ arabirimi (tüm örnekler) | Toplam bayt |
| Ağ arabirimi (tüm örnekler) | Çıkış sırası uzunluğu |
| Ağ arabirimi (tüm örnekler) | Atılan giden paketler |
| Ağ arabirimi (tüm örnekler) | Atılan alınan paketler |
| Ağ arabirimi (tüm örnekler) | Giden paket hataları |
| Ağ arabirimi (tüm örnekler) | Alınan paket hataları |

## <a name="net-applications-and-services"></a>.NET uygulamaları ve Hizmetleri

Kümenize .NET Hizmetleri dağıtıyorsanız, aşağıdaki sayaçları toplayın. 

| Sayaç kategorisi | Sayaç adı |
| --- | --- |
| .NET CLR belleği (hizmet başına) | İşlem Kimliği |
| .NET CLR belleği (hizmet başına) | Toplam kaydedilmiş bayt sayısı |
| .NET CLR belleği (hizmet başına) | Toplam ayrılan bayt sayısı |
| .NET CLR belleği (hizmet başına) | Tüm yığınlardaki bayt sayısı |
| .NET CLR belleği (hizmet başına) | Büyük nesne yığın boyutu |
| .NET CLR belleği (hizmet başına) | # GC tutamaçları |
| .NET CLR belleği (hizmet başına) | # Gen 0 toplamaları |
| .NET CLR belleği (hizmet başına) | # Gen 1 toplamaları |
| .NET CLR belleği (hizmet başına) | Gen 2 toplamaları sayısı |
| .NET CLR belleği (hizmet başına) | GC 'de% Time |

### <a name="service-fabrics-custom-performance-counters"></a>Service Fabric özel performans sayaçları

Service Fabric, önemli miktarda özel performans sayacı üretir. SDK yüklü ise, performans Izleyicisi uygulamanızda Windows makinenizde kapsamlı listeyi görebilirsiniz (> performans Izleyicisini başlatın). 

Kümenize dağıttığınız uygulamalarda, Reliable Actors kullanıyorsanız, `Service Fabric Actor` ve `Service Fabric Actor Method` kategorilerinden sayaç ekleyin (bkz. [Service Fabric Reliable Actors Diagnostics](service-fabric-reliable-actors-diagnostics.md)).

Reliable Services veya hizmet uzaktan Iletişim kullanıyorsanız, ' den sayaç toplamanız gereken `Service Fabric Service` ve `Service Fabric Service Method` sayaç kategorilerimiz vardır. bkz. [hizmet uzaktan iletişim](service-fabric-reliable-serviceremoting-diagnostics.md) ve [güvenilir hizmetler performans sayaçları](service-fabric-reliable-services-diagnostics.md#performance-counters)ile izleme. 

Güvenilir koleksiyonlar kullanıyorsanız, işlem ölçümü başına ortalama işleme gecikmesini toplamak için `Service Fabric Transactional Replicator` `Avg. Transaction ms/Commit` eklenmesini öneririz.


## <a name="next-steps"></a>Sonraki adımlar

* Service Fabric içindeki [platform düzeyinde olay oluşturma](service-fabric-diagnostics-event-generation-infra.md) hakkında daha fazla bilgi edinin
* [Log Analytics Agent](service-fabric-diagnostics-oms-agent.md) aracılığıyla performans ölçümlerini toplayın
