---
title: StorSimple Snapshot Manager nedir? | Microsoft Belgeleri
description: StorSimple Snapshot Manager'ı, mimarisini ve özelliklerini açıklar.
services: storsimple
documentationcenter: NA
author: twooley
manager: timlt
editor: ''
ms.assetid: 6094c31e-e2d9-4592-8a15-76bdcf60a754
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: twooley
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e58e2d929dd1e4db16ce495ad54045e9dc3a6fb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267423"
---
# <a name="an-introduction-to-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager'a giriş

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, Microsoft Azure StorSimple ortamında veri korumayı ve yedekleme yönetimini kolaylaştıran bir Microsoft Yönetim Konsolu (MMC) snap-in'dir. StorSimple Snapshot Manager ile, microsoft Azure StorSimple verilerini veri merkezinde ve bulutta tek bir entegre depolama çözümü olarak yönetebilir, böylece yedekleme işlemlerini basitleştirebilir ve maliyetleri azaltabilirsiniz.

Bu genel bakış, StorSimple Snapshot Manager'ı tanıtıyor, özelliklerini açıklar ve Microsoft Azure StorSimple'daki rolünü açıklar. 

StorSimple cihazı, StorSimple Manager hizmeti, StorSimple Snapshot Manager ve SharePoint için StorSimple Adapter dahil olmak üzere tüm Microsoft Azure StorSimple sistemine genel bakış için [StorSimple 8000 serisi: karma bulut depolama çözümü](storsimple-overview.md)bölümüne bakın. 

