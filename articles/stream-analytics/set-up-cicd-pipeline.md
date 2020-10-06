---
title: Stream Analytics işi için bir CI/CD işlem hattı oluşturmak için Azure DevOps kullanın
description: Bu makalede, Azure DevOps 'da bir Azure Stream Analytics işi için sürekli tümleştirme ve dağıtım (CI/CD) işlem hattının nasıl ayarlanacağı açıklanır
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 09/10/2020
ms.openlocfilehash: d9b6dfc977aab7d8907b5d3c3851a22f96227d78
ms.sourcegitcommit: 6a4687b86b7aabaeb6aacdfa6c2a1229073254de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91757767"
---
# <a name="use-azure-devops-to-create-a-cicd-pipeline-for-a-stream-analytics-job"></a>Stream Analytics işi için bir CI/CD işlem hattı oluşturmak için Azure DevOps kullanın

Bu makalede, Azure Stream Analytics CI/CD araçlarını kullanarak Azure DevOps [derleme](/azure/devops/pipelines/get-started/pipelines-get-started) ve [yayın](/azure/devops/pipelines/release/define-multistage-release-process) işlem hatları oluşturmayı öğreneceksiniz.

## <a name="commit-your-stream-analytics-project"></a>Stream Analytics projenizi işleyin

Başlamadan önce, tüm Stream Analytics projelerinizi bir [Azure DevOps](/azure/devops/user-guide/source-control) deposuna kaynak dosya olarak kaydedin. Bu [örnek depoya](https://dev.azure.com/wenyzou/azure-streamanalytics-cicd-demo) başvurabilirsiniz ve Azure Pipelines [Stream Analytics proje kaynak kodunu](https://dev.azure.com/wenyzou/_git/azure-streamanalytics-cicd-demo?path=%2FmyASAProject) kullanabilirsiniz.

Bu makaledeki adımlarda Stream Analytics Visual Studio Code projesi kullanılır. Bir Visual Studio projesi kullanıyorsanız, [CI/CD araçlarını kullanarak bir Azure Stream Analytics işinin yapılarını, testlerini ve dağıtımlarını otomatik hale](cicd-tools.md)getirme bölümündeki adımları izleyin.

## <a name="create-a-build-pipeline"></a>Derleme işlem hattı oluşturma

Bu bölümde, derleme işlem hattı oluşturmayı öğreneceksiniz. Azure DevOps 'da Bu örnek [otomatik derleme ve test](https://dev.azure.com/wenyzou/_git/azure-streamanalytics-cicd-demo?path=%2FmyASAProject) işlem hattına başvurabilirsiniz.

1. Bir Web tarayıcısı açın ve Azure DevOps 'da projenize gidin.  

1. Sol gezinti menüsünde işlem **hatları** ' nın altında, **yapılar**' ı seçin. Ardından yeni işlem **hattı**' nı seçin.

   :::image type="content" source="media/set-up-cicd-pipeline/new-pipeline.png" alt-text="Yeni Azure işlem hattı oluşturma":::

1. YAML olmadan bir işlem hattı oluşturmak için **Klasik düzenleyiciyi kullan** ' ı seçin.

1. Kaynak türü, takım projesi ve deponuzu seçin. Sonra **devam**' ı seçin.

   :::image type="content" source="media/set-up-cicd-pipeline/select-repo.png" alt-text="Yeni Azure işlem hattı oluşturma":::

1. **Şablon seçin** sayfasında **boş iş**' ı seçin.

## <a name="install-npm-package"></a>NPM paketini yükler

1. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. Görev aramasına *NPM* girin ve **NPM**'yi seçin.

   :::image type="content" source="media/set-up-cicd-pipeline/search-npm.png" alt-text="Yeni Azure işlem hattı oluşturma":::

2. Göreve bir **görünen ad**verin. **Komut** seçeneğini *özel* olarak değiştirin ve **komut ve bağımsız değişkenler**' de aşağıdaki komutu girin. Kalan varsayılan seçenekleri bırakın.

   ```bash
   install -g azure-streamanalytics-cicd
   ```

   :::image type="content" source="media/set-up-cicd-pipeline/npm-config.png" alt-text="Yeni Azure işlem hattı oluşturma":::

## <a name="add-a-build-task"></a>Derleme görevi ekleme

1. **Değişkenler** sayfasında, işlem **hattı değişkenlerinde** **+ Ekle** ' yi seçin. Aşağıdaki değişkenleri ekleyin. Tercihinize göre aşağıdaki değerleri ayarlayın:

   |Değişken adı|Değer|
   |-|-|
   |Projectrootyolu|YourProjectName|
   |outputPath|Çıktı|
   |Dağıtım yolu|Dağıtma|

2. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. **Komut satırı**araması yapın.

3. Göreve bir **görünen ad** verin ve aşağıdaki betiği girin. Betiği depo adınızla ve proje adınızla değiştirin.

   ```bash
   azure-streamanalytics-cicd build -project $(projectRootPath)/asaproj.json -outputpath $(projectRootPath)/$(outputPath)/$(deployPath)
   ```

   Aşağıdaki görüntüde örnek olarak bir Stream Analytics Visual Studio Code projesi kullanılmaktadır.

   :::image type="content" source="media/set-up-cicd-pipeline/command-line-config-build.png" alt-text="Yeni Azure işlem hattı oluşturma":::

## <a name="add-a-test-task"></a>Test görevi ekleme

1. **Değişkenler** sayfasında, işlem **hattı değişkenlerinde** **+ Ekle** ' yi seçin. Aşağıdaki değişkenleri ekleyin. Değerleri çıkış yolunuza ve depo adınızla değiştirin.

   |Değişken adı|Değer|
   |-|-|
   |testPath|Test etme|

   :::image type="content" source="media/set-up-cicd-pipeline/pipeline-variables-test.png" alt-text="Yeni Azure işlem hattı oluşturma":::

2. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. **Komut satırı**araması yapın.

3. Göreve bir **görünen ad** verin ve aşağıdaki betiği girin. Betiği proje dosya adınızla ve test yapılandırma dosyasının yoluyla değiştirin. 

   Test çalışmalarını ekleme ve yapılandırma hakkında ayrıntılı bilgi için bkz. [Otomatikleştirilmiş test yönergeleri](cicd-tools.md#automated-test) .

   ```bash
   azure-streamanalytics-cicd test -project $(projectRootPath)/asaproj.json -outputpath $(projectRootPath)/$(outputPath)/$(testPath) -testConfigPath $(projectRootPath)/test/testConfig.json 
   ```

   :::image type="content" source="media/set-up-cicd-pipeline/command-line-config-test.png" alt-text="Yeni Azure işlem hattı oluşturma":::

## <a name="add-a-copy-files-task"></a>Dosyaları Kopyala görevi ekleme

Test Özet dosyasını ve Azure Resource Manager şablon dosyalarını yapıt klasörüne kopyalamak için Dosya Kopyala görevi eklemeniz gerekir. 

1. **Görevler** sayfasında, **+** **Aracı işi 1**' in yanındaki öğesini seçin. **Kopya dosyalarını**arayın. Ardından aşağıdaki konfigürasyonları girin. Içeriğe atayarak `**` , **Contents**test sonuçlarının tüm dosyaları kopyalanır.

   |Parametre|Giriş|
   |-|-|
   |Görünen ad|Dosyaları kopyala: $ (Build. artifactstagingdirectory)|
   |Kaynak klasör|`$(system.defaultworkingdirectory)/$(outputPath)/`|
   |İçindekiler| `**` |
   |Hedef klasör| `$(build.artifactstagingdirectory)`|

2. **Denetim seçeneklerini**genişletin. **Önceki bir görev başarısız olsa bile, derleme** **Bu görevi çalıştır**bölümünde iptal edilmediği takdirde, öğesini seçin.

   :::image type="content" source="media/set-up-cicd-pipeline/copy-config.png" alt-text="Yeni Azure işlem hattı oluşturma":::

## <a name="add-a-publish-build-artifacts-task"></a>Derleme yapıtları yayımlama görevi ekleme

1. **Görevler** sayfasında, **Aracı işi 1**' in yanındaki artı işaretini seçin. **Derleme yapıtları Yayımla** ' yı arayın ve siyah ok simgesiyle seçeneği belirleyin.

2. **Denetim seçeneklerini**genişletin. **Önceki bir görev başarısız olsa bile, derleme** **Bu görevi çalıştır**bölümünde iptal edilmediği takdirde, öğesini seçin.

   :::image type="content" source="media/set-up-cicd-pipeline/publish-config.png" alt-text="Yeni Azure işlem hattı oluşturma":::

## <a name="save-and-run"></a>Kaydet ve Çalıştır

NPM paketini, komut satırını ekleme, dosyaları kopyalama ve yapı yapıtları yayımlama görevlerini tamamladıktan sonra **& kuyruğu kaydet**' i seçin. İstendiğinde, bir kaydet yorumu girin ve **Kaydet ve Çalıştır '** ı seçin. İşlem hattının **Özet** sayfasından test sonuçlarını indirebilirsiniz.

## <a name="check-the-build-and-test-results"></a>Derlemeyi ve test sonuçlarını denetleyin

Test Özet dosyası ve Azure Resource Manager şablonu dosyaları **yayımlanan** klasörde bulunabilir.

   :::image type="content" source="media/set-up-cicd-pipeline/check-build-test-result.png" alt-text="Yeni Azure işlem hattı oluşturma":::

   :::image type="content" source="media/set-up-cicd-pipeline/check-drop-folder.png" alt-text="Yeni Azure işlem hattı oluşturma":::

## <a name="release-with-azure-pipelines"></a>Azure Pipelines yayın

Bu bölümde, bir yayın işlem hattı oluşturmayı öğreneceksiniz. Azure DevOps 'da Bu örnek [yayın](https://dev.azure.com/wenyzou/azure-streamanalytics-cicd-demo/_release?_a=releases&view=mine&definitionId=2&preserve-view=true) işlem hattına başvurabilirsiniz.

Bir Web tarayıcısı açın ve Azure Stream Analytics Visual Studio Code projenize gidin.

1. Sol gezinti menüsündeki işlem **hatları** altında **yayınlar**' ı seçin. Ardından **Yeni işlem hattı**' nı seçin.

2. **Boş bir iş ile başla**' yı seçin.

3. **Yapıtlar** kutusunda **+ yapıt Ekle**' yi seçin. **Kaynak**altında oluşturduğunuz derleme işlem hattını seçin ve **Ekle**' yi seçin.

   :::image type="content" source="media/set-up-cicd-pipeline/build-artifact.png" alt-text="Yeni Azure işlem hattı oluşturma":::

4. **İşi test ortamına dağıtmak**için **Aşama 1** adını değiştirin.

5. Yeni bir aşama ekleyin ve uygulamayı **üretim ortamına dağıtın**.

### <a name="add-deploy-tasks"></a>Dağıtım görevleri ekleme

1. Görevler açılan menüsünde **işi test et**' i seçin.

2. **+** **Aracı işi** ' nün yanındaki öğesini seçin ve **ARM şablon dağıtımı**için arama yapın. Aşağıdaki parametreleri girin:

   |Parametre|Değer|
   |-|-|
   |Görünen ad| *MyASAProject dağıtma*|
   |Azure aboneliği| Aboneliğinizi seçin.|
   |Eylem| *Kaynak grubu oluşturma veya güncelleştirme*|
   |Kaynak grubu| Stream Analytics işinizi içerecek test kaynak grubu için bir ad seçin.|
   |Konum|Test kaynak grubunuzun konumunu seçin.|
   |Şablon konumu| Bağlı yapıt|
   |Şablon| $ (System. DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/Drop/myASAProject.JobTemplate.json |
   |Şablon parametreleri|$ (System. DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/Drop/myASAProject.JobTemplate.parameters.json |
   |Şablon parametrelerini geçersiz kılma|-<arm_template_parameter ">" değeri ". **Değişkenleri**kullanarak parametreleri tanımlayabilirsiniz.|
   |Dağıtım modu|Artımlı|

3. Görevler açılan menüsünde **işi üretim ortamına dağıt**' ı seçin.

4. **+** **Aracı işi** ' nün yanındaki öğesini seçin ve *ARM şablon dağıtımı*için arama yapın. Aşağıdaki parametreleri girin:

   |Parametre|Değer|
   |-|-|
   |Görünen ad| *MyASAProject dağıtma*|
   |Azure aboneliği| Aboneliğinizi seçin.|
   |Eylem| *Kaynak grubu oluşturma veya güncelleştirme*|
   |Kaynak grubu| Stream Analytics işinizi içerecek üretim kaynak grubu için bir ad seçin.|
   |Konum|Üretim kaynağı grubunuzun konumunu seçin.|
   |Şablon konumu| *Bağlı yapıt*|
   |Şablon| $ (System. DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/Drop/myASAProject.JobTemplate.json |
   |Şablon parametreleri|$ (System. DefaultWorkingDirectory)/_azure-streamanalytics-cicd-demo-CI-Deploy/Drop/myASAProject.JobTemplate.parameters.json |
   |Şablon parametrelerini geçersiz kılma|-<arm_template_parameter> "değeri"|
   |Dağıtım modu|Artımlı|

### <a name="create-a-release"></a>Yayın oluştur

Yayın oluşturmak için sağ üst köşedeki **yayın oluştur** ' u seçin.

:::image type="content" source="media/set-up-cicd-pipeline/create-release.png" alt-text="Yeni Azure işlem hattı oluşturma":::

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics için sürekli tümleştirme ve sürekli dağıtım](cicd-overview.md)
* [CI/CD araçları kullanarak Azure Stream Analytics işinin derlemesini, test ve dağıtımını otomatikleştirin](cicd-tools.md)