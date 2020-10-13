---
title: Sık sorulan sorular
description: Azure VMware çözümüyle ilgili bazı yaygın soruların yanıtlarını sağlar.
ms.topic: conceptual
ms.date: 09/25/2020
ms.author: dikamath
ms.openlocfilehash: a3e50a6a34a6588ecce8b9ecc3c6b358fddc38b7
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91948723"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution"></a>Azure VMware çözümü hakkında sık sorulan sorular

Azure VMware çözümü hakkında sık sorulan sorular için yanıtlar.

## <a name="general"></a>Genel

#### <a name="what-is-azure-vmware-solution"></a>Azure VMware Çözümü nedir?

Kuruluşlar, iş çevikliği geliştirmek, maliyetleri azaltmak ve yeniliklerini hızlandırmak için strateji yaparken, hibrit bulut platformları, müşterilerin dijital dönüşümünde önemli etkinleştiriciler olarak ortaya çıktı. Azure VMware çözümü, VMware 'nin yazılım tanımlı veri merkezi (SDDC) yazılımını Microsoft Azure küresel bulut hizmeti ekosistemi ile birleştirir. Azure VMware çözümü, performans, kullanılabilirlik, güvenlik ve uyumluluk gereksinimlerini karşılayacak şekilde yönetilir.

## <a name="azure-vmware-solution-service"></a>Azure VMware çözüm hizmeti

#### <a name="where-is-azure-vmware-solution-available-today"></a>Azure VMware çözümü bugün kullanılabilir mi?

Hizmet yeni bölgelere sürekli ekleniyor, bu nedenle daha fazla ayrıntı için [en son hizmet kullanılabilirliği bilgilerini](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) görüntüleyin. 

#### <a name="can-workloads-running-in-an-azure-vmware-solution-instance-consume-or-integrate-with-azure-services"></a>Azure VMware Çözüm örneğinde çalışan iş yükleri Azure hizmetleriyle tüketiliyor veya tümleştirilmesine neden olabilir?

Tüm Azure hizmetleri Azure VMware Çözüm müşterileri tarafından kullanılabilir. Belirli hizmetler için performans ve kullanılabilirlik sınırlamaları, büyük/küçük harfe göre değinilmesi gerekecektir.

#### <a name="do-i-use-the-same-tools-that-i-use-now-to-manage-private-cloud-resources"></a>Özel bulut kaynaklarını yönetmek için şu anda kullandığım araçları kullanıyorum?

Evet. Azure portal dağıtım ve bir dizi yönetim işlemi için kullanılır. vCenter ve NSX Yöneticisi, vSphere ve NSX-T kaynaklarını yönetmek için kullanılır.

#### <a name="can-i-manage-a-private-cloud-with-my-on-premises-vcenter"></a>Şirket içi vCenter 'um ile özel bir bulutu yönetebilir miyim?

Başlatma sırasında Azure VMware çözümü, şirket içi ve özel bulut ortamlarında tek bir yönetim deneyimini desteklemez. Özel bulut kümeleri, özel bir bulutta vCenter ve NSX Yöneticisi yerel ile yönetilir.

#### <a name="can-i-use-vrealize-suite-running-on-premises"></a>Şirket içinde çalışan VNET paketini kullanabilir miyim? 

Belirli tümleştirmeler ve kullanım durumları, büyük/küçük harf esasına göre değerlendirilebilir.

#### <a name="can-i-migrate-vsphere-vms-from-on-premises-environments-to-azure-vmware-solution-private-clouds"></a>Şirket içi ortamlardan vSphere VM 'lerini Azure VMware Çözüm özel bulutlarına geçirebilir miyim?

