---
title: İşlem düğümlerine uygulama paketleri dağıtma
description: Toplu işlem düğümlerinde yüklenmek üzere birden çok uygulamayı ve sürümü kolayca yönetmek için Azure Batch uygulama paketleri özelliğini kullanın.
ms.topic: how-to
ms.date: 09/16/2020
ms.custom: H1Hack27Feb2017, devx-track-csharp
ms.openlocfilehash: 0d705ca731c40563deaeb02c29da120211db7ff4
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90985065"
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a>Batch uygulama paketleriyle işlem düğümlerine uygulama dağıtma

Azure Batch uygulama paketleri özelliği, havuzlarınızdaki düğümlere işlem yapmak için görev uygulamalarını ve bunların dağıtımını yönetmenize yardımcı olur. Uygulama paketleri, Batch çözümünüzdeki kodu basitleştirebilir ve görevlerinizin çalıştırdığı uygulamaları yönetmek için gereken ek yükü düşürebilirler. Uygulama paketleriyle, görevlerinizin çalıştırdığı uygulamaların birden çok sürümünü, destekleyici dosyaları da dahil olmak üzere karşıya yükleyebilir ve yönetebilirsiniz. Daha sonra bu uygulamalardan bir veya daha fazlasını havuzunuzdaki işlem düğümlerine otomatik olarak dağıtabilirsiniz.

Uygulama paketleri, Iş, Batch özellikli hizmetlerinizle işleri işlerken kullanılacak tam sürümü belirterek müşterilerinizin işleri için uygulama seçmesini yardımcı olabilir. Müşterilerinizin hizmetinize kendi uygulamalarını yükleme ve izleme özelliği de sağlayabilirsiniz.

Uygulama paketleri oluşturmak ve yönetmek için API 'Ler [Batch yönetimi .net](/dotnet/api/overview/azure/batch/management) kitaplığı 'nın bir parçasıdır. Uygulama paketlerini bir işlem düğümüne yüklemek için API 'Ler [Batch .net](/dotnet/api/overview/azure/batch/client) kitaplığı 'nın bir parçasıdır. Benzer özellikler, diğer diller için kullanılabilir Batch API 'Lerinde bulunur.

Bu makalede, Azure portal uygulama paketlerini yükleme ve yönetme ve [Batch .net](/dotnet/api/overview/azure/batch/client) kitaplığı ile havuzun işlem düğümlerine yükleme işlemleri açıklanmaktadır.

## <a name="application-package-requirements"></a>Uygulama paketi gereksinimleri

