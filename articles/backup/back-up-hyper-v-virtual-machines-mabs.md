---
title: Hyper-V sanal makinelerini MABS ile yedekleme
description: Bu makalede, Microsoft Azure Yedekleme Sunucusu (MABS) kullanarak sanal makinelerin yedeklemesi ve kurtarılması için yordamlar yer almaktadır.
ms.topic: conceptual
ms.date: 07/18/2019
ms.openlocfilehash: 71cf446472ef0cf4f50bf64e47d359ea08ccc087
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420412"
---
# <a name="back-up-hyper-v-virtual-machines-with-azure-backup-server"></a>Azure Yedekleme Sunucusu ile Hyper-V sanal makinelerini yedekleme

Bu makalede, Microsoft Azure Yedekleme Sunucusu (MABS) kullanarak Hyper-V sanal makinelerinin nasıl yedeklenenekadar yedekleneneaçıklanmaktadır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

MABS, Hyper-V ana bilgisayar sunucularında çalışan sanal makineleri aşağıdaki senaryolarda yedekleyebilir:

- **Yerel veya doğrudan depolamaya sahip sanal makineler** - Yerel veya doğrudan bağlı depolama alanına sahip Hyper-V konak tek başına sunucularda barındırılan sanal makineleri yedekleyin. Örneğin: sabit disk, depolama alanı ağı (SAN) aygıtı veya ağa bağlı depolama (NAS) aygıtı. MABS koruma aracısı tüm ana bilgisayarlara yüklenmelidir.

- **CSV deposu ile kümede bulunan sanal makineler** - Küme Paylaşılan Birimi (CSV) depolama alanı ile bir Hyper-V kümesi üzerinde barındırılan sanal makineleri yedekleyin. MABS koruma aracısı her küme düğümüne yüklenir.

## <a name="host-versus-guest-backup"></a>Ev sahibi ve konuk yedekleme

MABS, Hyper-V VM'lerin ana bilgisayar veya konuk düzeyinde yedeklemesini yapabilir. Ana bilgisayar düzeyinde, MABS koruma aracısı Hyper-V ana bilgisayar sunucusuna veya kümesine yüklenir ve bu ana bilgisayarda çalışan tüm VM'leri ve veri dosyalarını korur.   Konuk düzeyinde, aracı her sanal makineye yüklenir ve bu makinede bulunan iş yükünü korur.

Her iki yöntemin artıları ve eksileri vardır:

- Konuk makinelerde çalışan işletim sistemi türüne bakılmaksızın çalıştıkları ve her VM'ye MABS koruma aracısının yüklenmesini gerektirmedikleri için ana bilgisayar düzeyinde yedeklemeler esnektir. Ana bilgisayar düzeyinde yedekleme dağıtıyorsanız, tüm sanal makineyi veya dosya ve klasörleri (öğe düzeyinde kurtarma) kurtarabilirsiniz.

- Konuk düzeyinde yedekleme, sanal bir makinede çalışan belirli iş yüklerini korumak istiyorsanız yararlıdır. Ana bilgisayar düzeyinde, tüm VM veya belirli dosyaları kurtarabilirsiniz, ancak belirli bir uygulama bağlamında kurtarma sağlamaz. Örneğin, yedeklenmiş bir VM'den belirli SharePoint öğelerini kurtarmak için, bu VM'nin konuk düzeyinde yedeklemesini yapmanız gerekir. Geçiş disklerinde depolanan verileri korumak istiyorsanız konuk düzeyinde yedeklemeyi kullanın. Geçiş, sanal makinenin depolama aygıtına doğrudan erişmesine izin verir ve sanal birim verilerini bir VHD dosyasında depolamaz.

## <a name="how-the-backup-process-works"></a>Yedekleme işlemi nasıl çalışır?

MABS aşağıdaki gibi VSS ile yedekleme gerçekleştirir. Bu açıklamadaki adımlar, anlamayı kolaylaştırmak için numaralanmıştır.

