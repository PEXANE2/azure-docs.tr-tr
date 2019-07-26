---
title: Önizleme-Azure Kubernetes Service (AKS) kümesinde Windows Server kapsayıcısı oluşturma
description: Azure CLı kullanarak Azure Kubernetes Service (AKS) içindeki bir Windows Server kapsayıcısında bir uygulama dağıtma ve bir Kubernetes kümesini hızlı bir şekilde oluşturmayı öğrenin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/17/2019
ms.author: mlearned
ms.openlocfilehash: 305901007180cfb197cf5c0dfb338800449560a1
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68382038"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Önizleme-Azure CLı kullanarak bir Azure Kubernetes Service (AKS) kümesinde Windows Server kapsayıcısı oluşturma

Azure Kubernetes hizmeti (AKS), kümelerinizi hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. Bu makalede, Azure CLı kullanarak bir AKS kümesi dağıtırsınız. Ayrıca, bir Windows Server kapsayıcısında kümeye bir ASP.NET örnek uygulaması da dağıtabilirsiniz.

Bu özellik şu anda önizleme sürümündedir.

![ASP.NET örnek uygulamasına göz atma görüntüsü](media/windows-container/asp-net-sample-app.png)

Bu makalede, Kubernetes kavramlarının temel bir şekilde anlaşıldığı varsayılır. Daha fazla bilgi için bkz. [Azure Kubernetes hizmeti (AKS) Için Kubernetes temel kavramları][kubernetes-concepts].

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.61 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="before-you-begin"></a>Başlamadan önce

Windows Server kapsayıcıları çalıştıran kümenizi oluşturduktan sonra ek bir düğüm havuzu eklemeniz gerekir. Ek düğüm havuzu eklemek sonraki bir adımda ele alınmıştır, ancak önce birkaç önizleme özelliğini etkinleştirmeniz gerekir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis, kabul etme özellikleridir. Topluluğumuza geri bildirim ve hata toplamak için sağlanırlar. Önizlemede, bu özellikler üretim kullanımı için tasarlanmamıştır. Genel önizlemede bulunan Özellikler ' en iyi çaba ' desteği altına düşmektedir. AKS teknik destek ekiplerinden yardım yalnızca çalışma saatleri Pasifik saat dilimi (PST) sırasında kullanılabilir. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Aks-Preview CLı uzantısını yükler

Windows Server kapsayıcıları kullanmak için, *aks-Preview* CLI uzantısının sürüm 0.4.1 veya daha yüksek olması gerekir. [Az Extension Add][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] komutunu kullanarak *aks-Preview* Azure CLI uzantısını yükler::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-windows-preview-feature"></a>Windows Preview özelliğini Kaydet

Birden çok düğüm havuzu kullanan ve Windows Server kapsayıcıları çalıştıran bir AKS kümesi oluşturmak için, önce aboneliğinizde *Windowspreview* Özellik bayraklarını etkinleştirin. *Windowspreview* özelliği ayrıca, Kubernetes düğümlerinin dağıtımını ve yapılandırmasını yönetmek için çok düğümlü havuz kümeleri ve sanal makine ölçek kümesi kullanır. Aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanarak *windowspreview* Özellik bayrağını kaydedin:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> *Windowspreview* özelliği bayrağını başarıyla kaydettikten sonra oluşturduğunuz aks kümesi, bu önizleme kümesi deneyimini kullanın. Düzenli, tam olarak desteklenen kümeler oluşturmaya devam etmek için üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek için ayrı bir test veya geliştirme Azure aboneliği kullanın.

Kaydın tamamlanabilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyin:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Kayıt durumu `Registered`olduğunda, durumu izlemeyi durdurmak için CTRL-C tuşlarına basın.  Ardından, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Sınırlamalar

Birden çok düğüm havuzunu destekleyen AKS kümelerini oluşturup yönetirken aşağıdaki sınırlamalar geçerlidir:

* *Windowspreview*başarıyla kaydedildikten sonra oluşturulan kümeler için birden çok düğüm havuzu kullanılabilir. Aboneliğiniz için *Multiagentpoolpreview* ve *VMSSPreview* özelliklerini kaydettiğinizde birden çok düğüm havuzu da kullanılabilir. Bu özellikler başarıyla kaydedilmeden önce, var olan bir AKS kümesiyle oluşturulmuş düğüm havuzları ekleyemez veya yönetemezsiniz.
* İlk düğüm havuzunu silemezsiniz.

Bu özellik önizlemedeyken aşağıdaki ek sınırlamalar geçerlidir:

* AKS kümesinde en fazla sekiz düğüm havuzu olabilir.
* AKS kümesi, bu sekiz düğüm havuzunda en fazla 400 düğüme sahip olabilir.
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

Windows Server kapsayıcıları için düğüm havuzlarını destekleyen bir AKS kümesini çalıştırmak için, kümenizin [Azure CNI][azure-cni-about] (advanced) network plugin. For more detailed information to help plan out the required subnet ranges and network considerations, see [configure Azure CNI networking][use-advanced-networking]kullanan bir ağ ilkesi kullanması gerekir. *Myakscluster*adlı bir aks kümesi oluşturmak için [az aks Create][az-aks-Create] komutunu kullanın. Mevcut değilse, bu komut gerekli ağ kaynaklarını oluşturur.
  * Küme bir düğüm ile yapılandırıldı
  * *Windows-Admin-Password* ve *Windows-admin-username* parametreleri, kümede oluşturulan herhangi bir Windows Server kapsayıcısı için yönetici kimlik bilgilerini ayarlar.

