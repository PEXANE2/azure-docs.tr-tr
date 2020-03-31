---
title: GitHub Eylemlerini kullanarak kapsayıcıları Azure Kubernetes Hizmetine oluşturun, test edin ve dağıtın
description: Kapsayıcınızı Kubernetes'e dağıtmak için GitHub Eylemleri'ni nasıl kullanacağınızı öğrenin
services: container-service
author: azooinmyluggage
ms.topic: article
ms.date: 11/04/2019
ms.author: atulmal
ms.openlocfilehash: 5ee8ee4d2c9e225d82e58daffeef9e5f09e43e6b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595374"
---
# <a name="github-actions-for-deploying-to-kubernetes-service"></a>Kubernetes hizmetine dağıtım için GitHub Eylemleri

[GitHub Eylemleri,](https://help.github.com/en/articles/about-github-actions) otomatik bir yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. Kubernetes eylemi, [azure/aks-set-context@v1](https://github.com/Azure/aks-set-context) Azure Kubernetes Hizmet kümelerine dağıtımları kolaylaştırır. Eylem, [azure/k8s-deploy,](https://github.com/Azure/k8s-deploy/tree/master) [azure/k8s-create-secret](https://github.com/Azure/k8s-create-secret/tree/master) vb. gibi diğer eylemler tarafından kullanılabilecek hedef AKS küme bağlamını ayarlar veya kubectl komutlarını çalıştırın.

İş akışı, deponuzdaki `/.github/workflows/` yoldaki BIR YAML (.yml) dosyası yla tanımlanır. Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

AKS'yi hedefleyen bir iş akışı için dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik doğrulaması** | Özel bir konteyner kayıt defterine (ACR) giriş |
|**Oluşturma** | Yapı & kapsayıcı görüntüsünü itin  |
|**Dağıt** | 1. Hedef AKS kümesini ayarlama |
| |2. Kubernetes kümesinde genel/docker-kayıt defteri sırrı oluşturma  |
||3. Kubernetes kümesine dağıl|

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI'deki](https://docs.microsoft.com/cli/azure/)az [reklam sp create-for-rbac](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet ilkesi](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portalında [Azure Bulut Su şutu'nu](https://shell.azure.com/) kullanarak veya **Try it** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```

Yukarıdaki komutta, yer tutucuları abonelik kimliğiniz ve kaynak grubunuzla değiştirin. Çıktı, kaynağınıza erişim sağlayan rol atama kimlik bilgileridir. Komut buna benzer bir JSON nesnesi çıktı gerekir.

```json
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```
GitHub'dan kimlik doğrulaması yapmak için kullanabileceğiniz bu JSON nesnesini kopyalayın.

## <a name="configure-the-github-secrets"></a>GitHub sırlarını yapılandırma

Sırları yapılandırmak için aşağıdaki adımları izleyin:

1. [GitHub'da,](https://github.com/)deponuza göz atın, **Yeni bir sır eklemek > Ayarlar > Sırlar'ı**seçin.

    ![Sır -larını](media/kubernetes-action/secrets.png)

2. Yukarıdaki `az cli` komutun içeriğini gizli değişkenin değeri olarak yapıştırın. Örneğin, `AZURE_CREDENTIALS`.

3. Benzer şekilde, kapsayıcı kayıt defteri kimlik bilgileri için aşağıdaki ek sırları tanımlayın ve Bunları Docker giriş eylemine ayarlayın. 

    - REGISTRY_USERNAME
    - REGISTRY_PASSWORD

4. Aşağıda tanımlandığı gibi sırları göreceksiniz.

    ![kubernetes-sırları](media/kubernetes-action/kubernetes-secrets.png)

##  <a name="build-a-container-image-and-deploy-to-azure-kubernetes-service-cluster"></a>Bir kapsayıcı görüntüsü oluşturun ve Azure Kubernetes Hizmet kümesine dağıtın

Kapsayıcı görüntülerinin oluşturulması ve itme `Azure/docker-login@v1` eylemi kullanılarak yapılır. AKS'ye bir kapsayıcı görüntüsü dağıtmak için `Azure/k8s-deploy@v1` eylemi kullanmanız gerekir. Bu eylemin beş parametresi vardır:

| **Parametre**  | **Açıklama**  |
|---------|---------|
| **ad alanı** | (İsteğe bağlı) Hedef Kubernetes ad alanını seçin. Ad alanı sağlanmazsa, komutlar varsayılan ad alanında çalışır | 
| **Bildirim** |  (Gerekli) Dağıtım için kullanılacak bildirim dosyalarına giden yol |
| **Görüntü** | (İsteğe bağlı) Bildirim dosyalarındaki ikameler için kullanılacak resmin(ler) tam nitelikli kaynak URL'si |
| **imagepullsecrets** | (İsteğe bağlı) Küme içinde zaten ayarlanmış bir docker-registry sırrının adı. Bu gizli adların her biri giriş bildirimi dosyalarında bulunan iş yükleri için imagePullSecrets alanına eklenir |
| **kubectl sürümü** | (İsteğe bağlı) Kubectl ikili belirli bir sürümünü yükler |

### <a name="deploy-to-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesine dağıt

Kapsayıcı görüntüleri oluşturmak ve bir Azure Kubernetes Hizmet kümesine dağıtmak için uçuca iş akışı.

```yaml
on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    
    - uses: Azure/docker-login@v1
      with:
        login-server: contoso.azurecr.io
        username: ${{ secrets.REGISTRY_USERNAME }}
        password: ${{ secrets.REGISTRY_PASSWORD }}
    
    - run: |
        docker build . -t contoso.azurecr.io/k8sdemo:${{ github.sha }}
        docker push contoso.azurecr.io/k8sdemo:${{ github.sha }}
      
    # Set the target AKS cluster.
    - uses: Azure/aks-set-context@v1
      with:
        creds: '${{ secrets.AZURE_CREDENTIALS }}'
        cluster-name: contoso
        resource-group: contoso-rg
        
    - uses: Azure/k8s-create-secret@v1
      with:
        container-registry-url: contoso.azurecr.io
        container-registry-username: ${{ secrets.REGISTRY_USERNAME }}
        container-registry-password: ${{ secrets.REGISTRY_PASSWORD }}
        secret-name: demo-k8s-secret

    - uses: Azure/k8s-deploy@v1
      with:
        manifests: |
          manifests/deployment.yml
          manifests/service.yml
        images: |
          demo.azurecr.io/k8sdemo:${{ github.sha }}
        imagepullsecrets: |
          demo-k8s-secret
```

## <a name="next-steps"></a>Sonraki adımlar

Eylem setimizi, Ci/CD için GitHub'ı kullanmanıza ve uygulamalarınızı Azure'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren, GitHub'daki farklı depolarda bulabilirsiniz.

- [kurulum-kubectl](https://github.com/Azure/setup-kubectl)

- [k8s-set-bağlam](https://github.com/Azure/k8s-set-context)

- [aks-set-bağlam](https://github.com/Azure/aks-set-context)

- [k8s-oluşturma-gizli](https://github.com/Azure/k8s-create-secret)

- [k8s-dağıtmak](https://github.com/Azure/k8s-deploy)

- [webapps-konteyner-dağıtmak](https://github.com/Azure/webapps-container-deploy)

- [eylemler-iş akışı-örnekleri](https://github.com/Azure/actions-workflow-samples)
