---
title: Azure VM 'lerinde SQL Server yedekleme için Azure Backup destek matrisi
description: Azure Backup hizmeti ile Azure VM 'lerinde SQL Server yedeklenirken destek ayarlarının ve sınırlamaların özetini sağlar.
ms.topic: conceptual
ms.date: 03/05/2020
ms.custom: references_regions
ms.openlocfilehash: 4d197f8b3c1ed74ef45c1f7942ead52ccef0c14a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513192"
---
# <a name="support-matrix-for-sql-server-backup-in-azure-vms"></a>Azure VM 'lerinde SQL Server yedekleme için destek matrisi

Azure Backup, Microsoft Azure bulut platformunda barındırılan Azure VM 'lerinde SQL Server veritabanlarını yedeklemek için kullanabilirsiniz. Bu makalede, Azure VM 'lerinde SQL Server yedekleme senaryoları ve dağıtımları için genel destek ayarları ve sınırlamaları özetlenmektedir.

## <a name="scenario-support"></a>Senaryo desteği

**Destek** | **Ayrıntılar**
--- | ---
**Desteklenen dağıtımlar** | SQL Market Azure VM 'Leri ve Market olmayan (SQL Server el ile yüklenmiş) VM 'Ler desteklenir.
**Desteklenen bölgeler** | Avustralya Güney Doğu (Ao), Doğu Avustralya (AE), Avustralya Orta (AC), Avustralya Orta 2 (AC) <br> Güney Brezilya (BRS)<br> Kanada Orta (CNC), Kanada Doğu (CE)<br> Güney Doğu Asya (SEA), Doğu Asya (EA) <br> Doğu ABD (EUS), Doğu ABD 2 (EUS2), Orta Batı ABD (WCUS), Batı ABD (WUS); Batı ABD 2 (WUS 2) Orta Kuzey ABD (NCUS) Orta ABD (cu DÜZEYINDE KAPSANıR) Orta Güney ABD (SCUS) <br> Hindistan Orta (ıNC), Hindistan Güney (INS), Hindistan Batı <br> Japonya Doğu (JPE), Japonya Batı (JPW) <br> Kore Orta (KRC), Kore Güney (KRS) <br> Kuzey Avrupa (NE), Batı Avrupa <br> UK Güney (UKS), UK Batı (UKW) <br> US Gov Arizona, US Gov Virginia, US Gov Teksas, US DoD Orta, US DoD Doğu <br> Almanya Kuzey, Almanya Orta Batı <br> İsviçre Kuzey, İsviçre Batı <br> Orta Fransa <br> Çin Doğu, Çin Doğu 2, Çin Kuzey, Çin Kuzey 2
**Desteklenen işletim sistemleri** | Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2 SP1 <br/><br/> Linux Şu anda desteklenmiyor.
**Desteklenen SQL Server sürümleri** | SQL Server 2019, [ürün yaşam döngüsünü ara](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017)sayfasında SQL Server 2017 SQL Server 2016 ve SPS 'yi [ürün yaşam döngüsünü ara sayfasında](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack)ayrıntılı olarak SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 <br/><br/> Enterprise, Standard, Web, geliştirici, Express.
**Desteklenen .NET sürümleri** | VM 'de yüklü .NET Framework 4.5.2 veya üzeri

## <a name="feature-consideration-and-limitations"></a>Özellik değerlendirmesi ve sınırlamaları

