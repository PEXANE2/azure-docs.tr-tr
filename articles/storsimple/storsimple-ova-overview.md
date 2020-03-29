---
title: Microsoft Azure StorBasit Sanal Dizi'ye genel bakış
description: Şirket içi sanal dizi ile Microsoft Azure bulut depolama sı arasında depolama görevlerini yöneten entegre bir depolama çözümü olan StorSimple Virtual Array'i açıklar.
author: alkohli
ms.assetid: 169c639b-1124-46a5-ae69-ba9695525b77
ms.service: storsimple
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: alkohli
ms.openlocfilehash: 32781a83aec996b23f161f5fe695f39a0de38685
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76273879"
---
# <a name="introduction-to-the-storsimple-virtual-array"></a>StorSimple Sanal Diziye Giriş

[!INCLUDE [storsimple-virtual-array-eol-banner](../../includes/storsimple-virtual-array-eol-banner.md)]

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorBasit Sanal Dizi, bir hipervizörde çalışan şirket içi sanal dizi ile Microsoft Azure bulut depolama alanı arasında depolama görevlerini yöneten entegre bir depolama çözümüdür. Sanal dizi, kurumsal depolama ve veri korumasıyla ilişkili sorunların ve giderlerin çoğunu ortadan kaldıran verimli, uygun maliyetli ve kolay yönetilen bir dosya sunucusu veya iSCSI sunucusu çözümüdür. Sanal dizi, özellikle seyrek erişilen arşiv verilerinin depolanması için uygundur.

Bu makalede, sanal dizi genel bir bakış sağlar - burada diğer bazı kaynaklar şunlardır:

