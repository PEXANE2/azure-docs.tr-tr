---
title: Önizleme - Azure Kubernetes Hizmeti (AKS) kümesinde Windows Server kapsayıcısı oluşturma
description: Azure CLI'yi kullanarak Azure Kubernetes Hizmeti'ndeki (AKS) bir Windows Server kapsayıcısında uygulamayı nasıl hızla oluşturabilirsiniz ve bir Uygulamayı nasıl hızla oluşturabilirsiniz öğrenin.
services: container-service
ms.topic: article
ms.date: 01/27/2020
ms.openlocfilehash: 259728da5ea7f71110ce183ae25bb47a0f873614
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475519"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Önizleme - Azure CLI'yi kullanarak Azure Kubernetes Hizmeti (AKS) kümesinde Windows Server kapsayıcısı oluşturma

Azure Kubernetes Service (AKS), kümeleri hızla dağıtmanızı ve yönetmenize olanak tanıyan yönetilen bir Kubernetes hizmetidir. Bu makalede, Azure CLI'yi kullanarak bir AKS kümesi dağıtMış olursunuz. Ayrıca, bir Windows Server kapsayıcısındaki ASP.NET örnek bir uygulamayı kümeye dağıtırsınız.

Bu özellik şu anda önizleme sürümündedir.

![Örnek ASP.NET uygulamaya tarama görüntüsü](media/windows-container/asp-net-sample-app.png)

Bu makalede, Kubernetes kavramlarıtemel bir anlayış varsayar. Daha fazla bilgi için Azure [Kubernetes Hizmeti (AKS) için Kubernetes temel kavramlarına][kubernetes-concepts]bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.61 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="before-you-begin"></a>Başlamadan önce

Kümenizi oluşturduktan sonra Windows Server kapsayıcılarını çalıştırabilen ek bir düğüm havuzu eklemeniz gerekir. Ek bir düğüm havuzu ekleme daha sonraki bir adımda ele alınmıştır, ancak önce birkaç önizleme özelliğini etkinleştirmeniz gerekir.

