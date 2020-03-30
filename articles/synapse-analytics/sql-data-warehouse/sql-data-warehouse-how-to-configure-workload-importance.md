---
title: İş yükü önem düzeyini yapılandırma
description: Azure Synapse Analytics'te istek düzeyinin önemini nasıl ayarlayamaize cereyan edin.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: ''
ms.topic: conceptual
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 85fa6a989ae1a6214392914ffd46cd12af27a8d2
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350489"
---
# <a name="configure-workload-importance-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te iş yükü önemini yapılandırma

Azure Synapse için SQL Analytics'te önem vermek, sorguların zamanlamasını etkilemenize olanak tanır. Daha yüksek öneme sahip sorgular, daha düşük öneme sahip sorgulardan önce çalışacak şekilde zamanlanır. Sorgulara önem atamak için bir iş yükü sınıflandırıcısı oluşturmanız gerekir.

## <a name="create-a-workload-classifier-with-importance"></a>Önemtaşıyan İş Yükü Sınıflandırıcısı Oluşturma

Genellikle bir veri ambarı senaryosunda, sorgularının hızlı çalışması için gereksinim duyan kullanıcılar vardır.  Kullanıcı, raporları çalıştırması gereken şirketin yöneticileri olabilir veya kullanıcı geçici bir sorgu çalıştıran bir analist olabilir. Bir sorguya önem atamak için bir iş yükü sınıflandırıcısı oluşturursunuz.  Aşağıdaki örnekler, iki sınıflandırıcı oluşturmak için yeni [iş yükü sınıflandırıcı](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) sözdizimini kullanır.  Üye adı tek bir kullanıcı veya grup olabilir. Bireysel kullanıcı sınıflandırmaları rol sınıflandırmalarından daha önceliklidir. Varolan veri ambarı kullanıcılarını bulmak için çalıştırın:

```sql
Select name from sys.sysusers
```

Daha yüksek öneme sahip bir kullanıcı için iş yükü sınıflandırıcısı oluşturmak için:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  above_normal);  

```

Daha düşük öneme sahip geçici sorgular çalıştıran bir kullanıcı için iş yükü sınıflandırıcısı oluşturmak için:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  below_normal);  
```

## <a name="next-steps"></a>Sonraki Adımlar
- İş yükü yönetimi hakkında daha fazla bilgi için [bkz.](sql-data-warehouse-workload-classification.md)
- Önem hakkında daha fazla bilgi için İş [yükünün önemi](sql-data-warehouse-workload-importance.md) ne redeleyecek

> [!div class="nextstepaction"]
> [İş Yükü Önemini Yönetme ve İzleme](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
