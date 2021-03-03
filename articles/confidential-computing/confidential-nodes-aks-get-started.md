---
title: 'Hızlı başlangıç: gizli bilgi işlem düğümleri ile Azure CLı kullanarak Azure Kubernetes hizmeti (AKS) kümesi dağıtma'
description: Gizli düğümlere sahip bir AKS kümesi oluşturmayı ve Azure CLı kullanarak bir Hello World uygulaması dağıtmayı öğrenin.
author: agowdamsft
ms.service: container-service
ms.topic: quickstart
ms.date: 2/25/2020
ms.author: amgowda
ms.openlocfilehash: 51b0813849236d9335d1482019f740fc8b23749f
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101703295"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-with-confidential-computing-nodes-dcsv2-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak gizli bilgi işlem düğümleri (DCsv2) ile bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Bu hızlı başlangıç, hızlı bir şekilde bir AKS kümesi oluşturmak ve Azure 'da yönetilen Kubernetes hizmetini kullanarak uygulamaları izlemek için bir uygulama dağıtmak isteyen geliştiricilere veya küme işletmenlerine yöneliktir. Ayrıca, kümeyi temin edebilir ve Azure portal gizli bilgi işlem düğümleri ekleyebilirsiniz.

## <a name="overview"></a>Genel Bakış

Bu hızlı başlangıçta, Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesini nasıl dağıtacağınızı ve bir kuşakın içinde basit bir Hello World uygulaması çalıştırmayı öğreneceksiniz. AKS, kümeleri hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. [Burada](../aks/intro-kubernetes.md)aks hakkında daha fazla bilgi edinin.

> [!NOTE]
> Gizli bilgi işlem DCsv2 VM 'Leri, daha yüksek fiyatlandırma ve bölge kullanılabilirliğine tabi olan özel donanımlardan yararlanır. Daha fazla bilgi için bkz. [kullanılabilir SKU 'lar ve desteklenen bölgeler](virtual-machine-solutions.md)için sanal makineler sayfası.

### <a name="confidential-computing-node-features-dcxs-v2"></a>Gizli bilgi işlem düğümü özellikleri (DC <x> s-v2)

1. Linux kapsayıcılarını destekleyen Linux çalışan düğümleri
1. Ubuntu 18,04 sanal makineler düğümleri ile 2. nesil VM
1. Şifrelenmiş sayfa önbelleği (EPC) ile Intel SGX tabanlı CPU. [Daha fazla bilgi edinin](./faq.md)
1. Kubernetes sürüm 1.16 + desteği
1. Intel SGX DCAP sürücüsü AKS düğümlerinde önceden yüklenmiş. [Daha fazla bilgi edinin](./faq.md)

## <a name="deployment-prerequisites"></a>Dağıtım önkoşulları
Dağıtım öğreticisi şunları gerektirir:

