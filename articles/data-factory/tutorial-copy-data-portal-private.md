---
title: Azure Data Factory işlem hattı oluşturmak için özel uç noktaları kullanma
description: Bu öğreticide işlem hattıyla veri fabrikası oluşturmak için Azure portalını kullanmaya yönelik adım adım yönergeler sağlanır. İşlem hattı, Azure Blob depolama alanından Azure SQL veritabanına veri kopyalamak için kopyalama etkinliğini kullanır.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 05/15/2020
ms.author: jingwang
ms.openlocfilehash: 9458c89922fab8450b7cb8e202491d4c47e250e8
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86537670"
---
# <a name="copy-data-securely-from-azure-blob-storage-to-a-sql-database-by-using-private-endpoints"></a>Özel uç noktaları kullanarak verileri Azure Blob depolamadan bir SQL veritabanına güvenli bir şekilde kopyalama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu öğreticide, Azure Data Factory kullanıcı arabirimini (UI) kullanarak bir veri fabrikası oluşturursunuz. **Bu veri fabrikasındaki işlem hattı, verileri Azure Blob depolama 'dan Azure SQL veritabanına güvenli bir şekilde kopyalar (her ikisi de yalnızca seçili ağlara erişime izin veriliyor) [Azure Data Factory yönetilen sanal ağdaki](managed-virtual-network-private-endpoint.md)özel uç noktaları kullanarak.** Bu öğreticideki yapılandırma düzeni, dosya tabanlı bir veri deposundan ilişkisel bir veri deposuna kopyalama için geçerlidir. Kaynak ve havuz olarak desteklenen veri depolarının listesi için [desteklenen veri depoları](https://docs.microsoft.com/azure/data-factory/copy-activity-overview) tablosuna bakın.

> [!NOTE]
>
> - İlk kez Data Factory kullanıyorsanız bkz. [Azure Data Factory'ye giriş](https://docs.microsoft.com/azure/data-factory/introduction).

Bu öğreticide, aşağıdaki adımları gerçekleştireceksiniz:

> * Veri fabrikası oluşturma
> * Kopyalama etkinliği ile işlem hattı oluşturma


## <a name="prerequisites"></a>Önkoşullar
* **Azure aboneliği**. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**. Blob depolama alanını *kaynak* veri deposu olarak kullanabilirsiniz. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-account-create?tabs=azure-portal). **Depolama hesabının yalnızca ' seçili ağlardan ' erişime izin verdiğinden emin olun.** 
* **Azure SQL veritabanı**. Veritabanını *havuz* veri deposu olarak kullanabilirsiniz. Azure SQL veritabanınız yoksa, oluşturma adımları için bkz. [SQL veritabanı oluşturma](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) . **Azure SQL veritabanı hesabının yalnızca ' seçili ağlar ' 'tan erişime izin verdiğinden emin olun.** 

### <a name="create-a-blob-and-a-sql-table"></a>Bir blob ve SQL tablosu oluşturma

Aşağıdaki adımları uygulayarak öğretici için Blob depolama alanınızı ve SQL veritabanınızı hazırlayın.

#### <a name="create-a-source-blob"></a>Kaynak blob oluşturma

1. Not Defteri'ni başlatın. Aşağıdaki metni kopyalayın ve diskinizde **emp.txt** dosyası olarak kaydedin:

    ```
    FirstName,LastName
    John,Doe
    Jane,Doe
    ```

1. Blob depolama alanınızda **adftutorial** adlı bir kapsayıcı oluşturun. Bu kapsayıcıda **input** adlı bir klasör oluşturun. Sonra, **emp.txt** dosyasını **input** klasörüne yükleyin. Bu görevleri yerine getirmek için Azure Portal'ı veya [Azure Depolama Gezgini](https://storageexplorer.com/) gibi araçları kullanın.

#### <a name="create-a-sink-sql-table"></a>Havuz SQL tablosu oluşturma

