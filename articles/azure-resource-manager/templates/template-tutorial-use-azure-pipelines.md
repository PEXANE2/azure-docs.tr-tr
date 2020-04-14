---
title: Azure Pipelines ile sürekli tümleştirme
description: Azure Kaynak Yöneticisi şablonlarını sürekli olarak nasıl oluşturup, test etmeyi ve dağıtmayı öğrenin.
author: mumian
ms.date: 10/29/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: b117861a528b6983876d28d5b343ea88c2bcadc0
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260692"
---
# <a name="tutorial-continuous-integration-of-azure-resource-manager-templates-with-azure-pipelines"></a>Öğretici: Azure Kaynak Yöneticisi şablonlarının Azure Pipelines ile sürekli entegrasyonu

Azure Kaynak Yöneticisi şablon projeleri oluşturmak ve dağıtmak için Azure Pipelines'ı nasıl kullanacağınızı öğrenin.

Azure DevOps, ekiplerin iş planlamasını, kod geliştirme konusunda işbirliği yapmalarını ve uygulamaları oluşturmalarını ve dağıtmalarını desteklemek için geliştirici hizmetleri sağlar. Geliştiriciler Azure DevOps Hizmetlerini kullanarak bulutta çalışabilir. Azure DevOps, web tarayıcınız veya IDE istemciniz aracılığıyla erişebileceğiniz tümleşik bir özellik kümesi sağlar. Azure Pipeline bu özelliklerden biridir. Azure Pipelines, tam özellikli sürekli tümleştirme (CI) ve sürekli teslimat (CD) hizmetidir. Tercih ettiğiniz Git sağlayıcısıyla çalışır ve en büyük bulut hizmetlerine dağıtılabilir. Ardından kodunuzu Microsoft Azure, Google Bulut Platformu veya Amazon Web Hizmetleri'ne otomatikleştirebilirsiniz.

Bu öğretici, yeni Azure DevOps Hizmetleri ve Azure Denetim Hatları olan Azure Kaynak Yöneticisi şablon geliştiricileri için tasarlanmıştır. GitHub ve DevOps'e zaten aşinaysanız, [bir ardışık hat lar oluşturmak](#create-a-pipeline)için atlayabilirsiniz.

