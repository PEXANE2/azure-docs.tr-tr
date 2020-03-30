---
title: GitHub Eylemleri özel konteyner CI / CD
description: Özel Linux kapsayıcınızı Bir CI/CD ardışık kaynaktan Uygulama Hizmetine dağıtmak için GitHub Eylemleri'ni nasıl kullanacağınızı öğrenin.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: d5f175d887cec1d5b5e567d3f716e6492f4516dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78246965"
---
# <a name="deploy-a-custom-container-to-app-service-using-github-actions"></a>GitHub Eylemlerini kullanarak Uygulama Hizmetine özel bir kapsayıcı dağıtma

[GitHub Eylemleri,](https://help.github.com/en/articles/about-github-actions) otomatik bir yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. [Kapsayıcılar için Azure Uygulama Hizmeti Eylemi](https://github.com/Azure/webapps-container-deploy)ile, uygulamaları GitHub Eylemleri'ni kullanarak Uygulama [Hizmetine özel kapsayıcı](https://azure.microsoft.com/services/app-service/containers/) lar olarak dağıtmak için iş akışınızı otomatikleştirebilirsiniz.

> [!IMPORTANT]
> GitHub Eylemleri şu anda beta da. İlk olarak GitHub hesabınızı kullanarak [önizlemeye katılmak için kaydolmalısınız.](https://github.com/features/actions)
> 

İş akışı, deponuzdaki `/.github/workflows/` yoldaki BIR YAML (.yml) dosyası yla tanımlanır. Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

Azure App Service kapsayıcı iş akışı için dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik doğrulaması** | 1. Bir hizmet prensibi tanımlayın. <br /> 2. Bir GitHub sırrı oluşturun. |
|**Oluşturma** | 1. Ortamı ayarlayın. <br /> 2. Kapsayıcı görüntüsünü oluşturun. |
|**Dağıt** | 1. Kapsayıcı görüntüsünü dağıtın. |

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI'deki](https://docs.microsoft.com/cli/azure/)az [reklam sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet ilkesi](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portalında [Azure Bulut Su şutu'nu](https://shell.azure.com/) kullanarak veya **Try it** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                            --sdk-auth
                            
# Replace {subscription-id}, {resource-group} with the subscription, resource group details of the WebApp
```

Çıktı, uygulama hizmeti uygulamanıza aşağıdaki gibi erişim sağlayan rol atama kimlik bilgilerine sahip bir JSON nesnesidir. GitHub'dan kimlik doğrulaması yapmak için bu JSON nesnesini kopyalayın.

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
> Her zaman minimum erişim vermek için iyi bir uygulamadır. Yukarıdaki Az CLI komutundaki kapsamı, konteyner görüntülerinin itildiği belirli App Service uygulaması ve Azure Konteyner Kayıt Defteri ile sınırlandırabilirsiniz.

## <a name="configure-the-github-secret"></a>GitHub sırrını yapılandırma

Aşağıdaki örnekte, dağıtım için Azure Hizmet Sorumlusu gibi kullanıcı düzeyinde kimlik bilgileri kullanınız. Sırrı yapılandırmak için aşağıdaki adımları izleyin:

1. [GitHub'da,](https://github.com/)deponuza göz atın, **Ayarlar > Sırlar > Yeni bir sır ekleyin'i** seçin

2. Aşağıdaki `az cli` komutun içeriğini gizli değişkenin değeri olarak yapıştırın. Örneğin, `AZURE_CREDENTIALS`.

    
    ```azurecli
    az ad sp create-for-rbac --name "myApp" --role contributor \
                                --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                                --sdk-auth
                                
    # Replace {subscription-id}, {resource-group} with the subscription, resource group details
    ```

3. Şimdi şubenizdeki iş akışı dosyasında: `.github/workflows/workflow.yml` Azure giriş eylemindeki sırrı sırrınızla değiştirin.

4. Benzer şekilde, kapsayıcı kayıt defteri kimlik bilgileri için aşağıdaki ek sırları tanımlayın ve Bunları Docker giriş eylemine ayarlayın. 

    - REGISTRY_USERNAME
    - REGISTRY_PASSWORD

5. Aşağıda tanımlandığı gibi sırları görürsünüz.

    ![konteyner sırları](../media/app-service-github-actions/app-service-secrets-container.png)

## <a name="build-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Aşağıdaki örnek, docker görüntüsünü oluşturan iş akışının bir bölümünü gösterir.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - uses: azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
        docker push contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
```

## <a name="deploy-to-an-app-service-container"></a>Bir Uygulama Hizmeti kapsayıcısına dağıtma

Resminizi App Service'deki özel bir kapsayıcıya dağıtmak için `azure/webapps-container-deploy@v1` eylemi kullanın. Bu eylemin beş parametresi vardır:

| **Parametre**  | **Açıklama**  |
|---------|---------|
| **uygulama adı** | (Gerekli) Uygulama Hizmeti uygulamasının adı | 
| **slot adı** | (İsteğe bağlı) Üretim yuvası dışında varolan bir Yuva girin |
| **Görüntü** | (Gerekli) Tam nitelikli kapsayıcı görüntü(ler) adını belirtin. Örneğin, 'myregistry.azurecr.io/nginx:latest' veya 'python:3.7.2-alpine/'. Çok kapsayıcılı bir uygulama için birden çok kapsayıcı resim adı sağlanabilir (çok satırlı ayrılmış) |
| **yapılandırma dosyası** | (İsteğe bağlı) Docker-Compose dosyasının yolu. Tam nitelikli bir yol veya varsayılan çalışma dizini göreli olmalıdır. Çoklu konteyner uygulamaları için gereklidir. |
| **konteyner komutu** | (İsteğe bağlı) Başlangıç komutunu girin. Eski sevgilim için. dotnet çalıştır veya dotnet filename.dll |

Aşağıda, Bir Düğüm.js uygulamasını Oluşturmak ve Uygulama Hizmeti'ndeki özel bir kapsayıcıya dağıtmak için örnek iş akışı verilmiştir.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout GitHub Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    - uses: azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/nodejssampleapp:${{ github.sha }}
        docker push contoso.azurecr.io/nodejssampleapp:${{ github.sha }} 
      
    - uses: azure/webapps-container-deploy@v1
      with:
        app-name: 'node-rnc'
        images: 'contoso.azurecr.io/nodejssampleapp:${{ github.sha }}'
    
    - name: Azure logout
      run: |
        az logout
```

## <a name="next-steps"></a>Sonraki adımlar

GitHub'da, her biri CI/CD için GitHub'ı kullanmanıza ve uygulamalarınızı Azure'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren farklı depolarda gruplanmış Eylem setimizi bulabilirsiniz.

- [Azure oturum açma](https://github.com/Azure/login)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Kapsayıcılar için Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker giriş/oturum açma](https://github.com/Azure/docker-login)

- [İş akışlarını tetikleyen olaylar](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8'ler dağıtmak](https://github.com/Azure/k8s-deploy)

- [Başlangıç CI İş Akışları](https://github.com/actions/starter-workflows)

- [Azure'a dağıtılabilmek için başlangıç iş akışları](https://github.com/Azure/actions-workflow-samples)
