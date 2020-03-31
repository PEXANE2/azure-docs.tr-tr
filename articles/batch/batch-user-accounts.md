---
title: Görevleri kullanıcı hesapları altında çalıştırma - Azure Toplu İş
description: Bir görevin çalışmasını istediğiniz kullanıcı hesabını yapılandırabilmek yararlıdır. Kullanıcı hesaplarının türlerini ve bunları nasıl yapılandırışlanızı öğrenin.
services: batch
author: LauraBrenner
manager: evansma
editor: ''
tags: ''
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 11/18/2019
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: fee3dc764d2052185160a4ba6b3d70854c54eeac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252278"
---
> [!NOTE] 
> Bu makalede tartışılan kullanıcı hesapları, güvenlik nedenleriyle Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) için kullanılan kullanıcı hesaplarından farklıdır. 
>
> SSH üzerinden Linux sanal makine yapılandırmasını çalıştıran bir düğüme bağlanmak [için, Azure'da Bir Linux VM'ye Uzak Masaüstü'nü Kullan'a](../virtual-machines/virtual-machines-linux-use-remote-desktop.md)bakın. RDP ile Windows çalıştıran düğümlere bağlanmak [için](../virtual-machines/windows/connect-logon.md)bkz.<br /><br />
> RDP üzerinden bulut hizmeti yapılandırmasını çalıştıran bir düğüme bağlanmak için Azure [Bulut Hizmetlerinde Rol Için Uzak Masaüstü Bağlantısını Etkinleştir](../cloud-services/cloud-services-role-enable-remote-desktop-new-portal.md)bölümüne bakın.
>
>


# <a name="run-tasks-under-user-accounts-in-batch"></a>Toplu İşlem'de kullanıcı hesapları altında görevleri çalıştırma

Azure Toplu İşlem'deki bir görev her zaman bir kullanıcı hesabı altında çalışır. Varsayılan olarak, görevler yönetici izinleri olmadan standart kullanıcı hesapları altında çalışır. Bu varsayılan kullanıcı hesabı ayarları genellikle yeterlidir. Ancak, belirli senaryolar için, bir görevin çalışmasını istediğiniz kullanıcı hesabını yapılandırabilmek yararlıdır. Bu makalede, kullanıcı hesaplarının türleri ve bunları senaryonuz için nasıl yapılandırabileceğiniz açıklanmıştır.

## <a name="types-of-user-accounts"></a>Kullanıcı hesabı türleri

Azure Toplu İş, görevleri çalıştırmak için iki tür kullanıcı hesabı sağlar:

- **Otomatik kullanıcı hesapları.** Otomatik kullanıcı hesapları, Toplu İşlem hizmeti tarafından otomatik olarak oluşturulan yerleşik kullanıcı hesaplarıdır. Varsayılan olarak, görevler otomatik kullanıcı hesabı altında çalışır. Bir görevin otomatik kullanıcı belirtimini, görevin hangi otomatik kullanıcı hesabının altında çalışması gerektiğini belirtmek için yapılandırabilirsiniz. Otomatik kullanıcı belirtimi, görevi çalıştıracak otomatik kullanıcı hesabının yükseklik düzeyini ve kapsamını belirtmenize olanak tanır. 

- **Adlandırılmış bir kullanıcı hesabı.** Havuzu oluştururken bir havuz için bir veya daha fazla adlandırılmış kullanıcı hesabı belirtebilirsiniz. Her kullanıcı hesabı havuzun her düğümünde oluşturulur. Hesap adına ek olarak, kullanıcı hesabı parolasını, yükseklik düzeyini ve Linux havuzları için SSH özel anahtarını belirtirsiniz. Bir görev eklediğinizde, bu görevin çalıştırılması gereken adlandırılmış kullanıcı hesabını belirtebilirsiniz.

