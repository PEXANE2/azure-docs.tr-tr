---
title: Dosyalardan veritabanına toplu kopyalama
description: Azure Veri Gölü Depolama Gen2'den Azure Synapse Analytics / Azure SQL Veritabanı'na kadar verileri toplu olarak kopyalamak için bir çözüm şablonu nasıl kullanacağınızı öğrenin.
services: data-factory
author: linda33wj
ms.author: jingwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/08/2020
ms.openlocfilehash: ae250c7d15801789ad22955845cfa535ed91f2c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75921149"
---
# <a name="bulk-copy-from-files-to-database"></a>Dosyalardan veritabanına toplu kopyalama

Bu makalede, verileri Azure Veri Gölü Depolama Gen2'den Azure Synapse Analytics / Azure SQL Veritabanı'na toplu olarak kopyalamak için kullanabileceğiniz bir çözüm şablonu açıklanmaktadır.

## <a name="about-this-solution-template"></a>Bu çözüm şablonu hakkında

Bu şablon, Azure Veri Gölü Depolama Gen2 kaynağından dosyaları alır. Daha sonra kaynaktaki her dosya üzerinde yinelemeler ve dosyayı hedef veri deposuna kopyalar. 

Şu anda bu şablon yalnızca **DelimitedText** biçiminde veri kopyalamayı destekler. Diğer veri biçimlerindeki dosyalar da kaynak veri deposundan alınabilir, ancak hedef veri deposuna kopyalanamaz.  

Şablon üç etkinlik içerir:
- **Meta veri** etkinliği alın Dosyaları Azure Veri Gölü Depolama Gen2'den alır ve sonraki *ForEach* etkinliğine aktarın.
- **ForEach** etkinliği Meta *veri* al etkinliğinden dosyaları alır ve her dosyayı *Kopyalama* etkinliğine yineler.
- **Kopyalama** etkinliği, her dosyayı kaynak veri deposundan hedef veri deposuna kopyalamak için *ForEach* etkinliğinde bulunur.

Şablon aşağıdaki iki parametreyi tanımlar:
- *SourceContainer,* Azure Veri Gölü Depolama Gen2'nizde verilerin kopyalandığı kök kapsayıcı yoludur. 
- *SourceDirectory,* Verilerin Azure Veri Gölü Depolama Gen2'nizde kopyalandığı kök kapsayıcısının altındaki dizin yoludur.

## <a name="how-to-use-this-solution-template"></a>Bu çözüm şablonu nasıl kullanılır?

1. **Dosyalardan Veritabanı şablonuna Toplu Kopya'ya** gidin. Kaynak Gen2 deposuna **yeni** bir bağlantı oluşturun. "GetMetadataDataset" ve "SourceDataset"in kaynak dosya deponuzun aynı bağlantısına yapılan atıflar olduğunu unutmayın.

    ![Kaynak veri deposuna yeni bir bağlantı oluşturma](media/solution-template-bulk-copy-from-files-to-database/source-connection.png)

2. Verileri kopyaladığınız lavabo veri deposuna **yeni** bir bağlantı oluşturun.

    ![Lavabo veri deposuna yeni bir bağlantı oluşturma](media/solution-template-bulk-copy-from-files-to-database/destination-connection.png)
    
3. **Bu şablonu kullan'ı**seçin.

    ![Bu şablonu kullan](media/solution-template-bulk-copy-from-files-to-database/use-template.png)
    
4. Aşağıdaki örnekte gösterildiği gibi oluşturulan bir ardışık kaynak görürsünüz:

    ![Boru hattını gözden geçirin](media/solution-template-bulk-copy-from-files-to-database/new-pipeline.png)

    > [!NOTE]
    > Yukarıda belirtilen **adım 2'deki** veri hedefi olarak **Azure Synapse Analytics'i (eski adıyla SQL DW)** seçtiyseniz, SQL Data Warehouse Polybase'in gerektirdiği şekilde evreleme için Azure Blob depolama alanına bağlantı girmeniz gerekir. Aşağıdaki ekran görüntüsünün gösterdiği gibi, şablon blob depolama alanınız için otomatik olarak bir *Depolama Yolu* oluşturur. Kapsayıcının boru hattı çalıştırıldıktan sonra oluşturulıp oluşturulmamasını denetleyin.
        
    ![Polybase ayarı](media/solution-template-bulk-copy-from-files-to-database/staging-account.png)

5. **Hata Ayıklama'yı**seçin, **Parametreleri**girin ve ardından **Bitir'i**seçin.

    ![**Hata Ayıklama** seçeneğini tıklatın](media/solution-template-bulk-copy-from-files-to-database/debug-run.png)

6. Ardışık hatlar çalışması başarıyla tamamlandığında, aşağıdaki örneğe benzer sonuçlar görürsünüz:

    ![Sonucu gözden geçirin](media/solution-template-bulk-copy-from-files-to-database/run-succeeded.png)

       
## <a name="next-steps"></a>Sonraki adımlar

- [Azure Data Factory'ye giriş](introduction.md)