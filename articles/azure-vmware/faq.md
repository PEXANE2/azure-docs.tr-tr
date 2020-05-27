---
title: Sık sorulan sorular
description: Azure VMware çözümü (AVS) ile ilgili bazı yaygın soruların yanıtlarını sağlar.
ms.topic: conceptual
ms.date: 05/04/2020
ms.author: dikamath
ms.openlocfilehash: c318a17e433f40b17e3dd9e3e95a655ecb48a160
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873312"
---
# <a name="frequently-asked-questions-about-azure-vmware-solution-avs-preview"></a>Azure VMware çözümü (AVS) önizleme hakkında sık sorulan sorular

Azure VMware çözümü (AVS) hakkında sık sorulan sorular için yanıtlar.

## <a name="general"></a>Genel

**Azure VMware çözümü (AVS) nedir?**

Kuruluşlar, iş çevikliği geliştirmek, maliyetleri azaltmak ve yeniliklerini hızlandırmak için strateji yaparken, hibrit bulut platformları, müşterilerin dijital dönüşümünde önemli etkinleştiriciler olarak ortaya çıktı. AVS, VMware 'nin yazılım tanımlı veri merkezi (SDDC) yazılımını Microsoft Azure küresel bulut hizmeti ekosistemi ile birleştirir. AVS çözümü, performans, kullanılabilirlik, güvenlik ve uyumluluk gereksinimlerini karşılayacak şekilde yönetilir.

## <a name="avs-service"></a>AVS hizmeti

**AVS bugün nerede mevcuttur?**

Önizleme sırasında, Batı Avrupa 'daki Kuzey Amerika ve Amsterdam ' de ABD Doğu kullanılabilir.

**Bir Azure VMware çözümü (AVS) örneğinde çalışan iş yükleri Azure hizmetleriyle tüketiliyor veya tümleştirilmesine neden olabilir?**

Tüm Azure Hizmetleri, AVS çözüm müşterileri tarafından kullanılabilir. Belirli hizmetler için performans ve kullanılabilirlik sınırlamaları, büyük/küçük harfe göre değinilmesi gerekecektir.

**Özel bulut kaynaklarını yönetmek için şu anda kullandığım araçları kullanıyorum?**

Evet. Azure portal dağıtım ve bir dizi yönetim işlemi için kullanılır. vCenter ve NSX Yöneticisi, vSphere ve NSX-T kaynaklarını yönetmek için kullanılır.

**Şirket içi vCenter 'um ile özel bir bulutu yönetebilir miyim?**

Başlatma sırasında, AVS şirket içi ve özel bulut ortamlarında tek bir yönetim deneyimini desteklemez. Özel bulut kümeleri, özel bir bulutta vCenter ve NSX Yöneticisi yerel ile yönetilir.

**Şirket içinde çalışan VNET paketini kullanabilir miyim?** 

Belirli tümleştirmeler ve kullanım durumları, büyük/küçük harf esasına göre değerlendirilebilir.

**VSphere sanal makinelerini şirket içi ortamlardan AVS özel bulutlarına geçirebilir miyim?**

Evet. Standart çapraz vCenter [vMotion Requirements] [] karşılanıyorsa VM 'Leri özel buluta taşımak için VM geçişi ve vMotion kullanılabilir https://kb.vmware.com/s/article/210695 .

**Şirket içi ortamlarda gerekli olan belirli bir vSphere sürümü mi?**

Tüm bulut ortamları, vMotion 'un Şirket içi ortamlarında HCX, vSphere 5,5 veya üzeri sürümlerle birlikte geldiğinden.

**Değişiklik denetimi işlemi nasıl görünür?**

Hizmette yapılan güncelleştirmeler, Microsoft Azure Standart değişiklik yönetimi işlemini izler. Müşteriler tüm iş yükü yönetim görevlerinden ve ilişkili değişiklik yönetimi işlemlerinden sorumludur.

**Bu, CloudSimple tarafından Azure VMware çözümünden farklı midir?**

