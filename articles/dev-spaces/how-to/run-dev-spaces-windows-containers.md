---
title: Windows kapsayıcıları ile etkileşim kurma
services: azure-dev-spaces
ms.date: 01/16/2020
ms.topic: conceptual
description: Windows kapsayıcıları ile mevcut bir kümede Azure Dev Spaces çalıştırmayı öğrenin
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Windows kapsayıcıları
ms.openlocfilehash: deee02bf13778950b934d87d02eb21068d89f323
ms.sourcegitcommit: 2ffa5bae1545c660d6f3b62f31c4efa69c1e957f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88077059"
---
# <a name="interact-with-windows-containers-using-azure-dev-spaces"></a>Azure Dev Spaces kullanarak Windows kapsayıcılarıyla etkileşim kurma

Hem yeni hem de mevcut Kubernetes ad alanlarında Azure Dev Spaces etkinleştirebilirsiniz. Azure Dev Spaces, Linux kapsayıcıları üzerinde çalışan hizmetleri çalıştıracak ve araç çalıştıracaktır. Bu hizmetler ayrıca aynı ad alanındaki Windows kapsayıcıları üzerinde çalışan uygulamalarla etkileşime geçebilir. Bu makalede, mevcut Windows kapsayıcılarıyla bir ad alanında hizmetleri çalıştırmak için geliştirme alanlarının nasıl kullanılacağı gösterilir. Şu anda Azure Dev Spaces ile hata ayıklamanıza veya Windows kapsayıcılarına iliştiremezsiniz.

## <a name="set-up-your-cluster"></a>Kümenizi ayarlama

Bu makalede, hem Linux hem de Windows düğüm havuzlarının bulunduğu bir kümeniz zaten var. Linux ve Windows düğüm havuzlarıyla bir küme oluşturmanız gerekiyorsa [buradaki][windows-container-cli]yönergeleri izleyebilirsiniz.

Kubernetes komut satırı istemcisi olan [kubectl][kubectl]kullanarak kümenize bağlanın. `kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnek çıktıda, hem Windows hem de Linux düğümü içeren bir küme gösterilmektedir. Devam etmeden önce durumun her *düğüm için olduğundan* emin olun.

```console
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmss000000   Ready    agent   13m    v1.14.8
aks-nodepool1-12345678-vmss000001   Ready    agent   13m    v1.14.8
aksnpwin000000                      Ready    agent   108s   v1.14.8
```

Windows düğümleriniz için bir [Taint][using-taints] uygulayın. Windows düğümlerinizin Taint 'i, geliştirme alanlarının Windows düğümleriniz üzerinde çalışması için Linux kapsayıcıları planlamasını önler. Aşağıdaki komut örnek komutu, önceki örnekteki *aksnpwin987654* Windows düğümüne bir taınt uygular.

```azurecli-interactive
kubectl taint node aksnpwin987654 sku=win-node:NoSchedule
```

> [!IMPORTANT]
> Bir düğüme bir Taint uyguladığınızda, hizmetinizi bu düğümde çalıştırmak için hizmetinizin dağıtım şablonunda eşleşen bir toleranation yapılandırmanız gerekir. Örnek uygulama, önceki komutta yapılandırdığınız taınt ile [eşleşen bir toleranation][sample-application-toleration-example] ile zaten yapılandırılmış.

## <a name="run-your-windows-service"></a>Windows hizmetinizi çalıştırın

Windows hizmetinizi AKS kümenizde çalıştırın ve *çalışır* durumda olduğunu doğrulayın. Bu makalede, kümenizde çalışan bir Windows ve Linux hizmetini göstermek için [örnek bir uygulama][sample-application] kullanılmaktadır.

GitHub 'dan örnek uygulamayı kopyalayın ve `dev-spaces/samples/existingWindowsBackend/mywebapi-windows` dizine gidin:

```console
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/existingWindowsBackend/mywebapi-windows
```

Örnek uygulama, Windows hizmetini kümenizde çalıştırmak için [Held 3][helm-installed] kullanır. `charts`Dizine gidin ve Held 'yi kullanarak Windows hizmetini çalıştırın:

```console
cd charts/
kubectl create ns dev
helm install windows-service . --namespace dev
```

Yukarıdaki komut, Windows hizmetinizi *geliştirme* ad alanında çalıştırmak Için Held kullanır. *Dev*adlı bir ad alanı yoksa, oluşturulur.

`kubectl get pods`Windows hizmetinizin kümenizde çalıştığını doğrulamak için komutunu kullanın. 

```console
$ kubectl get pods --namespace dev --watch
NAME                     READY   STATUS              RESTARTS   AGE
myapi-4b9667d123-1a2b3   0/1     ContainerCreating   0          47s
...
myapi-4b9667d123-1a2b3   1/1     Running             0          98s
```

## <a name="enable-azure-dev-spaces"></a>Azure Dev Spaces etkinleştir

Windows hizmetinizi çalıştırmak için kullandığınız ad alanında dev alanlarını etkinleştirin. Aşağıdaki komut *dev* ad alanındaki dev alanlarını sunar:

```console
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster --space dev --yes
```

## <a name="update-your-windows-service-for-dev-spaces"></a>Windows hizmetinizi geliştirme alanları için güncelleştirme

Zaten çalışmakta olan kapsayıcılarla mevcut bir ad alanında dev alanlarını etkinleştirdiğinizde, dev Spaces, bu ad alanında çalışan tüm yeni kapsayıcıları dener ve etkinleştirir. Geliştirme alanları aynı zamanda, ad alanında zaten çalışmakta olan hizmet için oluşturulan yeni kapsayıcıları da dener ve yeniden dener. Geliştirme boşlukların, ad Boşlukınızda çalışan bir kapsayıcıyı eklemesini engellemek için, *proxy olmayan* üst bilgisini öğesine ekleyin `deployment.yaml` .

`azds.io/no-proxy: "true"` `existingWindowsBackend/mywebapi-windows/charts/templates/deployment.yaml` Dosyaya ekleyin:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  ...
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    ...
  template:
    metadata:
      labels:
        app.kubernetes.io/name: {{ include "mywebapi.name" . }}
        app.kubernetes.io/instance: {{ .Release.Name }}
        azds.io/no-proxy: "true"
```

