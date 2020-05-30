---
title: GitHub eylemleri ile CI/CD 'yi yapılandırma
description: Bir CI/CD işlem hattından GitHub eylemleriyle Azure App Service kodunuzu dağıtmayı öğrenin. Yapı görevlerini özelleştirin ve karmaşık dağıtımları yürütün.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: be6b5f0af17aa8343dcb74fd5f0710d44332ce0e
ms.sourcegitcommit: 1f48ad3c83467a6ffac4e23093ef288fea592eb5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84193303"
---
# <a name="deploy-to-app-service-using-github-actions"></a>GitHub eylemlerini kullanarak App Service dağıtma

[GitHub eylemleri](https://help.github.com/en/articles/about-github-actions) size otomatik yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. GitHub için Azure App Service eylemlerle, GitHub eylemlerini kullanarak [Azure App Service](overview.md) dağıtmak üzere iş akışınızı otomatikleştirebilir.

> [!IMPORTANT]
> GitHub eylemleri Şu anda beta aşamasındadır. GitHub hesabınızı kullanarak [önizlemeye katmak için önce kaydolmanız](https://github.com/features/actions) gerekir.
> 

Bir iş akışı, deponuzdaki yoldaki bir YAML (. yıml) dosyası tarafından tanımlanır `/.github/workflows/` . Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

Azure App Service iş akışı için, dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik Doğrulaması** | 1. hizmet sorumlusu tanımlama <br /> 2. GitHub parolası oluşturma |
|**Yapı** | 1. ortamı ayarlama <br /> 2. Web uygulamasını oluşturma |
|**Dağıtma** | 1. Web uygulamasını dağıtma |

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI](https://docs.microsoft.com/cli/azure/)'de [az ad SP Create-for-RBAC](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portal [Azure Cloud Shell](https://shell.azure.com/) kullanarak veya **deneyin** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> --sdk-auth
```

Bu örnekte, kaynaktaki yer tutucuları abonelik KIMLIĞINIZ, kaynak grubu adı ve uygulama adınızla değiştirin. Çıktı, App Service uygulamanıza erişim sağlayan rol atama kimlik bilgileridir. GitHub 'dan kimlik doğrulamak için kullanabileceğiniz bu JSON nesnesini kopyalayın.

> [!NOTE]
> Kimlik doğrulaması için Yayımlama profili kullanmaya karar verirseniz bir hizmet sorumlusu oluşturmanız gerekmez.

> [!IMPORTANT]
> En az erişim sağlamak her zaman iyi bir uygulamadır. Bu nedenle, önceki örnekteki kapsamın tüm kaynak grubu değil, belirli App Service uygulamasıyla sınırlı olması neden olur.

## <a name="configure-the-github-secret"></a>GitHub gizliliğini yapılandırma

Uygulama düzeyi kimlik bilgilerini de kullanabilirsiniz. Örneğin, dağıtım için profili yayımlayın. Gizli anahtarı yapılandırmak için aşağıdaki adımları izleyin:

1. **Yayımla profili al** seçeneğini kullanarak portaldan App Service uygulama için yayımlama profilini indirin.

2. [GitHub](https://github.com/)'da deponuza gözatıp **Ayarlar > gizlilikler ' ı seçin > yeni bir gizli dizi ekleyin**

    ![kaynaklanır](media/app-service-github-actions/secrets.png)

3. İndirilen yayımlama profili dosyasının içeriğini gizli alanının değer alanına yapıştırın.

4. Şimdi dalınızdaki iş akışı dosyasında: `.github/workflows/workflow.yml` `publish-profile` Azure Web uygulaması dağıtma eyleminin girişi için parolayı değiştirin.
    
    ```yaml
        - uses: azure/webapps-deploy@v2
          with:
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
    ```

5. Gizli anahtarı, tanımlandıktan sonra aşağıda gösterildiği gibi görürsünüz.

    ![kaynaklanır](media/app-service-github-actions/app-service-secrets.png)

## <a name="set-up-the-environment"></a>Ortamı ayarlama

Ortamı ayarlamak, kurulum eylemlerinden biri kullanılarak yapılabilir.

|**Dil**  |**Kurulum eylemi**  |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**     | `actions/setup-java` |
|**JavaScript** | `actions/setup-node` |
|**Python**     | `actions/setup-python` |

Aşağıdaki örneklerde, desteklenen çeşitli diller için ortamı ayarlayan iş akışının bölümü gösterilmektedir:

**JavaScript**

```yaml
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
```
**Python**

```yaml
    - name: Setup Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
```

**.NET**

```yaml
    - name: Setup Dotnet 2.2.300
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: '2.2.300'
```

**Java**

```yaml
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

## <a name="build-the-web-app"></a>Web uygulamasını oluşturma

Bu, dile ve Azure App Service tarafından desteklenen dillere bağlıdır. Bu bölüm, her dilin standart derleme adımları olmalıdır.

Aşağıdaki örneklerde, desteklenen çeşitli dillerde Web uygulamasını oluşturan iş akışının bölümü gösterilmektedir.

**JavaScript**

```yaml
    - name: 'Run npm'
      shell: bash
      run: |
        # If your web app project is not located in your repository's root
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
        # If your web app project is not located in your repository's root
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
        # If your web app project is not located in your repository's root
        # Please consider using pushd to change your path
        pushd .
        dotnet build --configuration Release --output ./output
        popd
```

**Java**

```yaml
    - uses: actions/checkout@v1
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    - name: Build with Maven
      run: mvn -B package --file pom.xml
```
## <a name="deploy-to-app-service"></a>App Service’e dağıtma

Kodunuzu bir App Service uygulamasına dağıtmak için `azure/webapps-deploy@v2` eylemini kullanın. Bu eylemin dört parametresi vardır:

| **Parametre**  | **Açıklama**  |
|---------|---------|
| **uygulama adı** | Istenir App Service uygulamasının adı | 
| **Yayımlama profili** | Seçim Web Dağıtımı gizli dizileri ile profil dosyası içeriğini yayımlama |
| **leyebilir** | Seçim Paket veya klasörün yolu. *. zip, *. war, *. jar veya dağıtılacak bir klasör |
| **yuva adı** | Seçim Üretim yuvası dışında mevcut bir yuva girin |

### <a name="deploy-using-publish-profile"></a>Yayımlama profili kullanarak dağıtma

Aşağıda, yayımlama profili kullanarak bir Node. js uygulamasını derlemek ve Azure 'a dağıtmak için örnek iş akışı verilmiştir.

```yaml
# File: .github/workflows/workflow.yml

on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
       
    - name: 'Run Azure webapp deploy action using publish profile credentials'
          uses: azure/webapps-deploy@v2
          with: 
            app-name: node-rn
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

### <a name="deploy-using-azure-service-principal"></a>Azure hizmet sorumlusu kullanarak dağıtma

Bir Azure hizmet sorumlusu kullanarak bir Node. js uygulamasını derlemek ve Azure 'a dağıtmak için örnek iş akışı aşağıda verilmiştir.

```yaml
on: [push]

name: Node.js

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
   
    - uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
        
    - name: Setup Node 10.x
      uses: actions/setup-node@v1
      with:
        node-version: '10.x'
    
    - name: 'npm install, build, and test'
      run: |
        npm install
        npm run build --if-present
        npm run test --if-present
               
    # deploy web app using Azure credentials
    - uses: azure/webapps-deploy@v2
      with:
        app-name: 'node-rn'

    # Azure logout 
    - name: logout
      run: |
        az logout
```

## <a name="next-steps"></a>Sonraki adımlar

GitHub 'daki farklı depolarda gruplanmış eylem listemizi, her biri, CI/CD için GitHub kullanmanıza ve uygulamalarınızı Azure 'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren bir şekilde bulabilirsiniz.

- [Azure 'a dağıtılacak eylemler iş akışı](https://github.com/Azure/actions-workflow-samples)

- [Azure oturum açma](https://github.com/Azure/login)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Kapsayıcılar için Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker oturum açma/kapatma](https://github.com/Azure/docker-login)

- [İş akışlarını tetikleyen olaylar](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8s dağıtımı](https://github.com/Azure/k8s-deploy)

- [Başlangıç Iş akışları](https://github.com/actions/starter-workflows)