Uygulama paketlerini kullanmak için, Batch hesabınıza [bir Azure depolama hesabı bağlamanız](#link-a-storage-account) gerekir.

Bir Batch hesabı içindeki uygulama ve uygulama paketlerinin sayısı ve en fazla uygulama paketi boyutu için kısıtlamalar vardır. Bu limitlerin ayrıntıları için [Azure Batch hizmetine yönelik kotalar ve sınırlar](batch-quota-limit.md) bölümüne bakın.

> [!NOTE]
> 5 Temmuz 2017 tarihinden önce oluşturulan toplu iş havuzları uygulama paketlerini desteklemez (Cloud Services yapılandırması kullanılarak 10 Mart 2016 ' den sonra oluşturulmadıkları takdirde).
>
> Burada açıklanan uygulama paketleri özelliği, hizmetin önceki sürümlerinde bulunan Batch Apps özelliğinin yerini alır.

## <a name="about-applications-and-application-packages"></a>Uygulamalar ve uygulama paketleri hakkında

Azure Batch içinde bir *uygulama* , havuzunuzdaki işlem düğümlerine otomatik olarak indirilebilen bir sürümlü ikili dosyalar kümesine başvurur. Bir *uygulama paketi* , uygulamanın belirli bir sürümünü temsil eden belirli bir ikili dosya kümesini ifade eder.

:::image type="content" source="media/batch-application-packages/app_pkg_01.png" alt-text="Uygulamaların ve uygulama paketlerinin üst düzey görünümünü gösteren diyagram.":::

Batch 'deki bir *uygulama* bir veya daha fazla uygulama paketi içerir ve uygulama için yapılandırma seçeneklerini belirtir. Örneğin, bir uygulama, işlem düğümlerine yüklenecek varsayılan uygulama paketi sürümünü ve paketlerin güncelleştirilip güncelleştirimeyeceğini veya silinemeyeceğini belirtebilir.

Uygulama *paketi* , uygulama ikililerini içeren bir. zip dosyasıdır ve görevleriniz uygulamayı çalıştırmak için gerekli dosyaları destekler. Her uygulama paketi, uygulamanın belirli bir sürümünü temsil eder. Yalnızca. zip biçimi desteklenir.

Uygulama paketlerini havuzda veya görev düzeylerinde belirtebilirsiniz. Bir havuz veya görev oluşturduğunuzda, bu paketlerden bir veya daha fazlasını ve (isteğe bağlı olarak) bir sürümü belirtebilirsiniz.

- **Havuz uygulama paketleri** havuzdaki her düğüme dağıtılır. Uygulamalar, bir düğüm bir havuza katıldığında ve yeniden başlatıldığında veya yeniden görüntülendiğinde dağıtılır.
  
    Havuz uygulama paketleri, bir havuzdaki tüm düğümler bir işin görevlerini yürütür. Bir havuz oluşturduğunuzda bir veya daha fazla uygulama paketi belirtebilir ve var olan bir havuzun paketlerini ekleyebilir veya güncelleştirebilirsiniz. Mevcut bir havuzun uygulama paketlerini güncelleştirirseniz, yeni paketi yüklemek için düğümlerini yeniden başlatmanız gerekir.

- Görev **uygulama paketleri** , görevin komut satırı çalıştırılmadan hemen önce yalnızca bir görevi çalıştırmak üzere zamanlanan bir işlem düğümüne dağıtılır. Belirtilen uygulama paketi ve sürümü zaten düğümde ise, yeniden dağıtılmadı ve mevcut paket kullanılır.
  
    Görev uygulama paketleri, farklı işlerin tek bir havuzda çalıştırıldığı ve bir iş tamamlandığında havuzun silinmediği paylaşılan havuz ortamlarında yararlıdır. İşinizin havuzdaki görevleri, düğümlerinden azsa uygulamanız yalnızca görevleri çalıştıran düğümlere dağıtıldığı için görev uygulama paketleri veri aktarımını azaltabilir.
  
    Görev uygulama paketlerinden faydalanabilir diğer senaryolar, büyük bir uygulamayı çalıştıran, ancak yalnızca birkaç görevde çalışan işlerdir. Örneğin, ön işleme veya birleştirme uygulamasının ağır olduğu bir ön işleme aşaması veya birleştirme görevi, görev uygulama paketlerinin kullanılması yararlı olabilir.

Uygulama paketleriyle, havuzunuzun başlangıç görevinin düğümlere yüklenecek tek kaynak dosyalarının uzun bir listesini belirtmek zorunda değildir. Azure depolama 'da veya düğümlerdeki uygulama dosyalarınızın birden çok sürümünü el ile yönetmeniz gerekmez. Ve Depolama hesabınızdaki dosyalara erişim sağlamak için [SAS URL 'leri](../storage/common/storage-sas-overview.md) oluşturma konusunda endişelenmeniz gerekmez. Batch, uygulama paketlerini depolamak ve bunları işlem düğümlerine dağıtmak için Azure depolama ile arka planda çalışmaktadır.

> [!NOTE]
> Bir başlangıç görevinin toplam boyutunun kaynak dosyaları ve ortam değişkenleri dahil olmak üzere 32.768 karakter veya daha az olması gerekir. Başlangıç göreviniz bu sınırı aşarsa, uygulama paketlerinin kullanılması başka bir seçenektir. Ayrıca, kaynak dosyalarınızı içeren bir. zip dosyası oluşturabilir, Azure depolama 'ya blob olarak yükleyebilir ve sonra başlangıç görevinin komut satırından bu dosyayı indirebilirsiniz.

## <a name="upload-and-manage-applications"></a>Uygulamaları karşıya yükleme ve yönetme

Batch hesabınızdaki uygulama paketlerini yönetmek için [Azure Portal](https://portal.azure.com) veya Batch yönetimi API 'lerini kullanabilirsiniz. Sonraki birkaç bölümde, önce bir depolama hesabını bağlamayı, ardından uygulama ve paket ekleme ve bunları portalla yönetme hakkında tartışın.

### <a name="link-a-storage-account"></a>Depolama hesabını bağlama

Uygulama paketlerini kullanmak için Batch hesabınıza bir [Azure depolama hesabı](accounts.md#azure-storage-accounts) bağlamanız gerekir. Batch hizmeti, uygulama paketlerinizi depolamak için ilişkili depolama hesabını kullanır. Batch hesabınızla birlikte kullanmak için özel olarak bir depolama hesabı oluşturmanızı öneririz.

Henüz bir depolama hesabı yapılandırmadıysanız, Batch hesabınızdaki **uygulamaları** ilk kez seçtiğinizde Azure Portal bir uyarı görüntüler. Bir depolama hesabını Batch hesabınıza bağlamak için, **Uyarı** penceresinde **depolama hesabı** ' nı seçin ve ardından **depolama hesabı** ' nı seçin.

İki hesabı bağladıktan sonra toplu Işlem, bağlantılı depolama hesabında depolanan paketleri işlem düğümlerine otomatik olarak dağıtabilir.

> [!IMPORTANT]
> Uygulama paketlerini [güvenlik duvarı kurallarıyla](../storage/common/storage-network-security.md)yapılandırılmış Azure depolama hesaplarıyla veya **hiyerarşik ad alanı** **etkin**olarak ayarlanmış şekilde kullanamazsınız.

Batch hizmeti, uygulama paketlerinizi blok blob 'ları olarak depolamak için Azure Storage 'ı kullanır. Blok Blobu verileri için [normal olarak ücretlendirilirsiniz](https://azure.microsoft.com/pricing/details/storage/) ve her bir paketin boyutu, Blok Blobu boyutunun üst sınırını aşamaz. Daha fazla bilgi için bkz. [depolama hesapları Için Azure Storage ölçeklenebilirlik ve performans hedefleri](../storage/blobs/scalability-targets.md). Maliyetleri en aza indirmek için uygulama paketlerinizin boyutunu ve sayısını göz önünde bulundurduğunuzdan ve kullanım dışı bırakılan paketleri düzenli olarak kaldırdığınızdan emin olun.

### <a name="view-current-applications"></a>Geçerli uygulamaları görüntüle

Batch hesabınızdaki uygulamaları görüntülemek için sol gezinti menüsünde **uygulamalar** ' ı seçin.

:::image type="content" source="media/batch-application-packages/app_pkg_02.png" alt-text="Azure portal uygulamalar menü öğesinin ekran görüntüsü.":::

Bu menü seçeneği belirlendiğinde **uygulamalar** penceresi açılır. Bu pencere, hesabınızdaki her uygulamanın KIMLIĞINI ve aşağıdaki özellikleri görüntüler:

- **Paketler**: Bu uygulamayla ilişkili sürümlerin sayısı.
- **Varsayılan sürüm**: varsa, uygulamayı dağıttığınızda sürüm belirtilmemişse yüklenecek uygulama sürümü.
- **Güncelleştirmelere Izin ver**: paket güncelleştirmelerine ve silmelerini izin verilip verilmeyeceğini belirtir.

İşlem düğümünüz uygulama paketinin [dosya yapısını](files-and-directories.md) görmek Için Azure Portal Batch hesabınıza gidin. **Havuzlar** ' ı seçin ve ardından ilgilendiğiniz işlem düğümünü içeren havuzu seçin. Ardından uygulama paketinin yüklü olduğu işlem düğümünü seçin ve **uygulamalar** klasörünü açın.

### <a name="view-application-details"></a>Uygulama ayrıntılarını görüntüle

Bir uygulamanın ayrıntılarını görmek için **uygulamalar** penceresinde seçin. Uygulamanız için aşağıdaki ayarları yapılandırabilirsiniz.

- **Güncelleştirmelere Izin ver**: uygulama paketlerinin [güncelleştirilip güncelleştirilmediğini veya silinemeyeceğini](#update-or-delete-an-application-package)gösterir. Varsayılan değer **Evet**’tir. **Hayır**olarak ayarlanırsa, yeni uygulama paketi sürümleri eklenebilse de, uygulama için paket güncelleştirmelerine ve silmelere izin verilmez.
- **Varsayılan sürüm**: sürüm belirtilmemişse, uygulama dağıtıldığında kullanılacak varsayılan uygulama paketi.
- **Görünen ad**: Batch çözümünüzün uygulama hakkında bilgi görüntülediğinde kullanabileceği kolay bir ad. Örneğin, bu ad Batch aracılığıyla müşterilerinize sağladığınız bir hizmetin Kullanıcı arabiriminde kullanılabilir.

### <a name="add-a-new-application"></a>Yeni uygulama ekle

Yeni bir uygulama oluşturmak için, bir uygulama paketi ekler ve yeni, benzersiz bir uygulama KIMLIĞI belirtirsiniz.

Batch hesabınızda, **uygulamalar** ' ı seçin ve ardından **Ekle**' yi seçin.

:::image type="content" source="media/batch-application-packages/app_pkg_05.png" alt-text="Azure portal yeni uygulama oluşturma işleminin ekran görüntüsü.":::

Aşağıdaki bilgileri girin:

- **Uygulama kimliği**: yenı uygulamanızın kimliği.
- **Sürüm**": karşıya yüklediğiniz uygulama paketinin sürümü.
- Uygulama **paketi**: uygulamayı yürütmek için gerekli olan uygulama ikililerini ve destekleyici dosyaları içeren. zip dosyası.

Girdiğiniz **uygulama kimliği** ve **sürümü** şu gereksinimlere uymalıdır:

- Windows düğümlerinde, KIMLIK alfasayısal karakter, kısa çizgi ve alt çizgi birleşimini içerebilir. Linux düğümlerinde yalnızca alfasayısal karakterlere ve alt çizgilere izin verilir.
- 64 karakterden fazlasını içeremez.
- Batch hesabı dahilinde benzersiz olmalıdır.
- Kimlikler büyük/küçük harf koruma ve büyük/küçük harfe duyarlı değildir.

Hazırsanız **Gönder**' i seçin. . Zip dosyası Azure depolama hesabınıza yüklendikten sonra, Portal bir bildirim görüntüler. Karşıya yüklediğiniz dosyanın boyutuna ve ağ bağlantınızın hızına bağlı olarak bu işlem biraz zaman alabilir.

### <a name="add-a-new-application-package"></a>Yeni bir uygulama paketi Ekle

Mevcut bir uygulama için bir uygulama paketi sürümü eklemek istiyorsanız, Batch hesabınızın **uygulamalar** bölümünde uygulamayı seçin, sonra **Ekle**' yi seçin.

Yeni uygulama için yaptığınız gibi, yeni paketinizin **sürümünü** belirtin,. zip dosyanızı **uygulama paketi** alanına yükleyin ve ardından **Gönder**' i seçin.

### <a name="update-or-delete-an-application-package"></a>Uygulama paketini güncelleştirme veya silme

Var olan bir uygulama paketini güncelleştirmek veya silmek için Batch hesabınızın **uygulamalar** bölümünde uygulamayı seçin. Değiştirmek istediğiniz uygulama paketinin satırındaki üç noktayı seçin ve ardından gerçekleştirmek istediğiniz eylemi seçin.

:::image type="content" source="media/batch-application-packages/app_pkg_07.png" alt-text="Azure portal uygulama paketleri için güncelleştirme ve silme seçeneklerini gösteren ekran görüntüsü.":::

**Güncelleştir**' i seçerseniz, yeni bir. zip dosyasını karşıya yükleyebilirsiniz. Bu, bu sürüm için karşıya yüklediğiniz önceki. zip dosyasının yerini alır.

**Sil**' i seçerseniz, bu sürümün silinmesini onaylamanız istenir. **Tamam**' ı seçtiğinizde Batch, Azure depolama hesabınızdan. zip dosyasını silecektir. Bir uygulamanın varsayılan sürümünü silerseniz, bu uygulama için **varsayılan sürüm** ayarı kaldırılır.

## <a name="install-applications-on-compute-nodes"></a>İşlem düğümlerine uygulama yükler

Azure portal uygulama paketlerinin nasıl yönetileceğini öğrendiğinize göre, bunları işlem düğümlerine dağıtmayı ve Batch görevleriyle çalıştırmayı tartışabiliriz.

### <a name="install-pool-application-packages"></a>Havuz uygulama paketlerini yükler

Bir havuzdaki tüm işlem düğümlerine bir uygulama paketi yüklemek için havuz için bir veya daha fazla uygulama paketi başvurusu belirtin. Bir havuz için belirttiğiniz uygulama paketleri, düğüm havuza katıldığında ve düğüm yeniden başlatıldığında veya yeniden görüntülendiğinde her bir işlem düğümüne yüklenir.

Batch .NET sürümünde, yeni bir havuz oluştururken veya mevcut bir havuz için bir veya daha fazla [Cloudpool. Applicationpackagereferde](/dotnet/api/microsoft.azure.batch.cloudpool.applicationpackagereferences) belirtin. [Applicationpackagereference](/dotnet/api/microsoft.azure.batch.applicationpackagereference) sınıfı, bir havuzun işlem düğümlerine yüklenecek BIR uygulama kimliği ve sürümünü belirtir.

```csharp
// Create the unbound CloudPool
CloudPool myCloudPool =
    batchClient.PoolOperations.CreatePool(
        poolId: "myPool",
        targetDedicatedComputeNodes: 1,
        virtualMachineSize: "standard_d1_v2",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "5"));

// Specify the application and version to install on the compute nodes
myCloudPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "litware",
        Version = "1.1001.2b" }
};

// Commit the pool so that it's created in the Batch service. As the nodes join
// the pool, the specified application package is installed on each.
await myCloudPool.CommitAsync();
```

> [!IMPORTANT]
> Bir uygulama paketi dağıtımı herhangi bir nedenle başarısız olursa, Batch Hizmeti düğümü [kullanılamaz](/dotnet/api/microsoft.azure.batch.computenode.state)olarak işaretler ve bu düğümde yürütülmek üzere hiçbir görev planlanmaz. Bu durumda, paket dağıtımını yeniden başlatmak için düğümü yeniden başlatmanız gerekir. Düğüm yeniden başlatıldığında, düğüm üzerinde görev zamanlamayı yeniden de etkinleştirilir.

### <a name="install-task-application-packages"></a>Görev uygulama paketlerini yükler

Bir havuza benzer şekilde, bir görev için uygulama paketi başvurularını belirtirsiniz. Bir görev bir düğümde çalışmak üzere zamanlandığında, bu paket, görevin komut satırı yürütülmeden hemen önce indirilir ve ayıklanır. Belirtilen bir paket ve sürüm düğümde zaten yüklüyse, paket indirilmez ve mevcut paket kullanılır.

Bir görev uygulama paketini yüklemek için görevin [Cloudtask. Applicationpackagerefersin](/dotnet/api/microsoft.azure.batch.cloudtask.applicationpackagereferences) özelliğini yapılandırın:

```csharp
CloudTask task =
    new CloudTask(
        "litwaretask001",
        "cmd /c %AZ_BATCH_APP_PACKAGE_LITWARE%\\litware.exe -args -here");

task.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference
    {
        ApplicationId = "litware",
        Version = "1.1001.2b"
    }
};
```

## <a name="execute-the-installed-applications"></a>Yüklü uygulamaları yürütme

Bir havuz veya görev için belirttiğiniz paketler, düğüm içindeki adlandırılmış bir dizine indirilir ve ayıklanır `AZ_BATCH_ROOT_DIR` . Batch Ayrıca, adlandırılmış dizinin yolunu içeren bir ortam değişkeni oluşturur. Görev komut satırlarında, düğümdeki uygulamaya başvururken bu ortam değişkeni kullanılır.

Windows düğümlerinde, değişkeni aşağıdaki biçimdedir:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Linux düğümlerinde biçim biraz farklıdır. Nokta (.), tire (-) ve sayı işaretleri (#), ortam değişkeninde alt çizgi olarak düzleştirilir. Ayrıca, uygulama KIMLIĞI durumunun korunmadığını unutmayın. Örnek:

```
Linux:
AZ_BATCH_APP_PACKAGE_applicationid_version
```

`APPLICATIONID` ve `version` dağıtım için belirttiğiniz uygulama ve paket sürümüne karşılık gelen değerlerdir. Örneğin, *blender* uygulamasının 2,7 sürümünün Windows düğümlerinde yüklü olması gerektiğini belirttiyseniz, görev komut satırları bu ortam değişkenini, dosyalarına erişmek için kullanır:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Linux düğümlerinde, ortam değişkenini bu biçimde belirtin. Noktaları (.), kısa çizgiler (-) ve sayı işaretlerini (#) düzleştirin ve uygulama KIMLIĞI durumunu koruyun:

```
Linux:
AZ_BATCH_APP_PACKAGE_blender_2_7
```

Bir uygulama paketini karşıya yüklediğinizde, işlem düğümleriniz için dağıtım yapmak üzere varsayılan bir sürüm belirtebilirsiniz. Bir uygulama için varsayılan bir sürüm belirttiyseniz, uygulamaya başvuru yaptığınızda sürüm sonekini atlayabilirsiniz. **Uygulamalar penceresinde,** [uygulamaları karşıya yükleme ve yönetme](#upload-and-manage-applications)bölümünde gösterildiği gibi, varsayılan uygulama sürümünü Azure Portal belirleyebilirsiniz.

Örneğin, uygulama *blender*için varsayılan sürüm olarak "2,7" ayarlarsanız ve görevleriniz aşağıdaki ortam değişkenine Başvurdıysanız, Windows düğümleriniz 2,7 sürümünü yürütür:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Aşağıdaki kod parçacığında, *blender* uygulamasının varsayılan sürümünü başlatan örnek bir görev komut satırı gösterilmektedir:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> İşlem düğümü ortam ayarları hakkında daha fazla bilgi için bkz. [Görevler Için ortam ayarları](jobs-and-tasks.md#environment-settings-for-tasks).

## <a name="update-a-pools-application-packages"></a>Bir havuzun uygulama paketlerini güncelleştirme

Mevcut bir havuz zaten bir uygulama paketiyle yapılandırıldıysa, havuz için yeni bir paket belirtebilirsiniz. Bir havuz için yeni bir paket başvurusu belirtirseniz aşağıdakiler geçerlidir:

- Batch hizmeti, yeni belirtilen paketi havuza ve yeniden başlatılan veya yeniden görüntü oluşturulan tüm mevcut düğümlerde birleştirir.
- Paket başvurularını güncelleştirdiğinizde zaten havuzda olan işlem düğümleri yeni uygulama paketini otomatik olarak yüklemez. Yeni paketi almak için bu işlem düğümlerinin yeniden başlatılması veya yeniden oluşturulması gerekir.
- Yeni bir paket dağıtıldığında oluşturulan ortam değişkenleri yeni uygulama paketi başvurularını yansıtır.

Bu örnekte, mevcut havuzun [Cloudpool. Applicationpackagereferde](/dotnet/api/microsoft.azure.batch.cloudpool.applicationpackagereferences)biri olarak yapılandırılmış *blender* uygulamasının sürüm 2,7 ' i vardır. Havuzun düğümlerini 2.76 b sürümüyle güncelleştirmek için yeni sürümle yeni bir [Applicationpackagereference](/dotnet/api/microsoft.azure.batch.applicationpackagereference) belirtip değişikliği işleyin.

```csharp
string newVersion = "2.76b";
CloudPool boundPool = await batchClient.PoolOperations.GetPoolAsync("myPool");
boundPool.ApplicationPackageReferences = new List<ApplicationPackageReference>
{
    new ApplicationPackageReference {
        ApplicationId = "blender",
        Version = newVersion }
};
await boundPool.CommitAsync();
```

Yeni sürüm yapılandırıldıktan sonra Batch hizmeti, 2.76 b sürümünü havuza katılan yeni bir düğüme yüklemiştir. 2.76 b 'yi zaten havuzda olan düğümlere yüklemek için yeniden başlatın veya yeniden başlatın. Yeniden başlatılan düğümlerin dosyaları önceki paket dağıtımlarından korumasını unutmayın.

## <a name="list-the-applications-in-a-batch-account"></a>Bir Batch hesabındaki uygulamaları listeleyin

[Applicationoperations. Listapplicationözetler](/dotnet/api/microsoft.azure.batch.applicationoperations.listapplicationsummaries) yöntemini kullanarak uygulamaları ve bunların paketlerini bir Batch hesabında listeleyebilirsiniz.

```csharp
// List the applications and their application packages in the Batch account.
List<ApplicationSummary> applications = await batchClient.ApplicationOperations.ListApplicationSummaries().ToListAsync();
foreach (ApplicationSummary app in applications)
{
    Console.WriteLine("ID: {0} | Display Name: {1}", app.Id, app.DisplayName);

    foreach (string version in app.Versions)
    {
        Console.WriteLine("  {0}", version);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Batch REST API](/rest/api/batchservice) Ayrıca uygulama paketleriyle çalışmak için destek sağlar. Örneğin, yüklenecek paketleri belirtme ve uygulama bilgilerinin nasıl alınacağı için [uygulamalar](/rest/api/batchservice/application) hakkında bilgi için bkz. [applicationpackagereferges](/rest/api/batchservice/pool/add#applicationpackagereference) öğesi.
- [Batch yönetimi .NET ile Azure Batch hesaplarını ve kotaları](batch-management-dotnet.md)programlı bir şekilde yönetmeyi öğrenin. [Batch yönetimi .net](/dotnet/api/overview/azure/batch/management) kitaplığı, Batch uygulamanız veya hizmetiniz için hesap oluşturma ve silme özelliklerini etkinleştirebilir.
