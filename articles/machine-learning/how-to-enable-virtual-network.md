---
title: Ağ yalıtımı & gizliliği
titleSuffix: Azure Machine Learning
description: Deneme/eğitimin yanı sıra çıkarım/Puanlama işlerinin güvenliğini sağlamak için Azure Machine Learning ile yalıtılmış bir Azure sanal ağı kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 07/07/2020
ms.topic: conceptual
ms.custom: how-to, contperfq4, devx-track-python
ms.openlocfilehash: 58f0a25ebeb7fb715cfba27d7482a031d1fe8c32
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121213"
---
# <a name="network-isolation-during-training-and-inference-with-private-endpoints-and-virtual-networks"></a>Özel uç noktalar ve sanal ağlarla eğitim ve çıkarım sırasında ağ yalıtımı
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bir Azure sanal ağı (VNet) içinde Azure Machine Learning eğitim ve çıkarım işlerini yalıtarak makine öğrenimi yaşam döngülerini nasıl sağlayacağınızı öğreneceksiniz. Bir __sanal ağ__ , Azure kaynaklarınızı genel İnternet 'ten yalıtmak için bir güvenlik sınırı görevi görür. Ayrıca, bir Azure sanal ağını şirket içi ağınıza da katabilirsiniz. Ağları birleştirerek, modellerinizi güvenli bir şekilde eğitebilir ve çıkarım için dağıtılan modellerinize erişebilirsiniz.

Azure Machine Learning çalışma alanına, __özel bir uç nokta__kullanılarak sanal bir ağdan erişilebilir. Özel uç nokta, sanal ağınız içindeki özel IP adresleri kümesidir ve çalışma alanınıza erişim sanal ağla sınırlıdır. Özel uç nokta veri taşması riskini azaltmaya yardımcı olur. Özel uç noktalar hakkında daha fazla bilgi için bkz. [Azure özel bağlantı](/azure/private-link/private-link-overview) makalesi.

> [!NOTE]
> Mozilla Firefox kullanırken özel bir uç nokta aracılığıyla çalışma alanına erişme sorunlarıyla karşılaşabilirsiniz. Sorun, tarayıcıdaki HTTPS üzerinden DNS ayarıyla ilişkili olabilir. Bu sorunu geçici olarak çözmek için Microsoft Edge veya Google Chrome kullanmanızı öneririz.

Azure Machine Learning, veri depolama ve hesaplama kaynakları için diğer Azure hizmetlerini kullanır (modelleri eğitmek ve dağıtmak için kullanılır). Bu kaynaklar, bir sanal ağ içinde de oluşturulabilir. Örneğin, bir modeli eğitme ve sonra modeli Azure Kubernetes Service (AKS) ' e dağıtmak için Azure Machine Learning işlem kullanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

+ Azure Machine Learning [çalışma alanı](how-to-manage-workspace.md).

