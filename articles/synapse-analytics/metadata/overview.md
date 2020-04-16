---
title: Azure Synapse Analytics paylaşılan meta veri modeli
description: Azure Synapse Analytics, farklı çalışma alanı hesaplama lı motorlarının Veritabanlarını ve tabloları Spark havuzları (önizleme), SQL isteğe bağlı altyapısı (önizleme) ve SQL havuzları arasında paylaşmasına olanak tanır.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: 3b26d516080961a482a3ba67f314e98ece4c9f24
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424121"
---
# <a name="azure-synapse-analytics-shared-metadata"></a>Azure Synapse Analytics paylaşılan meta verileri

Azure Synapse Analytics, farklı çalışma alanı hesaplama lı motorlarının Veritabanlarını ve tabloları Spark havuzları (önizleme), SQL isteğe bağlı altyapısı (önizleme) ve SQL havuzları arasında paylaşmasına olanak tanır.

[!INCLUDE [preview](../includes/note-preview.md)]



Paylaşım sözde modern veri ambarı deseni destekler ve çalışma alanı SQL motorları veritabanları ve Tablolar Spark ile oluşturulan erişim sağlar. Ayrıca, SQL motorlarının diğer motorlarla paylaşılmayan kendi nesnelerini oluşturmasına da olanak tanır.

## <a name="support-the-modern-data-warehouse"></a>Modern veri ambarını destekleme

Paylaşılan meta veri modeli, modern veri ambarı deseni aşağıdaki şekilde destekler:

1. Veri gölünden elde edilen veriler, hazırlanan verileri muhtemelen çeşitli veritabanlarında bulunan (muhtemelen bölümlenmiş) Parke destekli tablolarda saklayarak Spark ile verimli bir şekilde hazırlanır ve yapılandırılır.

2. Spark veritabanları oluşturdu ve tüm tabloları Azure Synapse çalışma alanı Spark havuzu örneklerinden herhangi birinde görünür hale gelir ve Spark işlerinden herhangi birinden kullanılabilir. Çalışma alanındaki tüm Spark havuzları aynı temel katalog meta deposunu paylaştığından, bu özellik [izinlere](#security-model-at-a-glance) tabidir.

3. Spark veritabanları oluşturdu ve parke destekli tabloları çalışma alanı SQL on-demand altyapısında görünür hale gelir. [Veritabanları](database.md) SQL isteğe bağlı meta verilerde otomatik olarak oluşturulur ve bir Spark işi tarafından oluşturulan [hem harici hem de yönetilen tablolar,](table.md) ilgili veritabanının `dbo` şemasındaki SQL isteğe bağlı meta verilerdeki dış tablolar olarak erişilebilir hale getirilir. <!--For more details, see [ADD LINK].-->

4. Çalışma alanında meta veri eşitlemesi etkinleştirilmiş SQL havuzu örnekleri varsa <!--[ADD LINK]--> veya meta veri eşitlemesi etkinleştirilmiş yeni bir SQL havuzu örneği oluşturulursa, Spark oluşturulan veritabanları ve parke destekli tabloları [Azure Synapse Analytics paylaşılan veritabanında](database.md)açıklandığı gibi SQL havuz veritabanına otomatik olarak eşlenir.

<!--[INSERT PICTURE]-->

<!--__Figure 1 -__ Supporting the Modern Data Warehouse Pattern with shared metadata-->

Nesne eşitlemesi eşsenkronize oluşur. Nesneler, SQL bağlamında görünene kadar birkaç saniye lik hafif bir gecikmeye sahip olurlar. Göründüklerinde sorgulanabilirler, ancak bunlara erişimi olan SQL motorları tarafından güncelleştirilemez veya değiştirilemez.

## <a name="which-metadata-objects-are-shared"></a>Hangi meta veri nesneleri paylaşılır

Spark veritabanları, dış tablolar, yönetilen tablolar ve görünümler oluşturmanıza olanak tanır. Spark görünümleri tanımlayıcı Spark SQL deyimini işlemek için bir Kıvılcım altyapısı gerektirdiğinden ve bir SQL altyapısı tarafından işlenemediğinden, yalnızca veritabanları ve Parke depolama biçimini kullanan bunların içerdiği harici ve yönetilen tablolar çalışma alanı SQL motorları ile paylaşılır. Kıvılcım görünümleri yalnızca Spark havuzu örnekleri arasında paylaşılır.

## <a name="security-model-at-a-glance"></a>Bir bakışta güvenlik modeli

Spark veritabanları ve tabloları, SQL motorlarındaki senkronize gösterimleriyle birlikte, temel depolama düzeyinde güvenlidir. Tablo, sorgu gönderenin kullanma hakkına sahip olduğu altyapılardan herhangi biri tarafından sorgulandığında, sorgu gönderenin güvenlik ilkesi temel dosyalara geçirilir. İzinler dosya sistemi düzeyinde denetlenir.

Daha fazla bilgi için Azure [Synapse Analytics paylaşılan veritabanına](database.md)bakın.

## <a name="change-maintenance"></a>Bakımı değiştir

Bir meta veri nesnesi Spark ile silinir veya değiştirilirse, değişiklikler alınır ve SQL isteğe bağlı motora ve nesneleri eşitleyen SQL havuzlarına yayılır. Senkronizasyon asynchronous ve değişiklikler kısa bir gecikmeden sonra SQL motorlarında yansıtılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Synapse Analytics'in paylaşılan meta veri veritabanları hakkında daha fazla bilgi edinin](database.md)
- [Azure Synapse Analytics'in paylaşılan meta veri Tabloları hakkında daha fazla bilgi edinin](table.md)

