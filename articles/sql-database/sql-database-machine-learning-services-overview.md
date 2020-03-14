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
ms.openlocfilehash: ca223de2bc0b26e4968d400ea418761a399dacae
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79268944"
---
# <a name="azure-sql-database-machine-learning-services-with-r-preview"></a>R ile Azure SQL veritabanı Machine Learning Services (Önizleme)

Machine Learning Services, veritabanı içi R betiklerini yürütmek için kullanılan Azure SQL veritabanı 'nın bir özelliğidir. Özelliği, yüksek performanslı tahmine dayalı analiz ve makine öğrenimi için Microsoft R paketleri içerir. İlişkisel veriler, saklı yordamlar aracılığıyla R betiklerde, R deyimlerini içeren T-SQL betiği veya T-SQL içeren R kodu ile kullanılabilir.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

> [!NOTE]
> Önizleme, **genel amaçlı** ve **iş açısından kritik** hizmet katmanlarında sanal çekirdek tabanlı satın alma modeli kullanılarak tek veritabanları ve elastik havuzlar için kullanılabilir. Bu ilk önizlemede, **hiper ölçek** hizmet katmanı ve **yönetilen örnek** dağıtım seçeneği desteklenmez. Şu an için yalnızca R dili desteklenmektedir. Python desteği yoktur.
>
> Önizleme Şu anda şu bölgelerde kullanılabilir: Batı Avrupa, Kuzey Avrupa, Batı ABD 2, Doğu ABD, Orta Güney ABD, Orta Kuzey ABD, Kanada Orta, Güneydoğu Asya, Hindistan Güney ve Avustralya Güneydoğu.

## <a name="what-you-can-do-with-r"></a>R ile yapabilecekleriniz

Veritabanı içi gelişmiş analiz ve makine öğrenimi sağlamak için R dilinin gücünü kullanın. Bu özellik, verilerin bulunduğu yere hesaplamalar ve işleme getirir ve ağ üzerinden veri çekme gereksinimini ortadan kaldırır. Ayrıca, uygun ölçekte gelişmiş analizler sunmak için kurumsal R paketlerinin gücünden yararlanabilirsiniz.

Machine Learning Services, Microsoft 'un kurumsal R paketleriyle çakışan, R 'nin temel bir dağılımını içerir. Microsoft 'un R işlevleri ve algoritmaları, tahmine dayalı analiz, istatistiksel modelleme, veri görselleştirmeleri ve önde gelen uç makine öğrenimi algoritmaları sunarak hem ölçek hem de yardımcı olmak için tasarlanmıştır.

### <a name="r-packages"></a>R paketleri

En yaygın açık kaynaklı R paketleri Machine Learning Services önceden yüklenir. Microsoft 'un aşağıdaki R paketleri de dahil edilmiştir:

| R paketi | Açıklama|
|-|-|
| [Microsoft R açık](https://mran.microsoft.com/rro) | Microsoft R Open, Microsoft 'tan R 'nin gelişmiş bir dağıtımı olan Microsoft. İstatistiksel analiz ve veri bilimi için tam açık kaynaklı bir platformdur. Bu, R ile uyumlu olan ve %100 ' i temel alır ve gelişmiş performans ve reproducibility için ek yetenekler içerir. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | Geri alınamaz, ölçeklenebilir R için birincil kitaplıktır. bu kitaplıktaki Işlevler en yaygın olarak kullanılan bir seçenektir. Bu kitaplıklarda veri dönüştürmeleri ve düzenleme, istatistiksel özetleme, görselleştirme ve birçok modelleme ve analizler bulunur. Ayrıca, bu kitaplıkların işlevleri paralel işleme için kullanılabilir çekirdekler üzerinde iş yüklerini otomatik olarak dağıtır ve hesaplama altyapısı tarafından düzenlenen ve yönetilen veri öbeklerinde çalışma olanağı sağlar. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | MicrosoftML, metin analizi, görüntü analizi ve yaklaşım analizi için özel modeller oluşturmak üzere makine öğrenimi algoritmaları ekler. |

Önceden yüklenmiş paketlere ek olarak [ek paketler de yükleyebilirsiniz](sql-database-machine-learning-services-add-r-packages.md).

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Önizleme için kaydolun

> [!IMPORTANT]
> Azure SQL veritabanı için kaydolun Machine Learning Services (Önizleme) Şu anda kapalı.

Önizleme sırasında üretim iş yükü için R ile Machine Learning Services önerilmez.

## <a name="next-steps"></a>Sonraki adımlar

- [SQL Server Machine Learning Services arasındaki önemli farklılıkları](sql-database-machine-learning-services-differences.md)inceleyin.
- R 'yi kullanarak Azure SQL veritabanı Machine Learning Services (Önizleme) sorgulama hakkında bilgi edinmek için [hızlı başlangıç kılavuzuna](sql-database-connect-query-r.md)bakın.
- Bazı basit R betiklerini kullanmaya başlamak için bkz. [Azure SQL veritabanı 'nda basit r betikleri oluşturma ve çalıştırma Machine Learning Services (Önizleme)](sql-database-quickstart-r-create-script.md).
