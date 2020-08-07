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
ms.openlocfilehash: 7684933a7671f1f3d573cb8195551a56ebc27947
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87925830"
---
# <a name="network-isolation-during-training--inference-with-private-virtual-networks"></a>Özel sanal ağlarla eğitim sırasında ağ yalıtımı & çıkarım
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, bir Azure sanal ağı (VNet) içinde Azure Machine Learning eğitim ve çıkarım işlerini yalıtarak makine öğrenimi yaşam döngülerini nasıl sağlayacağınızı öğreneceksiniz. Azure Machine Learning, işlem [hedefleri](concept-compute-target.md)olarak da bilinen, modelleri eğitmek ve dağıtmak için işlem kaynakları Için diğer Azure hizmetlerini kullanır. Hedefler bir sanal ağ içinde oluşturulabilir. Örneğin, bir modeli eğitme ve sonra modeli Azure Kubernetes Service (AKS) ' e dağıtmak için Azure Machine Learning işlem kullanabilirsiniz. 

Bir __sanal ağ__ , Azure kaynaklarınızı genel İnternet 'ten yalıtmak için bir güvenlik sınırı görevi görür. Ayrıca, bir Azure sanal ağını şirket içi ağınıza da katabilirsiniz. Ağları birleştirerek, modellerinizi güvenli bir şekilde eğitebilir ve çıkarım için dağıtılan modellerinize erişebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ Azure Machine Learning [çalışma alanı](how-to-manage-workspace.md).

