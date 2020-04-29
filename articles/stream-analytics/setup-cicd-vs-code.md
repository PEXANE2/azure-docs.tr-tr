---
title: CI/CD NPM paketini kullanarak bir Azure Stream Analytics işi dağıtma
description: Bu makalede bir sürekli tümleştirme ve dağıtım işlemi ayarlamak için Azure Stream Analytics CI/CD NPM paketinin nasıl kullanılacağı açıklanır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: deb6c2439cc84f196b7f42fd9f49d3ebfd057cbb
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76962230"
---
# <a name="deploy-an-azure-stream-analytics-job-using-cicd-npm-package"></a>CI/CD NPM paketini kullanarak bir Azure Stream Analytics işi dağıtma 

Stream Analytics işleriniz için sürekli tümleştirme ve dağıtım işlemi ayarlamak üzere Azure Stream Analytics CI/CD NPM paketini kullanabilirsiniz. Bu makalede, her bir CI/CD sistemiyle NPM paketinin yanı sıra Azure Pipelines dağıtım için özel yönergelerin nasıl kullanılacağı açıklanır.

PowerShell ile dağıtma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi Şablon dosyası ile dağıtma ve Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Ayrıca, bir [nesnenin bir kaynak yöneticisi şablonunda parametre olarak nasıl kullanılacağı](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters)hakkında daha fazla bilgi edinebilirsiniz.

## <a name="build-the-vs-code-project"></a>VS Code projeyi oluşturma

**Asa-streamanalytics-cicd** NPM paketini kullanarak Azure Stream Analytics işleri için sürekli tümleştirme ve dağıtım sağlayabilirsiniz. NPM paketi, [Stream Analytics Visual Studio Code projelerinin](quick-create-vs-code.md)Azure Resource Manager şablonlarını oluşturmak için araçlar sağlar. Visual Studio Code yüklenmeden Windows, macOS ve Linux 'ta kullanılabilir.