Kendi güvenli *PASSWORD_WIN* sağlayın (Bu makaledeki komutların bir bash kabuğu 'na girildiğini unutmayın):

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --enable-addons monitoring \
    --kubernetes-version 1.14.1 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --enable-vmss \
    --network-plugin azure
```

> [!Note]
> Parola doğrulama hatası alırsanız, kaynak grubunuzu başka bir bölgede oluşturmayı deneyin.
> Ardından yeni kaynak grubuyla kümeyi oluşturmayı deneyin.

Birkaç dakika sonra komut tamamlanır ve küme hakkında JSON biçimli bilgileri döndürür.

## <a name="add-a-windows-server-node-pool"></a>Windows Server düğüm Havuzu Ekle

Varsayılan olarak, bir AKS kümesi, Linux kapsayıcıları çalıştırabilirler bir düğüm havuzuyla oluşturulur. Windows `az aks nodepool add` Server kapsayıcıları çalıştırabilirler ek bir düğüm havuzu eklemek için komutunu kullanın.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.14.1
```

Yukarıdaki komut, *npwin* adlı yeni bir düğüm havuzu oluşturur ve bunu *Myakscluster*öğesine ekler. Windows Server kapsayıcıları çalıştırmak için bir düğüm havuzu oluştururken, *düğüm-VM-boyutu* için varsayılan değer *Standard_D2s_v3*' dir. *Düğüm-VM-boyut* parametresini ayarlamayı seçerseniz, lütfen [kısıtlı VM boyutlarının][restricted-vm-sizes]listesini kontrol edin. Önerilen en düşük boyut *Standard_D2s_v3*' dir. Yukarıdaki komut, çalışırken `az aks create`oluşturulan varsayılan VNET 'teki varsayılan alt ağı da kullanır.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Kubernetes kümesini yönetmek için Kubernetes komut satırı istemcisi olan [kubectl][kubectl]'yi kullanırsınız. Azure Cloud Shell kullanıyorsanız, `kubectl` zaten yüklüdür. Yerel olarak `kubectl` yüklemek için [az aks install-cli][az-aks-install-cli] komutunu kullanın:

```azurecli
az aks install-cli
```

Kubernetes kümenize bağlanacak şekilde yapılandırmak `kubectl` için [az aks Get-Credentials][az-aks-get-credentials] komutunu kullanın. Bu komut, kimlik bilgilerini indirir ve Kubernetes CLı 'yi bunları kullanacak şekilde yapılandırır.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Kümenize bağlantıyı doğrulamak için [kubectl get][kubectl-get] komutunu kullanarak küme düğümleri listesini alın.

```azurecli-interactive
kubectl get nodes
```

Aşağıdaki örnekte önceki adımlarda oluşturulan tek düğüm gösterilmiştir. Düğüm durumunun olduğundan emin *olun:*

```
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
aksnpwin987654                      Ready    agent   108s   v1.14.1
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Bir Kubernetes bildirim dosyası, küme için, hangi kapsayıcı görüntülerinin çalıştırılacağı gibi istenen durumu tanımlar. Bu makalede, bir Windows Server kapsayıcısında ASP.NET örnek uygulamasını çalıştırmak için gerekli tüm nesneleri oluşturmak için bir bildirim kullanılır. Bu bildirimde, uygulamaya internet 'ten erişmek için bir [Kubernetes dağıtımı][kubernetes-deployment] for the ASP.NET sample application and an external [Kubernetes service][kubernetes-service] bulunur.

ASP.NET örnek uygulaması, [.NET Framework örneklerinin][dotnet-samples] bir parçası olarak sağlanır ve bir Windows Server kapsayıcısında çalıştırılır. AKS 'ler Windows Server kapsayıcıları 'nın *Windows server 2019* veya daha büyük görüntülerini temel alarak olmasını gerektirir. Kubernetes bildirim dosyası, AKS kümenizin, Windows Server kapsayıcıları çalıştırabilmiş bir düğümde ASP.NET örnek uygulamanızın Pod özelliğini çalıştırmasını söylemek için bir [düğüm seçici][node-selector] de tanımlamalıdır.

Aşağıdaki YAML tanımında `sample.yaml` adlı bir dosya oluşturun ve kopyalayın. Azure Cloud Shell kullanırsanız, bu dosya kullanılarak `vi` veya `nano` bir sanal veya fiziksel sistemde çalışırken oluşturulabilir:

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

```azurecli-interactive
kubectl apply -f sample.yaml
```

Aşağıdaki örnek çıktıda dağıtım ve hizmet başarıyla oluşturuldu gösterilmektedir:

```
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulama çalıştığında, bir Kubernetes hizmeti, uygulamanın ön ucuna internet 'e koyar. Bu işlemin tamamlanması birkaç dakika sürebilir.

İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service sample --watch
```

Başlangıçta *örnek* hizmet IÇIN *dış IP* , *Beklemede*olarak gösterilir.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

*Dış IP* adresi *bekliyor* durumundan gerçek ortak IP adresi olarak değiştiğinde, `CTRL-C` `kubectl` izleme işlemini durdurmak için kullanın. Aşağıdaki örnek çıktıda, hizmete atanmış geçerli bir genel IP adresi gösterilmektedir:

```
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Örnek uygulamayı eylemde görmek için, hizmetinizin dış IP adresine bir Web tarayıcısı açın.

![ASP.NET örnek uygulamasına göz atma görüntüsü](media/windows-container/asp-net-sample-app.png)

## <a name="delete-cluster"></a>Kümeyi sil

Küme artık gerekli değilse, [az Group Delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve tüm ilgili kaynakları kaldırın.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [aks hizmet sorumlusu konuları ve silme][sp-delete].

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
[use-advanced-networking]: configure-advanced-networking.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