Evet. Standart çapraz vCenter [vMotion gereksinimleri](https://kb.vmware.com/s/article/210695) karşılanıyorsa VM 'leri özel bir buluta TAŞıMAK için VM geçişi ve vMotion kullanılabilir.

#### <a name="is-a-specific-version-of-vsphere-required-in-on-premises-environments"></a>Şirket içi ortamlarda gerekli olan belirli bir vSphere sürümü mi?

Tüm bulut ortamları, vMotion için şirket içi ortamlarda VMware HCX, vSphere 5,5 veya sonraki bir sürümü ile gelir.

#### <a name="what-does-the-change-control-process-look-like"></a>Değişiklik denetimi işlemi nasıl görünür?

Hizmette yapılan güncelleştirmeler, Microsoft Azure Standart değişiklik yönetimi işlemini izler. Müşteriler tüm iş yükü yönetim görevlerinden ve ilişkili değişiklik yönetimi işlemlerinden sorumludur.

#### <a name="how-is-this-different-from-azure-vmware-solution-by-cloudsimple"></a>Bu, CloudSimple tarafından Azure VMware çözümünden farklı midir?

Yeni Azure VMware çözümüyle, Microsoft ve VMware 'nin doğrudan bir bulut sağlayıcısı ortaklığı vardır. Yeni çözüm, Microsoft tarafından tamamen tasarlanır, oluşturulmuştur ve desteklenir, VMware tarafından onaylanmıştır. Mimari türsel olarak, çözümler Azure adanmış bir altyapıda çalışan VMware teknoloji yığını ile tutarlıdır.

#### <a name="are-red-hat-solutions-supported-on-azure-vmware-solution"></a>Red hat çözümleri Azure VMware çözümünde destekleniyor mu?

Microsoft ve Red hat, Azure platformunda çalışan Red Hat ekosistemlerine yönelik Birleşik bir iletişim noktası sağlayan tümleşik, birlikte bulunan bir destek ekibini paylaşır.  Red Hat Enterprise Linux ile çalışan diğer Azure platformu hizmetleri gibi Azure VMware çözümü, bulut erişimi ve tümleşik destek şemsiye kapsamında bulunur ve Azure 'da Azure VMware çözümünün üzerinde çalışmak için Red Hat Enterprise Linux desteklenir.

#### <a name="is-vmware-hcx-enterprise-edition-available-and-if-so-how-much-does-it-cost"></a>VMware HCX Enterprise Edition mevcuttur ve bu durumda ne kadar maliyetlidir?

VMware HCX Enterprise Edition (EE), Azure VMware çözümüyle bir *Önizleme* işlevi/hizmeti olarak sunulmaktadır. Azure VMware çözümü için VMware HCX EE önizleme aşamasındadır. Bu, ücretsiz bir işlevdir/hizmettir ve hizmet hüküm ve koşullarına tabidir. VMware HCX EE hizmeti GA olduktan sonra, faturalandırmaya geçiş yapılacak 30 günlük bir bildirim alırsınız. Ayrıca, hizmeti kapatma/devre dışı bırakma seçeneğiniz de vardır.

## <a name="compute-network-storage-and-backup"></a>İşlem, ağ, depolama ve yedekleme

#### <a name="is-there-more-than-one-type-of-host-available"></a>Birden fazla konak türü var mı?

Kullanılabilir tek bir konak türü vardır.

#### <a name="what-are-the-cpu-specifications-in-each-type-of-host"></a>Her konak türünde CPU belirtimleri nelerdir?

Sunucularda iki adet 18 çekirdekli 2,3 GHz Intel CPU vardır.

#### <a name="how-much-memory-is-in-each-host"></a>Her konakta ne kadar bellek var?

Sunucularda 576 GB RAM vardır.

#### <a name="what-is-the-storage-capacity-of-each-host"></a>Her Konağın depolama kapasitesi nedir?

Her ESXi konağında, 15,2 TB 'lik kapasite katmanı ve 3,2-TB NVMe önbellek katmanı (her diskgroup 'ta 1,6 TB) içeren iki vSAN diskgroups vardır.

#### <a name="how-much-network-bandwidth-is-available-in-each-esxi-host"></a>Her ESXi konağında kullanılabilir ağ bant genişliği miktarı nedir?

Her ESXi Konağı Azure VMware çözümü, ESXi sistem trafiği için sağlanan iki NIC ve iş yükü trafiği için sağlanan iki NIC ile 4 25 GB/sn 'lik NIC 'ler ile yapılandırılır. 

#### <a name="is-data-stored-on-the-vsan-datastores-encrypted-at-rest"></a>Veriler, bekleyen vSAN veri depoları üzerinde mi depolanıyor?

Evet, tüm vSAN verileri, Azure Key Vault depolanan anahtarlar kullanılarak varsayılan olarak şifrelenir.

