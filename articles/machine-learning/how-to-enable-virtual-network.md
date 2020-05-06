---
title: Sanal ağda güvenli denemeleri ve çıkarım
titleSuffix: Azure Machine Learning
description: bir Azure sanal ağı içindeki Azure Machine Learning deneme/eğitim işlerinin ve çıkarım/Puanlama işlerinin güvenliğini nasıl sağlayacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 04/17/2020
ms.openlocfilehash: 5e4f811d39c75742f11c52de5c178fbf4063000d
ms.sourcegitcommit: 602e6db62069d568a91981a1117244ffd757f1c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82864649"
---
# <a name="secure-azure-ml-experimentation-and-inference-jobs-within-an-azure-virtual-network"></a>Azure sanal ağı içindeki Azure ML deneme ve çıkarım işlerinin güvenliğini sağlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bir Azure sanal ağı (VNet) içinde Azure Machine Learning deneme/eğitim işlerinin ve çıkarım/Puanlama işlerinin güvenliğini nasıl sağlayacağınızı öğreneceksiniz.

Bir **sanal ağ** , Azure kaynaklarınızı genel İnternet 'ten yalıtmak için bir güvenlik sınırı görevi görür. Ayrıca, bir Azure sanal ağını şirket içi ağınıza da katabilirsiniz. Ağları birleştirerek, modellerinizi güvenli bir şekilde eğitebilir ve çıkarım için dağıtılan modellerinize erişebilirsiniz.

Azure Machine Learning, işlem kaynakları için diğer Azure hizmetlerini kullanır. İşlem kaynakları veya [işlem hedefleri](concept-compute-target.md), modelleri eğitmek ve dağıtmak için kullanılır. Hedefler bir sanal ağ içinde oluşturulabilir. Örneğin, bir modeli eğitme ve modeli Azure Kubernetes Service (AKS) ' e dağıtmak için Microsoft Veri Bilimi Sanal Makinesi kullanabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için bkz. [Azure sanal ağa genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

Bu makalede, *Gelişmiş güvenlik ayarları*, temel veya Deneysel kullanım durumları için gerekli olmayan bilgiler hakkında ayrıntılı bilgiler de sağlanmaktadır. Bu makalenin bazı bölümleri çeşitli senaryolar için yapılandırma bilgileri sağlar. Yönergeleri sırasıyla veya tamamen doldurmanız gerekmez.

> [!TIP]
> Özellikle, bir sanal ağ içindeki depolama hesapları veya işlem hedefleri gibi kaynakları kullanmak, makine öğrenimi ardışık düzenleri ve betik çalıştırmaları gibi işlem hattı olmayan iş akışlarıyla birlikte çalışacaktır.

> [!WARNING]
> Microsoft, temeldeki depolamanın sanal ağı etkinse otomatik ML, veri kümeleri, veri etiketleme, tasarımcı ve Not defterleri gibi Azure Machine Learning Studio özelliklerinin kullanılmasını desteklemez.

## <a name="prerequisites"></a>Ön koşullar

+ Azure Machine Learning [çalışma alanı](how-to-manage-workspace.md).

