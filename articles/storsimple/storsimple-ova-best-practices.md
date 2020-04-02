---
title: StorSimple Virtual Array için en iyi uygulamalar | Microsoft Dokümanlar
description: StorSimple Virtual Array'i dağıtmak ve yönetmek için en iyi uygulamaları açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 57ac6eeb-c47c-442d-a5f4-b360d81a76a6
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/25/2019
ms.author: alkohli
ms.openlocfilehash: 82608c98fc8ea15167b690547906c2238b1b3c04
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80544336"
---
# <a name="storsimple-virtual-array-best-practices"></a>StorSimple Sanal Dizisi ile ilgili en iyi deneyimler

## <a name="overview"></a>Genel Bakış

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

Microsoft Azure StorSimple Virtual Array, bir hipervizörde çalışan şirket içi sanal aygıt ile Microsoft Azure bulut depolama alanı arasındaki depolama görevlerini yöneten entegre bir depolama çözümüdür. StorSimple Virtual Array, 8000 serisi fiziksel diziye etkili ve uygun maliyetli bir alternatiftir. Sanal dizi, mevcut hipervizör altyapınızda çalıştırılabilir, hem iSCSI hem de Kobİ protokollerini destekler ve uzak ofis/şube senaryoları için uygundur. StorSimple çözümleri hakkında daha fazla bilgi için [Microsoft Azure StorSimple Genel Bakış'a](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx)gidin.

Bu makale, StorSimple Virtual Array'in ilk kurulumu, dağıtımı ve yönetimi sırasında uygulanan en iyi uygulamaları kapsar. Bu en iyi uygulamalar, sanal dizinizin kurulumu ve yönetimi için doğrulanmış yönergeler sağlar. Bu makale, veri merkezlerinde sanal dizileri dağıtan ve yöneten BT yöneticilerine yöneliktir.

Kurulum veya işletim akışında değişiklikler yapıldığında cihazınızın hala uyumlu olduğundan emin olmak için en iyi uygulamaların periyodik olarak gözden geçirilmesini öneririz. Sanal dizinizde bu en iyi uygulamaları uygularken herhangi bir sorunla karşılaşırsanız, yardım için [Microsoft Destek'e başvurun.](storsimple-virtual-array-log-support-ticket.md)

## <a name="configuration-best-practices"></a>Yapılandırma en iyi uygulamaları
Bu en iyi uygulamalar, sanal dizilerin ilk kurulumu ve dağıtımı sırasında izlenmesi gereken yönergeleri kapsar. Bu en iyi uygulamalar, sanal makinenin sağlanması, grup ilkesi ayarları, boyutlandırma, ağ oluşturma, depolama hesaplarını yapılandırma ve sanal dizi için paylaşımlar ve birimler oluşturma ile ilgili uygulamaları içerir. 

### <a name="provisioning"></a>Sağlama
StorSimple Virtual Array, ana sunucunuzun hipervizöründe (Hyper-V veya VMware) sağlanan sanal bir makinedir (VM). Sanal makineyi sağlarken, ana makinenizin yeterli kaynakları ayırabilmesini sağlayın. Daha fazla bilgi için, bir dizi sağlamak için [minimum kaynak gereksinimlerine](storsimple-virtual-array-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-array-requirements) gidin.

Sanal diziyi uygularken aşağıdaki en iyi uygulamaları uygulayın:

|  | Hyper-V | VMware |
| --- | --- | --- |
| **Sanal makine türü** |**Nesil 2** Windows Server 2012 veya sonraki kullanım için VM ve *.vhdx* görüntü. <br></br> **Nesil 1** Windows Server 2008 veya sonraki bir ile ve *.vhd* görüntü ile kullanım için VM. |*.vmdk* görüntüsünü kullanırken sanal makine sürüm 8'i kullanın. |
| **Bellek türü** |**Statik bellek**olarak yapılandırın. <br></br> **Dinamik bellek** seçeneğini kullanmayın. | |
| **Veri diski türü** |Dinamik **olarak genişleyen**olarak sağlanması .<br></br> **Sabit boyutu** uzun zaman alır. <br></br> **Farklılaştırma** seçeneğini kullanmayın. |İnce **sağlama** seçeneğini kullanın. |
| **Veri diski modifikasyonu** |Genişletme veya küçültme izin verilmez. Bunu yapma girişimi, aygıttaki tüm yerel verilerin kaybolmasına neden olabilir. |Genişletme veya küçültme izin verilmez. Bunu yapma girişimi, aygıttaki tüm yerel verilerin kaybolmasına neden olabilir. |

### <a name="sizing"></a>Boyutlandırma
StorSimple Virtual Array'inizi boyutlandırmazken aşağıdaki faktörleri göz önünde bulundurun:

* Hacimler veya hisseler için yerel rezervasyon. Alanın yaklaşık %12'si, her verilen katmanlı hacim veya pay için yerel katmanda ayrılmıştır. Alanın kabaca %10'u da dosya sistemi için yerel olarak sabitlenmiş bir birim için ayrılmıştır.
* Anlık görüntü yükü. Yerel katmandaki kabaca %15 alan anlık görüntüler için ayrılmıştır.
* Geri yüklemelere ihtiyacım var. Yeni bir işlem olarak geri yükleme yapıyorsa, boyutlandırma geri yükleme için gereken alanı hesaba katmalıdır. Geri yükleme, aynı boyuttaki bir paylaşıma veya hacme yapılır.
* Beklenmeyen büyüme için bazı arabellek ayrılması gerekir.

Önceki etkenlere bağlı olarak, boyutlandırma gereksinimleri aşağıdaki denklemle temsil edilebilir:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`

Aşağıdaki örnekler, gereksinimlerinize göre sanal bir diziyi nasıl boyutlandırabileceğinizi göstermektedir.

#### <a name="example-1"></a>Örnek 1:
Sanal dizinizde,

* 2-TB katmanlı hacim veya pay sağlanması.
* 1-TB katmanlı hacim veya pay sağlanması.
* 300 GB'lık yerel olarak sabitlenmiş hacim veya pay sağlar.

Önceki birimler veya hisseler için, yerel katmandaki alan gereksinimlerini hesaplamamıza izin verin.

İlk olarak, her katmanlı hacim/pay için, yerel rezervasyon hacim/pay boyutunun %12'sine eşit olacaktır. Yerel olarak sabitlenmiş hacim/pay için yerel rezervasyon, yerel olarak sabitlenmiş hacim/pay boyutunun %10'udur (verilen boyuta ek olarak). Bu örnekte,

* 240 GB yerel rezervasyon (2-TB katmanlı hacim/pay için)
* 120 GB yerel rezervasyon (1-TB katmanlı hacim/pay için)
* Yerel olarak sabitlenmiş hacim veya pay için 330 GB (300 GB'lık verilen boyuta yerel rezervasyonun %10'u eklenmesi)

Yerel katmanda şu ana kadar gereken toplam alan: 240 GB + 120 GB + 330 GB = 690 GB.

İkinci olarak, en az yerel katmanda en büyük tek rezervasyon kadar alana ihtiyacımız var. Bu ek tutar, bulut anlık görüntüsünden geri yüklemeniz gerektiğinde kullanılır. Bu örnekte, en büyük yerel rezervasyon 330 GB 'dır (dosya sistemi için rezervasyon dahil), bu nedenle bunu 690 GB'a eklersiniz: 690 GB + 330 GB = 1020 GB.
Sonraki ek geri yüklemeleri gerçekleştirseydik, her zaman önceki geri yükleme işleminden alanı boşaltabiliriz.

Üçüncü olarak, yerel anlık görüntüleri depolamak için şimdiye kadar toplam yerel alanınızın %15'ine ihtiyacımız vardır, böylece yalnızca %85'i kullanılabilir. Bu örnekte, yaklaşık 1020 GB = 0,85&ast;verilen veri diski TB olacaktır. Yani, sağlanan veri diski (1020&ast;(1/0.85)=1200 GB = 1.20 TB ~ 1.25 TB (en yakın dörtte birlik yuvarlama) olacaktır

Beklenmeyen büyüme ve yeni geri yüklemeleri hesaba katarak, yaklaşık 1,25 - 1,5 TB yerel bir disk sağlamanız gerekir.

> [!NOTE]
> Ayrıca, yerel diskin ince bir şekilde sağlanmasını da öneririz. Bu öneri, geri yükleme alanının yalnızca beş günden eski verileri geri yüklemek istediğinizde gerekli olmasıdır. Öğe düzeyinde kurtarma, geri yükleme için ek alana gerek kalmadan son beş gün için verileri geri yüklemenize olanak tanır.


#### <a name="example-2"></a>Örnek 2:
Sanal dizinizde,

* 2-TB katmanlı hacim sağlanması
* 300 GB'lık yerel olarak sabitlenmiş bir hacim sağlama

Katmanlı hacimler/hisseler için yerel alan rezervasyonlarının %12'si ve yerel olarak sabitlenmiş hacimler/hisseler için %10'u temel alınabilmeniz

* 240 GB yerel rezervasyon (2 TB katmanlı hacim/pay için)
* Yerel olarak sabitlenmiş hacim veya pay için 330 GB (300 GB verilen alana yerel rezervasyonun %10'u eklenmesi)

Yerel katmanda gereken toplam alan: 240 GB + 330 GB = 570 GB

Geri yükleme için gereken minimum yerel alan 330 GB'dır.

Anlık görüntüleri depolamak için toplam diskinizin %15'i kullanılır, böylece yalnızca 0,85 kullanılabilir. Yani, disk boyutu (900&ast;(1/0.85)) = 1.06 TB ~ 1.25 TB (en yakın dörtte birlik yuvarlama)

Beklenmeyen büyümeyi hesaba katarak, 1,25 - 1,5 TB yerel disk sağlayabilirsiniz.

### <a name="group-policy"></a>Grup ilkesi
Grup İlkesi, kullanıcılar ve bilgisayarlar için belirli yapılandırmaları uygulamanızı sağlayan bir altyapıdır. Grup İlkesi ayarları, aşağıdaki Active Directory Etki Alanı Hizmetleri (AD DS) kapsayıcılarına bağlı grup ilkesi nesnelerinde (GPO) bulunur: siteler, etki alanları veya kuruluş birimleri (OS). 

Sanal diziniz etki alanına katıldıysa, GPO'lar buna uygulanabilir. Bu GDO'lar, StorSimple Virtual Array'in çalışmasını olumsuz etkileyebilen bir virüsten koruma yazılımı gibi uygulamaları yükleyebilir.

Bu nedenle, şunları yapmanızı öneririz:

* Sanal dizinizin Active Directory için kendi kuruluş biriminde (OU) olduğundan emin olun.
* Sanal dizinize grup ilkesi nesnelerinin (GPO' lar) uygulanmadığından emin olun. Sanal dizinin (alt düğüm) üst öğeden otomatik olarak gPO devralmadığından emin olmak için devralmayı engelleyebilirsiniz. Daha fazla bilgi [için, devralma yı engellemek](https://technet.microsoft.com/library/cc731076.aspx)için gidin.

### <a name="networking"></a>Ağ
Sanal dizinizin ağ yapılandırması yerel web web web ui üzerinden yapılır. Sanal dizinin sağlandığı hipervizör aracılığıyla sanal ağ arabirimi etkinleştirilir. Sanal ağ arabirimi IP adresini, alt ağ adresini ve ağ geçidini yapılandırmak için [Ağ Ayarları](storsimple-virtual-array-deploy3-fs-setup.md) sayfasını kullanın.  Cihazınız Için birincil ve ikincil DNS sunucusunu, zaman ayarlarını ve isteğe bağlı proxy ayarlarını da yapılandırabilirsiniz. Ağ yapılandırmasının çoğu tek seferlik bir kurulumdur. Sanal diziyi dağıtmadan önce [StorSimple ağ gereksinimlerini](storsimple-ova-system-requirements.md#networking-requirements) gözden geçirin.

Sanal dizinizi dağıtırken, aşağıdaki en iyi uygulamaları izlemenizi öneririz:

* Sanal dizinin dağıtıldığı ağın her zaman 5 Mbps Internet bant genişliği (veya daha fazla) ayırma kapasitesine sahip olduğundan emin olun.
  
  * Internet bant genişliği gereksinimi, iş yükü özelliklerinize ve veri değişim hızına bağlı olarak değişir.
  * İşlenebilir veri değişikliği, Internet bant genişliğinizle doğru orantılıdır. Bir yedek alırken örnek olarak, 5 Mbps bant genişliği 8 saat içinde yaklaşık 18 GB'lık bir veri değişimini barındırabilir. Dört kat daha fazla bant genişliği (20 Mbps) ile dört kat daha fazla veri değişikliği (72 GB) işleyebilirsiniz.
* Internet bağlantısının her zaman kullanılabilir olduğundan emin olun. Aygıtlara düzensiz veya güvenilir olmayan Internet bağlantıları buluttaki verilere erişimkaybına neden olabilir ve desteklenmeyen bir yapılandırmaya neden olabilir.
* Cihazınızı iSCSI sunucusu olarak dağıtmayı planlıyorsanız:
  
  * IP adresini otomatik olarak **al** seçeneğini (DHCP) devre dışı açıklamanızı öneririz.
  * Statik IP adreslerini yapılandırın. Birincil ve ikincil bir DNS sunucusu yapılandırmanız gerekir.
  * Sanal dizinizde birden çok ağ arabirimi tanımlıyorsanız, buluta yalnızca ilk ağ arabirimi (varsayılan olarak bu arabirim **Ethernet'tir)** ulaşabilir. Trafik türünü denetlemek için, sanal dizinizde (iSCSI sunucusu olarak yapılandırılan) birden çok sanal ağ arabirimi oluşturabilir ve bu arabirimleri farklı alt ağlara bağlayabilirsiniz.
* Bulut bant genişliğini yalnızca (sanal dizi tarafından kullanılan) daraltmak için, yönlendiricide veya güvenlik duvarında azaltmayı yapılandırın. Hipervizörünüzde azaltmayı tanımlarsanız, sadece bulut bant genişliği yerine iSCSI ve SMB dahil olmak üzere tüm protokolleri daraltır.
* Hipervizörler için zaman eşitlemesinin etkin olduğundan emin olun. Hyper-V kullanıyorsanız, Hyper-V Manager'da sanal dizinizi seçin, **Ayarlar &gt; Tümleştirme Hizmetleri'ne**gidin ve **Zaman eşitlemesinin** kontrol edildiğinden emin olun.

### <a name="storage-accounts"></a>Depolama hesapları
StorSimple Virtual Array tek bir depolama hesabıyla ilişkilendirilebilir. Bu depolama hesabı otomatik olarak oluşturulan bir depolama hesabı, hizmetle aynı abonelikteki bir hesap veya başka bir abonelikle ilgili bir depolama hesabı olabilir. Daha fazla bilgi için, [sanal dizinizin depolama hesaplarını](storsimple-virtual-array-manage-storage-accounts.md)nasıl yönetiniz gerektiğini görün.

Sanal dizinizle ilişkili depolama hesapları için aşağıdaki önerileri kullanın.

* Birden çok sanal diziyi tek bir depolama hesabıyla bağlarken, sanal dizi için maksimum kapasiteyi (64 TB) ve bir depolama hesabı için maksimum boyutu (500 TB) olarak katlayın. Bu, bu depolama hesabıyla ilişkilendirilebilen tam boyutlu sanal dizi lerin sayısını yaklaşık 7 ile sınırlar.
* Yeni bir depolama hesabı oluştururken
  
  * Gecikmeleri en aza indirmek için StorSimple Virtual Array'inizin dağıtıldığı uzak ofise/şube ofisine en yakın bölgede oluşturmanızı öneririz.
  * Bir depolama hesabını farklı bölgeler arasında taşıyamayacağınızı unutmayın. Ayrıca, bir hizmeti abonelikler arasında taşıyamazsınız.
  * Veri merkezleri arasında artıklık uygulayan bir depolama hesabı kullanın. Coğrafi Yedekli Depolama (GRS), Bölge Yedekli Depolama (ZRS) ve Yerel Yedekli Depolama (LRS) sanal dizinizle birlikte kullanılmak üzere desteklenir. Farklı depolama hesabı türleri hakkında daha fazla bilgi için [Azure depolama çoğaltma](../storage/common/storage-redundancy.md)bölümüne gidin.

### <a name="shares-and-volumes"></a>Hisse ler ve hacimler
StorSimple Virtual Array'iniz için, dosya sunucusu olarak yapılandırıldığında paylaşımlar ve iSCSI sunucusu olarak yapılandırıldığında hacimler sağlayabilirsiniz. Hisse ve birim oluşturmak için en iyi uygulamalar, yapılandırılan boyut ve türle ilgilidir.

#### <a name="volumeshare-size"></a>Birim/Paylaşım boyutu
Sanal dizinizde, dosya sunucusu olarak yapılandırıldığında paylaşımlar ve iSCSI sunucusu olarak yapılandırıldığında birimler sağlayabilirsiniz. Paylaşımlar ve birimler oluşturmak için en iyi uygulamalar, yapılandırılan boyut ve türle ilgilidir. 

Sanal cihazınızda hisse veya birim sağlarken aşağıdaki en iyi uygulamaları unutmayın.

* Katmanlı bir paylaşımın sağlanan boyutuna göre dosya boyutları katmanlama performansını etkileyebilir. Büyük dosyalarla çalışmak yavaş bir katman çıkmasına neden olabilir. Büyük dosyalarla çalışırken, en büyük dosyanın paylaşım boyutunun %3'ünden daha küçük olduğunu öneririz.
* Sanal dizide en fazla 16 birim/paylaşım oluşturulabilir. Yerel olarak sabitlenmiş ve katmanlı birimlerin/paylaşımların boyut sınırları için her zaman [StorSimple Virtual Array sınırlarına](storsimple-ova-limits.md)bakın.
* Bir hacim oluştururken, beklenen veri tüketiminin yanı sıra gelecekteki büyümeyi de hesaba katın. Birim veya pay daha sonra genişletilemez.
* Birim/paylaşım oluşturulduktan sonra StorSimple'daki birimin/paylaşımın boyutunu küçültemezsiniz.
* StorSimple'da katmanlı bir birimde yazarken, birim verileri belirli bir eşiğe ulaştığında (birim için ayrılmış yerel alana göre), IO daraltır. Bu sese yazmaya devam etmek IO'yu önemli ölçüde yavaşlatıyor. Katmanlı bir ses düzeyine, sağlanan kapasitenin ötesinde yazabiliyor olsanız da (kullanıcının verilen kapasitenin ötesinde yazmayı etkin olarak durdurmayız), aşırı abone olduğunuz etkiye ilişkin bir uyarı bildirimi görürsünüz. Uyarıyı bir kez gördüğünüzde, birim verilerini silme (birim genişletme şu anda desteklenmez) gibi düzeltici önlemler almanız zorunludur.
* Olağanüstü durum kurtarma kullanım durumlarında, izin verilen hisse/birim sayısı 16 ve paralel olarak işlenebilecek maksimum hisse/birim sayısı da 16 olduğundan, hisse/birim sayısı Nın RPO ve RPO'nuzla bir ilgisi yoktur.

#### <a name="volumeshare-type"></a>Hacim/Paylaş türü
StorSimple, kullanıma göre iki birim/paylaşım türünü destekler: yerel olarak sabitlenmiş ve katmanlı. Yerel olarak sabitlenmiş hacimler/hisseler kalın olarak karşılanırken, katmanlı hacimler/paylar ince bir şekilde karşılanır. Yerel olarak sabitlenmiş bir birimi/payı katmanlı veya *oluşturulduktan* sonra tam tersi olarak dönüştüremezsiniz.

StorSimple birimlerini/paylaşımlarını yapılandırırken aşağıdaki en iyi uygulamaları uygulamanızı öneririz:

* Birim oluşturmadan önce dağıtmayı planladığınız iş yüklerini temel alan birim türünü tanımlayın. Yerel veri garantileri (bulut kesintisi sırasında bile) gerektiren ve düşük bulut gecikmesi gerektiren iş yükleri için yerel olarak sabitlenmiş hacimleri kullanın. Sanal dizinizde bir birim oluşturduktan sonra, birim türünü yerel olarak sabitlenmiş ten katmanlı veya *tam tersi*olarak değiştiremezsiniz. Örnek olarak, SANAL makineler (VM) barındıran SQL iş yüklerini veya iş yüklerini dağıtırken yerel olarak sabitlenmiş birimler oluşturun; dosya paylaşımı iş yükleri için katmanlı birimleri kullanın.


#### <a name="volume-format"></a>Birim biçimi
iSCSI sunucunuzda StorSimple birimleri oluşturduktan sonra, birimleri başlatmanız, takmanız ve biçimlendirmeniz gerekir. Bu işlem StorSimple cihazınıza bağlı ana bilgisayarda gerçekleştirilir. StorSimple ana bilgisayara hacimleri monte ederken ve biçimlendirirken aşağıdaki en iyi uygulamalar önerilir.

* Tüm StorSimple birimlerinde hızlı bir biçim gerçekleştirin.
* StorSimple birimini biçimlendirirken, 64 KB'lik (varsayılan 4 KB) ayırma birimi boyutu (AUS) kullanın. 64 KB AUS, yaygın StorSimple iş yükleri ve diğer iş yükleri için şirket içinde yapılan testlere dayanır.
* iSCSI sunucusu olarak yapılandırılan StorSimple Virtual Array'i kullanırken, bu birimler veya diskler StorSimple tarafından desteklenmediği için yayılan birimleri veya dinamik diskleri kullanmayın.

#### <a name="share-access"></a>Erişimi paylaşma
Sanal dizi dosya sunucunuzda paylaşım oluştururken aşağıdaki yönergeleri izleyin:

* Paylaşım oluştururken, bir kullanıcı grubunu tek bir kullanıcı yerine paylaşım yöneticisi olarak atayın.
* Paylaşımlar Windows Gezgini üzerinden düzenleyerek paylaşım oluşturulduktan sonra NTFS izinlerini yönetebilirsiniz.

#### <a name="volume-access"></a>Birim erişim
StorSimple Virtual Array'unuzdaki iSCSI birimlerini yapılandırırken, gerektiğinde erişimi denetlemek önemlidir. Hangi ana bilgisayar sunucuların birimlere erişebileceğini belirlemek için erişim denetimi kayıtlarını (ACRs) StorSimple birimleriyle oluşturun ve ilişkilendirin.

StorSimple birimleri için ACR'ları yapılandırırken aşağıdaki en iyi uygulamaları kullanın:

* Her zaman en az bir ACR'yi bir ses düzeyiyle ilişkilendirin.

* Bir birimiçin birden fazla ACR atarken, birimin kümelenmemiş birden fazla ana bilgisayar tarafından aynı anda erişilebilen bir şekilde açıklanmadığından emin olun. Bir birim için birden çok ACR atadıysanız, yapılandırmanızı gözden geçirmeniz için bir uyarı iletisi açılır.

### <a name="data-security-and-encryption"></a>Veri güvenliği ve şifreleme
StorSimple Virtual Array'iniz, verilerinizin gizliliğini ve bütünlüğünü sağlayan veri güvenliği ve şifreleme özelliklerine sahiptir. Bu özellikleri kullanırken, aşağıdaki en iyi uygulamaları izlemeniz önerilir: 

* Veriler sanal dizinizden buluta gönderilmeden önce AES-256 şifrelemesi oluşturmak için bir bulut depolama şifreleme anahtarı tanımlayın. Verileriniz başlangıçta şifrelenmişse bu anahtar gerekli değildir. Anahtar, [Azure anahtar kasası](../key-vault/key-vault-overview.md)gibi bir anahtar yönetim sistemi kullanılarak oluşturulabilir ve güvende tutulabilir.
* Depolama hesabını StorSimple Manager hizmeti üzerinden yapılandırırken, TLS modunu etkinleştirerek StorSimple aygıtınız ile bulut arasında ağ iletişimi için güvenli bir kanal oluşturduğunuzdan emin olun.
* Değiştirilen yönetici listesine dayalı olarak erişimde yapılan değişiklikleri hesaba katmak için depolama hesaplarınız için anahtarları (Azure Depolama hizmetine erişerek) düzenli aralıklarla yeniden oluşturun.
* Sanal dizinizdeki veriler Azure'a gönderilmeden önce sıkıştırılır ve çoğaltılır. Windows Server ana bilgisayarınızda Veri Çoğaltma rol hizmetini kullanmanızı önermiyoruz.

## <a name="operational-best-practices"></a>Operasyonel en iyi uygulamalar
Operasyonel en iyi uygulamalar, sanal dizinin günlük yönetimi veya çalışması sırasında izlenmesi gereken yönergelerdir. Bu uygulamalar, yedekleme alma, yedekleme kümesinden geri getirme, bir hata yapma, diziyi devre dışı bırakma ve silme, sistem kullanımını ve durumunu izleme ve sanal dizinizde virüs taramaları çalıştırma gibi belirli yönetim görevlerini kapsar.

### <a name="backups"></a>Yedeklemeler
Sanal dizinizdeki veriler buluta iki şekilde yedeklenir, saat 22:30'dan itibaren tüm cihazın varsayılan otomatik günlük yedeklemesi veya isteğe bağlı manuel yedekleme yoluyla. Varsayılan olarak, aygıt üzerinde bulunan tüm verilerin günlük bulut anlık görüntülerini otomatik olarak oluşturur. Daha fazla bilgi için [StorSimple Sanal Dizinizi yedeklemeye](storsimple-virtual-array-backup.md)gidin.

Varsayılan yedeklemelerle ilişkili sıklık ve bekletme değiştirilemez, ancak günlük yedeklemelerin her gün başlatıldığı zamanı yapılandırabilirsiniz. Otomatik yedeklemelerin başlangıç saatini yapılandırırken şunları öneririz:

* Yedeklemelerinizi yoğun olmayan saatler için zamanlayın. Yedekleme başlangıç süresi çok sayıda ana bilgisayar IO ile çakışmamalıdır.
* Sanal dizinizdeki verileri korumak için bir aygıtın arızasını gerçekleştirmeyi planlarken veya bakım penceresinden önce isteğe bağlı yedekleme başlatın.

### <a name="restore"></a>Geri Yükleme
Yedekleme kümesinden iki şekilde geri yükleyebilirsiniz: başka bir büyük sese geri yükleme veya madde düzeyinde kurtarma gerçekleştirin (yalnızca dosya sunucusu olarak yapılandırılan sanal bir dizide kullanılabilir). Öğe düzeyinde kurtarma, StorSimple aygıtındaki tüm paylaşımların bulut yedeklemesinden dosya ve klasörlerin parçalı bir şekilde kurtarılmasını sağlar. Daha fazla bilgi [için, bir yedekleme geri yüklemek](storsimple-virtual-array-clone.md)için gidin.

Geri yükleme yaparken, aşağıdaki yönergeleri göz önünde bulundurun:

* StorSimple Virtual Array'iniz yerinde geri yüklemeyi desteklemez. Ancak bu, iki aşamalı bir işlemle kolayca elde edilebilir: sanal dizide yer açmak ve sonra başka bir topluluğa/paylaşıma geri yükleyin.
* Yerel bir birimden geri yükleme yaparken, geri yüklemenin uzun süren bir işlem olacağını unutmayın. Ses hızla çevrimiçi olabilir rağmen, veri arka planda sulu olmaya devam ediyor.
* Ses düzeyi türü geri yükleme işlemi sırasında aynı kalır. Katmanlı birim, başka bir katmanlı topluya ve yerel olarak sabitlenmiş bir birimde yerel olarak sabitlenmiş başka bir ses birimine geri yüklenir.
* Yedekleme kümesinden bir birim veya paylaşım geri yüklemeye çalışırken, geri yükleme işi başarısız olursa, portalda bir hedef birim veya paylaşım oluşturulabilir. Bu öğeden kaynaklanan gelecekteki sorunları en aza indirmek için bu kullanılmayan hedef hacmini silmeniz veya portaldaki paylaşımınız önemlidir.

### <a name="failover-and-disaster-recovery"></a>Failover ve olağanüstü durum kurtarma
Aygıt başarısız olması, verilerinizi veri merkezindeki bir *kaynak* aygıttan aynı veya farklı bir coğrafi konumda bulunan başka bir *hedef* aygıta geçirmenize olanak tanır. Cihaz arızası tüm aygıt içindir. Başarısız olma sırasında, kaynak aygıtın bulut verileri sahipliğini hedef aygıtın sahipliğini değiştirir.

StorSimple Virtual Array'iniz için, yalnızca aynı StorSimple Manager hizmeti tarafından yönetilen başka bir sanal dizide başarısız olabilirsiniz. 8000 serisi bir aygıta veya farklı bir StorSimple Manager hizmeti (kaynak aygıt için olandan daha fazla) tarafından yönetilen bir diziye geçilemeye izin verilmez. Failover hususlar hakkında daha fazla bilgi edinmek [için, cihaz failover için ön koşullara](storsimple-virtual-array-failover-dr.md)gidin.

Sanal diziniz için bir başarısızlık gerçekleştirirken aşağıdakileri aklınızda bulundurun:

* Planlı bir hata için, başarısızlığını başlatmadan önce tüm birimleri/paylaşımları çevrimdışı naalmak en iyi yöntemdir. Önce ana bilgisayardaki birimleri/paylaşımları çevrimdışı naalmak ve ardından sanal cihazınızdakileri çevrimdışı hale getirmek için işletim sistemine özgü yönergeleri izleyin.
* Bir dosya sunucusu olağanüstü durum kurtarma (DR) için, paylaşım izinlerinin otomatik olarak çözülmesi için hedef aygıta kaynakla aynı etki alanına katılmanızı öneririz. Bu sürümde yalnızca aynı etki alanında hedef bir aygıta yapılan başarısızlık desteklenir.
* DR başarıyla tamamlandıktan sonra, kaynak aygıt otomatik olarak silinir. Aygıt artık kullanılamasa da, ana bilgisayar sisteminde sağlanan sanal makine hala kaynak tüketiyor. Herhangi bir ücret tahakkuk etmesini önlemek için bu sanal makineyi ana bilgisayar sisteminizden silmenizi öneririz.
* Failover başarısız olsa bile, **verilerin bulutta her zaman güvenli olduğunu**unutmayın. Başarısızın başarıyla tamamlanamadığı aşağıdaki üç senaryoyu göz önünde bulundurun:
  
  * Dr ön kontrollerinin yapıldığı gibi, başarısızlığın ilk aşamalarında bir hata oluştu. Bu durumda, hedef aygıtınız hala kullanılabilir. Failover'ı aynı hedef aygıtta yeniden deneyebilirsiniz.
  * Gerçek hata işlemi sırasında bir hata oluştu. Bu durumda, hedef aygıt kullanılamaz olarak işaretlenir. Başka bir hedef sanal dizi sağlama ve yapılandırma ve başarısız olmak için kullanmanız gerekir.
  * Kaynak aygıtın silindiği ancak hedef aygıtın sorunları olduğu ve herhangi bir veriye erişemediğiniz başarısız olması nın ardından hata tamamlandı. Veriler bulutta hala güvenlidir ve başka bir sanal dizi oluşturup DR için hedef aygıt olarak kullanılarak kolayca alınabilir.

### <a name="deactivate"></a>Devre dışı bırak
Bir StorSimple Virtual Array'i devre dışı bıraktığınızda, aygıt la ilgili StorSimple Manager hizmeti arasındaki bağlantıyı kesersiniz. Devre dışı bırakma **kalıcı** bir işlemdir ve geri alınamaz. Devre dışı bırakılan aygıt, StorSimple Manager hizmetine yeniden kaydedilemez. Daha fazla bilgi için [StorSimple Virtual Array'inizi devre dışı bırakın ve silin.](storsimple-virtual-array-deactivate-and-delete-device.md)

Sanal dizinizi devre dışı dışı kalırken aşağıdaki en iyi uygulamaları aklınızda bulundurun:

* Sanal bir aygıtı devre dışı açıklamadan önce tüm verilerin bulut anlık görüntüsünü alın. Sanal bir diziyi devre dışı bıraktığınızda, tüm yerel aygıt verileri kaybolur. Bulut anlık görüntüsünü almak, daha sonraki bir aşamada verileri kurtarmanızı sağlar.
* StorSimple Sanal Dizisini devre dışı bırakmadan önce, bu aygıta bağlı olan istemcileri ve ana bilgisayarları durdurduğunuzdan veya sildiğinizden emin olun.
* Artık ücret tahakkuk etmemesi için kullanmıyorsanız devre dışı bırakılan aygıtı silin.

### <a name="monitoring"></a>İzleme
StorSimple Virtual Array'inizin sürekli sağlıklı bir durumda olduğundan emin olmak için diziyi izlemeniz ve uyarılar da dahil olmak üzere sistemden bilgi aldığınızdan emin olmanız gerekir. Sanal dizinin genel durumunu izlemek için aşağıdaki en iyi uygulamaları uygulayın:

* Sanal dizi veri diskinizin ve işletim sistemi diskinizin disk kullanımını izlemek için izlemeyi yapılandırın. Hyper-V çalıştırıyorsanız, sanallaştırma ana bilgisayarlarınızı izlemek için System Center Virtual Machine Manager (SCVMM) ve System Center Operations Manager'ın birleşimini kullanabilirsiniz.
* Belirli kullanım düzeylerinde uyarı göndermek için sanal dizinizdeki e-posta bildirimlerini yapılandırın.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Dizin arama ve virüs taraması uygulamaları
StorSimple Virtual Array, verileri yerel katmandan Microsoft Azure bulutuna otomatik olarak katmanlayabilir. StorSimple'da depolanan verileri taramak için dizin araması veya virüs taraması gibi bir uygulama kullanıldığında, bulut verilerine erişilmediğine ve yerel katmana geri çekilmemesine dikkat etmeliyim.

Sanal dizinizde dizin aramasını veya virüs taraması yapılandırırken aşağıdaki en iyi uygulamaları uygulamanızı öneririz:

* Otomatik olarak yapılandırılan tam tbmkasyon işlemlerini devre dışı kullanabilirsiniz.
* Katmanlı birimler için, artımlı bir tarama gerçekleştirmek için dizin arama veya virüs tarama uygulamasını yapılandırın. Bu, yalnızca yerel katmanda bulunan yeni verileri taykişi. Buluta katmanlı verilere artımlı bir işlem sırasında erişilemez.
* Yalnızca amaçlanan dosya türlerinin taranması için doğru arama filtrelerinin ve ayarlarının yapılandırıldığından emin olun. Örneğin, görüntü dosyaları (JPEG, GIF ve TIFF) ve mühendislik çizimleri artımlı veya tam dizin yeniden oluşturma sırasında taranmamalıdır.

Windows dizin oluşturma işlemini kullanıyorsanız, aşağıdaki yönergeleri izleyin:

* Dizin sık sık yeniden oluşturulması gerekiyorsa, buluttan büyük miktarda veri (TB) geri çağırır gibi katmanlı birimler için Windows Dizinleyici kullanmayın. Dizin yeniden oluşturmak, içeriklerini dizine eklemek için tüm dosya türlerini alır.
* Bu yalnızca dizin oluşturmak için yerel katmanlarda verilere erişeceği için yerel olarak sabitlenmiş birimler için Windows dizin oluşturma işlemini kullanın (bulut verilerine erişilmez).

### <a name="byte-range-locking"></a>Bayt aralığı kilitleme
Uygulamalar, dosyaların içinde belirli bir bayt aralığını kilitleyebilir. StorSimple'ınıza yazan uygulamalarda bayt aralığı kilitlemesi etkinse, katmanlama sanal dizinizde çalışmaz. Katmanlamanın çalışması için, erişilen dosyaların tüm alanlarının kilidi açılmalıdır. Bayt aralığı kilitleme, sanal dizinizdeki katmanlı birimlerle desteklenmez.

Bayt aralığı kilitlemeyi hafifletmek için önerilen önlemler şunlardır:

* Uygulama mantığınızda bayt aralığı kilitlemeyi kapatın.
* Bu uygulamayla ilişkili veriler için yerel olarak sabitlenmiş birimleri (katmanlı yerine) kullanın. Yerel olarak sabitlenmiş birimler buluta katman lanmaz.
* Bayt aralığı kilitleme etkin olan yerel olarak sabitlenmiş birimleri kullanırken, geri yükleme tamamlanmadan önce birim çevrimiçi olabilir. Bu gibi durumlarda, geri yüklemenin tamamlanmasını beklemeniz gerekir.

## <a name="multiple-arrays"></a>Birden çok dizi
Buluta dökülebilecek ve böylece aygıtın performansını etkileyebilecek büyüyen bir veri kümesini hesaba katmak için birden çok sanal dizinin dağıtılması gerekebilir. Bu gibi durumlarda, çalışma kümesi büyüdükçe aygıtları ölçeklendirmek en iyisidir. Bu, şirket içi veri merkezine bir veya daha fazla aygıtın eklenmesini gerektirir. Aygıtları eklerken şunları

* Geçerli veri kümesini bölün.
* Yeni aygıt(lar) için yeni iş yüklerini dağıtın.
* Birden çok sanal dizi dağıtıyorsanız, yük dengeleme perspektifinden diziyi farklı hipervizör ana bilgisayarlarına dağıtmanızı öneririz.
* Dağıtılmış Dosya Sistemi Ad alanında birden çok sanal dizi (dosya sunucusu veya iSCSI sunucusu olarak yapılandırıldığınızda) dağıtılabilir. Ayrıntılı adımlar [için, Karma Bulut Depolama Dağıtım Kılavuzu ile Dağıtılmış Dosya Sistemi Ad Alanı Çözümü'ne](https://www.microsoft.com/download/details.aspx?id=45507)gidin. Dağıtılmış Dosya Sistemi Çoğaltma şu anda sanal dizi ile kullanmak için tavsiye edilmez. 

## <a name="see-also"></a>Ayrıca bkz.
StorSimple Manager hizmeti aracılığıyla [StorSimple Virtual Array'inizi nasıl yöneteceklerinizi](storsimple-virtual-array-manager-service-administration.md) öğrenin.

