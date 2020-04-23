---
title: Güvenlik politikalarıyla çalışma | Microsoft Dokümanlar
description: Bu makalede, Azure Güvenlik Merkezi'nde güvenlik ilkeleriyle nasıl çalışılalış anlatılmaktadır.
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
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: c98ae7c95ac3fc186786612dd3d8d8bd55fa816f
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82024889"
---
# <a name="working-with-security-policies"></a>Güvenlik ilkeleriyle çalışma

Bu makalede, güvenlik ilkelerinin nasıl yapılandırıldığı ve Güvenlik Merkezi'nde nasıl görüntülenilen açıklanmaktadır. 

## <a name="introduction-to-security-policies"></a>Güvenlik ilkelerine giriş

Güvenlik ilkesi, iş yüklerinizin istenilen yapılandırmasını tanımlar ve şirketinizin veya düzenleyicilerinizin güvenlik gereksinimlerine uyduğunuzdan emin olun.

Azure Güvenlik Merkezi, güvenlik önerilerini seçtiğiniz ilkelere göre yapar. Güvenlik Merkezi ilkeleri, Azure İlkesi'nde oluşturulan ilke girişimlerini temel almaktadır. İlkelerinizi yönetmek ve Yönetim grupları arasında ve birden çok abonelikte ilke ayarlamak için [Azure İlkesi'ni](../governance/policy/overview.md) kullanabilirsiniz.

Güvenlik Merkezi, güvenlik ilkeleriyle çalışmak için aşağıdaki seçenekleri sunar:

* **Yerleşik varsayılan ilkeyi görüntüleyin ve edin** - Güvenlik Merkezi'ni etkinleştirdiğinizde, 'ASC varsayılanı' adlı yerleşik bir girişim otomatik olarak tüm Güvenlik Merkezi kayıtlı aboneliklerine (Ücretsiz veya Standart katmanlar) atanır. Bu girişimi özelleştirmek için, bu girişim içinde tek tek ilkeleri etkinleştirebilir veya devre dışı kullanabilirsiniz. Kutudan çıkan seçenekleri anlamak için [yerleşik güvenlik ilkeleri](security-center-policy-definitions.md) listesine bakın.

* **Kendi özel ilkelerinizi ekleyin** - Aboneliğinize uygulanan güvenlik girişimlerini özelleştirmek istiyorsanız, bunu Güvenlik Merkezi içinde yapabilirsiniz. Makineleriniz oluşturduğunuz ilkelere uymazsa öneriler alırsınız. Özel ilkeler oluşturma ve atama yönergeleri için [bkz.](custom-security-policies.md)

* **Mevzuata uygunluk ilkeleri ekleyin** - Güvenlik Merkezi'nin mevzuata uygunluk panosu, belirli bir standart veya düzenleme bağlamında (Azure CIS, NIST SP 800-53 R4, SWIFT CSP CSCF-v2020 gibi) ortamınızdaki tüm değerlendirmelerin durumunu gösterir. Daha fazla bilgi için [bkz.](security-center-compliance-dashboard.md)


## <a name="managing-your-security-policies"></a>Güvenlik ilkelerinizi yönetme

Güvenlik Merkezi'nde güvenlik ilkelerinizi görüntüleme:

1. Güvenlik **Merkezi** panosunda **Güvenlik ilkesini**seçin.

    ![İlke Yönetimi bölmesi](./media/security-center-policies/security-center-policy-mgt.png)

   **İlke yönetimi** ekranında, yönetim gruplarının, aboneliklerin ve çalışma alanlarının yanı sıra yönetim grubu yapınızı da görebilirsiniz.

1. İlkelerini görüntülemek istediğiniz abonelik veya yönetim grubunu seçin.

1. Bu abonelik veya yönetim grubunun güvenlik ilkesi sayfası görüntülenir. Kullanılabilir ve atanan ilkeleri gösterir.

   ![politika ekranı](./media/tutorial-security-policy/security-policy-page.png)

    > [!NOTE]
    > Varsayılan ilkenizin yanında "MG Devralındı" etiketi varsa, bu, ilkeğin bir yönetim grubuna atandığı ve görüntülemekte olduğunuz abonelik tarafından devralındığı anlamına gelir.


