---
title: Verileri taşıma - Veri Yönetimi Ağ Geçidi
description: Verileri şirket içi ve bulut arasında taşımak için bir veri ağ geçidi ayarlayın. Verilerinizi taşımak için Azure Veri Fabrikası'nda Veri Yönetimi Ağ Geçidi'ni kullanın.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
ms.assetid: 7bf6d8fd-04b5-499d-bd19-eff217aa4a9c
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: be797f76988c924503e11b6f66cce899b515e3a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75982193"
---
# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi ile verileri şirket içi kaynaklar la bulut arasında taşıma
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'nı kullanarak şirket içi ve bulut arasındaki verileri kopyala'ya](../tutorial-hybrid-copy-powershell.md)bakın.

Bu makalede, Veri Fabrikası'nı kullanarak şirket içi veri depoları ile bulut veri depoları arasındaki veri tümleştirmesine genel bir bakış sağk. [Veri Hareketi Etkinlikleri](data-factory-data-movement-activities.md) makale ve diğer veri fabrikası çekirdek kavramları makaleleri üzerine inşa: veri [kümeleri](data-factory-create-datasets.md) ve [boru hatları.](data-factory-create-pipelines.md)

## <a name="data-management-gateway"></a>Veri Yönetimi Ağ Geçidi
Şirket içi veri deposuna/şirket içi veri deposuna veri taşımayı etkinleştirmek için şirket içi makinenize Veri Yönetimi Ağ Geçidi yüklemeniz gerekir. Ağ geçidi, ağ geçidi veri deposuna bağlanabildiği sürece veri deposuyla aynı makineye veya farklı bir makineye yüklenebilir.

