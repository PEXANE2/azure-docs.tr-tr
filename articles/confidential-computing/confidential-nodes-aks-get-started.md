---
title: 'Hızlı başlangıç: gizli bilgi işlem düğümleri ile Azure CLı kullanarak Azure Kubernetes hizmeti (AKS) kümesi dağıtma'
description: Bu hızlı başlangıçta, gizli düğümlere sahip bir AKS kümesi oluşturmayı ve Azure CLı kullanarak bir Hello World uygulaması dağıtmayı öğreneceksiniz.
author: agowdamsft
ms.service: container-service
ms.subservice: confidential-computing
ms.topic: quickstart
ms.date: 03/18/2020
ms.author: amgowda
ms.custom: contentperf-fy21q3
ms.openlocfilehash: 73770acefc8a153e4a2f2fde146f9afd4c319cd3
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "105933143"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-with-confidential-computing-nodes-dcsv2-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak gizli bilgi işlem düğümleri (DCsv2) ile bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Bu hızlı başlangıç, hızlı bir şekilde bir AKS kümesi oluşturmak ve Azure 'da yönetilen Kubernetes hizmetini kullanarak uygulamaları izlemek için bir uygulama dağıtmak isteyen geliştiricilere veya küme işletmenlerine yöneliktir. Ayrıca, kümeyi temin edebilir ve Azure portal gizli bilgi işlem düğümleri ekleyebilirsiniz.

## <a name="overview"></a>Genel Bakış

Bu hızlı başlangıçta, Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesini nasıl dağıtacağınızı ve bir kuşakın içinde basit bir Hello World uygulaması çalıştırmayı öğreneceksiniz. AKS, kümeleri hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. Daha fazla bilgi edinmek için [aks tanıtım](../aks/intro-kubernetes.md) ve [aks gizli düğümlerine genel bakış](confidential-nodes-aks-overview.md)konusunu okuyun.

> [!NOTE]
> Gizli bilgi işlem DCsv2 VM 'Leri, daha yüksek fiyatlandırma ve bölge kullanılabilirliğine tabi olan özel donanımlardan yararlanır. Daha fazla bilgi için bkz. [kullanılabilir SKU 'lar ve desteklenen bölgeler](virtual-machine-solutions.md)için sanal makineler sayfası.

### <a name="confidential-computing-node-features-dcsv2"></a>Gizli bilgi işlem düğümü özellikleri (DCsv2)

1. Linux kapsayıcılarını destekleyen Linux çalışan düğümleri.
1. Ubuntu 18,04 sanal makineler düğümleri ile 2. nesil VM.
1. Şifrelenmiş sayfa önbelleği (EPC) ile Intel SGX tabanlı CPU. Daha fazla bilgi için [buraya](./faq.md) bakabilirsiniz.
1. Kubernetes sürüm 1.16 + desteği.
1. Intel SGX DCAP sürücüsü AKS düğümlerinde önceden yüklenmiş. Daha fazla bilgi için [buraya](./faq.md) bakabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

1. Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
1. Dağıtım makinenizde Azure CLı sürüm 2.0.64 veya üzeri yüklü ve yapılandırılmış ( `az --version` sürümü bulmak için ' i çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](../container-registry/container-registry-get-started-azure-cli.md).
1. Aboneliğinizde kullanım için en az altı **DCsv2** çekirdek kullanılabilir. Varsayılan olarak, Azure aboneliği başına gizli bilgi işlem için VM çekirdekleri kotası sekiz çekirdekdir. Sekiz taneden fazla çekirdek gerektiren bir küme sağlamayı planlıyorsanız, kota artışı bileti yükseltmek için [Bu](../azure-portal/supportability/per-vm-quota-requests.md) yönergeleri izleyin.

## <a name="create-a-new-aks-cluster-with-confidential-computing-nodes-and-add-on"></a>Gizli bilgi işlem düğümleri ve eklentisi ile yeni bir AKS kümesi oluşturma

Eklenti ile gizli bilgi işlem özellikli düğümler eklemek için aşağıdaki yönergeleri izleyin.

### <a name="create-an-aks-cluster-with-a-system-node-pool"></a>Sistem düğüm havuzu ile AKS kümesi oluşturma

Yukarıdaki gereksinimleri karşılayan bir AKS kümeniz zaten varsa, yeni bir gizli bilgi işlem düğümü havuzu eklemek için [mevcut küme bölümüne atlayın](#existing-cluster) .

İlk olarak, [az Group Create][az-group-create] komutunu kullanarak küme için bir kaynak grubu oluşturun. Aşağıdaki örnek *westus2* bölgesinde *myresourcegroup* adlı bir kaynak grubu adı oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Şimdi [az aks Create][az-aks-create] komutunu kullanarak bir aks kümesi oluşturun:

```azurecli-interactive
az aks create -g myResourceGroup --name myAKSCluster --generate-ssh-keys --enable-addon confcom
```

Yukarıdaki eklenti etkin bir sistem düğümü havuzu ile yeni bir AKS kümesi oluşturur. Ardından, AKS kümesine gizli bilgi işlem özelliklerine sahip bir Kullanıcı düğümü havuzu ekleyin.

### <a name="add-a-confidential-computing-node-pool-to-the-aks-cluster"></a>AKS kümesine gizli bilgi işlem düğümü havuzu ekleme 

Üç düğüme sahip bir Kullanıcı düğümü havuzu eklemek için aşağıdaki komutu çalıştırın `Standard_DC2s_v2` . Desteklenen [DCsv2 SKU 'ları ve bölgeleri](../virtual-machines/dcv2-series.md)listesinden başka bir SKU seçebilirsiniz.

```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-vm-size Standard_DC2s_v2
```

Çalıştırıldıktan sonra, **DCsv2** ile yeni bir düğüm havuzu, gizli bilgi işlem eklentisi Daemonsets ([SGX cihaz eklentisi](confidential-nodes-aks-overview.md#sgx-plugin)) ile görünür olmalıdır.

### <a name="verify-the-node-pool-and-add-on"></a>Düğüm havuzunu ve eklentiyi doğrulama

[Az aks Get-Credentials][az-aks-get-credentials] komutunu kullanarak aks kümeniz için kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Düğümlerin düzgün oluşturulduğunu ve SGX ile ilgili daemonsets, aşağıda gösterildiği gibi kubectl Get Pod & Nodes komutunu kullanarak **DCsv2** düğüm havuzlarında çalıştığını doğrulayın:

```console
$ kubectl get pods --all-namespaces

kube-system     sgx-device-plugin-xxxx     1/1     Running
```

Çıkış yukarıdaki ile eşleşiyorsa, AKS kümeniz artık gizli uygulamaları çalıştırmaya hazırdır.

Bir uygulamayı bir kuşta test etmek için, şifreleme dağıtım bölümüne [Merhaba Dünya](#hello-world) gidin. Veya AKS 'e ek düğüm havuzları eklemek için aşağıdaki yönergeleri izleyin (AKS, SGX düğüm havuzlarını ve SGX olmayan düğüm havuzlarını karıştırmaya olanak sağlar).

## <a name="add-a-confidential-computing-node-pool-to-an-existing-aks-cluster"></a>Mevcut bir AKS kümesine gizli bilgi işlem düğümü havuzu ekleme<a id="existing-cluster"></a>

Bu bölümde, önkoşul bölümünde listelenen ölçütlere uyan bir AKS kümeniz olduğu varsayılır (eklenti için geçerlidir).

### <a name="enable-the-confidential-computing-aks-add-on-on-the-existing-cluster"></a>Mevcut kümede gizli bilgi işlem AKS eklentisini etkinleştirin

Gizli bilgi işlem eklentisini etkinleştirmek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az aks enable-addons --addons confcom --name MyManagedCluster --resource-group MyResourceGroup 
```

### <a name="add-a-dcsv2-user-node-pool-to-the-cluster"></a>Kümeye **DCsv2** Kullanıcı düğümü havuzu ekleme

> [!NOTE]
> Gizli bilgi işlem özelliğini kullanmak için, mevcut AKS kümenizin en az bir **DCsv2** VM SKU 'su tabanlı düğüm havuzu olması gerekir. Gizli bilgi işlem DCs-v2 VM SKU 'larının hakkında daha fazla bilgi edinmek için bkz. [kullanılabilir SKU 'lar ve desteklenen bölgeler](virtual-machine-solutions.md).

Yeni bir düğüm havuzu oluşturmak için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-count 1 --node-vm-size Standard_DC4s_v2
```

Confcompool1 adlı yeni düğüm havuzunun oluşturulduğunu doğrulayın:

```azurecli-interactive
az aks nodepool list --cluster-name myAKSCluster --resource-group myResourceGroup
```

### <a name="verify-that-daemonsets-are-running-on-confidential-node-pools"></a>Daemonsets 'in gizli düğüm havuzlarında çalıştığını doğrulama

Aşağıdaki doğrulamayı gerçekleştirmek için mevcut AKS kümenizde oturum açın.

```console
kubectl get nodes
```

Çıktıda, AKS kümesinde yeni eklenen confcompool1 gösterilmelidir. Diğer daemonsets de görebilirsiniz.

```console
$ kubectl get pods --all-namespaces

kube-system     sgx-device-plugin-xxxx     1/1     Running
```

Çıkış yukarıdaki ile eşleşiyorsa, AKS kümeniz artık gizli uygulamaları çalıştırmaya hazırdır. Bir test uygulaması dağıtmak için aşağıdaki yönergeleri izleyin.

## <a name="hello-world-from-isolated-enclave-application"></a>Yalıtılmış şifreleme uygulamasından Merhaba Dünya <a id="hello-world"></a>
*Hello-World-Enclave. YAML* adlı bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın. Bu açık şifreleme tabanlı örnek uygulama kodu [Açık şifreleme projesinde](https://github.com/openenclave/openenclave/tree/master/samples/helloworld)bulunabilir. Aşağıdaki dağıtımda "confcom" eklentisini dağıttığınız varsayılmaktadır.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: sgx-test
  labels:
    app: sgx-test
spec:
  template:
    metadata:
      labels:
        app: sgx-test
    spec:
      containers:
      - name: sgxtest
        image: oeciteam/sgx-test:1.0
        resources:
          limits:
            kubernetes.azure.com/sgx_epc_mem_in_MiB: 5 # This limit will automatically place the job into confidential computing node. Alternatively you can target deployment to nodepools
      restartPolicy: Never
  backoffLimit: 0
  ```

Şimdi, aşağıdaki örnek çıktıda gösterildiği gibi güvenli bir kuşda başlatılacak örnek bir iş oluşturmak için kubectl Apply komutunu kullanın:

```console
$ kubectl apply -f hello-world-enclave.yaml

job "sgx-test" created
```

Aşağıdaki komutları çalıştırarak iş yükünün güvenilir bir yürütme ortamını (Enclave) başarıyla oluşturduğunu doğrulayabilirsiniz:

```console
$ kubectl get jobs -l app=sgx-test

NAME       COMPLETIONS   DURATION   AGE
sgx-test   1/1           1s         23s
```

```console
$ kubectl get pods -l app=sgx-test

NAME             READY   STATUS      RESTARTS   AGE
sgx-test-rchvg   0/1     Completed   0          25s
```

```console
$ kubectl logs -l app=sgx-test

Hello world from the enclave
Enclave called into host to print: Hello World!
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

İlişkili düğüm havuzlarını kaldırmak veya AKS kümesini silmek için aşağıdaki komutları kullanın:

### <a name="remove-the-confidential-computing-node-pool"></a>Gizli bilgi işlem düğüm havuzunu kaldırma

```azurecli-interactive
az aks nodepool delete --cluster-name myAKSCluster --name myNodePoolName --resource-group myResourceGroup
```

### <a name="delete-the-aks-cluster"></a>AKS kümesini silme

```azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```

## <a name="next-steps"></a>Sonraki adımlar

* [Gizli kapsayıcı örneklerini](https://github.com/Azure-Samples/confidential-container-samples)ziyaret ederek, gizli kapsayıcılar aracılığıyla Python, Node, vs. Applications confidentially çalıştırın.

* Şifreleme kullanan uygulamaları, [şifreli Azure Container örneklerini](https://github.com/Azure-Samples/confidential-computing/blob/main/containersamples/)ziyaret ederek çalıştırın.

<!-- LINKS -->
[az-group-create]: /cli/azure/group#az_group_create
[az-aks-create]: /cli/azure/aks#az_aks_create
[az-aks-get-credentials]: /cli/azure/aks#az_aks_get_credentials