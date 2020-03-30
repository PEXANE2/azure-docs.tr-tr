---
title: SharePoint için StorSimple Adaptörü yükleyin | Microsoft Dokümanlar
description: SharePoint sunucu çiftliğindeki SharePoint için StorSimple Adaptör'ün nasıl yüklenir, yapılandırılır veya kaldırılır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75930217"
---
# <a name="install-and-configure-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Adaptörü'ni yükleyin ve yapılandırır
## <a name="overview"></a>Genel Bakış
SharePoint için StorSimple Adaptörü, SharePoint sunucu çiftliklerine Microsoft Azure StorBasit esnek depolama ve veri koruması sağlamanızı sağlayan bir bileşendir. Bağdaştırıcıyı, Ikili Büyük Nesne (BLOB) içeriğini SQL Server içerik veritabanlarından Microsoft Azure StorSimple karma bulut depolama aygıtına taşımak için kullanabilirsiniz.

SharePoint için StorSimple Adaptörü, Uzak BLOB Depolama (RBS) sağlayıcısı olarak işlev görür ve Yapılandırılmış SharePoint içeriğini (BLOB s halinde) StorSimple aygıtı tarafından desteklenen bir dosya sunucusunda depolamak için SQL Server Remote BLOB Depolama özelliğini kullanır.

> [!NOTE]
> SharePoint için StorSimple Adaptörü SharePoint Server 2010 Remote BLOB Depolama (RBS) destekler. SharePoint Server 2010 Harici BLOB Depolama (EBS) desteklemez.