> [!IMPORTANT]
> Veri Yönetimi Ağ Geçidi hakkında ayrıntılar için [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesine bakın.

Aşağıdaki izbin, verileri şirket içi **BIR SQL Server** veritabanından Azure blob depolamasına aktaran bir ardışık yapı yla nasıl bir veri fabrikası oluşturabileceğinizi gösterir. Adım adım kılavuzun bir parçası olarak makinenize Veri Yönetimi Ağ Geçidi yükleyip bunu yapılandıracaksınız.

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Walkthrough: şirket içi verileri buluta kopyalama
Bu izne aşağıdaki adımları yapın:

1. Veri fabrikası oluşturma.
2. Bir veri yönetimi ağ geçidi oluşturun.
3. Kaynak ve lavabo veri depoları için bağlantılı hizmetler oluşturun.
4. Giriş ve çıktı verilerini temsil edecek veri kümeleri oluşturun.
5. Verileri taşımak için kopyalama etkinliği ile işlem hattı oluşturma.

## <a name="prerequisites-for-the-tutorial"></a>Öğretici için ön koşullar
Bu izlenme yi başlatmadan önce aşağıdaki ön koşullara sahip olmalısınız:

* **Azure aboneliği.**  Bir aboneliğiniz yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için [Ücretsiz Deneme](https://azure.microsoft.com/pricing/free-trial/) makalesine bakın.
* **Azure Depolama Hesabı**. Bu öğreticide blob depolamaalanını **hedef/lavabo** veri deposu olarak kullanırsınız. Azure depolama hesabınız yoksa, bir tane oluşturma adımları için [bir depolama hesabı oluştur](../../storage/common/storage-account-create.md) makalesine bakın.
* **SQL Server**. Bu öğreticide şirket içi SQL Server veritabanını bir **kaynak** veri deposu olarak kullanırsınız.

## <a name="create-data-factory"></a>Veri fabrikası oluşturma
Bu adımda, **ADFTutorialOnPremDF**adlı bir Azure Veri Fabrikası örneği oluşturmak için Azure portalını kullanırsınız.

1. [Azure portalına](https://portal.azure.com)giriş yapın.
2. **Kaynak Oluştur'u**tıklatın, **Intelligence + analytics'i**tıklatın ve **Veri Fabrikası'nı**tıklatın.

   ![Yeni->DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
3. Yeni **veri fabrikası** sayfasında, Ad için **ADFTutorialOnPremDF** girin.

    ![Başlangıç Panosuna Ekle](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

   > [!IMPORTANT]
   > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Hata alırsanız: **Veri fabrikası adı "ADFTutorialOnPremDF" mevcut değil,** veri fabrikasının adını değiştirin (örneğin, adınızıADFTutorialOnPremDF) ve yeniden oluşturmayı deneyin. Bu öğreticide kalan adımları gerçekleştirirken ADFTutorialOnPremDF yerine bu adı kullanın.
   >
   > Veri fabrikasının adı gelecekte bir **DNS** adı olarak kaydedilebilir ve bu nedenle genel olarak görünür hale gelir.
   >
   >
4. Data factory’yi oluşturmak istediğiniz **Azure aboneliği**’ni seçin.
5. Mevcut bir **kaynak grubu** seçin ya da bir kaynak grubu oluşturun. Öğretici için, adlı bir kaynak grubu oluşturun: **ADFTutorialResourceGroup**.
6. Yeni veri **fabrikası** sayfasında **Oluştur'u** tıklatın.

   > [!IMPORTANT]
   > Veri Fabrikası örnekleri oluşturmak için, abonelik/kaynak grubu düzeyinde [Veri Fabrikası Katılımcısı](../../role-based-access-control/built-in-roles.md#data-factory-contributor) rolünün bir üyesi olmalısınız.
   >
   >
7. Oluşturma tamamlandıktan sonra, aşağıdaki resimde gösterildiği gibi **Veri Fabrikası** sayfasını görürsünüz:

   ![Veri Fabrikası Giriş Sayfası](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Ağ geçidi oluşturma
1. Veri **Fabrikası** sayfasında, Veri Fabrikası için **Düzenleyici'yi** başlatmak için **Yazar'ı tıklatın ve** döşemeyi dağıtın.

    ![Geliştir ve Dağıt Kutucuğu](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2. Veri Fabrikası Düzenleyicisi'nde... ** **Araç çubuğu hakkında daha fazla bilgi ve ardından **Yeni veri ağ geçidi'ni**tıklatın. Alternatif olarak, ağaç görünümünde **Veri Ağ Geçitleri'ni** sağ tıklatabilir ve **Yeni veri ağ geçidini**tıklatabilirsiniz.

   ![Araç çubuğunda yeni veri ağ geçidi](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
3. **Oluştur** sayfasında, **ad**için **adftutorialgateway** girin ve **Tamam'ı**tıklatın.     

    ![Ağ Geçidi sayfası oluşturma](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

    > [!NOTE]
    > Bu walkthrough'ta, yalnızca bir düğüm (şirket içi Windows makinesi) ile mantıksal ağ geçidi oluşturursunuz. Birden çok şirket içi makineyi ağ geçidiyle ilişkilendirerek bir veri yönetimi ağ geçidini ölçeklendirebilirsiniz. Bir düğüm üzerinde aynı anda çalıştırılabilen veri hareketi işlerinin sayısını artırarak ölçeklendirebilirsiniz. Bu özellik, tek bir düğüme sahip mantıksal bir ağ geçidi için de kullanılabilir. Ayrıntılar için [Azure Veri Fabrikası makalesinde veri yönetimi ağ geçidini ölçekleme](data-factory-data-management-gateway-high-availability-scalability.md) makalesine bakın.  
4. **Yapılandırılan** sayfada, **doğrudan bu bilgisayara yükle'yi**tıklatın. Bu eylem ağ geçidi için yükleme paketini karşıdan yükler, yükler, yapılandırır ve bilgisayarda ağ geçidini kaydeder.  

   > [!NOTE]
   > Internet Explorer veya Microsoft ClickOnce uyumlu bir web tarayıcısı kullanın.
   >
   > Chrome kullanıyorsanız, [Chrome web mağazasına](https://chrome.google.com/webstore/) gidin, “ClickOnce” araması yapın, ClickOnce uzantılarından birini seçin ve yükleyin.
   >
   > Firefox için de aynısını yapın (eklentiyi yükleyin). Araç çubuğundaki **Menüyü Aç** düğmesini (sağ üst köşedeki**üç yatay çizgi)** tıklatın, **Eklentiler'e**tıklayın, "ClickOnce" anahtar sözcüğüyle arama yapın, ClickOnce uzantılarından birini seçin ve yükleyin.    
   >
   >

    ![Ağ Geçidi - Sayfayı yapılandır](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Bu şekilde, ağ geçidini tek bir adımda indirmenin, yüklemenin, yapılandırmanın ve kaydetmenin en kolay yolu (tek tıklamayla) olur. Bilgisayarınızda Microsoft **Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi** uygulamasının yüklü olduğunu görebilirsiniz. Ayrıca yürütülebilir **ConfigManager.exe** klasöründe bulabilirsiniz: **C:\Program Files\Microsoft Veri Yönetimi Ağ Geçidi\2.0\Paylaşılan**.

    Ayrıca, bu sayfadaki bağlantıları kullanarak ağ geçidini el ile indirebilir ve yükleyebilir ve **Yenİ KEY** metin kutusunda gösterilen anahtarı kullanarak kaydedebilirsiniz.

    Ağ geçidi yle ilgili tüm ayrıntılar için [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesine bakın.

   > [!NOTE]
   > Veri Yönetimi Ağ Geçidi'ni başarıyla yüklemek ve yapılandırmak için yerel bilgisayarda yönetici olmalısınız. **Veri Yönetimi Ağ Geçidi Kullanıcıları** yerel Windows grubuna ek kullanıcılar ekleyebilirsiniz. Bu grubun üyeleri ağ geçidini yapılandırmak için Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi aracını kullanabilir.
   >
   >
5. Birkaç dakika bekleyin veya aşağıdaki bildirim iletisini görene kadar bekleyin:

    ![Ağ geçidi yüklemesi başarılı](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png)
6. **Bilgisayarınızda Veri Yönetimi Ağ Geçidi Configuration Manager** uygulamasını başlatın. **Arama** penceresinde, bu yardımcı programa erişmek için **Veri Yönetimi Ağ Geçidi** yazın. Ayrıca yürütülebilir **ConfigManager.exe** klasöründe bulabilirsiniz: **C:\Program Files\Microsoft Veri Yönetimi Ağ Geçidi\2.0\Paylaşılan**

    ![Ağ Geçidi Yapılandırma Yöneticisi](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
7. İletiyi gördüğünüzden `adftutorialgateway is connected to the cloud service` onaylayın. Alttaki durum çubuğu, bulut **hizmetine bağlı** yeşil **onay işaretiyle**birlikte görüntülenir.

    Ana **Sayfa** sekmesinde, aşağıdaki işlemleri de yapabilirsiniz:

   * **Register** Kaydol düğmesini kullanarak Azure portalından bir anahtarla bir ağ geçidi kaydedin.
   * Ağ geçidi makinenizde çalışan Veri Yönetimi Ağ Geçidi Ana Bilgisayarı Hizmetini **durdurun.**
   * Günün belirli bir saatinde yüklenecek **güncelleştirmeleri zamanlama.**
   * Ağ geçidinin **en son ne**zaman güncelleştirileni görüntüleyin.
   * Ağ geçidine güncelleştirmenin yüklenebileceği zamanı belirtin.
8. **Ayarlar** sekmesine geçin. **Sertifika** bölümünde belirtilen sertifika, portalda belirttiğiniz şirket içi veri deposunun kimlik bilgilerini şifrelemek/çözmek için kullanılır. (isteğe bağlı) Bunun yerine kendi sertifikanızı kullanmak için **Değiştir'i** tıklatın. Varsayılan olarak, ağ geçidi Veri Fabrikası hizmeti tarafından otomatik olarak oluşturulan sertifikayı kullanır.

    ![Ağ geçidi sertifikası yapılandırması](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    **Ayarlar** sekmesinde aşağıdaki işlemleri de yapabilirsiniz:

   * Ağ geçidi tarafından kullanılan sertifikayı görüntüleyin veya dışa aktarın.
   * Ağ geçidi tarafından kullanılan HTTPS bitiş noktasını değiştirin.    
   * Ağ geçidi tarafından kullanılmak üzere bir HTTP proxy'si ayarlayın.     
9. (isteğe bağlı) **Tanılama** sekmesine geçin, ağ geçidiyle ilgili sorunları gidermek için kullanabileceğiniz ayrıntılı günlüğe kaydetmeyi etkinleştirmek istiyorsanız **verbose günlüğe** kaydetme seçeneğini işaretleyin. Günlük **bilgileri, Uygulamalar ve Hizmetler Günlükleri** -> **Veri Yönetimi Ağ Geçidi** düğümü altında Olay **Görüntüleyici'de** bulunabilir.

    ![Tanılama sekmesi](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    **Tanılama** sekmesinde aşağıdaki eylemleri de gerçekleştirebilirsiniz:

   * Ağ geçidini kullanarak şirket içi bir veri kaynağıiçin **Test Bağlantısı** bölümünü kullanın.
   * Olay Görüntüleyicisi penceresinde Veri Yönetimi Ağ Geçidi günlüğünü görmek için **Günlükleri** Görüntüle'yi tıklatın.
   * Sorunlarınızın giderilmesini kolaylaştırmak için son yedi günlükleri içeren bir zip dosyasını Microsoft'a yüklemek için **Günlük gönder'i** tıklatın.
10. **Tanılama** sekmesinde, **Test Bağlantısı** bölümünde, veri deposunun türü için **SqlServer'ı** seçin, veritabanı sunucusunun adını, veritabanının adını girin, kimlik doğrulama türünü belirtin, kullanıcı adı ve parola girin ve ağ geçidinin veritabanına bağlanıp bağlanamayacağını test etmek için **Test'i** tıklatın.
11. Web tarayıcısına ve Azure **portalına**geç, **Yapıl'da** Tamam'ı ve ardından **Yeni veri ağ geçidi** sayfasında **tamam'ı** tıklatın.
12. Soldaki ağaç görünümünde **Veri Ağ Geçitleri** altında **adftutorialgateway** görmelisiniz.  Tıklarsanız, ilişkili JSON görmeniz gerekir.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda, iki bağlantılı hizmet oluşturursunuz: **AzureStorageLinkedService** ve **SqlServerLinkedService.** **SqlServerLinkedService** şirket içi BIR SQL Server veritabanını ve **AzureStorageLinkedService** bağlantılı hizmeti bir Azure blob deposunu veri fabrikasına bağlar. Daha sonra bu izbin de şirket içi SQL Server veritabanından Azure blob deposuna verileri kopyalayan bir ardışık kaynak oluşturursunuz.

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Şirket içi SQL Server veritabanına bağlantılı hizmet ekleme
1. Veri **Fabrikası Düzenleyicisi'nde,** araç çubuğunda **Yeni veri deposunu** tıklatın ve **SQL Server'ı**seçin.

   ![Yeni SQL Server bağlantılı hizmet](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
2. Sağdaki **JSON editöründe** aşağıdaki adımları yapın:

   1. **GatewayName**için **adftutorialgateway**belirtin.    
   2. **ConnectionString'de**aşağıdaki adımları yapın:    

      1. **Sunucu adı**için, SQL Server veritabanını barındıran sunucunun adını girin.
      2. **Veritabanı adı**için, veritabanının adını girin.
      3. Araç çubuğundaki **Şifrele** düğmesini tıklatın. Kimlik Bilgileri Yöneticisi uygulamasını görürsünüz.

         ![Kimlik Bilgileri Yöneticisi uygulaması](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
      4. Kimlik **Bilgilerini Ayarlama** iletişim kutusunda kimlik doğrulama türünü, kullanıcı adını ve parolayı belirtin ve **Tamam'ı**tıklatın. Bağlantı başarılı olursa, şifrelenmiş kimlik bilgileri JSON'da depolanır ve iletişim kutusu kapanır.
      5. Otomatik olarak kapatılmazsa iletişim kutusunu başlatan boş tarayıcı sekmesini kapatın ve Azure portalı yla sekmeye geri dön.

         Ağ geçidi makinesinde, bu kimlik bilgileri Veri Fabrikası hizmetinin sahip olduğu bir sertifika kullanılarak **şifrelenir.** Bunun yerine Veri Yönetimi Ağ Geçidi ile ilişkili sertifikayı kullanmak istiyorsanız, kimlik bilgilerini güvenli bir şekilde ayarla'ya bakın.    
   3. SQL Server bağlantılı hizmeti dağıtmak için komut çubuğunda **Dağıt'ı** tıklatın. Ağaca görünümde bağlantılı hizmeti görmeniz gerekir.

      ![Ağaç görünümünde SQL Server bağlantılı hizmet](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)    

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Azure depolama hesabı için bağlantılı hizmet ekleme
1. Veri **Fabrikası Düzenleyicisi'nde**komut çubuğundaki **Yeni veri deposunu** tıklatın ve **Azure depolama alanını**tıklatın.
2. **Hesap adı**için Azure depolama hesabınızın adını girin.
3. **Hesap anahtarı**için Azure depolama hesabınız için anahtarı girin.
4. **AzureStorageLinkedService'i**dağıtmak için **Dağıt'ı** tıklatın.

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu adımda, kopyalama işlemi için girdi ve çıktı verilerini temsil eden girdi ve çıktı veri kümeleri oluşturursunuz (Şirket içi SQL Server veritabanı = > Azure blob depolama). Veri kümeleri oluşturmadan önce aşağıdaki adımları uygulayın (listeden sonra ayrıntılı adımlar verilir):

* Veri fabrikasına bağlı hizmet olarak eklediğiniz SQL Server Veritabanında **emp** adlı bir tablo oluşturun ve tabloya birkaç örnek giriş ekleyin.
* Veri fabrikasına bağlı hizmet olarak eklediğiniz Azure blob depolama hesabında **adftutorial** adlı bir blob kapsayıcı oluşturun.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Öğretici için şirket içi SQL Server'ı hazırlayın
1. Şirket içi SQL Server bağlı hizmeti için belirttiğiniz veritabanında (**SqlServerLinkedService**), aşağıdaki SQL betiğini kullanarak veritabanında **emp** tablosunu oluşturun.

    ```SQL   
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
        CONSTRAINT PK_emp PRIMARY KEY (ID)
    )
    GO
    ```
2. Tabloya bazı örnekler ekleyin:

    ```SQL
    INSERT INTO emp VALUES ('John', 'Doe')
    INSERT INTO emp VALUES ('Jane', 'Doe')
    ```

### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma

1. Veri **Fabrikası Editörü**olarak , tıklayın **... Daha fazla,** komut çubuğunda **Yeni veri kümesini** tıklatın ve **SQL Server tablosunu**tıklatın.
2. JSON'u sağ bölmedeki aşağıdaki metinle değiştirin:

    ```JSON   
    {        
        "name": "EmpOnPremSQLTable",
        "properties": {
            "type": "SqlServerTable",
            "linkedServiceName": "SqlServerLinkedService",
            "typeProperties": {
                "tableName": "emp"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }     
    ```     
   Aşağıdaki noktalara dikkat edin:

   * **türü** **SqlServerTable**olarak ayarlanır.
   * **tableName** **emp**olarak ayarlanır.
   * **linkedServiceName** **SqlServerLinkedService** olarak ayarlanır (bu bağlantılı hizmeti bu geçiş te daha önce oluşturmuştunuz.).
   * Azure Veri Fabrikası'nda başka bir ardışık ardışık hatlar tarafından oluşturulmayan bir giriş veri kümesi **için, dış** ait **olan doğru'ya**ayarlamanız gerekir. Azure Veri Fabrikası hizmetinin dışında üretilen girdi verilerini gösterir. **İlke** bölümündeharici Data öğesini kullanarak herhangi bir **dış** veri ilkesini isteğe bağlı olarak belirtebilirsiniz.    

   JSON özellikleri yle ilgili ayrıntılar için [verileri SQL Server'a/sin'den taşıyın.](data-factory-sqlserver-connector.md)
3. Veri kümesini dağıtmak için komut çubuğunda **Dağıt'ı** tıklatın.  

### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma

1. Veri **Fabrikası Düzenleyicisi'nde**komut çubuğundaki **Yeni veri kümesini** tıklatın ve **Azure Blob depolama alanını**tıklatın.
2. JSON'u sağ bölmedeki aşağıdaki metinle değiştirin:

    ```JSON   
    {
        "name": "OutputBlobTable",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/outfromonpremdf",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
     }
    ```   
   Aşağıdaki noktalara dikkat edin:

   * **türü** **AzureBlob**olarak ayarlanır.
   * **linkedServiceName** **AzureStorageLinkedService** olarak ayarlanır (bu bağlantılı hizmeti Adım 2'de oluşturmuştunuz).
   * **folderPath** **adftutorial/outfromonpremdf** olarak ayarlanır ve outfromonpremdf adftutorial kapsayıcısındaki klasördür. Zaten yoksa **adftutorial** kapsayıcıoluşturun.
   * **Kullanılabilirlik** **saatlik** olarak ayarlanır **(frekans** **saat** ve **aralık** **olarak**1 olarak ayarlanır).  Veri Fabrikası hizmeti, Azure SQL Veritabanı'ndaki **emp** tablosunda her saat başı bir çıktı veri dilimi oluşturur.

   Bir **çıktı tablosu**için bir **dosya Adı** belirtmezseniz, **folderPath'de** oluşturulan dosyalar `Data.<Guid>.txt` şu biçimde adlandırılır: (örneğin: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

   **folderPath** ve **fileName** öğelerini dinamik olarak **SliceStart** zamanı temelinde ayarlamak için partitionedBy özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName ise SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2014-10-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2014/10/20, fileName de 08.csv olarak ayarlanır.

    ```JSON
    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy":
    [

        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
    ],
    ```

    JSON özellikleri yle ilgili ayrıntılar için [verileri Azure Blob Depolama'ya taşıyın/buradan](data-factory-azure-blob-connector.md) taşıyın.
3. Veri kümesini dağıtmak için komut çubuğunda **Dağıt'ı** tıklatın. Ağaç görünümünde her iki veri kümesini de gördüğünüzden onaylayın.  

## <a name="create-pipeline"></a>İşlem hattı oluşturma
Bu adımda, giriş olarak **EmpOnPremSQLTable** ve çıktı olarak **OutputBlobTable** kullanan bir **Kopyalama Etkinliği** ile bir **ardışık işlem oluşturmak.**

1. Veri Fabrikası Düzenleyicisi'nde... ** Daha fazla**ve **Yeni ardışık tıklatın.**
2. JSON'u sağ bölmedeki aşağıdaki metinle değiştirin:    

    ```JSON   
     {
         "name": "ADFTutorialPipelineOnPrem",
         "properties": {
         "description": "This pipeline has one Copy activity that copies data from an on premises SQL to Azure blob",
         "activities": [
           {
             "name": "CopyFromSQLtoBlob",
             "description": "Copy data from on premises SQL server to blob",
             "type": "Copy",
             "inputs": [
               {
                 "name": "EmpOnPremSQLTable"
               }
             ],
             "outputs": [
               {
                 "name": "OutputBlobTable"
               }
             ],
             "typeProperties": {
               "source": {
                 "type": "SqlSource",
                 "sqlReaderQuery": "select * from emp"
               },
               "sink": {
                 "type": "BlobSink"
               }
             },
             "Policy": {
               "concurrency": 1,
               "executionPriorityOrder": "NewestFirst",
               "style": "StartOfInterval",
               "retry": 0,
               "timeout": "01:00:00"
             }
           }
         ],
         "start": "2016-07-05T00:00:00Z",
         "end": "2016-07-06T00:00:00Z",
         "isPaused": false
       }
     }
    ```   
   > [!IMPORTANT]
   > **start** özelliğinin değerini geçerli günle, **end** değerini de sonraki günle değiştirin.
   >
   >

   Aşağıdaki noktalara dikkat edin:

   * Etkinlikler bölümünde yalnızca **türü** **Kopyala**olarak ayarlanmış etkinlik vardır.
   * Etkinlik **girişi** **EmpOnPremSQLTable** olarak ayarlanır ve etkinlik için **çıktı** **OutputBlobTable**olarak ayarlanır.
   * **typeProperties** bölümünde **SqlSource** **kaynak türü** olarak belirtilir ve **BlobSink** **lavabo türü**olarak belirtilir.
   * `select * from emp` **SQLSource'un** **sqlReaderQuery** özelliği için SQL sorgusu belirtilir.

   Başlangıç ve bitiş tarih saatleri [ISO biçiminde](https://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2014-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız.

   **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9/9/9999** olarak ayarlayın.

   Her Azure Veri Fabrikası veri kümesi için tanımlanan **Kullanılabilirlik** özelliklerine göre veri dilimlerinin işlendiği süreyi tanımlıyorsunuz.

   Örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.        
3. Veri kümesini dağıtmak için komut çubuğunda **Dağıt'ı** tıklatın (tablo dikdörtgen bir veri kümesidir). **Ardışık** ardışık ardışık düğüm altında ağaç görünümünde gösterir doğrulayın.  
4. Şimdi, **ADFTutorialOnPremDF**için **Veri Fabrikası** sayfasına geri almak için sayfayı kapatmak için **x'i** iki kez tıklatın.

**Tebrikler!** Bir Azure veri fabrikası, bağlantılı hizmetler, veri kümeleri ve bir ardışık hatlar oluşturdunuz ve ardışık zamanlandı.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Data factory’yi Diyagram Görünümünde görüntüleme
1. Azure **portalında,** **ADFTutorialOnPremDF** veri fabrikasının ana **sayfasındaki Diyagram** döşemesini tıklatın. :

    ![Diyagram Bağlantısı](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)
2. Aşağıdaki görüntüye benzer bir diyagram görmeniz gerekir:

    ![Diyagram Görünümü](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Yakınlaştırabilir, yakınlaştırabilir, %100'e yakınlaştırabilir, sığdırabilirsiniz, ardışık alt hatları ve veri kümelerini otomatik olarak konumlandırabilir ve soy bilgilerini gösterebilirsiniz (seçili öğelerin yukarı ve aşağı öğelerini vurgular).  Özelliklerini görmek için bir nesneyi (giriş/çıktı veri kümesi veya ardışık) çift tıklatabilirsiniz.

## <a name="monitor-pipeline"></a>İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure Portal kullanacaksınız. Veri kümelerini ve işlem hatlarını izlemek için de PowerShell cmdlet'lerini kullanabilirsiniz. İzleme hakkında ayrıntılı bilgi [için, Bkz. İzleme ve Yönet Boru Hatları.](data-factory-monitor-manage-pipelines.md)

1. Diyagramda **EmpOnPremSQLTable'ı**çift tıklatın.  

    ![EmpOnPremSQLTable dilimleri](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)
2. Ardışık hatlar süresi (bitiş saatine başlangıç zamanı) geçmişte olduğundan, tüm veri dilimlerinin **Hazır** durumunda olduğuna dikkat edin. Ayrıca, verileri SQL Server veritabanına eklemeniz ve her zaman orada olmasıdır. Alttaki **Sorun dilimleri** bölümünde hiçbir dilimin görünmediğini doğrulayın. Tüm dilimleri görüntülemek için dilimlistesinin altındaki **Daha Fazlayı Gör'ü** tıklatın.
3. Şimdi, **Datasets** **sayfasında, OutputBlobTable'ı**tıklatın.

    ![OputputBlobTable dilimleri](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
4. Listeden herhangi bir veri dilimini tıklatın ve **Veri Dilimi** sayfasını görmeniz gerekir. Dilim için etkinlik çalışır bakın. Genellikle yalnızca bir etkinlik çalışır.  

    ![Veri Dilim Bıçağı](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Dilim **Hazır** durumunda değilse, Hazır olmayan ve geçerli dilimin yürütülmesini engelleyen yukarı akış dilimlerini **Hazır olmayan yukarı akış dilimleri** listesinde görebilirsiniz.
5. Etkinlik **çalıştırma** ayrıntılarını görmek için alttaki listeden etkinlik **çalıştır'ı**tıklatın.

   ![Etkinlik Çalıştır Ayrıntıları sayfası](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

   Veri aktarımı, süre ve verileri aktarmak için kullanılan ağ geçidi gibi bilgileri görürsünüz.
6. Tüm sayfaları kapatmak için **X'i** tıklatın
7. **ADFTutorialOnPremDF**için ana sayfaya geri dön.
8. (isteğe bağlı) **Boru Hatları'nı**tıklatın, **ADFTutorialOnPremDF'yi**tıklatın ve giriş tablolarını **(Tüketilen)** veya çıktı veri kümelerini delin (**Üretilir).**
9. Her saat için bir blob/dosya oluşturulduğunu doğrulamak için [Microsoft Storage Explorer](https://storageexplorer.com/) gibi araçları kullanın.

   ![Azure Storage Gezgini](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Sonraki adımlar
* Veri Yönetimi Ağ Geçidi ile ilgili tüm ayrıntılar için [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) makalesine bakın.
* Verileri kaynak veri deposundan lavabo veri deposuna taşımak için Kopyalama Etkinliği'ni nasıl kullanacağınızı öğrenmek için [Azure Blob'dan Azure SQL'e verileri](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kopyala'ya bakın.
