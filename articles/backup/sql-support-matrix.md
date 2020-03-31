---
title: Azure VM'lerde SQL Server Backup için Azure Yedekleme destek matrisi
description: Azure Yedekleme hizmetiyle Azure VM'lerde SQL Server'ı yedeklerken destek ayarlarının ve sınırlamaların bir özetini sağlar.
ms.topic: conceptual
ms.date: 03/05/2020
ms.openlocfilehash: 79a7e30ab9240c489a66b547ff85bea7887131b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79410005"
---
# <a name="support-matrix-for-sql-server-backup-in-azure-vms"></a>Azure VM'lerde SQL Server Backup için destek matrisi

Azure Yedekleme'yi, Microsoft Azure bulut platformunda barındırılan Azure VM'lerde SQL Server veritabanlarını yedeklemek için kullanabilirsiniz. Bu makalede, Azure VM'lerde SQL Server Backup senaryoları ve dağıtımları için genel destek ayarları ve sınırlamaları özetlenmiştir.

## <a name="scenario-support"></a>Senaryo desteği

**Destek** | **Şey**
--- | ---
**Desteklenen dağıtımlar** | SQL Marketplace Azure VM'ler ve Market dışı (SQL Server el ile yüklenir) VM'ler desteklenir.
**Desteklenen geos** | Avustralya Güney Doğu (ASE), Doğu Avustralya (AE), Avustralya Merkez (AC), Avustralya Merkez 2 (AC) <br> Güney Brezilya (BRS)<br> Kanada Merkez (CNC), Kanada Doğu (CE)<br> Güney Doğu Asya (SEA), Doğu Asya (EA) <br> Doğu ABD (EUS), Doğu ABD 2 (EUS2), Batı Orta ABD (WCUS), Batı ABD (WUS); Batı ABD 2 (WUS 2) Kuzey Orta ABD (NCUS) Orta ABD (CUS) Güney Orta ABD (SCUS) <br> Hindistan Merkez (INC), Hindistan Güney (INS), Hindistan Batı <br> Japonya Doğu (JPE), Batı Japonya (JPW) <br> Kore Merkez (KRC), Kore Güney (KRS) <br> Kuzey Avrupa (NE), Batı Avrupa <br> İngiltere Güney (UKS), İngiltere Batı (UKW) <br> ABD Gov Arizona, ABD Gov Virginia, ABD Gov Texas, ABD DoD Merkez, ABD DoD Doğu <br> Almanya Kuzey, Almanya Batı Orta <br> İsviçre Kuzey, İsviçre Batı <br> Orta Fransa <br> Çin Doğu, Çin Doğu 2, Çin Kuzey, Çin Kuzey 2
**Desteklenen işletim sistemleri** | Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2 SP1 <br/><br/> Linux şu anda desteklenmez.
**Desteklenen SQL Server sürümleri** | SQL Server 2019, SQL Server 2017, [Arama ürün yaşam döngüsü sayfasında](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202017)ayrıntılı olarak , SQL Server 2016 ve SPs olarak arama ürün yaşam döngüsü [sayfasında](https://support.microsoft.com/lifecycle/search?alpha=SQL%20server%202016%20service%20pack)ayrıntılı olarak , SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008 <br/><br/> Kurumsal, Standart, Web, Geliştirici, Express.
**Desteklenen .NET sürümleri** | .NET Framework 4.5.2 veya daha sonra VM yüklü

## <a name="feature-consideration-and-limitations"></a>Özellik dikkate alınması ve sınırlamalar

* SQL Server yedeklemesi Azure portalında veya **PowerShell'de**yapılandırılabilir. CLI'yi destekliyoruz.
* Çözüm, azure kaynak yöneticisi [vm'leri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) ve klasik VM'ler olmak gibi her iki dağıtım türünde de desteklenir.
* VM running SQL Server, Azure genel IP adreslerine erişmek için Internet bağlantısı gerektirir.
* SQL Server **Failover Cluster Instance (FCI)** desteklenmez.
* Ayna veritabanları ve veritabanı anlık görüntüleri için işlemleri yedekleme ve geri yükleme desteklenmez.
* Tek başına SQL Server örneğini veya SQL Her zaman kullanılabilirlik grubunda yedekleme hatanızı yedeklemek için birden fazla yedekleme çözümü kullanmak; bunu yapmaktan kaçının.
* Aynı veya farklı çözümlerle bir kullanılabilirlik grubunun iki düğümünün tek tek yedeklemesi de yedekleme hatasına yol açabilir.
* Azure Yedekleme yalnızca **Okunur** veritabanları için yalnızca Tam ve Kopyala Tam yedekleme türlerini destekler
* Çok sayıda dosyaiçeren veritabanları korunamıyor. Desteklenen dosyaların maksimum sayısı ~ **1000**olduğunu.  
* Bir kasada **~2000** SQL Server veritabanlarını yedekleyebilirsiniz. Daha fazla sayıda veritabanınız olması durumunda birden çok kasa oluşturabilirsiniz.
* Yedeklemeyi tek bir çekimde en fazla **50** veritabanları için yapılandırabilirsiniz; bu kısıtlama yedekleme yüklerini optimize etmesine yardımcı olur.
* **2 TB** boyutuna kadar veritabanlarını destekliyoruz; bu performans sorunları daha büyük boyutlarda için gelebilir.
* Sunucu başına kaç veritabanının korunabileceği konusunda bir anlayışa sahip olmak için bant genişliği, VM boyutu, yedekleme frekansı, veritabanı boyutu gibi faktörleri göz önünde bulundurun. VM kaynaklarını ve yedekleme ilkesini temel alarak sunucu başına sahip olabileceğiniz yaklaşık veritabanları sayısını hesaplamak için kaynak planlayıcısını [indirin.](https://download.microsoft.com/download/A/B/5/AB5D86F0-DCB7-4DC3-9872-6155C96DE500/SQL%20Server%20in%20Azure%20VM%20Backup%20Scale%20Calculator.xlsx)
* Kullanılabilirlik grupları yapılandırıldığında, yedeklemeler birkaç etkene bağlı olarak farklı düğümlerden alınır. Kullanılabilirlik grubunun yedekleme davranışı aşağıda özetlenmiştir.

### <a name="back-up-behavior-with-always-on-availability-groups"></a>Kullanılabilirlik gruplarında Her Zaman ile davranışı yedekleme

Yedeklemenin kullanılabilirlik grubunun (AG) yalnızca bir düğümünde yapılandırılmalarını öneririz. Yedeklemeyi her zaman birincil düğümle aynı bölgede yapılandırın. Başka bir deyişle, yedeklemeyi yapılandırdığınız bölgede birincil düğümün bulunması her zaman gerekir. AG'nin tüm düğümleri yedeklemenin yapılandırıldığı aynı bölgedeyse, herhangi bir sorun yoktur.

#### <a name="for-cross-region-ag"></a>Bölgeler arası AG için

* Yedekleme tercihinden bağımsız olarak, yedeklemeler yalnızca yedeklemenin yapılandırıldığı aynı bölgede bulunan düğümlerden çalışır. Bunun nedeni, bölgeler arası yedeklemelerin desteklenmemiş olmasıdır. Yalnızca iki düğümünüz varsa ve ikincil düğüm diğer bölgedeyse, yedeklemeler birincil düğümden çalışmaya devam eder (yedekleme tercihiniz 'yalnızca ikincil' değilse).
* Bir düğüm, yedeklemenin yapılandırıldığı bölgeden farklı bir bölgeye geçmezse, yedekler başarısız olan bölgedeki düğümlerde başarısız olur.

Yedekleme tercihine ve yedekleme türlerine (tam/diferansiyel/log/copy-only full) bağlı olarak, yedeklemeler belirli bir düğümden (birincil/ikincil) alınır.

#### <a name="backup-preference-primary"></a>Yedekleme tercihi: Birincil

**Yedekleme Türü** | **Node**
--- | ---
Tam | Birincil
Fark | Birincil
Günlük |  Birincil
Yalnızca Tam Kopyala |  Birincil

#### <a name="backup-preference-secondary-only"></a>Yedekleme tercihi: Yalnızca İkincil

**Yedekleme Türü** | **Node**
--- | ---
Tam | Birincil
Fark | Birincil
Günlük |  İkincil
Yalnızca Tam Kopyala |  İkincil

#### <a name="backup-preference-secondary"></a>Yedekleme tercihi: İkincil

**Yedekleme Türü** | **Node**
--- | ---
Tam | Birincil
Fark | Birincil
Günlük |  İkincil
Yalnızca Tam Kopyala |  İkincil

#### <a name="no-backup-preference"></a>Yedekleme tercihi yok

**Yedekleme Türü** | **Node**
--- | ---
Tam | Birincil
Fark | Birincil
Günlük |  İkincil
Yalnızca Tam Kopyala |  İkincil

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'de çalışan [bir SQL Server veritabanını](backup-azure-sql-database.md) nasıl yedekleyeceğimiz öğrenin.