* SharePoint için StorSimple Adapter'ı indirmek için Microsoft Download Center'daki [SharePoint için StorSimple Adapter'a][1] gidin.
* RBS ve RBS sınırlamaları için planlama hakkında daha fazla bilgi için, [SharePoint 2013'te VEYA][2] [Plan for RBS'de (SharePoint Server 2010)][3]RBS kullanmaya karar verme konusuna gidin.

Bu genel bakışın geri kalanı, SharePoint için StorSimple Adaptörü'nün rolünü ve bağdaştırıcıyı yüklemeden ve yapılandırmadan önce farkında olmanızı gereken SharePoint kapasite ve performans sınırlarını kısaca açıklar. Bu bilgileri gözden geçirdikten sonra, bağdaştırıcıyı kurmaya başlamak [için SharePoint yüklemesi için StorSimple Adapter'a](#storsimple-adapter-for-sharepoint-installation) gidin.

### <a name="storsimple-adapter-for-sharepoint-benefits"></a>SharePoint avantajları için StorSimple Adaptörü
SharePoint sitesinde içerik, bir veya daha fazla içerik veritabanlarında yapılandırılmamış BLOB verileri olarak depolanır. Varsayılan olarak, bu veritabanları SQL Server çalıştıran bilgisayarlarda barındırılır ve SharePoint sunucu çiftliğinde bulunur. BLOB'lar hızla boyutu artabilir, şirket içi depolama büyük miktarlarda tüketen. Bu nedenle, başka, daha az pahalı depolama çözümü bulmak isteyebilirsiniz. SQL Server, BLOB içeriğini SQL Server veritabanının dışında dosya sisteminde depolamanızı sağlayan Remote Blob Depolama (RBS) adı verilen bir teknoloji sağlar. RBS ile, BLOBs SQL Server çalıştıran bilgisayarda dosya sisteminde bulunabilir veya başka bir sunucu bilgisayarında dosya sisteminde saklanabilir.

RBS, SharePoint'te RBS'yi etkinleştirmek için SharePoint için StorSimple Adapter gibi bir RBS sağlayıcısı kullanmanızgerektiğini gerektirir. SharePoint için StorSimple Adaptörü, BLOB'ları Microsoft Azure StorSimple sistemi tarafından desteklenen bir sunucuya taşımanızı sağlayarak RBS ile birlikte çalışır. Microsoft Azure StorSimple, kullanıma bağlı olarak BLOB verilerini yerel olarak veya bulutta saklar. Çok etkin olan BLOB'lar (genellikle Katman 1 veya sıcak veri olarak adlandırılır) yerel olarak kullanılır. Daha az etkin veri ve arşiv verileri bulutta bulunmaktadır. Bir içerik veritabanında RBS'yi etkinleştirdikten sonra, SharePoint'te oluşturulan tüm yeni BLOB içeriği içerik veritabanında değil, StorSimple aygıtında depolanır.

RBS'nin Microsoft Azure StorBasit uygulaması aşağıdaki avantajları sağlar:

* BLOB içeriğini ayrı bir sunucuya taşıyarak, SQL Server'daki sorgu yükünü azaltarak SQL Server yanıt verme yeteneğini artırabilirsiniz. 
* Azure StorSimple, veri boyutunu azaltmak için çoğaltma ve sıkıştırma kullanır.
* Azure StorSimple, yerel ve bulut anlık görüntüleri şeklinde veri koruması sağlar. Ayrıca, veritabanının kendisini StorSimple aygıtına yerletirseniz, içerik veritabanını ve BLOB'ları tutarlı bir şekilde birlikte yedekleyebilirsiniz. (İçerik veritabanının aygıta taşınması yalnızca StorSimple 8000 serisi aygıt için desteklenir. Bu özellik 5000 veya 7000 serisi için desteklenmez.)
* Azure StorSimple, başarısız, dosya ve toplu kurtarma (test kurtarma dahil) ve verilerin hızlı bir şekilde geri kazanılması gibi olağanüstü durum kurtarma özelliklerini içerir.
* PayPoint içeriğinin öğe düzeyinde kurtarılmasını gerçekleştirmek için BLOB verilerinin StorSimple anlık görüntüleriyle Kroll Ontrack PowerControls gibi veri kurtarma yazılımını kullanabilirsiniz. (Bu veri kurtarma yazılımı ayrı bir satın almadır.)
* SharePoint için StorSimple Adaptörü, SharePoint Merkezi Yönetim portalına bağlanarak tüm SharePoint çözümünüzü merkezi bir konumdan yönetmenize olanak sağlar.

BLOB içeriğinin dosya sistemine taşınması diğer maliyet tasarrufları ve avantajlar sağlayabilir. Örneğin, RBS kullanmak pahalı Tier 1 depolama gereksinimini azaltabilir ve içerik veritabanını küçülttİğİnden, RBS SharePoint sunucu çiftliğinde gereken veritabanlarının sayısını azaltabilir. Ancak, veritabanı boyutu sınırları ve RBS dışı içerik miktarı gibi diğer etkenler de depolama gereksinimlerini etkileyebilir. RBS kullanmanın maliyetleri ve yararları hakkında daha fazla bilgi için [bkz.][4] [Deciding to use RBS in SharePoint 2013][5]

### <a name="capacity-and-performance-limits"></a>Kapasite ve performans sınırları
SharePoint çözümünde RBS kullanmayı düşünmeden önce, SharePoint Server 2010 ve SharePoint Server 2013'ün test edilmiş performans ve kapasite sınırlarının ve bu sınırların kabul edilebilir performansla nasıl ilişkili olduğunu biliyor olmalısınız. Daha fazla bilgi [için SharePoint 2013 için Yazılım Sınırları ve Sınırları'na](https://technet.microsoft.com/library/cc262787.aspx)bakın.

RBS yapılandırmadan önce aşağıdakileri gözden geçirin:

* İçeriğin toplam boyutunun (içerik veritabanının boyutu ve ilişkili dışsallaştırılmış BLOB'ların boyutu) SharePoint tarafından desteklenen RBS boyut sınırını aşmadığından emin olun. Bu sınır 200 GB'dır. 
  
    **İçerik veritabanını ve BLOB boyutunu ölçmek için**
  
  1. Bu sorguyu Merkezi Yönetim WFE'de çalıştırın. SharePoint Management Shell'i başlatın ve içerik veritabanlarının boyutunu almak için aşağıdaki Windows PowerShell komutunu girin:
     
     `Get-SPContentDatabase | Select-Object -ExpandProperty DiskSizeRequired`
     
      Bu adım, diskteki içerik veritabanının boyutunu alır.
  2. Her içerik veritabanındaki SQL server kutusunda SQL Management Studio'da aşağıdaki SQL sorgularından birini çalıştırın ve sonucu adım 1'de elde edilen sayıya ekleyin.
     
     SharePoint 2013 içerik veritabanlarına şunları girin:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[DocStreams] WHERE [Content] IS NULL`
     
     SharePoint 2010 içerik veritabanlarına şunları girin:
     
     `SELECT SUM([Size]) FROM [ContentDatabaseName].[dbo].[AllDocs] WHERE [Content] IS NULL`
     
     Bu adım, dışa dışlanmış BLOB'ların boyutunu alır.
* Tüm BLOB ve veritabanı içeriğini StorSimple aygıtında yerel olarak saklamanızı öneririz. StorSimple aygıtı, yüksek kullanılabilirlik için iki düğümlü bir kümedir. İçerik veritabanlarını ve BLOB'ları StorSimple cihazına yerleştirmek yüksek kullanılabilirlik sağlar.
  
    İçerik veritabanını StorSimple aygıtına taşımak için geleneksel SQL Server geçiş uygulamalarını kullanın. Veritabanındaki tüm BLOB içeriği RBS aracılığıyla dosya paylaşımına taşındıktan sonra veritabanını taşıyın. İçerik veritabanını StorSimple aygıtına taşımayı seçerseniz, aygıttaki içerik veritabanı depolama alanını birincil birim olarak yapılandırmanızı öneririz.
* Microsoft Azure StorSimple'da, katmanlı birimler kullanıyorsanız, StorSimple aygıtında yerel olarak depolanan içeriğin Microsoft Azure bulut depolamasına katmanlandırılmayacağını garanti etmenin bir yolu yoktur. Bu nedenle, SharePoint RBS ile birlikte StorSimple yerel olarak sabitlenmiş hacimleri kullanmanızı öneririz. Bu, tüm BLOB içeriğinin StorSimple aygıtında yerel olarak kalmasını ve Microsoft Azure'a taşınmamasını sağlar.
* İçerik veritabanlarını StorSimple aygıtında depolamazsanız, RbS'yi destekleyen geleneksel SQL Server yüksek kullanılabilirlik en iyi uygulamalarını kullanın. SQL Server kümeleme, RBS'yi desteklerken, SQL Server yansıtma sı desteklemez. 

> [!WARNING]
> RbS'yi etkinleştirmediyseniz, içerik veritabanını StorSimple aygıtına taşımanızı önermiyoruz. Bu test edilmemiş bir yapılandırmadır.

## <a name="storsimple-adapter-for-sharepoint-installation"></a>SharePoint kurulumu için StorSimple Adaptörü
SharePoint için StorSimple Adaptör'üne yüklemeden önce, StorSimple aygıtını yapılandırmanız ve SharePoint sunucu çiftliği ve SQL Server anlık durumunun tüm ön koşulları karşıladığından emin olmalısınız. Bu öğretici, SharePoint için StorSimple Adaptörü'nün yüklenmesi ve yükseltilmesiiçin yapılandırma gereksinimlerinin yanı sıra yordamları da açıklar.

## <a name="configure-prerequisites"></a>Ön koşulları yapılandırma
SharePoint için StorSimple Adaptör'üne yüklemeden önce, StorSimple cihazının, SharePoint sunucu ekskavasyonunun ve SQL Server anlık durumunun aşağıdaki ön koşulları karşıladığından emin olun.

### <a name="system-requirements"></a>Sistem gereksinimleri
SharePoint için StorSimple Adaptörü aşağıdaki donanım ve yazılımlarla çalışır:

* Desteklenen işletim sistemi – Windows Server 2008 R2 SP1, Windows Server 2012 veya Windows Server 2012 R2
* Desteklenen SharePoint sürümleri – SharePoint Server 2010 veya SharePoint Server 2013
* Desteklenen SQL Server sürümleri – SQL Server 2008 Enterprise Edition, SQL Server 2008 R2 Enterprise Edition veya SQL Server 2012 Enterprise Edition
* Desteklenen StorSimple cihazlar – StorSimple 8000 serisi, StorSimple 7000 serisi veya StorSimple 5000 serisi.

### <a name="storsimple-device-configuration-prerequisites"></a>StorBasit cihaz yapılandırma ön koşulları
StorSimple aygıtı bir blok aygıtıdır ve bu nedenle verilerin barındırılabildiği bir dosya sunucusu gerektirir. SharePoint çiftliğinden varolan bir sunucu yerine ayrı bir sunucu kullanmanızı öneririz. Bu dosya sunucusu, içerik veritabanlarını barındıran SQL Server bilgisayarıyla aynı yerel alan ağında (LAN) olmalıdır.

> [!TIP]
> * SharePoint çiftliğinizi yüksek kullanılabilirlik için yapılandırırsanız, dosya sunucusunu yüksek kullanılabilirlik için de dağıtmanız gerekir.
> * İçerik veritabanını StorSimple aygıtında depolamazsanız, RbS'yi destekleyen geleneksel yüksek kullanılabilirlik en iyi uygulamalarını kullanın. SQL Server kümeleme, RBS'yi desteklerken, SQL Server yansıtma sı desteklemez. 


StorSimple cihazınızın doğru şekilde yapılandırıldığından ve SharePoint dağıtımınızı destekleyecek uygun birimlerin SQL Server bilgisayarınızdan yapılandırıldığından ve erişilebilir olduğundan emin olun. StorSimple cihazınızı henüz dağıtmadıysanız ve yapılandırmadıysanız [şirket içi StorSimple aygıtınızı dağıtın.](storsimple-8000-deployment-walkthrough-u2.md) StorSimple cihazının IP adresine dikkat edin; SharePoint kurulumu için StorSimple Adaptörü sırasında ihtiyacınız olacaktır.

Ayrıca, BLOB dışsallaştırmaiçin kullanılacak ses düzeyinin aşağıdaki gereksinimleri karşıladığından emin olun:

* Birim 64 KB ayırma birimi boyutu ile biçimlendirilmelidir.
* Web ön uç (WFE) ve uygulama sunucuları evrensel adlandırma sözleşmesi (UNC) yolu üzerinden ses erişebilmelidir.
* SharePoint sunucu çiftliği, ses düzeyine yazacak şekilde yapılandırılmalıdır.

> [!NOTE]
> Bağdaştırıcıyı yükledikten ve yapılandırdıktan sonra, tüm BLOB haricileştirmenin StorSimple aygıtından geçmesi gerekir (aygıt birimleri SQL Server'a sunar ve depolama katmanlarını yönetir). BLOB dışsallaştırma sı için başka bir hedef kullanamazsınız.


BLOB ve veritabanı verilerinin anlık görüntülerini almak için StorSimple Snapshot Manager'ı kullanmayı planlıyorsanız, Windows Volume Shadow Copy Service'i (VSS) uygulamak için SQL Writer Service'i kullanabilmesi için StorSimple Snapshot Manager'ı veritabanı sunucusuna yüklediğinizden emin olun.

> [!IMPORTANT]
> StorSimple Snapshot Manager, SharePoint VSS Writer'ı desteklemez ve SharePoint verilerinin uygulama tutarlı anlık görüntülerini alamaz. Bir SharePoint senaryosunda, StorSimple Snapshot Manager yalnızca kilitlenme tutarlı yedeklemesağlar.


## <a name="sharepoint-farm-configuration-prerequisites"></a>SharePoint çiftlik yapılandırma ön koşulları
SharePoint sunucu çiftinizin aşağıdaki gibi doğru şekilde yapılandırıldığından emin olun:

* SharePoint sunucu çiftinizin sağlıklı bir durumda olduğunu doğrulayın ve aşağıdakileri kontrol edin:
* Çiftlikte kayıtlı tüm SharePoint WFE ve uygulama sunucuları çalışıyor ve SharePoint için StorSimple Adaptörü'nü kuracağınız sunucudan ping atılabilir.
* SharePoint Timer hizmeti (SPTimerV3 veya SPTimerV4) her WFE sunucusunda ve uygulama sunucusunda çalışır.
* Hem SharePoint Timer hizmeti hem de SharePoint Merkezi Yönetim sitesinin işlettiği IIS uygulama havuzu yönetim ayrıcalıklarına sahiptir.
* Internet Explorer Gelişmiş Güvenlik Bağlamı'nın (IE ESC) devre dışı bırakıldığından emin olun. IE ESC'yi devre dışı katmak için aşağıdaki adımları izleyin:
  
  1. Internet Explorer'ın tüm örneklerini kapatın.
  2. Sunucu Yöneticisi'ni başlatın.
  3. Sol bölmede Yerel **Sunucu'yu**tıklatın.
  4. Sağ bölmede, **IE Gelişmiş Güvenlik Yapılandırmasının**yanında, **Aç'ı**tıklatın.
  5. **Yöneticiler**altında, **Kapalı'yı**tıklatın.
  6. **Tamam**'a tıklayın.

## <a name="remote-blob-storage-rbs-prerequisites"></a>Uzak BLOB Depolama (RBS) ön koşullar
SQL Server'ın desteklenen bir sürümünü kullandığınızdan emin olun. Yalnızca aşağıdaki sürümler desteklenir ve KÇY'yi kullanabilir:

* SQL Server 2008 Kurumsal Sürüm
* SQL Server 2008 R2 Kurumsal Sürüm
* SQL Server 2012 Kurumsal Sürüm

BLOB'lar yalnızca StorSimple cihazının SQL Server'a sunduğu birimlerde dışsallaştırılabilir. BLOB dışsallaştırma için başka hedef desteklenmez.

Tüm ön koşul yapılandırma adımlarını tamamladığınızda, [SharePoint için StorSimple Adaptör'e yükleyin.](#install-the-storsimple-adapter-for-sharepoint)

## <a name="install-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Adaptörü yükleyin
SharePoint için StorSimple Adaptör'üne yüklemek için aşağıdaki adımları kullanın. Yazılımı yeniden yüklüyorsanız, [SharePoint için StorSimple Adaptörü'ne yükseltme veya yeniden yükleme](#upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint)konusuna bakın. Yükleme için gereken süre, SharePoint sunucu çiftliğinizdeki toplam SharePoint veritabanı sayısına bağlıdır.

[!INCLUDE [storsimple-install-sharepoint-adapter](../../includes/storsimple-install-sharepoint-adapter.md)]

## <a name="configure-rbs"></a>RbS'yi yapılandır
SharePoint için StorSimple Adaptörünü yükledikten sonra, aşağıdaki yordamda açıklandığı şekilde RBS yapılandırın.

> [!TIP]
> SharePoint için StorSimple Adaptörü, SharePoint farm'daki her içerik veritabanında RBS'nin etkinleştirilmesine veya devre dışı edilmesine olanak tanıyan SharePoint Merkezi Yönetim sayfasına takılır. Ancak, içerik veritabanında RbS'yi etkinleştirmek veya devre dışı bırakmak, çiftlik yapılandırmanıza bağlı olarak SharePoint web ön ucunun (WFE) kullanılabilirliğini anlık olarak bozabilecek bir IIS sıfırlamasına neden olur. (Ön uç yük dengeleyicisi kullanımı, geçerli sunucu iş yükü vb. gibi etkenler bu kesintiyi sınırlayabilir veya ortadan kaldırabilir.) Kullanıcıları bir kesintiye karşı korumak için, yalnızca planlanmış bir bakım penceresi sırasında KÇY'yi etkinleştirmenizi veya devre dışı etmenizi öneririz.


[!INCLUDE [storsimple-sharepoint-adapter-configure-rbs](../../includes/storsimple-sharepoint-adapter-configure-rbs.md)]

## <a name="configure-garbage-collection"></a>Çöp toplamayı yapılandırma
Nesneler SharePoint sitesinden silindiğinde, RÇY saklı mağazasından otomatik olarak silinmezler. Bunun yerine, bir eşzamanlı, arka plan bakım programı dosya deposundan yetim BLOBs siler. Sistem yöneticileri bu işlemi düzenli aralıklarla çalışacak şekilde zamanlayabilir veya gerektiğinde başlatabilirler.

Bu bakım programı (Microsoft.Data.SqlRemoteBlobs.Maintainer.exe) RBS'yi etkinleştirdiğinizde tüm SharePoint WFE sunucularına ve uygulama sunucularına otomatik olarak yüklenir. Program aşağıdaki konumda yüklenir: *önyükleme sürücüsü*:\Program Files\Microsoft SQL Remote Blob Depolama 10.50\Maintainer\

Bakım programının yapılandırılması ve kullanımı hakkında daha fazla bilgi için Bkz. [SharePoint Server 2013'te RbS'yi koruyun.][8]

> [!IMPORTANT]
> RBS bakım programı kaynak yoğundur. Yalnızca SharePoint çiftliğindeki hafif etkinlik dönemlerinde çalışacak şekilde zamanlamanız gerekir.


### <a name="delete-orphaned-blobs-immediately"></a>Yetim BLOB'ları hemen silme
Yetim BLOB'ları hemen silmeniz gerekiyorsa, aşağıdaki yönergeleri kullanabilirsiniz. Bu yönergelerin, aşağıdaki bileşenlerle sharepoint 2013 ortamında nasıl yapılabileceğinin bir örneği olduğunu unutmayın:

* İçerik veritabanı adı WSS_Content.
* SQL Server adı SHRPT13-SQL12\SHRPT13'dür.
* Web uygulama adı SharePoint - 80 olduğunu.

[!INCLUDE [storsimple-sharepoint-adapter-garbage-collection](../../includes/storsimple-sharepoint-adapter-garbage-collection.md)]

## <a name="upgrade-or-reinstall-the-storsimple-adapter-for-sharepoint"></a>SharePoint için StorSimple Adaptörü'ni yükseltme veya yeniden yükleme
SharePoint sunucusunu yükseltmek ve ardından SharePoint için StorSimple Adapter'ı yeniden yüklemek veya varolan bir SharePoint sunucu çiftliğinde bağdaştırıcıyı yükseltmek veya yeniden yüklemek için aşağıdaki yordamı kullanın.

> [!IMPORTANT]
> SharePoint yazılımınızı yükseltmeye ve/veya SharePoint için StorSimple Adapter'ı yükseltmeye veya yeniden yüklemeye çalışmadan önce aşağıdaki bilgileri gözden geçirin:
> 
> * Daha önce RBS aracılığıyla harici depolama ya taşınmış olan dosyalar, yeniden yükleme tamamlanana ve RbS özelliği yeniden etkinleştirilene kadar kullanılamaz. Kullanıcı etkisini sınırlamak için, planlanan bakım penceresi sırasında herhangi bir yükseltme veya yeniden yükleme gerçekleştirin.
> * Yükseltme/yeniden yükleme için gereken süre, SharePoint sunucu çiftliğindeki toplam SharePoint veritabanı sayısına bağlı olarak değişebilir.
> * Yükseltme/yeniden yükleme tamamlandıktan sonra, içerik veritabanları için KÇY'yi etkinleştirmeniz gerekir. Daha fazla bilgi için [BÇB S'yi Yapılandır'](#configure-rbs) a bakın.
> * Çok sayıda veritabanı (200'den büyük) olan bir SharePoint çiftliği için RBS yapılandırıyorsanız, **SharePoint Merkezi Yönetim** sayfası zaman alabilir. Bu durumda, sayfayı yenileyin. Bu yapılandırma işlemini etkilemez.


[!INCLUDE [storsimple-upgrade-sharepoint-adapter](../../includes/storsimple-upgrade-sharepoint-adapter.md)]

## <a name="storsimple-adapter-for-sharepoint-removal"></a>SharePoint kaldırma için StorSimple Adaptörü
Aşağıdaki yordamlar, BLOB'ların SQL Server içerik veritabanlarına nasıl geri taşınıp sharepoint için StorSimple Adapter'ını kaldırdığını açıklar. 

> [!IMPORTANT]
> Bağdaştırıcı yazılımını kaldırmadan önce BLOB'ları içerik veritabanlarına geri taşımanız gerekir.


### <a name="before-you-begin"></a>Başlamadan önce
Verileri SQL Server içerik veritabanlarına geri taşımadan ve bağdaştırıcı kaldırma işlemini başlatmadan önce aşağıdaki bilgileri toplayın:

* RBS etkin olduğu tüm veritabanlarının adları
* Yapılandırılan BLOB deposunun UNC yolu

### <a name="move-the-blobs-back-to-the-content-databases"></a>BLOB'ları içerik veritabanlarına geri taşıma
SharePoint yazılımı için StorSimple Adaptör'ü kaldırmadan önce, haricileştirilmiş tüm BLOB'ları SQL Server içerik veritabanlarına geçirmeniz gerekir. Tüm BLOB'ları içerik veritabanlarına geri taşımadan önce SharePoint için StorSimple Adapter'ı kaldırmaya çalışırsanız, aşağıdaki uyarı iletisini görürsünüz.

![Uyarı iletisi](./media/storsimple-adapter-for-sharepoint/sasp1.png)

#### <a name="to-move-the-blobs-back-to-the-content-databases"></a>BLOB'ları içerik veritabanlarına geri taşımak için
1. Dışa daki nesnelerin her birini indirin.
2. **SharePoint Merkezi Yönetim** sayfasını açın ve **Sistem Ayarları'na**göz atın.
3. **Azure StorSimple**altında, **StorSimple Adapter'ı Yapılandır'ı**tıklatın.
4. **StorSimple Adaptör'ü Yapılandır** sayfasında, harici BLOB depolamadan kaldırmak istediğiniz içerik veritabanlarının her birinin altındaki **Devre Ver** düğmesini tıklatın. 
5. Nesneleri SharePoint'ten silin ve yeniden yükleyin.

Alternatif olarak, SharePoint `RBS Migrate()` ile birlikte microsoft PowerShell cmdlet kullanabilirsiniz. Daha fazla bilgi için [bkz.](https://technet.microsoft.com/library/ff628255.aspx)

BLOB'ları içerik veritabanına taşıdıktan sonra bir sonraki adıma geçin: [Bağdaştırıcıyı kaldırın.](#uninstall-the-adapter)

### <a name="uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırın
BLOB'ları SQL Server içerik veritabanlarına taşıdıktan sonra, SharePoint için StorSimple Adaptör'ü kaldırmak için aşağıdaki seçeneklerden birini kullanın.

#### <a name="to-use-the-installation-program-to-uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırmak için yükleme programını kullanmak için
1. Web ön uç (WFE) sunucusunda oturum açmak için yönetici ayrıcalıklarına sahip bir hesap kullanın.
2. SharePoint yükleyicisi için StorSimple Adaptörü'ne çift tıklayın. Kurulum Sihirbazı başlar.
   
    ![Kurulum sihirbazı](./media/storsimple-adapter-for-sharepoint/sasp2.png)
3. **İleri**'ye tıklayın. Aşağıdaki sayfa görüntülenir.
   
    ![Kurulum sihirbazı sayfayı kaldırma](./media/storsimple-adapter-for-sharepoint/sasp3.png)
4. Kaldırma işlemini seçmek için **Kaldır'ı** tıklatın. Aşağıdaki sayfa görüntülenir.
   
    ![Kurulum sihirbazı onay sayfası](./media/storsimple-adapter-for-sharepoint/sasp4.png)
5. Kaldırmayı onaylamak için **Kaldır'ı** tıklatın. Aşağıdaki ilerleme sayfası görüntülenir.
   
    ![Kurulum sihirbazı ilerleme sayfası](./media/storsimple-adapter-for-sharepoint/sasp5.png)
6. Kaldırma işlemi tamamlandığında, bitiş sayfası görüntülenir. Kurulum Sihirbazı'nı kapatmak için **Bitir'i** tıklatın.

#### <a name="to-use-the-control-panel-to-uninstall-the-adapter"></a>Bağdaştırıcıyı kaldırmak için Denetim Panelini kullanmak için
1. Denetim Masası'nı açın ve **ardından Programlar ve Özellikler'i**tıklatın.
2. **SharePoint için StorSimple Adaptörü'nü**seçin ve ardından **Kaldır'ı**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple hakkında daha fazla bilgi edinin.](storsimple-overview.md)

<!--Reference links-->
[1]: https://www.microsoft.com/download/details.aspx?id=44073
[2]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[3]: https://technet.microsoft.com/library/ff628583(v=office.14).aspx
[4]: https://technet.microsoft.com/library/ff628569(v=office.14).aspx
[5]: https://technet.microsoft.com/library/ff628583(v=office.15).aspx
[8]: https://technet.microsoft.com/library/ff943565.aspx
