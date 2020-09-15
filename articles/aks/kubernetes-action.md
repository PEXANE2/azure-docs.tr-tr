---
title: GitHub eylemlerini kullanarak kapsayıcıları Azure Kubernetes hizmetine oluşturun, test edin ve dağıtın
description: Kapsayıcısını Kubernetes 'e dağıtmak için GitHub eylemlerini nasıl kullanacağınızı öğrenin
services: container-service
author: azooinmyluggage
ms.topic: article
ms.date: 11/04/2019
ms.author: atulmal
ms.openlocfilehash: 7743a3a8d6e77affd6229b648ab79b5b2f07a0af
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90564109"
---
# <a name="github-actions-for-deploying-to-kubernetes-service"></a>Kubernetes hizmetine dağıtmaya yönelik GitHub eylemleri

[GitHub eylemleri](https://help.github.com/en/articles/about-github-actions) size otomatik yazılım geliştirme yaşam döngüsü iş akışı oluşturma esnekliği sağlar. Kubernetes eylemi, [azure/aks-set-context@v1](https://github.com/Azure/aks-set-context) Azure Kubernetes hizmet kümelerine dağıtımları kolaylaştırır. Eylem, [Azure/k8s-Deploy](https://github.com/Azure/k8s-deploy/tree/master), [Azure/k8s-Create-Secret](https://github.com/Azure/k8s-create-secret/tree/master) vb. gibi diğer eylemler tarafından kullanılabilecek hedef aks kümesi bağlamını ayarlar veya herhangi bir kubectl komutunu çalıştırın.

Bir iş akışı, deponuzdaki yoldaki bir YAML (. yıml) dosyası tarafından tanımlanır `/.github/workflows/` . Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir.

AKS 'i hedefleyen bir iş akışı için, dosyanın üç bölümü vardır:

|Section  |Görevler  |
|---------|---------|
|**Kimlik Doğrulaması** | Özel bir kapsayıcı kayıt defterinde (ACR) oturum açma |
|**Derleme** | Kapsayıcı görüntüsünü oluşturun & gönderin  |
|**Dağıtma** | 1. hedef AKS kümesini ayarlama |
| |2. Kubernetes kümesinde genel/Docker-Registry gizli dizisi oluşturma  |
||3. Kubernetes kümesine dağıtın|

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure CLI](/cli/azure/)'de [az ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) komutunu kullanarak bir [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) oluşturabilirsiniz. Bu komutu Azure portal [Azure Cloud Shell](https://shell.azure.com/) kullanarak veya **deneyin** düğmesini seçerek çalıştırabilirsiniz.

```azurecli-interactive
az ad sp create-for-rbac --name "myApp" --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```

Yukarıdaki komutta yer tutucuları abonelik KIMLIĞINIZ ve kaynak grubuyla değiştirin. Çıktı, kaynağına erişim sağlayan rol atama kimlik bilgileridir. Komut şuna benzer bir JSON nesnesinin çıktısını almalıdır.

```json
  {
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    (...)
  }
```
GitHub 'dan kimlik doğrulamak için kullanabileceğiniz bu JSON nesnesini kopyalayın.

## <a name="configure-the-github-secrets"></a>GitHub gizli dizilerini yapılandırma

Gizli dizileri yapılandırmak için aşağıdaki adımları izleyin:

1. [GitHub](https://github.com/)'da deponuza gidin, **Ayarlar > gizlilikler ' ı seçin > yeni bir gizli dizi ekleyin**.

    ![Ekran görüntüsü, bir depo için yeni bir gizli dizi Ekle bağlantısı gösterir.](media/kubernetes-action/secrets.png)

2. Yukarıdaki `az cli` komutun içeriğini gizli değişkenin değeri olarak yapıştırın. Örneğin, `AZURE_CREDENTIALS`.

3. Benzer şekilde, kapsayıcı kayıt defteri kimlik bilgileri için aşağıdaki ek gizli dizileri tanımlayın ve bunları Docker oturum açma eyleminde ayarlayın. 

    - REGISTRY_USERNAME
    - REGISTRY_PASSWORD

4. Gizli dizileri, tanımlandıktan sonra aşağıda gösterildiği gibi göreceksiniz.

    ![Ekran görüntüsü, bir depo için mevcut gizli dizileri gösterir.](media/kubernetes-action/kubernetes-secrets.png)

##  <a name="build-a-container-image-and-deploy-to-azure-kubernetes-service-cluster"></a>Bir kapsayıcı görüntüsü oluşturun ve Azure Kubernetes hizmet kümesine dağıtın

Kapsayıcı görüntülerinin oluşturulması ve gönderimi eylem kullanılarak yapılır `Azure/docker-login@v1` . AKS 'e bir kapsayıcı görüntüsü dağıtmak için eylemini kullanmanız gerekir `Azure/k8s-deploy@v1` . Bu eylem beş parametreye sahiptir:

| **Parametre**  | **Açıklama**  |
|---------|---------|
| **uzayına** | Seçim Hedef Kubernetes ad alanını seçin. Ad alanı sağlanmazsa, komutlar varsayılan ad alanında çalıştırılır | 
| **listeleri** |  Istenir Dağıtım için kullanılacak olan bildirim dosyalarının yolu |
| **yansımasını** | Seçim Bildirim dosyalarındaki değişimler için kullanılacak görüntünün tam kaynak URL 'SI |
| **ımagepullgizlilikler** | Seçim Küme içinde zaten ayarlanmış olan Docker-Registry parolasının adı. Bu gizli adların her biri, giriş bildirimi dosyalarında bulunan iş yükleri için ımagepullsecret alanı altına eklenir |
| **kubectl-sürüm** | Seçim Belirli bir kubectl ikili sürümünü yüklüyor |

### <a name="deploy-to-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesine dağıtma

Kapsayıcı görüntüleri oluşturmak ve bir Azure Kubernetes hizmet kümesine dağıtmak için uçtan uca iş akışı.

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

GitHub 'daki farklı depolarda, her biri, CI/CD için GitHub ' ı kullanmanıza ve uygulamalarınızı Azure 'a dağıtmanıza yardımcı olacak belgeler ve örnekler içeren eylemler kümesini bulabilirsiniz.

- [Kurulum-kubectl](https://github.com/Azure/setup-kubectl)

- [k8s-set-Context](https://github.com/Azure/k8s-set-context)

- [aks-set-Context](https://github.com/Azure/aks-set-context)

- [k8s-oluştur-gizli](https://github.com/Azure/k8s-create-secret)

- [k8s-dağıt](https://github.com/Azure/k8s-deploy)

- [webapps-Container-Deploy](https://github.com/Azure/webapps-container-deploy)

- [eylemler-iş akışı-örnekler](https://github.com/Azure/actions-workflow-samples)
