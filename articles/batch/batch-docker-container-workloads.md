---
title: Kapsayıcı iş yükleri - Azure Toplu İş
description: Azure Toplu İş'teki kapsayıcı resimlerden uygulamaları nasıl çalıştırıp ölçeklendireceklerini öğrenin. Çalışan kapsayıcı görevlerini destekleyen bir işlem düğümü havuzu oluşturun.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.workload: na
ms.date: 03/02/2020
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: 81f4e753ffbaaefd5761c9396a6533bac9f212c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254833"
---
# <a name="run-container-applications-on-azure-batch"></a>Azure Toplu İş'te kapsayıcı uygulamalarını çalıştırma

Azure Toplu İş, Azure'da çok sayıda toplu bilgi işlem işini çalıştırmanızı ve ölçeklendirmenize olanak tanır. Toplu iş görevleri toplu iş havuzundaki sanal makinelerde (düğümler) doğrudan çalıştırılabilir, ancak düğümlerde Docker uyumlu kaplarda görevleri çalıştırmak için toplu iş havuzu da ayarlayabilirsiniz. Bu makalede, kapsayıcı görevleri çalıştıran bir işlem düğümleri havuzu oluşturmak ve sonra havuzda kapsayıcı görevleri çalıştırmak nasıl gösterilmektedir.

Kapsayıcı kavramları ve toplu iş havuzu ve iş oluşturma konusunda bilginiz olmalıdır. Kod örnekleri toplu iş .NET ve Python SDK'larını kullanır. Ayrıca, kapsayıcı özellikli Toplu Toplu Iş havuzları oluşturmak ve kapsayıcı görevlerini çalıştırmak için Azure portalı da dahil olmak üzere diğer Toplu SDK'ları ve araçları da kullanabilirsiniz.

## <a name="why-use-containers"></a>Neden konteyner kullanıyorsun?

Kapsayıcıları kullanmak, bir ortamı ve uygulamaları çalıştırmak için bağımlılıkları yönetmek zorunda kalmadan Toplu iş görevlerini çalıştırmanın kolay bir yolunu sağlar. Kapsayıcılar uygulamaları, birkaç farklı ortamda çalıştırılabilen hafif, taşınabilir, kendi kendine yeten birimler olarak dağır. Örneğin, bir kapsayıcıyı yerel olarak oluşturun ve sınayın, ardından kapsayıcı görüntüsünü Azure veya başka bir yerdeki bir kayıt defterine yükleyin. Kapsayıcı dağıtım modeli, uygulamanızın çalışma zamanı ortamının her zaman doğru şekilde yüklenmesini ve uygulamayı barındırdığınız her yerde yapılandırıldığından emin olur. Toplu İşlem'deki kapsayıcı tabanlı görevler, uygulama paketleri ve kaynak dosyalarının ve çıktı dosyalarının yönetimi de dahil olmak üzere kapsayıcı olmayan görevlerin özelliklerinden de yararlanabilir.

## <a name="prerequisites"></a>Ön koşullar

* **SDK sürümleri**: Toplu SDK'lar aşağıdaki sürümlerden itibaren kapsayıcı görüntülerini destekler:
    * Toplu REST API sürümü 2017-09-01.6.0
    * Toplu .NET SDK sürüm 8.0.0
    * Toplu Python SDK sürüm 4.0
    * Toplu Java SDK sürüm 3.0
    * Toplu Düğüm.js SDK sürüm 3.0

* **Hesaplar**: Azure aboneliğinizde bir Toplu İş hesabı ve isteğe bağlı olarak bir Azure Depolama hesabı oluşturmanız gerekir.

* **Desteklenen VM görüntüsü**: Kapsayıcılar yalnızca Sanal Makine Yapılandırması ile oluşturulan havuzlarda aşağıdaki bölümde ayrıntılı olarak "Desteklenen sanal makine görüntüleri" bölümünde ayrıntılı olarak açıklanan görüntülerden desteklenir. Özel bir görüntü sağlıyorsanız, aşağıdaki bölümdeki hususlara ve [sanal makineler havuzu oluşturmak için yönetilen özel bir görüntü kullanma gereksinimlerine](batch-custom-images.md)bakın.

