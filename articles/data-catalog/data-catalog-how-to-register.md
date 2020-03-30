---
title: Azure Veri Kataloğu'nda veri kaynaklarını kaydetme
description: Bu makalede, kayıt sırasında çıkarılan meta veri alanları da dahil olmak üzere Azure Veri Kataloğu'nda veri kaynaklarının nasıl kaydedilen olduğu vurgulanmaktadır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 0c5fdac7df41fec3a6206dbd78af74b7f1b58c7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68736314"
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda veri kaynaklarını kaydetme
## <a name="introduction"></a>Giriş
Azure Veri Kataloğu, kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren tam olarak yönetilen bir bulut hizmetidir. Başka bir deyişle, Veri Kataloğu kişilerin veri kaynaklarını keşfetmelerine, anlamalarına ve kullanmalarına yardımcı olur ve kuruluşların varolan verilerinden daha fazla değer elde etmelerine yardımcı olur. Veri Kataloğu aracılığıyla bir veri kaynağını bulunabilir hale getirmenin ilk adımı, bu veri kaynağını kaydetmektir.

## <a name="register-data-sources"></a>Veri kaynaklarını kaydetme
Kayıt, meta verileri veri kaynağından ayıklama ve bu verileri Veri Kataloğu hizmetine kopyalama işlemidir. Veriler o anda bulunduğu yerde kalır ve geçerli sistemin yöneticilerinin ve ilkelerinin denetiminde olmaya devam eder.

Bir veri kaynağını kaydetmek için aşağıdakileri yapın:
1. Azure Veri Kataloğu portalında Veri Kataloğu veri kaynağı kayıt aracını başlatın. 
2. Portalda oturum açtırmak için kullandığınız Azure Etkin Dizin kimlik bilgileriyle iş veya okul hesabınızla oturum açın.
3. Kaydetmek istediğiniz veri kaynağını seçin.

Daha fazla adım adım ayrıntı için [Azure Veri Kataloğu](data-catalog-get-started.md) ile Başlat'a bakın.

Veri kaynağını kaydettikten sonra, katalog konumunu izler ve meta verilerini dizine aktarır. Kullanıcılar, veri kaynağını arayabilir, göz atabilir ve keşfedebilir ve ardından kendi seçtikleri uygulamayı veya aracı kullanarak bu kaynağa bağlanmak için konumunu kullanabilir.

## <a name="supported-data-sources"></a>Desteklenen veri kaynakları
Şu anda desteklenen veri kaynaklarının listesi için [Bkz. Veri Kataloğu DSR.](data-catalog-dsr.md)

## <a name="structural-metadata"></a>Yapısal meta veriler
Bir veri kaynağı kaydettirdiğinizde, kayıt aracı seçtiğiniz nesnelerin yapısı hakkında bilgi ayıklar. Bu bilgiler yapısal meta veri olarak adlandırılır.

Tüm nesneler için bu yapısal meta veriler nesnenin konumunu içerir, böylece verileri keşfeden kullanıcılar bu bilgileri seçtikleri istemci araçlarındaki nesneye bağlanmak için kullanabilirler. Diğer yapısal meta veriler nesne adı ve türü ile öznitelik/sütun adı ve veri türünü içerir.

## <a name="descriptive-metadata"></a>Açıklayıcı meta veriler
Veri kaynağından çıkarılan temel yapısal meta verilere ek olarak, veri kaynağı kayıt aracı açıklayıcı meta verileri ayıklar. SQL Server Analysis Services ve SQL Server Reporting Services için bu meta veriler, bu hizmetlerin maruz kaçtığı Açıklama özelliklerinden alınır. SQL Server için ms\_açıklaması genişletilmiş özelliği kullanılarak sağlanan değerler ayıklanır. Oracle Database için, veri kaynağı kayıt aracı TÜM\_SEKME\_YORUMLAR görünümünden COMMENTS sütunundan ayıklar.

