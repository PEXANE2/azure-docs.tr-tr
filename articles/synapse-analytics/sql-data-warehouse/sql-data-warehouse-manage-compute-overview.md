---
title: SQL havuzu için bilgi işlem kaynağını yönetme
description: Azure Synapse Analytics SQL havuzundaki performans ölçeği yetenekleri hakkında bilgi edinin. DWUs'ları ayarlayarak ölçeklendirin veya veri ambarını duraklatarak maliyetleri düşürün.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 11/12/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: 4c6c4d97282387fbcee1d7e8b55b95c01e3dded5
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351607"
---
# <a name="manage-compute-in-azure-synapse-analytics-data-warehouse"></a>Azure Synapse Analytics veri ambarında bilgi işlem yönetme

Azure Synapse Analytics SQL havuzunda bilgi işlem kaynaklarını yönetme hakkında bilgi edinin. SQL havuzunu duraklatarak maliyetleri düşürün veya performans taleplerini karşılamak için veri ambarını ölçeklendirin. 

## <a name="what-is-compute-management"></a>İşlem yönetimi nedir?

Veri ambarının mimarisi depolama ve bilgi işlem ayrışarak her birinin bağımsız olarak ölçeklendirmesine olanak sağlar. Sonuçta, performans taleplerini karşılamak için işlemi veri depolamasından bağımsız olarak ölçeklendirebilirsiniz. Ayrıca işlem kaynaklarını duraklatabilir ve sürdürebilirsiniz. Bu mimarinin doğal bir sonucu, hesaplama ve depolama için [faturalandırmanın](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) ayrı olmasıdır. Veri ambarınızı bir süreliğine kullanmanız gerekmiyorsa, işlemi duraklatarak işlem maliyetlerinden tasarruf edebilirsiniz. 

## <a name="scaling-compute"></a>Ölçekleme hesaplaması

SQL havuzunuz için [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md) ayarını ayarlayarak hesaplamayı ölçeklendirebilir veya ölçeklendirebilirsiniz. Daha fazla veri ambarı birimi eklendikçe yükleme ve sorgu performansında doğrusal bir artış olur. 