#### <a name="you-document-that-commvault-veritas-and-veeam-have-extended-their-backup-solutions-to-work-with-azure-vmware-solution-what-about-other-independent-software-vendor-isv-backup-solutions"></a>Commkasasının, VERITAS ve Veead 'nin Azure VMware çözümüyle birlikte çalışmak için yedekleme çözümlerini genişletmiş olursunuz. Diğer bağımsız yazılım satıcısı (ISV) yedekleme çözümleri hakkında ne olacak?

Öğrendiğimiz kadar, HotAdd taşıma moduyla VMware VADP kullanan tüm yedekleme çözümlerinin Azure VMware çözümünde kutudan hemen çalışması gerekir.

#### <a name="what-about-support-for-isv-backup-solutions"></a>ISV yedekleme çözümleri için destek nedir?

Bu yedekleme çözümleri müşteriler tarafından yüklenip yönetildiğinden, destek için ilgili ISV 'ye ulaşabilirler. 

#### <a name="what-is-the-correct-storage-policy-for-the-dedup-set-up"></a>Yinelenenleri kaldırma ayarı için doğru depolama ilkesi nedir?

VM şablonunuz için *thin_provision* depolama ilkesini kullanın.  Varsayılan değer *thick_provision*.

## <a name="hosts-clusters-and-private-clouds"></a>Konaklar, kümeler ve özel bulutlar

#### <a name="is-the-underlying-infrastructure-shared"></a>Temel alınan altyapı paylaşılıyor mu?

Hayır, özel bulut konakları ve kümeleri ayrılmış ve kullanılmadan önce ve güvenli bir şekilde silinir.

#### <a name="what-are-the-minimum-and-maximum-number-of-hosts-per-cluster"></a>Küme başına en düşük ve en yüksek ana bilgisayar sayısı nelerdir?

Kümeler 3 ila 16 ESXi konakları arasında ölçeklendirebilir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

#### <a name="can-i-scale-my-private-cloud-clusters"></a>Özel bulut kümelerimi ölçeklendirebilir miyim?

Evet, kümeler en düşük ve en fazla ESXi ana bilgisayarı sayısı arasında ölçeklenir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

#### <a name="what-are-trial-clusters"></a>Deneme kümeleri nedir?

Deneme kümeleri, Azure VMware Çözüm özel bulutlarının bir aylık değerlendirmesi için kullanılan üç ana bilgisayar kümeleridir.

#### <a name="can-i-use-high-end-hosts-for-trial-clusters"></a>Deneme kümeleri için yüksek kaliteli Konakları kullanabilir miyim?

Hayır. Yüksek kaliteli ESXi Konakları üretim kümelerinde kullanılmak üzere ayrılmıştır.

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware çözümü ve VMware yazılımı

#### <a name="what-versions-of-vmware-software-is-used-in-private-clouds"></a>Özel bulutlarda VMware yazılımının hangi sürümleri kullanılıyor?

Özel bulutlar vSphere 6,7, vSAN 6,7, VMware HCX ve NSX-T sürüm 2,5 ' i kullanır.  

#### <a name="do-private-clouds-use-vmware-nsx"></a>Özel bulutlar VMware NSX kullanıyor mu?

Evet, NSX-T 2,5, Azure VMware Çözüm özel bulutlarında yazılım tanımlı ağ için kullanılır.

#### <a name="can-i-use-vmware-nsx-v-in-a-private-cloud"></a>VMware NSX-V ' y I özel bir bulutta kullanabilir miyim?

Hayır. NSX-T, NSX 'in desteklenen tek sürümüdür.

#### <a name="is-nsx-required-in-on-premises-environments-or-networks-that-connect-to-a-private-cloud"></a>NSX, şirket içi ortamlarda veya özel bir buluta bağlanan ağlarda gereklidir mi?

Hayır, NSX 'i şirket içinde kullanmak zorunda değilsiniz.

#### <a name="what-is-the-upgrade-and-update-schedule-for-vmware-software-in-a-private-cloud"></a>Özel buluttaki VMware yazılımı için yükseltme ve güncelleştirme zamanlaması nedir?

Özel bulut yazılım paketi yükseltmeleri, Yazılımı VMware 'den en son yazılım paketi sürümünün bir sürümünde tutmak için yapılır. Özel bulut yazılımı sürümleri, bireysel yazılım bileşenlerinin en son sürümlerinden (ESXi, NSX-T, vCenter, vSAN) farklı olabilir.

