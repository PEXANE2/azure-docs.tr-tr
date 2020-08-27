---
title: GitHub eylemlerinden özel kapsayıcı CI/CD 'si
description: Özel Linux kapsayıcınızı bir CI/CD işlem hattından App Service dağıtmak için GitHub eylemlerini nasıl kullanacağınızı öğrenin.
ms.devlang: na
ms.topic: article
ms.date: 10/25/2019
ms.author: jafreebe
ms.reviewer: ushan
ms.openlocfilehash: 6af23aba28ce3cda9982878ed08ec515aa25633a
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962613"
---
# <a name="deploy-a-custom-container-to-app-service-using-github-actions"></a>GitHub eylemlerini kullanarak App Service özel kapsayıcı dağıtma

[GitHub eylemleri](https://help.github.com/en/articles/about-github-actions) size otomatik yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. [Kapsayıcılar için Azure App Service eylemiyle](https://github.com/Azure/webapps-container-deploy), GitHub eylemlerini kullanarak [App Service](overview.md) özel kapsayıcılar dağıtmak üzere iş akışınızı otomatikleştirebilir.

> [!IMPORTANT]
> GitHub eylemleri Şu anda beta aşamasındadır. GitHub hesabınızı kullanarak [önizlemeye katmak için önce kaydolmanız](https://github.com/features/actions) gerekir.
> 

Bir iş akışı, deponuzdaki yoldaki bir YAML (. yıml) dosyası tarafından tanımlanır `/.github/workflows/` . Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

Azure App Service kapsayıcı iş akışı için, dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik Doğrulaması** | 1. bir hizmet sorumlusu tanımlayın. <br /> 2. GitHub gizli dizisi oluşturun. |
|**Derleme** | 1. ortamı ayarlayın. <br /> 2. kapsayıcı görüntüsünü oluşturun. |
|**Dağıtma** | 1. kapsayıcı görüntüsünü dağıtın. |

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI](/cli/azure/)'de [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portal [Azure Cloud Shell](https://shell.azure.com/) kullanarak veya **deneyin** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor \
                            --scopes /subscriptions/<subscription-id>/resourceGroups/<group-name> \
                            --sdk-auth
```

Yukarıdaki örnekte yer tutucuları abonelik KIMLIĞINIZ ve kaynak grubu adıyla değiştirin. Çıktı, aşağıda gösterilene benzer App Service uygulamanıza erişim sağlayan rol atama kimlik bilgileri içeren bir JSON nesnesidir. Bu JSON nesnesini daha sonra kopyalayın.

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
> En az erişim sağlamak her zaman iyi bir uygulamadır. Yukarıdaki az CLı komutundaki kapsamı, belirli App Service uygulamasıyla ve kapsayıcı görüntülerinin gönderildiği Azure Container Registry kısıtlayabilirsiniz.

## <a name="configure-the-github-secret"></a>GitHub gizliliğini yapılandırma

[GitHub](https://github.com/)'da deponuza gözatıp **Ayarlar > gizlilikler ' ı seçin > yeni bir gizli dizi ekleyin**.

[Bir hizmet sorumlusu oluştur](#create-a-service-principal) ' dan JSON çıktısının içeriğini gizli değişkeninin değeri olarak yapıştırın. Gizli dizi adını gibi verin `AZURE_CREDENTIALS` .

Daha sonra iş akışı dosyasını yapılandırdığınızda, `creds` Azure oturum açma eyleminin girişi için gizli anahtarı kullanırsınız. Örneğin:

```yaml
- uses: azure/login@v1
  with:
    creds: ${{ secrets.AZURE_CREDENTIALS }}
```

Benzer şekilde, kapsayıcı kayıt defteri kimlik bilgileri için aşağıdaki ek gizli dizileri tanımlayın ve bunları Docker oturum açma eyleminde ayarlayın.

- REGISTRY_USERNAME
- REGISTRY_PASSWORD

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

## <a name="deploy-to-an-app-service-container"></a>App Service kapsayıcısına dağıtma

Görüntünüzü App Service özel bir kapsayıcıya dağıtmak için `azure/webapps-container-deploy@v1` eylemini kullanın. Bu eylem beş parametreye sahiptir:

| **Parametre**  | **Açıklama**  |
|---------|---------|
| **uygulama adı** | Istenir App Service uygulamasının adı | 
| **yuva adı** | Seçim Üretim yuvası dışında mevcut bir yuva girin |
| **yansımasını** | Istenir Tam kapsayıcı görüntü adını belirtin. Örneğin, ' myregistry.azurecr.io/nginx:latest ' veya ' Python: 3.7.2-alçam/'. Çok kapsayıcılı bir uygulama için birden çok kapsayıcı görüntüsü adı sağlanmış olabilir (çok satırlı ayrılmış) |
| **yapılandırma-dosya** | Seçim Docker-Compose dosyasının yolu. Tam olarak nitelenmiş bir yol olmalıdır veya varsayılan çalışma dizinine göre değişir. Çok Kapsayıcılı uygulamalar için gereklidir. |
| **kapsayıcı-komut** | Seçim Başlangıç komutunu girin. For ex. DotNet Run veya DotNet filename.dll |

Aşağıda, App Service içindeki özel bir kapsayıcıya Node.js uygulaması derlemek ve dağıtmak için örnek iş akışı verilmiştir. `creds`Girişin `AZURE_CREDENTIALS` daha önce oluşturduğunuz gizli dizi ile nasıl başvurdığına göz önünde unutmayın.

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

GitHub 'daki farklı depolarda gruplanmış eylem listemizi, her biri, CI/CD için GitHub kullanmanıza ve uygulamalarınızı Azure 'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren bir şekilde bulabilirsiniz.

- [Azure 'a dağıtılacak eylemler iş akışları](https://github.com/Azure/actions-workflow-samples)

- [Azure oturum açma](https://github.com/Azure/login)

- [Azure WebApp](https://github.com/Azure/webapps-deploy)

- [Kapsayıcılar için Azure WebApp](https://github.com/Azure/webapps-container-deploy)

- [Docker oturum açma/kapatma](https://github.com/Azure/docker-login)

- [İş akışlarını tetikleyen olaylar](https://help.github.com/en/articles/events-that-trigger-workflows)

- [K8s dağıtımı](https://github.com/Azure/k8s-deploy)

- [Başlangıç Iş akışları](https://github.com/actions/starter-workflows)