---
title: Bir sanal ağda denemeleri ve çıkarımı Çalıştır
titleSuffix: Azure Machine Learning service
description: Makine öğrenimi denemeleri ve çıkarımı bir Azure sanal ağı içinde güvenli hale getirme. Model eğitimi için işlem hedefleri oluşturmayı ve bir sanal ağ içinde çıkarımını öğrenin. Gelen ve giden bağlantı noktaları gerektirme gibi güvenli sanal ağlara yönelik gereksinimler hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 07/10/2019
ms.openlocfilehash: 412eaac2f82a6d09761dcac53192916df215831f
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358779"
---
# <a name="securely-run-experiments-and-inference-inside-an-azure-virtual-network"></a>Bir Azure sanal ağı içinde denemeleri ve çıkarımı güvenle çalıştırın

Bu makalede, denemeleri ve çıkarımı bir sanal ağ içinde nasıl çalıştıracağınızı öğreneceksiniz. Bir sanal ağ, Azure kaynaklarınızı genel İnternet 'ten yalıtmak için bir güvenlik sınırı görevi görür. Ayrıca, bir Azure sanal ağını şirket içi ağınıza da katabilirsiniz. Modellerinizi güvenli bir şekilde eğmenize ve çıkarım için dağıtılan modellerinize erişmenize olanak tanır. Çıkarım veya model Puanlama, dağıtılan modelin tahmin için en yaygın olarak üretim verilerinde kullanıldığı aşamadır.

Azure Machine Learning hizmeti, işlem kaynakları için diğer Azure hizmetlerini kullanır. İşlem kaynakları (işlem hedefleri) modelleri eğitmek ve dağıtmak için kullanılır. Bu işlem hedefleri, bir sanal ağ içinde oluşturulabilir. Örneğin, bir modeli eğitme ve sonra modeli Azure Kubernetes Service (AKS) ' e dağıtmak için Microsoft Veri Bilimi Sanal Makinesi kullanabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için bkz. [Azure sanal ağa genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

## <a name="prerequisites"></a>Önkoşullar

Bu belgede, Azure sanal ağları ve genel olarak IP ağı hakkında bilgi sahibi olduğunuz varsayılır. Bu belge, işlem kaynaklarınızla kullanılacak bir sanal ağ ve alt ağ oluşturduğunuzu da varsayar. Azure sanal ağları hakkında bilginiz yoksa, hizmet hakkında bilgi edinmek için aşağıdaki makaleleri okuyun:

