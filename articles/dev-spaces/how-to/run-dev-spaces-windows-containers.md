---
title: Windows kapsayıcıları ile etkileşim kurma
services: azure-dev-spaces
ms.date: 01/16/2020
ms.topic: conceptual
description: Windows kapsayıcıları ile varolan bir kümede Azure Geliştirme Alanları'nı nasıl çalıştırılacizle öğrenin
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Hizmeti, kapsayıcılar, Windows kapsayıcıları
ms.openlocfilehash: 0b3f221c9e62343a02ba8742e4cf988c7cf26c12
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240477"
---
# <a name="interact-with-windows-containers-using-azure-dev-spaces"></a>Azure Geliştirme Alanları'nı kullanarak Windows kapsayıcılarıyla etkileşimkurun

Azure Dev Alanları'nı hem yeni hem de varolan Kubernetes ad alanlarında etkinleştirebilirsiniz. Azure Dev Spaces, Linux kapsayıcılarında çalışan araçları ve enstrüman hizmetlerini çalıştıracaktır. Bu hizmetler, aynı ad alanında Windows kapsayıcılarında çalışan uygulamalarla da etkileşimkurabilir. Bu makalede, varolan Windows kapsayıcıları ile bir ad alanında hizmetleri çalıştırmak için Dev Spaces nasıl kullanılacağını gösterir. Şu anda, Azure Dev Spaces ile Windows kapsayıcılarını ayıklama veya ekleme yapamazsınız.

## <a name="set-up-your-cluster"></a>Kümenizi ayarlama

Bu makalede, linux ve Windows düğüm havuzları ile zaten bir küme var varsayar. Linux ve Windows düğüm havuzları içeren bir küme oluşturmanız gerekiyorsa, yönergeleri [buradan][windows-container-cli]takip edebilirsiniz.

