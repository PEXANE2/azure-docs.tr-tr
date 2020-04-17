---
title: R ile Makine Öğrenimi Hizmetleri (önizleme)
description: Bu makalede, Azure SQL Veritabanı Makine Öğrenme Hizmetleri (R ile) açıklanır ve nasıl çalıştığını açıklar.
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
ms.openlocfilehash: 46ca4661d06b52c861431a680a69297575ac99b0
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81461421"
---
# <a name="azure-sql-database-machine-learning-services-with-r-preview"></a>R ile Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme)

Machine Learning Services, veritabanı içi R komut dosyalarını yürütmek için kullanılan Azure SQL Veritabanı'nın bir özelliğidir. Bu özellik, yüksek performanslı tahmine dayalı analitik ve makine öğrenimi için Microsoft R paketlerini içerir. İlişkisel veriler, depolanan yordamlar, R deyimleri içeren T-SQL komut dosyası veya T-SQL içeren R kodu aracılığıyla R komut dosyasında kullanılabilir.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="what-you-can-do-with-r"></a>R ile yapabilecekler

Gelişmiş analitik ve makine öğrenimi veritabanı nda sunmak için R dilinin gücünü kullanın. Bu yetenek, hesaplamaları ve işlemeyi verilerin bulunduğu yere getirerek ağ üzerinden veri çekme gereksinimini ortadan kaldırır. Ayrıca, ölçekte gelişmiş analitik sunmak için kurumsal R paketlerinin gücünden yararlanabilirsiniz.

Machine Learning Services, Microsoft'un kurumsal R paketleriyle kapatılan R'nin temel dağıtımını içerir. Microsoft'un R işlevleri ve algoritmaları, tahmine dayalı analitik, istatistiksel modelleme, veri görselleştirmeleri ve öndegelen makine öğrenme algoritmaları sunarak hem ölçek hem de yardımcı program için tasarlanır.

### <a name="r-packages"></a>R paketleri

En yaygın açık kaynak R paketleri Machine Learning Services'a önceden yüklenir. Microsoft'un aşağıdaki R paketleri de dahildir:

| R paketi | Açıklama|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open, Microsoft'un Geliştirilmiş R dağıtımıdır. Bu istatistiksel analiz ve veri bilimi için tam bir açık kaynak platformudur. R ile %100 uyumlu dur ve geliştirilmiş performans ve tekrarlanabilirlik için ek özellikler içerir. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | RevoScaleR bu kütüphanede ölçeklenebilir R. Fonksiyonları için birincil kütüphane en yaygın olarak kullanılan arasındadır. Veri dönüşümleri ve manipülasyon, istatistiksel özetleme, görselleştirme ve modelleme ve analizler birçok formları bu kütüphanelerde bulunur. Ayrıca, bu kitaplıktaki işlevler, hesaplama altyapısı tarafından koordine edilen ve yönetilen veri yığınları üzerinde çalışabilme olanağıyla, iş yüklerini paralel işleme için kullanılabilir çekirdekler arasında otomatik olarak dağıtır. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | MicrosoftML, metin analizi, görüntü analizi ve duyarlılık analizi için özel modeller oluşturmak için makine öğrenimi algoritmaları ekler. |

Önceden yüklenmiş paketlere ek [olarak, ek paketler yükleyebilirsiniz.](sql-database-machine-learning-services-add-r-packages.md)

<a name="signup"></a>

## <a name="sign-up-for-the-preview-closed"></a>Önizleme için kaydolun (kapalı)

> [!IMPORTANT]
> Azure SQL Veritabanı Makine Öğrenme Hizmetleri (önizleme) için kaydolun şu anda **kapalı.**

## <a name="next-steps"></a>Sonraki adımlar

- SQL [Server Machine Learning Services'daki temel farklara](sql-database-machine-learning-services-differences.md)bakın.
- Azure SQL Veritabanı Makine Öğrenme Hizmetleri 'ni (önizleme) sorgulamak için R'yi nasıl kullanacağınızı öğrenmek için [Quickstart kılavuzuna](sql-database-connect-query-r.md)bakın.
- Bazı basit R komut dosyalarıyla başlamak için Azure [SQL Veritabanı Makine Öğrenme Hizmetleri'nde (önizleme) basit R komut dosyası oluşturma ve çalıştırma](sql-database-quickstart-r-create-script.md)'ya bakın.