1. MABS blok tabanlı senkronizasyon motoru, korunan sanal makinenin ilk kopyasını yapar ve sanal makinenin kopyasının tam ve tutarlı olmasını sağlar.

2. İlk kopya yapıldıktan ve doğrulandıktan sonra, MABS yedeklemeleri yakalamak için Hyper-V VSS yazarını kullanır. VSS yazarı, MABS sunucusuyla eşitlenmiş veri tutarlı disk blokları kümesi sağlar. Bu yaklaşım, Ağ üzerinden aktarılması gereken yedekleme verilerini en aza indirirken MABS sunucusuyla "tam yedekleme" avantajı sağlar.

3. Hyper-V çalıştıran bir sunucudaki MABS koruma aracısı, korumalı sanal bir sanal makinenin VSS'yi de destekleyip desteklemediğini belirlemek için varolan Hyper-V API'leri kullanır.

   - Bir sanal makine, çevrimiçi yedekleme gereksinimlerini karşılıyorsa ve bu sanal makineye Hyper-V tümleştirme hizmetleri yüklüyse Hyper-V VSS yazıcı, VSS isteğini sanal makinedeki tüm VSS bilinçli işlemlere yinelemeli olarak iletir. Bu işlem, sanal makineye MABS koruma aracısı takılmadan gerçekleşir. Yinelemeli VSS isteği, Hyper-V VSS yazıcının, bir VSS anlık görüntüsünün veri kaybı yaşanmadan yakalanabilmesi için diske yazma işlemlerini eşitlemesini sağlar.

     Hyper-V tümleştirme hizmetleri bileşeni, uygulama verilerinin tutarlı bir durumda olduğundan emin olmak için Hyper-V VSS yazıcıyı sanal makinelerdeki Birim Gölge Kopyası Hizmetlerinde (VSS) başlatır.

   - Sanal makine çevrimiçi yedekleme gereksinimlerine uymuyorsa, MABS veri dosyalarını yakalamadan önce sanal makineyi duraklatmak için Otomatik olarak Hyper-V API'lerini kullanır.

4. Sanal makinenin ilk temel kopyası MABS sunucusuyla eşitledikten sonra, sanal makine kaynaklarında yapılan tüm değişiklikler yeni bir kurtarma noktasında yakalanır. Kurtarma noktası, sanal makinenin belirli bir zamandaki tutarlı durumunu temsil eder. Kurtarma noktası yakalamaları günde en az bir kez gerçekleşebilir. Yeni bir kurtarma noktası oluşturulduğunda, MABS, son kurtarma noktası oluşturulduktan sonra Hyper-V çalıştıran sunucuda hangi blokların değiştirildiğini belirlemek için Hyper-V VSS yazarıyla birlikte blok düzeyinde çoğaltma kullanır. Bu veri blokları daha sonra MABS sunucusuna aktarılır ve korunan verilerin çoğaltılması için uygulanır.

5. MABS sunucusu, kurtarma verilerini barındıran birimlerde VSS kullanır, böylece birden çok gölge kopya kullanılabilir. Bu gölge kopyaların her biri ayrı bir kurtarma sağlar. VSS kurtarma noktaları MABS sunucusunda depolanır. Hyper-V çalıştıran sunucuda yapılan geçici kopya, yalnızca MABS eşitleme süresi boyunca depolanır.

>[!NOTE]
>
>Windows Server 2016'dan itibaren Hyper-V sanal sabit diskler, esnek değişim izleme (RCT) olarak bilinen yerleşik değişim izleme özelliğine sahiptir. MABS, VM çökmeleri gibi senaryolarda zaman alan tutarlılık denetimlerine olan gereksinimi azaltan RCT (Hyper-V'deki yerel değişim izleme) kullanır. RCT, VSS anlık görüntü tabanlı yedeklemelerin sağladığı değişiklik izlemeden daha dayanıklıdır. MABS V3, herhangi bir tutarlılık denetimi sırasında yalnızca değiştirilen verileri aktararak ağ ve depolama tüketimini daha da optimize eder.