1. Etkin bir Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
1. Dağıtım makinenizde Azure CLı sürüm 2.0.64 veya üzeri yüklü ve yapılandırılmış ( `az --version` sürümü bulmak için ' i çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek](../container-registry/container-registry-get-started-azure-cli.md)
1. Aboneliğinizdeki kullanım için en az altı **DC <x> s-v2** çekirdeği mevcuttur. Varsayılan olarak, Azure abonelik 8 çekirdekleri başına gizli bilgi işlem için VM çekirdeklerinin kotası. 8 ' den fazla çekirdek gerektiren bir küme sağlamayı planlıyorsanız, kota artışı bileti yükseltmek için [Bu](../azure-portal/supportability/per-vm-quota-requests.md) yönergeleri izleyin

## <a name="creating-new-aks-cluster-with-confidential-computing-nodes-and-add-on"></a>Gizli bilgi işlem düğümleri ve eklentisi ile yeni AKS kümesi oluşturma
Eklenti ile gizli bilgi işlem özellikli düğümler eklemek için aşağıdaki yönergeleri izleyin.

### <a name="step-1-creating-an-aks-cluster-with-system-node-pool"></a>1. Adım: sistem düğüm havuzuyla AKS kümesi oluşturma

Yukarıdaki gereksinimleri karşılayan bir AKS kümeniz zaten varsa, yeni bir gizli bilgi işlem düğümü havuzu eklemek için [mevcut küme bölümüne atlayın](#existing-cluster) .

İlk olarak, az Group Create komutunu kullanarak küme için bir kaynak grubu oluşturun. Aşağıdaki örnek *westus2* bölgesinde *myresourcegroup* adlı bir kaynak grubu adı oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Şimdi az aks Create komutunu kullanarak bir AKS kümesi oluşturun.

```azurecli-interactive
# Create a new AKS cluster with system node pool with Confidential Computing addon enabled
az aks create -g myResourceGroup --name myAKSCluster --generate-ssh-keys --enable-addon confcom
```
Yukarıdaki, eklenti etkinken sistem düğüm havuzu ile yeni bir AKS kümesi oluşturur. Artık AKS (DCsv2) üzerinde gizli bilgi Işlem Nodepool türünün bir Kullanıcı düğümünü eklemeye devam edin

### <a name="step-2-adding-confidential-computing-node-pool-to-aks-cluster"></a>2. Adım: AKS kümesine gizli bilgi işlem düğüm havuzu ekleme 

3 düğümle bir Kullanıcı nodepool için aşağıdaki komutu çalıştırın `Standard_DC2s_v2` . DCsv2 SKU 'Larının ve bölgelerin desteklenen diğer listesini [buradan](../virtual-machines/dcv2-series.md)seçebilirsiniz:

```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-vm-size Standard_DC2s_v2
```
Yukarıdaki komut, **DC <x> s-v2** ile yeni bir düğüm havuzunun gizli bilgi işlem eklentisi Daemonsets ([SGX cihaz eklentisi](confidential-nodes-aks-overview.md#sgx-plugin) ) ile görünebilmelidir.
 
### <a name="step-3-verify-the-node-pool-and-add-on"></a>3. Adım: düğüm havuzunu ve eklentiyi doğrulama
Az aks Get-Credentials komutunu kullanarak AKS kümeniz için kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```
Düğümlerin düzgün oluşturulduğunu ve SGX ile ilgili daemonsets, aşağıda gösterildiği gibi kubectl Get Pod & Nodes komutunu kullanarak **DC <x> s-v2** düğüm havuzlarında çalıştığını doğrulayın:

```console
$ kubectl get pods --all-namespaces

output
kube-system     sgx-device-plugin-xxxx     1/1     Running
```
Çıkış yukarıdaki ile eşleşiyorsa, AKS kümeniz artık gizli uygulamaları çalıştırmaya hazırdır.

Bir uygulamayı bir kuşta test etmek için [Enclave](#hello-world) dağıtım bölümü Merhaba Dünya gidin. Veya AKS 'e ek düğüm havuzları eklemek için aşağıdaki yönergeleri izleyin (AKS, SGX düğüm havuzlarını ve SGX olmayan düğüm havuzlarını karıştırmaya olanak sağlar)

## <a name="adding-confidential-computing-node-pool-to-existing-aks-cluster"></a>Mevcut AKS kümesine gizli bilgi işlem düğüm havuzu ekleniyor<a id="existing-cluster"></a>

Bu bölümde, önkoşul bölümünde listelenen ölçütlere uyan bir AKS kümeniz olduğu varsayılır (eklenti için geçerlidir).

### <a name="step-1-enabling-the-confidential-computing-aks-add-on-on-the-existing-cluster"></a>1. Adım: var olan kümede gizli bilgi işlem AKS eklentisini etkinleştirme

Gizli bilgi işlem eklentisini etkinleştirmek için aşağıdaki komutu çalıştırın

```azurecli-interactive
az aks enable-addons --addons confcom --name MyManagedCluster --resource-group MyResourceGroup 
```
### <a name="step-2-add-dcxs-v2-user-node-pool-to-the-cluster"></a>2. Adım: **DC <x> s-v2** Kullanıcı düğümü havuzunu kümeye ekleme
    
> [!NOTE]
> Gizli bilgi işlem özelliğini kullanmak için mevcut AKS kümenizin en az bir **DC <x> s-v2** VM SKU 'su tabanlı düğüm havuzu olması gerekir. Gizli bilgi işlem DCsv2 VM SKU 'sunun [kullanılabilir SKU 'ları ve desteklenen bölgeleri](virtual-machine-solutions.md)hakkında daha fazla bilgi edinin.
    
  ```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-count 1 --node-vm-size Standard_DC4s_v2

output node pool added

Verify

az aks nodepool list --cluster-name myAKSCluster --resource-group myResourceGroup
```
Yukarıdaki komut, confcompool1 adıyla eklediğiniz en son düğüm havuzunu listelemelidir.

### <a name="step-3-verify-that-daemonsets-are-running-on-confidential-node-pools"></a>3. Adım: daemonsets 'in gizli düğüm havuzlarında çalıştığını doğrulama

Aşağıdaki doğrulamayı gerçekleştirmek için mevcut AKS kümenizde oturum açın. 

```console
kubectl get nodes
```
Çıktıda, AKS kümesinde yeni eklenen confcompool1 gösterilmelidir.

```console
$ kubectl get pods --all-namespaces

output (you may also see other daemonsets along SGX daemonsets as below)
kube-system     sgx-device-plugin-xxxx     1/1     Running
```
Çıkış yukarıdaki ile eşleşiyorsa, AKS kümeniz artık gizli uygulamaları çalıştırmaya hazırdır. Lütfen aşağıdaki test uygulaması dağıtımını izleyin.

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
```

```console
$ kubectl get jobs -l app=sgx-test
NAME       COMPLETIONS   DURATION   AGE
sgx-test   1/1           1s         23s
```

```console
$ kubectl get pods -l app=sgx-test
```

```console
$ kubectl get pods -l app=sgx-test
NAME             READY   STATUS      RESTARTS   AGE
sgx-test-rchvg   0/1     Completed   0          25s
```

```console
$ kubectl logs -l app=sgx-test
```

```console
$ kubectl logs -l app=sgx-test
Hello world from the enclave
Enclave called into host to print: Hello World!
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

İlişkili düğüm havuzlarını kaldırmak veya AKS kümesini silmek için aşağıdaki komutları kullanın:

AKS kümesini silme
``````azurecli-interactive
az aks delete --resource-group myResourceGroup --name myAKSCluster
```
Removing the confidential computing node pool

``````azurecli-interactive
az aks nodepool delete --cluster-name myAKSCluster --name myNodePoolName --resource-group myResourceGroup
``````

## <a name="next-steps"></a>Sonraki adımlar

Python, düğüm vb. çalıştırma Uygulamalar, gizli [kapsayıcı örneklerini](https://github.com/Azure-Samples/confidential-container-samples)ziyaret ederek gizli kapsayıcılar üzerinden confidentially.

Şifreleme kullanan uygulamaları, [şifreli Azure Container örneklerini](https://github.com/Azure-Samples/confidential-computing/blob/main/containersamples/)ziyaret ederek çalıştırın.