`helm list`Windows hizmetinizin dağıtımını listelemek için kullanın:

```cmd
$ helm list --namespace dev
NAME              REVISION  UPDATED                     STATUS      CHART           APP VERSION NAMESPACE
windows-service 1           Wed Jul 24 15:45:59 2019    DEPLOYED    mywebapi-0.1.0  1.0         dev  
```

Yukarıdaki örnekte, dağıtımınızın adı *Windows-Service*' dir. Aşağıdakileri kullanarak Windows hizmetinizi yeni yapılandırmayla güncelleştirin `helm upgrade` :

```cmd
helm upgrade windows-service . --namespace dev
```

Uygulamanızı güncelleştirmenizden sonra `deployment.yaml` dev Spaces, hizmetinizi denemeyecek ve bunları işaretlemecektir.

## <a name="run-your-linux-application-with-azure-dev-spaces"></a>Linux uygulamanızı Azure Dev Spaces ile çalıştırma

`webfrontend`Dizinine gidin ve `azds prep` `azds up` Linux uygulamanızı kümenizde çalıştırmak için ve komutlarını kullanın.

```console
cd ../../webfrontend-linux/
azds prep --enable-ingress
azds up
```

Bu `azds prep --enable-ingress` komut, uygulamanız Için Helu grafiğini ve Dockerfiles 'ı oluşturur.

> [!TIP]
> Projeniz için [Dockerfile ve HELI grafiği](../how-dev-spaces-works-prep.md#prepare-your-code) , kodunuzu derlemek ve çalıştırmak için Azure dev Spaces tarafından kullanılır, ancak projenin oluşturulup çalıştırıldığını değiştirmek istiyorsanız bu dosyaları değiştirebilirsiniz.

`azds up`Komut, hizmetinizi ad alanında çalıştırır.

```console
$ azds up
Using dev space 'dev' with target 'myAKSCluster'
Synchronizing files...4s
Installing Helm chart...11s
Waiting for container image build...6s
Building container image...
Step 1/12 : FROM mcr.microsoft.com/dotnet/core/sdk:2.2
...
Step 12/12 : ENTRYPOINT ["/bin/bash", "/entrypoint.sh"]
Built container image in 36s
Waiting for container...2s
Service 'webfrontend' port 'http' is available at http://dev.webfrontend.abcdef0123.eus.azds.io/
Service 'webfrontend' port 80 (http) is available via port forwarding at http://localhost:57648
```

Hizmetin çalışır durumda olduğunu, genel URL 'yi açarak, azds up komutunun çıktısında görüntülendiğini görebilirsiniz. Bu örnekte, genel URL olur `http://dev.webfrontend.abcdef0123.eus.azds.io/` . Bir tarayıcıda hizmete gidin ve üst kısımdaki *hakkında* ' ya tıklayın. Kapsayıcının kullandığı Windows sürümünü içeren *mywebapi* hizmetinden bir ileti görüntülendiğini doğrulayın.

![Mywebapi 'ten Windows sürümünü gösteren örnek uygulama](../media/run-dev-spaces-windows-containers/sample-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Dev Spaces birden çok kapsayıcı genelinde daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda kodunuzun farklı sürümleriyle veya dallarıyla çalışarak işbirliğine dayalı geliştirmeyi nasıl kolaylaştırabileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Dev Spaces 'de takım geliştirme][team-development-qs]

[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-installed]: https://helm.sh/docs/intro/install/
[sample-application]: https://github.com/Azure/dev-spaces/tree/master/samples/existingWindowsBackend
[sample-application-toleration-example]: https://github.com/Azure/dev-spaces/blob/master/samples/existingWindowsBackend/mywebapi-windows/charts/templates/deployment.yaml#L24-L27
[team-development-qs]: ../quickstart-team-development.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[using-taints]: ../../aks/use-multiple-node-pools.md#schedule-pods-using-taints-and-tolerations
[windows-container-cli]: ../../aks/windows-container-cli.md
