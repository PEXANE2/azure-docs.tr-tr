---
title: Azure Kubernetes Service (AKS) kümesinde Windows Server kapsayıcısı oluşturma
description: Azure CLı kullanarak Azure Kubernetes Service (AKS) içindeki bir Windows Server kapsayıcısında bir uygulama dağıtma ve bir Kubernetes kümesini hızlı bir şekilde oluşturmayı öğrenin.
services: container-service
ms.topic: article
ms.date: 05/06/2020
ms.openlocfilehash: 70dbe927c3d106e6a853f215f93c51bd7e547150
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84658508"
---
# <a name="create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Azure CLı kullanarak Azure Kubernetes Service (AKS) kümesinde Windows Server kapsayıcısı oluşturma

Azure Kubernetes hizmeti (AKS), kümelerinizi hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. Bu makalede, Azure CLı kullanarak bir AKS kümesi dağıtırsınız. Ayrıca, bir Windows Server kapsayıcısında kümeye bir ASP.NET örnek uygulaması da dağıtabilirsiniz.

![ASP.NET örnek uygulamasına göz atma görüntüsü](media/windows-container/asp-net-sample-app.png)

Bu makalede, Kubernetes kavramlarının temel bir şekilde anlaşıldığı varsayılır. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

### <a name="limitations"></a>Sınırlamalar

Birden çok düğüm havuzunu destekleyen AKS kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* İlk düğüm havuzunu silemezsiniz.

Windows Server düğüm havuzları için aşağıdaki ek sınırlamalar geçerlidir:

* AKS kümesinde en fazla 10 düğüm havuzu olabilir.
* AKS kümesi, her düğüm havuzunda en fazla 100 düğüme sahip olabilir.
* Windows Server düğüm havuzu adının 6 karakterlik bir sınırı vardır.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği mantıksal bir gruptur. Bir kaynak grubu oluştururken konum belirtmeniz istenir. Bu konum, kaynak grubu meta verilerinin depolandığı yerdir, kaynak oluşturma sırasında başka bir bölge belirtmezseniz kaynaklarınızın Azure 'da da çalıştığı yerdir. [Az Group Create][az-group-create] komutunu kullanarak bir kaynak grubu oluşturun.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

> [!NOTE]
> Bu makalede, bu öğreticideki komutlar için bash sözdizimi kullanılmaktadır.
> Azure Cloud Shell kullanıyorsanız, Cloud Shell penceresinin sol üst kısmındaki açılan listenin **Bash**olarak ayarlandığından emin olun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Aşağıdaki örnek çıktıda başarıyla oluşturulan kaynak grubu gösterilmektedir:

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

Windows Server kapsayıcıları için düğüm havuzlarını destekleyen bir AKS kümesi çalıştırmak için, kümenizin [Azure CNI][azure-cni-about] (Gelişmiş) ağ eklentisini kullanan bir ağ ilkesi kullanması gerekir. Gerekli alt ağ aralıklarını ve ağ konularını planlamaya yardımcı olacak daha ayrıntılı bilgi için bkz. [Azure CNI ağını yapılandırma][use-advanced-networking]. *Myakscluster*adlı bir aks kümesi oluşturmak için [az aks Create][az-aks-create] komutunu kullanın. Mevcut değilse, bu komut gerekli ağ kaynaklarını oluşturur.

* Küme iki düğüm ile yapılandırılmış
* *Windows-Admin-Password* ve *Windows-admin-username* parametreleri, kümede oluşturulan herhangi bir Windows Server kapsayıcısı için yönetici kimlik bilgilerini ayarlar.
* Düğüm havuzu şunu kullanır`VirtualMachineScaleSets`

> [!NOTE]
> Kümenizin güvenilir bir şekilde çalışmasını sağlamak için varsayılan düğüm havuzunda en az 2 (iki) düğüm çalıştırmanız gerekir.

