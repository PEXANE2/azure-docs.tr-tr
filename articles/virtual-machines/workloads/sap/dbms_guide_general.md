---
title: SAP iş yükü için Azure Sanal Makineler DBMS dağıtımı nda dikkat edilmesi gerekenler | Microsoft Dokümanlar
description: SAP iş yükü için Azure Sanal Makineler DBMS dağıtımında dikkat edilmesi gerekenler
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a852ddc68a6f51e677e5ff2e641ada25f4bf0105
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70101356"
---
# <a name="considerations-for-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>SAP iş yükü için Azure Sanal Makineler DBMS dağıtımında dikkat edilmesi gerekenler
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[deployment-guide]:deployment-guide.md
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[planning-guide]:planning-guide.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Bu kılavuz, SAP yazılımının Microsoft Azure'da nasıl uygulanacağı ve dağıtılanın gerektiğiyle ilgili belgelerin bir parçasıdır. Bu kılavuzu okumadan önce [Planlama ve uygulama kılavuzunu][planning-guide]okuyun. Bu belge, Azure altyapısını hizmet (IaaS) özellikleri olarak kullanarak Microsoft Azure sanal makinelerde (VM) SAP ile ilgili DBMS sistemlerinin genel dağıtım yönlerini kapsar.

Kağıt, SAP yazılımının belirli platformlardaki yüklemeleri ve dağıtımları için birincil kaynakları temsil eden SAP yükleme belgelerini ve SAP Notlarını tamamlar.

Bu belgede, Azure VM'lerinde SAP ile ilgili DBMS sistemlerinin çalıştırılmasıyla ilgili hususlar sunulmuştur. Bu bölümde belirli DBMS sistemlerine birkaç başvuru vardır. Bunun yerine, belirli DBMS sistemleri bu belgeden sonra bu kağıt içinde işlenir.

## <a name="definitions"></a>Tanımlar
Belge boyunca bu terimler kullanılır:

* **IaaS**: Hizmet olarak altyapı.
* **PaaS**: Platform bir hizmet olarak.
* **SaaS**: Bir hizmet olarak yazılım.
* **SAP bileşeni**: ERP Merkezi Bileşeni (ECC), İş Ambarı (BW), Çözüm Yöneticisi veya Kurumsal Portal (EP) gibi bireysel SAP uygulaması. SAP bileşenleri geleneksel ABAP veya Java teknolojilerine veya İş Nesneleri gibi NetWeaver tabanlı olmayan bir uygulamaya dayalı olabilir.
* **SAP ortamı**: Geliştirme, kalite güvencesi, eğitim, olağanüstü durum kurtarma veya üretim gibi bir iş işlevini gerçekleştirmek için mantıksal olarak gruplanmış bir veya daha fazla SAP bileşeni.
* **SAP peyzajı**: Bu terim, müşterinin BT ortamındaki tüm SAP varlıklarını ifade eder. SAP ortamı tüm üretim ve üretim dışı ortamları içerir.
* **SAP sistemi**: Bir DBMS katmanı ve bir uygulama katmanının birleşimi, örneğin, bir SAP ERP geliştirme sistemi, bir SAP İş Ambarı test sistemi veya sap CRM üretim sistemi. Azure dağıtımlarında, bu iki katmanı şirket içi ve Azure arasında bölmek desteklenmez. Sonuç olarak, bir SAP sistemi şirket içinde veya Azure'da dağıtılır. BIR SAP ortamının farklı sistemlerini Azure'da veya şirket içinde dağıtabilirsiniz. Örneğin, SAP CRM geliştirme ve test sistemlerini Azure'da dağıtabilir, ancak SAP CRM üretim sistemini şirket içinde dağıtabilirsiniz.
* **Tesisler arası**: Şirket içi veri merkezleri ve Azure arasında siteden siteye, çok siteye veya Azure ExpressRoute bağlantısına sahip bir Azure aboneliğinde SANAL'ların dağıtıldığı bir senaryoyu açıklar. Ortak Azure belgelerinde, bu tür dağıtımlar binalar arası senaryolar olarak da tanımlanır. 

    Bağlantının nedeni, şirket içi etki alanlarını, şirket içi Active Directory'yi ve şirket içi DNS'yi Azure'a genişletmektir. Şirket içi manzara, aboneliğin Azure varlıklarına genişletilir. Bu uzantıyla, VM'ler şirket içi etki alanının bir parçası olabilir. Şirket içi etki alanının etki alanı kullanıcıları sunuculara erişebilir ve DBMS hizmetleri gibi bu VM'lerde hizmet çalıştırabilir. Şirket içinde dağıtılan VM'ler ile Azure'da dağıtılan VM'ler arasında iletişim ve ad çözümlemesi mümkündür. Bu senaryo, SAP varlıklarını Azure'da dağıtmak için kullanılan en yaygın senaryodur. Daha fazla bilgi için [VPN ağ geçidi için Planlama ve tasarım bölümüne](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)bakın.

