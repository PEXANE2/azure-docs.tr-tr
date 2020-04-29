---
title: StorSimple Snapshot Manager nedir? | Microsoft Belgeleri
description: StorSimple Snapshot Manager, mimarisini ve özelliklerini açıklar.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79267423"
---
# <a name="an-introduction-to-storsimple-snapshot-manager"></a>StorSimple Snapshot Manager giriş

## <a name="overview"></a>Genel Bakış
StorSimple Snapshot Manager, bir Microsoft Azure StorSimple ortamında veri koruma ve yedekleme yönetimini kolaylaştıran bir Microsoft Yönetim Konsolu (MMC) ek bileşenidir. StorSimple Snapshot Manager, veri merkezindeki ve buluttaki Microsoft Azure StorSimple verileri tek bir tümleşik depolama çözümü olarak yönetebilir, böylece Yedekleme süreçlerini basitleştirir ve maliyetleri azaltabilirsiniz.

Bu genel bakış StorSimple Snapshot Manager tanıtır, özelliklerini açıklar ve Microsoft Azure StorSimple rolünü açıklar. 

StorSimple cihazı, StorSimple Yöneticisi hizmeti, StorSimple Snapshot Manager ve SharePoint için StorSimple Bağdaştırıcısı dahil tüm Microsoft Azure StorSimple sistemine genel bir bakış için bkz. [storsimple 8000 serisi: hibrit bulut depolama çözümü](storsimple-overview.md). 

> [!NOTE]
> * Microsoft Azure StorSimple Sanal dizilerini (StorSimple şirket içi sanal aygıtlar olarak da bilinir) yönetmek için StorSimple Snapshot Manager kullanamazsınız.
> * StorSimple cihazınıza StorSimple güncelleştirme 2 ' yi yüklemeyi planlıyorsanız, StorSimple **güncelleştirme 2 ' yi yüklemeden önce**storsimple Snapshot Manager 'nin en son sürümünü indirdiğinizden emin olun ve yükleyin. StorSimple Snapshot Manager 'nin en son sürümü, geriye dönük olarak uyumludur ve Microsoft Azure StorSimple yayınlanan tüm sürümleriyle birlikte çalışmaktadır. StorSimple Snapshot Manager 'ın önceki sürümünü kullanıyorsanız, bu sürümü güncelleştirmeniz gerekir (yeni sürümü yüklemeden önce önceki sürümü kaldırmanız gerekmez).
> 
> 

## <a name="storsimple-snapshot-manager-purpose-and-architecture"></a>StorSimple Snapshot Manager amacı ve mimarisi
StorSimple Snapshot Manager, yerel ve bulut verilerinin tutarlı, noktadan noktaya yedek kopyalarını oluşturmak için kullanabileceğiniz bir merkezi yönetim konsolu sağlar. Örneğin, konsolunu kullanarak şunları yapabilirsiniz:

* Birimleri yapılandırma, yedekleme ve silme.
* Yedeklenen verilerin uygulamayla tutarlı olduğundan emin olmak için birim gruplarını yapılandırın.
* Yedekleme ilkelerini, verilerin önceden belirlenmiş bir zamanlamaya göre yedeklenmesi için yönetin.
* Bulutta depolanabilecek ve olağanüstü durum kurtarma için kullanılan yerel ve bulut anlık görüntüleri oluşturun.

StorSimple Snapshot Manager, konaktaki VSS sağlayıcısına kayıtlı uygulamaların listesini getirir. Ardından, uygulamayla tutarlı yedeklemeler oluşturmak için, bir uygulama tarafından kullanılan birimleri denetler ve yapılandırmak için birim grupları önerir. StorSimple Snapshot Manager, uygulamayla tutarlı yedekleme kopyaları oluşturmak için bu birim gruplarını kullanır. (Tüm ilgili dosyalar ve veritabanları eşitlendiğinde ve zaman içinde belirli bir noktada uygulamanın doğru durumunu temsil eden uygulama tutarlılığı vardır.) 