+ [Azure sanal ağ hizmeti](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ve [IP ağının](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)genel çalışma bilgileri.

+ İşlem kaynaklarınızla kullanılacak önceden var olan bir sanal ağ ve alt ağ.

## <a name="use-a-storage-account-for-your-workspace"></a>Çalışma alanınız için bir depolama hesabı kullanın

> [!WARNING]
> Azure Machine Learning tasarımcısını kullanan veri bilimcileri varsa, bir sanal ağ içindeki depolama hesabından verileri görselleştirirken bir hata alırlar. Şu metin, aldıkları hatadır:
>
> __Hata: Bu veri kümesi profili oluşturulamıyor. Bunun nedeni verilerinizin bir sanal ağın arkasında depolanması veya verilerinizin profili desteklememesi olabilir.__

Bir sanal ağdaki çalışma alanı için bir Azure depolama hesabı kullanmak için aşağıdaki adımları kullanın:

1. Bir sanal ağın arkasında bir işlem kaynağı (örneğin, bir Machine Learning işlem örneği veya kümesi) oluşturun veya çalışma alanına (örneğin, bir HDInsight kümesi, sanal makine veya Azure Kubernetes hizmet kümesi) bir işlem kaynağı ekleyin. İşlem kaynağı deneme veya model dağıtımı için olabilir.

   Daha fazla bilgi için, bu makaledeki [Machine Learning Işlem kullanma](#amlcompute), [sanal makine veya HDInsight kümesi](#vmorhdi)kullanma ve [Azure Kubernetes hizmet](#aksvnet) bölümlerini kullanma bölümüne bakın.

1. Azure portal, çalışma alanınıza bağlı depolama alanına gidin.

   [![Azure Machine Learning çalışma alanına bağlı depolama alanı](./media/how-to-enable-virtual-network/workspace-storage.png)](./media/how-to-enable-virtual-network/workspace-storage.png#lightbox)

1. **Azure depolama** sayfasında __güvenlik duvarları ve sanal ağlar__' ı seçin.

   ![Azure portal Azure Storage sayfasındaki "güvenlik duvarları ve sanal ağlar" alanı](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)

1. __Güvenlik duvarları ve sanal ağlar__ sayfasında, aşağıdaki işlemleri yapın:
    - __Seçili ağlar__'ı seçin.
    - __Sanal ağlar__altında __var olan sanal ağ ekle__ bağlantısını seçin. Bu eylem, işlemin bulunduğu sanal ağı ekler (bkz. 1. adım).

        > [!IMPORTANT]
        > Depolama hesabı, eğitim veya çıkarım için kullanılan işlem örnekleri veya kümeler ile aynı sanal ağ ve alt ağ içinde olmalıdır.

    - __Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine Izin ver__ onay kutusunu seçin.

    > [!IMPORTANT]
    > Azure Machine Learning SDK ile çalışırken, geliştirme ortamınızın Azure depolama hesabına bağlanabiliyor olması gerekir. Depolama hesabı bir sanal ağın içindeyken güvenlik duvarının, geliştirme ortamının IP adresinden erişime izin vermelidir.
    >
    > Depolama hesabına erişimi etkinleştirmek için, *geliştirme istemcisinde bir Web tarayıcısından*depolama hesabı Için __güvenlik duvarlarını ve sanal ağları__ ziyaret edin. Ardından, istemcinin IP adresini __adres aralığına__eklemek IÇIN __Istemci IP adresini ekle__ onay kutusunu kullanın. Ayrıca, geliştirme ortamının IP adresini el ile girmek için __adres aralığı__ alanını da kullanabilirsiniz. İstemcinin IP adresi eklendikten sonra SDK 'Yı kullanarak depolama hesabına erişebilir.

   [![Azure portal "güvenlik duvarları ve sanal ağlar" bölmesi](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png#lightbox)

> [!IMPORTANT]
> Bir sanal ağda Azure Machine Learning veya _varsayılan olmayan depolama hesapları_ için hem _varsayılan depolama hesabını_ yerleştirebilirsiniz.
>
> Varsayılan depolama hesabı, bir çalışma alanı oluşturduğunuzda otomatik olarak sağlanır.
>
> Varsayılan olmayan depolama hesapları için, `storage_account` [ `Workspace.create()` işlevindeki](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) parametresi Azure kaynak kimliği 'ne göre özel bir depolama hesabı belirtmenizi sağlar.

## <a name="use-azure-data-lake-storage-gen-2"></a>Azure Data Lake Storage Gen 2 kullanın

Azure Data Lake Storage Gen 2, Azure Blob depolama üzerinde oluşturulmuş, büyük veri analizi için bir dizi yetenektir. Modelleri Azure Machine Learning ile eğitmek için kullanılan verileri depolamak için kullanılabilir. 

Azure Machine Learning çalışma alanınızın sanal ağı içinde Data Lake Storage Gen 2 ' yi kullanmak için aşağıdaki adımları kullanın:

1. Azure Data Lake Storage Gen 2 hesabı oluşturun. Daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Storage hesabı oluşturma](../storage/blobs/data-lake-storage-quickstart-create-account.md).

1. Hesabı sanal ağa yerleştirmek için önceki bölümde bulunan 2-4 adımlarını kullanın, [çalışma alanınız için bir depolama hesabı kullanın](#use-a-storage-account-for-your-workspace).

Bir sanal ağ içinde Data Lake Storage Gen 2 ile Azure Machine Learning kullanırken aşağıdaki kılavuzu kullanın:

* __Bir veri kümesi oluşturmak Için SDK__'yı kullanırsanız ve kodu çalıştıran sistem __Sanal ağda değilse__, `validate=False` parametresini kullanın. Bu parametre, sistem depolama hesabı ile aynı sanal ağda değilse başarısız olan doğrulamayı atlar. Daha fazla bilgi için [from_files ()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) yöntemine bakın.

* Veri kümesini kullanarak bir modeli eğitmek için Azure Machine Learning Işlem örneği veya işlem kümesi kullanırken, depolama hesabı ile aynı sanal ağda olması gerekir.

## <a name="use-a-key-vault-instance-with-your-workspace"></a>Çalışma alanınız ile bir Anahtar Kasası örneği kullanın

Çalışma alanıyla ilişkili anahtar kasası örneği, Azure Machine Learning tarafından aşağıdaki kimlik bilgilerini depolamak için kullanılır:
* İlişkili depolama hesabı bağlantı dizesi
* Azure Container Repository deposu örneklerine parolalar
* Veri depolarına yönelik bağlantı dizeleri

Bir sanal ağın arkasındaki Azure Key Vault Azure Machine Learning deneme yeteneklerini kullanmak için aşağıdaki adımları kullanın:

1. Çalışma alanıyla ilişkili olan anahtar kasasına gidin.

   [![Azure Machine Learning çalışma alanıyla ilişkili olan Anahtar Kasası](./media/how-to-enable-virtual-network/workspace-key-vault.png)](./media/how-to-enable-virtual-network/workspace-key-vault.png#lightbox)

1. **Key Vault** sayfasında, sol bölmede __güvenlik duvarları ve sanal ağlar__' ı seçin.

   ![Key Vault bölmesindeki "güvenlik duvarları ve sanal ağlar" bölümü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. __Güvenlik duvarları ve sanal ağlar__ sayfasında, aşağıdaki işlemleri yapın:
    - __Erişime Izin ver__' ın altında __Seçili ağlar__' ı seçin.
    - __Sanal ağlar__altında, deneme hesapla'nizin bulunduğu sanal ağı eklemek için __var olan sanal ağları Ekle__ ' yi seçin.
    - __Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver__altında __Evet__' i seçin.

   [![Key Vault bölmesindeki "güvenlik duvarları ve sanal ağlar" bölümü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png#lightbox)

<a id="amlcompute"></a>

## <a name="use-a-machine-learning-compute"></a><a name="compute-instance"></a>Machine Learning İşlem kullanma

Bir sanal ağda Azure Machine Learning işlem örneği veya işlem kümesi kullanmak için aşağıdaki ağ gereksinimlerinin karşılanması gerekir:

> [!div class="checklist"]
> * Sanal ağın, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olması gerekir.
> * İşlem örneği veya küme için belirtilen alt ağda, hedeflenen sanal makine sayısına uyum sağlamak için yeterli sayıda atanmamış IP adresi olmalıdır. Alt ağda yeterli sayıda atanmamış IP adresi yoksa, bir işlem kümesi kısmen tahsis edilir.
> * Sanal ağın aboneliğine veya kaynak grubuna yönelik güvenlik ilkelerinizin veya kilitlerinizin sanal ağı yönetmek için izinleri kısıtlayıp kısıtlamamadığını denetleyin. Trafiği kısıtlayarak sanal ağın güvenliğini sağlamayı planlıyorsanız, bazı bağlantı noktalarını işlem hizmeti için açık bırakın. Daha fazla bilgi için, [gerekli bağlantı noktaları](#mlcports) bölümüne bakın.
> * Bir sanal ağa birden çok işlem örneği veya kümesi koyacaksanız bir veya daha fazla kaynağınız için bir kota artışı istemeniz gerekebilir.
> * Çalışma alanı için Azure depolama hesabı bir sanal ağda da güvenlik altına alınırsa, Azure Machine Learning işlem örneğiyle veya kümeyle aynı sanal ağda olmaları gerekir. 

> [!TIP]
> Machine Learning işlem örneği veya kümesi, __sanal ağı içeren kaynak grubunda__ek ağ kaynaklarını otomatik olarak ayırır. Her işlem örneği veya kümesi için hizmet aşağıdaki kaynakları ayırır:
> 
> * Bir ağ güvenlik grubu
> * Bir genel IP adresi
> * Bir yük dengeleyici
> 
> Kümeler söz konusu olduğunda, küme 0 düğümlere her ölçeklendirilirken bu kaynaklar silinir (ve yeniden oluşturulur), ancak örnek için kaynaklar tamamen silinene kadar (durduruluyor, kaynakları kaldırmaz). 
> Bu kaynaklar, aboneliğin [kaynak kotalarıyla](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) sınırlıdır.


### <a name="required-ports"></a><a id="mlcports"></a>Gerekli bağlantı noktaları

Machine Learning İşlem şu anda, belirtilen sanal ağda VM 'Leri sağlamak için Azure Batch hizmetini kullanıyor. Alt ağ, Batch hizmetinden gelen iletişime izin vermelidir. Bu iletişimi, Machine Learning İşlem düğümlerinde çalıştırma zamanlamak ve Azure depolama ile diğer kaynaklarla iletişim kurmak için kullanırsınız. Batch hizmeti, sanal makinelere bağlı ağ arabirimi (NIC) düzeyinde ağ güvenlik grupları (NSG 'Ler) ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

- __Batchnodemanagement__ __hizmet etiketinden__ 29876 ve 29877 bağlantı noktalarında gelen TCP trafiği.

    ![BatchNodeManagement hizmet etiketini kullanan bir gelen kuralı](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- Seçim Uzaktan erişime izin vermek için 22 numaralı bağlantı noktasında gelen TCP trafiği. Bu bağlantı noktasını yalnızca genel IP üzerinde SSH kullanarak bağlanmak istiyorsanız kullanın.

- Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.

- İnternete giden herhangi bir bağlantı noktasında giden trafik.

- 44224 numaralı bağlantı noktasında işlem örneği gelen TCP trafiği için __AzureMachineLearning__bir __hizmet etiketinden__ .

Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Bir NSG, işlem düğümleriyle iletişimi engelliyorsa, işlem hizmeti işlem düğümlerinin durumunu kullanılamıyor olarak ayarlar.

Azure Batch hizmeti kendi NSG 'leri yapılandırdığından alt ağ düzeyinde NSG 'leri belirtmeniz gerekmez. Bununla birlikte, belirtilen alt ağda ilişkili NSG 'ler veya güvenlik duvarı varsa, gelen ve giden güvenlik kurallarını daha önce belirtilen şekilde yapılandırın.

Azure portal NSG kural yapılandırması aşağıdaki görüntülerde gösterilmektedir:

:::image type="content" source="./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png" alt-text="Machine Learning İşlem gelen NSG kuralları" border="true":::



![Machine Learning İşlem giden NSG kuralları](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="limit-outbound-connectivity-from-the-virtual-network"></a><a id="limiting-outbound-from-vnet"></a>Sanal ağ ile giden bağlantıyı sınırlayın

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
> Microsoft tarafından sunulan varsayılan Docker görüntülerini kullanmayı ve Kullanıcı tarafından yönetilen bağımlılıkları etkinleştirmeyi planlıyorsanız, __Microsoftcontainerregistry. Region_Name__ (örneğin, Microsoftcontainerregistry. EastUS) Için bir __hizmet etiketi__ de kullanmanız gerekir.
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

### <a name="user-defined-routes-for-forced-tunneling"></a>Zorlamalı tünel için Kullanıcı tanımlı yollar

Machine Learning İşlem ile Zorlamalı tünel kullanıyorsanız, [Kullanıcı tanımlı yollar (UDRs)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) işlem kaynağını içeren alt ağa ekleyin.

* Kaynaklarınızın bulunduğu bölgedeki Azure Batch hizmeti tarafından kullanılan her IP adresi için bir UDR oluşturun. Bu UDRs, Batch hizmetinin görev zamanlama için işlem düğümleriyle iletişim kurmasını sağlar. Ayrıca, Işlem örneklerine erişim için gerekli olduğundan, kaynakların bulunduğu Azure Machine Learning hizmeti için IP adresini de ekleyin. Batch hizmetinin ve Azure Machine Learning hizmetinin IP adreslerinin bir listesini almak için aşağıdaki yöntemlerden birini kullanın:

    * [Azure IP aralıklarını ve hizmet etiketlerini](https://www.microsoft.com/download/details.aspx?id=56519) indirin ve dosyada `BatchNodeManagement.<region>` ve `AzureMachineLearning.<region>`Azure bölgeniz olduğu yerde `<region>` arama yapın.

    * Bilgileri indirmek için [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 'yi kullanın. Aşağıdaki örnek, IP adresi bilgilerini indirir ve Doğu ABD 2 bölgesinin bilgilerini filtreler:

        ```azurecli-interactive
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'AzureMachineLearning')] | [?properties.region=='eastus2']"
        ```

* Azure depolama 'ya giden trafik, şirket içi ağ gereciniz tarafından engellenmemelidir. Özellikle, URL 'ler, ve `<account>.table.core.windows.net` `<account>.queue.core.windows.net` `<account>.blob.core.windows.net`biçiminde bulunur.

UDRs 'yi eklediğinizde, ilgili her Batch IP adresi ön eki için yolu tanımlayın ve __sonraki atlama türünü__ __Internet__olarak ayarlayın. Aşağıdaki görüntüde, Azure portal bu UDR 'nin bir örneği gösterilmektedir:

![Adres ön eki için UDR örneği](./media/how-to-enable-virtual-network/user-defined-route.png)

Daha fazla bilgi için bkz. [Sanal ağda Azure Batch havuzu oluşturma](../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling).

### <a name="create-a-compute-cluster-in-a-virtual-network"></a>Sanal ağda bir işlem kümesi oluşturma

Machine Learning İşlem kümesi oluşturmak için aşağıdaki adımları kullanın:

1. [Azure Machine Learning Studio](https://ml.azure.com/)' da oturum açın ve ardından aboneliğinizi ve çalışma alanınızı seçin.

1. Sol tarafta __işlem__ ' ı seçin.

1. Merkezden __eğitim kümeleri__ ' ni seçin ve ardından öğesini seçin __+__.

1. __Yeni eğitim kümesi__ Iletişim kutusunda __Gelişmiş ayarlar__ bölümünü genişletin.

1. Bu işlem kaynağını bir sanal ağ kullanacak şekilde yapılandırmak için, __sanal ağı Yapılandır__ bölümünde aşağıdaki işlemleri gerçekleştirin:

    1. __Kaynak grubu__ açılır listesinde, sanal ağı içeren kaynak grubunu seçin.
    1. __Sanal ağ__ açılan listesinde, alt ağı içeren sanal ağı seçin.
    1. __Alt ağ__ açılan listesinde, kullanılacak alt ağı seçin.

   ![Machine Learning İşlem için sanal ağ ayarları](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

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

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Oluşturma işlemi tamamlandığında, bir deneyde kümeyi kullanarak modelinizi eğitebilirsiniz. Daha fazla bilgi için bkz. [eğitim için bir işlem hedefi seçme ve kullanma](how-to-set-up-training-targets.md).

## <a name="use-azure-databricks"></a>Azure Databricks kullan

Çalışma alanınızdaki bir sanal ağda Azure Databricks kullanmak için aşağıdaki gereksinimlerin karşılanması gerekir:

> [!div class="checklist"]
> * Sanal ağın, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olması gerekir.
> * Çalışma alanı için Azure depolama hesabı bir sanal ağda da güvenlik altına alınırsa, Azure Databricks kümesiyle aynı sanal ağda olmaları gerekir.
> * Azure Databricks tarafından kullanılan __databricks-Private__ ve __databricks-genel__ alt ağlarına ek olarak, sanal ağ için oluşturulan __varsayılan__ alt ağ de gereklidir.

Bir sanal ağla Azure Databricks kullanma hakkında ayrıntılı bilgi için bkz. [Azure sanal ağınızda Azure Databricks dağıtma](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html).

<a id="vmorhdi"></a>

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Bir sanal makine veya HDInsight kümesi kullanma

> [!IMPORTANT]
> Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler.

Sanal bir makineyi veya Azure HDInsight kümesini çalışma alanınıza sahip bir sanal ağda kullanmak için aşağıdaki adımları kullanın:

1. Azure portal veya Azure CLı kullanarak bir VM veya HDInsight kümesi oluşturun ve kümeyi bir Azure sanal ağına yerleştirin. Daha fazla bilgi için aşağıdaki makalelere bakın:
    * [Linux VM 'Ler için Azure sanal ağları oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Azure sanal ağını kullanarak HDInsight 'ı genişletme](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Azure Machine Learning, VM veya kümedeki SSH bağlantı noktasıyla iletişim kurmasına izin vermek için, ağ güvenlik grubu için bir kaynak girişi yapılandırın. SSH bağlantı noktası genellikle bağlantı noktası 22 ' dir. Bu kaynaktan gelen trafiğe izin vermek için aşağıdaki işlemleri yapın:

    * __Kaynak__ açılan listesinde __hizmet etiketi__' ni seçin.

    * __Kaynak hizmet etiketi__ açılan listesinde __AzureMachineLearning__' yi seçin.

    * __Kaynak bağlantı noktası aralıkları__ açılan listesinde, öğesini seçin __*__.

    * __Hedef__ açılan listesinde __herhangi birini__seçin.

    * __Hedef bağlantı noktası aralıkları__ aşağı açılan listesinde __22__' yi seçin.

    * __Protokol__altında __herhangi bir__seçin.

    * __Eylem__altında __izin ver__' i seçin.

   ![Bir sanal ağ içindeki bir VM veya HDInsight kümesi üzerinde deneme yapmak için gelen kurallar](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Ağ güvenlik grubu için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.

    Varsayılan giden kurallarını kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlandırmak istiyorsanız, [sanal ağ ile giden bağlantıyı sınırla](#limiting-outbound-from-vnet) bölümüne bakın.

1. Azure Machine Learning çalışma alanınıza VM veya HDInsight kümesini ekleyin. Daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md).

<a id="aksvnet"></a>

## <a name="use-azure-kubernetes-service-aks"></a>Azure Kubernetes hizmetini (AKS) kullanma

Bir sanal ağdaki AKS 'leri çalışma alanınıza eklemek için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Aşağıdaki yordama başlamadan önce, [Azure Kubernetes Service (AKS) içindeki gelişmiş ağı yapılandırma (aks)](https://docs.microsoft.com/azure/aks/configure-azure-cni#prerequisites) ve kümenizin IP adreslemesini planlayın bölümündeki önkoşulları izleyin.
>
> AKS örneği ve Azure sanal ağı aynı bölgede olmalıdır. Bir sanal ağ içinde çalışma alanı tarafından kullanılan Azure depolama hesaplarını güvenli hale getirmek istiyorsanız, AKS örneğiyle aynı sanal ağda olmaları gerekir.

> [!WARNING]
> Azure Machine Learning, özel bağlantısı etkin olan bir Azure Kubernetes hizmetini kullanmayı desteklemez.

1. [Azure Machine Learning Studio](https://ml.azure.com/)' da oturum açın ve ardından aboneliğinizi ve çalışma alanınızı seçin.

1. Sol tarafta __işlem__ ' ı seçin.

1. Merkezden __çıkarım kümeleri__ ' ni seçin ve ardından öğesini __+__ seçin.

1. __Yeni çıkarım kümesi__ Iletişim kutusunda __ağ yapılandırması__altında __Gelişmiş__ ' i seçin.

1. Bu işlem kaynağını bir sanal ağ kullanacak şekilde yapılandırmak için aşağıdaki eylemleri gerçekleştirin:

    1. __Kaynak grubu__ açılır listesinde, sanal ağı içeren kaynak grubunu seçin.
    1. __Sanal ağ__ açılan listesinde, alt ağı içeren sanal ağı seçin.
    1. __Alt ağ__ açılan listesinde, alt ağı seçin.
    1. __Kubernetes hizmeti adres aralığı__ kutusuna Kubernetes hizmeti adres aralığını girin. Bu adres aralığı, küme için kullanılabilir IP adreslerini tanımlamak üzere sınıfsız etki alanları arası yönlendirme (CıDR) gösterimi IP aralığını kullanır. Herhangi bir alt ağ IP aralığı ile çakışmamalıdır (örneğin, 10.0.0.0/16).
    1. __KUBERNETES DNS HIZMETI IP adresi__ kutusuna Kubernetes DNS hizmeti IP adresini girin. Bu IP adresi, Kubernetes DNS hizmetine atanır. Bu, Kubernetes hizmeti adres aralığı içinde olmalıdır (örneğin, 10.0.0.10).
    1. __Docker köprü adresi__ kutusuna Docker köprü adresini girin. Bu IP adresi Docker köprüsüne atandı. Herhangi bir alt ağ IP aralığında olmamalı veya Kubernetes hizmeti adres aralığı (örneğin, 172.17.0.1/16) olmalıdır.

   ![Azure Machine Learning: Machine Learning İşlem sanal ağ ayarları](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Sanal ağı denetleyen NSG grubunun, sanal ağ dışından çağrılabilmesi için Puanlama uç noktası için etkin bir gelen güvenlik kuralına sahip olduğundan emin olun.
   > [!IMPORTANT]
   > NSG için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.

   [![Bir gelen güvenlik kuralı](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

Azure Kubernetes hizmetini bir sanal ağa eklemek için Azure Machine Learning SDK 'sını de kullanabilirsiniz. Bir sanal ağda zaten bir AKS kümeniz varsa, bunları [aks 'e dağıtma](how-to-deploy-and-where.md)bölümünde açıklandığı gibi çalışma alanına ekleyin. Aşağıdaki kod, adlı `default` `mynetwork`bir sanal ağın alt ağında yeni bir aks örneği oluşturur:

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

Oluşturma işlemi tamamlandığında, bir sanal ağın arkasındaki AKS kümesinde çıkarım veya model Puanlama çalıştırabilirsiniz. Daha fazla bilgi için bkz. [AKS 'e dağıtma](how-to-deploy-and-where.md).

### <a name="use-private-ips-with-azure-kubernetes-service"></a>Azure Kubernetes hizmeti ile özel IP 'Leri kullanma

Varsayılan olarak, AKS dağıtımlarına genel bir IP adresi atanır. Bir sanal ağ içinde AKS kullanırken, bunun yerine özel bir IP adresi kullanabilirsiniz. Özel IP adreslerine yalnızca sanal ağ veya birleştirilmiş ağların içinden erişilebilir.

Özel bir IP adresi, bir _iç yük dengeleyici_kullanmak üzere aks 'ler yapılandırılarak etkinleştirilir. 

> [!IMPORTANT]
> Azure Kubernetes hizmet kümesini oluştururken özel IP 'yi etkinleştiremezsiniz. Mevcut bir kümeye güncelleştirme olarak etkinleştirilmelidir.

Aşağıdaki kod parçacığı, **Yeni BIR AKS kümesi oluşturmayı**ve ardından özel bir IP/iç yük dengeleyici kullanmak üzere güncelleştirmeyi göstermektedir:

```python
import azureml.core
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute import AksCompute, ComputeTarget

# Verify that cluster does not exist already
try:
    aks_target = AksCompute(workspace=ws, name=aks_cluster_name)
    print("Found existing aks cluster")

except:
    print("Creating new aks cluster")

    # Subnet to use for AKS
    subnet_name = "default"
    # Create AKS configuration
    prov_config = AksCompute.provisioning_configuration(location = "eastus2")
    # Set info for existing virtual network to create the cluster in
    prov_config.vnet_resourcegroup_name = "myvnetresourcegroup"
    prov_config.vnet_name = "myvnetname"
    prov_config.service_cidr = "10.0.0.0/16"
    prov_config.dns_service_ip = "10.0.0.10"
    prov_config.subnet_name = subnet_name
    prov_config.docker_bridge_cidr = "172.17.0.1/16"

    # Create compute target
    aks_target = ComputeTarget.create(workspace = ws, name = "myaks", provisioning_configuration = prov_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
    
    # Update AKS configuration to use an internal load balancer
    update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
    aks_target.update(update_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
```

__Azure CLI__

```azurecli-interactive
az rest --method put --uri https://management.azure.com"/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-id>?api-version=2018-11-19 --body @body.json
```

Komutu tarafından başvurulan `body.json` dosyanın IÇERIĞI aşağıdaki JSON belgesine benzer:

```json
{ 
    "location": "<region>", 
    "properties": { 
        "resourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-id>", 
        "computeType": "AKS", 
        "provisioningState": "Succeeded", 
        "properties": { 
            "loadBalancerType": "InternalLoadBalancer", 
            "agentCount": <agent-count>, 
            "agentVmSize": "vm-size", 
            "clusterFqdn": "<cluster-fqdn>" 
        } 
    } 
} 
```

> [!NOTE]
> Şu anda, var olan bir kümede __iliştirme__ işlemi gerçekleştirirken yük dengeleyiciyi yapılandıramazsınız. Önce kümeyi eklemeniz ve sonra yük dengeleyiciyi değiştirmek için bir güncelleştirme işlemi gerçekleştirmeniz gerekir.

AKS ile iç yük dengeleyiciyi kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmeti ile iç yük dengeleyici kullanma](/azure/aks/internal-lb).

## <a name="use-azure-container-instances-aci"></a>Azure Container Instances kullanın (ACI)

Azure Container Instances, bir model dağıtıldığında dinamik olarak oluşturulur. Azure Machine Learning sanal ağ içinde ACI oluşturmak üzere etkinleştirmek için, dağıtım tarafından kullanılan alt ağ için __alt ağ temsilcisini__ etkinleştirmeniz gerekir.

Çalışma alanınıza bir sanal ağda ACI 'yi kullanmak için aşağıdaki adımları kullanın:

1. Sanal ağınızda alt ağ temsilcisini etkinleştirmek için [alt ağ temsili ekleme veya kaldırma](../virtual-network/manage-subnet-delegation.md) makalesindeki bilgileri kullanın. Bir sanal ağ oluştururken temsilciyi etkinleştirebilir veya var olan bir ağa ekleyebilirsiniz.

    > [!IMPORTANT]
    > Temsilci seçme etkinleştirildiğinde, `Microsoft.ContainerInstance/containerGroups` __hizmet verme için alt ağ__ olarak kullanın.

2. [Aciwebservice. deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none--vnet-name-none--subnet-name-none-)kullanarak modeli dağıtın, `vnet_name` ve `subnet_name` parametrelerini kullanın. Bu parametreleri, temsilciyi etkinleştirdiğiniz sanal ağ adı ve alt ağa ayarlayın.



## <a name="use-azure-firewall"></a>Azure Güvenlik Duvarı’nı kullanma

Azure Güvenlik Duvarı ile Azure Machine Learning kullanma hakkında bilgi için bkz. [Azure Güvenlik duvarının arkasında Azure Machine Learning çalışma alanı kullanma](how-to-access-azureml-behind-firewall.md).

## <a name="use-azure-container-registry"></a>Azure Container Registry’yi kullanma

> [!IMPORTANT]
> Azure Container Registry (ACR) bir sanal ağın içine yerleştirilebilir, ancak aşağıdaki önkoşulları karşılamanız gerekir:
>
> * Azure Machine Learning çalışma alanınız Enterprise Edition olmalıdır. Yükseltme hakkında bilgi için bkz. [Enterprise Edition 'A yükseltme](how-to-manage-workspace.md#upgrade).
> * Azure Container Registry Premium sürüm olmalıdır. Yükseltme hakkında daha fazla bilgi için bkz. [SKU 'Ları değiştirme](/azure/container-registry/container-registry-skus#changing-skus).
> * Azure Container Registry, eğitim veya çıkarım için kullanılan depolama hesabı ve işlem hedefleri ile aynı sanal ağ ve alt ağ içinde olmalıdır.
> * Azure Machine Learning çalışma alanınızın bir [Azure Machine Learning işlem kümesi](how-to-set-up-training-targets.md#amlcompute)içermesi gerekir.
>
>     ACR bir sanal ağın arkasındaysa, Azure Machine Learning Docker görüntülerini doğrudan oluşturmak için kullanamaz. Bunun yerine, görüntüleri oluşturmak için işlem kümesi kullanılır.

1. Çalışma alanınızın Azure Container Registry adını bulmak için aşağıdaki yöntemlerden birini kullanın:

    __Azure portal__

    Çalışma alanınızın genel bakış bölümünden __kayıt defteri__ değeri Azure Container Registry bağlanır.

    :::image type="content" source="./media/how-to-enable-virtual-network/azure-machine-learning-container-registry.png" alt-text="Çalışma alanı için Azure Container Registry" border="true":::

    __Azure CLI__

    [Azure CLI için Machine Learning uzantısını yüklediyseniz](reference-azure-machine-learning-cli.md), çalışma alanı bilgilerini göstermek için `az ml workspace show` komutunu kullanabilirsiniz.

    ```azurecli-interactive
    az ml workspace show -w yourworkspacename -g resourcegroupname --query 'containerRegistry'
    ```

    Bu komut şuna benzer bir değer döndürür `"/subscriptions/{GUID}/resourceGroups/{resourcegroupname}/providers/Microsoft.ContainerRegistry/registries/{ACRname}"`. Dizenin son bölümü, çalışma alanının Azure Container Registry adıdır.

1. Sanal ağınıza erişimi sınırlandırmak için, [kayıt defteri için ağ erişimini yapılandırma](../container-registry/container-registry-vnet.md#configure-network-access-for-registry)bölümündeki adımları kullanın. Sanal ağ eklenirken Azure Machine Learning kaynaklarınızın sanal ağını ve alt ağını seçin.

1. Docker görüntülerini derlemek üzere bir işlem kümesi yapılandırmak için Python SDK Azure Machine Learning kullanın. Aşağıdaki kod parçacığı bunun nasıl yapılacağını göstermektedir:

    ```python
    from azureml.core import Workspace
    # Load workspace from an existing config file
    ws = Workspace.from_config()
    # Update the workspace to use an existing compute cluster
    ws.update(image_build_compute = 'mycomputecluster')
    ```

    > [!IMPORTANT]
    > Depolama hesabınız, işlem kümeniz ve Azure Container Registry hepsi sanal ağın aynı alt ağında olmalıdır.
    
    Daha fazla bilgi için [Update ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#update-friendly-name-none--description-none--tags-none--image-build-compute-none--enable-data-actions-none-) Yöntem başvurusuna bakın.

1. Aşağıdaki Azure Resource Manager şablonunu uygulamanız gerekir. Bu şablon, çalışma alanınızın ACR ile iletişim kurmasını sağlar.

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "keyVaultArmId": {
        "type": "string"
        },
        "workspaceName": {
        "type": "string"
        },
        "containerRegistryArmId": {
        "type": "string"
        },
        "applicationInsightsArmId": {
        "type": "string"
        },
        "storageAccountArmId": {
        "type": "string"
        },
        "location": {
        "type": "string"
        }
    },
    "resources": [
        {
        "type": "Microsoft.MachineLearningServices/workspaces",
        "apiVersion": "2019-11-01",
        "name": "[parameters('workspaceName')]",
        "location": "[parameters('location')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "sku": {
            "tier": "enterprise",
            "name": "enterprise"
        },
        "properties": {
            "sharedPrivateLinkResources":
    [{"Name":"Acr","Properties":{"PrivateLinkResourceId":"[concat(parameters('containerRegistryArmId'), '/privateLinkResources/registry')]","GroupId":"registry","RequestMessage":"Approve","Status":"Pending"}}],
            "keyVault": "[parameters('keyVaultArmId')]",
            "containerRegistry": "[parameters('containerRegistryArmId')]",
            "applicationInsights": "[parameters('applicationInsightsArmId')]",
            "storageAccount": "[parameters('storageAccountArmId')]"
        }
        }
    ]
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim ortamlarını ayarlama](how-to-set-up-training-targets.md)
* [Modellerin dağıtılacağı yerler](how-to-deploy-and-where.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
