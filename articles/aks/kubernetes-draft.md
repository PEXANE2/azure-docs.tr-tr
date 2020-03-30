---
title: Taslak ile Azure Kubernetes Hizmetinde (AKS) geliştirin
description: TASLAK'ı AKS ve Azure Konteyner Kayıt Defteri ile kullanma
services: container-service
author: zr-msft
ms.topic: article
ms.date: 06/20/2019
ms.author: zarhoads
ms.openlocfilehash: b03256ee65a3c40d8a64d70b877c49e44e68f822
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595230"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-draft"></a>Quickstart: Taslak ile Azure Kubernetes Hizmeti (AKS) geliştirme

Draft, bir Kubernetes kümesinde uygulama kapsayıcılarının paketlenmesine ve çalıştırılabına yardımcı olan açık kaynak bir araçtır. Taslak ile, sürüm denetiminde değişikliklerinizi işlemek zorunda kalmadan kod değişiklikleri oluştuğunda bir uygulamayı Kubernetes'e hızla yeniden dağıtabilirsiniz. Taslak hakkında daha fazla bilgi için [GitHub'daki Taslak belgelere][draft-documentation]bakın.

Bu makalede, AKS'de bir uygulamayı paketlemek ve çalıştırmak için Taslak'ı nasıl kullanacağınızı gösterir.


## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
* Docker yüklendi ve yapılandırıldı. Docker [Mac][docker-for-mac], [Windows][docker-for-windows] veya [Linux][docker-for-linux] sisteminde Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.
* [Helm v2 yüklü][helm-install].
* [Taslak yüklendi.][draft-documentation]

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Hizmet kümesi oluşturma