StorSimple Snapshot Manager yedeklemeleri, yalnızca son yedeklemeden bu yana yapılan değişiklikleri yakalayan Artımlı anlık görüntü biçimini alır. Sonuç olarak, yedeklemeler daha az depolama gerektirir ve hızlı bir şekilde oluşturulup geri yüklenebilir. StorSimple Snapshot Manager, anlık görüntülerin uygulamayla tutarlı verileri yakalamasını sağlamak için Windows Birim Gölge Kopyası Hizmeti (VSS) kullanır. (Daha fazla bilgi için, Windows Birim Gölge Kopyası Hizmeti ile tümleştirme bölümüne gidin.) StorSimple Snapshot Manager, yedekleme zamanlamaları oluşturabilir veya gerektiğinde hemen yedeklemeler alabilirsiniz. Verileri bir yedekten geri yüklemeniz gerekiyorsa, StorSimple Snapshot Manager yerel veya bulut anlık görüntülerinin kataloğundan seçim yapmanızı sağlar. Azure StorSimple, yalnızca gerekli olduğu gibi verileri geri yükler, bu da geri yükleme işlemleri sırasında veri kullanılabilirliğine karşı gecikmeleri önler.)

![StorSimple Snapshot Manager mimarisi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_Overview.png)

**StorSimple Snapshot Manager mimarisi** 

## <a name="support-for-multiple-volume-types"></a>Birden çok birim türü için destek
Aşağıdaki tür birimleri yapılandırmak ve yedeklemek için StorSimple Snapshot Manager kullanabilirsiniz: 

