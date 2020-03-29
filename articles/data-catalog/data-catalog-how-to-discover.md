---
title: Azure Veri Kataloğu'nda veri kaynakları nasıl keşfedilir?
description: Bu makalede, Azure Veri Kataloğu ile kayıtlı veri varlıklarının nasıl keşfedilebilen, arama ve filtreleme ve Azure Veri Kataloğu portalının isabet vurgulama özelliklerini kullanma gibi bilgiler vurgulanmaktadır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: b12cb94832a1ea977fb13f5f2271984dc8780cee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68736369"
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda veri kaynakları nasıl keşfedilir?

## <a name="introduction"></a>Giriş

Azure Veri Kataloğu, kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren tam olarak yönetilen bir bulut hizmetidir. Başka bir deyişle, Veri Kataloğu insanların veri kaynaklarını keşfetmeye, anlamamıza ve kullanmamıza yardımcı olur. Kuruluşların varolan verilerinden daha fazla değer elde etmelerine yardımcı olur. Bir veri kaynağı Veri Kataloğu'na kaydedildikten sonra, meta verileri hizmet tarafından dizine eklenir, böylece ihtiyacınız olan verileri bulmak için kolayca arama yapabilirsiniz.

## <a name="searching-and-filtering"></a>Arama ve filtreleme

Veri Kataloğunda Bulma iki birincil mekanizma kullanır: arama ve filtreleme.

Arama hem sezgisel hem de güçlü olacak şekilde tasarlanmıştır. Varsayılan olarak, arama terimleri kullanıcı tarafından ek açıklamalar dahil olmak üzere katalogdaki herhangi bir özellikle eşleştirilir.

Filtreleme, aramayı tamamlamak üzere tasarlanmıştır. Uzmanlar, veri kaynağı türü, nesne türü ve etiketler gibi belirli özellikleri seçebilirsiniz. Yalnızca eşleşen veri varlıklarını görüntüleyebilir ve arama sonuçlarını eşleşen varlıklarla sınırlandırabilirsiniz.

Arama ve filtreleme nin bir birleşimini kullanarak, ihtiyacınız olan veri kaynaklarını keşfetmek için Veri Kataloğu'na kayıtlı olan veri kaynaklarında hızla gezinebilirsiniz.

## <a name="search-syntax"></a>Sözdizimini ara

Varsayılan ücretsiz metin araması basit ve sezgisel olsa da, arama sonuçları üzerinde daha fazla denetim için Veri Kataloğu arama sözdizimini de kullanabilirsiniz. Veri Kataloğu aramaaşağıdaki teknikleri destekler:

| Teknik | Kullanım | Örnek |
| --- | --- | --- |
| Temel arama |Bir veya daha fazla arama terimleri kullanan temel arama. Sonuçlar, belirtilen terimlerden biriyle veya daha fazlasıyla herhangi bir özelliği eşleştirebilen varlıklardır. |`sales data` |
| Özellik kapsamları |Yalnızca arama teriminin belirtilen özellikile eşleştiği veri kaynaklarını döndürün. |`name:finance` |
| Boole işleçleri |Boolean işlemlerini kullanarak aramayı genişletin veya daraltın. |`finance NOT corporate` |
| Parantez ile gruplandırma |Özellikle Boolean işleçleri ile birlikte mantıksal yalıtım elde etmek için sorgunun bölümlerini gruplandırmak için parantez kullanın. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Karşılaştırma işleçleri |Sayısal ve tarih veri türleri olan özellikler için eşitlik dışındaki karşılaştırmalar kullanın. |`modifiedTime > "11/05/2014"` |

Veri Kataloğu araması hakkında daha fazla bilgi için [Azure Veri Kataloğu](/rest/api/datacatalog/#search-syntax-reference) makalesine bakın.

## <a name="hit-highlighting"></a>İsabet vurgulama

Arama sonuçlarını görüntülediğinizde, belirli bir veri varlığının belirli bir arama tarafından neden döndürüldedildiğini belirlemeyi kolaylaştırmak için belirtilen arama terimleriyle (veri varlık adı, açıklama ve etiketler gibi) eşleşen görüntülenen özellikler vurgulanır.

> [!NOTE]
> Isabet vurgulamayı kapatmak için Veri Kataloğu portalındaki **Vurgu** anahtarını kullanın.

Arama sonuçlarını görüntülediğinizde, isabet vurgulama etkin olsa bile bir veri varlığının neden dahil edildiği her zaman açık olmayabilir. Tüm özellikler varsayılan olarak arandığından, sütun düzeyindeki bir özellikteki eşleşme nedeniyle bir veri kıymeti döndürülebilir. Ayrıca, birden çok kullanıcı kayıtlı veri varlıklarına kendi etiketleri ve açıklamalarıyla açıklama eklediği için, tüm meta veriler arama sonuçları listesinde görüntülenmez.

Varsayılan döşeme görünümünde, arama sonuçlarında görüntülenen her döşeme, eşlenin sayısını ve konumlarını hızla görüntüleyebilmeniz ve isterseniz onlara atlamak için bir **Görünüm arama terimi eşler** simgesi içerir.

 ![Azure Veri Kataloğu portalında isabet vurgulama ve arama eşleşmeleri](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Özet

Veri Kataloğu'na bir veri kaynağı kaydetmek, veri kaynağından katalog hizmetine yapısal ve açıklayıcı meta verileri kopyaladığı için, veri kaynağının bulunması ve anlaşılması daha kolay hale gelir. Bir veri kaynağını kaydettikten sonra, Veri Kataloğu portalı içinden filtreleme ve arama kullanarak bu kaynağı keşfedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Veri kaynaklarını nasıl keşfedeceğimiz hakkında adım adım ayrıntılar için Azure [Veri Kataloğu ile Başlayın'a](data-catalog-get-started.md)bakın.
