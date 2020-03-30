---
title: Azure Veri Kataloğu'nda veri kaynaklarına açıklama nasıl ek açıklama
description: Azure Veri Kataloğu'nda, güler yüzlü adlar, etiketler, açıklamalar ve uzmanlar da dahil olmak üzere veri varlıklarına nasıl açıklama ek önem verilebildiğini vurgulayan nasıl bir makale.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: c1e022591ce1aee073330055744fbd78d97c0b1d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68950255"
---
# <a name="how-to-annotate-data-sources-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda veri kaynaklarına açıklama nasıl ek açıklama

## <a name="introduction"></a>Giriş

**Microsoft Azure Veri Kataloğu,** kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren tam olarak yönetilen bir bulut hizmetidir. Başka bir deyişle, Veri Kataloğu tamamen insanların veri kaynaklarını keşfetmesine, anlamasına ve kullanmasına ve kuruluşların mevcut verilerinden daha fazla değer elde etmesine yardımcı olmak la ilgilidir. Bir veri kaynağı Veri Kataloğu'na kaydedildiğinde, meta verileri hizmet tarafından kopyalanır ve dizine eklenir, ancak hikaye burada bitmez. Veri Kataloğu, kullanıcıların veri kaynağından çıkarılan meta verileri tamamlamak ve veri kaynağını daha fazla kişi için daha anlaşılır hale getirmek için kendi açıklayıcı meta verilerini (açıklamalar ve etiketler gibi) sağlamalarına olanak tanır.

## <a name="annotation-and-crowdsourcing"></a>Ek açıklama ve crowdsourcing
Herkesin bir fikri vardır. Ve bu iyi bir şey.
Veri Kataloğu, farklı kullanıcıların kurumsal veri kaynaklarına farklı bakış açıları olduğunu ve bu bakış açılarının her birinin değerli olabileceğini kabul eder. Şu senaryoyu göz önünde bulundurun:

* Sistem yöneticisi, veri kaynağını barındıran sunucular veya hizmetler için hizmet düzeyi sözleşmesini bilir.
* Veritabanı yöneticisi, her veritabanı için yedekleme zamanlamasını ve izin verilen ETL işleme pencerelerini bilir.
* Sistem sahibi, kullanıcıların veri kaynağına erişim isteme işlemini bilir.
* Veri sorumlusu, veri kaynağı ndaki varlıkların ve özniteliklerin kurumsal veri modeline nasıl eşlenebildiğini bilir.
* Analist, verilerin destekledikleri iş süreçleri bağlamında nasıl kullanıldığını bilir.

Bu bakış açılarının her biri değerlidir ve Veri Kataloğu, her birinin yakalanmasına ve kayıtlı veri kaynaklarının tam bir resmini sağlamak için kullanılmasına olanak tanıyan meta verilere kitle kaynak kullanımı yaklaşımı kullanır. Veri Kataloğu portalını kullanarak, her kullanıcı diğer kullanıcılar tarafından sağlanan ek açıklamaları görüntüleyebilirken kendi ek açıklamalarını ekleyebilir ve edinebilir.

## <a name="different-types-of-annotations"></a>Farklı ek açıklama türleri
Veri Kataloğu aşağıdaki ek açıklama türlerini destekler:

| Ek Açıklama | Notlar |
| --- | --- |
| Kolay ad |Veri varlıklarının daha kolay anlaşılması nı sağlamak için veri varlıkları düzeyinde dost adlar sağlanabilir. Temel nesne adı şifreli olduğunda, kısaltıldığında veya kullanıcılar için anlamlı olmadığında, kısa laç tasnif adlar en kullanışlıdır. |
| Açıklama |Açıklamalar veri varlığı ve öznitelik / sütun düzeylerinde sağlanabilir. Açıklamalar, kullanıcının veri varlığına veya kullanımına ilişkin perspektifini açıklayan serbest biçimli kısa metin ek açıklamalarıdır. |
| Etiketler (kullanıcı etiketleri) |Etiketler veri varlığı ve öznitelik / sütun düzeylerinde sağlanabilir. Kullanıcı etiketleri, veri varlıklarını veya özniteliklerini kategorilere ayırmak için kullanılabilecek kullanıcı tanımlı etiketlerdir. |
| Etiketler (sözlük etiketleri) |Etiketler veri varlığı ve öznitelik / sütun düzeylerinde sağlanabilir. Sözlük etiketleri, ortak bir işletme taksonomisini kullanarak veri varlıklarını veya öznitelikleri kategorilere ayırmak için kullanılabilecek merkezi olarak tanımlanmış sözlük terimleridir. Daha fazla bilgi [için Yönetilmiş Etiketleme Için İş Sözlüğü'nün nasıl ayarlanış](data-catalog-how-to-business-glossary.md) |
| Uzman |Uzmanlar veri varlık düzeyinde sağlanabilir. Uzmanlar, veriler üzerinde uzman bakış açılarına sahip kullanıcıları veya grupları tanımlar ve kayıtlı veri kaynaklarını keşfeden ve mevcut ek açıklamalar tarafından yanıtlanmamış soruları olan kullanıcılar için iletişim noktası olarak hizmet verebilir. |
| Erişim izni isteme |İstek erişim bilgileri veri varlık düzeyinde sağlanabilir. Bu bilgiler, henüz erişim izinleri olmayan bir veri kaynağı keşfeden kullanıcılar içindir. Kullanıcılar, erişim sağlayan kullanıcının veya grubun e-posta adresini, kullanıcıların erişebilmek için ihtiyaç duyduğu işlemin URL'sini veya aracını girebilir veya işlemin kendisini metin olarak girebilir. |
| Belgeler |Belgeler veri varlık düzeyinde sağlanabilir. Varlık belgeleri, bağlantılar ve resimler içerebilen ve açıklamalar ve etiketler aracılığıyla aktarılmayan bilgileri sağlayabilen zengin metin bilgileridir. |

## <a name="annotating-multiple-assets"></a>Birden çok kıymete açıklama
Veri Kataloğu portalında birden çok veri varlığı seçerken, kullanıcılar seçili tüm varlıkları tek bir işlemde ekleyebilir. Ek açıklamalar, seçili tüm varlıklar için geçerli olacak ve bu da ilgili veri varlıkları için tutarlı bir açıklama ve etiket ve uzman kümeleri seçmeyi ve sağlamayı kolaylaştırır.

> [!NOTE]
> Veri Kataloğu veri kaynağı kayıt aracını kullanarak veri varlıklarını kaydederken etiketler ve uzmanlar da sağlanabilir.
>
>

Birden çok tablo ve görünüm seçilirken, Veri Kataloğu portalında yalnızca seçili tüm veri varlıklarının ortak olduğu sütunlar görüntülenir. Bu, kullanıcıların seçili tüm varlıklar için aynı ada sahip tüm sütunlar için etiketler ve açıklamalar sağlamasına olanak tanır.

## <a name="annotations-and-discovery"></a>Ek açıklamalar ve keşif
Kayıt sırasında veri kaynağından çıkarılan meta veriler Veri Kataloğu arama dizinine eklendikçe, kullanıcı tarafından sağlanan meta veriler de dizine eklenir. Bu, ek açıklamaların kullanıcıların buldukları verileri anlamalarını kolaylaştırmakla birlikte, ek açıklamaların kullanıcıların kendilerine anlamlı terimleri kullanarak arama yaparak açıklamalı veri varlıklarını keşfetmelerini de kolaylaştırdığı anlamına gelir.

## <a name="summary"></a>Özet
Veri Kataloğu'na bir veri kaynağı kaydolmak, veri kaynağından katalog hizmetine yapısal ve açıklayıcı meta verileri kopyalayarak bu verilerin bulunabilir hale geldiğini sağlar. Bir veri kaynağı kaydedildikten sonra, kullanıcılar Veri Kataloğu portalı nın içinden daha kolay keşfedilmeyi ve anlamalarını kolaylaştırmak için ek açıklamalar sağlayabilir.

## <a name="see-also"></a>Ayrıca bkz.
* Veri kaynaklarına nasıl açıklama ekinek olunacak hakkında adım adım ayrıntılar için Azure Veri Kataloğu öğreticisine [başlayın.](data-catalog-get-started.md)
