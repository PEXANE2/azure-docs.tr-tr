---
title: İş yükü önem düzeyi
description: Azure Synapse Analytics'te Synapse SQL havuz sorguları için önem belirleme kılavuzu.
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
ms.openlocfilehash: 2c8617cffaa81da6423011a494b8dbc82c42d218
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632450"
---
# <a name="azure-synapse-analytics-workload-importance"></a>Azure Synapse Analytics iş yükü önemi

Bu makalede, iş yükü önemi Azure Synapse'deki Synapse SQL havuz isteklerinin yürütme sırasını nasıl etkileyebileceği açıklanmaktadır.

## <a name="importance"></a>Önem

> [!Video https://www.youtube.com/embed/_2rLMljOjw8]

İş gereksinimleri, veri depolama iş yüklerinin diğerlerinden daha önemli olmasını gerektirebilir.  Görev kritik satış verilerinin mali dönem kapanmadan önce yüklendiği bir senaryo düşünün.  Hava durumu verileri gibi diğer kaynaklara ait veri yüklerinin katı SLA'ları yoktur. Satış verilerini yükleme isteğine büyük önem vermek ve hava durumu verilerini yüklemek için bir isteğe düşük önem vermek, satış veri yükünün kaynaklara ilk erişimi almasını ve daha hızlı tamamlanmasını sağlar.

## <a name="importance-levels"></a>Önem düzeyleri

Beş önem düzeyi vardır: düşük, below_normal, normal, above_normal ve yüksek.  Önem ayarlamayan isteklere varsayılan normal düzeyi atanır. Aynı önem düzeyine sahip istekler, bugün var olan zamanlama davranışıyla aynıdır.

## <a name="importance-scenarios"></a>Önem senaryoları

Satış ve hava durumu verileriyle yukarıda açıklanan temel önem senaryosunun ötesinde, iş yükünün öneminin veri işleme ve sorgulama gereksinimlerini karşılamaya yardımcı olduğu başka senaryolar da vardır.

### <a name="locking"></a>Kilitleme

Okuma ve yazma etkinliği için kilitlere erişim doğal çekişme bir alandır. [Bölüm değiştirme](/azure/sql-data-warehouse/sql-data-warehouse-tables-partition) veya [RENAME OBJECT](/sql/t-sql/statements/rename-transact-sql?view=azure-sqldw-latest) gibi etkinlikler yükseltilmiş kilitler gerektirir.  Azure Synapse'deki Synapse SQL havuzu, iş yükü açısından önem kaybetmeden, iş ortası için en iyi duruma getirilir. İş kaynağı için optimizasyon, çalışan ve sıraya giren istekler aynı kilitleme gereksinimlerine sahipken ve kaynaklar kullanılabilirken, sıralanmış isteklerin istek kuyruğuna daha önce gelen daha yüksek kilitleme gereksinimleriolan istekleri atlayabileceği anlamına gelir. Daha yüksek kilitleme gereksinimleri olan isteklere iş yükü önemi uygulandıktan sonra. Daha yüksek öneme sahip istek, daha düşük öneme sahip istekten önce yürütülecektir.

Aşağıdaki örneği inceleyin:

- Q1 etkin olarak çalışıyor ve SalesFact'ten veri seçiyor.
- Q2, Q1'in tamamlanmasını beklerken sıraya alınır.  Bu 09:00'da gönderildi ve SalesFact içine yeni veri geçiş bölüm çalışıyor.
- Q3 09:01'de gönderilir ve SalesFact'ten veri seçmek ister.

Q2 ve Q3 aynı öneme sahipse ve Q1 hala yürütülmeye devam ediyorsa, Q3 yürütmeye başlar. Q2 SalesFact üzerinde özel bir kilit için beklemeye devam edecektir.  Q2 Q3'ten daha yüksek öneme sahipse, Yürütmeye başlamadan önce Q2 bitene kadar Q3 bekleyecektir.

### <a name="non-uniform-requests"></a>Tektip olmayan istekler

Önemin sorgu taleplerini karşılamaya yardımcı olabileceği başka bir senaryo da, farklı kaynak sınıfları olan isteklerin gönderilmesidir.  Daha önce de belirtildiği gibi, aynı önem altında, Azure Synapse Synapse Synapse SQL havuzu iş için optimize eder. Karışık boyut istekleri (smallrc veya mediumrc gibi) sıraya girdiğinde, Synapse SQL havuzu kullanılabilir kaynaklara uyan en erken gelen isteği seçer. İş yükü önemi uygulanırsa, sonraki en yüksek önem isteği zamanlanır.
  
DW500c aşağıdaki örneği göz önünde bulundurun:

- Q1, Q2, Q3 ve Q4 smallrc sorguları çalıştırıyor.
- Q5 9:00'da mediumrc kaynak sınıfı ile gönderilir.
- Q6 09:01 de smallrc kaynak sınıfı ile gönderilir.

Q5 mediumrc olduğundan, iki eşzamanlılık yuvası gerektirir. Q5'in çalışan sorgulardan ikisinin tamamlanmasını beklemesi gerekir.  Ancak, çalışan sorgulardan biri (Q1-Q4) tamamlandığında, sorguyu yürütmek için kaynaklar bulunduğundan Q6 hemen zamanlanır.  Q5 Q6'dan daha yüksek öneme sahipse, Yürütmeye başlamadan önce Q5 çalışmaya başlayana kadar Q6 bekler.

## <a name="next-steps"></a>Sonraki adımlar

- Sınıflandırıcı oluşturma hakkında daha fazla bilgi için [CREATE WORKLOAD CLASSIFIER (Transact-SQL)](/sql/t-sql/statements/create-workload-classifier-transact-sql)'ye bakın.  
- İş yükü sınıflandırması hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-workload-classification.md)  
- İş yükü [sınıflandırıcısı](quickstart-create-a-workload-classifier-tsql.md) oluşturmak için Hızlı Başlangıç Oluşturma iş yükü sınıflandırıcısına bakın.
- [İş Yükü Önemini Yapılandırmak](sql-data-warehouse-how-to-configure-workload-importance.md) ve İş Yükü Yönetimini Nasıl [Yönetecek ve İzleyecek](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)makalelere bakın.
- Sorguları ve atanan önemi görüntülemek için [sys.dm_pdw_exec_requests'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) bakın.
