---
title: SQL havuzunuzu Gen2'ye geçirin
description: Varolan bir SQL havuzunu Gen2'ye geçirme yönergeleri ve bölgeye göre geçiş zamanlaması.
services: synapse-analytics
author: mlee3gsd
ms.author: anjangsh
ms.reviewer: jrasnick
manager: craigg
ms.assetid: 04b05dea-c066-44a0-9751-0774eb84c689
ms.service: synapse-analytics
ms.topic: article
ms.date: 01/21/2020
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 6f2af826473bfd13f8100796a540d41cbedbb037
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80631575"
---
# <a name="upgrade-your-sql-pool-to-gen2"></a>SQL havuzunuzu Gen2'ye yükseltin

Microsoft, SQL havuzu çalıştırmanın giriş düzeyi maliyetini azaltmaya yardımcı oluyor.  Zorlu sorguları işleyebilen daha düşük bilgi işlem katmanları artık SQL havuzu için kullanılabilir. [Gen2 için](https://azure.microsoft.com/blog/azure-sql-data-warehouse-gen2-now-supports-lower-compute-tiers/)alt bilgi işlem katmanı desteği tam duyuru okuyun. Yeni teklif aşağıdaki tabloda belirtilen bölgelerde mevcuttur. Desteklenen bölgeler için, varolan Gen1 SQL havuzları aşağıdakilerden biri aracılığıyla Gen2'ye yükseltilebilir:

- **Otomatik yükseltme işlemi:** Otomatik yükseltmeler, hizmet bir bölgede kullanılabilir olur olmaz başlamaz.  Belirli bir bölgede otomatik yükseltmeler başladığında, seçtiğiniz bakım zamanlaması sırasında tek tek veri ambarı yükseltmeleri gerçekleşir.
- [**Gen2'ye kendi kendine yükseltme:**](#self-upgrade-to-gen2) Gen2'ye kendi kendine yükseltme yaparak ne zaman yükseltme yapacağınızı kontrol edebilirsiniz. Bölgeniz henüz desteklenmiyorsa, geri yükleme noktasından desteklenen bir bölgedeki Gen2 örneğine doğrudan geri yükleyebilirsiniz.

## <a name="automated-schedule-and-region-availability-table"></a>Otomatik Zamanlama ve Bölge Kullanılabilirlik Tablosu

Aşağıdaki tablo, Alt Gen2 işlem katmanının ne zaman kullanıma sunulacağına ve otomatik yükseltmelerin ne zaman başlayacağına göre özetler. Tarihler değişebilir. Bölgenizin ne zaman kullanıma sunulduğuna görmek için tekrar kontrol edin.

\*bölge için belirli bir zamanlamanın şu anda kullanılamada olmadığını gösterir.

| **Bölge** | **Alt Gen2 kullanılabilir** | **Otomatik yükseltmeler başlar** |
|:--- |:--- |:--- |
| Doğu Kanada |1 Haziran 2020 |1 Temmuz 2020 |
| Doğu Çin |\* |\* |
| Kuzey Çin |\* |\* |
| Orta Almanya |\* |\* |
| Almanya Batı Orta |Kullanılabilir |Mayıs 1, 2020 |
| Hindistan Batı |Kullanılabilir |Mayıs 1, 2020  |

## <a name="automatic-upgrade-process"></a>Otomatik yükseltme işlemi

Yukarıdaki kullanılabilirlik grafiğine bağlı olarak, Gen1 örnekleriniz için otomatik yükseltmeler zamanlayacağız. SQL havuzunun kullanılabilirliği yle ilgili beklenmeyen kesintileri önlemek için, otomatik yükseltmeler bakım zamanlamanız sırasında zamanlanır. Yeni bir Gen1 örneği oluşturma yeteneği, Gen2'ye otomatik yükseltme yapılan bölgelerde devre dışı bırakılır. Gen1, otomatik yükseltmeler tamamlandıktan sonra amortismana dahil edilecektir. Zamanlamalar hakkında daha fazla bilgi için [bkz.](maintenance-scheduling.md#view-a-maintenance-schedule)

SQL havuzunuzu yeniden başlattığımızda yükseltme işlemi bağlantıda kısa bir düşüş (yaklaşık 5 dk) içerecektir.  SQL havuzunuz yeniden başlatıldıktan sonra, tamamen kullanılabilir olacaktır. Ancak, yükseltme işlemi arka plandaki veri dosyalarını yükseltmeye devam ederken performansta bir bozulma yaşayabilirsiniz. Performans düşüşünün toplam süresi veri dosyalarınızın boyutuna bağlı olarak değişiklik gösterir.

Ayrıca, yeniden başlatmadan sonra daha büyük bir SLO ve kaynak sınıfını kullanarak tüm birincil sütun deposu tablolarında [Alter Index yeniden oluşturmayı](sql-data-warehouse-tables-index.md) çalıştırarak veri dosyası yükseltme işlemini hızlandırabilirsiniz.

> [!NOTE]
> Alter Index yeniden oluşturma çevrimdışı bir işlemdir ve yeniden oluşturma tamamlanana kadar tablolar kullanılamaz.

## <a name="self-upgrade-to-gen2"></a>Gen2'ye kendi kendine yükseltme

Varolan bir Gen1 SQL havuzunda aşağıdaki adımları izleyerek kendi kendini yükseltmeyi seçebilirsiniz. Kendi kendine yükseltmeyi seçerseniz, bölgenizde otomatik yükseltme işlemi başlamadan önce bunu tamamlamanız gerekir. Bunu yapmak, bir çakışma neden otomatik yükseltmeleri herhangi bir risk önlemek sağlar.

Kendi kendine yükseltme yaparken iki seçenek vardır.  Geçerli SQL havuzunuzu yerinde yükseltebilir veya Gen1 SQL havuzunuzu Gen2 örneğine geri yükleyebilirsiniz.

- [Yerinde yükseltme](upgrade-to-latest-generation.md) - Bu seçenek, mevcut Gen1 SQL havuzunuzu Gen2'ye yükseltecektir. SQL havuzunuzu yeniden başlattığımızda yükseltme işlemi bağlantıda kısa bir düşüş (yaklaşık 5 dk) içerecektir.  SQL havuzunuz yeniden başlatıldıktan sonra, tamamen kullanılabilir olacaktır. Yükseltme sırasında sorunlarla karşılaşırsanız, bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) açın ve olası neden olarak "Gen2 yükseltmesi"ne başvurun.
- [Geri yükleme noktasından yükseltme](sql-data-warehouse-restore-points.md) - Geçerli Gen1 SQL havuzunuzda kullanıcı tanımlı bir geri yükleme noktası oluşturun ve ardından doğrudan bir Gen2 örneğine geri yükleyin. Mevcut Gen1 SQL havuzu yerinde kalacaktır. Geri yükleme tamamlandıktan sonra Gen2 SQL havuzunuz tamamen kullanılabilir olacaktır.  Geri yüklenen Gen2 örneğindeki tüm test ve doğrulama işlemlerini çalıştırdıktan sonra, orijinal Gen1 örneği silinebilir.

  - Adım 1: Azure portalından [kullanıcı tanımlı bir geri yükleme noktası oluşturun.](sql-data-warehouse-restore-active-paused-dw.md)
  - Adım 2: Kullanıcı tanımlı bir geri yükleme noktasından geri yükleme yaparken, "performans düzeyini" tercih ettiğiniz Gen2 katmanına ayarlayın.

Yükseltme işlemi arka planda veri dosyalarını yükseltmeye devam ederken bir süre için performansta düşüş yaşayabilirsiniz. Performans düşüşünün toplam süresi veri dosyalarınızın boyutuna bağlı olarak değişiklik gösterir.

Arka plan veri geçişi işlemini hızlandırmak için, daha büyük bir SLO ve kaynak sınıfında sorgulayacağınız tüm birincil sütun deposu tablolarında [Alter Index yeniden oluşturmayı](sql-data-warehouse-tables-index.md) çalıştırarak veri hareketini hemen zorlayabilirsiniz.

> [!NOTE]
> Alter Index yeniden oluşturma çevrimdışı bir işlemdir ve yeniden oluşturma tamamlanana kadar tablolar kullanılamaz.

SQL havuzunuzla ilgili herhangi bir sorunla karşılaşırsanız, olası neden olarak bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) oluşturun ve "Gen2 yükseltmesi" referansı oluşturun.

Daha fazla bilgi [için Gen2'ye Yükseltme'ye](upgrade-to-latest-generation.md)bakın.

## <a name="migration-frequently-asked-questions"></a>Sık sorulan sorular

**S: Gen2'nin maliyeti Gen1 ile aynı mıdır?**

- C: Evet.

**S: Yükseltmeler otomasyon komut dosyalarımı nasıl etkileyecek?**

- C: Hizmet Düzeyi Hedefine başvuran herhangi bir otomasyon komut dosyası Gen2 eşdeğerine karşılık gelecek şekilde değiştirilmelidir.  [Ayrıntılara buradan](upgrade-to-latest-generation.md#upgrade-in-a-supported-region-using-the-azure-portal)bakın.

**S: Kendi kendine yükseltme normalde ne kadar sürer?**

- C: Yerinde yükseltebilir veya geri yükleme noktasından yükseltebilirsiniz.

  - Yerinde yükseltme, SQL havuzunuzun anlık olarak duraklatlanmasına ve devam ına neden olur.  SQL havuzu çevrimiçiyken bir arka plan işlemi devam edecektir.  
  - Yükseltme tam geri yükleme işleminden geçecektir, çünkü bir geri yükleme noktası üzerinden yükseltme daha uzun sürer.

**S: Otomatik yükseltme ne kadar sürer?**

- C: Yükseltme için gerçek kapalı kalma süresi, yalnızca 5 ila 10 dakika arasında olan hizmeti duraklatmak ve devam ettirmek için gereken süredir. Kısa bir kapalı kalma süresinin ardından arka plan işlemi depolama geçişini çalıştırır. Arka plan işleminin süresi, SQL havuzunuzun boyutuna bağlıdır.

**S: Bu otomatik yükseltme ne zaman gerçekleşecek?**

- C: Bakım programınız sırasında. Seçtiğiniz bakım programından yararlanmak, işletmenizdeki aksamaları en aza indirir.

**S: Arka plan yükseltme işlemim sıkışmış gibi görünüyorsa ne yapmalıyım?**

- C: Sütun mağaza tablolarınızın yeniden dizini başlatın. Bu işlem sırasında tablonun yeniden dizini nin çevrimdışı olacağını unutmayın.

**S: Gen2'de Gen1'de Sahip Olduğum Hizmet Düzeyi Hedefi yoksa ne olur?**

- C: Gen1'de DW600 veya DW1200 çalıştırıyorsanız, Gen2 Gen1'den daha fazla bellek, kaynak ve daha yüksek performans sağladığından, sırasıyla DW500c veya DW1000c kullanmanız önerilir.

**S: Coğrafi yedeklemeyi devre dışı edebilir miyim?**

- C: Hayır. Coğrafi yedekleme, bir bölgenin kullanılamaması durumunda SQL havuzu kullanılabilirliğinizi korumak için bir kuruluş özelliğidir. Endişeleriniz varsa bir [destek isteği](sql-data-warehouse-get-started-create-support-ticket.md) açın.

**S: Gen1 ve Gen2 arasında T-SQL sözdiziminde bir fark var mıdır?**

- C: T-SQL dil sözdiziminde Gen1'den Gen2'ye geçiş yoktur.

**S: Gen2 Maintenance Windows'u destekliyor mu?**

- C: Evet.

**S: Bölgem yükseltildikten sonra yeni bir Gen1 örneği oluşturabilecek miyim?**

- C: Hayır. Bir bölge yükseltildikten sonra, yeni Gen1 örneklerinin oluşturulması devre dışı bırakılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Yükseltme adımları](upgrade-to-latest-generation.md)
- [Bakım pencereleri](maintenance-scheduling.md)
- [Kaynak durumu monitörü](../../service-health/resource-health-overview.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)
- [Geçişe başlamadan önce gözden geçir](upgrade-to-latest-generation.md#before-you-begin)
- [Yerinde yükseltme ve geri yükleme noktasından yükseltme](upgrade-to-latest-generation.md)
- [Kullanıcı tanımlı geri yükleme noktası oluşturma](sql-data-warehouse-restore-points.md)
- [Gen2'ye nasıl geri yükleyin öğrenin](sql-data-warehouse-restore-active-paused-dw.md)
- [SQL Veri Ambarı destek isteği açma](https://go.microsoft.com/fwlink/?linkid=857950)
