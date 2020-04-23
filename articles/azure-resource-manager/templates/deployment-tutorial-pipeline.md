---
title: Azure Pipelines ile sürekli tümleştirme
description: Azure Kaynak Yöneticisi şablonlarını sürekli olarak nasıl oluşturup, test etmeyi ve dağıtmayı öğrenin.
ms.date: 04/22/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d1c56ce913a1b63bab90f5dd5aaada382abbf493
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084337"
---
# <a name="tutorial-continuous-integration-of-azure-resource-manager-templates-with-azure-pipelines"></a>Öğretici: Azure Kaynak Yöneticisi şablonlarının Azure Pipelines ile sürekli entegrasyonu

Önceki [öğreticide,](./deployment-tutorial-linked-template.md)bağlantılı bir şablon dağıtırsınız.  Bu eğitimde, Azure Kaynak Yöneticisi şablon projeleri sürekli olarak oluşturmak ve dağıtmak için Azure Pipelines'ı nasıl kullanacağınızı öğreneceksiniz.

Azure DevOps, ekiplerin iş planlamasını, kod geliştirme konusunda işbirliği yapmalarını ve uygulamaları oluşturmalarını ve dağıtmalarını desteklemek için geliştirici hizmetleri sağlar. Geliştiriciler Azure DevOps Hizmetlerini kullanarak bulutta çalışabilir. Azure DevOps, web tarayıcınız veya IDE istemciniz aracılığıyla erişebileceğiniz tümleşik bir özellik kümesi sağlar. Azure Pipeline bu özelliklerden biridir. Azure Pipelines, tam özellikli sürekli tümleştirme (CI) ve sürekli teslimat (CD) hizmetidir. Tercih ettiğiniz Git sağlayıcısıyla çalışır ve en büyük bulut hizmetlerine dağıtılabilir. Ardından kodunuzu Microsoft Azure, Google Bulut Platformu veya Amazon Web Hizmetleri'ne otomatikleştirebilirsiniz.

> [!NOTE]
> Bir proje adı seçin. Öğreticiyi incelediğinizde, **AzureRmPipeline'dan** herhangi birini proje adınız ile değiştirin.
> Bu proje adı kaynak adları oluşturmak için kullanılır.  Kaynaktan biri depolama hesabıdır. Depolama hesabı adları uzunluk ve kullanım numaraları ve küçük harfler sadece 3 ve 24 karakter arasında olmalıdır. İsim benzersiz olmalı. Şablonda, depolama hesabı adı "depo" eklenen proje adıdır ve proje adı 3 ile 11 karakter arasında olmalıdır. Bu nedenle proje adı depolama hesabı adı gereksinimlerini karşılamalı ve 11 karakterden az karaktere sahiptir.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * GitHub deposu hazırlama
> * Azure DevOps projesi oluşturma
> * Azure ardışık bir iş tonu oluşturma
> * Boru hattı dağıtımını doğrulama
> * Şablonu güncelleştirin ve yeniden dağıtın
> * Kaynakları temizleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlamak için gerekenler:

* Şablonlarınız için bir depo oluşturmak için kullandığınız bir **GitHub hesabı.** Eğer yoksa, [ücretsiz bir oluşturabilirsiniz.](https://github.com) GitHub depolarını kullanma hakkında daha fazla bilgi için [GitHub depolarını oluştur'a](/azure/devops/pipelines/repos/github)bakın.
* **Git'i yükleyin.** Bu öğretici talimat *Git Bash* veya *Git Shell*kullanır. Talimatlar için [bkz.]( https://www.atlassian.com/git/tutorials/install-git)
* **Bir Azure DevOps kuruluşu.** Eğer yoksa, ücretsiz bir oluşturabilirsiniz. Bkz. [Kuruluş veya proje koleksiyonu oluşturma.]( https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops)
* (isteğe bağlı) **Kaynak Yöneticisi Araçları uzantısı ile Visual Studio Kodu**. Bkz. [Azure Kaynak Yöneticisi şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)

## <a name="prepare-a-github-repository"></a>GitHub deposu hazırlama

GitHub, Kaynak Yöneticisi şablonları da dahil olmak üzere proje kaynak kodunuzu depolamak için kullanılır. Desteklenen diğer depolar [için Azure DevOps tarafından desteklenen depolara](/azure/devops/pipelines/repos/?view=azure-devops)bakın.

### <a name="create-a-github-repository"></a>GitHub deposu oluşturma

GitHub hesabınız [yoksa, Ön koşullara](#prerequisites)bakın.

1. [GitHub'da](https://github.com)oturum açın.
1. Sağ üst köşedeki hesap resminizi seçin ve ardından **depolarınızı**seçin.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Ardışık Hatları GitHub deposu oluşturuyor](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-github-repository.png)

1. Yeşil düğme yle **Yeni'yi**seçin.
1. **Depo adına,** bir depo adı girin.  Örneğin, **AzureRmPipeline-repo**. **AzureRmPipeline'dan** herhangi birini proje adınız ile değiştirmeyi unutmayın. Bu öğretici geçmesi için **Ya Ortak** veya **özel** seçebilirsiniz. Ve sonra **depo oluştur'u**seçin.
1. URL'yi yazın. Depo URL'si aşağıdaki biçimdir - ** https://github.com/[Hesap Adınız]/[RepositoryName]**.

Bu depo, *uzak*depo olarak adlandırılır. Aynı projenin geliştiricilerinin her biri kendi *yerel deposunu*klonlayabilir ve değişiklikleri uzak depoda birleştirilebilir.

### <a name="clone-the-remote-repository"></a>Uzak depoyu klonlama

1. Git Shell veya Git Bash'i açın.  [Ön koşullara](#prerequisites) bakın.
1. Geçerli klasörünüzün **GitHub**olduğunu doğrulayın.
1. Şu komutu çalıştırın:

    ```bash
    git clone https://github.com/[YourAccountName]/[YourGitHubRepositoryName]
    cd [YourGitHubRepositoryName]
    mkdir CreateWebApp
    cd CreateWebApp
    pwd
    ```

    **[Hesap Adınızı]** GitHub hesap adınız ile değiştirin ve **[YourGitHubRepositoryName]** ile önceki yordamda oluşturduğunuz depo adını değiştirin.

**CreateWebApp** klasörü şablonun depolandığı klasördür. **pwd komutu** klasör yolunu gösterir. Yol, şablonu aşağıdaki yordamda kaydettiğiniz yoldur.

### <a name="download-a-quickstart-template"></a>Quickstart şablonu indirin

Şablonları oluşturmak yerine, şablonları indirebilir ve Bunları **CreateWebApp** klasörüne kaydedebilirsiniz.

* Ana şablon:https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/azuredeploy.json
* Bağlantılı şablon:https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/linkedStorageAccount.json

Hem klasör adı hem de dosya adları, ardışık ardışık lıkta olduğu gibi kullanılır.  Bu adları değiştirirseniz, ardışık alanda kullanılan adları güncelleştirmeniz gerekir.

### <a name="push-the-template-to-the-remote-repository"></a>Şablonu uzak depoya itin

Azuredeploy.json yerel depoya eklendi. Ardından, şablonu uzak depoya yüklersiniz.

1. Açık *Git Shell* veya Git *Bash*, açılmıyorsa.
1. Yerel deponuzdaki CreateWebApp klasörüne dizin değiştirin.
1. **Azuredeploy.json** dosyasının klasörde olduğunu doğrulayın.
1. Şu komutu çalıştırın:

    ```bash
    git add .
    git commit -m "Add web app templates."
    git push origin master
    ```

    LF hakkında bir uyarı alabilirsiniz. Uyarıyı yok sayabilirsiniz. **usta** ana dalıdır.  Genellikle her güncelleştirme için bir dal oluşturursunuz. Öğreticiyi basitleştirmek için ana dalı doğrudan kullanırsınız.
1. GitHub deponuza bir tarayıcıdan göz atın.  URL ** https://github.com/[Hesap Name]/[YourGitHubRepository]**. **CreateWebApp** klasörünü ve klasörün içindeki üç dosyayı görürsünüz.
1. Şablonu açmak için **linkedStorageAccount.json'ı** seçin.
1. **Raw** düğmesini seçin. URL **raw.githubusercontent.com**ile başlatılır.
1. URL’nin kopyasını oluşturun.  Ardışık yapıyı daha sonra öğreticide yapılandırırken bu değeri sağlamanız gerekir.

Şimdiye kadar bir GitHub deposu oluşturdunuz ve şablonları depoya yükledin.

## <a name="create-a-devops-project"></a>DevOps projesi oluşturma

Bir sonraki yordamı devam etmeden önce bir DevOps organizasyonu gereklidir.  Eğer yoksa, [Önkoşullar](#prerequisites)bakın.

1. [Azure DevOps'te](https://dev.azure.com)oturum açın.
1. Soldan bir DevOps kuruluşu seçin.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları Azure DevOps projesini oluşturuyor](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-create-devops-project.png)

1. **New project** (Yeni proje) öğesini seçin. Herhangi bir projeniz yoksa, proje oluşturma sayfası otomatik olarak açılır.
1. Aşağıdaki değerleri girin:

    * **Proje adı**: proje adını girin. Öğreticinin başında seçtiğiniz proje adını kullanabilirsiniz.
    * **Sürüm denetimi**: **Git'i**seç. Sürüm **denetimini** görmek **Version control**için Gelişmiş'i genişletmeniz gerekebilir.

    Diğer özellikler için varsayılan değeri kullanın.
1. **Oluştur**’u seçin.

Projeleri Azure'a dağıtmak için kullanılan bir hizmet bağlantısı oluşturun.

1. Sol menünün alt kısmından **Proje ayarlarını** seçin.
1. **Boru Hatları**altında Hizmet **bağlantılarını** seçin.
1. **Yeni Hizmet bağlantısını**seçin, Azure Kaynak **Yöneticisi'ni**seçin ve sonra **İleri'yi**seçin.
1. **Servis anası'nı**seçin ve sonra **İleri'yi**seçin.
1. Aşağıdaki değerleri girin:

    * **Kapsam düzeyi**: **Abonelik'i**seçin.
    * **Abonelik**: aboneliğinizi seçin.
    * **Kaynak Grubu**: Boş bırakın.
    * **Bağlantı adı**: bağlantı adı girin. Örneğin, **AzureRmPipeline-conn**. Bu adı yazın, ardışık alanı oluştururken isme ihtiyacınız var.
    * **Tüm boru hatlarına erişim izni ver.** (seçilmiş)
1. **Kaydet**’i seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Şimdiye kadar, aşağıdaki görevleri tamamladınız.  GitHub ve DevOps'leri bildiğiniz için önceki bölümleri atlarsanız, devam etmeden önce görevleri tamamlamanız gerekir.

* Bir GitHub deposu oluşturun ve şablonları depodaki **CreateWebApp** klasörüne kaydedin.
* Bir DevOps projesi oluşturun ve bir Azure Kaynak Yöneticisi hizmet bağlantısı oluşturun.

Şablon dağıtmak için adım içeren bir ardışık hatlar oluşturmak için:

1. Sol menüden **Boru Hatları'nı** seçin.
1. **Yeni ardışık hattı**seçin.
1. **Bağlan** sekmesinden **GitHub**’ı seçin. İstenirse, GitHub kimlik bilgilerinizi girin ve ardından yönergeleri izleyin. Aşağıdaki ekranı görürseniz, **Yalnızca depoları seçin**ve **& Yükle'yi onaylamayı**seçmeden önce deponuzun listede olduğunu doğrulayın.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Ardışık Hatları yalnızca belirli depolar](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-only-select-repositories.png)

1. **Seç** sekmesinden deponuzu seçin.  Varsayılan adı **[YourAccountName]/[YourGitHubRepositoryName]**.
1. **Yapılandırma** sekmesinden Başlangıç **ardışık hattını**seçin. İki komut dosyası adımı yla **azure-pipelines.yml** pipeline dosyasını gösterir.
1. Yml dosyasından iki komut dosyası adımını silin.
1. **İmleci adımlardan**sonra satıra taşıyın: .
1. **Görevler** bölmesini açmak için ekranın sağındaki **Asistanı Göster'i** seçin.
1. **ARM şablon dağıtımı'nı**seçin.
1. Aşağıdaki değerleri girin:

    * **deploymentScope**: **Kaynak Grubu'ni**seçin... Kapsamlar hakkında daha fazla bilgi edinmek için [Dağıtım kapsamlarına](deploy-rest.md#deployment-scope)bakın.
    * **Azure Kaynak Yöneticisi bağlantısı**: Daha önce oluşturduğunuz hizmet bağlantı adını seçin.
    * **Abonelik**: Hedef abonelik kimliğini belirtin.
    * **Eylem**: **Kaynak Grubu Oluştur veya Güncelleştir'i** seçin eylem 2 eylem yapar - 1. yeni bir kaynak grubu adı sağlanırsa bir kaynak grubu oluşturun; 2. belirtilen şablonu dağıtın.
    * **Kaynak grubu**: Yeni bir kaynak grubu adı girin. Örneğin, **AzureRmPipeline-rg**.
    * **Konum**: Kaynak grubu için bir konum seçin, örneğin, **Orta ABD.**
    * **Şablon konumu**: Görevin şablon dosyasını doğrudan bağlı depodan aradığı anlamına gelen **Bağlantılı yapıyı**seçin.
    * **Şablon**: **CreateWebApp/azuredeploy.json**girin. Klasör adını ve dosya adını değiştirdiyseniz, bu değeri değiştirmeniz gerekir.
    * **Şablon parametreleri**: Bu alanı boş bırakın. **Geçersiz kılma şablonparametrelerinde parametre değerlerini belirtirsiniz.
    * **overrideParametreleri**: Girin **-projectName [EnterAProjectName] -linkedTemplateUri [EnterTheLinkedTemplateURL]**. Proje adını ve bağlı şablon url'sini değiştirin. Bağlantılı şablon URL'si, [GitHub deposu Oluştur'un](#create-a-github-repository)sonunda yazdığınız dır.
    * **Dağıtım modu**: **Artımlı'yı**seçin.
    * **Dağıtım adı**: **DeployPipelineTemplate**girin . **Dağıtım adını**görmeden önce **Gelişmiş'i** seçin.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları adımı](./media/deployment-tutorial-pipeline/resource-manager-template-pipeline-configure.png)

1. **Add (Ekle)** seçeneğini belirleyin.

    Görev hakkında daha fazla bilgi için [Azure Kaynak Grubu Dağıtım görevi](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment)ve Azure Kaynak Yöneticisi şablon dağıtım [görevine](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md) bakın

    YML dosyası aşağıdakilere benzer olacaktır:

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları yaml](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-yml.png)

1. **Kaydet ve çalıştır**’ı seçin.
1. **Kaydet ve çalıştır** bölmesinden **Kaydet'i** seçin ve yeniden çalıştırın. YAML dosyasının bir kopyası bağlı depoya kaydedilir. YAML dosyasını deponuza göz atarak görebilirsiniz.
1. Ardışık hattın başarıyla yürütüldünden doğrulayın.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları yaml](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-status.png)

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kaynak grubunu açın. Ad, boru hattı YAML dosyasında belirttiğiniz addır.  Bir depolama hesabı nın oluşturulduğunu göreceksiniz.  Depolama hesabı adı **depo**ile başlar.
1. Açmak için depolama hesabı adını seçin.
1. **Özellikleri**seçin. **Çoğaltma** yerel **olarak yedekli depolama (LRS)** olduğuna dikkat edin.

## <a name="update-and-redeploy"></a>Güncelleştirme ve yeniden dağıtma

Şablonu güncelleştirdiğinizde ve değişiklikleri uzak depoya itdiğinizde, ardışık işlem bu durumda depolama hesabı olan kaynakları otomatik olarak güncelleştirir.

1. Visual Studio Code veya herhangi bir metin düzenleyicisi yerel deposundan **linkedStorageAccount.json'ı** açın.
1. **depolamaHesap Türü** **varsayılan Değerini** **Standard_GRS**güncelleştirin. Aşağıdaki ekran görüntüsüne bakın:

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Pipelines yaml güncelleştirme](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-update-yml.png)

1. Değişiklikleri kaydedin.
1. Git Bash/Shell'in aşağıdaki komutlarını çalıştırarak değişiklikleri uzak depoya itin.

    ```bash
    git pull origin master
    git add .
    git commit -m "Update the storage account type."
    git push origin master
    ```

    İlk komut (çekme) yerel depoyu uzak depoyla eşitler. Boru hattı YAML dosyası yalnızca uzak depoya eklendi. Çekme komutu çalıştırıl, YAML dosyasının bir kopyasını yerel şubeye indirir.

    Dördüncü komut (itme) gözden geçirilmiş linkedStorageAccount.json dosyasını uzak depoya yükler. Uzak deponun ana dalı güncelleştirilmesiyle, boru hattı yeniden ateşlenir.

Değişiklikleri doğrulamak için depolama hesabının Çoğaltma özelliğini denetleyebilirsiniz.  Bkz. [Dağıtımı doğrula.](#verify-the-deployment)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

GitHub deposunu ve Azure DevOps projesini de silmek isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, bu Kaynak Yöneticisi şablon dağıtım eğitimini tamamladınız. Geri bildirim bölümünde herhangi bir yorumunuz ve öneriniz varsa bize bildirin. Teşekkür ederiz!
Şablonlarla ilgili daha gelişmiş kavramlara atlamaya hazırsınız. Bir sonraki öğretici, dağıtılacak kaynakları tanımlamaya yardımcı olmak için şablon başvuru belgelerini kullanma hakkında daha fazla ayrıntıya gider.

> [!div class="nextstepaction"]
> [Şablon başvurusunu kullanma](./template-tutorial-use-template-reference.md)
