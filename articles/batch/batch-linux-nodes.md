---
title: Linux'u sanal makine bilgi işlem düğümlerinde çalıştırın - Azure Toplu İşi | Microsoft Dokümanlar
description: Azure Toplu İşlem'deki Linux sanal makinelerin havuzlarında paralel bilgi işlem iş yüklerinizi nasıl işleyebilirsiniz öğrenin.
services: batch
documentationcenter: python
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: na
ms.date: 06/01/2018
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 977504f41e93e37ae2c5ce9bdb1182a1cfe0a3fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252291"
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Toplu havuzlarda Linux işlem düğümlerini sağlama

Hem Linux hem de Windows sanal makinelerinde paralel bilgi işlem iş yüklerini çalıştırmak için Azure Toplu İşlem'i kullanabilirsiniz. Bu makalede, Toplu Python ve [Toplu .NET][py_batch_package] istemci kitaplıklarını kullanarak Toplu İşlem [Batch .NET][api_net] hizmetinde Linux bilgi işlem düğümlerinin havuzlarının nasıl oluşturulacak ayrıntıları.

> [!NOTE]
> Uygulama paketleri 5 Temmuz 2017’den sonra oluşturulmuş tüm Batch havuzlarında desteklenir. Bunların 10 Mart 2016 ve 5 Haziran 2017 arasında oluşturulmuş Batch havuzlarında desteklenebilmesi için, havuzun Bulut Hizmeti yapılandırması kullanılarak oluşturulmuş olması gerekir. 10 Mart 2016’dan önce oluşturulan Batch havuzları uygulama paketlerini desteklemez. Uygulama paketlerini kullanarak uygulamalarınızı Batch düğümlerine dağıtma hakkında daha fazla bilgi için bkz. [Batch uygulama paketleriyle işlem düğümlerine uygulama dağıtımı](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Sanal makine yapılandırması
Toplu İşlem'de bir bilgi işlem düğümleri havuzu oluşturduğunuzda, düğüm boyutunu ve işletim sistemini seçmek için iki seçeneğiniz vardır: Bulut Hizmetleri Yapılandırması ve Sanal Makine Yapılandırması.

**Cloud Services Yapılandırması***yalnızca* Windows işlem düğümleri sağlar. Kullanılabilir işlem düğümü boyutları [Bulut Hizmetleri Için Boyutlar'da](../cloud-services/cloud-services-sizes-specs.md)listelenir ve kullanılabilir işletim sistemleri Azure Konuk [İşlet imgelerinde ve SDK uyumluluk matrisinde](../cloud-services/cloud-services-guestos-update-matrix.md)listelenir. Azure Bulut Hizmetleri düğümlerini içeren bir havuz oluşturduğunuzda, daha önce belirtilen makalelerde açıklanan düğüm boyutunu ve işletim sistemi ailesini belirtirsiniz. Windows işlem düğümleri havuzları için bulut hizmetleri en sık kullanılır.

**Sanal Makine Yapılandırması,** bilgi işlem düğümleri için hem Linux hem de Windows görüntüleri sağlar. Kullanılabilir bilgi işlem düğümü boyutları, [Azure'daki sanal makineler için Boyutlar](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) ve [Azure'daki (Windows) sanal makineler için Boyutlar'da](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) listelenir. Sanal Makine Yapılandırma düğümleri içeren bir havuz oluşturduğunuzda, düğümlerin boyutunu, sanal makine görüntü başvurularını ve düğümlere yüklenecek Toplu düğüm aracısı SKU'yu belirtmeniz gerekir.

### <a name="virtual-machine-image-reference"></a>Sanal makine görüntü başvurusu

Toplu İşlem hizmeti, Sanal Makine Yapılandırmasında bilgi işlem düğümleri sağlamak için [sanal makine ölçek kümelerini](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) kullanır. [Azure][vm_marketplace]Marketi'nden bir resim belirtebilir veya sizin hazırladığınız özel bir resim sağlayabilirsiniz. Özel görüntüler hakkında daha fazla bilgi için [bkz.](batch-sig-images.md)

Sanal makine görüntüsü başvurusu yapılandırdığınızda, sanal makine görüntüsünün özelliklerini belirtirsiniz. Sanal makine görüntüsü başvurusu oluşturduğunuzda aşağıdaki özellikler gereklidir:

| **Görüntü referans özellikleri** | **Örnek** |
| --- | --- |
| Yayımcı |Canonical |
| Sunduğu |UbuntuServer |
| SKU |18.04-LTS |
| Sürüm |en son |

> [!TIP]
> Bu özellikler ve Gezinme'deki Market görüntülerini nasıl listeleyebileceğiniz [ve CLI veya PowerShell ile Azure'daki Linux sanal makine resimlerini nasıl seçebileceğiniz](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)hakkında daha fazla bilgi edinebilirsiniz. Tüm Market görüntülerinin şu anda Toplu İşlem ile uyumlu olmadığını unutmayın. Daha fazla bilgi için [Düğüm aracısı SKU'ya](#node-agent-sku)bakın.
>
>

### <a name="node-agent-sku"></a>Düğüm ajanı SKU
Toplu Iş bölümü aracısı, havuzdaki her düğümde çalışan ve düğüm ile Toplu İşlem hizmeti arasındaki komut ve denetim arabirimi sağlayan bir programdır. Farklı işletim sistemleri için SNU olarak bilinen düğüm aracısının farklı uygulamaları vardır. Esasen, Sanal Makine Yapılandırması oluşturduğunuzda, önce sanal makine görüntüsü başvurularını belirtirsiniz, sonra da görüntüye yüklenmesi gereken düğüm aracısını belirtirsiniz. Genellikle, her düğüm aracısı SKU birden çok sanal makine görüntüleri ile uyumludur. Düğüm aracısı SUS'a birkaç örnek aşağıda verilmiştir:

* batch.node.ubuntu 18.04
* batch.node.centos 7
* batch.node.windows amd64

> [!IMPORTANT]
> Market'te bulunan tüm sanal makine görüntüleri şu anda kullanılabilir Toplu iş düğümü aracılarıyla uyumlu değildir. Kullanılabilir düğüm aracısı SK'leri ve uyumlu oldukları sanal makine görüntülerini listelemek için Toplu SDK'ları kullanın. Daha fazla bilgi ve çalışma zamanında geçerli görüntülerin listesini nasıl alınız örnekleri için bu makalede daha sonra [Sanal Makine görüntüleri](#list-of-virtual-machine-images) listesine bakın.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Linux havuzu oluşturma: Toplu Python
Aşağıdaki kod snippet Ubuntu Server bilgi işlem düğümleri havuzu oluşturmak [için Python için Microsoft Azure Toplu İstemci Kitaplığı][py_batch_package] nasıl kullanılacağına bir örnek gösterir. Toplu Python modülü için başvuru belgeleri, Dokümanları Oku'da [azure.batch paketinde][py_batch_docs] bulunabilir.

Bu parçacık bir [ImageReference][py_imagereference] açıkça oluşturur ve özelliklerinin her birini (yayımcı, teklif, SKU, sürüm) belirtir. Ancak üretim kodunda, çalışma zamanında kullanılabilir görüntü ve düğüm aracısı SKU birleşimlerini belirlemek ve seçmek için [list_supported_images][py_list_supported_images] yöntemini kullanmanızı öneririz.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_D2_V3"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, batch_url)
client = batch.BatchServiceClient(creds, batch_url)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id=pool_id, vm_size=vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher="Canonical",
    offer="UbuntuServer",
    sku="18.04-LTS",
    version="latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference=ir,
    node_agent_sku_id="batch.node.ubuntu 18.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Daha önce de belirtildiği gibi, [ImageReference'ı][py_imagereference] açıkça oluşturmak yerine, şu anda desteklenen düğüm aracısı/Market görüntü kombinasyonlarından dinamik olarak seçim yapmak için [list_supported_images][py_list_supported_images] yöntemini kullanmanızı öneririz. Aşağıdaki Python snippet bu yöntemin nasıl kullanılacağını gösterir.

```python
# Get the list of supported images from the Batch service
images = client.account.list_supported_images()

# Obtain the desired image reference
image = None
for img in images:
  if (img.image_reference.publisher.lower() == "canonical" and
        img.image_reference.offer.lower() == "ubuntuserver" and
        img.image_reference.sku.lower() == "18.04-lts"):
    image = img
    break

if image is None:
  raise RuntimeError('invalid image reference for desired configuration')

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference=image.image_reference,
    node_agent_sku_id=image.node_agent_sku_id)
```

## <a name="create-a-linux-pool-batch-net"></a>Linux havuzu oluşturma: Toplu .NET
Aşağıdaki kod snippet Ubuntu Server bilgi işlem düğümleri havuzu oluşturmak için [Toplu Iş .NET][nuget_batch_net] istemci kitaplığı nasıl kullanılacağına bir örnek gösterir. Toplu Iş [.NET başvuru belgelerini][api_net] docs.microsoft.com bulabilirsiniz.

Aşağıdaki kod snippet [PoolOperations][net_pool_ops]kullanır. [ListSupportedImages][net_list_supported_images] yöntemi şu anda desteklenen Marketplace görüntü ve düğüm aracısı SKU kombinasyonları listesinden seçmek için. Desteklenen kombinasyonların listesi zaman zaman değişebileceğinden bu teknik istenir. En yaygın olarak, desteklenen kombinasyonlar eklenir.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_D2_V3";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<ImageInformation> images =
    batchClient.PoolOperations.ListSupportedImages().ToList();

// Find the appropriate image information
ImageInformation image = null;
foreach (var img in images)
{
    if (img.ImageReference.Publisher == "Canonical" &&
        img.ImageReference.Offer == "UbuntuServer" &&
        img.ImageReference.Sku == "18.04-LTS")
    {
        image = img;
        break;
    }
}

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(image.ImageReference, image.NodeAgentSkuId);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit the pool to the Batch service
await pool.CommitAsync();
```

Önceki parçacık [PoolOperations][net_pool_ops]kullanır rağmen. [ListSupportedImages][net_list_supported_images] yöntemi dinamik olarak listelemek ve desteklenen görüntü ve düğüm aracısı SKU kombinasyonları (önerilen) seçin, ayrıca açıkça bir [ImageReference][net_imagereference] yapılandırabilirsiniz:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "18.04-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Sanal makine görüntüleri listesi
Toplu Işlem hizmeti ve bunlara karşılık gelen düğüm aracıları için desteklenen tüm Market sanal makine görüntülerinin listesini almak için lütfen seçtiğiniz ilgili dil SDK'daki [list_supported_images][py_list_supported_images] (Python), [ListSupportedImages][net_list_supported_images] (Batch .NET) veya ilgili API'den yararlanın.

## <a name="connect-to-linux-nodes-using-ssh"></a>SSH kullanarak Linux düğümlerine bağlanma
Geliştirme sırasında veya sorun giderme sırasında, havuzunuzdaki düğümlerde oturum açmanız gerekebilir. Windows işlem düğümlerinin aksine, Linux düğümlerine bağlanmak için Uzak Masaüstü Protokolü'nü (RDP) kullanamazsınız. Bunun yerine, Toplu İşlem hizmeti uzak bağlantı için her düğümde SSH erişimi sağlar.

Aşağıdaki Python kod snippet uzak bağlantı için gerekli olan bir havuzda her düğüm üzerinde bir kullanıcı oluşturur. Daha sonra her düğüm için güvenli kabuk (SSH) bağlantı bilgilerini yazdırır.

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
    credentials,
    base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Burada dört Linux düğümü içeren bir havuz için önceki kod için örnek çıktı:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Parola yerine, düğüm üzerinde bir kullanıcı oluşturduğunuzda bir SSH ortak anahtarı belirtebilirsiniz. Python SDK'da [ComputeNodeUser'daki][py_computenodeuser] **ssh_public_key** parametresini kullanın. .NET'te [ComputeNodeUser'ı][net_computenodeuser]kullanın. [SshPublicKey][net_ssh_key] özelliği.

## <a name="pricing"></a>Fiyatlandırma
Azure Toplu İşlem, Azure Bulut Hizmetleri ve Azure Sanal Makineler teknolojisi üzerine kuruludur. Toplu İşlem hizmetinin kendisi hiçbir ücret ödemeden sunulur, bu da yalnızca Toplu İşlem çözümlerinizin tükettiği işlem kaynakları (ve bunlar gerektiren ilişkili maliyetler) için ücretlendirildiğiniz anlamına gelir. **Bulut Hizmetleri Yapılandırması'nı**seçtiğinizde, [Bulut Hizmetleri fiyatlandırma][cloud_services_pricing] yapısına göre ücretlendirilirsiniz. **Sanal Makine Yapılandırması'nı**seçtiğinizde, [Sanal Makineler fiyatlandırma][vm_pricing] yapısına göre ücretlendirilirsiniz.

[Uygulamaları, uygulama paketlerini](batch-application-packages.md)kullanarak Toplu Iş düğümlerinize dağılarsanız, uygulama paketlerinizin tükettiği Azure Depolama kaynakları için de ücretlendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

GitHub'daki [azure toplu iş örnekleri][github_samples] deposundaki Python kod [örnekleri,][github_samples_py] havuz, iş ve görev oluşturma gibi ortak Toplu İşlemişlemlerini nasıl gerçekleştireceğimizi gösteren komut dosyaları içerir. Python örneklerine eşlik eden [README'da][github_py_readme] gerekli paketlerin nasıl yüklenirken nasıl yüklenenekadar bilgi olduğu hakkında ayrıntılar vardır.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: /dotnet/api/microsoft.azure.batch.computenodeuser?view=azure-dotnet
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_supported_images]: https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.pooloperations.listsupportedimages
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: /dotnet/api/microsoft.azure.batch.computenodeuser.sshpublickey?view=azure-dotnet#Microsoft_Azure_Batch_ComputeNodeUser_SshPublicKey
[nuget_batch_net]: https://www.nuget.org/packages/Microsoft.Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: https://azure.github.io/azure-sdk-for-python/ref/Batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: /python/api/azure-batch/azure.batch.models.computenodeuser
[py_imagereference]: /python/api/azure-mgmt-batch/azure.mgmt.batch.models.imagereference
[py_list_supported_images]: https://docs.microsoft.com/python/api/azure-batch/azure.batch.operations.AccountOperations?view=azure-python
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