> [!NOTE]
> * Microsoft Azure StorBasit Sanal Dizileri (storsimple şirket içi sanal aygıtlar olarak da bilinir) yönetmek için StorSimple Snapshot Manager'ı kullanamazsınız.
> * StorSimple Update 2'yi StorSimple cihazınıza yüklemeyi planlıyorsanız, **StorSimple Update 2'yi yüklemeden önce**StorSimple Snapshot Manager'ın en son sürümünü indirdiğinizden ve yüklediğinizden emin olun. StorSimple Snapshot Manager'ın en son sürümü geriye dönük uyumludur ve Microsoft Azure StorSimple'ın tüm sürümleriyle çalışır. StorSimple Snapshot Manager'ın önceki sürümünü kullanıyorsanız, güncelleştirmeniz gerekir (yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager amacı ve mimarisi
StorSimple Snapshot Manager, yerel ve bulut verilerinin tutarlı, anında yedekleme kopyalarını oluşturmak için kullanabileceğiniz merkezi bir yönetim konsolu sağlar. Örneğin, konsolu aşağıdakiler için kullanabilirsiniz:

* Birimleri yapılandırın, yedekleyin ve silin.
* Yedeklenen verilerin uygulama tutarlı olduğundan emin olmak için birim gruplarını yapılandırın.
* Verilerin önceden belirlenmiş bir zamanlamada yedeklenebilmeleri için yedekleme ilkelerini yönetin.
* Bulutta depolanabilen ve olağanüstü durum kurtarma için kullanılabilen yerel ve bulut anlık görüntülerini oluşturun.

StorSimple Snapshot Yöneticisi, ana bilgisayardaki VSS sağlayıcısına kayıtlı uygulamaların listesini getirir. Daha sonra, uygulama tutarlı yedeklemeler oluşturmak için, bir uygulama tarafından kullanılan birimleri denetler ve yapılandırmak için birim grupları önerir. StorSimple Snapshot Manager, uygulama tutarlı yedekleme kopyaları oluşturmak için bu birim gruplarını kullanır. (İlgili tüm dosya ve veritabanları eşitlendiğinde ve uygulamanın gerçek durumunu belirli bir zamanda temsil ettiğinde uygulama tutarlılığı vardır.) 

StorSimple Snapshot Manager yedekleri, son yedeklemeden bu yana yalnızca değişiklikleri yakalayan artımlı anlık görüntüler biçimindedir. Sonuç olarak, yedeklemeler daha az depolama alanı gerektirir ve hızlı bir şekilde oluşturulabilir ve geri yüklenebilir. StorSimple Snapshot Manager, anlık görüntünün uygulama tutarlı verileri yakalamasını sağlamak için Windows Birim Gölge Kopyalama Hizmeti'ni (VSS) kullanır. (Daha fazla bilgi için, Windows Birim Gölge Kopyalama Hizmeti ile Tümleştirme bölümüne gidin.) StorSimple Snapshot Manager ile yedekleme zamanlamaları oluşturabilir veya gerektiğinde anında yedekleme alabilirsiniz. Bir yedeklemeden veri geri yüklemeniz gerekiyorsa, StorSimple Snapshot Manager yerel veya bulut anlık görüntüler kataloğundan seçim yapmanıza olanak tanır. Azure StorSimple yalnızca gerektiğinde gereken verileri geri yükler ve bu da geri yükleme işlemleri sırasında veri kullanılabilirliğindeki gecikmeleri önler.)

![StorSimple Snapshot Manager mimarisi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager mimarisi** 

## <a name="support-for-multiple-volume-types"></a>Birden çok birim türü için destek
Aşağıdaki birim türlerini yapılandırmak ve yedeklemek için StorSimple Snapshot Manager'ı kullanabilirsiniz: 

* Temel birimler – Temel **birim,** temel diskteki tek bir bölümdür. 
* **Basit birimler** – Basit bir birim, tek bir dinamik diskten disk alanı içeren dinamik bir birimdir. Basit bir birim, bir diskteki tek bir bölgeden veya aynı diskte birbirine bağlı birden çok bölgeden oluşur. (Yalnızca dinamik disklerde basit birimler oluşturabilirsiniz.) Basit hacimler hataya toleranslı değildir.
* Dinamik birimler – Dinamik **birim,** dinamik bir diskte oluşturulan bir birimdir. Dinamik diskler, bilgisayardaki dinamik disklerde bulunan birimler hakkındaki bilgileri izlemek için bir veritabanı kullanır. 
* **Yansıtma lı dinamik hacimler** – Yansıtmalı dinamik hacimler RAID 1 mimarisi üzerine inşa edilmiştir. RAID 1 ile aynı veriler iki veya daha fazla diske yazılır ve aynalı bir küme üretir. Okuma isteği daha sonra istenen verileri içeren herhangi bir disk tarafından işlenebilir.
* **Küme paylaşılan birimler** – Küme paylaşılan birimleri (CSV) ile, bir failover kümesindeki birden çok düğüm aynı anda aynı diske okunabilir veya yazılabilir. Bir düğümden diğerine, sürücü sahipliğinde veya montajda, sökmede ve bir birim kaldırmada değişiklik gerektirmeden hızlı bir şekilde oluşabilir. 

> [!IMPORTANT]
> Aynı anlık görüntüde CSV'leri ve C'siz leri karıştırmayın. Anlık görüntüde CSV'ler ve NON-CSV'lerin karıştırılması desteklenmez. 
> 
> 

Tüm birim gruplarını geri yüklemek veya tek tek birimleri klonlamak ve tek tek dosyaları kurtarmak için StorSimple Snapshot Manager'ı kullanabilirsiniz.

* [Hacimler ve hacim grupları](#volumes-and-volume-groups) 
* [Yedekleme türleri ve yedekleme ilkeleri](#backup-types-and-backup-policies) 

StorSimple Snapshot Manager özellikleri ve bunların nasıl kullanılacağı hakkında daha fazla bilgi için [StorSimple Snapshot Manager kullanıcı arabirimine](storsimple-use-snapshot-manager.md)bakın.

## <a name="volumes-and-volume-groups"></a>Hacimler ve hacim grupları
StorSimple Snapshot Manager ile, hacimler oluşturur ve bunları birim gruplarına göre yapılandırırsınız. 

StorSimple Snapshot Manager, uygulama tutarlı yedekleme kopyaları oluşturmak için birim gruplarını kullanır. İlgili tüm dosya ve veritabanları eşitlendiğinde ve belirli bir zamanda belirli bir noktada bir uygulamanın gerçek durumunu temsil ettiğinde uygulama tutarlılığı vardır. Birim grupları *(tutarlılık grupları*olarak da bilinir) yedekleme veya geri yükleme işinin temelini oluşturur.

Birim grupları birim kapsayıcıları ile aynı değildir. Birim kapsayıcı, bulut depolama hesabını ve şifreleme ve bant genişliği tüketimi gibi diğer öznitelikleri paylaşan bir veya daha fazla birim içerir. Tek bir birim kapsayıcı en fazla 256 ince şekilde sağlanan StorSimple birimleri içerebilir. Birim kapsayıcılar hakkında daha fazla bilgi [için, birim kapsayıcılarınızı yönet'e](storsimple-manage-volume-containers.md)gidin. Birim grupları, yedekleme işlemlerini kolaylaştırmak için yapılandırdığınız birim koleksiyonlarıdır. Farklı birim kapsayıcılarına ait iki birim seçerseniz, bunları tek bir birim grubuna yerleştirin ve sonra bu birim grubu için bir yedekleme ilkesi oluşturursanız, her birim uygun depolama hesabı kullanılarak uygun birim kapsayıcısında yedeklenir.

> [!NOTE]
> Bir birim grubundaki tüm birimler tek bir bulut hizmeti sağlayıcısından gelmelidir.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Windows Birim Gölge Kopyalama Hizmeti ile Tümleştirme
StorSimple Snapshot Manager, uygulama tutarlı verileri yakalamak için Windows Birim Gölge Kopyalama Hizmeti'ni (VSS) kullanır. VSS, artan anlık görüntü oluşturmayı koordine etmek için VSS'ye duyarlı uygulamalarla iletişim kurarak uygulama tutarlılığını kolaylaştırır. VSS, anlık görüntüler alındığında uygulamaların geçici olarak etkin olmamasını veya hareketli olmasını sağlar. 

VSS'nin StorSimple Snapshot Manager uygulaması SQL Server ve genel NTFS birimleriyle çalışır. İşlem aşağıdaki gibidir: 

1. Genellikle bir veri yönetimi ve koruma çözümü (StorSimple Snapshot Manager gibi) veya bir yedekleme uygulaması olan bir istekçi, VSS'yi çağırır ve hedef uygulamadaki yazar yazılımdan bilgi toplamasını ister.
2. VSS, verilerin açıklamasını almak için yazar bileşeniyle bağlantı kurur. Yazar yedeklenecek verilerin açıklamasını döndürür. 
3. VSS, yazarı yedekleme için uygulamayı hazırlaması için işaretler. Yazar, açık hareketleri tamamlayarak, hareket günlüklerini güncelleyerek ve vss'yi kaydederek verileri yedeklemeiçin hazırlar.
4. VSS, yazara uygulamanın veri depolarını geçici olarak durdurmasını ve gölge kopya oluşturulurken ses birimine veri yazOlmadığından emin olmasını bildirir. Bu adım veri tutarlılığı sağlar ve en fazla 60 saniye sürer.
5. VSS sağlayıcıya gölge kopyasını oluşturmasını bildirir. Yazılım veya donanım tabanlı olabilecek sağlayıcılar, şu anda çalışan birimleri yönetir ve isteğe bağlı olarak bunların gölge kopyalarını oluşturur. Sağlayıcı gölge kopyasını oluşturur ve tamamlandığında VSS'yi habereder.
6. VSS, uygulamanın devam edebileceğine dair uygulama bildirmek ve ayrıca G/Ç'nin gölge kopya oluşturma sırasında başarıyla duraklatılmış olduğunu doğrulamak için yazarla iletişime geçer. 
7. Kopya başarılı olduysa, VSS kopyanın konumunu istekte bulundurana döndürür. 
8. Gölge kopyası oluşturulurken veri yazıldıysa, yedekleme tutarsız olacaktır. VSS gölge kopyasını siler ve isteği olana haber ver. İstekçi yedekleme işlemini otomatik olarak yineleyebilir veya yöneticiye daha sonra yeniden denemesini bildirebilir.

Aşağıdaki resme bakın.

![VSS süreci](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows Birim Gölge Kopyalama Hizmeti işlemi** 

## <a name="backup-types-and-backup-policies"></a>Yedekleme türleri ve yedekleme ilkeleri
StorSimple Snapshot Manager ile verileri yedekleyebilir ve yerel olarak ve bulutta saklayabilirsiniz. Verileri hemen yedeklemek için StorSimple Snapshot Manager'ı veya yedekleme leri otomatik olarak almak için bir zamanlama oluşturmak için yedekleme ilkesi kullanabilirsiniz. Yedekleme ilkeleri, kaç anlık görüntünlerin tutulacağını belirtmenize de olanak tanır. 

### <a name="backup-types"></a>Yedekleme türleri
Aşağıdaki yedekleme türlerini oluşturmak için StorSimple Snapshot Manager'ı kullanabilirsiniz:

* **Yerel anlık görüntüler** – Yerel anlık görüntüler, StorSimple aygıtında depolanan birim verilerin in-time kopyalarıdır. Genellikle, bu tür yedekleme oluşturulabilir ve hızlı bir şekilde geri yüklenebilir. Yerel bir yedekleme kopyası gibi yerel bir anlık görüntü kullanabilirsiniz.
* **Bulut anlık görüntüleri** – Bulut anlık görüntüleri, bulutta depolanan birim verilerin in-time kopyalarıdır. Bulut anlık görüntüsü, farklı, site dışı depolama sisteminde çoğaltılan anlık görüntüye eşdeğerdir. Bulut anlık görüntüleri özellikle olağanüstü durum kurtarma senaryolarında yararlıdır.

### <a name="on-demand-and-scheduled-backups"></a>İsteğe bağlı ve zamanlanmış yedeklemeler
StorSimple Snapshot Manager ile, hemen oluşturulacak bir kerelik yedekleme başlatabilir veya yinelenen yedekleme işlemlerini zamanlamak için bir yedekleme ilkesi kullanabilirsiniz.

Yedekleme ilkesi, düzenli yedeklemeleri zamanlamak için kullanabileceğiniz bir dizi otomatik kuraldır. Yedekleme ilkesi, belirli bir birim grubunun anlık görüntülerini almak için sıklığını ve parametreleri tanımlamanıza olanak tanır. Hem yerel hem de bulut anlık görüntüleri için başlangıç ve son kullanma tarihlerini, saatlerini, sıklıkları ve bekletme gereksinimlerini belirtmek için ilkeleri kullanabilirsiniz. Bir ilke, siz tanımladıktan hemen sonra uygulanır. 

Gerektiğinde yedekleme ilkelerini yapılandırmak veya yeniden yapılandırmak için StorSimple Snapshot Manager'ı kullanabilirsiniz. 

Oluşturduğunuz her yedekleme ilkesi için aşağıdaki bilgileri yapılandırırsınız:

* **Ad** – Seçili yedekleme ilkesinin benzersiz adı.
* **Tür** – Yedekleme ilkesinin türü; yerel anlık görüntü veya bulut anlık görüntüsü.
* **Birim grubu** – Seçili yedekleme ilkesinin atandığı birim grubu.
* **Bekletme** – Saklanacak yedek kopya sayısı. **Tüm** kutusunu işaretleseniz, tüm yedekleme kopyaları birim başına maksimum yedekleme kopya sayısına ulaşılAna kadar tutulur ve bu noktada ilke başarısız olur ve bir hata iletisi oluşturur. Alternatif olarak, (1 ile 64 arasında) saklanacak bir dizi yedekleme belirtebilirsiniz.
* **Tarih** – Yedekleme ilkesinin oluşturulduğu tarih.

Yedekleme ilkelerini yapılandırma hakkında bilgi için, [yedekleme ilkeleri oluşturmak ve yönetmek için StorSimple Snapshot Manager'ı kullanın'a](storsimple-snapshot-manager-manage-backup-policies.md)gidin.

### <a name="backup-job-monitoring-and-management"></a>Yedekleme iş izleme ve yönetimi
Yaklaşan, zamanlanmış ve tamamlanan yedekleme işlerini izlemek ve yönetmek için StorSimple Snapshot Manager'ı kullanabilirsiniz. Ayrıca, StorSimple Snapshot Manager 64'e kadar tamamlanmış yedeklemenin bir kataloğunu sağlar. Birimleri veya dosyaları bulmak ve geri yüklemek için kataloğu kullanabilirsiniz. 

Yedekleme işlerini izleme hakkında daha fazla bilgi için, [yedekleme işlerini görüntülemek ve yönetmek için StorSimple Snapshot Manager'ı kullanın'a](storsimple-snapshot-manager-manage-backup-jobs.md)gidin.

## <a name="next-steps"></a>Sonraki adımlar
* [StorSimple çözümünüzü yönetmek için StorSimple Snapshot Manager'ı kullanma](storsimple-snapshot-manager-admin.md)hakkında daha fazla bilgi edinin.
* [StorSimple Snapshot Manager'ı](https://www.microsoft.com/download/details.aspx?id=44220)indirin.

