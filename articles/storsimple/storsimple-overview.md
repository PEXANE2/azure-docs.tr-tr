---
title: StorSimple 8000 serisi çözüme genel bakış | Microsoft Dokümanlar
description: StorSimple katmanlamaaçıklar, cihaz, sanal cihaz, hizmetler ve depolama yönetimi, ve StorSimple kullanılan anahtar terimleri tanıttı.
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: ''
ms.assetid: 7144d218-db21-4495-88fb-e3b24bbe45d1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 07/10/2017
ms.author: timlt
ms.openlocfilehash: 2a6650cac975c575415a329361da00d4fbfcaa9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68965110"
---
# <a name="storsimple-8000-series-a-hybrid-cloud-storage-solution"></a>StorSimple 8000 serisi: karma bulut depolama çözümü

[!INCLUDE [storsimple-8000-eol-banner](../../includes/storsimple-8000-eol-banner.md)]

## <a name="overview"></a>Genel Bakış
Şirket içi aygıtlar ve Microsoft Azure bulut depolama arasında depolama görevlerini yöneten entegre bir depolama çözümü olan Microsoft Azure StorSimple'a hoş geldiniz. StorSimple, kurumsal depolama ve veri korumasıyla ilişkili birçok sorunu ve gideri ortadan kaldıran verimli, uygun maliyetli ve kolay yönetilebilir bir depolama alanı ağı (SAN) çözümüdür. Özel StorSimple 8000 serisi cihazı kullanır, bulut hizmetleriyle tümleştirir ve bulut depolama dahil olmak üzere tüm kurumsal depolama birimlerinin sorunsuz bir görünümü için bir dizi yönetim aracı sağlar. (Microsoft Azure web sitesinde yayınlanan StorSimple dağıtım bilgileri yalnızca StorSimple 8000 serisi aygıtlar için geçerlidir. Bir StorSimple 5000/7000 serisi cihaz kullanıyorsanız, [StorSimple Yardım](http://onlinehelp.storsimple.com/)gidin .)

