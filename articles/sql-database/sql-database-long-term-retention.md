---
title: Uzun süreli yedek saklama
description: Azure SQL Veritabanı'nın uzun süreli saklama ilkesi yle tam veritabanı yedeklemelerini 10 yıla kadar nasıl depolamayı desteklediğini öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 05/18/2019
ms.openlocfilehash: d015eea21bcfa499d6751e024a882a7316b7f1a5
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81380754"
---
# <a name="azure-sql-database-long-term-retention"></a>Azure SQL Veritabanı uzun süreli bekletme

Birçok uygulama, Azure SQL Veritabanı [otomatik yedeklemeleri](sql-database-automated-backups.md)tarafından sağlanan 7-35 gün boyunca veritabanı yedeklemelerini korumanızı gerektiren düzenleyici, uyumluluk veya diğer iş amaçlarına sahiptir. Uzun süreli bekletme (LTR) özelliğini kullanarak, belirtilen SQL veritabanı tam yedeklemelerini 10 yıla kadar okuma erişimi coğrafi yedekdepolamayla Azure Blob depolama alanında depolayabilirsiniz. Daha sonra yeni bir veritabanı olarak herhangi bir yedekleme geri yükleyebilirsiniz. Azure Depolama artıklığı hakkında daha fazla bilgi için Bkz. [Azure Depolama artıklığı.](../storage/common/storage-redundancy.md) 

Tek ve birleştirilmiş veritabanları için uzun süre bekletme etkinleştirilebilir ve Azure SQL Veritabanı yönetilen örnekleri için sınırlı genel önizlemededir. 

> [!NOTE]
> 35 gün sonra LTR'ye alternatif olarak [yalnızca kopyaveritabanı yedeklemelerini](https://docs.microsoft.com/sql/relational-databases/backup-restore/copy-only-backups-sql-server) zamanlamak için SQL Aracısı işlerini kullanabilirsiniz.


## <a name="how-sql-database-long-term-retention-works"></a>SQL Veritabanı uzun süreli saklama nasıl çalışır?

Uzun süreli yedekleme tutma (LTR), nokta zamanı geri yüklemesini (PITR) etkinleştirmek için [otomatik olarak oluşturulan](sql-database-automated-backups.md) tam veritabanı yedeklemelerinden yararlanır. Bir LTR ilkesi yapılandırılırsa, bu yedeklemeler uzun süreli depolama için farklı blob'lara kopyalanır. Kopya, veritabanı iş yükü üzerinde hiçbir performans etkisi olmayan bir arka plan işidir. Her SQL veritabanı için LTR ilkesi, LTR yedeklemelerinin ne sıklıkta oluşturulduğunu da belirtebilir.

LTR'yi etkinleştirmek için, dört parametrenin birleşimini kullanarak bir ilke tanımlayabilirsiniz: haftalık yedekleme tutma (W), aylık yedekleme tutma (M), yıllık yedekleme tutma (Y) ve yılın haftası (WeekOfYear). W belirtirseniz, her hafta bir yedek uzun süreli depolama alanına kopyalanır. M belirtirseniz, her ayın ilk yedeklemesi uzun süreli depolama alanına kopyalanır. Y belirtirseniz, WeekOfYear tarafından belirtilen hafta boyunca bir yedekleme uzun vadeli depolama kopyalanır. Belirtilen WeekOfYear, ilkenin yapılandırıldığı tarihteyse, ilk LTR yedeklemesi bir sonraki yıl oluşturulur. Her yedekleme, LTR yedeklemesi oluşturulduğunda yapılandırılan ilke parametrelerine göre uzun süreli depolamada tutulur.