Bir AKS kümesi oluşturun. Aşağıdaki komutlar MyResourceGroup adında bir kaynak grubu ve MyAKS adında bir AKS kümesi oluşturur.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --generate-ssh-keys
```

## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma
Uygulamanızı AKS kümenizde çalıştırmak için Taslak'ı kullanmak için kapsayıcı resimlerinizi depolamak için bir Azure Kapsayıcı Kayıt Defteri gerekir. Aşağıdaki örnekte, *Temel* SKU ile *MyResourceGroup* kaynak grubunda *MyDraftACR* adlı bir ACR oluşturmak için [az acr oluşturma][az-acr-create] kullanır. Kendi benzersiz kayıt defteri adınızı sağlamanız gerekir. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. *Temel* SKU, geliştirme amaçlı dağıtımlar için uygun maliyetli, depolama ve aktarım hızı açısından dengeli bir giriş noktasıdır.

```azurecli
az acr create --resource-group MyResourceGroup --name MyDraftACR --sku Basic
```

Çıktı aşağıdaki örneğe benzerdir. Daha sonraki bir adımda kullanılacağı için ACR'niz için *girişServer* değerine dikkat edin. Aşağıdaki örnekte, *mydraftacr.azurecr.io* *MyDraftACR*için *girişServer* olduğunu.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2019-06-11T13:35:17.998425+00:00",
  "id": "/subscriptions/<ID>/resourceGroups/MyResourceGroup/providers/Microsoft.ContainerRegistry/registries/MyDraftACR",
  "location": "eastus",
  "loginServer": "mydraftacr.azurecr.io",
  "name": "MyDraftACR",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
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


Draft'ın ACR örneğini kullanabilmesi için önce oturum açmanız gerekir. Oturum açmak için [az acr giriş][az-acr-login] komutunu kullanın. Aşağıdaki örnek, *MyDraftACR*adlı bir ACR'de oturum açacaktır.

```azurecli
az acr login --name MyDraftACR
```

Komut tamamlandıktan sonra *Oturum Açma Başarılı* iletisini döndürür.

## <a name="create-trust-between-aks-cluster-and-acr"></a>AKS kümesi ve ACR arasında güven oluşturma

AKS kümenizin, kapsayıcı görüntüleri çekmek ve çalıştırmak için ACR'nize erişmesi de gerekir. Bir güven oluşturarak AKS'den ACR'ye erişime izin verirsiniz. AKS kümesi ile ACR kayıt defteri arasında güven oluşturmak için, AKS kümesi tarafından kullanılan Azure Etkin Dizin hizmet ilkesi için ACR kayıt defterine erişmek için izinler tanıyın. Aşağıdaki komutlar *MyResourceGroup'taki* *MyAKS* kümesinin hizmet sorumlusuna *MyResourceGroup'taki* *MyDraftACR* ACR'ye izin verir.

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group MyResourceGroup --name MyAKS --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group MyResourceGroup --name MyDraftACR --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

## <a name="connect-to-your-aks-cluster"></a>AKS kümenize bağlanın

Yerel bilgisayarınızdan Kubernetes kümesine bağlanmak için Kubernetes’in komut satırı istemcisini ([kubectl][kubectl]) kullanmanız gerekir.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. [az aks install-cli][] komutunu kullanarak da yerel ortama yükleyebilirsiniz:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][] komutunu kullanın. Aşağıdaki örnek, *MyResourceGroup'ta* *MyAKS* adlı AKS kümesi için kimlik bilgilerini alır:

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

## <a name="create-a-service-account-for-helm"></a>Helm için bir hizmet hesabı oluşturma

Helm'i RBAC özellikli bir AKS kümesine dağıtmadan önce, Tiller hizmeti için bir hizmet hesabı ve rol bağlama gerekir. Bir RBAC etkin kümede Helm / Tiller güvenliğini sağlama hakkında daha fazla bilgi için Bkz. [Tiller, Namespaces ve RBAC.][tiller-rbac] AKS kümeniz RBAC etkin değilse, bu adımı atlayın.

Adlandırılmış `helm-rbac.yaml` bir dosya oluşturun ve aşağıdaki YAML'de kopyalayın:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Hizmet hesabını oluşturun ve komutla rol bağlama: `kubectl apply`

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="configure-helm"></a>Helm'i Yapılandır
Temel bir Çapa Makinesi'ni bir AKS kümesine dağıtmak için [dümen init][helm-init] komutunu kullanın. Kümeniz RBAC etkin değilse, bağımsız `--service-account` değişkeni ve değeri kaldırın.

```console
helm init --service-account tiller --node-selectors "beta.kubernetes.io/os"="linux"
```

## <a name="configure-draft"></a>Taslağı Yapılandır

Taslak'ı yerel makinenizde yapılandırmadıysanız, `draft init`çalıştırın:

```console
$ draft init
Installing default plugins...
Installation of default plugins complete
Installing default pack repositories...
...
Happy Sailing!
```

Ayrıca, ACR'nizin *giriş Sunucusu'nu* kullanmak için Draft'ı yapılandırmanız gerekir. Aşağıdaki komut, `draft config set` kayıt `mydraftacr.azurecr.io` defteri olarak kullanmak için kullanır.

```console
draft config set registry mydraftacr.azurecr.io
```

Taslak'ı ACR'nizi kullanacak şekilde yapılandırıldınız ve Draft kapsayıcı görüntülerini ACR'nize itebilir. Draft, UYGULAMANIZI AKS kümenizde çalıştırdığında, ACR kayıt defterine itmek veya ondan çekmek için hiçbir parola veya sır gerekmez. AKS kümeniz ile ACR'niz arasında bir güven oluşturulduğundan, kimlik doğrulaması Azure Etkin Dizini'ni kullanarak Azure Kaynak Yöneticisi düzeyinde gerçekleşir.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı [başlatma, Draft GitHub deposundan örnek bir Java uygulaması][example-java]kullanır. Uygulamayı GitHub'dan klonla ve `draft/examples/example-java/` dizine gidin.

```console
git clone https://github.com/Azure/draft
cd draft/examples/example-java/
```

## <a name="run-the-sample-application-with-draft"></a>Örnek uygulamayı Taslak ile çalıştırma

Uygulamayı `draft create` hazırlamak için komutu kullanın.

```console
draft create
```

Bu komut, uygulamayı bir Kubernetes kümesinde çalıştırmak için kullanılan yapıları oluşturur. Bu öğeler arasında Dockerfile, Helm grafiği ve Taslak yapılandırma dosyası olan *draft.toml* dosyası yer almaktadır.

```console
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

AKS kümenizdeki örnek uygulamayı çalıştırmak için `draft up` komutu kullanın.

```console
draft up
```

Bu komut, bir kapsayıcı görüntüsü oluşturmak için Dockerfile oluşturur, acr için görüntü iter ve AKS uygulama başlatmak için Helm grafik yükler. Bu komutu ilk çalıştırdığınızda, kapsayıcı görüntüsünü itme niz ve çekerek biraz zaman alabilir. Taban katmanları önbelleğe alındıktan sonra, uygulamayı dağıtmak için gereken süre önemli ölçüde azalır.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