#### <a name="how-often-will-the-private-cloud-software-stack-be-updated"></a>Özel bulut yazılım yığını ne sıklıkla güncelleştirilir?

Özel bulut yazılımı, VMware 'den yazılım paketi yayını ile izleyen bir zamanlamaya göre yükseltilir. Özel bulutunuz yükseltmeler için kapalı kalma süresi gerektirmez.

## <a name="connectivity"></a>Bağlantı

#### <a name="what-network-ip-address-planning-is-required-to-incorporate-private-clouds-with-on-premises-environments"></a>Şirket içi ortamlarla özel bulutlar eklemek için hangi ağ IP adresi planlaması gerekir?

Bir Azure VMware çözümü özel bulutu dağıtmak için özel bir ağ/22 adres alanı gerekir. Bu özel adres alanı, bir abonelikteki diğer sanal ağlarla veya şirket içi ağlarla çakışmamalıdır.
 
#### <a name="how-do-i-connect-from-on-premises-environments-to-an-azure-vmware-solution-private-cloud"></a>Şirket içi ortamlardan Azure VMware Çözüm özel bulutuna Nasıl yaparım? mı bağlanacak?

Hizmete iki yöntemden birini kullanabilirsiniz: 

- Özel buluta ExpressRoute aracılığıyla eşlenen bir Azure sanal ağı üzerinde dağıtılan bir VM veya uygulama ağ geçidi ile.
- ExpressRoute aracılığıyla şirket içi veri merkezinizden bir Azure ExpressRoute devresine Global Reach.

#### <a name="how-do-i-connect-a-workload-vm-to-the-internet-or-an-azure-service-endpoint"></a>Bir iş yükü VM 'sini Internet 'e veya Azure hizmeti uç noktasına bağlamak Nasıl yaparım?.

Azure portal bir özel bulut için internet bağlantısını etkinleştirin. NSX-T Yöneticisi ile bir NSX-T T1 yönlendiricisi ve bir mantıksal anahtar oluşturun. Ardından, mantıksal anahtar tarafından tanımlanan ağ kesimine bir VM dağıtmak için vCenter 'ı kullanırsınız. Bu VM, internet ve Azure hizmetleri için ağ erişimine sahip olacaktır.

#### <a name="do-i-need-to-restrict-access-from-the-internet-to-vms-on-logical-networks-in-a-private-cloud"></a>İnternet 'ten özel bir buluttaki mantıksal ağlardaki VM 'lere erişimi kısıtlayacağım mi?

Hayır. İnternet 'ten özel bulutlara doğrudan gelen ağ trafiğine izin verilmez.

#### <a name="do-i-need-to-restrict-internet-access-from-vms-on-logical-networks-to-the-internet"></a>Mantıksal ağlardaki VM 'lerden internet 'e erişimi kısıtlam gerekir mi?

Evet. İnternet 'e yönelik VM erişimini kısıtlayan bir güvenlik duvarı oluşturmak için NSX-T Yöneticisi ' ni kullanmanız gerekir.



## <a name="accounts-and-privileges"></a>Hesaplar ve ayrıcalıklar

#### <a name="what-accounts-and-privileges-will-i-get-with-my-new-azure-vmware-solution-private-cloud"></a>Yeni Azure VMware çözümü özel bulutum ile hangi hesapları ve ayrıcalıkları alabilirim?

NSX-T Manager 'da vCenter ve yönetici erişimi içindeki bir cloudadmin kullanıcısı için kimlik bilgileri sağlıyoruz. Ayrıca, Azure Active Directory birleştirmek için kullanılabilecek bir CloudAdmin grubu da vardır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

#### <a name="can-have-administrator-access-to-esxi-hosts"></a>ESXi konaklarına yönetici erişimi olabilir mi?

Hayır, ESXi 'ye yönetici erişimi, çözümün güvenlik gereksinimlerini karşılayacak şekilde kısıtlıdır.

#### <a name="what-privileges-and-permissions-will-i-have-in-vcenter"></a>VCenter 'da hangi ayrıcalıkların ve izinlerin olması gerekir?

CloudAdmin grup ayrıcalıklarınız olacaktır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