## <a name="backup-prerequisites"></a>Yedekleme önkoşulları

Hyper-V sanal makinelerini MABS ile yedeklemenin ön koşulları şunlardır:

|Önkoşul|Ayrıntılar|
|------------|-------|
|MABS ön koşulları|- Sanal makineler (dosyaları, klasörleri, birimleri kurtarmak) için öğe düzeyinde kurtarma gerçekleştirmek istiyorsanız, Hyper-V rolünü MABS sunucusuna yüklemeniz gerekir.  Yalnızca sanal makineyi kurtarmak istiyorsanız ve öğe düzeyinde değil, rol gerekli değildir.<br />- Bir MABS sunucusunda her biri 100 GB'a kadar 800 sanal makineyi koruyabilir ve daha büyük kümeleri destekleyen birden fazla MABS sunucusuna izin verebilirsiniz.<br />- MABS, sanal makine yedekleme performansını artırmak için sayfa dosyasını artımlı yedeklemelerden dışlar.<br />- MABS, Bir Hyper-V sunucusunu veya kümesini MABS sunucusuyla aynı etki alanında veya bir alt veya güvenilir etki alanında yedekleyebilir. Hyper-V'yi bir çalışma grubunda veya güvenilmeyen bir etki alanında yedeklemek istiyorsanız, kimlik doğrulaması ayarlamanız gerekir. Tek bir Hyper-V sunucusu için NTLM veya sertifika kimlik doğrulaması kullanabilirsiniz. Bir küme için yalnızca sertifika kimlik doğrulaması kullanabilirsiniz.<br />-   Doğrudan geçiş disklerinde sanal makine verilerini yedeklemek için konak düzeyinde yedekleme kullanımı desteklenmez. Bu senaryoda, vhd dosyalarını yedeklemek için ana bilgisayar düzeyinde yedekleme ve ana bilgisayarda görünmeyen diğer verileri yedeklemek için konuk düzeyinde yedekleme kullanmanızı öneririz.<br />   -Deduplicated birimlerinde depolanan VM'leri yedekleyebilirsiniz.|
|Hyper-V VM önkoşulları|- Sanal makinede çalışan Tümleştirme Bileşenleri sürümü Hyper-V ana bilgisayar sürümü ile aynı olmalıdır. <br />-   Her bir sanal makine yedeklemesinde, Hyper-V'ye yedekleme sırasında farklı diskler (AVHD'nin) için yeterince alan bırakmak üzere sanal sabit disk dosyalarını barındıran birimde boş alan olması gerekir. Alan en az, hesaplanan **İlk disk boyutu\*Karmaşıklık oranı\*Yedekleme** penceresi zamanına eşit olmalıdır. Bir kümede birden fazla yedekleme çalıştırıyorsanız AVHD'leri bu hesaplamayı kullanan her bir sanal makine için uygun hale getirmek üzere yeterli depolama kapasitesine sahip olmanız gerekir.<br />- Windows Server 2012 R2 çalıştıran Hyper-V ana bilgisayar sunucularında bulunan sanal makineleri yedeklemek için, sanal makinenin herhangi bir şeye bağlı olmasa bile bir SCSI denetleyicisi belirtilmelidir. (Windows Server 2012 R2 çevrimiçi yedeklemesinde Hyper-V ana bilgisayar VM'ye yeni bir VHD bağlar ve daha sonra sökülür. Bunu yalnızca SCSI denetleyicisi destekleyebilir ve bu nedenle sanal makinenin çevrimiçi yedeklemesi için gereklidir.  Bu ayar olmadan, sanal makineyi yedeklemeye çalıştığınızda olay kimliği 10103 verilir.)|
|Linux önkoşulları|- MABS kullanarak Linux sanal makinelerini yedekleyebilirsiniz. Yalnızca dosya tutarlı anlık görüntüler desteklenir.|
|CSV depolama ile VM’leri yedekleyin|-   CSV depolama için Hyper-V sunucusuna Birim Gölge Kopya Hizmetleri (VSS) donanım sağlayıcısını yükleyin. VSS donanım sağlayıcısı için depolama alanı ağı (SAN) satıcınıza başvurun.<br />- Tek bir düğüm bir CSV kümesinde beklenmedik bir şekilde kapanırsa, MABS bu düğümüzerinde çalışan sanal makinelere karşı bir tutarlılık denetimi gerçekleştirir.<br />-   CSV kümesinde BitLocker Sürücü Şifrelemesi etkinleştirilmiş bir Hyper-V sunucusunu yeniden başlatmanız gerekirse, Hyper-V sanal makinelerine yönelik bir tutarlılık denetimi gerçekleştirmeniz gerekir.|
|SMB deposu ile VM’leri yedekleyin|-   Sanal makine korumasını etkinleştirmek için Hyper-V çalıştıran sunucuda otomatik bağlamayı açın.<br />   -   TCP Kanalı Boşaltma'yı devre dışı bırakın.<br />-   Tüm Hyper-V machine$ hesaplarının ilgili uzak SMB dosya paylaşımlarında tam izinleri olduğundan emin olun.<br />- Kurtarma sırasında tüm sanal makine bileşenlerinin alternatif konuma dosya yolunun 260 karakterden az olduğundan emin olun. Değilse, kurtarma başarılı olabilir, ancak Hyper-V sanal makine monte etmek mümkün olmayacaktır.<br />- Aşağıdaki senaryolar desteklenmez:<br />     Sanal makinenin bazı bileşenlerinin yerel birimlerde, bazı bileşenlerin uzak birimlerde olduğu dağıtımlar; depolama konumu dosya sunucusu için bir IPv4 veya IPv6 adresi ve sanal bir makinenin uzak Kobİ paylaşımları kullanan bir bilgisayara kurtarılması.<br />- Her Kobİ sunucusunda File Server VSS Agent hizmetini etkinleştirmeniz gerekir - **Ekle rolleri ve özellikleri** > **Ekle sunucu rolleri** > **Dosya ve Depolama Hizmetleri** > **Dosya Hizmetleri** > Dosya**Hizmeti** > **Dosya Sunucusu VSS Aracı Hizmeti**seçin.|

