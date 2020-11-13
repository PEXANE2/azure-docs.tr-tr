---
title: 'Hızlı başlangıç: gizli bilgi işlem düğümleri ile Azure CLı kullanarak Azure Kubernetes hizmeti (AKS) kümesi dağıtma'
description: Gizli düğümlere sahip bir AKS kümesi oluşturmayı ve Azure CLı kullanarak bir Hello World uygulaması dağıtmayı öğrenin.
author: agowdamsft
ms.service: container-service
ms.topic: quickstart
ms.date: 9/22/2020
ms.author: amgowda
ms.openlocfilehash: 95626836afb09ada286cf7e171f97db450167999
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94564353"
---
# <a name="quickstart-deploy-an-azure-kubernetes-service-aks-cluster-with-confidential-computing-nodes-using-azure-cli-preview"></a>Hızlı başlangıç: Azure CLı (Önizleme) kullanarak gizli bilgi işlem düğümleri ile bir Azure Kubernetes hizmeti (AKS) kümesi dağıtma

Bu hızlı başlangıç, hızlı bir şekilde bir AKS kümesi oluşturmak ve Azure 'da yönetilen Kubernetes hizmetini kullanarak uygulamaları izlemek için bir uygulama dağıtmak isteyen geliştiricilere veya küme işletmenlerine yöneliktir.

## <a name="overview"></a>Genel Bakış

Bu hızlı başlangıçta, Azure CLı kullanarak bir Azure Kubernetes hizmeti (AKS) kümesini nasıl dağıtacağınızı ve bir bir Hello World uygulamasını bir kuşakın içinde çalıştırmayı öğreneceksiniz. AKS, kümeleri hızlı bir şekilde dağıtmanıza ve yönetmenize olanak tanıyan bir yönetilen Kubernetes hizmetidir. [Burada](../aks/intro-kubernetes.md)aks hakkında daha fazla bilgi edinin.

> [!NOTE]
> Gizli bilgi işlem DCsv2 VM 'Leri, daha yüksek fiyatlandırma ve bölge kullanılabilirliğine tabi olan özel donanımlardan yararlanır. Daha fazla bilgi için bkz. [kullanılabilir SKU 'lar ve desteklenen bölgeler](virtual-machine-solutions.md)için sanal makineler sayfası.

### <a name="deployment-pre-requisites"></a>Dağıtım ön gereksinimleri

1. Etkin bir Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
1. Azure CLı sürüm 2.0.64 veya sonraki bir sürümü, dağıtım makinenizde yüklü ve yapılandırılmış olmalıdır ( `az --version` sürümü bulmak için ' i çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse bkz. [Azure CLI 'Yı yüklemek](../container-registry/container-registry-get-started-azure-cli.md)
1. [aks-önizleme uzantısı](https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview) en düşük sürüm 0.4.62 
1. Aboneliğinizde kullanılmak üzere en az altı **DC <x> s-v2** çekirdeği kullanılabilir. Varsayılan olarak, Azure abonelik 8 çekirdekleri başına gizli bilgi işlem için VM çekirdeklerinin kotası. 8 ' den fazla çekirdek gerektiren bir küme sağlamayı planlıyorsanız, kota artışı bileti yükseltmek için [Bu](../azure-portal/supportability/per-vm-quota-requests.md) yönergeleri izleyin

### <a name="confidential-computing-node-features-dcxs-v2"></a>Gizli bilgi işlem düğümü özellikleri (DC <x> s-v2)

1. Yalnızca Linux kapsayıcılarını destekleyen Linux çalışan düğümleri
1. Ubuntu Generation 2 18,04 sanal makineleri
1. Şifrelenmiş sayfa önbelleği (EPC) ile Intel SGX tabanlı CPU. [Daha fazla bilgi edinin](./faq.md)
1. Kubernetes sürüm 1.16 +
1. Önceden yüklenmiş Intel SGX DCAP sürücüsü. [Daha fazla bilgi edinin](./faq.md)
1. Önizleme sırasında dağıtılan CLı tabanlı


## <a name="installing-the-cli-pre-requisites"></a>CLı ön gereksinimleri yükleniyor

Aks-Preview 0.4.62 uzantısını veya üstünü yüklemek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli-interactive
az extension add --name aks-preview
az extension list
```
Aks-Preview CLı uzantısını güncelleştirmek için aşağıdaki Azure CLı komutlarını kullanın:

```azurecli-interactive
az extension update --name aks-preview
```

Gen2VMPreview kaydolun:

```azurecli-interactive
az feature register --name Gen2VMPreview --namespace Microsoft.ContainerService
```
Durumun kayıtlı olarak gösterilmesi birkaç dakika sürebilir. ' Az Feature List ' komutunu kullanarak kayıt durumunu kontrol edebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/Gen2VMPreview')].{Name:name,State:properties.state}"
```
Durum kayıtlı olarak görünüyorsa, ' az Provider Register ' komutunu kullanarak Microsoft. ContainerService kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="creating-an-aks-cluster"></a>AKS kümesi oluşturma

