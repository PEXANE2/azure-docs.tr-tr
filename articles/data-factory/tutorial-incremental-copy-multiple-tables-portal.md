---
title: Azure Data Factory kullanarak birden çok tabloyu artımlı olarak kopyalama | Microsoft Docs
description: Bu öğreticide, Delta verilerini şirket içi SQL Server veritabanındaki birden çok tablodan Azure SQL veritabanı 'na artımlı olarak kopyalayan bir Azure Data Factory işlem hattı oluşturacaksınız.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.date: 01/20/2018
ms.author: yexu
ms.openlocfilehash: 44ae433040c2c9cab47567cb663d4e588311a4a1
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177408"
---
# <a name="incrementally-load-data-from-multiple-tables-in-sql-server-to-an-azure-sql-database"></a>SQL Server birden çok tablodan Azure SQL veritabanına artımlı olarak veri yükleme
Bu öğreticide, şirket içi SQL Server birden çok tablodan Azure SQL veritabanına Delta verileri yükleyen bir işlem hattı ile bir Azure Data Factory oluşturacaksınız.    

Bu öğreticide aşağıdaki adımları gerçekleştirirsiniz:

> [!div class="checklist"]
> * Kaynak ve hedef veri depolarını hazırlayın.
> * Bir veri fabrikası oluşturun.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturun.
> * Tümleştirme çalışma zamanını yükler. 
> * Bağlı hizmetler oluşturun. 
> * Kaynak, havuz ve filigran veri kümeleri oluşturun.
> * İşlem hattı oluşturun, çalıştırın ve izleyin.
> * Sonuçları gözden geçirin.
> * Kaynak tablolarına veri ekleme veya güncelleştirme.
> * İşlem hattını yeniden çalıştırın ve izleyin.
> * Son sonuçları gözden geçirin.

## <a name="overview"></a>İlke
Bu çözümü oluşturmak için önemli adımlar şunlardır: 

1. **Filigran sütununu seçin**.
    
    Kaynak veri deposundaki her tablo için, her çalıştırma için yeni veya güncelleştirilmiş kayıtları tanımlamak üzere kullanılabilecek bir sütun seçin. Normal olarak, bu seçili sütundaki (örneğin, last_modify_time veya KIMLIK) veriler, satırlar oluşturulduğunda veya güncelleştirilirken artan şekilde devam eder. Bu sütundaki en büyük değer bir filigran olarak kullanılır.

1. **Bir veri deposunu, eşik değerini depolamak Için hazırlayın**.   
    
    Bu öğreticide, eşik değerini bir SQL veritabanında depoladığınızda.

1. **Aşağıdaki etkinliklerle bir işlem hattı oluşturun**: 
    
    a. İşlem hattına parametre olarak geçirilen kaynak tablo adlarının bir listesi üzerinden yinelenen bir ForEach etkinliği oluşturun. Her kaynak tablo için, bu tablo için Delta yükleme gerçekleştirmek üzere aşağıdaki etkinlikleri çağırır.

    b. İki arama etkinliği oluşturun. Son eşik değerini almak için ilk arama etkinliğini kullanın. Yeni eşik değerini almak için ikinci arama etkinliğini kullanın. Bu filigran değerleri kopyalama etkinliğine geçirilir.

    c. Kaynak veri deposundan satırları eski eşik değerinden büyük ve yeni eşik değerinden daha az olan filigran sütununun değeriyle kopyalayan bir kopyalama etkinliği oluşturun. Daha sonra, Delta verilerini kaynak veri deposundan Azure Blob depolama alanına yeni bir dosya olarak kopyalar.

    d. Bir sonraki sefer çalışan işlem hattı için eşik değerini güncelleştiren bir StoredProcedure etkinliği oluşturun. 

    Üst düzey çözüm diyagramı aşağıda verilmiştir: 

    ![Artımlı olarak veri yükleme](media/tutorial-incremental-copy-multiple-tables-portal/high-level-solution-diagram.png)


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Önkoşullar
* **SQL Server**. Bu öğreticide, bir şirket içi SQL Server veritabanını kaynak veri deposu olarak kullanırsınız. 
* **Azure SQL veritabanı**. Bir SQL veritabanını havuz veri deposu olarak kullanırsınız. SQL veritabanınız yoksa, oluşturma adımları için bkz. [Azure SQL veritabanı oluşturma](../sql-database/sql-database-get-started-portal.md) . 

### <a name="create-source-tables-in-your-sql-server-database"></a>SQL Server veritabanınızda kaynak tabloları oluşturma

1. SQL Server Management Studio açın ve şirket içi SQL Server veritabanınıza bağlanın.

1. **Sunucu Gezgini**, veritabanına sağ tıklayın ve **Yeni sorgu**' yı seçin.

