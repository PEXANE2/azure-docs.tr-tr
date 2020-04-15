---
title: Sanal ağda güvenli denemeler ve çıkarım
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de azure sanal ağda deneme/eğitim işlerinin ve çıkarım/puanlama işlerinin nasıl güvence altına alabildiğini öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 03/13/2020
ms.openlocfilehash: f70c24c91e048270696b244bb9775cb24f0ef30d
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383469"
---
# <a name="secure-azure-ml-experimentation-and-inference-jobs-within-an-azure-virtual-network"></a>Azure Sanal Ağı'nda Azure ML deneme ve çıkarım işlerini güvenli hale
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning'de bir Azure Sanal Ağı (vnet) içinde deneme/eğitim işlerinin ve çıkarım/puanlama işlerinin nasıl güvence altına alabileceğinizi öğreneceksiniz.

**Sanal ağ,** Azure kaynaklarınızı ortak internetten yalıtarak bir güvenlik sınırı görevi görür. Bir Azure sanal ağına şirket içi ağınıza da katılabilirsiniz. Ağlara katılarak, modellerinizi güvenli bir şekilde eğitebilir ve çıkarım için dağıtılan modellerinize erişebilirsiniz.

Azure Machine Learning, bilgi işlem kaynakları için diğer Azure hizmetlerine dayanır. Hesaplama kaynakları veya [hesaplama hedefleri,](concept-compute-target.md)modelleri eğitmek ve dağıtmak için kullanılır. Hedefler sanal ağ içinde oluşturulabilir. Örneğin, bir modeli eğitmek ve ardından modeli Azure Kubernetes Hizmeti'ne (AKS) dağıtmak için Microsoft Veri Bilimi Sanal Makinesi'ni kullanabilirsiniz. Sanal ağlar hakkında daha fazla bilgi için [Azure Sanal Ağ'a genel bakış](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)bilgisi ne bakın.

Bu makalede, gelişmiş *güvenlik ayarları*hakkında ayrıntılı bilgi sağlar, temel veya deneysel kullanım durumları için gerekli olmayan bilgiler. Bu makalenin belirli bölümleri çeşitli senaryolar için yapılandırma bilgileri sağlar. Talimatları sırayla veya bütünüyle tamamlamanız gerekmez.

> [!TIP]
> Özel olarak çağrıldığı sürece, sanal ağ içindeki depolama hesapları veya bilgi işlem hedefleri gibi kaynakların kullanılması hem makine öğrenimi ardışık hatlarıyla hem de komut dosyası çalıştırmaları gibi boru hattı olmayan iş akışlarıyla çalışır.

> [!WARNING]
> Microsoft, temel depolama alanı sanal ağ etkinse Otomatik ML, Datasets, Datalabeling, Designer ve Notebooks gibi Azure Machine Learning Studio özelliklerini kullanmayı desteklemez.

## <a name="prerequisites"></a>Ön koşullar

+ Azure Makine Öğrenimi [çalışma alanı.](how-to-manage-workspace.md)

+ Hem Azure Sanal [Ağ hizmeti](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) hem de [IP ağı](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)hakkında genel çalışma bilgisi.

+ Bilgi işlem kaynaklarınızla kullanmak üzere önceden varolan bir sanal ağ ve alt ağ.

## <a name="use-a-storage-account-for-your-workspace"></a>Çalışma alanınız için bir depolama hesabı kullanma

> [!WARNING]
> Azure Machine Learning tasarımcısını kullanan veri bilimcileriniz varsa, sanal ağ daki bir depolama hesabından verileri görselleştirerken bir hata alırlar. Aşağıdaki metin aldıkları hatadır:
>
> __Hata: Bu veri kümesinin profili çıkılamıyor. Bunun nedeni, verilerinizin sanal bir ağın arkasında depolanmış olması veya verilerinizin profili desteklememesi olabilir.__

Sanal ağdaki çalışma alanı için bir Azure depolama hesabı kullanmak için aşağıdaki adımları kullanın:

1. Sanal bir ağın arkasına bir bilgi işlem kaynağı (örneğin, Machine Learning bilgi işlem örneği veya küme) oluşturun veya çalışma alanına (örneğin, bir HDInsight kümesi, sanal makine veya Azure Kubernetes Hizmet kümesi) bir bilgi işlem kaynağı takın. Bilgi işlem kaynağı deneme veya model dağıtımı için olabilir.

   Daha fazla bilgi için, Machine Learning bilgi işlem işlem [ini kullanın](#amlcompute), [sanal makine veya HDInsight kümesini kullanın](#vmorhdi)ve bu makalede Azure [Kubernetes Hizmet](#aksvnet) bölümlerini kullanın' bölümüne bakın.

1. Azure portalında, çalışma alanınıza bağlı depolama alanına gidin.

   [![Azure Machine Learning çalışma alanına bağlı depolama alanı](./media/how-to-enable-virtual-network/workspace-storage.png)](./media/how-to-enable-virtual-network/workspace-storage.png#lightbox)

1. Azure **Depolama** sayfasında __Güvenlik Duvarları ve sanal ağları__seçin.

   ![Azure portalındaki Azure Depolama sayfasındaki "Güvenlik Duvarları ve sanal ağlar" alanı](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)

1. Güvenlik __Duvarları ve sanal ağlar__ sayfasında aşağıdaki işlemleri yapın:
    - __Seçili ağlar__'ı seçin.
    - __Sanal ağlar__ __altında, varolan sanal ağ__ ekle bağlantısını seçin. Bu eylem, bilgi işleminizin bulunduğu sanal ağı ekler (bkz. adım 1).

        > [!IMPORTANT]
        > Depolama hesabı, eğitim veya çıkarım için kullanılan bilgi işlem örnekleri veya kümelerle aynı sanal ağda ve alt ağda olmalıdır.

    - Bu depolama hesabı onay __kutusuna erişmek için güvenilen Microsoft hizmetlerine İzin Ver'i__ seçin.

    > [!IMPORTANT]
    > Azure Machine Learning SDK ile çalışırken, geliştirme ortamınız Azure Depolama Hesabı'na bağlanabilmeli. Depolama hesabı sanal bir ağ içindeyken, güvenlik duvarı geliştirme ortamının IP adresinden erişime izin vermelidir.
    >
    > Depolama hesabına erişimi etkinleştirmek için, *geliştirme istemcisindeki bir web tarayıcısından*depolama hesabı için __Güvenlik Duvarları ve sanal ağları__ ziyaret edin. Ardından, istemcinin IP adresini __ADRES ARALIĞI'na__eklemek için __istemcinizIN IP adresi__ ekle onay kutusunu kullanın. Geliştirme ortamının IP adresini el ile girmek için __ADDRESS RANGE__ alanını da kullanabilirsiniz. İstemcinin IP adresi eklendikten sonra, SDK'yı kullanarak depolama hesabına erişebilir.

   [![Azure portalındaki "Güvenlik Duvarları ve sanal ağlar" bölmesi](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png#lightbox)

> [!IMPORTANT]
> Hem Azure Machine Learning için _varsayılan depolama hesabını_ hem de varsayılan olmayan depolama _hesaplarını_ sanal ağa yerleştirebilirsiniz.
>
> Varsayılan depolama alanı hesabı, bir çalışma alanı oluşturduğunuzda otomatik olarak karşılanır.
>
> Varsayılan olmayan depolama hesapları `storage_account` için [ `Workspace.create()` işlevdeki](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) parametre, Azure kaynak kimliğine göre özel bir depolama hesabı belirtmenize olanak tanır.

## <a name="use-azure-data-lake-storage-gen-2"></a>Azure Veri Gölü Depolama Gen 2'yi kullanma

Azure Veri Gölü Depolama Gen 2, Azure Blob depolamasına dayalı büyük veri analitiği için bir dizi özelliktir. Azure Machine Learning ile modelleri eğitmek için kullanılan verileri depolamak için kullanılabilir. 

Azure Machine Learning çalışma alanınızın sanal ağında ki Veri Gölü Depolama Gen 2'yi kullanmak için aşağıdaki adımları kullanın:

1. Bir Azure Veri Gölü Depolama gen 2 hesabı oluşturun. Daha fazla bilgi için [bkz.](../storage/blobs/data-lake-storage-quickstart-create-account.md)

1. Önceki bölümde 2-4 adımları kullanın, sanal ağa hesap koymak [için, çalışma alanı için bir depolama hesabı kullanın.](#use-a-storage-account-for-your-workspace)

Sanal ağ içinde Veri Gölü Depolama Gen 2 ile Azure Machine Learning kullanırken aşağıdaki kılavuzu kullanın:

* Bir veri __kümesi oluşturmak için SDK__kullanıyorsanız ve kodu çalıştıran sistem sanal `validate=False` __ağda değilse,__ parametreyi kullanın. Bu parametre, sistem depolama hesabıyla aynı sanal ağda değilse başarısız olan doğrulamayı atlar. Daha fazla bilgi için [from_files()](https://docs.microsoft.com/python/api/azureml-core/azureml.data.dataset_factory.filedatasetfactory?view=azure-ml-py#from-files-path--validate-true-) yöntemine bakın.

* Veri kümesini kullanarak bir modeli eğitmek için Azure Machine Learning İşlem Örneği veya bilgi işlem kümesini kullanırken, depolama hesabıyla aynı sanal ağda olmalıdır.

## <a name="use-a-key-vault-instance-with-your-workspace"></a>Çalışma alanınız ile önemli bir kasa örneği kullanma

Çalışma alanıyla ilişkili anahtar kasa örneği, Azure Machine Learning tarafından aşağıdaki kimlik bilgilerini depolamak için kullanılır:
* İlişkili depolama hesabı bağlantı dizesi
* Azure Kapsayıcı Deposu örneklerinin parolaları
* Veri depolarına bağlantı dizeleri

Azure Machine Learning deneme özelliklerini sanal bir ağın arkasındaki Azure Key Vault ile kullanmak için aşağıdaki adımları kullanın:

1. Çalışma alanıyla ilişkili anahtar kasasına gidin.

   [![Azure Machine Learning çalışma alanıyla ilişkili anahtar kasa](./media/how-to-enable-virtual-network/workspace-key-vault.png)](./media/how-to-enable-virtual-network/workspace-key-vault.png#lightbox)

1. Key **Vault** sayfasında, sol bölmede __Güvenlik Duvarları ve sanal ağları__seçin.

   ![Key Vault bölmesinde "Güvenlik Duvarları ve sanal ağlar" bölümü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. Güvenlik __Duvarları ve sanal ağlar__ sayfasında aşağıdaki işlemleri yapın:
    - __Erişime izin ver__altında, __Seçili ağları__seçin.
    - __Sanal ağlar__altında, deneme bilgiişleminizin bulunduğu sanal ağı eklemek için __varolan sanal ağları ekle'yi__ seçin.
    - __Bu güvenlik duvarLarını atlamak için güvenilen Microsoft hizmetlerine İzin Ver__altında __Evet'i__seçin.

   [![Key Vault bölmesinde "Güvenlik Duvarları ve sanal ağlar" bölümü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png#lightbox)

<a id="amlcompute"></a>

## <a name="use-a-machine-learning-compute"></a><a name="compute-instance"></a>Makine Öğrenimi İşlemi Kullanma

Sanal ağda bir Azure Machine Learning bilgi işlem örneği veya bilgi işlem kümesi kullanmak için aşağıdaki ağ gereksinimlerinin karşılanması gerekir:

> [!div class="checklist"]
> * Sanal ağ, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olmalıdır.
> * İşlem örneği veya küme için belirtilen alt ağ, hedeflenen VM sayısını karşılamak için yeterli adverilmemiş IP adresine sahip olmalıdır. Alt ağda yeterince atanmamış IP adresi yoksa, bir işlem kümesi kısmen ayrılır.
> * Sanal ağın aboneliğindeki veya kaynak grubundaki güvenlik ilkelerinizin veya kilitlerinizin sanal ağı yönetme izinlerini kısıtlayıp kısıtlamadığını kontrol edin. Trafiği kısıtlayarak sanal ağı güvenli hale almayı planlıyorsanız, bazı bağlantı noktalarını bilgi işlem hizmeti için açık bırakın. Daha fazla bilgi için [Gerekli bağlantı noktaları](#mlcports) bölümüne bakın.
> * Bir sanal ağa birden çok işlem örneği veya küme koyacaksanız, kaynaklarınızdan biri veya daha fazlası için kota artışı istemeniz gerekebilir.
> * Çalışma alanı için Azure Depolama Hesabı(lar) sanal bir ağda da korunuyorsa, Azure Machine Learning bilgi işlem örneği veya kümesiyle aynı sanal ağda olmaları gerekir. 

> [!TIP]
> Machine Learning bilgi işlem örneği veya kümesi, sanal ağı içeren kaynak grubuna ek ağ kaynaklarını otomatik olarak ayırır. Her işlem örneği veya küme için hizmet aşağıdaki kaynakları ayırır:
> 
> * Bir ağ güvenlik grubu
> * Bir genel IP adresi
> * Bir yük dengeleyici
> 
> Bu kaynaklar, aboneliğin [kaynak kotalarıyla](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) sınırlıdır.


### <a name="required-ports"></a><a id="mlcports"></a>Gerekli bağlantı noktaları

Machine Learning Compute şu anda belirtilen sanal ağdaki VM'leri sağlamak için Azure Toplu İşlem hizmetini kullanmıştır. Alt ağ, Toplu İşlem hizmetinden gelen iletişime izin vermelidir. Bu iletişimi, Machine Learning Compute düğümlerinde çalıştırmaları zamanlamak ve Azure Depolama ve diğer kaynaklarla iletişim kurmak için kullanırsınız. Toplu İşlem hizmeti, VM'lere bağlı ağ arabirimleri (NIC' ler) düzeyinde ağ güvenlik grupları (NSG' ler) ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

- __BatchNodeManagement__servis __etiketinden__ 29876 ve 29877 bağlantı noktalarında gelen TCP trafiği.

    ![BatchNodeManagement hizmet etiketini kullanan gelen bir kural](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- (İsteğe bağlı) Uzaktan erişime izin vermek için 22. Bu bağlantı noktasını yalnızca genel IP'de SSH kullanarak bağlanmak istiyorsanız kullanın.

- Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.

- İnternete giden herhangi bir bağlantı noktasında giden trafik.

- __AzureMachineLearning__ __Hizmet Etiketinden__ 44224 bağlantı noktasındaki gelen TCP trafiğini hesaplamak için.

Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Bir NSG işlem düğümlerine iletişimi engellerse, işlem hizmeti işlem düğümlerinin durumunu kullanılamaz olarak ayarlar.

Azure Toplu İşlem hizmeti kendi NSG'lerini yapılandırdığından, NSG'leri alt net düzeyinde belirtmeniz gerekmez. Ancak, belirtilen alt ağ NSG'lerle veya bir güvenlik duvarıyla ilişkiliyse, daha önce belirtildiği gibi gelen ve giden güvenlik kurallarını yapılandırın.

Azure portalındaki NSG kural yapılandırması aşağıdaki resimlerde gösterilmiştir:

[![Makine Öğrenimi İşlemi için gelen NSG kuralları](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png#lightbox)

![Makine Öğrenimi İşlemi için giden NSG kuralları](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a name="limit-outbound-connectivity-from-the-virtual-network"></a><a id="limiting-outbound-from-vnet"></a>Sanal ağdan giden bağlantıyı sınırlama

Varsayılan giden kuralları kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlamak istiyorsanız, aşağıdaki adımları kullanın:

- NSG kurallarını kullanarak giden internet bağlantısını reddedin.

- Bir __işlem örneği__ veya __bir işlem kümesi__için, giden trafiği aşağıdaki öğelerle sınırlandırın:
   - Azure Depolama, Depolama Hizmet __Etiketini__ __kullanarak.RegionName__. Azure `{RegionName}` bölgesinin adı nerededir?
   - Azure Container Registry, __AzureContainerRegistry.RegionName__ __Hizmet Etiketini__ kullanarak. Azure `{RegionName}` bölgesinin adı nerededir?
   - Azure Machine Learning, __AzureMachineLearning__ __Hizmet Etiketini__ kullanarak
   - Azure Kaynak Yöneticisi, __AzureResourceManager__ __Hizmet Etiketini__ kullanarak
   - __Azure Active Directory__ Hizmet __Etiketini__ kullanarak Azure Active Directory

Azure portalındaki NSG kural yapılandırması aşağıdaki resimde gösterilmiştir:

[![Makine Öğrenimi İşlemi için giden NSG kuralları](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png#lightbox)

> [!NOTE]
> Microsoft tarafından sağlanan varsayılan Docker resimlerini kullanmayı ve kullanıcı tarafından yönetilen bağımlılıkları etkinleştirmeyi planlıyorsanız, __MicrosoftContainerRegistry.Region_Name'nin__ (Örneğin MicrosoftContainerRegistry.EastUS) bir __Hizmet Etiketini__ de kullanmanız gerekir.
>
> Bu yapılandırma, eğitim komut dosyalarınızın bir parçası olarak aşağıdaki parçacıklara benzer bir kodunuz varsa gereklidir:
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
> __Tahminci eğitimi__
> ```python
> est = Estimator(source_directory='.',
>                 script_params=script_params,
>                 compute_target='local',
>                 entry_script='dummy_train.py',
>                 user_managed=True)
> run = exp.submit(est)
> ```

### <a name="user-defined-routes-for-forced-tunneling"></a>Zorunlu tünelleme için kullanıcı tanımlı rotalar

Machine Learning Compute ile zorunlu tünel leme kullanıyorsanız, bilgi işlem kaynağını içeren alt ağa [kullanıcı tanımlı rotalar (ÜDR)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) ekleyin.

* Kaynaklarınızın bulunduğu bölgede Azure Toplu İş hizmeti tarafından kullanılan her IP adresi için bir UDR belirleyin. Bu ÜTK'ler, Toplu İşlem hizmetinin görev planlaması için bilgi işlem düğümleriyle iletişim kurmasını sağlar. Toplu İşlem hizmetinin IP adreslerinin listesini almak için aşağıdaki yöntemlerden birini kullanın:

    * Azure [IP Aralıkları ve Hizmet](https://www.microsoft.com/download/details.aspx?id=56519) Etiketleri'ni `<region>` indirin ve dosyada " Azure bölgeniz nerede" diye `BatchNodeManagement.<region>`arama yapın.

    * Bilgileri indirmek için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) kullanın. Aşağıdaki örnek IP adresi bilgilerini karşıdan yükler ve Doğu ABD 2 bölgesine ait bilgileri filtreler:

        ```azurecli-interactive
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
        ```

* Azure Depolama'ya giden trafik şirket içi ağ aygıtınız tarafından engellenmemelidir. Özellikle, URL'ler şeklinde `<account>.table.core.windows.net` `<account>.queue.core.windows.net`, ve `<account>.blob.core.windows.net`.

ÜTK'leri eklediğinizde, ilgili her Toplu IP adresi önekinin rotasını tanımlayın ve __Sonraki atlama türünü__ __Internet'e__ayarlayın. Aşağıdaki resim, Azure portalında bu UDR'nin bir örneğini gösterir:

![Adres öneki için UDR örneği](./media/how-to-enable-virtual-network/user-defined-route.png)

Daha fazla bilgi için [bkz.](../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling)

### <a name="create-a-compute-cluster-in-a-virtual-network"></a>Sanal ağda işlem kümesi oluşturma

Machine Learning Compute kümesi oluşturmak için aşağıdaki adımları kullanın:

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com/)oturum açın ve ardından aboneliğinizi ve çalışma alanınızı seçin.

1. Soldaki __İşlem'i__ seçin.

1. Merkezden __Eğitim kümelerini__ seçin ve __+__ sonra .

1. Yeni __Eğitim Kümesi__ iletişim kutusunda __Gelişmiş ayarlar__ bölümünü genişletin.

1. Bu bilgi işlem kaynağını sanal ağ kullanacak şekilde yapılandırmak için, sanal ağ yapılandırma bölümünde aşağıdaki eylemleri __gerçekleştirin:__

    1. Kaynak __grubu__ açılır listesinde, sanal ağı içeren kaynak grubunu seçin.
    1. Sanal __ağ__ açılır listesinde, alt ağı içeren sanal ağı seçin.
    1. Alt __ağ__ açılır listesinde kullanmak üzere alt ağı seçin.

   ![Machine Learning Compute için sanal ağ ayarları](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

Azure Machine Learning SDK'yı kullanarak bir Machine Learning Compute kümesi de oluşturabilirsiniz. Aşağıdaki kod adlı `default` `mynetwork`bir sanal ağın alt net yeni bir Machine Learning Compute küme oluşturur:

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

Oluşturma işlemi sona erdiğinde, bir denemede kümeyi kullanarak modelinizi eğitebilirsiniz. Daha fazla bilgi için [bkz.](how-to-set-up-training-targets.md)

## <a name="use-azure-databricks"></a>Azure Veri Tuğlaları kullanma

Azure Veri Tuğlalarını çalışma alanınızla sanal ağda kullanmak için aşağıdaki gereksinimlerin karşılanması gerekir:

> [!div class="checklist"]
> * Sanal ağ, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olmalıdır.
> * Çalışma alanı için Azure Depolama Hesabı(lar) sanal bir ağda da korunuyorsa, Azure Veri Tuğlaları kümesiyle aynı sanal ağda olmaları gerekir.
> * Azure Databricks tarafından kullanılan __veri tuğlaları ve__ __public__ alt ağlarına ek olarak, sanal ağ için oluşturulan __varsayılan__ alt ağ da gereklidir.

Sanal ağda Azure Veri Tuğlaları kullanma hakkında özel bilgiler için Azure [Sanal Ağınızda Azure Veri Tuğlaları Dağıt'a](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html)bakın.

<a id="vmorhdi"></a>

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Sanal bir makine veya HDInsight kümesi kullanma

> [!IMPORTANT]
> Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler.

Çalışma alanınızla sanal ağda sanal bir makine veya Azure HDInsight kümesi kullanmak için aşağıdaki adımları kullanın:

1. Azure portalını veya Azure CLI'yi kullanarak bir VM veya HDInsight kümesi oluşturun ve kümeyi bir Azure sanal ağına koyun. Daha fazla bilgi için aşağıdaki makalelere bakın:
    * [Linux Sanal M'leri için Azure sanal ağları oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Azure sanal ağını kullanarak HDInsight'ı genişletme](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Azure Machine Learning'in VM veya kümedeki SSH bağlantı noktasıyla iletişim kurmasına izin vermek için ağ güvenlik grubu için bir kaynak girişi yapılandırın. SSH bağlantı noktası genellikle 22 bağlantı noktasıdır. Bu kaynaktan gelen trafiğe izin vermek için aşağıdaki eylemleri yapın:

    * __Kaynak__ açılır listesinde __Hizmet Etiketi'ni__seçin.

    * Kaynak __hizmet etiketi__ açılır listesinde __AzureMachineLearning'i__seçin.

    * Kaynak bağlantı noktası açılır liste __*__ __aralıklarında.__

    * __Hedef__ açılır listesinde, __Herhangi__bir .

    * Hedef __bağlantı noktası aralıkları__ açılır listede __22'yi__seçin.

    * __Protokol__altında, __Herhangi__birini seçin.

    * __Eylem__altında, __İzin Ver'i__seçin.

   ![Sanal ağ içinde bir VM veya HDInsight kümesi üzerinde deneme yapmak için gelen kurallar](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Ağ güvenlik grubu için varsayılan giden kuralları tutun. Daha fazla bilgi için Güvenlik [gruplarındaki](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kurallarına bakın.

    Varsayılan giden kuralları kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlamak istiyorsanız, [sanal ağ bölümünden Sınır giden bağlantıbölümüne](#limiting-outbound-from-vnet) bakın.

1. Azure Machine Learning çalışma alanınıza VM veya HDInsight kümesini takın. Daha fazla bilgi için [bkz.](how-to-set-up-training-targets.md)

<a id="aksvnet"></a>

## <a name="use-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmetini (AKS) kullanın

Sanal ağdaki AKS'yi çalışma alanınıza eklemek için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Aşağıdaki yordamı başlatmadan önce, [Azure Kubernetes Hizmeti'nde (AKS) gelişmiş ağ yapılandırma'daki](https://docs.microsoft.com/azure/aks/configure-advanced-networking#prerequisites) ön koşulları izleyin ve kümeniz için IP adresini planlayın.
>
> AKS örneği ve Azure sanal ağı aynı bölgede olmalıdır. Çalışma alanı tarafından sanal ağda kullanılan Azure Depolama Hesabı'nı(lar) güvenli hale ederseniz, bunların AKS örneğiyle aynı sanal ağda olması gerekir.

> [!WARNING]
> Azure Machine Learning, özel bağlantı etkinleştirilmiş bir Azure Kubernetes Hizmeti kullanmayı desteklemez.

1. [Azure Machine Learning stüdyosunda](https://ml.azure.com/)oturum açın ve ardından aboneliğinizi ve çalışma alanınızı seçin.

1. Soldaki __İşlem'i__ seçin.

1. Merkezden __Çıkarım kümelerini__ seçin ve __+__ sonra .

1. Yeni __Çıkarım Kümesi__ iletişim kutusunda, __Ağ yapılandırması__altında __Gelişmiş'i__ seçin.

1. Sanal ağ kullanacak şekilde bu bilgi işlem kaynağını yapılandırmak için aşağıdaki işlemleri gerçekleştirin:

    1. Kaynak __grubu__ açılır listesinde, sanal ağı içeren kaynak grubunu seçin.
    1. Sanal __ağ__ açılır listesinde, alt ağı içeren sanal ağı seçin.
    1. Alt __ağ__ açılır listesinde alt ağı seçin.
    1. __Kubernetes Servis adres aralığı__ kutusuna, Kubernetes servis adresi aralığını girin. Bu adres aralığı, küme için kullanılabilen IP adreslerini tanımlamak için Sınıfsız Etki Alanları Yönlendirme (CIDR) gösterimIP aralığı nı kullanır. Herhangi bir alt ağ IP aralıklarıyla örtüşmemelidir (örneğin, 10.0.0.0/16).
    1. __Kubernetes DNS hizmet IP adres__ kutusuna Kubernetes DNS hizmet IP adresini girin. Bu IP adresi Kubernetes DNS hizmetine atanır. Kubernetes servis adres aralığında olmalıdır (örneğin, 10.0.0.10).
    1. Docker __köprüsü adres__ kutusuna Docker köprüsü adresini girin. Bu IP adresi Docker Köprüsü'ne atanır. Herhangi bir alt ağ IP aralığında veya Kubernetes servis adresi aralığında olmamalıdır (örneğin, 172.17.0.1/16).

   ![Azure Machine Learning: Machine Learning Sanal ağ ayarlarını hesaplama](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Sanal ağı kontrol eden NSG grubunun, sanal ağ dışından çağrılabilmek için puanlama bitiş noktası için etkin leştirilmiş bir güvenlik kuralıolduğundan emin olun.
   > [!IMPORTANT]
   > NSG için varsayılan giden kuralları tutun. Daha fazla bilgi için Güvenlik [gruplarındaki](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kurallarına bakın.

   [![Gelen güvenlik kuralı](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png#lightbox)

Azure Machine Learning SDK'yı sanal ağa Azure Kubernetes Hizmeti eklemek için de kullanabilirsiniz. Sanal ağda zaten bir AKS kümeniz varsa, [AKS'ye nasıl dağıtılır'da](how-to-deploy-and-where.md)açıklandığı gibi çalışma alanına takın. Aşağıdaki kod adlı `default` `mynetwork`bir sanal ağın alt ağında yeni bir AKS örneği oluşturur:

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

Oluşturma işlemi tamamlandığında, bir AKS kümesinde sanal bir ağın arkasındaki çıkarım veya model puanlama çalıştırabilirsiniz. Daha fazla bilgi için [AKS'ye nasıl dağıtılanın.](how-to-deploy-and-where.md)

### <a name="use-private-ips-with-azure-kubernetes-service"></a>Azure Kubernetes Hizmeti ile özel IP'leri kullanma

Varsayılan olarak, AKS dağıtımlarına ortak bir IP adresi atanır. Sanal ağ içinde AKS kullanırken, bunun yerine özel bir IP adresi kullanabilirsiniz. Özel IP adreslerine yalnızca sanal ağ veya birleştirilmiş ağların içinden erişilebilir.

Aks'ı _bir iç yük dengeleyicisi_kullanacak şekilde yapılandırarak özel bir IP adresi etkinleştirilir. 

> [!IMPORTANT]
> Azure Kubernetes Hizmet kümesini oluştururken özel IP'yi etkinleştiremezsiniz. Varolan bir kümeye güncelleştirme olarak etkinleştirilmelidir.

Aşağıdaki kod snippet nasıl **yeni bir AKS küme oluşturmak**ve daha sonra özel bir IP / iç yük dengeleyici kullanmak için güncelleştirin gösterir:

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

    # Create AKS configuration
    prov_config = AksCompute.provisioning_configuration(location = "eastus2")
    # Set info for existing virtual network to create the cluster in
    prov_config.vnet_resourcegroup_name = "myvnetresourcegroup"
    prov_config.vnet_name = "myvnetname"
    prov_config.service_cidr = "10.0.0.0/16"
    prov_config.dns_service_ip = "10.0.0.10"
    prov_config.subnet_name = "default"
    prov_config.docker_bridge_cidr = "172.17.0.1/16"

    # Create compute target
    aks_target = ComputeTarget.create(workspace = ws, name = "myaks", provisioning_configuration = prov_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
    
    # Update AKS configuration to use an internal load balancer
    update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", "default")
    aks_target.update(update_config)
    # Wait for the operation to complete
    aks_target.wait_for_completion(show_output = True)
```

__Azure CLI__

```azurecli-interactive
az rest --method put --uri https://management.azure.com"/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-id>?api-version=2018-11-19 --body @body.json
```

Komut tarafından `body.json` başvurulan dosyanın içeriği aşağıdaki JSON belgesine benzer:

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
> Şu anda, varolan bir küme üzerinde bir __ekleme__ işlemi gerçekleştirirken yük dengeleyicisini yapılandıramazsınız. Önce kümeeklemeniz ve sonra yük dengeleyicisini değiştirmek için bir güncelleştirme işlemi gerçekleştirmeniz gerekir.

AKS ile iç yük dengeleyicisini kullanma hakkında daha fazla bilgi için azure [Kubernetes Hizmeti ile dahili yük dengeleyicisini kullan'a](/azure/aks/internal-lb)bakın.

## <a name="use-azure-container-instances-aci"></a>Azure Kapsayıcı Örnekleri (ACI) kullanma

Azure Kapsayıcı Örnekleri, bir model dağıtılırken dinamik olarak oluşturulur. Azure Machine Learning'in sanal ağ içinde ACI oluşturmasını sağlamak için dağıtım tarafından kullanılan alt ağ için __alt ağ delegasyonu__ etkinleştirmeniz gerekir.

Çalışma alanınızda sanal ağdaki ACI'yi kullanmak için aşağıdaki adımları kullanın:

1. Sanal abunuzda alt ağ delegasyonu etkinleştirmek için Ekle'deki bilgileri kullanın [veya bir alt ağ delegasyonu](../virtual-network/manage-subnet-delegation.md) makalesinden kaldırın. Sanal ağ oluştururken temsilciliği etkinleştirebilir veya varolan bir ağa ekleyebilirsiniz.

    > [!IMPORTANT]
    > Temsilciyi etkinleştirirken, `Microsoft.ContainerInstance/containerGroups` __hizmet değerine Temsilci alt ağı__ olarak kullanın.

2. [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none--vnet-name-none--subnet-name-none-)kullanarak modeli `vnet_name` dağıtın, `subnet_name` kullanın ve parametreleri. Bu parametreleri, temsilciliği etkinleştirdiğiniz sanal ağ adı ve alt ağına ayarlayın.



## <a name="use-azure-firewall"></a>Azure Güvenlik Duvarı'nı kullanma

Azure Güvenlik Duvarı'nı kullanırken, trafiğin aşağıdaki adreslere girip girmesine izin verecek bir ağ kuralını yapılandırmanız gerekir:

- `*.batchai.core.windows.net`
- `ml.azure.com`
- `*.azureml.ms`
- `*.experiments.azureml.net`
- `*.modelmanagement.azureml.net`
- `mlworkspace.azure.ai`
- `*.aether.ms`

Kuralı eklerken, __Protokolü__ herhangi birine, bağlantı `*`noktalarını da .

Ağ kuralını yapılandırma hakkında daha fazla bilgi için Azure [Güvenlik DuvarLarını Dağıt ve yapılandırma](/azure/firewall/tutorial-firewall-deploy-portal#configure-a-network-rule)kınızda.

## <a name="use-azure-container-registry"></a>Azure Container Registry’yi kullanma

> [!IMPORTANT]
> Azure Kapsayıcı Kayıt Defteri (ACR) sanal bir ağa konulabilir, ancak aşağıdaki ön koşulları karşılamanız gerekir:
>
> * Azure Machine Learning çalışma alanınız Enterprise sürümü olmalıdır. Yükseltme hakkında daha fazla bilgi için, [Bkz. Enterprise sürümüne Yükseltme.](how-to-manage-workspace.md#upgrade)
> * Azure Konteyner Kayıt Defteriniz Premium sürüm olmalıdır. Yükseltme hakkında daha fazla bilgi için Bkz. [SK'leri Değiştirme.](/azure/container-registry/container-registry-skus#changing-skus)
> * Azure Konteyner Kayıt Defteriniz, eğitim veya çıkarım için kullanılan depolama hesabı ve bilgi işlem hedefleri yle aynı sanal ağda ve alt ağda olmalıdır.
> * Azure Machine Learning çalışma alanınız bir [Azure Machine Learning bilgi işlem kümesi](how-to-set-up-training-targets.md#amlcompute)içermelidir.
>
>     ACR sanal bir ağın arkasındaysa, Azure Machine Learning bu ağı doğrudan Docker görüntüleri oluşturmak için kullanamaz. Bunun yerine, görüntüleri oluşturmak için işlem kümesi kullanılır.

1. Çalışma alanınız için Azure Kapsayıcı Kayıt Defteri'nin adını bulmak için aşağıdaki yöntemlerden birini kullanın:

    __Azure portal__

    Çalışma alanınızın genel bakış bölümünden, __Kayıt Defteri__ değeri Azure Kapsayıcı Kayıt Defteri'ne bağlantılar bağlar.

    ![Çalışma alanı için Azure Kapsayıcı Kayıt Defteri](./media/how-to-enable-virtual-network/azure-machine-learning-container-registry.png)

    __Azure CLI__

    [Azure CLI için Machine Learning uzantısını](reference-azure-machine-learning-cli.md)yüklediyseniz, `az ml workspace show` çalışma alanı bilgilerini göstermek için komutu kullanabilirsiniz.

    ```azurecli-interactive
    az ml workspace show -w yourworkspacename -g resourcegroupname --query 'containerRegistry'
    ```

    Bu `"/subscriptions/{GUID}/resourceGroups/{resourcegroupname}/providers/Microsoft.ContainerRegistry/registries/{ACRname}"`komut, . Dize son bölümü çalışma alanı için Azure Kapsayıcı Kayıt Defteri adıdır.

1. Sanal ağınıza erişimi sınırlamak [için, kayıt defteri için ağ erişimini yapılandır'daki](../container-registry/container-registry-vnet.md#configure-network-access-for-registry)adımları kullanın. Sanal ağı eklerken, Azure Machine Learning kaynaklarınız için sanal ağı ve alt ağı seçin.

1. Docker görüntüleri oluşturmak için bir bilgi işlem kümesini yapılandırmak için Azure Machine Learning Python SDK'yı kullanın. Aşağıdaki kod parçacığı bunu nasıl yapacağız gösterir:

    ```python
    from azureml.core import Workspace
    # Load workspace from an existing config file
    ws = Workspace.from_config()
    # Update the workspace to use an existing compute cluster
    ws.update(image_build_compute = 'mycomputecluster')
    ```

    > [!IMPORTANT]
    > Depolama hesabınız, bilgi işlem kümeniz ve Azure Kapsayıcı Kayıt Defteri'nin tümü sanal ağın aynı alt ağında olmalıdır.
    
    Daha fazla bilgi için [güncelleştirme()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#update-friendly-name-none--description-none--tags-none--image-build-compute-none--enable-data-actions-none-) yöntemi başvurusuna bakın.

1. Azure Machine Learning çalışma alanınız için Özel Bağlantı kullanıyorsanız ve çalışma alanınız için Azure Kapsayıcı Kayıt Defteri'ni sanal bir ağa koyduysanız, aşağıdaki Azure Kaynak Yöneticisi şablonunu da uygulamanız gerekir. Bu şablon, çalışma alanınızın Özel Bağlantı üzerinden ACR ile iletişim kurmasını sağlar.

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
* [Azure Machine Learning aracılığıyla bir web hizmetini güvence altına almak için TLS'yi kullanın](how-to-secure-web-service.md)
