---
title: Azure portalLarını kullanarak verileri toplu olarak kopyalama
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
ms.openlocfilehash: 04469fa1bd0473710d9fa0bf0190c6459f1f8a07
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418788"
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Azure Data Factory kullanarak birden çok tabloyu toplu olarak kopyalama

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu öğretici, **Azure SQL Veritabanı'ndan Azure Synapse Analytics'e (eski adıyla SQL DW) birkaç tablo kopyalamayı**gösterir. Aynı düzeni diğer kopyalama senaryolarında da uygulayabilirsiniz. Örneğin, SQL Server/Oracle'dan Azure SQL Veritabanı/Azure Synapse Analytics'e (eski adıyla SQL DW)/Azure Blob'a tablo kopyalamak, Blob'dan Azure SQL Veritabanı tablolarına farklı yollar kopyalamak.

> [!NOTE]
> - Azure Data Factory kullanmaya yeni başlıyorsanız bkz. [Azure Data Factory'ye giriş](introduction.md).

Yüksek düzeyde, bu öğretici aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SQL Veritabanı, Azure Synapse Analytics (eski adıyla SQL DW) ve Azure Depolama bağlantılı hizmetler oluşturun.
> * Azure SQL Veritabanı ve Azure Synapse Analytics (eski adıyla SQL DW) veri kümeleri oluşturun.
> * Kopyalanacak tabloları aramak için bir işlem hattı ve gerçek kopyalama işlemini gerçekleştirmek için başka bir işlem hattı oluşturma. 
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Bu öğreticide Azure portalı kullanılır. Veri fabrikası oluşturmaya yönelik diğer araçlar/SDK’lar hakkında bilgi edinmek için bkz. [Hızlı Başlangıçlar](quickstart-create-data-factory-dot-net.md). 

## <a name="end-to-end-workflow"></a>Uçtan uca iş akışı
Bu senaryoda, Azure SQL Veritabanı'nda Azure Synapse Analytics'e (eski adıyla SQL DW) kopyalamak istediğiniz birkaç tablo vardır. İş akışının işlem hatlarında gerçekleşen adımlarının mantıksal sırası şöyledir:

![İş akışı](media/tutorial-bulk-copy-portal/tutorial-copy-multiple-tables.png)

* İlk işlem hattı, havuz veri depolarına kopyalanması gereken tabloların listesini arar.  Alternatif olarak, havuz veri deposuna kopyalanacak tüm tabloları listeleyen bir meta veri tablosu tutabilirsiniz. İşlem hattı daha sonra veritabanındaki her bir tabloda yinelenen ve veri kopyalama işlemini gerçekleştiren başka bir işlem hattını tetikler.
* İkinci işlem hattı gerçek kopyalama işlemini gerçekleştirir. Tablo listesini bir parametre olarak alır. Listedeki her tablo için, en iyi performans için [Blob depolama ve PolyBase aracılığıyla aşamalı kopya](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) kullanarak Azure SQL Veritabanı'ndaki belirli tabloyu Azure Synapse Analytics'teki (eski adıyla SQL DW) ilgili tabloya kopyalayın. Bu örnekte, ilk işlem hattı tablo listesini bir parametre değeri olarak geçirir. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar
* **Azure Depolama hesabı.** Azure Depolama hesabı, toplu kopyalama işleminde hazırlama blob depolama alanı olarak kullanılır. 
* **Azure SQL Veritabanı**. Bu veritabanı, kaynak verileri içerir. 
* **Azure Synapse Analytics (eski adıyla SQL DW)**. Bu veri ambarı, SQL Veritabanından kopyalanan verileri tutar. 

### <a name="prepare-sql-database-and-azure-synapse-analytics-formerly-sql-dw"></a>SQL Veritabanı ve Azure Synapse Analytics (eski adıyla SQL DW) hazırlayın

**Kaynak Azure SQL Veritabanı’nı hazırlama**:

[Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) makalesini izleyerek Adventure Works LT örnek verileriyle bir Azure SQL Veritabanı oluşturun. Bu öğretici, bu örnek veritabanındaki tüm tabloları Azure Synapse Analytics 'e (eski adıyla SQL DW) kopyalar.

**Lavabo Azure Synapse Analytics (eski adıyla SQL DW) hazırlayın:**

1. Azure Synapse Analytics (eski adıyla SQL DW) yoksa, oluşturmak için adımlar için [BIR SQL Veri Ambarı Oluştur](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md) makalesine bakın.

1. Azure Synapse Analytics'te (eski adıyla SQL DW) ilgili tablo şemaları oluşturun. Daha sonraki bir adımda verileri geçirmek/kopyalamak için Azure Data Factory’yi kullanın.

## <a name="azure-services-to-access-sql-server"></a>SQL sunucusuna erişime yönelik Azure hizmetleri

Hem SQL Veritabanı hem de Azure Synapse Analytics (eski adıyla SQL DW) için Azure hizmetlerinin SQL sunucusuna erişmesine izin verin. Azure SQL sunucunuz için **Azure hizmetlerinin ve kaynaklarının bu sunucu ayarına erişmesine izin** ver'in **açık** olduğundan emin olun. Bu ayar, Veri Fabrikası hizmetinin Azure SQL Veritabanınızdaki verileri okumasına ve Azure Synapse Analytics'inize (eski adıyla SQL DW) veri yazmasına olanak tanır. 

Bu ayarı doğrulamak ve açmak için, Azure > Güvenlik Duvarları ve sanal ağları > Azure SQL sunucunuza gidin > **Azure hizmetlerine ve kaynaklarına erişmesine izin** verin'in bu **sunucuyu A'ya**erişmesine izin verin.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. [Azure portalına](https://portal.azure.com)gidin. 
1. Azure portalı menüsünün solunda, bir > kaynak**Analitik** > **Veri Fabrikası** **Oluştur'u**seçin. 
   ![“Yeni” bölmesinde Data Factory seçimi](./media/doc-common-process/new-azure-data-factory-menu.png)
1. Yeni **veri fabrikası** sayfasında, **ad**için **ADFTutorialBulkCopyDF** girin. 
 
   Azure veri fabrikasının adı **genel olarak benzersiz**olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialBulkCopyDF). Data Factory yapıtlarını adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name "ADFTutorialBulkCopyDF" is not available`
1. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
1. Kaynak **Grubu**için aşağıdaki adımlardan birini yapın:
     
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
     Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).  
1. **Sürüm** için **V2**'yi seçin.
1. Data factory için **konum** seçin. Data Factory'nin kullanılabileceği Azure bölgelerinin bir listesi için bir sonraki sayfada ilgilendiğiniz bölgeleri seçin ve **Analytics**'i genişleterek **Data Factory**: [Products available by region](https://azure.microsoft.com/global-infrastructure/services/) (Bölgeye göre kullanılabilir durumdaki ürünler) bölümünü bulun. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
1. **Oluştur'u**tıklatın.
1. Oluşturma tamamlandıktan sonra, **Veri Fabrikası** sayfasına gitmek **için kaynağa** git'i seçin. 
   
1. Data Factory kullanıcı arabirimi uygulamasını ayrı bir sekmede açmak için **Author & Monitor** (Oluştur ve İzle) kutucuğuna tıklayın.
1. **Başlatalım** sayfasında, aşağıdaki resimde gösterildiği gibi sol paneldeki **Yazar** sekmesine geçin:

     ![Başlarken sayfası](./media/doc-common-process/get-started-page-author-button.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlemlerinizi veri fabrikasına bağlamak için bağlı hizmetler oluşturursunuz. Bağlı hizmetler, Data Factory hizmetinin çalışma zamanında veri deposuna bağlanmak için kullandığı bağlantı bilgilerini içerir. 

Bu eğitimde, Azure SQL Veritabanınızı, Azure Synapse Analytics'inizi (eski adıyla SQL DW) ve Azure Blob Depolama veri depolarınızı veri fabrikanıza bağlarsınız. Azure SQL Veritabanı, kaynak veri deposudur. Azure Synapse Analytics (eski adıyla SQL DW), lavabo/hedef veri deposudur. Azure Blob Depolama, veriler PolyBase kullanarak Verileri Azure Synapse Analytics'e (eski adıyla SQL DW) yüklenmeden önce aşamalı olarak düzenlemektir. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Kaynak Azure SQL Veritabanı bağlı hizmetini oluşturma
Bu adımda, Azure SQL veritabanınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. 

1. Pencerenin altındaki **Bağlantılar'a** tıklayın ve araç çubuğunda **+ Yeni'yi** tıklatın (**Bağlantılar** düğmesi **Fabrika Kaynakları'nın**altındaki sol sütunun alt kısmında yer alır). 

1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde **Azure SQL Veritabanı**’nı seçip **Devam**’a tıklayın. 
1. Yeni **Bağlantılı Hizmet (Azure SQL Veritabanı)** penceresinde aşağıdaki adımları yapın: 

    a. **Ad** için **AzureSqlDatabaseLinkedService** adını girin.
    
    b. **Sunucu adı** için Azure SQL sunucunuzu seçin
    
    c. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
    
    d. Azure SQL veritabanına bağlanacak **kullanıcının adını** girin. 
    
    e. Kullanıcının **parolasını** girin. 

    f. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
  
    g. Bağlantılı hizmeti kaydetmek için **Oluştur'u** tıklatın.


### <a name="create-the-sink-azure-synapse-analytics-formerly-sql-dw-linked-service"></a>Azure Synapse Analytics (eski adıyla SQL DW) bağlantılı hizmeti oluşturun

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
1. Yeni **Bağlantılı Hizmet** penceresinde **Azure Synapse Analytics'i (eski adıyla SQL DW)** seçin ve **Devam et'i**tıklatın. 
1. Yeni **Bağlantılı Hizmet (Azure Synapse Analytics (eski adıyla SQL DW))** penceresinde aşağıdaki adımları yapın: 
   
    a. **Ad** için **AzureSqlDWLinkedService** adını girin.
     
    b. **Sunucu adı** için Azure SQL sunucunuzu seçin
     
    c. **Veritabanı adı** için Azure SQL veritabanınızı seçin. 
     
    d. Azure SQL veritabanına bağlanmak için **Kullanıcı adını** girin. 
     
    e. Kullanıcı için **Parola** girin. 
     
    f. Belirtilen bilgileri kullanarak Azure SQL veritabanına bağlantıyı test etmek için, **Bağlantıyı sına**'ya tıklayın.
     
    g. **Oluştur'u**tıklatın.

### <a name="create-the-staging-azure-storage-linked-service"></a>Hazırlama Azure Depolama bağlı hizmetini oluşturma
Bu öğreticide Azure Blob depolamayı daha iyi bir kopyalama performansı için PolyBase’i etkinleştiren geçici bir hazırlama alanı olarak kullanırsınız.

1. **Bağlantılar** sekmesinde, araç çubuğundaki **+ Yeni** düğmesine tekrar tıklayın. 
1. **New Linked Service** (Yeni Bağlı Hizmet) penceresinde **Azure Blob Depolama**’yı seçip **Devam**’a tıklayın. 
1. Yeni **Bağlantılı Hizmet (Azure Blob Depolama)** penceresinde aşağıdaki adımları yapın: 

    a. **Ad** için **AzureStorageLinkedService** adını girin.                                                 
    b. **Depolama hesabı adı**için Azure Depolama **hesabınızı** seçin.
    
    c. **Oluştur'u**tıklatın.


## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu öğreticide, verilerin depolandığı konumu belirten kaynak ve havuz veri kümelerini oluşturacaksınız. 

Giriş veri kümesi **AzureSqlDatabaseDataset,** **AzureSqlDatabaseLinkedService'e**başvurur. Bağlı hizmet, veritabanına bağlanmaya yönelik bağlantı dizesini belirtir. Veri kümesi, kaynak verileri içeren veritabanı ve tablonun adını belirtir. 

Çıktı veri kümesi **AzureSqlDWDataset,** **AzureSqlDWLinkedService'e**başvurur. Bağlantılı hizmet, Azure Synapse Analytics'e (eski adıyla SQL DW) bağlanmak için bağlantı dizesini belirtir. Veri kümesi, verilerin kopyalandığı hedef veritabanı ve tabloyu belirtir. 

Bu öğreticide, kaynak ve hedef SQL tabloları veri kümesi tanımında sabit kodlanmamıştır. Bunun yerine, ForEach etkinliği tablonun adını çalışma zamanında Kopyalama etkinliğine geçirir. 

### <a name="create-a-dataset-for-source-sql-database"></a>Kaynak SQL Veritabanı için veri kümesi oluşturma

1. Sol bölmede **+ (artı)** seçeneğini tıklatın ve ardından **Dataset'i**tıklatın. 

    ![Yeni veri kümesi menüsü](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
1. Yeni **Veri Kümesi** penceresinde Azure **SQL Veritabanı'nı**seçin ve ardından **Devam et'i**tıklatın. 
    
1. Set **özellikleri** penceresinde, **Ad**altında, **AzureSqlDatabaseDataset**girin. **Bağlantılı hizmet**altında **AzureSqlDatabaseLinkedService'i**seçin. Ardından **Tamam**'a tıklayın.

1. **Bağlantı** sekmesine geçin, **Tablo**için herhangi bir tablo seçin. Bu tablo işlevsiz bir tablodur. İşlem hattını oluştururken kaynak veri kümesinde bir sorgu belirtirsiniz. Azure SQL veritabanından verileri ayıklamak için sorgu kullanılır. Alternatif olarak, **Yap onay** kutusunu tıklatabilir ve tablo adı olarak **dbo.dummyName'yi girebilirsiniz.** 
 

### <a name="create-a-dataset-for-sink-azure-synapse-analytics-formerly-sql-dw"></a>Azure Synapse Analytics (eski adıyla SQL DW) için bir veri kümesi oluşturma

1. Sol bölmedeki **+ (artı)** düğmesine ve **Veri Kümesi**'ne tıklayın. 
1. Yeni **Veri Kümesi** penceresinde Azure **Synapse Analytics'i (eski adıyla SQL DW)** seçin ve ardından **Devam et'i**tıklatın.
1. Set **özellikleri** penceresinde, **Ad**altında, **AzureSqlDWDataset**girin. **Bağlantılı hizmet**altında **AzureSqlDWLinkedService'i**seçin. Ardından **Tamam**'a tıklayın.
1. **Parametreler** sekmesine geçin, **+ Yeni**'ye tıklayın ve parametre adı olarak **DWTableName** girin. Bu adı sayfadan kopyalar/yapıştırın, **DWTableName'nin**sonunda **uzama alanı karakteri** olmadığından emin olun.
1. **Bağlantı** sekmesine geçin, 

    a. **Tablo** **için, Edit** seçeneğini işaretleyin. İlk tablo adı giriş kutusuna **dbo** girin. Ardından ikinci giriş kutusuna tıklayın ve aşağıdaki **dinamik içerik** ekle bağlantısını tıklatın. 

    ![Dataset bağlantı tablo adı](./media/tutorial-bulk-copy-portal/dataset-connection-tablename.png)

    b. Dinamik **İçerik Ekle** sayfasında, **üstteki**ifade metin `@dataset().DWTableName`kutusunu otomatik olarak dolduracak olan Parametreler altındaki **DWTAbleName'i** tıklatın ve **ardından Bitir'i**tıklatın. Veri kümesinin **tableName** özelliği, **DWTableName** parametresine bağımsız değişken olarak geçirilen değere ayarlanır. ForEach etkinliği bir tablo listesi boyunca yinelenir ve birer birer Kopyalama etkinliğine geçirilir. 

    ![Veri kümesi parametresi derleyici](./media/tutorial-bulk-copy-portal/dataset-parameter-builder.png)
 
## <a name="create-pipelines"></a>İşlem hattı oluşturma
Bu öğreticide, iki işlem hattı oluşturursunuz: **IterateAndCopySQLTables** ve **GetTableListAndTriggerCopyData**. 

**GetTableListAndTriggerCopyData** ardışık iki eylem gerçekleştirir:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için **IterateAndCopySQLTables** işlem hattını tetikler.

**IterateAndCopySQLTables** ardışık bir parametre olarak tabloların bir listesini alır. Listedeki her tablo için, Azure SQL Veritabanı'ndaki tablodaki verileri aşamalı kopya ve PolyBase kullanarak Azure Synapse Analytics'e (eski adıyla SQL DW) kopyalar.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>IterateAndCopySQLTables işlem hattını oluşturma

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.

    ![Yeni işlem hattı menüsü](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. **Genel** sekmesinde ad için **IterateAndCopySQLTables** girişini belirtin. 

1. **Parametreler** sekmesine geçin ve aşağıdaki eylemleri gerçekleştirin: 

    a. **+ Yeni** öğesine tıklayın. 
    
    b. Parametre **Adı**için **tabloListesi'ni** girin.
    
    c. **Tür** için **Dizi**'yi seçin.

1. **Etkinlikler** araç kutusunda **Yineleme ve Koşullar**’ı genişletin ve **ForEach** etkinliğini sürükleyerek işlem hattı tasarım yüzeyine bırakın. Ayrıca, **Etkinlikler** araç kutusunda etkinlikler için arama yapabilirsiniz. 

    a. En alttaki **Genel** sekmesinde, **Ad** olarak **IterateSQLTables** girin. 

    b. **Ayarlar** sekmesine geçin, **Öğeler**için giriş kutusunu tıklatın ve ardından aşağıdaki **dinamik içerik** ekle bağlantısını tıklatın. 

    c. Dinamik **İçerik Ekle** sayfasında, **Sistem Değişkenleri** ve **İşlevler** bölümlerini daraltma, üst teki ifade metin `@pipeline().parameter.tableList`kutusunu otomatik olarak ' olarak dolduracak olan **Parametreler**altındaki **tableList'i** tıklatın. Ardından, **Son**'a tıklayın. 

    ![Foreach parametresi derleyici](./media/tutorial-bulk-copy-portal/for-each-parameter-builder.png)
    
    d. **Etkinlikler** sekmesine geçin, **ForEach** etkinliğine bir alt etkinlik eklemek için **kalem simgesini** tıklatın.
    ![Foreach aktivite oluşturucu](./media/tutorial-bulk-copy-portal/for-each-activity-builder.png)

1. **Etkinlikler** araç kutusunda, **Taşı & Aktar'ı**genişletin ve **veri** etkinliğini boru hattı tasarımcısı yüzeyine sürükleyin. En üstteki içerik haritası menüsüne dikkat edin. **IterateAndCopySQLTable** ardışık hatlar adıdır ve **IterateSQLTables** ForEach etkinlik adıdır. Tasarımcı, etkinlik kapsamdadır. ForEach düzenleyicisinden boru hattı düzenleyicisine geri dönmek için kırıntı menüsündeki bağlantıyı tıklatabilirsiniz. 

    ![ForEach içinde kopyalama](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)

1. **Kaynak** sekmesine geçin ve aşağıdaki adımları izleyin:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    1. Sorgu kullanımı için **Sorgula** seçeneğini seçin. **Use query** 
    1. **Sorgu** giriş kutusuna tıklayın -> aşağıdaki **Dinamik içerik ekle**'yi seçin -> **Sorgu** için aşağıdaki ifadeyi girin -> **Son**'u seçin.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 


1. **Havuz** sekmesine geçin ve aşağıdaki adımları izleyin: 

    1. **Havuz Veri Kümesi** olarak **AzureSqlDWDataset** seçin.
    1. DWTableName parametresi -> aşağıdaki **dinamik içerik ekle'yi** seçin, `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]` ifadeyi komut dosyası olarak girin, -> **Bitiş'i**seçin.
    1. Kopyalama yöntemi için **PolyBase'i**seçin. 
    1. Kullanım **türü varsayılan** seçeneğini temizleyin. 
    1. **Betiği kopyala** giriş kutusuna tıklayın -> aşağıdaki **Dinamik içerik ekle**'yi seçin -> betik için aşağıdaki ifadeyi girin -> **Son**'u seçin. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Kopyalama havuz ayarları](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)
1. **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Hazırlamayı Etkinleştir**için onay kutusunu seçin.
    1. **Depo Hesabı Bağlı Hizmeti** için **AzureStorageLinkedService** hizmetini seçin.

1. İşlem hattı ayarlarını doğrulamak için üst işlem hattı araç çubuğunda **Doğrula**’ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>GetTableListAndTriggerCopyData işlem hattını oluşturma

Bu ardışık işlem iki eylem yapar:

* Kopyalanacak tabloların listesini almak için Azure SQL Veritabanı sistem tablosunu arar.
* Gerçek veri kopyalamayı yapmak için "IterateAndCopySQLTables" işlem hattını tetikler.

1. Sol bölmede, **+ (artı)** düğmesine ve sonra da **İşlem Hattı**’na tıklayın.
1. **Genel** sekmesinde, ardışık hattın adını **GetTableListAndTriggerCopyData olarak değiştirin.** 

1. **Etkinlikler** araç kutusunda, **Genel'i**genişletin ve **aramayı** boru hattı tasarımcısı yüzeyine sürükleyin ve aşağıdaki adımları yapın:

    1. **Ad** olarak **LookupTableList** girin. 
    1. **Açıklama** olarak **Tablo listesini Azure SQL veritabanından al** girin.

1. **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın:

    1. **Kaynak Veri Kümesi** olarak **AzureSqlDatabaseDataset** seçin. 
    1. **Kullanım**sorgusu için **Sorgu'yı** seçin. 
    1. **Sorgu** için aşağıdaki SQL sorgusunu girin.

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    1. **Yalnızca ilk satır** alanının onay işaretini temizleyin.

        ![Arama etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
1. Etkinlikler araç kutusundan boru hattı tasarımcısı yüzeyine sürükle-bırak **Yürütme Pipeline** etkinliği ve **TriggerCopy**adını ayarlayın.

1. **Arama** etkinliğini **Yürüt İşleme Yürüt** etkinliğine **bağlamak** için, Arama Gerçekleştirme Hattı etkinliğine bağlı **yeşil kutuyu** Yürüt İşleme Alanı etkinliğinin soluna sürükleyin.

    ![Arama ve İşlem Hattı Yürütme etkinliklerini bağlama](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)

1. **Yürüt İşleme Hattı** etkinliğinin **Ayarlar** sekmesine geçin ve aşağıdaki adımları yapın: 

    1. **Çağrılan işlem hattı** olarak **IterateAndCopySQLTables** seçin. 
    1. **Gelişmiş** bölümü genişletin ve **tamamlanmak üzere Bekleyin**onay kutusunu temizleyin.
    1. **Parametreler** bölümünde **+ Yeni** öğesine tıklayın. 
    1. Parametre **Adı**için **tabloListesi'ni** girin.
    1. DEĞER giriş kutusuna tıklayın -> altındaki **Dinamik içerik ekle**'yi seçin -> tablo adı değeri olarak `@activity('LookupTableList').output.value` girin -> **Son**' seçin. İkinci ardışık ardışık ardışık bölüme giriş olarak Arama etkinliğinden gelen sonuç listesini ayarlıyorsunuz. Sonuç listesinde yer alan tabloların içerdiği verilerin hedefe kopyalanması gerekir. 

        ![İşlem hattı yürütme etkinliği - ayarlar sayfası](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)

1. İşlem hattını doğrulamak için araç çubuğundaki **Doğrula**'ya tıklayın. Doğrulama hatası olmadığından emin olun. **İşlem Hattı Doğrulama Raporu**'nu kapatmak için **>>** seçeneğine tıklayın.

1. Varlıkları (veri kümeleri, ardışık hatlar, vb.) Veri Fabrikası hizmetine yayımlamak için pencerenin üstündeki **tümleri yayımla'yı** tıklatın. Yayımlama başarılı olana kadar bekleyin. 

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme

1. Ardışık **getTableListAndTriggerCopyData**gidin, üst boru hattı araç çubuğunda **Tetik ekle'yi** tıklatın ve sonra **şimdi Tetikle'yi**tıklatın. 

1. **Pipeline çalıştır sayfasında** çalıştır'ı onaylayın ve ardından **Finish'i**seçin.

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **Monitör** sekmesine geçin. Çözümünüzdeki her iki ardışık boru hattının da çalıştığını görene kadar **Yenile'yi** tıklatın. **Başarılı oldu** durumunu görene kadar listeyi yenilemeye devam edin. 

1. **GetTableListAndTriggerCopyData** ardışık lığıyla ilişkili etkinlik çalışanlarını görüntülemek için, ardışık hatlar için ardışık hat adı bağlantısını tıklatın. Bu işlem hattı çalıştırması için iki etkinlik çalıştırması görüyor olmalısınız. 
    ![Monitor Pipeline çalıştır](./media/tutorial-bulk-copy-portal/monitor-pipeline.png)
1. **Arama** etkinliğinin çıktısını görüntülemek **için, ACTIVITY NAME** sütununun altındaki etkinliğin yanındaki **Çıktı** bağlantısını tıklatın. **Çıkış** penceresinin ekranı kaplamasını sağlamalı ve pencereyi geri yüklemelisiniz. Gözden geçirdikten sonra, **X** simgesine tıklayarak **Çıkış** penceresini kapatın.

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
1. **Pipeline Runs** görünümüne geri dönmek için, kırıntı menüsünün üst kısmındaki **Tüm Boru Hattı Çalışır** bağlantısını tıklatın. Ardışık dizinin etkinlik çalışanlarını görüntülemek için **IterateAndCopySQLTables** bağlantısını **(PIPELINE NAME** sütunu altında) tıklatın. **Arama** etkinliği çıktısında her tablo için bir **Kopya** etkinliği çalıştırıldığını unutmayın. 

1. Verilerin bu öğreticide kullandığınız hedef Azure Synapse Analytics'e (eski adıyla SQL DW) kopyalandığını doğrulayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Azure SQL Veritabanı, Azure Synapse Analytics (eski adıyla SQL DW) ve Azure Depolama bağlantılı hizmetler oluşturun.
> * Azure SQL Veritabanı ve Azure Synapse Analytics (eski adıyla SQL DW) veri kümeleri oluşturun.
> * Kopyalanacak tabloları aramak için bir işlem hattı ve gerçek kopyalama işlemini gerçekleştirmek için başka bir işlem hattı oluşturma. 
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı ve etkinlik çalıştırmalarını izleme.

Kaynaktan hedefe verileri artımlı olarak koppyalama hakkında bilgi edinmek için aşağıdaki öğreticiye geçin:
> [!div class="nextstepaction"]
>[Veri artımlı olarak kopyalama](tutorial-incremental-copy-portal.md)
