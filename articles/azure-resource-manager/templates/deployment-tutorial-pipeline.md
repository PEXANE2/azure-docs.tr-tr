---
title: Azure Pipelines ile sürekli tümleştirme
description: Azure Resource Manager şablonlarını sürekli olarak derlemeyi, test etmek ve dağıtmak hakkında bilgi edinin.
ms.date: 04/22/2020
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: d1c56ce913a1b63bab90f5dd5aaada382abbf493
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82084337"
---
# <a name="tutorial-continuous-integration-of-azure-resource-manager-templates-with-azure-pipelines"></a>Öğretici: Azure Pipelines Azure Resource Manager şablonlarının sürekli tümleştirilmesi

[Önceki öğreticide](./deployment-tutorial-linked-template.md), bağlantılı bir şablon dağıtırsınız.  Bu öğreticide, Azure Resource Manager şablon projelerini sürekli olarak derlemek ve dağıtmak için Azure Pipelines kullanmayı öğreneceksiniz.

Azure DevOps, işleri planlamak, kod geliştirmeyle işbirliği yapmak ve uygulama derlemek ve dağıtmak için ekipleri desteklemek üzere Geliştirici Hizmetleri sağlar. Geliştiriciler Azure DevOps Services kullanarak bulutta çalışabilir. Azure DevOps, Web tarayıcınız veya IDE istemciniz aracılığıyla erişebileceğiniz tümleşik bir özellikler kümesi sağlar. Azure işlem hattı bu özelliklerden biridir. Azure Pipelines, tam özellikli bir sürekli tümleştirme (CI) ve sürekli teslim (CD) hizmetidir. Tercih ettiğiniz git sağlayıcınız ile birlikte çalışarak, en büyük bulut hizmetlerine dağıtabilirsiniz. Daha sonra Microsoft Azure, Google Cloud Platform veya Amazon Web Services için kodunuzun derleme, test etme ve dağıtımını otomatik hale getirebilirsiniz.

> [!NOTE]
> Bir proje adı seçin. Öğreticiden gittiğinizde, **Azurermpipeline** herhangi birini proje adınızla değiştirin.
> Bu proje adı, kaynak adları oluşturmak için kullanılır.  Kaynak bir depolama hesabıdır. Depolama hesabı adları 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakam ve küçük harf kullanılmalıdır. Ad benzersiz olmalıdır. Şablonda, depolama hesabı adı "depola" eklenmiş proje adı ve proje adı 3 ila 11 karakter arasında olmalıdır. Bu nedenle, proje adı depolama hesabı adı gereksinimlerini karşılamalıdır ve 11 ' den az karakter içermelidir.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * GitHub deposu hazırlama
> * Azure DevOps projesi oluşturma
> * Azure işlem hattı oluşturma
> * İşlem hattı dağıtımını doğrulama
> * Şablonu güncelleştirme ve yeniden dağıtma
> * Kaynakları temizleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi tamamlamak için gerekenler:

* Şablonunuz için bir depo oluşturmak üzere kullandığınız **bir GitHub hesabı**. Bir tane yoksa, [ücretsiz olarak bir tane oluşturabilirsiniz](https://github.com). GitHub depoları kullanma hakkında daha fazla bilgi için bkz. [GitHub depoları oluşturma](/azure/devops/pipelines/repos/github).
* **Git 'ı yükler**. Bu öğretici yönergesi, *Git Bash* veya *Git kabuğu*kullanır. Yönergeler için bkz. [Git 'ı Install]( https://www.atlassian.com/git/tutorials/install-git).
* **Azure DevOps organizasyonu**. Bir tane yoksa, ücretsiz olarak bir tane oluşturabilirsiniz. Bkz. [kuruluş veya proje koleksiyonu oluşturma]( https://docs.microsoft.com/azure/devops/organizations/accounts/create-organization?view=azure-devops).
* seçim **Kaynak Yöneticisi Araçları uzantısı ile Visual Studio Code**. Bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](use-vs-code-to-create-template.md).

## <a name="prepare-a-github-repository"></a>GitHub deposu hazırlama

GitHub, Kaynak Yöneticisi şablonları dahil olmak üzere proje kaynak kodunuzu depolamak için kullanılır. Desteklenen diğer depolar için bkz. [Azure DevOps tarafından desteklenen depolar](/azure/devops/pipelines/repos/?view=azure-devops).

### <a name="create-a-github-repository"></a>GitHub deposu oluşturma

GitHub hesabınız yoksa bkz. [Önkoşullar](#prerequisites).

1. [GitHub](https://github.com)'da oturum açın.
1. Sağ üst köşedeki hesap görüntünüzü seçin ve ardından **depolarınızı**seçin.

    ![Azure Resource Manager Azure DevOps Azure Pipelines GitHub deposu oluşturma](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-github-repository.png)

1. Yeşil düğme olan **Yeni**' yi seçin.
1. **Depo adı**alanına bir depo adı girin.  Örneğin, **Azurermpipeline-depo**. Herhangi bir **Azurermpipeline** öğesini proje adınızla değiştirmeyi unutmayın. Bu öğreticiden gezinmek için **genel** veya **özel** ' i seçebilirsiniz. Sonra **Depo oluştur**' u seçin.
1. URL 'YI yazın. Depo URL 'si şu biçimdedir- ** https://github.com/[youraccountname]/[yourhavuzadı]**.

Bu depo, *uzak depo*olarak adlandırılır. Aynı projenin geliştiricilerin her biri kendi *yerel deposunu*kopyalayabilir ve değişiklikleri uzak depoda birleştirebilirler.

### <a name="clone-the-remote-repository"></a>Uzak depoyu Kopyala

1. Git kabuğu veya git Bash 'i açın.  [Ön koşullara](#prerequisites) bakın.
1. Geçerli klasörünüzün **GitHub**olduğunu doğrulayın.
1. Şu komutu çalıştırın:

    ```bash
    git clone https://github.com/[YourAccountName]/[YourGitHubRepositoryName]
    cd [YourGitHubRepositoryName]
    mkdir CreateWebApp
    cd CreateWebApp
    pwd
    ```

    **[Youraccountname]** öğesini GitHub hesabınızın adıyla değiştirin ve **[Yourgithubdeptoryname]** öğesini önceki yordamda oluşturduğunuz depo adınızla değiştirin.

**Createwebapp** klasörü, şablonun depolandığı klasördür. **PWD** komutu klasör yolunu gösterir. Yol, aşağıdaki yordamda şablonu kaydettiğiniz yerdir.

### <a name="download-a-quickstart-template"></a>Hızlı başlangıç şablonu indirme

Şablonları oluşturmak yerine şablonları indirebilir ve **Createwebapp** klasörüne kaydedebilirsiniz.

* Ana şablon:https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/azuredeploy.json
* Bağlantılı şablon:https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/get-started-deployment/linked-template/linkedStorageAccount.json

Hem klasör adı hem de dosya adları ardışık düzende oldukları için kullanılır.  Bu adları değiştirirseniz, işlem hattında kullanılan adları güncelleştirmeniz gerekir.

### <a name="push-the-template-to-the-remote-repository"></a>Şablonu uzak depoya gönder

Azuredeploy. JSON yerel depoya eklendi. Sonra, şablonu uzak depoya yüklersiniz.

1. Açık değilse *Git kabuğu* veya *Git Bash*'i açın.
1. Dizini yerel deponuzdaki CreateWebApp klasörü ile değiştirin.
1. **Azuredeploy. JSON** dosyasının klasörde olduğunu doğrulayın.
1. Şu komutu çalıştırın:

    ```bash
    git add .
    git commit -m "Add web app templates."
    git push origin master
    ```

    LF hakkında bir uyarı alabilirsiniz. Uyarıyı yoksayabilirsiniz. **ana** dal ana daldır.  Genellikle her güncelleştirme için bir dal oluşturursunuz. Öğreticiyi basitleştirmek için, ana dalı doğrudan kullanırsınız.
1. Bir tarayıcıdan GitHub deponuza gidin.  URL ** https://github.com/[youraccountname]/[yourgithubrepository]**. **Createwebapp** klasörünü ve klasör içinde üç dosyayı görürsünüz.
1. Şablonu açmak için **Linkedstorageaccount. JSON** öğesini seçin.
1. **Ham** düğmesini seçin. URL, **RAW.githubusercontent.com**ile başlatılır.
1. URL’nin kopyasını oluşturun.  İşlem hattını öğreticide daha sonra yapılandırırken bu değeri sağlamanız gerekir.

Şimdiye kadar, bir GitHub deposu oluşturdunuz ve şablonları depoya yüklediniz.

## <a name="create-a-devops-project"></a>DevOps projesi oluşturma

Sonraki yordama geçebilmeniz için bir DevOps organizasyonu gerekir.  Bir tane yoksa, bkz. [Önkoşullar](#prerequisites).

1. [Azure DevOps](https://dev.azure.com)' da oturum açın.
1. Soldan bir DevOps organizasyonu seçin.

    ![Azure DevOps Azure Resource Manager Azure Pipelines Azure DevOps projesi oluşturma](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-create-devops-project.png)

1. **New project** (Yeni proje) öğesini seçin. Herhangi bir projeniz yoksa, proje oluştur sayfası otomatik olarak açılır.
1. Aşağıdaki değerleri girin:

    * **Proje adı**: bir proje adı girin. Öğreticinin en başından itibaren seçtiğiniz proje adını kullanabilirsiniz.
    * **Sürüm denetimi**: **Git**' i seçin. **Sürüm denetimini**görmek için **Gelişmiş** ' i genişletmeniz gerekebilir.

    Diğer özellikler için varsayılan değeri kullanın.
1. **Oluştur**’u seçin.

Azure 'a projeler dağıtmak için kullanılan bir hizmet bağlantısı oluşturun.

1. Sol menünün altından **proje ayarları** ' nı seçin.
1. İşlem **hatları**altında **hizmet bağlantıları** ' nı seçin.
1. **Yeni hizmet bağlantısı**' nı seçin, **Azure Resource Manager**öğesini seçin ve ardından **İleri**' yi seçin.
1. **Hizmet sorumlusu**' nı seçin ve ardından **İleri**' yi seçin.
1. Aşağıdaki değerleri girin:

    * **Kapsam düzeyi**: **abonelik**' ı seçin.
    * **Abonelik**: Aboneliğinizi seçin.
    * **Kaynak grubu**: boş bırakın.
    * **Bağlantı adı**: bir bağlantı adı girin. Örneğin, **Azurermpipeline-Conn**. Bu adı yazın, işlem hattınızı oluştururken adın olması gerekir.
    * **Tüm işlem hatları için erişim Izni verin**. seçildiğinde
1. **Kaydet**’i seçin.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Bu aşamada, aşağıdaki görevleri tamamladınız.  GitHub ve DevOps hakkında bilgi sahibi olduğunuz için önceki bölümleri atlarsanız, devam etmeden önce görevleri tamamlamalısınız.

* Bir GitHub deposu oluşturun ve şablonları depodaki **Createwebapp** klasörüne kaydedin.
* Bir DevOps projesi oluşturun ve bir Azure Resource Manager hizmet bağlantısı oluşturun.

Şablon dağıtmak için bir adımla işlem hattı oluşturmak için:

1. Sol menüden işlem **hatları** ' nı seçin.
1. **Yeni işlem hattı**' nı seçin.
1. **Bağlan** sekmesinden **GitHub**’ı seçin. İstenirse, GitHub kimlik bilgilerinizi girin ve ardından yönergeleri izleyin. Aşağıdaki ekranı görürseniz, **yalnızca depoları Seç**' i seçin ve **& yüklemeyi Onayla**' yı seçmeden önce depolarınızın listede olduğunu doğrulayın.

    ![Azure Resource Manager Azure DevOps Azure Pipelines yalnızca depoları Seç](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-only-select-repositories.png)

1. **Seç** sekmesinden deponuzu seçin.  Varsayılan ad **[Youraccountname]/[Yourgithubdepotoryname]**.
1. **Yapılandır** sekmesinde **Başlatıcı işlem hattı**' nı seçin. İki komut dosyası adımı ile **Azure-Pipelines. i ml** işlem hattı dosyasını gösterir.
1. IML dosyasından iki komut dosyası adımını silin.
1. İmleci adımdan sonraki satıra Taşı **:**.
1. **Görevler** bölmesini açmak için ekranın sağ tarafındaki **yardımcıyı göster** ' i seçin.
1. **ARM şablon dağıtımı**' nı seçin.
1. Aşağıdaki değerleri girin:

    * **Deploymentscope**: **kaynak grubunu**seçin.. Kapsamlar hakkında daha fazla bilgi edinmek için bkz. [dağıtım kapsamları](deploy-rest.md#deployment-scope).
    * **Azure Resource Manager bağlantısı**: daha önce oluşturduğunuz hizmet bağlantı adını seçin.
    * **Abonelik**: hedef abonelik kimliğini belirtin.
    * **Eylem**: **kaynak grubunu oluştur veya Güncelleştir** eylemini seçin 2 eylem-1. Yeni bir kaynak grubu adı sağlanmışsa, bir kaynak grubu oluşturun; iki. belirtilen şablonu dağıtın.
    * **Kaynak grubu**: yeni bir kaynak grubu adı girin. Örneğin, **Azurermpipeline-RG**.
    * **Konum**: kaynak grubu için bir konum seçin, örneğin **Orta ABD**.
    * **Şablon konumu**: **bağlı yapıt**' yi seçin, bu, görevin şablon dosyasını doğrudan bağlı depodan aramasını gösterir.
    * **Şablon**: **createwebapp/azuredeploy. JSON**girin. Klasör adını ve dosya adını değiştirdiyseniz, bu değeri değiştirmeniz gerekir.
    * **Şablon parametreleri**: bu alanı boş bırakın. Parametre değerlerini * * geçersiz kılma şablonu parametrelerinde belirtirsiniz.
    * **OverrideParameters**: ENTER **-ProjectName [enteraprojectname]-linkedtemplateuri [EnterTheLinkedTemplateURL]**. Proje adını ve bağlantılı şablon URL 'sini değiştirin. Bağlı şablon URL 'SI, [GitHub deposu oluşturma](#create-a-github-repository)sonunda yazdığınız şeydir.
    * **Dağıtım modu**: **artımlı**' i seçin.
    * **Dağıtım adı**: **Deploypipelinetemplate**girin. **Dağıtım adını**görebilmeniz için **Gelişmiş** ' i seçin.

    ![Azure Resource Manager Azure DevOps Azure Pipelines adımı](./media/deployment-tutorial-pipeline/resource-manager-template-pipeline-configure.png)

1. **Add (Ekle)** seçeneğini belirleyin.

    Görev hakkında daha fazla bilgi için bkz. [Azure Kaynak grubu dağıtım görevi](/azure/devops/pipelines/tasks/deploy/azure-resource-group-deployment)ve [Azure Resource Manager şablonu Dağıtım görevi](https://github.com/microsoft/azure-pipelines-tasks/blob/master/Tasks/AzureResourceManagerTemplateDeploymentV3/README.md)

    YML dosyası şuna benzer olacaktır:

    ![Azure Resource Manager Azure DevOps Azure Pipelines YAML](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-yml.png)

1. **Kaydet ve çalıştır**’ı seçin.
1. **Kaydet ve Çalıştır** bölmesinden Kaydet ' i **ve yeniden çalıştır** ' ı seçin. YAML dosyasının bir kopyası bağlı depoya kaydedilir. YAML dosyasını deponuza giderek görebilirsiniz.
1. İşlem hattının başarıyla yürütüldüğünü doğrulayın.

    ![Azure Resource Manager Azure DevOps Azure Pipelines YAML](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-status.png)

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Kaynak grubunu açın. Bu ad, ardışık düzen YAML dosyasında belirttiğiniz şeydir.  Oluşturulmuş bir depolama hesabı görürsünüz.  Depolama hesabı adı **Mağaza**ile başlar.
1. Açmak için depolama hesabı adını seçin.
1. **Özellikler**’i seçin. **Çoğaltmanın** **yerel olarak yedekli depolama (LRS)** olduğuna dikkat edin.

## <a name="update-and-redeploy"></a>Güncelleştir ve yeniden Dağıt

Şablonu güncelleştirdiğinizde ve değişiklikleri uzak depoya gönderdiğinizde, işlem hattı bu durumda kaynakları, depolama hesabını otomatik olarak güncelleştirir.

1. Visual Studio Code veya herhangi bir metin düzenleyicisinde yerel deponuzdan **Linkedstorageaccount. JSON** ' u açın.
1. **Storageaccounttype** değerinin **Standard_GRS** **olarak değerini güncelleştirin** . Aşağıdaki ekran görüntüsüne bakın:

    ![Azure Resource Manager Azure DevOps Azure Pipelines güncelleştirme YAML](./media/deployment-tutorial-pipeline/azure-resource-manager-devops-pipelines-update-yml.png)

1. Değişiklikleri kaydedin.
1. Git Bash/Shell ' den aşağıdaki komutları çalıştırarak, değişiklikleri uzak depoya gönderin.

    ```bash
    git pull origin master
    git add .
    git commit -m "Update the storage account type."
    git push origin master
    ```

    İlk komut (çekme), yerel depoyu uzak depoyla eşitler. Ardışık düzen YAML dosyası yalnızca uzak depoya eklendi. Pull komutunun çalıştırılması YAML dosyasının bir kopyasını yerel dala indirir.

    Dördüncü komut (push), düzeltilen linkedStorageAccount. json dosyasını uzak depoya yükler. Uzak deponun ana dalı güncelleştirildiğinden, işlem hattı yeniden tetiklenir.

Değişiklikleri doğrulamak için depolama hesabının çoğaltma özelliğini denetleyebilirsiniz.  Bkz. [dağıtımı doğrulama](#verify-the-deployment).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık Azure kaynakları gerekli değilse, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. **Ada göre filtrele** alanına kaynak grubu adını girin.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.

GitHub deposunu ve Azure DevOps projesini de silmek isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Tebrikler, bu Kaynak Yöneticisi şablonu dağıtım öğreticisini tamamladınız. Geri bildirim bölümünde herhangi bir yorum ve öneri varsa bize bilgi verin. Teşekkür ederiz!
Şablonlar hakkında daha gelişmiş kavramlara geçmek için hazırsınız. Sonraki öğreticide, dağıtılacak kaynakları tanımlamaya yardımcı olması için şablon başvuru belgelerinin kullanılmasıyla ilgili daha fazla ayrıntıya gidersiniz.

> [!div class="nextstepaction"]
> [Şablon başvurusunu kullanma](./template-tutorial-use-template-reference.md)