## <a name="back-up-virtual-machines"></a>Sanal makineleri yedekleme

1. [MABS sunucunuzu](backup-azure-microsoft-azure-backup.md) ve [depolama alanınızı](backup-mabs-add-storage.md)ayarlayın. Depolama alanınızı ayarlarken, bu depolama kapasitesi kurallarını kullanın.
   - Ortalama sanal makine boyutu: 100 GB
   - MABS sunucusu başına sanal makine sayısı - 800
   - 800 VM için toplam boyut: 80 TB
   - Yedekleme deposu için gereken alan: 80 TB

2. Hyper-V sunucusunda veya Hyper-V küme düğümlerinde MABS koruma aracısını ayarlayın. Konuk düzeyinde yedekleme yapıyorsanız, aracıyı konuk düzeyinde yedeklemek istediğiniz VM'lere yüklersiniz.

3. MABS Administrator konsolunda, Yeni Koruma Grubu **Oluştur** sihirbazını açmak için **Koruma** > **Oluştur koruma grubunu** tıklatın.

4. **Grup Üyelerini Seçin** sayfasında, üzerinde bulundukları Hyper-V konak sunucularından korumak istediğiniz VM’leri seçin. Aynı koruma ilkesine sahip olacak tüm VM’leri tek bir koruma grubuna koymanızı öneririz. Alanı verimli kullanmak için birlikte bulundurmayı etkinleştirin. Birlikte bulundurma, aynı diskte veya bant deposunda bulunan farklı koruma gruplarında veri bulundurmanıza olanak tanır. Böylece birden çok veri kaynağının tek bir çoğaltması ve kurtarma noktası birimi olur.