+ [Azure sanal ağ hizmeti](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ve [IP ağının](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)genel çalışma bilgileri.

+ İşlem kaynaklarınızla kullanılacak önceden var olan bir sanal ağ ve alt ağ.

+ Bir sanal ağa veya alt ağa kaynak dağıtmak için, Kullanıcı hesabınızın Azure rol tabanlı erişim denetimlerinde (RBAC) aşağıdaki eylemler için izinleri olmalıdır:

    - Sanal ağ kaynağında "Microsoft. Network/virtualNetworks/JOIN/Action".
    - Alt ağ kaynağında "Microsoft. Network/virtualNetworks/subnet/JOIN/Action".

    Ağ ile RBAC hakkında daha fazla bilgi için bkz. [ağ yerleşik rolleri](/azure/role-based-access-control/built-in-roles#networking)

## <a name="secure-your-workspace"></a>Çalışma alanınızın güvenliğini sağlama

Azure Machine Learning çalışma alanınız __ortak bir uç nokta__ veya __Özel uç nokta__içerebilir. Genel uç nokta, genel İnternet üzerinden erişilebilen bir IP adresi kümesidir, özel bir uç nokta ise bir sanal ağ içindeki özel IP adresleri kümesidir. 

Çalışma alanınıza erişimi yalnızca özel IP adresleri üzerinden gerçekleşecek şekilde sınırlamak için özel bir uç nokta kullanın.

Çalışma alanına yönelik iletişime yalnızca sanal ağdan izin verildiğinden, çalışma alanını kullanan tüm geliştirme ortamları sanal ağın üyesi olmalıdır. Örneğin, sanal ağdaki bir sanal makine.

> [!IMPORTANT]
> Özel uç nokta, çalışma alanını silme veya işlem kaynaklarını yönetme gibi Azure denetim düzlemi 'ni (yönetim işlemleri) etkilemez. Örneğin, bir işlem hedefi oluşturma, güncelleştirme veya silme. Bu işlemler, genel Internet üzerinden normal olarak gerçekleştirilir.
>
> Özel uç nokta, sanal ağın dışından çalışma alanına erişimi engeller.

Özel bir uç nokta içeren bazı kaynak birleşimleri, Enterprise Edition çalışma alanı gerektirir. Hangi senaryoların Enterprise Edition gerektirdiğini anlamak için aşağıdaki tabloyu kullanın:

| Senaryo | Kurumsal</br>Edition | Temel</br>Edition |
| ----- |:-----:|:-----:| 
| Sanal ağ veya özel uç nokta yok | ✔ | ✔ |
| Özel uç nokta olmadan çalışma alanı. Bir sanal ağdaki diğer kaynaklar (Azure Container Registry hariç) | ✔ | ✔ |
| Özel uç nokta olmadan çalışma alanı. Özel uç nokta ile diğer kaynaklar | ✔ | |
| Özel uç nokta içeren çalışma alanı. Bir sanal ağdaki diğer kaynaklar (Azure Container Registry hariç) | ✔ | ✔ |
| Özel uç nokta ile çalışma alanı ve diğer kaynaklar | ✔ | |
| Özel uç nokta içeren çalışma alanı. Özel uç nokta veya sanal ağ olmayan diğer kaynaklar | ✔ | ✔ |
| Bir sanal ağda Azure Container Registry | ✔ | |
| Çalışma alanı için müşteri tarafından yönetilen anahtarlar | ✔ | |

> [!WARNING]
> 
> Özel uç noktanın etkin olduğu bir çalışma alanında Azure Machine Learning işlem örnekleri desteklenmez.
>
> Azure Machine Learning, Özel uç noktası etkin olan bir Azure Kubernetes hizmetini kullanmayı desteklemez. Bunun yerine, bir sanal ağda Azure Kubernetes hizmeti kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure sanal ağı Içindeki güvenli Azure ML deneme ve çıkarım işleri](how-to-enable-virtual-network.md).

Azure 'da özel uç noktalar hakkında daha fazla bilgi edinmek için bkz. [Azure özel bağlantı](/azure/private-link/private-link-overview) makalesi.

### <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Özel uç nokta kullanan bir çalışma alanı oluşturma

Azure Machine Learning SDK, CLı, Azure Resource Manager şablonu veya Azure portal kullanarak özel bir uç nokta ile yeni bir çalışma alanı oluşturabilirsiniz.

__Gereksinimler__

* Özel uç noktayla kullandığınız sanal ağın ağ ilkelerinin devre dışı olması gerekir. Daha fazla bilgi için bkz. [özel bir uç nokta için ağ Ilkelerini devre dışı bırakma](/azure/private-link/disable-private-endpoint-network-policy).

__Sınırlamalar__

* Çalışma alanına, genel İnternet üzerinden yalnızca sanal ağ içinden bağlanamazsınız.

* Özel uç noktalar kullanılarak birden çok çalışma alanı oluşturulursa ve aynı özel DNS bölgesini kullanıyorsa, özel DNS bölgesinin __sanal ağ bağlantılarına__ yalnızca ilk çalışma alanı eklenir.

    Bu kısıtlamayı geçici olarak çözmek için, ek çalışma alanları için sanal ağ bağlantısını el ile ekleyin. Daha fazla bilgi için bkz. [sanal ağ bağlantısı nedir?](/azure/dns/private-dns-virtual-network-links).

__Yapılandırma__

Bir sanal ağ ve özel uç nokta kullanarak çalışma alanı oluşturma hakkında daha fazla bilgi için, diğer yapılandırma seçenekleriyle birlikte aşağıdaki makalelere bakın:

* [Azure Machine Learning için bir çalışma alanı oluşturmak üzere bir Azure Resource Manager şablonu kullanın](how-to-create-workspace-template.md).
* [Portalda çalışma alanları oluşturun](how-to-manage-workspace.md).
* [Azure CLI ile çalışma alanları oluşturun](how-to-manage-workspace-cli.md).
* Python SDK 'yı kullanmak için, [Privateendpointconfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.private_endpoint.privateendpointconfig?view=azure-ml-py) ve [Workspace. Create ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--private-endpoint-config-none--private-endpoint-auto-approval-true--exist-ok-false--show-output-true-) başvuru belgelerine bakın.

<a id="amlcompute"></a>

## <a name="machine-learning-studio"></a>Machine Learning Studio

__Gereksinimler__

* Depolama hesabındaki verilere erişmek için, depolama hesabının çalışma alanıyla aynı sanal ağda olması gerekir.

* Verileriniz bir sanal ağda depolanıyorsa, Studio 'ya verilerinize erişim vermek için çalışma alanı [tarafından yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md) kullanmanız gerekir.

    > [!IMPORTANT]
    > Çoğu Studio, bir sanal ağda depolanan verilerle birlikte çalışarak, tümleşik Not defterleri __değildir__. Tümleşik Not defterleri, sanal bir ağdaki depolamanın kullanımını desteklemez. Bunun yerine, Jupyıter not defterlerini bir işlem örneğinden kullanabilirsiniz. Daha fazla bilgi için bkz. [işlem kümeleri & örnekleri](#compute-instance) bölümü.

    Studio erişimi verilemez, bu hatayı alırsınız, `Error: Unable to profile this dataset. This might be because your data is stored behind a virtual network or your data does not support profile.` aşağıdaki işlemler kullanılamaz:

    * Studio 'daki verileri önizleyin.
    * Tasarımcıda verileri görselleştirin.
    * Bir oto ml denemesi gönder.
    * Etiketleme projesi başlatın.

__Sınırlamalar__

* Azure Machine Learning Studio, bir sanal ağdaki aşağıdaki veri deposu türlerinden veri okumayı destekler:

    * Azure Blob
    * Azure Data Lake Storage Gen1
    * Azure Data Lake Storage Gen2
    * Azure SQL Veritabanı

__Yapılandırma__

* Verilerinize erişmek için veri __depolarını yönetilen bir kimlik kullanacak şekilde yapılandırın__ . Bu adımlar, Azure Kaynak tabanlı erişim denetimi (RBAC) kullanarak çalışma alanı yönetilen kimliğini depolama hizmetine bir __okuyucu__ olarak ekler. __Okuyucu__ erişimi, çalışma alanının güvenlik duvarı ayarlarını almasına ve verilerin sanal ağdan çıkmadığınızdan emin olmanızı sağlar.

    1. Studio 'da __veri depoları__' nı seçin.

    1. Yeni bir veri deposu oluşturmak için __+ Yeni veri deposu__' nu seçin. Var olan bir güncelleştirmeyi güncelleştirmek için veri deposunu seçin ve __kimlik bilgilerini güncelleştir__' i seçin.

    1. Veri deposu ayarları ' nda, __çalışma alanı yönetilen kimliği ' ni kullanarak Azure Machine Learning hizmetinin depolamaya erişmesine Izin ver__' __i seçin.__

    > [!NOTE]
    > Bu değişikliklerin etkili olması 10 dakika kadar sürebilir.

* __Azure Blob depolama__için çalışma alanı yönetilen kimliği, blob depolamadan veri okuyabilmesi Için [BLOB veri okuyucusu](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) olarak da eklenmelidir.

* Tasarımcı, çıktıyı varsayılan olarak depolamak için çalışma alanınıza bağlı depolama hesabını kullanır. Bununla birlikte, erişimi olan herhangi bir veri deposu için çıktıyı depolamak üzere belirtebilirsiniz. Ortamınız sanal ağlar kullanıyorsa, verilerinizin güvenli ve erişilebilir kalmasını sağlamak için bu denetimleri kullanabilirsiniz. Bir işlem hattı için yeni bir varsayılan depolama alanı ayarlamak için:

    1. İşlem hattı Taslağınızda, işlem hatlarınızın başlığının yakınında bulunan **Ayarlar dişli simgesini** seçin.
    1. **Varsayılan veri deposu Seç** girişini seçin.
    1. Yeni bir veri deposu belirtin.

    Ayrıca modül temelinde varsayılan veri deposunu geçersiz kılabilirsiniz. Bu, her bir modülün depolama konumu üzerinde denetim sağlar.

    1. Çıktısını belirtmek istediğiniz modülü seçin.
    1. **Çıkış ayarları** bölümünü genişletin.
    1. **Varsayılan çıkış ayarlarını geçersiz kıl ' ı**seçin.
    1. **Çıkış ayarlarını ayarla**' yı seçin.
    1. Yeni bir veri deposu belirtin.

* __Azure Data Lake Storage 2.__ kullanıyorsanız, bir sanal ağ içindeki veri erişimini denetlemek IÇIN hem RBAC hem de POSIX stili erişim denetim listelerini (ACL 'ler) kullanabilirsiniz.

    RBAC 'yi kullanmak için, çalışma alanı yönetilen kimliğini [BLOB veri okuyucusu](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) rolüne ekleyin. Daha fazla bilgi için bkz. [rol tabanlı erişim denetimi](../storage/blobs/data-lake-storage-access-control.md#role-based-access-control).

    ACL 'Leri kullanmak için, her türlü güvenlik ilkesiyle aynı şekilde, çalışma alanı yönetilen kimliğine erişim atanabilir. Daha fazla bilgi için bkz. [dosya ve dizinlerdeki erişim denetim listeleri](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

* __Azure Data Lake Storage 1.__ yalnızca POSIX stili erişim denetim listelerini destekler. Çalışma alanı yönetilen kimlik erişimini diğer tüm güvenlik ilkelerine benzer şekilde kaynaklara atayabilirsiniz. Daha fazla bilgi için [Azure Data Lake Storage 1. erişim denetimi](../data-lake-store/data-lake-store-access-control.md)bölümüne bakın.

* Yönetilen kimlik kullanarak bir __Azure SQL veritabanında__ depolanan verilere erişmek için, yönetilen kimliğe eşlenen bir SQL kapsanan Kullanıcı oluşturmanız gerekir. Bir dış sağlayıcıdan Kullanıcı oluşturma hakkında daha fazla bilgi için bkz. [Azure AD kimliklerine eşlenmiş içerilen kullanıcılar oluşturma](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities).

    Bir SQL kapsanan Kullanıcı oluşturduktan sonra, bu kullanıcıya izin ver [T-SQL komutunu](https://docs.microsoft.com/sql/t-sql/statements/grant-object-permissions-transact-sql)kullanarak izin verin.

* __Studio 'ya bir sanal ağın içindeki bir kaynaktan erişiyorsanız__ (örneğin, bir işlem örneği veya sanal makine), sanal ağdan Studio 'ya giden trafiğe izin vermeniz gerekir. 

    Örneğin, giden trafiği kısıtlamak için ağ güvenlik grupları (NSG) kullanıyorsanız, __Azurefrontkapısı. ön uç__'nin __hizmet etiketi__ hedefine bir kural ekleyin.

## <a name="use-datastores-and-datasets"></a>Veri depolarını ve veri kümelerini kullanma

> [!NOTE]
> Bu bölümde SDK deneyiminin veri deposu ve veri kümesi kullanımı ele alınmaktadır. Studio deneyimi hakkında daha fazla bilgi için [studio Machine Learning](#machine-learning-studio)bölümüne bakın.

__Sınırlamalar__

Varsayılan olarak, Azure Machine Learning SDK kullanarak verilere erişmeye çalıştığınızda veri geçerliliği ve kimlik bilgisi denetimleri gerçekleştirir. Verileriniz bir sanal ağın arkasındaysa Azure Machine Learning verilere erişemez ve denetimlerinde başarısız olur. Bu sorunu geçici olarak çözmek için veri depoları ve veri kümeleri oluştururken doğrulamayı atlayın.

* Bir __veri deposu__kullanırken:

    Azure Data Lake Store gen1 ve Azure Data Lake Store Gen2, varsayılan olarak doğrulamayı atlayın, bu nedenle başka bir eylem gerekmez. Ancak aşağıdaki hizmetlerde, veri deposu doğrulamasını atlamak için benzer sözdizimini kullanabilirsiniz:

    - Azure Blob depolama
    - Azure FileShare
    - PostgreSQL
    - Azure SQL Veritabanı

    Aşağıdaki kod örneği, yeni bir Azure blob veri deposu ve kümesi oluşturur `skip_validation=True` .

    ```python
    blob_datastore = Datastore.register_azure_blob_container(workspace=ws,  
                                                            datastore_name=blob_datastore_name,  
                                                            container_name=container_name,  
                                                            account_name=account_name, 
                                                            account_key=account_key, 
                                                            skip_validation=True ) // Set skip_validation to true
    ```

* __Veri kümesi__kullanırken:

    Veri kümesi doğrulamasını atlamak için sözdizimi aşağıdaki veri kümesi türleri için benzerdir:
    - Ayrılmış dosya
    - JSON 
    - Parquet
    - SQL
    - Dosya

    Aşağıdaki kod, yeni bir JSON veri kümesi ve kümesi oluşturur `validate=False` .

    ```python
    json_ds = Dataset.Tabular.from_json_lines_files(path=datastore_paths, 
                                                   validate=False) 
    ```

## <a name="azure-storage-account"></a>Azure Storage hesabı

> [!IMPORTANT]
> Yalnızca Azure Machine Learning için _varsayılan depolama hesabını_ ve _varsayılan olmayan depolama hesaplarını_ bir sanal ağa yerleştirebilirsiniz.

__Gereksinimler__

* Depolama hesabı, eğitim veya çıkarım için kullanılan işlem örnekleri veya kümeler ile aynı sanal ağ ve alt ağ içinde olmalıdır.

__Yapılandırma__

Çalışma alanınız tarafından kullanılan Azure depolama hesabının güvenliğini sağlamak için, sanal ağınızdaki depolama hesabı için __özel bir uç nokta__ veya __hizmet uç noktası__ etkinleştirin.

* Depolama hesabını __özel bir uç nokta__kullanacak şekilde yapılandırmak için [Özel uç noktaları kullanma](/azure/storage/common/storage-private-endpoints.md) makalesine bakın.

* Depolama hesabını bir __hizmet uç noktası__kullanacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

    1. Depolama hesabını, çalışma alanınız tarafından kullanılan sanal ağa eklemek için [Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma](/azure/storage/common/storage-network-security#grant-access-from-a-virtual-network) makalesindeki __bir sanal ağ üzerinden erişim izni verme__ konusunun bilgilerini kullanın.
    1. Sanal ağdaki (örneğin, Azure Machine Learning) Microsoft hizmetlerinden erişime izin vermek için [Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma](/azure/storage/common/storage-network-security#exceptions) makalesinin __özel durumlar__ bölümünde yer alan bilgileri kullanın.
    1. Azure Machine Learning SDK ile çalışırken, geliştirme ortamınızın Azure depolama hesabına bağlanabiliyor olması gerekir. Depolama hesabı bir sanal ağın içindeyken güvenlik duvarının, geliştirme ortamının IP adresinden erişime izin vermelidir. Geliştirme ortamının IP adresini eklemek için, [Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma](/azure/storage/common/storage-network-security#grant-access-from-an-internet-ip-range) MAKALESININ __Internet IP aralığından erişim izni verme__ bölümünde bulunan bilgileri kullanın.

## <a name="azure-container-registry"></a>Azure Container Registry

__Gereksinimler__

* Azure Machine Learning çalışma alanınız Enterprise Edition olmalıdır. Yükseltme hakkında bilgi için bkz. [Enterprise Edition 'A yükseltme](how-to-manage-workspace.md#upgrade).
* Azure Machine Learning çalışma alanı bölgeniz [özel bağlantı etkin bölge](https://docs.microsoft.com/azure/private-link/private-link-overview#availability)olmalıdır. 
* Azure Container Registry Premium sürüm olmalıdır. Yükseltme hakkında daha fazla bilgi için bkz. [SKU 'Ları değiştirme](/azure/container-registry/container-registry-skus#changing-skus).
* Azure Container Registry, eğitim veya çıkarım için kullanılan depolama hesabı ve işlem hedefleri ile aynı sanal ağ ve alt ağ içinde olmalıdır.
* Azure Machine Learning çalışma alanınızın bir [Azure Machine Learning işlem kümesi](how-to-set-up-training-targets.md#amlcompute)içermesi gerekir.

__Sınırlamalar__

* ACR bir sanal ağın arkasındaysa, Azure Machine Learning Docker görüntülerini doğrudan oluşturmak için kullanamaz. Bunun yerine, görüntüleri oluşturmak için işlem kümesi kullanılır.

__Yapılandırma__

1. Çalışma alanınızın Azure Container Registry adını bulmak için aşağıdaki yöntemlerden birini kullanın:

    __Azure Portal__

    Çalışma alanınızın genel bakış bölümünden __kayıt defteri__ değeri Azure Container Registry bağlanır.

    :::image type="content" source="./media/how-to-enable-virtual-network/azure-machine-learning-container-registry.png" alt-text="Çalışma alanı için Azure Container Registry" border="true":::

    __Azure CLI__

    [Azure CLI için Machine Learning uzantısını yüklediyseniz](reference-azure-machine-learning-cli.md), `az ml workspace show` çalışma alanı bilgilerini göstermek için komutunu kullanabilirsiniz.

    ```azurecli-interactive
    az ml workspace show -w yourworkspacename -g resourcegroupname --query 'containerRegistry'
    ```

    Bu komut şuna benzer bir değer döndürür `"/subscriptions/{GUID}/resourceGroups/{resourcegroupname}/providers/Microsoft.ContainerRegistry/registries/{ACRname}"` . Dizenin son bölümü, çalışma alanının Azure Container Registry adıdır.

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

1. Çalışma alanınızın ACR örneğiyle iletişim kurmasını sağlamak için aşağıdaki Azure Resource Manager şablonunu uygulayın:

    > [!WARNING]
    > Bu şablon, çalışma alanınız için özel bir uç nokta sağlar ve onu bir kurumsal çalışma alanına dönüştürür. Bu değişiklikleri geri alamazsınız.

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

## <a name="key-vault-instance"></a>Anahtar Kasası örneği 

__Gereksinimler__

__Sınırlamalar__

__Yapılandırma__ 

Bir sanal ağın arkasındaki Azure Key Vault Azure Machine Learning deneme yeteneklerini kullanmak için, [Azure Key Vault güvenlik duvarlarını ve sanal ağları Yapılandır](/azure/key-vault/general/network-security) makalesini kullanın.

> [!IMPORTANT]
> Makalesindeki adımları izleyerek, deneme işlem kaynaklarınız tarafından kullanılan sanal ağı kullanın. Ayrıca, __Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına de izin vermeniz__gerekir.

## <a name="compute-clusters--instances"></a><a name="compute-instance"></a>İşlem kümeleri & örnekleri 

__Gereksinimler__

* Sanal ağın, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olması gerekir.
* İşlem örneği veya küme için belirtilen alt ağda, hedeflenen sanal makine sayısına uyum sağlamak için yeterli sayıda atanmamış IP adresi olmalıdır. Alt ağda yeterli sayıda atanmamış IP adresi yoksa, bir işlem kümesi kısmen tahsis edilir.
* Trafiği kısıtlayarak sanal ağı güvenli hale getirmeye çalışırsanız, işlem hizmeti için bazı bağlantı noktaları açık bırakılmalıdır.
* Bir sanal ağa birden çok işlem örneği veya kümesi koyacaksanız bir veya daha fazla kaynağınız için bir kota artışı istemeniz gerekebilir.
* Çalışma alanı için Azure depolama hesabı bir sanal ağda da güvenlik altına alınırsa, Azure Machine Learning işlem örneğiyle veya kümeyle aynı sanal ağda olmaları gerekir. 
* İşlem örneği jupi işlevinin çalışması için, Web yuva iletişiminin devre dışı bırakılmadığından emin olun.

__Sınırlamalar__

* Machine Learning işlem örneği veya kümesi, __sanal ağı içeren kaynak grubunda__ek ağ kaynaklarını otomatik olarak ayırır. Her işlem örneği veya kümesi için hizmet aşağıdaki kaynakları ayırır:

    * Bir ağ güvenlik grubu
    * Bir genel IP adresi
    * Bir yük dengeleyici
    
    __İşlem kümeleri__için, kümenin 0 düğüme her ölçeklendirilişinde bu kaynaklar silinir (ve yeniden oluşturulur).
    
    __İşlem örneği__ için, örnek silinene kadar kaynaklar üzerinde tutulur (durduruluyor kaynakları kaldırmaz).

    Bu kaynaklar, aboneliğin [kaynak kotalarıyla](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits) sınırlıdır.

__Yapılandırma__

* Machine Learning İşlem kümesi oluşturmak için aşağıdaki adımları kullanın:

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

* Azure Işlem örneği üzerinde not defterleri kullanıyorsanız, not defterinizin, verileriniz ile aynı sanal ağın ve alt ağın arkasındaki bir işlem kaynağında çalıştığından emin olmanız gerekir. 

    İşlem örneğinizi oluşturma sırasında aynı sanal ağda olacak şekilde yapılandırın **Gelişmiş ayarlar**altında  >  **sanal ağı yapılandırın**. Mevcut bir Işlem örneğini bir sanal ağa ekleyemezsiniz.

* Ağ trafiğini genel İnternet ile kısıtlayarak sanal ağın güvenliğini sağlamayı planlıyorsanız Azure Batch hizmetinden gelen iletişimlere izin vermeniz gerekir.

    Batch hizmeti, sanal makinelere bağlı ağ arabirimi (NIC) düzeyinde ağ güvenlik grupları (NSG 'Ler) ekler. Bu NSG'ler şu trafiğe izin vermek için gelen ve giden bağlantı kurallarını otomatik olarak yapılandırır:

    - __Batchnodemanagement__ __hizmet etiketinden__ 29876 ve 29877 bağlantı noktalarında gelen TCP trafiği.

    - Seçim Uzaktan erişime izin vermek için 22 numaralı bağlantı noktasında gelen TCP trafiği. Bu bağlantı noktasını yalnızca genel IP üzerinde SSH kullanarak bağlanmak istiyorsanız kullanın.

    - Sanal ağa giden herhangi bir bağlantı noktasında giden trafik.

    - İnternete giden herhangi bir bağlantı noktasında giden trafik.

    - 44224 numaralı bağlantı noktasında işlem örneği gelen TCP trafiği için __AzureMachineLearning__bir __hizmet etiketinden__ .

    > [!IMPORTANT]
    > Batch tarafından yapılandırılmış olan NSG'lerdeki gelen veya giden kurallarını değiştirirken veya yenilerini eklerken dikkatli olun. Bir NSG, işlem düğümleriyle iletişimi engelliyorsa, işlem hizmeti işlem düğümlerinin durumunu kullanılamıyor olarak ayarlar.
    >
    > Azure Batch hizmeti kendi NSG 'leri yapılandırdığından alt ağ düzeyinde NSG 'leri belirtmeniz gerekmez. Ancak, Azure Machine Learning işlem içeren alt ağda ilişkili NSG 'ler veya güvenlik duvarı varsa, daha önce listelenen trafiğe de izin vermeniz gerekir.

* Varsayılan giden kurallarını kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlandırmak istiyorsanız aşağıdaki adımları kullanın:

    1. NSG kurallarını kullanarak giden internet bağlantısını reddedin.
    1. Bir __işlem örneği__ veya bir __işlem kümesi__için, giden trafiği aşağıdaki öğelerle sınırlayın:
        - __Depolama. RegionName__ __hizmet etiketi__ kullanılarak Azure Storage. Burada `{RegionName}` bir Azure bölgesinin adıdır.
        - Azure Container Registry, __AzureContainerRegistry. RegionName__ __hizmet etiketi__ kullanılarak. Burada `{RegionName}` bir Azure bölgesinin adıdır.
        - __AzureMachineLearning__ __hizmet etiketi__ kullanılarak Azure Machine Learning
        - __AzureResourceManager__ __hizmet etiketi__ kullanılarak Azure Resource Manager
        - __AzureActiveDirectory__ __hizmet etiketi__ kullanılarak Azure Active Directory

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

* Azure Machine Learning işlem ile [Zorlamalı tünel](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm) kullanıyorsanız, genel İnternet ile işlem kaynağını içeren alt ağdan iletişime izin vermeniz gerekir. Bu iletişim, görev zamanlama ve Azure depolama erişimi için kullanılır.

    Bunu yapmanın iki yolu vardır:

    * Bir [sanal ağ NAT](../virtual-network/nat-overview.md)kullanın. Bir NAT ağ geçidi, sanal ağınızdaki bir veya daha fazla alt ağ için giden internet bağlantısı sağlar. Bilgi için bkz. [NAT Gateway kaynaklarıyla sanal ağları tasarlama](../virtual-network/nat-gateway-resource.md).

    * İşlem kaynağını içeren alt ağa [Kullanıcı tanımlı yollar (UDRs)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) ekleyin. Kaynaklarınızın bulunduğu bölgedeki Azure Batch hizmeti tarafından kullanılan her IP adresi için bir UDR oluşturun. Bu UDRs, Batch hizmetinin görev zamanlama için işlem düğümleriyle iletişim kurmasını sağlar. Ayrıca, Işlem örneklerine erişim için gerekli olduğundan, kaynakların bulunduğu Azure Machine Learning hizmeti için IP adresini de ekleyin. Batch hizmetinin ve Azure Machine Learning hizmetinin IP adreslerinin bir listesini almak için aşağıdaki yöntemlerden birini kullanın:

        * [Azure IP aralıklarını ve hizmet etiketlerini](https://www.microsoft.com/download/details.aspx?id=56519) indirin ve dosyada `BatchNodeManagement.<region>` ve `AzureMachineLearning.<region>` Azure bölgeniz olduğu yerde arama yapın `<region>` .

        * Bilgileri indirmek için [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 'yi kullanın. Aşağıdaki örnek, IP adresi bilgilerini indirir ve Doğu ABD 2 bölgesinin bilgilerini filtreler:

            ```azurecli-interactive
            az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
            az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'AzureMachineLearning')] | [?properties.region=='eastus2']"
            ```
        
        UDRs 'yi eklediğinizde, ilgili her Batch IP adresi ön eki için yolu tanımlayın ve __sonraki atlama türünü__ __Internet__olarak ayarlayın. 

        Tanımladığınız tüm UDRs 'ye ek olarak, şirket içi ağ gereciniz aracılığıyla Azure Storage 'a giden trafiğe izin verilmelidir. Özellikle, bu trafiğin URL 'Leri şu formlarda bulunur: `<account>.table.core.windows.net` , `<account>.queue.core.windows.net` ve `<account>.blob.core.windows.net` . 

        Daha fazla bilgi için bkz. [Sanal ağda Azure Batch havuzu oluşturma](../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling).

<a id="aksvnet"></a>

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

__Gereksinimler__

* Azure Kubernetes hizmeti (AKS) örneği ve Azure sanal ağı aynı bölgede olmalıdır. Bir sanal ağ içinde çalışma alanı tarafından kullanılan Azure depolama hesaplarını güvenli hale getirmek istiyorsanız, AKS örneğiyle aynı sanal ağda olmaları gerekir.

* Kümenizin __IP adresini planlamak__ Için [Azure Kubernetes SERVICE (aks) makalesinde gelişmiş ağı yapılandırma](https://docs.microsoft.com/azure/aks/configure-azure-cni#prerequisites) makalesindeki önkoşulları izleyin.

* AKS kümesiyle gelen ve giden iletişimi kısıtlamak için [Azure Kubernetes hizmeti makalesindeki küme düğümleri Için denetim çıkış trafiği](/azure/aks/limit-egress-traffic) bölümündeki yönergeleri izleyerek aks 'den _giden_ iletişimin doğru yapılandırıldığından emin olun. Varsa, _gelen_ iletişim gereksinimleri aşağıdaki yapılandırma bölümünde çağırılır.

__Sınırlamalar__

* Özel bağlantısı etkinleştirilmiş bir Azure Kubernetes hizmeti kullanmak istiyorsanız, çalışma alanınıza iliştirmelidir. Azure Machine Learning (SDK, Portal, CLı, vb.) özel bağlantısı olan bir Azure Kubernetes hizmet kümesi oluşturamazsınız.

__Yapılandırma__

> [!IMPORTANT]
> Bu bölüm birden çok yapılandırma içerir. Gereksinimlerinize en uygun olanı seçin.

* __Sanal ağın arkasındaki AKS 'leri ortak yük dengeleyici ile birlikte kullanmak__için:

    1. AKS kümesini oluşturun veya ekleyin. Yeni bir küme __oluşturuyorsanız__ , küme için kullanılacak sanal ağı belirtmeniz gerekir.
    
        Aşağıdaki örnek, Python SDK kullanarak yeni bir AKS kümesi oluşturmayı göstermektedir:

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

        Zaten sanal ağın arkasında olan bir AKS kümeniz varsa [Azure Kubernetes 'e dağıtma](how-to-deploy-azure-kubernetes-service.md#attach-an-existing-aks-cluster) makalesindeki bilgileri kullanın.

    1. Sanal ağı denetleyen ağ güvenlik grubunun, sanal ağ dışından çağrılabilmesi için Puanlama uç noktası için etkinleştirilmiş bir __gelen__ güvenlik kuralına sahip olduğundan emin olun.

* __Sanal ağın arkasındaki AKS 'leri özel yük dengeleyicisiyle birlikte kullanmak__için:

    1. AKS kümesini oluşturun veya ekleyin. Yeni bir küme __oluşturuyorsanız__ , küme için kullanılacak sanal ağı belirtmeniz gerekir.
    
        Aşağıdaki örnek, Python SDK kullanarak yeni bir AKS kümesi oluşturmayı göstermektedir:

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

        Zaten sanal ağın arkasında olan bir AKS kümeniz varsa [Azure Kubernetes 'e dağıtma](how-to-deploy-azure-kubernetes-service.md#attach-an-existing-aks-cluster) makalesindeki bilgileri kullanın.

    1. AKS için hizmet sorumlusu veya yönetilen kimlik KIMLIĞINI bulmak için aşağıdaki Azure CLı komutlarını kullanın. `<aks-cluster-name>`Kümenin adıyla değiştirin. `<resource-group-name>` _, Aks kümesini içeren_kaynak grubunun adıyla değiştirin:

        ```azurecli-interactive
        az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query servicePrincipalProfile.clientId
        ``` 

        Bu komut değerini döndürürse `msi` , yönetilen kimliğin asıl kimliğini tanımlamak için aşağıdaki komutu kullanın:

        ```azurecli-interactive
        az aks show -n <aks-cluster-name> --resource-group <resource-group-name> --query identity.principalId
        ```
    1. Sanal ağınızı içeren kaynak grubunun KIMLIĞINI bulmak için aşağıdaki komutu kullanın. `<resource-group-name>` _Sanal ağı içeren_kaynak grubunun adıyla değiştirin:

        ```azurecli-interactive
        az group show -n <resource-group-name> --query id
        ```

    1. Hizmet sorumlusu veya yönetilen kimliği bir ağ katılımcısı olarak eklemek için aşağıdaki komutu kullanın. `<SP-or-managed-identity>`Hizmet sorumlusu veya yönetilen kimlik için döndürülen kimlikle değiştirin. `<resource-group-id>`Sanal ağı içeren kaynak grubu için döndürülen kimlikle değiştirin:

        ```azurecli-interactive
        az role assignment create --assignee <SP-or-managed-identity> --role 'Network Contributor' --scope <resource-group-id>
        ```

    1. AKS kümesini __özel yük dengeleyici__kullanmak üzere güncelleştirmek için Python SDK 'sını kullanın. Aşağıdaki kod parçacığı, eklenmiş veya çalışma alanına eklenmiş olan bir AKS kümesinin nasıl güncelleştirilmesini göstermektedir:
    
        ```python
        import azureml.core
        from azureml.core.compute.aks import AksUpdateConfiguration
        from azureml.core.compute import AksCompute

        # ws = workspace object. Creation not shown in this snippet
        aks_target = AksCompute(ws,"myaks")

        # Change to the name of the subnet that contains AKS
        subnet_name = "default"
        # Update AKS configuration to use an internal load balancer
        update_config = AksUpdateConfiguration(None, "InternalLoadBalancer", subnet_name)
        aks_target.update(update_config)
        # Wait for the operation to complete
        aks_target.wait_for_completion(show_output = True)
        ```

* __Özel bir uç nokta kullanarak AKS eklemek__için:

    1. AKS kümesinin kullanacağı alt ağın __alt ağ kimliğini__ almak Için AŞAĞıDAKI Azure CLI komutunu kullanın. Örneğin, sanal ağınızın varsayılan alt ağı:
    
        ```azurecli
        az network vnet show -g myresourcegroup -n myvnet --query subnets[].id
        ```
        
        Bu komut, sanal ağdaki alt ağlar için bir kimlik dizisi döndürür. Aşağıdaki JSON yalnızca bir alt ağa sahip olan bir sanal ağdan bir örnektir:

        ```json
        [
            "/subscriptions/GUID/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/myvnet/subnets/default"
        ]
        ```

        Birden çok kimlik döndürülürse, kullanmak istediğiniz birini seçin.

    1. Bir AKS kümesini özel bir uç nokta oluşturmak için [özel Azure Kubernetes hizmet kümesi oluşturma](/azure/aks/private-clusters#advanced-networking) makalesinin __Gelişmiş ağ__ bölümünde bulunan bilgileri kullanın. Kümeyi oluştururken, önceki komuttan gelen alt ağ KIMLIĞINI `--vnet-subnet-id` parametresiyle kullanın.

    1. Kümeyi eklemek için [Azure Kubernetes 'e dağıtma](how-to-deploy-azure-kubernetes-service.md#attach-an-existing-aks-cluster) makalesindeki bilgileri kullanın.

    > [!TIP]
    > Bu yapılandırmayı kullanırken ve AKS kümesinden giden iletişimi kısıtlandığında, __1194__ veya __9000__bağlantı noktalarında iletişime izin vermeniz gerekmez. İzin verilmesi gereken diğer bağlantı noktaları için, [Azure Kubernetes hizmeti makalesindeki küme düğümleri Için denetim çıkış trafiği](/azure/aks/limit-egress-traffic) bölümündeki kılavuza bakın.

## <a name="use-azure-container-instances-aci"></a>Azure Container Instances kullanın (ACI)

__Gereksinimler__

* Azure Container Instances, bir model dağıtıldığında dinamik olarak oluşturulur. Azure Machine Learning sanal ağ içinde ACI oluşturmak üzere etkinleştirmek için, dağıtım tarafından kullanılan alt ağ için __alt ağ temsilcisini__ etkinleştirmeniz gerekir.

__Sınırlamalar__

* Sanal ağın Azure Machine Learning çalışma alanı ile aynı kaynak grubunda olması gerekir.

* Çalışma alanınızın Azure Container Registry (ACR) Sanal ağda de olamaz.

__Yapılandırma__

Çalışma alanınıza bir sanal ağda ACI 'yi kullanmak için aşağıdaki adımları kullanın:

1. Sanal ağınızda alt ağ temsilcisini etkinleştirmek için [alt ağ temsili ekleme veya kaldırma](../virtual-network/manage-subnet-delegation.md) makalesindeki bilgileri kullanın. Bir sanal ağ oluştururken temsilciyi etkinleştirebilir veya var olan bir ağa ekleyebilirsiniz.

    > [!IMPORTANT]
    > Temsilci seçme etkinleştirildiğinde, `Microsoft.ContainerInstance/containerGroups` __hizmet verme için alt ağ__ olarak kullanın.

2. [Aciwebservice. deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none--vnet-name-none--subnet-name-none-)kullanarak modeli dağıtın, `vnet_name` ve `subnet_name` parametrelerini kullanın. Bu parametreleri, temsilciyi etkinleştirdiğiniz sanal ağ adı ve alt ağa ayarlayın.

## <a name="azure-databricks"></a>Azure Databricks

__Gereksinimler__

* Sanal ağın, Azure Machine Learning çalışma alanıyla aynı abonelikte ve bölgede olması gerekir.
* Çalışma alanı için Azure depolama hesabı bir sanal ağda da güvenlik altına alınırsa, Azure Databricks kümesiyle aynı sanal ağda olmaları gerekir.
* Azure Databricks tarafından kullanılan __databricks-Private__ ve __databricks-genel__ alt ağlarına ek olarak, sanal ağ için oluşturulan __varsayılan__ alt ağ de gereklidir.

__Sınırlamalar__

__Yapılandırma__

Bir sanal ağla Azure Databricks kullanma hakkında ayrıntılı bilgi için bkz. [Azure sanal ağınızda Azure Databricks dağıtma](https://docs.azuredatabricks.net/administration-guide/cloud-configurations/azure/vnet-inject.html).

<a id="vmorhdi"></a>

## <a name="virtual-machine-or-hdinsight-cluster"></a>Sanal makine veya HDInsight kümesi

__Gereksinimler__

* Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler.
* SSH bağlantı noktasının sanal makinede veya HDInsight kümesinde etkinleştirilmesi gerekir.

__Sınırlamalar__

__Yapılandırma__

1. Azure portal veya Azure CLı kullanarak bir VM veya HDInsight kümesi oluşturun ve kümeyi bir Azure sanal ağına yerleştirin. Daha fazla bilgi için aşağıdaki makaleleri inceleyin:

    * [Linux VM 'Ler için Azure sanal ağları oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Azure sanal ağını kullanarak HDInsight 'ı genişletme](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Bir NSG, Linux tabanlı Azure sanal makineleri için otomatik olarak oluşturulur. Bu NSG, herhangi bir kaynaktan 22 numaralı bağlantı noktasına erişim sağlar. SSH bağlantı noktasına erişimi sınırlandırmak istiyorsanız Azure Machine Learning erişimine izin vermeniz gerekir. Azure ML erişimini korumak için, __AzureMachineLearning__ __kaynak hizmeti etiketiyle__ bir __kaynak hizmetten__ erişime izin vermeniz gerekir. Örneğin, aşağıdaki Azure CLı komutları SSH kuralını yalnızca Azure Machine Learning erişime izin verecek şekilde değiştirir.

    ```azurecli
    # Get default SSH rule
    nsgrule=$(az network nsg rule list --resource-group myResourceGroup --nsg-name myNetworkSecurityGroup --query [0].name -o tsv)
    # Update network security group rule to limit SSH to source service.
    az network nsg rule update --resource-group myResourceGroup --nsg-name myNetworkSecurityGroupBackEnd \
    --name $nsgrule --protocol tcp --direction inbound --priority 100 \
    --source-address-prefix AzureMachineLearning --source-port-range '*' --destination-address-prefix '*' \
    --destination-port-range 22 --access allow
    ```

    Daha fazla bilgi için, Linux sanal makineleri için Azure sanal ağları 'nın [ağ güvenlik grupları oluşturma](/azure/virtual-machines/linux/tutorial-virtual-network#create-network-security-groups) makalesine bakın.
    
    Ağ güvenlik grubu için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.

1. Azure Machine Learning çalışma alanınıza VM veya HDInsight kümesini ekleyin. Daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Güvenlik duvarının arkasındaki Azure Machine Learning çalışma alanını kullanın](how-to-access-azureml-behind-firewall.md).
* [Eğitim ortamlarını ayarlama](how-to-set-up-training-targets.md)
* [Özel uç noktaları ayarlama](how-to-configure-private-link.md)
* [Modellerin dağıtılacağı yerler](how-to-deploy-and-where.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
