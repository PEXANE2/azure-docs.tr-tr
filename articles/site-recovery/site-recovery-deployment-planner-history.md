---
title: Azure Sitesi Kurtarma Dağıtım Planlayıcısı Sürüm Geçmişi
description: Bilinen farklı Site Kurtarma Dağıtım Planlayıcısı Sürümleri, sürüm tarihleriyle birlikte düzeltir ve bilinir.
services: site-recovery
author: Daya-Patil
manager: carmonm
ms.topic: article
ms.service: site-recovery
ms.date: 10/16/2019
ms.author: dapatil
ms.openlocfilehash: bf32809f426f3bfcabd08ec3bd95e76202aa8f84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72433420"
---
# <a name="azure-site-recovery-deployment-planner-version-history"></a>Azure Sitesi Kurtarma Dağıtım Planlayıcısı Sürüm Geçmişi

Bu makalede, Azure Site Kurtarma Dağıtım Planlayıcısı'nın tüm sürümlerinin yanı sıra düzeltmeler, her birinde bilinen sınırlamalar ve bunların çıkış tarihlerinde geçmiş sağlanmaktadır.

## <a name="version-251"></a>Sürüm 2.51

**Yayın Tarihi: 22 Ağustos 2019**

**Giderir:**

- Dağıtım Planlayıcısı sürüm 2.5 ile maliyet önerisi sorunu düzeltildi

## <a name="version-25"></a>Sürüm 2.5

**Yayın Tarihi: 29 Temmuz 2019**

**Giderir:**

- VMware sanal makineleri ve fiziksel makineleri için, öneri Yönetilen Diskler çoğaltma dayalı olarak güncelleştirilir.
- Windows 10 (x64), Windows 8.1 (x64), Windows 8 (x64), Windows 7 (x64) SP1 veya sonrası için destek eklendi

## <a name="version-24"></a>Sürüm 2.4

**Yayın Tarihi: 17 Nisan 2019**

**Giderir:**

- Özellikle yerelleştirme tabanlı hataları işlerken işletim sistemi uyumluluğunu artırdı.
- Uyumluluk denetim listesine 20 Mbps'ye kadar veri değiştirme hızına (karmaşa) sahip VM'ler eklendi.
- Geliştirilmiş hata iletileri
- vCenter 6.7 için destek eklendi.
- Windows Server 2019 ve Red Hat Enterprise Linux (RHEL) iş istasyonu için destek eklendi.



## <a name="version-23"></a>Sürüm 2.3

**Yayın Tarihi: 3 Aralık 2018**

**Giderir:**

- Dağıtım Planlayıcısı'nın sağlanan hedef konum ve abonelikle birlikte rapor oluşturmasını engelleyen bir sorun düzeltildi.

## <a name="version-22"></a>Sürüm 2.2 

**Yayın Tarihi: 25 Nisan 2018**

**Giderir:**

- GetVMList işlemleri:
  - Belirtilen klasör yoksa GetVMList'in başarısız olmasına neden olan bir sorun giderildi. Şimdi varsayılan dizin oluşturur veya çıktı dosyası parametresinde belirtilen dizin oluşturur.
  - GetVMList için daha ayrıntılı hata nedenleri eklendi.
- Dağıtım Planlayıcısı raporunun uyumlu VM sayfasında sütun olarak VM türü bilgileri eklendi.
- Hyper-V'den Azure'a olağanüstü durum kurtarma:
  - Paylaşılan VHD'lere ve PassThrough disklerine sahip VM'leri profil oluşturmadışında. Başlangıç Profil oluşturma işlemi konsoldaki dışlanmış VM'lerin listesini gösterir.
  - Uyumsuz VM'ler listesine 64'ten fazla diski olan VM'ler eklendi.
  - İlk çoğaltma (IR) ve delta çoğaltma (DR) sıkıştırma faktörü güncelleştirildi.
  - Kobİ depolama için sınırlı destek eklendi.

## <a name="version-21"></a>Sürüm 2.1

**Yayın Tarihi: 3 Ocak 2018**

**Giderir:**

- Excel raporu güncelleştirildi.
- GetThroughput işlemindeki hatalar düzeltildi.
- Raporu oluşturmak veya profili oluşturmak için VM sayısını sınırlamak için seçenek eklendi. Varsayılan sınır 1.000 VM'dir.
- VMware'den Azure olağanüstü durum kurtarmasına:
  - Windows Server 2016 VM'nin uyumsuz tabloya girerken ki sorunu giderildi. 
  - Genişletilebilir Firmware Arabirimi (EFI) Windows VM'leri için güncelleştirilmiş uyumluluk iletileri.
- VMware'i Azure'a ve Hyper-V'den Azure'a, VM başına VM veri karmaşası sınırına güncelleştirin. 
- VM liste dosyası ayrıştının geliştirilmiş güvenilirliği.

## <a name="version-201"></a>Sürüm 2.0.1