1. @No__t-0 ve `project_table` adlı tablolar oluşturmak için aşağıdaki SQL komutunu veritabanınızda çalıştırın:

    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );
        
    INSERT INTO customer_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'John','9/1/2017 12:56:00 AM'),
    (2, 'Mike','9/2/2017 5:23:00 AM'),
    (3, 'Alice','9/3/2017 2:36:00 AM'),
    (4, 'Andy','9/4/2017 3:21:00 AM'),
    (5, 'Anny','9/5/2017 8:06:00 AM');
    
    INSERT INTO project_table
    (Project, Creationtime)
    VALUES
    ('project1','1/1/2015 0:00:00 AM'),
    ('project2','2/2/2016 1:23:00 AM'),
    ('project3','3/4/2017 5:16:00 AM');
    
    ```

### <a name="create-destination-tables-in-your-azure-sql-database"></a>Azure SQL veritabanınızda hedef tablolar oluşturma
1. SQL Server Management Studio açın ve Azure SQL veritabanınıza bağlanın.

1. **Sunucu Gezgini**, veritabanına sağ tıklayın ve **Yeni sorgu**' yı seçin.

1. @No__t-0 ve `project_table` adlı tablolar oluşturmak için SQL veritabanınızda aşağıdaki SQL komutunu çalıştırın:  
    
    ```sql
    create table customer_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );
    
    create table project_table
    (
        Project varchar(255),
        Creationtime datetime
    );

    ```

### <a name="create-another-table-in-the-azure-sql-database-to-store-the-high-watermark-value"></a>Üst eşik değerini depolamak için Azure SQL veritabanında başka bir tablo oluşturma
1. Eşik değerini depolamak için `watermarktable` adlı bir tablo oluşturmak üzere SQL veritabanınızda aşağıdaki SQL komutunu çalıştırın: 
    
    ```sql
    create table watermarktable
    (
    
        TableName varchar(255),
        WatermarkValue datetime,
    );
    ```
1. Her iki kaynak tabloya ait ilk filigran değerlerini filigran tablosuna ekleyin.

    ```sql

    INSERT INTO watermarktable
    VALUES 
    ('customer_table','1/1/2010 12:00:00 AM'),
    ('project_table','1/1/2010 12:00:00 AM');
    
    ```

### <a name="create-a-stored-procedure-in-the-azure-sql-database"></a>Azure SQL veritabanında bir saklı yordam oluşturma 

SQL veritabanınızda bir saklı yordam oluşturmak için aşağıdaki komutu çalıştırın. Bu saklı yordam, her işlem hattı çalıştırıldıktan sonra eşik değerini güncelleştirir. 

```sql
CREATE PROCEDURE usp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN

    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName

END

```

### <a name="create-data-types-and-additional-stored-procedures-in-azure-sql-database"></a>Azure SQL veritabanında veri türleri ve ek saklı yordamlar oluşturma
SQL veritabanınızda iki saklı yordam ve iki veri türü oluşturmak için aşağıdaki sorguyu çalıştırın. Bunlar, kaynak tablolardaki verileri hedef tablolarla birleştirmek için kullanılır.

Yolculuğun başlamasını kolaylaştırmak için, bu saklı yordamları doğrudan bir tablo değişkeni aracılığıyla içindeki Delta verileri geçirerek ve ardından bunları hedef depoda birleştirerek kullanırız. Tablo değişkeninde bir "büyük" sayıda Delta satırı (100 ' den fazla) beklenmediğinden emin olun.  

Hedef depoda çok sayıda Delta satırını birleştirmeniz gerekiyorsa, tüm Delta verileri önce hedef depodaki geçici bir "hazırlama" tablosuna kopyalamak için kopyalama etkinliği 'ni kullanmanızı ve ardından tablo VARI ' nı kullanmadan kendi saklı yordamınıza sahip olup olmadığını öneririz bunları "hazırlama" tablosundan "son" tablosuna birleştirebiliyor. 


```sql
CREATE TYPE DataTypeforCustomerTable AS TABLE(
    PersonID int,
    Name varchar(255),
    LastModifytime datetime
);

GO

CREATE PROCEDURE usp_upsert_customer_table @customer_table DataTypeforCustomerTable READONLY
AS

BEGIN
  MERGE customer_table AS target
  USING @customer_table AS source
  ON (target.PersonID = source.PersonID)
  WHEN MATCHED THEN
      UPDATE SET Name = source.Name,LastModifytime = source.LastModifytime
  WHEN NOT MATCHED THEN
      INSERT (PersonID, Name, LastModifytime)
      VALUES (source.PersonID, source.Name, source.LastModifytime);
END

GO

CREATE TYPE DataTypeforProjectTable AS TABLE(
    Project varchar(255),
    Creationtime datetime
);

GO

CREATE PROCEDURE usp_upsert_project_table @project_table DataTypeforProjectTable READONLY
AS

BEGIN
  MERGE project_table AS target
  USING @project_table AS source
  ON (target.Project = source.Project)
  WHEN MATCHED THEN
      UPDATE SET Creationtime = source.Creationtime
  WHEN NOT MATCHED THEN
      INSERT (Project, Creationtime)
      VALUES (source.Project, source.Creationtime);
END

