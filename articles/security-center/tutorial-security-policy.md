---
title: Güvenlik ilkeleriyle çalışma | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nde güvenlik ilkeleriyle nasıl çalışılacağı açıklanır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2019
ms.author: memildin
ms.openlocfilehash: fce8c75dadbb255a357b85bf85049423f50d18ec
ms.sourcegitcommit: 9f330c3393a283faedaf9aa75b9fcfc06118b124
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "71996342"
---
# <a name="working-with-security-policies"></a>Güvenlik ilkeleriyle çalışma

Bu makalede güvenlik ilkelerinin nasıl yapılandırıldığı ve Güvenlik Merkezi 'nde nasıl görüntüleneceği açıklanmaktadır. Azure Güvenlik Merkezi, eklendi olan her bir aboneliğe [yerleşik güvenlik ilkelerini](security-center-policy-definitions.md) otomatik olarak atar. Bunları [Azure ilkesinde](../governance/policy/overview.md)yapılandırabilir, bu da yönetim grupları ve birden çok abonelik arasında ilkeler ayarlamanıza olanak sağlar.

PowerShell kullanarak ilkelerin nasıl ayarlanacağı hakkında yönergeler için bkz. [hızlı başlangıç: Azure PowerShell modülünü kullanarak uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma](../governance/policy/assign-policy-powershell.md).

>[!NOTE]
> Güvenlik Merkezi, Azure Ilkesiyle tümleştirmesini başlattı. Mevcut müşteriler, güvenlik merkezi 'ndeki önceki güvenlik ilkeleri yerine Azure Ilkesinde yeni yerleşik girişim 'e otomatik olarak geçirilecektir. Bu değişiklik, Azure Ilkesinde yeni girişim varlığı dışında kaynaklarınızı veya ortamınızı etkilemeyecektir.

## <a name="what-are-security-policies"></a>Güvenlik ilkeleri nelerdir?
Güvenlik ilkesi, iş yüklerinizin istenen yapılandırmasını tanımlar ve şirket veya mevzuat güvenlik gereksinimleriyle uyumluluğu sağlamaya yardımcı olur. Azure Ilkesinde, Azure abonelikleriniz için ilkeler tanımlayabilir ve bunları iş yükü türüne veya verilerinizin duyarlılığına uyarlayabilirsiniz. Örneğin, kişisel veriler veya müşteri verileri gibi düzenlenmiş verileri kullanan uygulamalar diğer iş yüklerinden daha yüksek bir güvenlik düzeyi gerektirebilir. Abonelikler arasında veya yönetim gruplarında bir ilke ayarlamak için bunları [Azure ilkesinde](../governance/policy/overview.md)ayarlayın.

Güvenlik ilkeleriniz, Azure Güvenlik Merkezi 'nde aldığınız güvenlik önerilerini ister. Olası güvenlik açıklarını belirlemenize ve tehditleri azaltmanıza yardımcı olması için bunlarla uyumluluğu izleyebilirsiniz. Sizin için uygun olan seçeneği belirleme hakkında daha fazla bilgi için [yerleşik güvenlik ilkeleri](security-center-policy-definitions.md)listesine bakın.

Güvenlik Merkezi 'ni etkinleştirdiğinizde güvenlik merkezi 'nde yerleşik güvenlik ilkesi, Azure Ilkesinde kategori Güvenlik Merkezi altında yerleşik bir girişim olarak yansıtılır. Yerleşik girişim, tüm güvenlik merkezi kayıtlı aboneliklerine (ücretsiz veya standart katmanlara) otomatik olarak atanır. Yerleşik girişim yalnızca denetim ilkelerini içerir.


