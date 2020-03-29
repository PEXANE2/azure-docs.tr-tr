---
title: Azure DevTest Labs altyapısının yönetimi - Kaynak
description: Bu makale, kuruluşunuzdaki Azure DevTest Labs için kaynakların hizalanması ve yönetilmesini ele alır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/26/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: 8bb00c770c61a0a5462a01ae552bd7e40a7cdb36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77470658"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---resources"></a>Azure DevTest Labs altyapısının yönetimi - Kaynaklar
Bu makalede, kuruluşunuzdaki DevTest Labs kaynaklarının hizalanması ve yönetimi ele alılmıştır. 

## <a name="align-within-an-azure-subscription"></a>Azure aboneliği içinde hizalama 

### <a name="question"></a>Soru
Bir Azure aboneliği nde DevTest Labs kaynaklarını nasıl hizalayabilirim?

### <a name="answer"></a>Yanıt
Bir kuruluş genel uygulama geliştirme için Azure'u kullanmaya başlamadan önce, BT planlayıcılarının bu özelliği genel hizmet portföyünün bir parçası olarak nasıl tanıtacaklarını öncelikle gözden geçirmeleri gerekir. Gözden geçirilen alanlar aşağıdaki endişeleri ele almalıdır:

- Uygulama geliştirme yaşam döngüsüyle ilişkili maliyet nasıl ölçülür?
- Kuruluş önerilen hizmet teklifini kurumsal güvenlik ilkesiyle nasıl hizalar? 
- Geliştirme ve üretim ortamlarını ayırmak için segmentasyon gerekli midir? 
- Uzun vadeli yönetim kolaylığı, istikrar ve büyüme için hangi kontroller getirilir?

**Önerilen ilk uygulama,** üretim ve geliştirme abonelikleri arasındaki bölümlerin ana hatlarıyla belirtildiği kuruluşların Azure taksonomisini gözden geçirmektir. Aşağıdaki diyagramda, önerilen taksonomi geliştirme/test ve üretim ortamlarının mantıksal olarak ayrılmasına olanak sağlar. Bu yaklaşımla, bir kuruluş her ortamla ilişkili maliyetleri ayrı ayrı izlemek için faturalandırma kodları tanıtabilir. Daha fazla bilgi için, [Açıklayıcı abonelik yönetimi](/azure/architecture/cloud-adoption/appendix/azure-scaffold)ne bıyiklük. Ayrıca, kaynakları izleme ve faturalandırma amacıyla düzenlemek için [Azure etiketlerini](../azure-resource-manager/management/tag-resources.md) kullanabilirsiniz.

**Önerilen ikinci uygulama,** Azure Kurumsal portalında DevTest aboneliğini etkinleştirmektir. Bir kuruluşun, genellikle Azure kurumsal aboneliğinde bulunmayan istemci işletim sistemlerini çalıştırmasına olanak tanır. Ardından, yalnızca işlem için ödeme yaptığınız ve lisanslama konusunda endişelenmeyin kurumsal yazılımı kullanın. Microsoft SQL Server gibi IaaS'deki galeri resimleri de dahil olmak üzere, belirlenmiş hizmetlerin faturalandırmasının yalnızca tüketime dayalı olmasını sağlar. Azure DevTest aboneliğiyle ilgili ayrıntılar, Kurumsal Sözleşme (EA) müşterileri için [burada,](https://azure.microsoft.com/offers/ms-azr-0148p/) Siz Müşteriler e ve Öde müşterileri için [de](https://azure.microsoft.com/offers/ms-azr-0023p/) bulunabilir.

![Aboneliklerle kaynak hizalaması](./media/devtest-lab-guidance-governance/resource-alignment-with-subscriptions.png)

Bu model, bir kuruluşa Azure DevTest Laboratuvarlarını ölçekte dağıtma esnekliği sağlar. Bir kuruluş, paralel çalışan 100 ila 1000 sanal makine ile çeşitli iş birimleri için yüzlerce laboratuvar destekleyebilir. Yapılandırma yönetimi ve güvenlik denetimleri ile aynı ilkeleri paylaşabilen merkezi bir kurumsal laboratuvar çözümü kavramını destekler.

Bu model, kuruluşun Azure abonelikleriyle ilişkili kaynak sınırlarını tüketmemesini de sağlar. Abonelik ve hizmet sınırları hakkında ayrıntılı bilgi için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md)bölümüne bakın. DevTest Labs sağlama işlemi çok sayıda kaynak grubu tüketebilir. Azure DevTest aboneliğinde bir destek isteği aracılığıyla sınırların artırılmasını isteyebilirsiniz. Geliştirme aboneliği kullanımda büyüdükçe üretim aboneliği içindeki kaynaklar etkilenmez. DevTest Labs ölçekleme hakkında daha fazla bilgi için, [DevTest Labs ölçek kotaları ve sınırları](devtest-lab-scale-lab.md)bakın.

