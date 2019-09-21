---
title: Azure Işlevlerinde kod güncelleştirmeleri yapmak için GitHub eylemlerini kullanma
description: GitHub 'da Azure Işlevleri projelerini derlemek ve dağıtmak için bir iş akışı tanımlamak üzere GitHub eylemlerini nasıl kullanacağınızı öğrenin.
author: ahmedelnably
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: aelnably
ms.openlocfilehash: 2d97e46578775cd0d5612fb8d94680bb5be8080a
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71174779"
---
# <a name="continuous-delivery-by-using-github-action"></a>GitHub eylemini kullanarak sürekli teslim

[GitHub eylemleri](https://github.com/features/actions) , işlevlerinizi otomatik olarak derlemek ve Azure 'daki işlev uygulamasına dağıtmak için bir iş akışı tanımlamanızı sağlar. 

> [!IMPORTANT]  
> GitHub eylemleri Şu anda beta aşamasındadır. GitHub hesabınızı kullanarak [önizlemeye katmak için önce kaydolmanız](https://github.com/features/actions) gerekir.

GitHub eylemlerinde, bir [iş akışı](https://help.github.com/articles/about-github-actions#workflow) GitHub deponuzda tanımladığınız otomatikleştirilmiş bir işlemdir. Bu süreç, GitHub 'da işlevler uygulama projenizi nasıl oluşturup dağıtacağınızı gösterir. 

Bir iş akışı, deponuzdaki `/.github/workflows/` yoldaki bir YAML (. yıml) dosyası tarafından tanımlanır. Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir. 

Azure Işlevleri iş akışı için, dosyanın üç bölümü vardır: 

| `Section` | Görevler |
| ------- | ----- |
| **Kimlik Doğrulaması** | <ol><li>Hizmet sorumlusu tanımlayın.</li><li>GitHub gizli dizisi oluşturun.</li></ol>|  
| **Derleme** | <ol><li>Ortamı ayarlayın.</li><li>İşlev uygulamasını oluşturun.</li></ol> |
| **Dağıt** | <ol><li>İşlev uygulamasını dağıtın.</li></ol>| 

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI](/cli/azure/)'de [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portal [Azure Cloud Shell](https://shell.azure.com) kullanarak veya **deneyin** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.Web/sites/<APP_NAME> --sdk-auth
```

Bu örnekte, kaynak içindeki yer tutucuları abonelik KIMLIĞINIZ, kaynak grubunuz ve işlev uygulaması adıyla değiştirin. Çıktı, işlev uygulamanıza erişim sağlayan rol atama kimlik bilgileridir. GitHub 'dan kimlik doğrulamak için kullanabileceğiniz bu JSON nesnesini kopyalayın.

> [!IMPORTANT]
> En az erişim sağlamak her zaman iyi bir uygulamadır. Bu nedenle, önceki örnekteki kapsamın tüm kaynak grubu değil, belirli işlev uygulamasıyla sınırlı olması neden olur.

## <a name="configure-the-github-secret"></a>GitHub gizliliğini yapılandırma

1. [GitHub](https://github/com)'da deponuza gözatıp **Ayarlar** > **gizli** > dizileri**Yeni bir gizli dizi Ekle**' yi seçin.

    ![Gizli dizi Ekle](media/functions-how-to-github-actions/add-secret.png)

1. Değer `AZURE_CREDENTIALS` için **ad** ve kopyalanmış komut çıktısı için kullanın,ardından **gizli dizi Ekle**' yi seçin. 

GitHub artık Azure 'daki işlev uygulamanıza kimlik doğrulaması yapabilir.

## <a name="set-up-the-environment"></a>Ortamı ayarlama 

Ortamı ayarlamak, yayınlama kurulum eylemlerinden biri kullanılarak yapılabilir.

|Dil | Kurulum eylemi |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**    | `actions/setup-java` |
|**JavaScript**     | `actions/setup-node` |
|**Python**   | `actions/setup-python` |

Aşağıdaki örneklerde, desteklenen çeşitli diller için ortamı ayarlayan iş akışının bölümü gösterilmektedir:

**JavaScript**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```

**Python**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: 'Login via Azure CLI'
      uses: Azure/actions/login@master
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-function-app"></a>İşlev uygulaması oluşturma

Bu, dile ve Azure Işlevleri tarafından desteklenen dillere bağlı olarak, bu bölümün her dilin standart derleme adımları olması gerekir.

Aşağıdaki örneklerde, desteklenen çeşitli dillerde işlev uygulamasını oluşturan iş akışının bölümü gösterilmektedir.:

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for npm in pushd
        pushd .
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

**Python**

```yaml
    - name: 'Run pip'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for pip in pushd
        pushd .
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/python3.6/site-packages"
        popd
```

**.NET**

```yaml
    - name: 'Run dotnet build'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - name: 'Run mvn'
      shell: bash
      run: |
        # If your function app project is not located in your repository's root
        # Please change your directory for maven build in pushd
        pushd . ./POM_ARTIFACT_ID
        mvn clean package
        mvn azure-functions:package
        popd
```

## <a name="deploy-the-function-app"></a>İşlev uygulamasını dağıtma

Kodunuzu bir işlev uygulamasına dağıtmak için `Azure/functions-action` eylemini kullanmanız gerekir. Bu eylemin iki parametresi vardır:

|Parametre |Açıklama  |
|---------|---------|
|**_uygulama adı_** | Girilmesi İşlev uygulamanızın adı. |
|_**yuva adı**_ | Seçim Dağıtmak istediğiniz [dağıtım yuvasının](functions-deployment-slots.md) adı. Yuva, işlev uygulamanızda zaten tanımlanmış olmalıdır. |


Aşağıdaki örnek, öğesinin 1. `functions-action`sürümünü kullanır:

```yaml
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: PLEASE_REPLACE_THIS_WITH_YOUR_FUNCTION_APP_NAME
```

## <a name="next-steps"></a>Sonraki adımlar

Tüm iş akışı. YAML 'yi görüntülemek için, adında bulunan [Azure GitHub eylemleri iş akışı örnekleri](https://github.com/Azure/actions-workflow-samples) `functionapp` deposunda bulunan dosyalardan birine bakın. Bu örnekleri, iş akışınız için bir başlangıç noktası olarak kullanabilirsiniz.

> [!div class="nextstepaction"]
> [GitHub eylemleri hakkında daha fazla bilgi edinin](https://help.github.com/en/articles/about-github-actions)
