---
title: SharePoint için StorSimple Bağdaştırıcısı 'nı yükler | Microsoft Docs
description: SharePoint için StorSimple bağdaştırıcısının SharePoint sunucu grubunda nasıl yükleneceğini ve yapılandırılacağını açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: 36c20b75-f2e5-4184-a6b5-9c5e618f79b2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/06/2017
ms.author: twooley
ms.openlocfilehash: a841ce8b664389ccd8fdf55de9965f09412fecf5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75930217"
---
# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısını yükleyip yapılandırma
## <a name="overview"></a>Genel Bakış
SharePoint için StorSimple bağdaştırıcısı, SharePoint sunucu gruplarına Microsoft Azure StorSimple esnek depolama ve veri koruması sağlamanıza olanak tanıyan bir bileşendir. Bağdaştırıcısını, SQL Server içerik veritabanlarındaki Ikili büyük nesne (BLOB) içeriğini Microsoft Azure StorSimple Hibrit bulut depolama cihazına taşımak için kullanabilirsiniz.

SharePoint için StorSimple bağdaştırıcısı, uzak BLOB depolama (KÇY) sağlayıcısı olarak çalışır ve bir StorSimple cihazı tarafından desteklenen bir dosya sunucusundaki yapılandırılmamış SharePoint içeriğini (blob 'Lar biçiminde) depolamak için SQL Server uzak BLOB depolama özelliğini kullanır.

> [!NOTE]
> SharePoint için StorSimple bağdaştırıcısı, SharePoint Server 2010 uzak BLOB depolama alanını (KÇY) destekler. SharePoint Server 2010 dış BLOB depolama alanını (EBS) desteklemez.


* SharePoint için StorSimple bağdaştırıcısını indirmek için, Microsoft Indirme Merkezi ' nde [SharePoint Için StorSimple Bağdaştırıcısı][1] sayfasına gidin.
* KÇY ve KÇY sınırlamalarını planlama hakkında daha fazla bilgi için [SharePoint 2013 'de KÇY 'yi kullanma][2] veya [KÇY (SharePoint Server 2010)][3]planlaması konusuna gidin.

Bu genel bakışın geri kalanında, bağdaştırıcıyı yüklemeden ve yapılandırmadan önce, SharePoint için StorSimple bağdaştırıcısının ve SharePoint kapasite ve performans sınırlarının rolü açıklanır. Bu bilgileri gözden geçirdikten sonra, bağdaştırıcıyı ayarlamaya başlamak için [SharePoint Için StorSimple Bağdaştırıcısı yüklemesine](#storsimple-adapter-for-sharepoint-installation) gidin.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>SharePoint için StorSimple Bağdaştırıcısı avantajları
Bir SharePoint sitesinde içerik, bir veya daha fazla içerik veritabanında yapılandırılmamış BLOB verileri olarak depolanır. Varsayılan olarak, bu veritabanları SQL Server çalıştıran ve SharePoint sunucu grubunda bulunan bilgisayarlarda barındırılır. Blob 'Lar, büyük miktarlarda şirket içi depolamayı tüketerek hızla boyutunu artırabilir. Bu nedenle, daha az maliyetli bir depolama çözümü bulmak isteyebilirsiniz. SQL Server, uzak BLOB depolama (KÇY) adlı, dosya sisteminde BLOB içeriğini SQL Server veritabanının dışında depolamanıza olanak tanıyan bir teknoloji sağlar. KÇY ile, Bloblar SQL Server çalıştıran bilgisayardaki dosya sisteminde bulunabilir veya dosya sisteminde başka bir sunucu bilgisayarında depolanabilir.

KÇY, SharePoint 'te KÇY 'yi etkinleştirmek için SharePoint için StorSimple Bağdaştırıcısı gibi bir KÇY sağlayıcısı kullanmanızı gerektirir. SharePoint için StorSimple bağdaştırıcısı, KÇY ile birlikte çalışarak blob 'Ları Microsoft Azure StorSimple sistemi tarafından yedeklenen bir sunucuya taşımanıza olanak tanır. Microsoft Azure StorSimple daha sonra BLOB verilerini, kullanıma göre yerel olarak veya bulutta depolar. Çok etkin olan Bloblar (genellikle katman 1 veya sık erişimli veriler olarak adlandırılır) yerel olarak bulunur. Daha az etkin veri ve arşiv verileri bulutta bulunur. Bir içerik veritabanında KÇY 'yi etkinleştirdikten sonra, SharePoint 'te oluşturulan tüm yeni BLOB içerikleri, içerik veritabanında değil StorSimple cihazında depolanır.

KÇY 'nin Microsoft Azure StorSimple uygulanması aşağıdaki avantajları sağlar:

* BLOB içeriğini ayrı bir sunucuya taşıyarak, SQL Server üzerindeki sorgu yükünü azaltabilirsiniz ve bu da yanıt verme SQL Server iyileştirebilirler. 
* Azure StorSimple, veri boyutunu azaltmak için Yinelenenleri kaldırma ve sıkıştırma kullanır.
* Azure StorSimple, yerel ve bulut anlık görüntüleri biçiminde veri koruması sağlar. Ayrıca, veritabanının kendisini StorSimple cihazına yerleştirirseniz, içerik veritabanını ve Blobları kilitlenmeyle tutarlı bir şekilde yedekleyebilirsiniz. (İçerik veritabanını cihaza taşımak yalnızca StorSimple 8000 serisi cihaz için desteklenir. Bu özellik, 5000 veya 7000 Serisi için desteklenmez.)
* Azure StorSimple, yük devretme, dosya ve birim kurtarma (test kurtarma dahil) ve verilerin hızlı geri yüklenmesi gibi olağanüstü durum kurtarma özellikleri içerir.
* SharePoint içeriğinin öğe düzeyinde kurtarılmasını sağlamak için BLOB verilerinin StorSimple anlık görüntüleriyle birlikte, Kroll Ontrack PowerControls gibi veri kurtarma yazılımlarını kullanabilirsiniz. (Bu veri kurtarma yazılımı ayrı bir satın alımdır.)
* SharePoint için StorSimple bağdaştırıcısı, SharePoint Merkezi Yönetim portalına takılır ve tüm SharePoint Çözümünüzü merkezi bir konumdan yönetmenizi sağlar.

BLOB içeriğini dosya sistemine taşımak, diğer maliyet tasarrufları ve avantajları sağlayabilir. Örneğin, KÇY kullanılması pahalı katman 1 depolama gereksinimini azaltabilir ve içerik veritabanını küçültiğinden, KÇY SharePoint sunucu grubunda gereken veritabanlarının sayısını azaltabilir. Ancak, veritabanı boyutu sınırları ve KÇY olmayan içerik miktarı gibi diğer faktörler de depolama gereksinimlerini etkileyebilir. KÇY kullanmanın maliyetleri ve avantajları hakkında daha fazla bilgi için bkz. [KÇY Için plan (SharePoint Foundation 2010)][4] ve [SharePoint 2013 ' de KÇY kullanmaya karar verme][5].

### <a name="capacity-and-performance-limits"></a>Kapasite ve performans sınırları
SharePoint çözümünüzde KÇY 'yi kullanmadan önce, SharePoint Server 2010 ve SharePoint Server 2013 ' nin sınanmış performans ve kapasite limitlerini ve bu limitlerin kabul edilebilir performansla ilişkisini bilmeniz gerekir. Daha fazla bilgi için bkz. [SharePoint 2013 Için yazılım sınırları ve sınırları](https://technet.microsoft.com/library/cc262787.aspx).

KÇY 'yi yapılandırmadan önce aşağıdakileri gözden geçirin:

* İçeriğin toplam boyutunun (bir içerik veritabanının boyutu ile ilişkili externalized bloblarının boyutu) SharePoint tarafından desteklenen KÇY boyut sınırını aşmadığından emin olun. Bu sınır 200 GB 'dir. 
  
    **İçerik veritabanını ve BLOB boyutunu ölçmek için**
  
  1. Bu sorguyu merkezi yönetim WFE üzerinde çalıştırın. SharePoint Yönetim Kabuğu 'nu başlatın ve ardından içerik veritabanlarının boyutunu almak için aşağıdaki Windows PowerShell komutunu girin:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Bu adım, diskteki içerik veritabanının boyutunu alır.
  2. Her bir içerik veritabanının SQL Server kutusunda SQL Management Studio aşağıdaki SQL sorgularından birini çalıştırın ve sonucu 1. adımda elde edilen numaraya ekleyin.
     
     SharePoint 2013 içerik veritabanlarında şunu girin:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     SharePoint 2010 içerik veritabanlarında şunu girin:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Bu adım externalized olan Blobların boyutunu alır.
* Tüm BLOB ve veritabanı içeriğini StorSimple cihazında yerel olarak depolamanızı öneririz. StorSimple cihazı, yüksek kullanılabilirlik için iki düğümlü bir kümedir. İçerik veritabanlarının ve Blobların StorSimple cihazına yerleştirilmesi yüksek kullanılabilirlik sağlar.
  
    İçerik veritabanını StorSimple cihazına taşımak için geleneksel SQL Server geçişi en iyi yöntemlerini kullanın. Veritabanını yalnızca, veritabanındaki tüm BLOB içeriği KÇY aracılığıyla dosya paylaşımında taşındıktan sonra taşıyın. İçerik veritabanını StorSimple cihazına taşımayı seçerseniz, cihazdaki içerik veritabanı depolama alanını birincil birim olarak yapılandırmanızı öneririz.
* Microsoft Azure StorSimple, katmanlı birimler kullanılıyorsa, StorSimple cihazında yerel olarak depolanan içeriğin Microsoft Azure bulut depolama alanına katmanlanmadığını garanti etmenin bir yolu yoktur. Bu nedenle, StorSimple yerel olarak sabitlenmiş birimleri SharePoint KÇY ile birlikte kullanmanızı öneririz. Bu, tüm BLOB içeriğinin StorSimple cihazında yerel olarak kalmasını ve Microsoft Azure taşınmadığını sağlar.
* İçerik veritabanlarını StorSimple cihazında depolamadıysanız, KÇY 'yi destekleyen geleneksel SQL Server yüksek kullanılabilirliğe sahip en iyi uygulamaları kullanın. SQL Server kümeleme KÇY 'yi destekler, ancak SQL Server yansıtma bunu yapmaz. 

> [!WARNING]
> KÇY 'yi etkinleştirmediyseniz, içerik veritabanını StorSimple cihazına taşımayı önermiyoruz. Bu, test edilmemiş bir yapılandırmadır.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>SharePoint için StorSimple Bağdaştırıcısı yüklemesi
SharePoint için StorSimple bağdaştırıcısını yüklemeden önce, StorSimple cihazını yapılandırmanız ve SharePoint sunucu grubunun ve SQL Server örneklemesinin tüm önkoşulları karşıladığından emin olmanız gerekir. Bu öğreticide, yapılandırma gereksinimlerinin yanı sıra StorSimple bağdaştırıcısını SharePoint için yükleme ve yükseltme yordamları açıklanmaktadır.

## <a name="configure-prerequisites"></a>Önkoşulları yapılandırma
SharePoint için StorSimple bağdaştırıcısını yüklemeden önce, StorSimple cihazının, SharePoint sunucu grubunun ve SQL Server örneğinin aşağıdaki önkoşulları karşıladığından emin olun.

### <a name="system-requirements"></a>Sistem Gereksinimleri
SharePoint için StorSimple bağdaştırıcısı, aşağıdaki donanım ve yazılımla birlikte kullanılabilir:

* Desteklenen işletim sistemi – Windows Server 2008 R2 SP1, Windows Server 2012 veya Windows Server 2012 R2
* Desteklenen SharePoint sürümleri – SharePoint Server 2010 veya SharePoint Server 2013
* Desteklenen SQL Server sürümleri – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition veya SQL Server 2012 Enterprise Edition
* Desteklenen StorSimple cihazları – StorSimple 8000 serisi, StorSimple 7000 serisi veya StorSimple 5000 serisi.

### <a name="storsimple-device-configuration-prerequisites"></a>StorSimple cihaz yapılandırma önkoşulları
StorSimple cihazı bir blok aygıtıdır ve bu nedenle verilerin barındırıldığı bir dosya sunucusu gerekir. SharePoint grubundan mevcut bir sunucu yerine ayrı bir sunucu kullanmanızı öneririz. Bu dosya sunucusu, içerik veritabanlarını barındıran SQL Server bilgisayar ile aynı yerel ağda (LAN) olmalıdır.

> [!TIP]
> * SharePoint grubunuzu yüksek kullanılabilirlik için yapılandırırsanız, dosya sunucusunu da yüksek kullanılabilirlik için dağıtmanız gerekir.
> * İçerik veritabanını StorSimple cihazında depolamadıysanız, KÇY 'yi destekleyen geleneksel yüksek kullanılabilirliğe sahip en iyi uygulamaları kullanın. SQL Server kümeleme KÇY 'yi destekler, ancak SQL Server yansıtma bunu yapmaz. 


StorSimple cihazınızın doğru şekilde yapılandırıldığından ve SharePoint Dağıtımınızı desteklemek için uygun birimlerin SQL Server bilgisayarınızdan yapılandırıldığından ve erişilebilir olduğundan emin olun. StorSimple cihazınızı henüz dağıtmadıysanız ve yapılandırmadıysanız Şirket [Içi StorSimple cihazınızı dağıtma](storsimple-8000-deployment-walkthrough-u2.md) bölümüne gidin. StorSimple cihazının IP adresini aklınızda edin; SharePoint için StorSimple Bağdaştırıcısı yüklemesi sırasında buna ihtiyacınız olacaktır.

Ayrıca, BLOB dışsallaştırılması için kullanılacak birimin aşağıdaki gereksinimleri karşıladığından emin olun:

* Birim, 64 KB 'lik bir ayırma birimi boyutuyla biçimlendirilmelidir.
* Web ön ucu (WFE) ve uygulama sunucularınız, bir evrensel adlandırma kuralı (UNC) yolu aracılığıyla birime erişebilmelidir.
* SharePoint sunucu grubu, birime yazılacak şekilde yapılandırılmalıdır.

> [!NOTE]
> Bağdaştırıcıyı yükledikten ve yapılandırdıktan sonra, tüm BLOB dışsallaştırılması StorSimple cihazını (cihaz SQL Server ve depolama katmanlarını yönetmek için birimleri sunar). BLOB dışsallaştırılması için başka bir hedef kullanamazsınız.


BLOB ve veritabanı verilerinin anlık görüntülerini almak için StorSimple Snapshot Manager kullanmayı planlıyorsanız, Windows Birim Gölge Kopyası Hizmeti (VSS) uygulamasını uygulamak için SQL Writer Hizmeti kullanabilmesi için, veritabanı sunucusuna StorSimple Snapshot Manager yüklediğinizden emin olun.

> [!IMPORTANT]
> StorSimple Snapshot Manager, SharePoint VSS yazıcısını desteklemez ve SharePoint verilerinin uygulamayla tutarlı anlık görüntülerini alamaz. Bir SharePoint senaryosunda, StorSimple Snapshot Manager yalnızca çökme ile tutarlı yedeklemeler sağlar.


## <a name="sharepoint-farm-configuration-prerequisites"></a>SharePoint grubu yapılandırma önkoşulları
SharePoint sunucu grubunuzun doğru yapılandırıldığından emin olun, aşağıdaki gibi:

* SharePoint sunucu grubunuzun sağlıklı durumda olduğundan emin olun ve aşağıdakileri denetleyin:
* Grupta kayıtlı tüm SharePoint WFE ve uygulama sunucuları çalışıyor ve SharePoint için StorSimple bağdaştırıcısını yükleyeceğiniz sunucudan ping uygulanabilir.
* SharePoint Zamanlayıcı hizmeti (SPTimerV3 veya SPTimerV4) her WFE sunucusunda ve uygulama sunucusunda çalışmaktadır.
* Hem SharePoint Zamanlayıcı hizmeti hem de SharePoint Merkezi yönetim sitesinin çalıştırıldığı IIS uygulama havuzu yönetim ayrıcalıklarına sahiptir.
* Internet Explorer Artırılmış güvenlik bağlamının (IE ESC) devre dışı bırakıldığından emin olun. IE ESC 'yi devre dışı bırakmak için şu adımları izleyin:
  
  1. Internet Explorer 'ın tüm örneklerini kapatın.
  2. Sunucu Yöneticisi başlatın.
  3. Sol bölmede **yerel sunucu**' ya tıklayın.
  4. Sağ bölmedeki **IE artırılmış güvenlik yapılandırması**' nın yanındaki **Açık**' a tıklayın.
  5. **Yöneticiler**altında **kapalı**' ya tıklayın.
  6. **Tamam**'a tıklayın.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Uzak BLOB depolama (KÇY) önkoşulları
SQL Server desteklenen bir sürümünü kullandığınızdan emin olun. Yalnızca aşağıdaki sürümler desteklenir ve KÇY 'yi kullanabilir:

* SQL Server 2008 Enterprise Edition
* SQL Server 2008 R2 Enterprise Edition
* SQL Server 2012 Enterprise Edition

Blob 'Lar yalnızca StorSimple cihazının SQL Server sunmakta olduğu birimlerde externalized olabilir. BLOB dışsallaştırılması için başka bir hedef desteklenmez.

Önkoşul olan tüm yapılandırma adımlarını tamamladıktan sonra, [SharePoint Için StorSimple bağdaştırıcısını yüklemeye](#install-the-storsimple-adapter-for-sharepoint)gidin.

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısını yükler
SharePoint için StorSimple bağdaştırıcısını yüklemek üzere aşağıdaki adımları kullanın. Yazılımı yeniden yüklüyorsanız, bkz. [SharePoint Için StorSimple bağdaştırıcısını yükseltme veya yeniden yükleme](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint). Yükleme için gereken süre, SharePoint sunucu grubunuzdaki toplam SharePoint veritabanı sayısına bağlıdır.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>KÇY 'yi yapılandırma
SharePoint için StorSimple bağdaştırıcısını yükledikten sonra, aşağıdaki yordamda açıklandığı gibi KÇY 'yi yapılandırın.

> [!TIP]
> SharePoint için StorSimple bağdaştırıcısı, SharePoint Merkezi Yönetim sayfasına takılır ve SharePoint grubundaki her bir içerik veritabanında KÇY 'nin etkinleştirilmesini veya devre dışı bırakılacağını sağlar. Ancak, içerik veritabanında KÇY 'nin etkinleştirilmesi veya devre dışı bırakılması, Grup yapılandırmanıza bağlı olarak SharePoint Web ön ucunun (WFE) kullanılabilirliğini geçici olarak kesintiye uğratan bir IIS sıfırlamasına neden olabilir. (Ön uç yük dengeleyicinin kullanımı, geçerli sunucu iş yükü vb. gibi etkenler, bu kesintiyi sınırlayabilir veya ortadan kaldırabilir.) Kullanıcıların kesintiye uğramasını önlemek için, yalnızca planlanan bakım penceresi sırasında KÇY 'yi etkinleştirmenizi veya devre dışı bırakmanızı öneririz.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>Çöp toplamayı yapılandırma
Nesneler bir SharePoint sitesinden silindiğinde, otomatik olarak KÇY mağaza biriminden silinmez. Bunun yerine, zaman uyumsuz bir arka plan bakım programı dosya deposundan yalnız bırakılmış Blobları siler. Sistem yöneticileri, bu işlemi düzenli aralıklarla çalışacak şekilde zamanlayabilir veya gerektiğinde başlatabilir.

Bu bakım programı (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe), KÇY 'yi etkinleştirdiğinizde tüm SharePoint WFE sunucularına ve uygulama sunucularına otomatik olarak yüklenir. Program şu konuma yüklendi: *önyükleme sürücüsü*: \Program Files\Microsoft SQL Remote blob Storage 10.50 olan \ bakımcı \

Bakım programını yapılandırma ve kullanma hakkında daha fazla bilgi için bkz. [SharePoint Server 'DA KÇY 'Yi koruma 2013][8].

> [!IMPORTANT]
> KÇY bakım programı kaynak açısından yoğun. Yalnızca SharePoint grubundaki hafif etkinlik dönemlerinde çalışacak şekilde zamanlamanız gerekir.


### <a name="delete-orphaned-blobs-immediately"></a>Yalnız bırakılmış Blobları hemen Sil
Yalnız bırakılmış Blobları hemen silmeniz gerekiyorsa aşağıdaki yönergeleri kullanabilirsiniz. Bu yönergelerin bir SharePoint 2013 ortamında aşağıdaki bileşenlerle nasıl yapılabileceği hakkında bir örnek olduğunu unutmayın:

* İçerik veritabanı adı WSS_Content.
* SQL Server adı SHRPT13-SQL12\SHRPT13.
* Web uygulaması adı SharePoint – 80 ' dir.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple bağdaştırıcısını yükseltme veya yeniden yükleme
SharePoint Server 'ı yükseltmek için aşağıdaki yordamı kullanın ve SharePoint için StorSimple bağdaştırıcısını yeniden yükleyin veya var olan bir SharePoint sunucu grubunda bağdaştırıcıyı yükseltmeniz ya da yeniden yüklemeniz yeterlidir.

> [!IMPORTANT]
> SharePoint yazılımınızı yükseltmeyi ve/veya StorSimple bağdaştırıcısını SharePoint için yükseltmeyi veya yeniden yüklemeyi denemeden önce aşağıdaki bilgileri gözden geçirin:
> 
> * Daha önce KÇY aracılığıyla dış depolamaya taşınan dosyalar, yeniden yükleme tamamlanana ve KÇY özelliği tekrar etkinleştirilene kadar kullanılamaz. Kullanıcı etkisini sınırlandırmak için, planlanan bir bakım penceresi sırasında herhangi bir yükseltme veya yeniden yükleme gerçekleştirin.
> * Yükseltme/yeniden yükleme için gereken süre, SharePoint sunucu grubundaki SharePoint veritabanlarının toplam sayısına bağlı olarak farklılık gösterebilir.
> * Yükseltme/yeniden yükleme tamamlandıktan sonra içerik veritabanları için KÇY 'yi etkinleştirmeniz gerekir. Daha fazla bilgi için bkz. [KÇY 'Yi yapılandırma](#configure-rbs) .
> * Çok fazla sayıda veritabanına (200 'den büyük) sahip bir SharePoint grubu için KÇY yapılandırıyorsanız, **SharePoint Merkezi Yönetim** sayfası zaman aşımına uğrar. Böyle bir durumla karşılaşırsanız, sayfayı yenileyin. Bu, yapılandırma işlemini etkilemez.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>SharePoint için StorSimple Bağdaştırıcısı kaldırma
Aşağıdaki yordamlarda, Blobların SQL Server içerik veritabanlarına nasıl geri taşınacağı ve SharePoint için StorSimple bağdaştırıcısını nasıl kaldıracağınız açıklanır. 

> [!IMPORTANT]
> Bağdaştırıcı yazılımını kaldırmadan önce Blobları içerik veritabanlarına geri taşımanız gerekir.


### <a name="before-you-begin"></a>Başlamadan önce
Verileri SQL Server içerik veritabanlarına geri taşıdıktan sonra bağdaştırıcıyı kaldırma işlemine başlamadan önce aşağıdaki bilgileri toplayın:

* KÇY 'nin etkinleştirildiği tüm veritabanlarının adları
* Yapılandırılan BLOB deposunun UNC yolu

### <a name="move-the-blobs-back-to-the-content-databases"></a>Blobları içerik veritabanlarına geri taşıma
SharePoint yazılımı için StorSimple bağdaştırıcısını kaldırmadan önce, externalized olan tüm Blobları SQL Server içerik veritabanlarına geçirmeniz gerekir. Tüm Blobları içerik veritabanlarına geri taşımadan önce SharePoint için StorSimple bağdaştırıcısını kaldırmaya çalışırsanız, aşağıdaki uyarı iletisini görürsünüz.

![Uyarı iletisi](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="to-move-the-blobs-back-to-the-content-databases"></a>Blobları içerik veritabanlarına geri taşımak için
1. Externalized nesnelerinin her birini indirin.
2. **SharePoint Merkezi Yönetim** sayfasını açın ve **sistem ayarları**' na gidin.
3. **Azure StorSimple**altında **StorSimple bağdaştırıcısını Yapılandır**' a tıklayın.
4. **StorSimple bağdaştırıcısını Yapılandır** sayfasında, dış blob depolamadan kaldırmak istediğiniz her içerik veritabanlarının altındaki **devre dışı bırak** düğmesine tıklayın. 
5. Nesneleri SharePoint 'ten silin ve sonra yeniden yükleyin.

Alternatif olarak, `RBS Migrate()` SharePoint 'e dahil olan Microsoft PowerShell cmdlet 'ini de kullanabilirsiniz. Daha fazla bilgi için bkz. [KÇY içine veya dışına Içerik geçirme](https://technet.microsoft.com/library/ff628255.aspx).

Blobları içerik veritabanına geri taşıdıktan sonra sonraki adıma gidin: [bağdaştırıcıyı kaldırın](#uninstall-the-adapter).

### <a name="uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırma
Blob 'Ları SQL Server içerik veritabanlarına geri taşıdıktan sonra, SharePoint için StorSimple bağdaştırıcısını kaldırmak üzere aşağıdaki seçeneklerden birini kullanın.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırmak için yükleme programını kullanmak için
1. Web ön ucu (WFE) sunucusunda oturum açmak için yönetici ayrıcalıklarına sahip bir hesap kullanın.
2. SharePoint için StorSimple Bağdaştırıcısı Yükleyicisi ' ne çift tıklayın. Kurulum Sihirbazı başlatılır.
   
    ![Kurulum Sihirbazı](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. **İleri**’ye tıklayın. Aşağıdaki sayfa görüntülenir.
   
    ![Kurulum Sihirbazı sayfayı kaldır](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Kaldırma işlemini seçmek için **Kaldır** ' a tıklayın. Aşağıdaki sayfa görüntülenir.
   
    ![Kurulum Sihirbazı onay sayfası](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Kaldırmayı onaylamak için **Kaldır** ' a tıklayın. Aşağıdaki ilerleme durumu sayfası görüntülenir.
   
    ![Kurulum Sihirbazı ilerleme durumu sayfası](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. Kaldırma tamamlandığında, son sayfası görüntülenir. Kurulum sihirbazını kapatmak için **son** ' a tıklayın.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırmak için Denetim Masası 'nı kullanmak için
1. Denetim Masası ' nı açın ve ardından **Programlar ve Özellikler**' e tıklayın.
2. **SharePoint Için StorSimple Bağdaştırıcısı**' nı seçin ve ardından **Kaldır**' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple hakkında daha fazla bilgi edinin](storsimple-overview.md).

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/library/ff943565.aspx
