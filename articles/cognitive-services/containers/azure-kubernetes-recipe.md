---
title: Kubernetes Servisi'nde Dil Algılama konteynerini çalıştırın
titleSuffix: Text Analytics -  Azure Cognitive Services
description: Dil algılama kapsayıcısını, çalışan bir örnekle Azure Kubernetes Hizmeti'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: cdd1cf255c943c8dc6d55a5b749b30357bdcd373
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876734"
---
# <a name="deploy-the-text-analytics-language-detection-container-to-azure-kubernetes-service"></a>Text Analytics dil algılama konteynerini Azure Kubernetes Hizmetine dağıtın

Dil algılama kapsayıcısını nasıl dağıtılacayacak gerektiğini öğrenin. Bu yordam, yerel Docker kapsayıcılarını nasıl oluşturduğunuzu, kapsayıcıları kendi özel konteyner kayıt defterinize nasıl itdiğinizi, kapsayıcıyı Bir Kubernetes kümesinde çalıştırmayı ve bir web tarayıcısında nasıl test ettiğinizi gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu yordam, yüklenmesi ve yerel olarak çalıştırılması gereken çeşitli araçlar gerektirir. Azure Bulut uytun.

* Azure aboneliği kullanın. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
* Bu yordamda kullanılan [örneği](https://github.com/Azure-Samples/cognitive-services-containers-samples) klonlayabilmeniz için işletim sisteminizi [çalıştırın.](https://git-scm.com/downloads)
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* [Docker motoru](https://www.docker.com/products/docker-engine) ve Docker CLI bir konsol penceresinde çalıştığını doğrulayın.
* [kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.13.1/bin/windows/amd64/kubectl.exe).
* Doğru fiyatlandırma katmanına sahip bir Azure kaynağı. Tüm fiyatlandırma katmanları bu kapsayıcıyla çalışmaz:
  * Yalnızca F0 veya Standart fiyatlandırma katmanlarına sahip **Text Analytics** kaynağı.
  * S0 fiyatlandırma katmanı ile **Bilişsel Hizmetler** kaynak.

## <a name="running-the-sample"></a>Örneği çalıştırma

Bu yordam, dil algılama için Bilişsel Hizmetler Kapsayıcıörneğini yükler ve çalıştırır. Örnekte, biri istemci başvurusu, diğeri Bilişsel Hizmetler kapsayıcısı olmak üzere iki kapsayıcı vardır. Bu görüntülerin her ikisini de Azure Kapsayıcı Kayıt Defteri'ne taşıyacağız. Kendi kayıt defterinize girince, bu resimlere erişmek ve kapsayıcıları çalıştırmak için bir Azure Kubernetes Hizmeti oluşturun. Kapsayıcılar çalışırken, kapsayıcıların performansını izlemek için **kubectl** CLI'yi kullanın. Bir HTTP isteğiyle istemci uygulamasına erişin ve sonuçları görün.

![Örnek kapsayıcıların çalıştırıldırılabilme kavramsal fikri](../text-analytics/media/how-tos/container-instance-sample/containers.png)

## <a name="the-sample-containers"></a>Örnek kaplar

Örnek iki konteyner görüntüleri, ön yüz web sitesi için bir vardır. İkinci resim, algılanan metni (kültürü) döndüren dil algılama kapsayıcısıdır. İşbittiğinde her iki kapsayıcıya da harici bir IP'den erişilebilir.

### <a name="the-language-frontend-container"></a>Dil-ön uç konteyner

Bu web sitesi, dil algılama bitiş noktası isteklerini yapan kendi istemci tarafı uygulamaeşdeğerdir. Yordam tamamlandığında, bir tarayıcıda `http://<external-IP>/<text-to-analyze>`web sitesi kapsayıcıerişerek karakter dizisinin algılanan dilini elde . Bu URL'nin `http://132.12.23.255/helloworld!`bir örneği. Tarayıcıda sonuç `English`.

### <a name="the-language-container"></a>Dil kapsayıcısı

Bu özel yordamda dil algılama kapsayıcısı, herhangi bir dış istek için erişilebilir. Konteyner herhangi bir şekilde değiştirilmemiştir, bu nedenle standart Bilişsel Hizmetler konteynerözel dil algılama API kullanılabilir.

Bu kapsayıcı için, bu API dil algılama için bir POST isteğidir. Tüm Bilişsel Hizmetler kapsayıcılarında olduğu gibi, konteyner hakkında barındırılan `http://<external-IP>:5000/swagger/index.html`Swagger bilgisinden daha fazla bilgi edinebilirsiniz.

Port 5000, Bilişsel Hizmetler kapsayıcıları ile kullanılan varsayılan bağlantı noktasıdır.

## <a name="create-azure-container-registry-service"></a>Azure Kapsayıcı Kayıt Defteri hizmeti oluşturma

Kapsayıcıyı Azure Kubernetes Hizmeti'ne dağıtmak için kapsayıcı resimlerine erişilebilmeniz gerekir. Görüntüleri barındırmak için kendi Azure Kapsayıcı Kayıt Defteri hizmetinizi oluşturun.

1. Azure CLI'de oturum açın

    ```azurecli-interactive
    az login
    ```

1. Bu yordamda `cogserv-container-rg` oluşturulan her kaynağı tutmak için adlı bir kaynak grubu oluşturun.

    ```azurecli-interactive
    az group create --name cogserv-container-rg --location westus
    ```

1. Kendi Azure Kapsayıcı Kayıt Defterinizi adınızın `registry`biçimiyle `pattyregistry`oluşturun, örneğin . Addaki tire veya altı çizili karakterler kullanmayın.

    ```azurecli-interactive
    az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    ```

    **LoginServer** özelliğini almak için sonuçları kaydedin. Bu, daha sonra dosyada kullanılan barındırılan `language.yml` kapsayıcının adresinin bir parçası olacaktır.

    ```azurecli-interactive
    az acr create --resource-group cogserv-container-rg --name pattyregistry --sku Basic
    ```

    ```output
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

1. Konteyner kayıt defterinizde oturum açın. Görüntüleri kayıt defterinize itmeden önce oturum açmanız gerekir.

    ```azurecli-interactive
    az acr login --name pattyregistry
    ```

## <a name="get-website-docker-image"></a>Web sitesi Docker resmi alın

1. Bu yordamda kullanılan örnek kodu Bilişsel Hizmetler kapları örnekleri deposundadır. Örneğin yerel bir kopyasına sahip olmak için depoyu klonlayın.

    ```console
    git clone https://github.com/Azure-Samples/cognitive-services-containers-samples
    ```

    Depo yerel bilgisayarınıza yüklendikten sonra, web sitesini [\dotnet\Language\FrontendService](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) dizininde bulun. Bu web sitesi, dil algılama kapsayıcısında barındırılan dil algılama API'sini çağıran istemci uygulaması görevi görür.  

1. Bu web sitesi için Docker görüntü oluşturun. Aşağıdaki komutu çalıştırdığınızda konsolun Dockerfile'ın bulunduğu [\FrontendService](https://github.com/Azure-Samples/cognitive-services-containers-samples/tree/master/dotnet/Language/FrontendService) dizininde olduğundan emin olun:

    ```console
    docker build -t language-frontend -t pattiyregistry.azurecr.io/language-frontend:v1 .
    ```

    Kapsayıcı kayıt defterinizdeki sürümü izlemek için etiketi bir sürüm biçimiyle ekleyin. `v1`

1. Görüntüyü konteyner kayıt defterinize itin. Bu birkaç dakika sürebilir.

    ```console
    docker push pattyregistry.azurecr.io/language-frontend:v1
    ```

    Bir `unauthorized: authentication required` hata alırsanız, `az acr login --name <your-container-registry-name>` komutile giriş yapın. 

    İşlem yapıldığında, sonuçlar aşağıdakilere benzer olmalıdır:

    ```output
    The push refers to repository [pattyregistry.azurecr.io/language-frontend]
    82ff52ee6c73: Pushed
    07599c047227: Pushed
    816caf41a9a1: Pushed
    2924be3aed17: Pushed
    45b83a23806f: Pushed
    ef68f6734aa4: Pushed
    v1: digest: sha256:31930445deee181605c0cde53dab5a104528dc1ff57e5b3b34324f0d8a0eb286 size: 1580
    ```

## <a name="get-language-detection-docker-image"></a>Dil algılama Docker görüntü alın

1. Docker görüntüsünün en son sürümünü yerel makineye çekin. Bu birkaç dakika sürebilir. Bu kapsayıcının daha yeni bir sürümü varsa, `1.1.006770001-amd64-preview` değeri yeni sürüme değiştirin.

    ```console
    docker pull mcr.microsoft.com/azure-cognitive-services/language:1.1.006770001-amd64-preview
    ```

1. Resmi konteyner kayıt defterinizle etiketleyin. En son sürümü bulun `1.1.006770001-amd64-preview` ve daha yeni bir sürümünüz varsa sürümü değiştirin. 

    ```console
    docker tag mcr.microsoft.com/azure-cognitive-services/language pattiyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

1. Görüntüyü konteyner kayıt defterinize itin. Bu birkaç dakika sürebilir.

    ```console
    docker push pattyregistry.azurecr.io/language:1.1.006770001-amd64-preview
    ```

## <a name="get-container-registry-credentials"></a>Konteyner Kayıt Defteri kimlik bilgilerini alın

Konteyner kayıt defterinizi daha sonra bu yordamda oluşturduğunuz Azure Kubernetes Hizmeti'ne bağlamak için gerekli bilgileri almak için aşağıdaki adımlar gereklidir.

1. Hizmet ilkesi oluşturun.

    ```azurecli-interactive
    az ad sp create-for-rbac --skip-assignment
    ```

    3. `appId` adımda devralan parametrenin sonuç değerini `<appId>`kaydedin. Bir `password` sonraki bölümün istemci-gizli parametresini `<client-secret>`kaydedin.

    ```output
    {
      "appId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "displayName": "azure-cli-2018-12-31-18-39-32",
      "name": "http://azure-cli-2018-12-31-18-39-32",
      "password": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "tenant": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    }
    ```

1. Konteyner kayıt kimliğinizi alın.

    ```azurecli-interactive
    az acr show --resource-group cogserv-container-rg --name pattyregistry --query "id" --o table
    ```

    Çıktıyı bir sonraki adımda `<acrId>`kapsam parametresi değerine kaydedin. Bu gibi görünüyor:

    ```output
    /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/cogserv-container-rg/providers/Microsoft.ContainerRegistry/registries/pattyregistry
    ```

    Bu bölümde adım 3 için tam değeri kaydedin.

1. AKS kümesine konteyner kayıt defterinizde depolanan görüntüleri kullanması için doğru erişim sağlamak için bir rol ataması oluşturun. Önceki `<appId>` `<acrId>` iki adımda toplanan değerleri değiştirin ve değiştirin.

    ```azurecli-interactive
    az role assignment create --assignee <appId> --scope <acrId> --role Reader
    ```

## <a name="create-azure-kubernetes-service"></a>Azure Kubernetes Hizmeti Oluşturma

1. Kubernetes kümesi oluşturun. Ad parametresi dışında tüm parametre değerleri önceki bölümlerden dir. Onu kimin yarattığını ve amacını belirten bir `patty-kube`ad seçin, örneğin.

    ```azurecli-interactive
    az aks create --resource-group cogserv-container-rg --name patty-kube --node-count 2  --service-principal <appId>  --client-secret <client-secret>  --generate-ssh-keys
    ```

    Bu adım birkaç dakika sürebilir. Sonuç şudur:

    ```output
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

    Hizmet oluşturulur, ancak henüz web sitesi kapsayıcısı veya dil algılama kapsayıcısı yoktur.  

1. Kubernetes kümesinin kimlik bilgilerini alın.

    ```azurecli-interactive
    az aks get-credentials --resource-group cogserv-container-rg --name patty-kube
    ```

## <a name="load-the-orchestration-definition-into-your-kubernetes-service"></a>Orkestrasyon tanımını Kubernetes hizmetinize yükleyin

Bu bölümde, Azure Kubernetes Hizmeti ile konuşmak için **kubectl** CLI kullanır.

1. Orkestrasyon tanımını yüklemeden önce **kubectl'in** düğümlere erişimi olduğunu kontrol edin.

    ```console
    kubectl get nodes
    ```

    Yanıt gibi görünüyor:

    ```output
    NAME                       STATUS    ROLES     AGE       VERSION
    aks-nodepool1-13756812-0   Ready     agent     6m        v1.9.11
    aks-nodepool1-13756812-1   Ready     agent     6m        v1.9.11
    ```

1. Aşağıdaki dosyayı kopyalayın `language.yml`ve adlandırın. Dosya, iki `service` kapsayıcı `deployment` türü, `language-frontend` web sitesi kapsayıcısı ve algılama `language` kapsayıcısı için bir bölüm ve bir bölüm vardır.

    [!code-yml[Kubernetes orchestration file for the Cognitive Services containers sample](~/samples-cogserv-containers/Kubernetes/language/language.yml "Kubernetes orchestration file for the Cognitive Services containers sample")]

1. Kendi kapsayıcı kayıt defteri resim `language.yml` adlarınızı, istemci sırrınızı ve metin analizi ayarlarınızı eklemek için aşağıdaki tabloya göre dil önuç dağıtım satırlarını değiştirin.

    Dil önuç dağıtım ayarları|Amaç|
    |--|--|
    |Satır 32<br> `image`Özellik|Konteyner Kayıt Defterinizdeki ön uç görüntüsünün görüntü konumu<br>`<container-registry-name>.azurecr.io/language-frontend:v1`|
    |Satır 44<br> `name`Özellik|Önceki bölümde olarak `<client-secret>` adlandırılan görüntü için Konteyner Kayıt Defteri gizli.|

1. Kendi kapsayıcı kayıt `language.yml` defteri resim adlarınızı, istemci sırrınızı ve metin analizi ayarlarınızı eklemek için aşağıdaki tabloya göre dil dağıtım satırlarını değiştirin.

    |Dil dağıtım ayarları|Amaç|
    |--|--|
    |Satır 78<br> `image`Özellik|Konteyner Kayıt Defterinizdeki dil resminin görüntü konumu<br>`<container-registry-name>.azurecr.io/language:1.1.006770001-amd64-preview`|
    |Satır 95<br> `name`Özellik|Önceki bölümde olarak `<client-secret>` adlandırılan görüntü için Konteyner Kayıt Defteri gizli.|
    |Satır 91<br> `apiKey`Özellik|Metin analizi kaynak anahtarınız|
    |Satır 92<br> `billing`Özellik|Metin analizi kaynağınızın faturalandırma bitiş noktası.<br>`https://westus.api.cognitive.microsoft.com/text/analytics/v2.1`|

    **ApiKey** ve **faturalandırma bitiş noktası** Kubernetes orkestrasyon tanımının bir parçası olarak ayarlandığından, web sitesi kapsayıcısının bunları bilmesi veya isteğin bir parçası olarak geçmesi gerekmez. Web sitesi konteyner onun orkestratör adı `language`ile dil algılama konteyner anlamına gelir.

1. Oluşturduğunuz ve kaydettiğiniz klasörden bu örnek için düzenleme `language.yml`tanımı dosyasını yükleyin.

    ```console
    kubectl apply -f language.yml
    ```

    Yanıt:

    ```output
    service "language-frontend" created
    deployment.apps "language-frontend" created
    service "language" created
    deployment.apps "language" created
    ```

## <a name="get-external-ips-of-containers"></a>Kapsayıcıların harici IP'lerini alın

İki kapsayıcı için, `language-frontend` `language` hizmetlerin çalıştığını doğrulayın ve harici IP adresini alın.

```console
kubectl get all
```

```output
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

Hizmet `EXTERNAL-IP` için beklemede olarak gösteriliyorsa, bir sonraki adıma geçmeden önce IP adresi gösterilene kadar komutu yeniden çalıştırın.

## <a name="test-the-language-detection-container"></a>Dil algılama kabını test edin

Bir tarayıcı açın ve önceki bölümden kapsayıcının `language` harici `http://<external-ip>:5000/swagger/index.html`IP'sine gidin: . Dil algılama `Try it` bitiş noktasını sınamak için API özelliğini kullanabilirsiniz.

![Kapsayıcının gösterişli belgelerini görüntüleme](../text-analytics/media/how-tos/container-instance-sample/language-detection-container-swagger-documentation.png)

## <a name="test-the-client-application-container"></a>İstemci uygulama kapsayıcısını test etme

Tarayıcıdaki URL'yi aşağıdaki biçimi kullanarak `language-frontend` kapsayıcının harici `http://<external-ip>/helloworld`IP'si ile değiştirin: . İngilizce kültür metni `helloworld` olarak `English`tahmin edilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kümeyle ilgili çalışmanız bittiğinde Azure kaynak grubunu silin.

```azurecli-interactive
az group delete --name cogserv-container-rg
```

## <a name="related-information"></a>İlgili bilgiler

* [Docker Kullanıcılar için kubectl](https://kubernetes.io/docs/reference/kubectl/docker-cli-to-kubectl/)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilişsel Hizmetler Konteynerler](../cognitive-services-container-support.md)

<!--
kubectl get secrets

>az aks browse --resource-group diberry-cogserv-container-rg --name diberry-kubernetes-languagedetection

kubectl proxy

http://localhost:8001/api/v1/namespaces/kube-system/services/kubernetes-dashboard/proxy/#!/pod/default/language-frontend-6d65bdb77c-8f4qv?namespace=default

kubectl describe pod language-frontend-6d65bdb77c
-->