[Kubectl][kubectl], Kubernetes komut satırı istemcisi kullanarak kümenize bağlanın. `kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnek çıktı, hem Windows hem de Linux düğümü olan bir kümeyi gösterir. Devam etmeden önce her düğüm için durumun *Hazır* olduğundan emin olun.

```console
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmss000000   Ready    agent   13m    v1.14.8
aks-nodepool1-12345678-vmss000001   Ready    agent   13m    v1.14.8
aksnpwin000000                      Ready    agent   108s   v1.14.8
```

Windows düğümlerinize bir [bozuk para][using-taints] uygulayın. Windows düğümlerinizdeki bozuk para, Dev Spaces'in Windows düğümlerinizde çalışacak Şekilde Linux kapsayıcılarını zamanlamasını engeller. Aşağıdaki komut örneği komutu önceki örnekten *aksnpwin987654* Windows düğümüne bir leke uygular.

```azurecli-interactive
kubectl taint node aksnpwin987654 sku=win-node:NoSchedule
```

> [!IMPORTANT]
> Düğüme bir bozuk leke uyguladığınızda, hizmetinizi bu düğümüzerinde çalıştırmak için hizmetinizin dağıtım şablonunda eşleşen bir tolerasyon yapılandırmanız gerekir. Örnek uygulama zaten önceki komutta yapılandırdığınız leke yle eşleşen bir [tolere][sample-application-toleration-example] sahip olarak yapılandırıldı.

## <a name="run-your-windows-service"></a>Windows hizmetinizi çalıştırın

Windows hizmetinizi AKS kümenizde çalıştırın ve *Çalışan* durumunda olduğunu doğrulayın. Bu makalede, kümenizde çalışan bir Windows ve Linux hizmeti göstermek için örnek bir [uygulama][sample-application] kullanır.

Örnek uygulamayı GitHub'dan klonla `dev-spaces/samples/existingWindowsBackend/mywebapi-windows` ve dizine gidin:

```console
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/existingWindowsBackend/mywebapi-windows
```

Örnek uygulama, kümenizdeki Windows hizmetini çalıştırmak için [Helm 3'u][helm-installed] kullanır. Dizin `charts` gidin ve Windows hizmetini çalıştırın Miğfer'i kullanın:

```console
cd charts/
kubectl create ns dev
helm install windows-service . --namespace dev
```

Yukarıdaki komut, Windows hizmetinizi *dev* ad alanında çalıştırmak için Helm'i kullanır. *Dev*adında bir ad alanınız yoksa, bu oluşturulur.

Windows `kubectl get pods` hizmetinizin kümenizde çalıştığını doğrulamak için komutu kullanın. 

```console
$ kubectl get pods --namespace dev --watch
NAME                     READY   STATUS              RESTARTS   AGE
myapi-4b9667d123-1a2b3   0/1     ContainerCreating   0          47s
...
myapi-4b9667d123-1a2b3   1/1     Running             0          98s
```

## <a name="enable-azure-dev-spaces"></a>Azure Dev Alanlarını Etkinleştir

Windows hizmetinizi çalıştırmak için kullandığınız ad alanında Dev Spaces'i etkinleştirin. Aşağıdaki komut *dev* ad alanında Dev Spaces sağlar:

```console
az aks use-dev-spaces -g myResourceGroup -n myAKSCluster --space dev --yes
```

## <a name="update-your-windows-service-for-dev-spaces"></a>Dev Spaces için Windows hizmetinizi güncelleştirin

Dev Spaces'i, varsayılan olarak, zaten çalışmakta olan kapsayıcılarla varolan bir ad alanında etkinleştirdiğinizde, Dev Spaces bu ad alanında çalışan yeni kapsayıcıları enstrüman etmeye çalışır. Dev Spaces ayrıca, ad alanında çalışan hizmet için oluşturulan yeni kapsayıcıları da deneyecek ve enstrüman olarak kullanacaktır. Dev Spaces'in ad alanınızda çalışan bir kapsayıcıyı enstrümantize etmesini `deployment.yaml`önlemek için, proxy *olmayan* üstbilginin adını ekleyin.

Dosyaya `azds.io/no-proxy: "true"` `existingWindowsBackend/mywebapi-windows/charts/templates/deployment.yaml` ekleyin:

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

Windows `helm list` hizmetiniz için dağıtımı listelemek için kullanın:

```cmd
$ helm list --namespace dev
NAME              REVISION  UPDATED                     STATUS      CHART           APP VERSION NAMESPACE
windows-service 1           Wed Jul 24 15:45:59 2019    DEPLOYED    mywebapi-0.1.0  1.0         dev  
```

Yukarıdaki örnekte, dağıtımınızın adı *windows hizmetidir.* Windows hizmetinizi yeni yapılandırma `helm upgrade`ile güncelleyin:

```cmd
helm upgrade windows-service . --namespace dev
```

Geliştirme Spaces'inizi `deployment.yaml`güncellediğiniz için, hizmetinizi sağlamaya çalışmaz ve enstrüman verir.

## <a name="run-your-linux-application-with-azure-dev-spaces"></a>Azure Dev Spaces ile Linux uygulamanızı çalıştırın

Dizine `webfrontend` gidin ve Linux `azds prep` `azds up` uygulamanızı kümenizde çalıştırmak için ve komutları kullanın.

```console
cd ../../webfrontend-linux/
azds prep --enable-ingress
azds up
```

Komut, `azds prep --enable-ingress` uygulamanız için Helm grafiği ni ve Dockerfiles'i oluşturur.

> [!TIP]
> Projenizin [Dockerfile ve Helm grafiği,](../how-dev-spaces-works-prep.md#prepare-your-code) kodunuzu oluşturmak ve çalıştırmak için Azure Dev Spaces tarafından kullanılır, ancak projenin oluşturulma ve çalışma şeklini değiştirmek isterseniz bu dosyaları değiştirebilirsiniz.

Komut, `azds up` hizmetinizi ad alanında çalıştırın.

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

Azds up komutundan çıktıda görüntülenen genel URL'yi açarak hizmetin çalıştığını görebilirsiniz. Bu örnekte, genel `http://dev.webfrontend.abcdef0123.eus.azds.io/`URL. Bir tarayıcıda servise gidin ve en üstte *Hakkında'ya* tıklayın. *Mywebapi* hizmetinden kapsayıcının kullandığı Windows sürümünü içeren bir ileti gördüğünüzü doğrulayın.

![Mywebapi'den Windows sürümünü gösteren örnek uygulama](../media/run-dev-spaces-windows-containers/sample-app.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Geliştirme Alanları'nın birden çok kapsayıcıda daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olduğunu ve farklı alanlarda farklı sürümlerle veya kod dallarıyla çalışarak ortak geliştirmeyi nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanlarında ekip geliştirme][team-development-qs]

[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-installed]: https://helm.sh/docs/intro/install/
[sample-application]: https://github.com/Azure/dev-spaces/tree/master/samples/existingWindowsBackend
[sample-application-toleration-example]: https://github.com/Azure/dev-spaces/blob/master/samples/existingWindowsBackend/mywebapi-windows/charts/templates/deployment.yaml#L24-L27
[team-development-qs]: ../quickstart-team-development.md
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[team-development]: ../team-development-netcore.md
[using-taints]: ../../aks/use-multiple-node-pools.md#schedule-pods-using-taints-and-tolerations
[windows-container-cli]: ../../aks/windows-container-cli.md
