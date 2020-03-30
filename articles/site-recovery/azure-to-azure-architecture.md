---
title: Azure'dan Azure'a olağanüstü durum kurtarma mimarisi Azure Site Kurtarma'da
description: Azure Site Kurtarma hizmetini kullanarak Azure VM'leri için Azure bölgeleri arasında olağanüstü durum kurtarma ayarlarken kullanılan mimariye genel bakış.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 3/13/2020
ms.author: raynew
ms.openlocfilehash: 94da1639b5398a03b36fba3ff88877468a97ec36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294109"
---
# <a name="azure-to-azure-disaster-recovery-architecture"></a>Azure'dan Azure'a olağanüstü durum kurtarma mimarisi


Bu makalede, [Azure Site Kurtarma](site-recovery-overview.md) hizmetini kullanarak Azure sanal makineleri (VM) için olağanüstü durum kurtarma dağıtırken kullanılan mimari, bileşenler ve işlemler açıklanmaktadır. Olağanüstü durum kurtarma kurulumuyla, Azure VM'leri sürekli olarak farklı bir hedef bölgeye çoğaltır. Bir kesinti oluşursa, ikincil bölgeye VM üzerinden başarısız olabilir ve bu nedenle bunlara erişebilirsiniz. Her şey normal şekilde yeniden çalıştığında, geri başarısız olabilir ve birincil konumda çalışmaya devam edebilirsiniz.



## <a name="architectural-components"></a>Mimari bileşenler

Azure Sanal Taşıtları için olağanüstü durum kurtarma yla ilgili bileşenler aşağıdaki tabloda özetlenmiştir.

