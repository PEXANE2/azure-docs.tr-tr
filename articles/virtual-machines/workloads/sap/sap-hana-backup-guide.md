---
title: Azure sanal makinelerinde SAP HANA için yedekleme Kılavuzu | Microsoft Docs
description: SAP HANA için yedekleme Kılavuzu, Azure sanal makinelerinde SAP HANA için iki önemli yedekleme olanağı sağlar
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
ms.openlocfilehash: e1cfe7216c1b37812c482cfacbd5d1c3f155418f
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86507837"
---
# <a name="backup-guide-for-sap-hana-on-azure-virtual-machines"></a>Azure sanal makinelerinde SAP HANA için yedekleme Kılavuzu

## <a name="getting-started"></a>Başlarken

Azure sanal makinelerinde çalışan SAP HANA için yedekleme kılavuzu yalnızca Azure 'a özgü konuları tanımlıyor. Genel SAP HANA yedeklemeyle ilgili öğeler için SAP HANA belgelerini denetleyin. İlke veritabanı yedekleme stratejilerini, nedenleri ve bir ses ve geçerli yedekleme stratejisi hakkında bilgi sahibi olmanız ve şirketinizin yedekleme prosedürü, bekletme süresi ve geri yükleme yordamının elde etmesinin gerekli olduğu gereksinimlerin farkında olmanız beklenir.

SAP HANA, Azure M serisi gibi çeşitli Azure VM türlerinde resmi olarak desteklenir. SAP HANA sertifikalı Azure VM 'lerinin ve HANA büyük örnek birimlerinin tüm listesi için [sertifikalı IaaS platformları bulun](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)' i inceleyin. Microsoft Azure, SAP HANA fiziksel sunucularda sanallaştırılmamış bir şekilde çalıştığı birimlerin sayısını sağlar. Bu hizmet, [Hana büyük örnekleri](hana-overview-architecture.md)olarak adlandırılır. Bu kılavuz, HANA büyük örnekleri için Yedekleme süreçlerini ve araçları kapsamaz. Ancak Azure sanal makinelerle sınırlı olacak. HANA büyük örneklerle yedekleme/geri yükleme işlemlerine ilişkin ayrıntılar için [hLi yedekleme ve geri yükleme](./hana-backup-restore.md)makalesini okuyun.

Bu makalenin konusu, Azure sanal makinelerinde SAP HANA için üç yedekleme olasılıkdır:

- [Azure Backup Hizmetleri](../../../backup/backup-overview.md) aracılığıyla Hana yedeklemesi 
- Azure Linux sanal makinesindeki dosya sistemine HANA yedeklemesi (bkz. [SAP HANA Azure Backup dosya düzeyinde](sap-hana-backup-file-level.md))
- Azure Depolama Blobu anlık görüntü özelliğini el ile veya Azure Backup hizmeti kullanarak depolama anlık görüntülerini temel alan HANA yedeklemesi


SAP HANA, üçüncü taraf yedekleme araçlarının doğrudan SAP HANA tümleştirilmesine olanak sağlayan bir yedekleme API 'SI sunmaktadır. Azure Backup hizmeti veya [Commkasa](https://azure.microsoft.com/resources/protecting-sap-hana-in-azure/) gibi ürünler, SAP HANA veritabanını tetiklemek veya günlük yedeklerini yinelemek için bu özel arabirimi kullanıyor. 


Azure 'da hangi SAP yazılımlarını desteklebileceğinize ilişkin bilgiler, [Azure dağıtımları Için HANGI SAP yazılımlarının desteklenmiş](./sap-supported-product-on-azure.md)makalesinde bulunabilir.

## <a name="azure-backup-service"></a>Azure Backup hizmeti

Gösterilen ilk senaryo, Azure Backup hizmetin bir `backint` SAP HANA veritabanından bir akış yedeklemesi gerçekleştirmek için SAP HANA arabirimini kullandığı bir senaryodur. Ya da uygulamayla tutarlı bir disk anlık görüntüsü oluşturmak ve bir tane Azure Backup hizmetine aktarıldıklarından Azure Backup hizmeti için daha genel bir yetenek kullanın.

