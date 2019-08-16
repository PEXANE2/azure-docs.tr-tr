---
title: Verileri Azure 'a almak için tek tıklamayla alma 'yı kullanın Veri Gezgini
description: Yalnızca tek tıklamayla alma kullanarak verileri Azure Veri Gezgini 'a alma (yükleme) hakkında bilgi edinin.
author: orspod
ms.author: orspodek
ms.reviewer: tzgitlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: b1ce2d9efe44021b4e3191739bd2f922e34c44cb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69520055"
---
# <a name="use-one-click-ingestion-to-ingest-data-into-azure-data-explorer"></a>Verileri Azure 'a almak için tek tıklamayla alma 'yı kullanın Veri Gezgini

Bu makalede, Azure Veri Gezgini JSON veya CSV biçimlerinde yeni bir tablonun hızlı alımı için tek tıklamayla alma işlemlerinin nasıl kullanılacağı gösterilmektedir. Veriler alındıktan sonra, tabloyu düzenleyebilir ve sorguları Web Kullanıcı arabirimini kullanarak çalıştırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* [Uygulamada](https://dataexplorer.azure.com/) oturum açın.
* [Azure Veri Gezgini kümesi ve veritabanı](create-cluster-database-portal.md) oluşturma
* Azure depolama 'daki veri kaynağı.

## <a name="ingest-new-data"></a>Yeni verileri alma

1. *Veritabanı adına* sağ tıklayın ve **yeni verileri al (Önizleme)** seçeneğini belirleyin.

    ![Web Kullanıcı arabiriminde bir tıklama alımı seçin](media/ingest-data-one-click/one-click-ingestion-in-webui.png)   
 
1. Veri alma **(Önizleme)** penceresindeki **kaynak** sekmesinde, **proje ayrıntılarını**doldurun:

    * Yeni **tablo adı**girin. 
    * **Depolama alanından**alma **türünü** > seçin.
    * Depolama alanına **bağlantı** girin depolama alanına URL Ekle. Özel depolama hesapları için blob SAS URL 'sini kullanın. 
    * **Şemayı Düzenle** seçeneğini belirleyin
 
    ![bir tıklama kaynağı ayrıntıları](media/ingest-data-one-click/one-click-ingestion-source.png) 

1. **Şema** sekmesinde, **JSON** veya **CSV**> açılan listesinden **veri biçimi** ' ni seçin. 
   
   **CSV**'yi seç:
    * CSV dosyasının başlık satırını yoksaymak için onay kutusu başlığını **Yoksay** ' ı seçin.    
    * **Eşleme adı** otomatik olarak ayarlanır, ancak düzenlenebilir.

    ![tek tıklamayla alma CSV biçimi Schema. png](media/ingest-data-one-click/one-click-csv-format.png)

   **JSON**seç:
    * **JSON düzeylerini**seçin: açılan kutudan 1-10. JSON dosyasındaki düzeyler sağ alt taraftaki tabloda gösterilir. 
    * **Eşleme adı** otomatik olarak ayarlanır, ancak düzenlenebilir.

    ![Tek tıklama alma JSON biçim şeması](media/ingest-data-one-click/one-click-json-format.png)  

1. **Düzenleyici**' de, düzenleyiciyi açmak için sağ taraftaki **V** ' yi seçin. Düzenleyicide, girdinizden oluşturulan otomatik sorguları görüntüleyebilir ve kopyalayabilirsiniz. 

1.  Sağ alt kısımdaki tabloda: 
    * Sütunu **yeniden adlandırmak**, **sütunu silmek**, **artan düzende sıralamak**veya **azalan düzende sıralamak** için sütunun sağ üstünde bulunan **V** 'yi seçin
    * Düzenlemek için sütun adına çift tıklayın.
    * Veri türünü değiştirmek için sütun adının solundaki simgeyi seçin. 

1. Tablo oluşturmak, eşleme oluşturmak ve veri alımı yapmak için **Başlangıç** alımı ' nı seçin.
 
## <a name="query-data"></a>Verileri sorgulama

1. Veri alma **işlemi tamamlandı** penceresinde, veri alımı başarıyla tamamlanırsa tüm üç adım yeşil onay işaretleriyle işaretlenir. 
 
    ![bir tıklama veri alımı Tamam](media/ingest-data-one-click/one-click-data-ingestion-complete.png)

1. Sorguyu açmak için **V** 'yi seçin. Sorguyu düzenlemek için Web Kullanıcı arabirimine kopyalayın.

1. Sağdaki menü **hızlı sorguları** ve **araçları**içerir. 

    * **Hızlı sorgular** , örnek sorgularla Web Kullanıcı arabirimine bağlantılar içerir.
    * **Araçlar** , ilgili `.drop` komutu çalıştırarak sorunları gidermenize olanak sağlayan **bırakma komutlarıyla** Web Kullanıcı arabirimine bağlantı içerir.

    > [!TIP]
    > Veriler, komutlar kullanılarak `.drop` kaybolabilir. Bunları dikkatle kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini Web Kullanıcı arabirimindeki verileri sorgulama](web-query-data.md)
* [Kusto sorgu dilini kullanarak Azure Veri Gezgini için sorgular yazma](write-queries.md)
