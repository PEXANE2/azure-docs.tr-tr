---
title: Azure Dosya Eşitleme 'e StorSimple 8000 serisi geçişi
description: StorSimple 8100 veya 8600 gerecini Azure Dosya Eşitleme 'e geçirmeyi öğrenin.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: d937852ace8d9bf39495f1fdd92e6edfc4452a0a
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78943598"
---
# <a name="storsimple-8100-and-8600-migration-to-azure-file-sync"></a>StorSimple 8100 ve 8600 Azure Dosya Eşitleme 'e geçiş

StorSimple 8000 serisi, 8100 ya da 8600 fiziksel, şirket içi gereçlerden ve bulut hizmeti bileşenleriyle temsil edilir. Bu gereçlerden birindeki verileri bir Azure Dosya Eşitleme ortamına geçirmek mümkündür. Azure Dosya Eşitleme, StorSimple gereçlerinin geçirilebilmesi için varsayılan ve stratejik, uzun süreli Azure hizmetidir.

StorSimple 8000 serisi, adım 2022 ' de [yaşam sonuna](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%208000%20Series) ulaşacaktır. Geçişinizi mümkün olan en kısa sürede planlamaya başlamak önemlidir. Bu makalede, Azure Dosya Eşitleme başarılı bir geçiş için gereken arka plan bilgisi ve geçiş adımları sağlanmaktadır. 

## <a name="azure-file-sync"></a>Azure Dosya Eşitleme

> [!IMPORTANT]
> Microsoft, müşterilerine kendi geçişlerinde yardımcı olmaya kararlıdır. Özelleştirilmiş bir geçiş planı için e-posta AzureFilesMigration@microsoft. com ve geçiş sırasında yardım.

Azure Dosya Eşitleme, iki ana bileşeni temel alan bir Microsoft bulut hizmetidir:

* Dosya eşitleme ve bulut katmanlama.
* Azure 'da, SMB ve dosya geri kalanı gibi birden çok protokol üzerinden erişilebilen yerel depolama olarak dosya paylaşımları. Azure dosya paylaşımının bir Windows Server üzerindeki dosya paylaşımıyla karşılaştırılabilen bir ağ sürücüsü olarak yerel olarak takabilirsiniz. Öznitelikler, izinler ve zaman damgaları gibi önemli dosya uygunluk yönlerini destekler. Azure dosya paylaşımları ile, artık bulutta depolanan dosya ve klasörleri yorumlamak için bir uygulama veya hizmet gereksinimi yoktur. Bunlara Windows Dosya Gezgini gibi tanıdık protokoller ve istemciler üzerinden yerel olarak erişebilirsiniz. Bu, Azure dosya 'yı ideal ve genel amaçlı dosya sunucusu verilerinin yanı sıra, bazı uygulama verilerini bulutta depolamak için en esnek yaklaşımdan daha geniş bir yaklaşım sağlar.

Bu makalede geçiş adımları ele alınmaktadır. Azure Dosya Eşitleme hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makaleleri öneririz:

* [Azure Dosya Eşitleme-genel bakış](https://aka.ms/AFS "Genel Bakış")
* [Azure Dosya Eşitleme dağıtım kılavuzu](storage-sync-files-deployment-guide.md)

## <a name="migration-goals"></a>Geçiş hedefleri

Amaç, üretim verilerinin bütünlüğünü güvence altına almak ve kullanılabilirliği güvence altına almak sağlamaktır. İkinci olarak, kapalı kalma süresinin en az bir süre içinde tutulması gerekir, böylece normal bakım pencereleri içine sığacak veya yalnızca biraz daha fazla olabilir.

## <a name="storsimple-8000-series-migration-path-to-azure-file-sync"></a>Azure Dosya Eşitleme için StorSimple 8000 serisi geçiş yolu

Azure Dosya Eşitleme aracısını çalıştırmak için yerel bir Windows Server gereklidir. Windows Server en az bir 2012R2 sunucusu olabilir, ancak ideal bir Windows Server 2019 ' dir.

Birçok farklı geçiş yolu vardır ve bunların tümünü belgelemek ve bu makalede en iyi yöntem olarak önerdiğimiz yol üzerinde neden risk veya olumsuz yönleri oluşturduğunu göstermek için çok uzun bir makale oluşturur.

![StorSimple 8000 serisi geçiş aşamaları genel bakış](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-overview.png "StorSimple 8000 serisi geçiş rotası Bu makalede aşağıdaki evreler hakkında genel bakış.")

Önceki görüntüde, bu makaledeki bölümlere karşılık gelen aşamalar gösterilmektedir.
Dosyaları yerel StorSimple gerecinize gereksiz şekilde geri çekmeyi önlemek için bulut tarafı geçişi kullanıyoruz. Bu yaklaşım, yerel önbelleğe alma davranışının veya ağ bant genişliği kullanımını etkileyerek, bunlardan biri de üretim iş yüklerinizi etkileyebilir.
Bulut tarafında geçiş bir anlık görüntü (bir birim kopyası) üzerinde çalışıyor. Bu nedenle, geçiş işleminin sonunda, üretim verileriniz bu işlemden yalıtılarak kesilmez. Aslında bir yedekleme işlemi dışında çalışmak, geçişin güvenli ve kolayca tekrarlanabilir olmasını, herhangi bir zorluklara sahip olmanızı sağlar.

## <a name="considerations-around-existing-storsimple-backups"></a>Mevcut StorSimple yedeklemeleriyle ilgili konular

StorSimple, birim klonları biçiminde yedeklemeleri almanıza olanak sağlar. Bu makalede, canlı dosyalarınızı geçirmek için yeni bir birim kopyası kullanılmaktadır.
Canlı verilerinize ek olarak yedeklemeleri geçirmeniz gerekiyorsa, bu makaledeki tüm rehberlik yine de geçerlidir. Tek fark, yeni bir birim kopyalama ile başlamak yerine, geçirmeniz gereken en eski yedekleme birimi kopyası ile başlayacaksınız.

Sıra aşağıdaki gibidir:

* Geçirmeniz gereken en düşük birim kopyaları kümesini belirleyin. Daha uzun bir süre önce geçiş işlemi yaptığınız için daha fazla yedekleme yapılacağından, bu listenin en az olması mümkün olursa en az bir değer tutmanız önerilir.
* Geçiş işlemini ilerleyecekseniz, geçirmek istediğiniz en eski birim kopyasına ve sonraki bir geçişe başlayın, sonraki en eskileri kullanın.
* Her birim kopyalama geçişi tamamlandığında, bir Azure dosya paylaşımının anlık görüntüsünü almalısınız. Azure [dosya paylaşımı anlık görüntüleri](storage-snapshots-files.md) , Azure dosya paylaşımlarınız için dosya ve klasör yapısının zaman içindeki yedeklemelerini nasıl tutacağınıza yöneliktir. Geçiş tamamlandıktan sonra, geçiş işlemi sırasında ilerleme yaptığınız her birim klonlarınızın korunan sürümlerini kullandığınızdan emin olmak için, geçiş tamamlandıktan sonra bu anlık görüntülere ihtiyacınız olacak.
* Aynı StorSimple birimi tarafından sunulan tüm Azure dosya paylaşımları için Azure dosya paylaşımı anlık görüntülerini aldığınızdan emin olun. Birim klonları birim düzeyindedir; Azure dosya paylaşımının anlık görüntüleri, paylaşma düzeyindedir. Bir birim kopyasının geçirilmesi tamamlandıktan sonra bir paylaşılan anlık görüntü (her Azure dosya paylaşımında) almanız gerekir.
* Bir birim kopyası için geçiş işlemini yineleyin ve canlı verilerin bir anlık görüntüsüne ulaşana kadar her birim Klondan sonra paylaşma anlık görüntülerini alın. Bir birim kopyasının geçirilmesi işlemi aşağıdaki aşamalarda açıklanmıştır. 

Yedeklemeleri hiç taşımanız gerekmiyorsa ve yalnızca canlı verilerin geçişi yapıldıktan sonra Azure dosya paylaşımında yeni bir yedekleme zinciri başlatabilir, geçiş sırasında karmaşıklığı azaltmak ve geçişin yapılacağı süre miktarını azaltmak yararlı olur. Her bir birim için (her bir paylaşıma değil) StorSimple içinde yedeklerin taşınmasının ve taşınmayacağı kararı verebilirsiniz.

## <a name="phase-1-get-ready"></a>1\. Aşama: hazırlanın

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan, daha önce, genel bakış resminin bir parçasını gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-1.png)
    :::column-end:::
    :::column:::
        Geçişin temeli, StorSimple 8020 adlı bir birim kopyası ve sanal bulut gerecine yöneliktir.
Bu aşama, Azure 'da bu kaynakların dağıtımına odaklanır.
    :::column-end:::
:::row-end:::

### <a name="deploy-a-storsimple-8020-virtual-appliance"></a>StorSimple 8020 Sanal Gereç dağıtma

Bulut gerecinin dağıtımı, güvenlik, ağ ve diğer birkaç dikkat gerektiren bir işlemdir.

> [!IMPORTANT]
> Aşağıdaki kılavuzda bazı gereksiz bölümler yer almaktadır. "Adım 3" tarihine kadar başlayan makaleyi okuyun ve takip edin. Ardından bu makaleye geri dönün. Şu anda "Adım 3" veya bu kılavuzun ötesinde herhangi bir şeyi gerçekleştirmeniz gerekmez.

[StorSimple 8020 Sanal Gereç dağıtımı](../../storsimple/storsimple-8000-cloud-appliance-u2.md)