```

## <a name="create-a-data-factory"></a>Veri Fabrikası oluşturma

1. **Microsoft Edge** veya **Google Chrome** Web tarayıcısını başlatın. Şu anda Data Factory UI yalnızca Microsoft Edge ve Google Chrome Web tarayıcılarında desteklenir.
1. Sol taraftaki menüde **Yeni** ' ye tıklayın, **veri ve analiz**' a ve **Data Factory**' a tıklayın. 
   
   ![New-> DataFactory](./media/tutorial-incremental-copy-multiple-tables-portal/new-azure-data-factory-menu.png)
1. **Yeni Veri Fabrikası** sayfasında **ad**için **ADFMultiIncCopyTutorialDF** girin. 
      
     ![Yeni Veri Fabrikası sayfası](./media/tutorial-incremental-copy-multiple-tables-portal/new-azure-data-factory.png)
 
   Azure veri fabrikasının adı **genel olarak benzersiz**olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin, Adınızadfmultiınccopytutorialdf) ve yeniden oluşturmayı deneyin. Data Factory yapıtlara yönelik adlandırma kuralları için [Data Factory adlandırma kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name ADFMultiIncCopyTutorialDF is not available`
1. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin. 
1. **Kaynak grubu**için aşağıdaki adımlardan birini yapın:
     
      - **Mevcut olanı kullan**' ı seçin ve açılır listeden var olan bir kaynak grubunu seçin. 
      - **Yeni oluştur**' u seçin ve bir kaynak grubunun adını girin.   
         
        Kaynak grupları hakkında bilgi edinmek için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
1. **Sürüm**için **v2 (Önizleme)** öğesini seçin.
1. Veri fabrikasının **konumunu** seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları (Azure depolama, Azure SQL veritabanı vb.) ve işlemler (HDInsight, vb.) başka bölgelerde olabilir.
1. **Panoya sabitle ' yi**seçin.     
1. **Oluştur**'a tıklayın.      
1. Panoda aşağıdaki kutucuğu görürsünüz: **Veri Fabrikası dağıtılıyor**. 

    ![Veri Fabrikası dağıtma kutucuğu](media/tutorial-incremental-copy-multiple-tables-portal/deploying-data-factory.png)
1. Oluşturma işlemi tamamlandıktan sonra, görüntüde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data Factory giriş sayfası](./media/tutorial-incremental-copy-multiple-tables-portal/data-factory-home-page.png)
1. Azure Data Factory Kullanıcı arabirimi (UI) ayrı bir sekmede başlatmak için **& İzleyici** kutucuğuna yaz ' a tıklayın.
1. Azure Data Factory Kullanıcı arabiriminin Başlarken sayfasında, işlem **hattı oluştur** ' a tıklayın (veya) **Düzenle** sekmesine geçin. 

   ![Kullanmaya başlama sayfası](./media/tutorial-incremental-copy-multiple-tables-portal/get-started-page.png)

## <a name="create-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma
Verileri özel bir ağda (Şirket içi) bir Azure veri deposuna taşırken, şirket içi ortamınıza şirket içinde barındırılan bir tümleştirme çalışma zamanı (IR) yükleyebilirsiniz. Şirket içinde barındırılan IR, verileri özel ağınız ve Azure arasında taşımalıdır. 

1. Sol bölmenin altındaki **Bağlantılar** ' a tıklayın ve **Bağlantılar** penceresinde **tümleştirme çalışma zamanları** ' na geçin. 

   ![Bağlantılar sekmesi](./media/tutorial-incremental-copy-multiple-tables-portal/connections-tab.png)
1. Tümleştirme çalışma **zamanları** sekmesinde **+ Yeni**' ye tıklayın. 

   ![Yeni tümleştirme çalışma zamanı-düğme](./media/tutorial-incremental-copy-multiple-tables-portal/new-integration-runtime-button.png)
1. **Integration Runtime kurulum** penceresinde, **veri taşıma ve dağıtım etkinliklerini dış hesaplamalar**' ı seçin ve **İleri**' ye tıklayın. 

   ![Tümleştirme çalışma zamanı türünü seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-integration-runtime-type.png)
1. \* * Özel Ağ * * öğesini seçin ve **İleri**' ye tıklayın. 

   ![Özel ağ seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-private-network.png)
1. **Ad**Için **Myselfhostedir** yazın ve **İleri**' ye tıklayın. 

   ![Şirket içinde barındırılan IR adı](./media/tutorial-incremental-copy-multiple-tables-portal/self-hosted-ir-name.png)