* En iyi uygulamalar için [StorSimple Virtual Array en iyi uygulamaları](storsimple-ova-best-practices.md)görün.
* StorSimple 8000 serisi cihazlara genel bir bakış için [StorSimple 8000 serisine gidin: karma bulut çözümü.](storsimple-overview.md)
* StorSimple 5000/7000 serisi cihazlar hakkında bilgi için [StorSimple Online Help'e](http://onlinehelp.storsimple.com/)gidin.

Sanal dizi iSCSI veya Server Message Block (SMB) protokolünü destekler. Mevcut hipervizör altyapınızda çalışır ve buluta katmanlama, bulut yedeklemesi, hızlı geri yükleme, öğe düzeyinde kurtarma ve olağanüstü durum kurtarma özellikleri sağlar.

Aşağıdaki tablo, StorSimple Virtual Array'in önemli özelliklerini özetleyin.

| Özellik | StorSimple Sanal Dizisi |
| --- | --- |
| Yükleme gereksinimleri |Sanallaştırma altyapılarını kullanır (Hyper-V veya VMware) |
| Kullanılabilirlik |Tek düğüm |
| Toplam kapasite (bulut dahil) |Sanal dizi başına 64 TB'a kadar kullanılabilir kapasite |
| Yerel kapasite |Sanal dizi başına 390 GB ila 6,4 TB kullanılabilir kapasite (500 GB ile 8 TB disk alanı sağlamanız gerekir) |
| Yerel protokoller |iSCSI veya Kobİ |
| Kurtarma süresi hedefi (RTO) |iSCSI: boyutu ne olursa olsun 2 dakikadan az |
| Kurtarma noktası hedefi (RPO) |Günlük yedeklemeler ve isteğe bağlı yedeklemeler |
| Depolama katmanlama |Hangi verilerin katmanlandırılmalı veya dışarı edilmesi gerektiğini belirlemek için ısı eşleme kullanır |
| Destek |Tedarikçi tarafından desteklenen sanallaştırma altyapısı |
| Performans |Temel altyapıya bağlı olarak değişir |
| Veri hareketliliği |Aynı aygıta geri yüklenebilir veya öğe düzeyinde kurtarma yapabilir (dosya sunucusu) |
| Depolama katmanları |Yerel hipervizör depolama ve bulut |
| Paylaşım boyutu |Katmanlı: 20 TB'a kadar; yerel olarak sabitlenmiş: en fazla 2 TB |
| Birim boyutu |Katmanlı: 500 GB - 5 TB; yerel olarak sabitlenmiş: 50 GB - 200 GB <br> Katmanlı birimler için maksimum yerel rezervasyon 200 GB'dır. |
| Anlık Görüntüler |Kilitlenmeyle tutarlı |
| Öğe düzeyinde kurtarma |Evet; kullanıcılar paylaşımlardan geri yükleyebilir |

## <a name="why-use-storsimple"></a>Neden StorSimple kullanın?

StorSimple, kullanıcıları ve sunucuları uygulama değişikliği olmadan dakikalar içinde Azure depolamaalanına bağlar.

Aşağıdaki tabloda, StorSimple Virtual Array çözümünün sağladığı bazı temel avantajlar açıklanmaktadır.

| Özellik | Avantaj |
| --- | --- |
| Saydam entegrasyon |Sanal dizi iSCSI veya Kobİ protokolünü destekler. Yerel katman ve bulut katmanı arasındaki veri hareketi kullanıcı için sorunsuz ve saydamdır. |
| Daha düşük depolama maliyetleri |StorSimple ile, en çok kullanılan sıcak veriler için geçerli talepleri karşılamak için yeterli yerel depolama sağlarsınız. Depolama ihtiyaçları arttıkça, StorSimple soğuk verileri uygun maliyetli bulut depolamaalanına aktarır. Veriler, depolama gereksinimlerini ve gideri daha da azaltmak için buluta gönderilmeden önce çoğaltılır ve sıkıştırılır. |
| Basitleştirilmiş depolama yönetimi |StorSimple, birden çok cihazı yönetmek için StorSimple Device Manager'ı kullanarak bulutta merkezi yönetim sağlar. |
| Geliştirilmiş olağanüstü durum kurtarma ve uyumluluk |StorSimple, meta verileri hemen geri vererek ve gerektiğinde verileri geri alarak daha hızlı olağanüstü durum kurtarmayı kolaylaştırır. Bu, normal operasyonların en az kesintiyle devam edebileceği anlamına gelir. |
| Veri hareketliliği |Buluta katmanlı verilere kurtarma ve geçiş amacıyla diğer sitelerden erişilebilir. Verileri yalnızca orijinal sanal diziye geri yükleyebileceğinizi unutmayın. Ancak, tüm sanal diziyi başka bir sanal diziye geri yüklemek için olağanüstü durum kurtarma özelliklerini kullanırsınız. |

## <a name="storsimple-workload-summary"></a>StorBasit iş yükü özeti

Desteklenen StorSimple iş yüklerinin bir özeti aşağıda tablolanmıştır.

|Senaryo     |İş yükü     |Destekleniyor      |Kısıtlamalar               | Uygulanabilir sürümler|
|-------------|-------------|---------------|---------------------------|--------------------|
|Uzak Ofis/Şube Müdürlüğü (ROBO)  |Dosya paylaşımı     |Evet      |[Dosya sunucusu için maksimum sınırlara](storsimple-ova-limits.md)bakın.<br></br>[Desteklenen SMB sürümleri için sistem gereksinimlerine](storsimple-ova-system-requirements.md)bakın.| Tüm sürümler     |
|Bulut arşivleme  |Arşiv dosya paylaşımı     |Evet      |[Dosya sunucusu için maksimum sınırlara](storsimple-ova-limits.md)bakın.<br></br>[Desteklenen SMB sürümleri için sistem gereksinimlerine](storsimple-ova-system-requirements.md)bakın.| Tüm sürümler     |

StorSimple Virtual Array, seyrek erişilen veriler için en uygun olandır. Sanal dizinin performansı artırmak için yerel bir önbelleği olsa da, kullanıcılar aygıt hizmetleri dosyalarının en düşük depolama katmanında (bulut) olduğunu varsaymalıdır. Her sanal dizi yaklaşık 100 Mbps hızında Azure depolamasına yazabilir ve okuyabilir. Bu bağlantı cihaza gelen tüm istekler arasında paylaşılır ve aşağıdaki diyagramda gösterildiği gibi bir darboğaz alabilirsiniz.

![Bulut arşivleme](./media/storsimple-ova-overview/cloud-archiving.png)

Birden çok eşzamanlı kullanıcı sanal diziye eriştiğinde, hepsi Azure bağlantısını paylaşır ve daha düşük bir performansa yol açarak. Kullanıcı başına garantili bir performans yoktur ve cihaz tek tek istekleri geldiklerinde işler.

StorSimple Virtual Array, yüksek kullanılabilirlik gerektiren iş yükleri için uygun değildir. Sanal dizi, yazılım güncelleştirmeleri yüklendiğinde kapalı kalma süresiyle karşılanan tek bir düğüm aygıtıdır. Yöneticiler yılda 30 dakika 3-4 kez bir bakım penceresi için plan lamalıdır.

## <a name="workflows"></a>İş akışları

StorSimple Sanal Dizi özellikle aşağıdaki iş akışları için uygundur:

* [Bulut tabanlı depolama yönetimi](#cloud-based-storage-management)
* [Konumbağımsız yedekleme](#location-independent-backup)
* [Veri koruma ve olağanüstü durum kurtarma](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Bulut tabanlı depolama yönetimi
Azure portalında çalışan StorSimple Device Manager hizmetini birden çok aygıtta ve birden çok konumda depolanan verileri yönetmek için kullanabilirsiniz. Bu, özellikle dağıtılmış dal senaryolarında yararlıdır. Sanal dizileri ve fiziksel StorSimple aygıtlarını yönetmek için StorSimple Device Manager hizmetinin ayrı örnekleri oluşturmanız gerektiğini unutmayın. Ayrıca sanal dizinin artık Azure klasik portalı yerine yeni Azure portalını kullandığını da unutmayın.

![bulut tabanlı depolama yönetimi](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Konumbağımsız yedekleme
Sanal diziyle bulut anlık görüntüleri, bir birimin veya paylaşımın konumbağımsız, zaman içinde noktada nisken kopyasını sağlar. Bulut anlık görüntüleri varsayılan olarak etkinleştirilir ve devre dışı sürülemiyor. Tüm birimler ve paylaşımlar tek bir günlük yedekleme ilkesi yle aynı anda yedeklenir ve gerektiğinde ek geçici yedeklemeler alabilirsiniz.

### <a name="data-protection-and-disaster-recovery"></a>Veri koruma ve olağanüstü durum kurtarma
Sanal dizi aşağıdaki veri koruma ve olağanüstü durum kurtarma senaryolarını destekler:

* **Ses veya paylaşım geri yüklemesi** – Bir birim veya paylaşım kurtarmak için geri yüklemeyi yeni iş akışı olarak kullanın. Tüm hacmi veya payı kurtarmak için bu yaklaşımı kullanın.
* **Öğe düzeyinde kurtarma** – Paylaşımlar, son yedeklemelere basitleştirilmiş erişim sağlar. Bulutta bulunan özel bir *.yedekleme* klasöründen tek bir dosyayı kolayca kurtarabilirsiniz. Bu geri yükleme özelliği kullanıcı tarafından yönlendirilir ve hiçbir yönetim müdahalesi gerekmez.
* **Olağanüstü durum kurtarma** – Tüm birimleri veya paylaşımları yeni bir sanal dizide kurtarmak için başarısız etme özelliğini kullanın. Yeni sanal dizioluşturmak ve StorSimple Device Manager hizmeti ile kayıt, sonra orijinal sanal dizi üzerinde başarısız. Yeni sanal dizi daha sonra sağlanan kaynakları varsayar.

## <a name="storsimple-virtual-array-components"></a>StorSimple Virtual Array bileşenleri

Sanal dizi aşağıdaki bileşenleri içerir:

* [Sanal dizi](#virtual-array) – Sanallaştırılmış ortamınızda veya hipervizörünüzde bulunan sanal bir makineye dayalı karma bulut depolama aygıtı.
* [StorSimple Device Manager hizmeti](#storsimple-device-manager-service) – Azure portalının, farklı coğrafi konumlardan erişebileceğiniz tek bir web arabiriminden bir veya daha fazla StorSimple cihazını yönetmenize olanak tanıyan bir uzantısıdır. Hizmetleri oluşturmak ve yönetmek, aygıtları ve uyarıları görüntülemek ve yönetmek ve birimleri, hisseleri ve varolan anlık görüntüleri yönetmek için StorSimple Device Manager hizmetini kullanabilirsiniz.
* [Yerel web kullanıcı arabirimi](#local-web-user-interface) – Aygıtı yerel ağa bağlanabilmesi ve ardından aygıtı StorSimple Device Manager hizmetine kaydedilebilmeleri için yapılandırmak için kullanılan web tabanlı kullanıcı arabirimi. 
* [Komut satırı arabirimi](#command-line-interface) – Sanal dizide bir destek oturumu başlatmak için kullanabileceğiniz bir Windows PowerShell arabirimi.
  Aşağıdaki bölümler, bu bileşenlerin her birini daha ayrıntılı olarak açıklar ve çözümün verileri nasıl düzenlediğini, depolamayı nasıl ayırdığını ve depolama yönetimini ve veri korumasını nasıl kolaylaştırdığını açıklar.

### <a name="virtual-array"></a>Sanal dizi

Sanal dizi, birincil depolama sağlayan, bulut depolama ile iletişimi yöneten ve aygıtta depolanan tüm verilerin güvenliğini ve gizliliğini sağlamaya yardımcı olan tek düğümlü bir depolama çözümüdür.

Sanal dizi, indirilebilen tek bir modelde kullanılabilir. Sanal dizi, aygıtta en fazla 6,4 TB (altta yatan depolama gereksinimi 8 TB) ve bulut depolama dahil 64 TB kapasiteye sahiptir.

Sanal dizi aşağıdaki özelliklere sahiptir:

* Bu maliyet etkindir. Mevcut sanallaştırma altyapınızdan yararlanır ve mevcut Hyper-V veya VMware hipervizörünüzde dağıtılabilir.
* Veri merkezinde bulunur ve iSCSI sunucusu veya dosya sunucusu olarak yapılandırılabilir.
* Bulutla entegre edilmiştir.
* Yedeklemeler, olağanüstü durum kurtarmayı kolaylaştıran ve madde düzeyinde kurtarmayı (ILR) basitleştiren bulutta depolanır.
* Güncelleştirmeleri, fiziksel bir aygıta uygulayacağınız gibi sanal diziye de uygulayabilirsiniz.

> [!NOTE]
> Sanal bir dizi genişletilemez. Bu nedenle, sanal dizi oluştururken yeterli depolama sağlamak önemlidir.

### <a name="storsimple-device-manager-service"></a>StorSimple Device Manager hizmeti

Microsoft Azure StorSimple, StorSimple depolama alanını merkezi olarak yönetmenize olanak tanıyan web tabanlı bir kullanıcı arabirimi olan StorSimple Device Manager hizmeti sağlar. Aşağıdaki görevleri gerçekleştirmek için StorSimple Device Manager hizmetini kullanabilirsiniz:

* Tek bir hizmetten birden çok StorSimple Sanal Dizi'yi yönetin.
* StorSimple Virtual Arrays için güvenlik ayarlarını yapılandırın ve yönetin. (Buluttaki şifreleme Microsoft Azure API'leri'ne bağlıdır.)
* Depolama hesabı kimlik bilgilerini ve özelliklerini yapılandırın.
* Birimleri veya hisseleri yapılandırma ve yönetme.
* Hacimler veya paylaşımlar hakkındaki verileri yedekleyip geri yükleyin.
* Performansı izleyin.
* Sistem ayarlarını gözden geçirin ve olası sorunları tanımlayın.

Sanal dizinizin günlük yönetimini gerçekleştirmek için StorSimple Device Manager hizmetini kullanırsınız.

Daha fazla bilgi için [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanın.](storsimple-virtual-array-manager-service-administration.md)

### <a name="local-web-user-interface"></a>Yerel web kullanıcı arabirimi

Sanal dizi, aygıtın StorSimple Device Manager hizmetine tek seferlik yapılandırması ve kaydı için kullanılan web tabanlı bir kullanıcı arabirimi içerir. Sanal diziyi kapatmak ve yeniden başlatmak, tanılama testlerini çalıştırmak, yazılımı güncelleştirmek, aygıt yöneticisi parolasını değiştirmek, sistem günlüklerini görüntülemek ve hizmet isteği nde bulunmak için Microsoft Destek'e başvurmak için kullanabilirsiniz.

Web tabanlı kullanıcı arabirimi kullanma hakkında daha fazla bilgi [için, StorSimple Virtual Array'inizi yönetmek için web tabanlı kullanıcı arabirimi](storsimple-ova-web-ui-admin.md)kullanın'a gidin.

### <a name="command-line-interface"></a>Komut satırı arabirimi

Dahil edilen Windows PowerShell arabirimi, sanal dizinizde karşılaşabileceğiniz sorunları gidermenize ve çözmenize yardımcı olmak için Microsoft Desteği ile bir destek oturumu başlatmanızı sağlar.

## <a name="storage-management-technologies"></a>Depolama yönetimi teknolojileri

StorSimple çözümü, sanal dizive diğer bileşenlere ek olarak, önemli verilere hızlı erişim sağlamak, depolama tüketimini azaltmak ve sanal dizinizde depolanan verileri korumak için aşağıdaki yazılım teknolojilerini kullanır:

* [Otomatik depolama katmanı](#automatic-storage-tiering) 
* [Yerel olarak sabitlenmiş hisse ler ve hacimler](#locally-pinned-shares-and-volumes)
* Buluta katmanlı veya yedeklenmiş veriler için çoğaltma ve sıkıştırma 
* [Zamanlanmış ve isteğe bağlı yedeklemeler](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Otomatik depolama katmanı
Sanal dizi, sanal dizi ve bulut genelinde depolanan verileri yönetmek için yeni bir katmanlama mekanizması kullanır. Yalnızca iki katman vardır: yerel sanal dizi ve Azure bulut depolama. StorSimple Virtual Array, mevcut kullanımı, yaşı ve diğer verilerle olan ilişkileri izleyen bir ısı haritasını temel alan verileri katmanlara otomatik olarak düzenler. En etkin (en sık) veriler yerel olarak depolanırken, daha az etkin ve etkin olmayan veriler otomatik olarak buluta geçirilir. (Tüm yedeklemeler bulutta depolanır.) StorSimple, kullanım desenleri değiştikçe verileri ve depolama atamalarını ayarlar ve yeniden düzenler. Örneğin, bazı bilgiler zaman içinde daha az etkin hale gelebilir. Giderek daha az etkin hale geldikçe, buluta katmanlanır. Aynı veriler yeniden etkin hale gelirse, depolama dizisinde katmanlanır.

Belirli bir katmanlı hisse veya hacim için veriler kendi yerel katman alanı garanti edilir (bu pay veya hacim için toplam sağlanan alanın yaklaşık %10'u). Bu, bu paylaşım veya birim için sanal dizideki kullanılabilir depolamayı azaltırken, bir paylaşım veya birim için katmanlamanın diğer paylaşımların veya birimlerin katmanlama gereksinimlerinden etkilenmemesini sağlar. Bu nedenle, bir paylaşım veya birimdeki çok yoğun bir iş yükü diğer tüm iş yüklerini buluta zorlayamaz.

iSCSI için oluşturulan katmanlı hacimler, hacmin boyutuna bakılmaksızın maksimum 200 GB yerel rezervasyona sahiptir.

![otomatik depolama katmanı](./media/storsimple-ova-overview/automatic-storage-tiering.png)

> [!NOTE]
> Bir birim yerel olarak sabitlenmiş olarak belirtebilirsiniz, bu durumda veriler sanal dizide kalır ve hiçbir zaman buluta katmanlandırılmaz. Daha fazla bilgi için [Yerel olarak sabitlenmiş paylaşımlara ve birimlere](#locally-pinned-shares-and-volumes)gidin.


### <a name="locally-pinned-shares-and-volumes"></a>Yerel olarak sabitlenmiş hisse ler ve hacimler

Yerel olarak sabitlenmiş olarak uygun paylaşımlar ve birimler oluşturabilirsiniz. Bu özellik, kritik uygulamaların gerektirdiği verilerin sanal dizide kalmasını ve hiçbir zaman buluta katmanlandırılmamasını sağlar. Yerel olarak sabitlenmiş paylaşımlar ve birimler aşağıdaki özelliklere sahiptir:

* Bulut gecikmelerine veya bağlantı sorunlarına tabi değildir.
* StorSimple bulut yedekleme ve olağanüstü durum kurtarma özelliklerinden hala yararlanırlar.

Yerel olarak sabitlenmiş bir pay veya birimi katmanlı veya katmanlı bir pay veya birim olarak yerel olarak sabitlenmiş olarak geri yükleyebilirsiniz. 

Yerel olarak sabitlenmiş birimler hakkında daha fazla bilgi için, birimleri yönetmek için [StorSimple Device Manager hizmetini kullanın'a](storsimple-virtual-array-manage-volumes.md)gidin.

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Buluta katmanlı veya yedeklenmiş veriler için çoğaltma ve sıkıştırma

StorSimple, buluttaki depolama gereksinimlerini daha da azaltmak için çoğaltma ve veri sıkıştırma kullanır. Deduplication, depolanan veri kümesindeki artıklığı ortadan kaldırarak depolanan toplam veri miktarını azaltır. Bilgi değiştikçe, StorSimple değişmeyen verileri yoksa ve yalnızca değişiklikleri yakalar. Buna ek olarak, StorSimple yinelenen bilgileri tanımlayarak ve kaldırarak depolanan veri miktarını azaltır.

> [!NOTE]
> Sanal dizide depolanan veriler çoğaltılamaz veya sıkıştırılmez. Tüm çoğaltma ve sıkıştırma, veriler buluta gönderilmeden hemen önce gerçekleşir.

### <a name="scheduled-and-on-demand-backups"></a>Zamanlanmış ve isteğe bağlı yedeklemeler

StorBasit veri koruma özellikleri, isteğe bağlı yedeklemeler oluşturmanıza olanak tanır. Ayrıca, varsayılan yedekleme zamanlaması verilerin günlük olarak yedeklenmelerini sağlar. Yedeklemeler, bulutta depolanan artımlı anlık görüntüler şeklinde alınır. Yalnızca son yedeklemeden sonraki değişiklikleri kaydeden anlık görüntüler oluşturulabilir ve hızlı bir şekilde geri yüklenebilir. Bu anlık görüntüler, ikincil depolama sistemlerinin (teyp yedeklemesi gibi) yerini aldıkları ve verileri veri merkezinize veya gerekirse alternatif sitelere geri yüklemenize olanak sağladıkları için olağanüstü durum kurtarma senaryolarında kritik öneme ilebilir.

## <a name="managing-personal-information"></a>Kişisel bilgilerin yönetimi

Sanal seriiçin StorSimple Aygıt Yöneticisi kişisel bilgileri iki temel durumda toplar:
 - Kullanıcıların e-posta adreslerinin yapılandırıldığı kullanıcı ayarlarını uyar. Bu bilgiler yönetici tarafından temizlenebilir. 
 - Paylaşımlarda bulunan verilere erişebilen kullanıcılar. Paylaşım verilerine erişebilen kullanıcıların listesi görüntülenir ve dışa aktarılabilir. Paylaşımlar silindiğinde bu liste de silinir.

Daha fazla bilgi için [Güven Merkezi'ndeki Microsoft Gizlilik ilkesini](https://www.microsoft.com/trustcenter)inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sanal dizi portalını nasıl [hazırlayacağınızı](storsimple-virtual-array-deploy1-portal-prep.md)öğrenin.
