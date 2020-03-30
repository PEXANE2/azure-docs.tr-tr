---
title: Azure portalını ve PowerShell'i kullanarak boru hatlarını izleme ve yönetme
description: Oluşturduğunuz Azure veri fabrikalarını ve ardışık hatlarını izlemek ve yönetmek için Azure portalını ve Azure PowerShell'i nasıl kullanacağınızı öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/30/2018
ms.openlocfilehash: 44aadecfa80524345932c03abb51e8ebd040a902
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73666975"
---
# <a name="monitor-and-manage-azure-data-factory-pipelines-by-using-the-azure-portal-and-powershell"></a>Azure portalını ve PowerShell'i kullanarak Azure Veri Fabrikası boru hatlarını izleyin ve yönetin
> [!div class="op_single_selector"]
> * [Azure portalı/Azure PowerShell'i kullanma](data-factory-monitor-manage-pipelines.md)
> * [İzleme ve Yönetim uygulamasını kullanma](data-factory-monitor-manage-app.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası ardışık hatlarını şu anda izleyin ve yönetin.](../monitor-visually.md)

Bu makalede, Azure portalı ve PowerShell kullanarak ardışık hatlarınızı nasıl izleyip, yönetip hata ayıklanınca açıklanmaktadır.

> [!IMPORTANT]
> İzleme & yönetimi uygulaması, veri ardışık alanlarınızın izlenmesi ve yönetilmesi ve sorunları gidermek için daha iyi bir destek sağlar. Uygulamayı kullanma hakkında ayrıntılı bilgi için, [İzleme ve Yönetim uygulamasını kullanarak Veri Fabrikası boru hatlarını izleyin ve yönetin.](data-factory-monitor-manage-app.md) 

> [!IMPORTANT]
> Azure Veri Fabrikası sürüm 1 artık yeni [Azure Monitor uyarı altyapısını](../../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)kullanıyor. Eski uyarı altyapısı yıkıntılı. Sonuç olarak, sürüm 1 veri fabrikaları için yapılandırılan varolan uyarılarınız artık çalışmıyor. v1 veri fabrikaları için varolan uyarılarınız otomatik olarak geçirilemez. Bu uyarıları yeni uyarı altyapısında yeniden oluşturmanız gerekir. Azure portalında oturum açın ve sürüm 1 veri fabrikalarınız için ölçümler (başarısız çalıştırmalar veya başarılı çalıştırmalar gibi) hakkında yeni uyarılar oluşturmak için **Monitör'ü** seçin.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="understand-pipelines-and-activity-states"></a>Boru hatlarını ve etkinlik durumlarını anlama
Azure portalını kullanarak şunları yapabilirsiniz:

* Veri fabrikanızı bir diyagram olarak görüntüleyin.
* Etkinlikleri bir ardışık ardışık yönden görüntüleyin.
* Giriş ve çıktı veri kümelerini görüntüleyin.

Bu bölümde ayrıca, bir veri kümesi diliminin bir durumdan diğerine nasıl geçiş ler olduğunu da açıklar.   

### <a name="navigate-to-your-data-factory"></a>Veri fabrikanıza gidin
1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Soldaki menüdeki **Veri fabrikaları'nı** tıklatın. Görmüyorsanız, **daha fazla hizmet >'nı **tıklatın ve ardından INTELLIGENCE + **ANALYTICS** kategorisi altındaki **Veri fabrikalarını** tıklatın.

   ![Tüm > Veri fabrikalarına göz atın](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)
3. Veri **fabrikaları** bıçak, ilgilendiğiniz veri fabrikası seçin.

    ![Veri fabrikası seçme](./media/data-factory-monitor-manage-pipelines/select-data-factory.png)

   Veri fabrikasının ana sayfasını görmelisiniz.

   ![Veri fabrikası dikey penceresi](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### <a name="diagram-view-of-your-data-factory"></a>Veri fabrikanızın diyagram görünümü
Bir veri fabrikasının **Diyagram** görünümü, veri fabrikasını ve varlıklarını izlemek ve yönetmek için tek bir cam bölmesi sağlar. Veri fabrikanızın **Diyagram** görünümünü görmek için, veri fabrikasının ana **sayfasındaKi Diyagram'ı** tıklatın.

![Diyagram görünümü](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

Yakınlaştırabilir, yakınlaştırabilir, yakınlaştırabilir, sığacak şekilde yakınlaştırabilir, %100 yakınlaştırabilir, diyagramın düzenini kilitleyebilir ve ardışık hatları ve veri kümelerini otomatik olarak konumlandırabilirsiniz. Ayrıca veri soy bilgileri (diğer bir şekilde, seçili öğelerin yukarı ve aşağı akış öğelerini göster) de görebilirsiniz.

### <a name="activities-inside-a-pipeline"></a>Boru hattı içindeki etkinlikler
1. Ardışık hatlar hattını sağ tıklatın ve ardından etkinlikler için giriş ve çıktı veri kümeleriyle birlikte ardışık ardışık alandaki tüm etkinlikleri görmek için **açık ardışık hattını** tıklatın. Bu özellik, ardışık ardınız birden fazla etkinlik içeriyorsa ve tek bir ardışık boru hattının işletim soyundan anlamak istediğinizde yararlıdır.

    ![İşlem hattı menüsünü açma](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)     
2. Aşağıdaki örnekte, bir giriş ve çıktı ile boru hattında bir kopyalama etkinliği görürsünüz. 

    ![Boru hattı içindeki etkinlikler](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png)
3. Sol üst köşedeki kırıntıdaki Veri fabrikası bağlantısını tıklatarak veri **fabrikasının** ana sayfasına geri dönebilirsiniz.

    ![Veri fabrikasına geri dön](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### <a name="view-the-state-of-each-activity-inside-a-pipeline"></a>Bir ardışık hat içindeki her etkinliğin durumunu görüntüleme
Etkinlik tarafından üretilen veri kümelerinden herhangi birinin durumunu görüntüleyerek bir etkinliğin geçerli durumunu görüntüleyebilirsiniz.

**Diyagramdaki** **OutputBlobTable'ı** çift tıklatarak, farklı etkinlik tarafından üretilen tüm dilimleri bir boru hattı içinde görebilirsiniz. Kopyalama etkinliğinin son sekiz saat boyunca başarıyla çalıştırdığını ve dilimleri **Hazır** durumunda ürettiğini görebilirsiniz.  

![Boru hattının durumu](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

Veri fabrikasındaki veri kümesi dilimleri aşağıdaki durumlardan birine sahip olabilir:

<table>
<tr>
    <th align="left">Durum</th><th align="left">Alt eyalet</th><th align="left">Açıklama</th>
</tr>
<tr>
    <td rowspan="8">Bekleniyor</td><td>Zaman Çizelgesi</td><td>Dilimin çalışma zamanı gelmedi.</td>
</tr>
<tr>
<td>DatasetBağımlılıklar</td><td>Yukarı bağımlılıklar hazır değil.</td>
</tr>
<tr>
<td>HesaplamaKaynakları</td><td>İşlem kaynakları kullanılamıyor.</td>
</tr>
<tr>
<td>EşzamanlılıkLimit</td> <td>Tüm etkinlik örnekleri diğer dilimleri çalıştırmakla meşgul.</td>
</tr>
<tr>
<td>EtkinlikDevamı</td><td>Etkinlik duraklatıldı ve etkinlik sürdürülene kadar dilimleri çalıştıramıyor.</td>
</tr>
<tr>
<td>Yeniden Dene</td><td>Etkinlik yürütme yeniden denenmektedir.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama henüz başlamadı.</td>
</tr>
<tr>
<td>DoğrulamaRetry</td><td>Doğrulama yeniden denenmeyi bekliyor.</td>
</tr>
<tr>
<tr>
<td rowspan="2">Devam Ediyor</td><td>Doğrulama</td><td>Doğrulama devam ediyor.</td>
</tr>
<td>-</td>
<td>Dilim işleniyor.</td>
</tr>
<tr>
<td rowspan="4">Başarısız</td><td>TimedOut</td><td>Etkinlik yürütme, etkinlik tarafından izin verilenden daha uzun sürdü.</td>
</tr>
<tr>
<td>İptal edildi</td><td>Dilim kullanıcı eylemi tarafından iptal edildi.</td>
</tr>
<tr>
<td>Doğrulama</td><td>Doğrulama başarısız oldu.</td>
</tr>
<tr>
<td>-</td><td>Dilim oluşturulamadı ve/veya doğrulanmadı.</td>
</tr>
<td>Hazır</td><td>-</td><td>Dilim tüketime hazırdır.</td>
</tr>
<tr>
<td>Atlandı</td><td>None</td><td>Dilim işlenmiyor.</td>
</tr>
<tr>
<td>None</td><td>-</td><td>Farklı bir durumla birlikte var olan bir dilim, ancak sıfırlandı.</td>
</tr>
</table>



**Son Güncellenen Dilimler** bıçağındaki bir dilim girişini tıklatarak dilimle ilgili ayrıntıları görüntüleyebilirsiniz.

![Dilim ayrıntıları](./media/data-factory-monitor-manage-pipelines/slice-details.png)

Dilim birden çok kez yürütülmüşse, Etkinlik **çalışır** listesinde birden çok satır görürsünüz. **Etkinlik çalışır** listesindeki çalıştır girişini tıklatarak çalıştırılabilen bir etkinlikle ilgili ayrıntıları görüntüleyebilirsiniz. Liste, varsa bir hata iletisi ile birlikte tüm günlük dosyalarını gösterir. Bu özellik, veri fabrikanızdan ayrılmak zorunda kalmadan günlükleri görüntülemek ve hata ayıklamak için yararlıdır.

![Etkinlik çalışma ayrıntıları](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

Dilim **Hazır** durumunda değilse, hazır olmayan ve geçerli dilimin hazır olmayan **Upstream dilimlerinde** yürütülmesini engelleyen yukarı akış dilimlerini görebilirsiniz. Bu özellik, diliminiz **Bekleme** durumundayken ve dilimin beklediği yukarı akış bağımlılıklarını anlamak istediğinizde yararlıdır.

![Hazır olmayan yukarı akış dilimleri](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### <a name="dataset-state-diagram"></a>Dataset durum diyagramı
Bir veri fabrikası dağıttıktan ve ardışık hatlar geçerli bir etkin döneme sahip olduktan sonra, veri kümesi bir durumdan diğerine geçiş dilimler. Şu anda, dilim durumu aşağıdaki durum diyagramını izler:

![Durum diyagramı](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Veri fabrikasındaki veri kümesi durumu geçiş akışı aşağıdaki gibidir: Bekleme -> Devam Eden/Devam Eden (Doğrulama) -> Hazır/Başarısız.

Dilim, yürütmeden önce ön koşulların karşılanmasını bekleyen bir **Bekleme** durumunda başlar. Daha sonra, etkinlik yürütmeye başlar ve dilim **Devam Eden** duruma geçer. Etkinlik yürütme başarılı veya başarısız olabilir. Yürütme sonucuna bağlı olarak dilim **Hazır** veya **Başarısız**olarak işaretlenir.

**Hazır** veya **Başarısız** durumundan **Bekleme** durumuna geri dönmek için dilimi sıfırlayabilirsiniz. Ayrıca, etkinliğin yürütmesini ve dilimin işlememesini engelleyen Dilim durumunu **Atla'ya**da işaretleyebilirsiniz.

## <a name="pause-and-resume-pipelines"></a>Ardışık hatları duraklatma ve devam ettirme
Azure PowerShell'i kullanarak boru hatlarınızı yönetebilirsiniz. Örneğin, Azure PowerShell cmdlets çalıştırarak ardışık hatları duraklatabilir ve devam ettirebilirsiniz. 

> [!NOTE] 
> Diyagram görünümü, ardışık hatları duraklatma ve devam desteklemez. Bir kullanıcı arabirimi kullanmak istiyorsanız, uygulamayı izleme ve yönetme yi kullanın. Uygulamayı kullanma hakkında ayrıntılı bilgi için, [İzleme ve Yönetim uygulaması makalesini kullanarak Veri Fabrikası ardışık hatlarını izleyin ve yönetin.](data-factory-monitor-manage-app.md) 

**Askıya Alma-AzDataFactoryPipeline** PowerShell cmdlet'i kullanarak boru hatlarını duraklatabilir/askıya alabilirsiniz. Bu cmdlet, bir sorun giderilene kadar ardışık hatlarınızı çalıştırmak istemiyorsanız kullanışlıdır. 

```powershell
Suspend-AzDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Örnek:

```powershell
Suspend-AzDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

Sorun boru hattıyla giderildikten sonra, aşağıdaki PowerShell komutunu çalıştırarak askıya alınan ardışık hattı devam ettirebilirsiniz:

```powershell
Resume-AzDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>
```
Örnek:

```powershell
Resume-AzDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline
```

## <a name="debug-pipelines"></a>Hata ayıklama boru hatları
Azure Veri Fabrikası, Azure portalını ve Azure PowerShell'i kullanarak veri ayıklama ve sorun giderme için zengin özellikler sağlar.

> [!NOTE] 
> İzleme & Yönetimi Uygulamasını kullanarak hataları gidermek çok daha kolaydır. Uygulamayı kullanma hakkında ayrıntılı bilgi için, [İzleme ve Yönetim uygulaması makalesini kullanarak Veri Fabrikası ardışık hatlarını izleyin ve yönetin.](data-factory-monitor-manage-app.md) 

### <a name="find-errors-in-a-pipeline"></a>Bir ardışık ardışık bilgi ndeki hataları bulma
Etkinlik çalışması bir ardışık ardışık yapıda başarısız olursa, ardışık hatlar tarafından üretilen veri kümesi hata nedeniyle bir hata durumundadır. Aşağıdaki yöntemleri kullanarak Azure Veri Fabrikası'ndaki hataları ayıklayabilir ve sorun giderebilirsiniz.

#### <a name="use-the-azure-portal-to-debug-an-error"></a>Hata ayıklamak için Azure portalını kullanma
1. **Tablo** bıçağında, **Durum'u** **Başarısız**olarak ayarlayan sorun dilimini tıklatın.

   ![Sorun dilimi olan masa bıçağı](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
2. Veri **dilimi** bıçak üzerinde, başarısız etkinlik çalıştır'ı tıklatın.

   ![Hata ile veri dilimi](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
3. Activity **run details** blade'de, HDInsight işlemiyle ilişkili dosyaları indirebilirsiniz. Hata yla ilgili ayrıntıları içeren hata günlüğü dosyasını indirmek için Durum/stderr için **İndir'i** tıklatın.

   ![Etkinlik çalıştırma ayrıntıları hata ile bıçak](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)     

#### <a name="use-powershell-to-debug-an-error"></a>Hata ayıklamak için PowerShell'i kullanma
1. **PowerShell**’i başlatın.
2. Dilimleri ve durumlarını görmek için **Get-AzDataFactorySlice** komutunu çalıştırın. **Başarısız**olan bir dilim görmeniz gerekir.        

    ```powershell   
    Get-AzDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```   
   Örnek:

    ```powershell   
    Get-AzDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00
    ```

   **StartDateTime'ı** ardınızdaki başlangıç saati ile değiştirin. 
3. Şimdi, dilim için etkinlik çalıştırmak hakkında ayrıntılı bilgi almak için **Get-AzDataFactoryRun** cmdlet çalıştırın.

    ```powershell   
    Get-AzDataFactoryRun [-ResourceGroupName] <String> [-DataFactoryName] <String> [-DatasetName] <String> [-StartDateTime]
    <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    ```

    Örnek:

    ```powershell   
    Get-AzDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -DatasetName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"
    ```

    StartDateTime değeri, önceki adımda belirttiğiniz hata/sorun diliminin başlangıç saatidir. Tarih-saat çift tırnak içinde eklenmelidir.
4. Aşağıdakilere benzer hata yla ilgili ayrıntıları içeren çıktıyı görmeniz gerekir:

    ```   
    Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
    ResourceGroupName       : ADF
    DataFactoryName         : LogProcessingFactory3
    DatasetName               : EnrichedGameEventsTable
    ProcessingStartTime     : 10/10/2014 3:04:52 AM
    ProcessingEndTime       : 10/10/2014 3:06:49 AM
    PercentComplete         : 0
    DataSliceStart          : 5/5/2014 12:00:00 AM
    DataSliceEnd            : 5/6/2014 12:00:00 AM
    Status                  : FailedExecution
    Timestamp               : 10/10/2014 3:04:52 AM
    RetryAttempt            : 0
    Properties              : {}
    ErrorMessage            : Pig script failed with exit code '5'. See wasb://        adfjobs@spestore.blob.core.windows.net/PigQuery
                                    Jobs/841b77c9-d56c-48d1-99a3-
                8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                more details.
    ActivityName            : PigEnrichLogs
    PipelineName            : EnrichGameLogsPipeline
    Type                    :
    ```
5. Çıkıştan gördüğünüz Id değeri ile **Save-AzDataFactoryLog** cmdlet çalıştırabilirsiniz ve cmdlet için **-DownloadLogsoption** kullanarak günlük dosyalarını indirebilirsiniz.

    ```powershell
    Save-AzDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"
    ```

## <a name="rerun-failures-in-a-pipeline"></a>Bir ardışık ardışık yapıdaki hataları yeniden çalıştırma

> [!IMPORTANT]
> İzleme & Yönetimi Uygulamasını kullanarak hataları gidermek ve başarısız dilimleri yeniden çalıştırmak daha kolaydır. Uygulamayı kullanma hakkında ayrıntılı bilgi için, [İzleme ve Yönetim uygulamasını kullanarak Veri Fabrikası boru hatlarını izleyin ve yönetin.](data-factory-monitor-manage-app.md) 

### <a name="use-the-azure-portal"></a>Azure portalı kullanma
Bir ardışık ardışık ardışık alanda sorun giderme ve hata ayıklama giderme sonra, hata bölümüne navigasyon ve komut çubuğunda **Çalıştır** düğmesini tıklatarak hataları yeniden yapabilirsiniz.

![Başarısız bir dilimi yeniden çalıştırma](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

Bir ilke hatası nedeniyle dilimin doğrulamada başarısız olması durumunda (örneğin, veri yoksa), komut çubuğundaki **Doğrulama** düğmesini tıklatarak hatayı düzeltebilir ve yeniden doğrulayabilirsiniz.

![Hataları düzeltme ve doğrulama](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### <a name="use-azure-powershell"></a>Azure PowerShell kullanma
**Set-AzDataFactorySliceStatus** cmdlet'i kullanarak hataları yeniden çalıştırabilirsiniz. Sözdizimi ve cmdlet hakkındaki diğer ayrıntılar için [Set-AzDataFactorySliceStatus](https://docs.microsoft.com/powershell/module/az.datafactory/set-azdatafactoryslicestatus) konusuna bakın.

**Örnek:**

Aşağıdaki örnekte, 'DAWikiAggregatedData' tablosunun tüm dilimlerinin durumunu Azure veri fabrikası 'WikiADF'de 'Bekliyor' olarak ayarlar.

'UpdateType' 'UpstreamInPipeline' olarak ayarlanır, bu da tablo için her dilimin durumlarının ve tüm bağımlı (yukarı akış) tabloların 'Bekleme' olarak ayarlandığı anlamına gelir. Bu parametrenin diğer olası değeri 'Individual' dır.

```powershell
Set-AzDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -DatasetName DAWikiAggregatedData -Status Waiting -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00
```
## <a name="create-alerts-in-the-azure-portal"></a>Azure portalında uyarılar oluşturma

1.  Azure portalına giriş yapın ve Uyarılar sayfasını açmak için **> Uyarıları İzle'yi** seçin.

    ![Uyarılar sayfasını açın.](media/data-factory-monitor-manage-pipelines/v1alerts-image1.png)

2.  Yeni bir uyarı oluşturmak için **+ Yeni Uyarı kuralını** seçin.

    ![Yeni bir uyarı oluşturma](media/data-factory-monitor-manage-pipelines/v1alerts-image2.png)

3.  Uyarı **koşulunu**tanımlayın. (Kaynak türü alanına **göre Filtre'deki** **Veri fabrikalarını** seçtiğinizden emin olun.) **Boyutlar**için de değerler belirtebilirsiniz.

    ![Uyarı Koşulunu Tanımla - Hedefi seçin](media/data-factory-monitor-manage-pipelines/v1alerts-image3.png)

    ![Uyarı Koşulunu Tanımla - Uyarı ölçütleri ekleme](media/data-factory-monitor-manage-pipelines/v1alerts-image4.png)

    ![Uyarı Koşulunu Tanımla - Uyarı mantığı ekle](media/data-factory-monitor-manage-pipelines/v1alerts-image5.png)

4.  Uyarı **ayrıntılarını**tanımlayın.

    ![Uyarı Ayrıntılarını Tanımla](media/data-factory-monitor-manage-pipelines/v1alerts-image6.png)

5.  Eylem **grubunu**tanımlayın.

    ![Eylem Grubunu tanımla - yeni bir Eylem grubu oluşturma](media/data-factory-monitor-manage-pipelines/v1alerts-image7.png)

    ![Eylem Grubunu tanımla - özellikleri ayarla](media/data-factory-monitor-manage-pipelines/v1alerts-image8.png)

    ![Eylem Grubunu tanımlayın - yeni eylem grubu oluşturuldu](media/data-factory-monitor-manage-pipelines/v1alerts-image9.png)

## <a name="move-a-data-factory-to-a-different-resource-group-or-subscription"></a>Veri fabrikasını farklı bir kaynak grubuna veya aboneye taşıma
Veri fabrikanızın ana sayfasındaki **Taşı** komut çubuğu düğmesini kullanarak bir veri fabrikasını farklı bir kaynak grubuna veya farklı bir aboneye taşıyabilirsiniz.

![Veri fabrikasını taşıma](./media/data-factory-monitor-manage-pipelines/MoveDataFactory.png)

Ayrıca, veri fabrikasıyla birlikte ilgili kaynakları (veri fabrikasıyla ilişkili uyarılar gibi) da taşıyabilirsiniz.

![Kaynaklar iletişim kutusunu taşıma](./media/data-factory-monitor-manage-pipelines/MoveResources.png)