1. **Seçenek 1: hızlı kurulum** bölümünde **Bu bilgisayarda hızlı kurulumu başlatmak için buraya tıklayın öğesine** tıklayın. 

   ![Hızlı Kurulum bağlantısı ' na tıklayın](./media/tutorial-incremental-copy-multiple-tables-portal/click-express-setup.png)
1. **Integration Runtime (Şirket içinde barındırılan) hızlı kurulum** penceresinde **Kapat**' a tıklayın. 

   ![Tümleştirme çalışma zamanı kurulumu-başarılı](./media/tutorial-incremental-copy-multiple-tables-portal/integration-runtime-setup-successful.png)
1. Web tarayıcısında, **Integration Runtime kurulum** penceresinde **son**' a tıklayın. 

   ![Tümleştirme çalışma zamanı kurulumu-son](./media/tutorial-incremental-copy-multiple-tables-portal/click-finish-integration-runtime-setup.png)
1. Tümleştirme çalışma zamanları listesinde **Myselfhostedir** ' i görtığınızdan emin olun.

    ![Tümleştirme çalışma zamanları-liste](./media/tutorial-incremental-copy-multiple-tables-portal/integration-runtimes-list.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için bir veri fabrikasında bağlı hizmetler oluşturursunuz. Bu bölümde, şirket içi SQL Server veritabanınıza ve SQL veritabanınıza bağlı hizmetler oluşturacaksınız. 

### <a name="create-the-sql-server-linked-service"></a>SQL Server bağlı hizmetini oluşturma
Bu adımda, şirket içi SQL Server veritabanınızı veri fabrikasına bağlarsınız.

1. **Bağlantılar** penceresinde, **tümleştirme çalışma zamanları** sekmesinden **bağlı hizmetler** sekmesine geçin ve **+ Yeni**' ye tıklayın.

    ![Yeni bağlı hizmet düğmesi](./media/tutorial-incremental-copy-multiple-tables-portal/new-sql-server-linked-service-button.png)
1. **Yeni bağlı hizmet** penceresinde **SQL Server**' yi seçin ve **devam**' a tıklayın. 

    ![SQL Server seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-sql-server.png)
1. **Yeni bağlı hizmet** penceresinde aşağıdaki adımları uygulayın:

    1. Ad için **Sqlserverlinkedservice** **adını**girin. 
    1. **Tümleştirme çalışma zamanı aracılığıyla Bağlan**Için **Myselfhostedir** öğesini seçin. Bu **önemli** bir adımdır. Varsayılan tümleştirme çalışma zamanı, şirket içi veri deposuna bağlanamaz. Daha önce oluşturduğunuz şirket içinde barındırılan tümleştirme çalışma zamanını kullanın. 
    1. **Sunucu adı**için SQL Server veritabanına sahip bilgisayarınızın adını girin.
    1. **Veritabanı adı**için, kaynak verileri içeren SQL Server veritabanının adını girin. Önkoşulların bir parçası olarak bu veritabanına bir tablo oluşturdunuz ve veri eklediniz. 
    1. **Kimlik doğrulama türü**için, veritabanına bağlanmak üzere kullanmak istediğiniz **kimlik doğrulaması türünü** seçin. 
    1. **Kullanıcı adı**için SQL Server veritabanına erişimi olan kullanıcının adını girin. Kullanıcı hesabınızda veya sunucu adında eğik çizgi karakteri (`\`) kullanmanız gerekiyorsa, kaçış karakterini kullanın (`\`). Örnek `mydomain\\myuser` ' dır.
    1. **Parola**için kullanıcının **parolasını** girin. 
    1. Data Factory SQL Server veritabanınıza bağlanıp bağlanamamadığını test etmek için **Bağlantıyı Sına**' ya tıklayın. Bağlantı başarılı olana kadar tüm hataları düzeltin. 
    1. Bağlı hizmeti kaydetmek için **Kaydet**' e tıklayın.

        ![SQL Server bağlı hizmeti-ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/sql-server-linked-service-settings.png)

### <a name="create-the-azure-sql-database-linked-service"></a>Azure SQL veritabanı bağlı hizmetini oluşturma
Son adımda, kaynak SQL Server veritabanınızı veri fabrikasına bağlamak için bağlı bir hizmet oluşturursunuz. Bu adımda, hedef/havuz Azure SQL veritabanınızı veri fabrikasına bağlarsınız. 

1. **Bağlantılar** penceresinde, **tümleştirme çalışma zamanları** sekmesinden **bağlı hizmetler** sekmesine geçin ve **+ Yeni**' ye tıklayın.

    ![Yeni bağlı hizmet düğmesi](./media/tutorial-incremental-copy-multiple-tables-portal/new-sql-server-linked-service-button.png)
1. **Yeni bağlı hizmet** PENCERESINDE **Azure SQL veritabanı**' nı seçin ve **devam**' a tıklayın. 
1. **Yeni bağlı hizmet** penceresinde aşağıdaki adımları uygulayın:

    1. **Ad**Için **Azuressqldatabaselinkedservice** girin. 
    1. **Sunucu adı**için, açılan LISTEDEN Azure SQL sunucunuzun adını seçin. 
    1. **Veritabanı adı**için, önkoşulların bir parçası olarak customer_table ve Project_table oluşturduğunuz Azure SQL veritabanı ' nı seçin. 
    1. **Kullanıcı adı**IÇIN Azure SQL veritabanına erişimi olan kullanıcının adını girin. 
    1. **Parola**için kullanıcının **parolasını** girin. 
    1. Data Factory SQL Server veritabanınıza bağlanıp bağlanamamadığını test etmek için **Bağlantıyı Sına**' ya tıklayın. Bağlantı başarılı olana kadar tüm hataları düzeltin. 
    1. Bağlı hizmeti kaydetmek için **Kaydet**' e tıklayın.

        ![Azure SQL bağlı hizmeti-ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/azure-sql-linked-service-settings.png)
1. Listede iki bağlı hizmet görtığınızdan emin olun. 
   
    ![İki bağlı hizmet](./media/tutorial-incremental-copy-multiple-tables-portal/two-linked-services.png) 

## <a name="create-datasets"></a>Veri kümeleri oluştur
Bu adımda veri kaynağını, veri hedefini ve filigranın yerleştirileceği yeri temsil eden veri kümeleri oluşturacaksınız.

### <a name="create-a-source-dataset"></a>Kaynak veri kümesi oluşturma

1. Sol bölmede **+ (artı)** seçeneğine tıklayın ve **veri kümesi**' ne tıklayın.

   ![Yeni veri kümesi menüsü](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
1. **Yeni veri kümesi** penceresinde **SQL Server**' yi seçin, **son**' a tıklayın. 

   ![SQL Server seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-sql-server-for-dataset.png)
1. Web tarayıcısında veri kümesini yapılandırmak için yeni bir sekme açıldığını görürsünüz. TreeView 'da da bir veri kümesi görürsünüz. En alttaki Özellikler penceresi **genel** sekmesinde, **ad**için **sourceDataset** girin. 

   ![Kaynak veri kümesi-ad](./media/tutorial-incremental-copy-multiple-tables-portal/source-dataset-general.png)
1. Özellikler penceresi **bağlantı** sekmesine geçin ve **bağlı hizmet**için **sqlserverlinkedservice** ' i seçin. Burada bir tablo seçemezsiniz. İşlem hattındaki kopyalama etkinliği, tüm tabloyu yüklemek yerine verileri yüklemek için bir SQL sorgusu kullanır.

   ![Kaynak veri kümesi-bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/source-dataset-connection.png)


### <a name="create-a-sink-dataset"></a>Havuz veri kümesi oluşturma
1. Sol bölmede **+ (artı)** seçeneğine tıklayın ve **veri kümesi**' ne tıklayın.

   ![Yeni veri kümesi menüsü](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
1. **Yeni veri kümesi** PENCERESINDE **Azure SQL veritabanı**' nı seçin ve **son**' a tıklayın. 

   ![Azure SQL veritabanı 'nı seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-azure-sql-database.png)
1. Web tarayıcısında veri kümesini yapılandırmak için yeni bir sekme açıldığını görürsünüz. TreeView 'da da bir veri kümesi görürsünüz. En alttaki Özellikler penceresi **genel** sekmesinde, ad Için **sinkdataset** **adını**girin.

   ![Havuz veri kümesi-genel](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-general.png)
1. Özellikler penceresi **Parametreler** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Parametreleri oluştur/güncelleştir** bölümünde **Yeni** ' ye tıklayın. 
    1. **Ad**Için **sinktablename** ve **tür**için **dize** girin. Bu veri kümesi **Sinktablename** 'i parametre olarak alır. SinkTableName parametresi, işlem hattı tarafından çalışma zamanında dinamik olarak ayarlanır. İşlem hattındaki ForEach etkinliği, tablo adlarının bir listesi üzerinden yinelenir ve tablo adını her yinelemede bu veri kümesine geçirir.
   
       ![Havuz veri kümesi-Özellikler](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-parameters.png)
1. Özellikler penceresi **bağlantı** sekmesine geçin ve **bağlı hizmet**Için **azuressqllinkedservice** ' i seçin. **Tablo** özelliği için **dinamik içerik Ekle**' ye tıklayın. 

   ![Havuz veri kümesi-bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-connection.png)
    
    
1. **Parametreler** bölümünde **sinktablename** öğesini seçin
   
   ![Havuz veri kümesi-bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-connection-dynamicContent.png)

   
 1. **Son**' a tıkladıktan sonra **\@dataset () görürsünüz. Tablo adı olarak SinkTableName** .
   
   ![Havuz veri kümesi-bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/sink-dataset-connection-completion.png)

### <a name="create-a-dataset-for-a-watermark"></a>Filigran için veri kümesi oluşturma
Bu adımda, yüksek bir eşik değerini depolamak için bir veri kümesi oluşturursunuz. 

1. Sol bölmede **+ (artı)** seçeneğine tıklayın ve **veri kümesi**' ne tıklayın.

   ![Yeni veri kümesi menüsü](./media/tutorial-incremental-copy-multiple-tables-portal/new-dataset-menu.png)
1. **Yeni veri kümesi** PENCERESINDE **Azure SQL veritabanı**' nı seçin ve **son**' a tıklayın. 

   ![Azure SQL veritabanı 'nı seçin](./media/tutorial-incremental-copy-multiple-tables-portal/select-azure-sql-database.png)
1. En alttaki Özellikler penceresi **genel** sekmesinde, **ad**için **filigran markdataset** girin.
1. **Bağlantı** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Bağlı hizmet**Için **Azuressqldatabaselinkedservice** öğesini seçin.
    1. **[Dbo] öğesini seçin. [ Tablo için su marktable]** .

       ![Filigran veri kümesi-bağlantı](./media/tutorial-incremental-copy-multiple-tables-portal/watermark-dataset-connection.png)

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
İşlem hattı, tablo adlarının bir listesini parametre olarak alır. ForEach etkinliği, tablo adları listesinde yinelenir ve aşağıdaki işlemleri gerçekleştirir: 

1. Eski eşik değerini (ilk değer veya son yinelemede kullanılan bir değeri) almak için arama etkinliğini kullanın.

1. Yeni eşik değerini (kaynak tablodaki sınır sütununun en büyük değeri) almak için arama etkinliğini kullanın.

1. Bu iki eşik değeri arasında kaynak veritabanından hedef veritabanına veri kopyalamak için kopyalama etkinliğini kullanın.

1. Bir sonraki yinelemenin ilk adımında kullanılacak eski eşik değerini güncelleştirmek için StoredProcedure etkinliğini kullanın. 

### <a name="create-the-pipeline"></a>İşlem hattını oluşturma

1. Sol bölmede **+ (artı)** seçeneğine tıklayın ve işlem **hattı**' na tıklayın.

    ![Yeni işlem hattı-menü](./media/tutorial-incremental-copy-multiple-tables-portal/new-pipeline-menu.png)
1. **Özellikler** penceresinin **genel** sekmesinde **ad**için **ıncrementalcopypipeline** girin. 

    ![İşlem hattı adı](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-name.png)
1. **Özellikler** penceresinde aşağıdaki adımları uygulayın: 

    1. **+ Yeni**seçeneğine tıklayın. 
    1. Parametre **adı**Için **tablelist** girin. 
    1. Parametre **türü**için **nesne** ' yi seçin.

    ![İşlem hattı parametreleri](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-parameters.png) 
1. **Etkinlikler** araç kutusunda **yineleme &** koşullar ' ı genişletin ve **foreach** etkinliğini sürükleyerek işlem hattı tasarımcısının yüzeyine bırakın. **Özellikler** penceresinin **genel** sekmesinde **iteratesqltables**girin. 

    ![ForEach etkinliği-adı](./media/tutorial-incremental-copy-multiple-tables-portal/foreach-name.png)
1. **Özellikler** penceresinde **Ayarlar** sekmesine geçin ve **öğeler**için `@pipeline().parameters.tableList` girin. ForEach etkinliği bir tablo listesi boyunca yinelenir ve artımlı kopyalama işlemini gerçekleştirir. 

    ![ForEach etkinliği-ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/foreach-settings.png)
1. İşlem hattında zaten seçili değilse **foreach** etkinliğini seçin. **Düzenle (kurşun kalem simgesi)** düğmesine tıklayın.

    ![ForEach etkinliği-Düzenle](./media/tutorial-incremental-copy-multiple-tables-portal/edit-foreach.png)
1. **Etkinlikler** araç kutusunda **genel**' i genişletin, **arama** etkinliğini bir işlem hattı tasarlayıcı yüzeyine bırakın ve **ad**için **lookupoldsulu markactivity** ' i girin.

    ![İlk arama etkinliği-ad](./media/tutorial-incremental-copy-multiple-tables-portal/first-lookup-name.png)
1. **Özellikler** penceresinin **Ayarlar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **Kaynak veri kümesi**Için **filigran markdataset** öğesini seçin.
    1. **Kullanım sorgusu**için **sorgu** ' yı seçin. 
    1. **Sorgu**IÇIN aşağıdaki SQL sorgusunu girin. 

        ```sql
        select * from watermarktable where TableName  =  '@{item().TABLE_NAME}'
        ```

        ![İlk arama etkinliği-ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/first-lookup-settings.png)
1. **Etkinlikler** araç kutusundan **arama** etkinliğini sürükleyip bırakın ve **ad**için **lookupnewsulu markactivity** yazın.
        
    ![İkinci arama etkinliği-ad](./media/tutorial-incremental-copy-multiple-tables-portal/second-lookup-name.png)
1. **Ayarlar** sekmesine geçin.

    1. **Kaynak veri kümesi**Için **sourceDataset** öğesini seçin. 
    1. **Kullanım sorgusu**için **sorgu** ' yı seçin.
    1. **Sorgu**IÇIN aşağıdaki SQL sorgusunu girin.

        ```sql    
        select MAX(@{item().WaterMark_Column}) as NewWatermarkvalue from @{item().TABLE_NAME}
        ```
    
        ![İkinci arama etkinliği-ayarlar](./media/tutorial-incremental-copy-multiple-tables-portal/second-lookup-settings.png)
1. **Etkinlikler** araç kutusundan **kopyalama** etkinliğini sürükleyip bırakın ve **ad**için **ıncrementalcopyactivity** girin. 

    ![Kopyalama etkinliği-ad](./media/tutorial-incremental-copy-multiple-tables-portal/copy-activity-name.png)
1. **Arama** etkinliklerini tek tek **kopyalama** etkinliğine bağlayın. Bağlanmak için, **arama** etkinliğine eklenen **yeşil** kutuda sürüklemeye başlayın ve **kopyalama** etkinliğine bırakın. Kopyalama etkinliğinin kenarlık rengi **mavi**olarak değiştiğinde fare düğmesini bırakın.

    ![Arama etkinliklerini kopyalama etkinliğine bağlama](./media/tutorial-incremental-copy-multiple-tables-portal/connect-lookup-to-copy.png)
1. İşlem hattında **kopyalama** etkinliğini seçin. **Özellikler** penceresinde **kaynak** sekmesine geçin. 

    1. **Kaynak veri kümesi**Için **sourceDataset** öğesini seçin. 
    1. **Kullanım sorgusu**için **sorgu** ' yı seçin. 
    1. **Sorgu**IÇIN aşağıdaki SQL sorgusunu girin.

        ```sql
        select * from @{item().TABLE_NAME} where @{item().WaterMark_Column} > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and @{item().WaterMark_Column} <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'        
        ```

        ![Kopyalama etkinliği-kaynak ayarları](./media/tutorial-incremental-copy-multiple-tables-portal/copy-source-settings.png)
1. **Havuz** sekmesine geçin ve **Havuz veri kümesi**için **sinkdataset** ' i seçin. 
        
    ![Kopyalama etkinliği-havuz ayarları](./media/tutorial-incremental-copy-multiple-tables-portal/copy-sink-settings.png)
1. Aşağıdaki adımları uygulayın:

    1. **DataSet** özelliğinde, **sinktablename** parametresi için `@{item().TABLE_NAME}` girin.
    1. **Saklı yordam adı** özelliği için `@{item().StoredProcedureNameForMergeOperation}` girin.
    1. **Tablo türü** özelliği için `@{item().TableType}` girin.


        ![Kopyalama etkinliği-parametreler](./media/tutorial-incremental-copy-multiple-tables-portal/copy-activity-parameters.png)
1. **Etkinlikler** araç kutusundan **saklı yordam** etkinliğini sürükleyip işlem hattı tasarımcı yüzeyine bırakın. **Kopyalama** etkinliğini **saklı yordam** etkinliğine bağlayın. 

    ![Kopyalama etkinliği-parametreler](./media/tutorial-incremental-copy-multiple-tables-portal/connect-copy-to-sproc.png)
1. Işlem hattının **saklı yordam** etkinliğini seçin ve **Özellikler** penceresinin **genel** sekmesinde **ad** için **StoredProceduretoWriteWatermarkActivity** girin. 

    ![Saklı yordam etkinliği-adı](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-name.png)
1. **SQL hesabı** sekmesine geçin ve **bağlı hizmet**Için **azuressqldatabaselinkedservice** ' i seçin.

    ![Saklı yordam etkinliği-SQL hesabı](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-sql-account.png)
1. **Saklı yordam** sekmesine geçin ve aşağıdaki adımları uygulayın:

    1. **Saklı yordam adı**için `usp_write_watermark` ' i seçin. 
    1. **Içeri aktarma parametresini**seçin. 
    1. Parametreler için aşağıdaki değerleri belirtin: 

        | Ad | Tür | Değer | 
        | ---- | ---- | ----- |
        | Zamanı | Hem | `@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}` |
        | tableName | Dize | `@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}` |
    
        ![Saklı yordam etkinliği-saklı yordam ayarları](./media/tutorial-incremental-copy-multiple-tables-portal/sproc-activity-sproc-settings.png)
1. Sol bölmede **Yayımla**' ya tıklayın. Bu eylem, oluşturduğunuz varlıkları Data Factory hizmetine yayımlar. 

    ![Yayımla düğmesi](./media/tutorial-incremental-copy-multiple-tables-portal/publish-button.png)
1. **Başarıyla yayımlanmış** iletiyi görene kadar bekleyin. Bildirimleri görmek için **bildirimleri göster** bağlantısına tıklayın. **X**' i tıklatarak Bildirimler penceresini kapatın.

    ![Bildirimleri göster](./media/tutorial-incremental-copy-multiple-tables-portal/notifications.png)

 
## <a name="run-the-pipeline"></a>İşlem hattını çalıştırma

1. İşlem hattının araç çubuğunda **Tetikle**' ye tıklayıp **Şimdi Tetikle**' ye tıklayın.     

    ![Şimdi Tetikle](./media/tutorial-incremental-copy-multiple-tables-portal/trigger-now.png)
1. İşlem **hattı çalıştırma** penceresinde, **tablelist** parametresi için aşağıdaki değeri girin ve **son**' a tıklayın. 

    ```
    [
        {
            "TABLE_NAME": "customer_table",
            "WaterMark_Column": "LastModifytime",
            "TableType": "DataTypeforCustomerTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_customer_table"
        },
        {
            "TABLE_NAME": "project_table",
            "WaterMark_Column": "Creationtime",
            "TableType": "DataTypeforProjectTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_project_table"
        }
    ]
    ```

    ![İşlem hattı çalıştırma bağımsız değişkenleri](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-run-arguments.png)

## <a name="monitor-the-pipeline"></a>İşlem hattını izleme

1. Soldaki **izleyici** sekmesine geçin. **El ile tetikleyici**tarafından tetiklenen işlem hattı çalıştırmasını görürsünüz. Listeyi yenilemek için **Yenile** düğmesine tıklayın. Eylemler sütunundaki bağlantılar, Işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemenize ve **işlem** hattını yeniden çalıştırmanıza imkan tanır. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-runs.png)
1. **Eylemler** sütunundaki **etkinlik çalıştırmalarını görüntüle** bağlantısına tıklayın. Seçilen işlem hattı çalıştırmasıyla ilişkili tüm etkinlik çalıştırmalarını görürsünüz. 

    ![Etkinlik çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/activity-runs.png)

## <a name="review-the-results"></a>Sonuçları gözden geçirin
SQL Server Management Studio, verilerin kaynak tablolardan hedef tablolara kopyalandığını doğrulamak için, hedef SQL veritabanında aşağıdaki sorguları çalıştırın: 

**Sorgulayamadı** 
```sql
select * from customer_table
```

**Çıktıların**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           Alice   2017-09-03 02:36:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

**Sorgulayamadı**

```sql
select * from project_table
```

**Çıktıların**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
```

**Sorgulayamadı**

```sql
select * from watermarktable
```

**Çıktıların**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-05 08:06:00.000
project_table   2017-03-04 05:16:00.000
```

Her iki tablonun da eşik değerlerinin güncelleştirildiğini unutmayın. 

## <a name="add-more-data-to-the-source-tables"></a>Kaynak tablolara daha fazla veri ekleme

Customer_table ' de var olan bir satırı güncelleştirmek için kaynak SQL Server veritabanında aşağıdaki sorguyu çalıştırın. Project_table içine yeni bir satır ekleyin. 

```sql
UPDATE customer_table
SET [LastModifytime] = '2017-09-08T00:00:00Z', [name]='NewName' where [PersonID] = 3

INSERT INTO project_table
(Project, Creationtime)
VALUES
('NewProject','10/1/2017 0:00:00 AM');
``` 

## <a name="rerun-the-pipeline"></a>İşlem hattını yeniden çalıştırma
1. Web tarayıcısı penceresinde, soldaki **Düzenle** sekmesine geçin. 
1. İşlem hattının araç çubuğunda **Tetikle**' ye tıklayıp **Şimdi Tetikle**' ye tıklayın.   

    ![Şimdi Tetikle](./media/tutorial-incremental-copy-multiple-tables-portal/trigger-now.png)
1. İşlem **hattı çalıştırma** penceresinde, **tablelist** parametresi için aşağıdaki değeri girin ve **son**' a tıklayın. 

    ```
    [
        {
            "TABLE_NAME": "customer_table",
            "WaterMark_Column": "LastModifytime",
            "TableType": "DataTypeforCustomerTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_customer_table"
        },
        {
            "TABLE_NAME": "project_table",
            "WaterMark_Column": "Creationtime",
            "TableType": "DataTypeforProjectTable",
            "StoredProcedureNameForMergeOperation": "usp_upsert_project_table"
        }
    ]
    ```

## <a name="monitor-the-pipeline-again"></a>Ardışık düzeni yeniden izleyin

1. Soldaki **izleyici** sekmesine geçin. **El ile tetikleyici**tarafından tetiklenen işlem hattı çalıştırmasını görürsünüz. Listeyi yenilemek için **Yenile** düğmesine tıklayın. Eylemler sütunundaki bağlantılar, Işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemenize ve **işlem** hattını yeniden çalıştırmanıza imkan tanır. 

    ![İşlem hattı çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/pipeline-runs.png)
1. **Eylemler** sütunundaki **etkinlik çalıştırmalarını görüntüle** bağlantısına tıklayın. Seçilen işlem hattı çalıştırmasıyla ilişkili tüm etkinlik çalıştırmalarını görürsünüz. 

    ![Etkinlik çalıştırmaları](./media/tutorial-incremental-copy-multiple-tables-portal/activity-runs.png) 

## <a name="review-the-final-results"></a>Son sonuçları gözden geçirin
SQL Server Management Studio, güncelleştirilmiş/yeni verilerin kaynak tablolardan hedef tablolara kopyalandığını doğrulamak için, hedef veritabanında aşağıdaki sorguları çalıştırın. 

**Sorgulayamadı** 
```sql
select * from customer_table
```

**Çıktıların**
```
===========================================
PersonID    Name    LastModifytime
===========================================
1           John    2017-09-01 00:56:00.000
2           Mike    2017-09-02 05:23:00.000
3           NewName 2017-09-08 00:00:00.000
4           Andy    2017-09-04 03:21:00.000
5           Anny    2017-09-05 08:06:00.000
```

3 numaralı **kişinin PersonID** Için yeni **Name** ve **LastModifyTime** değerlerine dikkat edin. 

**Sorgulayamadı**

```sql
select * from project_table
```

**Çıktıların**

```
===================================
Project     Creationtime
===================================
project1    2015-01-01 00:00:00.000
project2    2016-02-02 01:23:00.000
project3    2017-03-04 05:16:00.000
NewProject  2017-10-01 00:00:00.000
```

**NewProject** girişinin project_table 'e eklendiğinden emin olun. 

**Sorgulayamadı**

```sql
select * from watermarktable
```

**Çıktıların**

```
======================================
TableName       WatermarkValue
======================================
customer_table  2017-09-08 00:00:00.000
project_table   2017-10-01 00:00:00.000
```

Her iki tablonun da eşik değerlerinin güncelleştirildiğini unutmayın.
     
## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide aşağıdaki adımları gerçekleştirdiniz: 

> [!div class="checklist"]
> * Kaynak ve hedef veri depolarını hazırlayın.
> * Bir veri fabrikası oluşturun.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı (IR) oluşturun.
> * Tümleştirme çalışma zamanını yükler.
> * Bağlı hizmetler oluşturun. 
> * Kaynak, havuz ve filigran veri kümeleri oluşturun.
> * İşlem hattı oluşturun, çalıştırın ve izleyin.
> * Sonuçları gözden geçirin.
> * Kaynak tablolarına veri ekleme veya güncelleştirme.
> * İşlem hattını yeniden çalıştırın ve izleyin.
> * Son sonuçları gözden geçirin.

Azure 'da Spark kümesi kullanarak verileri dönüştürme hakkında bilgi edinmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Değişiklik İzleme teknolojisini kullanarak Azure SQL veritabanından Azure Blob depolama alanına artımlı olarak veri yükleme](tutorial-incremental-copy-change-tracking-feature-portal.md)