**Bileşen** | **Gereksinimler**
--- | ---
**Kaynak bölgedeki VM'ler** | [Desteklenen kaynak bölgedeki](azure-to-azure-support-matrix.md#region-support)azure vm'lerinden biri daha.<br/><br/> VM'ler desteklenen herhangi bir [işletim sistemini](azure-to-azure-support-matrix.md#replicated-machine-operating-systems)çalıştırıyor olabilir.
**Kaynak VM depolama** | Azure VM'leri yönetilebilir veya yönetilmeyen disklerin depolama hesaplarına yayılmasına neden olabilir.<br/><br/>Desteklenen Azure depolama [alanı hakkında bilgi edinin.](azure-to-azure-support-matrix.md#replicated-machines---storage)
**Kaynak VM ağları** | VM'ler kaynak bölgede bir sanal ağda (VNet) bir veya daha fazla alt ağda bulunabilir. Ağ gereksinimleri hakkında [daha fazla bilgi edinin.](azure-to-azure-support-matrix.md#replicated-machines---networking)
**Önbellek depolama hesabı** | Kaynak ağda bir önbellek depolama hesabına ihtiyacınız var. Çoğaltma sırasında, VM değişiklikleri hedef depolamaya gönderilmeden önce önbellekte depolanır.  Önbellek depolama hesapları Standart olmalıdır.<br/><br/> Önbellek kullanmak, VM üzerinde çalışan üretim uygulamaları üzerinde en az etkiyi sağlar.<br/><br/> Önbellek depolama gereksinimleri hakkında [daha fazla bilgi edinin.](azure-to-azure-support-matrix.md#cache-storage) 
**Hedef kaynakları** | Hedef kaynaklar çoğaltma sırasında ve bir hata oluştuğunda kullanılır. Site Kurtarma varsayılan olarak hedef kaynağı ayarlayabilir veya bunları oluşturabilir/özelleştirebilirsiniz.<br/><br/> Hedef bölgede, VM'ler oluşturup oluşturabildiğinizi ve aboneliğinizin hedef bölgede ihtiyaç duyulacak VM boyutlarını desteklemek için yeterli kaynağa sahip olup olmadığını denetleyin. 

![Kaynak ve hedef çoğaltma](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

## <a name="target-resources"></a>Hedef kaynakları

Bir VM için çoğaltmayı etkinleştirdiğinizde, Site Kurtarma otomatik olarak hedef kaynakları oluşturma seçeneği verir. 

**Hedef kaynak** | **Varsayılan ayar**
--- | ---
**Hedef abonelik** | Kaynak abonelikle aynı.
**Hedef kaynak grubu** | Başarısız olduktan sonra VM'lerin ait olduğu kaynak grubu.<br/><br/> Kaynak bölge dışında herhangi bir Azure bölgesinde olabilir.<br/><br/> Site Kurtarma hedef bölgede "asr" soneki ile yeni bir kaynak grubu oluşturur.<br/><br/>
**Hedef VNet** | Çoğaltılan VM'lerin başarısız olduktan sonra bulunduğu sanal ağ (VNet). Kaynak ve hedef sanal ağlar arasında bir ağ eşleme oluşturulur ve bunun tersi de vardır.<br/><br/> Site Kurtarma yeni bir VNet ve alt ağ oluşturur, "asr" soneki ile.
**Hedef depolama hesabı** |  VM yönetilen bir disk kullanmıyorsa, bu verilerin çoğaltıldığı depolama hesabıdır.<br/><br/> Site Kurtarma, kaynak depolama hesabını yansıtmak için hedef bölgede yeni bir depolama hesabı oluşturur.
**Yineleme yönetilen diskler** | VM yönetilen bir disk kullanıyorsa, bu, verilerin çoğaltıldığı yönetilen disklertir.<br/><br/> Site Kurtarma, kaynağı yansıtmak için depolama bölgesinde yineleme yönetilen diskler oluşturur.
**Hedef kullanılabilirlik kümeleri** |  Başarısız olduktan sonra çoğaltma VM'lerinin bulunduğu kullanılabilirlik kümesi.<br/><br/> Site Kurtarma, kaynak konumda ayarlanan bir kullanılabilirlik kümesinde bulunan VM'ler için hedef bölgede "asr" sonekiyle bir kullanılabilirlik kümesi oluşturur. Kullanılabilirlik kümesi varsa, kullanılır ve yeni si oluşturulmaz.
**Hedef kullanılabilirlik bölgeleri** | Hedef bölge kullanılabilirlik bölgelerini destekliyorsa, Site Kurtarma kaynak bölgede kullanılan la aynı bölge numarasını atar.

### <a name="managing-target-resources"></a>Hedef kaynakları yönetme

Hedef kaynakları aşağıdaki gibi yönetebilirsiniz:

- Çoğaltmayı etkinleştirirken hedef ayarlarını değiştirebilirsiniz.
- Çoğaltma zaten çalışmaya başladıktan sonra hedef ayarlarını değiştirebilirsiniz. Özel durum kullanılabilirlik türüdür (tek örnek, küme veya bölge). Bu ayarı değiştirmek için çoğaltmayı devre dışı bırakıp, ayarı değiştirmeniz ve ardından yeniden etkinleştirmeniz gerekir.



## <a name="replication-policy"></a>Çoğaltma ilkesi 

Azure VM çoğaltmayı etkinleştirdiğinizde, varsayılan olarak Site Kurtarma tabloda özetlenen varsayılan ayarları içeren yeni bir çoğaltma ilkesi oluşturur.

**İlke ayarı** | **Şey** | **Varsayılan**
--- | --- | ---
**Kurtarma noktası tutma** | Site Kurtarma'nın kurtarma noktalarını ne kadar tuttuğunu belirtir | 24 saat
**Uygulama tutarlı anlık görüntü frekansı** | Site Kurtarma'nın uygulama tutarlı bir anlık görüntü alma sıklığı. | Her dört saatte bir

### <a name="managing-replication-policies"></a>Çoğaltma ilkelerini yönetme

Varsayılan çoğaltma ilkeleri ayarlarını aşağıdaki gibi yönetebilir ve değiştirebilirsiniz:
- Çoğaltmayı etkinleştirirken ayarları değiştirebilirsiniz.
- İstediğiniz zaman bir çoğaltma ilkesi oluşturabilir ve çoğaltmayı etkinleştirdiğinizde uygulayabilirsiniz.

### <a name="multi-vm-consistency"></a>Çoklu VM tutarlılığı

VM'lerin birlikte çoğaltılmasını istiyorsanız ve başarısız olan kilitlenme tutarlıve uygulama tutarlı kurtarma noktalarını paylaştıysanız, bunları bir çoğaltma grubunda bir araya getirebilirsiniz. Multi-VM tutarlılık iş yükü performansını etkiler ve yalnızca tüm makinelerde tutarlılık gerektiren iş yüklerini çalıştıran VM'ler için kullanılmalıdır. 



## <a name="snapshots-and-recovery-points"></a>Anlık görüntüler ve kurtarma noktaları

Kurtarma noktaları, belirli bir zamanda alınan VM disklerinin anlık görüntülerinden oluşturulur. Bir VM üzerinde başarısız olduğunuzda, hedef konumda VM geri yüklemek için bir kurtarma noktası kullanın.

Başarısız olduğunda, genellikle VM'nin herhangi bir bozulma veya veri kaybı olmadan başladığından ve VM verilerinin işletim sistemi ve VM'de çalışan uygulamalar için tutarlı olduğundan emin olmak isteriz. Bu, çekilen anlık görüntü türüne bağlıdır.

Site Kurtarma aşağıdaki gibi anlık alır:

1. Site Kurtarma, varsayılan olarak verilerin kilitlenme tutarlı anlık görüntülerini ve bunlar için bir sıklık belirtirseniz uygulama tutarlı anlık görüntüler alır.
2. Kurtarma noktaları anlık görüntülerden oluşturulur ve çoğaltma ilkesindeki bekletme ayarlarına göre depolanır.

### <a name="consistency"></a>Tutarlılık

Aşağıdaki tabloda farklı tutarlılık türleri açıklanmaktadır.

### <a name="crash-consistent"></a>Kilitlenme tutarlılığı

**Açıklama** | **Şey** | **Öneri**
--- | --- | ---
Kilitlenme tutarlı anlık görüntü, anlık görüntü alındığında diskteki verileri yakalar. Hafızada hiçbir şey yok.<br/><br/> VM kilitlenirse veya anlık görüntünün çekildiği anda güç kablosu sunucudan çekilirse, mevcut olacak disk üzerindeki verilerin eşdeğerini içerir.<br/><br/> Kilitlenme tutarlılığı, işletim sistemi veya VM'deki uygulamalar için veri tutarlılığını garanti etmez. | Site Kurtarma varsayılan olarak her beş dakikada bir kilitlenme tutarlı kurtarma noktaları oluşturur. Bu ayar değiştirilemez.<br/><br/>  | Günümüzde çoğu uygulama, kilitlenme tutarlı noktalarından iyi bir şekilde kurtulabilir.<br/><br/> Kilitlenme tutarlı kurtarma noktaları genellikle işletim sistemlerinin çoğaltılması ve DHCP sunucuları ve yazdırma sunucuları gibi uygulamalar için yeterlidir.

### <a name="app-consistent"></a>Uygulama tutarlılığı

**Açıklama** | **Şey** | **Öneri**
--- | --- | ---
Uygulama tutarlı kurtarma noktaları, uygulama tutarlı anlık görüntülerden oluşturulur.<br/><br/> Uygulama tutarlı anlık görüntü, kilitlenme tutarlı anlık görüntüdeki tüm bilgilerin yanı sıra bellekteki ve devam eden işlemlerdeki tüm verileri içerir. | Uygulama tutarlı anlık görüntüler, Birim Gölge Kopyalama Hizmeti'ni (VSS):<br/><br/>   1) Anlık görüntü başlatıldığında, VSS birim üzerinde bir kopya-on-write (COW) işlemi gerçekleştirir.<br/><br/>   2) COW'u gerçekleştirmeden önce VSS, makinedeki her uygulamayı bellekte yerleşik verilerini diske çekmesi için bilgilendirir.<br/><br/>   3) VSS sonra yedekleme / olağanüstü durum kurtarma uygulaması (bu durumda Site Kurtarma) anlık verileri okumak ve devam sağlar. | Uygulama tutarlı anlık görüntüler, belirttiğiniz frekansa göre alınır. Bu sıklık her zaman kurtarma noktalarını korumak için ayarladığınızdan daha az olmalıdır. Örneğin, varsayılan 24 saatlik ayarı kullanarak kurtarma noktalarını korursanız, sıklığı 24 saatten az olarak ayarlamanız gerekir.<br/><br/>Bunlar daha karmaşıktır ve çarpışma tutarlı anlık görüntülerden daha uzun sürer.<br/><br/> Çoğaltma için etkin leştirilmiş bir VM üzerinde çalışan uygulamaların performansını etkiler. 

## <a name="replication-process"></a>Çoğaltma işlemi

Bir Azure VM için çoğaltmayı etkinleştirdiğinizde aşağıdaki ler olur:

1. Site Kurtarma Mobilitehizmeti uzantısı VM'ye otomatik olarak yüklenir.
2. Uzantı, VM'yi Site Kurtarma ile kaydeder.
3. VM için sürekli çoğaltma başlar.  Disk yazmaları hemen kaynak konumdaki önbellek depolama hesabına aktarılır.
4. Site Kurtarma önbellekteki verileri işler ve hedef depolama hesabına veya çoğaltma yönetilen disklere gönderir.
5. Veriler işlendikten sonra, kilitlenme tutarlı kurtarma noktaları her beş dakikada bir oluşturulur. Uygulama tutarlı kurtarma noktaları çoğaltma ilkesinde belirtilen ayarına göre oluşturulur.

![Çoğaltma işlemini etkinleştirme, adım 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

**Çoğaltma işlemi**

## <a name="connectivity-requirements"></a>Bağlantı gereksinimleri

 Çoğaltdığınız Azure VM'ler için giden bağlantı gerekir. Site Kurtarma'nın VM'ye gelen bağlantıya asla ihtiyacı olmaz. 

### <a name="outbound-connectivity-urls"></a>Giden bağlantı (URL'ler)

VM'ler için giden erişim URL'lerle kontrol ediliyorsa, bu URL'lere izin verin.

| **URL** | **Şey** |
| ------- | ----------- |
| *.blob.core.windows.net | Verilerin VM’den kaynak bölgedeki önbellek depolama hesabına yazılmasına izin verir. |
| login.microsoftonline.com | Site Recovery hizmet URL’leri için yetkilendirme ve kimlik doğrulama özellikleri sağlar. |
| *.hypervrecoverymanager.windowsazure.com | VM’nin Site Recovery hizmetiyle iletişim kurmasına izin verir. |
| *.servicebus.windows.net | VM’nin Site Recovery izleme ve tanılama verilerini yazmasına izin verir. |
| *.vault.azure.net | Portal üzerinden ADE özellikli sanal makineler için çoğaltmayı etkinleştirme olanağı sağlar
| *.automation.ext.azure.com | Portal üzerinden çoğaltılan bir öğe için mobilite aracısının otomatik olarak yükseltilmesine olanak sağlar

### <a name="outbound-connectivity-for-ip-address-ranges"></a>IP adresi aralıkları için giden bağlantı

IP adreslerini kullanarak VM'ler için giden bağlantıyı denetlemek için bu adreslere izin verin.
Ağ bağlantısı gereksinimleriyle ilgili ayrıntıları ağ [beyaz kağıtlarında](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags) bulabileceğinizi lütfen unutmayın 

#### <a name="source-region-rules"></a>Kaynak bölge kuralları

**Kural** |  **Şey** | **Hizmet etiketi**
--- | --- | --- 
HTTPS giden izin ver: bağlantı noktası 443 | Kaynak bölgedeki depolama hesaplarına karşılık gelen aralıklara izin verme | Depolama. \<bölge adı>
HTTPS giden izin ver: bağlantı noktası 443 | Azure Etkin Dizin'e (Azure AD) karşılık gelen aralıklara izin verme  | AzureActiveDirectory
HTTPS giden izin ver: bağlantı noktası 443 | Hedef bölgedeki Olaylar Hub'ına karşılık gelen aralıklara izin verin. | EventsHub' da. \<bölge adı>
HTTPS giden izin ver: bağlantı noktası 443 | Azure Site Kurtarma'ya karşılık gelen aralıklara izin verme  | AzureSiteKurtarma
HTTPS giden izin ver: bağlantı noktası 443 | Azure Anahtar Kasası'na karşılık gelen aralıklara izin verin (Bu yalnızca portal üzerinden ADE özellikli sanal makinelerin çoğaltılmasını etkinleştirmek için gereklidir) | AzureKeyVault
HTTPS giden izin ver: bağlantı noktası 443 | Azure Otomasyon Denetleyicisine karşılık gelen aralıklara izin verin (Bu, yalnızca portal üzerinden çoğaltılan bir öğe için mobilite aracısının otomatik olarak yükseltilmesi için gereklidir) | GuestAndHybridManagement

#### <a name="target-region-rules"></a>Hedef bölge kuralları

**Kural** |  **Şey** | **Hizmet etiketi**
--- | --- | --- 
HTTPS giden izin ver: bağlantı noktası 443 | Hedef bölgedeki depolama hesaplarına karşılık gelen aralıklara izin verme | Depolama. \<bölge adı>
HTTPS giden izin ver: bağlantı noktası 443 | Azure AD'ye karşılık gelen aralıklara izin verme  | AzureActiveDirectory
HTTPS giden izin ver: bağlantı noktası 443 | Kaynak bölgedeki Olaylar Hub'ına karşılık gelen aralıklara izin verin. | EventsHub' da. \<bölge adı>
HTTPS giden izin ver: bağlantı noktası 443 | Azure Site Kurtarma'ya karşılık gelen aralıklara izin verme  | AzureSiteKurtarma
HTTPS giden izin ver: bağlantı noktası 443 | Azure Anahtar Kasası'na karşılık gelen aralıklara izin verin (Bu yalnızca portal üzerinden ADE özellikli sanal makinelerin çoğaltılmasını etkinleştirmek için gereklidir) | AzureKeyVault
HTTPS giden izin ver: bağlantı noktası 443 | Azure Otomasyon Denetleyicisine karşılık gelen aralıklara izin verin (Bu, yalnızca portal üzerinden çoğaltılan bir öğe için mobilite aracısının otomatik olarak yükseltilmesi için gereklidir) | GuestAndHybridManagement


#### <a name="control-access-with-nsg-rules"></a>NSG kurallarıyla erişimi kontrol edin

[NSG kurallarını](https://docs.microsoft.com/azure/virtual-network/security-overview)kullanarak ağ trafiğini Azure ağlarına/alt ağlarına filtreleyerek VM bağlantısını denetlerseniz, aşağıdaki gereksinimleri dikkate alabilirsiniz:

- Kaynak Azure bölgesi için NSG kuralları çoğaltma trafiği için giden erişime izin vermelidir.
- Bunları üretime sokmadan önce bir test ortamında kurallar oluşturmanızı öneririz.
- Tek tek IP adreslerine izin vermek yerine [hizmet etiketlerini](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) kullanın.
    - Hizmet etiketleri, güvenlik kuralları oluştururken karmaşıklığı en aza indirmek için bir araya getirilmiş bir grup IP adresi önekleri grubunu temsil eder.
    - Microsoft, servis etiketlerini zaman içinde otomatik olarak güncelleştirir. 
 
Site Kurtarma ve [NSG'lerle bağlantıyı denetleme](concepts-network-security-group-with-site-recovery.md)için [giden bağlantı](azure-to-azure-about-networking.md#outbound-connectivity-using-service-tags) hakkında daha fazla bilgi edinin.


### <a name="connectivity-for-multi-vm-consistency"></a>Çoklu VM tutarlılığı için bağlantı

Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar.
- VM’ler arasında 20004 numaralı bağlantı noktası üzerinden gerçekleştirilen iç iletişimi engelleyen bir güvenlik duvarı gereci olmadığından emin olun.
- Linux VM’lerinin çoğaltma grubunun bir parçası olmasını istiyorsanız, 20004 numaralı bağlantı noktası üzerinden giden trafiğin, belirli Linux sürümünün kılavuzuna göre el ile açıldığından emin olun.




## <a name="failover-process"></a>Başarısız işlem

Bir başarısızlık başlattığınızda, Sanal Kaynaklar Grubu'nda, hedef sanal ağda, hedef alt ağda ve hedef kullanılabilirlik kümesinde VM'ler oluşturulur. Bir hata sırasında, herhangi bir kurtarma noktası kullanabilirsiniz.

![Başarısız işlem](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure VM'yi ikinci bir bölgeye [hızla çoğaltın.](azure-to-azure-quickstart.md)
