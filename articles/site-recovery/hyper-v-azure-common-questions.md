---
title: Azure Site Kurtarma ile Hyper-V olağanüstü durum kurtarma için sık sorulan sorular
description: Bu makalede, Azure Site Kurtarma sitesini kullanarak şirket içi Hyper-V V VM'ler için Azure'a olağanüstü durum kurtarma ayarlamayla ilgili sık sorulan sorular özetlenmiştir.
ms.date: 11/12/2019
ms.topic: conceptual
ms.openlocfilehash: 7c5f55fbea67567ddf7a2afa6a61f6c76568d829
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75498200"
---
# <a name="common-questions---hyper-v-to-azure-disaster-recovery"></a>Sık sorulan sorular - Hyper-V'den Azure'a olağanüstü durum kurtarma

Bu makalede, şirket içi Hyper-V VM'leri Azure'a kopyalarken gördüğümüz sık karşılaşılan soruların yanıtlarını sağlar. 

## <a name="general"></a>Genel

### <a name="how-is-site-recovery-priced"></a>Site Kurtarma nasıl fiyatlandırılır?
[Azure Site Kurtarma fiyatlandırma](https://azure.microsoft.com/pricing/details/site-recovery/) ayrıntılarını inceleyin.

### <a name="how-do-i-pay-for-azure-vms"></a>Azure VM'ler için nasıl ödeme yaparım?
Çoğaltma sırasında veriler Azure depolama alanına çoğaltılır ve vm değişiklikleri ödemezsiniz. Azure'da bir hata yaptığınızda, Site Kurtarma otomatik olarak Azure IaaS sanal makineleri oluşturur. Bundan sonra Azure'da tükettiğiniz işlem kaynakları için faturalandırılırsınız.

### <a name="is-there-any-difference-in-cost-when-replicating-to-general-purpose-v2-storage-account"></a>Genel Amaçlı v2 depolama hesabına kopyalanırken maliyet farkı var mıdır?

Azure Site Kurtarma işlemleri ağır olduğundan, genellikle GPv2 depolama hesaplarında tahakkuk eden işlem maliyetinde bir artış görürsünüz. Değişikliği tahmin etmek için [daha fazla bilgi edinin.](../storage/common/storage-account-upgrade.md#pricing-and-billing)

## <a name="azure"></a>Azure

### <a name="what-do-i-need-in-hyper-v-to-orchestrate-replication-with-site-recovery"></a>Site Kurtarma ile çoğaltma düzenlemek için Hyper-V'de ne lere ihtiyacım var?

Hyper-V ana bilgisayar sunucusu için sahip olmanız gerekenler dağıtım senaryosuna bağlıdır. Aşağıdaki makalelerden Hyper-V önkoşullarını inceleyin:

* [Hyper-V VM'lerini (VMM olmadan) Azure'a çoğaltma](site-recovery-hyper-v-site-to-azure.md)
* [Hyper-V VM'lerini (VMM ile) Azure'a çoğaltma](site-recovery-vmm-to-azure.md)
* [Hyper-V sanal makinelerini ikincil veri merkezine çoğaltma](site-recovery-vmm-to-vmm.md)
* İkincil bir veri merkezine çoğalıyorsanız [Hyper-V VM'ler için Desteklenen konuk işletim sistemleri](https://technet.microsoft.com/library/mt126277.aspx)hakkında bilgi edinin.
* Azure'da çoğalıyorsanız, Site Kurtarma [Azure tarafından desteklenen](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)tüm konuk işletim sistemlerini destekler.

### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Hyper-V istemci işletim sisteminde çalışırken VM'leri koruyabilir miyim?
Hayır, VM'lerin desteklenen bir Windows sunucusu makinesinde çalışan Hyper-V ana bilgisayar sunucusunda bulunması gerekir. İstemci bir bilgisayarı korumanız gerekiyorsa, bilgisayarı fiziksel bir makine olarak [Azure'a](site-recovery-vmware-to-azure.md) veya ikincil bir [veri merkezine](site-recovery-vmware-to-vmware.md)kopyalayabilirsiniz.

### <a name="do-hyper-v-hosts-need-to-be-in-vmm-clouds"></a>Hyper-V ana bilgisayarlarının VMM bulutlarında olması gerekir mi?
İkincil bir veri merkezine çoğaltmak istiyorsanız, Hyper-V VM'ler VMM bulutu içinde bulunan Hyper-V ana bilgisayar sunucularında olmalıdır. Azure'da çoğaltmak istiyorsanız, VMM bulutları olan veya olmayan VM'leri çoğaltabilirsiniz. Azure'a Hyper-V çoğaltma hakkında [daha fazla bilgi edinin.](tutorial-hyper-v-to-azure.md)


### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hyper-V 2.nesil sanal makinelerini Azure'a çoğaltabilir miyim?
Evet. Site Kurtarma, başarısız lık sırasında nesil 2'den nesil 1'e dönüşür. Failback'te makine nesil 2'ye dönüştürülür. [Devamını oku](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).


### <a name="can-i-deploy-site-recovery-with-vmm-if-i-only-have-one-vmm-server"></a>Tek bir VMM sunucusuna sahip olsam da Site Recovery'yi VMM ile dağıtabilir miyim?

Evet. VMM bulutundaki Hyper-V sunucularında VM'leri Azure'a çoğaltabilir veya aynı sunucudaki VMM bulutları arasında çoğaltabilirsiniz. Şirket içi çoğaltma için, hem birincil hem de ikincil sitelerde bir VMM sunucunuz olmasını öneririz. 

### <a name="what-do-i-need-in-azure"></a>Azure'da neye ihtiyacım var?
Azure aboneliğine, Kurtarma Hizmetleri kasasına, depolama hesabına ve sanal ağa ihtiyacınız vardır. Kasa, depolama hesabı ve ağ aynı bölgede olmalıdır.

### <a name="what-azure-storage-account-do-i-need"></a>Hangi Azure depolama hesabına ihtiyacım var?
Bir LRS veya GRS depolama hesabına ihtiyacınız vardır. Bölgesel bir kesintinin meydana gelmesi veya birincil bölgenin kurtarılamaması durumunda verilerin korunması için GRS'yi tavsiye ederiz. Premium depolama desteği verilir.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>Azure hesabımın VM'ler oluşturmak için izinlere ihtiyacı var mı?
Abonelik yöneticisiyseniz, gereksinim duyduğunuz çoğaltma izinlerine sahipsiniz. Değilseniz, Kaynak grubunda ve sanal ağda Bir Azure VM oluşturmak için Site Kurtarma'yı yapılandırdığınızda belirttiğiniz izinlere ve seçili depolama hesabına yazma izinlerine ihtiyacınız vardır. [Daha fazla bilgi edinin](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="is-replication-data-sent-to-site-recovery"></a>Çoğaltma verileri Site Kurtarma'ya mı gönderilir?
Hayır, Site Kurtarma çoğaltılan verileri engellemez ve SANAL'larınızda ne yle ilgili herhangi bir bilgiye sahip değildir. Çoğaltma verileri Hyper-V ana bilgisayarları ve Azure depolama arasında değiştirilir. Site Recovery bu verilere müdahale edemez. Yalnızca çoğaltma ve yük devretme işlemlerini düzenlemek için gereken meta veriler Site Recovery hizmetine gönderilir.  

Site Kurtarma ISO 27001:2013, 27018, HIPAA, DPA sertifikalı ve SOC2 ve FedRAMP JAB değerlendirmeleri sürecindedir.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-region"></a>Şirket içi meta verileri coğrafi bir bölgede tutabilir miyiz?
Evet. Bir bölgede bir kasa oluşturduğunuzda, Site Kurtarma tarafından kullanılan tüm meta verilerin o bölgenin coğrafi sınırları içinde kalmasını sağlarız.

### <a name="does-site-recovery-encrypt-replication"></a>Site Recovery çoğaltma işlemini şifreleyebilir mi?
Evet, [Azure'da](https://docs.microsoft.com/azure/storage/storage-service-encryption) hem geçici şifreleme hem de şifreleme desteklenir.


## <a name="deployment"></a>Dağıtım

### <a name="what-can-i-do-with-hyper-v-to-azure-replication"></a>Hyper-V'den Azure çoğaltmasına ne yapabilirim?

- **Olağanüstü durum kurtarma**: Tam olağanüstü durum kurtarma ayarlayabilirsiniz. Bu senaryoda, şirket içi Hyper-V V M'leri Azure depolama alanına çoğaltırsınız:
    - VM'leri Azure'a çoğaltabilirsiniz. Şirket içi altyapınız kullanılamıyorsa, Azure'da başarısız olursunuz.
    - Başarısız olduğunuzda, Azure SANAL'ları çoğaltılan veriler kullanılarak oluşturulur. Azure VM'lerinde uygulamalara ve iş yüklerine erişebilirsiniz.
    - Şirket içi veri merkeziniz yeniden kullanılabilir olduğunda, Azure'dan şirket içi sitenize geri dönebilirsiniz.
- **Geçiş**: Şirket içi Hyper-V VM'leri Azure depolama alanına geçirmek için Site Kurtarma'yı kullanabilirsiniz. Daha sonra şirket içinde Azure'a geçemezsiniz. Başarısız olduktan sonra, uygulamalarınız ve iş yüklerin kullanılabilir ve Azure VM'lerde çalışır.


### <a name="what-do-i-need-on-premises"></a>Şirket içinde neye ihtiyacım var?

Bir veya daha fazla bağımsız veya kümelenmiş Hyper-V ana bilgisayarda çalışan bir veya daha fazla VM'ye ihtiyacınız vardır. System Center Virtual Machine Manager (VMM) tarafından yönetilen ana bilgisayarlarda çalışan VM'leri de çoğaltabilirsiniz.
- VMM çalıştırmıyorsanız, Site Kurtarma dağıtımı sırasında Hyper-V ana bilgisayarlarını ve kümeleri Hyper-V sitelerine toplarsınız. Site Kurtarma aracılarını (Azure Site Kurtarma Sağlayıcısı ve Kurtarma Hizmetleri aracısı) her Hyper-V ana bilgisayara yüklersiniz.
- Hyper-V ana bilgisayarları bir VMM bulutunda bulunuyorsa, VMM'de çoğaltma yı düzenleyebilirsiniz. Site Kurtarma Sağlayıcısı'nı VMM sunucusuna ve Kurtarma Hizmetleri aracısını her Hyper-V ana bilgisayarına yüklersiniz. VMM mantıksal/VM ağları ve Azure VNets ağları arasında eş oluşturursunuz.
- Hyper-V'den Azure mimarisine daha [fazla bilgi edinin.](hyper-v-azure-architecture.md)

### <a name="can-i-replicate-vms-located-on-a-hyper-v-cluster"></a>Hyper-V kümesinde bulunan VM'leri çoğaltabilir miyim?

Evet, Site Kurtarma kümelenmiş Hyper-V ana bilgisayarlarını destekler. Şunlara dikkat edin:

- Kümenin tüm düğümleri aynı tonoza kayıtlı olmalıdır.
- VMM kullanmıyorsanız, kümedeki tüm Hyper-V ana bilgisayarları aynı Hyper-V sitesine eklenmelidir.
- Kümedeki her Hyper-V ana bilgisayara Azure Site Kurtarma Sağlayıcısı ve Kurtarma Hizmetleri aracısını yükler ve her ana bilgisayarı bir Hyper-V sitesine eklersiniz.
- Küme üzerinde belirli adımlar atılması gerekmez.
- Hyper-V için Dağıtım Planlayıcısı aracını çalıştırırsanız, araç çalışan düğümden ve VM'nin çalıştığı düğümden profil verilerini toplar. Araç kapalı bir düğümden veri toplayamaz, ancak bu düğümü izler. Düğüm çalışmaya başladıktan sonra, araç vm profil verilerini ondan toplamaya başlar (VM profil VM listesinin bir parçasıysa ve düğümde çalışıyorsa).
- Site Kurtarma tonozundaki Hyper-V ana bilgisayardaki bir VM aynı kümedeki farklı bir Hyper-V ana bilgisayara veya bağımsız bir ana bilgisayara taşınırsa, VM için çoğaltma etkilenmez. Hyper-V ana bilgisayar [ön koşulları](hyper-v-azure-support-matrix.md#on-premises-servers)karşılamalı ve Site Kurtarma kasasında yapılandırılmalıdır. 


### <a name="can-i-protect-vms-when-hyper-v-is-running-on-a-client-operating-system"></a>Hyper-V istemci işletim sisteminde çalışırken VM'leri koruyabilir miyim?
Hayır, VM'lerin desteklenen bir Windows sunucusu makinesinde çalışan Hyper-V ana bilgisayar sunucusunda bulunması gerekir. İstemci bir bilgisayarı korumanız gerekiyorsa, [bilgisayarı azure'a fiziksel bir makine olarak çoğaltabilirsiniz.](physical-azure-disaster-recovery.md)

### <a name="can-i-replicate-hyper-v-generation-2-virtual-machines-to-azure"></a>Hyper-V 2.nesil sanal makinelerini Azure'a çoğaltabilir miyim?
Evet. Site Kurtarma, başarısız lık sırasında nesil 2'den nesil 1'e dönüşür. Failback'te makine nesil 2'ye dönüştürülür.

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Site Kurtarma senaryolarını Bir SDK ile otomatikleştirebilir miyim?
Evet. Rest API'si, PowerShell veya Azure SDK kullanarak Site Recovery iş akışlarını otomatikleştirebilirsiniz. PowerShell kullanarak Hyper-V'yi Azure'a çoğaltmak için şu anda desteklenen senaryolar:

- [PowerShell kullanarak VMM olmadan Hyper-V çoğaltma](hyper-v-azure-powershell-resource-manager.md)
- [Powershell kullanarak Hyper-V'yi VMM ile çoğaltma](hyper-v-vmm-powershell-resource-manager.md)

## <a name="replication"></a>Çoğaltma

### <a name="where-do-on-premises-vms-replicate-to"></a>Şirket içi VM'ler nerede çoğaltılır?
Veriler Azure depolamasına çoğalır. Bir hata yaptığınızda, Site Kurtarma otomatik olarak depolama hesabından Azure VM'leri oluşturur.

### <a name="what-apps-can-i-replicate"></a>Hangi uygulamaları çoğaltabilirim?
Çoğaltma gereksinimlerine uygun bir Hyper-V VM çalıştıran herhangi bir uygulamayı veya iş yükünü [çoğaltabilirsiniz.](hyper-v-azure-support-matrix.md#replicated-vms) Site Kurtarma, uygulamaların üzerinde başarısız olması ve akıllı bir duruma geri dönülememesi için uygulama tarafından denetlenmeyeduyarlı çoğaltma desteği sağlar. Site Kurtarma, SharePoint, Exchange, Dynamics, SQL Server ve Active Directory gibi Microsoft uygulamalarıyla tümleşir ve Oracle, SAP, IBM ve Red Hat gibi önde gelen satıcılarla yakın çalışır. İş yükü koruması hakkında [daha fazla bilgi edinin](site-recovery-workload.md).

### <a name="whats-the-replication-process"></a>Çoğaltma işlemi nedir?

1. İlk çoğaltma tetiklendiğinde, bir Hyper-V VM anlık görüntüsü alınır.
2. VM'deki sanal sabit diskler, tümü Azure'a kopyalanana kadar tektek çoğaltılır. VM boyutuna ve ağ bant genişliğine bağlı olarak bu işlem biraz zaman alabilir. Ağ bant genişliğini nasıl artıracağınızı öğrenin.
3. İlk çoğaltma devam ederken disk değişiklikleri gerçekleşirse, Hyper-V Çoğaltma İzleyicisi değişiklikleri Hyper-V çoğaltma günlükleri (.hrl) olarak izler. Bu günlük dosyaları disklerle aynı klasörde bulunur. Her diskin ikincil depolama alanına gönderilen ilişkili bir .hrl dosyası vardır. İlk çoğaltma sırasında anlık görüntü ve günlük dosyaları disk kaynaklarını kullanır.
4. İlk çoğaltma tamamlandığında, VM anlık görüntüsü silinir.
5. Günlükteki tüm disk değişiklikleri eşitlenir ve üst diskle birleştirilir.
6. İlk çoğaltma bittikten sonra, sanal makine iş üzerinde Finalize koruma çalışır. VM'nin korunması için ağ ve diğer çoğaltma sonrası ayarları yapılandırır.
7. Bu aşamada, başarısız olmaya hazır olduğundan emin olmak için VM ayarlarını kontrol edebilirsiniz. Beklendiği gibi başarısız olup olmadığını kontrol etmek için VM için bir olağanüstü durum kurtarma matkabı (test failover) çalıştırabilirsiniz.
8. İlk çoğaltmadan sonra, çoğaltma ilkesine uygun olarak delta çoğaltma başlar.
9. Değişiklikler .hrl dosyaları günlüğe kaydedilir. Çoğaltma için yapılandırılmış her diskin ilişkili bir .hrl dosyası vardır.
10. Günlük müşterinin depolama hesabına gönderilir. Bir günlük Azure'a aktarılırken, birincil diskteki değişiklikler aynı klasördeki başka bir günlük dosyasında izlenir.
11. Hem ilk hem de delta çoğaltma sırasında, Azure portalında VM'yi izleyebilirsiniz.

Çoğaltma işlemi hakkında [daha fazla bilgi edinin.](hyper-v-azure-architecture.md#replication-process)

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>Siteden siteye VPN ile Azure'da çoğaltabilir miyim?

Site Kurtarma, genel bir bitiş noktası üzerinden veya ExpressRoute Microsoft bakışlarını kullanarak şirket içi verileri şirket içi azure depolamasına çoğaltır. Siteden siteye VPN ağı üzerinden çoğaltma desteklenmez.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>ExpressRoute ile Azure'a çoğaltabilir miyim?

Evet, ExpressRoute VM'leri Azure'a çoğaltmak için kullanılabilir. Site Kurtarma, verileri genel bir bitiş noktası üzerinden Azure Depolama Hesabı'nda çoğaltır ve Site Kurtarma çoğaltma için [Microsoft'a bakan](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) bilgiler ayarlamanız gerekir. Sanal M'ler bir Azure sanal ağına geçemedikten sonra, [bunlara özel bir bakış kullanarak](../expressroute/expressroute-circuit-peerings.md#privatepeering)erişebilirsiniz.


### <a name="why-cant-i-replicate-over-vpn"></a>Vpn üzerinden neden kopyalamıyorum?

Azure'da çoğaltma yaptığınızda, çoğaltma trafiği bir Azure Depolama hesabının ortak uç noktalarına ulaşır. Böylece yalnızca ExpressRoute (Microsoft peering) ile ortak internet üzerinden çoğaltmak ve VPN çalışmıyor. 

### <a name="what-are-the-replicated-vm-requirements"></a>Çoğaltılan VM gereksinimleri nelerdir?

Çoğaltma için, Hyper-V VM desteklenen bir işletim sistemi çalıştırıyor olmalıdır. Ayrıca, VM'nin Azure VM'leri için gereksinimleri karşılaması gerekir. Destek matrisinde [daha fazla bilgi edinin.](hyper-v-azure-support-matrix.md#replicated-vms)

### <a name="how-often-can-i-replicate-to-azure"></a>Azure'a ne sıklıkta çoğaltabilirim?

Hyper-V VM'ler her 30 saniyede bir çoğaltılabilir (premium depolama hariç), 5 dakika veya 15 dakika.

### <a name="can-i-extend-replication"></a>Çoğaltmayı genişletebilir miyim?
Genişletilmiş veya zincir çoğaltma desteklenmez. Geri bildirim [forumunda](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959)bu özelliği isteyin.

### <a name="can-i-do-an-offline-initial-replication"></a>Çevrimdışı ilk çoğaltma yapabilir miyim?
Bu özellik desteklenmez. [Geri bildirim forumunda](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)bu özelliği isteyin.

### <a name="can-i-exclude-disks"></a>Diskleri hariç tutabilir miyim?
Evet, diskleri çoğaltmadan hariç tutabilirsiniz. 

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>VM'leri dinamik disklerle çoğaltabilir miyim?
Dinamik diskler çoğaltılabilir. İşletim sistemi diski temel bir disk olmalıdır.



## <a name="security"></a>Güvenlik

### <a name="what-access-does-site-recovery-need-to-hyper-v-hosts"></a>Hyper-V ana bilgisayarlarına Site Kurtarma'nın ne gibi bir erişime ihtiyacı var?

Site Kurtarma seçtiğiniz VM'leri çoğaltmak için Hyper-V ana bilgisayarlarına erişmesi gerekir. Site Kurtarma Hyper-V ana bilgisayarlarına aşağıdakileri yükler:

- VMM çalıştırmıyorsanız, her ana bilgisayara Azure Site Kurtarma Sağlayıcısı ve Kurtarma Hizmetleri aracısı yüklenir.
- VMM çalıştırıyorsanız, Kurtarma Hizmetleri aracısı her ana bilgisayara yüklenir. Sağlayıcı VMM sunucusunda çalışır.


### <a name="what-does-site-recovery-install-on-hyper-v-vms"></a>Hyper-V VM'lere Site Kurtarma ne yükler?

Site Kurtarma, çoğaltma için etkinleştirilen Hyper-V VM'lere açıkça bir şey yüklemez.




## <a name="failover-and-failback"></a>Yük devretme ve yeniden çalışma


### <a name="how-do-i-fail-over-to-azure"></a>Azure'da nasıl başarısız olabilirim?

Şirket içi Hyper-V VM’lerinden Azure’a planlanmış veya planlanmamış bir yük devretme gerçekleştirebilirsiniz.

- Planlı bir yük devretme çalıştırırsanız, veri kaybı olmaması için kaynak VM’ler kapatılır.
- Birincil sitenize erişilemiyorsa, planlanmamış bir başarısızlık çalıştırabilirsiniz.
- Birden çok makinenin başarısız olmasını sağlamak için tek bir makinede başarısız olabilir veya kurtarma planları oluşturabilirsiniz.
- Failover iki bölümden oluşur:
    - Failover'ın ilk aşaması tamamlandıktan sonra, oluşturulan çoğaltma VM'lerini Azure'da görebilirsiniz. Gerekli olursa VM’ye genel bir IP adresi atayabilirsiniz.
    - Daha sonra, azure VM çoğaltmadan iş yüküne erişmeye başlamak için başarısız olmayı taahhüt elersiniz.
   

### <a name="how-do-i-access-azure-vms-after-failover"></a>Başarısız olduktan sonra Azure VM'lere nasıl erişebilirim?
Başarısız olduktan sonra, Azure VM'lerine güvenli bir Internet bağlantısı üzerinden, siteden siteye VPN üzerinden veya Azure ExpressRoute üzerinden erişebilirsiniz. Bağlanmak için bir dizi şey hazırlamanız gerekir. [Daha fazla bilgi edinin](failover-failback-overview.md#connect-to-azure-after-failover).

### <a name="is-failed-over-data-resilient"></a>Veri esnek üzerinde başarısız mı?
Azure esneklik için tasarlanmıştır. Site Kurtarma, Azure SLA'ya uygun olarak ikincil bir Azure veri merkezine geçememesi için tasarlanır. Başarısız olduğunda, meta verilerinizin ve kasalarınızın kasanız için seçtiğiniz coğrafi bölgede kalmasını sağlıyoruz.

### <a name="is-failover-automatic"></a>Yük devretme işlemi otomatik midir?
[Failover](site-recovery-failover.md) otomatik değil. Portalda tek bir tıklamayla başarısızlıklar başlatın veya bir başarısızı tetiklemek için [PowerShell'i](/powershell/module/az.recoveryservices) kullanabilirsiniz.

### <a name="how-do-i-fail-back"></a>Nasıl geri başarısız olabilirim?

Şirket içi altyapınız yeniden çalışmaya başladıktan sonra başarısız olabilirsiniz. Failback üç aşamada oluşur:

1. Birkaç farklı seçeneği kullanarak Azure'dan şirket içi siteye planlanmış bir başarısızlığı başlatın:

    - Kapalı kalma süresini en aza indirin: Bu seçeneği kullanırsanız Site Kurtarma, başarısız olmadan önce verileri eşitler. Değiştirilen veri bloklarını denetler ve bunları şirket içi siteye indirirken, Azure VM çalışmaya devam ederek kapalı kalma süresini en aza indirir. Başarısız ın tamamlanması gerektiğini el ile belirttiğiniz zaman, Azure VM kapatılır, son delta değişiklikleri kopyalanır ve hata geçersiz liği başlar.
    - Tam indirme: Bu seçenek ile veri failover sırasında senkronize edilir. Bu seçenek tüm diski karşıdan yükler. Daha hızlıdır, çünkü hiçbir çek kontrol süresi hesaplanmaz, ancak daha fazla kapalı kalma süresi vardır. Azure VM'lerini bir süredir çalıştırıyorsanız veya şirket içi VM silinmişse bu seçeneği kullanın.

2. Aynı VM'ye veya alternatif bir VM'ye geri dönmeyi seçebilirsiniz. Site Kurtarma'nın vm'yi oluşturması gerektiğini belirtebilirsiniz.
3. İlk eşitleme tamamlandıktan sonra, başarısız olmayı tamamlamak için seçin. Tamamlandıktan sonra, her şeyin beklendiği gibi çalıştığını kontrol etmek için şirket içi VM'de oturum açabilirsiniz. Azure portalında, Azure VM'lerinin durdurulduğunu görebilirsiniz.
4. Sona ermek için başarısız olmayı taahhüt eder ve şirket içi VM'den iş yüküne yeniden erişmeye başlarsınız.
5. İş yükleri geri başarısız olduktan sonra ters çoğaltmayı etkinleştirirsiniz, böylece şirket içi VM'ler yeniden Azure'da çoğaltılır.

### <a name="can-i-fail-back-to-a-different-location"></a>Başka bir yere geri dönebilir miyim?
Evet, Azure'da başarısız olduysanız, orijinali kullanılamıyorsa farklı bir konuma geri dönebilirsiniz. [Daha fazla bilgi edinin](hyper-v-azure-failback.md#fail-back-to-an-alternate-location).