> [!IMPORTANT] 
> Toplu Servis sürümü 2017-01-01.4.0, kodunuzu bu sürümü aramak için güncelleştirmenizi gerektiren bir kesme değişikliği sunar. Toplu Iş'in eski bir sürümünden kod geçirtiyorsanız, **runElevated** özelliğinin artık REST API veya Toplu istemci kitaplıklarında desteklenmediğini unutmayın. Yükseklik düzeyini belirtmek için görevin yeni **kullanıcı Kimliği** özelliğini kullanın. İstemci kitaplıklarından birini kullanıyorsanız Toplu İş kodunuzu güncelleştirmek için hızlı yönergeler için [kodunuzu en son Toplu İstemci kitaplığına güncelleştir](#update-your-code-to-the-latest-batch-client-library) başlıklı bölüme bakın.
>
>

## <a name="user-account-access-to-files-and-directories"></a>Dosyalara ve dizinlere kullanıcı hesabı erişimi

Hem otomatik kullanıcı hesabı hem de adlandırılmış kullanıcı hesabı, görevin çalışma dizinine, paylaşılan dizinine ve çok örnekli görevler dizinine okuma/yazma erişimine sahiptir. Her iki hesap türü de başlangıç ve iş hazırlama dizinlerine okuma erişimine sahiptir.

Bir görev, başlangıç görevini çalıştırmak için kullanılan aynı hesabın altında çalışırsa, görev başlangıç görev dizinine okuma-yazma erişimine sahiptir. Benzer şekilde, bir görev bir iş hazırlama görevi çalıştırmak için kullanılan aynı hesap altında çalışırsa, görev iş hazırlama görev dizinine okuma yazma erişimi vardır. Bir görev başlangıç görevi veya iş hazırlama görevinden farklı bir hesap altında çalışıyorsa, görev yalnızca ilgili dizine erişimi okumuştur.

