---
title: GitHub eylemleri ile bir CI/CD işlem hattından kapsayıcınızı dağıtma-Azure App Service | Microsoft Docs
description: App Service kapsayıcınızı dağıtmak için GitHub eylemlerini nasıl kullanacağınızı öğrenin
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
ms.date: 10/09/2019
ms.author: jafreebe
ms.openlocfilehash: 2341eba2c24c06d654c9d2eeda96788d168fe27c
ms.sourcegitcommit: ec2b75b1fc667c4e893686dbd8e119e7c757333a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72809818"
---
# <a name="github-actions-for-deploying-to-web-app-for-containers"></a>Kapsayıcılar için Web App dağıtmaya yönelik GitHub eylemleri

[GitHub eylemleri](https://help.github.com/en/articles/about-github-actions) size otomatik yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. GitHub için Azure App Service eylemlerle, GitHub eylemlerini kullanarak [kapsayıcılar Için Azure Web Apps](https://azure.microsoft.com/services/app-service/containers/) dağıtmak üzere iş akışınızı otomatikleştirebilir.

> [!IMPORTANT]
> GitHub eylemleri Şu anda beta aşamasındadır. GitHub hesabınızı kullanarak [önizlemeye katmak için önce kaydolmanız](https://github.com/features/actions) gerekir.
> 

Bir iş akışı, deponuzdaki `/.github/workflows/` yolundaki bir YAML (. yıml) dosyası tarafından tanımlanır. Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

Bir Azure Web uygulaması kapsayıcısı iş akışı için, dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik doğrulaması** | 1. hizmet sorumlusu tanımlama <br /> 2. GitHub parolası oluşturma |
|**Derlemeyi** | 1. ortamı ayarlama <br /> 2. kapsayıcı görüntüsünü oluşturma |
|**Dağıtma** | 1. kapsayıcı görüntüsünü dağıtma |

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

Aşağıdaki örnekte, Kullanıcı düzeyi kimlik bilgileri (örneğin, dağıtım için Azure hizmet sorumlusu) kullanılmaktadır. Gizli anahtarı yapılandırmak için aşağıdaki adımları izleyin:

1. [GitHub](https://github.com/)'da deponuza gözatıp **Ayarlar > gizlilikler ' ı seçin > yeni bir gizli dizi ekleyin**

2. Aşağıdaki `az cli` komutunun içeriğini gizli değişkenin değeri olarak yapıştırın. Örneğin, `AZURE_CREDENTIALS`.

    
    ```azurecli
    az ad sp create-for-rbac --name "myApp" --role contributor \
                                --scopes /subscriptions/{subscription-id}/resourceGroups/{resource-group} \
                                --sdk-auth
                                
    # Replace {subscription-id}, {resource-group} with the subscription, resource group details
    ```

3. Şimdi dalınızdaki iş akışı dosyasında: `.github/workflows/workflow.yml` Azure oturum açma eyleminde gizli anahtarı gizli diziyle değiştirin.

4. Benzer şekilde, kapsayıcı kayıt defteri kimlik bilgileri için aşağıdaki ek gizli dizileri tanımlayın ve bunları Docker oturum açma eyleminde ayarlayın. 

    - REGISTRY_USERNAME
    - REGISTRY_PASSWORD

5. Gizli dizileri, tanımlandıktan sonra aşağıda gösterildiği gibi göreceksiniz.

    ![kapsayıcı gizli dizileri](../media/app-service-github-actions/app-service-secrets-container.png)

## <a name="build-the-container-image"></a>Kapsayıcı görüntüsünü oluşturma

Aşağıdaki örnek, Docker görüntüsünü oluşturan iş akışının bir parçasını gösterir.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/actions/login@v1
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

## <a name="deploy-to-web-app-container"></a>Web uygulaması kapsayıcısına dağıtma

Görüntünüzü bir Web uygulaması kapsayıcısına dağıtmak için `Azure/appservice-actions/webapp@master` eylemini kullanmanız gerekir. Bu eylem 5 parametreye sahiptir:

| **Parametresinin**  | **Açıklama**  |
|---------|---------|
| **uygulama adı** | Istenir Azure Web uygulamasının adı | 
| **yuva adı** | Seçim Üretim yuvası dışında mevcut bir yuva girin |
| **yansımasını** | Istenir Tam kapsayıcı görüntü adını belirtin. Örneğin, ' myregistry.azurecr.io/nginx:latest ' veya ' Python: 3.7.2-alçam/'. Çok Kapsayıcılı senaryo için birden çok kapsayıcı görüntüsü adı sağlanıyor (çok satırlı ayrılmış) |
| **yapılandırma-dosya** | Seçim Docker-Compose dosyasının yolu. Tam olarak nitelenmiş bir yol olmalıdır veya varsayılan çalışma dizinine göre değişir. Çok Kapsayıcılı senaryo için gerekli |
| **kapsayıcı-komut** | Seçim Başlangıç komutunu girin. For ex. DotNet Run veya DotNet filename. dll |

Aşağıda, bir Node. js web uygulamasını derlemek ve Azure Web App Container 'a dağıtmak için örnek iş akışı verilmiştir.

```yaml
on: [push]

name: Linux_Container_Node_Workflow

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    # checkout the repo
    - name: 'Checkout Github Action' 
      uses: actions/checkout@master
    
    - name: 'Login via Azure CLI'
      uses: azure/actions/login@v1
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

GitHub 'daki farklı depolarda gruplanmış eylem listemizi, her biri, CI/CD için GitHub kullanmanıza ve uygulamalarınızı Azure 'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren bir şekilde bulabilirsiniz.

- [Azure oturum açma](https://github.com/Azure/actions)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Kapsayıcılar için Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker oturum açma/kapatma](https://github.com/Azure/docker-login)

- [İş akışlarını tetikleyen olaylar](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8s dağıtımı](https://github.com/Azure/k8s-deploy)

- [Başlangıç Iş akışları](https://github.com/actions/starter-workflows)
