---
title: Azure DevTest Labs altyapı yönetimi-kaynak
description: Bu makalede, kuruluşunuzdaki Azure DevTest Labs yönelik kaynakların hizalaması ve yönetimi ele alınmaktadır.
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
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84899266"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---resources"></a>Azure DevTest Labs altyapı yönetimi-kaynaklar
Bu makalede, kuruluşunuzdaki DevTest Labs için kaynakların hizalaması ve yönetimi ele alınmaktadır. 

## <a name="align-within-an-azure-subscription"></a>Bir Azure aboneliği içinde hizalayın 

### <a name="question"></a>Soru
Azure aboneliği içindeki DevTest Labs kaynaklarını Nasıl yaparım? hizalayın?

### <a name="answer"></a>Yanıt
Bir kuruluş genel uygulama geliştirme için Azure 'u kullanmaya başlamadan önce, BT planlayıcıları öncelikle genel hizmet portföyünün bir parçası olarak özelliğin nasıl tanıtılmasını incelemelidir. İnceleme için alanlarda aşağıdaki kaygıları ele alınmalıdır:

- Uygulama geliştirme yaşam döngüsü ile ilişkili maliyet nasıl ölçülmektedir?
- Kuruluş, önerilen hizmet teklifini kurumsal güvenlik ilkesiyle nasıl hizalasın? 
- Geliştirme ve üretim ortamlarını ayırmak için segmentleme gereklidir mi? 
- Uzun süreli yönetim, kararlılık ve büyüme için hangi denetimler tanıtılmaktadır?

**Önerilen ilk yöntem** , kuruluşların Azure taksonomi 'yi, üretim ve geliştirme abonelikleri arasındaki bölümlerin ana hatlarıyla incelenmektir. Aşağıdaki diyagramda, önerilen taksonomi geliştirme/test ve üretim ortamlarının mantıksal ayrımı için izin verir. Bu yaklaşım sayesinde bir kuruluş, her ortamla ilişkili maliyetleri ayrı ayrı izlemek için fatura kodları ortaya çıkarabilir. Daha fazla bilgi için bkz. [seçkin abonelik](/azure/architecture/cloud-adoption/appendix/azure-scaffold)İdaresi. Ayrıca, izleme ve faturalama amaçlarıyla kaynakları düzenlemek için [Azure etiketlerini](../azure-resource-manager/management/tag-resources.md) de kullanabilirsiniz.

Azure Enterprise Portal içinde DevTest aboneliğini etkinleştirmek **Önerilen ikinci uygulamadır** . Bir kuruluşun, genellikle bir Azure Kurumsal aboneliğinde bulunmayan istemci işletim sistemlerini çalıştırmasına izin verir. Daha sonra, yalnızca işlem için ödeme yapacağınız ve lisanslama hakkında endişelenmeniz gereken kurumsal yazılımları kullanın. Microsoft SQL Server gibi, IaaS içindeki galeri görüntüleri de dahil olmak üzere, belirlenen hizmetlerin faturalandırmasını yalnızca tüketimine göre sağlar. Azure DevTest aboneliğine ilişkin ayrıntılar [burada](https://azure.microsoft.com/offers/ms-azr-0148p/) kurumsal anlaşma (EA) müşterileri Için ve kullandıkça öde müşterileri için [burada bulunabilir](https://azure.microsoft.com/offers/ms-azr-0023p/) .

![Aboneliklerle kaynak hizalaması](./media/devtest-lab-guidance-governance/resource-alignment-with-subscriptions.png)

Bu model, bir kuruluşa Azure DevTest Labs dağıtma esnekliği sağlar. Bir kuruluş, 100 ile 1000 arasındaki sanal makinelere paralel olarak çalışan çeşitli iş birimleri için yüzlerce Laboratuvarı destekleyebilir. Aynı yapılandırma yönetimi ve güvenlik denetimleri ilkelerini paylaşabilen merkezi bir kurumsal laboratuvar çözümü kavramını yükseltir.

Bu model Ayrıca, kuruluşun Azure aboneliğiyle ilişkili kaynak sınırlarını tüketmemesini sağlar. Abonelik ve hizmet limitleri hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../azure-resource-manager/management/azure-subscription-service-limits.md). DevTest Labs sağlama işlemi çok sayıda kaynak grubu kullanabilir. Limitlerin Azure DevTest aboneliğindeki bir destek isteğiyle artırılmasını isteyebilirsiniz. Geliştirme aboneliği kullanımda büyüdükçe, üretim aboneliği içindeki kaynaklar etkilenmez. DevTest Labs 'i ölçeklendirme hakkında daha fazla bilgi için bkz. [DevTest Labs 'de ölçek kotaları ve sınırları](devtest-lab-scale-lab.md).