* SQL Server yedekleme Azure portal veya **PowerShell**içinde yapılandırılabilir. CLı 'yi desteklemiyoruz.
* Çözüm her iki tür [dağıtım](../azure-resource-manager/management/deployment-models.md) için de desteklenir-Azure Resource Manager VM 'ler ve klasik VM 'ler.
* SQL Server çalıştıran VM, Azure genel IP adreslerine erişmek için internet bağlantısı gerektirir.
* SQL Server **Yük devretme kümesi örneği (FCı)** desteklenmez.
* Yansıtma veritabanları ve veritabanı anlık görüntüleri için yedekleme ve geri yükleme işlemleri desteklenmez.
* Tek başına SQL Server örneğinizi veya SQL Always on kullanılabilirlik grubunu yedeklemek için birden fazla yedekleme çözümü kullanmak yedekleme hatasına neden olabilir; Bunu yapmaktan kaçının.
* Bir kullanılabilirlik grubunun iki düğümünü aynı veya farklı çözümlerle tek tek yedeklemek, yedekleme hatasına da neden olabilir.
* Azure Backup **salt okunurdur** veritabanları Için yalnızca tam ve salt kopya tam yedekleme türlerini destekler
* Çok sayıda dosya içeren veritabanları korunamaz. Desteklenen en fazla dosya sayısı **~ 1000**' dir.  
* Bir kasadaki **~ 2000** SQL Server veritabanlarını yedekleyebilirsiniz. Daha fazla veritabanınız olması durumunda birden çok kasa oluşturabilirsiniz.
* Yedeklemeyi tek bir go 'da en fazla **50** veritabanına yapılandırabilirsiniz; Bu kısıtlama, yedekleme yüklerini iyileştirmenize yardımcı olur.
* Boyutu **2 TB** 'a kadar olan veritabanlarını destekliyoruz; performans sorunlarından daha büyük boyutlarda olabilir.
* Sunucu başına kaç tane veritabanı korunduğuna ilişkin bir fikir sahibi olmak için bant genişliği, VM boyutu, yedekleme sıklığı, veritabanı boyutu vb. gibi faktörleri göz önünde bulundurun. VM kaynaklarına ve yedekleme ilkesine göre sunucu başına sahip olduğunuz birçok veritabanı sayısını hesaplamak için kaynak Planlayıcısı ' nı [indirin](https://download.microsoft.com/download/A/B/5/AB5D86F0-DCB7-4DC3-9872-6155C96DE500/SQL%20Server%20in%20Azure%20VM%20Backup%20Scale%20Calculator.xlsx) .
* Kullanılabilirlik grupları yapılandırıldığında, yedeklemeler, birkaç etkene göre farklı düğümlerden alınır. Bir kullanılabilirlik grubu için yedekleme davranışı aşağıda özetlenmiştir.

### <a name="back-up-behavior-with-always-on-availability-groups"></a>Always On kullanılabilirlik gruplarında yedekleme davranışı

Yedeklemenin, bir kullanılabilirlik grubunun (AG) yalnızca bir düğümünde yapılandırılmasını öneririz. Her zaman yedeklemeyi birincil düğümle aynı bölgede yapılandırın. Diğer bir deyişle, her zaman yedeklemeyi yapılandırdığınız bölgede birincil düğümün bulunması gerekir. AG 'nin tüm düğümleri yedeklemenin yapılandırıldığı bölgede ise sorun yoktur.

#### <a name="for-cross-region-ag"></a>Bölgeler arası AG için

* Yedekleme tercihinden bağımsız olarak, yedeklemeler yalnızca yedeklemenin yapılandırıldığı bölgedeki düğümlerden çalıştırılır. Bunun nedeni, bölgeler arası yedeklemelerin desteklenmemektedir. Yalnızca iki düğümünüz varsa ve ikincil düğüm diğer bölgedeyse, yedeklemeler birincil düğümden çalışmaya devam eder (yedekleme tercihiniz ' ikincil yalnızca ' değilse).
* Bir düğüm, yedeklemenin yapılandırıldığı noktadan farklı bir bölgeye yük devreder, yedeklemeler başarısız olan bölgedeki düğümlerde başarısız olur.

Yedekleme tercihine ve yedeklemeler türlerine (tam/değişiklik/günlük/salt kopya) bağlı olarak, yedeklemeler belirli bir düğümden alınır (birincil/ikincil).

#### <a name="backup-preference-primary"></a>Yedekleme tercihi: birincil

**Yedekleme türü** | **Düğüm**
--- | ---
Tam | Birincil
Di | Birincil
Log |  Birincil
Salt kopya tam |  Birincil

#### <a name="backup-preference-secondary-only"></a>Yedekleme tercihi: yalnızca Ikincil

**Yedekleme türü** | **Düğüm**
--- | ---
Tam | Birincil
Di | Birincil
Log |  İkincil
Salt kopya tam |  İkincil

#### <a name="backup-preference-secondary"></a>Yedekleme tercihi: Ikincil

**Yedekleme türü** | **Düğüm**
--- | ---
Tam | Birincil
Di | Birincil
Log |  İkincil
Salt kopya tam |  İkincil

#### <a name="no-backup-preference"></a>Yedekleme tercihi yok

**Yedekleme türü** | **Düğüm**
--- | ---
Tam | Birincil
Di | Birincil
Log |  İkincil
Salt kopya tam |  İkincil

## <a name="next-steps"></a>Sonraki adımlar

Azure VM 'de çalışan [SQL Server bir veritabanını nasıl yedekleyeceğinizi](backup-azure-sql-database.md) öğrenin.