Bir görevden dosyalara ve dizinlere erişim hakkında daha fazla bilgi için [bkz.](batch-api-basics.md#files-and-directories)

## <a name="elevated-access-for-tasks"></a>Görevler için yüksek erişim 

Kullanıcı hesabının yükseklik düzeyi, bir görevin yüksek erişimle çalışıp çalışılmadığını gösterir. Hem otomatik kullanıcı hesabı hem de adlandırılmış kullanıcı hesabı yüksek erişimle çalıştırılabilir. Yükseklik düzeyi için iki seçenek şunlardır:

- **NonAdmin:** Görev, yüksek erişim olmadan standart bir kullanıcı olarak çalışır. Toplu kullanıcı hesabı için varsayılan yükseklik düzeyi her zaman **NonAdmin'dir.**
- **Yönetici:** Görev, yüksek erişime sahip bir kullanıcı olarak çalışır ve tam Yönetici izinleriyle çalışır. 

## <a name="auto-user-accounts"></a>Otomatik kullanıcı hesapları

Varsayılan olarak, görevler toplu iş hesabında otomatik kullanıcı hesabı altında, yüksek erişim olmadan ve görev kapsamı yla standart bir kullanıcı olarak çalıştırılır. Otomatik kullanıcı belirtimi görev kapsamı için yapılandırıldığında, Toplu İşlem hizmeti yalnızca bu görev için bir otomatik kullanıcı hesabı oluşturur.

Görev kapsamının alternatifi havuz kapsamıdır. Bir görevin otomatik kullanıcı belirtimi havuz kapsamı için yapılandırıldığinde, görev havuzdaki herhangi bir göreviçin kullanılabilen bir otomatik kullanıcı hesabı altında çalışır. Havuz kapsamı hakkında daha fazla bilgi için, havuz kapsamı olan otomatik kullanıcı olarak görev çalıştır başlıklı bölüme bakın.   

Varsayılan kapsam Windows ve Linux düğümlerinde farklıdır:

- Windows düğümlerinde görevler varsayılan olarak görev kapsamı altında çalıştırılır.
- Linux düğümleri her zaman havuz kapsamı altında çalışır.

Otomatik kullanıcı belirtimi için her biri benzersiz bir otomatik kullanıcı hesabına karşılık gelen dört olası yapılandırma vardır:

- Görev kapsamı yla yönetici olmayan erişim (varsayılan otomatik kullanıcı belirtimi)
- Görev kapsamı ile yönetici (yükseltilmiş) erişim
- Havuz kapsamı ile yönetici olmayan erişim
- Havuz kapsamı ile yönetici erişimi

> [!IMPORTANT] 
> Görev kapsamı altında çalışan görevler, düğümdeki diğer görevlere fiilen erişmez. Ancak, hesaba erişimi olan kötü amaçlı bir kullanıcı, yönetici ayrıcalıklarıyla çalışan ve diğer görev dizinlerine erişen bir görev göndererek bu kısıtlamayı geçici olarak çözebilir. Kötü niyetli bir kullanıcı düğüme bağlanmak için RDP veya SSH'yi de kullanabilir. Böyle bir senaryoyu önlemek için Toplu Iş hesabı anahtarlarınıza erişimi korumak önemlidir. Hesabınızın gizliliğinin ihlal edilmiş olabileceğinden şüpheleniyorsanız, anahtarlarınızı yeniden oluşturduğunızdan emin olun.
>
>

### <a name="run-a-task-as-an-auto-user-with-elevated-access"></a>Yüksek erişime sahip otomatik kullanıcı olarak bir görevi çalıştırma

Yüksek erişime sahip bir görevi çalıştırmanız gerektiğinde yönetici ayrıcalıkları için otomatik kullanıcı belirtimini yapılandırabilirsiniz. Örneğin, bir başlangıç görevi düğüme yazılım yüklemek için yüksek erişim gerekebilir.

> [!NOTE] 
> Genel olarak, yalnızca gerektiğinde yüksek erişimi kullanmak en iyisidir. En iyi uygulamalar, istenen sonuca ulaşmak için gerekli minimum ayrıcalığı vermeyi önerir. Örneğin, bir başlangıç görevi tüm kullanıcılar yerine geçerli kullanıcı için yazılım yüklerse, görevlere yüksek erişim vermekten kaçınabilirsiniz. Başlangıç görevi de dahil olmak üzere aynı hesap altında çalışması gereken tüm görevler için havuz kapsamı ve yönetici olmayan erişim için otomatik kullanıcı belirtimini yapılandırabilirsiniz. 
>
>

Aşağıdaki kod parçacıkları otomatik kullanıcı belirtiminin nasıl yapılandırılabildiğini gösterir. Örnekler yükseklik düzeyini `Admin` ve kapsamını `Task`. Görev kapsamı varsayılan ayardır, ancak örnek olarak buraya eklenmiştir.

#### <a name="batch-net"></a>Batch .NET

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin, scope: AutoUserScope.Task));
```
#### <a name="batch-java"></a>Batch Java

```java
taskToAdd.withId(taskId)
        .withUserIdentity(new UserIdentity()
            .withAutoUser(new AutoUserSpecification()
                .withElevationLevel(ElevationLevel.ADMIN))
                .withScope(AutoUserScope.TASK));
        .withCommandLine("cmd /c echo hello");                        
```

#### <a name="batch-python"></a>Batch Python

```python
user = batchmodels.UserIdentity(
    auto_user=batchmodels.AutoUserSpecification(
        elevation_level=batchmodels.ElevationLevel.admin,
        scope=batchmodels.AutoUserScope.task))
task = batchmodels.TaskAddParameter(
    id='task_1',
    command_line='cmd /c "echo hello world"',
    user_identity=user)