Kendi güvenli *PASSWORD_WIN* sağlayın (Bu makaledeki komutların bash kabuğu 'na girildiğini unutmayın):

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 2 \
    --enable-addons monitoring \
    --kubernetes-version 1.16.9 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --vm-set-type VirtualMachineScaleSets \
    --network-plugin azure
```

> [!NOTE]
> Sürüm bu bölgede desteklenmediğinden AKS kümesini oluşturamadığı takdirde, bu bölgeye yönelik desteklenen sürüm listesini bulmak için [az aks get-versions--location eastus] komutunu kullanabilirsiniz.
>  
> Parola doğrulama hatası alırsanız, kaynak grubunuzu başka bir bölgede oluşturmayı deneyin.
> Ardından yeni kaynak grubuyla kümeyi oluşturmayı deneyin.

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON biçimli bilgileri döndürür. Bazen kümenin sağlanması birkaç dakikadan uzun sürebilir. Bu durumlarda en fazla 10 dakika bekleyin.

## <a name="add-a-windows-server-node-pool"></a>Windows Server düğüm Havuzu Ekle

Varsayılan olarak, bir AKS kümesi, Linux kapsayıcıları çalıştırabilirler bir düğüm havuzuyla oluşturulur. `az aks nodepool add`Linux düğüm havuzunun yanı sıra Windows Server kapsayıcıları çalıştırabilirler ek bir düğüm havuzu eklemek için komutunu kullanın.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.16.9
```

Yukarıdaki komut, *npwin* adlı yeni bir düğüm havuzu oluşturur ve bunu *Myakscluster*öğesine ekler. Windows Server kapsayıcıları çalıştırmak için bir düğüm havuzu oluştururken, *düğüm-VM-boyutu* için varsayılan değer *Standard_D2s_v3*. *Düğüm-VM-boyut* parametresini ayarlamayı seçerseniz, lütfen [kısıtlı VM boyutlarının][restricted-vm-sizes]listesini kontrol edin. Önerilen en düşük boyut *Standard_D2s_v3*. Yukarıdaki komut, çalışırken oluşturulan varsayılan VNET 'teki varsayılan alt ağı da kullanır `az aks create` .

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. Azure Cloud Shell kullanıyorsanız, `kubectl` zaten yüklüdür. `kubectl`Yerel olarak yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

`kubectl` istemcisini Kubernetes kümenize bağlanacak şekilde yapılandırmak için [az aks get-credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```console
kubectl get nodes
```

Aşağıdaki örnek çıktı kümedeki tüm düğümleri gösterir. Tüm düğümlerin *durumunun olduğundan emin olun:*

```output
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.16.9
aksnpwin987654                      Ready    agent   108s   v1.16.9
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Kubernetes bildirim dosyası, küme için, hangi kapsayıcı görüntülerinin çalıştırılacağı gibi istenen durumu tanımlar. Bu makalede, bir Windows Server kapsayıcısında ASP.NET örnek uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimde, ASP.NET örnek uygulaması için bir [Kubernetes dağıtımı][kubernetes-deployment] ve uygulamaya internet 'ten erişmek için bir dış [Kubernetes hizmeti][kubernetes-service] dahildir.

ASP.NET örnek uygulaması, [.NET Framework örneklerinin][dotnet-samples] bir parçası olarak sağlanır ve bir Windows Server kapsayıcısında çalıştırılır. AKS 'ler Windows Server kapsayıcıları 'nın *Windows server 2019* veya daha büyük görüntülerini temel alarak olmasını gerektirir. Kubernetes bildirim dosyası, AKS kümenizin, Windows Server kapsayıcıları çalıştırabilmiş bir düğümde ASP.NET örnek uygulamanızın Pod özelliğini çalıştırmasını söylemek için bir [düğüm seçici][node-selector] de tanımlamalıdır.

`sample.yaml`Aşağıdaki YAML tanımında adlı bir dosya oluşturun ve kopyalayın. Azure Cloud Shell kullanırsanız, bu dosya kullanılarak `vi` veya `nano` bir sanal veya fiziksel sistemde çalışırken oluşturulabilir:

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

[Kubectl Apply][kubectl-apply] komutunu kullanarak uygulamayı dağıtın ve YAML bildiriminizde adı belirtin:

```console
kubectl apply -f sample.yaml
```

Aşağıdaki örnek çıktıda dağıtım ve hizmet başarıyla oluşturuldu gösterilmektedir:

```output
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında, bir Kubernetes hizmeti, uygulamanın ön ucuna internet 'e koyar. Bu işlemin tamamlanması birkaç dakika sürebilir. Bazen hizmetin sağlanması birkaç dakikadan uzun sürebilir. Bu durumlarda en fazla 10 dakika bekleyin.

İlerlemeyi izlemek için, [kubectl Get Service][kubectl-get] komutunu bağımsız değişkeniyle birlikte kullanın `--watch` .

```console
kubectl get service sample --watch
```

Başlangıçta *örnek* hizmet IÇIN *dış IP* , *Beklemede*olarak gösterilir.

```output
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*Dış IP* adresi *bekliyor* durumundan gerçek ortak IP adresi olarak değiştiğinde, `CTRL-C` izleme işlemini durdurmak için kullanın `kubectl` . Aşağıdaki örnek çıktıda, hizmete atanmış geçerli bir genel IP adresi gösterilmektedir:

```output
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Örnek uygulamayı eylemde görmek için, hizmetinizin dış IP adresine bir Web tarayıcısı açın.

![ASP.NET örnek uygulamasına göz atma görüntüsü](media/windows-container/asp-net-sample-app.png)

> [!Note]
> Sayfayı yüklemeye çalışırken bir bağlantı zaman aşımı alırsanız, örnek uygulamanın şu komutla hazır olduğunu doğrulamanız gerekir [kubectl Get Pod--Watch]. Bazen Windows kapsayıcısı, dış IP adresiniz kullanılabilir olduğunda başlamacaktır.

## <a name="delete-cluster"></a>Kümeyi silme

Kümeye artık ihtiyacınız yoksa [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete]. Yönetilen bir kimlik kullandıysanız, kimlik platform tarafından yönetilir ve kaldırma gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede bir Kubernetes kümesi dağıttınız ve bir Windows Server kapsayıcısında bir ASP.NET örnek uygulaması dağıttınız. Az önce oluşturduğunuz küme için [Kubernetes web panosuna erişin][kubernetes-dashboard] .

AKS hakkında daha fazla bilgi ve dağıtım örneği için tam kod açıklaması için Kubernetes küme öğreticisine geçin.

> [!div class="nextstepaction"]
> [AKS öğreticisi][aks-tutorial]

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
[use-advanced-networking]: configure-azure-cni.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
