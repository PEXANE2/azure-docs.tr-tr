---
title: GitHub eylemleri ile CI/CD 'yi yapılandırma
description: Bir CI/CD işlem hattından GitHub eylemleriyle Azure App Service kodunuzu dağıtmayı öğrenin. Yapı görevlerini özelleştirin ve karmaşık dağıtımları yürütün.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.custom: devx-track-python
ms.openlocfilehash: 264976fdfe514a8778c60fe9242ac555f268718d
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962579"
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
|**Kimlik Doğrulaması** | 1. bir hizmet sorumlusu tanımlayın. <br /> 2. GitHub gizli dizisi oluşturun. |
|**Derleme** | 1. ortamı ayarlayın. <br /> 2. Web uygulamasını oluşturun. |
|**Dağıtma** | 1. Web uygulamasını dağıtın. |

## <a name="generate-deployment-credentials"></a>Dağıtım kimlik bilgileri oluştur

# <a name="user-level-credentials"></a>[Kullanıcı düzeyi kimlik bilgileri](#tab/userlevel)

[Azure CLI](/cli/azure/)'de [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portal [Azure Cloud Shell](https://shell.azure.com/) kullanarak veya **deneyin** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> \
                            --sdk-auth
```

Yukarıdaki örnekte yer tutucuları abonelik KIMLIĞINIZ, kaynak grubu adı ve uygulama adınızla değiştirin. Çıktı, aşağıda gösterilene benzer App Service uygulamanıza erişim sağlayan rol atama kimlik bilgileri içeren bir JSON nesnesidir. Bu JSON nesnesini daha sonra kopyalayın.

```output 
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```

> [!IMPORTANT]
> En az erişim sağlamak her zaman iyi bir uygulamadır. Önceki örnekteki kapsam, kaynak grubunun tamamı değil, belirli App Service uygulamasıyla sınırlandırılmıştır.

# <a name="app-level-credentials"></a>[Uygulama düzeyi kimlik bilgileri](#tab/applevel)

Uygulamanızın Yayımla profilini kullanarak uygulama düzeyi kimlik bilgilerini kullanabilirsiniz. Portalda uygulamanızın yönetim sayfasına gidin. **Genel bakış** sayfasında, **Yayımlama profili al** seçeneğini tıklayın.

Daha sonra dosyanın içeriğine ihtiyacınız olacak.

---

## <a name="configure-the-github-secret"></a>GitHub gizliliğini yapılandırma

# <a name="user-level-credentials"></a>[Kullanıcı düzeyi kimlik bilgileri](#tab/userlevel)

[GitHub](https://github.com/)'da deponuza gözatıp **Ayarlar > gizlilikler ' ı seçin > yeni bir gizli dizi ekleyin**.

[Kullanıcı düzeyi kimlik bilgilerini](#generate-deployment-credentials)kullanmak IÇIN Azure CLI KOMUTUNDAN tüm JSON çıkışını gizli dizi değeri alanına yapıştırın. Gizli dizi adını gibi verin `AZURE_CREDENTIALS` .

Daha sonra iş akışı dosyasını yapılandırdığınızda, `creds` Azure oturum açma eyleminin girişi için gizli anahtarı kullanırsınız. Örneğin:

```yaml
- uses: azure/login@v1
  with:
    creds: ${{ secrets.AZURE_CREDENTIALS }}
```

# <a name="app-level-credentials"></a>[Uygulama düzeyi kimlik bilgileri](#tab/applevel)

[GitHub](https://github.com/)'da deponuza gözatıp **Ayarlar > gizlilikler ' ı seçin > yeni bir gizli dizi ekleyin**.

[Uygulama düzeyi kimlik bilgilerini](#generate-deployment-credentials)kullanmak için, indirilen yayımlama profili dosyasının içeriğini gizli dizinin değer alanına yapıştırın. Gizli dizi adını gibi verin `azureWebAppPublishProfile` .

Daha sonra iş akışı dosyasını yapılandırdığınızda, `publish-profile` Azure Web uygulaması dağıtma eyleminin girişi için gizli anahtarı kullanın. Örneğin:
    
```yaml
- uses: azure/webapps-deploy@v2
  with:
    publish-profile: ${{ secrets.azureWebAppPublishProfile }}
```

---

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

# <a name="user-level-credentials"></a>[Kullanıcı düzeyi kimlik bilgileri](#tab/userlevel)

Azure hizmet sorumlusu kullanarak Azure 'da Node.js uygulaması derlemek ve dağıtmak için örnek iş akışı aşağıda verilmiştir. `creds`Girişin `AZURE_CREDENTIALS` daha önce oluşturduğunuz gizli dizi ile nasıl başvurdığına göz önünde unutmayın.

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

# <a name="app-level-credentials"></a>[Uygulama düzeyi kimlik bilgileri](#tab/applevel)

Aşağıda, uygulamanın yayımlama profilini kullanarak Azure 'da bir Node.js uygulaması derlemek ve dağıtmak için örnek iş akışı verilmiştir. `publish-profile`Girişin `azureWebAppPublishProfile` daha önce oluşturduğunuz gizli dizi ile nasıl başvurdığına göz önünde unutmayın.

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

---

## <a name="next-steps"></a>Sonraki adımlar

GitHub 'daki farklı depolarda gruplanmış eylem listemizi, her biri, CI/CD için GitHub kullanmanıza ve uygulamalarınızı Azure 'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren bir şekilde bulabilirsiniz.

- [Azure 'a dağıtılacak eylemler iş akışları](https://github.com/Azure/actions-workflow-samples)

- [Azure oturum açma](https://github.com/Azure/login)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Kapsayıcılar için Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker oturum açma/kapatma](https://github.com/Azure/docker-login)

- [İş akışlarını tetikleyen olaylar](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8s dağıtımı](https://github.com/Azure/k8s-deploy)

- [Başlangıç Iş akışları](https://github.com/actions/starter-workflows)