batch_client.task.add(job_id=jobid, task=task)
```

### <a name="run-a-task-as-an-auto-user-with-pool-scope"></a>Havuz kapsamı olan otomatik kullanıcı olarak görev çalıştırma

Bir düğüm sağlandığında, havuzdaki her düğümde biri yüksek erişime sahip, diğeri de yüksek erişime sahip iki havuz çapında otomatik kullanıcı hesabı oluşturulur. Otomatik kullanıcının kapsamını belirli bir görev için kapsam havuzuna ayarlamak, görevi havuz genelindeki bu iki otomatik kullanıcı hesabından biri altında çalıştırıyor. 

Otomatik kullanıcı için havuz kapsamı belirttiğiniz zaman, yönetici erişimiyle çalışan tüm görevler aynı havuz çapında otomatik kullanıcı hesabı altında çalışır. Benzer şekilde, yönetici izinleri olmadan çalışan görevler de havuz genelinde tek bir otomatik kullanıcı hesabı altında çalışır. 

> [!NOTE] 
> Havuz genelindeki iki otomatik kullanıcı hesabı ayrı hesaplardır. Havuz genelindeki yönetim hesabı altında çalışan görevler, verileri standart hesap altında çalışan görevlerle paylaşamaz ve bunun tersi de vardır. 
>
>

Aynı otomatik kullanıcı hesabı altında çalıştırmanın avantajı, görevlerin verileri aynı düğümüzerinde çalışan diğer görevlerle paylaşabilmesidir.

Görevler arasında sırları paylaşmak, görevleri havuz genelindeki iki otomatik kullanıcı hesabından biri altında çalıştırmanın yararlı olduğu bir senaryodur. Örneğin, bir başlangıç görevinin diğer görevlerin kullanabileceği düğüme gizli bir sağlama sı gerektiğini varsayalım. Windows Veri Koruma API'sini (DPAPI) kullanabilirsiniz, ancak yönetici ayrıcalıkları gerektirir. Bunun yerine, kullanıcı düzeyinde gizli koruyabilirsiniz. Aynı kullanıcı hesabı altında çalışan görevler, yüksek erişim olmadan gizliye erişebilir.

Havuzları kapsamında bir otomatik kullanıcı hesabı altında görevleri çalıştırmak isteyebilirsiniz başka bir senaryo İleti geçen arabirim (MPI) dosya paylaşımı. MPI dosyası paylaşımı, MPI görevindeki düğümlerin aynı dosya verileri üzerinde çalışması gerektiğinde yararlıdır. Baş düğümü, aynı otomatik kullanıcı hesabı altında çalışan alt düğümlerin erişebileceği bir dosya paylaşımı oluşturur. 

Aşağıdaki kod snippet, Otomatik Kullanıcının kapsamını Toplu İşlem .NET'teki bir görev için kapsam havuzuna ayarlar. Yükseklik düzeyi atlanır, bu nedenle görev standart havuz genelindeki otomatik kullanıcı hesabı altında çalışır.

```csharp
task.UserIdentity = new UserIdentity(new AutoUserSpecification(scope: AutoUserScope.Pool));
```

## <a name="named-user-accounts"></a>Adlandırılmış kullanıcı hesapları

Bir havuz oluştururken adlandırılmış kullanıcı hesaplarını tanımlayabilirsiniz. Adlandırılmış bir kullanıcı hesabının sizin sağladığınız bir adı ve parolası vardır. Adlandırılmış bir kullanıcı hesabının yükseklik düzeyini belirtebilirsiniz. Linux düğümleri için, bir SSH özel anahtarı da sağlayabilirsiniz.

Adlandırılmış bir kullanıcı hesabı havuzdaki tüm düğümlerde bulunur ve bu düğümlerde çalışan tüm görevler için kullanılabilir. Bir havuz için herhangi bir sayıda adlandırılmış kullanıcı tanımlayabilirsiniz. Görev veya görev koleksiyonu eklediğinizde, görevin havuzda tanımlanan adlandırılmış kullanıcı hesaplarından birinin altında çalıştığını belirtebilirsiniz.

Bir işteki tüm görevleri aynı kullanıcı hesabı altında çalıştırmak istediğinizde, ancak bunları aynı anda diğer işlerde çalışan görevlerden ayırmak istediğinizde, adlandırılmış bir kullanıcı hesabı yararlıdır. Örneğin, her iş için adlandırılmış bir kullanıcı oluşturabilir ve her işin görevlerini bu adlandırılmış kullanıcı hesabı altında çalıştırabilirsiniz. Her iş daha sonra kendi görevleriyle bir sırrı paylaşabilir, ancak diğer işlerde çalışan görevlerle paylaşamaz.

Dosya paylaşımları gibi dış kaynaklarda izinler ayarlayan bir görevi çalıştırmak için adlandırılmış bir kullanıcı hesabı da kullanabilirsiniz. Adlandırılmış bir kullanıcı hesabıyla, kullanıcı kimliğini denetlersiniz ve izinleri ayarlamak için bu kullanıcı kimliğini kullanabilirsiniz.  

Adlandırılmış kullanıcı hesapları, Linux düğümleri arasında şifresiz SSH'yi etkinleştirin. Çok örnekli görevleri çalıştırması gereken Linux düğümleri olan adlandırılmış bir kullanıcı hesabı kullanabilirsiniz. Havuzdaki her düğüm, tüm havuzda tanımlanan bir kullanıcı hesabı altında görevleri çalıştırabilir. Çok örnekli görevler hakkında daha fazla bilgi için bkz. [MPI uygulamalarını çalıştırmak için çoklu\-örnekli görevleri kullan.](batch-mpi.md)

### <a name="create-named-user-accounts"></a>Adlandırılmış kullanıcı hesapları oluşturma

Toplu Iş'te adlandırılmış kullanıcı hesapları oluşturmak için havuza bir kullanıcı hesabı koleksiyonu ekleyin. Aşağıdaki kod parçacıkları ,NET, Java ve Python'da adlandırılmış kullanıcı hesaplarının nasıl oluşturulup oluşturulacagIni gösterir. Bu kod parçacıkları, bir havuzda hem yönetici hem de yönetici olmayan adlı hesapların nasıl oluşturulabildiğini gösterir. Örnekler bulut hizmeti yapılandırmasını kullanarak havuzlar oluşturur, ancak sanal makine yapılandırmasını kullanarak bir Windows veya Linux havuzu oluştururken aynı yaklaşımı kullanırsınız.

#### <a name="batch-net-example-windows"></a>Toplu .NET örneği (Windows)

```csharp
CloudPool pool = null;
Console.WriteLine("Creating pool [{0}]...", poolId);

