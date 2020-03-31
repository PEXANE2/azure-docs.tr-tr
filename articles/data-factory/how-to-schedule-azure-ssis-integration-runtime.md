---
title: Azure-SSIS Tümleştirme Tümleştirme Çalışma Süresi nasıl zamanlanır?
description: Bu makalede, Azure Veri Fabrikası'nı kullanarak Azure-SSIS Tümleştirme Çalışma Zamanı'nın başlangıç ve durdurulmasınasıl zamanlanır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 8/2/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: anandsub
ms.openlocfilehash: 5263af2708ee30566e90cdf59ef69f52f76a9d32
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75440311"
---
# <a name="how-to-start-and-stop-azure-ssis-integration-runtime-on-a-schedule"></a>Azure-SSIS Integration Runtime'ı belirli bir zamanlamaya göre başlatma ve durdurma
Bu makalede, Azure Veri Fabrikası 'nı (ADF) kullanarak Azure-SSIS Tümleştirme Çalışma Zamanı'nın (IR) başlangıç ve durdurma zamanlanması nasıl zamanlanır. Azure-SSIS IR, SQL Server Tümleştirme Hizmetleri (SSIS) paketlerini yürütmek için ayrılmış ADF bilgi işlem kaynağıdır. Azure-SSIS IR'nin çalıştırılanın bununla ilişkili bir maliyeti vardır. Bu nedenle, IR'nizi yalnızca Azure'da SSIS paketlerini yürütmeniz gerektiğinde çalıştırmak ve artık ihtiyacınız olmadığında IR'nizi durdurmak istersiniz. [IR'nizi el ile başlatmak veya durdurmak](manage-azure-ssis-integration-runtime.md)için ADF Kullanıcı Arabirimi (UI)/uygulaması veya Azure PowerShell'i kullanabilirsiniz).

Alternatif olarak, IR'nizi zamanında başlatmak/durdurmak için ADF ardışık işaklerinde Web etkinlikleri oluşturabilir, örneğin günlük ETL iş yüklerinizi çalıştırmadan önce sabah başlatabilir ve yapıldıktan sonra öğleden sonra durdurabilirsiniz.  Ir'nizi başlatan ve durduran iki Web etkinliği arasında bir Execute SSIS Paketi etkinliğini de zincirleyebilirsiniz, böylece IR'niz paket yürütmenizden hemen önce/sonra isteğe bağlı olarak başlar/durur. Execute SSIS Paketi etkinliği hakkında daha fazla bilgi için [bkz.](how-to-invoke-ssis-package-ssis-activity.md)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar
Azure-SSIS IR'nizi zaten sağlamadıysanız, öğreticideki talimatları izleyerek bunu [uygulayın.](tutorial-create-azure-ssis-runtime-portal.md) 

## <a name="create-and-schedule-adf-pipelines-that-start-and-or-stop-azure-ssis-ir"></a>Azure-SSIS IR'yi başlatan ve durduran ADF ardışık hatlar oluşturma ve zamanlama
Bu bölümde, Azure-SSIS IR'nizi zamanında başlatmak/durdurmak veya isteğe bağlı olarak durdurmak & başlatmak için ADF ardışık işdizilerinde Web etkinliklerini nasıl kullanacağınızı gösterir. Üç boru hattı oluşturmanız için size rehberlik edeceğiz: 

1. İlk ardışık iş, Azure-SSIS IR'nizi başlatan bir Web etkinliği içerir. 
2. İkinci ardışık alan, Azure-SSIS IR'nizi durduran bir Web etkinliği içerir.
3. Üçüncü ardışık iş, Azure-SSIS IR'nizi başlatan/durduran iki Web etkinliği arasında zincirlenmiş bir Execute SSIS Paketi etkinliği içerir. 