> [!IMPORTANT]
> AKS önizleme özellikleri self servis tercihidir. Önizlemeler "olduğu gibi" ve "mevcut olduğu gibi" sağlanır ve hizmet düzeyi anlaşmaları ve sınırlı garanti dışında dır. AKS Önizlemeler kısmen en iyi çaba temelinde müşteri desteği tarafından karşılanır. Bu nedenle, bu özellikler üretim kullanımı için değildir. Ek infromation için lütfen aşağıdaki destek makalelerini görmek için:
>
> * [AKS Destek Politikaları][aks-support-policies]
> * [Azure Destek SSS][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Aks-preview CLI uzantısını yükleyin

Windows Server kapsayıcılarını kullanmak için *aks önizleme* CLI uzantısı 0.4.12 veya daha yüksek olması gerekir. [az uzantı ekle][az-extension-add] komutunu kullanarak *aks önizleme* Azure CLI uzantısını yükleyin ve az [uzantı güncelleştirme][az-extension-update] komutunu kullanarak kullanılabilir güncelleştirmeleri denetleyin::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-windows-preview-feature"></a>Windows önizleme özelliğini kaydedin

Birden çok düğüm havuzu kullanabilen ve Windows Server kapsayıcılarını çalıştırabilen bir AKS kümesi oluşturmak için, önce aboneliğinizdeki *WindowsPreview* özellik bayraklarını etkinleştirin. *WindowsPreview* özelliği, Kubernetes düğümlerinin dağıtımını ve yapılandırmasını yönetmek için çok düğümlü havuz kümeleri ve sanal makine ölçeği kümesi de kullanır. Aşağıdaki örnekte gösterildiği gibi [az özellik kayıt][az-feature-register] komutunu kullanarak *WindowsPreview* özellik bayrağını kaydedin:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> *WindowsPreview* özellik bayrağını başarıyla kaydettikten sonra oluşturduğunuz herhangi bir AKS kümesi bu önizleme kümesi deneyimini kullanır. Düzenli, tam destekli kümeler oluşturmaya devam etmek için, üretim aboneliklerinde önizleme özelliklerini etkinleştirme. Önizleme özelliklerini test etmek için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Kaydın tamamlanması birkaç dakika sürer. [az özellik listesi][az-feature-list] komutunu kullanarak kayıt durumunu kontrol edin:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Kayıt durumu olduğunda, `Registered`durumu izlemeyi durdurmak için Ctrl-C tuşuna basın.  Ardından, [az sağlayıcı kayıt][az-provider-register] komutunu kullanarak *Microsoft.ContainerService* kaynak sağlayıcısının kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Sınırlamalar

Birden çok düğüm havuzunu destekleyen AKS kümeleri oluştururken ve yönetirken aşağıdaki sınırlamalar geçerlidir:

* İlk düğüm havuzunu silemezsiniz.

Bu özellik önizlemede yken, aşağıdaki ek sınırlamalar geçerlidir:

* AKS kümesien fazla sekiz düğüm havuzuna sahip olabilir.
* AKS kümesi, bu sekiz düğüm havuzunda en fazla 400 düğüme sahip olabilir.
* Windows Server düğüm havuzu adı 6 karakter sınırı vardır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure'da çalıştığı yerdir. az grubu oluşturma komutunu kullanarak bir kaynak grubu [oluşturun.][az-group-create]

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

> [!NOTE]
> Bu makalede, bu öğreticikomutları için Bash sözdizimini kullanır.
> Azure Bulut Su Şuru kullanıyorsanız, Bulut Kabuğu penceresinin sol üst kısmındaki açılır pencerenin **Bash**olarak ayarlandığından emin olun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Aşağıdaki örnek çıktı, başarıyla oluşturulan kaynak grubunu gösterir:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-an-aks-cluster"></a>AKS kümesi oluşturma

Windows Server kapsayıcıları için düğüm havuzlarını destekleyen bir AKS kümesini çalıştırmak için kümenizin [Azure CNI][azure-cni-about] (gelişmiş) ağ eklentisi kullanan bir ağ ilkesi kullanması gerekir. Gerekli alt ağ aralıklarını ve ağ hususlarını planlamaya yardımcı olmak için daha ayrıntılı bilgi için Azure [CNI ağlarını yapılandırma][use-advanced-networking]konusuna bakın. *myAKSCluster*adlı bir AKS kümesi oluşturmak için [az aks oluşturma][az-aks-create] komutunu kullanın. Bu komut, yoksa gerekli ağ kaynaklarını oluşturur.
  * Küme iki düğüm le yapılandırılır
  * *Windows-admin-password* ve *windows-admin-kullanıcı adı parametreleri* kümede oluşturulan windows server kapsayıcıları için yönetici kimlik bilgilerini ayarlar.

> [!NOTE]
> Kümenizin güvenilir çalışmasını sağlamak için varsayılan düğüm havuzunda en az 2 (iki) düğüm çalıştırmanız gerekir.

Kendi güvenli *PASSWORD_WIN* sağlayın (bu makaledeki komutların bir BASH kabuğuna girildiğini unutmayın):

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --enable-addons monitoring \
    --kubernetes-version 1.15.7 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --network-plugin azure
```

> [!Note]
> Parola doğrulama hatası alırsanız, kaynak grubunuzu başka bir bölgede oluşturmayı deneyin.
> Ardından, yeni kaynak grubuyla küme oluşturmayı deneyin.

> [!Note]
> Sürüm bu bölgede desteklenmediği için AKS kümesini oluşturamıyorsanız, bu bölge için desteklenen sürüm listesini bulmak için [az aks get-versions --location eastus] komutunu kullanabilirsiniz.


Birkaç dakika sonra komut tamamlar ve küme hakkında JSON biçimlendirilmiş bilgileri döndürür. Bazen kümenin sağlanması birkaç dakikadan uzun sürebilir. Bu gibi durumlarda en fazla 10 dakika bekleyin. 

## <a name="add-a-windows-server-node-pool"></a>Windows Server düğüm havuzu ekleme

Varsayılan olarak, Linux kapsayıcıları çalıştırabilen bir düğüm havuzu ile bir AKS kümesi oluşturulur. Windows `az aks nodepool add` Server kapsayıcılarını çalıştırabilen ek bir düğüm havuzu eklemek için komutu kullanın.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.15.7
```

Yukarıdaki komut *npwin* adında yeni bir düğüm havuzu oluşturur ve *myAKSCluster'a*ekler. Windows Server kapsayıcılarını çalıştırmak için bir düğüm havuzu oluştururken, *düğüm vm boyutu* için varsayılan değer *Standard_D2s_v3.* *Düğüm-vm boyutu* parametresini ayarlamayı seçerseniz, lütfen [kısıtlı VM boyutlarının][restricted-vm-sizes]listesini kontrol edin. Önerilen minimum boyut *Standard_D2s_v3.* Yukarıdaki komut, çalışırken `az aks create`oluşturulan varsayılan vnet'te varsayılan alt net'i de kullanır.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bir Kubernetes kümesini yönetmek için [kubectl][kubectl], Kubernetes komut satırı istemcisi kullanırsınız. Azure Bulut Su Şur'u kullanıyorsanız, `kubectl` zaten yüklenmiş. Yerel `kubectl` olarak yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut kimlik bilgilerini karşıdan yükler ve Kubernetes CLI'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnek çıktı kümedeki tüm düğümleri gösterir. Tüm düğümlerin durumunun *Hazır*olduğundan emin olun:

```output
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.15.7
aksnpwin987654                      Ready    agent   108s   v1.15.7
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Kubernetes bildirim dosyası, küme için hangi kapsayıcı görüntülerinin çalıştırılması gibi istenen durumu tanımlar. Bu makalede, bir Windows Server kapsayıcısında ASP.NET örnek uygulama çalıştırmak için gereken tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirim, ASP.NET örnek uygulama için bir [Kubernetes dağıtımı][kubernetes-deployment] ve internetten uygulamaya erişmek için harici bir [Kubernetes hizmetini][kubernetes-service] içerir.

ASP.NET örnek uygulama [.NET Framework Samples'in][dotnet-samples] bir parçası olarak sağlanır ve bir Windows Server kapsayıcısında çalışır. AKS, Windows Server kapsayıcılarının *Windows Server 2019* veya daha büyük görüntülerine dayanmasını gerektirir. Kubernetes bildirim dosyası, AKS kümenize Windows Server kapsayıcılarını çalıştırabilen bir düğüm üzerinde ASP.NET örnek uygulama bölmenizi çalıştırmasını söylemek için bir [düğüm seçici][node-selector] de tanımlamalıdır.

Adlandırılmış `sample.yaml` bir dosya oluşturun ve aşağıdaki YAML tanımında kopyalayın. Azure Bulut Kabuğu'nu kullanıyorsanız, bu `vi` dosya `nano` sanal veya fiziksel bir sistem üzerinde çalışıyormuş gibi oluşturulabilir:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample
  labels:
    app: sample
spec:
  replicas: 1
  template:
    metadata:
      name: sample
      labels:
        app: sample
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": windows
      containers:
      - name: sample
        image: mcr.microsoft.com/dotnet/framework/samples:aspnetapp
        resources:
          limits:
            cpu: 1
            memory: 800M
          requests:
            cpu: .1
            memory: 300M
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sample
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: sample
```

[Kubectl uygula][kubectl-apply] komutunu kullanarak uygulamayı dağıtın ve YAML manifestonuzun adını belirtin:

```console
kubectl apply -f sample.yaml
```

Aşağıdaki örnek çıktı, başarıyla oluşturulan Dağıtım ve Hizmet'i gösterir:

```output
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında, bir Kubernetes hizmeti uygulamanın ön ucunu internete maruz bırakır. Bu işlemin tamamlanması birkaç dakika sürebilir. Bazen hizmetin sağlanması birkaç dakikadan uzun sürebilir. Bu gibi durumlarda en fazla 10 dakika bekleyin.

İlerlemeyi izlemek için, bağımsız değişkenle birlikte `--watch` [kubectl get service][kubectl-get] komutunu kullanın.

```console
kubectl get service sample --watch
```

Başlangıçta *örnek* hizmetin *external-IP* *beklemede*olarak gösterilir.

```output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*EXTERNAL-IP* adresi *beklemeden* gerçek bir genel IP `CTRL-C` adresine `kubectl` değiştiğinde, izleme işlemini durdurmak için kullanın. Aşağıdaki örnek çıktı, hizmete atanan geçerli bir genel IP adresini gösterir:

```output
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Örnek uygulamayı iş başında görmek için, hizmetin harici IP adresine bir web tarayıcısı açın.

![Örnek ASP.NET uygulamaya tarama görüntüsü](media/windows-container/asp-net-sample-app.png)

> [!Note]
> Sayfayı yüklemeye çalışırken bir bağlantı zaman aşımı alırsanız, örnek uygulamanın aşağıdaki komutla hazır olduğunu doğrulamanız gerekir [kubectl get pods --watch]. Bazen windows kapsayıcısı harici IP adresiniz kullanılabilir olduğunda başlatılacaktır.

## <a name="delete-cluster"></a>Kümeyi silme

Kümeye artık ihtiyacınız yoksa [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete].

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Kubernetes kümesi ni dağıttınız ve windows server kapsayıcısında ASP.NET bir örnek uygulama dağıttınız. Az önce oluşturduğunuz küme [için Kubernetes web panosuna erişin.][kubernetes-dashboard]

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS eğitimi][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-provider-register]: /cli/azure/provider#az-provider-register
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cni-about]: concepts-network.md#azure-cni-advanced-networking
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[restricted-vm-sizes]: quotas-skus-regions.md#restricted-vm-sizes
[use-advanced-networking]: configure-advanced-networking.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