// Create a pool using the cloud service configuration.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,
    virtualMachineSize: "standard_d1_v2",
    cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));   

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount("adminUser", "xyz123", ElevationLevel.Admin),
    new UserAccount("nonAdminUser", "123xyz", ElevationLevel.NonAdmin),
};

// Commit the pool.
await pool.CommitAsync();
```

#### <a name="batch-net-example-linux"></a>Toplu .NET örneği (Linux)

```csharp
CloudPool pool = null;

// Obtain a collection of all available node agent SKUs.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. 
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the virtual machine configuration to use to create the pool.
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

Console.WriteLine("Creating pool [{0}]...", poolId);

// Create the unbound pool.
pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 3,                                             
    virtualMachineSize: "Standard_A1",                                      
    virtualMachineConfiguration: virtualMachineConfiguration);                  

// Add named user accounts.
pool.UserAccounts = new List<UserAccount>
{
    new UserAccount(
        name: "adminUser",
        password: "xyz123",
        elevationLevel: ElevationLevel.Admin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 12345,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
    new UserAccount(
        name: "nonAdminUser",
        password: "123xyz",
        elevationLevel: ElevationLevel.NonAdmin,
        linuxUserConfiguration: new LinuxUserConfiguration(
            uid: 45678,
            gid: 98765,
            sshPrivateKey: new Guid().ToString()
            )),
};

// Commit the pool.
await pool.CommitAsync();
```


#### <a name="batch-java-example"></a>Toplu Java örneği

```java
List<UserAccount> userList = new ArrayList<>();
userList.add(new UserAccount().withName(adminUserAccountName).withPassword(adminPassword).withElevationLevel(ElevationLevel.ADMIN));
userList.add(new UserAccount().withName(nonAdminUserAccountName).withPassword(nonAdminPassword).withElevationLevel(ElevationLevel.NONADMIN));
PoolAddParameter addParameter = new PoolAddParameter()
        .withId(poolId)
        .withTargetDedicatedNodes(POOL_VM_COUNT)
        .withVmSize(POOL_VM_SIZE)
        .withCloudServiceConfiguration(configuration)
        .withUserAccounts(userList);
batchClient.poolOperations().createPool(addParameter);
```

#### <a name="batch-python-example"></a>Toplu Python örneği

```python
users = [
    batchmodels.UserAccount(
        name='pool-admin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.admin)
    batchmodels.UserAccount(
        name='pool-nonadmin',
        password='******',
        elevation_level=batchmodels.ElevationLevel.non_admin)
]
pool = batchmodels.PoolAddParameter(
    id=pool_id,
    user_accounts=users,
    virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        node_agent_sku_id=sku_to_use),
    vm_size=vm_size,
    target_dedicated=vm_count)
batch_client.pool.add(pool)
```

### <a name="run-a-task-under-a-named-user-account-with-elevated-access"></a>Yüksek erişime sahip adlandırılmış bir kullanıcı hesabı altında görev çalıştırma

Bir görevi yükseltilmiş bir kullanıcı olarak çalıştırmak için, görevin **Kullanıcı Kimliği** özelliğini, **Yükseklik Düzeyi** özelliği `Admin`olarak ayarlanmış olarak oluşturulan adlandırılmış bir kullanıcı hesabına ayarlayın.

Bu kod snippet görev adlı bir kullanıcı hesabı altında çalışması gerektiğini belirtir. Bu adlandırılmış kullanıcı hesabı, havuz oluşturulduğunda havuzda tanımlanmıştır. Bu durumda, adlandırılmış kullanıcı hesabı yönetici izinleri ile oluşturuldu:

```csharp
CloudTask task = new CloudTask("1", "cmd.exe /c echo 1");
task.UserIdentity = new UserIdentity(AdminUserAccountName);
```

## <a name="update-your-code-to-the-latest-batch-client-library"></a>Kodunuzu en son Toplu İstem istemci kitaplığına güncelleştirin

Toplu hizmet sürümü 2017-01-01.4.0, önceki sürümlerde mevcut **olan runElevated** özelliğini **userIdentity** özelliğiyle değiştirerek bir kırılma değişikliği sunar. Aşağıdaki tablolar, kodunuzu istemci kitaplıklarının önceki sürümlerinden güncelleştirmek için kullanabileceğiniz basit bir eşleme sağlar.

### <a name="batch-net"></a>Batch .NET

| Kodunuz kullanıyorsa...                  | Güncelleme ....                                                                                                 |
|---------------------------------------|------------------------------------------------------------------------------------------------------------------|
| `CloudTask.RunElevated = true;`       | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.Admin));`    |
| `CloudTask.RunElevated = false;`      | `CloudTask.UserIdentity = new UserIdentity(new AutoUserSpecification(elevationLevel: ElevationLevel.NonAdmin));` |
| `CloudTask.RunElevated`belirtilmemiş | Güncelleştirme gerekmez                                                                                               |

### <a name="batch-java"></a>Batch Java

| Kodunuz kullanıyorsa...                      | Güncelleme ....                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `CloudTask.withRunElevated(true);`        | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.ADMIN));`    |
| `CloudTask.withRunElevated(false);`       | `CloudTask.withUserIdentity(new UserIdentity().withAutoUser(new AutoUserSpecification().withElevationLevel(ElevationLevel.NONADMIN));` |
| `CloudTask.withRunElevated`belirtilmemiş | Güncelleştirme gerekmez                                                                                                                     |

### <a name="batch-python"></a>Batch Python

| Kodunuz kullanıyorsa...                      | Güncelleme ....                                                                                                                       |
|-------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| `run_elevated=True`                       | `user_identity=user`Nerede <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.admin))`                |
| `run_elevated=False`                      | `user_identity=user`Nerede <br />`user = batchmodels.UserIdentity(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`auto_user=batchmodels.AutoUserSpecification(`<br />&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`elevation_level=batchmodels.ElevationLevel.non_admin))`             |
| `run_elevated`belirtilmemiş | Güncelleştirme gerekmez                                                                                                                                  |


## <a name="next-steps"></a>Sonraki adımlar

* Toplu İşleme'ye derinlemesine bir bakış için [bkz.](batch-api-basics.md)