5. **Veri Koruma Yöntemini Seçin** sayfasında, koruma grubunun adını belirtin. Verileri Azure Yedekleme hizmetini kullanarak Azure'a yedeklemek istiyorsanız **Disk kullanarak kısa vadeli koruma istiyorum** 'u seçin ve **Çevrimiçi koruma istiyorum** 'u seçin.

6. Kısa Vadeli Hedefler > **Bekletme aralığını** **belirtin,** disk verilerini ne kadar süreyle saklamak istediğinizi belirtin. **Eşitleme sıklığında,** verilerin artımlı yedeklemelerinin ne sıklıkta çalışması gerektiğini belirtin. Alternatif olarak, artımlı yedeklemeler için bir aralık seçmek yerine **Bir kurtarma noktasından hemen önce**seçeneğini etkinleştirebilirsiniz. Bu ayar etkinleştirildiğinde, MABS her zamanlanan kurtarma noktasından hemen önce bir ekspres tam yedekleme çalıştıracaktır.

    > [!NOTE]
    >
    >Uygulama iş yüklerini koruyorsanız, uygulamanın artımlı yedeklemeleri desteklemesi koşuluyla kurtarma noktaları Eşitleme sıklığına uygun olarak oluşturulur. Değilse, MABS artımlı yedekleme yerine bir ekspres tam yedekleme çalıştırır ve ekspres yedekleme zamanlamasına uygun olarak kurtarma noktaları oluşturur.

7. Gözden **Geçir disk ayırma** sayfasında, koruma grubu için ayrılan depolama havuzu disk alanını gözden geçirin.

   **Toplam Veri boyutu** yedeklemek istediğiniz verilerin boyutudur ve **MABS'de sağlanacak disk alanı, MABS'nin** koruma grubu için önerdiği alandır. MABS, ayarlara göre ideal yedekleme hacmini seçer. Ancak, yedekleme birimi seçeneklerini **Disk ayırma ayrıntıları**'nda düzenleyebilirsiniz. İş yükleri için açılan menüden tercih edilen depolamayı seçin. Düzenlemeleriniz, **Kullanılabilir Disk Depolaması** bölmesindeki **Toplam Depolama** ve **Boş Depolama** değerlerini değiştirir. Underprovisioned alan depolama MABS miktarı gelecekte düzgün yedekleme ile devam etmek, ses eklemek öneriyor.

8. **Çoğaltma Oluşturma Yöntemini Seçin** sayfasında, veri koruma grubundaki ilk çoğaltmanın nasıl gerçekleştirileceğini belirtin. Ağ üzerinden **otomatik olarak çoğaltmayı**seçerseniz, yoğun olmayan bir zaman seçmenizi tavsiye ettik. Büyük miktarda veya en uygun ağ koşullarından daha az veri için, çıkarılabilir ortam kullanarak verileri çevrimdışı olarak çoğaltmayı gerektiren **El Ile'yi**seçmeyi düşünün.

9. **Tutarlılık Denetimi Seçenekleri** sayfasında, tutarlılık denetimlerinin nasıl otomatikleştirilmesini istediğinizi seçin. Veri çoğaltma tutarsız hale geldiğinde veya bir zamanlamaya göre çalıştırmak için bir denetim etkinleştirebilirsiniz. Otomatik tutarlılık denetimini yapılandırmak istemiyorsanız, herhangi bir zamanda koruma grubuna sağ tıklayarak ve **Tutarlılık Denetimi Gerçekleştir**'i seçerek el ile denetim gerçekleştirebilirsiniz.

    Veri koruma grubu oluşturulduktan sonra ilk veri çoğaltma işlemi seçtiğiniz yönteme uygun olarak gerçekleşir. İlk çoğaltmadan sonra her yedekleme koruma grubu ayarlarına uygun olarak gerçekleşir. Yedeklenen verileri kurtarmanız gerekiyorsa, aşağıdakileri unutmayın:

## <a name="back-up-replica-virtual-machines"></a>Çoğaltma sanal makineleri yedekleme

MABS Windows Server 2012 R2 veya daha büyük çalışıyorsa, çoğaltma sanal makineleri yedekleyebilirsiniz. Bu, çeşitli nedenlerle yararlıdır:

**Çalıştırma iş yükü üzerindeki yedeklemelerin etkisini azaltır** - Bir sanal makinenin yedeğini almak bir anlık görüntü oluşturulduğu için bazı ek yükler getirir. Yedekleme işlemini ikincil bir uzak siteye boşaltarak, çalışan iş yükü artık yedekleme işleminden etkilenmez. Bu, yalnızca yedekleme kopyasının uzak bir siteye depolandığı dağıtımlar için geçerlidir. Örneğin, geri yüklemelerin hızlı olmasını sağlamak için günlük olarak yedek alabilir ve yerel olarak veri depolayabilirsiniz ancak uzun vadeli bekletme için uzakta depolanan çoğaltma sanal makinelerinden aylık veya üç aylık olarak yedek alın.

**Bant genişliği tasarrufu sağlar** - Tipik bir uzak şubeye/merkeze dağıtımda siteler arasında yedekleme verilerini aktarmak için uygun miktarda bant genişliğiniz olması gerekir. Veri yedekleme stratejinize ek olarak bir çoğaltma ve yük devretme stratejisi oluşturursanız, ağ üzerinden gönderilen fazladan veri miktarını düşürebilirsiniz. Birincil yerine yineleme sanal makine verilerini yedekleyerek, yedeklenen verileri ağ üzerinden gönderme nin genel kısmını kaydedebilirsiniz.

**Barındırıcı yedeği sağlar** - Barındırılan bir veri merkezini bir çoğaltma site olarak kullanabilirsiniz; ikincil bir veri merkezine gerek kalmaz. Bu durumda, ana bilgisayar SLA çoğaltma sanal makinelerin tutarlı yedekleme gerektirir.

Bir yük devretme başlatılana kadar bir çoğaltma sanal makine kapalıdır ve VSS, bir çoğaltma sanal makine için uygulamada tutarlı olan bir yedeklemeyi garanti edemez. Bu nedenle çoğaltma sanal makinesinin yedeklemesi yalnızca kilitlenme tutarlı olur. Kilitlenme tutarlılığı garanti edilemiyorsa yedekleme başarısız olur ve bu birkaç koşulda gerçekleşebilir:

- Çoğaltma sanal makinesi iyi durumda değildir ve kritik durumda.

- Çoğaltma sanal makinesi yeniden eşitleme yapıyor (Yeniden eşitleme sürüyor veya Yeniden Eşitleme Gerekiyor durumunda).

- Birincil ve ikincil site arasındaki ilk çoğaltma sürüyor veya sanal makineyi bekliyor.

- .hrl günlükleri çoğaltma sanal makineye uygulanıyor veya sanal diskteki .hrl günlüklerini uygulamak için önceki bir eylem başarısız oldu veya iptal edildi veya kesildi.

- Çoğaltma sanal makinesinin geçişi veya yük devretmesi sürüyor

## <a name="recover-backed-up-virtual-machines"></a>Yedeklenen sanal makineleri kurtar

Yedeklenen bir sanal makineyi kurtardığınızda, sanal makineyi ve özgül kurtarma noktasını seçmek için Kurtarma sihirbazını kullanırsınız. Kurtarma Sihirbazı'nı açmak ve bir sanal makineyi kurtarmak için:

1. MABS Administrator konsolunda, VM'nin adını yazın veya korumalı öğeler listesini genişletin ve kurtarmak istediğiniz VM'yi seçin.

2. **Kurtarma noktaları** bölmesinde, takvimin üzerinde, bulunan kurtarma noktalarını görmek için herhangi bir tarihe tıklayın. Sonra, **Yol** bölmesinde, Kurtarma sihirbazında kullanmak istediğiniz kurtarma noktasını seçin.

