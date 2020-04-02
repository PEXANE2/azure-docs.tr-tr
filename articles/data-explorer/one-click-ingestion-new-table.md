---
title: Yeni bir Azure Veri Gezgini tablosuna veri almak için tek tıklamayla yararlanma kullanma
description: Tek tıklatma işlemini kullanarak verileri yeni bir Azure Veri Gezgini tablosuna yutma (yükleme)
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: overview
ms.date: 03/29/2020
ms.openlocfilehash: 1e337a46d1d8ddda1b07bde6f12d4c1f7feda42f
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549226"
---
# <a name="use-one-click-ingestion-to-ingest-data-to-a-new-table-in-azure-data-explorer"></a>Azure Veri Gezgini'nde yeni bir tabloya veri almak için tek tıklatma kullanma

Tek tıklatma işlemi, JSON, CSV ve diğer biçimlerde ki verileri hızlı bir şekilde bir tabloya yutmanızı sağlar. Azure Veri Gezgini Web UI'sini kullanarak, depolamadan, yerel bir dosyadan veya bir kapsayıcıdan veri yutabilirsiniz. 

Bu belge, bir kapsayıcıdan yeni bir tabloya CSV verileri almak için sezgisel tek tıklama sihirbazı kullanarak açıklar. Daha sonra tabloyu dinleyebilir ve Azure Veri Gezgini Web UI ile sorguları çalıştırabilirsiniz. Ayrıca, kaynak veri güncellediğinde verileri otomatik olarak tabloya yutacak şekilde sürekli alım ayarlayabilirsiniz.

Tek tıklatma, verileri ilk kez sindirirken veya verilerinizin şeması size yabancı olduğunda özellikle yararlıdır. 

Tek tıklamayla giriş ve ön koşullar listesine genel bakış için tek [tıklamayla giriş](ingest-data-one-click.md)bölümüne bakın.
Azure Veri Gezgini'nde varolan bir tabloya veri alma hakkında bilgi için [varolan tabloya tek tıklamayla](one-click-ingestion-existing-table.md) giriş

## <a name="ingest-new-data"></a>Yeni verileri yutma

