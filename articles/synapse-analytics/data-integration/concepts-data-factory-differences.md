---
title: Azure Data Factory farklılıklar
description: Azure SYNAPSE Analytics 'in veri tümleştirme yetilerinin Azure Data Factory birbirinden farklı olduğunu öğrenin
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 09/23/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: be098977ba51e529aaecfb5dc3b7a03444f55a7e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91345170"
---
# <a name="data-integration-in-azure-synapse-analytics-versus-azure-data-factory"></a>Azure SYNAPSE Analytics 'te veri tümleştirmesi ve Azure Data Factory karşılaştırması

Azure SYNAPSE Analytics 'te, SYNAPSE işlem hatları ve veri akışları gibi veri tümleştirme özellikleri Azure Data Factory birine göre belirlenir. Daha fazla bilgi için bkz. [Azure Data Factory nedir?](../../data-factory/introduction.md). Neredeyse tüm yetenekler aynı veya benzerdir ve belgeler iki hizmet arasında paylaşılır. Bu makalede, Azure Data Factory ile Azure SYNAPSE arasındaki geçerli farklar vurgulanmıştır ve tanımlanmaktadır.

Azure Data Factory bir özelliğin veya makalenin Azure SYNAPSE için geçerli olup olmadığını görmek için makalenin en üstündeki bilinen adı denetleyin.

![Bilinen ad için geçerlidir](../media/concepts-data-factory-differences/applies-to-moniker.png "Bilinen ad için geçerlidir")

## <a name="features-in-azure-data-factory-not-planned-for-azure-synapse"></a>Azure SYNAPSE için planlı Azure Data Factory Özellikler

Aşağıdaki özellikler Azure Data Factory mevcuttur, ancak Azure SYNAPSE için planlanmazlar.

* SSIS paketlerini kaldırma ve kaydırma özelliği.
* Kopyalama etkinliği ve eşleme veri akışında havuz olarak parça olarak Sun.
* Eşleme verileri, Azure tümleştirme çalışma zamanının yaşam süresi ayarını canlı olarak ayarlar.

## <a name="azure-synapse-features-not-supported-in-azure-data-factory"></a>Azure Data Factory 'de desteklenmeyen Azure SYNAPSE özellikleri

Aşağıdaki özellikler Azure SYNAPSE 'te mevcuttur, ancak Azure Data Factory için planlanmazlar.

* Eşleme veri akışlarının Spark İş izlemesi yalnızca SYNAPSE içinde kullanılabilir. SYNAPSE ' de Spark motoru kullanıcının aboneliğinde bulunur, böylece kullanıcılar ayrıntılı Spark günlüklerini görüntüleyebilir. Azure Data Factory, iş yürütmesi Azure Data Factory yönetilen bir Spark kümesinde gerçekleşir. 

## <a name="azure-data-factory-features-that-behave-differently-in-synapse"></a>SYNAPSE 'de farklı davranan özellikleri Azure Data Factory

Aşağıdaki özellikler farklı şekilde davranır veya Azure SYNAPSE 'de Şu anda mevcut değildir. 

* Wrangling veri akışları
* Çözüm şablonu Galerisi
* Git tümleştirmesi ve yerel bir CI/CD çözümü
* Azure izleyici ile tümleştirme
* Yayımladıktan sonra kaynakları yeniden adlandırma
* Bir Synapse çalışma alanı içinde karma tümleştirme çalışma zamanı yapılandırması. Bir kullanıcının hem yönetilen VNet IR hem de bir Azure IR olamaz.
* SYNAPSE çalışma alanları arasında tümleştirme çalışma zamanı paylaşımı

## <a name="next-steps"></a>Sonraki adımlar

[Azure Data Lake Storage Gen2 hesabına veri](data-integration-data-lake.md)almayı öğrenerek SYNAPSE çalışma alanınızdaki veri tümleştirmesini kullanmaya başlayın.