* Temel **birimler** – temel birim, temel bir diskteki tek bölümdür. 
* **Basit birimler** – basit bir birim, tek bir dinamik diskten disk alanı içeren dinamik bir birimdir. Basit bir birim, bir diskteki tek bir bölgeden veya aynı diskte birbirine bağlanmış birden çok bölgeye sahip oluşur. (Yalnızca dinamik disklerde Basit birimler oluşturabilirsiniz.) Basit birimler hataya dayanıklı değildir.
* Dinamik **birimler** : dinamik birim dinamik bir diskte oluşturulan birimdir. Dinamik diskler bir bilgisayardaki dinamik disklerde bulunan birimler hakkındaki bilgileri izlemek için bir veritabanı kullanır. 
* **Yansıtma** ile dinamik birimler – yansıtma içeren dinamik BIRIMLER, RAID 1 mimarisine göre oluşturulmuştur. RAID 1 ile, aynı veriler iki veya daha fazla diske yazılır ve yansıtmalı bir küme oluşturulur. Daha sonra, istenen verileri içeren herhangi bir disk tarafından bir okuma isteği işlenebilir.
* **Küme Paylaşılan birimleri** – küme paylaşılan birimleri (CSV 'ler) ile, bir yük devretme kümesindeki birden çok düğüm aynı diski aynı anda okuyabilir veya yazılabilir. Bir düğümden başka bir düğüme yük devretme işlemi, sürücü sahipliğinin bir değişikliğe gerek olmadan veya bir birimi bağlama, kaldırma ve kaldırma işlemi yapılmadan hızlı bir şekilde gerçekleşebilir. 

> [!IMPORTANT]
> CSV 'leri ve CSV olmayan 'leri aynı anlık görüntüde karıştırmayın. CSV 'leri ve bir anlık görüntüde CSV olmayan karıştırma desteklenmez. 
> 
> 

Tüm birim gruplarını geri yüklemek veya tek tek birimleri klonlamak ve tek dosyaları kurtarmak için StorSimple Snapshot Manager kullanabilirsiniz.

* [Birimler ve birim grupları](#volumes-and-volume-groups) 
* [Yedekleme türleri ve yedekleme ilkeleri](#backup-types-and-backup-policies) 

StorSimple Snapshot Manager özellikleri ve bunların nasıl kullanılacağı hakkında daha fazla bilgi için bkz. [storsimple Snapshot Manager Kullanıcı arabirimi](storsimple-use-snapshot-manager.md).

## <a name="volumes-and-volume-groups"></a>Birimler ve birim grupları
StorSimple Snapshot Manager, birimler oluşturup bunları birim grupları olarak yapılandırırsınız. 

StorSimple Snapshot Manager, uygulama tutarlılığı olan yedek kopyalar oluşturmak için birim gruplarını kullanır. Tüm ilgili dosyalar ve veritabanları eşitlendiğinde uygulama tutarlılığı vardır ve belirli bir noktadaki uygulamanın doğru durumunu temsil eder. Birim grupları ( *tutarlılık grupları*olarak da bilinir), bir yedekleme veya geri yükleme işinin temelini oluşturur.

Birim grupları birim kapsayıcılarıyla aynı değildir. Birim kapsayıcısı, bulut depolama hesabı ve şifreleme ve bant genişliği tüketimi gibi diğer öznitelikleri paylaşan bir veya daha fazla birim içerir. Tek bir birim kapsayıcısı, en fazla 256 ölçülü kaynak sağlanmış StorSimple birimi içerebilir. Birim kapsayıcıları hakkında daha fazla bilgi için [birim Kapsayıcılarınızı yönetme](storsimple-manage-volume-containers.md)sayfasına gidin. Birim grupları, yedekleme işlemlerini kolaylaştırmak için yapılandırdığınız birimlerin koleksiyonlarıdır. Farklı birim kapsayıcılarına ait iki birim seçerseniz, bunları tek bir birim grubuna yerleştirin ve ardından bu birim grubu için bir yedekleme ilkesi oluşturursanız, her birim uygun depolama hesabı kullanılarak uygun birim kapsayıcısında yedeklenir.

> [!NOTE]
> Bir birim grubundaki tüm birimlerin tek bir bulut hizmeti sağlayıcısından gelmesi gerekir.
> 
> 

## <a name="integration-with-windows-volume-shadow-copy-service"></a>Windows Birim Gölge Kopyası Hizmeti ile tümleştirme
StorSimple Snapshot Manager uygulamayla tutarlı verileri yakalamak için Windows Birim Gölge Kopyası Hizmeti (VSS) kullanır. VSS, artımlı anlık görüntülerin oluşturulmasını koordine etmek üzere VSS kullanan uygulamalarla iletişim kurarak uygulama tutarlılığını kolaylaştırır. VSS, anlık görüntüler çekilirken uygulamaların geçici olarak devre dışı veya quiescent olmasını sağlar. 

VSS 'nin StorSimple Snapshot Manager uygulanması SQL Server ve genel NTFS birimleri ile birlikte kullanılır. İşlem aşağıdaki gibidir: 

1. Genellikle bir veri yönetimi ve koruma çözümü (StorSimple Snapshot Manager) veya bir yedekleme uygulaması olan bir istek sahibi VSS 'yi çağırır ve Hedef uygulamadaki yazıcı yazılımından bilgi toplamasını ister.
2. VSS, verilerin bir açıklamasını almak için yazıcı bileşeniyle iletişim kurar. Yazıcı yedeklenecek verilerin açıklamasını döndürür. 
3. VSS, yazıcıya uygulamayı yedekleme için hazırlamasını bildirir. Yazıcı, açık işlemleri tamamlayarak, işlem günlüklerini güncelleştirerek ve sonra VSS 'yi bildiren verileri yedekleme için hazırlar.
4. VSS, yazıcıya uygulamanın veri depolarını geçici olarak durmasını ve gölge kopya oluşturulurken birime hiçbir veri yazılmamasını sağlar. Bu adım veri tutarlılığını sağlar ve 60 saniyeden fazla sürer.
5. VSS, sağlayıcıya gölge kopya oluşturmasını söyler. Yazılım veya donanım tabanlı olabilecek sağlayıcılar, çalışmakta olan birimleri yönetebilir ve isteğe bağlı olarak bunların gölge kopyalarını oluşturabilir. Sağlayıcı, gölge kopyayı oluşturur ve tamamlandığında VSS 'yi bilgilendirir.
6. VSS, g/ç 'nin sürdürüleceği uygulamayı bilgilendirmek ve ayrıca, gölge kopya oluşturma sırasında g/ç 'nin başarıyla duraklatıldığını doğrulamak için yazıcı ile iletişim kurar. 
7. Kopyalama başarılı olduysa VSS, kopyanın konumunu istek sahibine döndürür. 
8. Gölge kopya oluşturulurken veriler yazılmışsa, yedekleme tutarsız olur. VSS, gölge kopyayı siler ve istek sahibine bildirir. İstek sahibi, yedekleme işlemini otomatik olarak yineleyebilir ya da yöneticiyi daha sonra yeniden denemek üzere bilgilendirir.

Aşağıdaki çizime bakın.

![VSS işlemi](./media/storsimple-what-is-snapshot-manager/HCS_SSM_VSS_process.png)

**Windows Birim Gölge Kopyası Hizmeti işlemi** 

## <a name="backup-types-and-backup-policies"></a>Yedekleme türleri ve yedekleme ilkeleri
StorSimple Snapshot Manager, verileri yedekleyebilir ve yerel olarak ve buluta kaydedebilirsiniz. Verileri hemen yedeklemek için StorSimple Snapshot Manager kullanabilirsiniz veya yedeklemeleri otomatik olarak almak için bir zamanlama oluşturmak üzere bir yedekleme İlkesi kullanabilirsiniz. Yedekleme ilkeleri Ayrıca kaç tane anlık görüntü korunacağını belirtmenizi sağlar. 

### <a name="backup-types"></a>Yedekleme türleri
Aşağıdaki Yedekleme türlerini oluşturmak için StorSimple Snapshot Manager kullanabilirsiniz:

* **Yerel anlık görüntüler** – yerel anlık görüntüler, StorSimple cihazında depolanan birim verilerinin zaman içinde bir noktadaki kopyalardır. Genellikle, bu yedekleme türü hızlı bir şekilde oluşturulup geri yüklenebilir. Yerel bir yedek kopya gibi yerel bir anlık görüntü kullanabilirsiniz.
* **Bulut anlık görüntüleri** – bulut anlık görüntüleri, bulutta depolanan birim verilerinin zaman içindeki bir kopyalardır. Bulut anlık görüntüsü, farklı, site dışı bir depolama sisteminde çoğaltılan bir anlık görüntüye eşdeğerdir. Bulut anlık görüntüleri özellikle olağanüstü durum kurtarma senaryolarında yararlıdır.

### <a name="on-demand-and-scheduled-backups"></a>İsteğe bağlı ve zamanlanmış yedeklemeler
StorSimple Snapshot Manager, hemen oluşturulabilmesi için bir kerelik yedekleme başlatabilir veya yinelenen yedekleme işlemlerini zamanlamak için bir yedekleme İlkesi kullanabilirsiniz.

Yedekleme ilkesi, düzenli yedeklemeler zamanlamak için kullanabileceğiniz otomatik kurallar kümesidir. Bir yedekleme ilkesi, belirli bir birim grubunun anlık görüntülerini almak için sıklık ve parametreleri tanımlamanızı sağlar. Hem yerel hem de bulut anlık görüntüleri için başlangıç ve sona erme tarihlerini, zamanları, frekansları ve bekletme gereksinimlerini belirtmek için ilkeleri kullanabilirsiniz. Bir ilke, tanımladıktan hemen sonra uygulanır. 

Her gerektiğinde yedekleme ilkelerini yapılandırmak veya yeniden yapılandırmak için StorSimple Snapshot Manager kullanabilirsiniz. 

Oluşturduğunuz her yedekleme ilkesi için aşağıdaki bilgileri yapılandırın:

* **Name** : Seçili yedekleme ilkesinin benzersiz adı.
* **Tür** : yedekleme ilkesinin türü; Yerel anlık görüntü veya bulut anlık görüntüsü.
* **Birim grubu** : Seçilen yedekleme ilkesinin atandığı birim grubu.
* **Bekletme** : tutulacak yedek kopyaların sayısı. **Tümü** kutusunu belirlerseniz, birim başına en fazla yedekleme kopyası sayısına ulaşılana kadar tüm yedekleme kopyaları korunur, bu noktada ilke başarısız olur ve bir hata iletisi oluşturur. Alternatif olarak, tutulacak bir dizi yedekleme belirtebilirsiniz (1 ile 64 arasında).
* **Tarih** : yedekleme ilkesinin oluşturulduğu tarih.

Yedekleme ilkelerini yapılandırma hakkında daha fazla bilgi için, [yedekleme ilkeleri oluşturmak ve yönetmek üzere StorSimple Snapshot Manager kullanma](storsimple-snapshot-manager-manage-backup-policies.md)bölümüne gidin.

### <a name="backup-job-monitoring-and-management"></a>Yedekleme işi izleme ve yönetimi
Yakında, zamanlanmış ve tamamlanmış yedekleme işlerini izlemek ve yönetmek için StorSimple Snapshot Manager kullanabilirsiniz. Ayrıca, StorSimple Snapshot Manager, 64 tamamlanmış yedeklemelerin kataloğunu sunmaktadır. Birimleri veya tek tek dosyaları bulmak ve geri yüklemek için kataloğunu kullanabilirsiniz. 

Yedekleme işlerini izleme hakkında daha fazla bilgi için bkz. [StorSimple Snapshot Manager kullanarak yedekleme işlerini görüntüleyin ve yönetin](storsimple-snapshot-manager-manage-backup-jobs.md).

## <a name="next-steps"></a>Sonraki adımlar
* StorSimple [çözümünüzü yönetmek Için storsimple Snapshot Manager kullanma](storsimple-snapshot-manager-admin.md)hakkında daha fazla bilgi edinin.
* [StorSimple Snapshot Manager](https://www.microsoft.com/download/details.aspx?id=44220)indirin.

