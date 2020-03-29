---
title: Kaynakları Ölçeklendir
description: Bu makalede, ayrılan kaynakları ekleyerek veya kaldırarak veritabanınızı nasıl ölçeklendiriniz açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnik, carlrab
ms.date: 06/25/2019
ms.openlocfilehash: c4366b2718271b1e27325e6946c5016e9230cea4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76835921"
---
# <a name="dynamically-scale-database-resources-with-minimal-downtime"></a>En az kapalı kalma süresiyle veritabanı kaynaklarını dinamik olarak ölçeklendirin

Azure SQL Veritabanı, en az [kapalı kalma süresiyle](https://azure.microsoft.com/support/legal/sla/sql-database/v1_2/)veritabanınıza dinamik olarak daha fazla kaynak eklemenizi sağlar; ancak, bağlantının kısa bir süre için veritabanına kaybolduğu ve yeniden deneme mantığı kullanılarak azaltılabilen bir geçiş süresi boyunca vardır.

## <a name="overview"></a>Genel Bakış

Uygulamanıza olan talep bir avuç cihaz ve müşteriden milyonlarca müşteriye kadar arttığında, Azure SQL Veritabanı en az kapalı kalma süresiyle anında ölçeklenir. Ölçeklenebilirlik, PaaS'ın gerektiğinde hizmetinize dinamik olarak daha fazla kaynak eklemenizi sağlayan en önemli özelliklerinden biridir. Azure SQL Veritabanı, veritabanlarınıza ayrılan kaynakları (CPU gücü, bellek, IO oluşturma ve depolama) kolayca değiştirmenizi sağlar.

Dizin oluşturma veya sorgu yeniden yazma yöntemleri kullanılarak düzeltilemeyen uygulamanızın artan kullanımı nedeniyle performans sorunlarını azaltabilirsiniz. Daha fazla kaynak eklemek, veritabanınız geçerli kaynak sınırlarına ulaştığında ve gelen iş yükünü işlemek için daha fazla güce ihtiyaç duyduğunda hızlı bir şekilde tepki vermenizi sağlar. Azure SQL Veritabanı, maliyeti düşürmek için gerekli olmadığında kaynakları ölçeklendirmenize de olanak tanır.

Donanım satın alma ve temel altyapı değiştirme konusunda endişelenmenize gerek yoktur. Veritabanı ölçekleme, bir kaydırıcı kullanılarak Azure portalı üzerinden kolayca yapılabilir.

![Veritabanı performansını ölçeklendir](media/sql-database-scalability/scale-performance.svg)

Azure SQL [Veritabanı, DTU tabanlı satın alma modelini](sql-database-service-tiers-dtu.md) ve [vCore tabanlı satın alma modelini](sql-database-service-tiers-vcore.md)sunar.

- [DTU tabanlı satın alma modeli,](sql-database-service-tiers-dtu.md) ağır veritabanı iş yüklerine hafifliği desteklemek için üç hizmet katmanında işlem, bellek ve IO kaynaklarının bir karışımını sunar: Temel, Standart ve Premium. Her katman içindeki performans düzeyleri, bu kaynakların farklı bir karışımını sağlar ve bunlara ek depolama kaynakları da eklenebilir.
- [vCore tabanlı satın alma modeli,](sql-database-service-tiers-vcore.md) vCores sayısını, miktarı veya bellek ve depolama miktarı ve hızı seçmenizi sağlar. Bu satın alma modeli üç hizmet katmanı sunar: Genel Amaç, İş Açısından Kritik ve Hiper Ölçek.

İlk uygulamanızı Temel, Standart veya Genel Amaçlı hizmet katmanında ayda düşük bir maliyetle küçük, tek bir veritabanında oluşturabilir ve daha sonra hizmet katmanını herhangi bir zamanda premium veya business critical hizmet katmanına el ile veya programlı olarak değiştirebilirsiniz. çözümünüzün ihtiyaçları. Performansı uygulamanız veya müşterileriniz kesinti yaşamadan ayarlayabilirsiniz. Dinamik ölçeklenebilirlik, veritabanınızın hızla değişen kaynak gereksinimlerine hızlı şekilde yanıt vermesini ve yalnızca ihtiyaç duyduğunuz kaynaklara ve ihtiyaç duyduğunuz süre boyunca ödeme yapmanızı sağlar.

> [!NOTE]
> Dinamik ölçeklenebilirlik, otomatik ölçeklendirmeden farklıdır. Otomatik ölçeklendirme, bir hizmetin ölçütlere göre otomatik olarak ölçeklendirildiğinde, dinamik ölçeklenebilirlik ise en az kapalı kalma süresiyle manuel ölçeklendirmeye olanak sağlar.

Tek Azure SQL Veritabanı, el ile dinamik ölçeklenebilirliği destekler, ancak otomatik ölçeklendirmeyi desteklemez. Daha *otomatik* bir deneyim için, veritabanlarının tek tek veritabanı gereksinimlerine göre bir havuzdaki kaynakları paylaşmasına olanak sağlayan elastik havuzları kullanın.
Ancak, tek bir Azure SQL Veritabanı için ölçeklenebilirliği otomatikleştirmeye yardımcı olabilecek komut dosyaları vardır. Örnek için bkz. [PowerShell kullanarak tek bir SQL Veritabanını izleme ve ölçeklendirme](scripts/sql-database-monitor-and-scale-database-powershell.md).

Uygulamanızda çok az (genellikle ortalama dört saniyenin altında) kesinti ile dilediğiniz zaman [DTU hizmet katmanlarını](sql-database-service-tiers-dtu.md) veya [sanal çekirdek özelliklerini](sql-database-vcore-resource-limits-single-databases.md) değiştirebilirsiniz. Veritabanı oluşturabilmek ve veritabanı performansını isteğe göre yükseltip düşürebilmek, özellikle kullanım biçimlerinin nispeten tahmin edilebilir olduğu durumlarda birçok işletme ve uygulama için yeterlidir. Ancak tahmin edilemeyen kullanım biçimlerine sahipseniz bu durum maliyetlerin ve iş modelinizin yönetimini zorlaştırabilir. Bu senaryo için, havuzdaki birden çok veritabanları arasında paylaşılan belirli sayıda eLASTIK elastik havuz kullanırsınız.

![SQL Veritabanı'na Giriş: Katmana ve düzeye göre tek veritabanı DTU’ları](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Azure SQL Veritabanı'nın üç tadına da sahip olduğu üç tat, veritabanlarınızı dinamik olarak ölçeklendirme olanağı sunar:

- Tek bir veritabanıyla, her [veritabanına](sql-database-single-database-scale.md)atanacak maksimum kaynak miktarını tanımlamak için [DTU](sql-database-dtu-resource-limits-single-databases.md) veya [vCore](sql-database-vcore-resource-limits-single-databases.md) modellerini kullanabilirsiniz.
- [Yönetilen Örnek](sql-database-managed-instance.md) [vCores](sql-database-managed-instance.md#vcore-based-purchasing-model) modunu kullanır ve en yüksek CPU çekirdeklerini ve örneğinize ayrılan maksimum depolama alanını tanımlamanızı sağlar. Örnekteki tüm veritabanları, örne ayrılan kaynakları paylaşır.
- [Esnek havuzlar,](sql-database-elastic-pool-scale.md) havuzdaki veritabanları grubu başına maksimum kaynak sınırı tanımlamanızı sağlar.

Herhangi bir tatta ölçeği ölçeklendirmek veya küçültme küçme işlemi veritabanı altyapısı işlemini yeniden başlatacak ve gerekirse farklı bir sanal makineye taşıyacak. Veritabanı altyapısı işlemini yeni bir sanal makineye taşımak, işlem devam ederken varolan Azure SQL Veritabanı hizmetinizi kullanmaya devam edebileceğiniz **çevrimiçi işlemdir.** Hedef veritabanı altyapısı tamamen başlatılan ve sorguları işlemek için hazır olduğunda, bağlantılar [kaynaktan hedef veritabanı altyapısına](sql-database-single-database-scale.md#impact)geçirilecektir. 


> [!NOTE]
> Ölçeklendirme/küçültme işlemi tamamlandığında kısa bir bağlantı sonu bekleyebilirsiniz. [Standart geçici hatalar için Yeniden Deneme mantığını](sql-database-connectivity-issues.md#retry-logic-for-transient-errors)uyguladıysanız, başarısızlığı fark etmezsiniz.

## <a name="alternative-scale-methods"></a>Alternatif ölçek yöntemleri

Kaynakları ölçekleme, veritabanıveya uygulama kodunu değiştirmeden veritabanınızın performansını artırmanın en kolay ve en etkili yoludur. Bazı durumlarda, en yüksek hizmet katmanları, işlem boyutları ve performans optimizasyonları bile iş yükünüzü başarılı ve uygun maliyetli bir şekilde işlemeyebilir. Bu durumda veritabanınızı ölçeklendirmek için şu ek seçeneklere sahipsiniz:

- [Okuma ölçeği,](sql-database-read-scale-out.md) raporlar gibi yalnızca okunan sorguları gerçekleştirebileceğiniz verilerinizin salt okunur kopyasını aldığınız kullanılabilir bir özelliktir. Salt okunur yineleme, birincil veritabanınızdaki kaynak kullanımını etkilemeden salt okunur iş yükünüzü işler.
- [Veritabanı parçalama,](sql-database-elastic-scale-introduction.md) verilerinizi çeşitli veritabanlarına bölmenize ve bunları bağımsız olarak ölçeklendirmenize olanak tanıyan bir teknik ler kümesidir.

## <a name="next-steps"></a>Sonraki adımlar

- Veritabanı kodunu değiştirerek veritabanı performansını artırma hakkında bilgi [için](sql-database-advisor-portal.md)bkz.
- Yerleşik veritabanı zekasının veritabanınızı optimize etmesine izin verme hakkında bilgi için [Otomatik ayar'a](sql-database-automatic-tuning.md)bakın.
- Azure SQL Veritabanı hizmetinde Okuma Ölçeği hakkında bilgi için, [salt okunur sorgu iş yüklerini yüklemek için salt okunur yinelemelerin](sql-database-read-scale-out.md)nasıl kullanılacağını öğrenin.
- Veritabanı parçalama hakkında daha fazla bilgi için [Azure SQL Veritabanı ile ölçekleme](sql-database-elastic-scale-introduction.md)bkz.