Verilerden çıkarılan açıklayıcı meta verilere ek olarak, kullanıcılar veri kaynağı kayıt aracını kullanarak açıklayıcı meta veriler girebilir. Kullanıcılar etiket ekleyebilir ve kayıtlı nesneler için uzmanları tanımlayabilir. Tüm bu açıklayıcı meta veriler yapısal meta verilerle birlikte Veri Kataloğu hizmetine kopyalanır.

## <a name="include-previews"></a>Önizlemeekleme
Varsayılan olarak, yalnızca meta veriler veri kaynaklarından ayıklanır ve Veri Kataloğu hizmetine kopyalanır, ancak içerdiği verilerin bir örneğini görüntülediğinizde veri kaynağını anlamak genellikle daha kolay hale getirilir.

Veri Kataloğu veri kaynağı kayıt aracını kullanarak, her tabloya ve kayıtlı görünüme verilerin anlık görüntüsü önizlemesini ekleyebilirsiniz. Kayıt sırasında önizlemeeklemeyi seçerseniz, kayıt aracı her tablo ve görünümden en fazla 20 kayıt içerir. Bu anlık görüntü daha sonra yapısal ve açıklayıcı meta verilerle birlikte kataloga kopyalanır.

> [!NOTE]
> Çok sayıda sütuna sahip geniş tabloların önizlemelerinde 20'den az kayıt olabilir.
>
>

## <a name="include-data-profiles"></a>Veri profilleri ekleme
Önizlemeler dahil olmak üzere, veri profili de dahil olmak üzere Veri Kataloğu'nda veri kaynakları arayan kullanıcılar için değerli bağlam lar sağlayabilirse, keşfedilen veri kaynaklarının anlaşılmasını da kolaylaştırabilir.

Veri Kataloğu veri kaynağı kayıt aracını kullanarak, her tablo ve kayıtlı görünüm için bir veri profili ekleyebilirsiniz. Kayıt sırasında bir veri profili eklemeyi seçerseniz, kayıt aracı her tablo ve görünümdeki verilerle ilgili toplu istatistikler içerir:

* Satır sayısı ve nesnedeki verilerin boyutu.
* Verilerin en son güncelleştirme tarihi ve nesne şeması.
* Null kayıtların sayısı ve sütunlar için farklı değerler.
* Sütunlar için minimum, maksimum, ortalama ve standart sapma değerleri.

Bu istatistikler daha sonra yapısal ve açıklayıcı meta verilerle birlikte kataloga kopyalanır.

> [!NOTE]
> Metin ve tarih sütunları veri profillerinde ortalama veya standart sapma istatistikleri içermez.
>
>

## <a name="update-registrations"></a>Kayıtları güncelleştir
Bir veri kaynağını kaydetmek, kayıt sırasında çıkarılan meta verileri ve isteğe bağlı önizlemeyi kullandığınızda Veri Kataloğu'nda bulunabilir hale getirir. Veri kaynağının katalogda güncellenmesi gerekiyorsa (örneğin, bir nesnenin şeması değiştiyse, başlangıçta dışlanan tablolar dahil edilmeli veya önizlemelerde yer alan verileri güncelleştirmek istiyorsanız), veri kaynağı kayıt aracı yeniden çalıştırılabilir.

Zaten kayıtlı bir veri kaynağının yeniden kaydedilmesi birleştirme "ekle" işlemi gerçekleştirir: varolan nesneler güncelleştirilir ve yeni nesneler oluşturulur. Veri Kataloğu portalı aracılığıyla kullanıcılar tarafından sağlanan tüm meta veriler korunur.

## <a name="summary"></a>Özet
Yapısal ve açıklayıcı meta verileri bir veri kaynağından katalog hizmetine kopyaladığı için, Veri Kataloğu'nda veri kaynağını kaydetmek verilerin bulunmasını ve anlaşılmasını kolaylaştırır. Veri kaynağını kaydettikten sonra, Veri Kataloğu portalını kullanarak açıklama ekleyebilir, yönetebilir ve keşfedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Veri kaynaklarını kaydetme hakkında daha fazla bilgi için [Azure Veri Kataloğu ile Başlat'a](data-catalog-get-started.md) bakın.
