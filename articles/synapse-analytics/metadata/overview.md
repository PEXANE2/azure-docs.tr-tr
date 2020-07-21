---
title: Paylaşılan meta veri modeli
description: Azure SYNAPSE Analytics, farklı çalışma alanı hesaplama altyapılarının, Spark havuzları (Önizleme), SQL isteğe bağlı altyapısı (Önizleme) ve SQL havuzları arasında veritabanları ve tablolar paylaşmasına izin verir.
services: synapse-analytics
author: MikeRys
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: metadata
ms.date: 05/01/2020
ms.author: mrys
ms.reviewer: jrasnick
ms.openlocfilehash: c41c3346d04dd1fc00a7ddb9e7fea66f619995e6
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86496069"
---
# <a name="azure-synapse-analytics-shared-metadata"></a>Azure SYNAPSE Analytics paylaşılan meta verileri

Azure SYNAPSE Analytics, farklı çalışma alanı hesaplama altyapılarının Spark havuzları (Önizleme) ve SQL isteğe bağlı motoru (Önizleme) arasında veritabanlarını ve tabloları paylaşmasına izin verir.

[!INCLUDE [preview](../includes/note-preview.md)]

Paylaşım, bu şekilde adlandırılan modern veri ambarı modelini destekler ve çalışma alanı SQL altyapılarını Spark ile oluşturulan veritabanlarına ve tablolara erişim sağlar. Ayrıca, SQL altyapılarının diğer altyapılarla paylaşılmayan kendi nesnelerini oluşturmalarına de olanak tanır.

## <a name="support-the-modern-data-warehouse"></a>Modern veri ambarını destekleme

Paylaşılan meta veri modeli, modern veri ambarı modelini aşağıdaki şekilde destekler:

1. Veri Gölü veriler, çok sayıda veritabanında bulunan (büyük olasılıkla bölümlenmiş) Parquet tarafından desteklenen tablolar depolanarak Spark ile birlikte hazırlanır ve verimli bir şekilde yapılandırılmıştır.

2. Spark tarafından oluşturulan veritabanları ve tüm tabloları Azure SYNAPSE çalışma alanı Spark havuz örneklerinde görünür hale gelir ve Spark işlerinin herhangi birinden kullanılabilir. Çalışma alanındaki tüm Spark havuzlarında aynı temel katalog meta deposunu paylaştığından, bu özellik [izinlere](#security-model-at-a-glance) tabidir.

3. Spark tarafından oluşturulan veritabanları ve kendi Parquet tarafından desteklenen tabloları, çalışma alanı SQL isteğe bağlı altyapısında görünür hale gelir. [Veritabanları](database.md) , SQL isteğe bağlı meta verilerde otomatik olarak oluşturulur ve bir Spark işi tarafından oluşturulan [dış ve yönetilen tablolar](table.md) , `dbo` karşılık gelen veritabanının şemasında SQL isteğe bağlı meta verilerde dış tablo olarak erişilebilir hale getirilir. 

<!--[INSERT PICTURE]-->

<!--__Figure 1 -__ Supporting the Modern Data Warehouse Pattern with shared metadata-->

Nesne eşitleme zaman uyumsuz olarak gerçekleştirilir. Nesneler, SQL bağlamında görünene kadar birkaç saniyelik bir gecikme olur. Bu olduklarında, bunlara erişimi olan SQL altyapılarını sorgulanabilir, ancak güncelleştirilemez veya değiştirilmez.

## <a name="which-metadata-objects-are-shared"></a>Paylaşılan meta veri nesneleri

Spark veritabanları, dış tablolar, yönetilen tablolar ve görünümler oluşturmanıza olanak sağlar. Spark görünümleri, tanımlayan Spark SQL ifadesini işlemek için bir Spark altyapısı gerektirdiğinden ve bir SQL altyapısı tarafından işlenemediği için, yalnızca veritabanları ve Parquet depolama biçimini kullanan dış ve yönetilen tabloları, çalışma alanı SQL altyapısıyla paylaşılır. Spark görünümleri yalnızca Spark havuz örnekleri arasında paylaşılır.

## <a name="security-model-at-a-glance"></a>Bir bakışta güvenlik modeli

Spark veritabanları ve tabloları, SQL altyapısındaki eşitlenmiş temsillerinin yanı sıra, temel alınan depolama düzeyinde güvenli hale getirilir. Tablo, sorgu gönderenin kullanım hakkına sahip olduğu altyapılardan herhangi biri tarafından sorgulandığında, sorgu gönderenin güvenlik sorumlusu, temel alınan dosyalara geçirilir. İzinler dosya sistemi düzeyinde denetlenir.

Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics paylaşılan veritabanı](database.md).

## <a name="change-maintenance"></a>Değişiklik Bakımı

Meta veri nesnesi silinir veya Spark ile değiştirilirse, değişiklikler alınır ve SQL isteğe bağlı altyapıya yayılır. Eşitleme zaman uyumsuzdur ve değişiklikler, kısa bir gecikmeden sonra SQL altyapısına yansıtılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SYNAPSE Analytics ' paylaşılan meta veri veritabanları hakkında daha fazla bilgi edinin](database.md)
- [Azure SYNAPSE Analytics ' paylaşılan meta veri tabloları hakkında daha fazla bilgi edinin](table.md)

