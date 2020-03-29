---
title: Azure Veri Gezgini'ne veri almak için tek tıklamayla yararlanma kullanma
description: Tek tıklamayla Azure Veri Gezgini'ne nasıl veri yükleyebilirsiniz (yüklenir) hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 4a53f7e68501ce7f9b19dea0822d3896ec241fb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75444546"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Azure Veri Gezgini'ne veri almak için tek tıklamayla yararlanma kullanma

Bu makalede, JSON veya CSV biçimlerinde yeni bir tablonun hızlı bir şekilde alınması için tek tıklamayla nasıl kullanılacağı gösterilmektedir. Veriler depolama dan veya yerel bir dosyadan varolan bir tabloya veya yeni bir tabloya yutulabilir. Sezgisel tek tıklama sihirbazını kullanın ve verileriniz birkaç dakika içinde yutunur. Ardından, Azure Veri Gezgini Web UI'sini kullanarak tabloyu ve sorguları çalıştırabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Uygulamada](https://dataexplorer.azure.com/)oturum açın.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md)oluşturun.
* [Web UI'da](https://dataexplorer.azure.com/) oturum açın ve [kümenize bağlantı ekleyin.](/azure/data-explorer/web-query-data#add-clusters)

## <a name="ingest-new-data"></a>Yeni verileri yutma

1. Web UI'nin sol menüsünde *veritabanına* veya *tablo* satırına sağ tıklayın ve **yeni verileri Edin 'i (Önizleme)** seçin.

    ![Web Web Web Arama Sürümü'nde tek tıklatma oluşturma yı seçme](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. Yeni **verileri Yut (Önizleme)** penceresinde **Kaynak** sekmesini seçin ve **Proje Ayrıntılarını**tamamlayın:

    * **Tablo**için açılan menüden varolan bir tablo adı seçin veya yeni bir tablo oluşturmak için **yeni oluştur'u** seçin.
    * **Yutma türü için,** depolama dan veya **dosyadan** seçin. **from file**
      * **Depolama dan**seçtiyseniz, URL'yi eklemek için depolama **alanına Bağlantı'yı** seçin. Özel depolama hesapları için [Blob SAS URL'yi](/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) kullanın. 
      * **Dosyadan**seçtiyseniz, **Gözat'ı** seçin ve dosyayı kutuya sürükleyin.
    * Tablo sütun yapılandırmanızı görüntülemek ve düzenlemek için **şemayı edin'i** seçin.
 
    ![Tek tıklamayla yutma kaynak ayrıntıları](media/ingest-data-one-click/one-click-ingestion-source.png) 

    > [!TIP]
    > *Tablo* satırında **yeni verileri (Önizleme) yutacağını** seçerseniz, seçili tablo adı **Proje Ayrıntıları'nda**görünür.

1. Varolan bir tablo seçtiyseniz, **Harita sütunları** penceresi, hedef tablo sütunlarına kaynak veri sütunlarını eşlemek için açılır. 
    * Hedef sütunu tablodan kaldırmak için **Omit sütununa** kullanın.
    * Tabloya yeni bir sütun eklemek için **Yeni sütunu** kullanın.

    ![Harita sütunları penceresi](media/ingest-data-one-click/one-click-map-columns-window.png)

1. **Şema** sekmesinde:

    * Açılan menüden **Sıkıştırma türünü** seçin ve ardından **Sıkıştırılmamış** veya **GZip'i**seçin.
    * Açılan menüden **Veri biçimini** seçin ve ardından **JSON,** **CSV,** **TSV,** **SCSV,** **SOHSV**, **TSVE**veya **PSV'yi**seçin. 
        * **JSON** biçimini seçtiğinizde, 1 ile 10 arasında **JSON düzeylerini**de seçmeniz gerekir. Düzeyleri tablo sütun veri tasviri etkiler. 
        * JSON dışında bir biçim seçerseniz, dosyanın başlık satırını yoksaymak için **sütun adlarını ekle** onay kutusunu seçmeniz gerekir.
    * **Eşleme adı** otomatik olarak ayarlanır, ancak düzenlenebilir.
    * Varolan bir tabloseçtiyseniz, **Harita sütunları** penceresini açmak için Harita **sütunlarını** seçebilirsiniz.

    ![Tek tıklamayla yutma CSV format Şeması](media/ingest-data-one-click/one-click-csv-format.png)

1. **Düzenleyici** bölmesinin üzerinde, düzenleyiciyi açmak için **v** düğmesini seçin. Düzenleyicide, girişlerinizden oluşturulan otomatik sorguları görüntüleyebilir ve kopyalayabilirsiniz. 

1. Tabloda: 
    * **Veri türünü değiştirmek**için yeni sütun üstbilgilerini sağ tıklatın , **sütunu yeniden adlandırın**, **Sütunu sil**, artan **sıralama,** veya **azalan sıralama**. Varolan sütunlarda yalnızca veri sıralama kullanılabilir. 
    * Yeniden tıklamak için yeni sütun adını çift tıklatın.

1. Tablo oluşturmak ve eşleme yapmak ve veri alımını başlatmak için **başlat'ı** seçin.

    ![Tek tıklamayla yutma JSON formatında şema](media/ingest-data-one-click/one-click-json-format.png) 
 
## <a name="query-data"></a>Verileri sorgulama

1. Veri **alım tamamlanan** penceresinde, veri alımı başarıyla biterse üç adım da yeşil onay işaretleriyle işaretlenir.
 
    ![Tek tıklamayla veri alımı tamamlandı](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. Sorguyu açmak için **v** düğmesini seçin. Sorguyu sağlamak için Web UI'ye kopyalayın.

1. Sağdaki menüde **Hızlı sorgular** ve **Araçlar** seçenekleri bulunmaktadır. 

    * **Hızlı sorgular,** örnek sorgularla Birlikte Web UI'ye bağlantılar içerir.
    * **Araçlar,** ilgili `.drop` **komutları** çalıştırarak sorunları gidermenize olanak tanıyan Web UI'daki Drop komutlarına bağlantı içerir.

    > [!TIP]
    > Komutları kullandığınızda `.drop` veri kaybedebilirsiniz. Dikkatli kullan.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Web UI'deki verileri sorgula](web-query-data.md)
* [Kusto Query Language'i kullanarak Azure Veri Gezgini için sorgu yazma](write-queries.md)
