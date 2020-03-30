---
title: Hızlandırılmış veritabanı kurtarma
description: Azure SQL Veritabanı, Azure SQL Veritabanı'nda tek veritabanları ve havuzlu veritabanları ve Azure SQL Verileri veritabanları için hızlı ve tutarlı veritabanı kurtarma, anlık işlem geri alma ve agresif günlük budaması sağlayan yeni bir özelliğe sahiptir Ambar.
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: mashamsft
ms.author: mathoma
ms.reviewer: carlrab
ms.date: 03/24/2020
ms.openlocfilehash: 57ca594dd067d15009de5e3abf7276fae48720d2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238673"
---
# <a name="accelerated-database-recovery"></a>Hızlandırılmış Veritabanı Kurtarma

**Hızlandırılmış Veritabanı Kurtarma (ADR),** özellikle uzun süren işlemlerin varlığında, SQL veritabanı altyapısı kurtarma işlemini yeniden tasarlayarak veritabanı kullanılabilirliğini büyük ölçüde iyileştiren bir SQL veritabanı altyapısı özelliğidir. ADR şu anda Azure SQL Veritabanı tek, elastik havuz ve yönetilen örnek ve Azure SQL Veri Ambarı'ndaki veritabanları için kullanılabilir (şu anda önizlemede). ADR'nin başlıca yararları şunlardır:

- **Hızlı ve tutarlı veritabanı kurtarma**

  ADR ile, uzun süren işlemler genel kurtarma süresini etkilemez ve sistemdeki etkin işlem sayısına veya boyutlarına bakılmaksızın hızlı ve tutarlı veritabanı kurtarmasını sağlar.

- **Anlık işlem geri alma**

  ADR ile, hareketin etkin olduğu zamana veya gerçekleştirilen güncelleştirme sayısına bakılmaksızın, işlem geri alma anında gerçekleşir.

- **Agresif günlük kesilme**

  ADR ile, işlem günlüğü, etkin uzun süren hareketler in durumunda bile agresif bir şekilde kesilir ve bu da işlemin kontrolden çıkmasına engel olur.

## <a name="the-current-database-recovery-process"></a>Geçerli veritabanı kurtarma işlemi

SQL Server'da veritabanı kurtarma, [KOÇ](https://people.eecs.berkeley.edu/~brewer/cs262/Aries.pdf) kurtarma modelini izler ve aşağıdaki diyagramda gösterilen ve diyagramı takiben daha ayrıntılı olarak açıklanan üç aşamadan oluşur.

![geçerli kurtarma süreci](./media/sql-database-accelerated-database-recovery/current-recovery-process.png)

- **Analiz aşaması**

  SQL Server'ın durdurulduğu anda her işlemin durumunu belirlemek için son başarılı denetim noktasının (veya en eski kirli sayfa LSN'nin) başından sonuna kadar işlem günlüğünün ileritme taramayı.

- **Yeniden yapma aşaması**

  İşlem günlüğünün en eski işlenmemiş işlemden sonuna kadar tarayın, veritabanını tüm taahhüt edilen işlemleri yeniden yaparak kilitlenme sırasındaki durumuna getirmek için.

- **Geri ala**

  Kilitlenme zamanı itibariyle etkin olan her hareket için, bu işlemin gerçekleştirdiği işlemleri geri aldanan günlük geriye doğru geçiş eder.

Bu tasarıma dayanarak, SQL veritabanı altyapısının beklenmeyen bir yeniden başlatmadan kurtarılması için gereken süre (kabaca) kilitlenme sırasında sistemdeki en uzun etkin işlemin boyutuyla orantılıdır. Kurtarma, tüm eksik hareketlerin geri alınmasını gerektirir. Gerekli süre, hareketin gerçekleştirdiği çalışma ve etkin olduğu süreile orantılıdır. Bu nedenle, SQL Server kurtarma işlemi uzun süren hareketler (büyük toplu ekleme işlemleri veya büyük bir tabloya karşı dizin oluşturma işlemleri gibi) varlığında uzun zaman alabilir.

Ayrıca, bu tasarıma dayalı büyük bir işlemi iptal etmek/geri almak da yukarıda açıklandığı gibi aynı Geri Alma kurtarma aşamasını kullandığından uzun zaman alabilir.

Buna ek olarak, SQL veritabanı altyapısı, uzun süren hareketler olduğunda işlem günlüğünü kaldıramaz, çünkü bunların karşılık gelen günlük kayıtları kurtarma ve geri alma işlemleri için gereklidir. SQL veritabanı altyapısının bu tasarımının bir sonucu olarak, bazı müşteriler işlem günlüğüboyutu çok büyük büyür ve sürücü alanı büyük miktarda tüketir sorunu yla karşı karşıya kullanılır.

## <a name="the-accelerated-database-recovery-process"></a>Hızlandırılmış Veritabanı Kurtarma işlemi

ADR, SQL veritabanı altyapısı kurtarma işlemini tamamen yeniden tasarlayarak yukarıdaki sorunları şu adrese giderir:

- En eski etkin işlemin başlangıcından/günlüğünden başlangıcına kadar taramaktan kaçınarak sabit zaman/anlık yapın. ADR ile işlem günlüğü yalnızca son başarılı denetim noktasından (veya en eski kirli sayfa Günlük Sıra Numarasından (LSN)) işlenir. Sonuç olarak, kurtarma süresi uzun süren işlemlerden etkilenmez.
- Tüm işlem için günlük işlemek için artık bir ihtiyaç olduğundan gerekli işlem günlüğü alanı en aza indirin. Sonuç olarak, denetim noktaları ve yedeklemeler gerçekleştiğinde işlem günlüğü agresif bir şekilde kesilir.