> [!NOTE]
> SAP sistemlerinin şirket içi dağıtımları, SAP sistemlerini çalıştıran Azure sanal makinelerinin şirket içi bir etki alanının üyeleri olduğu ve üretim SAP sistemleri için desteklendiği yerlerdir. Binalar arası yapılandırmalar, parçaları dağıtmak veya SAP manzaralarını Azure'a dağıtmak için desteklenir. Azure'da tüm SAP ortamını çalıştırmak bile bu VM'lerin şirket içi etki alanının ve Active Directory/LDAP'ın bir parçası olmasını gerektirir. 
>
> Belgelerin önceki sürümlerinde hibrit-BT senaryolarından bahsedilmiştir. *Karma* terimi, şirket içi ve Azure arasında bir şirket içi bağlantı olduğu gerçeğinden kaynaklanıyor. Bu durumda hibrit, Azure'daki VM'lerin şirket içi Active Directory'nin bir parçası olduğu anlamına da gelir.
>
>

Bazı Microsoft belgeleri, özellikle DBMS yüksek kullanılabilirlik yapılandırmaları için, binalar arası senaryoları biraz farklı olarak açıklar. SAP ile ilgili belgeler söz konusu olduğunda, tesisler arası senaryo siteden siteye veya özel [ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantısına ve şirket içi ve Azure arasında dağıtılan bir SAP ortamına indirgenir.

## <a name="resources"></a>Kaynaklar
Azure'da SAP iş yüküyle ilgili başka makaleler de vardır. [Azure'da SAP iş yüküyle başlayın: Başlayın](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) ve ilgi alanınızı seçin.

Aşağıdaki SAP Notları, bu belgede kapsanan alanla ilgili olarak Azure'daki SAP ile ilgilidir.

| Not numarası | Başlık |
| --- | --- |
| [1928533] |Azure'daki SAP uygulamaları: Desteklenen ürünler ve Azure VM türleri |
| [2015553] |Microsoft Azure'da SAP: Ön koşulları destekleyin |
| [1999351] |SAP için gelişmiş Azure izleme sorun giderme |
| [2178632] |Microsoft Azure'da SAP için temel izleme ölçümleri |
| [1409604] |Windows'da sanallaştırma: Gelişmiş izleme |
| [2191498] |Azure ile Linux'ta SAP: Gelişmiş izleme |
| [2039619] |Oracle veritabanını kullanarak Microsoft Azure'daki SAP uygulamaları: Desteklenen ürünler ve sürümler |
| [2233094] |DB6: Linux, UNIX ve Windows için IBM DB2'yi kullanarak Azure'daki SAP uygulamaları: Ek bilgiler |
| [2243692] |Microsoft Azure (IaaS) VM üzerinde Linux: SAP lisans sorunları |
| [1984787] |SUSE LINUX Enterprise Server 12: Kurulum notları |
| [2002167] |Red Hat Enterprise Linux 7.x: Kurulum ve yükseltme |
| [2069760] |Oracle Linux 7.x SAP kurulumu ve yükseltmesi |
| [1597355] |Linux için takas alanı önerisi |
| [2171857] |Oracle Database 12c: Linux'ta dosya sistemi desteği |
| [1114181] |Oracle Database 11g: Linux dosya sistemi desteği |


Linux için tüm SAP Notları hakkında bilgi için [SAP topluluk wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes)bakın.

