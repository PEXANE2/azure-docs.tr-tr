---
title: SAP HANA'nın Azure sanal makinelere kurulumu | Microsoft Dokümanlar'
description: AZURE VM'lere SAP HANA yükleme kılavuzu
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/19/2020
ms.author: juergent
ms.openlocfilehash: e017e082472e7a4a2fab6a2845e52d3dc7acc460
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123359"
---
# <a name="installation-of-sap-hana-on-azure-virtual-machines"></a>SAP HANA'nın Azure sanal makinelere kurulumu
## <a name="introduction"></a>Giriş
Bu kılavuz, Azure sanal makinelerinde HANA'yı başarıyla dağıtmak için doğru kaynakları işaret etmenize yardımcı olur. Bu kılavuz, bir Azure VM'ye SAP HANA'yı yüklemeden önce denetlemeniz gereken dokümantasyon kaynaklarına yönlendirecektir. Böylece, Azure VM'lerde SAP HANA'nın desteklenen yapılandırmasıyla bitirmek için doğru adımları gerçekleştirebilirsiniz.  

> [!NOTE]
> Bu kılavuzda, SAP HANA'nın Azure VM'lerine dağıtımları açıklanmaktadır. SAP HANA'nın HANA'nın büyük örneklerine nasıl dağıtılanabildiğini öğrenmek için [Azure'da SAP HANA'yı (Büyük Örnekler) nasıl yükleyip yapılandırılabilirsiniz.](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-installation)
 
