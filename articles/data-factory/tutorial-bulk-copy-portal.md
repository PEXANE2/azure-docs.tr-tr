---
title: Azure portal kullanarak verileri toplu olarak kopyalama
description: Azure Data Factory ve Kopyalama Etkinliği’ni kullanarak bir kaynak veri deposundan hedef veri deposuna toplu veri kopyalama hakkında bilgi edinin.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 02/27/2020
ms.openlocfilehash: fd14945069b2786fa4acb994c37c17d3b434893e
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84118279"
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu öğreticide **, Azure SQL veritabanı 'Ndan Azure SYNAPSE Analytics 'e (eski ADıYLA SQL DW) bir dizi tablonun kopyalanması**gösterilmektedir. Aynı düzeni diğer kopyalama senaryolarında da uygulayabilirsiniz. Örneğin, tabloları SQL Server/Oracle 'dan Azure SQL veritabanı/Azure SYNAPSE Analytics (eski adıyla SQL DW)/Azure Blob 'a kopyalayarak blob 'tan Azure SQL veritabanı tablolarına farklı yollar kopyalarsınız.

> [!NOTE]
> - Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Yüksek düzeyde, bu öğretici aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SQL veritabanı, Azure SYNAPSE Analytics (eski adıyla SQL DW) ve Azure Storage bağlı hizmetleri oluşturun.
> * Azure SQL veritabanı ve Azure SYNAPSE Analytics (eski adıyla SQL DW) veri kümeleri oluşturun.
> * Kopyalanacak tabloları aramak için bir işlem hattı ve gerçek kopyalama işlemini gerçekleştirmek için başka bir işlem hattı oluşturun. 
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Bu öğreticide Azure portalı kullanılır. Veri fabrikası oluşturmaya yönelik diğer araçlar/SDK’lar hakkında bilgi edinmek için bkz. [Hızlı Başlangıçlar](quickstart-create-data-factory-dot-net.md). 

## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı
Bu senaryoda, Azure SQL veritabanı 'nda Azure SYNAPSE Analytics 'e (eski adıyla SQL DW) kopyalamak istediğiniz bir dizi tablonuz vardır. İş akışının işlem hatlarında gerçekleşen adımlarının mantıksal sırası şöyledir:

![İş akışı](media/tutorial-bulk-copy-portal/tutorial-copy-multiple-tables.png)

* İlk işlem hattı, havuz veri depolarına kopyalanması gereken tabloların listesini arar.  Alternatif olarak, havuz veri deposuna kopyalanacak tüm tabloları listeleyen bir meta veri tablosu tutabilirsiniz. İşlem hattı daha sonra veritabanındaki her bir tabloda yinelenen ve veri kopyalama işlemini gerçekleştiren başka bir işlem hattını tetikler.
* İkinci işlem hattı gerçek kopyalama işlemini gerçekleştirir. Tablo listesini bir parametre olarak alır. Listedeki her tablo için, Azure SQL veritabanı 'ndaki belirli bir tabloyu, en iyi performans için [BLOB depolama ve PolyBase aracılığıyla hazırlanan kopyayı](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) kullanarak Azure SYNAPSE Analytics 'te (eskı ADıYLA SQL DW) ilgili tabloya kopyalayın. Bu örnekte, ilk işlem hattı tablo listesini bir parametre değeri olarak geçirir. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Ön koşullar
* **Azure depolama hesabı**. Azure Depolama hesabı, toplu kopyalama işleminde hazırlama blob depolama alanı olarak kullanılır. 
* **Azure SQL veritabanı**. Bu veritabanı, kaynak verileri içerir. 
* **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)**. Bu veri ambarı, SQL Veritabanından kopyalanan verileri tutar. 

### <a name="prepare-sql-database-and-azure-synapse-analytics-formerly-sql-dw"></a>SQL veritabanı ve Azure SYNAPSE Analytics (eski adıyla SQL DW) hazırlama

**Kaynak Azure SQL Veritabanı’nı hazırlama**:

[Azure SQL veritabanı oluşturma](../azure-sql/database/single-database-create-quickstart.md) makalesini izleyerek Adventure Works LT örnek verileriyle bir Azure SQL Veritabanı oluşturun. Bu öğretici bu örnek veritabanındaki tüm tabloları bir Azure SYNAPSE Analytics 'e (eski adıyla SQL DW) kopyalar.