Hesaba katılmaması gereken ortak bir abonelik düzeyi sınırı, ağ IP aralığı atamalarının hem üretim hem de geliştirme aboneliklerini desteklemek için nasıl tahsis edildiğidir. Bu atamalar zaman içinde büyümeyi hesaba katmalıdır (şirket içi bağlantı veya kurumsalın Azure'un uygulanmasını varsayılan olarak yerine ağ yığınını yönetmesini gerektiren başka bir ağ topolojisi varsayarsak). Önerilen uygulama, küçük alt ağlara sahip birden çok sanal ağa sahip olmak yerine, büyük bir IP adresi öneki atanmış ve birçok büyük alt ağlarile bölünmüş birkaç sanal ağa sahip olmaktır. Örneğin, 10 abonelikle 10 sanal ağ (her abonelik için bir tane) tanımlayabilirsiniz. Yalıtım gerektirmeyen tüm laboratuvarlar, aboneliğin vnet'inde aynı alt ağı paylaşabilir.

## <a name="maintain-naming-conventions"></a>Adlandırma kurallarını koruma

### <a name="question"></a>Soru
DevTest Labs ortamımda bir adlandırma kuralını nasıl korurum?

### <a name="answer"></a>Yanıt
Geçerli kurumsal adlandırma sözleşmelerini Azure işlemlerine genişletmek ve Bunları DevTest Labs ortamında tutarlı hale getirmek isteyebilirsiniz.

DevTest Labs'ı dağıtırken, belirli başlangıç ilkelerine sahip olduğunuzu öneririz. Tutarlılığı zorlamak için bu ilkeleri merkezi bir komut dosyasına ve JSON şablonlarına göre dağıtın. Adlandırma ilkeleri, abonelik düzeyinde uygulanan Azure ilkeleri aracılığıyla uygulanabilir. Azure İlkesi için JSON örnekleri için [Azure İlkesi örneklerine](../governance/policy/samples/index.md)bakın.

## <a name="number-of-users-per-lab-and-labs-per-organization"></a>Laboratuvar başına kullanıcı sayısı ve kuruluş başına laboratuvar sayısı

### <a name="question"></a>Soru 
Laboratuvar başına kullanıcı oranını ve bir kuruluş genelinde gereken toplam laboratuvar sayısını nasıl belirleyebilirim?

### <a name="answer"></a>Yanıt
Aynı geliştirme projesiyle ilişkili iş birimlerinin ve geliştirme gruplarının aynı laboratuvarla ilişkilendirilmesini öneririz. Her iki gruba da aynı tür ilkeler, görüntüler ve kapatma ilkeleri uygulanmasına olanak tanır. 

Coğrafi sınırları da göz önünde bulundurmanız gerekebilir. Örneğin, kuzey doğu Amerika Birleşik Devletleri 'ndeki (ABD) geliştiriciler Doğu US2'de bulunan bir laboratuvar kullanabilirler. Dallas, Teksas ve Denver, Colorado'daki geliştiriciler, ABD Güney Merkez'de bir kaynak kullanmaya yönlendirilebilir. Harici bir üçüncü tarafla ortak bir çaba varsa, bunlar dahili geliştiriciler tarafından kullanılmayan bir laboratuvara atanabilir. 

Azure DevOps projeleri içinde belirli bir proje için de bir laboratuvar kullanabilirsiniz. Ardından, güvenliği, her iki kaynağa da erişim sağlayan belirli bir Azure Etkin Dizin grubu aracılığıyla uygularsınız. Laboratuvara atanan sanal ağ, kullanıcıları birleştirmek için başka bir sınır olabilir.

## <a name="deletion-of-resources"></a>Kaynakların silinmesi

### <a name="question"></a>Soru
Laboratuvardaki kaynakların silinmesini nasıl önleyebiliriz?

### <a name="answer"></a>Yanıt
Yalnızca yetkili kullanıcıların kaynakları silebilmeleri veya laboratuvar ilkelerini değiştirebilmeleri için uygun izinleri laboratuvar düzeyinde ayarlamanızı öneririz. Geliştiriciler **DevTest Labs Kullanıcıları** grubuna yerleştirilmelidir. Müşteri adayı geliştirici veya altyapı lideri **DevTest Labs Sahibi**olmalıdır. Yalnızca iki laboratuvar sahibiniz olduğunu öneririz. Bu ilke, bozulmayı önlemek için kod deposuna doğru uzanır. Laboratuvar kullanımları kaynakları kullanma haklarına sahiptir, ancak laboratuvar ilkelerini güncelleştiremez. Her yerleşik grubun bir laboratuvarda sahip olduğu rolleri ve hakları listeleyen aşağıdaki makaleye bakın: [Azure DevTest Labs'daki sahiplerini ve kullanıcılarını ekleyin.](devtest-lab-add-devtest-user.md)

## <a name="move-lab-to-another-resource-group"></a>Laboratuarı başka bir kaynak grubuna taşıma 

### <a name="question"></a>Soru
Bir laboratuarı başka bir Kaynak Grubuna taşımak desteklenir mi?

### <a name="answer"></a>Yanıt
Evet. Laboratuvarınızın ana sayfasından Kaynak Grubu sayfasına gidin. Ardından, araç çubuğunda **Taşı'yı** seçin ve farklı bir kaynak grubuna taşımak istediğiniz laboratuarı seçin. Bir laboratuvar oluşturduğunuzda, sizin için otomatik olarak bir kaynak grubu oluşturulur. Ancak, laboratuarı kuruluş adlandırma kurallarını izleyen farklı bir kaynak grubuna taşımak isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [Maliyet ve sahipliği yönet.](devtest-lab-guidance-governance-cost-ownership.md)
