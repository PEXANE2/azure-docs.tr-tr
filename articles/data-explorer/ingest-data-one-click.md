---
title: Verileri Azure 'a almak için tek tıklamayla alma 'yı kullanın Veri Gezgini
description: Yalnızca tek tıklamayla alma kullanarak verileri Azure Veri Gezgini 'a alma (yükleme) hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 10/31/2019
ms.openlocfilehash: 0d7c8b2661ee3361b3a485b8cae4eef3a8225120
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74975253"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Verileri Azure 'a almak için tek tıklamayla alma 'yı kullanın Veri Gezgini

Bu makalede, JSON veya CSV biçiminde yeni bir tablonun hızlı bir şekilde alımı için tek tıklamayla alma 'nın nasıl kullanılacağı gösterilmektedir. Veriler depolama alanından veya yerel bir dosyadan var olan bir tabloya veya yeni bir tabloya alınabilir. Sezgisel tek tıklamayla Sihirbazı 'nı ve verilerinizi birkaç dakika içinde geri almak için kullanın. Daha sonra, Azure Veri Gezgini Web Kullanıcı arabirimini kullanarak tabloyu düzenleyebilir ve sorguları çalıştırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Uygulamada](https://dataexplorer.azure.com/) oturum açın.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md)oluşturun.
* [Web Kullanıcı arabiriminde](https://dataexplorer.azure.com/) oturum açın ve [kümenize bir bağlantı ekleyin](/azure/data-explorer/web-query-data#add-clusters).
* Azure depolama 'da bir veri kaynağı oluşturun.

## <a name="ingest-new-data"></a>Yeni verileri alma

1. Web Kullanıcı arabiriminin sol menüsünde *veritabanı* veya *tablo* satırına sağ tıklayın ve **yeni verileri al (Önizleme)** seçeneğini belirleyin.

    ![Web Kullanıcı arabiriminde tek tıklamayla alma ' yı seçin.](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. **Yeni veri (Önizleme)** alma penceresinde **kaynak** sekmesini seçin ve **proje ayrıntılarını**doldurun:

    * **Tablo**için, açılan menüden var olan bir tablo adı seçin veya yeni bir tablo **oluşturmak için yeni oluştur** ' u seçin.
    * Alma **türü**için **depolama alanından** **veya dosyadan birini seçin.**
        * **Depolama alanından**SEÇTIYSENIZ, URL eklemek Için **depolama bağlantısı** ' nı seçin. Özel depolama hesapları için [BLOB SAS URL 'sini](/azure/vs-azure-tools-storage-explorer-blobs#get-the-sas-for-a-blob-container) kullanın. 
            * **Dosya**' yı seçtiyseniz, **Araştır** ' ı seçin ve dosyayı kutuya sürükleyin.
    * Tablo sütunu yapılandırmanızı görüntülemek ve düzenlemek için **Şemayı Düzenle** ' yi seçin.
 
    ![Tek tıklamayla alım kaynağı ayrıntıları.](media/ingest-data-one-click/one-click-ingestion-source.png) 

    > [!TIP]
    > *Tablo* satırında **Yeni veri al (Önizleme)** seçeneğini belirlerseniz, seçilen tablo adı **Proje ayrıntılarında**görüntülenir.

1. Varolan bir tabloyu seçtiyseniz, kaynak veri sütunlarını hedef tablo sütunlarıyla eşlemek için **harita sütunları** penceresi açılır. 
    * Tablodan bir hedef sütunu kaldırmak için **sütunu atla** ' yı kullanın. 
    * Tabloya yeni bir sütun eklemek için **Yeni sütun** ' i kullanın. 

    ![Harita sütunları penceresi.](media/ingest-data-one-click/one-click-map-columns-window.png)

1. **Şema** sekmesinde:

    * Açılan menüden **sıkıştırma türü** ' nü seçin ve ardından **sıkıştırılmamış** ya da **gzip**' i seçin.
    * Açılan menüden **veri biçimini** seçin ve ardından **JSON**, **CSV**, **TSV**, **SCSV**, **sohsv**, **tsve**veya **PSV**' yi seçin. 
        * **JSON** biçimini seçtiğinizde, 1 ile 10 arasındaki **JSON düzeylerini**de seçmeniz gerekir. Düzeyler tablo sütunu veri gösterimi ' nı etkiler. 
        * JSON dışında bir biçim seçerseniz, dosyanın başlık satırını yoksaymak için **sütun adlarını dahil** et onay kutusunu seçmeniz gerekir.
    * **Eşleme adı** otomatik olarak ayarlanır, ancak düzenlenebilir.
    * Varolan bir tabloyu seçtiyseniz, eşleme **sütunları** penceresini açmak Için **sütunları eşle** ' yi seçebilirsiniz.

    ![Tek tıklamayla alma CSV biçim şeması.](media/ingest-data-one-click/one-click-csv-format.png)

1. **Düzenleyici** bölmesinin üzerinde, düzenleyiciyi açmak için **v** düğmesini seçin. Düzenleyicide, girdinizden oluşturulan otomatik sorguları görüntüleyebilir ve kopyalayabilirsiniz. 

1.  Tabloda: 
    * Yeni sütun başlıklarına sağ tıklayarak **veri türünü değiştirin**, **sütunu yeniden adlandırın**, **sütunu silin**, **artan düzende sıralayın**veya **azalan düzende sıralayın**. Mevcut sütunlarda yalnızca veri sıralama kullanılabilir. 
    * Düzenlemek için yeni sütun adına çift tıklayın.

1. Bir tablo ve eşleme oluşturmak ve veri alımı başlatmak için **Başlangıç** alımı ' nı seçin.

    ![Tek Tıklamasız alma JSON biçim şeması.](media/ingest-data-one-click/one-click-json-format.png) 
 
## <a name="query-data"></a>Verileri sorgulama

1. Veri alma **işlemi tamamlandı** penceresinde, verilerin alımı başarıyla tamamlanırsa, tüm üç adım yeşil onay işaretleriyle işaretlenir.
 
    ![Tek tıklamalı veri alma işlemi tamamlanmıştır.](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. Sorguyu açmak için **v** düğmesini seçin. Sorguyu düzenlemek için Web Kullanıcı arabirimine kopyalayın.

1. Sağdaki menü **hızlı sorgular** ve **Araçlar** seçeneklerini içerir. 

    * **Hızlı sorgular** , örnek sorgularla Web Kullanıcı arabirimine bağlantılar içerir.
    * **Araçlar** , Web Kullanıcı arabirimindeki **bırakma komutlarının** bir bağlantısını içerir ve bu, ilgili `.drop` komutlarını çalıştırarak sorunları gidermenize olanak sağlar.

    > [!TIP]
    > `.drop` komutları kullanılarak veriler kaybolabilir. Bunları dikkatle kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Web Kullanıcı arabirimindeki verileri sorgulama](web-query-data.md)
* [Kusto sorgu dilini kullanarak Azure Veri Gezgini için sorgular yazma](write-queries.md)