1. Web UI'nin sol menüsünde, bir *veritabanını* sağ tıklatın ve **yeni verileri (Önizleme) yut'u**seçin.

    ![Web Web Web Arama Sürümü'nde tek tıklatma oluşturma yı seçme](media/one-click-ingestion-new-table/one-click-ingestion-in-web-ui.png)   
 
1. Yeni **veri (Önizleme) Yut'u Yut'ta** **Kaynak** sekmesi otomatik olarak seçilir. 

1. **Yeni tablo Oluştur'u** seçin ve yeni tablo için bir ad girin. Alfasayısal, tire ve alt çizgi kullanarak. Özel karakterler desteklenmez.

![Yeni bir tablo oluşturma](media/one-click-ingestion-new-table/create-new-table.png) 

[!INCLUDE [data-explorer-one-click-ingestion-types](../../includes/data-explorer-one-click-ingestion-types.md)]

Tablo sütun yapılandırmanızı görüntülemek ve düzenlemek için **şemayı edin'i** seçin. Sistem rastgele lekeler birini seçecek ve şema bu blob dayalı oluşturulacak. Kaynağın adına bakarak, hizmet sıkıştırılmış olup olmadığını otomatik olarak tanımlar.

## <a name="edit-the-schema"></a>Şeayı edin

1. **Şema** sekmesinde:

    1. **Veri biçimini**seçin:

        [!INCLUDE [data-explorer-one-click-ingestion-edit-schema](../../includes/data-explorer-one-click-ingestion-edit-schema.md)]

    1. **JSON'u**seçerseniz, 1'den 10'a kadar **JSON düzeylerini**de seçmeniz gerekir. Düzeyleri tablo sütun veri tasviri etkiler. 

    ![JSON düzeylerini seçin](media/one-click-ingestion-new-table/json-levels.png)

    * JSON dışında bir biçim seçerseniz, dosyanın başlık satırını yoksaymak için **sütun adlarını ekle** onay kutusunu seçebilirsiniz.

        ![Sütun Adlarını Ekle'yi seçin](media/one-click-ingestion-new-table/non-json-format.png)
        
1. **Eşleme adı** alanına bir eşleme adı girin. Alfasayısal karakterler ve alt çizgi kullanarak. Boşluklar, özel karakterler ve tireler desteklenmez.
    
    ![Tablo eşleme adı](media/one-click-ingestion-new-table/table-mapping.png)

## <a name="copy-and-paste-queries"></a>Sorguları kopyalama ve yapıştır

1. **Düzenleyici** bölmesinin üzerinde, düzenleyiciyi açmak için **v** düğmesini seçin. Düzenleyicide, girişlerinizden oluşturulan otomatik sorguları görüntüleyebilir ve kopyalayabilirsiniz. 
1. Tabloda: 
    * Yeniden tıklamak için yeni sütun adını çift tıklatın.
    * Yeni sütun üstbilgilerini seçin ve aşağıdakilerden birini yapın:
    
|Eylem         |Açıklama                                  |
|-----------------|-------------------------------------------|
|Veri türünü değiştirme |Veri türünü hizmet tarafından otomatik olarak seçilen den diğer [desteklenen veri türlerinden](#edit-the-schema) birine değiştirme|
|Sütunu yeniden adlandır    |Sütun adını değiştirme |
|Yeni sütun       |Yeni bir sütun ekleme|
|Sütunu silme    |Seçili sütunu silme|
|Artan düzende sırala   |Tabloyu artan sırada seçili sütuna göre sırala (yalnızca varolan sütunlar)|
|Azalan düzende sırala  |Tabloyu azalan sırada seçili sütuna göre sırala (yalnızca varolan sütunlar) |

> [!Note]
> Tabular biçimler için her sütun Azure Veri Gezgini'nde bir sütuna yutulabilir.
> Farklı JSON düzeylerinden yeni sütunlar oluşturabilirsiniz.

[![](media/one-click-ingestion-new-table/edit-view.png "One-click ingestion edit view")](media/one-click-ingestion-new-table/edit-view.png#lightbox) 

## <a name="start-ingestion"></a>Yutma ya da yutma işlemine başlama

Tablo oluşturmak ve eşleme yapmak ve veri alımını başlatmak için **başlat'ı** seçin.
![Yutma ya da yutma işlemine başlama](media/one-click-ingestion-new-table/start-ingestion.png)

## <a name="data-ingestion-completed"></a>Veri alımı tamamlandı

Veri **alım tamamlanan** penceresinde, veri alımı başarıyla biterse üç adım da yeşil onay işaretleriyle işaretlenir.
 
![Tek tıklamayla veri alımı tamamlandı](media/one-click-ingestion-new-table/one-click-data-ingestion-complete.png)

[!INCLUDE [data-explorer-one-click-ingestion-query-data](../../includes/data-explorer-one-click-ingestion-query-data.md)]

**Sürekli yutma**

Sürekli yutma, kaynak kapsayıcıyı dinleyen bir olay ızgarası oluşturmanıza olanak tanır. Önceden tanımlanmış parametrelerin (önek, sonek vb.) ölçütlerini karşılayan herhangi bir yeni blob otomatik olarak hedef tabloya yutulur.

> [!Note]
> Sürekli yutma sadece bir kaptan sindirilirken ilgilidir.

1. Azure portalını açmak için **Sürekli başlatma'yı** seçin. Veri bağlantısı sayfası, olay ızgarası veri bağlayıcısı açılırken ve kaynak ve hedef parametreleri girildi (kaynak kapsayıcısı, tablolar ve eşlemeler) ile açılır.

1. Kapsayıcı kapsayıcıdaki değişiklikleri dinleyecek bir veri bağlantısı oluşturmak için **Oluştur'u** seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Web UI'deki verileri sorgula](/azure/data-explorer/web-query-data)
* [Kusto Query Language'i kullanarak Azure Veri Gezgini için sorgu yazma](/azure/data-explorer/write-queries)
