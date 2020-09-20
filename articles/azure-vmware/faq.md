---
title: Sık sorulan sorular
description: Azure VMware çözümüyle ilgili bazı yaygın soruların yanıtlarını sağlar.
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: dikamath
ms.openlocfilehash: 7b4abc2d711a3da6a6df125854759e083d7e04a7
ms.sourcegitcommit: 0fd1f3fe7817ad44d878d580ec167e1508051795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2020
ms.locfileid: "90817845"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution-preview"></a>Azure VMware Çözüm önizlemesi hakkında sık sorulan sorular

Azure VMware çözümü hakkında sık sorulan sorular için yanıtlar.

## <a name="general"></a>Genel

### <a name="what-is-azure-vmware-solution"></a>Azure VMware Çözümü nedir?

Kuruluşlar, iş çevikliği geliştirmek, maliyetleri azaltmak ve yeniliklerini hızlandırmak için strateji yaparken, hibrit bulut platformları, müşterilerin dijital dönüşümünde önemli etkinleştiriciler olarak ortaya çıktı. Azure VMware çözümü, VMware 'nin yazılım tanımlı veri merkezi (SDDC) yazılımını Microsoft Azure küresel bulut hizmeti ekosistemi ile birleştirir. Azure VMware çözümü, performans, kullanılabilirlik, güvenlik ve uyumluluk gereksinimlerini karşılayacak şekilde yönetilir.

## <a name="azure-vmware-solution-service"></a>Azure VMware çözüm hizmeti

### <a name="where-is-azure-vmware-solution-available-today"></a>Azure VMware çözümü bugün kullanılabilir mi?

