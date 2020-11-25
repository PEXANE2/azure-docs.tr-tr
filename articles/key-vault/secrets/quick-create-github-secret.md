---
title: Hızlı başlangıç-GitHub eylemleri iş akışlarında Azure Key Vault gizli dizileri kullanma
description: GitHub eylemlerinizi kullanarak yazılım geliştirme iş akışlarınızı otomatik hale getirmek için Key Vault gizli dizileri kullanın
author: juliakm
ms.custom: github-actions-azure
ms.author: jukullam
ms.date: 11/24/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 9509f84b14a42180189a529282b5db348deab279
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "95920289"
---
# <a name="use-key-vault-secrets-in-github-actions-workflows"></a>GitHub eylemleri iş akışlarında Key Vault gizli dizileri kullanma

[GitHub eylemlerinizi](https://help.github.com/en/articles/about-github-actions) Key Vault gizli dizileri kullanın ve Azure Anahtar Kasası 'nda parolaları ve diğer gizli dizileri güvenli bir şekilde depolayın. [Key Vault](../general/overview.md)hakkında daha fazla bilgi edinin. 

Key Vault ve GitHub eylemleri sayesinde, merkezi bir gizli dizi yönetim aracının avantajlarına ve GitHub eylemlerinin tüm avantajlarına sahip olursunuz. GitHub eylemleri, yazılım geliştirme iş akışlarınızı otomatikleştirmek için GitHub 'daki bir özellik paketidir. İş akışlarını, kodu depoladığınız ve çekme isteklerinde ve sorunlarda işbirliği yaptığınız aynı yere dağıtabilirsiniz. 


## <a name="prerequisites"></a>Önkoşullar 
- GitHub hesabı. Bir hesabınız yoksa [ücretsiz](https://github.com/join)kaydolun.  
- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- GitHub deposuna bağlanmış bir Azure uygulaması. Bu örnek [Azure App Service için kapsayıcı dağıtma](https://docs.microsoft.com/azure/developer/javascript/tutorial-vscode-docker-node-01)kullanır. 
- Azure Anahtar Kasası.  Azure portal, Azure CLı veya Azure PowerShell kullanarak Azure Key Vault oluşturabilirsiniz.

## <a name="workflow-file-overview"></a>İş akışı dosyasına genel bakış

Bir iş akışı, deponuzdaki yoldaki bir YAML (. yıml) dosyası tarafından tanımlanır `/.github/workflows/` . Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

Dosya GitHub eylemleriyle iki bölümden kimlik doğrulaması yapmak için sahiptir:

|Section  |Görevler  |
|---------|---------|
|**Kimlik Doğrulaması** | 1. bir hizmet sorumlusu tanımlayın. <br /> 2. GitHub gizli dizisi oluşturun. <br /> 3. bir rol ataması ekleyin. |
|**Anahtar Kasası** | 1. Anahtar Kasası eylemini ekleyin. <br /> 2. Anahtar Kasası gizli anahtarı başvurusu. |

## <a name="authentication"></a>Kimlik Doğrulaması

[Azure CLI](/cli/azure/)'de [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac&preserve-view=true) komutuyla bir [hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portal [Azure Cloud Shell](https://shell.azure.com/) veya **deneyin** düğmesini seçerek çalıştırın.

```azurecli-interactive
   az ad sp create-for-rbac --name {myApp} --role contributor --scopes /subscriptions/{subscription-id}/resourceGroups/{MyResourceGroup} --sdk-auth
```

Yukarıdaki örnekte yer tutucuları abonelik KIMLIĞINIZ ve kaynak grubu adıyla değiştirin. Yer tutucusunu `myApp` uygulamanızın adıyla değiştirin. Çıktı, aşağıda gösterilene benzer App Service uygulamanıza erişim sağlayan rol atama kimlik bilgileri içeren bir JSON nesnesidir. Bu JSON nesnesini daha sonra kopyalayın. Yalnızca `clientId` ,, `clientSecret` `subscriptionId` ve değerlerine sahip bölümlere ihtiyacınız olacaktır `tenantId` . 

```output 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

### <a name="configure-the-github-secret"></a>GitHub gizliliğini yapılandırma

Azure kimlik bilgileriniz, kaynak grubunuz ve abonelikleriniz için gizli diziler oluşturun. 

1. [GitHub](https://github.com/)'da deponuza gözatamazsınız.

1. **Yeni gizli > > ayarlar**' ı seçin.

1. Azure CLı komutundan tüm JSON çıkışını gizli dizi değeri alanına yapıştırın. Gizli dizi adını verin `AZURE_CREDENTIALS` .

1. Değerini `clientId` daha sonra kullanmak üzere kopyalayın. 

### <a name="add-a-role-assignment"></a>Rol ataması ekleyin 
 
Anahtar kasanıza ve işlemlerine erişebilmek için Azure Hizmet sorumlusuna erişim vermeniz gerekir `get` `list` . Bunu yapmazsanız, hizmet sorumlusu 'nı kullanamayacaksınız. 

`keyVaultName`Anahtar kasanızın adıyla ve değeri ile değiştirin `clientIdGUID` `clientId` . 

```azurecli-interactive
    az keyvault set-policy -n {keyVaultName} --secret-permissions get list --spn {clientIdGUID}
```

## <a name="use-the-azure-key-vault-action"></a>Azure Anahtar Kasası eylemini kullanma

[Azure Anahtar Kasası eylemiyle](https://github.com/marketplace/actions/azure-key-vault-get-secrets), bir Azure Anahtar Kasası örneğinden bir veya daha fazla gizli dizi getirip onu GitHub eylem iş akışlarınızda kullanabilirsiniz.

Getirilen gizli diziler çıkış olarak ve ayrıca ortam değişkenleri olarak ayarlanır. Değişkenler konsola veya günlüklere yazdırıldığında otomatik olarak maskelenir.

```yaml
    - uses: Azure/get-keyvault-secrets@v1
      with:
        keyvault: "my Vault" # name of key vault in Azure portal
        secrets: 'mySecret'  # comma separated list of secret keys to fetch from key vault 
      id: myGetSecretAction # ID for secrets that you will reference
```

İş akışınızda bir anahtar kasası kullanmak için, her iki Anahtar Kasası eyleminin ve bu eyleme başvurulmasına ihtiyacınız vardır. 

Bu örnekte, Anahtar Kasası adlandırılır `containervault` . Anahtar Kasası eylemi ile ortama iki Anahtar Kasası gizli anahtarı eklenir `containerPassword` `containerUsername` . 

Anahtar Kasası değerlerine daha sonra, bu önek ile Docker oturum açma görevinde başvurulur `steps.myGetSecretAction.outputs` . 

```yaml
name: Example key vault flow

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - uses: actions/checkout@v2
    - uses: Azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - uses: Azure/get-keyvault-secrets@v1
      with: 
        keyvault: "containervault"
        secrets: 'containerPassword, containerUsername'
      id: myGetSecretAction
    - uses: azure/docker-login@v1
      with:
        login-server: myregistry.azurecr.io
        username: ${{ steps.myGetSecretAction.outputs.containerUsername }}
        password: ${{ steps.myGetSecretAction.outputs.containerPassword }}
    - run: |
        docker build . -t myregistry.azurecr.io/myapp:${{ github.sha }}
        docker push myregistry.azurecr.io/myapp:${{ github.sha }}     
    - uses: azure/webapps-deploy@v2
      with:
        app-name: 'myapp'
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
        images: 'myregistry.azurecr.io/myapp:${{ github.sha }}'
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure uygulamanız, GitHub deposu ve Anahtar Kasası artık gerekli olmadığında, uygulama, GitHub deposu ve Anahtar Kasası için kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Key Vault hakkında daha fazla bilgi edinin](../general/overview.md)
