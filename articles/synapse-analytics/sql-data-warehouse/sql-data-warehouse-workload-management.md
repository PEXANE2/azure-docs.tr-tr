---
title: İş yükü yönetimi
description: Azure Synapse Analytics'te iş yükü yönetimini uygulama kılavuzu.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: dd867d4aa9a9ef5ed73e78a46826a8cd5239039b
ms.sourcegitcommit: bd5fee5c56f2cbe74aa8569a1a5bce12a3b3efa6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80744227"
---
# <a name="what-is-workload-management"></a>İş yükü yönetimi nedir?

Karışık iş yüklerini çalıştırmak, meşgul sistemlerde kaynak zorlukları oluşturabilir.  Çözüm Mimarlar, SLA'ları vurmak için yeterli kaynağın olduğundan emin olmak için klasik veri depolama etkinliklerini (verileri yükleme, dönüştürme ve sorgulama gibi) ayırmanın yollarını arar.  

Fiziksel sunucu yalıtımı, az kullanılan, fazla rezervasyonlu veya önbelleklerin sürekli olarak donanım başlatma ve durdurma ile astarlandığı bir durumda altyapı ceplerine yol açabilir.  Başarılı bir iş yükü yönetimi şeması kaynakları etkin bir şekilde yönetir, yüksek verimli kaynak kullanımını sağlar ve yatırım getirisini (YG) en üst düzeye çıkarır.

Veri ambarı iş yükü, bir veri ambarıyla ilişkili olarak geçen tüm işlemleri ifade eder. Bu bileşenlerin derinliği ve genişliği veri ambarının olgunluk düzeyine bağlıdır.  Veri ambarı iş yükü aşağıdakileri kapsar:

- Verileri ambara yükleme işleminin tamamı
- Veri ambarı analizi ve raporlaması
- Veri ambarındaki verileri yönetme
- Veri ambarından veri verme

Bir veri ambarının performans kapasitesi [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md)tarafından belirlenir.

- Tüm performans profilleri için ayrılan kaynakları görüntülemek için [Bellek ve eşzamanlılık sınırlarına](memory-concurrency-limits.md)bakın.
- Kapasiteyi ayarlamak için [yukarı veya aşağı ölçeklendirebilirsiniz.](quickstart-scale-compute-portal.md)

## <a name="workload-management-concepts"></a>İş yükü yönetimi kavramları

Geçmişte, Azure Synapse'deki SQL Analytics için sorgu performansını [kaynak sınıfları](resource-classes-for-workload-management.md)aracılığıyla yönetesiniz.  Rol üyeliğine dayalı bir sorguya bellek atanmasına izin verilen kaynak sınıfları.  Kaynak sınıfları ile birincil sorun, bir kez yapılandırıldıktan sonra, iş yükünü denetlemek için hiçbir yönetim veya yeteneği olmasıdır.  

Örneğin, smallrc'e geçici bir kullanıcı rolü üyeliği vermek, bu kullanıcının sistemdeki belleğin %100'ünü kullanmasına olanak sağladı.  Kaynak sınıflarında, kaynakların kritik iş yükleri için kullanılabilir olmasını sağlamanın ve ayırmanın bir yolu yoktur.

Azure Synapse'deki Synapse SQL havuzu iş yükü yönetimi üç üst düzey kavramdan oluşur: [İş Yükü Sınıflandırması,](sql-data-warehouse-workload-classification.md)İş [Yükü Önemi](sql-data-warehouse-workload-importance.md) ve İş [Yükü Yalıtımı.](sql-data-warehouse-workload-isolation.md)  Bu özellikler, iş yükünüzün sistem kaynaklarını nasıl kullandığı üzerinde daha fazla denetim sağlar.

İş yükü sınıflandırması, bir iş yükü grubuna istek atama ve önem düzeylerini ayarlama kavramıdır.  Tarihsel olarak, bu atama rol üyeliği yoluyla [sp_addrolemember](resource-classes-for-workload-management.md#change-a-users-resource-class)kullanılarak yapıldı.  Bu artık [CREATE İŞ YÜKÜ SıNıFLANDıRıCısı](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)aracılığıyla yapılabilir.  Sınıflandırma özelliği, etiket, oturum ve istekleri sınıflandırma zamanı gibi daha zengin bir seçenek kümesi sağlar.

İş yükü önemi, bir isteğin kaynaklara erişim sırasını etkiler.  Yoğun bir sistemde, daha yüksek öneme sahip bir istek öncelikle kaynaklara erişebilir.  Önem de kilitlere düzenli erişim sağlayabilir.

İş yükü yalıtımı, iş yükü grubu için kaynak ayırır.  İş yükü grubunda ayrılmış kaynaklar, yürütmeyi sağlamak için yalnızca bu iş yükü grubuna ait olarak tutulur.  İş yükü grupları, kaynak sınıfları gibi istek başına atanan kaynak miktarını da tanımlamanıza olanak tanır.  İş yükü grupları, bir dizi istek tenin tüketebileceği kaynak miktarını rezerve etme veya kapatma olanağı sağlar.  Son olarak, iş yükü grupları, sorgu zaman ası gibi kuralları isteklere uygulamak için bir mekanizmadır.  

## <a name="next-steps"></a>Sonraki adımlar

- İş yükü sınıflandırması hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-workload-classification.md)  
- İş yükü yalıtımı hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-workload-isolation.md)  
- İş yükünün önemi hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-workload-importance.md)  
- İş yükü yönetimi izleme hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-workload-management-portal-monitor.md)  