**Havuz Azure SYNAPSE Analytics (eski ADıYLA SQL DW) hazırlama**:

1. Bir Azure SYNAPSE analizinin (eski adıyla SQL DW) yoksa, oluşturma adımları için [SQL veri ambarı oluşturma](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md) makalesine bakın.

1. Azure SYNAPSE Analytics 'te (eski adıyla SQL DW) ilgili tablo şemaları oluşturun. Daha sonraki bir adımda verileri geçirmek/kopyalamak için Azure Data Factory’yi kullanın.

## <a name="azure-services-to-access-sql-server"></a>SQL sunucusuna erişime yönelik Azure hizmetleri

Hem SQL veritabanı hem de Azure SYNAPSE Analytics (eski adıyla SQL DW) için, Azure hizmetlerinin SQL Server 'a erişmesine izin verin. Sunucunuz için **Azure hizmetlerinin ve kaynaklarının bu sunucuya erişmesine Izin ver** **ayarının açık olduğundan** emin olun. Bu ayar Data Factory hizmetinin Azure SQL veritabanınızdaki verileri okumasını ve Azure SYNAPSE Analizinizden (eski adıyla SQL DW) veri yazmasına izin verir. 

Bu ayarı doğrulamak ve etkinleştirmek için, sunucunuza > güvenlik > güvenlik duvarları ve sanal ağlar ' a gidin > **Azure hizmetlerinin ve kaynaklarının bu sunucuya erişmesine Izin ver** ayarını **Açık**olarak ayarlayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. [Azure Portal](https://portal.azure.com)gidin. 
1. Azure Portal menüsünün solunda, **kaynak**  >  **Analizi**oluştur  >  **Data Factory**' u seçin. 
   ![“Yeni” bölmesinde Data Factory seçimi](./media/doc-common-process/new-azure-data-factory-menu.png)