3. Kurtarma Sihirbazı'nı açmak için **Eylemler** menüsünden **Kurtar**'a tıklayın.

    Seçtiğiniz VM ve kurtarma noktası, **Kurtarma Seçimini İnceleyin** ekranında görünür. **İleri**'ye tıklayın.

4. **Kurtarma Türünü Seçin** ekranında, verileri geri yüklemek istediğiniz yeri seçin ve **İleri**'ye tıklayın.

    - **Özgün örneğe kurtar**: Özgün örneğe kurtardığınızda, özgün VHD silinir. MABS, Hyper-V VSS yazıcısını kullanarak VHD ve diğer yapılandırma dosyalarını orijinal konuma kurtarır. Kurtarma işleminin sonunda, sanal makineler yüksek oranda kullanılabilir olarak kalır.
        Kurtarma için kaynak grubunun mevcut olması gerekir. Mevcut değilse, başka bir konuma kurtarın ve ardından sanal makineyi yüksek oranda kullanılabilir hale getirin.

    - **Herhangi bir ana bilgisayara sanal makine olarak kurtarma**: MABS, işlemci mimarisinden bağımsız olarak, korumalı hyper-v sanal makinenin farklı bir Hyper-V ana bilgisayarına sorunsuz bir şekilde kurtarılmasını sağlayan alternatif konum kurtarmayı (ALR) destekler. Küme düğümüne kurtarılan Hyper-V sanal makineleri yüksek oranda kullanılabilir olmaz. Bu seçeneği işaretlerseniz, Kurtarma Sihirbazı, hedefi ve hedef yolunu tanımlamak için size ek bir ekran sunar.

    - **Ağ klasörüne kopyalama**: MABS, Hyper-V sanal makinelerin ana bilgisayar düzeyindeyedeklemesinden bir ağ paylaşımına veya MABS korumalı sunucudaki bir birime kadar dosya, klasör, birim ve sanal sabit disklerin (VHD) öğe düzeyinde kurtarılmasını yapmanızı sağlayan öğe düzeyinde kurtarmayı (ILR) destekler. MABS koruma aracısının madde düzeyinde kurtarma gerçekleştirmek için konuğun içine yüklenmesi gerekmez. Bu seçeneği işaretlerseniz, Kurtarma Sihirbazı, hedefi ve hedef yolunu tanımlamak için size ek bir ekran sunar.

5. **Kurtarma Seçeneklerini Belirtin**'de, kurtarma seçeneklerini yapılandırın ve **İleri**'ye tıklayın:

    - Bir VM'yi düşük bant genişliği üzerinden kurtarıyorsanız, **Ağ bant genişliği kullanımı azaltma**'yı etkinleştirmek için **Değiştir**'e tıklayın. Azaltma seçeneğini açtıktan sonra, kullandırmak istediğiniz bant genişliği miktarını ve bu bant genişliğinin kullanılabileceği zamanı belirtebilirsiniz.
    - Ağınızı yapılandırdıysanız, **Donanım anlık görüntüleri kullanan SAN tabanlı kurtarmayı etkinleştir**'i seçin.
    - Kurtarma işlemi tamamlandığında e-posta bildirimleri gönderilmesini istiyorsanız, **Kurtarma tamamlandığında bir e-posta gönder**'i seçin ve e-posta adreslerini girin.

6. Özet ekranında, tüm ayrıntıların doğru olduğundan emin olun. Ayrıntılar doğru değilse veya bir değişiklik yapmak istiyorsanız, **Geri**'ye tıklayın. Ayarlardan memnunsanız, kurtarma işlemini başlatmak için **Kurtar**'a tıklayın.

7. **Kurtarma Durumu** ekranı kurtarma işi hakkında bilgi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Backup Sunucusu’ndan veri kurtarma](https://docs.microsoft.com/azure/backup/backup-azure-alternate-dpm-server)