+ [Azure sanal ağ hizmeti](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ve [IP ağının](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)genel çalışma bilgileri.

+ İşlem kaynaklarınızla kullanılacak önceden var olan bir sanal ağ ve alt ağ.

## <a name="private-endpoints"></a>Özel uç noktalar

Ayrıca, özel bir uç nokta kullanarak çalışma alanınıza bağlanmak için [Azure özel bağlantısını etkinleştirebilirsiniz](how-to-configure-private-link.md) . Özel uç nokta, sanal ağınız içindeki özel IP adresleri kümesidir. [Bu özel uç noktayı ayarlamayı öğrenin.](how-to-configure-private-link.md)



> [!TIP]
> Çalışma alanınız ve diğer Azure kaynakları arasındaki iletişimi korumak için sanal ağ ve özel bağlantıyı birlikte birleştirebilirsiniz. Ancak, bazı birleşimler Enterprise Edition çalışma alanı gerektirir. Hangi senaryoların Enterprise Edition gerektirdiğini anlamak için aşağıdaki tabloyu kullanın:
>
> | Senaryo | Kurumsal</br>Edition | Temel</br>Edition |
> | ----- |:-----:|:-----:| 
> | Sanal ağ veya özel bağlantı yok | ✔ | ✔ |
> | Özel bağlantı olmadan çalışma alanı. Bir sanal ağdaki diğer kaynaklar (Azure Container Registry hariç) | ✔ | ✔ |
> | Özel bağlantı olmadan çalışma alanı. Özel bağlantısı olan diğer kaynaklar | ✔ | |
> | Özel bağlantılı çalışma alanı. Bir sanal ağdaki diğer kaynaklar (Azure Container Registry hariç) | ✔ | ✔ |
> | Çalışma alanı ve özel bağlantısı olan diğer kaynaklar | ✔ | |
> | Özel bağlantılı çalışma alanı. Özel bağlantı veya sanal ağ içermeyen diğer kaynaklar | ✔ | ✔ |
> | Bir sanal ağda Azure Container Registry | ✔ | |
> | Çalışma alanı için müşteri tarafından yönetilen anahtarlar | ✔ | |
> 

> [!WARNING]
> 
> Azure Machine Learning işlem örnekleri önizlemesi, özel bağlantının etkinleştirildiği bir çalışma alanında desteklenmez.

<a id="amlcompute"></a>

## <a name="machine-learning-studio"></a>Machine Learning Studio

Verileriniz bir sanal ağda depolanıyorsa, Studio 'ya verilerinize erişim vermek için çalışma alanı [tarafından yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md) kullanmanız gerekir.

> [!IMPORTANT]
> Çoğu Studio, bir sanal ağda depolanan verilerle birlikte çalışarak, tümleşik Not defterleri __değildir__. Tümleşik Not defterleri, sanal bir ağdaki depolamanın kullanımını desteklemez. Bunun yerine, Jupyıter not defterlerini bir işlem örneğinden kullanabilirsiniz. Daha fazla bilgi için, [Işlem örneği Not Defteri Içindeki erişim verileri](#access-data-in-a-compute-instance-notebook) bölümüne bakın.

Studio erişimi verilemez, bu hatayı alırsınız `Error: Unable to profile this dataset. This might be because your data is stored behind a virtual network or your data does not support profile.` ve aşağıdaki işlemleri devre dışı bırakabilirsiniz:

* Studio 'daki verileri önizleyin.
* Tasarımcıda verileri görselleştirin.
* Bir oto ml denemesi gönder.
* Etiketleme projesi başlatın.

Studio, bir sanal ağdaki aşağıdaki veri deposu türlerinden veri okumayı destekler:

* Azure Blob
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Azure SQL Veritabanı

### <a name="add-resources-to-the-virtual-network"></a>Sanal ağa kaynak ekleme 

Çalışma alanınızı ve depolama hesabınızı birbirlerine erişebilecek şekilde aynı sanal ağa ekleyin.

1. Çalışma alanınızı sanal ağa bağlamak için [Azure özel bağlantısını etkinleştirin](how-to-configure-private-link.md). Bu özellik şu anda önizleme aşamasındadır ve ABD Doğu, ABD Batı 2 ABD Orta Güney bölgelerinde kullanılabilir.

1. Depolama hesabınızı sanal ağa bağlamak için [güvenlik duvarları ve sanal ağlar ayarlarını yapılandırın](#use-a-storage-account-for-your-workspace).

### <a name="configure-a-datastore-to-use-managed-identity"></a>Bir veri deposunu yönetilen kimlik kullanacak şekilde yapılandırma

Çalışma alanınızı ve depolama hizmeti hesabınızı sanal ağa ekledikten sonra, verilerinize erişmek için veri depolarını yönetilen kimlik kullanacak şekilde yapılandırmanız gerekir. Bu adımlar, Azure Kaynak tabanlı erişim denetimi (RBAC) kullanarak çalışma alanı yönetilen kimliğini depolama hizmetine bir __okuyucu__ olarak ekler. __Okuyucu__ erişimi, çalışma alanının güvenlik duvarı ayarlarını almasına ve verilerin sanal ağdan çıkmadığınızdan emin olmanızı sağlar.

1. Studio 'da __veri depoları__' nı seçin.

1. Yeni bir veri deposu oluşturmak için __+ Yeni veri deposu__' nu seçin. Var olan bir güncelleştirmeyi güncelleştirmek için veri deposunu seçin ve __kimlik bilgilerini güncelleştir__' i seçin.

1. Veri deposu ayarları ' nda, __çalışma alanı yönetilen kimliği ' ni kullanarak Azure Machine Learning hizmetinin depolamaya erişmesine Izin ver__' __i seçin.__

> [!NOTE]
> Bu değişikliklerin etkili olması 10 dakika kadar sürebilir.

### <a name="azure-blob-storage-blob-data-reader"></a>Azure Blob Depolama Blobu veri okuyucu

__Azure Blob depolama__için çalışma alanı yönetilen kimliği, blob depolamadan veri okuyabilmesi Için bir [BLOB veri okuyucusu](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) olarak da eklenir.


### <a name="azure-machine-learning-designer-default-datastore"></a>Azure Machine Learning Designer varsayılan veri deposu

Tasarımcı, çıktıyı varsayılan olarak depolamak için çalışma alanınıza bağlı depolama hesabını kullanır. Bununla birlikte, erişimi olan herhangi bir veri deposu için çıktıyı depolamak üzere belirtebilirsiniz. Ortamınız sanal ağlar kullanıyorsa, verilerinizin güvenli ve erişilebilir kalmasını sağlamak için bu denetimleri kullanabilirsiniz.

Bir işlem hattı için yeni bir varsayılan depolama alanı ayarlamak için:

1. İşlem hattı Taslağınızda, işlem hatlarınızın başlığının yakınında bulunan **Ayarlar dişli simgesini** seçin.
1. **Varsayılan veri deposunu Seç**' i seçin.
1. Yeni bir veri deposu belirtin.

Ayrıca modül temelinde varsayılan veri deposunu geçersiz kılabilirsiniz. Bu, her bir modülün depolama konumu üzerinde denetim sağlar.

1. Çıktısını belirtmek istediğiniz modülü seçin.
1. **Çıkış ayarları** bölümünü genişletin.
1. **Varsayılan çıkış ayarlarını geçersiz kıl ' ı**seçin.
1. **Çıkış ayarlarını ayarla**' yı seçin.
1. Yeni bir veri deposu belirtin.

### <a name="azure-data-lake-storage-gen2-access-control"></a>Azure Data Lake Storage 2. Access Control

Bir sanal ağ içindeki veri erişimini denetlemek için RBAC ve POSIX stili erişim denetim listelerini (ACL 'Ler) kullanabilirsiniz.

RBAC 'yi kullanmak için, çalışma alanı yönetilen kimliğini [BLOB veri okuyucusu](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) rolüne ekleyin. Daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi (Azure RBAC)](../storage/blobs/data-lake-storage-access-control.md#role-based-access-control).

ACL 'Leri kullanmak için, her türlü güvenlik ilkesiyle aynı şekilde, çalışma alanı yönetilen kimliğine erişim atanabilir. Daha fazla bilgi için bkz. [dosya ve dizinlerdeki erişim denetim listeleri](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).


### <a name="azure-data-lake-storage-gen1-access-control"></a>Azure Data Lake Storage 1. Access Control

Azure Data Lake Storage 1. yalnızca POSIX stili erişim denetim listelerini destekler. Çalışma alanı yönetilen kimlik erişimini diğer tüm güvenlik ilkelerine benzer şekilde kaynaklara atayabilirsiniz. Daha fazla bilgi için [Azure Data Lake Storage 1. erişim denetimi](../data-lake-store/data-lake-store-access-control.md)bölümüne bakın.


### <a name="azure-sql-database-contained-user"></a>Azure SQL veritabanı kullanıcı içeriyordu

Yönetilen kimlik kullanarak bir Azure SQL veritabanında depolanan verilere erişmek için, yönetilen kimliğe eşlenen bir SQL kapsanan Kullanıcı oluşturmanız gerekir. Bir dış sağlayıcıdan Kullanıcı oluşturma hakkında daha fazla bilgi için bkz. [Azure AD kimliklerine eşlenmiş içerilen kullanıcılar oluşturma](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities).

Bir SQL kapsanan Kullanıcı oluşturduktan sonra, bu kullanıcıya izin ver [T-SQL komutunu](https://docs.microsoft.com/sql/t-sql/statements/grant-object-permissions-transact-sql)kullanarak izin verin.

### <a name="connect-to-the-studio"></a>Studio 'ya bağlanma

Studio 'ya bir sanal ağın içindeki bir kaynaktan erişiyorsanız (örneğin, bir işlem örneği veya sanal makine), sanal ağdan Studio 'ya giden trafiğe izin vermeniz gerekir. 

Örneğin, giden trafiği kısıtlamak için ağ güvenlik grupları (NSG) kullanıyorsanız, __Azurefrontkapısı. ön uç__'nin __hizmet etiketi__ hedefine bir kural ekleyin.

## <a name="use-a-storage-account-for-your-workspace"></a>Çalışma alanınız için bir depolama hesabı kullanın

> [!IMPORTANT]
> Bir sanal ağda Azure Machine Learning veya _varsayılan olmayan depolama hesapları_ için hem _varsayılan depolama hesabını_ yerleştirebilirsiniz.
>
> Varsayılan depolama hesabı, bir çalışma alanı oluşturduğunuzda otomatik olarak sağlanır.
>
> Varsayılan olmayan depolama hesapları için, `storage_account` [ `Workspace.create()` işlevindeki](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) parametresi Azure kaynak kimliği 'ne göre özel bir depolama hesabı belirtmenizi sağlar.

Bir sanal ağdaki çalışma alanı için bir Azure depolama hizmeti kullanmak üzere aşağıdaki adımları kullanın:

1. Bir sanal ağın arkasında bir işlem kaynağı (örneğin, bir Machine Learning işlem örneği veya kümesi) oluşturun veya çalışma alanına (örneğin, bir HDInsight kümesi, sanal makine veya Azure Kubernetes hizmet kümesi) bir işlem kaynağı ekleyin. İşlem kaynağı deneme veya model dağıtımı için olabilir.

   Daha fazla bilgi için, bu makaledeki [Machine Learning Işlem kullanma](#amlcompute), [sanal makine veya HDInsight kümesi](#vmorhdi)kullanma ve [Azure Kubernetes hizmet](#aksvnet) bölümlerini kullanma bölümüne bakın.

1. Azure portal, çalışma alanınızda kullanmak istediğiniz depolama hizmetine gidin.

   [![Azure Machine Learning çalışma alanına bağlı depolama alanı](./media/how-to-enable-virtual-network/workspace-storage.png)](./media/how-to-enable-virtual-network/workspace-storage.png#lightbox)

1. Depolama hizmeti hesabı sayfasında __güvenlik duvarları ve sanal ağlar__' ı seçin.

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

## <a name="use-datastores-and-datasets"></a>Veri depolarını ve veri kümelerini kullanma

Bu bölümde SDK deneyiminin veri deposu ve veri kümesi kullanımı ele alınmaktadır. Studio deneyimi hakkında daha fazla bilgi için [studio Machine Learning](#machine-learning-studio)bölümüne bakın.

Varsayılan olarak, Azure Machine Learning SDK kullanarak verilere erişmeye çalıştığınızda veri geçerliliği ve kimlik bilgisi denetimleri gerçekleştirir. Verileriniz bir sanal ağın arkasındaysa Azure Machine Learning verilere erişemez ve denetimlerinde başarısız olur. Bunu önlemek için, doğrulamayı atlayan veri depoları ve veri kümeleri oluşturmanız gerekir.

### <a name="use-a-datastore"></a>Veri deposu kullanma

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

### <a name="use-a-dataset"></a>Veri kümesi kullanma

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


### <a name="required-ports"></a><a id="mlcports"></a>Gerekli bağlantı noktaları

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
    
    UDRs 'yi eklediğinizde, ilgili her Batch IP adresi ön eki için yolu tanımlayın ve __sonraki atlama türünü__ __Internet__olarak ayarlayın. Aşağıdaki görüntüde, Azure portal bu UDR 'nin bir örneği gösterilmektedir:

    ![Adres ön eki için UDR örneği](./media/how-to-enable-virtual-network/user-defined-route.png)

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

<a id="aksvnet"></a>

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Bir sanal ağa Azure Kubernetes hizmeti (AKS) çalışma alanınıza eklemek için aşağıdaki adımları kullanın:

> [!IMPORTANT]
> Aşağıdaki yordama başlamadan önce, [Azure Kubernetes Service (AKS) içindeki gelişmiş ağı yapılandırma (aks)](https://docs.microsoft.com/azure/aks/configure-azure-cni#prerequisites) ve kümenizin IP adreslemesini planlayın bölümündeki önkoşulları izleyin.
>
> AKS örneği ve Azure sanal ağı aynı bölgede olmalıdır. Bir sanal ağ içinde çalışma alanı tarafından kullanılan Azure depolama hesaplarını güvenli hale getirmek istiyorsanız, AKS örneğiyle aynı sanal ağda olmaları gerekir.

1. [Azure Machine Learning Studio](https://ml.azure.com/)' da oturum açın ve ardından aboneliğinizi ve çalışma alanınızı seçin.

1. Sol tarafta __işlem__ ' ı seçin.

1. Merkezden __çıkarım kümeleri__ ' ni seçin ve ardından öğesini seçin __+__ .

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

Azure Kubernetes hizmetini bir sanal ağa eklemek için Azure Machine Learning SDK 'sını de kullanabilirsiniz. Bir sanal ağda zaten bir AKS kümeniz varsa, bunları [aks 'e dağıtma](how-to-deploy-and-where.md)bölümünde açıklandığı gibi çalışma alanına ekleyin. Aşağıdaki kod, `default` adlı bir sanal ağın alt ağında yeni BIR AKS örneği oluşturur `mynetwork` :

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

#### <a name="network-contributor-role"></a>Ağ katılımcısı rolü

> [!IMPORTANT]
> Daha önce oluşturduğunuz bir sanal ağ sağlayarak bir AKS kümesi oluşturur veya iliştirdiyseniz, AKS kümeniz için hizmet sorumlusu (SP) veya yönetilen kimliğe, sanal ağı içeren kaynak grubuna _ağ katılımcısı_ rolü vermelisiniz. İç yük dengeleyiciyi özel IP olarak değiştirmeye çalışmadan önce bunun yapılması gerekir.
>
> Kimliği ağ katılımcısı olarak eklemek için aşağıdaki adımları kullanın:

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
AKS ile iç yük dengeleyiciyi kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes hizmeti ile iç yük dengeleyici kullanma](/azure/aks/internal-lb).

#### <a name="enable-private-ip"></a>Özel IP 'yi etkinleştir

> [!IMPORTANT]
> Azure Kubernetes hizmet kümesini oluştururken özel IP 'yi etkinleştiremezsiniz. Mevcut bir kümeye güncelleştirme olarak etkinleştirilmelidir.

Aşağıdaki kod parçacığı, __Yeni BIR AKS kümesi oluşturmayı__ve ardından özel bir IP/iç yük dengeleyici kullanmak üzere güncelleştirmeyi göstermektedir:

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
az rest --method put --uri https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace>/computes/<compute>?api-version=2018-11-19 --body @body.json
```

`body.json`Komutu tarafından başvurulan dosyanın içeriği AŞAĞıDAKI JSON belgesine benzer:

```json
{ 
    "location": "<region>", 
    "properties": { 
        "resourceId": "/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.ContainerService/managedClusters/<aks-resource-name>", 
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

__Var olan bir kümeyi çalışma alanınıza iliştirirken__ , yük dengeleyiciyi yapılandırmak için iliştirme işleminden sonra beklemeniz gerekir.

Bir küme ekleme hakkında bilgi için bkz. [var olan AKS kümesi ekleme](how-to-deploy-azure-kubernetes-service.md#attach-an-existing-aks-cluster).

Mevcut kümeyi iliştirdikten sonra, kümeyi özel bir IP kullanacak şekilde güncelleştirebilirsiniz.

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

## <a name="use-azure-container-instances-aci"></a>Azure Container Instances kullanın (ACI)

Azure Container Instances, bir model dağıtıldığında dinamik olarak oluşturulur. Azure Machine Learning sanal ağ içinde ACI oluşturmak üzere etkinleştirmek için, dağıtım tarafından kullanılan alt ağ için __alt ağ temsilcisini__ etkinleştirmeniz gerekir.

> [!WARNING]
> Bir sanal ağda Azure Container Instances kullanırken, sanal ağın Azure Machine Learning çalışma alanınız ile aynı kaynak grubunda olması gerekir.
>
> Sanal ağ içinde Azure Container Instances kullanırken, çalışma alanınızın Azure Container Registry (ACR) Sanal ağda de olamaz.

Çalışma alanınıza bir sanal ağda ACI 'yi kullanmak için aşağıdaki adımları kullanın:

1. Sanal ağınızda alt ağ temsilcisini etkinleştirmek için [alt ağ temsili ekleme veya kaldırma](../virtual-network/manage-subnet-delegation.md) makalesindeki bilgileri kullanın. Bir sanal ağ oluştururken temsilciyi etkinleştirebilir veya var olan bir ağa ekleyebilirsiniz.

    > [!IMPORTANT]
    > Temsilci seçme etkinleştirildiğinde, `Microsoft.ContainerInstance/containerGroups` __hizmet verme için alt ağ__ olarak kullanın.

2. [Aciwebservice. deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none--vnet-name-none--subnet-name-none-)kullanarak modeli dağıtın, `vnet_name` ve `subnet_name` parametrelerini kullanın. Bu parametreleri, temsilciyi etkinleştirdiğiniz sanal ağ adı ve alt ağa ayarlayın.

## <a name="azure-firewall"></a>Azure Güvenlik Duvarı

Azure Güvenlik Duvarı ile Azure Machine Learning kullanma hakkında bilgi için bkz. [Azure Güvenlik duvarının arkasında Azure Machine Learning çalışma alanı kullanma](how-to-access-azureml-behind-firewall.md).

## <a name="azure-container-registry"></a>Azure Container Registry

> [!IMPORTANT]
> Azure Container Registry (ACR) bir sanal ağın içine yerleştirilebilir, ancak aşağıdaki önkoşulları karşılamanız gerekir:
>
> * Azure Machine Learning çalışma alanınız Enterprise Edition olmalıdır. Yükseltme hakkında bilgi için bkz. [Enterprise Edition 'A yükseltme](how-to-manage-workspace.md#upgrade).
> * Azure Machine Learning çalışma alanı bölgeniz [privated bağlantısı etkinleştirilmiş bölge](https://docs.microsoft.com/azure/private-link/private-link-overview#availability)olmalıdır. 
> * Azure Container Registry Premium sürüm olmalıdır. Yükseltme hakkında daha fazla bilgi için bkz. [SKU 'Ları değiştirme](/azure/container-registry/container-registry-skus#changing-skus).
> * Azure Container Registry, eğitim veya çıkarım için kullanılan depolama hesabı ve işlem hedefleri ile aynı sanal ağ ve alt ağ içinde olmalıdır.
> * Azure Machine Learning çalışma alanınızın bir [Azure Machine Learning işlem kümesi](how-to-set-up-training-targets.md#amlcompute)içermesi gerekir.
>
>     ACR bir sanal ağın arkasındaysa, Azure Machine Learning Docker görüntülerini doğrudan oluşturmak için kullanamaz. Bunun yerine, görüntüleri oluşturmak için işlem kümesi kullanılır.

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

## <a name="key-vault-instance"></a>Anahtar Kasası örneği 

Çalışma alanıyla ilişkili anahtar kasası örneği, Azure Machine Learning tarafından aşağıdaki kimlik bilgilerini depolamak için kullanılır:
* İlişkili depolama hesabı bağlantı dizesi
* Azure Container Repository deposu örneklerine parolalar
* Veri depolarına yönelik bağlantı dizeleri

Bir sanal ağın arkasındaki Azure Key Vault Azure Machine Learning deneme yeteneklerini kullanmak için aşağıdaki adımları kullanın:

1. Çalışma alanıyla ilişkili olan anahtar kasasına gidin.

   [![Azure Machine Learning çalışma alanıyla ilişkili olan Anahtar Kasası](./media/how-to-enable-virtual-network/workspace-key-vault.png)](./media/how-to-enable-virtual-network/workspace-key-vault.png#lightbox)

1. __Key Vault__ sayfasında, sol bölmede __güvenlik duvarları ve sanal ağlar__' ı seçin.

   ![Key Vault bölmesindeki "güvenlik duvarları ve sanal ağlar" bölümü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. __Güvenlik duvarları ve sanal ağlar__ sayfasında, aşağıdaki işlemleri yapın:
    - __Erişime Izin ver__' ın altında __Seçili ağlar__' ı seçin.
    - __Sanal ağlar__altında, deneme hesapla'nizin bulunduğu sanal ağı eklemek için __var olan sanal ağları Ekle__ ' yi seçin.
    - __Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver__altında __Evet__' i seçin.

   [![Key Vault bölmesindeki "güvenlik duvarları ve sanal ağlar" bölümü](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png#lightbox)


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

Sanal bir makineyi veya Azure HDInsight kümesini çalışma alanınıza sahip bir sanal ağda kullanmak için aşağıdaki adımları kullanın:

1. Azure portal veya Azure CLı kullanarak bir VM veya HDInsight kümesi oluşturun ve kümeyi bir Azure sanal ağına yerleştirin. Daha fazla bilgi için aşağıdaki makaleleri inceleyin:
    * [Linux VM 'Ler için Azure sanal ağları oluşturma ve yönetme](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Azure sanal ağını kullanarak HDInsight 'ı genişletme](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Azure Machine Learning, VM veya kümedeki SSH bağlantı noktasıyla iletişim kurmasına izin vermek için, ağ güvenlik grubu için bir kaynak girişi yapılandırın. SSH bağlantı noktası genellikle bağlantı noktası 22 ' dir. Bu kaynaktan gelen trafiğe izin vermek için aşağıdaki işlemleri yapın:

    * __Kaynak__ açılan listesinde __hizmet etiketi__' ni seçin.

    * __Kaynak hizmet etiketi__ açılan listesinde __AzureMachineLearning__' yi seçin.

    * __Kaynak bağlantı noktası aralıkları__ açılan listesinde, öğesini seçin __*__ .

    * __Hedef__ açılan listesinde __herhangi birini__seçin.

    * __Hedef bağlantı noktası aralıkları__ aşağı açılan listesinde __22__' yi seçin.

    * __Protokol__altında __herhangi bir__seçin.

    * __Eylem__altında __izin ver__' i seçin.

   ![Bir sanal ağ içindeki bir VM veya HDInsight kümesi üzerinde deneme yapmak için gelen kurallar](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Ağ güvenlik grubu için varsayılan giden kuralları saklayın. Daha fazla bilgi için bkz. [güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules)varsayılan güvenlik kuralları.

    Varsayılan giden kurallarını kullanmak istemiyorsanız ve sanal ağınızın giden erişimini sınırlandırmak istiyorsanız, [sanal ağ ile giden bağlantıyı sınırla](#limiting-outbound-from-vnet) bölümüne bakın.

1. Azure Machine Learning çalışma alanınıza VM veya HDInsight kümesini ekleyin. Daha fazla bilgi için bkz. [model eğitimi için işlem hedeflerini ayarlama](how-to-set-up-training-targets.md).


## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim ortamlarını ayarlama](how-to-set-up-training-targets.md)
* [Özel uç noktaları ayarlama](how-to-configure-private-link.md)
* [Modellerin dağıtılacağı yerler](how-to-deploy-and-where.md)
* [Azure Machine Learning aracılığıyla bir Web hizmetinin güvenliğini sağlamak için TLS kullanma](how-to-secure-web-service.md)
