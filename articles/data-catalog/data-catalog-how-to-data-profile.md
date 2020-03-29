---
title: Azure Veri Kataloğu'nda veri profil oluşturma veri kaynakları nasıl kullanılır?
description: Azure Veri Kataloğu'nda veri kaynaklarını kaydederken tablo ve sütun düzeyinde veri profillerinin nasıl eklenebilir ve veri kaynaklarını anlamak için veri profillerinin nasıl kullanılacağını vurgulayan nasıl yapılır makalesi.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 04ac6c2bf0137289221a4ae6ed58d5a71ad21739
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68950230"
---
# <a name="how-to-data-profile-data-sources-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda veri profili kaynakları nasıl verilenebilir?

## <a name="introduction"></a>Giriş

**Microsoft Azure Veri Kataloğu,** kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren tam olarak yönetilen bir bulut hizmetidir. Başka bir deyişle, **Azure Veri Kataloğu** tamamen insanların veri kaynaklarını keşfetmesine, anlamasına ve kullanmasına ve kuruluşların mevcut verilerinden daha fazla değer elde etmesine yardımcı olmak için önemlidir. Bir veri kaynağı **Azure Veri Kataloğu'na**kaydedildiğinde, meta verileri hizmet tarafından kopyalanır ve dizine eklenir, ancak hikaye burada bitmez.

**Azure Veri Kataloğu'nun** **Veri Profil oluşturma** özelliği kataloğunuzdaki desteklenen veri kaynaklarından gelen verileri inceler ve bu verilerle ilgili istatistik ve bilgi toplar. Veri varlıklarınızın profilini eklemek kolaydır. Bir veri varlığını kaydettirdiğinizde, veri kaynağı kayıt aracına **Veri Profili Ekle'yi** seçin.

## <a name="what-is-data-profiling"></a>Veri Profilleme Nedir

Veri profiloluşturma, kayıtlı olan veri kaynağındaki verileri inceler ve bu veriler le ilgili istatistik ve bilgi toplar. Veri kaynağı bulma sırasında, bu istatistikler verilerin iş sorunlarını çözmek için uygunluğunu belirlemenize yardımcı olabilir.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Aşağıdaki veri kaynakları veri profiloluşturmayı destekler:

* SQL Server (Azure SQL DB ve Azure SQL Veri Ambarı dahil) tabloları ve görünümleri
* Oracle tabloları ve görünümleri
* Teradata tabloları ve görünümleri
* Hive tabloları

Veri varlıklarını kaydederken veri profilleri dahil olmak üzere, kullanıcıların veri kaynakları yla ilgili soruları yanıtlamalarına yardımcı olur:

* İş problemimi çözmek için kullanılabilir mi?
* Veriler belirli standartlara veya desenlere uygun mu?
* Veri kaynağının bazı anormallikleri nelerdir?
* Bu verileri uygulamama entegre etmenin olası zorlukları nelerdir?

> [!NOTE]
> Ayrıca, verilerin bir uygulamaya nasıl entegre edilebildiğini açıklamak için bir varlığa belge ekleyebilirsiniz. Veri [kaynaklarını nasıl belgeleyeceklerine](data-catalog-how-to-documentation.md)bakın.
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Bir veri kaynağını kaydederken veri profili nasıl ekilir?

Veri kaynağınızın profilini eklemek kolaydır. Bir veri kaynağı kaydettirdiğinizde, veri kaynağı kayıt aracının kayıtlı paneli **olacak Nesneler'de** **Veri Profili Ekle'yi**seçin.

![Veri Profili onay kutusunu ekle](media/data-catalog-data-profile/data-catalog-register-profile.png)

Veri kaynaklarını nasıl kaydedebilirsiniz hakkında daha fazla bilgi edinmek için [veri kaynaklarını nasıl kaydedin](data-catalog-how-to-register.md) ve [Azure Veri Kataloğu'na nasıl başlayın.](data-catalog-get-started.md)

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Veri profilleri içeren veri varlıklarına filtre uygulama

Veri profili içeren veri varlıklarını keşfetmek için `has:tableDataProfiles` `has:columnsDataProfiles` arama terimlerinizden birini ekleyebilir veya ekleyebilirsiniz.

> [!NOTE]
> Veri kaynağı kayıt aracında **Veri Profili Ekle'yi** seçmek hem tablo hem de sütun düzeyinde profil bilgilerini içerir. Ancak, Veri Kataloğu API veri varlıklarının yalnızca bir profil bilgisi kümesiyle kaydedilmesine olanak tanır.
>

## <a name="viewing-data-profile-information"></a>Veri profili bilgilerini görüntüleme

Profilli uygun bir veri kaynağı bulduğunuzda, veri profili ayrıntılarını görüntüleyebilirsiniz. Veri profilini görüntülemek için bir veri varlığı seçin ve Veri Kataloğu portalı penceresinde **Veri Profili'ni** seçin.

![Veri Profili sekmesi](media/data-catalog-data-profile/data-catalog-view.png)

Azure Veri **Kataloğu'ndaki** bir veri profili, tablo ve sütun profili bilgilerini şunları içeren şekilde gösterir:

### <a name="object-data-profile"></a>Nesne veri profili

* Satır sayısı
* Tablo boyutu
* Nesne en son güncelleştirildiğinde

### <a name="column-data-profile"></a>Sütun veri profili

* Sütun veri türü
* Farklı değerlerin sayısı
* NULL değerlerine sahip satır sayısı
* Sütun değerleri için minimum, maksimum, ortalama ve standart sapma

## <a name="summary"></a>Özet

Veri profiloluşturma, verilerin iş sorunlarını çözmek için uygunluğunu belirlemenize yardımcı olmak için kayıtlı veri varlıkları hakkında istatistikler ve bilgiler sağlar. Veri profilleri, açıklama açıklama ve veri kaynaklarını belgelemenin yanı sıra, kullanıcılara verilerinizin daha derin bir şekilde anlaşılmasını sağlayabilir.

## <a name="see-also"></a>Ayrıca Bkz.

* [Veri kaynaklarını kaydetme](data-catalog-how-to-register.md)
* [Azure Veri Kataloğu ile çalışmaya başlama](data-catalog-get-started.md)
