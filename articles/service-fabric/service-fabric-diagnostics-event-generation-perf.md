---
title: Azure Hizmet Kumaş Performans İzleme
description: Azure Hizmet Kumaşı kümelerinin izlenmesi ve teşhisi için performans sayaçları hakkında bilgi edinin.
author: srrengar
ms.topic: conceptual
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 30b9b8393007033a7c2e6798cd57d9cf0128820d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464709"
---
# <a name="performance-metrics"></a>Performans ölçümleri

Kümenizin performansını ve içinde çalışan uygulamaları anlamak için ölçümler toplanmalıdır. Service Fabric kümeleri için aşağıdaki performans sayaçlarını toplamanızı öneririz.

## <a name="nodes"></a>Düğümler

Kümenizdeki makineler için, her makinedeki yükü daha iyi anlamak ve uygun küme ölçekleme kararları almak için aşağıdaki performans sayaçlarını toplamayı düşünün.

| Sayaç Kategorisi | Sayaç Adı |
| --- | --- |
| Mantıksal Disk | Mantıksal Disk Boş Alan |
| PhysicalDisk(Disk başına) | Avg. Disk Okuma Sıra Uzunluğu |
| PhysicalDisk(Disk başına) | Avg. Disk Yazma Sıra Uzunluğu |
| PhysicalDisk(Disk başına) | Avg. Disk sn/Oku |
| PhysicalDisk(Disk başına) | Avg. Disk sn/Yazma |
| PhysicalDisk(Disk başına) | Disk Okuma/sn |
| PhysicalDisk(Disk başına) | Disk Okuma Bayt/sn |
| PhysicalDisk(Disk başına) | Disk Yazma/sn |
| PhysicalDisk(Disk başına) | Disk Bayt/sn Yazma |
| Bellek | Kullanılabilir MBytes |
| PagingDosya | % Kullanım |
| İşlemci(Toplam) | % İşlemci zamanı |
| İşlem (hizmet başına) | % İşlemci zamanı |
| İşlem (hizmet başına) | Kimlik İşlemi |
| İşlem (hizmet başına) | Özel Baytlar |
| İşlem (hizmet başına) | Ilmekli |
| İşlem (hizmet başına) | Sanal Bayt Sayısı |
| İşlem (hizmet başına) | Çalışma Kümesi |
| İşlem (hizmet başına) | Çalışma Seti - Özel |
| Ağ Arabirimi (tüm örnekler) | Bayt recd |
| Ağ Arabirimi (tüm örnekler) | Gönderilen Bayt |
| Ağ Arabirimi (tüm örnekler) | Bayt toplamı |
| Ağ Arabirimi (tüm örnekler) | Çıktı Sıra Uzunluğu |
| Ağ Arabirimi (tüm örnekler) | Giden Atılan Paketler |
| Ağ Arabirimi (tüm örnekler) | Alınan Paketler Atılır |
| Ağ Arabirimi (tüm örnekler) | Paketler Giden Hatalar |
| Ağ Arabirimi (tüm örnekler) | Alınan Paketler Hatalar |

## <a name="net-applications-and-services"></a>.NET uygulamaları ve hizmetleri

.NET hizmetlerini kümenize dağıtıyorsanız aşağıdaki sayaçları toplayın. 

| Sayaç Kategorisi | Sayaç Adı |
| --- | --- |
| .NET CLR Bellek (hizmet başına) | İşlem Kimliği |
| .NET CLR Bellek (hizmet başına) | # Toplam taahhüt Bayt |
| .NET CLR Bellek (hizmet başına) | # Toplam ayrılmış Bayt |
| .NET CLR Bellek (hizmet başına) | # Tüm Yığınlar içinde Bayt |
| .NET CLR Bellek (hizmet başına) | Büyük Nesne Yığını boyutu |
| .NET CLR Bellek (hizmet başına) | # GC Kolları |
| .NET CLR Bellek (hizmet başına) | # Gen 0 Koleksiyonları |
| .NET CLR Bellek (hizmet başına) | # Gen 1 Koleksiyonları |
| .NET CLR Bellek (hizmet başına) | # Gen 2 Koleksiyonları |
| .NET CLR Bellek (hizmet başına) | GC'de % Süre |

### <a name="service-fabrics-custom-performance-counters"></a>Service Fabric'in özel performans sayaçları

Service Fabric önemli miktarda özel performans sayacı üretir. SDK yüklüyse, Performans Monitörü uygulamanızda (Performans İzleme > Başlat) Windows makinenizdeki kapsamlı listeyi görebilirsiniz. 

Kümenize dağıttığınız uygulamalarda, Güvenilir Aktörler kullanıyorsanız, sayaçlar `Service Fabric Actor` ve `Service Fabric Actor Method` kategoriler ekleyin (Bkz. Service Fabric Reliable Actors [Diagnostics).](service-fabric-reliable-actors-diagnostics.md)

Güvenilir Hizmetler veya Hizmet Remoting kullanıyorsanız, `Service Fabric Service` `Service Fabric Service Method` biz benzer ve sayaçları toplamak gerekir sayaçları [sayaçları sayaçları sayaçları sayaçları sayaçları sayaçları](service-fabric-reliable-services-diagnostics.md#performance-counters)var, [hizmet remoting](service-fabric-reliable-serviceremoting-diagnostics.md) ve güvenilir hizmet performans sayaçları ile izleme bakın. 

Güvenilir Koleksiyonlar kullanıyorsanız, işlem `Avg. Transaction ms/Commit` ölçümü `Service Fabric Transactional Replicator` başına ortalama gecikme gecikmesi toplamak için göndereni eklemenizi öneririz.


## <a name="next-steps"></a>Sonraki adımlar

* Service [Fabric'te platform düzeyinde etkinlik oluşturma](service-fabric-diagnostics-event-generation-infra.md) hakkında daha fazla bilgi edinin
* [Log Analytics aracısı](service-fabric-diagnostics-oms-agent.md) aracılığıyla performans ölçümleri toplama