Yeni Azure VMware çözümüyle, Microsoft ve VMware 'nin doğrudan bir bulut sağlayıcısı ortaklığı vardır. Yeni çözüm tamamen tasarlanır, Microsoft tarafından oluşturulup desteklenir ve VMware tarafından doğrulanmıştır. Mimari türsel olarak, çözümler Azure adanmış bir altyapıda çalışan VMware teknoloji yığını ile tutarlıdır.

**Var olan bir Azure VMware Çözüm müşterisiyseniz, bu önizleme benim için ne anlama geliyor?**

CloudSimple ile mevcut Azure VMware çözümünde değişiklik yapılmaz. Azure 'da çözümü desteklemeye devam ediyoruz. Azure VMware çözümü, hizmet düzeyi sözleşmenizde [(SLA)](https://aka.ms/CSVMwareSLA)desteklenir. Müşteriler, üretim iş yükleri için hizmeti kullanmaya devam etmelidir; Bu, [Microsoft 'un hizmet koşullarına](https://azure.microsoft.com/support/legal/)göre sunulan bir çözümdür.

**Bu yeni çözüme CloudSimple ile Azure VMware çözümünden geçiş yapabilir miyim?**

Evet, Azure VMware çözümü, HCX gibi tanıdık VMware araçları kullanılarak geçişi destekler. Yeni çözüme geçiş yapmak isteyen müşteriler için lütfen seçenekleri ve kullanılabilir desteği incelemek üzere Microsoft hesabı ekibinizle birlikte çalışın.

<a name="how-to-request-a-quota-increase-for-existing-avs"></a>**Nasıl yaparım? var olan bir Azure VMware çözümü için bir konak kotası artışı ister misiniz?**

Bir [destek isteği göndererek](..\azure-portal\supportability\how-to-create-azure-support-request.md)kota artışı isteyebilirsiniz. Kota yönetimi ekibi, isteği değerlendirir ve üç iş günü içinde onaylar.  

> [!IMPORTANT]
> Kota artışı isteyebilmeniz için önce [ **MICROSOFT. AVS** kaynak sağlayıcısını](tutorial-create-private-cloud.md) Azure Portal kaydettiğinizden emin olun.  
> ```azurecli-interactive
> az provider register -n Microsoft.AVS --subscription <your subscription ID>
> ```
> Kaynak sağlayıcısını kaydetmek için ek yollar için bkz. [Azure kaynak sağlayıcıları ve türleri](https://review.docs.microsoft.com/en-us/azure/azure-resource-manager/management/resource-providers-and-types).

1. Azure portal, **Yardım + Destek**altında, **Yeni bir destek isteği** oluşturun ve bilet için aşağıdaki bilgileri sağlayın:
   - **Sorun türü:** Teknik
   - **Abonelik:** Abonelik KIMLIĞINIZ
   - **Hizmet:**  Azure VMware çözümü 
   - **Özet:** Kota artışı
   - **Sorun türü:** Kapasite yönetimi sorunları
   - **Sorun alt türü:** Ek konak kotası/kapasitesi için müşteri Isteği

1. Destek bileti açıklamasında, Ayrıntılar sekmesinde şunları sağlayın:
   - Ek düğüm sayısı   
   - Düğüm SKU 'SU
   - Bölge

   > [!NOTE] 
   > Varsayılan olarak, en az dört düğüm verilecektir.

1. İsteği göndermek için **gözden geçir + oluştur** ' a tıklayın.

## <a name="compute-network-and-storage"></a>İşlem, ağ ve depolama

**Birden fazla konak türü var mı?**

Kullanılabilir tek bir konak türü vardır.

**Her konak türünde CPU belirtimleri nelerdir?**

Sunucularda iki adet 18 çekirdekli 2,3 GHz Intel CPU vardır.

**Her konakta ne kadar bellek var?**

Sunucularda 576 GB RAM vardır.

**Her Konağın depolama kapasitesi nedir?**

Her ESXi konağında, 15,2 TB 'lik kapasite katmanı ve 3,2 TB NVMe önbellek katmanı (her diskgroup 'ta 1,6 TB) içeren iki VSAN diskgroups vardır.

**Her ESXi konağında kullanılabilir ağ bant genişliği miktarı nedir?**

ESXi Konakları, 25 Gbps 'e kadar bağlantı bant genişliğini destekler.

**Veriler, bekleyen VSAN veri depoları üzerinde mi depolanıyor?**

Evet, tüm VSAN verileri, Azure Key Vault depolanan anahtarlar kullanılarak varsayılan olarak şifrelenir.

## <a name="hosts-clusters-and-private-clouds"></a>Konaklar, kümeler ve özel bulutlar

**Temel alınan altyapı paylaşılıyor mu?**

Hayır, özel bulut konakları ve kümeleri ayrılmış ve kullanılmadan önce ve güvenli bir şekilde silinir.

**Küme başına en düşük ve en yüksek ana bilgisayar sayısı nelerdir?**

Kümeler 3 ila 16 ESXi konakları arasında ölçeklendirebilir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

**Özel bulut kümelerimi ölçeklendirebilir miyim?**

Evet, kümeler en düşük ve en fazla ESXi ana bilgisayarı sayısı arasında ölçeklenir. Deneme kümeleri üç ana bilgisayar ile sınırlıdır.

**Deneme kümeleri nedir?**

Deneme kümeleri, AVS özel bulutlarının bir aylık değerlendirmesi için kullanılan üç ana bilgisayar kümelerinden biridir.

**Deneme kümeleri için yüksek kaliteli Konakları kullanabilir miyim?**

Hayır. Yüksek kaliteli ESXi Konakları üretim kümelerinde kullanılmak üzere ayrılmıştır.

## <a name="avs-and-vmware-software"></a>AVS ve VMware yazılımı

**Özel bulutlarda VMware yazılımının hangi sürümleri kullanılıyor?**

Özel bulutlar vSphere 6,7, vSAN 6,7, HCX ve NSX-T sürüm 2,5 ' i kullanır.  

**Özel bulutlar VMware NSX kullanıyor mu?**

Evet, NSX-T 2,5, AVS özel bulutlarında yazılım tanımlı ağ için kullanılır.

**VMware NSX-V ' y I özel bir bulutta kullanabilir miyim?**

Hayır. NSX-T, NSX 'in desteklenen tek sürümüdür.

**NSX, şirket içi ortamlarda veya özel bir buluta bağlanan ağlarda gereklidir.**

Hayır, NSX 'i şirket içinde kullanmak zorunda değilsiniz.

**Özel buluttaki VMware yazılımı için yükseltme ve güncelleştirme zamanlaması nedir?**

Özel bulut yazılım paketi yükseltmeleri, yazılımın en son sürümü olan VMware uygulamasının tek bir sürümünde kalmasını sağlamak için yapılır. Özel bulut yazılımı sürümleri, bireysel yazılım bileşenlerinin en son sürümlerinden (ESXi, NSX-T, vCenter, VSAN) farklı olabilir.

**Özel bulut yazılım yığını ne sıklıkla güncelleştirilir?**

Özel bulut yazılımı, VMware 'den yazılım paketi yayını ile izleyen bir zamanlamaya göre yükseltilir. Özel bulutunuz yükseltmeler için kapalı kalma süresi gerektirmez.

## <a name="connectivity"></a>Bağlantı

**Şirket içi ortamlarla özel bulutlar eklemek için hangi ağ IP adresi planlaması gerekir?**

Bir AVS özel bulutu dağıtmak için özel bir ağ/22 adres alanı gerekir. Bu özel adres alanı, bir abonelikteki diğer sanal ağlarla veya şirket içi ağlarla çakışmamalıdır.
 
**Şirket içi ortamlardan bir AVS özel bulutuna mi bağlanmak Nasıl yaparım??**

Hizmete iki yöntemden birini kullanabilirsiniz: 

- Özel buluta ExpressRoute aracılığıyla eşlenen bir Azure sanal ağı üzerinde dağıtılan bir VM veya uygulama ağ geçidi ile.
- ExpressRoute aracılığıyla şirket içi veri merkezinizden bir Azure ExpressRoute devresine Global Reach.

**Bir iş yükü VM 'sini Internet 'e veya Azure hizmeti uç noktasına bağlamak Nasıl yaparım?.**

Azure portal bir özel bulut için internet bağlantısını etkinleştirin. NSX-T Yöneticisi ile bir NSX-T T1 yönlendiricisi ve bir mantıksal anahtar oluşturun. Ardından, mantıksal anahtar tarafından tanımlanan ağ kesimine bir VM dağıtmak için vCenter 'ı kullanırsınız. Bu VM, internet ve Azure hizmetleri için ağ erişimine sahip olacaktır.

**İnternet 'ten özel bir buluttaki mantıksal ağlardaki VM 'lere erişimi kısıtlayacağım mi?**

Hayır. İnternet 'ten özel bulutlara doğrudan gelen ağ trafiğine izin verilmez.

**Mantıksal ağlardaki VM 'lerden internet 'e erişimi kısıtlam gerekir mi?**

Evet. İnternet 'e yönelik VM erişimini kısıtlayan bir güvenlik duvarı oluşturmak için NSX-T Yöneticisi ' ni kullanmanız gerekir.

## <a name="accounts-and-privileges"></a>Hesaplar ve ayrıcalıklar

**Yeni AVS özel bulutum ile hangi hesapları ve ayrıcalıkları alabilirim?**

NSX-T Manager 'da vCenter ve yönetici erişimi içindeki bir cloudadmin kullanıcısı için kimlik bilgileri sağlıyoruz. Ayrıca, Azure Active Directory birleştirmek için kullanılabilecek bir CloudAdmin grubu da vardır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

**ESXi konaklarına yönetici erişimi olabilir mi?**

Hayır, ESXi 'ye yönetici erişimi, çözümün güvenlik gereksinimlerini karşılayacak şekilde kısıtlıdır.

**VCenter 'da hangi ayrıcalıkların ve izinlerin olması gerekir?**

CloudAdmin grup ayrıcalıklarınız olacaktır. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

**NSX-T Manager 'da hangi ayrıcalıkları ve izinleri almam gerekir?**

NSX-T üzerinde tam yönetici ayrıcalıklarına sahip olacaksınız ve şirket içi NSX-T veri merkezi ile yaptığınız gibi rol tabanlı erişim denetimini yönetebilirsiniz. Daha fazla bilgi için bkz. [erişim ve kimlik kavramları](concepts-identity.md).

> [!NOTE]
> Bir T0 yönlendiricisi, özel bulut dağıtımının bir parçası olarak oluşturulur ve yapılandırılır. Bu mantıksal yönlendiricide veya NSX-T Edge düğümü VM 'lerinde yapılan herhangi bir değişiklik, özel bulutunuzun bağlantısını etkileyebilir.

## <a name="billing-and-support"></a>Faturalandırma ve destek

**AVS 'nin önizlemesi boyunca nasıl faturalandırılırım?**

Önizleme süresince AVS için faturalandırma, Kullandıkça Öde esasına göre aylık olarak yapılır. Ek Seçenekler genel kullanıma sunulacaktır.

**AVS 'nin önizlemesi sırasında fiyatlandırma nasıl yapılandırılır?**

Fiyatlandırma hakkında genel sorular için bkz. Azure VMware Çözüm [fiyatlandırma](https://azure.microsoft.com/pricing/details/azure-vmware) sayfası. Önizleme fiyatlandırması istek üzerine alınabilir, lütfen hesap ekibinize başvurun veya satış ile iletişim kurmak için fiyatlandırma sayfasındaki bağlantıyı izleyin.

**AVS 'yi destekliyor mu?**

AVS desteği Microsoft tarafından dağıtılır. Lütfen önizleme yönergelerimize göre 9-5 pm pasıfık saati Pazartesi-Cuma arasında destek sağlayacağız. [Bu bağlantıdan](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) bir destek bileti yükseltebilirsiniz

**AVS özel bulutu oluşturmak için hangi hesaplara ihtiyacım var?**

Azure aboneliğinde bir Azure hesabınızın olması gerekir.

<!-- LINKS - external -->
[kb2106952]: https://kb.vmware.com/s/article/2106952

<!-- LINKS - internal -->
[Access and Identity Concepts]: concepts-identity.md