**Yayın Tarihi: 7 Aralık 2017**

**Giderir:**

- Ağ bant genişliğini optimize etmek için öneri eklendi.

## <a name="version-20"></a>Sürüm 2.0

**Yayın Tarihi: 28 Kasım 2017**

**Giderir:**

- Azure olağanüstü durum kurtarma için Hyper-V desteği eklendi.
- Maliyet hesaplayıcısı eklendi.
- VM'nin koruma için uyumlu veya uyumsuz olup olmadığını belirlemek için Azure olağanüstü durum kurtarma sına VMware için İşletim sürümü denetimi eklendi. Araç, söz le vM için vCenter sunucusu tarafından döndürülen işletim sistemi sürüm dizesini kullanır. VMware'de VM'yi oluştururken kullanıcının seçtiği konuk işletim sistemi sürümü.

**Bilinen sınırlamalar:**

- Hyper-V'den Azure olağanüstü durum kurtarmaiçin, VM `,`gibi `"` `[`karakterleri `]`içeren bir ad la birlikte: , , , ve ``` ` ``` desteklenmez. Profilli ise, rapor oluşturma başarısız olur veya yanlış bir sonuç elde edecektir.
- VMware'den Azure olağanüstü durum kurtarmasına kadar virgül içeren adiçeren VM desteklenmez. Profilli yse, rapor oluşturma başarısız olur veya yanlış bir sonuç elde eder.

## <a name="version-131"></a>Sürüm 1.3.1

**Yayın Tarihi: 19 Temmuz 2017** 

**Giderir:**

- Rapor oluşturmada büyük diskler (> 1 TB) için destek eklendi. Artık Dağıtım Planlayıcısı'nı kullanarak, 1 TB'den (4095 GB'a kadar) disk boyutlarına sahip sanal makineler için çoğaltmayı planlayabilirsiniz.
[Azure Site Recovery'de büyük disk desteği](https://azure.microsoft.com/blog/azure-site-recovery-large-disks/)

## <a name="version-13"></a>Sürüm 1.3

**Yayın Tarihi: 9 Mayıs 2017**

**Giderir:**

- Rapor oluşturmada yönetilen disk için destek eklendi. Yönetilen disk Failover/Test Failover için seçilip seçilmediğini temel alan, tek bir depolama hesabına yerleştirilebilen VM sayısı hesaplanır.

## <a name="version-12"></a>Sürüm 1.2

**Yayın Tarihi: 7 Nisan 2017**

**Giderir:**

- VM'nin koruma için uyumlu veya uyumsuz olup olmadığını belirlemek için her VM için eklenen önyükleme türü (BIOS veya EFI) denetler.
- Uyumlu VM'lerde ve uyumsuz VM çalışma sayfalarında her sanal makine için işletim sistemi türü bilgileri eklendi.
- ABD Hükümeti ve Çin Microsoft Azure bölgeleri için GetThroughput işlemi için destek eklendi.
- vCenter ve ESXi Server için birkaç tane daha ön koşul denetimi eklendi.
- Yerel ayarlar İngilizce olmayan ayarlarayarlandığında oluşturulan yanlış rapor sorunu düzeltildi.

## <a name="version-11"></a>Sürüm 1.1

**Yayın Tarihi: 9 Mart 2017**

**Giderir:**

- Çeşitli vCenter ESXi ana bilgisayarlarında aynı ada veya IP adresine sahip iki veya daha fazla VM olduğunda, VM'lerin profilinin profilini çıkarmayı engelleyen bir sorun giderildi.
- Uyumlu VM'ler ve uyumsuz VM'ler çalışma sayfaları için kopyalama ve aramanın devre dışı edilmesine neden olan bir sorun düzeltildi.

## <a name="version-10"></a>Sürüm 1.0

**Yayın Tarihi: 23 Şubat 2017**

**Bilinen sınırlamalar:**

- Yalnızca Azure olağanüstü durum kurtarma senaryolarına yalnızca VMware için destekler. Hyper-V'den Azure'a olağanüstü durum kurtarma senaryoları için [Hyper-V kapasite planlayıcısı aracını](./site-recovery-capacity-planning-for-hyper-v-replication.md)kullanın.
- ABD Hükümeti ve Çin Microsoft Azure bölgeleri için GetThroughput işlemini desteklemez.
- VCenter sunucusunda çeşitli ESXi ana bilgisayarlarında aynı ada veya IP adresine sahip iki veya daha fazla VM varsa, araç VM'leri profilleyebilir.
Bu sürümde, araç VMListFile dosyasındaki yinelenen sanal makine adları veya IP adresleri için profil oluşturmayı atlar. Bunun geçici çözümü, sanal makine profillerinin vCenter sunucusu yerine bir ESXi ana bilgisayarı kullanılarak oluşturulmasıdır. Her ESXi ana bilgisayarı için bir örnek çalıştırdığından emin olun.
