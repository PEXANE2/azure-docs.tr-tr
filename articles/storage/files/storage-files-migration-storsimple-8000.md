---
title: Azure Dosya Eşitlemi için StorSimple 8000 serisi geçiş
description: StorSimple 8100 veya 8600 cihazını Azure Dosya Eşitleme'ye nasıl geçirebilirsiniz öğrenin.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 03/09/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 7e5f70d0323aa5c502491ab99db303fde31ade83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528634"
---
# <a name="storsimple-8100-and-8600-migration-to-azure-file-sync"></a>Azure Dosya Eşitlemi için StorSimple 8100 ve 8600 geçişi

StorSimple 8000 serisi, 8100 veya 8600 fiziksel, şirket içi cihazlar ve bulut hizmeti bileşenleri yle temsil edilir. Bu aygıtlardan herhangi birinden verileri Azure Dosya Eşitleme ortamına geçirmek mümkündür. Azure Dosya Eşitlemi, StorSimple cihazlarının geçirilebileceği varsayılan ve stratejik uzun vadeli Azure hizmetidir.

StorSimple 8000 serisi Aralık 2022'de ömrünü niçin [sona erdirecek.](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%208000%20Series) Geçişinizi mümkün olan en kısa sürede planlamaya başlamak önemlidir. Bu makalede, Azure Dosya Eşitlemi için başarılı bir geçiş için gerekli arka plan bilgisi ve geçiş adımları sağlar. 

## <a name="azure-file-sync"></a>Azure Dosya Eşitleme

> [!IMPORTANT]
> Microsoft, müşterilerine geçişlerinde yardımcı olmayı taahhüt eder. Özelleştirilmiş bir geçiş planı nın yanı sıra geçiş sırasında yardım için .com adresine e-posta gönderin. AzureFilesMigration@microsoft

Azure Dosya Eşitlemi, iki ana bileşene dayanan bir Microsoft bulut hizmetidir:

* Dosya eşitleme ve bulut katmanlama.
* Dosya paylaşımları Azure'da yerel depolama alanı olarak paylaşArak SMB ve DOSYA REST gibi birden çok protokol üzerinden erişilebilir. Azure dosya paylaşımı, bir Windows Sunucusu'ndaki bir dosya paylaşımıyla karşılaştırılabilir ve bu dosyayı ağ sürücüsü olarak yerel olarak monte edebilirsiniz. Öznitelikler, izinler ve zaman damgaları gibi önemli dosya doğruluğu yönlerini destekler. Azure dosya paylaşımlarında, bulutta depolanan dosya ve klasörleri yorumlamak için artık bir uygulamaya veya hizmete gerek kalmaz. Bunları tanıdık protokoller ve Windows Dosya Gezgini gibi istemciler üzerinden yerel olarak erişebilirsiniz. Bu, Azure dosya paylaşımlarını genel amaçlı dosya sunucusu verilerinin yanı sıra bazı uygulama verilerini bulutta depolamak için ideal ve en esnek yaklaşım haline getirir.

Bu makalede, geçiş adımları üzerinde duruluyor. Azure Dosya Eşitlemi hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makaleleri öneririz:

