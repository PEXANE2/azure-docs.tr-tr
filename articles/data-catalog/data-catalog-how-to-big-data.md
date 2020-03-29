---
title: Azure Veri Kataloğu'nda büyük veri kataloğu nasıl
description: Azure Veri Kataloğu'nu Azure Blob Depolama, Azure Veri Gölü ve Hadoop HDFS gibi 'büyük veri' veri kaynaklarıyla kullanma desenleri vurgulayan nasıl bir makale.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 88dc85003fa2a3e41d8a31055ff8ba9b0fcc7492
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71300578"
---
# <a name="how-to-catalog-big-data-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda büyük veri kataloğu nasıl

## <a name="introduction"></a>Giriş

**Microsoft Azure Veri Kataloğu,** kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren tam olarak yönetilen bir bulut hizmetidir. Her şey, insanların veri kaynaklarını keşfetmelerine, anlamalarına ve kullanmalarına ve kuruluşların büyük veriler de dahil olmak üzere mevcut veri kaynaklarından daha fazla değer elde etmelerine yardımcı olmakla ilgilidir.

**Azure Veri Kataloğu,** Azure Blog Depolama blob'larının ve dizinlerinin yanı sıra Hadoop HDFS dosyalarının ve dizinlerinin kaydını destekler. Bu veri kaynaklarının yarı yapılandırılmış yapısı büyük esneklik sağlar. Ancak, bunları **Azure Veri Kataloğu'na**kaydederek en fazla değeri elde etmek için, kullanıcıların veri kaynaklarının nasıl düzenlendiğini göz önünde bulundurmaları gerekir.

## <a name="directories-as-logical-data-sets"></a>Mantıksal veri kümeleri olarak dizinler

Büyük veri kaynaklarını düzenlemek için ortak bir desen, dizinleri mantıksal veri kümeleri olarak ele almaktır. Alt klasörler bölümleri tanımlarken, üst düzey dizinler bir veri kümesini tanımlamak için kullanılır ve içerdikleri dosyalar verilerin kendisini depolar.

Bu desenin bir örneği şu olabilir:

```text
    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...
```

Bu örnekte, vehicle_maintenance_events ve location_tracking_events mantıksal veri kümelerini temsil ediyor. Bu klasörlerin her biri, yıl ve ay alt klasörler halinde düzenlenen veri dosyaları içerir. Bu klasörlerin her biri yüzlerce veya binlerce dosya içerebilir.

Bu desende, tek tek dosyaları **Azure Veri Kataloğu'na** kaydetmek büyük olasılıkla mantıklı değildir. Bunun yerine, verilerle çalışan kullanıcılar için anlamlı olan veri kümelerini temsil eden dizinleri kaydedin.

## <a name="reference-data-files"></a>Referans veri dosyaları

Tamamlayıcı bir desen, başvuru veri kümelerini tek tek dosyalar olarak depolamaktır. Bu veri kümeleri büyük verilerin 'küçük' tarafı olarak düşünülebilir ve genellikle analitik veri modelindeki boyutlara benzer. Başvuru veri dosyaları, büyük veri deposunda başka bir yerde depolanan veri dosyalarının büyük bir kısmı için bağlam sağlamak için kullanılan kayıtları içerir.

Bu desenin bir örneği şu olabilir:

```text
    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv
```

Bir analist veya veri bilimci daha büyük dizin yapılarında bulunan verilerle çalışırken, bu başvuru dosyalarındaki veriler, daha büyük veri kümesinde yalnızca ad veya kimlikle anılan varlıklar için daha ayrıntılı bilgi sağlamak için kullanılabilir.

Bu desende, tek tek başvuru veri dosyalarını **Azure Veri Kataloğu'na**kaydetmek mantıklıdır. Her dosya bir veri kümesini temsil eder ve her biri açıklamalı ve ayrı ayrı keşfedilebilir.

## <a name="alternate-patterns"></a>Alternatif desenler

Önceki bölümde açıklanan desenler, büyük bir veri deposunun düzenlenebileceği iki olası yöntemdir, ancak her uygulama farklıdır. Veri kaynaklarınız nasıl yapılandırıldığından bağımsız olarak, **azure veri kataloğuna**büyük veri kaynakları kaydederken, kuruluşunuzdaki diğer kişiler için değerli olan veri kümelerini temsil eden dosyaları ve dizinleri kaydetmeye odaklanın. Tüm dosyaları ve dizinleri kaydetmek, kullanıcıların ihtiyaç duydukları şeyi bulmalarını zorlaştırarak kataloğu darmadağın edebilir.

## <a name="summary"></a>Özet

Veri kaynaklarını **Azure Veri Kataloğu'na** kaydetmek, kaynakları keşfetmelerini ve anlamalarını kolaylaştırır. Mantıksal veri kümelerini temsil eden büyük veri dosyalarını ve dizinlerini kaydedip açıklama yaparak, kullanıcıların gereksinim duydukları büyük veri kaynaklarını bulmalarına ve kullanmalarına yardımcı olabilirsiniz.
