---
title: Azure portalLarını kullanarak SQL Server'dan Blob depolamasına verileri kopyalama
description: Azure Data Factory’de şirket içinde barındırılan tümleştirme çalışma zamanını kullanarak şirket içi veri deposundan buluta veri kopyalama hakkında bilgi edinin.
services: data-factory
ms.author: abnarain
author: nabhishek
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019; seo-dt-2019
ms.date: 03/12/2020
ms.openlocfilehash: c073d3e51234e0ed8e524c2ae557d4158ad9e7d7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80065106"
---
# <a name="copy-data-from-an-on-premises-sql-server-database-to-azure-blob-storage"></a>Verileri şirket içi SQL Server veritabanından Azure Blob depolamaya kopyalama
Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak verileri şirket içi bir SQL Server veritabanından Azure Blob depolama alanına kopyalayan bir veri fabrikası işlem hattı oluşturursunuz. Verileri şirket içi ile bulut veri depoları arasında taşıyan, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturup kullanabilirsiniz.

> [!NOTE]
> Bu makale, Data Factory’ye giriş konusunda ayrıntılı bilgi sağlamaz. Daha fazla bilgi için bkz. [Data Factory'ye giriş](introduction.md).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * SQL Server ve Azure Depolama bağlı hizmetlerini oluşturma.
> * SQL Server ve Azure Blob veri kümeleri oluşturma.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı çalıştırmasını izleme.

## <a name="prerequisites"></a>Ön koşullar
### <a name="azure-subscription"></a>Azure aboneliği
Başlamadan önce, mevcut bir Azure aboneliğiniz yoksa [ücretsiz hesap oluşturun](https://azure.microsoft.com/free/).

### <a name="azure-roles"></a>Azure rolleri
Veri fabrikası örnekleri oluşturmak için Azure’da oturum açarken kullandığınız kullanıcı hesabına *Katkıda bulunan* veya *Sahip* rolü atanmalı ya da bu hesap Azure aboneliğinin *yöneticisi* olmalıdır.

Abonelikte sahip olduğunuz izinleri görüntülemek için Azure portalına gidin. Sağ üst köşeden kullanıcı adınızı ve sonra **İzinler**’i seçin. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. Bir role kullanıcı eklemeye ilişkin örnek yönergeler için, bkz. [RBAC ve Azure portalı kullanarak erişimi yönetme](../role-based-access-control/role-assignments-portal.md).

### <a name="sql-server-2014-2016-and-2017"></a>SQL Server 2014, 2016 ve 2017
Bu öğreticide, şirket içi SQL Server veritabanını bir *kaynak* veri deposu olarak kullanırsınız. Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Blob depolama alanına (havuz) kopyalar. Daha sonra SQL Server veritabanınızda **emp** adında bir tablo oluşturur sunuz ve tabloya birkaç örnek giriş eklersiniz.

1. SQL Server Management Studio’yu başlatın. Makinenizde zaten yüklü değilse [SQL Server Management Studio'yu indirme](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) sayfasına gidin.

1. Kimlik bilgilerinizi kullanarak SQL Server örneğinize bağlanın.

1. Örnek bir veritabanı oluşturun. Ağaç görünümünde **Veritabanları**'na sağ tıklayın ve **Yeni Veritabanı**'nı seçin.
1. **Yeni Veritabanı** penceresinde, veritabanı için bir ad girin ve **Tamam**'ı seçin.

1. **Emp** tablosunu oluşturmak ve içine bazı örnek veriler eklemek için aşağıdaki sorgu komut dosyasını veritabanına karşı çalıştırın. Ağaç görünümünde, oluşturduğunuz veritabanına sağ tıklayın ve **Yeni Sorgu**'yu seçin.

   ```
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    INSERT INTO emp (FirstName, LastName) VALUES ('John', 'Doe')
    INSERT INTO emp (FirstName, LastName) VALUES ('Jane', 'Doe')
    GO
   ```

### <a name="azure-storage-account"></a>Azure depolama hesabı
Bu öğreticide, genel amaçlı Azure depolama hesabını (özel olarak Blob depolama) hedef/havuz veri deposu olarak kullanırsınız. Genel amaçlı bir Azure depolama hesabınız yoksa bkz. [Depolama hesabı oluşturma](../storage/common/storage-account-create.md). Bu öğreticide oluşturduğunuz veri fabrikasındaki işlem hattı, verileri bu şirket içi SQL Server veritabanından (kaynak) Blob depolama alanına (havuz) kopyalar. 

#### <a name="get-the-storage-account-name-and-account-key"></a>Depolama hesabı adını ve hesap anahtarını alma
Bu öğreticide, depolama hesabınızın adını ve anahtarını kullanırsınız. Depolama hesabınızın adını ve anahtarını almak için aşağıdaki adımları gerçekleştirin:

1. Azure kullanıcı adı ve parolanızla [Azure portalında](https://portal.azure.com) oturum açın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Depolama** anahtar sözcüğünü kullanarak filtreleyin ve **Depolama hesapları**’nı seçin.

    ![Depolama hesabı araması](media/doc-common-process/search-storage-account.png)

1. Depolama hesapları listesinde, gerekirse depolama hesabınıza filtre uygulayın. Sonra depolama hesabınızı seçin.

1. **Depolama hesabı** penceresinde **Erişim anahtarları**'nı seçin.

1. **Depolama hesabı adı** ve **key1** kutularında değerleri kopyalayın ve ardından onları öğreticide daha sonra kullanmak için Not Defteri'ne veya başka bir düzenleyiciye yapıştırın.

#### <a name="create-the-adftutorial-container"></a>Adftutorial kapsayıcını oluşturma
Bu bölümde, Blob depolama alanınızda **adftutorial** adlı bir blob kapsayıcısı oluşturursunuz.

1. Depolama **hesabı** penceresinde Genel **Bakış'a**gidin ve ardından **Kapsayıcılar'ı**seçin.

    ![Bloblar seçeneğini belirleyin](media/tutorial-hybrid-copy-powershell/select-blobs.png)

1. **Kapsayıcılar** penceresinde, yeni bir tane oluşturmak için **+ Kapsayıcı'yı** seçin.

1. **Yeni kapsayıcı** penceresinde, **Ad** bölümüne **adftutorial** adını girin. Ardından **Oluştur**’u seçin.

1. Kapsayıcılar listesinde, yeni oluşturduğunuz **adftutorial'i** seçin.

1. **adftutorial** için **kapsayıcı** penceresini açık tutun. Öğreticinin sonundaki çıktıyı doğrulamak için kullanırsınız. Data Factory bu kapsayıcıda çıktı klasörünü otomatik olarak oluşturduğundan sizin oluşturmanız gerekmez.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturacak ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini başlatacaksınız.

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
1. Sol menüde, **bir kaynak** > **Analiz** > **Veri Fabrikası**Oluştur'u seçin:

   ![“Yeni” bölmesinde Data Factory seçimi](./media/doc-common-process/new-azure-data-factory-menu.png)

1. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

   Veri fabrikasının adı *genel olarak benzersiz* olmalıdır. Ad alanı için aşağıdaki hata iletisini görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](naming-rules.md).

   ![Yeni veri fabrikasının adı](./media/doc-common-process/name-not-available-error.png)

1. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin.
1. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

   - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.
        
     Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/management/overview.md).
1. **Sürüm** bölümünde **V2**'yi seçin.
1. **Konum**altında, veri fabrikasının konumunu seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Data Factory tarafından kullanılan veri depoları (örneğin, Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.
1. **Oluştur'u**seçin.

1. Oluşturma tamamlandıktan sonra, resimde gösterildiği gibi **Veri Fabrikası** sayfasını görürsünüz:

    ![Data factory giriş sayfası](./media/doc-common-process/data-factory-home-page.png)
1. Data Factory kullanıcı arabirimini ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğunu seçin.


## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin. Sizin için otomatik olarak bir işlem hattı oluşturulur. İşlem hattının ağaç görünümünde yer aldığını ve düzenleyicisinin açık olduğunu görürsünüz.

   ![Başlayalım sayfası](./media/doc-common-process/get-started-page.png)

1. **Özellikler** penceresinin altındaki **Genel** sekmesinde, **Ad**için **SQLServerToBlobPipeline'ı**girin.

1. **Etkinlikler** araç kutusunda, **Taşı & Dönüştür'i**genişletin. **Kopyalama** etkinliğini kopyalayıp işlem hattı tasarım yüzeyine bırakın. Etkinliğin adını **CopySqlServerToAzureBlobActivity** olarak ayarlayın.

1. **Özellikler** penceresinde **Kaynak** sekmesine gidin ve **+ Yeni**’yi seçin.

1. Yeni **Dataset** iletişim kutusunda, **SQL Server'ı**arayın. **SQL Server'ı**seçin ve ardından **Devam et'i**seçin.
    ![Yeni SqlServer veri seti](./media/tutorial-hybrid-copy-portal/create-sqlserver-dataset.png)

1. Özellikleri **Ayarla** iletişim kutusunda, **Ad**altında, **SqlServerDataset**girin. **Bağlantılı hizmet**altında , seçin **+ Yeni**. Bu adımda, kaynak veri deposuna (SQL Server veritabanı) yönelik bir bağlantı oluşturursunuz.

1. Yeni **Bağlantılı Hizmet** iletişim kutusuna, **SqlServerLinkedService**olarak **Ad** ekleyin. **Tümleştirme çalışma süresi ile Bağlan**altında **+Yeni'yi**seçin.  Bu bölümde, şirket içinde barındırılan bir tümleştirme çalışma zamanı oluşturur ve SQL Server veritabanını içeren bir şirket içi makine ile ilişkilendirirsiniz. Şirket içinde barındırılan tümleştirme çalışma zamanı, makinenizdeki SQL Server veitabanınızdaki verileri Blob depolama alanına kopyalayan bileşendir.

1. **Tümleştirme Runtime Kurulumu** iletişim kutusunda, **Kendi Kendine Barındırılan'ı**seçin ve ardından **Devam et'i**seçin.

1. Ad altında, **TutorialIntegrationRuntime**girin. Ardından **Oluştur**’u seçin.

1. Ayarlar **için, bu bilgisayarın ekspres kurulumünü başlatmak için buraya tıklayın'ı**seçin. Bu işlem, tümleştirme çalışma zamanını makinenize yükler ve Data Factory’ye kaydeder. Alternatif olarak, el ile kurulum seçeneğini kullanarak yükleme dosyasını indirip çalıştırabilir ve anahtarı kullanarak tümleştirme çalışma zamanını kaydedebilirsiniz.
    ![Tümleştirme çalışma zamanı kurulumu](./media/tutorial-hybrid-copy-portal/intergration-runtime-setup.png)

1. **Tümleştirme Runtime (Self-hosted) Express Kurulum** penceresinde, işlem tamamlandığında **Kapat'ı** seçin.

    ![Integration runtime (şirket içinde barındırılan) hızlı kurulum](./media/tutorial-hybrid-copy-portal/integration-runtime-setup-successful.png)

1. Yeni **bağlantılı hizmet (SQL Server)** iletişim kutusunda, **TutorialIntegrationRuntime'ın** **tümleştirme çalışma süresi ile Connect**altında seçildiğini doğrulayın. Ardından, aşağıdaki adımları izleyin:

    a. **Ad** bölümüne **SqlServerLinkedService** adını girin.

    b. **Sunucu adı** bölümüne SQL Server örneğinizin adını girin.

    c. **Veritabanı adı** alanında, **emp** tablosunu içeren veritabanının adını belirtin.

    d. **Kimlik doğrulaması türü** bölümünde, Data Factory’nin SQL Server veritabanınıza bağlanmak için kullanması gereken uygun kimlik doğrulaması türünü seçin.

    e. **Kullanıcı adı** ve **Parola** bölümlerine kullanıcı adını ve parolasını girin. Kullanıcı hesabınızda veya sunucu adında ters eğik çizgi karakteri (\\) kullanmanız gerekirse önüne kaçış karakterini (\\) koyun. Örneğin, *\\\\mydomain myuser*kullanın.

    f. **Bağlantıyı sına**’yı seçin. Bu adım, Veri Fabrikası'nın oluşturduğunuz kendi barındırılan tümleştirme çalışma saatini kullanarak SQL Server veritabanınıza bağlanabileceğini doğrulamaktır.

    g. Bağlantılı hizmeti kaydetmek için **Oluştur'u**seçin.
 
    ![Yeni bağlantılı hizmet (SQL Server)](./media/tutorial-hybrid-copy-portal/new-sqlserver-linked-service.png)

1. Bağlantılı hizmet oluşturulduktan sonra, SqlServerDataset için **Set özellikleri** sayfasına geri dönersiniz. Aşağıdaki adımları uygulayın:

    a. **Bağlı hizmet** bölümünde **SqlServerLinkedService**’i gördüğünüzü onaylayın.

    b. **Tablo adı altında** **[dbo] seçeneğini belirleyin.[ emp]**.
    
    c. **Tamam'ı**seçin.

1. **SQLServerToBlobPipeline**’ı içeren sekmeye gidin veya ağaç görünümünden **SQLServerToBlobPipeline**’ı seçin.

1. **Özellikler** penceresinin altındaki **Havuz** sekmesine gidin ve **+ Yeni**’yi seçin.

1. Yeni **Veri Kümesi** iletişim kutusunda **Azure Blob Depolama'yı**seçin. Daha sonra **Devam** seçeneğini belirleyin.

1. **Biçimbiçim** kutusunu seç'te, verilerinizin biçim türünü seçin. Daha sonra **Devam** seçeneğini belirleyin.

    ![Veri biçimi seçimi](./media/doc-common-process/select-data-format.png)

1. Özellikleri **Ayarla** iletişim kutusuna, Ad için **AzureBlobDataset'i** girin. **Bağlı hizmet** metin kutusunun yanındaki **+ Yeni** seçeneğini belirleyin.

1. Yeni **Bağlantılı Hizmet (Azure Blob Depolama)** iletişim kutusunda, ad olarak **AzureStorageLinkedService'i** girin, **Depolama hesabı** adı listesinden depolama hesabınızı seçin. Bağlantıyı test edin ve ardından bağlantılı hizmeti dağıtmak için **Oluştur'u** seçin.

1. Bağlantılı hizmet oluşturulduktan sonra, **Özellikleri Ayarla** sayfasına geri dönersiniz. **Tamam'ı**seçin.

1. Lavabo veri kümesini açın. **Bağlantı** sekmesinde aşağıdaki adımları uygulayın:

    a. **Bağlı hizmet** bölümünde **AzureStorageLinkedService**’in seçildiğini onaylayın.

    b. **Dosya yolunda,** **Kapsayıcı/ Dizin** bölümü için **adftutorial/fromonprem** girin. Çıktı klasörü adftutorial kapsayıcısında mevcut değilse Data Factory tarafından otomatik olarak oluşturulur.

    c. **Dosya** bölümü için **dinamik içerik ekle'yi**seçin.
    ![Dosya adını çözümlemek için dinamik ifade](./media/tutorial-hybrid-copy-portal/file-name.png)

    d. Ekle `@CONCAT(pipeline().RunId, '.txt')`ve **sonra Bitir'i**seçin. Bu eylem, dosyayı PipelineRunID.txt ile yeniden adlandıracaktır.

1. İşlem hattının açık olduğu sekmeye gidin veya ağaç görünümünde işlem hattını seçin. **Havuz Veri Kümesi** bölümünde **AzureBlobDataset**’in seçili olduğunu onaylayın.

1. İşlem hattı ayarlarını doğrulamak için işlem hattının araç çubuğunda **Doğrula**’yı seçin. **Boru doğrulama çıktısını**kapatmak için **>>** simgeyi seçin.
    ![boru hattını doğrulamak](./media/tutorial-hybrid-copy-portal/validate-pipeline.png)
    

1. Oluşturduğunuz varlıkları Veri Fabrikası'nda yayımlamak için **Tümünü Yayımla'yı**seçin.

1. **Yayımlama'nın açılır** penceresini tamamladığını görene kadar bekleyin. Yayımlama durumunu denetlemek için pencerenin üst kısmındaki **Bildirimleri Göster** bağlantısını seçin. Bildirim penceresini kapatmak için **Kapat**’ı seçin.


## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme
Ardışık iş tonu için araç çubuğuna **Tetikleyici Ekle'yi** ve ardından **Şimdi Tetikle'yi**seçin.

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **Monitör** sekmesine gidin. Önceki adımda el ile tetiklediğiniz ardışık hattı görürsünüz.

1. Pipeline run ile ilişkili etkinlik çalışır görüntülemek *için, PIPELINE NAME*altında **SQLServerToBlobPipeline** bağlantısını seçin. 
    ![İzleme boru hattı çalışır](./media/tutorial-hybrid-copy-portal/pipeline-runs.png)

1. Etkinlik **çalışır** sayfasında, kopyalama işlemiyle ilgili ayrıntıları görmek için Ayrıntılar (gözlük görüntüsü) bağlantısını seçin. Pipeline Runs görünümüne geri dönmek için, **tüm ardışık hatlar çalışır** üstte seçin.

## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı, `adftutorial` blob kapsayıcısında *fromonprem* adlı çıktı klasörünü otomatik olarak oluşturur. Çıktı klasöründe *[pipeline().RunId].txt* dosyasını gördüğünüzü onaylayın.


## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, verileri bir konumdan Blob depolama alanındaki başka bir konuma kopyalar. Şunları öğrendiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma.
> * Şirket içinde barındırılan tümleştirme çalışma zamanı oluşturma.
> * SQL Server ve Depolama bağlı hizmetlerini oluşturun.
> * SQL Server ve Blob depolama veri kümeleri oluşturun.
> * Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.
> * Bir işlem hattı çalıştırması başlatma.
> * İşlem hattı çalıştırmasını izleme.

Data Factory tarafından desteklenen veri depolarının listesi için [Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) konusuna bakın.

Verilerin toplu olarak kaynaktan hedefe nasıl kopyalanacağını öğrenmek için aşağıdaki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
>[Verileri toplu halde kopyalama](tutorial-bulk-copy-portal.md)