1. **Yeni Veri Fabrikası** sayfasında **ad**için **ADFTutorialBulkCopyDF** girin. 
 
   Azure veri fabrikasının adı **genel olarak benzersiz**olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialBulkCopyDF). Data Factory yapıtlarını adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name "ADFTutorialBulkCopyDF" is not available`
1. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
1. **Kaynak grubu**için aşağıdaki adımlardan birini yapın:
     
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
     Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
1. **Sürüm** için **V2**'yi seçin.
1. Data factory için **konum** seçin. Data Factory'nin kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **Data Factory**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
1. **Oluştur**' a tıklayın.
1. Oluşturma işlemi tamamlandıktan sonra **Data Factory** sayfasına gitmek Için **Kaynağa Git** ' i seçin. 
   
1. Data Factory kullanıcı arabirimi uygulamasını ayrı bir sekmede açmak için **Author & Monitor** (Oluştur ve İzle) kutucuğuna tıklayın.
1. **Haydi** başlayın sayfasında, aşağıdaki görüntüde gösterildiği gibi sol paneldeki **Yazar** sekmesine geçin:

     ![Başlarken sayfası](./media/doc-common-process/get-started-page-author-button.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlemlerinizi veri fabrikasına bağlamak için bağlı hizmetler oluşturursunuz. Bağlı hizmetler, Data Factory hizmetinin çalışma zamanında veri deposuna bağlanmak için kullandığı bağlantı bilgilerini içerir. 

Bu öğreticide, Azure SQL veritabanı, Azure SYNAPSE Analytics (eski adıyla SQL DW) ve Azure Blob depolama veri depolarınızı veri fabrikanıza bağlarsınız. Azure SQL Veritabanı, kaynak veri deposudur. Azure SYNAPSE Analytics (eski adıyla SQL DW), havuz/hedef veri deposudur. Azure Blob depolama, verileri, PolyBase kullanılarak Azure SYNAPSE Analytics 'e (eski adıyla SQL DW) yüklenmeden önce verileri aşamalandırmaktır. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Kaynak Azure SQL Veritabanı bağlı hizmetini oluşturma
Bu adımda, Azure SQL veritabanınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. 

1. Pencerenin alt kısmındaki **Bağlantılar** ' a tıklayın ve araç çubuğunda **+ Yeni** ' ye tıklayın (**Bağlantılar** düğmesi, **fabrika kaynakları**altındaki sol sütunun altında bulunur). 

1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde **Azure SQL Veritabanı**’nı seçip **Devam**’a tıklayın. 
1. **Yeni bağlı hizmet (Azure SQL veritabanı)** penceresinde aşağıdaki adımları uygulayın: 

    a. **Ad** için **AzureSqlDatabaseLinkedService** adını girin.
    
    b. **Sunucu adı** için sunucunuzu seçin
    
    c. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
    
    d. Azure SQL veritabanına bağlanacak **kullanıcının adını** girin. 
    
    e. Kullanıcının **parolasını** girin. 

    f. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
  
    g. Bağlı hizmeti kaydetmek için **Oluştur** ' a tıklayın.


### <a name="create-the-sink-azure-synapse-analytics-formerly-sql-dw-linked-service"></a>Havuz Azure SYNAPSE Analytics (eski adıyla SQL DW) bağlantılı hizmetini oluşturma

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
1. **Yeni bağlı hizmet** penceresinde **Azure SYNAPSE Analytics (eskı adıyla SQL DW)** öğesini seçin ve **devam**' a tıklayın. 
1. **Yeni bağlı hizmet (Azure SYNAPSE Analytics (eski ADıYLA SQL DW))** penceresinde aşağıdaki adımları uygulayın: 
   
    a. **Ad** için **AzureSqlDWLinkedService** adını girin.
     
    b. **Sunucu adı** için sunucunuzu seçin
     
    c. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
     
    d. Azure SQL veritabanı 'na bağlanmak için **Kullanıcı adı** girin. 
     
    e. Kullanıcı için **parola** girin. 
     
    f. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
     
    g. **Oluştur**' a tıklayın.

### <a name="create-the-staging-azure-storage-linked-service"></a>Hazırlama Azure Depolama bağlı hizmetini oluşturma
Bu öğreticide Azure Blob depolamayı daha iyi bir kopyalama performansı için PolyBase’i etkinleştiren geçici bir hazırlama alanı olarak kullanırsınız.

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde **Azure Blob Depolama**’yı seçip **Devam**’a tıklayın. 
1. **Yeni bağlı hizmet (Azure Blob depolama)** penceresinde aşağıdaki adımları uygulayın: 

    a. **Ad** için **AzureStorageLinkedService** adını girin.                                                 
    b. **Depolama hesabı adı**Için **Azure depolama hesabınızı** seçin.
    
    c. **Oluştur**' a tıklayın.


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu öğreticide, verilerin depolandığı konumu belirten kaynak ve havuz veri kümelerini oluşturacaksınız. 

**Azuressqldatabasedataset** giriş veri kümesi, **Azuressqldatabaselinkedservice**öğesine başvurur. Bağlı hizmet, veritabanına bağlanmaya yönelik bağlantı dizesini belirtir. Veri kümesi, kaynak verileri içeren veritabanı ve tablonun adını belirtir. 

**Azuresqldwdataset** çıkış veri kümesi **Azuresqldwlinkedservice**öğesine başvurur. Bağlı hizmet, Azure SYNAPSE Analytics 'e (eski adıyla SQL DW) bağlanacak bağlantı dizesini belirtir. Veri kümesi, verilerin kopyalandığı hedef veritabanı ve tabloyu belirtir. 

Bu öğreticide, kaynak ve hedef SQL tabloları veri kümesi tanımında sabit kodlanmamıştır. Bunun yerine, ForEach etkinliği tablonun adını çalışma zamanında Kopyalama etkinliğine geçirir. 

### <a name="create-a-dataset-for-source-sql-database"></a>Kaynak SQL Veritabanı için veri kümesi oluşturma

1. Sol bölmedeki **+ (artı)** seçeneğine ve ardından **veri kümesi**' ne tıklayın. 

    ![Yeni veri kümesi menüsü](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
1. **Yeni veri kümesi** PENCERESINDE **Azure SQL veritabanı**' nı seçin ve ardından **devam**' a tıklayın. 
    
1. **Özellikleri ayarla** penceresinde, **ad**' ın altına **azuressqldatabasedataset**adını girin. **Bağlı hizmet**altında **Azuressqldatabaselinkedservice**' i seçin. Ardından **Tamam**'a tıklayın.

1. **Bağlantı** sekmesine geçin, **tablo**için herhangi bir tablo seçin. Bu tablo işlevsiz bir tablodur. İşlem hattını oluştururken kaynak veri kümesinde bir sorgu belirtirsiniz. Azure SQL veritabanından verileri ayıklamak için sorgu kullanılır. Alternatif olarak, **Düzenle** onay kutusunu tıklatabilir ve tablo adı olarak **dbo. dummyName** yazabilirsiniz. 
 

### <a name="create-a-dataset-for-sink-azure-synapse-analytics-formerly-sql-dw"></a>Havuz Azure SYNAPSE Analytics (eski adıyla SQL DW) için bir veri kümesi oluşturma

1. Sol bölmedeki **+ (artı)** düğmesine ve **Veri Kümesi**'ne tıklayın. 
1. **Yeni veri kümesi** penceresinde **Azure SYNAPSE Analytics (eskı adıyla SQL DW)** öğesini seçin ve ardından **devam**' a tıklayın.
1. **Özellikleri ayarla** penceresinde, **ad**' ın altına **Azuresqldwdataset**adını girin. **Bağlı hizmet**altında **Azuresqldwlinkedservice**öğesini seçin. Ardından **Tamam**'a tıklayın.
1. **Parametreler** sekmesine geçin, **+ Yeni**'ye tıklayın ve parametre adı olarak **DWTableName** girin. Bu adı sayfadan kopyalayıp yapıştırırsanız **Dwtablename**sonunda **boşluk karakteri** bulunmadığından emin olun.
1. **Bağlantı** sekmesine geçin, 

    a. **Tablo**için **Düzenle** seçeneğini işaretleyin. İlk tablo adı giriş kutusuna **dbo** girin. Sonra ikinci giriş kutusunu seçin ve aşağıdaki **dinamik Içerik Ekle** bağlantısına tıklayın. 

    ![Veri kümesi bağlantısı TableName](./media/tutorial-bulk-copy-portal/dataset-connection-tablename.png)

    b. **Dinamik Içerik Ekle** sayfasında, **Parametreler**altındaki **dwtablename** öğesine tıklayarak üstteki ifade metin kutusunu otomatik olarak doldurur `@dataset().DWTableName` ve ardından **son**' a tıklayın. Veri kümesinin **tableName** özelliği, **DWTableName** parametresine bağımsız değişken olarak geçirilen değere ayarlanır. ForEach etkinliği bir tablo listesi boyunca yinelenir ve birer birer Kopyalama etkinliğine geçirilir. 

    ![Veri kümesi parametresi derleyici](./media/tutorial-bulk-copy-portal/dataset-parameter-builder.png)
 
## <a name="create-pipelines"></a>İşlem hattı oluşturma
Bu öğreticide, iki işlem hattı oluşturursunuz: **IterateAndCopySQLTables** ve **GetTableListAndTriggerCopyData**. 

**Gettablelistandtriggercopydata** işlem hattı iki eylem gerçekleştirir:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için **IterateAndCopySQLTables** işlem hattını tetikler.

**Iterateandcopysqltables** işlem hattı, bir tablo listesini parametre olarak alır. Listedeki her tablo için, Azure SQL veritabanı 'ndaki tablodaki verileri, hazırlanan Copy ve PolyBase kullanarak Azure SYNAPSE Analytics 'e (eski adıyla SQL DW) kopyalar.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>IterateAndCopySQLTables işlem hattını oluşturma

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.

    ![Yeni işlem hattı menüsü](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. **Genel** sekmesinde ad için **IterateAndCopySQLTables** girişini belirtin. 

1. **Parametreler** sekmesine geçin ve aşağıdaki eylemleri gerçekleştirin: 

    a. **+ Yeni** öğesine tıklayın. 
    
    b. Parametre **adı**Için **tablelist** girin.
    
    c. **Tür** için **Dizi**'yi seçin.

1. **Etkinlikler** araç kutusunda **Yineleme ve Koşullar**’ı genişletin ve **ForEach** etkinliğini sürükleyerek işlem hattı tasarım yüzeyine bırakın. Ayrıca, **Etkinlikler** araç kutusunda etkinlikler için arama yapabilirsiniz. 

    a. En alttaki **Genel** sekmesinde, **Ad** olarak **IterateSQLTables** girin. 

    b. **Ayarlar** sekmesine geçin, **öğeler**için giriş kutusuna tıklayın, ardından aşağıdaki **dinamik içerik Ekle** bağlantısına tıklayın. 

    c. **Dinamik Içerik Ekle** sayfasında, **Sistem değişkenleri** ve **işlevler** bölümlerini daraltın, **Parametreler**altındaki **tablelist** öğesine tıklayarak üstteki ifade metin kutusunu otomatik olarak doldurur `@pipeline().parameter.tableList` . Ardından, **Son**'a tıklayın. 

    ![Foreach parametresi derleyici](./media/tutorial-bulk-copy-portal/for-each-parameter-builder.png)
    
    d. **Etkinlikler** sekmesine geçin, **foreach** etkinliğine bir alt etkinlik eklemek için **kurşun kalem simgesine** tıklayın.
    ![ForEach etkinlik Oluşturucusu](./media/tutorial-bulk-copy-portal/for-each-activity-builder.png)

1. **Etkinlikler** araç kutusunda **Taşı & aktar**' ı genişletin ve **veri kopyalama** etkinliğini ardışık düzen Tasarımcısı yüzeyine sürükleyin. En üstteki içerik haritası menüsüne dikkat edin. **Iterateandcopysqltable** , işlem hattı adıdır ve **ıteratesqltables** , ForEach etkinlik adıdır. Tasarımcı, etkinlik kapsamdadır. ForEach düzenleyicisinden işlem hattı düzenleyicisine geri dönmek için, içerik haritası menüsündeki bağlantıya tıklayabilirsiniz. 

    ![ForEach içinde kopyalama](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)

1. **Kaynak** sekmesine geçin ve aşağıdaki adımları izleyin:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    1. **Kullanım sorgusu**için **sorgu** seçeneğini belirleyin. 
    1. **Sorgu** giriş kutusuna tıklayın -> aşağıdaki **Dinamik içerik ekle**'yi seçin -> **Sorgu** için aşağıdaki ifadeyi girin -> **Son**'u seçin.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 


1. **Havuz** sekmesine geçin ve aşağıdaki adımları izleyin: 

    1. **Havuz Veri Kümesi** olarak **AzureSqlDWDataset** seçin.
    1. DWTableName parametresinin DEĞERI için giriş kutusuna tıklayın-> aşağıdan **dinamik Içerik Ekle** ' yi seçin, `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]` komut dosyası olarak ifade girin,-> **son**' u seçin.
    1. Copy yöntemi için **PolyBase**' i seçin. 
    1. **Varsayılan tür kullan** seçeneğini temizleyin. 
    1. **Betiği kopyala** giriş kutusuna tıklayın -> aşağıdaki **Dinamik içerik ekle**'yi seçin -> betik için aşağıdaki ifadeyi girin -> **Son**'u seçin. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Kopyalama havuz ayarları](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)
1. **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Hazırlamayı etkinleştir**onay kutusunu seçin.
    1. **Depo Hesabı Bağlı Hizmeti** için **AzureStorageLinkedService** hizmetini seçin.

1. İşlem hattı ayarlarını doğrulamak için üst işlem hattı araç çubuğunda **Doğrula**’ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>GetTableListAndTriggerCopyData işlem hattını oluşturma

Bu işlem hattı iki eylem yapar:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için "IterateAndCopySQLTables" işlem hattını tetikler.

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.
1. **Genel** sekmesinde, işlem hattının adını **Gettablelistandtriggercopydata**olarak değiştirin. 

1. **Etkinlikler** araç kutusunda **genel**' i genişletin ve **arama** etkinliğini, işlem hattı Tasarımcısı yüzeyine sürükleyip bırakın ve aşağıdaki adımları uygulayın:

    1. **Ad** olarak **LookupTableList** girin. 
    1. **Açıklama** olarak **Tablo listesini Azure SQL veritabanından al** girin.

1. **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    1. **Kullanım sorgusu**için **sorgu** ' yı seçin. 
    1. **Sorgu** için aşağıdaki SQL sorgusunu girin.

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    1. **Yalnızca ilk satır** alanının onay işaretini temizleyin.

        ![Arama etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
1. Etkinlik araç kutusu ' ndan işlem **hattı** Tasarımcısı yüzeyine sürükleyip bırakın ve adı **triggercopy**olarak ayarlayın.

1. **Arama** etkinliğini Işlem hattını **Yürüt** etkinliğine **bağlamak** için, arama etkinliğine eklenen **yeşil kutuyu** ardışık düzen yürütme etkinliğinin soluna sürükleyin.

    ![Arama ve İşlem Hattı Yürütme etkinliklerini bağlama](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)

1. İşlem **hattı yürütme** etkinliğinin **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Çağrılan işlem hattı** olarak **IterateAndCopySQLTables** seçin. 
    1. **Gelişmiş** bölümünü genişletin ve **tamamlandığında bekle**onay kutusunu temizleyin.
    1. **Parametreler** bölümünde **+ Yeni** öğesine tıklayın. 
    1. Parametre **adı**Için **tablelist** girin.
    1. DEĞER giriş kutusuna tıklayın -> altındaki **Dinamik içerik ekle**'yi seçin -> tablo adı değeri olarak `@activity('LookupTableList').output.value` girin -> **Son**' seçin. Sonuç listesini, arama etkinliğinden ikinci işlem hattına giriş olarak ayarlıyoruz. Sonuç listesinde yer alan tabloların içerdiği verilerin hedefe kopyalanması gerekir. 

        ![İşlem hattı yürütme etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)

1. İşlem hattını doğrulamak için araç çubuğundaki **Doğrula**'ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

1. Varlıkları (veri kümeleri, işlem hatları, vb.) Data Factory hizmetine yayımlamak için pencerenin üst kısmında **Tümünü Yayımla** ' ya tıklayın. Yayımlama başarılı olana kadar bekleyin. 

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme

1. İşlem hattı **Gettablelistandtriggercopydata**' a gidin, üst işlem hattı araç çubuğunda **tetikleyici Ekle** ' ye tıklayın ve sonra **Şimdi Tetikle**' ye tıklayın. 

1. İşlem **hattı çalıştırması** sayfasında çalıştırmayı onaylayın ve ardından **son**' u seçin.

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **İzleyici** sekmesine geçin. çözümünüzdeki her iki işlem hattı için de çalıştırmaları görene kadar **Yenile** 'ye tıklayın. **Başarılı oldu** durumunu görene kadar listeyi yenilemeye devam edin. 

1. **Gettablelistandtriggercopydata** işlem hattı ile ilişkili etkinlik çalıştırmalarını görüntülemek için işlem hattının işlem hattı adı bağlantısına tıklayın. Bu işlem hattı çalıştırması için iki etkinlik çalıştırması görüyor olmalısınız. 
    ![İşlem hattı çalıştırmasını izleme](./media/tutorial-bulk-copy-portal/monitor-pipeline.png)
1. **Arama** etkinliğinin çıkışını görüntülemek Için, **etkinlik adı** sütununun altındaki etkinliğin yanındaki **Çıkış** bağlantısına tıklayın. **Çıkış** penceresinin ekranı kaplamasını sağlamalı ve pencereyi geri yüklemelisiniz. Gözden geçirdikten sonra, **X** simgesine tıklayarak **Çıkış** penceresini kapatın.

    ```json
    {
        "count": 9,
        "value": [
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Customer"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Product"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductModelProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductCategory"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Address"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "CustomerAddress"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderDetail"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderHeader"
            }
        ],
        "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
        "effectiveIntegrationRuntimes": [
            {
                "name": "DefaultIntegrationRuntime",
                "type": "Managed",
                "location": "East US",
                "billedDuration": 0,
                "nodes": null
            }
        ]
    }
    ```    
1. İşlem **hattı çalıştırmaları** görünümüne geri dönmek için, içerik haritası menüsünün en üstündeki **tüm işlem hattı çalıştırmaları** bağlantısına tıklayın. İşlem hattının etkinlik çalıştırmalarını görüntülemek için **Iterateandcopysqltables** bağlantısına ( **ardışık düzen adı** sütunu altında) tıklayın. **Arama** etkinliği çıkışında her tablo Için bir **kopyalama** etkinliği çalıştırılmış olduğuna dikkat edin. 

1. Verilerin bu öğreticide kullandığınız hedef Azure SYNAPSE Analytics 'e (eski adıyla SQL DW) kopyalandığını doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SQL veritabanı, Azure SYNAPSE Analytics (eski adıyla SQL DW) ve Azure Storage bağlı hizmetleri oluşturun.
> * Azure SQL veritabanı ve Azure SYNAPSE Analytics (eski adıyla SQL DW) veri kümeleri oluşturun.
> * Kopyalanacak tabloları aramak için bir işlem hattı ve gerçek kopyalama işlemini gerçekleştirmek için başka bir işlem hattı oluşturma. 
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Kaynaktan hedefe verileri artımlı olarak koppyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:
> [!div class="nextstepaction"]
>[Verileri artımlı olarak kopyalama](tutorial-incremental-copy-portal.md)