Bu ardışık lıkları oluşturduktan ve test ettikten sonra, bir zamanlama tetikleyicisi oluşturabilir ve herhangi bir ardışık lıkla ilişkilendirebilirsiniz. Zamanlama tetikleyicisi ilişkili ardışık alanı çalıştırmak için bir zamanlama tanımlar. 

Örneğin, iki tetikleyici oluşturabilirsiniz, ilki günlük olarak 6 ve ilk ardışık durumla ilişkili olarak zamanlanırken, ikincisi günlük olarak 18:00'de çalışacak ve ikinci ardışık alanla ilişkili olarak zamanlanır.  Bu şekilde, IR'niz çalışırken her gün 06:00 ile 18:00 arasında günlük ETL iş yüklerinizi çalıştırmaya hazır bir süreniz vardır.  

Her gün gece yarısı çalışması planlanan ve üçüncü ardışık işlem le ilişkili üçüncü bir tetikleyici oluşturursanız, bu ardışık işlem her gün gece yarısı çalışır, paket yürütülmesinden hemen önce IR'nizi başlatır, ardından paketinizi çalıştırır ve hemen paket yürütülmesinden hemen sonra IR'nizi durdurur, böylece IR'niz boş çalışmaz.

### <a name="create-your-adf"></a>ADF'nizi oluşturun

