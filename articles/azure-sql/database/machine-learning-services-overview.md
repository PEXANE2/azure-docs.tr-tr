---
title: R ile Machine Learning Services (Önizleme)
description: Bu makalede, Azure SQL veritabanı Machine Learning Services (R ile) açıklanmakta ve nasıl çalıştığı açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 11/20/2019
ROBOTS: NOINDEX
ms.openlocfilehash: c45d52b89d23b2b030f6292a2257dfd732e8f766
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344773"
---
# <a name="azure-sql-database-machine-learning-services-with-r-preview"></a>R ile Azure SQL veritabanı Machine Learning Services (Önizleme)
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Machine Learning Services, veritabanı içi R betiklerini yürütmek için kullanılan Azure SQL veritabanı 'nın bir özelliğidir. Özelliği, yüksek performanslı tahmine dayalı analiz ve makine öğrenimi için Microsoft R paketleri içerir. İlişkisel veriler, saklı yordamlar aracılığıyla R betiklerde, R deyimlerini içeren T-SQL betiği veya T-SQL içeren R kodu ile kullanılabilir.

[!INCLUDE[ml-preview-note](../../../includes/sql-database-ml-preview-note.md)]

## <a name="what-you-can-do-with-r"></a>R ile yapabilecekleriniz

Veritabanı içi gelişmiş analiz ve makine öğrenimi sağlamak için R dilinin gücünü kullanın. Bu özellik, verilerin bulunduğu yere hesaplamalar ve işleme getirir ve ağ üzerinden veri çekme gereksinimini ortadan kaldırır. Ayrıca, uygun ölçekte gelişmiş analizler sunmak için kurumsal R paketlerinin gücünden yararlanabilirsiniz.

Machine Learning Services, Microsoft 'un kurumsal R paketleriyle çakışan, R 'nin temel bir dağılımını içerir. Microsoft 'un R işlevleri ve algoritmaları, tahmine dayalı analiz, istatistiksel modelleme, veri görselleştirmeleri ve önde gelen uç makine öğrenimi algoritmaları sunarak hem ölçek hem de yardımcı olmak için tasarlanmıştır.

### <a name="r-packages"></a>R paketleri

En yaygın açık kaynaklı R paketleri Machine Learning Services önceden yüklenir. Microsoft 'un aşağıdaki R paketleri de dahil edilmiştir:

| R paketi | Description|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open, Microsoft 'tan R 'nin gelişmiş bir dağıtımı olan Microsoft. İstatistiksel analiz ve veri bilimi için tam açık kaynaklı bir platformdur. Bu, R ile uyumlu olan ve %100 ' i temel alır ve gelişmiş performans ve reproducibility için ek yetenekler içerir. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | Geri alınamaz, ölçeklenebilir R için birincil kitaplıktır. bu kitaplıktaki Işlevler en yaygın olarak kullanılan bir seçenektir. Bu kitaplıklarda veri dönüştürmeleri ve düzenleme, istatistiksel özetleme, görselleştirme ve birçok modelleme ve analizler bulunur. Ayrıca, bu kitaplıkların işlevleri paralel işleme için kullanılabilir çekirdekler üzerinde iş yüklerini otomatik olarak dağıtır ve hesaplama altyapısı tarafından düzenlenen ve yönetilen veri öbeklerinde çalışma olanağı sağlar. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | MicrosoftML, metin analizi, görüntü analizi ve yaklaşım analizi için özel modeller oluşturmak üzere makine öğrenimi algoritmaları ekler. |

Önceden yüklenmiş paketlere ek olarak [ek paketler de yükleyebilirsiniz](machine-learning-services-add-r-packages.md).

<a name="signup"></a>

## <a name="sign-up-for-the-preview-closed"></a>Önizlemeye kaydolun (kapalı)

> [!IMPORTANT]
> Azure SQL veritabanı için kaydolun Machine Learning Services (Önizleme) Şu anda **kapalı**.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server Machine Learning Services arasındaki önemli farklılıkları](machine-learning-services-differences.md)inceleyin.
- R 'yi kullanarak Azure SQL veritabanı Machine Learning Services (Önizleme) sorgulama hakkında bilgi edinmek için [hızlı başlangıç kılavuzuna](connect-query-r.md)bakın.
- Bazı basit R betiklerini kullanmaya başlamak için bkz. [Azure SQL veritabanı 'nda basit r betikleri oluşturma ve çalıştırma Machine Learning Services (Önizleme)](r-script-create-quickstart.md).
 