1. SQL veritabanınızda **dbo.emp** tablosu oluşturmak için aşağıdaki SQL betiğini kullanın:

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

1. Azure hizmetlerinin SQL Server’a erişmesine izin ver. Data Factory’nin SQL Server’ınıza veri yazabilmesi için SQL Server’ınız için **Azure hizmetlerine erişime izin ver** ayarının **AÇIK** olduğundan emin olun. Bu ayarı doğrulamak ve etkinleştirmek için Azure SQL Server > genel bakış > sunucu güvenlik duvarını ayarla ' ya gidin> **Azure hizmetlerine erişime Izin ver** seçeneğini **Açık**olarak ayarlayın.

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, bir veri fabrikası oluşturacak ve veri fabrikasında bir işlem hattı oluşturmak için Data Factory kullanıcı arabirimini başlatacaksınız.

1. **Microsoft Edge** veya **Google Chrome**'ı açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.


2. Sol menüde, **kaynak**  >  **Analizi**  >  **Data Factory**oluştur ' u seçin.

3. **Yeni veri fabrikası** sayfasında **Ad** bölümüne **ADFTutorialDataFactory** girin.

   Azure veri fabrikasının adı *genel olarak benzersiz*olmalıdır. Ad değeriyle ilgili bir hata iletisi alırsanız, veri fabrikası için farklı bir ad girin. (örneğin, Adınızadftutorialdatafactory). Data Factory yapıtlarının adlandırma kuralları için bkz.[Data Factory adlandırma kuralları](https://docs.microsoft.com/azure/data-factory/naming-rules).

4. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğinizi** seçin.

5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:

    a. **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin.

    b. **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin. 
         
    Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](https://docs.microsoft.com/azure/azure-resource-manager/management/overview). 

6. **Sürüm** bölümünde **V2**'yi seçin.

7. **Konum** bölümünden veri fabrikası için bir konum seçin. Açılan listede yalnızca desteklenen konumlar görüntülenir. Veri fabrikası tarafından kullanılan veri depoları (örneğin, Azure Depolama ve SQL Veritabanı) ve işlemler (örneğin, Azure HDInsight) başka bölgelerde olabilir.


8. **Oluştur**’u seçin.


9. Oluşturma işlemi tamamlandıktan sonra, Bildirim Merkezi ' nde bildirimi görürsünüz. Data Factory sayfasına gitmek için **Kaynağa Git** ' i seçin.

10. Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede başlatmak için **Geliştir ve İzle**’yi seçin.

## <a name="create-an-azure-integration-runtime-in-adf-managed-virtual-network"></a>ADF tarafından yönetilen sanal ağda Azure Integration Runtime oluşturma
Bu adımda, bir Azure Integration Runtime oluşturup yönetilen sanal ağı etkinleştirirsiniz.

1. ADF portalında, **hub 'ı Yönet** ' e gidin ve yeni bir Azure Integration Runtime oluşturmak için **Yeni** ' ye tıklayın.
   ![Yeni Azure Integration Runtime oluştur](./media/tutorial-copy-data-portal-private/create-new-azure-ir.png)
2. Azure * * Integration Runtime oluşturmayı seçin.
   ![Yeni Azure Integration Runtime](./media/tutorial-copy-data-portal-private/azure-ir.png)
3. **Sanal ağı**etkinleştirin.
   ![Yeni Azure Integration Runtime](./media/tutorial-copy-data-portal-private/enable-managed-vnet.png)
4. **Oluştur**’u seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, veri fabrikasında kopyalama etkinliği ile bir işlem hattı oluşturacaksınız. Kopyalama etkinliği, verileri Blob depolama alanından SQL Veritabanı'na kopyalar. [Hızlı başlangıç eğitiminde](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal), şu adımları izleyerek bir işlem hattı oluşturdunuz:

1. Bağlı hizmeti oluşturma.
1. Giriş ve çıkış veri kümelerini oluşturma.
1. İşlem hattı oluşturma.

Bu öğreticide işlem hattını oluşturmaya başlayacaksınız. Daha sonra işlem hattını yapılandırmanız gerektiğinde bağlı hizmetleri ve veri kümelerini oluşturacaksınız.

1. **Kullanmaya başlama** sayfasında **İşlem hattı oluştur** seçeneğini belirleyin.

   ![İşlem hattı oluşturma](./media/doc-common-process/get-started-page.png)
1. İşlem hattının **Özellikler** bölmesinde, Işlem hattının **adı** için **copypipeline** girin.

1. **Etkinlikler** araç kutusunda **taşıma ve dönüştürme** kategorisini genişletin ve araç kutusundan **veri kopyalama** etkinliğini sürükleyin ve ardışık düzen Tasarımcısı yüzeyine bırakın. **Ad** için **CopyFromBlobToSql** adını belirtin.

    ![Kopyalama etkinliği](./media/tutorial-copy-data-portal-private/drag-drop-copy-activity.png)

### <a name="configure-source"></a>Kaynağı yapılandırma

>[!TIP]
>Bu öğreticide, *hesap anahtarını* kaynak veri deponuzu kimlik doğrulama türü olarak kullanacaksınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz: *SAS URI 'Si*,*hizmet sorumlusu* ve gerekirse *yönetilen kimlik* . Ayrıntılar için [Bu makaledeki](https://docs.microsoft.com/azure/data-factory/connector-azure-blob-storage#linked-service-properties) ilgili bölümlere bakın.
>Veri depolarının gizli dizilerini güvenli bir şekilde depolamak için bir Azure Key Vault kullanılması da önerilir. Ayrıntılı çizimler için [Bu makaleye](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) bakın.

#### <a name="create-source-dataset-and-linked-service"></a>Kaynak veri kümesi ve bağlı hizmet oluşturma

1. **Kaynak** sekmesine gidin. kaynak veri kümesi oluşturmak Için **+ Yeni** seçeneğini belirleyin.

1. **Yeni veri kümesi** Iletişim kutusunda **Azure Blob depolama**' yı seçin ve ardından **devam**' ı seçin. Kaynak veriler bir Blob depolama alanında olduğundan kaynak veri kümesi olarak **Azure Blob Depolama Alanı**'nı seçmeniz gerekir.

1. **Biçim Seç** iletişim kutusunda verilerinizin biçim türünü seçin ve ardından **devam**' ı seçin.

1. **Özellikleri ayarla** Iletişim kutusunda ad Için **sourceblobdataset** girin. **Üst bilgi olarak ilk satır**onay kutusunu seçin. **Bağlı hizmet** metin kutusu altında **+ Yeni**' yi seçin.

1. **Yeni bağlı hizmet (Azure Blob depolama)** Iletişim kutusunda **AzureStorageLinkedService** as Name yazın, **depolama hesabı adı** listesinden depolama hesabınızı seçin. 

1. **Etkileşimli yazma**özelliğini etkinleştirdiğinizden emin olun. 1 dakika içinde etkinleştirilmesi gerekebilir.

    ![Etkileşimli yazma](./media/tutorial-copy-data-portal-private/interactive-authoring.png)

1. **Bağlantıyı test**et ' i seçtiğinizde, depolama hesabı yalnızca ' seçili ağdan ' erişime izin veriyorsa ve bu, kullanılmadan önce onaylanması gereken özel bir uç nokta oluşturması için Azure Data Factory olması gerekir. Hata iletisinde, yönetilen özel uç nokta oluşturmak için izleyebileceğiniz **özel bir uç nokta** oluşturmak için bir bağlantı görmeniz gerekir. *Diğer bir seçenek de Yönet sekmesine doğrudan gidebileceği gibi, yönetilen bir özel uç nokta oluşturmak için [sonraki bölümdeki](#create-a-managed-private-endpoint) yönergeleri de izlemelidir*
> [!NOTE]
> Yönet sekmesi, tüm Data Factory örnekleri için kullanılamayabilir. Bunu görmüyorsanız, '**Yazar**' sekmesi aracılığıyla özel uç noktalara erişmeye devam edebilirsiniz--> '**Bağlantılar**'--> '**Özel uç nokta**'
1. İletişim kutusunu açık tutun ve yukarıda seçili depolama hesabınıza gidin.

1. Özel bağlantıyı onaylamak için [Bu bölümdeki](#approval-of-a-private-link-in-storage-account) yönergeleri izleyin.

1. İletişim kutusuna geri dönün. **Bağlantıyı yeniden test** edin ve bağlı hizmeti dağıtmak için **Oluştur** ' u seçin.

1. Bağlı hizmet oluşturulduktan sonra, **özelliği ayarla** sayfasına geri gidilmesini sağlar. **Dosya yolu**’nun yanındaki **Gözat** seçeneğini belirleyin.

1. **Adföğreticisi/giriş** klasörüne gidin, **emp.txt** dosyasını seçin ve ardından **Tamam**' ı seçin.

1. **Tamam**’ı seçin. Otomatik olarak işlem hattı sayfasına gider. **Kaynak** sekmesinde **sourceblobdataset** ' in seçili olduğunu onaylayın. Bu sayfadaki verilerin önizlemesini görüntülemek için **Veri önizleme **‘yi seçin.

    ![Kaynak veri kümesi](./media/tutorial-copy-data-portal-private/source-dataset-selected.png)

#### <a name="create-a-managed-private-endpoint"></a>Yönetilen özel uç nokta oluşturma

Yukarıdaki bağlantıyı sınarken köprüye tıklamadığınızda aşağıdaki yolu izleyin. Şimdi, yukarıda oluşturulan bağlı hizmete bağlanacak bir yönetilen özel uç noktası oluşturmanız gerekir.

1. Yönet sekmesine gidin.
> [!NOTE]
> Yönet sekmesi, tüm Data Factory örnekleri için kullanılamayabilir. Bunu görmüyorsanız, '**Yazar**' sekmesi aracılığıyla özel uç noktalara erişmeye devam edebilirsiniz--> '**Bağlantılar**'--> '**Özel uç nokta**'

1. Yönetilen özel uç noktalar bölümüne gidin.

1. Yönetilen özel uç noktalar altında **+ Yeni** ' yi seçin.

    ![Yeni yönetilen özel uç nokta](./media/tutorial-copy-data-portal-private/new-managed-private-endpoint.png) 

1. Listeden Azure Blob depolama kutucuğunu seçin ve **devam**' ı seçin.

1. Yukarıda oluşturduğunuz depolama hesabının adını girin.

1. **Oluştur**’u seçin.

1. Özel bağlantının oluşturulduğu saniye bekledikten sonra bir onay gerektiğini görmeniz gerekir.

1. Yukarıda oluşturduğunuz özel uç noktayı seçin. Özel uç noktayı, depolama hesabı düzeyinde onaylamaya yol açacak bir köprü görebilirsiniz.

    ![Özel uç noktayı Yönet](./media/tutorial-copy-data-portal-private/manage-private-endpoint.png) 

#### <a name="approval-of-a-private-link-in-storage-account"></a>Depolama hesabındaki özel bağlantının onaylanması
1. Depolama hesabında, ayarlar bölümünde **Özel uç nokta bağlantıları** ' na gidin.

1. Yukarıda oluşturduğunuz özel uç noktayı işaret edin ve **Onayla**' yı seçin.

    ![Özel uç noktayı Onayla](./media/tutorial-copy-data-portal-private/approve-private-endpoint.png)

1. Bir açıklama ekleyin ve **Evet** ' e tıklayın.
1. Azure Data Factory ' deki **Yönet** sekmesinin **yönetilen özel uç noktalar** bölümüne geri dönün.
1. Özel uç nokta Azure Data Factory Kullanıcı arabiriminde yansıtmasıyla ilgili onay almak için 1-2 dakika sürer.


### <a name="configure-sink"></a>Havuzu yapılandırma
>[!TIP]
>Bu öğreticide, havuz veri deponuzda kimlik doğrulama türü olarak *SQL kimlik doğrulamasını* kullanırsınız, ancak desteklenen diğer kimlik doğrulama yöntemlerini seçebilirsiniz: *hizmet sorumlusu* ve gerekirse *yönetilen kimlik* . Ayrıntılar için [Bu makaledeki](https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#linked-service-properties) ilgili bölümlere bakın.
>Veri depolarının gizli dizilerini güvenli bir şekilde depolamak için bir Azure Key Vault kullanılması da önerilir. Ayrıntılı çizimler için [Bu makaleye](https://docs.microsoft.com/azure/data-factory/store-credentials-in-key-vault) bakın.

#### <a name="create-sink-dataset-and-linked-service"></a>Havuz veri kümesi ve bağlı hizmet oluşturma
1. **Havuz** sekmesine gidin ve havuz veri kümesi oluşturmak için **+Yeni** seçeneğini belirleyin.

1. **Yeni veri kümesi** iletişim kutusunda, bağlayıcılar filtrelemek için arama kutusuna "SQL" girin, **Azure SQL veritabanı**' nı seçin ve ardından **devam**' ı seçin. Bu öğreticide verileri bir SQL veritabanına kopyalayacaksınız.

1. **Özellikleri ayarla** Iletişim kutusunda ad Için **outputsqldataset** girin. **Bağlı hizmet** açılan listesinden **+ Yeni**' yi seçin. Bağlı hizmeti bir veri kümesi ile ilişkilendirilmelidir. Bağlı hizmet, Data Factory’nin çalışma zamanında SQL veritabanına bağlanmak için kullandığı bağlantı dizesini içerir. Veri kümesi, verilerin kopyalanacağı kapsayıcıyı, klasörü ve dosyayı (isteğe bağlı) belirtilir.

1. **Yeni bağlı hizmet (Azure SQL veritabanı)** iletişim kutusunda aşağıdaki adımları uygulayın:

    1. **Ad** bölümüne **AzureSqlDatabaseLinkedService** girin.
    1. **Sunucu adı** bölümünde SQL Server örneğinizi seçin.
    1. **Etkileşimli yazma**özelliğini etkinleştirdiğinizden emin olun.
    1. **Veritabanı adı** bölümünde SQL veritabanınızı seçin.
    1. **Kullanıcı adı** bölümüne kullanıcının adını girin.
    1. **Parola** bölümüne kullanıcının parolasını girin.
    1. **Bağlantıyı sına**’yı seçin. SQL Server yalnızca ' seçili ağlardan ' erişime izin verdiğinden ve kendisine özel bir uç nokta oluşturmasını Azure Data Factory gerektirdiğinden (Bu, kullanılmadan önce onaylanmalıdır) başarısız olmalıdır. Hata iletisinde, yönetilen özel uç nokta oluşturmak için izleyebileceğiniz **özel bir uç nokta** oluşturmak için bir bağlantı görmeniz gerekir. *Diğer bir seçenek de Yönet sekmesine doğrudan gidebileceği gibi, yönetilen bir özel uç nokta oluşturmak için sonraki bölümdeki yönergeleri de izlemelidir*
    1. İletişim kutusunu açık tutun ve yukarıda seçili SQL Server 'a gidin.    
    1. Özel bağlantıyı onaylamak için [Bu bölümdeki](#approval-of-a-private-link-in-sql-server) yönergeleri izleyin.
    1. İletişim kutusuna geri dönün. **Bağlantıyı yeniden test** edin ve bağlı hizmeti dağıtmak için **Oluştur** ' u seçin.

1. Otomatik olarak **ayarlanan özellikler** iletişim kutusuna gider. **Tablo**’da **[dbo].[emp]** seçeneğini belirleyin. Ardından **Tamam**’ı seçin.

1. İşlem hattının bulunduğu sekmeye gidin ve **Havuz Veri Kümesi**’nde **OutputSqlDataset** seçeneğinin belirlendiğinden emin olun.

    ![İşlem hattı sekmesi](./media/tutorial-copy-data-portal-private/pipeline-tab-2.png)       

[Kopyalama etkinliğinde şema eşlemesini](https://docs.microsoft.com/azure/data-factory/copy-activity-schema-and-type-mapping)izleyerek kaynak şemasını isteğe bağlı hedef şemasına eşleyebilirsiniz.

#### <a name="create-a-managed-private-endpoint"></a>Yönetilen özel uç nokta oluşturma

Yukarıdaki bağlantıyı sınarken köprüye tıklamadığınızda aşağıdaki yolu izleyin. Şimdi, yukarıda oluşturulan bağlı hizmete bağlanacak bir yönetilen özel uç noktası oluşturmanız gerekir.

1. Yönet sekmesine gidin.
1. Yönetilen özel uç noktalar bölümüne gidin.
1. Yönetilen özel uç noktalar altında **+ Yeni** ' yi seçin.

    ![Yeni yönetilen özel uç nokta](./media/tutorial-copy-data-portal-private/new-managed-private-endpoint.png) 

1. Listeden Azure SQL veritabanı kutucuğunu seçin ve **devam**' ı seçin.
1. Yukarıda seçtiğiniz SQL Server 'ın adını girin.
1. **Oluştur**’u seçin.
1. Özel bağlantının oluşturulduğu saniye bekledikten sonra bir onay gerektiğini görmeniz gerekir.
1. Yukarıda oluşturduğunuz özel uç noktayı seçin. SQL Server düzeyinde özel uç noktayı onaylamaya yönelik bir köprü görebilirsiniz.


#### <a name="approval-of-a-private-link-in-sql-server"></a>SQL Server 'da özel bağlantı onayı
1. SQL Server 'da, ayarlar bölümünde **Özel uç nokta bağlantıları** ' na gidin.
1. Yukarıda oluşturduğunuz özel uç noktayı işaret edin ve **Onayla**' yı seçin.
1. Bir açıklama ekleyin ve **Evet**' e tıklayın.
1. Azure Data Factory ' deki **Yönet** sekmesinin **yönetilen özel uç noktalar** bölümüne geri dönün.
1. Onayın özel uç noktanıza yansıtılmasıyla ilgili olarak 1-2 dakika sürer.

#### <a name="debug-and-publish-the-pipeline"></a>İşlem hattında hata ayıklama ve işlem hattını yayımlama

Yapıtları (bağlı hizmetler, veri kümeleri ve işlem hattı) Data Factory'de veya kendi Azure Repos Git deponuzda yayımlamadan önce işlem hattında hata ayıklayabilirsiniz.

1. İşlem hattında hata ayıklamak için araç çubuğunda **Hata Ayıkla**'yı seçin. Pencerenin altındaki **Çıkış** sekmesinde işlem hattı çalıştırmasının durumu görüntülenir.
2. İşlem hattı başarıyla çalıştırıldığında, üstteki araç çubuğunda **Tümünü Yayımla**' yı seçin. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve işlem hatları) Data Factory'de yayımlar.
3. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sağ üst köşedeki **bildirimleri göster** (zil düğmesi) seçeneğine tıklayın.


#### <a name="summary"></a>Özet
Bu örnekteki işlem hattı, yönetilen sanal ağda özel uç nokta kullanarak blob depolamadan verileri Azure SQL VERITABANı 'na kopyalar. Şunları öğrendiniz:

> * Veri fabrikası oluşturma
> * Kopyalama etkinliği ile işlem hattı oluşturma

