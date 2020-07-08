---
title: GitHub eylemlerini kullanarak Kaynak Yöneticisi şablonları dağıtma
description: GitHub eylemleri kullanılarak Azure Resource Manager şablonlarının nasıl dağıtılacağını açıklar.
ms.topic: conceptual
ms.date: 07/02/2020
ms.openlocfilehash: 313354499901bc69ec6e00f0ba7c385065cae615
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85854749"
---
# <a name="deploy-azure-resource-manager-templates-by-using-github-actions"></a>GitHub eylemlerini kullanarak Azure Resource Manager şablonları dağıtma

[GitHub eylemleri](https://help.github.com/en/actions) , Azure Resource Manager (ARM) şablonlarınızın depolandığı GitHub deponuzda doğrudan özel yazılım geliştirme yaşam döngüsü iş akışları oluşturmanıza olanak sağlar. Bir [iş akışı](https://help.github.com/actions/reference/workflow-syntax-for-github-actions) BIR YAML dosyası tarafından tanımlanır. İş akışları, her bir işi tek tek gerçekleştiren bir adım kümesi içeren bir veya daha fazla iş akışı içerir. Adımlar, komutları çalıştırabilir veya bir eylem kullanabilir. Kendi eylemlerinizi oluşturabilir, [GitHub topluluğu](https://github.com/marketplace?type=actions) tarafından paylaşılan eylemleri kullanabilir ve bunları gerektiği şekilde özelleştirebilirsiniz. Bu makalede, Kaynak Yöneticisi şablonlarını dağıtmak için [Azure CLI eyleminin](https://github.com/marketplace/actions/azure-cli-action) nasıl kullanılacağı gösterilmektedir.

Azure CLı eyleminde iki bağımlı eylem vardır:

- **[Kullanıma al](https://github.com/marketplace/actions/checkout)**: iş akışının belirtilen kaynak yöneticisi şablonuna erişebilmesi için deponuza göz atın.
- **[Azure oturum açma](https://github.com/marketplace/actions/azure-login)**: Azure kimlik bilgilerinizle oturum açın

Kaynak Yöneticisi şablonu dağıtmaya yönelik temel bir iş akışında üç adım bulunabilir:

1. Şablon dosyasına göz atın.
2. Azure'da oturum açın.
3. Kaynak Yöneticisi şablonu dağıtma

## <a name="prerequisites"></a>Ön koşullar

Kaynak Yöneticisi şablonlarınızı ve iş akışı dosyalarınızı depolamak için bir GitHub deposuna ihtiyacınız vardır. Bir tane oluşturmak için bkz. [Yeni bir depo oluşturma](https://help.github.com/en/enterprise/2.14/user/articles/creating-a-new-repository).

## <a name="configure-deployment-credentials"></a>Dağıtım kimlik bilgilerini yapılandırma

Azure oturum açma eylemi, Azure 'da kimlik doğrulaması yapmak için bir hizmet sorumlusu kullanır. CI/CD iş akışının sorumlusu, genellikle Azure kaynaklarını dağıtmak için yerleşik katkı sahibine ihtiyaç duyuyor.

Aşağıdaki Azure CLı betiği, bir Azure Kaynak grubunda katkıda bulunan izinlerle Azure hizmet sorumlusu oluşturmayı gösterir. Bu kaynak grubu, iş akışının Kaynak Yöneticisi şablonunuzda tanımlanan kaynakları dağıttığı yerdir.

```azurecli
$projectName="[EnterAProjectName]"
$location="centralus"
$resourceGroupName="${projectName}rg"
$appName="http://${projectName}"
$scope=$(az group create --name $resourceGroupName --location $location --query 'id')
az ad sp create-for-rbac --name $appName --role Contributor --scopes $scope --sdk-auth
```

**$ProjectName** değerini ve betikteki **$Location** özelleştirin. Kaynak grubu adı, **RG** eklenmiş proje adıdır. İş akışınızda kaynak grubu adını belirtmeniz gerekir.

Betik şuna benzer bir JSON nesnesi çıktı:

```json
{
   "clientId": "<GUID>",
   "clientSecret": "<GUID>",
   "subscriptionId": "<GUID>",
   "tenantId": "<GUID>",
   (...)
}
```

JSON çıkışını kopyalayın ve GitHub deponuzdaki bir GitHub gizli dizisi olarak depolayın. Henüz bir deponuz yoksa [Önkoşul](#prerequisites) ' i inceleyin.

1. GitHub deponuzdan **Ayarlar** sekmesini seçin.
1. Sol menüden **gizli** dizileri seçin.
1. Aşağıdaki değerleri girin:

    - **Ad**: AZURE_CREDENTIALS
    - **Değer**: (JSON çıkışını Yapıştır)
1. **Gizli dizi Ekle**' yi seçin.

İş akışında gizli adı belirtmeniz gerekir.

## <a name="add-resource-manager-template"></a>Kaynak Yöneticisi şablonu Ekle

GitHub deposuna bir Kaynak Yöneticisi şablonu ekleyin. Bir tane yoksa, aşağıdaki şablonu kullanabilirsiniz. Şablon bir depolama hesabı oluşturur.

```url
https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Dosyayı depodaki herhangi bir yere koyabilirsiniz. Sonraki bölümde yer alan iş akışı örneği, şablon dosyasının **azuredeploy.js**olarak adlandırıldığını varsayar ve deponuzun kök dizininde **Şablonlar** adlı bir klasörde depolanır.

## <a name="create-workflow"></a>İş akışı oluştur

İş akışı dosyası, deponuzın kökündeki **. GitHub/iş akışları** klasöründe depolanmalıdır. İş akışı dosyası uzantısı **. yıml** ya da **. YAML**olabilir.

Bir iş akışı dosyası oluşturabilir, sonra dosyayı depoya gönderebilir/karşıya yükleyebilir ya da aşağıdaki yordamı kullanabilirsiniz:

1. GitHub deponuzdan üstteki menüden **Eylemler** ' i seçin.
1. **Yeni iş akışı**' nı seçin.
1. **Bir iş akışını kendiniz ayarlayın**öğesini seçin.
1. **Main. yml**dışında farklı bir ad tercih ediyorsanız, iş akışı dosyasını yeniden adlandırın. Örneğin: **Deploystorageaccount. yml**.
1. YML dosyasının içeriğini aşağıdakiler ile değiştirin:

    ```yml
    name: Deploy ARM Template

    on:
      push:
        branches:
          - master
        paths:
          - ".github/workflows/deployStorageAccount.yml"
          - "templates/azuredeploy.json"

    jobs:
      deploy-storage-account-template:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout source code
            uses: actions/checkout@master

          - name: Login to Azure
            uses: azure/login@v1
            with:
              creds: ${{ secrets.AZURE_CREDENTIALS }}


          - name: Deploy ARM Template
            uses: azure/CLI@v1
            with:
              inlineScript: |
                az deployment group create --resource-group myResourceGroup --template-file ./templates/azuredeploy.json
    ```

    İş akışı dosyası üç bölümden oluşur:

    - **ad**: iş akışının adı.
    - **Açık**: iş akışını tetikleyen GitHub olaylarının adı. Ana dalda, belirtilen iki dosyadan en az birini değiştiren bir anında iletme olayı olduğunda iş akışı tetiklenir. İki dosya, iş akışı dosyası ve şablon dosyasıdır.

        > [!IMPORTANT]
        > İki dosyayı ve yollarının sizinkilerle eşleştiğini doğrulayın.
    - **işler**: bir iş akışı çalıştırması bir veya daha fazla işin üzerinde yapılır. **Dağıt-Storage-Account-Template**adlı yalnızca bir iş vardır.  Bu işin üç adımı vardır:

        - **Kaynak kodunu kullanıma alın**.
        - **Azure 'Da oturum açın**.

            > [!IMPORTANT]
            > Gizli adı, deponuza kaydettiğiniz değerle eşleştiğini doğrulayın. Bkz. [dağıtım kimlik bilgilerini yapılandırma](#configure-deployment-credentials).
        - **ARM şablonunu dağıtın**. **Resourcegroupname**değerini değiştirin.  [Dağıtım kimlik bilgilerini yapılandırma](#configure-deployment-credentials)bölümünde Azure CLI betiği kullandıysanız, oluşturulan kaynak grubu adı **RG** eklenmiş olan proje adıdır. **Templatelocation**değerini doğrulayın.

1. **Yürütmeyi Başlat**' ı seçin.
1. **Doğrudan ana dala kaydet**' i seçin.
1. **Yeni dosya Kaydet** ' i (veya **değişiklikleri Yürüt**) seçin.

İş akışı, iş akışı dosyası ya da güncelleştirilmekte olan şablon dosyası tarafından tetiklenecek şekilde yapılandırıldığından, değişiklikleri kaydettikten sonra iş akışı hemen başlar.

## <a name="check-workflow-status"></a>İş akışı durumunu denetle

1. **Eylemler** sekmesini seçin. Listelenen bir **deployStorageAccount. yml** iş akışı görürsünüz. İş akışının yürütülmesi 1-2 dakika sürer.
1. Açmak için iş akışını seçin.
1. Sol menüden **Dağıt-depolama-hesap-şablon** (iş adı) öğesini seçin. İş, iş akışında tanımlanmıştır.
1. Genişletmek için **ARM şablonunu dağıt** (adım adı) seçeneğini belirleyin. REST API yanıtını görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şablon oluşturma sürecinde size kılavuzluk eden adım adım bir öğretici için bkz. [öğretici: Ilk ARM şablonunuzu oluşturma ve dağıtma](template-tutorial-create-first-template.md).