### <a name="determine-a-volume-clone-to-use"></a>Kullanılacak birim klonu belirleme

Geçişe başlamaya hazırsanız ilk adım, yedekleme için yaptığınız gibi, StorSimple bulut depolamanın geçerli durumunu yakalayan yeni bir birim kopyası hazırlanmalıdır. Sahip olduğunuz StorSimple birimlerinin her biri için bir kopya alın.
Yedeklemeleri taşımaya ihtiyaç duyuyorsanız, kullandığınız ilk birim kopyası yeni oluşturulmuş bir kopya değildir ancak geçirmeniz gereken en eski birim kopyası (en eski yedekleme) değildir.
Ayrıntılı rehberlik için ["mevcut StorSimple yedeklemelerine Ilişkin önemli noktalar"](#considerations-around-existing-storsimple-backups) bölümüne bakın.

> [!IMPORTANT]
> Aşağıdaki kılavuzda bazı gereksiz bölümler yer almaktadır. Yalnızca bağlantılı bölümde özetlenen adımları okuyun ve izleyin. Ardından bu makaleye geri dönün. "Sonraki adımlar" bölümünü izlemeniz gerekmez.

[Birimin kopyasını oluşturma](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)

### <a name="use-the-volume-clone"></a>Birim klonu kullanma

1\. aşamanın son aşaması, seçtiğiniz birim kopyasını Azure 'daki 8020 sanal gereci üzerinde kullanıma hazır hale getirmektir.

> [!IMPORTANT]
> Aşağıdaki kılavuzda, birimi biçimlendirmek için gereken adımlar ve ayrıca son bir yönerge yer almaktadır. **BIRIMI biçimlendirmeyin** "10" yönergesinden başlayarak, "10. Basit bir birimi biçimlendirmek için... "  Bu adımı takip etmeden önce durdurun ve bu makaleye geri dönün.

[Azure 'da 8020 sanal gerecine bir birim kopyası bağlama](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-1-summary"></a>Aşama 1 Özeti

1\. aşama tamamlandığınıza göre şunları tamamladınız:

* Azure 'da StorSimple 8020 sanal gereci dağıtıldı.
* Hangi birim kopyasının geçişe başlayacağınızı belirler.
* Birim kopyalarınızı (her canlı birim için bir tane) Azure 'daki StorSimple Sanal gerecine, verileri daha fazla kullanım için kullanılabilir olacak şekilde bağladı.

## <a name="phase-2-cloud-vm"></a>2\. Aşama: bulut VM 'si

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan, daha önce, genel bakış resminin bir parçasını gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-2.png)
    :::column-end:::
    :::column:::
        Azure 'daki StorSimple 8020 sanal gereci üzerinde ilk kopyam kullanıma sunulduktan sonra, bir VM sağlama ve bu VM 'nin Iscsı üzerinden birim kopyasını (veya birden çok kez) kullanıma sunma süresi artık sürüyor.
    :::column-end:::
:::row-end:::

### <a name="deploy-an-azure-vm"></a>Azure VM dağıtma

Azure 'daki Windows Server sanal makinesi, yalnızca geçiş sırasında gerekli olan geçici bir altyapı parçası olan StorSimple 8020 gibidir.
Dağıttığınız VM 'nin yapılandırması, genellikle eşitlendirilecektir öğe sayısına (dosya ve klasör) göre değişir. Herhangi bir endişeniz varsa daha yüksek performans yapılandırması yapmanızı öneririz.

Tek bir Windows Server, en fazla 30 Azure dosya paylaşımını eşitleyebilir.
Her paylaşımın/yolun, StorSimple biriminin köküne göre veya öğelerin (dosyalar ve klasörler) sayısını kapsayacak şekilde seçim yapmanıza karar verirsiniz.

Verilerin genel boyutu bir darboğazdan daha düşüktür; makine özelliklerini uyarlamak için gereken öğe sayısıdır.

* [Eşitleme yapmanız gereken öğe sayısına (dosya ve klasör) göre bir Windows Server 'ın nasıl boyutlandıralınacağını öğrenin.](storage-sync-files-planning.md#recommended-system-resources)
* [Windows Server VM 'yi dağıtmayı öğrenin.](../../virtual-machines/windows/quick-create-portal.md)

> [!IMPORTANT]
> VM 'nin StorSimple 8020 sanal gereci ile aynı Azure bölgesinde dağıtıldığından emin olun. Bu geçişin bir parçası olarak, bulut verilerinizin bölgesini bugün içinde depolandığı bölgeden değiştirmeniz de gerekir. bu işlemi Azure dosya paylaşımlarını sağladığınızda daha sonraki bir adımda yapabilirsiniz.

### <a name="expose-the-storsimple-8020-volumes-to-the-vm"></a>StorSimple 8020 birimlerini VM 'de kullanıma sunma

Bu aşamada, Iscsı üzerinden 8020 sanal gerecinden bir veya birkaç StorSimple birimini sağladığınız Windows Server VM 'sine bağlanıyorsunuz.

> [!IMPORTANT]
> Aşağıdaki makalelerde, **bulut gereci için yalnızca özel IP al** ' ı doldurun ve Iscsı bölümlerine **bağlanın** ve bu makaleye geri dönün.

1. [Bulut gereci için özel IP al](../../storsimple/storsimple-8000-cloud-appliance-u2.md#get-private-ip-for-the-cloud-appliance)
2. [Iscsı üzerinden Bağlan](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-2-summary"></a>2\. aşama Özeti

2\. aşama 'u tamamladığınıza göre şunları yaptınız: 

* 8020 sanal StorSimple gereci ile aynı bölgede bir Windows Server VM 'si sağlandı
* 8020 ' dan tüm uygulanabilir birimler, Iscsı üzerinden Windows Server VM 'sine kullanıma sunuldu.
* Artık bağlı birimlerdeki sunucu sanal makinesinde dosya Gezgini 'ni kullandığınızda dosya ve klasör içeriğini görmeniz gerekir.

Yalnızca geçiş gerektiren tüm birimler için bu adımları tamamladığınızda 3. aşama 'e ilerleyin.

## <a name="phase-3-set-up-azure-file-shares-and-get-ready-for-azure-file-sync"></a>3\. Aşama: Azure dosya paylaşımlarını ayarlama ve Azure Dosya Eşitleme hazırlanın

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan, daha önce, genel bakış resminin bir parçasını gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-3.png)
    :::column-end:::
    :::column:::
        Bu aşamada, bir dizi Azure dosya paylaşımı belirleyerek, şirket içinde bir StorSimple gereci değişikliği olarak bir Windows Server oluşturup bu sunucuyu Azure Dosya Eşitleme için yapılandıracaksınız. 
    :::column-end:::
:::row-end:::

### <a name="map-your-existing-namespaces-to-azure-file-shares"></a>Mevcut ad alanlarınızı Azure dosya paylaşımlarına eşleyin

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="deploy-azure-file-shares"></a>Azure dosya paylaşımlarını dağıtma

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

> [!TIP]
> Azure bölgesini StorSimple verilerinizin bulunduğu geçerli bölgeden değiştirmeniz gerekiyorsa, Azure dosya paylaşımlarını kullanmak istediğiniz yeni bölgede sağlayın. Azure dosya paylaşımlarınızı tutan depolama hesaplarını sağladığınızda bölgeyi seçerek belirlersiniz. Ayrıca, sağlayacağınız Azure Dosya Eşitleme kaynağının aynı, yeni bölge olduğundan emin olun.

### <a name="deploy-the-azure-file-sync-cloud-resource"></a>Azure Dosya Eşitleme bulut kaynağını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

> [!TIP]
> StorSimple verilerinizin bulunduğu geçerli bölgeden Azure bölgesini değiştirmeniz gerekiyorsa, yeni bölgedeki Azure dosya paylaşımlarınız için depolama hesapları sağlamış olursunuz. Bu depolama eşitleme hizmeti 'ni dağıtırken aynı bölgeyi seçtiğinizden emin olun.

### <a name="deploy-an-on-premises-windows-server"></a>Şirket içi Windows Server dağıtma

* Bir sanal makine veya fiziksel sunucu olarak en az 2012R2 ' de bir Windows Server 2019 oluşturun. Windows Server yük devretme kümesi de desteklenir. StorSimple 8100 veya 8600 ' den daha fazla işlem yapmış olabileceğiniz sunucuyu yeniden kullanmayın.
* Doğrudan bağlı depolama alanı sağlayın veya ekleyin (desteklenmeyen bir şekilde, NAS ile karşılaştırıldığında).

Yeni Windows Server 'a, StorSimple 8100 veya 8600 gerecinin önbelleğe alma için yerel olarak kullanılabilir olandan eşit veya daha büyük bir depolama miktarı vermesi en iyi uygulamadır. Windows Server 'ı StorSimple gerecini kullandığınız şekilde kullanırsınız. Bu, Gereç ile aynı miktarda depolama alanına sahipse, aynı değilse önbelleğe alma deneyiminin benzer olması gerekir.
Windows Server 'dan ' de depolama ekleyebilir veya depolama alanını kaldırabilirsiniz. Bu, yerel birim boyutunuzu ve önbelleğe alma için kullanılabilir yerel depolama miktarını ölçeklendirmenizi sağlar.

### <a name="prepare-the-windows-server-for-file-sync"></a>Windows Server 'ı dosya eşitleme için hazırlama

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

### <a name="configure-azure-file-sync-on-the-windows-server"></a>Windows Server 'da Azure Dosya Eşitleme yapılandırma

Kayıtlı şirket içi Windows Server, bu işlem için önceden ve internet 'e bağlı olmalıdır.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!WARNING]
> **Bulut katmanlaması 'nı etkinleştirdiğinizden emin olun!** Bulut katmanlaması, yerel sunucunun bulutta depolankıyasla daha az depolama kapasitesine sahip olmasına izin veren, ancak tam ad alanı kullanılabilir olan AFS özelliğidir. Yerel olarak ilginç veriler de hızlı, yerel erişim performansı için yerel olarak önbelleğe alınır. Bu adımda bulut katmanlamayı açmak için başka bir nedenden dolayı, bu aşamada dosya içeriğini eşitlemek istemiyorum, yalnızca ad alanı şu anda taşınıyor.

## <a name="phase-4-configure-the-azure-vm-for-sync"></a>4\. Aşama: Azure VM 'yi eşitleme için yapılandırma

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan, daha önce, genel bakış resminin bir parçasını gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-4.png)
    :::column-end:::
    :::column:::
        Bu aşama, Iscsı bağlı, ilk birim kopyalarıyla Azure VM 'niz ile ilgilidir. Bu aşamada, sanal makineyi Azure Dosya Eşitleme aracılığıyla bağlanır ve dosyaları StorSimple birim kopyalarınızın ilk bir turunda başlatın.
        
    :::column-end:::
:::row-end:::

Azure Dosya Eşitleme için StorSimple 8100 veya 8600 gerecinizi değiştirecek şirket içi sunucunuzu zaten yapılandırmış olmanız gerekir. 

Azure VM 'nin yapılandırılması, tek bir ek adımla birlikte neredeyse özdeş bir işlemdir. Aşağıdaki adımlar süreç boyunca size yol gösterecektir.

> [!IMPORTANT]
> Azure VM 'nin **bulut katmanlaması etkinleştirilmiş olarak yapılandırılmamış** olması önemlidir! Bu sunucunun birimini geçiş boyunca daha yeni birim klonlarla değiş tokuş edersiniz. Bulut katmanlaması, CPU kullanımı için avantaj ve ek yük içermez.

1. [AFS aracısını dağıtın. (önceki bölüme bakın)](#prepare-the-windows-server-for-file-sync)
2. [Azure Dosya Eşitleme için VM hazırlanıyor.](#get-the-vm-ready-for-azure-file-sync)
3. [Eşitlemeyi yapılandır](#configure-azure-file-sync-on-the-azure-vm)

### <a name="get-the-vm-ready-for-azure-file-sync"></a>VM 'yi Azure Dosya Eşitleme için hazırlayın

Azure Dosya Eşitleme, dosyaları bağlı Iscsı StorSimple birimlerinden hedef Azure dosya paylaşımlarına taşımak için kullanılır.
Bu geçiş işlemi sırasında, VM 'nize aynı sürücü harfi altında birkaç birim klonu bağlayacaksınız. Azure Dosya Eşitleme, dosya ve klasörlerin daha yeni bir sürümü olarak bağladığınız bir sonraki birim kopyasını görmek ve Azure Dosya Eşitleme ile bağlantılı Azure dosya paylaşımlarını güncelleştirmek için yapılandırılmalıdır. 

> [!IMPORTANT]
> Bunun çalışması için Azure Dosya Eşitleme yapılandırılmadan önce sunucuda bir kayıt defteri anahtarı ayarlanmalıdır.

1. VM 'nin sistem sürücüsünde yeni bir dizin oluşturun. Azure Dosya Eşitleme bilgilerin bağlı birim klonları yerine bu şekilde kalıcı olması gerekir. Örneğin, `"C:\syncmetadata"`
2. Regedit ' i açın ve aşağıdaki kayıt defteri kovanını bulun: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync`
3. Dize türünde, ***Metadatarootpath*** adlı yeni bir anahtar oluşturun
4. Sistem biriminde oluşturduğunuz dizinin tam yolunu ayarlayın, örneğin: `C:\syncmetadata"`

### <a name="configure-azure-file-sync-on-the-azure-vm"></a>Azure VM 'de Azure Dosya Eşitleme yapılandırma

Bu adım, şirket içi sunucuda AFS 'yi nasıl yapılandıracağınızı ele alan önceki bölüme benzer.

Farkı, bu sunucuda bulut katmanlamayı etkinleştirmemelidir ve doğru klasörlerin doğru Azure dosya paylaşımlarına bağlı olduğundan emin olmanız gerekir. Aksi takdirde, Azure dosya paylaşımlarınız ve veri içeriğiyle eşleşmemelidir ve eşitleme yeniden yapılandırmadan bulut kaynaklarını veya yerel klasörleri yeniden adlandırmanın bir yolu yoktur.

[Windows Server 'da Azure dosya eşitleme yapılandırma hakkında önceki bölüme](#configure-azure-file-sync-on-the-windows-server)bakın.

### <a name="step-4-summary"></a>4\. adım Özeti

Bu noktada, Azure VM 'de Azure Dosya Eşitleme başarıyla yapılandırdığınıza sonra StorSimple birim kopyalarınızı Iscsı aracılığıyla bağlacaksınız.
Veriler artık Azure VM 'den çeşitli Azure dosya paylaşımlarına akar ve şirket içi Windows Server 'da tam bir yorgun ad alanı görüntülenir.

> [!IMPORTANT]
> Şu anda Windows Server 'a yapılan hiçbir değişiklik veya Kullanıcı erişimi olmadığından emin olun.

Azure VM 'den Azure dosya paylaşımlarına taşınan ilk birim kopyalama verileri uzun sürebilir, potansiyel olarak hafta olabilir. Bu işlem için gereken süreyi tahmin etmek ve birçok faktöre bağlıdır. En yaygın olarak, Azure VM 'nin StorSimple birimlerindeki dosyalara erişme hızı ve eşitleme gerektiren dosya ve klasörleri Azure Dosya Eşitleme hızlı bir şekilde nasıl işleyebileceği gösterilmektedir. 

Deneyimden itibaren bant genişliğinin bu nedenle gerçek veri boyutunun bir alt rolü yürüttüğünde olduğunu varsayabiliriz. Bu veya sonraki geçiş yuvarlaklığının çoğu zaman, saniye başına işlenebilecek öğe sayısına bağlıdır. Örneğin, 100.000 dosya ve klasör içeren 1 TiB, yalnızca 50.000 dosya ve klasörleriyle en büyük olasılıkla 1 TiB 'den daha yavaş bitecektir.

## <a name="phase-5-iterate-through-multiple-volume-clones"></a>5\. Aşama: birden çok birim klonlarını yineleme

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan, daha önce, genel bakış resminin bir parçasını gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-5.png)
    :::column-end:::
    :::column:::
        Önceki aşamada anlatıldığı gibi, ilk eşitleme uzun zaman alabilir. Kullanıcılarınız ve uygulamalarınız hala şirket içi StorSimple 8100 veya 8600 gerecine erişiyor. Diğer bir deyişle, değişiklikler birikmekte ve her gün canlı veriler ile ilk birim kopyası arasında daha büyük bir Delta ile Şu anda geçişiniz, formlardır. Bu bölümde, birden fazla birim klonu kullanarak kapalı kalma süresini en aza indirme ve eşitlemenin ne zaman yapıldığını öğrenirsiniz.
    :::column-end:::
:::row-end:::

Ne yazık ki geçiş işlemi anında olmaz. Diğer bir deyişle, canlı veriler için belirtilen tüm Delta, kaçınılmaz bir sonucudur. İyi haber, yeni birim klonlarını bağlama işlemini tekrarlamanız olabilir. Her birim kopyasının Delta 'i giderek küçülecektir. Bu nedenle, son olarak, kullanıcıların ve uygulamaların şirket içi Windows Server 'ınızla kesilmesini sağlamak üzere çevrimdışı duruma alınması için kabul edilebilir bir süre içinde zaman uyumlu olacak.

Eşitleme, kullanıcıları ve uygulamaları çevrimdışına almak için yeterince hızlı bir süre tamamlanana kadar aşağıdaki adımları yineleyin:

1. [Belirli bir birim kopyası için Eşitlemenin tamamlandığını belirleme.](#determine-when-sync-is-done)
2. [Yeni bir birim klonları alın ve bunu 8020 sanal gerecine bağlayın.](#the-next-volume-clones)
3. [Eşitlemenin ne zaman yapılacağını belirleme.](#determine-when-sync-is-done)
4. [Kesme stratejisi](#cut-over-strategy)

### <a name="the-next-volume-clones"></a>Sonraki birim klonları

Bu makalenin önceki kısımlarında bir birim klonları ele alınıyor.
Bu aşamada iki eylem vardır:

1. [Birim kopyası alma](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)
2. [Birim kopyasının bağlama (yukarıya bakın)](#use-the-volume-clone)

### <a name="determine-when-sync-is-done"></a>Eşitlemenin ne zaman yapıldığını belirleme

Eşitleme tamamlandığında zaman ölçülerinizi durdurabilir ve bir birim kopyası alma ve bağlama sürecini yinelemenize gerek olup olmadığını veya son birim kopyalama ile geçen zaman eşitlemenin yeterince küçük olup olmadığını belirleyebilirsiniz.

Eşitlemenin tamamlandığını öğrenmek için:

1. Olay Görüntüleyicisi açın ve **uygulamalar ve hizmetler** ' e gidin
2. **Microsoft\filesync\fik\telemetri** ' i gezin ve açın
3. Tamamlanan bir eşitleme oturumuna karşılık gelen en son **olay 9102**' i arayın
4. **Ayrıntılar** ' ı seçin ve **Syncdirection** değerinin **karşıya** yüklendiğini onaylayın
5. **HRESULT** 'yi denetleyin ve **0**olduğunu onaylayın. Bu, eşitleme oturumunun başarılı olduğu anlamına gelir. HResult sıfır olmayan bir değer ise eşitleme sırasında bir hata oluştu. **Peritemerrorcount** değeri 0 ' dan büyükse, bazı dosyalar veya klasörler düzgün şekilde eşitlenmez. 0 ' dan büyük olan bir PerItemErrorCount değeri 0 ' dan büyük bir HResult olması mümkündür. Bu noktada, PerItemErrorCount için endişelenmeniz gerekmez. Bu dosyaları daha sonra yakalayacağız. Bu hata sayısı önemli, binlerce öğe ise müşteri desteğine başvurun ve en iyi, sonraki aşamalar hakkında doğrudan rehberlik için Azure Dosya Eşitleme ürün grubuna bağlanmasını isteyin.
6. Bir satırda HResult 0 olan birden çok 9102 olayını görmek için işaretleyin. Bu, bu birim kopyası için Eşitlemenin tamamlandığını gösterir.

### <a name="cut-over-strategy"></a>Kesme stratejisi

1. Bir birim kopyasının eşitlemenin şimdi yeterince hızlı olup olmadığını belirleme. (Delta az, yeterince küçük.)
2. StorSimple gerecini çevrimdışına alın.
3. Son RoboCopy.

Saati ölçün ve son birim Klondan eşitlemenin, bir zaman penceresinde, yeterince küçük bir süre biteceğini ve sisteminizde kapalı kalma süresi olarak uygun olup olmadığını saptayın.

Şimdi, StorSimple gerecine Kullanıcı erişimini devre dışı bırakmıştır. Başka değişiklik yok. Kapalı kalma süresi başladı.
Gereci çevrimiçi bırakmanız ve bağlanmamanız gerekir, ancak şimdi üzerinde değişiklik yapılmasını önlemektir.

6\. aşamada, son birim Klondan bu yana canlı verilerde herhangi bir değişikliği fark edersiniz.

## <a name="phase-6-a-final-robocopy"></a>6\. Aşama: son RoboCopy

Bu noktada, şirket içi Windows Server ve StorSimple 8100 ya da 8600 gereci arasında iki fark vardır:

1. Eşitlenmemiş dosyalar olabilir (Yukarıdaki olay günlüğünden **Peritemerrors** konusuna bakın)
2. StorSimple gereci, Windows Server 'ın Şu anda yerel olarak depolanan bir dosya adı olmayan bir ad alanı ile doldurulmuş bir önbellektir.

![Makalenin bu alt bölümüne odaklanmaya yardımcı olan, daha önce, genel bakış resminin bir parçasını gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-6.png)

Windows Server önbelleğini gerecin durumuna getirebilir ve son bir RoboCopy ile hiçbir dosya ayrılmadığımızda emin olabilirsiniz.

> [!CAUTION]
> Takip ettiğiniz RoboCopy komutunun tam olarak aşağıda açıklandığı gibi olması zorunludur. Yalnızca yerel olan dosyaları ve birim kopyalama + eşitleme yaklaşımının önüne taşınmayan dosyaları kopyalamak istiyoruz. Geçiş tamamlandıktan sonra, daha sonra eşitlenmedikleri sorunları çözebiliriz. (Bkz. [Azure dosya eşitleme sorun giderme](storage-sync-files-troubleshoot.md#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing). Dosya adlarında, silindiklerinde kaçırmayabilmeniz gereken en büyük olasılıkla yazdırılabilir karakterlerdir.)

RoboCopy komutu:

```console
Robocopy /MT:32 /UNILOG:<file name> /TEE /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Arka plan

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      RoboCopy 'nin çoklu iş parçacıklı çalıştırmasına izin verir. Varsayılan 8 ' dir, Max 128 ' dir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:<file name>
   :::column-end:::
   :::column span="1":::
      GÜNLÜK dosyasının durumunu UNICODE olarak çıkış (varolan günlüğün üzerine yaz).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /T
   :::column-end:::
   :::column span="1":::
      Konsol penceresine çıkış. Günlük dosyasına çıktılarla birlikte kullanılır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MıR
   :::column-end:::
   :::column span="1":::
      Robocopy 'nin yalnızca kaynak (StorSimple gereci) ve hedef (Windows Server dizini) arasında değişimleri 'yi kabul etmesine izin verir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY: copyflag [s]
   :::column-end:::
   :::column span="1":::
      dosya kopyasının doğruluğu (varsayılan:/COPY: DAT), kopya bayrakları: D = veri, A = öznitelikler, T = zaman damgaları, S = güvenlik = NTFS ACL 'Leri, O = sahip bilgileri, U = denetim bilgileri
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      Tüm dosya bilgilerini kopyala (/COPY: DATSOU ile eşdeğer)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY: copyflag [s]
   :::column-end:::
   :::column span="1":::
      dizinlerin kopyasına uygunluk (varsayılan:/DCOPY: DA), kopyalama bayrakları: D = veri, A = öznitelikler, T = zaman damgaları
   :::column-end:::
:::row-end:::

Bu RoboCopy komutunu, Windows Server üzerindeki her bir dizin için bir Azure dosyasına dosya eşitleme ile yapılandırdığınız bir hedef olarak çalıştırmalısınız.

Bu komutların birden fazlasını paralel olarak çalıştırabilirsiniz.
Bu RoboCopy adımı tamamlandıktan sonra kullanıcılarınızın ve uygulamalarınızın, daha önce StorSimple gereci gibi Windows Server 'a erişmesine izin verebilirsiniz.

Dosyaların gerisinde bırakılmış olup olmadığını görmek için Robocopy günlük dosyalarına başvurun. Sorunların mevcut olması gerekiyorsa, çoğu durumda geçiş tamamlandıktan sonra kullanıcılar ve uygulamalarınız Windows Server 'a yeniden bağlandıktan sonra bunları çözebilirsiniz. Herhangi bir sorunu çözmeniz gerekiyorsa, bu işlemi 7. aşamada yapın.

Daha önce StorSimple verilerinde bulunan Windows Server üzerinde SMB paylaşımlarının oluşturulması gerekir. Bu adımı önyükleyebilir ve daha önce zaman kaybetmezsiniz. bu noktadan önce, Windows Server 'da dosyalarda değişiklik olmamasını sağlamalısınız.

Bir DFS-N dağıtımınız varsa, DFN-ad alanlarını yeni sunucu klasörü konumlarına işaret edebilirsiniz. Bir DFS-N dağıtımınız yoksa ve 8100 8600 gerecinizi bir Windows Server ile yerel olarak aldıysanız, bu sunucuyu etki alanı dışına alabilir ve etki alanına yeni Windows Server 'ı AFS ile etki alanına katabilirsiniz, eski sunucuyla aynı sunucu adına sahip olabilirsiniz ve aynı paylaşma adlarına sahip olacak şekilde, yeni sunucuya yönelik kesme, kullanıcılarınız, Grup ilkeniz veya betikleriniz için saydam kalır.

## <a name="phase-7-deprovision"></a>7\. Aşama: sağlamayı kaldırma

Son aşamada birden çok birim klonlarını tekrarlandırdınız ve sonunda StorSimple gereci çevrimdışı olduktan sonra yeni Windows Server 'a Kullanıcı erişimini kesebiliyordu.

Artık gereksiz kaynakları sağlamayı başlayacaksınız.
Başlamadan önce, bir bit için yeni Azure Dosya Eşitleme dağıtımınızı üretimde gözlemleyecek en iyi uygulamadır. Bu, karşılaşabileceğiniz sorunları gidermeye yönelik seçenekler sağlar.

Daha memnun olduktan sonra, AFS dağıtımınızı en az birkaç güne kadar gözlemledikten sonra kaynakları sağlamayı şu sırada yapmaya başlayabilirsiniz:

1. Birim klonlarından verileri dosya eşitleme aracılığıyla Azure dosya paylaşımlarına taşımak için kullandığımız Azure VM 'yi kapatın.
2. Azure 'daki depolama eşitleme hizmeti kaynağına gidin ve Azure VM kaydını silin. Bu, tüm eşitleme gruplarından kaldırır.

    > [!WARNING]
    > **Doğru makineyi seçtiğinizden emin olun.** Bulut sanal makinesini kapatmış oldunuz, yani kayıtlı sunucular listesinde tek bir çevrimdışı sunucu olarak gösterilmesi gerekir. Bu adımda şirket içi Windows Server 'ı seçmemelidir, bunu yapmanız bunun kaydını siler.

3. Azure VM 'yi ve kaynaklarını silin.
4. 8020 sanal StorSimple gerecini devre dışı bırakın.
5. Azure 'daki tüm StorSimple kaynaklarının sağlamasını kaldırma.
6. StorSimple fiziksel gerecini veri merkezinizden çıkarın.

Geçişiniz tamamlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosya Eşitleme hakkında daha fazla bilgi edinin. Özellikle de bulut katmanlama ilkelerinin esnekliği vardır.

Azure portal veya önceki olaylarda, bazı dosyalar kalıcı olarak eşitlenmezse, bu sorunları çözmek için sorun giderme kılavuzunu gözden geçirin.

* [Azure Dosya Eşitleme genel bakış: aka.ms/AFS](https://aka.ms/AFS)
* [Bulut katmanlama](storage-sync-cloud-tiering.md) 
* [Azure Dosya Eşitleme sorun giderme kılavuzu](storage-sync-files-troubleshoot.md)
