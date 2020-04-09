---
title: Azure İşlevleri'nde kod güncelleştirmeleri yapmak için GitHub Eylemleri'ni kullanın
description: GitHub'da Azure İşlevleri projeleri oluşturmak ve dağıtmak için bir iş akışı tanımlamak için GitHub Eylemleri'ni nasıl kullanacağınızı öğrenin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/16/2019
ms.author: cshoe
ms.openlocfilehash: 54010269e5b61ebf28a29dd3165c4310f3472817
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878213"
---
# <a name="continuous-delivery-by-using-github-action"></a>GitHub Action kullanarak sürekli teslimat

[GitHub Eylemleri,](https://github.com/features/actions) Azure'da uygulama işlevi için işlev kodunuzu otomatik olarak oluşturmak ve dağıtmak için bir iş akışı tanımlamanıza olanak tanır. 

GitHub Eylemleri'nde [iş akışı,](https://help.github.com/articles/about-github-actions#workflow) GitHub deponuzda tanımladığınız otomatik bir işlemdir. Bu işlem GitHub'a işlevler uygulama projenizi GitHub'da nasıl oluşturup dağıtılayacaklarını söyler. 

İş akışı, deponuzdaki `/.github/workflows/` yoldaki BIR YAML (.yml) dosyası yla tanımlanır. Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir. 

Azure İşlevleri iş akışı için dosyanın üç bölümü vardır: 

| Section | Görevler |
| ------- | ----- |
| **Kimlik Doğrulaması** | <ol><li>Bir hizmet ilkesi tanımlayın.</li><li>Yayımlama profilini indirin.</li><li>Bir GitHub sırrı oluşturun.</li></ol>|
| **Yapı** | <ol><li>Ortamı ayarlayın.</li><li>İşlev uygulamasını oluşturun.</li></ol> |
| **Dağıt** | <ol><li>İşlev uygulamasını dağıtın.</li></ol>|

> [!NOTE]
> Kimlik doğrulaması için yayımlama profilini kullanmaya karar verirseniz, bir hizmet ilkesi oluşturmanız gerekmez.

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI'deki](/cli/azure/)az [reklam sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet ilkesi](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portalında [Azure Bulut Su şutu'nu](https://shell.azure.com) kullanarak veya **Try it** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.Web/sites/<APP_NAME> --sdk-auth
```

Bu örnekte, kaynaktaki yer tutucuları abonelik kimliğiniz, kaynak grubunuz ve işlev uygulama adınız ile değiştirin. Çıktı, işlev uygulamanıza erişim sağlayan rol atama kimlik bilgileridir. GitHub'dan kimlik doğrulaması yapmak için kullanabileceğiniz bu JSON nesnesini kopyalayın.

> [!IMPORTANT]
> Her zaman minimum erişim vermek için iyi bir uygulamadır. Bu nedenle, önceki örnekteki kapsam kaynak grubunun tamamıyla değil, belirli işlev uygulamasıyla sınırlıdır.

## <a name="download-the-publishing-profile"></a>Yayımlama profilini indirin

Uygulamanızın **Genel Bakış** sayfasına giderek ve Yayın **Profilini Al'ı**tıklayarak işlev uygulamanızın yayın profilini indirebilirsiniz.

   ![Yayın profilini indirin](media/functions-how-to-github-actions/get-publish-profile.png)

Dosyanın içeriğini kopyalayın.

## <a name="configure-the-github-secret"></a>GitHub sırrını yapılandırma

1. [GitHub'da,](https://github.com)deponuza göz atın, **Ayarlar** > **Sırları'nı** > seçin**Yeni bir sır ekleyin.**

   ![Gizli Ekle](media/functions-how-to-github-actions/add-secret.png)

1. Yeni bir sır ekle.

   * Azure CLI'yi kullanarak oluşturduğunuz hizmet ilkesini kullanıyorsanız, `AZURE_CREDENTIALS` **Ad**için kullanın. Sonra **Değer**için kopyalanan JSON nesne çıktısını yapıştırın ve **gizli ekle'yi**seçin.
   * Yayımlama profili kullanıyorsanız, `SCM_CREDENTIALS` **Ad**için kullanın. Ardından, yayımlama profilinin dosya içeriğini **Değer**için kullanın ve **gizli ekle'yi**seçin.

GitHub artık Azure'daki işlev uygulamanızın kimliğini doğrulayabilir.

## <a name="set-up-the-environment"></a>Ortamı ayarlama 

Ortamı ayarlama, dile özgü bir yayımlama kurulum eylemi kullanılarak yapılır.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Aşağıdaki örnek, iş akışının ortamı ayarlamak `actions/setup-node` için eylemi kullanan bölümünü gösterir:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```

# <a name="python"></a>[Python](#tab/python)

Aşağıdaki örnek, iş akışının ortamı ayarlamak `actions/setup-python` için eylemi kullanan bölümünü gösterir:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

# <a name="c"></a>[C#](#tab/csharp)

Aşağıdaki örnek, iş akışının ortamı ayarlamak `actions/setup-dotnet` için eylemi kullanan bölümünü gösterir:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

# <a name="java"></a>[Java](#tab/java)

Aşağıdaki örnek, iş akışının ortamı ayarlamak `actions/setup-java` için eylemi kullanan bölümünü gösterir:

```yaml
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```
---

## <a name="build-the-function-app"></a>İşlev uygulamasını oluşturun

Bu, dile ve Azure İşlevleri tarafından desteklenen dillere bağlıdır, bu bölüm her dilin standart yapı adımları olmalıdır.

Aşağıdaki örnek, iş akışının işlev uygulamasını oluşturan ve dile özgü olan kısmını gösterir:

# <a name="javascript"></a>[JavaScript](#tab/javascript)

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

# <a name="python"></a>[Python](#tab/python)

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

# <a name="c"></a>[C#](#tab/csharp)

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

# <a name="java"></a>[Java](#tab/java)

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
---

## <a name="deploy-the-function-app"></a>İşlev uygulamasını dağıtma

Kodunuzu bir işlev uygulamasına dağıtmak için `Azure/functions-action` eylemi kullanmanız gerekir. Bu eylemin iki parametresi vardır:

|Parametre |Açıklama  |
|---------|---------|
|**_uygulama adı_** | (Zorunlu) İşlev uygulamanızın adı. |
|_**slot adı**_ | (İsteğe bağlı) Dağıtmak istediğiniz [dağıtım yuvasının](functions-deployment-slots.md) adı. Yuva, işlev uygulamanızda zaten tanımlanmış olmalıdır. |


Aşağıdaki örnekte sürüm 1 `functions-action`kullanır:

```yaml
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: PLEASE_REPLACE_THIS_WITH_YOUR_FUNCTION_APP_NAME
```

## <a name="next-steps"></a>Sonraki adımlar

Tam bir iş akışını görüntülemek için .yaml, `functionapp` [Azure GitHub Eylemleri iş akışı örnekleri repo'daki](https://aka.ms/functions-actions-samples) dosyalardan birine bakın. Bu örnekleri iş akışınız için bir başlangıç noktası olarak kullanabilirsiniz.

> [!div class="nextstepaction"]
> [GitHub Eylemleri hakkında daha fazla bilgi edinin](https://help.github.com/en/articles/about-github-actions)