> [!NOTE]
> LTR ilkesindeki herhangi bir değişiklik yalnızca gelecekteki yedeklemeler için geçerlidir. Örneğin, haftalık yedekleme bekletme (W), aylık yedekleme bekletme (M) veya yıllık yedekleme bekletme (Y) değiştirilirse, yeni bekletme ayarı yalnızca yeni yedeklemeler için geçerli olacaktır. Varolan yedeklemelerin bekletilmesi değiştirilmez. Niyetiniz eski LTR yedeklemelerini bekletme süreleri dolmadan silmekse, [yedekleri el ile silmeniz](https://docs.microsoft.com/azure/sql-database/sql-database-long-term-backup-retention-configure#delete-ltr-backups)gerekir.
> 

LTR ilkesine örnekler:

-  W=0, M=0, Y=5, Haftanın Tamamı=3

   Her yılın üçüncü tam yedekleme beş yıl boyunca tutulacaktır.
   
- W=0, M=3, Y=0

   Her ayın ilk tam yedekleme üç ay boyunca tutulacaktır.

- W=12, M=0, Y=0

   Her haftalık tam yedekleme 12 hafta boyunca tutulacaktır.

- W=6, M=12, Y=10, Haftanın Haftası=16

   Her haftalık tam yedekleme altı hafta boyunca tutulacaktır. 12 ay boyunca tutulacak her ayın ilk tam yedekleme hariç. Yılın 16. 

Aşağıdaki tablo, aşağıdaki ilke için uzun vadeli yedeklemelerin sona ermesini ve sona ermesini göstermektedir:

W=12 hafta (84 gün), M=12 ay (365 gün), Y=10 yıl (3650 gün), WeekOfYear=15 (15 Nisan'dan sonra hafta)

   ![ltr örnek](./media/sql-database-long-term-retention/ltr-example.png)



Yukarıdaki ilkeyi değiştirir ve W=0 (haftalık yedekleme yok) ayarlarsanız, yukarıdaki tabloda vurgulanan tarihlere göre yedek kopyaların cadence'ı değişir. Bu yedeklemeleri tutmak için gereken depolama miktarı buna göre azalır. 

> [!IMPORTANT]
> Tek tek LTR yedeklemelerinin zamanlaması Azure SQL Veritabanı tarafından denetlenir. El ile LTR yedeklemesi oluşturamaz veya yedekleme oluşturmanın zamanlamasını denetleyemezsiniz. Bir LTR ilkesini yapılandırdıktan sonra, ilk LTR yedeklemesinin kullanılabilir yedeklemelistesinde gösterilmesi 7 gün kadar sürebilir.  
> 

## <a name="geo-replication-and-long-term-backup-retention"></a>Coğrafi çoğaltma ve uzun vadeli yedekleme tutma

İş sürekliliği çözümünüzü olarak etkin coğrafi çoğaltma veya başarısız gruplar kullanıyorsanız, nihai başarısızlığa hazırlanmalı ve jeo-ikincil veritabanında aynı LTR ilkesini yapılandırmanız gerekir. Yedekler ikincilerden oluşturulmamadıkça LTR depolama maliyetiniz artmaz. Yalnızca ikincil birincil olduğunda yedekler oluşturulur. Başarısızlık tetiklendiğinde ve birincil ikincil bölgeye taşındığında, ltr yedeklemelerinin kesintisiz neslini sağlar. 

> [!NOTE]
> Özgün birincil veritabanı, başarısızolmaya neden olan bir kesintiden kurtarıldığında, yeni bir ikincil veritabanı na dönüşecektir. Bu nedenle, yedekleme oluşturma devam etmez ve varolan LTR ilkesi birincil yeniden olana kadar etkili olmayacaktır. 

## <a name="managed-instance-support"></a>Yönetilen Örnek desteği

Azure SQL Veritabanı yönetilen örnekleri ile uzun vadeli yedekleme bekletme kullanarak aşağıdaki sınırlamalar vardır:

- **Sınırlı genel önizleme** - Bu önizleme yalnızca EA ve CSP abonelikleri için kullanılabilir ve sınırlı kullanılabilirlik durumuna bağlıdır.  
- [**Yalnızca PowerShell**](sql-database-managed-instance-long-term-backup-retention-configure.md) - Şu anda Azure portal desteği yok. LTR PowerShell kullanılarak etkinleştirilmelidir. 

Kayıt istemek için, destek konusu **Yedekleme, Geri Yükleme ve İş sürekliliği / Uzun vadeli yedekleme tutma**altında bir Azure destek [bileti](https://azure.microsoft.com/support/create-ticket/) oluşturun.


## <a name="configure-long-term-backup-retention"></a>Uzun süreli yedek saklama yapılandırma

Azure portalını veya PowerShell'i kullanarak uzun süreli bekletmeyi nasıl yapılandırıştırılabilirsiniz, [bkz.](sql-database-long-term-backup-retention-configure.md)

## <a name="restore-database-from-ltr-backup"></a>LTR yedeklemesinden veritabanını geri yükleme

LTR depolama dan bir veritabanı geri yüklemek için, zaman damgası dayalı belirli bir yedekleme seçebilirsiniz. Veritabanı, özgün veritabanıyla aynı abonelik altında varolan herhangi bir sunucuya geri yüklenebilir. Azure portalını veya PowerShell'i kullanarak veritabanınızı LTR yedeklemesinden nasıl geri yükleyin, [bkz.](sql-database-long-term-backup-retention-configure.md)

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı yedeklemeleri verileri yanlışlıkla oluşan bozulmalara veya silmelere karşı koruduğundan, herhangi bir iş sürekliliği ve olağanüstü durum kurtarma stratejisinin önemli bir parçasıdır. Diğer SQL Veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [İş sürekliliğine genel bakış](sql-database-business-continuity.md)ala.s.
