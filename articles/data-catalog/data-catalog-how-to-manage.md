---
title: Azure Veri Kataloğu'nda veri varlıklarını yönetme
description: Makalede, Azure Veri Kataloğu'nda kayıtlı veri varlıklarının görünürlüğünü ve sahipliğini nasıl denetleneeceğimi vurgulamaktadır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 9905ed72ec54304bbdb0f7ee607cbb013fc645bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68736338"
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda veri varlıklarını yönetme
## <a name="introduction"></a>Giriş
Azure Veri Kataloğu, analiz yapmak ve karar vermek için ihtiyacınız olan veri kaynaklarını kolayca keşfedebilmeniz ve anlayabilirsiniz. Bu keşif yetenekleri, siz ve diğer kullanıcılar mevcut veri kaynaklarının en geniş aralığını bulup anlayınca en büyük etkiyi yaratır. Bu öğeler göz önünde bulundurularak, Veri Kataloğu'nun varsayılan davranışı, tüm kayıtlı veri kaynaklarının tüm katalog kullanıcıları tarafından görülebilmesi ve bulunabilmesiiçindir.

Veri Kataloğu, verilerin kendisine erişmenizi sağlamaz. Veri erişimi, veri kaynağının sahibi tarafından denetlenir. Veri Kataloğu ile veri kaynaklarını keşfedebilir ve katalogda kayıtlı kaynaklarla ilgili meta verileri görüntüleyebilirsiniz.

Ancak, veri kaynaklarının yalnızca belirli kullanıcılar veya belirli grupların üyeleri tarafından görülebileceği durumlar olabilir. Bu tür senaryolarda, kullanıcılar katalogdaki kayıtlı veri varlıklarının sahipliğini alabilir ve sahip oldukları varlıkların görünürlüğünü denetleyebilir.

> [!NOTE]
> Bu makalede açıklanan işlevsellik yalnızca Azure Veri Kataloğu'nun Standart Sürümü'nde kullanılabilir. Free Edition, sahiplik ve veri varlığı görünürlüğünü kısıtlama özellikleri sağlamaz.
>
>

## <a name="manage-ownership-of-data-assets"></a>Veri varlıklarının sahipliğini yönetme
Varsayılan olarak, Veri Kataloğu'nda kayıtlı veri varlıkları sahipsiz. Catalog'a erişim izni olan herhangi bir kullanıcı bu varlıkları keşfedebilir ve açıklama ekleyebilir. Kullanıcılar sahipsiz veri varlıklarının sahipliğini alabilir ve sahip oldukları varlıkların görünürlüğünü sınırlayabilir.

Veri Kataloğu'ndaki bir veri kıymetine sahip olunduğunda, yalnızca sahipler tarafından yetkilendirilen kullanıcılar varlığı keşfedebilir ve meta verilerini görüntüleyebilir ve yalnızca sahipler varlığı katalogdan silebilir.

> [!NOTE]
> Veri Kataloğu'nda sahiplik yalnızca katalogda depolanan meta verileri etkiler. Sahiplik, temel veri kaynağına herhangi bir izin vermez.
>
>

### <a name="take-ownership"></a>Sahipliği alın
Kullanıcılar, Veri Kataloğu portalındaki **Sahiplik Al** seçeneğini seçerek veri varlıklarının sahipliğini alabilir. Sahipsiz bir veri varlığının sahipliğini almak için özel izinler gerekmez. Herhangi bir kullanıcı sahipsiz bir veri varlığının sahipliğini alabilir.

### <a name="add-owners-and-co-owners"></a>Sahiplerini ve ortak sahiplerini ekleme
Bir veri kıymetine zaten sahip olunmuşsa, diğer kullanıcılar yalnızca sahiplenilemez. Bunlar, mevcut bir sahip tarafından ortak olarak eklenmelidir. Herhangi bir sahip ortak olarak ek kullanıcılar veya güvenlik grupları ekleyebilirsiniz.

> [!NOTE]
> Sahip olunan herhangi bir veri kıymetinin sahibi olarak en az iki kişinin olması en iyi yöntemdir.
>
>

### <a name="remove-owners"></a>Sahipleri kaldırma
Herhangi bir varlık sahibinin ortak sahiplerini ekleyebileceği gibi, herhangi bir varlık sahibi de herhangi bir ortak sahibini kaldırabilir.

Sahibi olarak kendilerini kaldıran bir varlık sahibi artık varlığı yönetemez. Varlık sahibi kendilerini sahip olarak kaldırırsa ve başka ortak sahip yoksa, varlık sahipsiz bir duruma geri döner.

## <a name="control-visibility"></a>Görünürlüğü kontrol edin
Veri varlık sahipleri sahip oldukları veri varlıklarının görünürlüğünü denetleyebilir. Tüm Veri Kataloğu kullanıcılarının veri varlığını keşfedip görüntülebildiği varsayılan olarak görünürlüğü kısıtlamak için, varlık sahibi varlık için özelliklerde Bu Kullanıcılar & **Herkes'ten** **Sahiplere** görünürlük ayarını ayarlayabilir. Sahipleri daha sonra belirli kullanıcılar ve güvenlik grupları ekleyebilirsiniz.

> [!NOTE]
> Mümkün olduğunda, varlık sahipliği ve görünürlük izinleri tek tek kullanıcılara değil, güvenlik gruplarına atanmalıdır.
>
>

## <a name="catalog-administrators"></a>Katalog yöneticileri
Veri Kataloğu yöneticileri, katalogdaki tüm varlıkların dolaylı olarak ortak sahipleridir. Varlık sahipleri yöneticilerin görünürlüğünü kaldıramaz ve yöneticiler katalogdaki tüm veri varlıklarının sahipliğini ve görünürlüğünü yönetebilir.

## <a name="summary"></a>Özet
Meta verilere ve veri varlık keşfine kadar veri kataloğu crowdsourcing modeli tüm katalog kullanıcılarının katkıda bulunmasını ve keşfetmesini sağlar. Veri Kataloğu'nun Standart Sürümü, belirli veri varlıklarının görünürlüğünü ve kullanımını sınırlamak için sahiplik ve yönetim için tasarlanmıştır.
