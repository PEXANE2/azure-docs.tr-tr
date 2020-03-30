---
title: Azure Sanal Makinelerde SAP HANA için yedekleme kılavuzu | Microsoft Dokümanlar
description: SAP HANA için yedekleme kılavuzu, Azure sanal makinelerinde SAP HANA için iki önemli yedekleme olasılığı sağlar
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: juergent
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ums.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/01/2020
ms.author: juergent
ms.openlocfilehash: bb32350597059209e5baf01d53b0c59fdc2344f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255249"
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Azure Sanal Makinelerde SAP HANA için yedekleme kılavuzu

## <a name="getting-started"></a>Başlarken

Azure sanal Makineler'de çalışan SAP HANA yedekleme kılavuzu yalnızca Azure'a özgü konuları açıklar. Genel SAP HANA yedekleme ile ilgili öğeler için SAP HANA belgelerini kontrol edin. İlke veritabanı yedekleme stratejileri, sağlam ve geçerli bir yedekleme stratejisine sahip olma nedenleri ve motivasyonları hakkında bilgi sahibi olmanızı ve şirketinizin yedekleme prosedürü, yedeklemelerin bekletme süresi ve geri yükleme için sahip olduğu gereksinimlerin farkında olmanızı bekliyoruz Yordam.

SAP HANA, Azure M Serisi gibi çeşitli Azure VM türlerinde resmi olarak desteklenir. SAP HANA sertifikalı Azure VM'lerin ve HANA Büyük Örnek birimlerinin tam listesi için [Sertifikalı IaaS Platformlarını Bul'a](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)göz atın. Microsoft Azure, SAP HANA'nın fiziksel sunucularda sanallaştırılmayan çalıştığı bir dizi birim sunar. Bu hizmet [HANA Büyük Örnekleri](hana-overview-architecture.md)denir. Bu kılavuz, HANA Büyük Örnekleri için yedekleme işlemlerini ve araçlarını kapsamaz. Ancak Azure sanal makineleri ile sınırlı olacak. HANA Büyük Örnekleri ile yedekleme/geri yükleme işlemleri hakkında daha fazla bilgi için [HLI Yedekleme ve Geri Yükleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore)makalesini okuyun.

Bu makalenin odak noktası, SAP HANA'nın Azure sanal makinelerinde üç yedekleme olasılığı üzerindedir:

- Azure Yedekleme Hizmetleri aracılığıyla HANA [yedeklemesi](https://docs.microsoft.com/azure/backup/backup-overview) 
- Bir Azure Linux Sanal Makinesi'nde dosya sistemine HANA yedeklemesi [(bkz. dosya düzeyinde SAP HANA Azure Yedeklemesi](sap-hana-backup-file-level.md))
- Azure depolama blob anlık görüntü özelliğini el ile veya Azure Yedekleme hizmetini kullanarak depolama anlık görüntülerine dayalı HANA yedeklemesi


SAP HANA, üçüncü taraf yedekleme araçlarının SAP HANA ile doğrudan tümleştirmesine olanak tanıyan bir yedekleme API'si sunar. Azure Yedekleme hizmeti veya [Commvault](https://azure.microsoft.com/resources/protecting-sap-hana-in-azure/) gibi ürünler, SAP HANA veritabanını tetiklemek veya günlük yedeklemelerini yeniden yapmak için bu özel arabirimi kullanıyor. 


Azure'da hangi SAP yazılımının desteklendiredildiğini nasıl bulabileceğinize ilişkin bilgiler makalede, [Azure dağıtımları için hangi SAP yazılımının desteklenir.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-supported-product-on-azure)

## <a name="azure-backup-service"></a>Azure Yedekleme Hizmeti

Gösterilen ilk senaryo, Azure Yedekleme Hizmeti'nin SAP HANA veritabanından bir akış yedeklemesi gerçekleştirmek için SAP HANA `backint` arabirimini kullandığı bir senaryodur. Veya uygulama tutarlı bir disk anlık görüntüsü oluşturmak ve bunu Azure Yedekleme hizmetine aktarmak için Azure Yedekleme hizmetinin daha genel bir özelliğini kullanırsınız.

Azure Yedekleme entegre ve [backint](https://www.sap.com/dmc/exp/2013_09_adpd/enEN/#/d/solutions?id=8f3fd455-a2d7-4086-aa28-51d8870acaa5)olarak adlandırılan tescilli SAP HANA arabirimi kullanarak SAP HANA için yedekleme çözümü olarak onaylanmıştır. Çözümün daha fazla ayrıntısı, özellikleri ve kullanılabildiği Azure bölgeleri için, [Azure VM'lerinde SAP HANA veritabanlarının yedeklemesi için](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support)destek matrisini okuyun. HANA için Azure Yedekleme hizmeti yle ilgili ayrıntılar ve ilkeler için Azure [VM'lerde SAP HANA veritabanı yedeklemesi hakkındaki](https://docs.microsoft.com/azure/backup/sap-hana-db-about)makaleyi okuyun. 

Azure Yedekleme hizmetinden yararlanmanın ikinci olasılığı, Azure Premium Depolama'nın disk anlık görüntülerini kullanarak uygulama tutarlı bir yedekleme oluşturmaktır. [Azure Ultra disk](https://docs.microsoft.com/azure/virtual-machines/linux/disks-enable-ultra-ssd) ve Azure [NetApp Dosyaları](https://azure.microsoft.com/services/netapp/) gibi diğer HANA sertifikalı Azure depolama ları, Azure Yedekleme hizmeti aracılığıyla bu tür anlık görüntüleri desteklemiyor. Bu makaleleri okuma:

- [Azure’da sanal makine yedekleme altyapınızı planlama](https://docs.microsoft.com/azure/backup/backup-azure-vms-introduction)
- [Azure Linux VM'lerinin uygulama tutarlı yedeklemesi](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent) 

bu etkinlik dizisi ortaya çıkar:

- Azure Yedekleme tutarlı bir durumda, bu durumda SAP HANA, uygulama koyar bir ön anlık görüntü komut yürütmesi gerekir
- Bu tutarlı durum doğrulandıkça, Azure Yedekleme disk anlık görüntülerini yürütecek
- Anlık görüntüleri tamamladıktan sonra, Azure Yedekleme ön anlık görüntü komut dosyasında yaptığı etkinliği geri alacaktır
- Başarılı bir şekilde yürütülmesinden sonra Azure Yedekleme verileri Yedekleme kasasına aktaracak

SAP HANA durumunda, çoğu müşteri SAP HANA redo günlüklerini içeren birimler için Azure Yazma Hızlandırıcısı kullanır. Azure Yedekleme hizmeti bu birimleri anlık görüntülerden otomatik olarak hariç tutar. Bu dışlama, HANA'nın geri yükleme yeteneğine zarar vermez. Neredeyse tüm diğer SAP desteklenen DBMS ile geri yükleme yeteneğini engelleyecek olsa da.

Bu olasılığın dezavantajı, kendi anlık görüntü öncesi ve sonrası komut dosyası geliştirmeniz gerektiğidir. Anlık görüntü öncesi komut dosyasının bir HANA anlık görüntüsü oluşturması ve nihai özel durum durumlarını işlemesi gerekir. Oysa anlık görüntü sonrası komut dosyasının HANA anlık görüntüsünü yeniden silmesi gerekir. Gerekli mantık hakkında daha fazla bilgi için, [SAP destek notu ile](https://launchpad.support.sap.com/#/notes/2039883)#2039883 ile başlayın. Bu makalede yer alan 'SAP HANA veri tutarlılığı' bölümünün göz önünde bulundurulması, bu tür yedeklemeler için tam olarak geçerlidir.

> [!NOTE]
> Birden çok veritabanı kapsayıcısının kullanıldığı dağıtımlarda SAP HANA için disk anlık görüntü tabanlı yedeklemeler, en az HANA 2.0 SP04 sürümü gerektirir
> 

Daha sonra bu belgede depolama anlık görüntüleri yle ilgili ayrıntılara bakın.

![Bu rakam, geçerli VM durumunu kaydetmek için iki olasılık gösterir](media/sap-hana-backup-guide/azure-backup-service-for-hana.png)

## <a name="other-hana-backup-methods"></a>Diğer HANA yedekleme yöntemleri
Düşünülebilecek diğer üç yedekleme yöntemi veya yolu vardır:

- Azure NetApp Dosyalarını (ANF) temel alan bir NFS paylaşımına karşı yedekleme. ANF yine yedekleme leri depoladığınız bu birimlerin anlık görüntülerini oluşturma yeteneğine sahiptir. Sonunda yedekleri yazmak için gereken iş hacmi göz önüne alındığında, bu çözüm pahalı bir yöntem haline gelebilir. HANA yedeklemeleri doğrudan Azure yerel NFS paylaşımına yazabileceğinden, kurulumu kolay olsa da
- HANA Yedekleme'yi Standart SSD veya Azure Premium Depolama'nın VM ekli disklerine karşı yürütmek. Bir sonraki adımolarak, bu yedekleme dosyalarını Azure Blob depolama alanına kopyalayabilirsiniz. Bu strateji fiyat akıllıca çekici olabilir
- HANA Yedekleme'yi Standart SSD veya Azure Premium Depolama'nın VM ekli disklerine karşı yürütmek. Bir sonraki adımda disk düzenli olarak anlık görüntüalır. İlk anlık görüntüden sonra, artımlı anlık görüntüler maliyetleri azaltmak için kullanılabilir

![Bu şekil, VM içinde bir SAP HANA dosya yedeklemealmak için seçenekleri gösterir](media/sap-hana-backup-guide/other-hana-backup-paths.png)

Bu rakam, VM içinde bir SAP HANA dosya yedekleme si alma ve daha sonra farklı araçlar kullanarak başka bir yerde HANA yedekleme dosyaları depolama kseçenekleri gösterir. Ancak, bir üçüncü taraf yedekleme hizmeti veya Azure Yedekleme hizmeti içermeyen tüm çözümlerin ortak birkaç engeli vardır. Bunlardan bazıları, bekletme yönetimi, otomatik geri yükleme işlemi ve Azure Yedekleme hizmeti veya diğer özel üçüncü taraf yedekleme paketleri ve hizmetleri sağladığı gibi otomatik zaman diliminde kurtarma sağlama gibi listelenebilir. Bu üçüncü taraf hizmetlerin çoğu Azure'da çalıştırılabiliyor. 


## <a name="sap-resources-for-hana-backup"></a>HANA yedeklemesi için SAP kaynakları

### <a name="sap-hana-backup-documentation"></a>SAP HANA yedekleme belgeleri

- [SAP HANA İdaresine Giriş](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Yedekleme ve Kurtarma Stratejinizi Planlama](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [ABAP DBACOCKPIT kullanarak HANA Yedekleme programı](https://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Veri Yedeklemelerini Zamanlama (SAP HANA Kokpit)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- [SAP Note 1642148'de](https://launchpad.support.sap.com/#/notes/1642148) SAP HANA yedeklemesi hakkında SSS
- [SAP Note 2039883'te](https://launchpad.support.sap.com/#/notes/2039883) SAP HANA veritabanı ve depolama anlık görüntüleri hakkında SSS
- [SAP Note 1820529'da](https://launchpad.support.sap.com/#/notes/1820529) yedekleme ve kurtarma için uygun olmayan ağ dosya sistemleri

### <a name="how-to-verify-correctness-of-sap-hana-backup"></a>SAP HANA yedeklemesinin doğruluğu nasıl doğrulayabilirsiniz?
Yedekleme yönteminizden bağımsız olarak, farklı bir sisteme karşı bir test geri yüklemesi çalıştırmak mutlak bir gerekliliktir. Bu yaklaşım, yedeklemenin doğru olduğundan emin olmak için bir yol sağlar ve destek ve beklendiği gibi çalışmayı geri yükleme için dahili işlemler. Yedeklemeleri geri yedekleme, altyapı gereksinimi nedeniyle şirket içinde bir engel teşkil etse de, bu amaç için geçici olarak gerekli kaynakları sağlayarak bulutta gerçekleştirmek çok daha kolaydır. Geri yükleme yeteneği yedek dosyaları kontrol edebilirsiniz HANA ile sağlanan araçlar olduğu doğrudur. Ancak, sık sık geri yükleme egzersizlerinin amacı, bir veritabanı geri yükleme işlemini test etmek ve bu işlemi operasyon personeliyle eğitmektir.

Basit bir geri yükleme yapmanın ve HANA'nın çalışır durumda olup olmadığını kontrol etmek için yeterli olmadığını unutmayın. Geri yüklenen veritabanının iyi olduğundan emin olmak için bir tablo tutarlılığı denetimi çalıştırmalısınız. SAP [HANA, SAP Note 1977584'te](https://launchpad.support.sap.com/#/notes/1977584)açıklanan çeşitli tutarlılık denetimleri sunar.

Tablo tutarlılık denetimi ile ilgili bilgiler, [Tablo ve Katalog Tutarlılık Denetimleri'ndeki](https://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b)SAP web sitesinde de bulunabilir.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>HANA yedekleme sinin artıları ve eksileri ile depolama anlık görüntüsü

SAP&#39;hana yedekleme karşı depolama anlık tercih vermez. Bu onların artılarını ve eksilerini listeler, böylece bir duruma ve kullanılabilir depolama teknolojisine bağlı olarak kullanmak için belirleyebilirsiniz (Yedekleme [ve Kurtarma Stratejisi Planlama](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)bakınız).

Azure'da, Azure blob anlık görüntü özelliğinin&#39;birden çok diskte dosya sistemi tutarlılığı sağlamadığını unutmayın (bkz. [PowerShell ile blob anlık görüntülerini kullanma).](https://blogs.msdn.microsoft.com/cie/2016/05/17/using-blob-snapshots-with-powershell/) 

Buna ek olarak, bu makalede açıklandığı gibi blob anlık görüntüleri ile sık sık çalışırken faturalama sonuçlarını anlamak gerekir: [Anlık Görüntülerin Ücretleri Nasıl Tahakkuk](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)Ettiğini Anlamak —Azure sanal disklerini kullanmak kadar açık&#39;değildir.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>Depolama anlık görüntüleri çekerken SAP HANA veri tutarlılığı

Daha önce de belirtildiği gibi, depolama anlık görüntüleri çekerken Azure Yedekleme, dosya sistemi ve uygulama tutarlılığı anlık yedekleme özelliklerini açıklamak zorunludur. Sorunları önlemek için en kolay yolu SAP HANA, hatta belki de tüm sanal makine kapatmak olacaktır. Bir üretim örneği için mümkün olmayan bir şey.

> [!NOTE]
> Birden çok veritabanı kapsayıcısının kullanıldığı dağıtımlarda SAP HANA için disk anlık görüntü tabanlı yedeklemeler, en az HANA 2.0 SP04 sürümü gerektirir
> 

Azure depolama, anlık görüntü işlemi sırasında vm'ye bağlı birden çok disk veya birim arasında dosya sistemi tutarlılığı sağlamaz. Bu, anlık görüntü sırasında uygulama tutarlılığı uygulama tarafından teslim edilmesi gerektiği anlamına gelir, bu durumda SAP HANA kendisi. [SAP Note 2039883](https://launchpad.support.sap.com/#/notes/2039883) depolama anlık görüntüleri tarafından SAP HANA yedeklemehakkında önemli bilgilere sahiptir. Örneğin, XFS dosya sistemlerinde, uygulama tutarlılığı sağlamak için depolama anlık görüntüsünü başlatmadan önce **xfs\_dondurma** çalışması gerekir (xfs [\_freeze(8) - XFs](https://linux.die.net/man/8/xfs_freeze) **\_freeze**ile ilgili ayrıntılar için Linux man sayfasına bakınız).

Dört Azure sanal diskini kapsayan bir XFS dosya sistemi olduğunu varsayarsak, aşağıdaki adımlar HANA veri alanını temsil eden tutarlı bir anlık görüntü sağlar:

1. HANA veri anlık oluşturma hazırlamak
1. Tüm disklerin/birimlerin dosya sistemlerini dondurma (örneğin, **xfs\_donma**kullanın)
1. Azure'da gerekli tüm blob anlık görüntülerini oluşturun
1. Dosya sistemini çözme
1. HANA veri anlık görüntüsünü onaylayın (anlık görüntü silinir)

Azure Yedekleme'nin uygulama tutarlı anlık görüntü yedeklemeleri gerçekleştirme yeteneğini kullanırken, #1 adımların anlık görüntü öncesi komut dosyası için sizin kodlanması/komut dosyası olması gerekir. Azure Yedekleme hizmeti, #2 ve #3 adımları yürütecektir. #4 ve #5 adımların anlık görüntü sonrası komut dosyasında kodunuz tarafından yeniden sağlanmalıdır. Azure yedekleme hizmetini kullanmıyorsanız, #2 ve #3 kendi #3 kod/komut dosyası da yapmanız gerekir.
HANA veri anlık oluşturma hakkında daha fazla bilgi bu makalelerde bulunabilir:

- [HANA veri anlık görüntüleri] (https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/en-US/ac114d4b34d542b99bc390b34f8ef375.html
- Adım #1 gerçekleştirmek için daha fazla ayrıntı makalede Bir [Veri Anlık Görüntü Oluşturma (Native SQL)](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/en-US/9fd1c8bb3b60455caa93b7491ae6d830.html) bulunabilir 
- Adım #5 HANA veri anlık görüntülerini onaylamak/silmek için ayrıntılar veri [anlık görüntüsü oluştur (Yerel SQL)](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/en-US/9fd1c8bb3b60455caa93b7491ae6d830.html) makalesinde bulunabilir 

HANA anlık görüntüsünü onaylamak önemlidir. &quot;Copy-on-Write nedeniyle,&quot; SAP HANA bu anlık görüntü hazırlama modunda yken ek disk alanı gerektirmeyebilir. SAP HANA anlık görüntüsü onaylanana kadar yeni yedeklemeler başlatmak da mümkün&#39;.


### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA yedekleme zamanlama stratejisi

SAP HANA makalesi [Yedekleme ve Kurtarma Stratejinizi Planlama,](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) yedekleme yapmak için temel bir planı belirtir. SAP HANA'nın yedekleme/geri yükleme stratejisini ve sürecini tanımlamada HANA çevresindeki SAP belgelerine ve diğer DBMS ile olan deneyimlerinize güvenin. Farklı yedekleme türlerinin sırası ve bekletme süresi, sağlamanız gereken SLA'lara büyük ölçüde bağlıdır.


### <a name="sap-hana-backup-encryption"></a>SAP HANA yedekleme şifrelemesi

SAP HANA veri ve günlük şifreleme söner. SAP HANA verileri ve günlüğü şifrelenmiyorsa, yedeklemeler varsayılan olarak şifrelenmez. Ancak SAP HANA, [SAP HANA Yedekleme Şifrelemesinde](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/5f837a57ce5e468d9db21c8683bc84da.html)belgelenen ayrı bir yedekleme şifrelemesi sunar. SAP HANA'nın eski sürümlerini çalıştırıyorsanız, yedekleme şifrelemesinin zaten sağlanan işlevselliğin bir parçası olup olmadığını denetlemeniz gerekebilir.  


## <a name="next-steps"></a>Sonraki adımlar
* [DOSYA düzeyindeSAP HANA Azure Yedekleme,](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneğini açıklar.
* Yüksek kullanılabilirlik oluşturmayı ve AZURE'da SAP HANA'nın olağanüstü durum kurtarmasını nasıl planlayabilirsiniz (büyük örnekler), Bkz. [SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve Azure'da olağanüstü durum kurtarma.](hana-overview-high-availability-disaster-recovery.md)
