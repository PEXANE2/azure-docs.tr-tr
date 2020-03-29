---
title: Azure VM için sistem yeniden başlatmayı anlama | Microsoft Dokümanlar
description: VM'nin yeniden başlatılmasına neden olabilecek olayları listeler
services: virtual-machines
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: e94ffb3d34082745c3d7ca86cfda2b93c0ed08da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919422"
---
# <a name="understand-a-system-reboot-for-azure-vm"></a>Azure VM için bir sistem yeniden başlatmayı anlama

Azure sanal makineleri (VM'ler), yeniden başlatma işlemini başlattığınıza dair kanıt olmadan bazen hiçbir neden olmaksızın yeniden başlatabilir. Bu makalede, VM'lerin yeniden başlatılmasına neden olabilecek eylemleri ve olayları listeler ve beklenmeyen yeniden başlatma sorunlarını nasıl önleyebileceğimiz veya bu tür sorunların etkisini nasıl azaltabileceğinizhakkında bilgi sağlar.

## <a name="configure-the-vms-for-high-availability"></a>Yüksek kullanılabilirlik için VM'leri yapılandırın

Azure'da çalışan bir uygulamayı VM yeniden başlatmalarına ve kapalı kalma sürelerine karşı korumanın en iyi yolu, yüksek kullanılabilirlik için VM'leri yapılandırmaktır.

Uygulamanıza bu fazlalık düzeyini sağlamak için, bir kullanılabilirlik kümesinde iki veya daha fazla VM gruplandırmanızı öneririz. Bu yapılandırma, planlı veya planlanmamış bir bakım olayı sırasında en az bir VM kullanılabilir olmasını ve yüzde 99,95 [Azure SLA'sını](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/)karşılamasını sağlar.

Kullanılabilirlik kümeleri hakkında daha fazla bilgi için [bkz.](../windows/manage-availability.md)

## <a name="resource-health-information"></a>Kaynak Durumu bilgileri

Azure Kaynak Durumu, tek tek Azure kaynaklarının sistem durumunu ortaya çıkaran ve sorun giderme sorunları için eyleme geçirilebilir yönergeler sağlayan bir hizmettir. Sunuculara veya altyapı öğelerine doğrudan erişilemediği bir bulut ortamında, Kaynak Durumu'nun amacı sorun gidermeye harcadığınız zamanı azaltmaktır. Özellikle amaç, sorunun kaynağının uygulamada mı yoksa Azure platformundaki bir etkinlikte mi olduğunu belirlemek için harcadığınız zamanı azaltmaktır. Daha fazla bilgi için Kaynak [Durumu'nun Anlamasını ve kullanımını](../../resource-health/resource-health-overview.md)görün.

## <a name="actions-and-events-that-can-cause-the-vm-to-reboot"></a>VM'nin yeniden başlatılmasına neden olabilecek eylemler ve olaylar

### <a name="planned-maintenance"></a>Planlı bakım

Microsoft Azure, VM'lerin altında yatan ana bilgisayar altyapısının güvenilirliğini, performansını ve güvenliğini artırmak için düzenli aralıklarla dünya çapında güncelleştirmeler gerçekleştirir. Bellek koruma güncelleştirmeleri de dahil olmak üzere bu güncelleştirmelerin çoğu, VM'leriniz veya bulut hizmetleriniz üzerinde herhangi bir etkisi olmadan gerçekleştirilir.

Ancak, bazı güncelleştirmeler yeniden başlatma gerektirir. Bu gibi durumlarda, biz altyapıyı yamalarken VM'ler kapatılır ve sonra VM'ler yeniden başlatılır.

Azure'un planladığı bakımın ne olduğunu ve Linux VM'lerinizin kullanılabilirliğini nasıl etkileyebileceğini anlamak için burada listelenen makalelere bakın. Makaleler Azure planlı bakım işlemi ve etkiyi daha da azaltmak için planlı bakımın nasıl zamanlanacağı hakkında arka plan bilgileri sağlar.

- [Azure’da VM’ler için planlı bakım](../windows/planned-maintenance.md)
- [Azure sanal makinelerinde planlı bakımı zamanlama](../windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Bellek koruma güncelleştirmeleri

Microsoft Azure'daki bu güncelleştirme sınıfı için, kullanıcılar çalışan VM'leri üzerinde hiçbir etki yaşamaz. Bu güncelleştirmelerin çoğu, çalışan örneği engellemeden güncelleştirilebilecek bileşenlere veya hizmetler içindir. Bazıları, VM'lerin yeniden başlatılması olmadan uygulanabilen ana bilgisayar işletim sistemindeki platform altyapısı güncelleştirmeleridir.

Bu bellek koruma güncelleştirmeleri, yerinde dinamik geçiş olanağı sağlayan teknolojiyle gerçekleştirilir. Güncelleştirildiğinde, VM *duraklatılmış* bir duruma yerleştirilir. Bu durum RAM belleğini korur ve bu arada alttaki konak işletim sistemi gerekli güncelleştirmeleri ve düzeltme eklerini alır. VM duraklatıldıktan sonra 30 saniye içinde devam ettirilir. VM devam ettirildikten sonra saati otomatik olarak eşitlenir.

Kısa duraklama süresi nedeniyle, güncelleştirmeleri bu mekanizma üzerinden dağıtmak VM'ler üzerindeki etkisini büyük ölçüde azaltır. Ancak, tüm güncelleştirmeler bu şekilde dağıtılamaz. 

Çok örnekli güncelleştirmeler (bir kullanılabilirlik kümesindeki VM'ler için), bir seferde bir güncelleme etki alanı şeklinde uygulanır.

> [!NOTE]
> Eski çekirdek sürümlerine sahip Linux makineleri bu güncelleştirme yöntemi sırasında çekirdek paniğinden etkilenir. Bu sorunu önlemek için çekirdek sürüm 3.10.0-327.10.1 veya sonraki sürümlere güncelleştirin. Daha fazla bilgi için, [ana bilgisayar düğümü yükseltmeden sonra 3,10 tabanlı çekirdek teazure Linux VM'ye](https://support.microsoft.com/help/3212236)bakın.

### <a name="user-initiated-reboot-or-shutdown-actions"></a>Kullanıcı tarafından başlatılan yeniden başlatma veya kapatma eylemleri

Azure portalı, Azure PowerShell, komut satırı arabirimi veya REST API'den yeniden başlatma yaparsanız, olayı [Azure Etkinlik Günlüğü'nde](../../azure-monitor/platform/platform-logs-overview.md)bulabilirsiniz.

Eylemi VM'nin işletim sisteminden gerçekleştirirseniz, olayı sistem günlüklerinde bulabilirsiniz.

Genellikle VM'nin yeniden başlatılmasına neden olan diğer senaryolar, birden çok yapılandırma değiştirme eylemi içerir. Normalde belirli bir eylemi yürütmenin VM'nin yeniden başlatılmasıyla sonuçolacağını belirten bir uyarı iletisi görürsünüz. Örnekler arasında VM yeniden boyutlandırma işlemleri, yönetim hesabının parolasını değiştirme ve statik bir IP adresi ayarlama sayılabilir.

### <a name="azure-security-center-and-windows-update"></a>Azure Güvenlik Merkezi ve Windows Güncelleştirmesi

Azure Güvenlik Merkezi, işletim sistemi güncelleştirmelerinin eksik olması için günlük Windows ve Linux VM'lerini izler. Güvenlik Merkezi, hangi hizmetin Windows VM'de yapılandırıldığına bağlı olarak, Windows Update veya Windows Server Update Services'ten (WSUS) kullanılabilir güvenlik ve kritik güncelleştirmelerin bir listesini alır. Güvenlik Merkezi ayrıca Linux sistemleri için en son güncellemeleri denetler. VM'nizde bir sistem güncelleştirmesi yoksa, Güvenlik Merkezi sistem güncelleştirmelerini uygulamanızı önerir. Bu sistem güncelleştirmelerinin uygulaması Azure portalındaki Güvenlik Merkezi aracılığıyla denetlenir. Bazı güncelleştirmeleri uyguladıktan sonra VM yeniden başlatması gerekebilir. Daha fazla bilgi için Azure [Güvenlik Merkezi'nde sistem güncelleştirmelerini uygula'ya](../../security-center/security-center-apply-system-updates.md)bakın.

Şirket içi sunucular gibi Azure da Windows Update'ten Windows VM'lerine güncelleştirmeler yüklemez, çünkü bu makineler kullanıcıları tarafından yönetilecek şekilde tasarlanmıştır. Ancak, otomatik Windows Update ayarını etkin bırakmanız için teşvik edilirsiniz. Windows Update güncelleştirmelerinin otomatik olarak yüklenmesi, güncelleştirmeler uygulandıktan sonra yeniden başlatmaların oluşmasına da neden olabilir. Daha fazla bilgi için [Windows Update SSS'ye](https://support.microsoft.com/help/12373/windows-update-faq)bakın.

### <a name="other-situations-affecting-the-availability-of-your-vm"></a>VM'nizin kullanılabilirliğini etkileyen diğer durumlar

Azure'un bir VM kullanımını etkin olarak askıya alabileceği başka durumlar da vardır. Bu işlem alınmadan önce e-posta bildirimleri alırsınız, böylece altta yatan sorunları çözme şansınız olur. VM kullanılabilirliğini etkileyen sorunlara örnek olarak güvenlik ihlalleri ve ödeme yöntemlerinin sona ermesi verilebilir.

### <a name="host-server-faults"></a>Ana bilgisayar sunucusu arızaları

VM, Azure veri merkezi içinde çalışan fiziksel bir sunucuda barındırılır. Fiziksel sunucu, birkaç diğer Azure bileşenine ek olarak Ana Bilgisayar Aracısı adlı bir aracı çalıştırın. Fiziksel sunucudaki bu Azure yazılım bileşenleri yanıt vermediğinde, izleme sistemi kurtarma girişiminde bulunmak için ana bilgisayar sunucusunun yeniden başlatılmasını tetikler. VM genellikle beş dakika içinde tekrar kullanılabilir ve daha önce aynı ana bilgisayarda yaşamaya devam ediyor.

Sunucu hataları genellikle sabit disk veya katı hal sürücüsünün arızası gibi donanım hatasından kaynaklanır. Azure bu oluşumları sürekli olarak izler, temel hataları tanımlar ve azaltma uygulandıktan ve test edildikten sonra güncelleştirmeleri dışarı çıkarır.

Bazı ana bilgisayar sunucusu hataları bu sunucuya özgü olabileceğinden, vm'yi başka bir ana bilgisayar sunucusuna el ile yeniden dağıtarak yinelenen vm yeniden başlatma durumu iyileştirilmiş olabilir. Bu işlem, VM'nin ayrıntılar sayfasındayeniden **dağıtseçeneğikullanılarak** veya Azure portalında VM'yi durdurup yeniden başlatarak tetiklenebilir.

### <a name="auto-recovery"></a>Otomatik kurtarma

Ana bilgisayar sunucusu herhangi bir nedenle yeniden başlatamazsa, Azure platformu hatalı ana bilgisayar sunucusunu daha fazla araştırma için döndürme dışında almak için bir otomatik kurtarma eylemi başlatır. 

Bu ana bilgisayardaki tüm VM'ler otomatik olarak farklı, sağlıklı bir ana bilgisayar sunucusuna taşınır. Bu işlem genellikle 15 dakika içinde tamamlanır. Otomatik kurtarma işlemi hakkında daha fazla bilgi edinmek için [VM'lerin otomatik olarak kurtarılmasına](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines)bakın.

### <a name="unplanned-maintenance"></a>Plansız bakım

Nadir durumlarda, Azure operasyon ekibinin Azure platformunun genel durumunu sağlamak için bakım etkinlikleri gerçekleştirmesi gerekebilir. Bu davranış VM kullanılabilirliğini etkileyebilir ve genellikle daha önce açıklandığı gibi aynı otomatik kurtarma eylemi ile sonuçlanır.  

Planlanmamış bakım aşağıdakileri içerir:

- Acil düğüm parçalanması
- Acil ağ anahtarı güncellemeleri

### <a name="vm-crashes"></a>VM çöküyor

VM'ler, VM'nin kendisindeki sorunlar nedeniyle yeniden başlatılabilir. VM'de çalışan iş yükü veya rol, konuk işletim sisteminde bir hata denetimini tetikleyebilir. Çökme nin nedenini belirlemeye yardımcı olmak için, Windows VM'leri için sistem ve uygulama günlüklerini ve Linux VM'leri için seri günlüklerini görüntüleyin.

### <a name="storage-related-forced-shutdowns"></a>Depolama ile ilgili zorla kapatma işlemleri

Azure'daki SANAL M'ler, azure depolama altyapısında barındırılan işletim sistemi ve veri depolama için sanal disklere güvenir. VM ile ilişkili sanal diskler arasındaki kullanılabilirlik veya bağlantı 120 saniyeden uzun süre etkilendiğinde, Azure platformu veri bozulmasını önlemek için VM'lerin zorla kapatılmasını gerçekleştirir. Depolama bağlantısı geri yüklendikten sonra VM'ler otomatik olarak yeniden çalıştırılır. 

Kapatma süresi beş dakika kadar kısa olabilir, ancak önemli ölçüde daha uzun olabilir. Aşağıda, depolamayla ilgili zorla kapatmalarla ilişkili belirli durumlardan biri vereme aşağıdakiler yer alan durumlar şunlardır: 

**IO limitlerini aşma**

Saniyede G/Ç işlemlerinin hacmi (IOPS) diskin G/Ç sınırlarını aştığından, G/Ç istekleri sürekli olarak azaltıldığında VM'ler geçici olarak kapatılabilir. (Standart disk depolama 500 IOPS ile sınırlıdır.) Bu sorunu azaltmak için, iş yüküne bağlı olarak disk şeridi kullanın veya konuk VM içindeki depolama alanını yapılandırın. Ayrıntılar için, [En İyi Depolama Performansı için Azure VM'lerini Yapılandırma'ya](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)bakın.

### <a name="other-incidents"></a>Diğer olaylar

Nadir durumlarda, yaygın bir sorun bir Azure veri merkezindeki birden çok sunucuyu etkileyebilir. Bu sorun oluşursa, Azure ekibi etkilenen aboneliklere e-posta bildirimleri gönderir. Devam eden kesintilerin ve geçmiş olayların durumuna karşı [Azure Hizmet Durumu panosunu](https://azure.microsoft.com/status/) ve Azure portalını kontrol edebilirsiniz.