1. [Azure portalında](https://portal.azure.com/)oturum açın.    
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
   
3. Yeni **veri fabrikası** sayfasına, **MyAzureSsisDataFactory** for **Name'i**girin. 
      
   ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   ADF'nizin adı genel olarak benzersiz olmalıdır. Aşağıdaki hatayı alırsanız, ADF'nizin adını (örn. adınızMyAzureSsisDataFactory) değiştirin ve yeniden oluşturmayı deneyin. Bkz. [Veri Fabrikası -](naming-rules.md) ADF yapıtlarının adlandırma kuralları hakkında bilgi edinmek için Adlandırma Kuralları makalesi.
  
   `Data factory name MyAzureSsisDataFactory is not available`
      
4. ADF'nizi oluşturmak istediğiniz Azure **Aboneliğinizi** seçin. 
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - **Yeni Oluştur'u**seçin ve yeni kaynak grubunuzun adını girin.   
         
   Kaynak grupları hakkında bilgi edinmek için Azure [kaynakları makalenizi yönetmek için kaynak gruplarını kullanma'ya](../azure-resource-manager/management/overview.md) bakın.
   
6. **Sürüm**için **V2'yi** seçin.
7. **Konum**için açılan listeden ADF oluşturma için desteklenen konumlardan birini seçin.
8. **Panoya sabitle**’yi seçin.     
9. **Oluştur'u**tıklatın.
10. Azure panosunda, durumiçeren aşağıdaki döşemeyi görürsünüz: **Veri Fabrikası'nı dağıtma.** 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
   
11. Oluşturma tamamlandıktan sonra, ADF sayfanızı aşağıda gösterildiği gibi görebilirsiniz.
   
    ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
   
12. ADF Kullanıcı İyiu/uygulamasını ayrı bir sekmede başlatmak için **Yazar & Monitör'ü** tıklatın.

### <a name="create-your-pipelines"></a>Boru hatlarınızı oluşturun

1. **Başlatalım** **sayfasında, ardışık hat oluştur'u**seçin. 

   ![Başlarken sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
   
2. **Etkinlikler** araç kutusunda, **Genel** menüyü genişletin ve bir **Web** etkinliğini boru hattı tasarımcısı yüzeyine & sürükleyin. Etkinlik özellikleri penceresinin **Genel** sekmesinde, **miyir başlatmak**için etkinlik adını değiştirin. **Ayarlar** sekmesine geçin ve aşağıdaki işlemleri yapın.

    1. **URL**için, Azure-SSIS IR'yi başlatan REST API `{subscriptionId}`için `{resourceGroupName}` `{factoryName}`aşağıdaki `{integrationRuntimeName}` URL'yi girin, IR'nizin gerçek değerlerini değiştirerek, ve IR'nizin gerçek değerleriyle birlikte: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}/start?api-version=2018-06-01` Alternatif olarak, yukarıdaki URL'nin aşağıdaki bölümünü değiştirmek için Ir'nizin kaynak kimliğini ADF Kullanıcı Arabirimi/uygulamasındaki izleme sayfasından kopyalayabilir & yapıştırabilirsiniz:`/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}`
    
       ![ADF SSIS IR Kaynak Kimliği](./media/how-to-schedule-azure-ssis-integration-runtime/adf-ssis-ir-resource-id.png)
  
    2. **Yöntem**için **POST'u**seçin. 
    3. **Vücut**için `{"message":"Start my IR"}`, girin . 
    4. **Kimlik Doğrulama**için, ADF'nizin yönetilen kimliğini kullanmak için **MSI'ı** seçin, daha fazla bilgi [için Veri Fabrikası için Yönetilen kimlik](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) makalesine bakın.
    5. **Kaynak**için `https://management.azure.com/`, girin .
    
       ![ADF Web Etkinlik Programı SSIS IR](./media/how-to-schedule-azure-ssis-integration-runtime/adf-web-activity-schedule-ssis-ir.png)
  
3. İkinci bir tane oluşturmak için ilk ardışık lığı klonlayarak, etkinlik adını **durdurup MyIR'ı durdurun** ve aşağıdaki özellikleri değiştirin.

    1. **URL**için, Azure-SSIS IR'yi durduran REST API `{subscriptionId}` `{resourceGroupName}`için `{factoryName}`aşağıdaki `{integrationRuntimeName}` URL'yi girin, IR'nizin gerçek değerlerini değiştirin ve değiştirin:`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}/stop?api-version=2018-06-01`
    
    2. **Vücut**için `{"message":"Stop my IR"}`, girin . 

4. Üçüncü bir ardışık kaynak oluştur, Bir **Execute SSIS Paketi** etkinliğini **Etkinlikler** araç kutusundan boru hattı tasarımcısı yüzeyine & sürükleyin ve [ADF makalesinde SSIS Paketini Yürüt'ünü kullanarak Bir SSIS paketini çağır'daki](how-to-invoke-ssis-package-ssis-activity.md) yönergeleri izleyerek yapılandırın.  Alternatif olarak, bunun yerine Bir **Saklı** Yordam etkinliği kullanabilir ve [ADF makalesinde Depolanan Yordam etkinliğini kullanarak bir SSIS paketindeki](how-to-invoke-ssis-package-stored-procedure-activity.md) yönergeleri izleyerek yapılandırabilirsiniz.  Ardından, SSIS Paketini/Depolanmış Yordametkinliğini, birinci/ikinci ardışık işlemlerdeki Web etkinliklerine benzer şekilde IR'nizi başlatan/durduran iki Web etkinliği arasında zincirleyin.

   ![ADF Web Etkinliği İsteğe Bağlı SSIS IR](./media/how-to-schedule-azure-ssis-integration-runtime/adf-web-activity-on-demand-ssis-ir.png)

5. ADF'niz için yönetilen kimliği kendisine **bir Katılımcı** rolü atayın, böylece boru hatlarındaki Web etkinlikleri, içinde bulunan Azure-SSIS IR'lerini başlatmak/durdurmak için REST API'sini arayabilir.  Azure portalındaki ADF sayfanızda **Access denetimi (IAM) ,+** **Rol ataması ekle'yi**tıklatın ve ardından rol atama bıçağı ekle'yi tıklatın ve ardından rol atama bıçağı **ekle'de** aşağıdaki işlemleri yapın.

    1. **Rol** **için, Katkıda Bulunan'ı**seçin. 
    2. **Erişim atamak için** **Azure AD kullanıcısı, grubu veya hizmet sorumlusunu**seçin. 
    3. **Select**için, ADF adınızı arayın ve seçin. 
    4. **Kaydet**'e tıklayın.
    
   ![ADF Yönetilen Kimlik Rol Atama](./media/how-to-schedule-azure-ssis-integration-runtime/adf-managed-identity-role-assignment.png)

6. Fabrika/boru hattı araç çubuğundaki **Tümlerini Doğrula/Doğrula'yı** tıklatarak ADF'nizi ve tüm boru hattı ayarlarınızı doğrulayın. Düğmeyi tıklatarak **Fabrika/Boru Hattı Doğrulama Çıktısını** **>>** kapatın.  

   ![İşlem hattını doğrulama](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-your-pipelines"></a>Boru hatlarınızı test edin

1. Her ardışık alan için araç çubuğunda **Test Çalıştır'ı** seçin ve alt bölmedeki **Çıktı** penceresine bakın. 

   ![Test Çalıştır](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
    
2. Üçüncü ardışık hattı test etmek için SQL Server Management Studio'yu (SSMS) başlatın. **Sunucuya Bağlan** penceresinde aşağıdaki işlemleri yapın. 

    1. **Sunucu adı için**Azure SQL Veritabanı sunucu adınızı ** &lt;&gt;.database.windows.net**girin.
    2. **Seçenekler >>'yi **seçin.
    3. **Veritabanına Bağlan**için **SSISDB'yi**seçin.
    4. **Bağlan**’ı seçin. 
    5. **Entegrasyon Hizmetleri Kataloglarını** -> Genişlet**SSISDB** -> Klasörünüz -> **Projeleri** -> SSIS projeniz -> **Paketleri.** 
    6. Çalıştırmak için belirtilen SSIS paketini sağ tıklatın ve**Tüm Yürütmeleri** **Raporlar** -> Standart**Raporlar'ı** -> seçin. 
    7. Koştukuzun doğrula. 

   ![SSIS paket çalışmasını doğrulayın](./media/how-to-schedule-azure-ssis-integration-runtime/verify-ssis-package-run.png)

### <a name="schedule-your-pipelines"></a>Boru hatlarınızı zamanlama

Artık ardışık ardışık hatlarınız beklediğiniz gibi çalıştığına göre, bunları belirtilen cadences'de çalıştırmak için tetikleyiciler oluşturabilirsiniz. Tetikleyicileri ardışık hatlarla ilişkilendirme hakkında ayrıntılar için, [bir zamanlama makalesinde ardışık programı tetikle'ye](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule) bakın.

1. Ardışık takım çubuğunda **Tetikle'yi** seçin ve **Yeni/Edit'i**seçin. 

   ![Tetikleyici -> Yeni/Edit](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)

2. **Tetikleyiciekle** bölmesinde + **Yeni'** yi seçin.

   ![Tetikleyici Ekle - Yeni](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)

3. **Yeni Tetikleyici** bölmesinde aşağıdaki eylemleri yapın: 

    1. **Ad**için, tetikleyici için bir ad girin. Aşağıdaki örnekte, **Run daily** tetikleyici adıdır. 
    2. **Türü**için , **Zamanlama'yı**seçin. 
    3. **Başlangıç Tarihi (UTC)** için UTC'ye bir başlangıç tarihi ve saati girin. 
    4. **Yineleme**için, tetikleyici için bir cadence girin. Aşağıdaki örnekte, bir kez **Günlük.** 
    5. **Bitiş için,** **Bitiş Yok'u** seçin veya **Tarih'i**seçtikten sonra bitiş tarihi ve saati girin. 
    6. Tüm ADF ayarlarını yayımladıktan hemen sonra tetikleyiciyi etkinleştirmek için **Etkin'i** seçin. 
    7. **Sonraki'ni**seçin.

   ![Tetikleyici -> Yeni/Edit](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
    
4. **Tetikleyici Çalıştır Parametreleri** sayfasında, herhangi bir uyarıyı gözden geçirin ve **Finish'i**seçin. 
5. Tümünü fabrika araç çubuğunda **Yayımla'yı** seçerek tüm ADF ayarlarını yayımlayın. 

   ![Tümlerini Yayınla](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)

### <a name="monitor-your-pipelines-and-triggers-in-azure-portal"></a>Azure portalında boru hatlarınızı ve tetikleyicilerinizi izleyin

1. Tetikleyici çalıştırmaları ve ardışık hatlar çalıştırmalarını izlemek için ADF Kullanıcı Birası/uygulamasının solundaki **Monitör** sekmesini kullanın. Ayrıntılı adımlar için [bkz.](quickstart-create-data-factory-portal.md#monitor-the-pipeline)

   ![İşlem hattı çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)

2. Bir dizi adlı diziyle ilişkin etkinlik çalışanını günü günü günü günü bulunmak için, **Eylemler** sütunundaki ilk**View Activity Runs** Üçüncü ardışık işlem için, ardışık işlem deki her zincirleme etkinlik için bir tane olmak üzere üç etkinlik çalıştırır (IR'nizi başlatmak için Web etkinliği, paketinizi çalıştırmak için Depolanmış Yordam etkinliği ve IR'nizi durdurmak için Web etkinliği). Ardışık listenin yeniden çalıştığını görmek için, en üstteki **Boru Hatları** bağlantısını seçin.

   ![Etkinlik çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)

3. Tetikleyici çalışır görüntülemek için, üstteki **Pipeline Runs** altında açılan listeden **Tetik Çalışır'ı** seçin. 

   ![Tetikleyici çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

### <a name="monitor-your-pipelines-and-triggers-with-powershell"></a>PowerShell ile boru hatlarınızı ve tetikleyicilerinizi izleyin

Ardışık hatlarınızı ve tetikleyicilerinizi izlemek için aşağıdaki örneklerdeki gibi komut dosyalarını kullanın.

1. Bir boru hattı çalıştırının durumunu alın.

   ```powershell
   Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $myPipelineRun
   ```

2. Tetikleyici hakkında bilgi alın.

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name  "myTrigger"
   ```

3. Tetikleyici çalıştırma durumunu alın.

   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "myTrigger" -TriggerRunStartedAfter "2018-07-15" -TriggerRunStartedBefore "2018-07-16"
   ```

## <a name="create-and-schedule-azure-automation-runbook-that-startsstops-azure-ssis-ir"></a>Azure-SSIS IR'yi başlatan/durduran Azure Otomasyonu runbook'unu oluşturma ve zamanlama

Bu bölümde, PowerShell komut dosyasını çalıştıran ve Azure-SSIS IR'nizi bir zamanlamada başlatma/durdurma yı öğrenen Azure Otomasyon runbook'u oluşturmayı öğreneceksiniz.  Bu, ön/post-processing için IR'nizi başlatmadan/durdurduktan sonra/başlatmadan/durdurduktan sonra ek komut dosyaları yürütmek istediğinizde kullanışlıdır.

### <a name="create-your-azure-automation-account"></a>Azure Otomasyon hesabınızı oluşturun

Zaten bir Azure Otomasyon hesabınız yoksa, bu adımdaki yönergeleri izleyerek bir hesap oluşturun. Ayrıntılı adımlar için [bkz.](../automation/automation-quickstart-create-account.md) Bu adımın bir parçası olarak, bir **Azure Run As** hesabı (Azure Etkin Dizininizde bir hizmet yöneticisi) oluşturur ve Azure aboneliğinizde bir **Katılımcı** rolü atarsınız. Azure SSIS IR ile ADF'nizi içeren abonelikten emin olun. Azure Otomasyonu bu hesabı Azure Kaynak Yöneticisi'ne kimlik doğrulamak ve kaynaklarınızı çalıştırmak için kullanır. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda, ADF Kullanıcı Arabirimi/uygulaması yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenir.
2. [Azure portalında](https://portal.azure.com/)oturum açın.    
3. Sol menüde **Yeni'yi** seçin, **İzleme + Yönetim'i**seçin ve **Otomasyon'u**seçin. 

   ![Yeni -> İzleme + Yönetim -> Otomasyonu](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
    
2. **Otomasyon Hesabı Ekle** bölmesinde aşağıdaki işlemleri yapın.

    1. **Ad**için Azure Otomasyon hesabınıza bir ad girin. 
    2. **Abonelik**için, Azure-SSIS IR ile ADF'niz olan aboneliği seçin. 
    3. **Kaynak grubu**için, yeni bir kaynak grubu oluşturmak için **yeni oluştur'u** veya varolan bir taneseçmek için varolan ı **kullan'ı** seçin. 
    4. **Konum**için Azure Otomasyon hesabınız için bir konum seçin. 
    5. **Azure Çalıştır'ı** **Evet**olarak hesap olarak oluştur'u onaylayın. Azure Etkin Dizininizde bir hizmet ilkesi oluşturulur ve Azure aboneliğinizde bir **Katılımcı** rolü atanır.
    6. Azure panosunda kalıcı olarak görüntülemek için **panoya** Sabitle'yi seçin. 
    7. **Oluştur'u**seçin. 

   ![Yeni -> İzleme + Yönetim -> Otomasyonu](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
   
3. Azure Otomasyon hesabınızın dağıtım durumunu Azure panosunda ve bildirimlerinde görürsünüz. 
    
   ![Otomasyondağıtma](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
    
4. Azure Otomasyon hesabınızın ana sayfasını başarıyla oluşturulduktan sonra görürsünüz. 

   ![Otomasyon ana sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-adf-modules"></a>ADF modüllerini içe aktarma

1. Sol menüdeki **PAYLAŞıLAN KAYNAKLAR** **bölümündeki Modülleri** seçin ve modüller listesinde **Az.DataFactory** + **Az.Profile** olup olmadığını doğrulayın.

   ![Gerekli modülleri doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image1.png)

2.  **Az.DataFactory'niz**yoksa, [Az.DataFactory modülü](https://www.powershellgallery.com/packages/Az.DataFactory/)için PowerShell Galerisi'ne gidin , **Azure Otomasyonuna Dağıt'ı**seçin, Azure Otomasyon uğrama hesabınızı seçin ve ardından **Tamam'ı**seçin. Sol menüdeki PAYLAŞıLAN **KAYNAKLAR** bölümündeki **Modülleri** görüntülemek **için**geri gidin ve **Az.DataFactory** modülünün Kullanılabilir olarak değiştirilme **durumunu** görene kadar bekleyin.

    ![Veri Fabrikası modüllerini doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image2.png)

3.  **Az.Profile'ınız**yoksa, [Az.Profile modülü](https://www.powershellgallery.com/packages/Az.profile/)için PowerShell Galerisi'ne gidin , **Azure Otomasyonuna Dağıt'ı**seçin, Azure Otomasyon uğrama hesabınızı seçin ve ardından **Tamam'ı**seçin. Sol menüdeki PAYLAŞıLAN **KAYNAKLAR** bölümündeki **Modülleri** görüntülemek için geri gidin ve **Az.Profile** modülünün **Durumu'nun Kullanılabilir**olarak değiştirilmesini görene kadar bekleyin. **STATUS**

    ![Profil modüllerini doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image3.png)

### <a name="create-your-powershell-runbook"></a>PowerShell runbook'unuzu oluşturun

Aşağıdaki bölümde PowerShell runbook oluşturmak için adımlar sağlar. Çalışma kitabınızla ilişkili komut dosyası, **IŞLEM** parametresi için belirttiğiniz komuta göre Azure-SSIS IR'yi başlatır/durdurur. Bu bölümde runbook oluşturmak için tüm ayrıntıları sağlamaz. Daha fazla bilgi için [bkz.](../automation/automation-quickstart-create-runbook.md)

1. **Runbook** sekmesine geçin ve araç çubuğundan **runbook ekle seçeneğini** belirleyin. 

   ![Runbook düğmesi ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
   
2. **Yeni bir runbook oluştur'u** seçin ve aşağıdaki işlemleri yapın: 

    1. **Ad**için, **StartStopAzureSsisRuntime**girin.
    2. **Runbook türü için** **PowerShell'i**seçin.
    3. **Oluştur'u**seçin.
    
   ![Runbook düğmesi ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
   
3. Aşağıdaki PowerShell komut dosyasını runbook komut dosyası pencerenize & yapıştırın. Araç çubuğundaki **Kaydet** ve **Yayımla** düğmelerini kullanarak runbook'unuzu kaydedin ve yayımlayın. 

    ```powershell
    Param
    (
          [Parameter (Mandatory= $true)]
          [String] $ResourceGroupName,
    
          [Parameter (Mandatory= $true)]
          [String] $DataFactoryName,
    
          [Parameter (Mandatory= $true)]
          [String] $AzureSSISName,
    
          [Parameter (Mandatory= $true)]
          [String] $Operation
    )
    
    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
    
        "Logging in to Azure..."
        Connect-AzAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
        if (!$servicePrincipalConnection)
        {
            $ErrorMessage = "Connection $connectionName not found."
            throw $ErrorMessage
        } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
        }
    }
    
    if($Operation -eq "START" -or $operation -eq "start")
    {
        "##### Starting #####"
        Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $AzureSSISName -Force
    }
    elseif($Operation -eq "STOP" -or $operation -eq "stop")
    {
        "##### Stopping #####"
        Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    }  
    "##### Completed #####"    
    ```

   ![PowerShell runbook'u edin](./media/how-to-schedule-azure-ssis-integration-runtime/edit-powershell-runbook.png)
    
4. Araç çubuğundaki **Başlat** düğmesini seçerek runbook'unuzu test edin. 

   ![Runbook düğmesini başlat](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
    
5. **RunBook'u Başlat** bölmesinde aşağıdaki eylemleri yapın: 

    1. **KAYNAK GRUBU ADInA,** Azure-SSIS IR ile ADF'niz olan kaynak grubunun adını girin. 
    2. **DATA FACTORY NAME**için Azure-SSIS IR ile ADF'nizin adını girin. 
    3. **AZURESSISNAME**için Azure-SSIS IR adını girin. 
    4. **OPERASYON**için **START'ı**girin. 
    5. **Tamam'ı**seçin.  

   ![Çalışma defteri pencereni başlat](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
   
6. İş penceresinde Çıktı **döşemesini** seçin. Çıkış penceresinde, ##### İletiyi bekleyin **##### Tamamlandı #####** **##### Başlangıç ####** iletisini gördükten sonra. Azure-SSIS IR'yi başlatmak yaklaşık 20 dakika sürer. **İş** penceresini kapatın ve **Runbook** penceresine geri dön.

   ![Azure SSIS IR - başladı](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
    
7. **İşlem**değeri olarak **STOP'u** kullanarak önceki iki adımı yineleyin. Araç çubuğundaki **Başlat** düğmesini seçerek runbook'unuzu yeniden başlatın. Kaynak grubunuzu, ADF'yi ve Azure-SSIS IR adlarınızı girin. **OPERASYON**için **STOP**girin. Çıkış penceresinde, **##### Tamamlandı #####** iletisini bekleyin **##### ##### Stop #####** mesajını gördün. Azure-SSIS IR'yi durdurmak, başlatmak kadar uzun sürmez. **İş** penceresini kapatın ve **Runbook** penceresine geri dön.

8. Ayrıca, **Webhooks** menü öğesini seçerek veya aşağıda belirtildiği gibi **Zamanlamalar** menü öğesini seçerek oluşturulabilecek bir zamanlamada oluşturulabilecek bir webhook aracılığıyla runbook'unuzu tetikleyebilirsiniz.  

## <a name="create-schedules-for-your-runbook-to-startstop-azure-ssis-ir"></a>Azure-SSIS IR'yi başlatmak/durdurmak için runbook'unuzun zamanlamaları oluşturma

Önceki bölümde, Azure-SSIS IR'yi başlatabilecek veya durdurabilen Azure Otomasyon runbook'unuzu oluşturdunuz. Bu bölümde, runbook için iki zamanlama oluşturur. İlk zamanlamayı yapılandırırken, **OPERASYON**için **START'ı** belirtirsiniz. Benzer şekilde, ikincisini yapılandırırken, **OPERASYON**İçİn **STOP'u** belirtirsiniz. Zamanlamaoluşturmak için ayrıntılı adımlar için zamanlama makalesi [oluştur'a](../automation/shared-resources/schedules.md#creating-a-schedule) bakın.

1. **Runbook** **penceresinde, Zamanlamalar'ı**seçin ve araç çubuğuna **zamanlama + ekle'yi** seçin. 

   ![Azure SSIS IR - başladı](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
   
2. **Zamanlama Runbook** bölmesinde aşağıdaki eylemleri yapın: 

    1. **Runbook'unuza bir zamanlama**bağla'yı seçin. 
    2. **Yeni bir zamanlama oluştur'u**seçin.
    3. **Yeni Zamanlama** bölmesinde, **Ad**için günlük **Başlat IR'yi** girin. 
    4. **Başlangıçlar**için, geçerli saati birkaç dakika geçmiş bir zaman girin. 
    5. **Yineleme**için **Yinelenen'i**seçin. 
    6. **Her Yineleme**için, **1** girin ve **Gün**seçin. 
    7. **Oluştur'u**seçin. 

   ![Azure SSIS IR başlangıcı için zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
    
3. Parametrelere geç **ve ayarlar sekmesini çalıştır.** Kaynak grubunuzu, ADF'nizi ve Azure-SSIS IR adlarınızı belirtin. **OPERASYON**için **START'ı** girin ve **Tamam'ı**seçin. Runbook'unuzun **Zamanlamalar** sayfasındaki zamanlamayı görmek için yeniden **Tamam'ı** seçin. 

   ![Azure SSIS IR'ye bakma programı](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
    
4. Günlük **Stop IR**adlı bir zamanlama oluşturmak için önceki iki adımı yineleyin. **Başlangıç IR günlük** zamanlaması için belirttiğiniz süreden en az 30 dakika sonra bir süre girin. **OPERASYON**için **STOP'a** girin ve **Tamam'ı**seçin. Runbook'unuzun **Zamanlamalar** sayfasındaki zamanlamayı görmek için yeniden **Tamam'ı** seçin. 

5. **Runbook** penceresinde, sol menüde **İşler'i** seçin. Zamanlamalarınızın belirlediği saatlerde ve durumları tarafından oluşturulan işleri görmeniz gerekir. Çalışma kitabınızı test ettikten sonra gördüklerinize benzer çıktısı gibi iş ayrıntılarını görebilirsiniz. 

   ![Azure SSIS IR'ye bakma programı](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
    
6. Sınama bittikten sonra, bunları düzenleyerek zamanlamalarınızı devre dışı bırakın. Sol menüde **Zamanlamaları** seçin, **günlük Başlat IR/Stop IR'yi**seçin ve **Etkin emene** **Hayır'ı** seçin. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki blog gönderisini görün:
-   [ADF boru hatlarındaki SSIS faaliyetleri ile ETL/ELT iş akışlarınızı modernize edin ve genişletin](https://techcommunity.microsoft.com/t5/SQL-Server-Integration-Services/Modernize-and-Extend-Your-ETL-ELT-Workflows-with-SSIS-Activities/ba-p/388370)

SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Azure’da bir SSIS paketi dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure’da SSIS kataloğuna bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure’da paket yürütmeyi zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth)
