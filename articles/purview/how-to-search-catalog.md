---
title: 'Nasıl yapılır: veri kataloğunu arama'
description: Bu makale, veri kataloğu arama hakkında genel bakış sunar.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/15/2020
ms.openlocfilehash: 87ca3c49d6a96f083cc50f01f5b9a4a739d688a1
ms.sourcegitcommit: 65db02799b1f685e7eaa7e0ecf38f03866c33ad1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96554030"
---
# <a name="search-the-azure-purview-data-catalog"></a>Azure purview Veri Kataloğu 'Nda arama

Bu makalede, Azure purview veri kataloğunda çeşitli arama özelliklerinin nasıl kullanılacağı açıklanır.

## <a name="search-the-catalog-for-assets"></a>Katalogda varlıkları ara

Bir varlık aramasını yürütmek için gereken adımlar şunlardır:

1. **Arama kataloğu**' nu seçerek [varlık arama iletişim kutusunu açın](#open-the-asset-search-dialog) .
1. Koşullara uyan özelliklerle varlıkları bulmak için [arama terimleri girin](#enter-search-terms) .
1. Aramayı daraltmak için [hızlı filtreler ayarlayın](#set-quick-filters) .
1. [Aramayı başlatın](#start-the-search) ve arama sonuçlarına gidin.

Arama terimlerini girmeden önce veya sonra hızlı filtreleri ayarlayıp ayarlamanıza bakılmaksızın.

Arama terimi yoksa ve filtre yoksa, arama sonuçları tüm varlıkları içerir.

### <a name="open-the-asset-search-dialog"></a>Varlık arama iletişim kutusunu aç

**Arama kataloğu**' nu seçerek varlık arama iletişim kutusunu açın.

:::image type="content" source="./media/how-to-search-catalog/search-catalog.png" alt-text="' Arama Kataloğu ' altında, arama filtreleri içeren bir sol bölme ve son aramalarla sağ bölme bulunur." border="true":::

Arama iletişim kutusunda hızlı filtreler, arama geçmişi ve son erişilen varlıkların bir listesi gösterilir.

:::image type="content" source="./media/how-to-search-catalog/asset-search-dialog.png" alt-text="Arama listesi, sağ bölmede, ' Arama Kataloğu ' ' nın altında bulunur." border="true":::

### <a name="enter-search-terms"></a>Arama terimlerini girin

**Arama kataloğunda** bir veya daha fazla arama terimi girin. Siz yazarken, son aramalardaki arama terimleri, **son aramalarınızda** listelenir, önerilen eşleşen arama terimleri **arama önerileri** bölümünde listelenir ve eşleşen veri varlıkları **varlık önerileri** bölümünde listelenir.

:::image type="content" source="./media/how-to-search-catalog/enter-search-terms.png" alt-text="Arama kataloğu kutusuna girilen bir aramanın sonuçlarını gösteren ekran görüntüsü":::

Arama sonuçları yalnızca arama terimleriyle eşleşen bir veya daha fazla özelliği olan varlıkları içerir. Bu özellikler varlık adını, varlık türünü, sınıflandırmaları ve ilgili kişileri içerir.

#### <a name="types-of-search-criteria"></a>Arama ölçütü türleri

Azure takip görünümü aşağıdaki arama ölçütü türlerini destekler.

> [!Note]
> Her zaman tüm büyük harfler içinde Boole işleçleri (**ve** **veya**, **değil**) belirtin. Aksi takdirde, büyük/küçük harf ek boşluklar değildir.

- **Hive**: **Hive** içeren belgeleri bulun.
- **Hive veritabanı**: tam olarak **Hive veritabanı** içeren belgeleri bulun.
- **HIVE veya veritabanı**: **Hive** veya **veritabanı** içeren belgeleri veya her ikisini de bulun.
- **HIVE ve veritabanı**, **Hive && veritabanı**: hem **Hive** hem de **veritabanı** içeren belgeleri bulun.
- **HIVE ve (VERITABANı veya ambar)**: **Hive** , **veritabanı** veya **Ambar** ya da her ikisini de içeren belgeleri bulun.
- **Hive VERITABANı değil**: **Hive** içeren, ancak **veritabanı** olmayan belgeleri bulun.
- **IV**: **IV** ile başlayan bir sözcük içeren belgeleri bulun. Örneğin, **IV**, **Hive**, **hivbar** (* herhangi bir sayıda karakterle eşleşen bir joker karakterdir).

### <a name="set-quick-filters"></a>Hızlı filtreleri ayarla

Arama sonuçları listesi, **arama kataloğuna** girdiğiniz arama koşullarına ve hızlı filtreler için seçtiğiniz değerlere göre belirlenir.

Hızlı filtre, arama sonuçları listesini, bir özellik için seçili bir değere sahip olan varlıklarla sınırlandırır. Filtrenin açılan listesi ve metin kutusu vardır. Açılan listede, *geçerli* arama sonuçlarında bulunan özelliğin değerleri gösterilir. Listedeki her bir değerin yanında, geçerli arama sonuçlarındaki varlık sayısının bu değere sahip olduğu bir sayıdır. Listeden bir değer seçerseniz, arama sonuçları bu değere sahip olan varlıklarla kısıtlanır. Yalnızca bir değer seçebilirsiniz.

Açılan listeyi oluşturulurken kullanılan geçerli arama sonuçları şu şekilde belirlenir:

- **Arama kataloğuna** girilen arama terimleri. 
- Hızlı filtrelerinde seçilen değerler.

"Varlık türü" hızlı filtresine bir örnek aşağıda verilmiştir.

:::image type="content" source="./media/how-to-search-catalog/asset-type-quick-filter.png" alt-text="Varlık türü hızlı filtre örneği." border="true":::

Açılan listedeki değerleri, metinle eşleşen veya kısmen eşleşen değerlerle kısıtlamak için metin kutusuna metin girebilirsiniz. Metin kutusunu kullanma örnekleri için bkz. [Arama hızlı filtre: varlık türüne göre filtreleme](#search-quick-filter-filter-by-asset-type)ve [hızlı filtre arama: sınıflandırmaya göre filtreleme](#search-quick-filter-filter-by-classification).

#### <a name="search-quick-filter-filter-by-asset-type"></a>Hızlı filtre ara: varlık türüne göre filtrele

Varlık türüne göre filtrelemek için **varlık türü** hızlı filtre ' yi kullanın. Açılan liste, arama terimleri ve hızlı filtreler tarafından belirlendiği şekilde, geçerli arama sonuçlarında bulunan varlık türlerini gösterir. Her tür için, bu tür varlıkların sayısı gösterilir.

:::image type="content" source="./media/how-to-search-catalog/asset-type-quick-filter.png" alt-text="Varlık türü hızlı filtreleri vurgulanır. Varlık türlerini ve her biri için bir sayıyı gösterir." border="true":::

Arama sonuçlarını bu türdeki varlıklarla sınırlamak için bir varlık türü seçin. Yalnızca bir tür seçebilirsiniz.

Yalnızca adları bir dizeyle eşleşen varlık türlerini göstermek için, metin kutusuna dizeyi girin. Örneğin, adlarında **SQL** ile yalnızca varlık türlerini göstermek için **SQL** girin.

:::image type="content" source="./media/how-to-search-catalog/filter-asset-types.png" alt-text="Hızlı filtreler bölmesi ' varlık türü ' için SQL 'e sahiptir. SQL içeren varlıkların listesi üç giriş gösterir." border="true":::

Arama sonuçlarını bu türdeki varlıklarla sınırlamak için bir varlık türü seçin. Yalnızca bir tür seçebilirsiniz.

#### <a name="search-quick-filter-filter-by-classification"></a>Hızlı filtre ara: sınıflandırmaya göre filtrele

Varlık sınıflandırmasına göre filtrelemek için **Sınıflandırma** hızlı filtresini kullanın. Açılan listede, arama terimleri ve hızlı filtreler tarafından belirlendiği şekilde, geçerli arama sonuçlarındaki bir veya daha fazla kıymete atanmış sınıflandırmalar gösterilmektedir. Her sınıflandırma için, sınıflandırmanın atandığı varlıkların sayısı gösterilir.

:::image type="content" source="./media/how-to-search-catalog/classification-quick-filter.png" alt-text="Sınıflandırma hızlı filtreleri vurgulanır." border="true":::

Arama sonuçlarını bu sınıflandırmanın atandığı varlıklarla sınırlamak için bir sınıflandırma seçin. Yalnızca bir sınıflandırma seçebilirsiniz.

Yalnızca adları bir dizeyle eşleşen sınıflandırmaları göstermek için, metin kutusuna dizeyi girin. Örneğin, yalnızca adlarında **sayı** olan sınıflandırmaları göstermek için **numara** girin.

:::image type="content" source="./media/how-to-search-catalog/filter-classifications.png" alt-text="Hızlı filtreler bölmesinde, sınıflandırma ' Bank ' ve tümü listelenen sınıflandırmalar bu değeri içerir." border="true":::

Arama sonuçlarını sınıflandırmanın atandığı varlıklarla sınırlamak için bir sınıflandırma seçin. Birden fazla sınıflandırma seçemezsiniz.

#### <a name="search-quick-filter-filter-by-contacts"></a>Hızlı filtre ara: kişilere göre filtrele

*Kişi* , bir varlığa sahip veya uzman olarak atanan bir kişidir. Varlık ayrıntılarını görüntülediğinizde, kişiler **iletişim** sekmesinde görüntülenir.

Belirli bir kişinin atandığı varlıkları aramak için iki yol vardır.

- **Arama kataloğunda** kişi adının tamamını veya bir kısmını girin ve bir arama yapın. Arama sonuçları, adları arama terimlerle eşleşen kişileri olan varlıkları içerir.
- **İletişim** hızlı filtreleyerek ilgilendiğiniz kişiyi seçin ve bir arama yapın.

:::image type="content" source="./media/how-to-search-catalog/contact-quick-filter.png" alt-text="Hızlı filtreler bölmesindeki kişinin değeri ' Darren ' olur. Öneriler bölmesinde üç öneri vardır." border="true":::

## <a name="search-example"></a>Arama örneği

Arama terimlerini ve hızlı filtrelerin arama sonuçlarını belirlemede nasıl etkileşime gireceğini görmek için bir kuramsal örnek ele alalım. Özellikle, **Azure Blob Storage** varlık türü sayısını izliyoruz.

- Arama terimi girilmemiş ve hızlı filtrelerdeki hiçbir değer seçili olmayan ilk aramayı yaptık. Arama, katalogdaki tüm varlıkları bulur. Arama sonuçları listesi ve **varlık türü** hızlı filtre açığa çıkar:

    - Arama sonuçları listesinde, katalogdaki tüm varlıklar olan 164.230 varlık bulunur.
    - **Varlık türü** açılan listesinde 43 girişi vardır. Bunlar, katalogdaki tüm varlık türleridir. Her varlık bir ve yalnızca bir tür olduğundan, 43 varlık türünün toplam sayısı 164.230 ' dir.
    - **Azure Blob depolama** varlık türü, **varlık türü** hızlı filtresinin açılan listesindeki ilk girdidir. Değerler öncelikle sayımla, en büyük olarak sıralanmıştır, bu nedenle **Azure Blob depolaması** en yaygın varlık türüdür. Sayısı 118.174 ' dir.

- Şimdi **arama kataloğuna** **Parquet** girip başka bir arama yapacağız. Arama sonuçları yalnızca **Parquet** ile eşleşen özelliklere sahip varlıkları içerir. Bu, tüm sayıları aşağıda gösterildiği gibi azaltır:

    - Arama sonuçları listesinde 493 varlık bulunur. Katalogdaki 493 varlıkların yalnızca 164.230 ' i "Parquet" ile eşleşen özellikleri vardır.
    - **Varlık türü** açılan listesinde 15 girişi vardır. 493 varlıklarından her biri bu 15 türden biridir ve 15 tür sayımlarının toplamı 493 ' dir.
    - **Azure Blob depolama** türünde 456 varlık vardır. Diğer 37 (493 eksi 456) varlıkları, diğer 14 türlerinden biridir.

- Artık farklı bir hızlı filtrenin ve **sınıflandırmanın** aşağı açılan listesine baktık:

    - Arama sonuçları listesinde 493 varlık için 12 sınıflandırma vardır. Bir varlığa herhangi bir sayıda sınıflandırma atanabileceği için 493 varlıkların sayısı 493 ' e kadar değildir.
    - **Kişinin ad** sınıflandırması, diğer sınıflandırmadan daha fazlası olan 36 varlıklara atanır.

- **Kişinin ad** sınıflandırmasını seçtik. **Kişi adının** sayısı 36 olduğundan, arama sonuçları listesi, beklenen şekilde 36 kıymete düşer. Bu sonuçlardan hiçbiri **Azure Blob depolama** türünde değildir.

Türü, **Azure Blob depolama** olan ve bir **kişinin adı** sınıflandırmasına sahip hiçbir varlık **olmadığını de** sonuçlarız.

## <a name="start-the-search"></a>Aramayı Başlat

Arama yaparken, **arama kataloğunda** girdiğiniz arama terimleri varlık özellikleriyle eşleştirilir. Bu özellikler ad, tür, sınıflandırma ve ilgili kişileri içerir. Eşleşen özelliklere sahip varlıklar, hızlı bir filtre tarafından dışlanmadığı takdirde arama sonuçları listesinde görünür.

Arama terimlerini girdikten ve hızlı filtreleri ayarladıktan sonra, aramayı şu yollarla başlatın:

- Girdiğiniz koşullara göre arama yapmak için arama simgesini ( :::image type="icon" source="./media/how-to-search-catalog/search-icon.png"::: ) seçin, **ENTER** tuşuna basın veya **arama sonuçlarını görüntüle**' yi seçin.
- Önceki bir aramanın koşullarını kullanarak arama yapmak için **son aramalardan** bunları seçin.
- Önerilen terimleri kullanarak arama yapmak için **arama önerilerinde** bunları seçin.

Varlık **önerilerinde** bir varlık seçerek varlık için ayrıntılar sayfasına doğrudan gidebilirsiniz. Arama yapılmaz.

Öneriler ve Kullanıcı aramaları için sonuçlar listesi biraz farklı olabilir. Öneriler listesinin sonuçları, benzer eşleşmelerin temel alınarak, Kullanıcı tarafından başlatılan arama sonuçları tam eşleşmeleri temel alır.

Arama yaptığınızda **Arama sonuçları** sayfası görünür ve arama tarafından bulunan varlıkları listeler.

:::image type="content" source="./media/how-to-search-catalog/search-results.png" alt-text="Contoso Arama değerinin arama sonuçlarını gösteren ekran görüntüsü.":::

Varlık ayrıntılarını görmek için bir varlık adı seçin.

Diğer arama sonuçları sayfalarına gitmek için en alttaki bir arama sonuçları sayfasındaki denetimleri kullanın.

:::image type="content" source="./media/how-to-search-catalog/page-navigation.png" alt-text="Arama sonuçları sayfalarında nasıl gezinileceğini gösteren ekran görüntüsü.":::

### <a name="sort-search-results"></a>Arama sonuçlarını Sırala

Arama sonuçlarını **ilgi** veya **ada** göre sıralamak için **sıralama ölçütü** ' ni kullanın.

:::image type="content" source="./media/how-to-search-catalog/sort-by.png" alt-text="Arama sonuçlarının nasıl sıralanacağını gösteren ekran görüntüsü. Bu örnek için sıralama ölçütü açılan listesi Ilgi olarak ayarlanır.":::

### <a name="search-results-dynamic-filters"></a>Arama sonuçları dinamik filtreleri

**Arama sonuçları** sayfasındaki **filtre** bölmesinde, arama sonuçları listesinde varlıkların dinamik filtrelemesini sağlayan filtreler bulunur. Filtreleme, ek filtrelerin filtre seçimlerini temel alarak görünebilen dinamik bir değer.

Dinamik filtrelerin, açılan listedeki her bir değer için bir onay kutusu vardır. İstediğiniz sayıda değeri filtrelemek için bu onay kutularını kullanın.

#### <a name="search-results-dynamic-filter-filter-by-asset-type"></a>Arama sonuçları dinamik filtresi: varlık türüne göre filtrele

**Varlık türü** aşağı açılan listesinde bir varlık türü seçerseniz, arama sonuçlarınızı daraltmak için ek yollar sağlayan dinamik filtreler görüntülenir. Filtreler, seçilen varlık türüne bağlı olarak değişir. Örneğin, **Azure SQL veritabanı**' nı seçerseniz sunucu, veritabanı ve şema için dinamik filtreler görüntülenir. Bu filtrelerdeki değerler, seçilen türün arama sonuçlarındaki varlıklardır.

:::image type="content" source="./media/how-to-search-catalog/asset-type-dynamic-filter.png" alt-text="Azure SQL veritabanı filtre öğesi, seçilen tek ' varlık türü ' öğesidir. Bu varlık türünün arama sonucu da vurgulanır." border="true":::

#### <a name="search-results-dynamic-filter-filter-by-classification"></a>Arama sonuçları dinamik filtresi: sınıflandırmayla filtrele

**Sınıflandırma** listesindeki her sınıflandırma, arama sonuçları listesinde en az bir öğe için geçerlidir. Arama sonuçlarınızı seçili sınıflandırmaların varlıklarına daraltmak için bir veya daha fazla sınıflandırma seçin.

:::image type="content" source="./media/how-to-search-catalog/classification-dynamic-filter.png" alt-text="' Arama sonuçları ' sınıflandırma filtresi vurgulanmıştır." border="true":::

#### <a name="edit-and-delete-search-results-filters"></a>Arama sonuçları Filtrelerini Düzenle ve Sil

Bir filtreyi kaldırmak için, filtre adının yanındaki onay kutusunu temizleyin.

### <a name="recently-accessed-assets"></a>Son erişilen varlıklar

Genişletilmiş arama kutusunun **son erişilen** bölümü, varsa en son erişilen varlıklarınızı görüntüler.

- Son erişilen varlıkların tam listesini görmek için **son erişilen** bölümünde **Tümünü görüntüle** ' yi seçin.

   :::image type="content" source="./media/how-to-search-catalog/get-to-recent-view.png" alt-text="Genişletilmiş arama kutusunun son erişilen bölümünü gösteren ekran görüntüsü.":::

   Son erişilen varlıkların listesi görüntülenir.

   :::image type="content" source="./media/how-to-search-catalog/recent.png" alt-text="Son erişilen varlıkların listesini gösteren ekran görüntüsü.":::

- Ada göre filtrelemek için, **ada göre filtrele**' ye bir arama dizesi girin.

- Listeden öğeleri kaldırmak için bunları onay kutularıyla birlikte seçin ve ardından **Kaldır**' ı seçin.

   :::image type="content" source="./media/how-to-search-catalog/remove-from-recent-view.png" alt-text="Son erişilen varlıklar listesinden öğelerin nasıl kaldırılacağını gösteren ekran görüntüsü.":::

- Tüm listeyi temizlemek için **Temizle**' yi seçin.

   :::image type="content" source="./media/how-to-search-catalog/clear-recent-view-selections.png" alt-text="Son erişilen bir varlık listesinin nasıl temizlendiğini gösteren ekran görüntüsü":::

### <a name="search-assets"></a>Varlıkları ara

**Evden** farklı birçok sayfanın üst kısımdaki bir **Arama varlıkları** kutusu vardır. Örneğin, **Arama varlıkları** vurgulanmış şekilde bir varlık ayrıntıları sayfası aşağıda verilmiştir.

:::image type="content" source="./media/how-to-search-catalog/search-assets.png" alt-text="Arama varlıkları vurgulanmış bir varlık ayrıntıları sayfasını gösteren ekran görüntüsü":::

Aynı yetenekler sayesinde, **evde** **arama kataloğu** 'ndan aldığınız gibi bir arama kutusunu başlatmak için **varlıkları ara** ' yı seçin.

:::image type="content" source="./media/how-to-search-catalog/search-assets-dialog.png" alt-text="Genişletilmiş bir arama varlıkları kutusunu gösteren ekran görüntüsü.":::

## <a name="next-steps"></a>Sonraki adımlar

- [Sözlük terimleri oluşturma, içeri aktarma ve dışarı aktarma](how-to-create-import-export-glossary.md)
- [İş sözlüğü için terim şablonlarını yönetme](how-to-manage-term-templates.md)