Azure Backup, [backint](https://www.sap.com/dmc/exp/2013_09_adpd/enEN/#/d/solutions?id=8f3fd455-a2d7-4086-aa28-51d8870acaa5)adlı özel SAP HANA arabirimini kullanarak SAP HANA için yedekleme çözümü olarak tümleştirilir ve sertifikalandırilmiştir. Çözüm hakkında daha fazla ayrıntı için, özellikleri ve kullanılabilir olduğu Azure bölgeleri, [Azure VM 'lerinde SAP HANA veritabanlarının yedeklenmesi Için destek matrisi](../../../backup/sap-hana-backup-support-matrix.md#scenario-support)makalesini okuyun. HANA için Azure Backup hizmeti hakkında ayrıntılar ve ilkeler için, [Azure VM 'lerinde SAP HANA veritabanı yedeklemesi hakkındaki](../../../backup/sap-hana-db-about.md)makaleyi okuyun. 

Azure Backup hizmetin faydalanma olasılığı, Azure Premium Depolama 'nın disk anlık görüntülerini kullanarak uygulamayla tutarlı bir yedekleme oluşturmaktır. [Azure Ultra disk](../../linux/disks-enable-ultra-ssd.md) ve [Azure NetApp FILES](https://azure.microsoft.com/services/netapp/) gibi diğer Hana sertifikalı Azure depolama, Azure Backup hizmet aracılığıyla bu tür bir anlık görüntüyü desteklememektedir. Şu makaleler okunuyor:

- [Azure’da sanal makine yedekleme altyapınızı planlama](../../../backup/backup-azure-vms-introduction.md)
- [Uygulama ile tutarlı Azure Linux VM yedekleri](../../../backup/backup-azure-linux-app-consistent.md) 

Bu etkinlik sırası oluşur:

- Azure Backup, uygulamayı bu durumda SAP HANA tutarlı bir durumda yerleştiren anlık görüntü öncesi bir betik yürütmeniz gerekir
- Bu tutarlı durum onaylandığından Azure Backup disk anlık görüntülerini yürütecektir
- Anlık görüntüleri tamamladıktan sonra, Azure Backup anlık görüntü öncesi betikteki etkinliği geri alacak
- Başarılı yürütmeden sonra, Azure Backup verileri yedekleme kasasına akışa acaktır

SAP HANA olması durumunda çoğu müşteri, SAP HANA yineleme günlüğünü içeren birimler için Azure Yazma Hızlandırıcısı kullanıyor. Azure Backup hizmet bu birimleri otomatik olarak anlık görüntülerle hariç bırakır. Bu dışlama, HANA 'nın geri yükleme özelliğine zarar vermez. Bununla birlikte, neredeyse tüm diğer SAP destekli DBMS ile geri yükleme özelliğini engelleyebilse de.

Bu olasılığa ait aşağı, kendi ön ve anlık görüntü sonrası komut dosyanızı geliştirmeniz için ihtiyacınız olan bir olgu vardır. Anlık görüntü öncesi betiğin bir HANA anlık görüntüsü oluşturması ve nihai özel durum durumlarını işlemesi gerekir. Anlık görüntü sonrası betiğin HANA anlık görüntüsünü yeniden silmesi gerekir. Gerekli mantık hakkında daha fazla bilgi için, [#2039883 sap desteği](https://launchpad.support.sap.com/#/notes/2039883)' ni başlatın. Bu makalede, ' depolama anlık görüntülerini alırken veri tutarlılığı SAP HANA ' bölümünün önemli noktaları, bu tür bir yedekleme için tam olarak geçerlidir.

> [!NOTE]
> Birden çok veritabanı kapsayıcısının kullanıldığı dağıtımlarda SAP HANA için disk anlık görüntüsü tabanlı yedeklemeler, en düşük HANA 2,0 SP04 sürümünü gerektirir
> 

Bu belgede daha sonra bulunan depolama anlık görüntüleri hakkındaki ayrıntılara bakın.

![Bu şekilde, geçerli VM durumunun kaydedilmesine yönelik iki olasılık gösterilmektedir](media/sap-hana-backup-guide/azure-backup-service-for-hana.png)

## <a name="other-hana-backup-methods"></a>Diğer HANA yedekleme yöntemleri
Göz önünde bulundurmanız gereken üç farklı yedekleme yöntemi veya yolu vardır:

- Azure NetApp Files (ANF) tabanlı bir NFS paylaşımında yedekleme. ANF, yedeklemeleri depoladığınız birimlerin anlık görüntülerini oluşturma olanağına sahiptir. Yedeklemeleri yazmak için gereken aktarım hızı, bu çözüm pahalı bir yöntem olabilir. HANA 'nın yedeklemeleri doğrudan Azure yerel NFS paylaşımında yazabilmesi daha kolay hale gelse
- Standart SSD veya Azure Premium Storage 'ın VM 'ye bağlı disklerine yönelik HANA yedeklemesini yürütme. Sonraki adımda bu yedekleme dosyalarını Azure Blob depolamaya göre kopyalayabilirsiniz. Bu strateji fiyat temelinde etkileyici olabilir
- Standart SSD veya Azure Premium Storage 'ın VM 'ye bağlı disklerine yönelik HANA yedeklemesini yürütme. Sonraki adımda, disk düzenli olarak anlık görüntüyle alınır. İlk anlık görüntüden sonra, maliyetleri azaltmak için Artımlı anlık görüntüler kullanılabilir

![Bu şekilde, sanal makine içinde SAP HANA dosya yedeklemesi alma seçenekleri gösterilmektedir](media/sap-hana-backup-guide/other-hana-backup-paths.png)

Bu şekilde, sanal makine içinde bir SAP HANA dosya yedeklemesi alma seçenekleri gösterilmektedir ve ardından bu farklı araçları kullanarak bu şekilde BT HANA yedekleme dosyalarını başka bir yerde depolarsınız. Bununla birlikte, üçüncü taraf bir yedekleme hizmeti veya Azure Backup hizmeti içermeyen tüm çözümlerin ortak olarak çeşitli salları vardır. Bunlardan bazıları, bekletme yönetimi, otomatik geri yükleme işlemi ve Azure Backup hizmeti ya da diğer özel üçüncü taraf yedekleme paketleri ve hizmetleri için otomatik olarak belirli bir zaman kurtarma olanağı gibi listelenebilir. Bu üçüncü taraf hizmetlerin birçoğu Azure 'da çalıştırabiliyor. 


## <a name="sap-resources-for-hana-backup"></a>HANA yedeklemesi için SAP kaynakları

### <a name="sap-hana-backup-documentation"></a>SAP HANA yedekleme belgeleri

- [SAP HANA yönetimine giriş](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US)
- [Yedekleme ve kurtarma stratejinizi planlama](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)
- [ABAP DBAKOKPIT kullanarak HANA yedeklemesini zamanlama](https://www.hanatutorials.com/p/schedule-hana-backup-using-abap.html)
- [Veri yedeklemeleri zamanla (SAP HANA kokpiti)](https://help.sap.com/saphelp_hanaplatform/helpdata/en/6d/385fa14ef64a6bab2c97a3d3e40292/frameset.htm)
- [SAP Note 1642148](https://launchpad.support.sap.com/#/notes/1642148) SAP HANA yedekleme hakkında SSS
- [SAP Note 2039883](https://launchpad.support.sap.com/#/notes/2039883) SAP HANA veritabanı ve depolama anlık GÖRÜNTÜLERI hakkında SSS
- [SAP Note](https://launchpad.support.sap.com/#/notes/1820529) 'da yedekleme ve kurtarma için uygun olmayan ağ dosya sistemleri 1820529

### <a name="how-to-verify-correctness-of-sap-hana-backup"></a>SAP HANA yedeğinin doğruluğunu doğrulama
Yedekleme yönteminizin bağımsız olarak, farklı bir sisteme karşı bir test geri yükleme çalıştırmak mutlak bir zorunludur. Bu yaklaşım, bir yedeklemenin doğru olduğundan ve yedekleme ve geri yükleme için iç işlemlerin beklendiği gibi çalışmasını sağlamak için bir yol sağlar. Yedeklemeleri geri yükleme, altyapı gereksinimleri nedeniyle Şirket içi olarak bir arada olabilir, bu amaçla gerekli kaynakları geçici olarak sağlayarak bulutta gerçekleştirilmesi çok daha kolay olur. HANA ile birlikte sunulan, yedekleme dosyalarını geri yükleme yeteneğine sahip araçların bulunduğu araçlar vardır. Ancak, sık geri yükleme işleminin amacı, bir veritabanı geri yükleme sürecini test etmek ve bu işlemi operasyon personeli ile eğitmeniz sağlamaktır.

Basit bir geri yükleme ve HANA 'nın çalışır durumda olup olmadığını kontrol etmek için yeterli olup olmadığını aklınızda bulundurun. Geri yüklenen veritabanının iyi olduğundan emin olmak için bir tablo tutarlılık denetimi çalıştırmanız gerekir. SAP HANA, [SAP Note 1977584](https://launchpad.support.sap.com/#/notes/1977584)' de açıklanan çeşitli türlerde tutarlılık denetimleri sunmaktadır.

Tablo tutarlılık denetimiyle ilgili bilgiler ayrıca, [tablo ve Katalog tutarlılığı DENETIMLERINDE](https://help.sap.com/saphelp_hanaplatform/helpdata/en/25/84ec2e324d44529edc8221956359ea/content.htm#loio9357bf52c7324bee9567dca417ad9f8b)SAP web sitesinde bulunabilir.

### <a name="pros-and-cons-of-hana-backup-versus-storage-snapshot"></a>HANA yedeklemesini ve depolama anlık görüntüsüne yönelik olumlu ve olumsuz yönleri

SAP&#39;t, depolama anlık görüntüsüne göre HANA yedeklemesini tercih edebilir. Profesyonelleri ve dezavantajlarını listeler. bu nedenle, bir diğeri duruma ve kullanılabilir depolama teknolojisine bağlı olarak hangisinin kullanılacağını belirleyebilir (bkz. [yedekleme ve kurtarma stratejinizi planlama](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm)).

Azure 'da, Azure Blob anlık görüntüsü&#39;özelliğinin birden çok diskte dosya sistemi tutarlılığı sunduğuna dikkat edin (bkz. [PowerShell ile blob anlık görüntüleri kullanma](/archive/blogs/cie/using-blob-snapshots-with-powershell)). 

Bunlara ek olarak, bu makalede açıklandığı gibi BLOB anlık görüntüleri ile sık sık çalışırken faturalandırma etkilerini anlamak gerekir: [anlık görüntülerin nasıl tahakkuk ettirildiğini anlama](/rest/api/storageservices/understanding-how-snapshots-accrue-charges)— Azure sanal diskleri 'ni kullanarak açık olarak&#39;t.

### <a name="sap-hana-data-consistency-when-taking-storage-snapshots"></a>Depolama anlık görüntüleri alırken veri tutarlılığını SAP HANA

Daha önce belirtildiği gibi, Azure Backup anlık görüntü yedekleme özelliklerini açıklayan dosya sistemi ve uygulama tutarlılığı, depolama anlık görüntüleri alınırken zorunludur. Sorunlardan kaçınmak için en kolay yol SAP HANA veya belki de sanal makinenin tamamını kapatmaktır. Bir üretim örneği için uygulanabilir olmayan bir şey.

> [!NOTE]
> Birden çok veritabanı kapsayıcısının kullanıldığı dağıtımlarda SAP HANA için disk anlık görüntüsü tabanlı yedeklemeler, en düşük HANA 2,0 SP04 sürümünü gerektirir
> 

Azure depolama, anlık görüntü işlemi sırasında bir VM 'ye bağlı birden çok diskte veya birimde dosya sistemi tutarlılığı sağlamaz. Bu, anlık görüntü sırasında uygulama tutarlılığının uygulama tarafından teslim edilmesi gerektiği anlamına gelir, bu durumda SAP HANA. [SAP Note 2039883](https://launchpad.support.sap.com/#/notes/2039883) , depolama anlık görüntülerine göre SAP HANA yedeklemeleri hakkında önemli bilgiler içerir. Örneğin, XFS dosya sistemleri ile, uygulama tutarlılığı sağlamak üzere bir depolama anlık görüntüsü başlatmadan önce **XFS \_ donması** 'nı çalıştırmak gerekir ( **XFS \_ **donması hakkında ayrıntılar için bkz. [XFS \_ dondurma (8)-Linux man sayfası](https://linux.die.net/man/8/xfs_freeze) ).

Dört Azure sanal diski kapsayan bir XFS dosya sistemi olduğu varsayılarak, aşağıdaki adımlarda, HANA veri alanını temsil eden tutarlı bir anlık görüntü sağlanmıştır:

1. HANA veri anlık görüntüsü hazırlama oluşturma
1. Tüm disklerin/birimlerin dosya sistemlerini dondurun (örneğin, **XFS \_ donması**kullanın)
1. Tüm gerekli blob anlık görüntülerini Azure 'da oluşturun
1. Dosya sistemini çöz
1. HANA veri anlık görüntüsünü onaylayın (anlık görüntü silinir)

Uygulamayla tutarlı anlık görüntü yedeklemeleri gerçekleştirmek için Azure Backup özelliğini kullanırken, #1, ön anlık görüntü betiği için sizin tarafınızdan kodlanmış/komut dosyası alma yapmanız gerekir. Azure Backup hizmet, #2 ve #3 adımlarını yürütür. #4 ve #5 adımlarının, anlık görüntü sonrası betikte kodunuzun tarafından yeniden sağlanması gerekir. Azure Backup hizmetini kullanmıyorsanız, kod/betik adım #2 ve #3 kendiniz de gereklidir.
Aşağıdaki makalelerde HANA veri anlık görüntülerini oluşturma hakkında daha fazla bilgi bulabilirsiniz:

- [HANA veri anlık görüntüleri] (https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/en-US/ac114d4b34d542b99bc390b34f8ef375.html
- Adım #1 gerçekleştirme hakkında daha fazla ayrıntı, [veri anlık görüntüsü oluşturma (yerel SQL)](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/en-US/9fd1c8bb3b60455caa93b7491ae6d830.html) makalesinde bulunabilir 
- [Veri anlık görüntüsü oluşturma (yerel SQL)](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.04/en-US/9fd1c8bb3b60455caa93b7491ae6d830.html) makalesinde #5 adımında Hana veri anlık görüntülerini onaylama/silme hakkında bilgi bulabilirsiniz 

HANA anlık görüntüsünü doğrulamak önemlidir. &quot;Kopyalama-yazma nedeniyle, &quot; Bu anlık görüntü hazırlama modundayken SAP HANA ek disk alanı gerektirmeyebilir. Bu&#39;, SAP HANA anlık görüntüsü onaylanana kadar yeni yedeklemeler başlatmak için de mümkün değildir.


### <a name="sap-hana-backup-scheduling-strategy"></a>SAP HANA yedekleme zamanlama stratejisi

[Yedekleme ve kurtarma stratejinizi planlama](https://help.sap.com/saphelp_hanaplatform/helpdata/en/ef/085cd5949c40b788bba8fd3c65743e/content.htm) SAP HANA makalesinde, yedeklemeler yapılacak temel bir plan belirtilir. Yedekleme/geri yükleme stratejisi ve SAP HANA işlemini tanımlama bölümündeki diğer DBMS ile HANA 'daki SAP belgelerini ve deneyimlerinizi yedekleyin. Farklı yedekleme türlerinin sırası ve Bekletme dönemi, sağlamanız gereken SLA 'Lara oldukça bağlıdır.


### <a name="sap-hana-backup-encryption"></a>SAP HANA yedekleme şifrelemesi

SAP HANA veri ve günlük şifrelemeyi sağlar. SAP HANA veri ve günlük şifrelenmemişse, yedeklemeler varsayılan olarak şifrelenmez. Ancak, SAP HANA [SAP HANA yedekleme şifrelemesi](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.03/en-US/5f837a57ce5e468d9db21c8683bc84da.html)bölümünde belgelendiği gibi ayrı bir yedekleme şifrelemesi sunmaktadır. Daha eski SAP HANA sürümleri çalıştırıyorsanız, yedekleme şifrelemesinin zaten belirtilen işlevselliğin bir parçası olup olmadığını denetlemeniz gerekebilir.  


## <a name="next-steps"></a>Sonraki adımlar
* [Dosya düzeyinde Azure Backup SAP HANA](sap-hana-backup-file-level.md) dosya tabanlı yedekleme seçeneğini açıklar.
* Azure 'da SAP HANA olağanüstü durum kurtarma için yüksek kullanılabilirlik ve plan (büyük örnekler) oluşturma hakkında bilgi edinmek için bkz. [Azure 'da SAP HANA (büyük örnekler) yüksek kullanılabilirlik ve olağanüstü durum kurtarma](hana-overview-high-availability-disaster-recovery.md).
