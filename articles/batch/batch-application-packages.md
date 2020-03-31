---
title: Uygulama paketlerini bilgi işlem düğümlerine yükleme - Azure Toplu İşi | Microsoft Dokümanlar
description: Toplu İşlem düğümlerinde yükleme için birden çok uygulamayı ve sürümü kolayca yönetmek için Azure Toplu İşlem'in uygulama paketleri özelliğini kullanın.
services: batch
documentationcenter: .net
author: LauraBrenner
manager: evansma
editor: ''
ms.assetid: 3b6044b7-5f65-4a27-9d43-71e1863d16cf
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/26/2019
ms.author: labrenne
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30301832381bdc7b5f001eec2c449c571f9fd671
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79086228"
---
# <a name="deploy-applications-to-compute-nodes-with-batch-application-packages"></a>Toplu uygulama paketleriyle düğümleri hesaplamak için uygulamaları dağıtma

Azure Toplu İşlem'in uygulama paketleri özelliği, görev uygulamalarının kolay yönetilmesini ve bunların havuzunuzdaki bilgi işlem düğümlerine dağıtımını sağlar. Uygulama paketleriyle, yardımcı dosyaları da dahil olmak üzere görevlerinizin çalıştırıştıcı uygulamalarının birden çok versiyonunu yükleyebilir ve yönetebilirsiniz. Daha sonra, bu uygulamalardan birini veya birkaçını havuzunuzdaki bilgi işlem düğümlerine otomatik olarak dağıtabilirsiniz.

Bu makalede, Azure portalında uygulama paketlerini nasıl yükleyip yöneteceklerini öğreneceksiniz. Daha sonra [bunları Toplu .NET][api_net] kitaplığıyla bir havuzun bilgi işlem düğümlerine nasıl yükleyeceklerini öğrenirsiniz.

> [!NOTE]
> Uygulama paketleri 5 Temmuz 2017’den sonra oluşturulmuş tüm Batch havuzlarında desteklenir. Bunların 10 Mart 2016 ve 5 Haziran 2017 arasında oluşturulmuş Batch havuzlarında desteklenebilmesi için, havuzun Bulut Hizmeti yapılandırması kullanılarak oluşturulmuş olması gerekir. 10 Mart 2016’dan önce oluşturulan Batch havuzları uygulama paketlerini desteklemez.
>
> Uygulama paketleri oluşturmak ve yönetmek için API'ler [Toplu İşlem Yönetimi .NET][api_net_mgmt] kitaplığı'nın bir parçasıdır. Uygulama paketlerini bir bilgi işlem düğümüne yüklemek için api'ler [Toplu İşlem .NET][api_net] kitaplığında yer alır. Karşılaştırılabilir özellikler diğer diller için kullanılabilir Toplu İş API'lerinde bulunmaktadır. 
>
> Burada açıklanan uygulama paketleri özelliği, hizmetin önceki sürümlerinde bulunan Toplu Uygulamalar özelliğinin yerini alır.

