---
title: Sanal ağlarla güvenli eğitim ortamları
titleSuffix: Azure Machine Learning
description: Azure Machine Learning eğitim ortamınızı güvenli hale getirmek için yalıtılmış bir Azure sanal ağı kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 07/16/2020
ms.custom: contperfq4, tracking-python
ms.openlocfilehash: 5a71476db6f57841a0057de5b8c95f07ef5d90ad
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90603093"
---
# <a name="secure-an-azure-machine-learning-training-environment-with-virtual-networks"></a>Sanal ağlarla Azure Machine Learning eğitim ortamının güvenliğini sağlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning bir sanal ağla eğitim ortamlarının güvenliğini nasıl sağlayacağınızı öğreneceksiniz.

Bu makale, bir Azure Machine Learning iş akışını güvenli hale getirmek için size kılavuzluk eden beş bölümlü bir serinin üçüncü bölümüdür. İlk olarak birinci mimariyi anlamak için [tek bir bölüm (VNet genel bakış](how-to-network-security-overview.md) ) okumanız önemle tavsiye ederiz. 

Bu serideki diğer makalelere göz atın:

[1. VNET genel bakış](how-to-network-security-overview.md)  >  [çalışma alanı](how-to-secure-workspace-vnet.md)3 ' ü güvenceye alın  >  **. Eğitim ortamının**4 ' ü koruyun  >  [. Invenli ortam](how-to-secure-inferencing-vnet.md)5 ' i güvenli hale getirin   >  [. Studio işlevselliğini etkinleştir](how-to-enable-studio-virtual-network.md)

Bu makalede, bir sanal ağda aşağıdaki eğitim işlem kaynaklarını güvenli hale getirme hakkında bilgi edineceksiniz:
> [!div class="checklist"]
> - Azure Machine Learning işlem kümesi
> - Azure Machine Learning işlem örneği
> - Azure Databricks
> - Sanal Makine
> - HDInsight kümesi

## <a name="prerequisites"></a>Önkoşullar

+ Genel sanal ağ senaryolarını ve genel sanal ağ mimarisini anlamak için [ağ güvenliğine genel bakış](how-to-network-security-overview.md) makalesini okuyun.

+ İşlem kaynaklarınızla kullanılacak mevcut bir sanal ağ ve alt ağ.

