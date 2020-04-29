---
title: İş yükü önem düzeyini yapılandırma
description: Azure SYNAPSE Analytics 'te istek düzeyi önemini ayarlamayı öğrenin.
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
ms.openlocfilehash: 0ab7b8be8780f7edb2734d99587bc7709ced9436
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80633357"
---
# <a name="configure-workload-importance-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te iş yükü önem derecesini yapılandırma

Azure için SYNAPSE SQL SYNAPSE ' deki önem derecesi, sorguların zamanlamasını etkiler. Daha yüksek önem taşıyan sorgular, daha düşük öneme sahip sorgulardan önce çalıştırılmak üzere zamanlanır. Sorgulara önem atamak için bir iş yükü Sınıflandırıcısı oluşturmanız gerekir.

## <a name="create-a-workload-classifier-with-importance"></a>Önem derecesine sahip bir Iş yükü Sınıflandırıcısı oluşturun

Genellikle bir veri ambarı senaryosunda, sorgularının hızla çalışmasını gerektiren kullanıcılarınız vardır.  Kullanıcı, raporları çalıştırması gereken şirketin Yöneticiler olabilir veya Kullanıcı geçici sorgu çalıştıran bir analist olabilir. Bir sorguya önem düzeyi atamak için bir iş yükü Sınıflandırıcısı oluşturursunuz.  Aşağıdaki örneklerde, iki sınıflandırıcı oluşturmak için yeni [iş yükü sınıflandırıcı](/sql/t-sql/statements/create-workload-classifier-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) sözdizimi kullanılır. `Membername`tek bir kullanıcı veya grup olabilir. Bireysel kullanıcı sınıflandırmaları, rol sınıflandırmalarına göre önceliklidir. Mevcut veri ambarı kullanıcılarını bulmak için şunu çalıştırın:

```sql
Select name from sys.sysusers
```

Daha yüksek önem taşıyan bir kullanıcı için iş yükü Sınıflandırıcısı oluşturmak için:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  above_normal);  

```

Daha düşük önem taşıyan bir kullanıcı için geçici sorgular çalıştıran bir iş yükü Sınıflandırıcısı oluşturmak için:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
         ,MEMBERNAME     = 'name'  
         ,IMPORTANCE     =  below_normal);  
```

## <a name="next-steps"></a>Sonraki Adımlar

- İş yükü yönetimi hakkında daha fazla bilgi için bkz. [Iş yükü sınıflandırması](sql-data-warehouse-workload-classification.md)
- Önem hakkında daha fazla bilgi için bkz. [Iş yükü önemi](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Manage ve Monitor Iş yükünü önem derecesi ' ne gidin](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
