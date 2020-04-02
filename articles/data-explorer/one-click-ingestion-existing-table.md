---
title: Varolan bir Azure Veri Gezgini tablosuna veri almak için tek tıklatma kullanma
description: Tek tıklatma işlemini kullanarak verileri varolan bir Azure Veri Gezgini tablosuna yalnızca alma (yükleme) alma.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: overview
ms.date: 03/29/2020
ms.openlocfilehash: a7fdab66394c132bcd9134669b841d178c559f28
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80546204"
---
# <a name="use-one-click-ingestion-to-ingest-data-to-an-existing-table-in-azure-data-explorer"></a>Azure Veri Gezgini'nde varolan bir tabloya veri almak için tek tıklatma kullanma

Tek tıklatma işlemi, JSON, CSV ve diğer biçimlerde ki verileri hızlı bir şekilde bir tabloya yutmanızı sağlar. Azure Veri Gezgini Web UI'sini kullanarak, depolamadan, yerel bir dosyadan veya bir kapsayıcıdan veri yutabilirsiniz. 

Bu belge, bir dosyadan JSON verilerini varolan bir tabloya almak için sezgisel tek tıklama sihirbazı kullanılarak açıklanır. Daha sonra tabloyu dinleyebilir ve Azure Veri Gezgini Web UI ile sorguları çalıştırabilirsiniz.

Tek tıklatma, verileri ilk kez sindirirken veya verilerinizin şeması size yabancı olduğunda özellikle yararlıdır. 

Tek tıklamayla giriş ve ön koşullar listesine genel bakış için tek [tıklamayla giriş](ingest-data-one-click.md)bölümüne bakın.
Azure Veri Gezgini'nde yeni bir tabloya veri alma hakkında bilgi için [bkz.](one-click-ingestion-new-table.md)

## <a name="ingest-new-data"></a>Yeni verileri yutma

1. Web UI'nin sol menüsünde, bir *veritabanını* veya *tabloyu* sağ tıklatın ve **yeni verileri (Önizleme) yut'u**seçin.

    ![Web Web Web Arama Sürümü'nde tek tıklatma oluşturma yı seçme](media/one-click-ingestion-existing-table/one-click-ingestion-in-webui.png)   
 
1. Yeni **veri (Önizleme) Yut'u Yut'ta** **Kaynak** sekmesi otomatik olarak seçilir.

1. **Tablo** alanı otomatik olarak doldurulmazsa, açılan menüden varolan bir tablo adı seçin.
    > [!TIP]
    > *Tablo* satırında **yeni verileri (Önizleme) yutacağını** seçerseniz, seçili tablo adı **Proje Ayrıntıları'nda**görünür.

[!INCLUDE [data-explorer-one-click-ingestion-types](../../includes/data-explorer-one-click-ingestion-types.md)]
    
Tablo sütun yapılandırmanızı görüntülemek ve düzenlemek için **şemayı edin'i** seçin.

## <a name="edit-the-schema"></a>Şeayı edin

1. **Harita sütunları** iletişim kutusu açılır ve kaynak veri sütunlarını hedef tablo sütunlarına eşleyebilirsiniz. 
    * Kaynak **sütun** alanlarında, **Hedef sütunlarla**eşlenecek sütun adlarını girin.
    * Eşlemeyi silmek için çöp kutusu simgesini seçin.

    ![Harita sütunları penceresi](media/one-click-ingestion-existing-table/map-columns.png)

1. **Güncelleştir** seçeneğini belirleyin.
1. **Şema** sekmesinde:
    1. **Sıkıştırma türünü**seçin ve ardından **Sıkıştırılmamış** veya **GZip'i**seçin.

        [!INCLUDE [data-explorer-one-click-ingestion-edit-schema](../../includes/data-explorer-one-click-ingestion-edit-schema.md)]
        
    1. **JSON'u**seçerseniz, 1'den 10'a kadar **JSON düzeylerini**de seçmeniz gerekir. Düzeyleri tablo sütun veri tasviri etkiler.

    ![JSON düzeylerini seçin](media/one-click-ingestion-existing-table/json-levels.png)

    * JSON dışında bir biçim seçerseniz, dosyanın başlık satırını yoksaymak için **sütun adlarını ekle** onay kutusunu seçebilirsiniz.
        
    ![Sütun Adlarını Ekle'yi seçin](media/one-click-ingestion-existing-table/non-json-format.png)

    > [!Note]
    > Tablo biçimleri, sütun verilerini Azure Veri Gezgini tablosunda yalnızca bir sütuna alabilir. 

    * Yeni eşlemeler otomatik olarak ayarlanır, ancak varolan bir tane kullanmak için değiştirebilirsiniz. 
    * **Harita sütunları** penceresini açmak için Harita **sütunlarını** seçebilirsiniz.

## <a name="copy-and-paste-queries"></a>Sorguları kopyalama ve yapıştır

1. **Düzenleyici** bölmesinin üzerinde, düzenleyiciyi açmak için **v** düğmesini seçin. Düzenleyicide, girişlerinizden oluşturulan otomatik komutları görüntüleyebilir ve kopyalayabilirsiniz. 
1. Tabloda: 
    * **Yeni sütun**eklemek için yeni sütun üstbilgileri seçin , **sütunu sil**, artan **sırala**veya **azalan sıralama**. Varolan sütunlarda yalnızca veri sıralama kullanılabilir.

    > [!Note]
    > * Varolan tabloların adını ve veri türünü güncelleştiremezsiniz.
    > * Bırakma komutları yalnızca yutma akışı (yeni boyutlar ve sütunlar) tarafından yapılan değişiklikleri geri alacaktır. Başka hiçbir şey bırakılacak.

[![](media/one-click-ingestion-existing-table/edit-view.png "One-click ingestion edit view")](media/one-click-ingestion-existing-table/edit-view.png#lightbox) 

## <a name="start-ingestion"></a>Yutma ya da yutma işlemine başlama

Tablo oluşturmak ve eşleme yapmak ve veri alımını başlatmak için **başlat'ı** seçin.

![Yutma ya da yutma işlemine başlama](media/one-click-ingestion-existing-table/start-ingestion.png)

## <a name="data-ingestion-completed"></a>Veri alımı tamamlandı

Veri **alım tamamlanan** penceresinde, veri alımı başarıyla biterse üç adım da yeşil onay işaretleriyle işaretlenir.
 
![Tek tıklamayla veri alımı tamamlandı](media/one-click-ingestion-existing-table/one-click-data-ingestion-complete.png)

[!INCLUDE [data-explorer-one-click-ingestion-query-data](../../includes/data-explorer-one-click-ingestion-query-data.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Web UI'deki verileri sorgula](/azure/data-explorer/web-query-data)
* [Kusto Query Language'i kullanarak Azure Veri Gezgini için sorgu yazma](/azure/data-explorer/write-queries)