## <a name="application-package-requirements"></a>Uygulama paketi gereksinimleri
Uygulama paketlerini kullanmak için bir Azure Depolama hesabını Toplu Iş hesabınıza [bağlamanız](#link-a-storage-account) gerekir.

## <a name="about-applications-and-application-packages"></a>Uygulamalar ve uygulama paketleri hakkında
Azure Toplu İş'te bir *uygulama,* havuzunuzdaki bilgi işlem düğümlerine otomatik olarak indirilebilen bir dizi sürümlü ikili başvurur. Bir *uygulama paketi* bu ikili belirli bir *dizi* anlamına gelir ve uygulamanın belirli bir *sürümünü* temsil eder.

![Uygulamaların ve uygulama paketlerinin üst düzey diyagramı][1]

### <a name="applications"></a>Uygulamalar
Toplu İşlem'deki bir uygulama bir veya daha fazla uygulama paketi içerir ve uygulama için yapılandırma seçeneklerini belirtir. Örneğin, bir uygulama bilgi işlem düğümlerine yüklenmek üzere varsayılan uygulama paketi sürümünü ve paketlerinin güncelleştirilip güncelleştirilmeyeceğini veya silinip silinmeyeceğini belirtebilir.

### <a name="application-packages"></a>Uygulama paketleri
Uygulama paketi, uygulama ikililerini ve uygulamanızın çalıştırılabilmesi için görevleriniziçin gerekli olan destekleyici dosyaları içeren bir .zip dosyasıdır. Her uygulama paketi uygulamanın belirli bir sürümünü temsil eder.

Uygulama paketlerini havuzda ve görev düzeylerinde belirtebilirsiniz. Havuz veya görev oluşturduğunuzda bu paketlerden birini veya daha fazlasını ve (isteğe bağlı olarak) bir sürüm belirtebilirsiniz.

* **Havuz uygulama paketleri** havuzdaki *her* düğüme dağıtılır. Uygulamalar, düğüm bir havuza katıldığında ve yeniden başlatıldığında veya yeniden görüntülendiğinde dağıtılır.
  
    Havuz uygulama paketleri, havuzdaki tüm düğümler işin görevlerini yürüttüğünde uygundur. Havuz oluştururken bir veya daha fazla uygulama paketi belirtebilir ve varolan bir havuzun paketlerini ekleyebilir veya güncelleştirebilirsiniz. Varolan bir havuzun uygulama paketlerini güncellerseniz, yeni paketi yüklemek için düğümlerini yeniden başlatmanız gerekir.
* **Görev uygulama paketleri,** görevin komut satırını çalıştırmadan hemen önce yalnızca görevi çalıştırmak için zamanlanan bir bilgi işlem düğümüne dağıtılır. Belirtilen uygulama paketi ve sürümü düğümüzerinde zaten varsa, yeniden dağıtılanmaz ve varolan paket kullanılır.
  
    Görev uygulama paketleri, farklı işlerin tek bir havuzda çalıştırıldığı ve bir iş tamamlandığında havuzun silindiği paylaşılan havuz ortamlarında kullanışlıdır. İşinizin havuzdaki görevleri, düğümlerinden azsa uygulamanız yalnızca görevleri çalıştıran düğümlere dağıtıldığı için görev uygulama paketleri veri aktarımını azaltabilir.
  
    Görev uygulama paketlerinden yararlanabilecek diğer senaryolar, büyük bir uygulamayı çalıştıran, ancak yalnızca birkaç görev için olan işlerdir. Örneğin, ön işleme veya birleştirme uygulamasının ağır olduğu bir ön işleme aşaması veya birleştirme görevi, görev uygulama paketlerini kullanmaktan yararlanabilir.

> [!IMPORTANT]
> Toplu iş hesabındaki uygulama ve uygulama paketlerinin sayısı ve maksimum uygulama paketi boyutunda kısıtlamalar vardır. Bu sınırlar hakkında ayrıntılı bilgi için [Azure Toplu İş hizmetinin Kotaları ve sınırları](batch-quota-limit.md) na bakın.
> 
> 

### <a name="benefits-of-application-packages"></a>Uygulama paketlerinin faydaları
Uygulama paketleri Toplu İşlem çözümünüzdeki kodu basitleştirebilir ve görevlerinizi çalıştıran uygulamaları yönetmek için gereken ek yükü düşürebilir.

Uygulama paketlerinde, havuzunuzun başlangıç görevi düğümlere yüklenmesi için tek tek kaynak dosyalarının uzun bir listesini belirtmek zorunda değildir. Azure Depolama'da veya düğümlerinizde uygulama dosyalarınızın birden çok sürümüne el ile yönetmeniz gerekmez. Ayrıca, Depolama hesabınızdaki dosyalara erişim sağlamak için [SAS URL'leri](../storage/common/storage-dotnet-shared-access-signature-part-1.md) oluşturma konusunda endişelenmenize gerek yoktur. Toplu işlem kümelerini hesaplamak için uygulama paketlerini depolamak ve dağıtmak için Azure Depolama ile arka planda çalışır.