Paketi doğrudan [indirebilir](https://www.npmjs.com/package/azure-streamanalytics-cicd) veya `npm install -g azure-streamanalytics-cicd` komutu aracılığıyla [Global olarak](https://docs.npmjs.com/downloading-and-installing-packages-globally) yükleyebilirsiniz. Bu, **Azure Pipelines**bir derleme Işlem hattının PowerShell veya Azure CLI betik görevinde de kullanılabilecek olan önerilen yaklaşımdır.

Paketi yükledikten sonra, Azure Resource Manager şablonlarının çıktısını almak için aşağıdaki komutu kullanın. **ScriptPath** bağımsız değişkeni, projenizdeki **aşama QL** dosyasının mutlak yoludur. Asaproj. JSON ve JobConfig. JSON dosyalarının betik dosyası ile aynı klasörde olduğundan emin olun. **OutputPath** belirtilmemişse, şablonlar projenin **bin** klasörü altındaki **Dağıt** klasörüne yerleştirilir.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Örnek (macOS 'ta)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Bir Stream Analytics Visual Studio Code projesi başarıyla oluşturulduğunda, **bin/[Debug/Retail]/Deploy** klasörü altında aşağıdaki iki Azure Resource Manager şablon dosyasını oluşturur: 

*  Şablon dosyası Kaynak Yöneticisi

       [ProjectName].JobTemplate.json 

*  Kaynak Yöneticisi Parameters dosyası

       [ProjectName].JobTemplate.parameters.json   

Parameters. JSON dosyasındaki varsayılan parametreler Visual Studio Code projenizdeki ayarlardan alınır. Başka bir ortama dağıtmak istiyorsanız, parametreleri uygun şekilde değiştirin.

> [!NOTE]
> Tüm kimlik bilgileri için varsayılan değerler null olarak ayarlanır. Buluta dağıtmadan önce değerleri ayarlamanız **gerekir** .

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```

## <a name="deploy-with-azure-pipelines"></a>Azure Pipelines ile dağıtma

Bu bölümde, NPM kullanarak Azure Pipelines [oluşturma](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav) ve [yayınlama](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) işlem hatları oluşturma ayrıntıları anlatılmaktadır.

Bir Web tarayıcısı açın ve Azure Stream Analytics Visual Studio Code projenize gidin.

1. Sol gezinti menüsünde işlem **hatları** ' nın altında, **yapılar**' ı seçin. Ardından **Yeni işlem hattı** seçin

   ![Yeni Azure işlem hattı oluşturma](./media/setup-cicd-vs-code/new-pipeline.png)

2. YAML olmadan bir işlem hattı oluşturmak için **Klasik düzenleyiciyi kullan** ' ı seçin.

3. Kaynak türü, takım projesi ve deponuzu seçin. Daha sonra **Devam** seçeneğini belirleyin.

   ![Azure Stream Analytics projesi seçin](./media/setup-cicd-vs-code/select-repo.png)

4. **Şablon seçin** sayfasında **boş iş**' ı seçin.

### <a name="add-npm-task"></a>NPM görevi Ekle

1. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. Görev aramasına "NPM" yazın ve **NPM**' yi seçin.

   ![NPM görevi seçin](./media/setup-cicd-vs-code/search-npm.png)

2. Göreve bir **görünen ad**verin. **Komut** seçeneğini *özel* olarak değiştirin ve **komut ve bağımsız değişkenler**' de aşağıdaki komutu girin. Kalan varsayılan seçenekleri bırakın.

   ```cmd
   install -g azure-streamanalytics-cicd
   ```

   ![NPM görevi için yapılandırma girin](./media/setup-cicd-vs-code/npm-config.png)

### <a name="add-command-line-task"></a>Komut satırı görevi Ekle

1. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. **Komut satırı**araması yapın.

2. Göreve bir **görünen ad** verin ve aşağıdaki betiği girin. Betiği depo adınızla ve proje adınızla değiştirin.

   ```cmd
   azure-streamanalytics-cicd build -scriptPath $(Build.SourcesDirectory)/myASAProject/myASAProj.asaql
   ```

   ![Komut satırı görevi için yapılandırma girin](./media/setup-cicd-vs-code/commandline-config.png)

### <a name="add-copy-files-task"></a>Dosya kopyalama görevi Ekle

1. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. **Kopya dosyalarını**arayın. Ardından aşağıdaki konfigürasyonları girin.

   |Parametre|Girdi|
   |-|-|
   |Görünen ad|Dosyaları kopyala: $ (Build. artifactstagingdirectory)|
   |Kaynak klasör|`$(system.defaultworkingdirectory)`| 
   |İçindekiler| `**\Deploy\**` |
   |Hedef klasör| `$(build.artifactstagingdirectory)`|

   ![Kopyalama görevi için yapılandırma girin](./media/setup-cicd-vs-code/copy-config.png)

### <a name="add-publish-build-artifacts-task"></a>Derleme yapılarını Yayımla görevi Ekle

1. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. **Derleme yapıtları Yayımla** ' yı arayın ve siyah ok simgesiyle seçeneği belirleyin. 

2. Varsayılan yapılandırmaların hiçbirini değiştirmeyin.

### <a name="save-and-run"></a>Kaydet ve Çalıştır

NPM, komut satırı, dosyaları kopyala ve yapı yapılarını Yayımla görevlerini tamamladıktan sonra **& kuyruğu kaydet**' i seçin. İstendiğinde, bir kaydet yorumu girin ve **Kaydet ve Çalıştır '** ı seçin.

## <a name="release-with-azure-pipelines"></a>Azure Pipelines yayın

Bir Web tarayıcısı açın ve Azure Stream Analytics Visual Studio Code projenize gidin.

1. Sol gezinti menüsündeki işlem **hatları** altında **yayınlar**' ı seçin. Ardından **Yeni işlem hattı**' nı seçin.

2. **Boş bir iş ile başla**' yı seçin.

3. **Yapıtlar** kutusunda **+ yapıt Ekle**' yi seçin. **Kaynak**altında, az önce oluşturduğunuz derleme işlem hattını seçin ve **Ekle**' yi seçin.

   ![Derleme işlem hattı yapıtı girin](./media/setup-cicd-vs-code/build-artifact.png)

4. **İşi test ortamına dağıtmak**için **Aşama 1** adını değiştirin.

5. Yeni bir aşama ekleyin ve uygulamayı **üretim ortamına dağıtın**.

### <a name="add-tasks"></a>Görev Ekle

1. Görevler açılan menüsünde **işi test et**' i seçin. 

2. **+** **Aracı işi** ' nin yanındaki ' ı seçin ve *Azure Kaynak grubu dağıtımı*' nı arayın. Aşağıdaki parametreleri girin:

   |Ayar|Değer|
   |-|-|
   |Görünen ad| *MyASAJob dağıtma*|
   |Azure aboneliği| Aboneliğinizi seçin.|
   |Eylem| *Kaynak grubu oluşturma veya güncelleştirme*|
   |Kaynak grubu| Stream Analytics işinizi içerecek test kaynak grubu için bir ad seçin.|
   |Konum|Test kaynak grubunuzun konumunu seçin.|
   |Şablon konumu| *Bağlı yapıt*|
   |Şablon| $ (Build. ArtifactStagingDirectory) \drop\myASAJob.JobTemplate.json |
   |Şablon parametreleri|($ (Build. ArtifactStagingDirectory) \drop\myASAJob.JobTemplate.parameters.json|
   |Şablon parametrelerini geçersiz kılma|-Input_IoTHub1_iotHubNamespace $ (test_eventhubname)|
   |Dağıtım modu|Artımlı|

3. Görevler açılan menüsünde **işi üretim ortamına dağıt**' ı seçin.

4. **+** **Aracı işi** ' nin yanındaki ' ı seçin ve *Azure Kaynak grubu dağıtımı*' nı arayın. Aşağıdaki parametreleri girin:

   |Ayar|Değer|
   |-|-|
   |Görünen ad| *MyASAJob dağıtma*|
   |Azure aboneliği| Aboneliğinizi seçin.|
   |Eylem| *Kaynak grubu oluşturma veya güncelleştirme*|
   |Kaynak grubu| Stream Analytics işinizi içerecek üretim kaynak grubu için bir ad seçin.|
   |Konum|Üretim kaynağı grubunuzun konumunu seçin.|
   |Şablon konumu| *Bağlı yapıt*|
   |Şablon| $ (Build. ArtifactStagingDirectory) \drop\myASAJob.JobTemplate.json |
   |Şablon parametreleri|($ (Build. ArtifactStagingDirectory) \drop\myASAJob.JobTemplate.parameters.json|
   |Şablon parametrelerini geçersiz kılma|-Input_IoTHub1_iotHubNamespace $ (eventhubname)|
   |Dağıtım modu|Artımlı|

### <a name="create-release"></a>Yayın oluştur

Bir yayın oluşturmak için sağ üst köşedeki **yayın oluştur** ' u seçin.

![Azure Pipelines kullanarak yayın oluşturma](./media/setup-cicd-vs-code/create-release.png)

## <a name="additional-resources"></a>Ek kaynaklar

Azure Data Lake Store Gen1 için yönetilen kimliği çıkış havuzu olarak kullanmak için, Azure 'a dağıtılmadan önce PowerShell kullanarak hizmet sorumlusuna erişim sağlamanız gerekir. [Kaynak Yöneticisi şablonuyla yönetilen kimlik ile ADLS 1. dağıtma](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment)hakkında daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio Code Azure Stream Analytics bulut işi oluşturma (Önizleme)](quick-create-vs-code.md)
* [Sorguları Visual Studio Code ile yerel olarak test Stream Analytics (Önizleme)](visual-studio-code-local-run.md)
* [Visual Studio Code ile Azure Stream Analytics araştırma (Önizleme)](visual-studio-code-explore-jobs.md)
