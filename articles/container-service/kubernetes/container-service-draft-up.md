---
title: Kullanım DıŞı Azure Container Service ve Azure Container Registry birlikte taslağı kullanın
description: Draft ile Azure’da ilk uygulamanızı oluşturmak için bir ACS Kubernetes kümesi ve bir Azure Container Registry oluşturun.
author: squillace
ms.service: container-service
ms.topic: conceptual
ms.date: 09/14/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: 8d688d2918c9100019d033e93e9a3dca9e492de2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76271144"
---
# <a name="deprecated-use-draft-with-azure-container-service-and-azure-container-registry-to-build-and-deploy-an-application-to-kubernetes"></a>Kullanım DıŞı Kubernetes 'te uygulama derlemek ve dağıtmak için Azure Container Service ve Azure Container Registry ile taslak kullanma

> [!TIP]
> Azure Kubernetes hizmetini kullanan Bu makalenin güncelleştirilmiş sürümü için bkz. [Azure Kubernetes Service (AKS) Ile taslak kullanma](../../aks/kubernetes-draft.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

[Draft](https://aka.ms/draft), Docker ve Kubernetes hakkında pek fazla bilginiz olmadan, hatta bunları yüklemeden kapsayıcı tabanlı uygulamalar geliştirmeyi ve bu uygulamaları Kubernetes kümelerine dağıtmayı kolaylaştıran yeni bir açık kaynak araçtır. Draft gibi araçların kullanılması, sizin ve ekiplerinizin altyapıya eskisi kadar dikkat etmesine gerek kalmadan Kubernetes ile uygulama oluşturmaya odaklanmasına imkan sağlar.

Draft’ı yerel kullanım dahil olmak üzere herhangi bir Docker görüntü kayıt defteri ve herhangi bir Kubernetes kümesiyle kullanabilirsiniz. Bu öğreticide, Kubernetes ve ACR ile ACS kullanarak Kubernetes 'te taslak ile canlı ancak güvenli bir geliştirici işlem hattı oluşturma ve bu geliştirici işlem hattının başkalarının bir etki alanında görmesi için Azure DNS kullanma hakkında bilgi verilmektedir.


## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma
Kolayca [yeni Azure Container Registry](../../container-registry/container-registry-get-started-azure-cli.md) oluşturabilirsiniz, ancak adımlar aşağıdaki gibidir:

1. ACS 'de ACR kayıt defterinizi ve Kubernetes kümesini yönetmek için bir Azure Kaynak grubu oluşturun.
      ```azurecli
      az group create --name draft --location eastus
      ```

2. [Az ACR Create](/cli/azure/acr#az-acr-create) kullanarak bir ACR görüntü kayıt defteri oluşturun ve `--admin-enabled` seçeneğin olarak `true`ayarlandığından emin olun.
      ```azurecli
      az acr create --resource-group draft --name draftacs --sku Basic
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a>Kubernetes ile Azure Container Service oluşturma

Artık [az acs create](/cli/azure/acs#az-acs-create) komutu ile `--orchestrator-type` değeri olarak Kubernetes’i kullanarak bir ACS kümesi oluşturmaya hazırsınız.
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes --generate-ssh-keys
```

> [!NOTE]
> Kubernetes varsayılan düzenleyici olmadığından, `--orchestrator-type kubernetes` anahtarını kullandığınızdan emin olun.

İşlem başarılı olursa çıktı aşağıdaki gibi görünür.

```json
waiting for AAD role to propagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

Artık bir kümeniz olduğuna göre, [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes) komutunu kullanarak kimlik bilgilerini içeri aktarabilirsiniz. Artık kümeniz için yerel bir yapılandırma dosyanız vardır ve Helm ile Draft’ın işlerini yapabilmesi için bu gerekir.

## <a name="install-and-configure-draft"></a>Draft’ı yükleme ve yapılandırma


1. https://github.com/Azure/draft/releases ' De ortamınız için taslak indirin ve komutun KULLANıLABILMESI için yolunuza yükleyin.
2. Ortamınızdaki Held 'yi indirin https://github.com/kubernetes/helm/releases ve [komutun kullanılabilmesi için yolunuza yükleyin](https://github.com/kubernetes/helm/blob/master/docs/install.md#installing-the-helm-client).
3. Draft’ı kayıt defterinizi kullanacak şekilde yapılandırın ve Draft’ın oluşturduğu her Helm grafiği için alt etki alanları oluşturun. Draft’ı yapılandırmak için şunlar gerekir:
   - Azure Container Registry adınız (bu örnekte `draftacsdemo` kullanılmıştır)
   - `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"` dosyasından kayıt defteri anahtarınız veya parolanız.

   Çağrı `draft init` ve yapılandırma işlemi sizden yukarıdaki değerleri ister. kayıt defteri URL 'sinin URL biçiminin kayıt defteri adı (Bu örnekte, `draftacsdemo`) Plus `.azurecr.io`olduğunu unutmayın. Kullanıcı adınız kayıt defteri adıdır. İşlem ilk çalıştırıldığında aşağıdaki gibi görünür.
   ```bash
    $ draft init
    Creating /home/ralph/.draft 
    Creating /home/ralph/.draft/plugins 
    Creating /home/ralph/.draft/packs 
    Creating pack go...
    Creating pack python...
    Creating pack ruby...
    Creating pack javascript...
    Creating pack gradle...
    Creating pack java...
    Creating pack php...
    Creating pack csharp...
    $DRAFT_HOME has been configured at /home/ralph/.draft.

    In order to configure Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): draftacsdemo.azurecr.io
    2. Enter your username: draftacsdemo
    3. Enter your password: 
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
   ```

Artık uygulama dağıtmaya hazırsınız.


## <a name="build-and-deploy-an-application"></a>Uygulama oluşturma ve dağıtma

Draft deposunda [altı basit örnek uygulama](https://github.com/Azure/draft/tree/master/examples) yer alır. Depoyu kopyalayın ve [Java örneğini](https://github.com/Azure/draft/tree/master/examples/example-java)kullanalım. Örneklere/Java dizinine geçin ve uygulamayı derlemek için yazın `draft create` . Aşağıdaki örnekteki gibi görünmelidir.
```bash
$ draft create
--> Draft detected the primary language as Java with 91.228814% certainty.
--> Ready to sail
```

Çıktı bir Docker dosyası ve Helm grafiği içerir. Derlemek ve dağıtmak için `draft up` yazmanız yeterlidir. Çıktı kapsamlıdır, ancak aşağıdaki örnekte olduğu gibi olmalıdır.
```bash
$ draft up
Draft Up Started: 'handy-labradoodle'
handy-labradoodle: Building Docker Image: SUCCESS ⚓  (35.0232s)
handy-labradoodle: Pushing Docker Image: SUCCESS ⚓  (17.0062s)
handy-labradoodle: Releasing Application: SUCCESS ⚓  (3.8903s)
handy-labradoodle: Build ID: 01BT0ZJ87NWCD7BBPK4Y3BTTPB
```

## <a name="securely-view-your-application"></a>Uygulamanızı güvenle görüntüleyin

Kapsayıcınız artık ACS 'de çalışmaktadır. Bunu görüntülemek için, uygulamanızın belirli `draft connect` bir bağlantı noktasıyla, yerel olarak görüntüleyebilmeniz için, kümenin IP 'si ile güvenli bir bağlantı oluşturan komutunu kullanın. Başarılı olursa, **başarılı** göstergeden sonra ilk satırdaki uygulamanıza bağlanmak için URL 'yi arayın.

> [!NOTE]
> Hiç Pod 'nin hazırlandığını söyleyen bir ileti alırsanız, bir süre bekleyip yeniden deneyin ya da Ayrıca, ile birlikte `kubectl get pods -w` çalışmaya ve sonra yeniden deneyin.

```bash
draft connect
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Yukarıdaki örnekte, yanıtı almak için yazabilirsiniz `curl -s http://localhost:46143` `Hello World, I'm Java!`. CTRL + veya CMD + C (OS ortamınıza bağlı olarak) kullandığınızda, güvenli tünel kaldırılır ve yinelenmeye devam edebilirsiniz.

## <a name="sharing-your-application-by-configuring-a-deployment-domain-with-azure-dns"></a>Azure DNS ile dağıtım etki alanı yapılandırarak uygulamanızı paylaşma

Önceki adımlarda taslak tarafından oluşturulan geliştirici yineleme döngüsünü zaten gerçekleştirdiyseniz. Ancak, uygulamanızı Internet üzerinden şu şekilde paylaşabilirsiniz:
1. ACS kümenize giriş yükleme (uygulamanın görüntüleneceği genel bir IP adresi sağlamak için)
2. Özel etki alanınızı Azure DNS ve etki alanınızı, ACS 'nin giriş denetleyicinize atadığı IP adresine eşlemek için temsilci seçme

### <a name="use-helm-to-install-the-ingress-controller"></a>Giriş denetleyicisini yüklemek için Held kullanın.
Derlemeleriniz için gelen istekleri etkinleştirmek üzere bir `stable/traefik`giriş denetleyicisi aramak ve yüklemek için **Held** 'yi kullanın.
```bash
$ helm search traefik
NAME            VERSION DESCRIPTION
stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

$ helm install stable/traefik --name ingress
```
Şimdi, dış IP değeri dağıtıldığında bu değeri yakalaması için `ingress` denetleyicisinde bir izleme ayarlayın. Bu IP adresi, bir sonraki bölümde dağıtım etki alanınızla eşleştirilecek adrestir.

```bash
$ kubectl get svc -w
NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
kubernetes                    10.0.0.1       <none>          443/TCP                      7h
```

Bu durumda, dağıtım etki alanının dış IP’si şudur: `13.64.108.240`. Artık etki alanınızı bu IP ile eşleyebilirsiniz.

### <a name="map-the-ingress-ip-to-a-custom-subdomain"></a>Giriş IP 'sini özel bir alt etki alanı ile eşleme

Draft, oluşturduğu her Helm grafiği (üzerinde çalıştığınız her uygulama) için bir yayın oluşturur. Her biri, sizin denetlediğiniz kök _dağıtım etki alanının_ üzerinde bir alt _etki_ alanı olarak **taslak** tarafından kullanılan bir oluşturulan ad alır. (Bu örnekte, dağıtım etki alanı `squillace.io` olarak kullanırız.) Bu alt etki alanı davranışını etkinleştirmek için, oluşturulan her alt etki alanının `'*.draft'` Kubernetes kümesinin giriş denetleyicisine yönlendirilmesi için, dağıtım etkı alanınızın DNS girdinizde Için bir A kaydı oluşturmanız gerekir. 

Etki alanı sağlayıcınız, DNS sunucularını atamak için kendi yöntemini kullanır; [Azure DNS’yi etki alanı ad sunucularınızın temsilcisi olarak atamak için](../../dns/dns-delegate-domain-azure-dns.md) aşağıdaki adımları gerçekleştirirsiniz:

1. Bölgeniz için bir kaynak grubu oluşturun.
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. Etki alanınız için bir DNS bölgesi oluşturun.
   Etki alanınız için Azure DNS’yi DNS denetimi temsilcisi olarak atamak istiyorsanız [az network dns zone create](/cli/azure/network/dns/zone#az-network-dns-zone-create) komutunu kullanarak ad sunucularını edinin.
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. Size döndürülen DNS sunucularını, dağıtım etki alanınızın etki alanı sağlayıcısına ekleyin. Bunu yaptığınızda, Azure DNS’nizi kullanarak etki alanınızı istediğiniz yeri gösterecek şekilde ayarlayabilirsiniz. Bunu yapmanın yolu, etki alanına göre değişir; [etki alanı kullanır ' a temsilci olarak Azure DNS](../../dns/dns-delegate-domain-azure-dns.md) , bilmeniz gereken bazı ayrıntıları içerir. 
4. Etki alanınız Azure DNS için temsilci seçildikten sonra, önceki bölümün 2. adımından `ingress` IP 'ye eşlenen dağıtım etki alanınız için bir kayıt kümesi girişi oluşturun.
   ```azurecli
   az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*.draft' -g squillace.io -z squillace.io
   ```
   Çıktı şuna benzer:
   ```json
   {
    "arecords": [
      {
        "ipv4Address": "13.64.108.240"
      }
    ],
    "etag": "<guid>",
    "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
    "metadata": null,
    "name": "*.draft",
    "resourceGroup": "squillace.io",
    "ttl": 3600,
    "type": "Microsoft.Network/dnszones/A"
   }
   ```
5. **Taslağı** yeniden yükle

   1. `helm delete --purge draft`Yazarak **draftd** 'yi kümeden kaldırın. 
   2. **Taslağı** aynı `draft-init` komutu kullanarak, ancak `--ingress-enabled` seçeneğiyle yeniden yükleyin:
      ```bash
      draft init --ingress-enabled
      ```
      Yukarıdaki ilk kez yaptığınız gibi istemleri yanıtlayın. Ancak, Azure DNS ile yapılandırdığınız tüm etki alanı yolunu kullanarak yanıt vermeye yönelik bir sorunuz daha vardır.

6. Giriş için en üst düzey etki alanınızı girin (ör. draft.example.com): draft.squillace.io
7. Bu saati çağırdığınızda `draft up` , uygulamanızın (veya `curl` Bu) formun `<appname>.draft.<domain>.<top-level-domain>`URL 'sinde olduğunu görürsünüz. Bu örnekte, `http://handy-labradoodle.draft.squillace.io`. 
   ```bash
   curl -s http://handy-labradoodle.draft.squillace.io
   Hello World, I'm Java!
   ```


## <a name="next-steps"></a>Sonraki adımlar

Artık bir ACS Kubernetes kümeniz olduğuna göre, bu senaryonun daha fazla sayıda ve daha farklı dağıtımlarını oluşturmak için [Azure Container Registry](../../container-registry/container-registry-intro.md)’yi kullanarak araştırma yapabilirsiniz. Örneğin, belirli ACS dağıtımları için ayarları daha derin bir alt etki alanından denetleyen bir draft._basedomain.toplevel_ etki alanı DNS kayıt kümesi oluşturabilirsiniz.