Hizmet yeni bölgelere sürekli ekleniyor, bu nedenle daha fazla ayrıntı için [en son hizmet kullanılabilirliği bilgilerini](https://azure.microsoft.com/global-infrastructure/services/?products=azure-vmware) görüntüleyin. 

### <a name="can-workloads-running-in-an-azure-vmware-solution-instance-consume-or-integrate-with-azure-services"></a>Azure VMware Çözüm örneğinde çalışan iş yükleri Azure hizmetleriyle tüketiliyor veya tümleştirilmesine neden olabilir?

Tüm Azure hizmetleri Azure VMware Çözüm müşterileri tarafından kullanılabilir. Belirli hizmetler için performans ve kullanılabilirlik sınırlamaları, büyük/küçük harfe göre değinilmesi gerekecektir.

### <a name="do-i-use-the-same-tools-that-i-use-now-to-manage-private-cloud-resources"></a>Özel bulut kaynaklarını yönetmek için şu anda kullandığım araçları kullanıyorum?

Evet. Azure portal dağıtım ve bir dizi yönetim işlemi için kullanılır. vCenter ve NSX Yöneticisi, vSphere ve NSX-T kaynaklarını yönetmek için kullanılır.

### <a name="can-i-manage-a-private-cloud-with-my-on-premises-vcenter"></a>Şirket içi vCenter 'um ile özel bir bulutu yönetebilir miyim?

Başlatma sırasında Azure VMware çözümü, şirket içi ve özel bulut ortamlarında tek bir yönetim deneyimini desteklemez. Özel bulut kümeleri, özel bir bulutta vCenter ve NSX Yöneticisi yerel ile yönetilir.

### <a name="can-i-use-vrealize-suite-running-on-premises"></a>Şirket içinde çalışan VNET paketini kullanabilir miyim? 

Belirli tümleştirmeler ve kullanım durumları, büyük/küçük harf esasına göre değerlendirilebilir.

### <a name="can-i-migrate-vsphere-vms-from-on-premises-environments-to-azure-vmware-solution-private-clouds"></a>Şirket içi ortamlardan vSphere VM 'lerini Azure VMware Çözüm özel bulutlarına geçirebilir miyim?

Evet. Standart çapraz vCenter [vMotion gereksinimleri](https://kb.vmware.com/s/article/210695) karşılanıyorsa VM 'leri özel bir buluta TAŞıMAK için VM geçişi ve vMotion kullanılabilir.

### <a name="is-a-specific-version-of-vsphere-required-in-on-premises-environments"></a>Şirket içi ortamlarda gerekli olan belirli bir vSphere sürümü mi?

Tüm bulut ortamları, vMotion 'un Şirket içi ortamlarında HCX, vSphere 5,5 veya üzeri sürümlerle birlikte geldiğinden.

### <a name="what-does-the-change-control-process-look-like"></a>Değişiklik denetimi işlemi nasıl görünür?

Hizmette yapılan güncelleştirmeler, Microsoft Azure Standart değişiklik yönetimi işlemini izler. Müşteriler tüm iş yükü yönetim görevlerinden ve ilişkili değişiklik yönetimi işlemlerinden sorumludur.

### <a name="how-is-this-different-from-azure-vmware-solution-by-cloudsimple"></a>Bu, CloudSimple tarafından Azure VMware çözümünden farklı midir?

Yeni Azure VMware çözümüyle, Microsoft ve VMware 'nin doğrudan bir bulut sağlayıcısı ortaklığı vardır. Yeni çözüm, Microsoft tarafından tamamen tasarlanır, oluşturulmuştur ve desteklenir, VMware tarafından onaylanmıştır. Mimari türsel olarak, çözümler Azure adanmış bir altyapıda çalışan VMware teknoloji yığını ile tutarlıdır.

### <a name="if-im-an-existing-azure-vmware-solution-customer-what-does-this-preview-mean-for-me"></a>Var olan bir Azure VMware Çözüm müşterisiyseniz, bu önizleme benim için ne anlama geliyor?

CloudSimple ile mevcut Azure VMware çözümünde değişiklik yapılmaz. Azure 'da çözümü desteklemeye devam ediyoruz. Azure VMware çözümü, hizmet düzeyi sözleşmenizde [(SLA)](https://aka.ms/CSVMwareSLA)desteklenir. Müşteriler, üretim iş yükleri için hizmeti kullanmaya devam etmelidir; Bu, [Microsoft 'un hizmet koşullarına](https://azure.microsoft.com/support/legal/)göre sunulan bir çözümdür.

### <a name="can-i-migrate-from-azure-vmware-solution-by-cloudsimple-to-this-new-solution"></a>Bu yeni çözüme CloudSimple ile Azure VMware çözümünden geçiş yapabilir miyim?

Evet, Azure VMware çözümü, HCX gibi tanıdık VMware araçları kullanılarak geçişi destekler. Yeni çözüme geçiş ile ilgilenen müşteriler için, seçenekleri ve kullanılabilir desteği incelemek üzere Microsoft hesabı ekibinizle birlikte çalışın.



## <a name="compute-network-and-storage"></a>İşlem, ağ ve depolama

### <a name="is-there-more-than-one-type-of-host-available"></a>Birden fazla konak türü var mı?

Kullanılabilir tek bir konak türü vardır.

### <a name="what-are-the-cpu-specifications-in-each-type-of-host"></a>Her konak türünde CPU belirtimleri nelerdir?

Sunucularda iki adet 18 çekirdekli 2,3 GHz Intel CPU vardır.

### <a name="how-much-memory-is-in-each-host"></a>Her konakta ne kadar bellek var?

Sunucularda 576 GB RAM vardır.

### <a name="what-is-the-storage-capacity-of-each-host"></a>Her Konağın depolama kapasitesi nedir?

Her ESXi konağında, 15,2 TB 'lik kapasite katmanı ve 3,2 TB NVMe önbellek katmanı (her diskgroup 'ta 1,6 TB) içeren iki vSAN diskgroups vardır.

### <a name="how-much-network-bandwidth-is-available-in-each-esxi-host"></a>Her ESXi konağında kullanılabilir ağ bant genişliği miktarı nedir?

Her ESXi ana bilgisayarı Azure VMware çözümü, ESXi sistem trafiği için sağlanan iki NIC ve iş yükü trafiği için sağlanan iki NIC ile 4 25 Gbps NIC ile yapılandırılır. 

### <a name="is-data-stored-on-the-vsan-datastores-encrypted-at-rest"></a>Veriler, bekleyen vSAN veri depoları üzerinde mi depolanıyor?

Evet, tüm vSAN verileri, Azure Key Vault depolanan anahtarlar kullanılarak varsayılan olarak şifrelenir.

## <a name="hosts-clusters-and-private-clouds"></a>Konaklar, kümeler ve özel bulutlar

### <a name="is-the-underlying-infrastructure-shared"></a>Temel alınan altyapı paylaşılıyor mu?

Hayır, özel bulut konakları ve kümeleri ayrılmış ve kullanılmadan önce ve güvenli bir şekilde silinir.

### <a name="what-are-the-minimum-and-maximum-number-of-hosts-per-cluster"></a>Küme başına en düşük ve en yüksek ana bilgisayar sayısı nelerdir?

Kümeler 3 ila 16 ESXi konakları arasında ölçeklendirebilir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

### <a name="can-i-scale-my-private-cloud-clusters"></a>Özel bulut kümelerimi ölçeklendirebilir miyim?

Evet, kümeler en düşük ve en fazla ESXi ana bilgisayarı sayısı arasında ölçeklenir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

### <a name="what-are-trial-clusters"></a>Deneme kümeleri nedir?

Deneme kümeleri, Azure VMware Çözüm özel bulutlarının bir aylık değerlendirmesi için kullanılan üç ana bilgisayar kümeleridir.

### <a name="can-i-use-high-end-hosts-for-trial-clusters"></a>Deneme kümeleri için yüksek kaliteli Konakları kullanabilir miyim?

Hayır. Yüksek kaliteli ESXi Konakları üretim kümelerinde kullanılmak üzere ayrılmıştır.

## <a name="azure-vmware-solution-and-vmware-software"></a>Azure VMware çözümü ve VMware yazılımı

### <a name="what-versions-of-vmware-software-is-used-in-private-clouds"></a>Özel bulutlarda VMware yazılımının hangi sürümleri kullanılıyor?

Özel bulutlar vSphere 6,7, vSAN 6,7, HCX ve NSX-T sürüm 2,5 ' i kullanır.  

### <a name="do-private-clouds-use-vmware-nsx"></a>Özel bulutlar VMware NSX kullanıyor mu?

Evet, NSX-T 2,5, Azure VMware Çözüm özel bulutlarında yazılım tanımlı ağ için kullanılır.

### <a name="can-i-use-vmware-nsx-v-in-a-private-cloud"></a>VMware NSX-V ' y I özel bir bulutta kullanabilir miyim?

Hayır. NSX-T, NSX 'in desteklenen tek sürümüdür.

### <a name="is-nsx-required-in-on-premises-environments-or-networks-that-connect-to-a-private-cloud"></a>NSX, şirket içi ortamlarda veya özel bir buluta bağlanan ağlarda gereklidir mi?

Hayır, NSX 'i şirket içinde kullanmak zorunda değilsiniz.

### <a name="what-is-the-upgrade-and-update-schedule-for-vmware-software-in-a-private-cloud"></a>Özel buluttaki VMware yazılımı için yükseltme ve güncelleştirme zamanlaması nedir?

Özel bulut yazılım paketi yükseltmeleri, Yazılımı VMware 'den en son yazılım paketi sürümünün bir sürümünde tutmak için yapılır. Özel bulut yazılımı sürümleri, bireysel yazılım bileşenlerinin en son sürümlerinden (ESXi, NSX-T, vCenter, vSAN) farklı olabilir.

### <a name="how-often-will-the-private-cloud-software-stack-be-updated"></a>Özel bulut yazılım yığını ne sıklıkla güncelleştirilir?

Özel bulut yazılımı, VMware 'den yazılım paketi yayını ile izleyen bir zamanlamaya göre yükseltilir. Özel bulutunuz yükseltmeler için kapalı kalma süresi gerektirmez.

## <a name="connectivity"></a>Bağlantı

### <a name="what-network-ip-address-planning-is-required-to-incorporate-private-clouds-with-on-premises-environments"></a>Şirket içi ortamlarla özel bulutlar eklemek için hangi ağ IP adresi planlaması gerekir?

Bir Azure VMware çözümü özel bulutu dağıtmak için özel bir ağ/22 adres alanı gerekir. Bu özel adres alanı, bir abonelikteki diğer sanal ağlarla veya şirket içi ağlarla çakışmamalıdır.
 
### <a name="how-do-i-connect-from-on-premises-environments-to-an-azure-vmware-solution-private-cloud"></a>Şirket içi ortamlardan Azure VMware Çözüm özel bulutuna Nasıl yaparım? mı bağlanacak?

Hizmete iki yöntemden birini kullanabilirsiniz: 

- Özel buluta ExpressRoute aracılığıyla eşlenen bir Azure sanal ağı üzerinde dağıtılan bir VM veya uygulama ağ geçidi ile.
- ExpressRoute aracılığıyla şirket içi veri merkezinizden bir Azure ExpressRoute devresine Global Reach.

### <a name="how-do-i-connect-a-workload-vm-to-the-internet-or-an-azure-service-endpoint"></a>Bir iş yükü VM 'sini Internet 'e veya Azure hizmeti uç noktasına bağlamak Nasıl yaparım?.

Azure portal bir özel bulut için internet bağlantısını etkinleştirin. NSX-T Yöneticisi ile bir NSX-T T1 yönlendiricisi ve bir mantıksal anahtar oluşturun. Ardından, mantıksal anahtar tarafından tanımlanan ağ kesimine bir VM dağıtmak için vCenter 'ı kullanırsınız. Bu VM, internet ve Azure hizmetleri için ağ erişimine sahip olacaktır.

### <a name="do-i-need-to-restrict-access-from-the-internet-to-vms-on-logical-networks-in-a-private-cloud"></a>İnternet 'ten özel bir buluttaki mantıksal ağlardaki VM 'lere erişimi kısıtlayacağım mi?

Hayır. İnternet 'ten özel bulutlara doğrudan gelen ağ trafiğine izin verilmez.

### <a name="do-i-need-to-restrict-internet-access-from-vms-on-logical-networks-to-the-internet"></a>Mantıksal ağlardaki VM 'lerden internet 'e erişimi kısıtlam gerekir mi?

Evet. İnternet 'e yönelik VM erişimini kısıtlayan bir güvenlik duvarı oluşturmak için NSX-T Yöneticisi ' ni kullanmanız gerekir.

## <a name="accounts-and-privileges"></a>Hesaplar ve ayrıcalıklar

### <a name="what-accounts-and-privileges-will-i-get-with-my-new-azure-vmware-solution-private-cloud"></a>Yeni Azure VMware çözümü özel bulutum ile hangi hesapları ve ayrıcalıkları alabilirim?

NSX-T Manager 'da vCenter ve yönetici erişimi içindeki bir cloudadmin kullanıcısı için kimlik bilgileri sağlıyoruz. Ayrıca, Azure Active Directory birleştirmek için kullanılabilecek bir CloudAdmin grubu da vardır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

### <a name="can-have-administrator-access-to-esxi-hosts"></a>ESXi konaklarına yönetici erişimi olabilir mi?

Hayır, ESXi 'ye yönetici erişimi, çözümün güvenlik gereksinimlerini karşılayacak şekilde kısıtlıdır.

### <a name="what-privileges-and-permissions-will-i-have-in-vcenter"></a>VCenter 'da hangi ayrıcalıkların ve izinlerin olması gerekir?

CloudAdmin grup ayrıcalıklarınız olacaktır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

### <a name="what-privileges-and-permissions-will-i-have-on-the-nsx-t-manager"></a>NSX-T Manager 'da hangi ayrıcalıkları ve izinleri almam gerekir?

NSX-T üzerinde tam yönetici ayrıcalıklarına sahip olacaksınız ve şirket içi NSX-T veri merkezi ile yaptığınız gibi rol tabanlı erişim denetimini yönetebilirsiniz. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

> [!NOTE]
> Bir T0 yönlendiricisi, özel bulut dağıtımının bir parçası olarak oluşturulur ve yapılandırılır. Bu mantıksal yönlendiricide veya NSX-T Edge düğümü VM 'lerinde yapılan herhangi bir değişiklik, özel bulutunuzun bağlantısını etkileyebilir.

## <a name="billing-and-support"></a>Faturalandırma ve destek

### <a name="how-will-i-be-billed-during-the-preview-of-azure-vmware-solution"></a>Azure VMware çözümünün önizlemesi sırasında nasıl faturalandırılırım?

Önizleme sırasında Azure VMware çözümü için faturalandırma, Kullandıkça Öde temelinde aylık olarak yapılır. Ek Seçenekler genel kullanıma sunulacaktır.

### <a name="how-will-pricing-be-structured-during-the-preview-of-azure-vmware-solution"></a>Azure VMware çözümünün önizlemesi sırasında fiyatlandırma nasıl yapılandırılır?

Fiyatlandırma hakkında genel sorular için bkz. Azure VMware Çözüm [fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-vmware) sayfası. Önizleme fiyatlandırması istek üzerine alınabilir, hesap ekibinize başvurun veya satış ile iletişim kurmak için fiyatlandırma sayfasındaki bağlantıyı izleyin.

### <a name="who-supports-azure-vmware-solution"></a>Azure VMware çözümünü kimler destekler?

Azure VMware çözümü desteği Microsoft tarafından dağıtılır. Göz önünde olmak üzere, önizleme kılavuzlarımıza göre 9-5 PM PST iş saati Pazartesi-Cuma sürecinde destek sağlayacağız. [Bu bağlantıdan](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) bir destek bileti yükseltebilirsiniz

### <a name="what-accounts-do-i-need-to-create-an-azure-vmware-solution-private-cloud"></a>Azure VMware çözümü özel bulutu oluşturmak için hangi hesaplara ihtiyacım var?

Azure aboneliğinde bir Azure hesabınızın olması gerekir.

### <a name="how-do-i-request-a-host-quota-increase-for-azure-vmware-solution"></a>Nasıl yaparım? Azure VMware çözümü için bir konak kotası artışı ister misiniz?

* Microsoft ile bir [Azure Kurumsal Anlaşma (EA)](https://docs.microsoft.com/azure/cost-management-billing/manage/ea-portal-agreements) gerekir.
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