## <a name="connect-to-the-running-sample-application-from-your-local-machine"></a>Yerel makinenizden çalışan numune uygulamasına bağlanma

Uygulamayı test etmek için `draft connect` komutu kullanın.

```console
draft connect
```

Bu komut, Kubernetes bölmesine güvenli bir bağlantı sağlar. Tamamlandığında, uygulamaya sağlanan URL'den erişilebilir.

```console
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Örnek uygulamayı görmek için URL'yi `localhost` kullanarak bir tarayıcıdaki uygulamaya gidin. Yukarıdaki örnekte, URL. `http://localhost:49804` Bağlantıyı kullanarak `Ctrl+c`durdurun.

## <a name="access-the-application-on-the-internet"></a>Uygulamaya internetüzerinden erişin

Önceki adım, AKS kümenizdeki uygulama bölmesine proxy bağlantısı oluşturdu. Uygulamanızı geliştirirken ve test ettikçe, uygulamayı internet üzerinden kullanılabilir hale getirmek isteyebilirsiniz. Internet üzerinde bir uygulama ortaya çıkarmak için, [LoadBalancer][kubernetes-service-loadbalancer]türü ile bir Kubernetes hizmeti oluşturabilirsiniz.

`charts/example-java/values.yaml` *LoadBalancer* hizmeti oluşturmak için güncelleştirin. *Service.type* değerini *ClusterIP'den* *LoadBalancer'a*değiştirin.

```yaml
...
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
...
```

Değişikliklerinizi kaydedin, dosyayı `draft up` kapatın ve uygulamayı yeniden çalıştırmak için çalıştırın.

```console
draft up
```

Hizmetin genel bir IP adresini döndürmesi birkaç dakika sürer. İlerlemeyi izlemek için `kubectl get service` *saat* parametresi ile komutu kullanın:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Örnek uygulamayı görmek için *EXTERNAL-IP'yi* kullanarak bir tarayıcıda uygulamanızın yük dengeleyicisine gidin. Yukarıdaki örnekte, IP `52.175.224.118`.

## <a name="iterate-on-the-application"></a>Uygulama üzerinde iterate

Yerel değişiklikler yaparak ve yeniden çalıştırarak `draft up`uygulamanızı yineleyebilirsiniz.

[src/main/java/helloworld/Hello.java hattının 7 satırında][example-java-hello-l7] döndürülen iletiyi güncelleştirin

```java
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
```

Uygulamayı `draft up` yeniden dağıtmak için komutu çalıştırın:

```console
$ draft up

Draft Up Started: 'example-java': 01CMZC9RF0TZT7XPWGFCJE15X4
example-java: Building Docker Image: SUCCESS ⚓  (25.0202s)
example-java: Pushing Docker Image: SUCCESS ⚓  (7.1457s)
example-java: Releasing Application: SUCCESS ⚓  (3.5773s)
Inspect the logs with `draft logs 01CMZC9RF0TZT7XPWGFCJE15X4`
```

Güncelleştirilmiş uygulamayı görmek için yük bakiyecinizin IP adresine tekrar gidin ve değişikliklerinizin göründüğünü doğrulayın.

## <a name="delete-the-cluster"></a>Küme silme

Kümeye artık gerek kalmadığında, kaynak grubunu, AKS kümesini, kapsayıcı kayıt defterini, orada depolanan kapsayıcı görüntülerini ve ilgili tüm kaynakları kaldırmak için [az grubu silme][az-group-delete] komutunu kullanın.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete].

## <a name="next-steps"></a>Sonraki adımlar

Taslak'ı kullanma hakkında daha fazla bilgi için GitHub'daki Taslak belgelerine bakın.

> [!div class="nextstepaction"]
> [Taslak belgeler][draft-documentation]


[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-create]: /cli/azure/acr#az-acr-login
[az-group-delete]: /cli/azure/group#az-group-delete
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[kubernetes-ingress]: ./ingress-basic.md

[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[example-java]: https://github.com/Azure/draft/tree/master/examples/example-java
[example-java-hello-l7]: https://github.com/Azure/draft/blob/master/examples/example-java/src/main/java/helloworld/Hello.java#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[helm-init]: https://v2.helm.sh/docs/helm/#helm-init
[helm-install]: https://v2.helm.sh/docs/using_helm/#installing-helm
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[tiller-rbac]: https://v2.helm.sh/docs/using_helm/#tiller-namespaces-and-rbac