1. Bu sayfadaki mevcut seçenekler arasından seçim yapın:

    1. Endüstri ilkeleriyle çalışmak için **daha fazla standart ekle'yi**seçin. Daha fazla bilgi için [dinamik uyumluluk paketlerine güncelleştir'e](update-regulatory-compliance-packages.md)bakın.

    1. Özel girişimleri atamak ve yönetmek için **Özel girişimler ekle'yi**seçin. Daha fazla bilgi için [bkz.](custom-security-policies.md)

    1. Varsayılan ilkeyi görüntülemek ve görüntülemek için **etkili ilkeyi Görüntüle'yi** seçin ve aşağıda açıklandığı gibi devam edin. 

       ![politika ekranı](./media/security-center-policies/policy-screen.png)
       
       Bu **Güvenlik ilkesi** ekranı, seçtiğiniz abonelik veya yönetim grubunda atanan ilkeler tarafından gerçekleştirilen eylemi yansıtır.
       
       * Abonelik veya yönetim grubunda geçerli olan bir ilke **ataması** açmak için en üstteki bağlantıları kullanın. Bu bağlantılar, atamaya erişmenizi ve ilkeyi değiştirmenizi veya devre dışı bırakmanızı sağlar. Örneğin, belirli bir ilke atamasının uç nokta korumasını etkili bir şekilde reddettiğini görüyorsanız, ilkeyi düzenliletmek veya devre dışı etmek için bağlantıyı kullanın.
       
       * İlkeler listesinde, ilkenin abonelik veya yönetim grubuna etkin bir şekilde uygulanmasını görebilirsiniz. Kapsam için geçerli olan her ilkenin ayarları dikkate alınır ve ilke tarafından gerçekleştirilen eylemlerin kümülatif sonucu gösterilir. Örneğin, ilkeğin bir atamasında devre dışı bırakılmışsa, ancak diğerinde AuditIfNotExist olarak ayarlanmışsa, kümülatif efekt AuditIfNotExist'ı uygular. Daha etkin etki her zaman önceliklidir.
       
       * İlkelerin etkisi şu olabilir: Ek, Denetim, AuditIfNotExists, Reddet, DeployIfNotExists, Devre Dışı. Efektlerin nasıl uygulandığı hakkında daha fazla bilgi için [bkz.](../governance/policy/concepts/effects.md)

       > [!NOTE]
       > Atanan ilkeleri görüntülediğinizde, birden çok atama görebilirsiniz ve her atamanın kendi başına nasıl yapılandırıldığınızı görebilirsiniz.


## <a name="who-can-edit-security-policies"></a>Güvenlik ilkelerini kimler edinebilir?

Güvenlik ilkelerini Azure İlkesi portalı, REST API üzerinden veya Windows PowerShell'i kullanarak edinebilirsiniz.

Güvenlik Merkezi, Azure'daki kullanıcılara, gruplara ve hizmetlere atanabilen yerleşik roller sağlayan Rol Tabanlı Erişim Denetimi'ni (RBAC) kullanır. Kullanıcılar Güvenlik Merkezi'ni açtıklarında, yalnızca erişebildikleri kaynaklarla ilgili bilgileri görürler. Bu da, kullanıcıların kaynağın aboneliğine *sahip*, *katkıda bulunan*veya *okuyucu* rolü atandığı anlamına gelir. Bu rollerin yanı sıra, iki özel Güvenlik Merkezi rolü vardır:

- **Güvenlik okuyucu**: Öneriler, uyarılar, ilke ve sistem durumu içeren Güvenlik Merkezi'nin görünüm haklarına sahip, ancak değişiklik yapamazlar.
- **Güvenlik yöneticisi**: *Güvenlik okuyucusuyla*aynı görünüm haklarına sahiptir ve güvenlik ilkesini güncelleyebilir ve önerileri ve uyarıları reddedebilir.


## <a name="disable-security-policies"></a>Güvenlik ilkelerini devre dışı
Varsayılan güvenlik ilkesi ortamınız için uygun olmayan bir öneri oluşturuyorsa, öneriyi gönderen ilke tanımını devre dışı bırakarak bunu durdurabilirsiniz.
Öneriler hakkında daha fazla bilgi için [bkz.](security-center-recommendations.md)

1. Güvenlik Merkezi'nde, **İlke & Uyumluluk** bölümünden Güvenlik **ilkesini**seçin.

   ![politika yönetimi](./media/tutorial-security-policy/policy-management.png)

2. Öneriyi devre dışı kaltın istediğiniz abonelik veya yönetim grubunu seçin.

   > [!NOTE]
   > Bir yönetim grubunun ilkelerini aboneliklerine uygulayacağını unutmayın. Bu nedenle, bir aboneliğin ilkesini devre dışı bırakırsanız ve abonelik hala aynı ilkeyi kullanan bir yönetim grubuna aitse, ilke önerilerini almaya devam edersiniz. İlke hala yönetim düzeyinden uygulanır ve öneriler yine de oluşturulur.

1. **Etkili İlkeyi Görüntüle'yi**seçin.

   ![devre dışı at](./media/tutorial-security-policy/view-effective-policy.png)

1. Atanan ilkeyi seçin.

   ![devre dışı at](./media/tutorial-security-policy/security-policy.png)

1. **PARAMETRELER** bölümünde, devre dışı bırakmak istediğiniz öneriyi çağıran ilkeyi arayın ve açılır listeden **Devre Dışı**

   ![devre dışı at](./media/tutorial-security-policy/disable-policy.png)

1. **Kaydet**’i seçin.

   > [!NOTE]
   > Devre dışı ilke değişikliklerinin yürürlüğe girmesi 12 saat kadar sürebilir.



## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, güvenlik ilkeleri hakkında bilgi aldınız. İlgili bilgiler için aşağıdaki makalelere bakın:

* PowerShell kullanarak ilkeleri nasıl ayarlayacaklarına ilişkin talimatlar için [bkz.](../governance/policy/assign-policy-powershell.md)

* Azure İlkesi'nde bir güvenlik ilkesini nasıl yapılandıracağınız hakkındaki talimatlar [için, uyumluluğu uygulamak için İlkeler oluştur ve yönet'](../governance/policy/tutorials/create-and-manage.md)e bakın.

* Azure İlkesi'ni kullanarak abonelikler arasında veya Yönetim gruplarında bir ilkenin nasıl ayarlanır? [What is Azure Policy?](../governance/policy/overview.md)