* [IP adresleme](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)
* [Güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview)
* [Hızlı Başlangıç: Sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)
* [Ağ trafiğini filtreleme](https://docs.microsoft.com/azure/virtual-network/tutorial-filter-network-traffic)

## <a name="storage-account-for-your-workspace"></a>Çalışma alanınızın depolama hesabı

> [!IMPORTANT]
> Azure Machine Learning hizmetiniz için __varsayılan depolama hesabı__ __yalnızca deneme çalışırken__bir sanal ağa yerleştirilebilir.
>
> __Deneme için varsayılan olmayan depolama hesapları__için veya __çıkarımı__için bir depolama hesabı kullanıyorsanız, __depolama hesabına Kısıtlanmamış erişiminizin__olması gerekir.
> 
> Bu ayarları değiştirdiğinizden emin değilseniz, bkz. [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security)bölümünde __varsayılan ağ erişim kuralını değiştirme__ . Çıkarım veya model Puanlama sırasında tüm ağlardan erişime izin vermek için adımları kullanın.

Çalışma alanı için varsayılan Azure Depolama hesabını bir sanal ağa yerleştirmek için aşağıdaki adımları kullanın:

1. Deneme işlem EX oluşturun. Bir sanal ağın arkasında Machine Learning İşlem veya çalışma alanı EX 'e bir deneme işlemi iliştirin. HDInsight kümesi veya sanal makine. Daha fazla bilgi için bkz. [Machine Learning işlem kullanma](#use-machine-learning-compute) ve bu belgedeki [bir sanal makine veya HDInsight küme bölümleri kullanma](#use-a-virtual-machine-or-hdinsight-cluster)
2. Çalışma alanına bağlı depolamaya gidin. ![Azure Machine Learning hizmeti çalışma alanına bağlı Azure Storage 'ı gösteren Azure portal görüntüsü](./media/how-to-enable-virtual-network/workspace-storage.png)
3. Azure depolama sayfasında __güvenlik duvarları ve sanal ağlar__' ı seçin. ![Azure depolama sayfasında güvenlik duvarları ve sanal ağlar bölümünü gösteren Azure portal görüntüsü](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)
4. __Güvenlik duvarları ve sanal ağlar__ sayfasında aşağıdaki girişleri seçin:
    - __Seçili ağlar__'ı seçin.
    - __Sanal ağlar__altında, deneme hesapla'nizin bulunduğu sanal ağı eklemek için __var olan sanal ağı ekle__ ' yi seçin. (Bkz. 1. adım)
    - __Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine Izin ver__' i seçin.
![Azure depolama altındaki güvenlik duvarları ve sanal ağlar sayfasını gösteren Azure portal görüntüsü](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png) 

5. Denemeyi çalıştırırken, deneme kodunuzda Run config 'i blob Storage kullanacak şekilde değiştirin:
    ```python
    run_config.source_directory_data_store = "workspaceblobstore"
    ```
    
## <a name="key-vault-for-your-workspace"></a>Çalışma alanınız için Anahtar Kasası
Çalışma alanıyla ilişkili Key Vault örnek, Azure Machine Learning hizmeti tarafından çeşitli türlerdeki kimlik bilgilerini depolamak için kullanılır:
* İlişkili depolama hesabı bağlantı dizesi
* Azure Container Repository deposu örneklerine parolalar
* Veri depolarına yönelik bağlantı dizeleri. 

Bir sanal ağın arkasında Key Vault Azure Machine Learning deneme özelliklerini kullanmak için aşağıdaki adımları izleyin:
1. Çalışma alanıyla ilişkili Key Vault gidin. ![Azure Machine Learning hizmeti çalışma alanıyla ilişkili Key Vault gösteren Azure portal görüntüsü](./media/how-to-enable-virtual-network/workspace-key-vault.png)
2. Key Vault sayfasında __güvenlik duvarları ve sanal ağlar__ bölümünü seçin. ![Key Vault sayfasındaki güvenlik duvarları ve sanal ağlar bölümünü gösteren Azure portal görüntüsü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)
3. __Güvenlik duvarları ve sanal ağlar__ sayfasında aşağıdaki girişleri seçin:
    - __Seçili ağlar__'ı seçin.
    - __Sanal ağlar__altında, deneme hesapla'nizin bulunduğu sanal ağı eklemek için __var olan sanal ağları Ekle__ ' yi seçin.
    - __Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver__' i seçin.
![Key Vault altındaki güvenlik duvarlarını ve sanal ağları gösteren Azure portal görüntüsü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png) 


## <a name="use-machine-learning-compute"></a>Machine Learning İşlem kullan

Bir sanal ağda Azure Machine Learning Işlem kullanmak için, ağ gereksinimleriyle ilgili olarak aşağıdaki bilgileri kullanın:

- Sanal ağın, Azure Machine Learning hizmeti çalışma alanıyla aynı abonelikte ve bölgede olması gerekir.

- Machine Learning İşlem kümesi için belirtilen alt ağda, küme için hedeflenen VM sayısına uyum sağlamak için yeterli sayıda atanmamış IP adresi olmalıdır. Alt ağda yeterli sayıda atanmamış IP adresi yoksa, küme kısmen ayrılacaktır.

- Trafiği kısıtlayarak sanal ağın güvenliğini sağlamayı planlıyorsanız Machine Learning İşlem hizmeti için bazı bağlantı noktalarını açık bırakın. Daha fazla bilgi için bkz. [gerekli bağlantı noktaları](#mlcports).

- Sanal ağın aboneliğine veya kaynak grubuna yönelik güvenlik ilkelerinizin veya kilitlerinizin sanal ağı yönetmek için izinleri kısıtlayıp kısıtlamamadığını denetleyin.

- Birden çok Machine Learning İşlem kümesini tek bir sanal ağa koyacaksanız bir veya daha fazla kaynağınız için bir kota artışı istemeniz gerekebilir.

    Machine Learning İşlem, sanal ağı içeren kaynak grubunda otomatik olarak ek ağ kaynakları ayırır. Her bir Machine Learning İşlem kümesi için, Azure Machine Learning hizmeti aşağıdaki kaynakları ayırır:

    - Bir ağ güvenlik grubu (NSG)

    - Bir genel IP adresi

    - Bir yük dengeleyici

  Bu kaynaklar, aboneliğin [kaynak kotalarıyla](https://docs.microsoft.com/azure/azure-subscription-service-limits) sınırlıdır.

### <a id="mlcports"></a>Gerekli bağlantı noktaları

Machine Learning İşlem şu anda, belirtilen sanal ağda VM 'Leri sağlamak için Azure Batch hizmetini kullanıyor. Alt ağ, Batch hizmetinden gelen iletişime izin vermelidir. Bu iletişim Machine Learning İşlem düğümlerinde çalışan zamanlamak ve Azure depolama ile diğer kaynaklarla iletişim kurmak için kullanılır. Batch, sanal makinelere bağlı ağ arabirimi (NIC) düzeyinde NSG 'Ler ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

- __Batchnodemanagement__ __hizmet etiketinden__ 29876 ve 29877 bağlantı noktalarında gelen TCP trafiği.

    ![BatchNodeManagement hizmet etiketini kullanarak bir gelen kuralı gösteren Azure portal görüntüsü](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)
 
- seçim Uzaktan erişime izin vermek için 22 numaralı bağlantı noktasında gelen TCP trafiği. Bu bağlantı noktası yalnızca ortak IP üzerinde SSH kullanarak bağlanmak istiyorsanız gereklidir.
 
- Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.

- İnternete giden herhangi bir bağlantı noktasında giden trafik. 

Batch ile yapılandırılmış NSG 'lerdeki gelen/giden kuralları değiştirir veya eklerseniz dikkatli olun. Bir NSG, işlem düğümleriyle iletişimi engelliyorsa Machine Learning İşlem Hizmetleri, işlem düğümlerinin durumunu kullanılamıyor olarak ayarlar.

Batch kendi NSG 'leri yapılandırdığından alt ağ düzeyinde NSG 'leri belirtmeniz gerekmez. Ancak, belirtilen alt ağda ilişkili NSG 'ler ve/veya güvenlik duvarı varsa, gelen ve giden güvenlik kurallarını daha önce belirtilen şekilde yapılandırın. 

Aşağıdaki ekran görüntüsünde, NSG kural yapılandırmasının Azure portal nasıl göründüğü gösterilmektedir:

![Machine Learning İşlem yönelik gelen NSG kurallarının ekran görüntüsü](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Machine Learning İşlem giden NSG kurallarının ekran görüntüsü](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a>Sanal ağ ile giden bağlantıyı sınırlandırma

Varsayılan giden kurallarını kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlandırmak istiyorsanız aşağıdaki adımları izleyin:

- NSG kurallarını kullanarak giden Internet bağlantısını reddetme 

- Giden trafiği Azure depolama ile sınırlayın ( __Storage. Region_Name__ EX __hizmet etiketini__ kullanarak). Storage. EastUS), Azure Container Registry ( __AzureContainerRegistry. Region_Name__ EX __hizmet etiketi__ kullanılıyor. AzureContainerRegistry. EastUS) ve Azure Machine Learning hizmeti ( __AzureMachineLearning__ __hizmet etiketini__ kullanarak)

Aşağıdaki ekran görüntüsünde, NSG kural yapılandırmasının Azure portal nasıl göründüğü gösterilmektedir:

![Machine Learning İşlem giden NSG kurallarının ekran görüntüsü](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)

### <a name="user-defined-routes-for-forced-tunneling"></a>Zorlamalı tünel için Kullanıcı tanımlı yollar

Azure Machine Learning Işlem ile Zorlamalı tünel kullanıyorsanız, [Kullanıcı tanımlı yollar (UDR)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) işlem kaynağını içeren alt ağa eklemeniz gerekir.

* Kaynaklarınızın bulunduğu bölgedeki Azure Batch hizmeti tarafından kullanılan her IP adresi için Kullanıcı tanımlı bir yol. Bu UDRs, Batch hizmetinin görev zamanlama için işlem düğümleriyle iletişim kurmasını sağlar. Batch hizmetinin IP adreslerinin bir listesini almak için Azure desteği 'ne başvurun.

* Azure depolama 'ya giden trafik (özellikle, formun `<account>.table.core.windows.net` `<account>.queue.core.windows.net`URL 'leri, ve `<account>.blob.core.windows.net`) Şirket içi ağ gereciniz tarafından engellenmemelidir.

Kullanıcı tanımlı yolları eklediğinizde, her bir ilgili Batch IP adresi ön eki için yolu tanımlayın ve __sonraki atlama türünü__ __Internet__olarak ayarlayın. Aşağıdaki görüntüde, Azure portal bu UDR 'nin bir örneği gösterilmektedir:

![Bir adres ön eki için Kullanıcı tanımlı yol örneği](./media/how-to-enable-virtual-network/user-defined-route.png)

Daha fazla bilgi için bkz. [sanal ağ 'da Azure Batch havuzu oluşturma](../../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling) makalesi.

### <a name="create-machine-learning-compute-in-a-virtual-network"></a>Sanal ağda Machine Learning İşlem oluşturma

Azure portal kullanarak bir Machine Learning İşlem kümesi oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com), Azure Machine Learning hizmeti çalışma alanınızı seçin.

1. __Uygulama__ bölümünde __işlem__' ı seçin. Sonra __Işlem Ekle__' yi seçin. 

    ![Azure Machine Learning hizmetinde işlem ekleme](./media/how-to-enable-virtual-network/add-compute.png)

1. Bu işlem kaynağını bir sanal ağ kullanacak şekilde yapılandırmak için aşağıdaki seçenekleri kullanın:

    - __Ağ yapılandırması__: __Gelişmiş__'i seçin.

    - __Kaynak grubu__: Sanal ağı içeren kaynak grubunu seçin.

    - __Sanal ağ__: Alt ağı içeren sanal ağı seçin.

    - __Alt ağ__: Kullanılacak alt ağı seçin.

   ![Machine Learning işlem için sanal ağ ayarlarını gösteren ekran görüntüsü](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Ayrıca, Azure Machine Learning SDK kullanarak bir Machine Learning İşlem kümesi de oluşturabilirsiniz. Aşağıdaki kod, adlı `default` `mynetwork`bir sanal ağın alt ağında yeni bir Machine Learning işlem kümesi oluşturur:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")

    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to complete, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Oluşturma işlemi tamamlandığında, bir kümeyi kullanarak modelinize eğitebilirsiniz. Daha fazla bilgi için bkz. [eğitim için bir işlem hedefi seçme ve kullanma](how-to-set-up-training-targets.md).

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Bir sanal makine veya HDInsight kümesi kullanma

Bir sanal makineyi veya Azure HDInsight kümesini çalışma alanınıza sahip bir sanal ağda kullanmak için şu adımları izleyin:

> [!IMPORTANT]
> Azure Machine Learning hizmeti yalnızca Ubuntu çalıştıran sanal makineleri destekler.

1. Azure portal veya Azure CLı kullanarak bir VM veya HDInsight kümesi oluşturun ve bunu bir Azure sanal ağına yerleştirin. Daha fazla bilgi için aşağıdaki belgelere bakın:
    * [Linux VM 'Ler için Azure sanal ağları oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Azure sanal ağını kullanarak HDInsight 'ı genişletme](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network) 

1. Azure Machine Learning hizmetinin VM veya kümedeki SSH bağlantı noktasıyla iletişim kurmasına izin vermek için NSG için bir kaynak girişi yapılandırmanız gerekir. SSH bağlantı noktası genellikle bağlantı noktası 22 ' dir. Bu kaynaktan gelen trafiğe izin vermek için aşağıdaki bilgileri kullanın:

    * __Kaynak__: __Hizmet etiketi__seçin.

    * __Kaynak hizmet etiketi__: __AzureMachineLearning__öğesini seçin.

    * __Kaynak bağlantı noktası aralıkları__: Öğesini __*__ seçin.

    * __Hedef__: __Herhangi birini__seçin.

    * __Hedef bağlantı noktası aralıkları__: __22__' yi seçin.

    * __Protokol__: __Herhangi birini__seçin.

    * __Eylem__: __Izin ver__' i seçin.

   ![Bir sanal ağ içinde bir VM veya HDInsight kümesi üzerinde deneme yapmak için gelen kuralların ekran görüntüsü](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    NSG için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.

    Varsayılan giden kurallarını kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlandırmak istiyorsanız, bkz [. sanal ağ ile giden bağlantıyı sınırlandırma](#limiting-outbound-from-vnet)
    
1. VM veya HDInsight kümesini Azure Machine Learning hizmet çalışma alanınıza ekleyin. Daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md).

## <a name="use-azure-kubernetes-service"></a>Azure Kubernetes hizmetini kullanma

> [!IMPORTANT]
> Adımlara geçmeden önce ön koşulları denetleyin ve kümenizin IP adreslemesini planlayın. Daha fazla bilgi için bkz. [Azure Kubernetes hizmetinde gelişmiş ağı yapılandırma](https://docs.microsoft.com/azure/aks/configure-advanced-networking).
> 
>
> NSG için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.
>
> Azure Kubernetes hizmeti ve Azure sanal ağı aynı bölgede olmalıdır.

Sanal bir ağa Azure Kubernetes hizmetini çalışma alanınıza eklemek için Azure portal şu adımları izleyin:

1. Sanal ağı denetleyen NSG grubunun __AzureMachineLearning__ __hizmet etiketi__ kullanılarak Azure Machine Learning hizmeti için etkin gelen kuralı olduğundan emin olun.

    ![Azure Machine Learning hizmetinde işlem ekleme](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)     
 
1. [Azure Portal](https://portal.azure.com), Azure Machine Learning hizmeti çalışma alanınızı seçin.

1. __Uygulama__ bölümünde __işlem__' ı seçin. Sonra __Işlem Ekle__' yi seçin. 

    ![Azure Machine Learning hizmetinde işlem ekleme](./media/how-to-enable-virtual-network/add-compute.png)

1. Bu işlem kaynağını bir sanal ağ kullanacak şekilde yapılandırmak için aşağıdaki seçenekleri kullanın:

    - __Ağ yapılandırması__: __Gelişmiş__'i seçin.

    - __Kaynak grubu__: Sanal ağı içeren kaynak grubunu seçin.

    - __Sanal ağ__: Alt ağı içeren sanal ağı seçin.

    - __Alt ağ__: Alt ağı seçin.

    - __Kubernetes hizmeti adres aralığı__: Kubernetes hizmeti adres aralığını seçin. Bu adres aralığı, küme için kullanılabilir IP adreslerini tanımlamak üzere bir CıDR gösterimi IP aralığı kullanır. Alt ağ IP aralıklarıyla çakışmamalıdır. Örneğin: 10.0.0.0/16.

    - __Kubernetes DNS HIZMETI IP adresi__: Kubernetes DNS hizmeti IP adresini seçin. Bu IP adresi, Kubernetes DNS hizmetine atanır. Bu, Kubernetes hizmeti adres aralığı içinde olmalıdır. Örneğin: 10.0.0.10.

    - __Docker köprü adresi__: Docker köprü adresini seçin. Bu IP adresi Docker köprüsüne atandı. Herhangi bir alt ağ IP aralığında olmamalı veya Kubernetes hizmeti adres aralığı olmamalıdır. Örneğin: 172.17.0.1/16.

   ![Azure Machine Learning hizmeti: Sanal ağ ayarlarını Machine Learning İşlem](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Sanal ağı denetleyen NSG grubunun, sanal ağ dışından çağrılabilmesi için Puanlama uç noktası için etkin gelen kuralı olduğundan emin olun

    ![Azure Machine Learning hizmetinde işlem ekleme](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)

    > [!TIP]
    > Bir sanal ağ içinde bir AKS kümeniz zaten varsa, çalışma alanına iliştirebilirsiniz. Daha fazla bilgi için bkz. [AKS 'e dağıtma](how-to-deploy-to-aks.md).

Azure Kubernetes hizmetini bir sanal ağa eklemek için **Azure Machine Learning SDK 'sını** de kullanabilirsiniz. Aşağıdaki kod, adlı `default` `mynetwork`bir sanal ağın alt ağında yeni bir Azure Kubernetes hizmet örneği oluşturur:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

Oluşturma işlemi tamamlandığında, bir sanal ağın arkasındaki AKS kümesinde çıkarım/puan verebilirsiniz. Daha fazla bilgi için bkz. [AKS 'e dağıtma](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim ortamlarını ayarlama](how-to-set-up-training-targets.md)
* [Modelleri dağıtılacağı yeri](how-to-deploy-and-where.md)
* [SSL ile güvenli bir şekilde model dağıtma](how-to-secure-web-service.md)