+ Bir sanal ağa veya alt ağa kaynak dağıtmak için, Kullanıcı hesabınızın Azure rol tabanlı erişim denetimlerinde (RBAC) aşağıdaki eylemler için izinleri olmalıdır:

    - Sanal ağ kaynağında "Microsoft. Network/virtualNetworks/JOIN/Action".
    - Alt ağ kaynağında "Microsoft. Network/virtualNetworks/subnet/JOIN/Action".

    Ağ ile RBAC hakkında daha fazla bilgi için bkz. [ağ yerleşik rolleri](/azure/role-based-access-control/built-in-roles#networking)


## <a name="compute-clusters--instances"></a><a name="compute-instance"></a>İşlem kümeleri & örnekleri 

Bir sanal ağda [yönetilen Azure Machine Learning __işlem hedefi__ ](concept-compute-target.md#azure-machine-learning-compute-managed) veya [Azure Machine Learning işlem __örneği__ ](concept-compute-instance.md) kullanmak için aşağıdaki ağ gereksinimlerinin karşılanması gerekir:

> [!div class="checklist"]
> * Sanal ağın, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olması gerekir.
> * İşlem örneği veya küme için belirtilen alt ağda, hedeflenen sanal makine sayısına uyum sağlamak için yeterli sayıda atanmamış IP adresi olmalıdır. Alt ağda yeterli sayıda atanmamış IP adresi yoksa, bir işlem kümesi kısmen tahsis edilir.
> * Sanal ağın aboneliğine veya kaynak grubuna yönelik güvenlik ilkelerinizin veya kilitlerinizin sanal ağı yönetmek için izinleri kısıtlayıp kısıtlamamadığını denetleyin. Trafiği kısıtlayarak sanal ağın güvenliğini sağlamayı planlıyorsanız, bazı bağlantı noktalarını işlem hizmeti için açık bırakın. Daha fazla bilgi için, [gerekli bağlantı noktaları](#mlcports) bölümüne bakın.
> * Bir sanal ağa birden çok işlem örneği veya kümesi koyacaksanız bir veya daha fazla kaynağınız için bir kota artışı istemeniz gerekebilir.
> * Çalışma alanı için Azure depolama hesabı bir sanal ağda da güvenlik altına alınırsa, Azure Machine Learning işlem örneğiyle veya kümeyle aynı sanal ağda olmaları gerekir. 
> * İşlem örneği jupi işlevinin çalışması için, Web yuva iletişiminin devre dışı bırakılmadığından emin olun.

> [!TIP]
> Machine Learning işlem örneği veya kümesi, __sanal ağı içeren kaynak grubunda__ek ağ kaynaklarını otomatik olarak ayırır. Her işlem örneği veya kümesi için hizmet aşağıdaki kaynakları ayırır:
> 
> * Bir ağ güvenlik grubu
> * Bir genel IP adresi
> * Bir yük dengeleyici
> 
> Kümeler söz konusu olduğunda, küme 0 düğümlere her ölçeklendirilirken bu kaynaklar silinir (ve yeniden oluşturulur), ancak örnek için kaynaklar tamamen silinene kadar (durduruluyor, kaynakları kaldırmaz). 
> Bu kaynaklar, aboneliğin [kaynak kotalarıyla](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) sınırlıdır.


### <a name="required-ports"></a><a id="mlcports"></a> Gerekli bağlantı noktaları

Ağ trafiğini genel İnternet ile kısıtlayarak sanal ağın güvenliğini sağlamayı planlıyorsanız Azure Batch hizmetinden gelen iletişimlere izin vermeniz gerekir.

Batch hizmeti, sanal makinelere bağlı ağ arabirimi (NIC) düzeyinde ağ güvenlik grupları (NSG 'Ler) ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

- __Batchnodemanagement__ __hizmet etiketinden__ 29876 ve 29877 bağlantı noktalarında gelen TCP trafiği.

    ![BatchNodeManagement hizmet etiketini kullanan bir gelen kuralı](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- Seçim Uzaktan erişime izin vermek için 22 numaralı bağlantı noktasında gelen TCP trafiği. Bu bağlantı noktasını yalnızca genel IP üzerinde SSH kullanarak bağlanmak istiyorsanız kullanın.

- Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.

- İnternete giden herhangi bir bağlantı noktasında giden trafik.

- 44224 numaralı bağlantı noktasında işlem örneği gelen TCP trafiği için __AzureMachineLearning__bir __hizmet etiketinden__ .

> [!IMPORTANT]
> Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Bir NSG, işlem düğümleriyle iletişimi engelliyorsa, işlem hizmeti işlem düğümlerinin durumunu kullanılamıyor olarak ayarlar.
>
> Azure Batch hizmeti kendi NSG 'leri yapılandırdığından alt ağ düzeyinde NSG 'leri belirtmeniz gerekmez. Ancak, Azure Machine Learning işlem içeren alt ağda ilişkili NSG 'ler veya güvenlik duvarı varsa, daha önce listelenen trafiğe de izin vermeniz gerekir.

Azure portal NSG kural yapılandırması aşağıdaki görüntülerde gösterilmektedir:

:::image type="content" source="./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png" alt-text="Machine Learning İşlem gelen NSG kuralları" border="true":::



![Machine Learning İşlem için gelen NSG kuralları](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="limit-outbound-connectivity-from-the-virtual-network"></a><a id="limiting-outbound-from-vnet"></a> Sanal ağ ile giden bağlantıyı sınırlayın

Varsayılan giden kurallarını kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlandırmak istiyorsanız aşağıdaki adımları kullanın:

- NSG kurallarını kullanarak giden internet bağlantısını reddedin.

- Bir __işlem örneği__ veya bir __işlem kümesi__için, giden trafiği aşağıdaki öğelerle sınırlayın:
   - __Depolama. RegionName__ __hizmet etiketi__ kullanılarak Azure Storage. Burada `{RegionName}` bir Azure bölgesinin adıdır.
   - Azure Container Registry, __AzureContainerRegistry. RegionName__ __hizmet etiketi__ kullanılarak. Burada `{RegionName}` bir Azure bölgesinin adıdır.
   - __AzureMachineLearning__ __hizmet etiketi__ kullanılarak Azure Machine Learning
   - __AzureResourceManager__ __hizmet etiketi__ kullanılarak Azure Resource Manager
   - __AzureActiveDirectory__ __hizmet etiketi__ kullanılarak Azure Active Directory

Azure portal NSG kural yapılandırması aşağıdaki görüntüde gösterilmektedir:

[![Machine Learning İşlem giden NSG kuralları](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png#lightbox)

> [!NOTE]
> Microsoft tarafından sunulan varsayılan Docker görüntülerini kullanmayı ve Kullanıcı tarafından yönetilen bağımlılıkları etkinleştirmeyi planlıyorsanız, aşağıdaki __hizmet etiketlerini__de kullanmanız gerekir:
>
> * __MicrosoftContainerRegistry__
> * __Azurefrontkapısı. Firstpartisi__
>
> Bu yapılandırma, eğitim betiklerinizin bir parçası olarak aşağıdaki kod parçacıklarına benzer bir kodunuz olduğunda gereklidir:
>
> __RunConfig eğitimi__
> ```python
> # create a new runconfig object
> run_config = RunConfiguration()
> 
> # configure Docker 
> run_config.environment.docker.enabled = True
> # For GPU, use DEFAULT_GPU_IMAGE
> run_config.environment.docker.base_image = DEFAULT_CPU_IMAGE 
> run_config.environment.python.user_managed_dependencies = True
> ```
>
> __Estimator eğitimi__
> ```python
> est = Estimator(source_directory='.',
>                 script_params=script_params,
>                 compute_target='local',
>                 entry_script='dummy_train.py',
>                 user_managed=True)
> run = exp.submit(est)
> ```

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Azure Machine Learning işlem ile [Zorlamalı tünel](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm) kullanıyorsanız, genel İnternet ile işlem kaynağını içeren alt ağdan iletişime izin vermeniz gerekir. Bu iletişim, görev zamanlama ve Azure depolama erişimi için kullanılır.

Bunu yapmanın iki yolu vardır:

* Bir [sanal ağ NAT](../virtual-network/nat-overview.md)kullanın. Bir NAT ağ geçidi, sanal ağınızdaki bir veya daha fazla alt ağ için giden internet bağlantısı sağlar. Bilgi için bkz. [NAT Gateway kaynaklarıyla sanal ağları tasarlama](../virtual-network/nat-gateway-resource.md).

* İşlem kaynağını içeren alt ağa [Kullanıcı tanımlı yollar (UDRs)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) ekleyin. Kaynaklarınızın bulunduğu bölgedeki Azure Batch hizmeti tarafından kullanılan her IP adresi için bir UDR oluşturun. Bu UDRs, Batch hizmetinin görev zamanlama için işlem düğümleriyle iletişim kurmasını sağlar. Ayrıca, Işlem örneklerine erişim için gerekli olduğundan, kaynakların bulunduğu Azure Machine Learning hizmeti için IP adresini de ekleyin. Batch hizmetinin ve Azure Machine Learning hizmetinin IP adreslerinin bir listesini almak için aşağıdaki yöntemlerden birini kullanın:

    * [Azure IP aralıklarını ve hizmet etiketlerini](https://www.microsoft.com/download/details.aspx?id=56519) indirin ve dosyada `BatchNodeManagement.<region>` ve `AzureMachineLearning.<region>` Azure bölgeniz olduğu yerde arama yapın `<region>` .

    * Bilgileri indirmek için [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 'yi kullanın. Aşağıdaki örnek, IP adresi bilgilerini indirir ve Doğu ABD 2 bölgesinin bilgilerini filtreler:

        ```azurecli-interactive
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'AzureMachineLearning')] | [?properties.region=='eastus2']"
        ```

        > [!TIP]
        > US-Virginia, US-Arizlonregion veya Çin-Doğu-2 bölgelerini kullanıyorsanız, bu komutlar IP adresi döndürmez. Bunun yerine, IP adreslerinin bir listesini indirmek için aşağıdaki bağlantılardan birini kullanın:
        >
        > * [Azure Kamu için Azure IP aralıkları ve hizmet etiketleri](https://www.microsoft.com/download/details.aspx?id=57063)
        > * [Azure Çin için Azure IP aralıkları ve hizmet etiketleri](https://www.microsoft.com//download/details.aspx?id=57062)
    
    UDRs 'yi eklediğinizde, ilgili her Batch IP adresi ön eki için yolu tanımlayın ve __sonraki atlama türünü__ __Internet__olarak ayarlayın. Aşağıdaki görüntüde, Azure portal bu UDR 'nin bir örneği gösterilmektedir:

    ![Adres ön eki için UDR örneği](./media/how-to-enable-virtual-network/user-defined-route.png)

    > [!IMPORTANT]
    > IP adresleri zaman içinde değişebilir.

    Tanımladığınız tüm UDRs 'ye ek olarak, şirket içi ağ gereciniz aracılığıyla Azure Storage 'a giden trafiğe izin verilmelidir. Özellikle, bu trafiğin URL 'Leri şu formlarda bulunur: `<account>.table.core.windows.net` , `<account>.queue.core.windows.net` ve `<account>.blob.core.windows.net` . 

    Daha fazla bilgi için bkz. [Sanal ağda Azure Batch havuzu oluşturma](../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling).


### <a name="create-a-compute-cluster-in-a-virtual-network"></a>Sanal ağda bir işlem kümesi oluşturma

Machine Learning İşlem kümesi oluşturmak için aşağıdaki adımları kullanın:

1. [Azure Machine Learning Studio](https://ml.azure.com/)' da oturum açın ve ardından aboneliğinizi ve çalışma alanınızı seçin.

1. Sol tarafta __işlem__ ' ı seçin.

1. Merkezden __eğitim kümeleri__ ' ni seçin ve ardından öğesini seçin __+__ .

1. __Yeni eğitim kümesi__ Iletişim kutusunda __Gelişmiş ayarlar__ bölümünü genişletin.

1. Bu işlem kaynağını bir sanal ağ kullanacak şekilde yapılandırmak için, __sanal ağı Yapılandır__ bölümünde aşağıdaki işlemleri gerçekleştirin:

    1. __Kaynak grubu__ açılır listesinde, sanal ağı içeren kaynak grubunu seçin.
    1. __Sanal ağ__ açılan listesinde, alt ağı içeren sanal ağı seçin.
    1. __Alt ağ__ açılan listesinde, kullanılacak alt ağı seçin.

   ![Machine Learning İşlem için sanal ağ ayarları](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Ayrıca, Azure Machine Learning SDK kullanarak bir Machine Learning İşlem kümesi de oluşturabilirsiniz. Aşağıdaki kod, `default` adlı bir sanal ağın alt ağında yeni bir Machine Learning işlem kümesi oluşturur `mynetwork` :

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

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Oluşturma işlemi tamamlandığında, bir deneyde kümeyi kullanarak modelinizi eğitebilirsiniz. Daha fazla bilgi için bkz. [eğitim için bir işlem hedefi seçme ve kullanma](how-to-set-up-training-targets.md).

[!INCLUDE [low-pri-note](../../includes/machine-learning-low-pri-vm.md)]

### <a name="access-data-in-a-compute-instance-notebook"></a>Işlem örneği not defterindeki verilere erişme

Azure Işlem örneği üzerinde not defterleri kullanıyorsanız, not defterinizin, verileriniz ile aynı sanal ağın ve alt ağın arkasındaki bir işlem kaynağında çalıştığından emin olmanız gerekir. 

İşlem örneğinizi oluşturma sırasında aynı sanal ağ içinde olacak şekilde yapılandırmanız gerekir, **Gelişmiş ayarlar**altında  >  **sanal ağı yapılandırın**. Mevcut bir Işlem örneğini bir sanal ağa ekleyemezsiniz.

## <a name="azure-databricks"></a>Azure Databricks

Çalışma alanınızdaki bir sanal ağda Azure Databricks kullanmak için aşağıdaki gereksinimlerin karşılanması gerekir:

> [!div class="checklist"]
> * Sanal ağın, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olması gerekir.
> * Çalışma alanı için Azure depolama hesabı bir sanal ağda da güvenlik altına alınırsa, Azure Databricks kümesiyle aynı sanal ağda olmaları gerekir.
> * Azure Databricks tarafından kullanılan __databricks-Private__ ve __databricks-genel__ alt ağlarına ek olarak, sanal ağ için oluşturulan __varsayılan__ alt ağ de gereklidir.

Bir sanal ağla Azure Databricks kullanma hakkında ayrıntılı bilgi için bkz. [Azure sanal ağınızda Azure Databricks dağıtma](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html).

<a id="vmorhdi"></a>

## <a name="virtual-machine-or-hdinsight-cluster"></a>Sanal makine veya HDInsight kümesi

> [!IMPORTANT]
> Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler.

Bu bölümde, bir sanal makineyi veya Azure HDInsight kümesini çalışma alanınıza sahip bir sanal ağda nasıl kullanacağınızı öğreneceksiniz.

### <a name="create-the-vm-or-hdinsight-cluster"></a>VM veya HDInsight kümesi oluşturma

Azure portal veya Azure CLı kullanarak bir VM veya HDInsight kümesi oluşturun ve kümeyi bir Azure sanal ağına yerleştirin. Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
* [Linux VM 'Ler için Azure sanal ağları oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

* [Azure sanal ağını kullanarak HDInsight 'ı genişletme](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

### <a name="configure-network-ports"></a>Ağ bağlantı noktalarını yapılandırma 

Azure Machine Learning VM veya kümedeki SSH bağlantı noktasıyla iletişim kurmasına izin ver, ağ güvenlik grubu için bir kaynak girişi yapılandırın. SSH bağlantı noktası genellikle bağlantı noktası 22 ' dir. Bu kaynaktan gelen trafiğe izin vermek için aşağıdaki işlemleri yapın:

1. __Kaynak__ açılan listesinde __hizmet etiketi__' ni seçin.

1. __Kaynak hizmet etiketi__ açılan listesinde __AzureMachineLearning__' yi seçin.

    ![Bir sanal ağ içindeki bir VM veya HDInsight kümesi üzerinde deneme yapmak için gelen kurallar](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

1. __Kaynak bağlantı noktası aralıkları__ açılan listesinde, öğesini seçin __*__ .

1. __Hedef__ açılan listesinde __herhangi birini__seçin.

1. __Hedef bağlantı noktası aralıkları__ aşağı açılan listesinde __22__' yi seçin.

1. __Protokol__altında __herhangi bir__seçin.

1. __Eylem__altında __izin ver__' i seçin.

Ağ güvenlik grubu için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.

Varsayılan giden kurallarını kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlandırmak istiyorsanız, [sanal ağ ile giden bağlantıyı sınırla](#limiting-outbound-from-vnet) bölümüne bakın.

### <a name="attach-the-vm-or-hdinsight-cluster"></a>VM veya HDInsight kümesini iliştirme

Azure Machine Learning çalışma alanınıza VM veya HDInsight kümesini ekleyin. Daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, dört bölümlü bir sanal ağ serisinde üç bölümden oluşan bir parçasıdır. Bir sanal ağın güvenliğini nasıl sağlayacağınızı öğrenmek için makalelerin geri kalanına bakın:

* [1. kısım: sanal ağa genel bakış](how-to-network-security-overview.md)
* [2. Bölüm: çalışma alanı kaynaklarını güvenli hale getirme](how-to-secure-workspace-vnet.md)
* [4. Bölüm: ikinci dereceden sınırlama ortamının güvenliğini sağlama](how-to-secure-inferencing-vnet.md)
* [5. Bölüm: Studio işlevselliğini etkinleştirme](how-to-enable-studio-virtual-network.md)