> [!NOTE] 
> Bir başlangıç görevinin toplam boyutunun kaynak dosyaları ve ortam değişkenleri dahil olmak üzere 32.768 karakter veya daha az olması gerekir. Başlangıç göreviniz bu sınırı aşıyorsa, uygulama paketlerini kullanmak başka bir seçenektir. Ayrıca kaynak dosyalarınızı içeren sıkıştırılmış bir arşiv oluşturabilir, Azure Depolama'ya blob olarak yükleyebilir ve başlangıç görevinizin komut satırından fermuarını açabilirsiniz. 
>
>

## <a name="upload-and-manage-applications"></a>Uygulamaları yükleme ve yönetme
Toplu İş hesabınızdaki uygulama paketlerini yönetmek için [Azure portalını][portal] veya Toplu İşlem Yönetimi API'lerini kullanabilirsiniz. Sonraki birkaç bölümde, önce bir Depolama hesabının nasıl bağlanılsüreceğini gösteririz, ardından uygulama ve paket eklemeyi ve bunları portalla yönetmeyi tartışırız.

### <a name="link-a-storage-account"></a>Depolama hesabı bağlama
Uygulama paketlerini kullanmak için öncelikle bir [Azure Depolama hesabını](batch-api-basics.md#azure-storage-account) Toplu Iş hesabınıza bağlamanız gerekir. Henüz bir Depolama hesabı yapılandırmadıysanız, Azure portalı Toplu İş hesabınızda **Uygulamalar'ı** ilk kez tıklattığınızda bir uyarı görüntüler.



![Azure portalında 'depolama hesabı yapılandırılmama' uyarısı][9]

Toplu İşlem hizmeti, uygulama paketlerinizi depolamak için ilişkili Depolama hesabını kullanır. İki hesabı bağladıktan sonra, Toplu İşlem bağlantılı Depolama hesabında depolanan paketleri otomatik olarak işlem düğümlerinize dağıtabilir. Bir Depolama hesabını Toplu Hesabınıza bağlamak için **Uyarı** penceresinde **Ki Depolama hesabını** tıklatın ve ardından Depolama **Hesabı'nı** yeniden tıklatın.

![Azure portalında depolama hesabı kılıcını seçin][10]

Toplu İş hesabınızda kullanılmak üzere *özel olarak* bir Depolama hesabı oluşturmanızı ve buradan seçmenizi öneririz. Bir Depolama hesabı oluşturduktan sonra, **Depolama Hesabı** penceresini kullanarak bu hesabı Toplu Hesabınıza bağlayabilirsiniz.

> [!IMPORTANT] 
> - Şu [anda, güvenlik duvarı kurallarıyla](../storage/common/storage-network-security.md)yapılandırılan bir Azure Depolama hesabıyla uygulama paketlerini kullanamazsınız.
> - **Hiyerarşik** ad **alanı** etkin olarak ayarlanmış bir Azure Depolama hesabı uygulama paketleri için kullanılamaz.

Toplu İşlem hizmeti, uygulama paketlerinizi blok blobs olarak depolamak için Azure Depolama'yı kullanır. Blok blob verileri için [normal olarak ücretlendirilirsiniz][storage_pricing] ve her paketin boyutu maksimum blok blob boyutunu geçemez. Daha fazla bilgi için [bkz.](../storage/blobs/scalability-targets.md) Uygulama paketlerinizin boyutunu ve sayısını göz önünde bulundurun ve maliyetleri en aza indirmek için amortismana alınan paketleri düzenli aralıklarla kaldırın.

### <a name="view-current-applications"></a>Geçerli uygulamaları görüntüleme
Toplu Iş hesabınızdaki uygulamaları görüntülemek için, **Toplu İş hesabınızı**görüntülerken sol menüdeki **Uygulamalar** menüsü öğesini tıklatın.

![Uygulamalar döşeme][2]

Bu menü seçeneğini **seçmek, Uygulamalar** penceresini açar:

![Uygulamaları listeleme][3]

Bu pencere, hesabınızdaki her uygulamanın kimliğini ve aşağıdaki özellikleri görüntüler:

* **Paketler**: Bu uygulamayla ilişkili sürüm sayısı.
* **Varsayılan sürüm**: Havuz için uygulamayı belirtirken bir sürümü belirtmezseniz, uygulama sürümü yüklenir. Bu ayar isteğe bağlıdır.
* **Güncelleştirmelere izin ver**: Paket güncelleştirmelerine, silmelere ve eklemelere izin verilip verilmediğini belirten değerdir. Bu **No**olarak ayarlanırsa, paket güncelleştirmeleri ve silmeler uygulama için devre dışı bırakılır. Yalnızca yeni uygulama paketi sürümleri eklenebilir. Varsayılan değer **Evet**’tir.

İşlem düğümünüzde uygulama paketinin dosya yapısını görmek istiyorsanız, portaldaki Toplu İşlem hesabınıza gidin. Toplu İşlem hesabınızdan **Havuzlar'a**gidin. İlgilendiğiniz işlem düğümü(ler)i içeren havuzu seçin.

![Havuzdaki düğümler][13]

Havuzunuzu seçtikten sonra, uygulama paketinin yüklü olduğu bilgi işlem düğümüne gidin. Buradan, uygulama paketinin ayrıntıları **uygulamalar** klasöründe yer alır. İşlem düğümündeki ek klasörler, başlangıç görevleri, çıktı dosyaları, hata çıktısı gibi diğer dosyaları içerir.

![Düğümle ilgili dosyalar][14]

### <a name="view-application-details"></a>Uygulama ayrıntılarını görüntüleme
Uygulama ayrıntılarını görmek için **Uygulamalar** penceresindeuygulamayı seçin.

![Uygulama ayrıntıları][4]

Uygulama ayrıntılarında, uygulamanız için aşağıdaki ayarları yapılandırabilirsiniz.

* **Güncelleştirmeleri İzin ver**: Uygulama paketlerinin güncelleştirilip güncelleştirilemeyeceğini veya silinip silinemeyeceğini belirtin. Bu makalenin ilerleyen saatlerinde "Bir uygulama paketini güncelleştir veya sil" başlıklı makaleye bakın.
* **Varsayılan sürüm**: Düğümleri hesaplamak için dağıtmak için varsayılan bir uygulama paketi belirtin.
* **Ekran adı**: Toplu İşlem çözümünüzün uygulama yla ilgili bilgileri görüntülediğinde kullanabileceği, örneğin Toplu İşlem aracılığıyla müşterilerinize sağladığınız bir hizmetin kullanıcı arabirimi'nde kullanabileceği dostu bir ad belirtin.

### <a name="add-a-new-application"></a>Yeni bir uygulama ekleme
Yeni bir uygulama oluşturmak için bir uygulama paketi ekleyin ve yeni, benzersiz bir uygulama kimliği belirtin. Yeni uygulama kimliği ile eklediğiniz ilk uygulama paketi de yeni uygulamayı oluşturur.

**Uygulamaları** > **Ekle'yi**tıklatın.

![Azure portalında yeni uygulama bıçağı][5]

**Yeni uygulama** penceresi, yeni uygulama ve uygulama paketinizin ayarlarını belirtmek için aşağıdaki alanları sağlar.

**Başvuru Kimliği**

Bu alan, standart Azure Toplu İş Kimliği doğrulama kurallarına tabi olan yeni uygulamanızın kimliğini belirtir. Uygulama kimliği sağlama kuralları aşağıdaki gibidir:

* Windows düğümlerinde, kimlik alfasayısal karakterlerin, tirelerin ve alt çizginin herhangi bir birleşimini içerebilir. Linux düğümlerinde yalnızca alfasayısal karakterlere ve alt çizgide izin verilir.
* 64'ten fazla karakter içeremez.
* Toplu İşlem hesabı içinde benzersiz olmalıdır.
* Büyük/küçük harf koruma ve büyük/küçük harf duyarsız.

**Sürüm**

Bu alan, yüklediğiniz uygulama paketinin sürümünü belirtir. Sürüm dizeleri aşağıdaki doğrulama kurallarına tabidir:

* Windows düğümlerinde, sürüm dizesi alfasayısal karakterlerin, tirelerin, alt çizgilerinin ve dönemlerin herhangi bir birleşimini içerebilir. Linux düğümlerinde, sürüm dizesi yalnızca alfasayısal karakterler ve alt çizgi içerebilir.
* 64'ten fazla karakter içeremez.
* Uygulama içinde benzersiz olmalıdır.
* Büyük/küçük harf koruma ve büyük/küçük harf duyarsız.

**Başvuru paketi**

Bu alan, uygulama ikililerini ve uygulamayı yürütmek için gereken destekleyici dosyaları içeren .zip dosyasını belirtir. Göz atmak için dosya kutusunu veya klasör simgesini **seç'i** tıklatın ve uygulamanızın dosyalarını içeren bir .zip dosyasını seçin.

Bir dosya seçtikten sonra, Azure Depolama'ya yüklemeye başlamak için **Tamam'ı** tıklatın. Yükleme işlemi tamamlandığında, portal bir bildirim görüntüler. Yüklediğiniz dosyanın boyutuna ve ağ bağlantınızın hızına bağlı olarak bu işlem biraz zaman alabilir.

> [!WARNING]
> Yükleme işlemi tamamlanmadan **Yeni uygulama** penceresini kapatmayın. Bunu yapmak yükleme işlemini durdurur.
> 
> 

### <a name="add-a-new-application-package"></a>Yeni bir uygulama paketi ekleme
Varolan bir uygulama için uygulama paketi sürümü eklemek için **Uygulamalar** penceresinde bir uygulama seçin ve **Paketler** > **Ekle'yi**tıklatın.

![Azure portalında uygulama paketi bıçağı ekleme][8]

Gördüğünüz gibi, alanlar **Yeni uygulama** penceresiyle eşleşir, ancak **Uygulama Kimliği** kutusu devre dışı bırakılır. Yeni uygulamada yaptığınız gibi, yeni paketinizin **Sürümünü** belirtin, **Uygulama paketinize** göz atın .zip dosyasına bakın ve paketi yüklemek için **Tamam'ı** tıklatın.

### <a name="update-or-delete-an-application-package"></a>Bir uygulama paketini güncelleştirme veya silme
Varolan bir uygulama paketini güncelleştirmek veya silmek için uygulamanın ayrıntılarını açın, **Paketler'i**tıklatın, değiştirmek istediğiniz uygulama paketinin satırındaki **elipsleri** tıklatın ve gerçekleştirmek istediğiniz eylemi seçin.

![Azure portalında paketi güncelleştirme veya silme][7]

**Güncelleştirme**

**Güncelleştir'i**tıklattığınızda, **Güncelleştirme paketi** pencereleri görüntülenir. Bu pencere Yeni **uygulama paketi** penceresine benzer, ancak yalnızca paket seçim alanı etkinleştirilir ve yüklemek için yeni bir ZIP dosyası belirtmenize olanak sağlar.

![Azure portalında paket bıçağını güncelleştirin][11]

**Sil**

**Sil'i**tıklattığınızda, paket sürümünün silinmesini onaylamanız istenir ve Toplu İşlem paketi Azure Depolama'dan siler. Bir uygulamanın varsayılan sürümünü silerseniz, uygulama için **Varsayılan sürüm** ayarı kaldırılır.

![Uygulamayı silme][12]

## <a name="install-applications-on-compute-nodes"></a>Uygulamaları bilgi işlem düğümlerine yükleme
Azure portalı ile uygulama paketlerini nasıl yönetebileceğinizi öğrendiğinize göre, düğümleri hesaplamak ve Toplu işlerle çalıştırmak için bunları nasıl dağıtabileceğimizi tartışabiliriz.

### <a name="install-pool-application-packages"></a>Havuz uygulama paketlerini yükleme
Havuzdaki tüm bilgi işlem düğümlerine uygulama paketi yüklemek için havuz için bir veya daha fazla uygulama paketi *başvurusu* belirtin. Bir havuz için belirttiğiniz uygulama paketleri, düğüm havuza katıldığında ve düğüm yeniden başlatıldığında veya yeniden görüntülendiğinde her işlem düğümüne yüklenir.

Toplu .NET'te bir veya daha fazla [CloudPool][net_cloudpool]belirtin. [ApplicationPackageReferences][net_cloudpool_pkgref] yeni bir havuz oluşturduğunuzda veya varolan bir havuz için. [ApplicationPackageReference][net_pkgref] sınıfı, havuzun bilgi işlem düğümlerine yüklenmesi için bir uygulama kimliği ve sürümünü belirtir.

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
> Bir uygulama paketi dağıtımı herhangi bir nedenle başarısız olursa, Toplu İşlem hizmeti düğümü [kullanılamaz][net_nodestate]olarak işaretler ve bu düğümüzerinde yürütülmesi için hiçbir görev zamanlanmaz. Bu durumda, paket dağıtımını yeniden başlatmak için düğümü **yeniden başlatmanız** gerekir. Düğümü yeniden başlatmak, düğümüzerinde görev zamanlamasını da sağlar.
> 
> 

### <a name="install-task-application-packages"></a>Görev uygulama paketlerini yükleme
Havuza benzer şekilde, görev için uygulama paketi *başvurularını* belirtirsiniz. Bir görev düğüm üzerinde çalışacak şekilde zamanlandığında, paket karşıdan yüklenir ve görevin komut satırı yürütülmeden hemen önce ayıklanır. Belirtilen paket ve sürüm düğüme zaten yüklenmişse, paket indirilmez ve varolan paket kullanılır.

Görev uygulama paketini yüklemek için görevin [CloudTask'ını][net_cloudtask]yapılandırın. [ApplicationPackageReferences][net_cloudtask_pkgref] özelliği:

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

## <a name="execute-the-installed-applications"></a>Yüklenen uygulamaları yürütme
Bir havuz veya görev için belirttiğiniz paketler karşıdan yüklenir ve düğüm `AZ_BATCH_ROOT_DIR` içinde adlandırılmış bir dizine ayıklanır. Toplu iş, adlandırılmış dizine giden yolu içeren bir ortam değişkeni de oluşturur. Görev komut satırları, düğümüzerinde uygulama başvururken bu ortam değişkenini kullanır. 

Windows düğümlerinde değişken aşağıdaki biçimdedir:

```
Windows:
AZ_BATCH_APP_PACKAGE_APPLICATIONID#version
```

Linux düğümlerinde biçim biraz farklıdır. Periyotlar (.), tireler (-) ve sayı işaretleri (#) ortam değişkenindeki alt çizgiye düzleştirilmiştir. Ayrıca, uygulama kimliği durumunda korunur unutmayın. Örnek:

```
Linux:
AZ_BATCH_APP_PACKAGE_applicationid_version
```

`APPLICATIONID`ve `version` dağıtım için belirttiğiniz uygulama ve paket sürümüne karşılık gelen değerlerdir. Örneğin, uygulama *karıştırıcı* sürümü 2.7 sürümü Windows düğümleri yüklü olması gerektiğini belirttiyseniz, görev komut satırları dosyalarına erişmek için bu ortam değişkeni kullanır:

```
Windows:
AZ_BATCH_APP_PACKAGE_BLENDER#2.7
```

Linux düğümlerinde, bu biçimde ortam değişkenini belirtin. Alt çizgideki dönemleri (.), tireleri (-) ve sayı işaretlerini (#) düzleştirmek ve uygulama kimliğinin durumunu koruyun:

```
Linux:
AZ_BATCH_APP_PACKAGE_blender_2_7
``` 

Bir uygulama paketi yüklediğinizde, bilgi işlem düğümlerinize dağıtılabilmek için varsayılan bir sürüm belirtebilirsiniz. Bir uygulama için varsayılan bir sürüm belirttiyseniz, uygulamaya başvururken sürüm sonekini atlayabilirsiniz. Varsayılan uygulama sürümünü Azure portalında, **Uygulamalar** penceresinde, Yükleme ve [uygulamaları yönetmede](#upload-and-manage-applications)gösterildiği gibi belirtebilirsiniz.

Örneğin, "2.7"yi uygulama *karıştırıcıiçin*varsayılan sürüm olarak ayarlarsanız ve görevleriniz aşağıdaki ortam değişkenine başvurursa, Windows düğümleriniz sürüm 2.7'yi çalıştırır:

`AZ_BATCH_APP_PACKAGE_BLENDER`

Aşağıdaki kod snippet *blender* uygulamasının varsayılan sürümünü başlatan bir örnek görev komut satırı gösterir:

```csharp
string taskId = "blendertask01";
string commandLine =
    @"cmd /c %AZ_BATCH_APP_PACKAGE_BLENDER%\blender.exe -args -here";
CloudTask blenderTask = new CloudTask(taskId, commandLine);
```

> [!TIP]
> Bilgi işlem düğümü ortamı ayarları hakkında daha fazla bilgi için [Toplu İşlem özelliğindeki](batch-api-basics.md) [görevler için Ortam ayarlarına](batch-api-basics.md#environment-settings-for-tasks) bakın.
> 
> 

## <a name="update-a-pools-application-packages"></a>Bir havuzun uygulama paketlerini güncelleştirme
Varolan bir havuz zaten bir uygulama paketiyle yapılandırıldıysa, havuz için yeni bir paket belirtebilirsiniz. Havuz için yeni bir paket başvurusu belirtirseniz, aşağıdakileri uygulayın:

* Toplu İşlem hizmeti, yeni belirtilen paketi havuza katılan tüm yeni düğümlere ve yeniden başlatılan veya yeniden görüntülenen varolan düğümlere yükler.
* Paket başvurularını güncellediğinizde zaten havuzda bulunan işlem düğümleri yeni uygulama paketini otomatik olarak yüklemez. Bu işlem düğümleri yeniden başlatılmalıdır veya yeni paketi almak için yeniden görüntülenmeli.
* Yeni bir paket dağıtıldığında, oluşturulan ortam değişkenleri yeni uygulama paketi başvurularını yansıtır.

Bu örnekte, varolan havuz, [CloudPool][net_cloudpool]biri olarak yapılandırılan *blender* uygulamasının sürüm 2.7 vardır. [UygulamaPaket Referanslar][net_cloudpool_pkgref]. Havuzun düğümlerini sürüm 2.76b ile güncelleştirmek için, yeni sürümle birlikte yeni bir [ApplicationPackageReference][net_pkgref] belirtin ve değişikliği gerçekleştirin.

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

Şimdi yeni sürüm yapılandırıldı, Toplu hizmet havuzu birleştiren herhangi bir *yeni* düğüm sürüm 2.76b yükler. Havuzda *bulunan* düğümlere 2.76b yüklemek için yeniden başlatın veya yeniden görüntüleyin. Yeniden başlatılan düğümlerin önceki paket dağıtımlarından dosyaları koruduğunu unutmayın.

## <a name="list-the-applications-in-a-batch-account"></a>Uygulamaları Toplu İşlem hesabında listelama
Uygulamaları ve paketlerini Bir Toplu İşlem hesabında [ApplicationOperations'ı][net_appops]kullanarak listeleyebilirsiniz. [ListApplicationSummaries][net_appops_listappsummaries] yöntemi.

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

## <a name="wrap-up"></a>Sarın
Uygulama paketleriyle, müşterilerinizin işleri için uygulamaları seçmelerine yardımcı olabilir ve Toplu Iş özellikli hizmetinizle işleri işlerken kullanılacak tam sürümü belirtebilirsiniz. Ayrıca, müşterilerinizin hizmetinizde kendi uygulamalarını yükleme ve izleme olanağı da sağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Toplu REST API][api_rest] ayrıca uygulama paketleri yle çalışmak için destek sağlar. Örneğin, REST API'sini kullanarak yükleyecek paketleri nasıl belirteceğimiz hakkında bilgi almak için [bir havuza ekle'deki][rest_add_pool] [uygulamaPackageReferences][rest_add_pool_with_packages] öğesine bakın. Toplu REST API'sini kullanarak uygulama bilgilerinin nasıl alınabildiğini hakkında ayrıntılar için [uygulamalara][rest_applications] bakın.
* Toplu İşlem Yönetimi [.NET ile Azure Toplu İş hesaplarını ve kotalarını](batch-management-dotnet.md)programlı olarak nasıl yöneteceklerini öğrenin. [Toplu İşlem Yönetimi .NET][api_net_mgmt] kitaplığı, Toplu Işlem uygulamanız veya hizmetiniz için hesap oluşturma ve silme özelliklerini etkinleştirebilir.

[api_net]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/client?view=azure-dotnet
[api_net_mgmt]: https://docs.microsoft.com/dotnet/api/overview/azure/batch/management?view=azure-dotnet
[api_rest]: https://docs.microsoft.com/rest/api/batchservice/
[batch_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[github_samples]: https://github.com/Azure/azure-batch-samples
[storage_pricing]: https://azure.microsoft.com/pricing/details/storage/
[net_appops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.aspx
[net_appops_listappsummaries]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationoperations.listapplicationsummaries.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.applicationpackagereferences.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.aspx
[net_cloudtask_pkgref]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudtask.applicationpackagereferences.aspx
[net_nodestate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.state.aspx
[net_pkgref]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.applicationpackagereference.aspx
[portal]: https://portal.azure.com
[rest_applications]: https://msdn.microsoft.com/library/azure/mt643945.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_pool_with_packages]: https://msdn.microsoft.com/library/azure/dn820174.aspx#bk_apkgreference

[1]: ./media/batch-application-packages/app_pkg_01.png "Uygulama paketleri üst düzey diyagram"
[2]: ./media/batch-application-packages/app_pkg_02.png "Azure portalında uygulamalar döşemesi"
[3]: ./media/batch-application-packages/app_pkg_03.png "Azure portalında uygulamalar bıçak"
[4]: ./media/batch-application-packages/app_pkg_04.png "Azure portalında uygulama ayrıntıları bıçak"
[5]: ./media/batch-application-packages/app_pkg_05.png "Azure portalında yeni uygulama bıçağı"
[7]: ./media/batch-application-packages/app_pkg_07.png "Azure portalında açılan paketleri güncelleştirme veya silme"
[8]: ./media/batch-application-packages/app_pkg_08.png "Azure portalında yeni uygulama paketi bıçağı"
[9]: ./media/batch-application-packages/app_pkg_09.png "Bağlantılı Depolama hesabı uyarısı yok"
[10]: ./media/batch-application-packages/app_pkg_10.png "Azure portalında depolama hesabı kılıcını seçin"
[11]: ./media/batch-application-packages/app_pkg_11.png "Azure portalında paket bıçağını güncelleştirin"
[12]: ./media/batch-application-packages/app_pkg_12.png "Azure portalında paket onay iletişim kutusunu silme"
[13]: ./media/batch-application-packages/package-file-structure.png "Azure portalında düğüm bilgilerini hesaplama"
[14]: ./media/batch-application-packages/package-file-structure-node.png "Azure portalında görüntülenen işlem düğümündeki dosyalar"