Yukarıdaki gereksinimleri karşılayan bir AKS kümeniz zaten varsa, yeni bir gizli bilgi işlem düğümü havuzu eklemek için [mevcut küme bölümüne atlayın](#existing-cluster) .

İlk olarak, az Group Create komutunu kullanarak küme için bir kaynak grubu oluşturun. Aşağıdaki örnek *westus2* bölgesinde *myresourcegroup* adlı bir kaynak grubu adı oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westus2
```

Şimdi az aks Create komutunu kullanarak bir AKS kümesi oluşturun. Aşağıdaki örnek tek bir düğümü olan bir küme oluşturur `Standard_DC2s_v2` . DCsv2 SKU 'ların desteklenen diğer listesini [buradan](../virtual-machines/dcv2-series.md)seçebilirsiniz:

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-vm-size Standard_DC2s_v2 \
    --node-count 3 \
    --enable-addon confcom \
    --network-plugin azure \
    --vm-set-type VirtualMachineScaleSets \
    --aks-custom-headers usegen2vm=true
```
Yukarıdaki komutun **DC <x> s-v2** düğüm havuzlarıyla yeni bir aks kümesi sağlaması ve iki Daemon kümesini otomatik olarak yüklemesi gerekir-( [SGX cihaz eklentisi](confidential-nodes-aks-overview.md#sgx-plugin)  &  [SGX quote Yardımcısı](confidential-nodes-aks-overview.md#sgx-quote))

Az aks Get-Credentials komutunu kullanarak AKS kümeniz için kimlik bilgilerini alın:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```
Düğümlerin düzgün bir şekilde oluşturulduğunu ve SGX ile ilgili Daemon kümelerinin, kubectl Get Pod & Nodes komutunu kullanarak aşağıda gösterildiği gibi **DC <x> s-v2** düğüm havuzlarında çalıştığını doğrulayın:

```console
$ kubectl get pods --all-namespaces

output
kube-system     sgx-device-plugin-xxxx     1/1     Running
```
Çıkış yukarıdaki ile eşleşiyorsa, AKS kümeniz artık gizli uygulamaları çalıştırmaya hazırdır.

Bir uygulamayı bir kuşta test etmek için [Enclave](#hello-world) dağıtım bölümü Merhaba Dünya gidin. Ya da, AKS 'e ek düğüm havuzları eklemek için aşağıdaki yönergeleri izleyin (AKS, SGX düğüm havuzlarını ve SGX olmayan düğüm havuzlarını karıştırmaya olanak sağlar)

>DCSv2 node Havuzlarınızda SGX ile ilgili Daemon kümeleri yüklü değilse, aşağıdan çalıştırın.

```azurecli-interactive
az aks update --enable-addons confcom --resource-group myResourceGroup --name myAKSCluster
```

![DCSv2 AKS kümesi oluşturma](./media/confidential-nodes-aks-overview/CLIAKSProvisioning.gif)

## <a name="adding-confidential-computing-node-to-existing-aks-cluster"></a>Mevcut AKS kümesine gizli bilgi işlem düğümü ekleme<a id="existing-cluster"></a>

Bu bölüm, önceden koşul bölümünde listelenen ölçütlere uyan bir AKS kümeniz olduğunu varsayar.

İlk olarak, var olan kümedeki bilgi işlem ile ilgili AKS eklentilerini etkinleştirir:

```azurecli-interactive
az aks enable-addons --addons confcom --name MyManagedCluster --resource-group MyResourceGroup 
```
Şimdi kümeye bir **DC <x> s-v2** düğüm havuzu ekleyin
    
> [!NOTE]
> Gizli bilgi işlem özelliğini kullanmak için mevcut AKS kümenizin en az bir **DC <x> s-v2** VM SKU 'su tabanlı düğüm havuzu olması gerekir. Gizli bilgi işlem DCsv2 VM SKU 'sunun [kullanılabilir SKU 'ları ve desteklenen bölgeleri](virtual-machine-solutions.md)hakkında daha fazla bilgi edinin.
    
  ```azurecli-interactive
az aks nodepool add --cluster-name myAKSCluster --name confcompool1 --resource-group myResourceGroup --node-count 1 --node-vm-size Standard_DC4s_v2 --aks-custom-headers usegen2vm=true

output node pool added

Verify

az aks nodepool list --cluster-name myAKSCluster --resource-group myResourceGroup
```

```console
kubectl get nodes
```
Çıktıda, AKS kümesinde yeni eklenen confcompool1 gösterilmelidir.

```console
$ kubectl get pods --all-namespaces

output (you may also see other daemonsets along SGX daemonsets as below)
kube-system     sgx-device-plugin-xxxx     1/1     Running
kube-system     sgx-quote-helper-xxxx      1/1     Running
```
Çıkış yukarıdaki ile eşleşiyorsa, AKS kümeniz artık gizli uygulamaları çalıştırmaya hazırdır.

## <a name="hello-world-from-isolated-enclave-application"></a>Yalıtılmış şifreleme uygulamasından Merhaba Dünya <a id="hello-world"></a>
*Hello-World-Enclave. YAML* adlı bir dosya oluşturun ve aşağıdaki YAML bildirimini yapıştırın. Bu açık şifreleme tabanlı örnek uygulama kodu [Açık şifreleme projesinde](https://github.com/openenclave/openenclave/tree/master/samples/helloworld)bulunabilir.

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