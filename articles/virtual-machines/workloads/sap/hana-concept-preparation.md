---
title: Azure'da SAP HANA'da olağanüstü durum kurtarma ilkeleri ve hazırlık (Büyük Örnekler) | Microsoft Dokümanlar
description: Azure'da SAP HANA'da olağanüstü durum kurtarma ilkeleri ve hazırlık (Büyük Örnekler)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 33d52f871de75a7f7d34016b040e44d6f1623fd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70101252"
---
# <a name="disaster-recovery-principles"></a>Olağanüstü Durum Kurtarma ilkeleri

HANA Büyük Örnekleri, farklı Azure bölgelerindeki HANA Büyük Örnek damgaları arasında olağanüstü durum kurtarma işlevi sunar. Örneğin, ABD'nin Batı Bölgesi Azure'da HANA Büyük Örnek birimlerini dağıtıyorsanız, ABD Doğu bölgesindeki HANA Büyük Örnek birimlerini olağanüstü durum kurtarma birimleri olarak kullanabilirsiniz. Daha önce de belirtildiği gibi, olağanüstü durum kurtarma otomatik olarak yapılandırılmaz, çünkü DR bölgesindeki başka bir HANA Büyük Örnek birimi için ödeme niz gerekmektedir. Olağanüstü durum kurtarma kurulumu ölçeklendirmenin yanı sıra ölçeklendirme kurulumları için de çalışır. 

Şimdiye kadar dağıtılan senaryolarda, müşteriler yüklü bir HANA örneğini kullanan üretim dışı sistemleri çalıştırmak için DR bölgesindeki birimi kullanır. HANA Büyük Örnek ünitesi, üretim amacıyla kullanılan SKU ile aynı SKU'ya ait olmalıdır. Aşağıdaki resim, Azure üretim bölgesindeki sunucu birimi ile olağanüstü durum kurtarma bölgesi arasındaki disk yapılandırmasının nasıl göründüğünü gösterir:

![Disk açısından DR kurulum yapılandırması](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Bu genel bakış grafiğinde gösterildiği gibi, daha sonra ikinci bir disk birimleri kümesi sipariş etmeniz gerekir. Hedef disk birimleri, olağanüstü durum kurtarma birimlerindeki üretim örneğinin üretim hacimleri ile aynı boyuttadır. Bu disk birimleri, olağanüstü durum kurtarma sitesindeki HANA Büyük Örnek sunucu birimiyle ilişkilidir. Aşağıdaki birimler üretim bölgesinden DR sitesine çoğaltılır:

- /hana/veri
- /hana/logbackups 
- /hana/shared (/usr/sap içerir)

SAP HANA işlem günlüğü, bu birimlerden geri yükleme nin yapıldığı şekilde gerekli olmadığından / hana/günlük hacmi çoğaltılamaz. 

Sunulan olağanüstü durum kurtarma işlevinin temeli, HANA Büyük Örnek altyapısı tarafından sunulan depolama çoğaltma işlevidir. Depolama tarafında kullanılan işlevsellik, depolama biriminde değişiklikler meydana geldikçe, eşzamanlı bir şekilde çoğalan sabit bir değişiklik akışı değildir. Bunun yerine, bu birimlerin anlık görüntüleri düzenli olarak oluşturulan gerçeğine dayanan bir mekanizmadır. Zaten çoğaltılmış anlık görüntü ile henüz çoğaltılmadı yeni bir anlık görüntü arasındaki delta, daha sonra olağanüstü durum kurtarma sitesine hedef disk birimlerine aktarılır.  Bu anlık görüntüler birimlerde depolanır ve olağanüstü durum kurtarma başarısız olması durumunda, bu birimlerde geri yüklenmeleri gerekir.  

Birimin tüm verilerinin ilk aktarımı, veri miktarı anlık görüntüler arasındaki deltalardan daha küçük hale gelmeden önce olmalıdır. Sonuç olarak, DR sitesindeki birimler üretim alanında gerçekleştirilen birim anlık görüntülerinin her birini içerir. Sonuç olarak, bu DR sistemini, üretim sistemini geri almadan kayıp verileri kurtarmak için daha önceki bir duruma ulaşmak için kullanabilirsiniz.

Bir HANA Büyük Örnek biriminde birden fazla bağımsız SAP HANA örneği içeren bir MCOD dağıtımı varsa, tüm SAP HANA örneklerinin depolamayı DR tarafına çoğaltması beklenmektedir.

HANA System Replication'ı üretim sitenizde yüksek kullanılabilirlik işlevselliği olarak kullandığınız ve DR sitesi için depolama tabanlı çoğaltma kullandığınız durumlarda, birincil siteden DR örneğine kadar her iki düğümün hacimleri çoğaltılır. Dr'ye hem birincil hem de ikincil çoğaltmayı karşılamak için DR sitesinde ek depolama alanı (birincil düğümle aynı boyutta) satın almalısınız. 



>[!NOTE]
>HANA Büyük Örnek depolama çoğaltma işlevi, depolama anlık görüntülerini yansıtıyor ve çoğaltıyor. Bu makalenin Yedekleme ve geri yükleme bölümünde tanıtılan depolama anlık görüntülerini gerçekleştirmezseniz, olağanüstü durum kurtarma sitesinde herhangi bir çoğaltma olamaz. Depolama anlık görüntü yürütme olağanüstü durum kurtarma sitesine depolama çoğaltma için bir ön koşuldur.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>Olağanüstü durum kurtarma senaryosunun hazırlanması
Bu senaryoda, üretim Azure bölgesinde HANA Büyük Örnekleri üzerinde çalışan bir üretim sisteminiz vardır. İzleyen adımlar için, hana sisteminin SID'sinin "PRD" olduğunu ve DR Azure bölgesindeki HANA Büyük Örnekleri'nde çalışan bir üretim dışı sisteme sahip olduğunuzu varsayalım. İkincisi için, onun SID olduğunu varsayalım "TST." Aşağıdaki resim bu yapılandırmayı gösterir:

![DR kurulumunun başlangıcı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Sunucu örneği ek depolama birimi kümesiyle önceden sipariş edilmemişse, Azure Hizmet Yönetimi'ndeki SAP HANA, tst'yi çalıştırdığınız HANA Büyük Örnek birimine üretim yinelemesi için hedef olarak ek birim kümesini bağlar HANA örneği. Bu amaçla, üretim HANA örneğinIN SID sağlamanız gerekir. Azure Hizmet Yönetimi'ndeki SAP HANA bu birimlerin ekini doğruladıktan sonra, bu birimleri HANA Büyük Örnek birimine takmanız gerekir.

![DR kurulumu sonraki adım](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

Bir sonraki adım, TST HANA örneğini çalıştırdığınız DR Azure bölgesindeki HANA Büyük Örnek birimine ikinci SAP HANA örneğini yüklemenizdir. Yeni yüklenen SAP HANA örneğinin aynı SID'ye sahip olması gerekir. Oluşturulan kullanıcıların üretim örneğinin sahip olduğu UID ve Grup Kimliği'ne sahip olması gerekir. Ayrıntılar için [Yedekleme'yi](hana-backup-restore.md) okuyun ve geri yükleyin. Yükleme başarılı olduysa, şunları yapmanız gerekir:

- [Yedekleme ve geri yükleme](hana-backup-restore.md)de açıklanan depolama anlık görüntü hazırlama adım 2 çalıştırın.
- Henüz yapmadıysanız HANA Büyük Örnek biriminin DR birimi için ortak bir anahtar oluşturun. [Yedekleme ve geri](hana-backup-restore.md)yüklemede açıklanan depolama anlık görüntü hazırlama adım 3'e bakın.
- *HANABackupCustomerDetails.txt'yi* yeni HANA örneğiyle koruyun ve depolamaya bağlantının doğru çalışıp çalışmadığını test edin.  
- DR Azure bölgesindeki HANA Büyük Örnek biriminde yeni yüklenen SAP HANA örneğini durdurun.
- Bu PRD birimlerini çıkarın ve Azure Hizmet Yönetimi'nde SAP HANA ile iletişime geçin. Birimler, depolama çoğaltma hedefi olarak işlev görürken erişilebilir olamadığı için üniteye monte edilemiyor.  

![Çoğaltma oluşturmadan önce DR kurulum adımı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

İşlemekibi, üretim Azure bölgesindeki PRD birimleri yle DR Azure bölgesindeki PRD birimleri arasındaki çoğaltma ilişkisini kurar.

>[!IMPORTANT]
>/hana/log birimi çoğaltılamaz, çünkü çoğaltılan SAP HANA veritabanını olağanüstü durum kurtarma sitesinde tutarlı bir duruma geri yüklemek gerekli değildir.

Ardından, olağanüstü durum durumunda RTO ve RPO'nuza ulaşmak için depolama anlık görüntü yedekleme zamanlamasını ayarlayın veya ayarlayın. Kurtarma noktası hedefini en aza indirmek için HANA Büyük Örnek hizmetinde aşağıdaki çoğaltma aralıklarını ayarlayın:
- Birleşik anlık görüntü (anlık görüntü türü **hana)** tarafından kapsanan birimler için, her 15 dakikada bir olağanüstü durum kurtarma sitesindeeşdeğer depolama hacmi hedeflerine çoğaltmak için ayarlayın.
- Hareket günlüğü yedekleme hacmi (anlık görüntü türü **günlükleri),** her 3 dakikada bir olağanüstü durum kurtarma sitesindeeşdeğer depolama hacmi hedeflerine çoğaltmak için ayarlayın.

Kurtarma noktası hedefini en aza indirmek için aşağıdakileri ayarlayın:
- **Hana** tipi depolama anlık görüntüsünü gerçekleştirin (bkz. "Adım 7: Anlık görüntüleri gerçekleştirin") her 30 dakikada bir ila 1 saat.
- SAP HANA hareket günlüğü yedeklemelerini her 5 dakikada bir gerçekleştirin.
- Günlükleri **logs** tür depolama anlık her 5-15 dakikada bir gerçekleştirin. Bu aralık süresi ile, yaklaşık 15-25 dakika bir RPO elde.

Bu kurulumla, işlem günlüğü yedeklemeleri, depolama anlık görüntüleri ve HANA işlem günlüğü yedekleme hacmi ve /hana/veri çoğaltma ve /hana/paylaşılan (/usr/sap içerir) bu grafikte gösterilen verilere benzeyebilir:

 ![Hareket günlüğü yedekleme anlık görüntüsü ile zaman eksenindeki anlık ayna arasındaki ilişki](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Olağanüstü durum kurtarma durumunda daha da iyi bir RPO elde etmek için, Azure'daki SAP HANA'daki HANA işlem günlüğü yedeklemelerini diğer Azure bölgesine kopyalayabilirsiniz. Bu daha fazla RPO azaltma elde etmek için aşağıdaki adımları gerçekleştirin:

1. HANA hareket günlüğünü mümkün olduğunca sık /hana/logbackups'a yedekle.
1. Hareket günlüğü yedeklemelerini NFS paylaşımbarındırılan Azure sanal makinelerine kopyalamak için rsync'i kullanın. SANAL M'ler Azure üretim bölgesinde ve DR bölgelerindeki Azure sanal ağlarında dır. Her iki Azure sanal ağını da üretim HANA Large Instances' ı Azure' a bağlayan devreye bağlamanız gerekir. [HANA Büyük Örnekler bölümüyle olağanüstü durum kurtarma için Ağ göz önünde bulundurulması](#Network-considerations-for-disaster recovery-with-HANA-Large-Instances) gereken grafiklere bakın. 
1. Bölgedeki hareket günlüğü yedeklemelerini NFS dışa aktarılan depolama alanına bağlı VM'de tutun.
1. Bir felaket başarısız durumda, felaket kurtarma sitesinde NFS payı üzerinde daha yakın zamanda alınan işlem günlüğü yedeklemeleri ile /hana/logbackups hacminde bulduğunuz işlem günlüğü yedeklemeleri tamam. 
1. DR bölgesine kaydedilebilen en son yedeklemeye geri yüklemek için bir hareket günlüğü yedeklemesi başlatın.

HANA Büyük Örnek işlemleri çoğaltma ilişkisi kurulumonaylamak ve yürütme depolama anlık görüntü yedekleri başlattığınızda, veri çoğaltma başlar.

![Çoğaltma oluşturmadan önce DR kurulum adımı](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Çoğaltma ilerledikçe, DR Azure bölgelerindeki PRD birimlerindeki anlık görüntüler geri yüklenmez. Sadece saklanır. Birimler böyle bir durumda monte edilmişse, PRD SAP HANA örneği DR Azure bölgesindeki sunucu birimine yüklendikten sonra bu birimleri kaldırdığınız durumu temsil eder. Ayrıca, henüz geri yüklenmemiş depolama yedeklemelerini de temsil eder.

Bir hata varsa, en son depolama anlık görüntüsü yerine eski bir depolama anlık görüntüsüne geri yüklemeyi de seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Olağanüstü durum kurtarma başarısız yordamı.](hana-failover-procedure.md)