### <a name="management-groups"></a>Yönetim grupları
Kuruluşunuzun birçok aboneliği varsa, bu abonelikler için erişimi, ilkeleri ve uyumluluğu verimli bir şekilde yönetmeniz için bir yol gerekebilir. Azure Yönetim Grupları, aboneliklerin üzerinde bir kapsam düzeyi sağlar. Abonelikleri "Yönetim grupları" adlı kapsayıcılara düzenler ve idare ilkelerinizi yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki tüm abonelikler, yönetim grubuna uygulanan ilkeleri otomatik olarak devralınır. Her dizine, "kök" yönetim grubu adlı tek bir üst düzey yönetim grubu verilir. Bu kök yönetim grubu, tüm yönetim gruplarının ve aboneliklerinin kendisine katlaması için hiyerarşide yerleşik olarak bulunur. Bu kök yönetim grubu, genel ilkelerin ve RBAC atamalarının dizin düzeyinde uygulanmasını sağlar. Yönetim gruplarını Azure Güvenlik Merkezi ile kullanmak üzere ayarlamak için [Azure Güvenlik Merkezi için kiracı genelinde görünürlük elde](security-center-management-groups.md)edin bölümündeki yönergeleri izleyin.

> [!NOTE]
> Yönetim gruplarının ve aboneliklerinin hiyerarşisini anlamanız önemlidir. Yönetim grupları, kök yönetim ve yönetim grubu erişimi hakkında daha fazla bilgi edinmek için bkz. [Azure Yönetim grupları ile kaynaklarınızı düzenleme](../governance/management-groups/overview.md#root-management-group-for-each-directory) .
>

## <a name="how-security-policies-work"></a>Güvenlik ilkeleri nasıl çalışır?
Güvenlik Merkezi, Azure aboneliklerinizin her biri için otomatik olarak varsayılan bir güvenlik ilkesi oluşturur. Azure Ilkesinde ilkeleri düzenleyerek şunları yapabilirsiniz:
- Yeni ilke tanımları oluşturun.
- Kuruluş içindeki bir kuruluşun tamamını veya bir iş birimini temsil eden Yönetim grupları ve abonelikler arasında ilkeler atayın.
- İlke uyumluluğunu izleyin.

Azure Ilkesi hakkında daha fazla bilgi için bkz. [uyumluluğu zorlamak için Ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md).

Bir Azure ilkesi aşağıdaki bileşenlerden oluşur:

- **İlke** bir kuraldır.
- **Girişim** bir ilke koleksiyonudur.
- **Atama** , bir girişim veya ilkenin belirli bir kapsama (Yönetim grubu, abonelik veya kaynak grubu) bir uygulamadır.

## <a name="view-security-policies"></a>Güvenlik ilkelerini görüntüle

Güvenlik Merkezi 'nde güvenlik ilkelerinizi görüntülemek için:

1. **Güvenlik Merkezi** panosunda **güvenlik ilkesi**' ni seçin.

    ![Ilke yönetimi bölmesi](./media/security-center-policies/security-center-policy-mgt.png)

   **İlke yönetimi** ekranında yönetim gruplarının, aboneliklerin ve çalışma alanlarının yanı sıra yönetim grubu yapınızı da görebilirsiniz.

   > [!NOTE]
   > Güvenlik Merkezi panosu, **abonelik kapsamı** kapsamında, **ilke yönetimi**altında gösterilen aboneliklerin sayısından daha yüksek sayıda abonelik gösterebilir. Abonelik kapsamı, standart, ücretsiz ve "kapsanmayan" aboneliklerin sayısını gösterir. "Kapsanmayan" aboneliklerde Güvenlik Merkezi etkinleştirilmemiştir ve **ilke yönetimi**altında gösterilmez.
   >

2. İlkelerini görüntülemek istediğiniz aboneliği veya yönetim grubunu seçin.

   - **Güvenlik ilkesi** ekranı, seçtiğiniz abonelik veya yönetim grubunda atanan ilkeler tarafından gerçekleştirilen eylemi yansıtır.
   - En üstte, abonelik veya yönetim grubu üzerinde geçerli olan her ilke **atamasını** açmak için belirtilen bağlantıları kullanın. Atamaya erişmek ve ilkeyi düzenlemek veya devre dışı bırakmak için bağlantıları kullanabilirsiniz. Örneğin, belirli bir ilke atamasının Endpoint Protection 'ın etkin bir şekilde reddediyor olduğunu görürseniz, ilkeye erişmek ve bunu düzenlemek veya devre dışı bırakmak için bağlantıyı kullanabilirsiniz.
   - İlke listesinde, ilkenin etkin olan uygulamasını aboneliğinizde veya yönetim grubunuzda görebilirsiniz. Bu, kapsam için uygulanan her bir ilkenin ayarlarının dikkate alınması ve ilke tarafından hangi eylemin alındığını elde ettiğiniz anlamına gelir. Örneğin, bir atamada ilke devre dışı bırakılır, ancak diğeri sessiz Audinotexist olarak ayarlanır, ardından birikmeli efekt Auditınotexist uygular. Her zaman daha etkin olan etkilerden önceliklidir.
   - İlkelerin etkisi: Append, Audit, Auditınotexists, deny, DeployIfNotExists, Disabled. Efektlerin nasıl uygulandığı hakkında daha fazla bilgi için bkz. [ilke efektlerini anlama](../governance/policy/concepts/effects.md).

   ![ilke ekranı](./media/security-center-policies/policy-screen.png)

> [!NOTE]
> Atanan ilkeleri görüntülediğinizde, birden çok atamayı görebilir ve her atamanın kendi kendine nasıl yapılandırıldığını görebilirsiniz.

## <a name="edit-security-policies"></a>Güvenlik ilkelerini düzenleme
Azure [Ilkesinde](../governance/policy/tutorials/create-and-manage.md)Azure aboneliklerinizin ve yönetim gruplarının her biri için varsayılan güvenlik ilkesini düzenleyebilirsiniz. Bir güvenlik ilkesini değiştirmek için, aboneliğin veya içeren yönetim grubunun bir sahibi veya güvenlik yöneticisi olmanız gerekir.

Azure Ilkesinde bir güvenlik ilkesinin nasıl düzenleneceği hakkında yönergeler için, bkz. ve [uyumluluğu zorlamak için Ilke oluşturma ve yönetme](../governance/policy/tutorials/create-and-manage.md).

Azure Ilke portalından, REST API aracılığıyla veya Windows PowerShell kullanarak güvenlik ilkelerini düzenleyebilirsiniz. Aşağıdaki örnek, REST API kullanarak düzenlemeyle ilgili yönergeler sağlar.


## <a name="disable-security-policies"></a>Güvenlik ilkelerini devre dışı bırak
Varsayılan güvenlik ilkesi ortamınız için uygun olmayan bir öneri üretiyorsa, öneriyi gönderen ilke tanımını devre dışı bırakarak bunu durdurabilirsiniz.
Öneriler hakkında daha fazla bilgi için bkz. [güvenlik önerilerini yönetme](security-center-recommendations.md).

1. Güvenlik Merkezi 'nde, **ilke & uyumluluk** bölümünde **güvenlik ilkesi**' ne tıklayın.

   ![ilke yönetimi](./media/tutorial-security-policy/policy-management.png)

2. Öneriyi devre dışı bırakmak istediğiniz abonelik veya yönetim grubuna tıklayın.

   > [!Note]
   > Bir yönetim grubunun ilkelerini aboneliklerine uygulayacağını unutmayın. Bu nedenle, bir aboneliğin ilkesini devre dışı bırakırsanız ve abonelik hala aynı ilkeyi kullanan bir yönetim grubuna aitse, ilke önerilerini almaya devam edersiniz. İlke hala yönetim düzeyinden uygulanır ve öneriler yine de oluşturulacaktır.

1. Atanan ilkeye tıklayın.

   ![ilkeyi devre dışı bırak](./media/tutorial-security-policy/security-policy.png)

1. **Parametreler** bölümünde, devre dışı bırakmak istediğiniz öneriyi çağıran ilkeyi arayın ve açılan listeden **devre dışı** ' yı seçin.

   ![ilkeyi devre dışı bırak](./media/tutorial-security-policy/disable-policy.png)
1. **Kaydet**'e tıklayın.
   > [!Note]
   > İlke devre dışı bırakma değişikliklerinin etkili olması 12 saate kadar sürebilir.


### <a name="configure-a-security-policy-using-the-rest-api"></a>REST API kullanarak bir güvenlik ilkesi yapılandırma

Azure Güvenlik Merkezi, Azure Ilkesi ile yerel tümleştirmenin bir parçası olarak, ilke atamaları oluşturmak için Azure Ilkesinin REST API avantajlarından yararlanmanızı sağlar. Aşağıdaki yönergelerde, ilke atamalarının oluşturulması ve var olan atamaların özelleştirilmesi adım adım açıklanmaktadır. 

Azure Ilkesinde önemli kavramlar: 

- **İlke tanımı** bir kuraldır 

- **Girişim** , ilke tanımlarının koleksiyonudur (kurallar) 

- **Atama** , bir girişim veya ilkenin belirli bir kapsama (Yönetim grubu, abonelik vb.) bir uygulamasıdır. 

Güvenlik Merkezi 'nin tüm güvenlik ilkelerini içeren yerleşik bir girişimi vardır. Güvenlik Merkezi 'nin Azure kaynaklarınızın ilkelerini değerlendirmek için, yönetim grubunda veya değerlendirmek istediğiniz abonelikte bir atama oluşturmanız gerekir.

Yerleşik girişim, tüm güvenlik merkezi ilkelerinin varsayılan olarak etkinleştirilmiş olduğunu içerir. Yerleşik girişimden belirli ilkeleri devre dışı bırakmayı seçebilirsiniz, örneğin, ilkenin efekt parametresinin değerini **devre dışı**olarak değiştirerek, **Web uygulaması güvenlik duvarı**hariç tüm güvenlik merkezi ilkelerini uygulayabilirsiniz. 

### <a name="api-examples"></a>API örnekleri

Aşağıdaki örneklerde şu değişkenleri değiştirin:

- **{scope}** ilkeyi uyguladığınız yönetim grubunun veya aboneliğin adını girin.
- **{Policyassignmentname}** [ilgili ilke atamasının adını](#policy-names)girin.
- **{Name}** adınızı veya ilke değişikliğini onaylayan yöneticinin adını girin.

Bu örnekte, yerleşik güvenlik merkezi girişiminin bir abonelikte veya yönetim grubunda nasıl atanacağı gösterilmektedir
 
 ```
    PUT  
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 

    Request Body (JSON) 

    { 

      "properties":{ 

    "displayName":"Enable Monitoring in Azure Security Center", 

    "metadata":{ 

    "assignedBy":"{Name}" 

    }, 

    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 

    "parameters":{}, 

    } 

    } 
 ```

Bu örnekte, aşağıdaki ilkeler devre dışı bırakıldığında yerleşik güvenlik merkezi girişiminin bir abonelikte nasıl atanacağı gösterilmektedir: 

- Sistem güncelleştirmeleri ("systemUpdatesMonitoringEffect") 

- Güvenlik Yapılandırması ("systemConfigurationsMonitoringEffect") 

- Endpoint Protection ("endpointProtectionMonitoringEffect") 

 ```
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
    
    Request Body (JSON) 
    
    { 
    
      "properties":{ 
    
    "displayName":"Enable Monitoring in Azure Security Center", 
    
    "metadata":{ 
    
    "assignedBy":"{Name}" 
    
    }, 
    
    "policyDefinitionId":"/providers/Microsoft.Authorization/policySetDefinitions/1f3afdf9-d0c9-4c3d-847f-89da613e70a8", 
    
    "parameters":{ 
    
    "systemUpdatesMonitoringEffect":{"value":"Disabled"}, 
    
    "systemConfigurationsMonitoringEffect":{"value":"Disabled"}, 
    
    "endpointProtectionMonitoringEffect":{"value":"Disabled"}, 
    
    }, 
    
     } 
    
    } 
 ```
Bu örnek, bir atamanın nasıl kaldırılacağını gösterir:
 ```
    DELETE   
    https://management.azure.com/{scope}/providers/Microsoft.Authorization/policyAssignments/{policyAssignmentName}?api-version=2018-05-01 
 ```

### İlke adları başvurusu<a name="policy-names"></a>

|Güvenlik Merkezi 'nde ilke adı|Azure Ilkesinde görünen ilke adı |İlke etkisi parametre adı|
|----|----|----|
|SQL şifrelemesi |Azure Güvenlik Merkezi 'nde şifrelenmemiş SQL veritabanını izleme |sqlEncryptionMonitoringEffect| 
|SQL denetimi |Azure Güvenlik Merkezi 'nde denetlenen SQL veritabanını izleme |sqlAuditingMonitoringEffect|
|Sistem güncelleştirmeleri |Azure Güvenlik Merkezi 'nde eksik sistem güncelleştirmelerini izleme |systemUpdatesMonitoringEffect|
|Depolama şifrelemesi |Depolama hesapları için eksik blob şifrelemesini denetle |storageEncryptionMonitoringEffect|
|JıT ağ erişimi |Azure Güvenlik Merkezi 'nde olası ağ tam zamanında (JıT) erişimi izleme |jitNetworkAccessMonitoringEffect |
|Uyarlamalı uygulama denetimleri |Azure Güvenlik Merkezi 'nde olası uygulama beyaz listesini izleme |adaptiveApplicationControlsMonitoringEffect|
|Ağ güvenlik grupları |Azure Güvenlik Merkezi 'nde izin veren ağ erişimini izleme |networkSecurityGroupsMonitoringEffect| 
|Güvenlik Yapılandırması |Azure Güvenlik Merkezi 'nde işletim sistemi güvenlik açıklarını izleme |systemConfigurationsMonitoringEffect| 
|Endpoint Protection |Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme |endpointProtectionMonitoringEffect |
|Disk şifrelemesi |Azure Güvenlik Merkezi 'nde şifrelenmemiş VM disklerini izleme |diskEncryptionMonitoringEffect|
|Güvenlik açığı değerlendirmesi |Azure Güvenlik Merkezi 'nde VM güvenlik açıklarını izleme |vulnerabilityAssessmentMonitoringEffect|
|Web uygulaması güvenlik duvarı |Azure Güvenlik Merkezi 'nde korumasız Web uygulamasını izleme |webApplicationFirewallMonitoringEffect |
|Yeni nesil güvenlik duvarı |Azure Güvenlik Merkezi 'nde korumasız ağ uç noktalarını izleme| |


### <a name="who-can-edit-security-policies"></a>Güvenlik ilkelerini kimler düzenleyebilir?
Güvenlik Merkezi, Azure 'daki kullanıcılara, gruplara ve hizmetlere atanabilen yerleşik roller sağlayan rol tabanlı Access Control (RBAC) kullanır. Kullanıcılar Güvenlik Merkezi 'ni açtıklarında yalnızca erişimi olan kaynaklarla ilgili bilgileri görürler. Bu, kullanıcılara bir kaynağın ait olduğu aboneliğe veya kaynak grubuna sahip, katkıda bulunan veya okuyucu rolünün atandığı anlamına gelir. Bu rollere ek olarak, iki özel güvenlik merkezi rolü vardır:

- Güvenlik okuyucusu: öneriler, uyarılar, ilke ve sistem durumunu içeren, ancak değişiklik yapaistemler için Güvenlik Merkezi 'ne yönelik görünüm haklarına sahiptir.
- Güvenlik Yöneticisi: güvenlik okuyucusu ile aynı görünüm haklarına sahip olan ve ayrıca güvenlik ilkesini güncelleştirebilir ve öneriler ile uyarıları kapatabilir.



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Ilkesinde güvenlik ilkelerinin nasıl düzenleneceğini öğrendiniz. Güvenlik Merkezi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md): Azure Güvenlik Merkezi ile ilgili tasarım konularını nasıl planlayıp anlayacağınızı öğrenin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md): Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi 'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md): güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md): iş ortağı çözümlerinizin sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi için kiracı genelinde görünürlük elde](security-center-management-groups.md)edin: Azure Güvenlik Merkezi için yönetim gruplarının nasıl ayarlanacağını öğrenin.
* [Azure Güvenlik MERKEZI SSS](security-center-faq.md): hizmeti kullanma hakkında sık sorulan soruların yanıtlarını alın.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/): Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini bulun.

Azure Ilkesi hakkında daha fazla bilgi edinmek için bkz. [Azure ilkesi nedir?](../governance/policy/overview.md)