## <a name="prerequisites"></a>Ön koşullar
Bu kılavuz da aşina olduğunuzu varsayar:
* SAP HANA ve SAP NetWeaver ve nasıl şirket içinde bunları yüklemek için.
* SAP HANA ve SAP uygulama örnekleriAzure'da nasıl yüklenir ve çalıştırılabilen.
* Belgelenen kavramlar ve yordamlar:
   * Azure Sanal Ağ planlamave Azure Depolama kullanımını içeren Azure'da SAP dağıtımı planlaması. [Azure Sanal Makinelerde SAP NetWeaver '](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide) a bakın - Planlama ve uygulama kılavuzu
   * Dağıtım ilkeleri ve Azure'da VM dağıtma yolları. [SAP için Azure Sanal Makineler dağıtımına](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide) bakın
   * SAP HANA için yüksek kullanılabilirlik kavramları, [Azure sanal makineleri için SAP HANA'da belgelenen yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)

## <a name="step-by-step-before-deploying"></a>Dağıtmadan önce adım adım
Bu bölümde, BIR Azure sanal makinede SAP HANA yükleme ile başlamadan önce gerçekleştirmeniz gereken farklı adımlar listelenir. Sipariş numaralandırılır ve bu şekilde numaralandırılmış olarak takip edilmelidir:

1. Azure'da olası dağıtım senaryolarının tümü desteklenmez. Bu nedenle, SAP HANA dağıtımınızda aklınızda olan senaryo için [Azure sanal makine destekli senaryolarda](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-planning-supported-configurations) BELGE SAP iş yükünü denetlemeniz gerekir. Senaryo listelenmiyorsa, test edilmemiştir ve sonuç olarak desteklenmez varsaymanız gerekir
2. SAP HANA dağıtımınız için bellek gereksiniminiz hakkında kabaca bir fikriniz olduğunu varsayarsak, uygun bir Azure VM bulmanız gerekir. SAP Destek [Notu #1928533](https://launchpad.support.sap.com/#/notes/1928533)belgelenen SAP NetWeaver için sertifikalı olan tüm VM'ler SAP HANA sertifikalı değildir. SAP HANA sertifikalı Azure VM'ler için gerçeğin [kaynağı, SAP HANA donanım dizini](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)web sitesidir. **S** ile başlayan birimler Azure VM'leri değil [HANA Büyük Örnekler](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) birimleridir.
3. Farklı Azure VM türleri, SUSE Linux veya Red Hat Linux için farklı minimum işletim sistemi sürümlerine sahiptir. Web sitesi [SAP HANA donanım dizini](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure), Bu birimin ayrıntılı veri almak için SAP HANA sertifikalı birimlerin listesinde bir giriş tıklamanız gerekir. Desteklenen HANA iş yükünün yanı sıra, SAP HANA için bu birimlerile desteklenen işletim sistemi sürümleri listelenir
4. İşletim sistemi sürümleri itibariyle, belirli minimum çekirdek sürümlerini göz önünde bulundurmanız gerekir. Bu minimum sürümler bu SAP destek notlarında belgelenmiştir:
    - [SAP HANA Yedekleme#2814271 SAP destek notu, Azure'da Checksum Hatası ile başarısız oldu](https://launchpad.support.sap.com/#/notes/2814271)
    - [ZAMANLAYıCı Geri Dönüş Nedeniyle Potansiyel Performans Düşüşü #2753418 SAP destek notu](https://launchpad.support.sap.com/#/notes/2753418)
    - [SAP destek notu #2791572 Azure'da Hyper-V için Eksik VDSO Desteği Nedeniyle Performans Bozulması](https://launchpad.support.sap.com/#/notes/2791572)
4. Tercih edilen sanal makine türü için desteklenen işletim sistemi sürümüne bağlı olarak, istediğiniz SAP HANA sürümübu işletim sistemi sürümüyle desteklenip desteklenmediğini kontrol etseniz gerekir. Sap HANA bültenlerinin farklı İşletim Sistemi sürümlerine sahip destek matrisi için [SAP destek notunu #2235581](https://launchpad.support.sap.com/#/notes/2235581) okuyun.
5. Azure VM türü, işletim sistemi sürümü ve SAP HANA sürümünden geçerli bir kombinasyon bulmuş olabileceğiniz için SAP Ürün Kullanılabilirlik Matrisi'ni iade etmeniz gerekir. SAP Kullanılabilirlik Matrisi'nde, SAP HANA veritabanınıza karşı çalıştırmak istediğiniz SAP ürününüzün desteklenip desteklenmediğini öğrenebilirsiniz.


## <a name="step-by-step-vm-deployment-and-guest-os-considerations"></a>Adım adım VM dağıtımı ve konuk İşletim Sistemi konuları
Bu aşamada, HANA'yı yüklemek için VM(ler)i dağıtan adımlardan geçmeniz ve kurulumdan sonra seçilen işletim sistemini en iyi duruma getirmeniz gerekir.

1. Azure galerisinden temel görüntüyü seçti. SAP HANA için kendi işletim sistemi görüntünüzoluşturmak istiyorsanız, başarılı bir SAP HANA kurulumu için gerekli olan tüm farklı paketleri bilmeniz gerekir. Aksi takdirde, Azure resim galerisinden SAP veya SAP HANA için SUSE ve Red Hat görüntülerinin kullanılması önerilir. Bu görüntüler, başarılı bir HANA yüklemesi için gerekli paketleri içerir. İşletim sistemi sağlayıcısıyla olan destek sözleşmenize bağlı olarak, kendi lisansınızı getirdiğiniz bir resim seçmeniz gerekir. Veya destek içeren bir işletim sistemi görüntüsü seçin
2. Kendi lisansınızı getirmenizi gerektiren bir konuk işletim sistemi görüntüsü seçtiyseniz, en son düzeltme ekine sahip olve uygulayabilmeniz için işletim sistemi görüntüsünü aboneliğinize kaydetmeniz gerekir. Bu adım kamu internet erişimi gerektirecektir. Örneğin Azure'da bir SMT sunucusunun özel örneğini ayarlamadığınız sürece.
3. VM'nin ağ yapılandırmasını belirleyin. [Azure'daki SAP HANA altyapı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)nde daha fazla bilgi okuyabilirsiniz. Azure'daki sanal ağ kartlarına atayabileceğiniz ağ iş başı kotası olmadığını unutmayın. Sonuç olarak, trafiği farklı vNIC'ler üzerinden yönlendirmenin tek amacı güvenlik hususlarına bağlıdır. Birden çok vNIC üzerinden trafik yönlendirme karmaşıklığı ve güvenlik yönleri tarafından uygulanan gereksinimleri arasında desteklenebilir bir uzlaşma bulmak için size güveniyoruz.
3. VM dağıtıldıktan ve kaydedildikten sonra en son düzeltme em'lerini işletim sistemine uygulayın. Kendi aboneliğinize kayıtlı. Ya da işletim sistemi desteği içeren bir görüntü seçtiyseniz VM'nin düzeltme eki nerelere zaten erişebilmeli. 
4. SAP HANA için gerekli melodileri uygulayın. Bu melodiler bu SAP destek notlarında listelenir:

    - [SAP destek notu #2694118 - Red Hat Enterprise Linux HA Eklentisi Azure'da](https://launchpad.support.sap.com/#/notes/2694118)
    - [SAP destek notu #1984787 - SUSE LINUX Enterprise Server 12: Kurulum notları](https://launchpad.support.sap.com/#/notes/1984787) 
    - [SAP destek notu #2578899 - SUSE Linux Enterprise Server 15: Kurulum Notu](https://launchpad.support.sap.com/#/notes/2578899)
    - [SAP destek notu #2002167 - Red Hat Enterprise Linux 7.x: Kurulum ve Yükseltme](https://launchpad.support.sap.com/#/notes/0002002167)
    - [SAP destek notu #2292690 - SAP HANA DB: RHEL 7 için önerilen işletim sistemi ayarları](https://launchpad.support.sap.com/#/notes/0002292690) 
    -  [SAP destek notu #2772999 - Red Hat Enterprise Linux 8.x: Kurulum ve Yapılandırma](https://launchpad.support.sap.com/#/notes/2772999) 
    -  [SAP destek notu #2777782 - SAP HANA DB: RHEL 8 için önerilen Işletim Sistemi Ayarları](https://launchpad.support.sap.com/#/notes/2777782)
    -  [SAP destek notu #2455582 - Linux: GCC 6.x ile derlenen SAP uygulamalarını çalıştırmak](https://launchpad.support.sap.com/#/notes/0002455582)
    -  [SAP destek notu #2382421 - HANA ve OS Düzeyinde Ağ Yapılandırmasını Optimize Etme](https://launchpad.support.sap.com/#/notes/2382421)

1. SAP HANA için Azure depolama türünü seçin. Bu adımda, SAP HANA yüklemesi için depolama düzenine karar vermeniz gerekir. Ekli Azure diskleri veya yerel Azure NFS paylaşımlarını kullanacaksınız. Kullanılabilecek farklı Azure depolama türlerinin desteklenen veya desteklenen Azure depolama türleri ve kombinasyonları [SAP HANA Azure sanal makine depolama yapılandırmalarında](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)belgelenmiştir. Başlangıç noktası olarak belgelenen yapılandırmaları alın. Üretim dışı sistemler için daha düşük iş ortası veya IOPS yapılandırmak mümkün olabilir. Üretim amacıyla, biraz daha fazla iş ve IOPS yapılandırmanız gerekebilir.
2. M-Serisi veya Mv2 Serisi VM'leri kullanırken DBMS işlem günlüklerini veya redo günlüklerini içeren birimleriniz için [Azure Yazma Hızlandırıcısı](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) yapılandırdığınızdan emin olun. Belgelenmiş olarak Yazma Hızlandırıcısı ile ilgili sınırlamaların farkında olun.
2. Dağıtılan VM(ler)'de [Azure Hızlandırılmış Ağ](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) etkinleştirilip etkinleştirilmediğini denetleyin.

> [!NOTE]
> Farklı sap-tune profillerindeki veya notlarda açıklandığı gibi tüm komutlar Azure'da başarılı bir şekilde çalıştırılamaz. Temel Azure ana bilgisayar donanımının güç modu değiştirilemediğinden, VM'lerin güç modunu manipüle edecek komutlar genellikle bir hatayla geri döner.

## <a name="step-by-step-preparations-specific-to-azure-virtual-machines"></a>Azure sanal makinelerine özel adım adım hazırlıklar
Azure özelliklerinden biri, SAP Ana Bilgisayar Aracısı için izleme verileri sunan bir Azure VM uzantısının yüklenmesidir. Bu izleme uzantısı yükleme ile ilgili ayrıntılar belgelenmiştir:

-  [SAP Note 2191498,](https://launchpad.support.sap.com/#/notes/2191498/E) Azure'da Linux VM'lerle SAP gelişmiş izlemeyi tartışıyor 
-  [SAP Note 1102124](https://launchpad.support.sap.com/#/notes/1102124/E) Linux'ta SAPOSCOL hakkında bilgi tartışıyor 
-  [SAP Note 2178632,](https://launchpad.support.sap.com/#/notes/2178632/E) Microsoft Azure'da SAP için temel izleme ölçümlerini tartışıyor
-  [SAP NetWeaver için Azure Sanal Makineler dağıtımı](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide#d98edcd3-f2a1-49f7-b26a-07448ceb60ca)

## <a name="sap-hana-installation"></a>SAP HANA kurulumu
Azure sanal makineleri dağıtıldı ve işletim sistemleri kayıtlı ve yapılandırılmış, SAP yükleme göre SAP HANA yükleyebilirsiniz. Bu dokümantasyona ulaşmak için iyi bir başlangıç olarak, bu SAP web sitesi [HANA kaynakları](https://www.sap.com/products/hana/implementation/resources.html) ile başlayın

Azure Premium Depolama veya Ultra diskin doğrudan bağlı disklerini kullanan SAP HANA ölçeklendirme yapılandırmaları [için, Azure'daki SAP HANA altyapı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations#configuring-azure-infrastructure-for-sap-hana-scale-out) belgesindeki ayrıntıları okuyun


## <a name="additional-resources-for-sap-hana-backup"></a>SAP HANA yedeklemesi için ek kaynaklar
Azure VM'lerde SAP HANA veritabanlarını yedekleme hakkında bilgi için bkz:
* [Azure Sanal Makinelerde SAP HANA için yedekleme kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [DOSYA DÜZEYINDE SAP HANA Azure Yedekleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)

## <a name="next-steps"></a>Sonraki adımlar
Belgeleri okuyun:

- [Azure'da SAP HANA altyapı yapılandırmaları ve işlemleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations)
- [SAP HANA Azure sanal makine depolama alanı yapılandırmaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)





