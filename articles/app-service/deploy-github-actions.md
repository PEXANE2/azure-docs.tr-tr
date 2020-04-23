---
title: GITHub Eylemleri ile CI/CD'yi yapılandır
description: GitHub Eylemleri ile bir CI/CD ardışık kaynaktan kodunuzu Azure Uygulama Hizmetine nasıl dağıtılacağize kadar öğrenin. Yapı görevlerini özelleştirin ve karmaşık dağıtımları gerçekleştirin.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: 57ca5b0880d4b027e33bc0d01fc6225eb886029b
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085000"
---
# <a name="deploy-to-app-service-using-github-actions"></a>GitHub Eylemlerini Kullanarak Uygulama Hizmetine Dağıt

[GitHub Eylemleri,](https://help.github.com/en/articles/about-github-actions) otomatik bir yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. GitHub için Azure Uygulama Hizmeti Eylemleri ile, GitHub Eylemleri'ni kullanarak Iş Akışınızı [Azure Uygulama Hizmeti'ne](overview.md) dağıtmak için otomatikleştirebilirsiniz.

> [!IMPORTANT]
> GitHub Eylemleri şu anda beta da. İlk olarak GitHub hesabınızı kullanarak [önizlemeye katılmak için kaydolmalısınız.](https://github.com/features/actions)
> 

İş akışı, deponuzdaki `/.github/workflows/` yoldaki BIR YAML (.yml) dosyası yla tanımlanır. Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

Azure Uygulama Hizmeti iş akışı için dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik Doğrulaması** | 1. Bir hizmet ilkesi tanımlayın <br /> 2. GitHub sırrı oluşturma |
|**Yapı** | 1. Çevreyi ayarlama <br /> 2. Web uygulaması oluşturun |
|**Dağıt** | 1. Web uygulamasını dağıtma |

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI'deki](https://docs.microsoft.com/cli/azure/)az [reklam sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet ilkesi](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portalında [Azure Bulut Su şutu'nu](https://shell.azure.com/) kullanarak veya **Try it** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name>/providers/Microsoft.Web/sites/<app-name> --sdk-auth
```

Bu örnekte, kaynaktaki yer tutucuları abonelik kimliğiniz, kaynak grup adınız ve uygulama adınız ile değiştirin. Çıktı, Uygulama Hizmeti uygulamanıza erişim sağlayan rol atama kimlik bilgileridir. GitHub'dan kimlik doğrulaması yapmak için kullanabileceğiniz bu JSON nesnesini kopyalayın.

> [!NOTE]
> Kimlik doğrulama için yayımlama profilini kullanmaya karar verirseniz, bir hizmet ilkesi oluşturmanız gerekmez.

> [!IMPORTANT]
> Her zaman minimum erişim vermek için iyi bir uygulamadır. Bu nedenle, önceki örnekteki kapsam kaynak grubunun tamamıyla değil, belirli App Service uygulamasıyla sınırlıdır.

## <a name="configure-the-github-secret"></a>GitHub sırrını yapılandırma

Uygulama düzeyinde kimlik bilgilerini, yani dağıtım için profil yayımlamayı da kullanabilirsiniz. Sırrı yapılandırmak için aşağıdaki adımları izleyin:

1. **Profili Al** seçeneğini kullanarak Portal'dan Uygulama Hizmeti uygulamasının yayın profilini indirin.

2. [GitHub'da,](https://github.com/)deponuza göz atın, **Ayarlar > Sırlar > Yeni bir sır ekleyin'i** seçin

    ![Sır -larını](media/app-service-github-actions/secrets.png)

3. İndirilen yayımlama profil dosyasının içeriğini sırrın değer alanına yapıştırın.

4. Şimdi şubenizdeki iş akışı dosyasında: `.github/workflows/workflow.yml` Azure Web `publish-profile` Uygulaması'nı dağıtma eyleminin gizlisini değiştirin.
    
    ```yaml
        - uses: azure/webapps-deploy@v2
          with:
            creds: ${{ secrets.azureWebAppPublishProfile }}
    ```

5. Aşağıda tanımlandığı gibi sırrı görürsünüz.

    ![Sır -larını](media/app-service-github-actions/app-service-secrets.png)

## <a name="set-up-the-environment"></a>Ortamı ayarlama

Ortamı ayarlama kurulum eylemlerinden biri kullanılarak yapılabilir.

|**Dil**  |**Kurulum Eylemi**  |
|---------|---------|
|**.NET**     | `actions/setup-dotnet` |
|**Java**     | `actions/setup-java` |
|**JavaScript** | `actions/setup-node` |
|**Python**     | `actions/setup-python` |

Aşağıdaki örnekler, iş akışının çeşitli desteklenen diller için ortamı oluşturan bölümünü gösterir:

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

## <a name="build-the-web-app"></a>Web uygulamasını oluşturun

Bu, dile ve Azure Uygulama Hizmeti tarafından desteklenen dillere bağlıdır, bu bölüm her dilin standart yapı adımları olmalıdır.

Aşağıdaki örnekler, web uygulamasını oluşturan iş akışının çeşitli desteklenen dillerdeki bölümünü gösterir.

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

Kodunuzu bir Uygulama Hizmeti uygulamasına `azure/webapps-deploy@v2` dağıtmak için eylemi kullanın. Bu eylemin dört parametresi vardır:

| **Parametre**  | **Açıklama**  |
|---------|---------|
| **uygulama adı** | (Gerekli) Uygulama Hizmeti uygulamasının adı | 
| **yayımlama-profili** | (İsteğe bağlı) Profil dosyası içeriğini Web Dağıtımı sırlarıyla yayımlama |
| **Paket** | (İsteğe bağlı) Pakete veya klasöre giden yol. *.zip, *.war, *.jar veya dağıtmak için bir klasör |
| **slot adı** | (İsteğe bağlı) Üretim yuvası dışında varolan bir Yuva girin |

### <a name="deploy-using-publish-profile"></a>Yayımlama Profilini Kullanarak Dağıtma

Aşağıda, yayımlama profilini kullanarak bir Düğüm.js uygulamasını oluşturmak ve Azure'a dağıtmak için örnek iş akışı verilmiştir.

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

### <a name="deploy-using-azure-service-principal"></a>Azure hizmet ilkesini kullanarak dağıtma

Aşağıda, bir Azure hizmet ilkesini kullanarak bir Düğüm.js uygulamasını oluşturmak ve Azure'a dağıtmak için örnek iş akışı verilmiştir.

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

GitHub'da, her biri CI/CD için GitHub'ı kullanmanıza ve uygulamalarınızı Azure'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren farklı depolarda gruplanmış Eylem setimizi bulabilirsiniz.

- [Azure'a dağıtılabilmek için eylemler iş akışı](https://github.com/Azure/actions-workflow-samples)

- [Azure oturum açma](https://github.com/Azure/login)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Kapsayıcılar için Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker giriş/oturum açma](https://github.com/Azure/docker-login)

- [İş akışlarını tetikleyen olaylar](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8'ler dağıtmak](https://github.com/Azure/k8s-deploy)

- [Başlangıç İş Akışları](https://github.com/actions/starter-workflows)
