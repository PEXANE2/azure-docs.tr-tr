---
title: Azure Data Factory ile arasındaki farklar
description: Azure SYNAPSE Analytics 'in veri tümleştirme yetilerinin Azure Data Factory birbirinden farklı olduğunu öğrenin
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 11/06/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 10f5336dd4c8a02acf623b1b14226ca676006953
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2020
ms.locfileid: "94357659"
---
# <a name="data-integration-in-azure-synapse-analytics-versus-azure-data-factory"></a>Azure SYNAPSE Analytics 'te veri tümleştirmesi ve Azure Data Factory karşılaştırması

Azure SYNAPSE Analytics 'te, SYNAPSE işlem hatları ve veri akışları gibi veri tümleştirme özellikleri Azure Data Factory birine göre belirlenir. Daha fazla bilgi için bkz. [Azure Data Factory nedir?](../../data-factory/introduction.md). Neredeyse tüm yetenekler aynı veya benzerdir ve belgeler iki hizmet arasında paylaşılır. Bu makalede, Azure Data Factory ile Azure SYNAPSE arasındaki geçerli farklar vurgulanmıştır ve tanımlanmaktadır.

Azure Data Factory bir özelliğin veya makalenin Azure SYNAPSE için geçerli olup olmadığını görmek için makalenin en üstündeki bilinen adı denetleyin.

![Bilinen ad için geçerlidir](../media/concepts-data-factory-differences/applies-to-moniker.png "Bilinen ad için geçerlidir")

## <a name="features-in-azure-data-factory-not-planned-for-azure-synapse"></a>Azure SYNAPSE için planlı Azure Data Factory Özellikler

Aşağıdaki özellikler Azure Data Factory mevcuttur, ancak Azure SYNAPSE için planlanmazlar.

* **Yaşam ve KAYDıRMA SSIS paketleri:** Azure Data Factory, SSIS tümleştirme çalışma zamanını kullanarak SSIS paketlerini kaldırma ve kaydırma imkanına sahipsiniz. SSIS tümleştirme çalışma zamanı ve SSIS paketi yürütme etkinliği, SYNAPSE çalışma alanlarında kullanılamaz. 
* **Yaşam süresi:** Yaşam süresi, bir veri akışı tamamlandıktan sonra bir *süre boyunca veri* akışlarının eşlenmesine Izin veren Azure tümleştirme çalışma zamanındaki bir ayardır. Bu özellik SYNAPSE çalışma alanlarında kullanılamaz.

## <a name="azure-synapse-features-not-supported-in-azure-data-factory"></a>Azure Data Factory 'de desteklenmeyen Azure SYNAPSE özellikleri

Aşağıdaki özellikler Azure SYNAPSE 'te mevcuttur, ancak Azure Data Factory için planlanmazlar.

* **Eşleme veri akışlarının Spark İş izlemesi:** SYNAPSE ' de Spark motoru kullanıcının aboneliğinde bulunur, böylece kullanıcılar ayrıntılı Spark günlüklerini görüntüleyebilir. Azure Data Factory, iş yürütmesi Azure Data Factory tarafından yönetilen bir Spark kümesinde gerçekleşir ve bu bilgiler kullanılamaz. 

## <a name="azure-data-factory-features-that-behave-differently-in-synapse"></a>SYNAPSE 'de farklı davranan özellikleri Azure Data Factory

Aşağıdaki özellikler farklı şekilde davranır veya Azure SYNAPSE 'de Şu anda mevcut değildir. 

* **Wrangling veri akışları:** Denetimi veri akışı etkinliği şu anda yalnızca Azure Data Factory kullanılabilir.
* **Çözüm şablonu Galerisi:** Azure Data Factory, kullanıcılar, çözüm şablonu galerisinde işlem hattı şablonlarını bulabilir. SYNAPSE çalışma alanlarında, bilgi merkezi, ek veri kümeleri ve SQL betikleriyle birlikte farklı bir şablon kümesi içerir. 
* **Git tümleştirmesi ve yerel BIR CI/CD çözümü:** Şu anda bir Synapse çalışma alanı bir git deposuna bağlanamaz veya Azure Data Factory aynı sürekli tümleştirme ve teslim sürecini takip etmez.
* **Azure izleyici Ile tümleştirme:** SYNAPSE çalışma alanları Azure Data Factory olduğu gibi Azure Izleyici ile tümleştirilemez.
* **Karma tümleştirme çalışma zamanı yapılandırması:** Bir Synapse çalışma alanı içinde, bir kullanıcının hem yönetilen VNet IR hem de bir Azure IR olamaz. Bu özellik Azure Data Factory desteklenir.
* **Tümleştirme çalışma zamanı paylaşımı:** Şirket içinde barındırılan tümleştirme çalışma zamanları SYNAPSE çalışma alanları arasında paylaştırılamaz. Bu özellik Azure Data Factory desteklenir.
* **Veri akışları Için çapraz bölge tümleştirme çalışma zamanları:** Veri akışları, SYNAPSE çalışma alanından farklı bölgelerdeki tümleştirme çalışma zamanları üzerinde çalıştırılamaz. Bu özellik Azure Data Factory desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Data Lake Storage Gen2 hesabına veri](data-integration-data-lake.md)almayı öğrenerek SYNAPSE çalışma alanınızdaki veri tümleştirmesini kullanmaya başlayın.