* [Azure Dosya Eşitleme - genel bakış](https://aka.ms/AFS "Genel Bakış")
* [Azure Dosya Eşitleme - dağıtım kılavuzu](storage-sync-files-deployment-guide.md)

## <a name="migration-goals"></a>Geçiş hedefleri

Amaç, üretim verilerinin bütünlüğünü garanti altına almak ve kullanılabilirliği garanti etmektir. İkincisi, normal bakım pencerelerine sığabilmesi veya sadece biraz aşabilmesi için kapalı kalma süresini en aza kadar tutmayı gerektirir.

## <a name="storsimple-8000-series-migration-path-to-azure-file-sync"></a>Azure Dosya Eşitleme için StorSimple 8000 serisi geçiş yolu

Azure Dosya Eşitleme aracısını çalıştırmak için yerel bir Windows Sunucusu gereklidir. Windows Server en az bir 2012R2 sunucusu olabilir ama ideal bir Windows Server 2019 olduğunu.

Çok sayıda, alternatif geçiş yolları vardır ve bunların hepsini belgelemek ve bu makalede en iyi uygulama olarak önerdiğimiz yol üzerinde neden risk veya dezavantajlar taşıdığını göstermek için çok uzun bir makale oluşturur.

![StorSimple 8000 serisi geçiş aşamalarına genel bakış](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-overview.png "StorSimple 8000 serisi geçiş rotası bu makalede daha aşağıdaki aşamaları genel.")

Önceki resim, bu makaledeki bölümlere karşılık gelen aşamaları görüntüler.
Yerel StorSimple cihazınıza gereksiz dosyaların geri çağrılmasını önlemek için bulut tarafı geçişi kullanırız. Bu yaklaşım, üretim iş yüklerinizi etkileyebilecek yerel önbelleğe alma davranışını veya ağ bant genişliği kullanımını etkilemeyi önler.
Bulut tarafı geçişi, verilerinizin anlık görüntüsü (birim klonu) üzerinde çalışıyor. Yani üretim verileriniz bu işlemden izole edilir- geçişin sonunda kesintiye kadar. Temelde bir yedekleme ne kapalı çalışma, herhangi bir zorlukla karşı karşıya eğer geçiş güvenli ve kolayca tekrarlanabilir hale getirir.

## <a name="considerations-around-existing-storsimple-backups"></a>Varolan StorSimple yedeklemeleri ile ilgili dikkat edilecek noktalar

StorSimple, ses klonları şeklinde yedekleme yapmanızı sağlar. Bu makalede, canlı dosyalarınızı geçirmek için yeni bir birim klon kullanır.
Canlı verilerinize ek olarak yedeklemeleri geçirmeniz gerekiyorsa, bu makaledeki tüm kılavuzlar yine de geçerlidir. Tek fark, yeni bir birim klonla başlamak yerine, geçirmeniz gereken en eski yedek hacim klonunu başlatmanızdır.

Dizi aşağıdaki gibidir:

* Geçirmeniz gereken en az birim klon kümesini belirleyin. Bu listeyi mümkünse en az da tutmanızı öneririz, çünkü ne kadar çok yedekleme olursa, genel geçiş işlemi o kadar uzun sürer.
* Geçiş sürecinden geçerken, geçirmek istediğiniz en eski hacimli klonla başlayın ve sonraki her geçişte, sonraki en eski sini kullanın.
* Her birim klon geçişi tamamlandığında, bir Azure dosya paylaşımı anlık görüntüsü almanız gerekir. [Azure dosya paylaşımı anlık görüntüleri,](storage-snapshots-files.md) Azure dosya paylaşımlarınız için dosya ve klasör yapısının zamanında yedeklemelerini nasıl saklarsınız. Geçiş te ilerledikçe birim klonlarınızın her birinin sürümlerini koruduğunuzdan emin olmak için, geçiş tamamlandıktan sonra bu anlık görüntülere ihtiyacınız olacaktır.
* Aynı StorSimple birimi tarafından sunulan tüm Azure dosya paylaşımları için Azure dosya paylaşımı anlık görüntülerini aldığınızdan emin olun. Birim klonları ses düzeyinde, Azure dosya paylaşımı anlık görüntüleri paylaşım düzeyindedir. Bir birim klon geçişi bittikten sonra bir paylaşım anlık görüntüsü (her Azure dosya paylaşımında) almanız gerekir.
* Bir birim klon için geçiş işlemini yineleyin ve canlı verilerin anlık görüntüsüne yakalanana kadar her birim klondan sonra paylaşım anlık görüntülerini alın. Bir hacim klon geçirme işlemi aşağıdaki aşamalarda açıklanmıştır. 

Yedeklemeleri hiç taşımanız gerekmiyorsa ve yalnızca canlı verilerin geçişi yapıldıktan sonra Azure dosya paylaşım tarafında yeni bir yedekleme zinciri başlatabiliyorsanız, geçişteki karmaşıklığı ve geçişin alacağı süreyi azaltmak için yararlıdır. StorSimple'da yedeklemeleri taşıyıp taşımama ve her birim için kaç (her paylaşım değil) kaç tane olduğunu kararverebilirsiniz.

## <a name="phase-1-get-ready"></a>Aşama 1: Hazır olun

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan önceki genel bakış resminin bir bölümünü gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-1.png)
    :::column-end:::
    :::column:::
        Geçişin temeli, StorSimple 8020 adı verilen bir hacim klonu ve sanal bulut cihazıdır.
Bu aşama, bu kaynakların Azure'da dağıtımına odaklanır.
    :::column-end:::
:::row-end:::

### <a name="deploy-a-storsimple-8020-virtual-appliance"></a>StorSimple 8020 sanal cihazı nı dağıtın

Bulut cihazı dağıtmak, güvenlik, ağ ve diğer birkaç husus gerektiren bir işlemdir.

> [!IMPORTANT]
> Aşağıdaki kılavuz bazı gereksiz bölümler içerir. Makaleyi baştan "Adım 3"e kadar okuyun ve izleyin. Sonra bu makaleye geri dönün. Şu anda, bu kılavuzda "Adım 3" ya da ötesinde bir şey tamamlamak gerekmez.

[StorSimple 8020 sanal cihazının dağıtımı](../../storsimple/storsimple-8000-cloud-appliance-u2.md)

### <a name="determine-a-volume-clone-to-use"></a>Kullanılacak bir birim klon belirleme

