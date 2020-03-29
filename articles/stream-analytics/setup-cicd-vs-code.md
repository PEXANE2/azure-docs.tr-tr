---
title: CI/CD npm paketini kullanarak Azure Akışı Analizi işini dağıtma
description: Bu makalede, sürekli bir tümleştirme ve dağıtım işlemi ayarlamak için Azure Akış Analizi CI/CD npm paketinin nasıl kullanılacağı açıklanmaktadır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/28/2020
ms.openlocfilehash: deb6c2439cc84f196b7f42fd9f49d3ebfd057cbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76962230"
---
# <a name="deploy-an-azure-stream-analytics-job-using-cicd-npm-package"></a>CI/CD npm paketini kullanarak Azure Akışı Analizi işini dağıtma 

Stream Analytics işleriniz için sürekli bir tümleştirme ve dağıtım işlemi ayarlamak için Azure Akış Analizi CI/CD npm paketini kullanabilirsiniz. Bu makalede, npm paketinin genel olarak herhangi bir CI/CD sistemiyle nasıl kullanılacağının yanı sıra Azure Pipelines ile dağıtım için özel yönergeler açıklanmaktadır.

Powershell ile dağıtma hakkında daha fazla bilgi için kaynak [yöneticisi şablon dosyası ve Azure PowerShell ile dağıtma'ya](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)bakın. Ayrıca, [Kaynak Yöneticisi şablonunda bir nesnenin parametre olarak](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters)nasıl kullanılacağı hakkında daha fazla bilgi edinebilirsiniz.

## <a name="build-the-vs-code-project"></a>VS Kodu projesini oluşturun

**Asa-streamanalytics-cicd** npm paketini kullanarak Azure Akış Analizi işleri için sürekli tümleştirme ve dağıtım sağlayabilirsiniz. npm [paketi, Stream Analytics Visual Studio Code projelerinin](quick-create-vs-code.md)Azure Kaynak Yöneticisi şablonlarını oluşturmak için araçlar sağlar. Visual Studio Code yüklemeden Windows, macOS ve Linux'ta kullanılabilir.

Paketi doğrudan [indirebilir](https://www.npmjs.com/package/azure-streamanalytics-cicd) veya komut u yla `npm install -g azure-streamanalytics-cicd` genel [olarak](https://docs.npmjs.com/downloading-and-installing-packages-globally) yükleyebilirsiniz. Bu, Azure Ardışık Bir yapı ardışık hattının PowerShell veya Azure CLI komut dosyası görevinde de kullanılabilen önerilen **yaklaşımdır.**

Paketi yükledikten sonra, Azure Kaynak Yöneticisi şablonlarını çıktılamak için aşağıdaki komutu kullanın. **ScriptPath** bağımsız değişkeni projenizdeki **asaql** dosyasına giden mutlak yoldur. Asaproj.json ve JobConfig.json dosyalarının komut dosyası dosyasıyla aynı klasörde olduğundan emin olun. **OutputPath** belirtilmemişse, şablonlar projenin **çöp kutusu** klasörü altında **Dağıt** klasörüne yerleştirilir.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Örnek (macOS'ta)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

Bir Akış Analizi Görsel Stüdyo Kodu projesi başarıyla oluşturduğunda, **depo gözü/[Hata Ayıklama/Perakende Satış]/Dağıt** klasörü altında aşağıdaki iki Azure Kaynak Yöneticisi şablon dosyası oluşturur: 

*  Kaynak Yöneticisi şablon dosyası

       [ProjectName].JobTemplate.json 

*  Kaynak Yöneticisi parametreleri dosyası

       [ProjectName].JobTemplate.parameters.json   

parameters.json dosyasındaki varsayılan parametreler Visual Studio Code projenizdeki ayarlardan dır. Başka bir ortama dağıtmak istiyorsanız, parametreleri buna göre değiştirin.

> [!NOTE]
> Tüm kimlik bilgileri için varsayılan değerler null olarak ayarlanır. Buluta dağıtılmadan önce değerleri ayarlamanız **gerekir.**

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```

## <a name="deploy-with-azure-pipelines"></a>Azure Pipelines ile dağıtma

Bu bölümde, npm kullanarak Azure Ardışık Hatları [oluşturma](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav) ve [serbest bırakma](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts) nasıl oluşturulacak ayrıntıları.

Bir web tarayıcısı açın ve Azure Akışı Analitik Görsel Stüdyo Kodu projenize gidin.

1. Sol gezinti menüsünde **Pipelines** altında **Yapılar'ı**seçin. Ardından **Yeni ardışık**

   ![Yeni Azure Ardışık Bir Erzama Oluşturma](./media/setup-cicd-vs-code/new-pipeline.png)

2. YAML olmadan bir boru hattı oluşturmak için **klasik düzenleyiciyi kullan'ı** seçin.

3. Kaynak türünüzü, takım projenizi ve deponuzu seçin. Daha sonra **Devam** seçeneğini belirleyin.

   ![Azure Akış Analizi projesini seçin](./media/setup-cicd-vs-code/select-repo.png)

4. **Şablon** seç sayfasında Boş **iş'i**seçin.

### <a name="add-npm-task"></a>npm görev ekle

1. **Görevler** sayfasında, **Aracı iş 1'in**yanındaki artı işaretini seçin. Görev aramasında "npm" girin ve **npm'yi**seçin.

   ![npm görev seçin](./media/setup-cicd-vs-code/search-npm.png)

2. Göreve bir **Görüntü adı**verin. **Komut** seçeneğini *özel* olarak değiştirin ve Komut ve **bağımsız değişkenlerde**aşağıdaki komutu girin. Kalan varsayılan seçenekleri bırakın.

   ```cmd
   install -g azure-streamanalytics-cicd
   ```

   ![npm görevi için yapılandırmaları girin](./media/setup-cicd-vs-code/npm-config.png)

### <a name="add-command-line-task"></a>Komut satırı görevi ekleme

1. **Görevler** sayfasında, **Aracı iş 1'in**yanındaki artı işaretini seçin. Komut **satırı**için arama .

2. Göreve bir **Görüntü adı** verin ve aşağıdaki komut dosyasını girin. Komut dosyasını depo adınız ve proje adınız ile değiştirin.

   ```cmd
   azure-streamanalytics-cicd build -scriptPath $(Build.SourcesDirectory)/myASAProject/myASAProj.asaql
   ```

   ![Komut satırı görevi için yapılandırmaları girin](./media/setup-cicd-vs-code/commandline-config.png)

### <a name="add-copy-files-task"></a>Kopya dosyaları görev ekleme

1. **Görevler** sayfasında, **Aracı iş 1'in**yanındaki artı işaretini seçin. Kopya **la dosyalarını**arayın. Ardından aşağıdaki yapılandırmaları girin.

   |Parametre|Giriş|
   |-|-|
   |Görünen ad|Dosyaları kopyala: $(build.artifactstagingdirectory)|
   |Kaynak Klasör|`$(system.defaultworkingdirectory)`| 
   |İçindekiler| `**\Deploy\**` |
   |Hedef Klasör| `$(build.artifactstagingdirectory)`|

   ![Kopyalama görevi için yapılandırmaları girin](./media/setup-cicd-vs-code/copy-config.png)

### <a name="add-publish-build-artifacts-task"></a>Yapı yapı larını yayımla görev ekle

1. **Görevler** sayfasında, **Aracı iş 1'in**yanındaki artı işaretini seçin. Yapı **yapılarını** yayımla'yı arayın ve siyah ok simgesiyle seçeneği belirleyin. 

2. Varsayılan yapılandırmaların hiçbirini değiştirmeyin.

### <a name="save-and-run"></a>Kaydet ve çalıştır

npm, komut satırı, dosyaları kopyalama ve yapı yapı görevlerini yayımlamayı bitirdikten sonra **& sırasını kaydet'i**seçin. Sizden istendiğinde, bir kaydet yorumu girin ve **Kaydet ve çalıştır'ı**seçin.

## <a name="release-with-azure-pipelines"></a>Azure Ardışık Hatları ile Sürüm

Bir web tarayıcısı açın ve Azure Akışı Analitik Görsel Stüdyo Kodu projenize gidin.

1. Sol gezinti menüsünde **Pipelines** **altında, Sürümler'i**seçin. Ardından **Yeni ardışık hattı**seçin.

2. **Boş bir iş ile başlat'ı**seçin.

3. **Yapılar** kutusunda , **+ Yapı ekle'yi**seçin. **Kaynak**altında, yeni oluşturduğunuz yapı ardışık hattını seçin ve **Ekle'yi**seçin.

   ![Yapı boru hattı artifakı girin](./media/setup-cicd-vs-code/build-artifact.png)

4. **Test ortamına işi dağıtmak için**Aşama **1'in** adını değiştirin.

5. Yeni bir aşama ekleyin ve **üretim ortamına iş dağıtın**adını.

### <a name="add-tasks"></a>Görev ekleme

1. Görev açılır tarihinden, **test ortamıiçin İşi dağıt'ı**seçin. 

2. Aracı **+** **işinin** yanındakini seçin ve *Azure kaynak grubu dağıtımı*için arama yapın. Aşağıdaki parametreleri girin:

   |Ayar|Değer|
   |-|-|
   |Görünen ad| *myASAJob'ı dağıtın*|
   |Azure aboneliği| Aboneliğinizi seçin.|
   |Eylem| *Kaynak grubu oluşturma veya güncelleştirme*|
   |Kaynak grubu| Akış Analizi işinizi içerecek test kaynak grubu için bir ad seçin.|
   |Konum|Test kaynak grubunuzun konumunu seçin.|
   |Şablon konumu| *Bağlantılı artefakt*|
   |Şablon| $(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.json |
   |Şablon parametreleri|($(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.parameters.json|
   |Şablon parametrelerini geçersiz kılma|-Input_IoTHub1_iotHubNamespace $(test_eventhubname)|
   |Dağıtım modu|Artımlı|

3. Görev açılır tarihinden, **işi üretim ortamına dağıt'ı**seçin.

4. Aracı **+** **işinin** yanındakini seçin ve *Azure kaynak grubu dağıtımı*için arama yapın. Aşağıdaki parametreleri girin:

   |Ayar|Değer|
   |-|-|
   |Görünen ad| *myASAJob'ı dağıtın*|
   |Azure aboneliği| Aboneliğinizi seçin.|
   |Eylem| *Kaynak grubu oluşturma veya güncelleştirme*|
   |Kaynak grubu| Stream Analytics işinizi içerecek üretim kaynak grubu için bir ad seçin.|
   |Konum|Üretim kaynak grubunuzun konumunu seçin.|
   |Şablon konumu| *Bağlantılı artefakt*|
   |Şablon| $(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.json |
   |Şablon parametreleri|($(Build.ArtifactStagingDirectory)\drop\myASAJob.JobTemplate.parameters.json|
   |Şablon parametrelerini geçersiz kılma|-Input_IoTHub1_iotHubNamespace $(eventhubname)|
   |Dağıtım modu|Artımlı|

### <a name="create-release"></a>Sürüm oluşturma

Sürüm oluşturmak için sağ üst köşede **sürüm oluştur'u** seçin.

![Azure Ardışık Hatlar'ı kullanarak sürüm oluşturma](./media/setup-cicd-vs-code/create-release.png)

## <a name="additional-resources"></a>Ek kaynaklar

Azure Veri Gölü Deposu Gen1 için Yönetilen Kimlik'i çıktı olarak kullanmak için, Azure'a dağıtmadan önce PowerShell'i kullanarak hizmet ilkesine Erişim sağlamanız gerekir. [Kaynak Yöneticisi şablonu yla Yönetilen Kimlik ile ADLS Gen1'in nasıl dağıtılancaya kadar dağıtılabildiğini](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment)öğrenin.


## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç: Visual Studio Code'da Bir Azure Akışı Analizi bulut işi oluşturun (Önizleme)](quick-create-vs-code.md)
* [Visual Studio Code (Önizleme) ile yerel olarak Test Akışı Analizi sorguları](visual-studio-code-local-run.md)
* [Visual Studio Code (Önizleme) ile Azure Akış Analizini Keşfedin](visual-studio-code-explore-jobs.md)