### <a name="limitations"></a>Sınırlamalar

* Toplu işlem yalnızca Linux havuzlarında çalışan kaplar için RDMA desteği sağlar

* Windows kapsayıcı iş yükleri için, havuzunuz için çok çekirdekli bir VM boyutu seçmeniz önerilir

## <a name="supported-virtual-machine-images"></a>Desteklenen sanal makine görüntüleri

Kapsayıcı iş yükleri için VM işlem düğümleri havuzu oluşturmak için aşağıdaki desteklenen Windows veya Linux görüntülerinden birini kullanın. Toplu İşlem ile uyumlu Market görüntüleri hakkında daha fazla bilgi için [sanal makine görüntülerinin listesine](batch-linux-nodes.md#list-of-virtual-machine-images)bakın.

### <a name="windows-support"></a>Windows desteği

Toplu işlem, kapsayıcı destek tanımlamaları olan Windows sunucu görüntülerini destekler. Genellikle bu görüntü sku adları ile `-with-containers` `-with-containers-smalldisk`suffixed veya . Ayrıca, [Toplu İşlem'de desteklenen tüm görüntüleri listeleyecek API,](batch-linux-nodes.md#list-of-virtual-machine-images) görüntü Docker kapsayıcılarını destekliyorsa bir `DockerCompatible` yeteneği gösterir.

Ayrıca, Windows'ta Docker çalıştıran VM'lerden özel görüntüler de oluşturabilirsiniz.

### <a name="linux-support"></a>Linux desteği

Linux kapsayıcı iş yükleri için Batch, microsoft Azure Toplu İş Yeri tarafından Azure Marketi'nde yayınlanan aşağıdaki Linux görüntülerini özel bir görüntüye gerek kalmadan destekler.

#### <a name="vm-sizes-without-rdma"></a>RDMA olmadan VM boyutları

- Yayımcı:`microsoft-azure-batch`
  - Teklif:`centos-container`
  - Teklif:`ubuntu-server-container`

#### <a name="vm-sizes-with-rdma"></a>RDMA ile VM boyutları

- Yayımcı:`microsoft-azure-batch`
  - Teklif:`centos-container-rdma`
  - Teklif:`ubuntu-server-container-rdma`

Bu görüntüler yalnızca Azure Toplu Toplu Havuzlarda kullanılmak üzere desteklenir ve Docker konteyner yürütmesi için kullanılır. Bunlar:

* Önceden yüklenmiş Docker uyumlu [Moby](https://github.com/moby/moby) konteyner çalışma süresi

* Azure N serisi VM'lerde dağıtımı kolaylaştırmak için önceden yüklenmiş NVIDIA GPU sürücüleri ve NVIDIA kapsayıcı çalışma zamanı

* Sonekli görüntüler için Infiniband RDMA VM boyutları desteği ile önceden yüklenmiş/önceden `-rdma`yapılandırılmış görüntü. Şu anda bu görüntüler SR-IOV IB/RDMA VM boyutlarını desteklememektedir.

Ayrıca, Toplu İşlem ile uyumlu Linux dağıtımlarından birinde Docker çalıştıran VM'lerden özel görüntüler oluşturabilirsiniz. Kendi özel Linux resminizi sağlamayı seçerseniz, [sanal makineler havuzu oluşturmak için yönetilen özel bir görüntü kullanma yönergelerine](batch-custom-images.md)bakın.

Özel bir görüntüde Docker desteği için [Docker Community Edition (CE)](https://www.docker.com/community-edition) veya [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition)yükleyin.

Özel bir Linux görüntüsü kullanmak için ek hususlar:

* Özel bir görüntü kullanırken Azure N serisi boyutlarının GPU performansından yararlanmak için NVIDIA sürücülerini önceden yükleyin. Ayrıca, NVIDIA GPU'lar, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker)için Docker Motor Utility yüklemeniz gerekir.

* Azure RDMA ağına erişmek için RDMA özellikli bir VM boyutu kullanın. Gerekli RDMA sürücüleri, Batch tarafından desteklenen CentOS HPC ve Ubuntu görüntülerine yüklenir. MPI iş yüklerini çalıştırmak için ek yapılandırma gerekebilir. Bkz. [Toplu İşlem havuzunda RDMA özellikli veya GPU özellikli örnekleri kullanın.](batch-pool-compute-intensive-sizes.md)


## <a name="container-configuration-for-batch-pool"></a>Toplu havuz için kapsayıcı yapılandırması

Kapsayıcı iş yüklerini çalıştırmak için Toplu İşlem havuzunu etkinleştirmek için havuzun [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) nesnesinde [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) ayarlarını belirtmeniz gerekir. (Bu makalede, Toplu .NET API başvurusuna bağlantılar sağlar. İlgili ayarlar [Toplu Python](/python/api/overview/azure/batch) API'sinde yer alır.)

Aşağıdaki örneklerde gösterildiği gibi, önceden getirilen kapsayıcı görüntüleri olan veya olmayan kapsayıcı özellikli bir havuz oluşturabilirsiniz. Çekme (veya ön alma) işlemi, Konteyner görüntülerini Docker Hub'dan veya Internet'teki başka bir konteyner kayıt defterinden önceden yüklemenize olanak tanır. En iyi performans için, Toplu Iş hesabıyla aynı bölgede bir [Azure kapsayıcı kayıt defteri](../container-registry/container-registry-intro.md) kullanın.

Kapsayıcı görüntülerini önceden getirmenin avantajı, görevler ilk çalışmaya başladığında kapsayıcı görüntüsünün indirilmesi için beklemek zorunda kalmamalarıdır. Kapsayıcı yapılandırması, havuz oluşturulduğunda kapsayıcı görüntülerini VM'lere çeker. Havuzda çalışan görevler daha sonra kapsayıcı görüntüleri ve kapsayıcı çalıştırma seçenekleri listesine başvurur.


### <a name="pool-without-prefetched-container-images"></a>Önceden getirilmiş konteyner görüntüleri olmayan havuz

Kaplanmış bir havuzu önceden getirilen kapsayıcı görüntüleri olmadan `ContainerConfiguration` `VirtualMachineConfiguration` yapılandırmak için, aşağıdaki Python örneğinde gösterildiği gibi tanımlayın ve nesneleri tanımlayın. Bu örnekte, Market'teki Azure Toplu Toplu Toplu kutu havuzları için Ubuntu Server görüntüsü kullanmaktadır.


```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Konteyner yapılandırması için prefetch görüntüleri

Havuzdaki kapsayıcı görüntülerini önceden almak için,`container_image_names` `ContainerConfiguration`kapsayıcı görüntülerinin listesini (Python'da) .

Aşağıdaki temel Python [örneği, Docker Hub'dan](https://hub.docker.com)standart bir Ubuntu kapsayıcı görüntüsünü niçin önceden alsüreceğini gösterir.

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub.
"""

container_conf = batch.models.ContainerConfiguration(
    container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


Aşağıdaki C# örneği, [Docker Hub'dan](https://hub.docker.com)bir TensorFlow görüntüsünü önceden almak istediğinizi varsayar. Bu örnek, havuz düğümlerinde VM ana bilgisayarda çalışan bir başlangıç görevi içerir. Örneğin, kapsayıcılardan erişilebilen bir dosya sunucusu takmak için ana bilgisayarda bir başlangıç görevi çalıştırabilirsiniz.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Özel bir konteyner kayıt defterinden ön alma görüntüleri

Ayrıca, özel bir kapsayıcı kayıt defteri sunucusuna kimlik doğrulaması yaparak kapsayıcı görüntülerini önceden getirebilirsiniz. Aşağıdaki örnekte, `ContainerConfiguration` nesneler `VirtualMachineConfiguration` özel bir Azure kapsayıcı kayıt defterinden özel bir TensorFlow görüntüsünü önceden getirir. Resim başvurusu önceki örnektekiyle aynıdır.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```

## <a name="container-settings-for-the-task"></a>Görev için kapsayıcı ayarları

Kapsayıcı özellikli bir havuzda kapsayıcı görevi çalıştırmak için kapsayıcıya özgü ayarları belirtin. Ayarlar, kullanılacak görüntüyü, kayıt defterini ve kapsayıcı çalıştırma seçeneklerini içerir.

* Kapsayıcıya `ContainerSettings` özgü ayarları yapılandırmak için görev sınıflarının özelliğini kullanın. Bu ayarlar [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings) sınıfı tarafından tanımlanır. Toplu işlemle halledilmesi nedeniyle kapsayıcı seçeneğinin `--rm` ek `--runtime` bir seçenek gerektirmediğini unutmayın.

* Kapsayıcı görüntülerinde görevleri çalıştırArsanız, [bulut görevi](/dotnet/api/microsoft.azure.batch.cloudtask) ve [iş yöneticisi görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) kapsayıcı ayarları gerektirir. Ancak, [başlangıç görevi,](/dotnet/api/microsoft.azure.batch.starttask) [iş hazırlama görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask)ve iş sürümü [görevi](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) kapsayıcı ayarları gerektirmez (diğer bir şekilde, bir kapsayıcı bağlamında veya doğrudan düğüm üzerinde çalıştırılabilir).

### <a name="container-task-command-line"></a>Kapsayıcı görev komut satırı

Bir kapsayıcı görevi çalıştırdığınızda, Toplu iş, görevde belirtilen görüntüyü kullanarak bir kapsayıcı oluşturmak için [docker oluşturma](https://docs.docker.com/engine/reference/commandline/create/) komutunu otomatik olarak kullanır. Toplu işlem daha sonra görev yürütmeyi kapsayıcıda denetler.

Kapsayıcı olmayan Toplu İşlem görevleriyle olduğu gibi, kapsayıcı görevi için bir komut satırı ayarlarsınız. Toplu İşlem kapsayıcıyı otomatik olarak oluşturduğundan, komut satırı yalnızca kapsayıcıda çalışacak komutveya komutları belirtir.

Toplu iş görevinin kapsayıcı görüntüsü bir [ENTRYPOINT](https://docs.docker.com/engine/reference/builder/#exec-form-entrypoint-example) komut dosyasıyla yapılandırılırsa, komut satırınızı varsayılan ENTRYPOINT'i kullanacak veya geçersiz kılacak şekilde ayarlayabilirsiniz:

* Kapsayıcı görüntüsünün varsayılan ENTRYPOINT'ini kullanmak için görev komut `""`satırını boş dize olarak ayarlayın.

* Varsayılan ENTRYPOINT'i geçersiz kılmak için veya görüntüde ENTRYPOINT yoksa, örneğin kapsayıcıya uygun bir `/app/myapp` `/bin/sh -c python myscript.py`komut satırı ayarlayın veya .

İsteğe bağlı [ContainerRunOptions,](/dotnet/api/microsoft.azure.batch.taskcontainersettings.containerrunoptions) Toplu `docker create` İşlem'in kapsayıcıyı oluşturmak ve çalıştırmak için kullandığı komuta sağladığınız ek bağımsız değişkenlerdir. Örneğin, kapsayıcı için çalışma dizini ayarlamak için `--workdir <directory>` seçeneği ayarlayın. Ek seçenekler için [docker'a](https://docs.docker.com/engine/reference/commandline/create/) başvuru oluşturma bilgisini görün.

### <a name="container-task-working-directory"></a>Konteyner görev çalışma dizini

Toplu İşlem kapsayıcısı görevi, Toplu İşlem'in düzenli (kapsayıcı olmayan) bir görev için ayarladığını dizine çok benzeyen kapsayıcıdaki çalışma dizininde yürütür. Görüntüde veya varsayılan kapsayıcı çalışma dizininde (Windows`C:\` kapsayıcısında veya `/` Linux kapsayıcısında) yapılandırıldıysa, bu çalışma dizininin [WORKDIR'dan](https://docs.docker.com/engine/reference/builder/#workdir) farklı olduğunu unutmayın.

Toplu işlem kapsayıcı görevi için:

* Ana bilgisayar düğümünün (Azure Toplu `AZ_BATCH_NODE_ROOT_DIR` Iş dizilişlerinin kökü) altındaki tüm dizinler kapsayıcıya eşlenir
* Tüm görev ortamı değişkenleri kapsayıcıya eşlenir
* Düğümdeki görev `AZ_BATCH_TASK_WORKING_DIR` çalışma dizini normal bir görevle aynı şekilde ayarlanır ve kapsayıcıya eşlenir.

Bu eşlemeler, kapsayıcı olmayan görevlerle aynı şekilde kapsayıcı görevleriyle çalışmanızı sağlar. Örneğin, uygulama paketlerini kullanarak uygulamaları yükleyin, Azure Depolama'dan kaynak dosyalarına erişin, görev ortamı ayarlarını kullanın ve kapsayıcı durduktan sonra görev çıktıdosyalarını devam ettiren.

### <a name="troubleshoot-container-tasks"></a>Sorun giderme kapsayıcı görevleri

Kapsayıcı göreviniz beklendiği gibi çalışmıyorsa, kapsayıcı görüntüsünün WORKDIR veya ENTRYPOINT yapılandırması hakkında bilgi almanız gerekebilir. Yapılandırmayı görmek için [docker görüntü denetim](https://docs.docker.com/engine/reference/commandline/image_inspect/) komutunu çalıştırın.

Gerekirse, kapsayıcı görevinin ayarlarını görüntüye göre ayarlayın:

* Görev komut satırında mutlak bir yol belirtin. Görev komut satırı için görüntünün varsayılan ENTRYPOINT'i kullanılıyorsa, mutlak bir yolun ayarlı olduğundan emin olun.

* Görevin kapsayıcı çalıştırma seçeneklerinde, çalışma dizini görüntüdeki WORKDIR ile eşleşecek şekilde değiştirin. Örneğin, ayarlayın. `--workdir /app`

## <a name="container-task-examples"></a>Kapsayıcı görev örnekleri

Aşağıdaki Python parçacığı, Docker Hub'dan çekilen hayali bir görüntüden oluşturulan bir kapsayıcıda çalışan temel bir komut satırı gösterir. Burada `--rm` kapsayıcı seçeneği görev tamamlandıktan sonra kapsayıcıyı kaldırır `--workdir` ve seçenek çalışma dizini ayarlar. Komut satırı, ana bilgisayardaki görev çalışma dizinine küçük bir dosya yazan basit bir kabuk komutuyla kapsayıcı ENTRYPOINT'i geçersiz kılar.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='myimage',
    container_run_options='--rm --workdir /')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='/bin/sh -c \"echo \'hello world\' > $AZ_BATCH_TASK_WORKING_DIR/output.txt\"',
    container_settings=task_container_settings
)
```

Aşağıdaki C# örneği, bulut görevi için temel kapsayıcı ayarlarını gösterir:

```csharp
// Simple container task command

string cmdLine = "c:\\app\\myApp.exe";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "myimage",
    containerRunOptions: "--rm --workdir c:\\app"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine);
```


## <a name="next-steps"></a>Sonraki adımlar

* Ayrıca, Tersane tarifleri aracılığıyla Azure Toplu'da konteyner iş yüklerinin kolay dağıtımı için [Toplu Tersane](https://github.com/Azure/batch-shipyard) araç [setine](https://github.com/Azure/batch-shipyard/tree/master/recipes)bakın.

* Docker CE'nin Linux'ta yüklenmesi ve kullanılması hakkında daha fazla bilgi için [Docker](https://docs.docker.com/engine/installation/) belgelerine bakın.

* Özel görüntüleri kullanma hakkında daha fazla bilgi [için](batch-custom-images.md)bkz.

* Konteyner tabanlı sistemler oluşturmak için bir çerçeve olan [Moby projesi](https://mobyproject.org/)hakkında daha fazla bilgi edinin.
