---
title: Azure Site Kurtarma ile Citrix XenDesktop/XenApp olağanüstü kurtarma yı ayarlama
description: Bu makalede, Azure Site Kurtarma kullanarak Citrix XenDesktop ve XenApp dağıtımlarının nasıl ayarlanış yapılacağı açıklanmaktadır.
author: ponatara
manager: abhemraj
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ponatara
ms.openlocfilehash: 29fbe5389da924a2ecc660aa5ce5c4bb0a0902b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74084549"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-citrix-xenapp-and-xendesktop-deployment"></a>çok katmanlı Citrix XenApp ve XenDesktop dağıtımı için olağanüstü durum kurtarma ayarlama



Citrix XenDesktop, masaüstü bilgisayarları ve uygulamaları her yerde, her yerde isteğe bağlı hizmet olarak sunan bir masaüstü sanallaştırma çözümüdür. FlexCast dağıtım teknolojisi ile XenDesktop, kullanıcılara uygulamaları ve masaüstü bilgisayarları hızlı ve güvenli bir şekilde sunabilir.
Bugün, Citrix XenApp herhangi bir felaket kurtarma yetenekleri sağlamaz.

İyi bir olağanüstü durum kurtarma çözümü, yukarıdaki karmaşık uygulama mimarileri etrafında kurtarma planları modelleme izin vermeli ve aynı zamanda çeşitli katmanlar arasında uygulama eşlemeleri işlemek için özelleştirilmiş adımlar eklemek için yeteneği dolayısıyla tek bir tıklama sağlayan daha düşük bir RTO giden bir felaket durumunda emin atış çözüm.

Bu belge, Hyper-V ve VMware vSphere platformlarında şirket içi Citrix XenApp dağıtımlarınız için bir olağanüstü durum kurtarma çözümü oluşturmak için adım adım kılavuzluk sağlar. Bu belge, kurtarma planlarını, desteklenen yapılandırmaları ve ön koşulları kullanarak bir test başarısızlığı (olağanüstü durum kurtarma meddi) ve planlanmamış bir başarısızlığı azure'da nasıl gerçekleştirilemeyeceğinizi de açıklar.


## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakileri anladığınızdan emin olun:

1. [Sanal makineyi Azure'a çoğaltma](site-recovery-vmware-to-azure.md)
1. Kurtarma [ağı](site-recovery-network-design.md) nasıl tasarlar?
1. [Azure'da test başarısız olması](site-recovery-test-failover-to-azure.md)
1. [Azure'da başarısız olmak](site-recovery-failover.md)
1. Etki [alanı denetleyicisi nasıl çoğaltılır?](site-recovery-active-directory.md)
1. SQL Server nasıl [çoğaltılır](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Dağıtım desenleri

Bir Citrix XenApp ve XenDesktop çiftliğinde genellikle aşağıdaki dağıtım deseni vardır:

**Dağıtım deseni**

AD DNS sunucusu, SQL veritabanı sunucusu, Citrix Dağıtım Denetleyicisi, StoreFront sunucusu, XenApp Master (VDA), Citrix XenApp Lisans Sunucusu ile Citrix XenApp ve XenDesktop dağıtımı

![Dağıtım Deseni 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Site Recovery desteği

Bu makalenin amacı için, VSphere 6.0 tarafından yönetilen VMware sanal makinelerde Citrix dağıtımları / System Center VMM 2012 R2 DR kurulumu için kullanılmıştır.

### <a name="source-and-target"></a>Kaynak ve hedef

**Senaryo** | **İkincil siteye** | **Azure'a**
--- | --- | ---
**Hyper-V** | Kapsam içinde değil | Evet
**VMware** | Kapsam içinde değil | Evet
**Fiziksel sunucu** | Kapsam içinde değil | Evet

### <a name="versions"></a>Sürümler
Müşteriler XenApp bileşenlerini Hyper-V veya VMware'de çalışan Sanal Makineler veya Fiziksel Sunucular olarak dağıtabilirler. Azure Site Kurtarma, Azure'a hem fiziksel hem de sanal dağıtımları koruyabilir.
XenApp 7.7 veya sonraki sürümler Azure'da desteklendirildiğinden, yalnızca bu sürümlere sahip dağıtımlar Olağanüstü Durum Kurtarma veya geçiş için Azure'da başarısız olabilir.

### <a name="things-to-keep-in-mind"></a>Akılda tutulması gereken noktalar

1. XenApp tarafından yayınlanan uygulamaları ve XenApp yayınlanan masaüstü bilgisayarları sunmak için Server OS makinelerini kullanarak şirket içi dağıtımların korunması ve kurtarılması desteklenir.

2. Windows 10 da dahil olmak üzere istemci sanal masaüstü bilgisayarlar için Masaüstü VDI sunmak için masaüstü işletim sistemi makineleri kullanarak şirket içi dağıtımların korunması ve kurtarılması desteklenmez. Bunun nedeni, Site Kurtarma masaüstü işletim sistemi ile makinelerin kurtarma desteklemez olmasıdır.  Ayrıca, bazı istemci sanal masaüstü işletim sistemleri (örneğin. Windows 7) azure'da lisanslama için henüz desteklenmez. Azure’da istemci/sunucu masaüstlerini lisanslama hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/pricing/licensing-faq/).

3.  Azure Site Kurtarma, varolan şirket içi MCS veya PVS klonlarını çoğaltabilir ve koruyamaz.
Bu klonları, Teslim denetleyicisinden Azure RM sağlamasını kullanarak yeniden oluşturmanız gerekir.

4. NetScaler FreeBSD'yi temel alsa da, Azure Site Kurtarma FreeBSD işletim sistemi korumayı desteklemediği için NetScaler Azure Site Kurtarma kullanılarak korunamaz. Azure'a geçemeden Azure Market'ten yeni bir NetScaler cihazı dağıtmanız ve yapılandırmanız gerekir.


## <a name="replicating-virtual-machines"></a>Sanal makineleri çoğaltma

Citrix XenApp dağıtımının aşağıdaki bileşenlerinin çoğaltılması ve geri kazanılmasını sağlamak için korunması gerekir.

* AD DNS sunucusunun korunması
* SQL veritabanı sunucusunun korunması
* Citrix Teslimat Denetleyicisinin Korunması
* StoreFront sunucusunun korunması.
* XenApp Master (VDA) koruması
* Citrix XenApp Lisans Sunucusunun Korunması


**AD DNS sunucu çoğaltma**

Azure'da bir etki alanı denetleyicisini çoğaltma ve yapılandırma kılavuzunda [Azure Site Kurtarma ile Etkin Dizin ve DNS'yi koruyun'a](site-recovery-active-directory.md) bakın.

**SQL veritabanı Server çoğaltma**

SQL sunucularını korumak için önerilen seçenekler hakkında ayrıntılı teknik kılavuz için lütfen [SQL Server'ı SQL Server'ı Koruyun olağanüstü kurtarma ve Azure Site Kurtarma'ya](site-recovery-sql.md) bakın.

Diğer bileşen sanal makinelerini Azure'da çoğaltmaya başlamak için [bu kılavuzu](site-recovery-vmware-to-azure.md) izleyin.

![XenApp Bileşenlerinin Korunması](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**Bilgi İşlem ve Ağ Ayarları**

Makineler korunduktan sonra (durum Çoğaltılan Öğeler altında "Korumalı" olarak gösterir), İşlem ve Ağ ayarlarının yapılandırılması gerekir.
Bilgi İşlem ve Ağ > Bilgi İşlem özelliklerinde, Azure VM adını ve hedef boyutunu belirtebilirsiniz.
Gerekirse Azure gereksinimleri ile uyum sağlamak için adı değiştirin. Ayrıca, Azure VM'ye atanacak hedef ağ, alt ağ ve IP adresi hakkında bilgi görüntüleyebilir ve ekleyebilirsiniz.

Şunlara dikkat edin:

* Hedef IP adresini ayarlayabilirsiniz. Bir IP adresi sağlamazsanız yük devredilen makine DHCP kullanır. Başarısız bir adreste kullanılamayan bir adres ayarlarsanız, başarısız lık çalışmaz. Hedef IP adresi, yük devretme ağı testinde kullanılabilirse aynı IP adresi yük devretme sınamasında da kullanılabilir.

* AD/DNS sunucusu için şirket içi adresi korumak, Azure Sanal ağı için DNS sunucusuyla aynı adresi belirtmenize olanak tanır.

Ağ bağdaştırıcılarının sayısı, hedef sanal makine için sizin belirlediğiniz boyuta göre aşağıdaki gibi belirlenmiştir:

*   Kaynak makinedeki ağ bağdaştırıcılarının sayısı, hedef makine boyutu için verilen ağ bağdaştırıcısı sayısına eşitse veya daha azsa hedef makine kaynakla aynı sayıda bağdaştırıcıya sahip olur.
*   Kaynak sanal makinenin bağdaştırıcı sayısı, hedef boyut için izin verilen sayıyı aşarsa maksimum hedef boyutu kullanılır.
* Örneğin, kaynak makinenin iki bağdaştırıcısı varsa ve hedef makine boyutu dört adet bağdaştırıcıyı destekliyorsa hedef makinenin iki bağdaştırıcısı olur. Kaynak makinenin iki bağdaştırıcısı varken hedef boyut yalnızca bir bağdaştırıcıyı destekliyorsa hedef makinenin bir bağdaştırıcısı olur.
*   Sanal makinede birden çok ağ bağdaştırıcısı varsa, hepsi aynı ağa bağlanır.
*   Sanal makinede birden çok ağ bağdaştırıcısı varsa, listede gösterilen ilk makine Azure sanal makinesinde Varsayılan ağ bağdaştırıcısı olur.


## <a name="creating-a-recovery-plan"></a>Kurtarma planı oluşturma

XenApp bileşeni VM'ler için çoğaltma etkinleştirildikten sonra, bir sonraki adım bir kurtarma planı oluşturmaktır.
Kurtarma planı, başarısız olmak ve kurtarma için benzer gereksinimlere sahip sanal makineleri bir araya getirir.  

**Kurtarma planı oluşturma adımları**

1. Kurtarma Planı'na XenApp bileşeni sanal makineleri ekleyin.
2. Kurtarma Planları -> + Kurtarma Planı'nı tıklatın. Kurtarma planı için sezgisel bir ad sağlayın.
3. VMware sanal makineleri için: Kaynağı VMware işlem sunucusu olarak seçin, Microsoft Azure olarak hedefleyin ve Kaynak Yöneticisi olarak dağıtım modelini seçin ve Öğeleri Seç'e tıklayın.
4. Hyper-V sanal makineler için: Kaynağı VMM sunucusu olarak seçin, Microsoft Azure olarak hedefleyin ve Kaynak Yöneticisi olarak dağıtım modelini seçin ve Öğeleri Seç'e tıklayın ve ardından XenApp dağıtım Sanal M'lerini seçin.

### <a name="adding-virtual-machines-to-failover-groups"></a>Başarısız gruplara sanal makineler ekleme

Kurtarma planları, belirli başlangıç sırası, komut dosyaları veya el ile eylemler için başarısız gruplar eklemek üzere özelleştirilebilir. Aşağıdaki grupların kurtarma planına eklenmesi gerekir.

1. Failover Group1: AD DNS
2. Failover Group2: SQL Server VMs
2. Failover Group3: VDA Master Image VM
3. Failover Group4: Teslim Denetleyicisi ve StoreFront sunucu VMs


### <a name="adding-scripts-to-the-recovery-plan"></a>Kurtarma planına komut dosyaları ekleme

Komut dosyaları, kurtarma planında belirli bir gruptan önce veya sonra çalıştırılabilir. El ile eylemler de dahil edilebilir ve failover sırasında gerçekleştirilir.

Özelleştirilmiş kurtarma planı aşağıdaki gibi görünür:

1. Failover Group1: AD DNS
2. Failover Group2: SQL Server VMs
3. Failover Group3: VDA Master Image VM

   >[!NOTE]     
   >Manuel veya komut dosyası eylemleri içeren 4, 6 ve 7 adımları yalnızca MCS/PVS kataloglarına sahip şirket içi XenApp >ortamları için geçerlidir.

4. Grup 3 El kitabı veya komut dosyası eylemi: Ana VDA VM'yi kapatın.
Master VDA VM, Azure'da başarısız olduğunda çalışan bir durumda olacaktır. Azure barındırma yı kullanarak yeni MCS katalogları oluşturmak için ana VDA VM'nin Durduruldu (de allocated) durumunda olması gerekir. Azure portalından VM'yi kapatın.

5. Failover Group4: Teslim Denetleyicisi ve StoreFront sunucu VMs
6. Grup3 el kitabı veya komut dosyası eylemi 1:

    ***Azure RM ana bilgisayar bağlantısı ekleme***

    Azure'da yeni MCS katalogları sağlamak için Teslim Denetleyicisi makinesinde Azure ana bilgisayar bağlantısı oluşturun. Bu [makalede](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/)açıklandığı gibi adımları izleyin.

7. Grup3 el kitabı veya komut dosyası eylemi 2:

    ***Azure'da MCS Kataloglarını yeniden oluşturun***

    Birincil sitedeki varolan MCS veya PVS klonları Azure'a çoğaltılamaz. Bu klonları, Teslim denetleyicisinden çoğaltılan ana VDA ve Azure sağlamasını kullanarak yeniden oluşturmanız gerekir. Azure'da MCS katalogları oluşturmak için bu [makalede](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) açıklanan adımları izleyin.

![XenApp Bileşenleri için kurtarma planı](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >DNS'yi >sanal makineler üzerinden başarısız olan yeni IP'lerle güncelleştirmek veya gerekirse sanal makine üzerinden başarısız olan asıma üzerinde bir yük dengeleyicisi takmak için komut dosyalarını [yerinde](https://github.com/Azure/azure-quickstart-templates/tree/master/asr-automation-recovery/scripts) kullanabilirsiniz.


## <a name="doing-a-test-failover"></a>Bir test başarısız yapma

Bir test başarısız yapmak için [bu kılavuzu](site-recovery-test-failover-to-azure.md) izleyin.

![Kurtarma Planı](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>Bir failover yapma

Bir failover yaparken [bu kılavuzu](site-recovery-failover.md) izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu teknik incelemede Citrix XenApp ve XenDesktop dağıtımlarını çoğaltma hakkında [daha fazla bilgi edinebilirsiniz.](https://aka.ms/citrix-xenapp-xendesktop-with-asr) Site Kurtarma'yı kullanarak [diğer uygulamaları çoğaltmakılavuzuna](site-recovery-workload.md) bakın.
