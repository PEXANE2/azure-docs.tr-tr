---
title: Microsoft Excel için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin
description: Bu makalede, Azure Veri Gezgini için Excel bağlayıcısını nasıl kullanacağınızı öğreneceksiniz.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 08/30/2019
ms.openlocfilehash: 42f52581d8f2f80deb5d6250ed54ab64fc1ba4d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74849063"
---
# <a name="visualize-data-using-the-azure-data-explorer-connector-for-excel"></a>Excel için Azure Veri Gezgini bağlayıcısını kullanarak verileri görselleştirin

Azure Veri Gezgini, Excel'deki verilere bağlanmak için iki seçenek sunar: yerel bağlayıcıyı kullanın veya Azure Veri Gezgini'nden bir sorgu içe aktarma. Bu makalede, Excel'deki yerel bağlayıcıyı nasıl kullanacağınızı ve verileri almak ve görselleştirmek için Azure Veri Gezgini kümesine nasıl bağlanabileceğinizi gösterir.

Azure Veri Gezgini Excel yerel bağlayıcısı, sorgu sonuçlarını Excel'e dışa aktarma olanağı sunar. Ek hesaplamalar veya görselleştirmeler için Excel veri kaynağı olarak bir KQL sorgusu da ekleyebilirsiniz.

## <a name="define-kusto-query-as-an-excel-data-source-and-load-the-data-to-excel"></a>Kusto sorgusunu Excel veri kaynağı olarak tanımlayın ve verileri Excel'e yükleyin

1. **Microsoft Excel'i**açın.
1. **Veri** sekmesinde, Azure > **Veri Gezgini'nden**Veri**From Azure** >  **Al'ı**seçin.

    ![Azure Veri Gezgini'nden veri alma](media/excel-connector/get-data-from-adx.png)

1. Azure **Veri Gezgini (Kusto)** penceresinde, aşağıdaki alanları tamamlayın ve **Tamam'ı**seçin.

    ![Azure Veri Gezgini (Kusto) penceresi](media/excel-connector/adx-connection-window.png)
    
    |Alan   |Açıklama |
    |---------|---------|
    |**Küme**   |   Kümeadı (zorunlu)      |    
    |**Database**     |    Veritabanının adı      |    
    |**Tablo adı veya Azure Veri Gezgini sorgusu**    |     Tablonun adı veya Azure Veri Gezgini sorgusu    | 
    
    **Gelişmiş Seçenekler:**

     |Alan   |Açıklama |
    |---------|---------|
    |**Sorgu sonuç kayıt numarasını sınırla**     |     Excel'e yüklenen kayıt sayısını sınırlama  |    
    |**Sorgu sonuç veri boyutunu sınırlama (bayt)**    |    Veri boyutunu sınırlama      |   
    |**Sonuç kümesini devre dışı bırak**    |         |      
    |**Ek Set deyimleri (yarı iki nokta nokta ile ayrılmış)**    |    Veri `set` kaynağına uygulanacak ifadeler ekleme     |   

1.  **Gezgin** bölmesinde, doğru tabloya gidin. Tablo önizleme bölmesinde, verilerinizde değişiklik yapmak için **Verileri Dönüştür'i** veya Excel'e yüklemek için **Yükle'yi** seçin.

![Tablo önizleme penceresi](media/excel-connector/navigate-table-preview-window.png)

   > [!TIP]
   > **Veritabanı** ve/veya **Tablo adı veya Azure Veri Gezgini sorgusu** zaten belirtilmişse, doğru tablo önizleme bölmesi otomatik olarak açılır. 

## <a name="analyze-and-visualize-data-in-excel"></a>Excel'de verileri analiz edin ve görselleştirin

Veriler excel'e yüklendikten ve Excel sayfanızda kullanılabilir hale geldikten sonra, ilişkiler ve görseller oluşturarak verileri analiz edebilir, özetleyebilir ve görselleştirebilirsiniz. 

1.  Tablo **Tasarımı** sekmesinde **PivotTable ile Özetle'yi**seçin. **PivotTable Oluştur** penceresinde, ilgili tabloyu seçin ve **Tamam'ı**seçin.

    ![Pivot tablosu oluşturma](media/excel-connector/create-pivot-table.png)

1. **PivotTable Alanları** bölmesinde, özet tablolar oluşturmak için ilgili tablo sütunlarını seçin. Aşağıdaki örnekte **EventId** ve **Durum** seçili.
    
    ![PivotTable alanlarını seçin](media/excel-connector/pivot-table-pick-fields.png)

1. **PivotTable Analyze** sekmesinde, tabloya dayalı görseller oluşturmak için **PivotChart'ı** seçin. 

    ![Özet grafik](media/excel-connector/pivot-table-analyze-pivotchart.png)

1. Aşağıdaki örnekte, hava durumu olayları hakkında ek bilgileri görüntülemek için **Olay Kimliği**, **Başlangıç Saati**ve **EventType'ı** kullanın.

    ![Verileri görselleştirme](media/excel-connector/visualize-excel-data.png)

1. Verilerinizi izlemek için tam panolar oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

[Microsoft Excel'e aktarılan Azure Veri Gezgini Kusto sorgusukullanarak verileri görselleştirin](excel-blank-query.md)