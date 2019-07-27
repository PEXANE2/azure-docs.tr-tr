---
title: Kubernetes hizmetini çalıştırma-Metin Analizi
titleSuffix: Azure Cognitive Services
description: Dil algılama kapsayıcısını, çalışan bir örnekle, Azure Kubernetes hizmetine dağıtın ve bir Web tarayıcısında test edin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 7b6621273b04d019791f604ca0db26a73c289f2b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562611"
---
# <a name="deploy-the-language-detection-container-to-azure-kubernetes-service"></a>Dil algılama kapsayıcısını Azure Kubernetes hizmetine dağıtma

Dil algılama kapsayıcısını dağıtmayı öğrenin. Bu yordamda yerel Docker Kapsayıcıları oluşturma, kapsayıcıları kendi özel kapsayıcı Kayıt defterinize gönderme, kapsayıcıyı bir Kubernetes kümesinde çalıştırma ve bir Web tarayıcısında test etme işlemlerinin nasıl yapılacağı gösterilir. Kapsayıcıları kullanmak, dikkat etmeniz gereken altyapıyı, uygulama geliştirmeye odaklanmak yerine altyapıya kadar bir yere kaydırabilirler.

## <a name="prerequisites"></a>Önkoşullar

Bu yordam, yüklenmesi ve yerel olarak çalıştırılması gereken çeşitli araçlar gerektirir. Azure Cloud Shell kullanmayın. Şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* Bu yordamda kullanılan [örneği](https://github.com/Azure-Samples/cognitive-services-containers-samples) kopyalayabilmeniz için işletim sisteminiz için [Git](https://git-scm.com/downloads) .
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Docker altyapısı](https://www.docker.com/products/docker-engine), DOCKER CLI 'nın konsol penceresinde çalışıp çalışmadığını doğrular.
* [kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.13.1/bin/windows/amd64/kubectl.exe).
* Doğru fiyatlandırma katmanına sahip bir Azure kaynağı. Fiyatlandırma katmanlarının tümü bu kapsayıcı ile çalışmaz:
    * Yalnızca F0 veya standart fiyatlandırma katmanlarına sahip **Azure metin analizi** kaynağı.
    * S0 fiyatlandırma katmanı ile Azure bilişsel **Hizmetler** kaynağı.

## <a name="run-the-sample"></a>Örneği çalıştırma

Bu yordam, dil algılama için bilişsel hizmetler kapsayıcı örneğini yükler ve çalıştırır. Örnek, biri istemci uygulaması, diğeri de bilişsel hizmetler kapsayıcısı için olmak üzere iki kapsayıcı içerir. Bu resimlerin her ikisini de Azure Container Registry kendi örneğine göndermeniz gerekir. Kendi kayıt defterinizde olduktan sonra, bu görüntülere erişmek ve kapsayıcıları çalıştırmak için Azure Kubernetes hizmeti 'nin (AKS) bir örneğini oluşturun. Kapsayıcılar çalışırken, kapsayıcıların performansını izlemek için **kubectl** CLI kullanın. İstemci uygulamasına bir HTTP isteğiyle erişin ve sonuçları görüntüleyin.

![Örnek kapsayıcıları çalıştırmanın kavramsal fikri](../media/how-tos/container-instance-sample/containers.png)

## <a name="the-sample-containers"></a>Örnek kapsayıcılar

Örnekte iki kapsayıcı görüntüsü vardır. Bunlardan biri ön uç Web sitesi içindir. İkinci görüntü, metnin algılanan dilini (kültür) döndüren dil algılama kapsayıcısıdır. İşiniz bittiğinde her iki kapsayıcıya de dış IP 'den erişilebilir.

### <a name="the-language-front-end-container"></a>Dil ön uç kapsayıcısı

Bu web sitesi, dil algılama uç noktası isteklerini yapan kendi istemci tarafı uygulamanız ile eşdeğerdir. Yordam tamamlandığında, ile `http://<external-IP>/<text-to-analyze>`bir tarayıcıda Web sitesi kapsayıcısına erişerek bir karakter dizesinin algılanan dilini alırsınız. Bu URL 'ye `http://132.12.23.255/helloworld!`bir örnek. Tarayıcıdaki `English`sonuç.

### <a name="the-language-container"></a>Dil kapsayıcısı

Bu belirli yordamda, dil algılama kapsayıcısına herhangi bir dış istek tarafından erişilebilir. Kapsayıcı hiçbir şekilde değiştirilmez, bu nedenle standart bilişsel hizmetler kapsayıcısına özgü dil algılama API 'SI kullanılabilir.

Bu kapsayıcı için bu API, dil algılama için bir POST isteğidir. Tüm bilişsel hizmetler kapsayıcılarında olduğu gibi, barındırılan Swagger bilgileriyle `http://<external-IP>:5000/swagger/index.html`kapsayıcı hakkında daha fazla bilgi edinebilirsiniz.

Bağlantı noktası 5000, bilişsel hizmetler kapsayıcılarıyla kullanılan varsayılan bağlantı noktasıdır.

## <a name="create-an-azure-container-registry-service"></a>Azure Container Registry hizmeti oluşturma

Kapsayıcıyı Azure Kubernetes hizmetine dağıtmak için kapsayıcı görüntülerinin erişilebilir olması gerekir. Görüntüleri barındırmak için kendi Azure Container Registry hizmetinizi oluşturun.

1. Azure CLı 'da oturum açın.

    ```azurecli
    az login
    ```

1. Bu yordamda oluşturulan her kaynağı `cogserv-container-rg` tutan adlı bir kaynak grubu oluşturun.

    ```azurecli
    az group create --name cogserv-container-rg --location westus
    ```

1. Adınızın ve sonrasında `registry`kendi Azure Container Registry kendi örneğini oluşturun. `pattyregistry` bunun bir örneğidir. Ad içinde tire kullanmayın veya karakterlerin altını çizin.

    ```azurecli
    az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    ```

    **Loginserver** özelliğini almak için sonuçları kaydedin. Bu özellik, daha sonra `language.yml` dosyasında kullanılan barındırılan kapsayıcının adresinin bir parçasıdır.

    ```console
    > az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    {
        "adminUserEnabled": false,
        "creationDate": "2019-01-02T23:49:53.783549+00:00",
        "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/cogserv-container-rg/providers/Microsoft.ContainerRegistry/registries/pattyregistry",
        "location": "westus",
        "loginServer": "pattyregistry.azurecr.io",
        "name": "pattyregistry",
        "provisioningState": "Succeeded",
        "resourceGroup": "cogserv-container-rg",
        "sku": {
            "name": "Basic",
            "tier": "Basic"
        },
        "status": null,
        "storageAccount": null,
        "tags": {},
        "type": "Microsoft.ContainerRegistry/registries"
    }
    ```

1. Kapsayıcı kayıt defterinizde oturum açın. Kayıt defterinize görüntü gönderebilmeniz için önce oturum açmalısınız.

    ```azurecli
    az acr login --name pattyregistry
    ```

## <a name="get-the-website-docker-image"></a>Web sitesi Docker görüntüsünü al

1. Bu yordamda kullanılan örnek kod, bilişsel hizmetler kapsayıcıları örnekleri deposundadır. Depoyu, örneğin yerel kopyasına sahip olacak şekilde kopyalayın.

    ```console
    git clone https://github.com/Azure-Samples/cognitive-services-containers-samples
    ```

    Depo yerel bilgisayarınızda olduktan sonra, [\dotnet\Language\FrontendService](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) dizininde Web sitesini bulun. Bu web sitesi, dil algılama kapsayıcısında barındırılan dil algılama API 'sini çağıran istemci uygulaması gibi davranır. 

1. Bu web sitesi için Docker görüntüsünü oluşturun. Aşağıdaki komutu çalıştırdığınızda, konsolunun Docker dosyasının bulunduğu [\Frontendservice](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) dizininde olduğundan emin olun:

    ```console
    docker build -t language-frontend -t pattiyregistry.azurecr.io/language-frontend:v1 .
    ```

    Kapsayıcı kayıt defterinizde sürümü izlemek için etiketi, gibi bir sürüm biçimiyle `v1`ekleyin. 

1. Görüntüyü kapsayıcı Kayıt defterinize gönderin. Bu adım birkaç dakika sürebilir. 

    ```console
    docker push pattyregistry.azurecr.io/language-frontend:v1
    ```

    Bir `unauthorized: authentication required` hata alırsanız `az acr login --name <your-container-registry-name>` komutuyla oturum açın. 

    İşlem tamamlandığında, sonuçlar şuna benzerdir:

    ```console
    > docker push pattyregistry.azurecr.io/language-frontend:v1
    The push refers to repository [pattyregistry.azurecr.io/language-frontend]
    82ff52ee6c73: Pushed
    07599c047227: Pushed
    816caf41a9a1: Pushed
    2924be3aed17: Pushed
    45b83a23806f: Pushed
    ef68f6734aa4: Pushed
    v1: digest: sha256:31930445deee181605c0cde53dab5a104528dc1ff57e5b3b34324f0d8a0eb286 size: 1580
    ```

## <a name="get-the-language-detection-docker-image"></a>Dil algılama Docker görüntüsünü al 

1. Docker görüntüsünün en son sürümünü yerel makineye çekin. Bu adım birkaç dakika sürebilir. Bu kapsayıcının daha yeni bir sürümü varsa, değerini `1.1.006770001-amd64-preview` daha yeni bir sürüm olarak değiştirin. 

    ```console
    docker pull mcr.microsoft.com/azure-cognitive-services/language:1.1.006770001-amd64-preview
    ```

1. Görüntüyü kapsayıcı Kayıt defterinize etiketleyerek. En son sürümü bulun ve daha yeni bir sürüme `1.1.006770001-amd64-preview` sahipseniz sürümü değiştirin. 

    ```console
    docker tag mcr.microsoft.com/azure-cognitive-services/language pattiyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

1. Görüntüyü kapsayıcı Kayıt defterinize gönderin. Bu adım birkaç dakika sürebilir. 

    ```console
    docker push pattyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

## <a name="get-container-registry-credentials"></a>Container Registry kimlik bilgilerini al

Bu yordamda daha sonra oluşturduğunuz Azure Kubernetes hizmeti örneğiyle kapsayıcı kayıt defterinizi bağlamak üzere gerekli bilgileri almak için aşağıdaki adımlar gereklidir.

1. Hizmet sorumlusu oluşturun.

    ```azurecli
    az ad sp create-for-rbac --skip-assignment
    ```

    3\. adımdaki `appId` atanan parametrenin sonuç değerini kaydedin. `<appId>` Sonraki bölümün istemci gizli parametresinin `<client-secret>`parolasını kaydedin.

    ```console
    > az ad sp create-for-rbac --skip-assignment
    {
      "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "displayName": "azure-cli-2018-12-31-18-39-32",
      "name": "http://azure-cli-2018-12-31-18-39-32",
      "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. Kapsayıcı kayıt defteri KIMLIĞINIZI alın.

    ```azurecli
    az acr show --resource-group cogserv-container-rg --name pattyregistry --query "id" --o table
    ```

    Kapsam parametre değeri `<acrId>`için çıktıyı bir sonraki adımda kaydedin. Şöyle görünür:

    ```console
    > az acr show --resource-group cogserv-container-rg --name pattyregistry --query "id" --o table
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/cogserv-container-rg/providers/Microsoft.ContainerRegistry/registries/pattyregistry
    ```

    3\. adım için tam değeri bu bölümde saklayın. 

1. AKS kümesine yönelik doğru erişimi, kapsayıcı kayıt defterinizde depolanan görüntüleri kullanmak üzere vermek için, bir rol ataması oluşturun. `<appId>` Ve`<acrId>` değerlerini önceki iki adımda toplanan değerlerle değiştirin.

    ```azurecli
    az role assignment create --assignee <appId> --scope <acrId> --role Reader
    ```

## <a name="create-the-azure-kubernetes-service-cluster"></a>Azure Kubernetes hizmet kümesi oluşturma

1. Kubernetes kümesi oluşturun. Tüm parametre değerleri, ad parametresi dışında önceki bölümden oluşur. Kendisini kimin oluşturduğunu ve bunun amacını `patty-kube`belirten bir ad seçin. 

    ```azurecli
    az aks create --resource-group cogserv-container-rg --name patty-kube --node-count 2  --service-principal <appId>  --client-secret <client-secret>  --generate-ssh-keys
    ```

    Bu adım birkaç dakika sürebilir. Sonuç: 

    ```console
    > az aks create --resource-group cogserv-container-rg --name patty-kube --node-count 2  --service-principal <appId>  --client-secret <client-secret>  --generate-ssh-keys
    {
      "aadProfile": null,
      "addonProfiles": null,
      "agentPoolProfiles": [
        {
          "count": 2,
          "dnsPrefix": null,
          "fqdn": null,
          "maxPods": 110,
          "name": "nodepool1",
          "osDiskSizeGb": 30,
          "osType": "Linux",
          "ports": null,
          "storageProfile": "ManagedDisks",
          "vmSize": "Standard_DS1_v2",
          "vnetSubnetId": null
        }
      ],
      "dnsPrefix": "patty-kube--65a101",
      "enableRbac": true,
      "fqdn": "patty-kube--65a101-341f1f54.hcp.westus.azmk8s.io",
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/cogserv-container-rg/providers/Microsoft.ContainerService/managedClusters/patty-kube",
      "kubernetesVersion": "1.9.11",
      "linuxProfile": {
        "adminUsername": "azureuser",
        "ssh": {
          "publicKeys": [
            {
              "keyData": "ssh-rsa AAAAB3NzaC...ohR2d81mFC
            }
          ]
        }
      },
      "location": "westus",
      "name": "patty-kube",
      "networkProfile": {
        "dnsServiceIp": "10.0.0.10",
        "dockerBridgeCidr": "172.17.0.1/16",
        "networkPlugin": "kubenet",
        "networkPolicy": null,
        "podCidr": "10.244.0.0/16",
        "serviceCidr": "10.0.0.0/16"
      },
      "nodeResourceGroup": "MC_patty_westus",
      "provisioningState": "Succeeded",
      "resourceGroup": "cogserv-container-rg",
      "servicePrincipalProfile": {
        "clientId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "keyVaultSecretRef": null,
        "secret": null
      },
      "tags": null,
      "type": "Microsoft.ContainerService/ManagedClusters"
    }
    ```

    Hizmet oluşturuldu, ancak Web sitesi kapsayıcısı veya dil algılama kapsayıcısı henüz yok. 

1. Kubernetes kümesinin kimlik bilgilerini alın. 

    ```azurecli
    az aks get-credentials --resource-group cogserv-container-rg --name patty-kube
    ```

## <a name="load-the-orchestration-definition-into-your-kubernetes-service"></a>Kubernetes hizmetinize Orchestration tanımını yükleyin

Bu bölümde, Azure Kubernetes hizmeti örneğinizle konuşmak için **kubectl** CLI kullanılmaktadır. 

1. Orchestration tanımını yüklemeden önce, **kubectl** 'nin düğümlere erişimi olduğundan emin olun.

    ```console
    kubectl get nodes
    ```

    Yanıt şöyle görünür:

    ```console
    > kubectl get nodes
    NAME                       STATUS    ROLES     AGE       VERSION
    aks-nodepool1-13756812-0   Ready     agent     6m        v1.9.11
    aks-nodepool1-13756812-1   Ready     agent     6m        v1.9.11
    ```

1. Aşağıdaki dosyayı kopyalayın ve adlandırın `language.yml`. Dosyada bir `service` bölüm `deployment` ve her biri `language-frontend` iki kapsayıcı türü, Web sitesi kapsayıcısı ve `language` algılama kapsayıcısı için bir bölüm bulunur. 

    [!code-yml[Kubernetes orchestration file for the Cognitive Services containers sample](~/samples-cogserv-containers/Kubernetes/language/language.yml "Kubernetes orchestration file for the Cognitive Services containers sample")]

1. Kendi kapsayıcı kayıt defteri görüntü adlarınızı, istemci gizli `language.yml` anahtarını ve metin analizi ayarlarını eklemek için aşağıdaki tabloya göre dil ön uç dağıtım hatlarını değiştirin.

    Dil ön uç dağıtım ayarları|Amaç|
    |--|--|
    |Satır 32<br> `image`özelliði|Kapsayıcı kayıt defterinizde ön uç görüntüsünün görüntü konumu<br>`<container-registry-name>.azurecr.io/language-frontend:v1`.|
    |Satır 44<br> `name`özelliði|Resim için kapsayıcı kayıt defteri gizli dizisi, önceki bölümde `<client-secret>` olarak adlandırılır.|

1. Kendi kapsayıcı kayıt defteri görüntü adlarınızı `language.yml` , istemci gizli anahtarını ve metin analizi ayarlarını eklemek için aşağıdaki tabloya göre dil dağıtım çizgilerini değiştirin.

    |Dil dağıtım ayarları|Amaç|
    |--|--|
    |Satır 78<br> `image`özelliði|Kapsayıcı kayıt defterinizde dil görüntüsünün görüntü konumu<br>`<container-registry-name>.azurecr.io/language:1.1.006770001-amd64-preview`.|
    |Satır 95<br> `name`özelliði|Resim için kapsayıcı kayıt defteri gizli dizisi, önceki bölümde `<client-secret>` olarak adlandırılır.|
    |Satır 91<br> `apiKey`özelliði|Metin Analizi kaynak anahtarınız.|
    |Satır 92<br> `billing`özelliði|Metin Analizi kaynağınız için faturalandırma uç noktası.<br>`https://westus.api.cognitive.microsoft.com/text/analytics/v2.1`.|

    **Apikey** ve **faturalandırma uç noktası** özellikleri Kubernetes Orchestration tanımının bir parçası olarak ayarlandığı için, Web sitesi kapsayıcısının bu ilgili bilgileri bilmeleri veya isteğin bir parçası olarak iletmeleri gerekmez. Web sitesi kapsayıcısı, Orchestrator adına `language`göre dil algılama kapsayıcısını ifade eder. 

1. Oluşturduğunuz ve kaydettiğiniz `language.yml`klasörden bu örnek için Orchestration tanım dosyasını yükleyin. 

    ```console
    kubectl apply -f language.yml
    ```

    Yanıt:

    ```console
    > kubectl apply -f language.yml
    service "language-frontend" created
    deployment.apps "language-frontend" created
    service "language" created
    deployment.apps "language" created
    ```

## <a name="get-external-ips-of-containers"></a>Kapsayıcılardan dış IP 'Leri al

İki kapsayıcı için, `language-frontend` ve `language` hizmetlerinin çalıştığını doğrulayın ve dış IP adresini alın. 

```console
kubectl get all
```

```console
> kubectl get all
NAME                                     READY     STATUS    RESTARTS   AGE
pod/language-586849d8dc-7zvz5            1/1       Running   0          13h
pod/language-frontend-68b9969969-bz9bg   1/1       Running   1          13h

NAME                        TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)          AGE
service/kubernetes          ClusterIP      10.0.0.1      <none>          443/TCP          14h
service/language            LoadBalancer   10.0.39.169   104.42.172.68   5000:30161/TCP   13h
service/language-frontend   LoadBalancer   10.0.42.136   104.42.37.219   80:30943/TCP     13h

NAME                                      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/language            1         1         1            1           13h
deployment.extensions/language-frontend   1         1         1            1           13h

NAME                                                 DESIRED   CURRENT   READY     AGE
replicaset.extensions/language-586849d8dc            1         1         1         13h
replicaset.extensions/language-frontend-68b9969969   1         1         1         13h

NAME                                DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/language            1         1         1            1           13h
deployment.apps/language-frontend   1         1         1            1           13h

NAME                                           DESIRED   CURRENT   READY     AGE
replicaset.apps/language-586849d8dc            1         1         1         13h
replicaset.apps/language-frontend-68b9969969   1         1         1         13h
```

`EXTERNAL-IP` Hizmeti için bekliyor olarak gösteriliyorsa, sonraki adıma geçmeden önce IP adresi gösterilene kadar komutu yeniden çalıştırın. 

## <a name="test-the-language-detection-container"></a>Dil algılama kapsayıcısını test etme

Bir tarayıcı açın ve önceki bölümden `language` kapsayıcının dış IP adresine gidin:. `http://<external-ip>:5000/swagger/index.html` Dil algılama uç noktasını test etmek için API 'nin özelliğinikullanın.`Try it` 

![Kapsayıcının Swagger belgelerini görüntüleme](../media/how-tos/container-instance-sample/language-detection-container-swagger-documentation.png)

## <a name="test-the-client-application-container"></a>İstemci uygulama kapsayıcısını test etme

Tarayıcıdaki URL 'yi, aşağıdaki biçimi kullanarak `language-frontend` kapsayıcının dış IP 'sine değiştirin:. `http://<external-ip>/helloworld` ' Nin `helloworld` İngilizce kültür metni olarak `English`tahmin edilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Küme ile işiniz bittiğinde Azure kaynak grubunu silin. 

```azure-cli
az group delete --name cogserv-container-rg
```

## <a name="related-information"></a>İlgili bilgiler

* [Docker kullanıcıları için kubectl](https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/)

## <a name="next-steps"></a>Sonraki adımlar 

* Daha fazla bilişsel [Hizmetler kapsayıcısı](../../cognitive-services-container-support.md)kullanın.
* [Metin analizi bağlı hizmetlerini](../vs-text-connected-service.md)kullanın.


<!--
kubectl get secrets

>az aks browse --resource-group diberry-cogserv-container-rg --name diberry-kubernetes-languagedetection

kubectl proxy

http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/pod/default/language-frontend-6d65bdb77c-8f4qv?namespace=default

kubectl describe pod language-frontend-6d65bdb77c
-->