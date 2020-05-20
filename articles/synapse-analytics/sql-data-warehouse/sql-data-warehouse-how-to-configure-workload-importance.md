---
title: İş yükü önem düzeyini yapılandırma
description: Azure SYNAPSE Analytics 'te istek düzeyi önemini ayarlamayı öğrenin.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.subservice: ''
ms.topic: conceptual
ms.date: 05/15/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 9440d5a13973a245320bc465e3997e3cdf414b3f
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83660316"
---
# <a name="configure-workload-importance-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te iş yükü önem derecesini yapılandırma

Azure için SYNAPSE SQL SYNAPSE ' deki önem derecesi, sorguların zamanlamasını etkiler. Daha yüksek önem taşıyan sorgular, daha düşük öneme sahip sorgulardan önce çalıştırılmak üzere zamanlanır. Sorgulara önem atamak için bir iş yükü Sınıflandırıcısı oluşturmanız gerekir.

## <a name="create-a-workload-classifier-with-importance"></a>Önem derecesine sahip bir Iş yükü Sınıflandırıcısı oluşturun

Genellikle bir veri ambarı senaryosunda, kullanıcıların, sorgularını hızlı bir şekilde çalıştırması gereken, meşgul bir sistemde kullanıcıları vardır.  Kullanıcı, raporları çalıştırması gereken şirketin Yöneticiler olabilir veya Kullanıcı geçici sorgu çalıştıran bir analist olabilir. Önem derecesi atamak için bir iş yükü Sınıflandırıcısı oluşturun ve bir sorguya önem verilir.  Aşağıdaki örneklerde iki sınıflandırıcı oluşturmak için [iş yükü sınıflandırıcı sözdizimi oluştur](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanılır. `Membername`tek bir kullanıcı veya grup olabilir.  Mevcut veri ambarı kullanıcılarını bulmak için şunu çalıştırın:

```sql
Select name from sys.sysusers
```

Daha yüksek önem taşıyan bir kullanıcı için iş yükü Sınıflandırıcısı oluşturmak için:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name' 
         ,IMPORTANCE     = above_normal);
```

Daha düşük önem taşıyan bir kullanıcı için geçici sorgular çalıştıran bir iş yükü Sınıflandırıcısı oluşturmak için:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name' 
         ,IMPORTANCE     = below_normal);
```

## <a name="next-steps"></a>Sonraki Adımlar

- İş yükü yönetimi hakkında daha fazla bilgi için bkz. [Iş yükü sınıflandırması](sql-data-warehouse-workload-classification.md)
- Önem hakkında daha fazla bilgi için bkz. [Iş yükü önemi](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Manage ve Monitor Iş yükünü önem derecesi ' ne gidin](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