Microsoft Azure mimarisi ve Microsoft Azure sanal makinelerinin nasıl dağıtılar ve çalıştırılan hakkında çalışma bilgisine ihtiyacınız vardır. Daha fazla bilgi için Azure [belgelerine](https://docs.microsoft.com/azure/)bakın.

Genel olarak, Windows, Linux ve DBMS yükleme ve yapılandırma temelde herhangi bir sanal makine veya çıplak metal makine size tesislerinde yüklemek aynıdır. Azure IaaS kullandığınızda farklı olan bazı mimari ve sistem yönetimi uygulama kararları vardır. Bu belge, Azure IaaS'i kullandığınızda hazırlıklı olunması gereken belirli mimari ve sistem yönetimi farklılıklarını açıklar.


## <a name="storage-structure-of-a-vm-for-rdbms-deployments"></a><a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>RDBMS dağıtımları için vm depolama yapısı
Bu bölümü takip etmek için [Dağıtım Kılavuzu'nun][deployment-guide] [bu bölümünde][deployment-guide-3] sunulan bilgileri okuyun ve anlayın. Bu bölümü okumadan önce farklı VM-Serisi ve standart ve premium depolama arasındaki farkları anlamanız ve bilmeniz gerekir. 

Azure VM'ler için Azure Depolama hakkında bilgi edinmek için bkz:

- [Azure Windows VM'leri için yönetilen disklere giriş.](../../windows/managed-disks-overview.md)
- [Azure Linux VM'leri için yönetilen disklere giriş.](../../linux/managed-disks-overview.md)

Temel bir yapılandırmada, genellikle işletim sistemi, DBMS ve nihai SAP ikililerinin veritabanı dosyalarından ayrı olduğu bir dağıtım yapısı öneririz. Azure sanal makinelerinde çalışan SAP sistemlerinin işletim sistemi, veritabanı yönetim sistemi çalıştırılabilirleri ve SAP yürütülebilirleriyle yüklü temel VHD veya diske sahip olması önerilir. 

DBMS verileri ve günlük dosyaları standart depolama veya premium depolama da saklanır. Bunlar ayrı disklerde depolanır ve orijinal Azure işletim sistemi görüntüsü VM'ye mantıksal disk olarak eklenir. Linux dağıtımları için, özellikle SAP HANA için farklı öneriler belgelenmiştir.

Disk düzeninizi planlarken, bu öğeler arasındaki en iyi dengeyi bulun:

* Veri dosyalarının sayısı.
* Dosyaları içeren disk sayısı.
* Tek bir diskin IOPS kotaları.
* Disk başına veri aktarımı.
* VM boyutu başına mümkün olan ek veri disklerinin sayısı.
* Bir VM'nin sağlayabileceği genel depolama iş bilgililiği.
* Farklı Azure Depolama türleri gecikme alanı sağlayabilir.
* VM SLA'lar.

Azure, veri diski başına bir IOPS kotası uygular. Bu kotalar, standart depolama ve premium depolama da barındırılan diskler için farklıdır. G/Ç gecikmesi de iki depolama türü arasında farklıdır. Premium depolama daha iyi G/Ç gecikmesi sağlar. 

Farklı VM türlerinin her biri, ekebileceğiniz sınırlı sayıda veri diski vardır. Başka bir kısıtlama, yalnızca belirli VM türlerinin premium depolama yı kullanabiliyor olmasıdır. Genellikle, CPU ve bellek gereksinimlerine dayalı belirli bir VM türü kullanmaya karar verirsiniz. Ayrıca, genellikle disk sayısı veya premium depolama disklerinin türüyle ölçeklenmiş olan IOPS, gecikme ve disk iş boyutu gereksinimlerini de düşünebilirsiniz. IOPS sayısı ve her disk tarafından elde edilecek iş bölümü, özellikle premium depolama alanı yla disk boyutunu belirleyebilir.

> [!NOTE]
> DBMS dağıtımları için, tüm veriler, işlem günlüğü veya yeniden işlem dosyaları için premium depolama alanı kullanılmasını öneririz. Üretim veya üretim dışı sistemleri dağıtmak isteyip istemediğiniz önemli değildir.

> [!NOTE]
> Azure'un benzersiz [tek VM SLA'sından](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)yararlanabilmek için, bağlı olan tüm diskler temel VHD'yi içeren premium depolama türü olmalıdır.

> [!NOTE]
> Azure veri merkezlerine bitişik üçüncü taraf veri merkezlerinde bulunan depolama donanımındaki SAP veritabanlarının veri ve günlük dosyaları gibi ana veritabanı dosyalarını barındırmak desteklenmez. SAP iş yükleri için, SAP veritabanlarının veri ve işlem günlüğü dosyaları için yalnızca yerel Azure hizmeti olarak temsil edilen depolama alanı desteklenir.

Veritabanı dosyalarının yerleşimi, günlük ve redo dosyaları ve kullandığınız Azure Depolama türü IOPS, gecikme sonu ve iş yeri gereksinimleri tarafından tanımlanır. Yeterli IOPS'ye sahip olmak için, birden çok disk kullanmak veya daha büyük bir premium depolama diski kullanmak zorunda kalabilirsiniz. Birden çok disk kullanıyorsanız, diskler arasında veri dosyalarını veya günlük ve redo dosyalarını içeren bir yazılım şeridi oluşturun. Bu gibi durumlarda, Altta yatan premium depolama disklerinin IOPS ve disk iş bilgili SLA'ları veya standart depolama disklerinin ulaşılabilen maksimum IOPS'leri, elde edilen şerit kümesi için birikebilir.

Daha önce de belirtildiği gibi, IOPS gereksiniminiz tek bir VHD'nin sağlayabileceğini aşıyorsa, veritabanı dosyaları için gereken IOPS sayısını bir dizi VHD arasında dengeleyin. IOPS yükünü diskler arasında dağıtmanın en kolay yolu, farklı disklerin üzerine bir yazılım şeridi oluşturmaktır. Ardından, SAP DBMS'nin bir dizi veri dosyasını yazılım şeridinden oyulmuş LUN'lara yerleştirin. Şeritteki disk sayısı IOS talepleri, disk iş hacmi talepleri ve ses talepleri tarafından yönlendirilir.


---
> ![Windows][Logo_Windows] Windows
>
> Birden çok Azure VHD'de şerit kümeleri oluşturmak için Windows Depolama Alanları'nı kullanmanızı öneririz. En az Windows Server 2012 R2 veya Windows Server 2016 kullanın.
>
> ![Linux][Logo_Linux] Linux
>
> Linux'ta bir yazılım RAID oluşturmak için yalnızca MDADM ve Mantıksal Birim Yöneticisi (LVM) desteklenir. Daha fazla bilgi için bkz.
>
> - MDADM kullanarak [Linux üzerinde yazılım RAID yapılandırma](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
> - [LVM'yi kullanarak Azure'daki bir Linux VM üzerinde LVM yapılandırın](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)
>
>

---

> [!NOTE]
> Azure Depolama VHD'lerin üç görüntüsünü sakladığından, şerit yaptığınızda fazlalık yapılandırmanız mantıklı değildir. Yalnızca, G/Ç'lerin farklı VHD'ler üzerinden dağıtılması için şeritleme yapılandırmanız gerekir.
>

### <a name="managed-or-nonmanaged-disks"></a>Yönetilen veya yönetilmeyen diskler
Azure depolama hesabı yönetimsel bir yapıdır ve aynı zamanda sınırlamalar konusudur. Sınırlamalar standart depolama hesapları ile premium depolama hesapları arasında farklılık gösterir. Özellikler ve sınırlamalar hakkında bilgi için Azure [Depolama ölçeklenebilirliği ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)ne redelenebilir.

Standart depolama için, depolama hesabı başına IOPS'de bir sınır olduğunu unutmayın. [Azure Depolama ölçeklenebilirliği ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)makalesinde Toplam İstek **Oranı** içeren satıra bakın. Ayrıca, Azure aboneliği başına depolama hesabı sayısında da bir başlangıç sınırı vardır. Bu depolama hesaplarının sınırlarını vurmamak için farklı depolama hesaplarındaki daha büyük SAP ortamı için VHD'leri dengeleyin. Binden fazla VHD'si olan birkaç yüz sanal makineden bahsederken bu sıkıcı bir iştir.

Bir SAP iş yüküyle birlikte DBMS dağıtımları için standart depolama alanı kullanılması önerilmediği için, standart depolamaya yapılan başvurular ve öneriler bu kısa [makaleyle](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx) sınırlıdır

Microsoft, farklı Azure depolama hesaplarında VHD'leri planlama ve dağıtma nın yönetimle çalışmasını önlemek için 2017 yılında [Azure Yönetilen Diskler'i](https://azure.microsoft.com/services/managed-disks/) tanıttı. Yönetilen diskler standart depolama ve birinci sınıf depolama için kullanılabilir. Yönetilen disklerin yönetilmeyen disklere göre başlıca avantajları şunlardır:

- Yönetilen diskler için Azure, dağıtım zamanında farklı depolama hesaplarına farklı VHD'leri otomatik olarak dağıtır. Bu şekilde, veri hacmi, G/Ç iş hacmi ve IOPS için depolama hesabı sınırları vurulmaz.
- Azure Depolama, yönetilen diskleri kullanarak Azure kullanılabilirlik kümelerinin kavramlarını onurlandırıyor. VM bir Azure kullanılabilirlik kümesinin bir parçasıysa, vm'nin temel VHD'si ve bağlı diski farklı hata ve güncelleştirme etki alanlarında dağıtılır.


> [!IMPORTANT]
> Azure Yönetilen Disklerin avantajları göz önüne alındığında, DBMS dağıtımlarınız ve SAP dağıtımlarınız için genel olarak Azure Yönetilen Diskler kullanmanızı öneririz.
>

Yönetilmeyen disklerden yönetilen disklere dönüştürmek için bkz:

- [Windows sanal makinesini yönetilmeyen disklerden yönetilen disklere dönüştürün.](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)
- [Yönetilen disklerden yönetilen disklere linux sanal makine dönüştürün.](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks)


### <a name="caching-for-vms-and-data-disks"></a><a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>VM'ler ve veri diskleri için önbelleğe alma
Diskleri VM'lere monte ettiğinizde, VM ile Azure depolama alanında bulunan diskler arasındaki G/Ç trafiğinin önbelleğe geçirilip kapitülmediğini seçebilirsiniz. Standart ve üstün depolama, bu tür önbellekler için iki farklı teknoloji kullanır.

Aşağıdaki öneriler, standart DBMS için bu G/Ç özelliklerini varsayar:

- Çoğunlukla bir veritabanının veri dosyalarına karşı okunan bir iş yükü. Bu okumalar DBMS sistemi için performans açısından önemlidir.
- Veri dosyalarına karşı yazma, denetim noktalarını veya sabit akışı temel alan patlamalar halinde gerçekleşir. Bir gün içinde ortalama, okuma daha az yazma vardır. Veri dosyalarından okumaların tam tersi, bu yazılar eşzamanlıdır ve kullanıcı hareketlerini geciktirmez.
- İşlem günlüğünden veya yeniden işlem dosyalarından neredeyse hiç okuma yoktur. İşlem günlüğü yedeklemelerini gerçekleştirirken özel durumlar büyük G/Ç'lerdir.
- İşlem veya redo günlük dosyalarına karşı ana yük yazıyor. İş yükünün yapısına bağlı olarak, 4 KB kadar küçük G/Ç'lere veya diğer durumlarda 1 MB veya daha fazla G/Ç boyutuna sahip olabilirsiniz.
- Tüm yazılar güvenilir bir şekilde diskte kalıcı olmalıdır.

Standart depolama için olası önbellek türleri şunlardır:

* None
* Okuma
* Okuma/Yazma

Tutarlı ve deterministik performans elde etmek için, DBMS ile ilgili veri dosyalarını içeren tüm diskler için standart depolama alanında önbelleğe alma, günlük ve yeniden yapma dosyaları ve tablo alanını **NONE**olarak ayarlayın. Temel VHD önbelleğe alma varsayılan olarak kalabilir.

Birinci sınıf depolama için aşağıdaki önbelleğe alma seçenekleri vardır:

* None
* Okuma
* Okuma/yazma
* Yalnızca Azure M Serisi VM'ler için olan Yok + Yazma Hızlandırıcısı
* Yalnızca Azure M Serisi VM'ler için olan Okuma + Yazma Hızlandırıcısı

Premium depolama için, SAP veritabanının **veri dosyaları için Okuma önbelleğe alma** nızı kullanmanızı ve günlük **dosyası(lar) diskleri için önbelleğe alma mayı**seçmenizi öneririz.

M Serisi dağıtımları için, DBMS dağıtımınız için Azure Yazma Hızlandırıcısı kullanmanızı öneririz. Azure Yazma Hızlandırıcısı'nın ayrıntıları, kısıtlamaları ve dağıtımı için [bkz.](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator)


### <a name="azure-nonpersistent-disks"></a>Azure kalıcı olmayan diskler
Azure VM'ler, Bir VM dağıtıldıktan sonra kalıcı olmayan diskler sunar. VM yeniden başlatılması durumunda, bu sürücülerdeki tüm içerik silinir. Veri dosyalarının ve veritabanlarının günlük ve redo dosyalarının hiçbir koşulda bu kalıcı olmayan sürücülerde yer almaması gerektiği verilir. Bu kalıcı olmayan sürücülerin tempdb ve geçici tablo alanları için uygun olabileceği bazı veritabanları için özel durumlar olabilir. Bu kalıcı olmayan sürücüler bu VM ailesiyle sınırlı olduğundan, bu sürücüleri A Serisi VM'ler için kullanmaktan kaçının. 

Daha fazla bilgi için bkz: [Azure'daki Windows VM'lerde geçici sürücüyü anlayın.](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

---
> ![Windows][Logo_Windows] Windows
>
> Azure VM'deki Sürücü D, Azure işlem düğümündeki bazı yerel diskler tarafından desteklenen kalıcı olmayan bir sürücüdür. Devam etmediğinden, VM yeniden başlatıldığında D sürücüsündeki içerikte yapılan değişiklikler kaybolur. Değişiklikler, depolanan dosyaları, oluşturulan dizinleri ve yüklenen uygulamaları içerir.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure VM'ler, Azure bilgi işlem düğümündeki yerel diskler tarafından desteklenen kalıcı olmayan bir sürücü olan /mnt/kaynağa otomatik olarak bir sürücü monte eder. Devam etmediğinden, VM yeniden başlatıldığında /mnt/resource'daki içerikte yapılan değişiklikler kaybolur. Değişiklikler, depolanan dosyaları, oluşturulan dizinleri ve yüklenen uygulamaları içerir.
>
>

---



### <a name="microsoft-azure-storage-resiliency"></a><a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Depolama esnekliği
Microsoft Azure Depolama, işletim sistemi ve bağlı diskler veya lekeler içeren temel VHD'yi en az üç ayrı depolama düğümünde saklar. Bu depolama türüne yerel olarak yedekli depolama (LRS) adı verilir. LRS, Azure'daki tüm depolama türleri için varsayılan değerdir.

Başka fazlalık yöntemleri de var. Daha fazla bilgi için Azure [Depolama çoğaltma'ya](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)bakın.

> [!NOTE]
>Premium depolama, veritabanını depolayan ve dosyaları günlüğe kaydeden ve yeniden yapan DBMS VM'leri ve diskleri için önerilen depolama türüdür. Premium depolama için kullanılabilen tek artıklık yöntemi LRS'dir. Sonuç olarak, veritabanı veri çoğaltmayı başka bir Azure bölgesine veya kullanılabilirlik bölgesine etkinleştirmek için veritabanı yöntemlerini yapılandırmanız gerekir. Veritabanı yöntemleri arasında SQL Server Always Open, Oracle Data Guard ve HANA System Replication yer almaktadır.


> [!NOTE]
> DBMS dağıtımları için, coğrafi yedekli depolama (GRS) kullanımı standart depolama için önerilmez. GRS performansı ciddi şekilde etkiler ve VM'ye bağlı farklı VHD'ler arasında yazma sırasını onurlandırmaz. Farklı VHD'ler arasında yazma sırası onurlandırma potansiyel çoğaltma hedef tarafında tutarsız veritabanlarına yol açar. Bu durum, veritabanı ve günlük ve redo dosyaları kaynak VM tarafında, genellikle olduğu gibi birden çok VHD'ye yayılırsa oluşur.



## <a name="vm-node-resiliency"></a>VM düğümü esnekliği
Azure, VM'ler için birkaç farklı SLA sunar. Daha fazla bilgi için, [Sanal Makineler için SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/)en son sürümü bakın. DBMS katmanı genellikle sap sisteminde kullanılabilirlik açısından kritik olduğundan, kullanılabilirlik kümelerini, kullanılabilirlik bölgelerini ve bakım olaylarını anlamanız gerekir. Bu kavramlar hakkında daha fazla bilgi [için](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) bkz. [Manage the availability of Linux virtual machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability)

SAP iş yükü olan üretim DBMS senaryoları için minimum öneri şudur:

- Aynı Azure bölgesinde ayrı bir kullanılabilirlik kümesine iki VM dağıtın.
- Bu iki VM'yi aynı Azure sanal ağında çalıştırın ve aynı alt ağlardan NIC'ler eklenmiş olabilir.
- İkinci VM ile sıcak beklemede kalmak için veritabanı yöntemlerini kullanın. Yöntemler SQL Server Always Open, Oracle Data Guard veya HANA System Replication olabilir.

Ayrıca, başka bir Azure bölgesinde üçüncü bir VM dağıtabilir ve başka bir Azure bölgesinde eşzamanlı yineleme sağlamak için aynı veritabanı yöntemlerini kullanabilirsiniz.

Azure kullanılabilirlik kümelerini nasıl ayarlayıp ayarlayabilirsiniz hakkında bilgi için [bu öğreticiye](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets)bakın.



## <a name="azure-network-considerations"></a>Azure ağ hususları
Büyük ölçekli SAP dağıtımlarında [Azure Sanal Veri Merkezi'nin](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter)planını kullanın. Sanal ağ yapılandırmanız ve kuruluşunuzun farklı bölümlerine yönelik izinleriniz ve rol atamalarınız için kullanın.

Bu en iyi uygulamalar, yüzlerce müşteri dağıtımının sonucu:

- SAP uygulamasının dağıtılan sanal ağların internet erişimi yoktur.
- Veritabanı VM'leri uygulama katmanıyla aynı sanal ağda çalışır.
- Sanal ağ daki VM'ler özel IP adresinin statik bir dağılımına sahiptir. Daha fazla bilgi için [Azure'daki IP adres türlerine ve ayırma yöntemlerine](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)bakın.
- Yerel DBMS VM'lerine ve DBMS VM'lerine yönlendirme kısıtlamaları, güvenlik duvarlarıyla *ayarlanmaz.* Bunun yerine, trafik yönlendirme [ağ güvenlik grupları (NSGs)](https://docs.microsoft.com/azure/virtual-network/security-overview)ile tanımlanır.
- Trafiği DBMS VM'ye ayırmak ve yalıtmak için VM'ye farklı NIC'ler atayın. Her NIC farklı bir IP adresi alır ve her NIC farklı bir sanal ağ alt ağına atanır. Her alt net farklı NSG kuralları vardır. Ağ trafiğinin yalıtımı veya ayrılması yönlendirme için bir ölçüdür. Ağ iş birliş için kota ayarlamak için kullanılmaz.

> [!NOTE]
> Statik IP adreslerini Azure üzerinden atamak, bunları tek tek sanal NIC'lere atamak anlamına gelir. Konuk işletim sistemi içindeki statik IP adreslerini sanal bir NIC'e atayın. Azure Yedekleme gibi bazı Azure hizmetleri, en azından birincil sanal NIC'nin statik IP adreslerine göre değil, DHCP olarak ayarlanmış olmasına dayanır. Daha fazla bilgi için [Sorun Giderme Azure sanal makine](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking)yedeklemesi'ne bakın. Bir VM'ye birden çok statik IP adresi atamak için, bir VM'ye birden çok sanal NIC atayın.
>


> [!IMPORTANT]
> SAP uygulaması ile SAP NetWeaver, Hybris veya S/4HANA tabanlı SAP sisteminin DBMS katmanı arasındaki iletişim [yolundaki ağ sanal cihazlarının](https://azure.microsoft.com/solutions/network-appliances/) yapılandırılması desteklenmez. Bu kısıtlama işlevsellik ve performans nedenleriyle dir. SAP uygulama katmanı ile DBMS katmanı arasındaki iletişim yolu doğrudan olmalıdır. Bu ASG ve NSG kuralları doğrudan iletişim yoluna izin verirse, kısıtlama [uygulama güvenlik grubu (ASG) ve NSG kurallarını](https://docs.microsoft.com/azure/virtual-network/security-overview) içermez. 
>
> Ağ sanal cihazlarının desteklenmediği diğer senaryolar şunlardır:
>
> * SUSE Linux Enterprise Server for [SAP Applications'daki Azure VM'lerde SAP NetWeaver için Yüksek kullanılabilirlikte](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse)açıklandığı şekilde Linux Pacemaker küme düğümlerini temsil eden Azure VM'leri ile SBD aygıtları arasındaki iletişim yolları.
> * Azure VM'leri ile Windows Server Scale Out Dosya Sunucusu (SOFS) arasındaki iletişim yolları, Cluster'da açıklandığı gibi [bir SAP ASCS/SCS örneğinde Windows failover kümesinde Azure'da bir dosya paylaşımı kullanarak](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share)ayarlanır. 
>
> İletişim yollarında ağ sanal cihazlar kolayca iki iletişim ortakları arasında ağ gecikme iki katına çıkarabilirsiniz. Sap uygulama katmanı ile DBMS katmanı arasındaki kritik yollarda iş elde ini de kısıtlayabilirler. Bazı müşteri senaryolarında, ağ sanal cihazları Pacemaker Linux kümelerinin başarısız lığa neden olabilir. Bunlar, Linux Pacemaker küme düğümleri arasındaki iletişimin bir ağ sanal cihazı aracılığıyla SBD aygıtlarıyla iletişim kurduğu durumlardır.
>

> [!IMPORTANT]
> *Desteklenmeyen* bir diğer tasarım da SAP uygulama katmanı nın ve DBMS katmanının birbirinden [farklı](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağlarına ayrılmasıdır. Sap uygulama katmanını ve DBMS katmanını farklı Azure sanal ağları kullanmak yerine bir Azure sanal ağındaalt ağları kullanarak ayırmanızı öneririz. 
>
> Öneriye uymamaya ve bunun yerine iki katmanı farklı sanal ağlara ayırmamaya karar verirseniz, iki sanal ağa [bakılması](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)gerekir. 
>
> [İki bakan](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Azure sanal ağı arasındaki ağ trafiğinin aktarım maliyetlerine tabi olduğunu unutmayın. Sap uygulama katmanı ile DBMS katmanı arasında birçok terabayttan oluşan büyük veri hacmi değiştirilir. SAP uygulama katmanı ve DBMS katmanı iki bakan Azure sanal ağı arasında ayrılmışsa önemli maliyetler biriktirebilirsiniz.

Bir Azure kullanılabilirlik kümesi içinde üretim DBMS dağıtımınız için iki VM kullanın. Ayrıca SAP uygulama katmanı ve iki DBMS VM'ye giden yönetim ve operasyon trafiği için ayrı yönlendirme kullanın. Aşağıdaki resme bakın:

![İki alt ağda iki VM diyagramı](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="use-azure-load-balancer-to-redirect-traffic"></a>Trafiği yeniden yönlendirmek için Azure Yük Bakiyesi'ni kullanma
SQL Server Always On veya HANA System Replication gibi işlevlerde kullanılan özel sanal IP adreslerinin kullanılması için Azure yük dengeleyicisinin yapılandırması gerekmektedir. Yük dengeleyici, etkin DBMS düğümlerini belirlemek ve trafiği yalnızca bu etkin veritabanı düğümüne yönlendirmek için sonda bağlantı noktalarını kullanır. 

Veritabanı düğümünde bir hata varsa, SAP uygulamasının yeniden yapılandırmasına gerek yoktur. Bunun yerine, en yaygın SAP uygulama mimarileri özel sanal IP adresine karşı yeniden bağlanır. Bu arada, yük dengeleyicisi, trafiği özel sanal IP adresine ikinci düğüme yönlendirerek düğüm arızasına tepki verir.

Azure iki farklı [yük dengeleyici SKU'su](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)sunar: temel bir SKU ve standart bir SKU. Azure kullanılabilirlik bölgeleri arasında dağıtmak istemiyorsanız, temel yük dengeleyicisi SKU iyi olur.

DBMS VM'ler ve SAP uygulama katmanı arasındaki trafik her zaman yük dengeleyicisi üzerinden yönlendirilir mi? Yanıt, yük dengeleyicisini nasıl yapılandırdığınıza bağlıdır. 

Şu anda, DBMS VM'ye gelen trafik her zaman yük dengeleyicisi üzerinden yönlendirilir. DBMS VM'den uygulama katmanı VM'ye giden trafik rotası yük dengeleyicisinin yapılandırmasına bağlıdır. 

Yük dengeleyicisi DirectServerReturn seçeneği sunar. Bu seçenek yapılandırılırsa, DBMS VM'den SAP uygulama katmanına *not* yönlendirilen trafik yük dengeleyicisi aracılığıyla yönlendirilir. Bunun yerine, doğrudan uygulama katmanına gider. DirectServerReturn yapılandırılmamışsa, SAP uygulama katmanına dönüş trafiği yük bakiyesi üzerinden yönlendirilir.

DirectServerReturn'i SAP uygulama katmanı ile DBMS katmanı arasında konumlandırılmış yük dengeleyicileriyle birlikte yapılandırmanızı öneririz. Bu yapılandırma, iki katman arasındaki ağ gecikmesini azaltır.

Bu yapılandırmayı SQL Server Always On ile nasıl ayarleyeceğiniz hakkında bir örnek için, [Azure'daki Her Zaman Kullanılabilirlik Grupları için Bir ILB dinleyicisini Yapılandır'a](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener)bakın.

Azure'daki SAP altyapı dağıtımlarınız için başvuru olarak yayımlanmış GitHub JSON şablonlarını kullanıyorsanız, sap 3 katmanlı bir sistem için bu [şablonu](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md)inceleyin. Bu şablonda, yük dengeleyicisi için doğru ayarları da görebilirsiniz.

### <a name="azure-accelerated-networking"></a>Azure Hızlandırılmış Ağ
Azure VM'ler arasındaki ağ gecikmesini daha da azaltmak için [Azure Hızlandırılmış Ağ'ı](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/)seçmenizi öneririz. Özellikle SAP uygulama katmanı ve SAP DBMS katmanı için Bir SAP iş yükü için Azure VM'lerini dağıtırken kullanın.

> [!NOTE]
> Tüm VM türleri Hızlandırılmış Ağ'ı desteklemez. Önceki makalede, Hızlandırılmış Ağ'ı destekleyen VM türleri listelenmiştir.
>

---
> ![Windows][Logo_Windows] Windows
>
> Windows için Hızlandırılmış Ağ ile VM'leri nasıl dağıtılayabilirsiniz öğrenmek için [bkz.](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell)
>
> ![Linux][Logo_Linux] Linux
>
> Linux dağıtımı hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)
>
>

---

> [!NOTE]
> SUSE, Red Hat ve Oracle Linux durumunda, Hızlandırılmış Ağ son sürümleri ile desteklenir. SLES 12 SP2 veya RHEL 7.2 gibi eski sürümler Azure Hızlandırılmış Ağ'ı desteklemez.
>


## <a name="deployment-of-host-monitoring"></a>Ana bilgisayar izleme dağıtımı
SAP uygulamalarının Azure sanal makinelerinde üretim kullanımı için SAP, Azure sanal makinelerini çalıştıran fiziksel ana bilgisayarlardan ana bilgisayar izleme verilerini alma özelliğine ihtiyaç durabilir. SAPOSCOL ve SAP Ana Bilgisayar Aracısı'nda bu yeteneği sağlayan belirli bir SAP Ana Bilgisayar Aracısı yama düzeyi gereklidir. Tam yama düzeyi SAP Note [1409604'te]belgelenmiştir.

SAPOSCOL ve SAP Ana Bilgisayar Aracısı'na ana bilgisayar verilerini teslim eden bileşenlerin dağıtımı ve bu bileşenlerin yaşam döngüsü yönetimi hakkında daha fazla bilgi için [Dağıtım kılavuzuna][deployment-guide]bakın.


## <a name="next-steps"></a>Sonraki adımlar
Belirli bir DBMS hakkında daha fazla bilgi için bkz:

- [SAP iş yükü için SQL Server Azure Sanal Makineler DBMS dağıtımı](dbms_guide_sqlserver.md)
- [SAP iş yükü için Oracle Azure Sanal Makineler DBMS dağıtımı](dbms_guide_oracle.md)
- [SAP iş yükü için IBM DB2 Azure Sanal Makineler DBMS dağıtımı](dbms_guide_ibm.md)
- [SAP iş yükü için SAP ASE Azure Sanal Makineler DBMS dağıtımı](dbms_guide_sapase.md)
- [Azure'da SAP maxDB, Live Cache ve Content Server dağıtımı](dbms_guide_maxdb.md)
- [Azure işlemlerinde SAP HANA kılavuzu](hana-vm-operations.md)
- [Azure sanal makineleri için SAP HANA yüksek kullanılabilirlik](sap-hana-availability-overview.md)
- [Azure sanal makinelerde SAP HANA için yedekleme kılavuzu](sap-hana-backup-guide.md)