#### <a name="what-privileges-and-permissions-will-i-have-on-the-nsx-t-manager"></a>NSX-T Manager 'da hangi ayrıcalıkları ve izinleri almam gerekir?

NSX-T üzerinde tam yönetici ayrıcalıklarına sahip olacaksınız ve şirket içi NSX-T veri merkezi ile yaptığınız gibi rol tabanlı erişim denetimini yönetebilirsiniz. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

> [!NOTE]
> Bir T0 yönlendiricisi, özel bulut dağıtımının bir parçası olarak oluşturulur ve yapılandırılır. Bu mantıksal yönlendiricide veya NSX-T Edge düğümü VM 'lerinde yapılan herhangi bir değişiklik, özel bulutunuzun bağlantısını etkileyebilir.

## <a name="billing-and-support"></a>Faturalandırma ve destek

#### <a name="how-will-pricing-be-structured-for-azure-vmware-solution"></a>Azure VMware çözümü için fiyatlandırma nasıl yapılandırılır?

Fiyatlandırma hakkında genel sorular için bkz. Azure VMware Çözüm [fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-vmware) sayfası. 

#### <a name="who-supports-azure-vmware-solution"></a>Azure VMware çözümünü kimler destekler?

Azure VMware çözümü desteği Microsoft tarafından dağıtılır. Bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)gönderebilirsiniz.

#### <a name="what-accounts-do-i-need-to-create-an-azure-vmware-solution-private-cloud"></a>Azure VMware çözümü özel bulutu oluşturmak için hangi hesaplara ihtiyacım var?

Azure aboneliğinde bir Azure hesabınızın olması gerekir.

#### <a name="how-do-i-request-a-host-quota-increase-for-azure-vmware-solution"></a>Nasıl yaparım? Azure VMware çözümü için bir konak kotası artışı ister misiniz?

* Microsoft ile bir [Azure Kurumsal Anlaşma (EA)](../cost-management-billing/manage/ea-portal-agreements.md) gerekir.
* Azure aboneliğinde bir Azure hesabınızın olması gerekir.

Azure VMware Çözüm kaynağını oluşturmadan önce, düğümlerinizin ayrılması için bir destek bileti göndermeniz gerekir. Destek ekibi isteğinizi aldıktan sonra, isteğinizi doğrulamak ve düğümlerinizi ayırmak için beş iş günü sürer. Mevcut bir Azure VMware çözümü özel bulutunuz varsa ve daha fazla düğüm ayırdıysanız, aynı işleme gidebilirsiniz.


1. Azure portal, **Yardım + Destek**altında, **[Yeni bir destek isteği](https://rc.portal.azure.com/#create/Microsoft.Support)** oluşturun ve bilet için aşağıdaki bilgileri sağlayın:
   - **Sorun türü:** Teknik
   - **Abonelik:** Aboneliğinizi seçin
   - **Hizmet:** Tüm hizmetler Azure VMware çözümüne >
   - **Kaynak:** Genel soru 
   - **Özet:** Kapasiteye ihtiyaç duyuyor
   - **Sorun türü:** Kapasite yönetimi sorunları
   - **Sorun alt türü:** Ek konak kotası/kapasitesi için müşteri Isteği

1. Destek bileti **açıklamasında** , **Ayrıntılar** sekmesinde şunları belirtin:

   - POC veya üretim 
   - Bölge Adı
   - Düğüm sayısı
   - Diğer ayrıntılar

   >[!NOTE]
   >Azure VMware çözümü, özel bulutunuzu ve artıklığı N + 1 düğümlerini çalıştırmak için en az üç düğüm önerir. 

1. İsteği göndermek için **gözden geçir + oluştur** ' u seçin.

   Bir destek temsilcisinin isteğinizi onaylamasını beş adede kadar iş günü sürer.

   >[!IMPORTANT] 
   >Zaten mevcut bir Azure VMware çözümünüz varsa ve ek düğümler istiyorsanız, düğümlerin ayrılması için beş iş gününe ihtiyacımız olduğunu lütfen unutmayın. 

1. Düğümlerinizi sağlayabilmeniz için önce **Microsoft. AVS** kaynak sağlayıcısını Azure Portal kaydettiğinizden emin olun.  

   ```azurecli-interactive
   az provider register -n Microsoft.AVS --subscription <your subscription ID>
   ```

   Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md).

<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md