Ölçeklendirme adımları için [Azure portalı](quickstart-scale-compute-portal.md), [PowerShell](quickstart-scale-compute-powershell.md)veya [T-SQL](quickstart-scale-compute-tsql.md) hızlı başlatmalar bakın. Ayrıca, [REST API](sql-data-warehouse-manage-compute-rest-api.md#scale-compute)ile ölçeklendirme işlemleri de gerçekleştirebilirsiniz.

Bir ölçek işlemi gerçekleştirmek için, SQL havuzu önce gelen tüm sorguları öldürür ve tutarlı bir durum sağlamak için hareketleri geri döndürer. Ölçeklendirme ancak işlemlerin geri alınması tamamlandıktan sonra gerçekleşir. Bir ölçek işlemi için sistem depolama katmanını işlem düğümlerinden ayırır, işlem düğümleri ekler ve depolama katmanını İşlem katmanına yeniden bağlar. Her SQL havuzu, işlem düğümlerine eşit olarak dağıtılan 60 dağıtım olarak depolanır. Daha fazla işlem düğümü eklemek daha fazla işlem gücü ekler. Bilgi işlem düğümlerinin sayısı arttıkça, işlem düğümü başına dağıtım sayısı azalır ve sorgularınız için daha fazla işlem gücü sağlar. Aynı şekilde, veri ambarı birimlerinin azaltılması, sorguların bilgi işlem kaynaklarını azaltan bilgi işlem düğümlerinin sayısını azaltır.

Aşağıdaki tablo, veri ambarı birimleri değiştikçe Bilgi İşlem düğümü başına dağılım sayısının nasıl değiştiğini gösterir.  DW30000c 60 İşlem düğümü sağlar ve DW100c'den çok daha yüksek sorgu performansı sağlar. 

| Veri ambarı birimleri  | \#işlem düğümleri | \#düğüm başına dağılımların |
| -------- | ---------------- | -------------------------- |
| DW100c   | 1                | 60                         |
| DW200c   | 1                | 60                         |
| DW300c   | 1                | 60                         |
| DW400c   | 1                | 60                         |
| DW500c'yi seçin   | 1                | 60                         |
| DW1000c  | 2                | 30                         |
| DW1500c  | 3                | 20                         |
| DW2000c  | 4                | 15                         |
| DW2500c  | 5                | 12                         |
| DW3000c  | 6                | 10                         |
| DW5000c  | 10               | 6                          |
| DW6000c  | 12               | 5                          |
| DW7500c  | 15               | 4                          |
| DW10000c | 20               | 3                          |
| DW15000c | 30               | 2                          |
| DW30000c | 60               | 1                          |


## <a name="finding-the-right-size-of-data-warehouse-units"></a>Veri ambarı birimlerinin doğru boyutunu bulma

Özellikle daha büyük veri ambarı birimleri için ölçeklemenin performans avantajlarını görmek için en az 1-TB veri kümesi kullanmak istiyorsunuz. SQL havuzunuz için en iyi veri ambarı birimi sayısını bulmak için yukarı ve aşağı ölçeklemayı deneyin. Verilerinizi yükledikten sonra farklı sayıda veri ambarı birimiiçeren birkaç sorgu çalıştırın. Ölçekleme hızlı olduğundan, bir saat veya daha kısa sürede çeşitli performans düzeylerini deneyebilirsiniz. 

En iyi veri ambarı birimlerini bulmak için öneriler:

- Geliştirme aşamasındaki bir SQL havuzu için, daha az sayıda veri ambarı birimi seçerek başlayın.  İyi bir başlangıç noktası DW400c veya DW200c olduğunu.
- Gözlemlediğiniz performansla karşılaştırıldığında seçilen veri ambarı birimlerinin sayısını gözlemleyerek uygulama performansınızı izleyin.
- Doğrusal bir ölçek varsayalım ve veri ambarı birimlerini artırmak veya azaltmak için ne kadar ihtiyacınız olduğunu belirleyin. 
- İş gereksinimleriniz için optimum performans düzeyine ulaşana kadar ayarlamalar yapmaya devam edin.

## <a name="when-to-scale-out"></a>Ne zaman ölçeklendirin

Veri ambarı birimlerinin ölçekletilmesi performansın bu yönlerini etkiler:

- Doğrusal taramaları, toplamalar için sistemin performansını artırır ve CTAS ifadeleri.
- Veri yüklemek için okuyucu ve yazar sayısını artırır.
- Maksimum eşzamanlı sorgu sayısı ve eşzamanlılık yuvası.

Veri ambarı birimlerini ne zaman ölçeklendirecek öneriler:

- Ağır bir veri yükleme veya dönüştürme işlemi gerçekleştirmeden önce, verileri daha hızlı kullanılabilir hale getirmek için ölçeklendirin.
- Yoğun çalışma saatlerinde, daha fazla eşzamanlı sorguyu barındıracak şekilde ölçeklendirin. 

## <a name="what-if-scaling-out-does-not-improve-performance"></a>Ölçekleme performansı artırmazsa ne olur?

Paralelliği artıran veri ambarı birimleri ekleme. Çalışma Bilgi İşlem düğümleri arasında eşit olarak bölünürse, ek paralellik sorgu performansını artırır. Ölçekleme performansınızı değiştirmiyorsa, bunun bazı nedenleri olabilir. Verileriniz dağıtımlar arasında çarpık olabilir veya sorgular büyük miktarda veri hareketi başlatıyor olabilir. Sorgu performansı sorunlarını araştırmak için [Bkz. Performans sorun giderme.](sql-data-warehouse-troubleshoot.md#performance) 

## <a name="pausing-and-resuming-compute"></a>İşlemi duraklatma ve sürdürme

İşlemi duraklatma, depolama katmanının İşlem düğümlerinden ayrılmasına neden olur. İşlem kaynakları hesabınızdan serbest bırakılır. İşlem duraklatılmışken işlem için ücretlendirilmezsiniz. İşleme devam etmek depolamayı İşlem düğümlerine yeniden bağlar ve İşlem için ücretleri devam ettirer. Bir SQL havuzuduraklattığınızda:

* Bilgi işlem ve bellek kaynakları veri merkezindeki kullanılabilir kaynaklar havuzuna döndürülür
* Veri ambarı birim maliyetleri duraklama süresi için sıfırdır.
* Veri depolama etkilenmez ve verileriniz bozulmadan kalır. 
* Çalışan veya sıraya verilen tüm işlemler iptal edilir.

Bir SQL havuzuna devam ettiğinizde:

* SQL havuzu, veri ambarı birimleri ayarınız için bilgi işlem ve bellek kaynakları edinir.
* Veri ambarı birimlerinizin işlem ücretleri devam ediyor.
* Verileriniz kullanılabilir hale gelir.
* SQL havuzu çevrimiçi olduktan sonra iş yükü sorgularınızı yeniden başlatmanız gerekir.

SQL havuzunuza her zaman erişilebilir olmasını istiyorsanız, duraklamak yerine en küçük boyuta kadar ölçeklemeyi düşünün. 

Duraklatma ve devam adımları için [Azure portalına](pause-and-resume-compute-portal.md)veya [PowerShell](pause-and-resume-compute-powershell.md) hızlı başlatmabaşlat'a bakın. Ayrıca [pause REST API](sql-data-warehouse-manage-compute-rest-api.md#pause-compute) veya [devam REST API](sql-data-warehouse-manage-compute-rest-api.md#resume-compute)kullanabilirsiniz.

## <a name="drain-transactions-before-pausing-or-scaling"></a>Duraklatma veya ölçeklendirme öncesinde işlemleri boşaltın

Duraklatma veya ölçeklendirme işlemi başlatmadan önce varolan hareketlerin tamamlanmasına izin vermenizi öneririz.

SQL havuzunuzu duraklattığınızda veya ölçeklendirdiğinizde, duraklatma veya ölçeklendirme isteğini başlattığınızda sorgularınız iptal edilir. Basit bir SELECT sorgusunu hızlıca ve örnek duraklatma veya ölçeklendirme süresini neredeyse hiç etkilemeden iptal edebilirsiniz.  Ancak, verilerinizi veya verilerinizin yapısını değiştiren işlem sorguları o kadar hızlı durdurulamayabilir. **Bir işlem sorgusunun tamamlanması veya yaptığı değişiklikleri geri alması gerekir.** Bir işlem sorgusunun tamamladığı işi geri almak, sorgunun değişiklik yapmak için harcadığı süre kadar, hatta bazen daha fazla zaman alabilir. Örneğin, bir saattir çalışan ve satır silen bir sorguyu iptal etmeniz halinde sistemin silinmiş olan satırları geri eklemesi bir saat sürebilir. Duraklatma veya ölçeklendirme isteklerini işlemler devam ederken çalıştırmanız halinde, devam etmek için geri alma işleminin tamamlanmasını bekleyeceğinden ilgili duraklatma veya ölçeklendirme işleminin tamamlanması uzun sürebilir.

Ayrıca bkz. [Hareketleri anlama](sql-data-warehouse-develop-transactions.md)ve hareketleri [optimize etme.](sql-data-warehouse-develop-best-practices-transactions.md)

## <a name="automating-compute-management"></a>Hesaplama yönetimini otomatikleştirmek

İşlem yönetimi işlemlerini otomatikleştirmek için Azure [işlevleriyle işlemi yönet'e](manage-compute-with-azure-functions.md)bakın.

Ölçeklendirme, duraklatma ve devam işlemlerinin tamamlanması birkaç dakika sürebilir. Otomatik olarak ölçekleme, duraklatma veya devam ediyorsanız, belirli işlemlerin başka bir eyleme geçmeden önce tamamlandığından emin olmak için mantık uygulamanızı öneririz. SQL havuzu durumunu çeşitli uç noktalardan denetlemek, bu tür işlemlerin otomasyonunu doğru şekilde uygulamanızı sağlar. 

SQL havuz durumunu denetlemek için [PowerShell](quickstart-scale-compute-powershell.md#check-data-warehouse-state) veya [T-SQL](quickstart-scale-compute-tsql.md#check-data-warehouse-state) quickstart'ına bakın. [Ayrıca, REST API](sql-data-warehouse-manage-compute-rest-api.md#check-database-state)ile SQL havuz durumunu da kontrol edebilirsiniz.


## <a name="permissions"></a>İzinler

SQL havuzunu ölçekleme [alter DATABASE](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)açıklanan izinleri gerektirir.  Duraklatma ve Devam et, özellikle Microsoft.Sql/servers/databases/action olmak üzere [SQL DB Katılımcısı](../../role-based-access-control/built-in-roles.md#sql-db-contributor) iznini gerektirir.


## <a name="next-steps"></a>Sonraki adımlar
[Bilgi işlem](manage-compute-with-azure-functions.md) için nasıl yol gösterin başka bir yönü tek tek sorgular için farklı bilgi işlem kaynakları ayırmaktır. Daha fazla bilgi [için iş yükü yönetimi için Kaynak sınıfları'na](resource-classes-for-workload-management.md)bakın.