StorSimple, depolanan verileri çeşitli depolama ortamları arasında yönetmek için [depolama katmanlamasını](#automatic-storage-tiering) kullanır. Geçerli çalışma kümesi katı durum sürücülerinde (SSD'lerde) şirket içinde depolanır, daha az kullanılan veriler sabit disk sürücülerinde (HDD'lerde) depolanır ve arşiv verileri buluta itilir. Ayrıca, StorSimple verilerin tükettiği depolama miktarını azaltmak için deduplication ve sıkıştırma kullanır. Daha fazla bilgi için, [Deduplication ve sıkıştırma](#deduplication-and-compression)gidin. StorSimple 8000 serisi belgelerinde kullanılan diğer anahtar terim ve kavramların tanımları için, bu makalenin sonunda [StorSimple terminolojisine](#storsimple-terminology) gidin.

Depolama yönetimine ek olarak, StorSimple veri koruma özellikleri isteğe bağlı ve zamanlanmış yedeklemeler oluşturmanıza ve bunları yerel olarak veya bulutta depolamanıza olanak tanır. Yedeklemeler, hızla oluşturulup geri yüklenebilecekleri anlamına gelen artımlı anlık görüntüler şeklinde alınır. Bulut anlık görüntüleri, ikincil depolama sistemlerinin (teyp yedekleme si gibi) yerini aldığı ve verileri veri merkezinize veya gerekirse alternatif sitelere geri yüklemenize olanak sağladıklarından, olağanüstü durum kurtarma senaryolarında kritik öneme ilebilir.

![video simgesi](./media/storsimple-overview/video_icon.png) Microsoft Azure StorSimple'a hızlı bir giriş için videoyu izleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/StorSimple-Hybrid-Cloud-Storage-Solution/player]

## <a name="why-use-storsimple"></a>Neden StorSimple kullanın?
Aşağıdaki tabloda, Microsoft Azure StorSimple'ın sağladığı bazı önemli avantajlar açıklanmaktadır.

| Özellik | Avantaj |
| --- | --- |
| Saydam entegrasyon |Veri depolama tesislerini görünmez bir şekilde bağlamak için iSCSI protokolünü kullanır. Bu, bulutta, veri merkezinde veya uzak sunucularda depolanan verilerin tek bir konumda depolanmış gibi görünmesini sağlar. |
| Daha düşük depolama maliyetleri |Geçerli talepleri karşılamak için yeterli yerel veya bulut depolama alanı ayırır ve bulut depolamayı yalnızca gerektiğinde genişletir. Ayrıca, aynı verilerin (çoğaltma) gereksiz sürümlerini ortadan kaldırarak ve sıkıştırma kullanarak depolama gereksinimlerini ve gideri azaltır. |
| Basitleştirilmiş depolama yönetimi |Şirket içinde, uzak bir sunucuda ve bulutta depolanan verileri yapılandırmak ve yönetmek için sistem yönetim araçları sağlar. Ayrıca, microsoft yönetim konsolu (MMC) snap-in'den yedekleme ve geri yükleme işlevlerini yönetebilirsiniz.|
| Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk |Uzun kurtarma süresi gerektirmez. Bunun yerine, gerektiğinde verileri geri yükler. Bu, normal operasyonların en az kesintiyle devam edebileceği anlamına gelir. Ayrıca, yedekleme zamanlamaları ve veri saklama belirtmek için ilkeleri yapılandırabilirsiniz. |
| Veri hareketliliği |Microsoft Azure bulut hizmetlerine yüklenen verilere kurtarma ve geçiş amacıyla diğer sitelerden erişilebilir. Ayrıca, StorSimple Bulut Cihazları'nı Microsoft Azure'da çalışan sanal makinelerde (VM) yapılandırmak için StorSimple'ı kullanabilirsiniz. VM'ler daha sonra depolama veya kurtarma amacıyla depolanan verilere erişmek için sanal aygıtlar kullanabilir. |
| İş sürekliliği |StorSimple 5000-7000 serisi kullanıcıların verilerini StorSimple 8000 serisi bir cihaza geçirmelerine olanak tanır. |
| Azure Kamu Portalı'nda kullanılabilirlik |StorSimple, Azure Devlet Portalı'nda kullanılabilir. Daha fazla bilgi için, [şirket içi StorSimple cihazınızı Devlet Portalı'na dağıtın.](storsimple-8000-deployment-walkthrough-gov-u2.md) |
| Veri koruma ve kullanılabilirlik |StorSimple 8000 serisi, Yerel Yedekli Depolama (LRS) ve Coğrafi Yedekli Depolamaya (GRS) ek olarak Bölge Yedekli Depolamayı (ZRS) destekler. ZRS ayrıntıları için [Azure Depolama artıklığı seçenekleri yle ilgili bu makaleye](https://azure.microsoft.com/documentation/articles/storage-redundancy/) bakın. |
| Kritik uygulamalar için destek |StorSimple, uygun birimleri yerel olarak sabitlenmiş olarak tanımlamanıza olanak tanır ve bu da kritik uygulamaların gerektirdiği verilerin buluta katmanlanmamasını sağlar. Yerel olarak sabitlenmiş birimler bulut gecikmelerine veya bağlantı sorunlarına tabi değildir. Yerel olarak sabitlenmiş birimler hakkında daha fazla bilgi için [bkz.](storsimple-8000-manage-volumes-u2.md) |
| Düşük gecikme ve yüksek performans |Azure premium depolama alanının yüksek performanslı, düşük gecikme lisyonu özelliklerinden yararlanan bulut cihazları oluşturabilirsiniz. StorSimple premium bulut cihazları hakkında daha fazla bilgi için [Azure'da bir StorSimple Cloud Appliance'ı dağıt'a bakın ve yönetin.](storsimple-8000-cloud-appliance-u2.md) |


## <a name="storsimple-components"></a>StorBasit bileşenler
Microsoft Azure StorSimple çözümü aşağıdaki bileşenleri içerir:

* **Microsoft Azure StorSimple aygıt** - yedek denetleyicileri ve otomatik arıza yetenekleri ile birlikte SSD'ler ve HDD'ler içeren şirket içi karma depolama dizisi. Denetleyiciler depolama katmanlamayı yönetir, şu anda kullanılan (veya sıcak) verileri yerel depolamaya (aygıtveya şirket içi sunuculara) yerleştirirken, daha az kullanılan verileri buluta taşıyor.
* **StorSimple Cloud Appliance** - storsimple Sanal Cihaz olarak da bilinir, bu fiziksel hibrid depolama cihazının mimarisini ve en yeteneklerini çoğaltan StorSimple cihazının bir yazılım sürümüdür. StorSimple Cloud Appliance, Azure sanal makinesinde tek bir düğümüzerinde çalışır. Azure premium depolama alanından yararlanan premium sanal aygıtlar, Güncelleme 2 ve sonraki yerlerde kullanılabilir.
* **StorSimple Device Manager hizmeti** – Bir StorSimple cihazını veya StorSimple Cloud Appliance'ı tek bir web arabiriminden yönetmenize olanak tanıyan Azure portalının bir uzantısıdır. Hizmetleri oluşturmak ve yönetmek, cihazları görüntülemek ve yönetmek, uyarıları görüntülemek, birimleri yönetmek ve yedekleme ilkelerini ve yedekleme kataloğunu görüntülemek ve yönetmek için StorSimple Device Manager hizmetini kullanabilirsiniz.
* **StorSimple için Windows PowerShell** – StorSimple cihazını yönetmek için kullanabileceğiniz bir komut satırı arabirimi. StorSimple için Windows PowerShell, StorSimple cihazınızı kaydetmenize, cihazınızdaki ağ arabirimini yapılandırmanıza, belirli güncelleştirme türlerini yüklemenize, destek oturumuna erişerek cihazınızı sorun gidermenize ve aygıtı değiştirmenize olanak tanıyan özelliklere sahiptir Durum. StorSimple için Windows PowerShell'e seri konsola bağlanarak veya Windows PowerShell remoting'i kullanarak erişebilirsiniz.
* **Azure PowerShell StorBasit cmdlets** – komut satırından hizmet düzeyi ve geçiş görevlerini otomatikleştirmenize olanak tanıyan Windows PowerShell cmdlets koleksiyonu. StorSimple için Azure PowerShell cmdlets hakkında daha fazla bilgi için [cmdlet referansına](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0#azure)gidin.
* **StorSimple Snapshot Manager** – uygulama tutarlı yedeklemeler oluşturmak için birim grupları ve Windows Birim Gölge Kopyalama Hizmeti kullanan bir MMC snap-in. Buna ek olarak, yedekleme zamanlamaları oluşturmak ve birimleri klonlamak veya geri yüklemek için StorSimple Snapshot Manager'ı kullanabilirsiniz.
* Microsoft Azure StorSimple depolama ve veri korumasını SharePoint Server çiftliklerine saydam olarak genişleten ve StorSimple depolamayı SharePoint Merkezi Yönetim portalından görüntülenebilir ve yönetilebilir hale getiren bir araç olan **SharePoint için StorSimple Adapter.**

Aşağıdaki diyagram, Microsoft Azure StorSimple mimarisinin ve bileşenlerinin üst düzey bir görünümünü sağlar.

![StorBasit mimari](./media/storsimple-overview/overview-big-picture.png)

Aşağıdaki bölümler, bu bileşenlerin her birini daha ayrıntılı olarak açıklar ve çözümün verileri nasıl düzenlediğini, depolamayı nasıl ayırdığını ve depolama yönetimini ve veri korumasını nasıl kolaylaştırdığını açıklar. Son bölümde, StorSimple bileşenleri ve bunların yönetimi ile ilgili bazı önemli terim ve kavramlar için tanımlar sağlar.

## <a name="storsimple-device"></a>StorBasit cihaz
Microsoft Azure StorSimple aygıtı, üzerinde depolanan verilere birincil depolama ve iSCSI erişimi sağlayan şirket içi karma depolama dizisidir. Bulut depolama ile iletişimi yönetir ve Microsoft Azure StorSimple çözümünde depolanan tüm verilerin güvenliğini ve gizliliğini sağlamaya yardımcı olur.

StorSimple cihazı, SSD'ler ve sabit disk sürücüleri HDD'lerinin yanı sıra kümeleme ve otomatik arıza desteği içerir. Paylaşılan bir işlemci, paylaşılan depolama alanı ve iki aynalı denetleyici içerir. Her denetleyici aşağıdakileri sağlar:

* Ana bilgisayara bağlantı
* Yerel alan ağına bağlanmak için en fazla altı ağ bağlantı noktası (LAN)
* Donanım izleme
* Güç kesintiye uğrasa bile bilgileri saklayan geçici olmayan rasgele erişim belleği (NVRAM)
* Bir başarısız kümedeki sunucularda yazılım güncelleştirmelerini yönetmek için kümeye duyarlı güncelleme, böylece güncelleştirmelerin hizmet kullanılabilirliği üzerinde en az etkiye sahip olması veya hiçbir etkisi olmaması
* Bir arka uç kümesi gibi çalışan, yüksek kullanılabilirlik sağlayan ve bir HDD veya SSD başarısız olursa veya çevrimdışı alınırsa oluşabilecek yan etkileri en aza indiren küme hizmeti

Herhangi bir zamanda yalnızca bir denetleyici etkindir. Etkin denetleyici başarısız olursa, ikinci denetleyici otomatik olarak etkin olur.

Daha fazla bilgi için [StorSimple donanım bileşenlerine ve durumuna](storsimple-8000-monitor-hardware-status.md)gidin.

## <a name="storsimple-cloud-appliance"></a>StorSimple Bulut Cihazı
StorSimple'ı, fiziksel karma depolama aygıtının mimarisini ve yeteneklerini kopyalayan bir bulut aygıtı oluşturmak için kullanabilirsiniz. StorSimple Cloud Appliance (StorSimple Virtual Appliance olarak da bilinir) Azure sanal makinesinde tek bir düğümüzerinde çalışır. (Bir bulut cihazı yalnızca bir Azure sanal makinesinde oluşturulabilir. StorSimple aygıtında veya şirket içi sunucuda bir aygıt oluşturamazsınız.)

Bulut cihazı aşağıdaki özelliklere sahiptir:

* Fiziksel bir cihaz gibi olur ve buluttaki sanal makinelere iSCSI arabirimi sunabilir.
* Bulutta sınırsız sayıda bulut cihazı oluşturabilir ve gerektiğinde açıp kapatabilirsiniz.
* Olağanüstü durum kurtarma, geliştirme ve test senaryolarında şirket içi ortamların simüle olmasına yardımcı olabilir ve yedeklemelerden madde düzeyinde alma konusunda yardımcı olabilir.

StorSimple Cloud Appliance iki model mevcuttur: 8010 cihaz (eski olarak bilinen 1100 modeli) ve 8020 cihaz. 8010 cihazı maksimum 30 TB kapasiteye sahiptir. Azure premium depolamadan yararlanan 8020 cihazı maksimum 64 TB kapasiteye sahiptir. (Yerel katmanlarda, Azure premium depolama alanı verileri SSD'lerde, standart depolama depolama alanı ise HDD'lerde depolar.) Premium depolama alanını kullanmak için bir Azure premium depolama hesabınız olması gerektiğini unutmayın.

StorSimple Cloud Appliance hakkında daha fazla bilgi için [Azure'da bir StorSimple Cloud Appliance'ı dağıt'a gidin ve yönetin.](storsimple-8000-cloud-appliance-u2.md)

## <a name="storsimple-device-manager-service"></a>StorSimple Device Manager hizmeti
Microsoft Azure StorSimple, veri merkezi ve bulut depolamayı merkezi olarak yönetmenize olanak tanıyan web tabanlı bir kullanıcı arabirimi (StorSimple Device Manager hizmeti) sağlar. Aşağıdaki görevleri gerçekleştirmek için StorSimple Device Manager hizmetini kullanabilirsiniz:

* StorSimple aygıtları için sistem ayarlarını yapılandırın.
* StorSimple aygıtları için güvenlik ayarlarını yapılandırın ve yönetin.
* Bulut kimlik bilgilerini ve özelliklerini yapılandırın.
* Sunucudaki birimleri yapılandırın ve yönetin.
* Birim gruplarını yapılandırın.
* Verileri yedekle ve geri yükleyin.
* Performansı izleyin.
* Sistem ayarlarını gözden geçirin ve olası sorunları tanımlayın.

StorSimple Device Manager hizmetini, sistem kapatma süresi gerektirenler dışında, ilk kurulum ve güncelleştirmelerin yüklenmesi gibi tüm yönetim görevlerini gerçekleştirmek için kullanabilirsiniz.

Daha fazla bilgi için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-8000-manager-service-administration.md)

## <a name="windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell
StorSimple için Windows PowerShell, Microsoft Azure StorSimple hizmetini oluşturmak ve yönetmek ve StorSimple aygıtlarını kurmak ve izlemek için kullanabileceğiniz bir komut satırı arabirimi sağlar. StorSimple cihazınızı yönetmek için özel cmdletler içeren Windows PowerShell tabanlı, komut satırı arabirimidir. StorSimple için Windows PowerShell şunları sağlar:

* Bir aygıt kaydettirin.
* Ağ arabirimini aygıtta yapılandırın.
* Belirli güncelleştirme türlerini yükleyin.
* Destek oturumuna erişerek aygıtınızın sorun giderin.
* Aygıt durumunu değiştirin.

StorSimple için Windows PowerShell'e seri konsoldan (doğrudan aygıta bağlı ana bilgisayarda) veya Windows PowerShell remoting'i kullanarak uzaktan erişebilirsiniz. StorSimple için bazı Windows PowerShell görevlerinin (örneğin, ilk aygıt kaydı) yalnızca seri konsolda yapılabileceğini unutmayın.

Daha fazla bilgi [için, cihazınızı yönetmek için StorSimple için Windows PowerShell'i kullanın'](storsimple-8000-windows-powershell-administration.md)a gidin.

## <a name="azure-powershell-storsimple-cmdlets"></a>Azure PowerShell StorBasit cmdlets
Azure PowerShell StorBasit cmdlets, komut satırından hizmet düzeyi ve geçiş görevlerini otomatikleştirmenize olanak tanıyan Windows PowerShell cmdletkoleksiyonudur. StorSimple için Azure PowerShell cmdlets hakkında daha fazla bilgi için [cmdlet referansına](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0)gidin.

## <a name="storsimple-snapshot-manager"></a>StorSimple Snapshot Manager
StorSimple Snapshot Manager, yerel ve bulut verilerinin tutarlı, zamanında yedekleme kopyalarını oluşturmak için kullanabileceğiniz bir Microsoft Yönetim Konsolu (MMC) snap-in'dir. Snap-in, Windows Server tabanlı bir ana bilgisayarda çalışır. StorSimple Snapshot Manager'ı şu şekilde kullanabilirsiniz:

* Birimleri yapılandırın, yedekleyin ve silin.
* Yedeklenen verilerin uygulama tutarlı olduğundan emin olmak için birim gruplarını yapılandırın.
* Verilerin önceden belirlenmiş bir zamanlamada yedeklenmeleri ve belirlenmiş bir konumda (yerel olarak veya bulutta) depolanabilmesi için yedekleme ilkelerini yönetin.
* Birimleri ve tek tek dosyaları geri yükleyin.

Yedeklemeler anlık görüntü olarak yakalanır ve bu da son anlık görüntünün alınmasından bu yana yalnızca değişiklikleri kaydeder ve tam yedeklemelerden çok daha az depolama alanı gerektirir. Yedekleme zamanlamaları oluşturabilir veya gerektiğinde anında yedekleme alabilirsiniz. Ayrıca, kaç anlık görüntü kaydedileceğini denetleyen bekletme ilkeleri oluşturmak için StorSimple Snapshot Manager'ı kullanabilirsiniz. Daha sonra bir yedeklemeden veri geri yüklemeniz gerekiyorsa, StorSimple Snapshot Manager yerel veya bulut anlık görüntüleri kataloğundan seçim yapmanıza olanak tanır. 

Bir felaket oluşursa veya başka bir nedenle verileri geri yüklemeniz gerekiyorsa, StorSimple Snapshot Manager gerektiğinde yeniden yüklenir. Veri geri yükleme, bir dosyayı geri yüklediğiniz, ekipmanı değiştirirken veya işlemleri başka bir siteye taşırken tüm sistemi kapatmanızı gerektirmez.

Daha fazla bilgi için [StorSimple Snapshot Manager nedir?'na gidin?](storsimple-what-is-snapshot-manager.md)

## <a name="storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Bağdaştırıcısı
Microsoft Azure StorSimple, StorSimple depolama ve veri koruma özelliklerini SharePoint sunucu çiftliklerine saydam olarak genişleten isteğe bağlı bir bileşen olan SharePoint için StorSimple Adapter'ı içerir. Bağdaştırıcı, Bir Uzak Blob Depolama (RBS) sağlayıcısı ve SQL Server RBS özelliğiyle çalışır ve BLOB'ları Microsoft Azure StorSimple sistemi tarafından yedeklenen bir sunucuya taşımanızı sağlar. Microsoft Azure StorSimple, kullanıma bağlı olarak BLOB verilerini yerel olarak veya bulutta saklar.

SharePoint için StorSimple Adaptörü SharePoint Merkezi Yönetim portalı içinden yönetilir. Sonuç olarak, SharePoint yönetimi merkezi olarak kalır ve tüm depolama SharePoint çiftliğinde gibi görünür.

Daha fazla bilgi [için SharePoint için StorSimple Adaptörü'ne](storsimple-adapter-for-sharepoint.md)gidin. 

## <a name="storage-management-technologies"></a>Depolama yönetimi teknolojileri
Özel StorSimple aygıtına, sanal aygıta ve diğer bileşenlere ek olarak, Microsoft Azure StorSimple verilere hızlı erişim sağlamak ve depolama tüketimini azaltmak için aşağıdaki yazılım teknolojilerini kullanır:

* [Otomatik depolama katmanı](#automatic-storage-tiering) 
* [Ölçülü kaynak sağlama](#thin-provisioning) 
* [Çoğaltma ve sıkıştırma](#deduplication-and-compression) 

### <a name="automatic-storage-tiering"></a>Otomatik depolama katmanı
Microsoft Azure StorSimple, verileri geçerli kullanıma, yaşa ve diğer verilerle ilişkiye bağlı olarak mantıksal katmanlarda otomatik olarak düzenler. En etkin olan veriler yerel olarak depolanırken, daha az etkin ve etkin olmayan veriler buluta otomatik olarak geçirilir. Aşağıdaki diyagram bu depolama yaklaşımını göstermektedir.

![StorBasit depolama katmanları](./media/storsimple-overview/hcs-data-services-storsimple-components-tiers.png)

Hızlı erişim sağlamak için StorSimple, SSD'lerde çok aktif verileri (sıcak veri) StorSimple aygıtında depolar. Aygıtta veya veri merkezindeki sunucularda HDD'lerde ara sıra kullanılan verileri (sıcak veriler) depolar. Etkin olmayan verileri, yedekleme verilerini ve arşivleme veya buluta uyumluluk amacıyla tutulan verileri taşır. 

> [!NOTE]
> Güncelleştirme 2 veya daha sonra, verilerin yerel aygıtta kalması ve buluta katmanlı olmaması durumunda, bir birim yerel olarak sabitlenmiş olarak belirtebilirsiniz. 


StorSimple, kullanım desenleri değiştikçe verileri ve depolama atamalarını ayarlar ve yeniden düzenler. Örneğin, bazı bilgiler zaman içinde daha az etkin hale gelebilir. Giderek daha az etkin hale geldikçe, SSD'lerden HDD'lere ve daha sonra buluta geçirilir. Aynı veriler yeniden etkin hale gelirse, depolama aygıtına geri taşınır.

Depolama katmanlama işlemi aşağıdaki gibi gerçekleşir:

1. Sistem yöneticisi bir Microsoft Azure bulut depolama hesabı kurar.
2. Yönetici, aygıtı ve dosya sunucusunu yapılandırmak ve hacimler ve veri koruma ilkeleri oluşturmak için seri konsolu ve StorSimple Device Manager hizmetini (Azure portalında çalışan) kullanır. Şirket içi makineler (dosya sunucuları gibi) StorSimple aygıtına erişmek için Internet Small Computer System Interface 'i (iSCSI) kullanır.
3. Başlangıçta StorSimple verileri aygıtın hızlı SSD katmanında depolar.
4. SSD katmanı kapasiteye yaklaştıkça, StorSimple en eski veri bloklarını çoğaltır ve sıkıştırır ve bunları HDD katmanına taşır.
5. HDD katmanı kapasiteye yaklaştıkça, StorSimple en eski veri bloklarını şifreler ve https aracılığıyla Güvenli bir şekilde Microsoft Azure depolama hesabına gönderir.
6. Microsoft Azure, veri merkezinde ve uzak bir veri merkezinde verilerin birden çok kopyasını oluşturarak, bir felaket oluşursa verilerin kurtarılmasını sağlar.
7. Dosya sunucusu bulutta depolanan verileri istediğinde, StorSimple verileri sorunsuz bir şekilde döndürür ve bir kopyasını StorSimple aygıtının SSD katmanında depolar.

#### <a name="how-storsimple-manages-cloud-data"></a>StorSimple bulut verilerini nasıl yönetir?

StorSimple, müşteri verilerini tüm anlık görüntüler ve birincil veriler (ana bilgisayarlar tarafından yazılmış veriler) arasında yineler. Çoğaltma depolama verimliliği için harika olsa da, "bulutta ne var" sorusunu karmaşık hale getirir. Katmanlı birincil veriler ve anlık görüntü verileri birbiriyle çakışıyor. Buluttaki tek bir veri yığını katmanlı birincil veri olarak kullanılabilir ve birkaç anlık görüntüyle de başvurulabilir. Her bulut anlık görüntüsü, anlık görüntü silinene kadar tüm zaman nokta verilerinin bir kopyasının buluta kilitolmasını sağlar.

Veriler yalnızca bu verilere başvuru olmadığında buluttan silinir. Örneğin, StorSimple aygıtındaki tüm verilerin bulut anlık görüntüsünü alıp bazı birincil verileri silersek, _birincil verilerin_ hemen düştüğünü görürsünüz. Katmanlı verileri ve yedeklemeleri içeren _bulut verileri_ aynı kalır. Bunun nedeni, bulut verilerine hala başvuran bir anlık görüntünün olmasıdır. Bulut anlık görüntüsü silindikten (ve aynı verilere başvuran diğer anlık görüntülerden) sonra, bulut tüketimi düşer. Bulut verilerini kaldırmadan önce, bu verilere hala başvuru pbulunmadığını kontrol ediyoruz. Bu işlem _çöp toplama_ olarak adlandırılır ve aygıtta çalışan bir arka plan hizmetidir. Çöp toplama hizmeti, silinmeden önce bu verilere yapılan diğer başvuruları denetlediği için bulut verilerinin kaldırılması hemen başlamaz. Çöp toplama nın hızı, anlık görüntünün toplam sayısına ve toplam veriye bağlıdır. Genellikle bulut verileri bir haftadan kısa bir sürede temizlenir.


### <a name="thin-provisioning"></a>Ölçülü kaynak sağlama
İnce sağlama, kullanılabilir depolamanın fiziksel kaynakları aştığı bir sanallaştırma teknolojisidir. StorSimple, yeterli depolama alanını önceden ayırmak yerine, geçerli gereksinimleri karşılamak için yeterli alan ayırmak için ince sağlama kullanır. StorSimple değişen talepleri karşılamak için bulut depolamayı artırabilir veya azaltabilir, çünkü bulut depolamanın esnek yapısı bu yaklaşımı kolaylaştırır.

> [!NOTE]
> Yerel olarak sabitlenmiş birimler ince olarak karşılanmaz. Yalnızca yerel bir hacme ayrılan depolama, birim oluşturulduğunda bütünüyle karşılanır.


### <a name="deduplication-and-compression"></a>Çoğaltma ve sıkıştırma
Microsoft Azure StorSimple, depolama gereksinimlerini daha da azaltmak için çoğaltma ve veri sıkıştırma kullanır.

Deduplication, depolanan veri kümesindeki artıklığı ortadan kaldırarak depolanan toplam veri miktarını azaltır. Bilgi değiştikçe, StorSimple değişmeyen verileri yoksa ve yalnızca değişiklikleri yakalar. Buna ek olarak, StorSimple, gereksiz bilgileri tanımlayarak ve kaldırarak depolanan veri miktarını azaltır. 

> [!NOTE]
> Yerel olarak sabitlenmiş birimlerdeki veriler çoğaltılamaz veya sıkıştırılanmaz. Ancak, yerel olarak sabitlenmiş birimlerin yedekleri çoğaltılır ve sıkıştırılır.


## <a name="storsimple-workload-summary"></a>StorBasit iş yükü özeti
Desteklenen StorSimple iş yüklerinin bir özeti aşağıda tablolanmıştır.

| Senaryo | İş yükü | Destekleniyor | Kısıtlamalar | Sürüm |
| --- | --- | --- | --- | --- |
| İş Birliği |Dosya paylaşımı |Evet | |Tüm sürümler |
| İş Birliği |Dağıtılmış dosya paylaşımı |Evet | |Tüm sürümler |
| İş Birliği |SharePoint |Evet* |Yalnızca yerel olarak sabitlenmiş birimlerle desteklenir |2 ve sonraki güncelleştirme |
| Arşiv |Basit dosya arşivleme |Evet | |Tüm sürümler |
| Sanallaştırma |Sanal makineler |Evet* |Yalnızca yerel olarak sabitlenmiş birimlerle desteklenir |2 ve sonraki güncelleştirme |
| Database |SQL |Evet* |Yalnızca yerel olarak sabitlenmiş birimlerle desteklenir |2 ve sonraki güncelleştirme |
| Video gözetimi |Video gözetimi |Evet* |StorSimple aygıtı yalnızca bu iş yüküne adandığında desteklenen |2 ve sonraki güncelleştirme |
| Backup |Birincil hedef yedekleme |Evet* |StorSimple aygıtı yalnızca bu iş yüküne adandığında desteklenen |3 ve sonraki güncelleştirme |
| Backup |İkincil hedef yedekleme |Evet* |StorSimple aygıtı yalnızca bu iş yüküne adandığında desteklenen |3 ve sonraki güncelleştirme |

*Evet&#42; - Çözüm yönergeleri ve kısıtlamaları uygulanmalıdır.*

Aşağıdaki iş yükleri StorSimple 8000 serisi aygıtlar tarafından desteklenmez. StorSimple'da dağıtılırsa, bu iş yükleri desteklenmeyen bir yapılandırmaya neden olur.

* Tıbbi görüntüleme
* Exchange
* Vdı
* Oracle
* SAP
* Büyük veriler
* İçerik dağıtımı
* SCSI'den önyükleme

Aşağıda StorSimple desteklenen altyapı bileşenlerinin bir listesi vetir.

| Senaryo | İş yükü | Destekleniyor | Kısıtlamalar | Sürüm |
| --- | --- | --- | --- | --- |
| Genel |Express Route |Evet | |Tüm sürümler |
| Genel |DataCore FC |Evet* |DataCore Senfoni ile desteklenir |Tüm sürümler |
| Genel |DFSR |Evet* |Yalnızca yerel olarak sabitlenmiş birimlerle desteklenir |Tüm sürümler |
| Genel |Dizin Oluşturma |Evet* |Katmanlı birimler için yalnızca meta veri dizini desteklenir (veri yok).<br>Yerel olarak sabitlenmiş birimler için tam dizin oluşturma desteklenir. |Tüm sürümler |
| Genel |Virüsten koruma |Evet* |Katmanlı birimler için yalnızca açık ve kapalı tarar desteklenir.<br> Yerel olarak sabitlenmiş birimler için tam taramaya destek verilir. |Tüm sürümler |

*Evet&#42; - Çözüm yönergeleri ve kısıtlamaları uygulanmalıdır.*

Aşağıda, çözüm oluşturmak için StorSimple ile birlikte kullanılan diğer yazılımların bir listesi verilmiştir.

| İş yükü türü | StorSimple ile kullanılan yazılım | Desteklenen sürümler|Çözüm kılavuzuna bağlantı| 
| --- | --- | --- | --- |
| Yedek hedef |Veeam |Veeam v 9 ve sonrası |[Veaam ile yedek hedef olarak StorSimple](storsimple-configure-backup-target-veeam.md)|
| Yedek hedef |Veritas Yedekleme Exec |Yedek Exec 16 ve sonrası |[Backup Exec ile yedek hedef olarak StorSimple](storsimple-configure-backup-target-using-backup-exec.md)|
| Yedek hedef |Veritas NetBackup |NetBackup 7.7.x ve sonrası  |[NetBackup ile yedek hedef olarak StorSimple](storsimple-configure-backuptarget-netbackup.md)|
| Genel Dosya Paylaşımı <br></br> İş Birliği |Pençe  |[Talon ile StorSimple](https://www.talonstorage.com/products/archive/fast-deployment-azure-storsimple) | |

## <a name="storsimple-terminology"></a>StorBasit terminoloji
Microsoft Azure StorSimple çözümünüzü dağıtmadan önce aşağıdaki terimleri ve tanımları gözden geçirmenizi öneririz.

### <a name="key-terms-and-definitions"></a>Anahtar terimler ve tanımlar
| Terim (Kısaltma veya kısaltma) | Açıklama |
| --- | --- |
| erişim kontrol kaydı (ACR) |Microsoft Azure StorSimple aygıtınızdaki bir birimle ilişkili ve hangi ana bilgisayarların bu aygıta bağlanabileceğini belirleyen bir kayıt. Bu belirleme, StorSimple cihazınıza bağlanan ana bilgisayarların (ACR'de bulunan) iSCSI Kalifiye Adı'na (IQN) dayanır. |
| AES-256 |Buluta gidip gelen verileri şifrelemek için 256 bit Gelişmiş Şifreleme Standardı (AES) algoritması. |
| ayırma birim boyutu (AUS) |Windows dosya sistemlerinizde bir dosyayı tutmak için ayrılabilecek en küçük disk alanı miktarı. Dosya boyutu küme boyutunun çift katı değilse, dosyayı tutmak için fazladan alan kullanılmalıdır (küme boyutunun bir sonraki katLarına kadar) bu da kayıp alan ve sabit diskin parçalanmasına neden olur. <br>Azure StorSimple birimleri için önerilen AUS, çoğaltma algoritmalarıyla iyi çalıştığından 64 KB'dir. |
| otomatik depolama katmanı |SSD'lerden HDD'lere ve ardından buluttaki bir katmana daha az aktif veri otomatik olarak taşımak ve ardından merkezi bir kullanıcı arabiriminden tüm depolama nın yönetimini etkinleştirmek. |
| yedek katalog |Genellikle kullanılan uygulama türüyle ilişkili yedekleme koleksiyonu. Bu koleksiyon, StorSimple Device Manager hizmeti Kullanıcı UI'nin Yedekleme Kataloğu'nda görüntülenir. |
| yedekleme kataloğu dosyası |Şu anda StorSimple Snapshot Manager'ın yedekleme veritabanında depolanan kullanılabilir anlık görüntü listesi içeren bir dosya. |
| yedekleme ilkesi |Birim seçimi, yedekleme türü ve önceden tanımlanmış bir zamanlamada yedekleme oluşturmanıza olanak tanıyan bir zaman çizelgesi. |
| ikili büyük nesneler (BLOBs) |Veritabanı yönetim sisteminde tek bir varlık olarak depolanan ikili veri topluluğu. BLOB'lar genellikle görüntüler, ses veya diğer multimedya nesneleridir, ancak bazen ikili yürütülebilir kod BLOB olarak depolanır. |
| Karşılıklı Kimlik Doğrulama Protokolü (CHAP) |Parola veya gizli paylaşım eşini temel alan bağlantı nın eşini doğrulamak için kullanılan bir iletişim kuralı. CHAP tek yönlü veya karşılıklı olabilir. Tek yönlü CHAP ile hedef, bir başlatıcıyı doğrular. Karşılıklı CHAP, hedefin başlatıcıyı doğrulamasını ve başlatıcının hedefi doğrulamasını gerektirir. |
| clone |Birimin yinelenen kopyası. |
| Katman Olarak Bulut (CaaT) |Tüm depolama nın tek bir kurumsal depolama ağının parçası gibi görünmesi için depolama mimarisinde katman olarak entegre edilen bulut depolama. |
| bulut hizmet sağlayıcısı (CSP) |Bulut bilgi işlem hizmetleri sağlayıcısı. |
| bulut anlık görüntüsü |Bulutta depolanan birim verilerin intime kopyası. Bulut anlık görüntüsü, farklı, site dışı depolama sisteminde çoğaltılan anlık görüntüye eşdeğerdir. Bulut anlık görüntüleri özellikle olağanüstü durum kurtarma senaryolarında yararlıdır. |
| bulut depolama şifreleme anahtarı |Aygıtınız tarafından buluta gönderilen şifrelenmiş verilere erişmek için StorSimple cihazınız tarafından kullanılan bir parola veya anahtar. |
| küme ye duyarlı güncelleme |Bir başarısız kümedeki sunucularda yazılım güncelleştirmelerini yönetme, böylece güncelleştirmelerin hizmet kullanılabilirliği üzerinde en az veya hiç etkisi yoktur. |
| veri yolu |Bağlı veri işleme işlemlerini gerçekleştiren işlevsel birimler topluluğu. |
| devre dışı bırak |StorSimple aygıtı ile ilişkili bulut hizmeti arasındaki bağlantıyı koparan kalıcı bir eylem. Aygıtın bulut anlık görüntüleri bu işlemden sonra kalır ve klonlanabilir veya olağanüstü durum kurtarma için kullanılabilir. |
| disk yansıtma |Sürekli kullanılabilirlik sağlamak için farklı sabit disklerde mantıksal disk birimlerinin gerçek zamanlı olarak çoğaltılması. |
| dinamik disk yansıtma |Dinamik disklerde mantıksal disk birimlerinin çoğaltılması. |
| dinamik diskler |Verileri birden çok fiziksel diskarasında depolamak ve yönetmek için Mantıksal Disk Yöneticisi'ni (LDM) kullanan bir disk hacmi biçimi. Dinamik diskler daha fazla boş alan sağlamak için büyütülebilir. |
| Genişletilmiş Disk Demeti (EBOD) muhafazası |Ek depolama alanı için ekstra sabit diskler içeren Microsoft Azure StorSimple cihazınızın ikincil bir muhafazası. |
| yağ sağlama |Depolama alanının beklenen ihtiyaçlar (ve genellikle mevcut ihtiyacın ötesinde) göre tahsis edildiği geleneksel bir depolama sağlama. Ayrıca *ince sağlama*bakın. |
| sabit disk sürücüsü (HDD) |Verileri depolamak için dönen plakaları kullanan bir sürücü. |
| hibrit bulut depolama |Bulut depolama da dahil olmak üzere yerel ve iş yeri dışı kaynakları kullanan bir depolama mimarisi. |
| Internet Küçük Bilgisayar Sistemi Arabirimi (iSCSI) |Veri depolama donanımı veya tesislerini bağlamak için Internet Protokolü (IP) tabanlı depolama ağı standardı. |
| iSCSI başlatıcısı |Windows çalıştıran bir ana bilgisayarın harici bir iSCSI tabanlı depolama ağına bağlanmasını sağlayan bir yazılım bileşeni. |
| iSCSI Kalifiye Adı (IQN) |iSCSI hedefini veya başlatıcıyı tanımlayan benzersiz bir ad. |
| iSCSI hedefi |Depolama alanı ağlarında merkezi iSCSI disk alt sistemleri sağlayan bir yazılım bileşeni. |
| canlı arşivleme |Arşiv verilerine her zaman erişilebildiği bir depolama yaklaşımı (örneğin, kayıt dışında bantta depolanmaz). Microsoft Azure StorSimple canlı arşivleme kullanır. |
| yerel olarak sabitlenmiş hacim |aygıtta bulunan ve hiçbir zaman buluta katmanlandırılamayan bir birim. |
| yerel anlık görüntü |Microsoft Azure StorSimple aygıtında depolanan birim verilerin intime kopyası. |
| Microsoft Azure StorSimple |VERI merkezi depolama aygıtı ve BT kuruluşlarının veri merkezi depolama sıyrıkları ymış gibi bulut depolamadan yararlanmalarını sağlayan yazılımdan oluşan güçlü bir çözüm. StorSimple, maliyetleri azaltırken veri korumave veri yönetimini kolaylaştırır. Çözüm, bulutla sorunsuz tümleştirme yoluyla birincil depolama, arşiv, yedekleme ve olağanüstü durum kurtarma (DR) birleştirir. StorSimple cihazları, SAN depolama ve bulut veri yönetimini kurumsal sınıf bir platformda birleştirerek, depolamayla ilgili tüm ihtiyaçlar için hız, basitlik ve güvenilirlik sağlar. |
| Güç ve Soğutma Modülü (PCM) |Güç kaynakları ve soğutma fanı, dolayısıyla adı Güç ve Soğutma modülü oluşan StorSimple cihazınızın donanım bileşenleri. Cihazın birincil kasası iki adet 764W PC'ye sahipken, EBOD kasada iki adet 580W PC MM vardır. |
| birincil muhafaza |Uygulama platformu denetleyicilerini içeren StorSimple cihazınızın ana muhafazası. |
| kurtarma süresi hedefi (RTO) |Bir iş süreci veya sistem bir felaketten sonra tamamen geri yüklenmeden önce harcanması gereken maksimum süre. |
| seri ekli SCSI (SAS) |Sabit disk sürücüsü türü (HDD). |
| hizmet veri şifreleme anahtarı |StorSimple Device Manager hizmetine kaydolan yeni StorSimple aygıtları için kullanılabilir hale getirilen anahtar. StorSimple Device Manager hizmeti ile aygıt arasında aktarılan yapılandırma verileri ortak anahtar kullanılarak şifrelenir ve daha sonra yalnızca özel bir anahtar kullanılarak aygıtta şifresi çözülebilir. Hizmet veri şifreleme anahtarı, hizmetin şifre çözme için bu özel anahtarı elde etmesini sağlar. |
| hizmet kayıt anahtarı |StorSimple aygıtının, daha fazla yönetim eylemi için Azure portalında görünmesi için StorSimple aygıtını StorSimple Aygıt Yöneticisi hizmetine kaydetmeye yardımcı olan bir anahtar. |
| Küçük Bilgisayar Sistemi Arabirimi (SCSI) |Bilgisayarları fiziksel olarak bağlamak ve aralarında veri aktarmak için bir dizi standart. |
| katı hal sürücüsü (SSD) |Hareketli parça içermeyen bir disk; örneğin, bir flash sürücü. |
| depolama hesabı |Belirli bir bulut hizmeti sağlayıcısı için depolama hesabınızla bağlantılı erişim kimlik bilgileri kümesi. |
| SharePoint için StorSimple Bağdaştırıcısı |StorSimple depolama ve veri korumasını SharePoint sunucu çiftliklerine saydam olarak genişleten bir Microsoft Azure StorSimple bileşeni. |
| StorSimple Device Manager hizmeti |Azure Portalı'nın azure StorSimple şirket içi ve sanal cihazlarınızı yönetmenize olanak tanıyan bir uzantısı. |
| StorSimple Snapshot Manager |Microsoft Azure StorSimple'da yedekleme ve geri yükleme işlemlerini yönetmek için bir Microsoft Yönetim Konsolu (MMC) tutturdu. |
| yedekleme almak |Kullanıcının bir birimin etkileşimli yedeklemesini almasını sağlayan bir özellik. Tanımlanmış bir ilke aracılığıyla otomatik yedekleme almak yerine bir birimin el ile yedekleme alma alternatif bir yoludur. |
| ince sağlama |Depolama sistemlerinde kullanılabilir depolama alanının kullanıldığı verimliliği optimize etme yöntemi. İnce sağlamada, depolama alanı, herhangi bir zamanda her kullanıcı tarafından gereken minimum alana göre birden çok kullanıcı arasında ayrılır. Ayrıca bakınız *yağ temini*. |
| katmanlama |Geçerli kullanıma, yaşa ve diğer verilerle ilişkiye göre mantıksal gruplandırmalarda veri düzenleme. StorSimple verileri katmanlar halinde otomatik olarak düzenler. |
| Ses düzeyi |Mantıksal depolama alanları sürücüler şeklinde sunulur. StorSimple birimleri, iSCSI ve StorSimple aygıtının kullanımı yla keşfedilenler de dahil olmak üzere ana bilgisayar tarafından monte edilen birimlere karşılık gelir. |
| hacim konteyner |Birimlerin ve bunlariçin geçerli ayarların gruplandırması. StorSimple cihazınızdaki tüm birimler birim kapsayıcılar halinde gruplandırılır. Birim kapsayıcı ayarları arasında depolama hesapları, ilişkili şifreleme anahtarlarıyla buluta gönderilen veriler için şifreleme ayarları ve bulutu içeren işlemler için tüketilen bant genişliği yer almaktadır. |
| hacim grubu |StorSimple Snapshot Manager'da, bir birim grubu yedekleme işlemini kolaylaştırmak için yapılandırılan birimler topluluğudur. |
| Birim Gölge Kopyalama Hizmeti (VSS) |Artan anlık görüntü oluşturmayı koordine etmek için VSS'ye duyarlı uygulamalarla iletişim kurarak uygulama tutarlılığını kolaylaştıran bir Windows Server işletim sistemi hizmeti. VSS, anlık görüntüler alındığında uygulamaların geçici olarak etkin olmamasını sağlar. |
| StorSimple için Windows PowerShell |StorSimple cihazınızı çalıştırmak ve yönetmek için kullanılan Windows PowerShell tabanlı komut satırı arabirimi. Windows PowerShell'in bazı temel özelliklerini korurken, bu arabirimde StorSimple aygıtını yönetmeye yönelik ek özel cmdletler bulunur. |

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple güvenliği](storsimple-8000-security.md)hakkında bilgi edinin.