Yüksek Düzeyde, ADR tüm fiziksel veritabanı değişikliklerini sürümleyerek ve yalnızca sınırlı olan ve neredeyse anında geri alınabilen mantıksal işlemleri geri alarak hızlı veritabanı kurtarma sağlar. Kilitlenme zamanı itibariyle etkin olan tüm hareketler iptal edilmiş olarak işaretlenir ve bu nedenle, bu hareketler tarafından oluşturulan tüm sürümler eşzamanlı kullanıcı sorguları tarafından yoksayılabilir.

ADR kurtarma işlemi, geçerli kurtarma işlemiyle aynı üç aşamaya sahiptir. Bu aşamaların ADR ile nasıl çalıştığı aşağıdaki diyagramda gösterilmiştir ve diyagramdan sonra daha ayrıntılı olarak açıklanmıştır.

![ADR kurtarma süreci](./media/sql-database-accelerated-database-recovery/adr-recovery-process.png)

- **Analiz aşaması**

  İşlem, sLog'u yeniden yapılandırma ve sürümlenmemiş işlemler için günlük kayıtlarının kopyalanmasıyla öncekiyle aynı kalır.
  
- **Yeniden yapma** aşaması

  İki aşamaya ayrılmıştır (P)
  - Faz 1

      sLog'dan yeniden yapın (en eski işlenmemiş işlem son denetim noktasına kadar). Redo sadece sLog birkaç kayıtları işlemek için gereken hızlı bir işlemdir.
      
  - 2. Aşama

     İşlem Günlüğü'nden yeniden işlem son denetim noktasından başlar (en eski kaydedilmemiş işlem yerine)
     
- **Geri ala**

   ADR ile Geri Al aşaması, sürüm dışı işlemleri geri almak için sLog'u ve satır düzeyi sürüm tabanlı Geri Alma gerçekleştirmek için Mantıksal Geri Dönüş ile Kalıcı Sürüm Deposu'nu (PVS) kullanarak neredeyse anında tamamlanır.

## <a name="adr-recovery-components"></a>ADR kurtarma bileşenleri

ADR'nin dört temel bileşeni şunlardır:

- **Kalıcı Sürüm Deposu (PVS)**

  Kalıcı sürüm deposu, geleneksel `tempdb` sürüm deposu yerine veritabanının kendisinde oluşturulan satır sürümlerini kalıcı olarak sürdürmek için yeni bir SQL veritabanı altyapısı mekanizmasıdır. PVS, kaynak yalıtımı sağlar ve okunabilir ikincillerin kullanılabilirliğini artırır.

- **Mantıksal Geri Revert**

  Mantıksal geri dönüş, satır düzeyinde sürüm tabanlı Geri Alma'yı gerçekleştirmekten sorumlu olan ve tüm sürümlere yönelik işlemler için anında işlem geri alma ve geri alma sağlayan eşzamanlı işlemdir. Mantıksal geri çevirme tarafından gerçekleştirilir:

  - İptal edilen tüm hareketleri takip etmek ve diğer işlemlere görünmez işaretlemek. 
  - İşlem günlüğünü fiziksel olarak taramak ve değişiklikleri teker teker geri almak yerine tüm kullanıcı hareketleri için PVS kullanarak geri alma gerçekleştirme.
  - İşlem iptal edildikten hemen sonra tüm kilitleri serbest bırakma. İptal yalnızca bellekteki değişiklikleri işaretlemeyi içerdiğinden, işlem çok verimlidir ve bu nedenle kilitlerin uzun süre tutulması gerekmez.

- **sLog**

  sLog, sürüm dışı işlemler (meta veri önbelleği geçersiz liği, satın almaları kilitleme vb.) için günlük kayıtlarını depolayan ikincil bir bellek içi günlük akışıdır. sLog şu şekildedir:

  - Düşük ses düzeyi ve bellek içi
  - Denetim noktası işlemi sırasında seri hale getirilerek diskte kalıcı
  - Hareketler işlendikçe periyodik olarak kesilir
  - Yalnızca sürümlenmemiş işlemleri işleyerek yeniden yapmayı ve geri alma işlemini hızlandırır  
  - Yalnızca gerekli günlük kayıtlarını koruyarak agresif işlem günlüğü truncation sağlar

- **Temizleyici**

  Temizleyici, periyodik olarak uyanan ve gerekli olmayan sayfa sürümlerini temizleyen eşzamanlı işlemdir.

## <a name="accelerated-database-recovery-patterns"></a>Hızlandırılmış Veritabanı Kurtarma Desenleri

Aşağıdaki iş yükü türleri en çok ADR'den yararlanır:

- Uzun süren işlemlerle çalışan iş yükleri.
- Etkin hareketlerin hareket günlüğünün önemli ölçüde büyümesine neden olduğu durumlarda görülen iş yükleri.  
- SQL Server uzun süren kurtarma (beklenmeyen SQL Server yeniden başlatma veya el ile işlem geri alma gibi) nedeniyle veritabanı kullanılabilirliği uzun süre yaşamış iş yükleri.