> [!NOTE]
> Bir proje adı seçin. Öğreticiyi incelediğinizde, **AzureRmPipeline'dan** herhangi birini proje adınız ile değiştirin.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * GitHub deposu hazırlama
> * Azure DevOps projesi oluşturma
> * Azure ardışık bir iş tonu oluşturma
> * Boru hattı dağıtımını doğrulama
> * Şablonu güncelleştirin ve yeniden dağıtın
> * Kaynakları temizleme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Şablonlarınız için bir depo oluşturmak için kullandığınız bir **GitHub hesabı.** Eğer yoksa, [ücretsiz bir oluşturabilirsiniz.](https://github.com) GitHub depolarını kullanma hakkında daha fazla bilgi için [GitHub depolarını oluştur'a](/azure/devops/pipelines/repos/github)bakın.
* **Git'i yükleyin.** Bu öğretici talimat *Git Bash* veya *Git Shell*kullanır. Talimatlar için [bkz.]( https://www.atlassian.com/git/tutorials/install-git)
* **Bir Azure DevOps kuruluşu.** Eğer yoksa, ücretsiz bir oluşturabilirsiniz. Bkz. [Kuruluş veya proje koleksiyonu oluşturma.]( https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops)
* Visual Studio Code ve Resource Manager Araçları uzantısı. Bkz. [Azure Kaynak Yöneticisi şablonları oluşturmak için Görsel Stüdyo Kodunu Kullan.](use-vs-code-to-create-template.md)

## <a name="prepare-a-github-repository"></a>GitHub deposu hazırlama

GitHub, Kaynak Yöneticisi şablonları da dahil olmak üzere proje kaynak kodunuzu depolamak için kullanılır. Desteklenen diğer depolar [için Azure DevOps tarafından desteklenen depolara](/azure/devops/pipelines/repos/?view=azure-devops)bakın.

### <a name="create-a-github-repository"></a>GitHub deposu oluşturma

GitHub hesabınız [yoksa, Ön koşullara](#prerequisites)bakın.

1. [GitHub'da](https://github.com)oturum açın.
2. Sağ üst köşedeki hesap resminizi seçin ve ardından **depolarınızı**seçin.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Ardışık Hatları GitHub deposu oluşturuyor](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-repository.png)

1. Yeşil düğme yle **Yeni'yi**seçin.
1. **Depo adına,** bir depo adı girin.  Örneğin, **AzureRmPipeline-repo**. **AzureRmPipeline'dan** herhangi birini proje adınız ile değiştirmeyi unutmayın. Bu öğretici geçmesi için **Ya Ortak** veya **özel** seçebilirsiniz. Ve sonra **depo oluştur'u**seçin.
1. URL'yi yazın. Depo URL'si aşağıdaki biçimdir:

    ```url
    https://github.com/[YourAccountName]/[YourRepositoryName]
    ```

Bu depo, *uzak*depo olarak adlandırılır. Aynı projenin geliştiricilerinin her biri kendi *yerel deposunu*klonlayabilir ve değişiklikleri uzak depoda birleştirilebilir.

### <a name="clone-the-remote-repository"></a>Uzak depoyu klonlama

1. Git Shell veya Git Bash'i açın.  [Ön koşullara](#prerequisites) bakın.
1. Geçerli klasörünüzün **github**olduğunu doğrulayın.
1. Şu komutu çalıştırın:

    ```bash
    git clone https://github.com/[YourAccountName]/[YourGitHubRepositoryName]
    cd [YourGitHubRepositoryName]
    mkdir CreateAzureStorage
    cd CreateAzureStorage
    pwd
    ```

    **[Hesap Adınızı]** GitHub hesap adınız ile değiştirin ve **[YourGitHubRepositoryName]** ile önceki yordamda oluşturduğunuz depo adını değiştirin.

    Aşağıdaki ekran görüntüsü bir örnek gösterir.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları GitHub bash oluşturur](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-github-bash.png)

**CreateAzureStorage** klasörü şablonun depolandığı klasördür. **pwd komutu** klasör yolunu gösterir. Yol, şablonu aşağıdaki yordamda kaydettiğiniz yoldur.

### <a name="download-a-quickstart-template"></a>Quickstart şablonu indirin

Şablon oluşturmak yerine, [Bir Quickstart şablonu]( https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json)indirebilirsiniz. Bu şablon bir Azure Depolama hesabı oluşturur.

1. Visual Studio kodunu açın. [Ön koşullara](#prerequisites) bakın.
2. Şablonu aşağıdaki URL ile açın:

    ```URL
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```

3. Dosyayı **azuredeploy.json** olarak **CreateAzureStorage** klasörüne kaydedin. Hem klasör adı hem de dosya adı, ardışık ardışık lıkta olduğu gibi kullanılır.  Bu adları değiştirirseniz, ardışık alanda kullanılan adları güncelleştirmeniz gerekir.

### <a name="push-the-template-to-the-remote-repository"></a>Şablonu uzak depoya itin

Azuredeploy.json yerel depoya eklendi. Ardından, şablonu uzak depoya yüklersiniz.

1. Açık *Git Shell* veya Git *Bash*, açılmıyorsa.
1. Yerel deponuzdaki CreateAzureStorage klasöründe dizindeğiştirin.
1. **Azuredeploy.json** dosyasının klasörde olduğunu doğrulayın.
1. Şu komutu çalıştırın:

    ```bash
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    LF hakkında bir uyarı alabilirsiniz. Uyarıyı yok sayabilirsiniz. **usta** ana dalıdır.  Genellikle her güncelleştirme için bir dal oluşturursunuz. Öğreticiyi basitleştirmek için ana dalı doğrudan kullanırsınız.
1. GitHub deponuza bir tarayıcıdan göz atın.  URL ** https://github.com/[Hesap Name]/[YourGitHubRepository]**. **CreateAzureStorage** klasörünü ve **Azuredeploy.json** klasörünü göreceksiniz.

Şimdiye kadar bir GitHub deposu oluşturdunuz ve depoya bir şablon yükledin.

## <a name="create-a-devops-project"></a>DevOps projesi oluşturma

Bir sonraki yordamı devam etmeden önce bir DevOps organizasyonu gereklidir.  Eğer yoksa, [Önkoşullar](#prerequisites)bakın.

1. [Azure DevOps'te](https://dev.azure.com)oturum açın.
1. Soldan bir DevOps kuruluşu seçin.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları Azure DevOps projesini oluşturuyor](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-create-devops-project.png)

1. **Create project** (Proje oluştur) öğesini seçin. Herhangi bir projeniz yoksa, proje oluşturma sayfası otomatik olarak açılır.
1. Aşağıdaki değerleri girin:

    * **Proje adı**: proje adını girin. Öğreticinin başında seçtiğiniz proje adını kullanabilirsiniz.
    * **Sürüm denetimi**: **Git'i**seç. Sürüm **denetimini** görmek **Version control**için Gelişmiş'i genişletmeniz gerekebilir.

    Diğer özellikler için varsayılan değeri kullanın.
1. **Create project** (Proje oluştur) öğesini seçin.

Projeleri Azure'a dağıtmak için kullanılan bir hizmet bağlantısı oluşturun.

1. Sol menünün alt kısmından **Proje ayarlarını** seçin.
1. **Boru Hatları**altında Hizmet **bağlantılarını** seçin.
1. **Yeni Hizmet bağlantısını**seçin ve ardından **AzureResourceManager'ı**seçin.
1. Aşağıdaki değerleri girin:

    * **Bağlantı adı**: bağlantı adı girin. Örneğin, **AzureRmPipeline-conn**. Bu adı yazın, ardışık alanı oluştururken isme ihtiyacınız var.
    * **Kapsam düzeyi**: **Abonelik'i**seçin.
    * **Abonelik**: aboneliğinizi seçin.
    * **Kaynak Grubu**: Boş bırakın.
    * **Tüm ardışık lıkların bu bağlantıyı kullanmasına izin verin.** (seçilmiş)
1. **Tamam'ı**seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Şimdiye kadar, aşağıdaki görevleri tamamladınız.  GitHub ve DevOps'leri bildiğiniz için önceki bölümleri atlarsanız, devam etmeden önce görevleri tamamlamanız gerekir.

- Bir GitHub deposu oluşturun ve [bu şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) depodaki **CreateAzureStorage** klasörüne kaydedin.
- Bir DevOps projesi oluşturun ve bir Azure Kaynak Yöneticisi hizmet bağlantısı oluşturun.

Şablon dağıtmak için adım içeren bir ardışık hatlar oluşturmak için:

1. Sol menüden **Boru Hatları'nı** seçin.
1. **Yeni ardışık hattı**seçin.
1. **Bağlan** sekmesinden **GitHub**’ı seçin. İstenirse, GitHub kimlik bilgilerinizi girin ve ardından yönergeleri izleyin. Aşağıdaki ekranı görürseniz, **Yalnızca depoları seçin**ve **& Yükle'yi onaylamayı**seçmeden önce deponuzun listede olduğunu doğrulayın.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Ardışık Hatları yalnızca belirli depolar](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-only-select-repositories.png)

1. **Seç** sekmesinden deponuzu seçin.  Varsayılan adı **[YourAccountName]/[YourGitHubRepositoryName]**.
1. **Yapılandırma** sekmesinden Başlangıç **ardışık hattını**seçin. İki komut dosyası adımı yla **azure-pipelines.yml** pipeline dosyasını gösterir.
1. **Adımlar** bölümünü aşağıdaki YAML ile değiştirin:

    ```yaml
    steps:
    - task: AzureResourceManagerTemplateDeployment@3
      inputs:
        deploymentScope: 'Resource Group'
        ConnectedServiceName: '[EnterYourServiceConnectionName]'
        subscriptionName: '[EnterTheTargetSubscriptionID]'
        action: 'Create Or Update Resource Group'
        resourceGroupName: '[EnterANewResourceGroupName]'
        location: 'Central US'
        templateLocation: 'Linked artifact'
        csmFile: 'CreateAzureStorage/azuredeploy.json'
        deploymentMode: 'Incremental'
    ```

    Şöyle görünecektir:

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları yaml](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-yml.png)

    Aşağıdaki değişiklikleri yapın:

    * **deploymentScope**: Seçeneklerden dağıtım kapsamını `Management Group`seçin: , `Subscription` ve `Resource Group`. Bu öğreticide **Kaynak Grubu'ndan** yararlanın. Kapsamlar hakkında daha fazla bilgi edinmek için [Dağıtım kapsamlarına](deploy-rest.md#deployment-scope)bakın.
    * **ConnectedServiceName**: Daha önce oluşturduğunuz hizmet bağlantı adını belirtin.
    * **Abonelik Adı**: Hedef abonelik kimliğini belirtin.
    * **eylem**: **Kaynak Grubu Oluştur veya Güncelleştir** eylemi 2 eylem yapar - 1. yeni bir kaynak grubu adı sağlanırsa bir kaynak grubu oluşturun; 2. belirtilen şablonu dağıtın.
    * **resourceGroupName**: yeni bir kaynak grubu adı belirtin. Örneğin, **AzureRmPipeline-rg**.
    * **location**: kaynak grubunun konumunu belirtin.
    * **templateLocation**: **Bağlantılı yapı** belirtildiğinde, görev şablon dosyasını doğrudan bağlı depodan arar.
    * **csmFile** şablon dosyasına giden yoldur. Şablonda tanımlanan parametrelerin tümü varsayılan değerlere sahip olduğundan şablon parametreleri dosyası belirtmeniz gerekmez.

    Görev hakkında daha fazla bilgi için [Azure Kaynak Grubu Dağıtım görevi](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment)ve Azure Kaynak Yöneticisi şablon dağıtım [görevine](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md) bakın
1. **Kaydet ve çalıştır**’ı seçin.
1. Kaydet ve yeniden **çalıştır'ı** seçin. YAML dosyasının bir kopyası bağlı depoya kaydedilir. YAML dosyasını deponuza göz atarak görebilirsiniz.
1. Ardışık hattın başarıyla yürütüldünden doğrulayın.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Boru Hatları yaml](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-status.png)

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kaynak grubunu açın. Ad, boru hattı YAML dosyasında belirttiğiniz addır.  Bir depolama hesabı nın oluşturulduğunu göreceksiniz.  Depolama hesabı adı **depo**ile başlar.
1. Açmak için depolama hesabı adını seçin.
1. **Özellikleri**seçin. **Çoğaltma** yerel **olarak yedekli depolama (LRS)** olduğuna dikkat edin.

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Pipelines portal doğrulama](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-portal-verification.png)

## <a name="update-and-redeploy"></a>Güncelleştirme ve yeniden dağıtma

Şablonu güncelleştirdiğinizde ve değişiklikleri uzak depoya itdiğinizde, ardışık işlem bu durumda depolama hesabı olan kaynakları otomatik olarak güncelleştirir.

1. Visual Studio Code'daki yerel deponuzdan **azuredeploy.json'u** açın.
1. **depolamaHesap Türü** **varsayılan Değerini** **Standard_GRS**güncelleştirin. Aşağıdaki ekran görüntüsüne bakın:

    ![Azure Kaynak Yöneticisi Azure DevOps Azure Pipelines yaml güncelleştirme](./media/template-tutorial-use-azure-pipelines/azure-resource-manager-devops-pipelines-update-yml.png)

1. Değişiklikleri kaydedin.
1. Git Bash/Shell'in aşağıdaki komutlarını çalıştırarak değişiklikleri uzak depoya itin.

    ```bash
    git pull origin master
    git add .
    git commit -m “Add a new create storage account template.”
    git push origin master
    ```

    İlk komut, yerel depoyu uzak depoyla eşitler. Boru hattı YAML dosyasının uzak depoya eklenmiştir unutmayın.

    Uzak deponun ana dalı güncelleştirilmesiyle, boru hattı yeniden ateşlenir.

Değişiklikleri doğrulamak için depolama hesabının Çoğaltma özelliğini denetleyebilirsiniz.  Bkz. [Dağıtımı doğrula.](#verify-the-deployment)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portalından sol menüden **Kaynak grubunu** seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üst menüden **kaynak grubunu sil'i** seçin.

GitHub deposunu ve Azure DevOps projesini de silmek isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Azure Kaynak Yöneticisi şablonu dağıtmak için bir Azure DevOps ardışık hattı oluşturursunuz. Azure kaynaklarını birden fazla bölgede dağıtma ve güvenli dağıtım uygulamalarını kullanma hakkında bilgi edinmek için bkz.

> [!div class="nextstepaction"]
> [Güvenli dağıtım uygulamalarını kullanma](./deployment-manager-tutorial.md)
