---
title: GitHub eylemleri ile bir CI/CD işlem hattından kodunuzu dağıtma-Azure App Service | Microsoft Docs
description: Kodunuzu dağıtmak için GitHub eylemlerini nasıl kullanacağınızı öğrenin App Service
services: app-service
documentationcenter: ''
author: jasonfreeberg
writer: ''
manager: ''
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/22/2019
ms.author: jafreebe
ms.openlocfilehash: b7ec1ae1d04fb1dbe16fd9f4a2640b2b3d9584c2
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809779"
---
# <a name="github-actions-for-deploying-to-app-service"></a>App Service dağıtmaya yönelik GitHub eylemleri

[GitHub eylemleri](https://help.github.com/en/articles/about-github-actions) size otomatik yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. GitHub için Azure App Service eylemlerle, GitHub eylemlerini kullanarak [Azure Web Apps](https://azure.microsoft.com/services/app-service/web/) dağıtmak üzere iş akışınızı otomatikleştirebilir.

> [!IMPORTANT]
> GitHub eylemleri Şu anda beta aşamasındadır. GitHub hesabınızı kullanarak [önizlemeye katmak için önce kaydolmanız](https://github.com/features/actions) gerekir.
> 

Bir iş akışı, deponuzdaki `/.github/workflows/` yolundaki bir YAML (. yıml) dosyası tarafından tanımlanır. Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

Bir Azure Web uygulaması iş akışı için, dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik doğrulaması** | 1. hizmet sorumlusu tanımlama <br /> 2. GitHub parolası oluşturma |
|**Derlemeyi** | 1. ortamı ayarlama <br /> 2. Web uygulamasını oluşturma |
|**Dağıtma** | 1. Web uygulamasını dağıtma |

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI](https://docs.microsoft.com/cli/azure/)'de [az ad SP Create-for-RBAC](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portal [Azure Cloud Shell](https://shell.azure.com/) kullanarak veya **deneyin** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>/providers/Microsoft.Web/sites/<APP_NAME> --sdk-auth
```

Bu örnekte, kaynaktaki yer tutucuları abonelik KIMLIĞINIZ, kaynak grubunuz ve Web uygulaması adıyla değiştirin. Çıktı, Web uygulamanıza erişim sağlayan rol atama kimlik bilgileridir. GitHub 'dan kimlik doğrulamak için kullanabileceğiniz bu JSON nesnesini kopyalayın.

> [!NOTE]
> Kimlik doğrulaması için Yayımlama profili kullanmaya karar verirseniz bir hizmet sorumlusu oluşturmanız gerekmez.

> [!IMPORTANT]
> En az erişim sağlamak her zaman iyi bir uygulamadır. Bu nedenle, önceki örnekteki kapsamın tüm kaynak grubu değil, belirli Web uygulamasıyla sınırlı olması neden olur.

## <a name="configure-the-github-secret"></a>GitHub gizliliğini yapılandırma

Uygulama düzeyi kimlik bilgilerini de kullanabilirsiniz. Örneğin, dağıtım için profili yayımlayın. Gizli anahtarı yapılandırmak için aşağıdaki adımları izleyin:

1. **Yayımla profili al** seçeneğini kullanarak portaldan Web uygulamasına ait yayımlama profilini indirin.

2. [GitHub](https://github.com/)'da deponuza gözatıp **Ayarlar > gizlilikler ' ı seçin > yeni bir gizli dizi ekleyin**

    ![kaynaklanır](media/app-service-github-actions/secrets.png)

3. İndirilen yayımlama profili dosyasının içeriğini gizli alanının değer alanına yapıştırın.

4. Şimdi dalınızdaki iş akışı dosyasında: `.github/workflows/workflow.yml` Azure Web uygulaması dağıtma eyleminin giriş `publish-profile` için parolayı değiştirin.
    
    ```yaml
        - uses: azure/webapps-deploy@v1
          with:
            creds: ${{ secrets.azureWebAppPublishProfile }}
    ```

5. Gizli dizi tanımlandıktan sonra aşağıda gösterildiği gibi görüntülenir.

    ![kaynaklanır](media/app-service-github-actions/app-service-secrets.png)

## <a name="setup-the-environment"></a>Ortamı kurma

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

Bu, dile ve Azure Web Apps tarafından desteklenen dillere bağlıdır. Bu bölüm, her dilin standart derleme adımları olmalıdır.

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
## <a name="deploy-the-web-app"></a>Web uygulamasını dağıtma

Kodunuzu bir Web uygulamasına dağıtmak için `Azure/appservice-actions/webapp@master` eylemini kullanmanız gerekir. Bu eylemin 4 parametresi vardır:

| **Parametresinin**  | **Açıklama**  |
|---------|---------|
| **uygulama adı** | Istenir Azure Web uygulamasının adı | 
| **Yayımlama profili** | Seçim Web Dağıtımı gizli dizileri ile profil dosyası içeriğini yayımlama |
| **leyebilir** | Seçim Paket veya klasörün yolu. *. zip, *. war, *. jar veya dağıtılacak bir klasör |
| **yuva adı** | Seçim Üretim yuvası dışında mevcut bir yuva girin |

### <a name="deploy-using-publish-profile"></a>Yayımlama profili kullanarak dağıtma

Aşağıda, yayımlama profili kullanarak bir Node. js web uygulamasını derlemek ve Azure 'da dağıtmak için örnek iş akışı verilmiştir.

```yaml
# File: .github/workflows/workflow.yml

on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
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
          uses: azure/webapps-deploy@v1
          with: 
            app-name: node-rn
            publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

### <a name="deploy-using-azure-service-principal"></a>Azure hizmet sorumlusu kullanarak dağıtma

Aşağıda, Azure hizmet sorumlusu kullanarak bir Node. js web uygulamasını derlemek ve Azure 'a dağıtmak için örnek iş akışı verilmiştir.

```yaml
on: [push]

name: Node.js

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
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
    - uses: azure/webapps-deploy@v1
      with:
        app-name: 'node-rn'

    # Azure logout 
    - name: logout
      run: |
        az logout
```

## <a name="next-steps"></a>Sonraki adımlar

GitHub 'daki farklı depolarda gruplanmış eylem listemizi, her biri, CI/CD için GitHub kullanmanıza ve uygulamalarınızı Azure 'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren bir şekilde bulabilirsiniz.

- [Azure oturum açma](https://github.com/Azure/actions)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Kapsayıcılar için Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker oturum açma/kapatma](https://github.com/Azure/docker-login)

- [İş akışlarını tetikleyen olaylar](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8s dağıtımı](https://github.com/Azure/k8s-deploy)

- [Başlangıç Iş akışları](https://github.com/actions/starter-workflows)