İçin atanması gereken ortak bir abonelik düzeyi sınırı, hem üretim hem de geliştirme aboneliklerini desteklemek için ağ IP aralığı atamalarının nasıl ayrıldığı. Bu atamalar zaman içinde büyüme (Şirket içi bağlantının veya kuruluşun Azure 'un uygulamasına varsayılan olarak kullanmak yerine ağ yığınını yönetmesini gerektiren başka bir ağ topolojisi varsayarak) için hesap almalıdır. Önerilen uygulama, büyük bir IP adresi ön ekine sahip ve küçük alt ağlarla birden çok sanal ağa sahip olmak yerine birçok büyük alt ağa ayrılmış olan birkaç sanal ağa sahip olmalıdır. Örneğin, 10 abonelikle, 10 sanal ağ (her abonelik için bir tane) tanımlayabilirsiniz. Yalıtım gerektirmeyen tüm laboratuvarlar, aboneliğin VNET 'inde aynı alt ağı paylaşabilir.

## <a name="maintain-naming-conventions"></a>Adlandırma kurallarını koruma

### <a name="question"></a>Soru
Nasıl yaparım? DevTest Labs ortamımı genelinde bir adlandırma kuralına sahip mi?

### <a name="answer"></a>Yanıt
Geçerli kurumsal adlandırma kurallarını Azure işlemlerine genişletmek ve bunları DevTest Labs ortamında tutarlı hale getirmek isteyebilirsiniz.

DevTest Labs dağıtımı yaparken, belirli başlangıç ilkelerine sahip olmanız önerilir. Bu ilkeleri, tutarlılığı zorlamak için bir merkezi betik ve JSON şablonları ile dağıtırsınız. Adlandırma ilkeleri, abonelik düzeyinde uygulanan Azure ilkeleri aracılığıyla uygulanabilir. Azure Ilkesi için JSON örnekleri için bkz. [Azure ilke örnekleri](../governance/policy/samples/index.md).

## <a name="number-of-users-per-lab-and-labs-per-organization"></a>Laboratuvar başına Kullanıcı sayısı ve kuruluş başına Labs

### <a name="question"></a>Soru 
Nasıl yaparım? laboratuvar başına Kullanıcı oranını ve bir kuruluş genelinde gereken laboratuvarların genel sayısını belirlemektir mi?

### <a name="answer"></a>Yanıt
Aynı geliştirme projesiyle ilişkili iş birimlerinin ve geliştirme gruplarının aynı laboratuvarda ilişkili olmasını öneririz. Her iki gruba da aynı ilke, görüntü ve kapalı ilke türlerinin uygulanmasını sağlar. 

Coğrafi sınırları da dikkate almanız gerekebilir. Örneğin, Kuzey Doğu Birleşik Devletler (US) geliştiriciler Doğu ABD2 sağlanan bir laboratuvarı kullanabilir. Ve Dallas, Texas ve Denver 'daki geliştiriciler ABD Orta Güney bir kaynağı kullanmak için yönlendirilebilir. Dış üçüncü taraflarla işbirliğine dayalı bir çaba varsa, bunlar dahili geliştiriciler tarafından kullanılmayan bir laboratuvara atanabilir. 

Ayrıca, Azure DevOps Projeleri içindeki belirli bir proje için de laboratuvar kullanabilirsiniz. Daha sonra, her iki kaynak kümesine erişime izin veren belirtilen bir Azure Active Directory grubu aracılığıyla güvenliği uygularsınız. Laboratuvara atanan sanal ağ, kullanıcıları birleştirmek için başka bir sınır olabilir.

## <a name="deletion-of-resources"></a>Kaynakları silme

### <a name="question"></a>Soru
Laboratuvardaki kaynakların silinmesini nasıl önleyebiliriz?

### <a name="answer"></a>Yanıt
Yalnızca yetkili kullanıcıların kaynakları silmesi veya laboratuvar ilkelerini değiştirmesi için uygun izinleri laboratuvar düzeyinde ayarlamanızı öneririz. Geliştiricilerin **DevTest Labs Kullanıcı** grubu içine yerleştirilmesi gerekir. Lider geliştiricisi veya altyapı lideri, **DevTest Labs sahibi**olmalıdır. Yalnızca iki laboratuar sahibi olmasını öneririz. Bu ilke, bozulmayı önlemek için kod deposuna doğru genişletilir. Laboratuvar kullanımları kaynakları kullanma hakkına sahiptir ancak laboratuvar ilkelerini güncelleştiremez. Her bir yerleşik grubun bir laboratuvar dahilinde sahip olduğu rolleri ve hakları listeleyen aşağıdaki makaleye bakın: [Azure DevTest Labs sahip ve Kullanıcı ekleme](devtest-lab-add-devtest-user.md).

## <a name="move-lab-to-another-resource-group"></a>Laboratuvarı başka bir kaynak grubuna taşıma 

### <a name="question"></a>Soru
Bir laboratuvarın başka bir kaynak grubuna taşınması destekleniyor mu?

### <a name="answer"></a>Yanıt
Evet. Laboratuvarınızın giriş sayfasından kaynak grubu sayfasına gidin. Ardından, araç çubuğunda **Taşı** ' yı seçin ve farklı bir kaynak grubuna taşımak istediğiniz Laboratuvarı seçin. Bir laboratuvar oluşturduğunuzda sizin için otomatik olarak bir kaynak grubu oluşturulur. Ancak, Laboratuvarı kurumsal adlandırma kurallarını izleyen farklı bir kaynak grubuna taşımak isteyebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
Bkz. [maliyeti ve sahipliği yönetme](devtest-lab-guidance-governance-cost-ownership.md).