Geçişe başlamaya hazır olduğunuzda, ilk adım, StorSimple bulut depolamanızın geçerli durumunu yakalayan yeni bir hacim klonu (tıpkı yedekleme için olduğu gibi) almaktır. Sahip olduğunuz StorSimple birimlerinin her biri için bir klon alın.
Yedeklemeleri taşımanız gerekiyorsa, kullandığınız ilk birim klon yeni oluşturulan bir klon değil, geçirmeniz gereken en eski birim klondur (en eski yedekleme).
Ayrıntılı kılavuz için ["Varolan StorSimple yedeklemeleri ile ilgili dikkat edilmesi gerekenler"](#considerations-around-existing-storsimple-backups) bölümüne bakın.

> [!IMPORTANT]
> Aşağıdaki kılavuz bazı gereksiz bölümler içerir. Yalnızca bağlantılı bölümde özetlenen adımları okuyun ve izleyin. Sonra bu makaleye geri dönün. "Sonraki adımlar" bölümünü izlemeniz gerekmez.

[Bir birim klon oluşturma](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)

### <a name="use-the-volume-clone"></a>Ses klonunu kullanma

Aşama 1'in son aşaması, seçtiğiniz hacim klonunu Azure'daki 8020 sanal cihazda kullanılabilir hale getirmektir.

> [!IMPORTANT]
> Aşağıdaki kılavuz gerekli adımları içerir, aynı zamanda - sonunda - birim biçimlendirmek için bir yönerge. **SES DÜZEYINI BIÇIMLENDIRMEYIN** En başından talimata kadar bağlantılı "bölüm 7"yi okuyun ve izleyin: "10. Basit bir birim biçimlendirmek için..."  Bu adımı takip etmeden önce durdurun ve bu makaleye dönün.

[Azure'daki 8020 sanal cihaza ses klonu takma](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-1-summary"></a>Faz 1 özeti

1. aşamayı tamamladığınızda aşağıdakileri yapmış olabilirsiniz:

* Azure'da StorSimple 8020 sanal cihazı dağıtıldı.
* Geçişe hangi hacim klonunu başlayacağınızı belirleyin.
* Ses klonunuzu (her canlı birim için bir tane) Azure'daki StorSimple sanal cihazına monte edin ve verileri daha fazla kullanıma hazır hale getirilebilir.

## <a name="phase-2-cloud-vm"></a>Aşama 2: Bulut VM

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan önceki genel bakış resminin bir bölümünü gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-2.png)
    :::column-end:::
    :::column:::
        İlk klonunuzu Azure'daki StorSimple 8020 sanal cihazında kullanıma sunulduktan sonra, artık bir VM sağlama ve ses klonunu (veya birden çok) iSCSI üzerinden bu VM'ye maruz bırakma nın zamanı geldi.
    :::column-end:::
:::row-end:::

### <a name="deploy-an-azure-vm"></a>Azure VM dağıtma

Azure'daki Windows Server sanal makinesi, yalnızca geçiş sırasında gerekli olan geçici bir altyapı parçası olan StorSimple 8020 gibi.
Dağıttığınız VM'nin yapılandırması çoğunlukla eşitlediğiniz öğe (dosya ve klasör) sayısına bağlıdır. Herhangi bir endişeniz varsa daha yüksek bir performans yapılandırması ile devam öneririz.

Tek bir Windows Sunucusu en fazla 30 Azure dosya paylaşımını eşitleyebilir.
Karar verdiğiniz özelliklerin her paylaşım/yolu veya StorSimple biriminin kökünü kapsaması ve öğeleri (dosya ve klasörler) sayması gerekir.

Verilerin genel boyutu bir darboğaz daha azdır - bu makine özellikleri terzi için gereken öğelerin sayısıdır.

* [Bir Windows Server'ı eşitlemeniz gereken öğe (dosya ve klasörler) sayısına göre nasıl boyutlandırabileceğinizi öğrenin.](storage-sync-files-planning.md#recommended-system-resources)

    **Lütfen dikkat:** Daha önce bağlanan makale, sunucu belleği (RAM) için bir aralık içeren bir tablo sunar. Azure VM için büyük sayıya doğru yönlendirin. Şirket içi makineniz için daha küçük sayıya yönelebilirsiniz.

* [Windows Sever VM'yi nasıl dağıtılayacağımı öğrenin.](../../virtual-machines/windows/quick-create-portal.md)

> [!IMPORTANT]
> VM'nin StorSimple 8020 sanal cihazla aynı Azure bölgesinde dağıtıldıkdığından emin olun. Bu geçişin bir parçası olarak, bulut verilerinizin bugün depolanan bölgeden bölgesini de değiştirmeniz gerekiyorsa, bunu Azure dosya paylaşımlarını sağlarken daha sonraki bir adımda yapabilirsiniz.

> [!IMPORTANT]
> Performans için optimize etmek için bulut VM'iniz için çok hızlı bir **işletim sistemi diski** dağıtın. Tüm veri birimleriniz için eşitleme veritabanını işletim sistemi diskinde saklarsınız. Ayrıca, büyük bir **işletim sistemi diski**oluşturduğunuzdan emin olun. StorSimple birimlerinizdeki öğelerin (dosya ve klasörler) sayısına bağlı olarak, işletim sistemi diskinin eşitleme veritabanını barındırmak için **birkaç yüz GiB** alanı gerekebilir.

### <a name="expose-the-storsimple-8020-volumes-to-the-azure-vm"></a>StorSimple 8020 birimlerini Azure VM'ye maruz bırak

Bu aşamada, iSCSI üzerinden 8020 sanal cihazdan bir veya birkaç StorSimple cildini, sizin sağdığınız Windows Server VM'ye bağlarsınız.

> [!IMPORTANT]
> Aşağıdaki makaleler için yalnızca **bulut cihazı için özel IP Alın'ı** doldurun ve **iSCSI** bölümleri üzerinden bağlanın ve bu makaleye geri dönün.

1. [Bulut gereci özel IP alma](../../storsimple/storsimple-8000-cloud-appliance-u2.md#get-private-ip-for-the-cloud-appliance)
2. [iSCSI üzerinden bağlanın](../../storsimple/storsimple-8000-deployment-walkthrough-u2.md#step-7-mount-initialize-and-format-a-volume)

### <a name="phase-2-summary"></a>Aşama 2 özeti

İkinci aşamayı tamamladığınızda, şunları var: 

* 8020 sanal StorSimple cihazıyla aynı bölgede bir Windows Server VM'si
* 8020'den Windows Server VM'ye iSCSI üzerinden tüm geçerli birimleri açığa çıkardı.
* Artık, bağlı birimlerde Server VM'de Dosya Gezgini'ni kullandığınızda dosya ve klasör içeriğini görmeniz gerekir.

Yalnızca geçiş gerektiren tüm birimler için bu adımları tamamladığınızda 3.

## <a name="phase-3-set-up-azure-file-shares-and-get-ready-for-azure-file-sync"></a>Aşama 3: Azure dosya paylaşımlarını ayarlama ve Azure Dosya Eşitlemi için hazırlan

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan önceki genel bakış resminin bir bölümünü gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-3.png)
    :::column-end:::
    :::column:::
        Bu aşamada, bir dizi Azure dosya paylaşımını belirleyecek ve bunları sağlayacak, StorSimple cihaz yerine bir Windows Server oluşturacak ve bu sunucuya Azure Dosya Eşitlemi için yapılandırılacak. 
    :::column-end:::
:::row-end:::

### <a name="map-your-existing-namespaces-to-azure-file-shares"></a>Varolan ad alanlarınızı Azure dosya paylaşımlarına göre eşle

[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="deploy-azure-file-shares"></a>Azure dosya paylaşımlarını dağıtma

[!INCLUDE [storage-files-migration-provision-azfs](../../../includes/storage-files-migration-provision-azure-file-share.md)]

> [!TIP]
> Azure bölgesini StorSimple verilerinizin bulunduğu geçerli bölgeden değiştirmeniz gerekiyorsa, ardından kullanmak istediğiniz yeni bölgede Azure dosya paylaşımlarını sağlama. Azure dosya paylaşımlarınızı elinde bulunduran depolama hesaplarını sağlarken bölgeyi seçerek bölgeyi belirlersiniz. Aşağıda sağacağınız Azure Dosya Eşitleme kaynağının da aynı yeni bölgede olduğundan emin olun.

### <a name="deploy-the-azure-file-sync-cloud-resource"></a>Azure Dosya Eşitleme bulut u kaynağını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

> [!TIP]
> Azure bölgesini StorSimple verilerinizin bulunduğu geçerli bölgeden değiştirmeniz gerekiyorsa, yeni bölgedeki Azure dosya paylaşımlarınızın depolama hesaplarını sağlamış sınız demektir. Bu Depolama Eşitleme Hizmetini dağıtırken aynı bölgeyi seçtiğinizden emin olun.

### <a name="deploy-an-on-premises-windows-server"></a>Şirket içinde Windows Server dağıtma

* Sanal makine veya fiziksel sunucu olarak en az 2012R2 olarak bir Windows Server 2019 oluşturun. Windows Server hata kümesi de desteklenir. StorSimple 8100 veya 8600'ün önünde sahip olabileceğiniz sunucuyu yeniden kullanmayın.
* Doğrudan Bağlı Depolama (DESTEKLENMEYEn NAS ile karşılaştırıldığında DAS) sağlama veya ekleme.

Yeni Windows Server'ınıza StorSimple 8100 veya 8600 cihazınızın önbelleğe almak için yerel olarak mevcut olduğundan eşit veya daha fazla depolama alanı vermek en iyi yöntemdir. Windows Server'ı StorSimple cihazını kullandığınız gibi kullanacaksınız, eğer cihazla aynı miktarda depolama alanına sahipse, önbelleğe alma deneyimi aynı olmasa da benzer olmalıdır.
Istediğiniz zaman Windows Server'ınızdan depolama ekleyebilir veya kaldırabilirsiniz. Bu, yerel birim boyutunuzu ve önbelleğe almak için kullanılabilir yerel depolama miktarını ölçeklendirmenize olanak tanır.

### <a name="prepare-the-windows-server-for-file-sync"></a>Windows Server'ı dosya eşitleme için hazırlama

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

### <a name="configure-azure-file-sync-on-the-windows-server"></a>Windows Sunucusunda Azure Dosya Eşitlemesini Yapılandırma

Kayıtlı şirket içi Windows Server'ınız bu işlem için hazır ve internete bağlı olmalıdır.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!WARNING]
> **Bulut katmanlamayı açtıktan emin olun!** Bulut katmanlama, yerel sunucunun bulutta depolanandan daha az depolama kapasitesine sahip olmasına ve ancak tam ad alanına sahip olmasına olanak tanıyan AFS özelliğidir. Yerel olarak ilginç veriler de hızlı, yerel erişim performansı için yerel olarak önbelleğe aledilir. Bu adımda bulut katmanlamayı açmak için başka bir neden, bu aşamada dosya içeriğini eşitlemek istemememizdir, yalnızca ad alanı şu anda hareket ediyor olmalıdır.

## <a name="phase-4-configure-the-azure-vm-for-sync"></a>Aşama 4: Eşitleme için Azure VM'yi yapılandırın

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan önceki genel bakış resminin bir bölümünü gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-4.png)
    :::column-end:::
    :::column:::
        Bu aşama, Azure VM'nizi iSCSI monte edilmiş, ilk birim klon(lar) ile ilgilidir. Bu aşamada, VM'yi Azure Dosya Eşitlemi üzerinden bağlayacak ve StorSimple birim klonunuzdan dosyaların taşınmasının ilk turunu başlatın.
        
    :::column-end:::
:::row-end:::

Azure Dosya Eşitlemi için StorSimple 8100 veya 8600 cihazınızın yerini alacak şirket içi sunucunuzu zaten yapılandırıldınız. 

Azure VM'yi yapılandırmak, tek bir adımla hemen hemen aynı bir işlemdir. Aşağıdaki adımlar işlem boyunca size rehberlik edecektir.

> [!IMPORTANT]
> Azure VM'nin bulut **katmanlama etkinken yapılandırılmamanız önemlidir!** Bu sunucunun hacmini geçiş boyunca yeni hacim klonları ile değiştirirsiniz. Bulut katmanlamanın CPU kullanımında hiçbir yararı ve yükü yoktur.

1. [AFS aracısını dağıtın. (bkz. önceki bölüm)](#prepare-the-windows-server-for-file-sync)
2. [VM'yi Azure Dosya Eşitlemi için hazırlama.](#get-the-vm-ready-for-azure-file-sync)
3. [Eşitleme yapılandırma](#configure-azure-file-sync-on-the-azure-vm)

### <a name="get-the-vm-ready-for-azure-file-sync"></a>Azure Dosya Eşitlemi için VM'yi hazırlayın

Azure Dosya Eşitlemi, dosyaları monte edilmiş iSCSI StorSimple birimlerinden hedef Azure dosya paylaşımlarına taşımak için kullanılır.
Bu geçiş işlemi sırasında, aynı sürücü harfi nin altına VM'nize birkaç hacimklisi monte edeyim. Azure Dosya Eşitlemesi, dosya ve klasörlerin daha yeni bir sürümü olarak monte ettiğiniz bir sonraki birim klonunu görecek ve Azure Dosya Eşitlemesi ile bağlanan Azure dosya paylaşımlarını güncelleyecek şekilde yapılandırılmalıdır. 

> [!IMPORTANT]
> Bunun işe yaraması için, Azure Dosya Eşitlemesi yapılandırılmadan önce sunucuda bir kayıt defteri anahtarının ayarlanması gerekir.

1. VM'nin sistem sürücüsünde yeni bir dizin oluşturun. Azure Dosya Eşitleme bilgilerinin monte edilen ses klonları yerine burada kalıcı olması gerekir. Örneğin, `"C:\syncmetadata"`
2. Regedit'i açın ve aşağıdaki kayıt kovanını bulun:`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync`
3. Adı geçen yeni bir String türü Anahtar oluşturma: ***MetadataRootPath***
4. Sistem hacminde oluşturduğunuz dizine tam yol ayarlayın, örneğin:`C:\syncmetadata"`

### <a name="configure-azure-file-sync-on-the-azure-vm"></a>Azure VM'de Azure Dosya Eşitlemesini Yapılandırma

Bu adım, afs'yi şirket içi sunucuda nasıl yapılandırdığınızı anlatan önceki bölüme benzer.

Aradaki fark, bu sunucuda bulut katmanlamayı etkinleştirmemeniz ve doğru klasörlerin doğru Azure dosya paylaşımlarına bağlı olduğundan emin olmanız gerektiğidir. Aksi takdirde Azure dosya paylaşımlarını ve veri içeriğini adlandırmanız eşleşmez ve eşitlemeyi yeniden yapılandırmadan bulut kaynaklarını veya yerel klasörleri yeniden adlandırmanın bir yolu yoktur.

[Windows Server'da Azure Dosya Eşitlemeyi'nin nasıl yapılandırılabildiğini anlatan önceki bölüme](#configure-azure-file-sync-on-the-windows-server)bakın.

### <a name="step-4-summary"></a>Adım 4 özeti

Bu noktada, iSCSI üzerinden StorSimple birim klonunuzu (lar) monte ettiğiniz Azure VM'de Azure Dosya Eşitlemeyi'ni başarıyla yapılandırmış olabilirsiniz.
Veriler artık Azure VM'den çeşitli Azure dosya paylaşımlarına akıyor ve buradan şirket içi Windows Server'ınızda tamamen yorgun bir ad alanı görünüyor.

> [!IMPORTANT]
> Şu anda Windows Server'da değişiklik yapılmadığından veya kullanıcı erişimi verilmedidiğinden emin olun.

Azure VM'den Azure dosya paylaşımlarına taşınan ilk birim klon verileri uzun sürebilir, potansiyel olarak haftalar sürebilir. Bunun alacağı zamanı tahmin etmek zordur ve birçok faktöre bağlıdır. En önemlisi, Azure VM'nin StorSimple birimlerindeki dosyalara erişebildiği hız ve Azure Dosya Eşitlemesi'nin eşitlenmesi gereken dosya ve klasörleri ne kadar hızlı işleyebileceği. 

Deneyimlerinden, bant genişliğinin - dolayısıyla gerçek veri boyutunun - alt rol oynadığını varsayabiliriz. Bu veya sonraki geçiş turunun alacağı süre, çoğunlukla saniyede işlenebilecek öğe sayısına bağlıdır. Yani örneğin 1 00.000 dosya ve klasörleri ile 1 TiB büyük olasılıkla sadece 50.000 dosya ve klasörleri ile 1 TiB daha yavaş bitirecektir.

## <a name="phase-5-iterate-through-multiple-volume-clones"></a>Aşama 5: Birden çok hacimli klonlar aracılığıyla iterate

:::row:::
    :::column:::
        ![Makalenin bu alt bölümüne odaklanmaya yardımcı olan önceki genel bakış resminin bir bölümünü gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-5.png)
    :::column-end:::
    :::column:::
        Önceki aşamada tartışıldığı gibi, ilk eşitleme uzun zaman alabilir. Kullanıcılarınız ve uygulamalarınız hala şirket içi StorSimple 8100 veya 8600 cihazına erişmektedir. Bu, değişikliklerin biriktiği ve her gün canlı veriler le ilk birim klonarasında daha büyük bir delta nın bulunduğu anlamına gelir, şu anda geçiş yapıyorsunuz, formlar. Bu bölümde, birden çok birim klon kullanarak ve eşitlemenin ne zaman yapılacağını söyleyerek kapalı kalma süresini nasıl en aza indireceğinizi öğreneceksiniz.
    :::column-end:::
:::row-end:::

Ne yazık ki, geçiş süreci anlık değildir. Bu, canlı verilere yukarıda bahsedilen delta kaçınılmaz bir sonucu olduğu anlamına gelir. İyi haber, yeni hacim klonları montaj sürecini tekrarlayabilirsiniz. Her hacim klonunun deltası giderek daha küçük olacaktır. Bu nedenle, bir eşitleme, kullanıcıları ve uygulamaları şirket içi Windows sunucunuza kesmek için çevrimdışı hale getirmek için kabul edilebilir olduğunu düşündüğünüz bir süre içinde sona erecektir.

Kullanıcıları ve uygulamaları çevrimdışı naalarak kendinizi rahat hissedeceğiniz kadar hızlı bir süre içinde eşitleme tamamlanana kadar aşağıdaki adımları yineleyin:

1. [Belirli bir birim klon için eşitlemenin tamamlanın.](#determine-when-sync-is-done)
2. [Yeni bir hacim klon(lar) alın ve 8020 sanal cihaza monte edin.](#the-next-volume-clones)
3. [Eşitlemenin ne zaman yapılacağını belirleyin.](#determine-when-sync-is-done)
4. [Kesme stratejisi](#cut-over-strategy)

### <a name="the-next-volume-clones"></a>Bir sonraki birim klon(lar)

Biz bu makalede daha önce bir hacim klon(lar) alarak tartıştık.
Bu aşamada iki eylem vardır:

1. [Hacimli bir klon alın](../../storsimple/storsimple-8000-clone-volume-u2.md#create-a-clone-of-a-volume)
2. [Bu hacim klonunu monte edin (yukarıya bakın)](#use-the-volume-clone)

### <a name="determine-when-sync-is-done"></a>Eşitlemenin ne zaman yapılacağını belirleme

Eşitleme yapıldığında, zaman ölçümünüzü durdurabilir ve bir hacim klonunu alıp monte etme işlemini tekrarlamanız gerekip gerekmediğinizi veya son birim klonla yapılan zaman eşitlemenin yeterince küçük olup olmadığını belirleyebilirsiniz.

Eşitleme belirlemek için tamamlandı:

1. Olay Görüntüleyicisi'ni açın ve **Uygulamalar ve Hizmetlere** gidin
2. Gezinme ve **Microsoft\FileSync\Agent\Telemetri'yi** açın
3. Tamamlanmış eşitleme oturumuna karşılık gelen en son **olay 9102'yi**arayın
4. **Ayrıntılar'ı** seçin ve **SyncDirection** değerinin **Yükle** olduğunu onaylayın
5. **HResult** kontrol edin ve **0**gösterir onaylayın. Bu, eşitleme oturumunun başarılı olduğu anlamına gelir. HResult sıfır olmayan bir değerse, eşitleme sırasında bir hata oluştu. **PerItemErrorCount** 0'dan büyükse, bazı dosya veya klasörler düzgün eşitlenmemiştir. 0'dan daha büyük bir PerItemErrorCount'a sahip olmak mümkündür. Bu noktada, PerItemErrorCount hakkında endişelenmenize gerek yok. Bu dosyaları daha sonra yakalayacağız. Bu hata sayısı önemliyse, binlerce öğe, müşteri desteğine başvurun ve en iyi, sonraki aşamalarda doğrudan rehberlik için Azure Dosya Eşitle misiniz ürün grubuna bağlanmayı isteyin.
6. Art arda HResult 0 içeren birden fazla 9102 olayını görmek için denetleyin. Bu, bu birim klon için eşitlemenin tamamladığını gösterir.

### <a name="cut-over-strategy"></a>Kesme stratejisi

1. Birim klonundan eşitlemenin artık yeterince hızlı olup olmadığını belirleyin. (Delta yeterince küçük.)
2. StorSimple cihazını çevrimdışına alın.
3. Son bir RoboCopy.

Zamanı ölçün ve son birim klondan eşitlemenin sisteminizde kesinti süresini karşılayabileceğikadar küçük bir zaman penceresi içinde bitirip bitirilemeyebileceğini belirleyin.

Artık Kullanıcının StorSimple cihazına erişimini devre dışı bırakın. Daha fazla değişiklik yok. Kesinti başladı.
Cihazı çevrimiçi ve bağlı bırakmanız gerekir, ancak artık cihazdaki değişiklikleri önlemeniz gerekir.

Faz 6'da son birim klondan beri canlı verilerdeki herhangi bir deltaya yetişeceksiniz.

## <a name="phase-6-a-final-robocopy"></a>Faz 6: Son Bir RoboCopy

Bu noktada, şirket içi Windows Server'ınız ile StorSimple 8100 veya 8600 cihazı arasında iki fark vardır:

1. Eşitlenmemiş dosyalar olabilir (yukarıdaki olay günlüğünden **PerItem Hataları'na** bakın)
2. StorSimple cihazı, Windows Server'a karşı doldurulan bir önbelleğe sahiptir ve şu anda yerel olarak depolanan dosya içeriği olmayan bir ad alanı vardır.

![Makalenin bu alt bölümüne odaklanmaya yardımcı olan önceki genel bakış resminin bir bölümünü gösteren bir resim.](media/storage-files-migration-storsimple-shared/storsimple-8000-migration-phase-6.png)

Windows Server önbelleğini cihazın durumuna getirebilir ve son bir RoboCopy ile hiçbir dosyanın geride bırakılmamasını sağlayabiliriz.

> [!CAUTION]
> Takip ettiğiniz RoboCopy komutunun tam olarak aşağıda açıklandığı gibi olması zorunludur. Yalnızca yerel dosyaları ve daha önce birim klon+eşitleme yaklaşımında hareket etmemiş dosyaları kopyalamak istiyoruz. Geçiş tamamlandıktan sonra neden senkronize olmadıkları sorunlarını çözebiliriz. (Bkz. [Azure Dosya Eşitleme sorun giderme.](storage-sync-files-troubleshoot.md#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing) Dosya adlarında büyük olasılıkla yazdırılamayan ve silindiğinde gözden kaçırmayacağınız karakterlerdir.)

RoboCopy komutu:

```console
Robocopy /MT:32 /UNILOG:<file name> /TEE /B /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Arka plan:

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      RoboCopy'nin çok iş parçacığı çalışmasını sağlar. Varsayılan değer 8, en fazla 128.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:<file name>
   :::column-end:::
   :::column span="1":::
      UNICODE olarak LOG dosyasına durum çıktıları (varolan günlüğün üzerine yazar).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /TEE
   :::column-end:::
   :::column span="1":::
      Konsol penceresine çıkan çıktılar. Bir günlük dosyasına çıktı ile birlikte kullanılır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /B
   :::column-end:::
   :::column span="1":::
      RoboCopy'i yedek bir uygulamanın kullanacağı modda çalıştırır. RoboCopy'nin geçerli kullanıcının izinleri olmayan dosyaları taşımasına olanak tanır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MIR
   :::column-end:::
   :::column span="1":::
      RoboCopy'nin yalnızca kaynak (StorSimple cihazı) ve hedef (Windows Server dizini) arasındaki deltaları dikkate almalarına olanak tanır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      dosya kopyasının doğruluğu (varsayılan /COPY:DAT' dır), kopya bayrakları: D=Data, A=Öznitelikler, T=Zaman damgaları, S=Security=NTFS AKLAR, O=Sahip bilgileri, U=aUditing bilgi
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      TÜM dosya bilgilerini KOPYALA (/COPY:DATSOU'ya eşdeğer)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY:copyflag[s]
   :::column-end:::
   :::column span="1":::
      dizinlerin kopyası için sadakat (varsayılan dır /DCOPY:DA), kopya bayrakları: D=Data, A=Öznitelikler, T=Zaman damgaları
   :::column-end:::
:::row-end:::

Windows Server'daki her dizin için bu RoboCopy komutunu, bir Azure dosyasıyla dosya eşitle'yle yapılandırdığınız bir hedef olarak çalıştırmalısınız.

Bu komutların birden çoğunu paralel olarak çalıştırabilirsiniz.
Bu RoboCopy adımı tamamlandıktan sonra, kullanıcılarınızın ve uygulamalarınızın Daha önce StorSimple cihazında olduğu gibi Windows Server'a erişmesine izin verebilirsiniz.

Dosyaların geride bırakIlip bırakılmadığını görmek için robocopy günlük dosyasına(lar) başvurun. Sorunlar varsa, çoğu durumda geçiş tamamlandıktan ve kullanıcılarınız ve uygulamalarınız Windows Server'ınıza yeniden yerleştirildikten sonra bunları çözebilirsiniz. Herhangi bir sorunu çözmeniz gerekiyorsa, bunu 7.

Büyük olasılıkla Daha önce StorSimple verilerinde olan Windows Server'da SMB paylaşımlarını oluşturmak gerekir. Bu adımı önceden yükleyebilir ve burada zaman kaybetmemek için daha erken yükleyebilirsiniz, ancak bu noktadan önce Windows sunucusunda dosyalarda değişiklik yapılmadığından emin olmalısınız.

DFS-N dağıtımınız varsa, DFN-Namespaces'i yeni sunucu klasörü konumlarına yönlendirebilirsiniz. DFS-N dağıtımınız yoksa ve 8100 8600 cihazınızı bir Windows Server ile yerel olarak ön plana çıkardıysanız, bu sunucuyu etki alanından alabilir ve etki alanınıza AFS ile yeni Windows Server'ınızı katabilir ve eski sunucuyla aynı sunucu adını verebilirsiniz ve aynı paylaşım adları, ardından yeni sunucuya kesme kullanıcılarınız, grup ilkeniz veya komut dosyaları için saydam kalır.

## <a name="phase-7-deprovision"></a>7. Aşama: Deprovision

Son aşamada birden çok ses klonu aracılığıyla yinelendiniz ve sonunda StorSimple cihazını çevrimdışına aldıktan sonra kullanıcının yeni Windows Server'a erişimini kesmeyi başardınız.

Artık gereksiz kaynakları yok etmeye başlayabilirsiniz.
Başlamadan önce, yeni Azure Dosya Eşitleme dağıtımınızı üretimde bir süre gözlemlemek en iyi yöntemdir. Bu, karşılaşabileceğiniz sorunları gidermek için seçenekler sunar.

Tatmin olduktan ve AFS dağıtımınızı en az birkaç gün gözlemledikten sonra, kaynakları şu sırayla deprovision'ı deprovision etmeye başlayabilirsiniz:

1. Verileri birim klonlardan dosya eşitleme yoluyla Azure dosya paylaşımlarına taşımak için kullandığımız Azure VM'sini kapatın.
2. Azure'daki Depolama Eşitleme Hizmeti kaynağınıza gidin ve Azure VM'nin kaydını silin. Bu tüm eşitleme gruplarından kaldırır.

    > [!WARNING]
    > **Doğru makineyi seçtiğinizden emin olun.** Bulut VM'yi kapattınız, bu da kayıtlı sunucular listesindeki tek çevrimdışı sunucu olarak gösterilmesi gerektiği anlamına gelir. Bu adımda şirket içi Windows Server'ı seçmemelisiniz, bunu yaptığınızda kayıt dışı olacaktır.

3. Azure VM'yi ve kaynaklarını silin.
4. 8020 sanal StorSimple cihazını devre dışı bırakın.
5. Azure'daki tüm StorSimple kaynaklarını deprovision.
6. StorSimple fiziksel cihazını veri merkezinizden çıkarın.

Geçişin tamamlandı.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosya Eşitlemeyi'ni daha iyi taşkınlık edin. Özellikle bulut katmanlama politikalarının esnekliği yle.

Azure portalında veya önceki olaylardan bazı dosyaların kalıcı olarak eşitlenmediğini görürseniz, bu sorunları gidermeye yönelik adımlar için sorun giderme kılavuzunu gözden geçirin.

* [Azure Dosya Eşitleme genel bakış: aka.ms/AFS](https://aka.ms/AFS)
* [Bulut katmanlaması](storage-sync-cloud-tiering.md) 
* [Azure Dosya Eşitleme sorun giderme kılavuzu](storage-sync-files-troubleshoot.md)
