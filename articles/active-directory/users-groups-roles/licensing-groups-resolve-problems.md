---
title: Grup lisans atama sorunlarını çözme - Azure Etkin Dizin | Microsoft Dokümanlar
description: Azure Active Directory grup tabanlı lisanslama kullanırken lisans atama sorunlarını belirleme ve çözme
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ddfc4bf7ed3bdf214a44a5dfe03259d32b2f3f94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74025688"
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Azure Etkin Dizini'ndeki bir grubun lisans atama sorunlarını belirleme ve çözme

Azure Etkin Dizini'ndeki (Azure AD) grup tabanlı lisanslama, lisans hatası durumundaki kullanıcı kavramını sunar. Bu makalede, kullanıcıların neden bu duruma son verebileceklerini açıklıyoruz.

Grup tabanlı lisans kullanmadan lisansları doğrudan tek tek kullanıcılara atadığınızda, atama işlemi başarısız olabilir. Örneğin, PowerShell cmdlet'i `Set-MsolUserLicense` bir kullanıcı sisteminde çalıştırdığınızda, cmdlet iş mantığıyla ilgili birçok nedenden dolayı başarısız olabilir. Örneğin, aynı anda atanmayan iki hizmet planı arasında yetersiz sayıda lisans veya çakışma olabilir. Sorun hemen size bildirilir.

Grup tabanlı lisanslama kullanırken, aynı hatalar oluşabilir, ancak Azure AD hizmeti lisans atarken bunlar arka planda gerçekleşir. Bu nedenle, hatalar size hemen iletılamaz. Bunun yerine, kullanıcı nesnesi üzerinde kaydedilir ve daha sonra yönetim portalı üzerinden bildirilir. Kullanıcıyı lisanslamak için orijinal niyet asla kaybolmaz, ancak gelecekteki araştırma ve çözüm için bir hata durumunda kaydedilir.

## <a name="find-license-assignment-errors"></a>Lisans atama hatalarını bulma

### <a name="to-find-users-in-an-error-state-in-a-group"></a>Bir gruptaki hata durumundaki kullanıcıları bulmak için

1. Grubu genel bakış sayfasına açın ve **Lisansları**seçin. Hata durumunda herhangi bir kullanıcı varsa bir bildirim görüntülenir.

   ![Grup ve hata bildirimleri iletisi](./media/licensing-groups-resolve-problems/group-error-notification.png)

1. Etkilenen tüm kullanıcıların listesini açmak için bildirimi seçin. Daha fazla ayrıntı görmek için her kullanıcıyı tek tek seçebilirsiniz.

   ![grup lisanslama hata durumundaki kullanıcıların listesi](./media/licensing-groups-resolve-problems/list-of-users-with-errors.png)

1. En az bir hata içeren tüm grupları bulmak için **Azure Active Directory** **blade'de Lisansları**seçin ve ardından **Genel Bakış'ı**seçin. Gruplar dikkatinizi gerektirdiğinde bir bilgi kutusu görüntülenir.

   ![Hata durumundaki gruplar hakkında genel bakış ve bilgi](./media/licensing-groups-resolve-problems/group-errors-widget.png)

1. Hataları olan tüm grupların listesini görmek için kutuyu seçin. Daha fazla ayrıntı için her grubu seçebilirsiniz.

   ![Genel bakış ve hataiçeren grupların listesi](./media/licensing-groups-resolve-problems/list-of-groups-with-errors.png)

Aşağıdaki bölümlerde her olası sorunun bir açıklaması ve bunu çözme yolu verilmiştir.

## <a name="not-enough-licenses"></a>Yeterli lisans yok

**Sorun:** Grupta belirtilen ürünlerden biri için yeterli lisans yok. Ürün için daha fazla lisans satın almanız veya diğer kullanıcılardan veya gruplardan kullanılmayan lisansları boşaltmanız gerekir.

Kullanılabilir lisans sayısının kaç olduğunu görmek için **Azure Etkin Dizin** > **Lisansları** > **Tüm ürünlere**gidin.

Hangi kullanıcıların ve grupların lisansları tükettiğini görmek için bir ürün seçin. **Lisanslı kullanıcılar**altında, lisansları doğrudan veya bir veya daha fazla grup üzerinden atanmış olan tüm kullanıcıların listesini görürsünüz. **Lisanslı gruplar**altında, bu ürünlerin atanmış olduğu tüm grupları görürsünüz.

**PowerShell:** PowerShell cmdlets _CountViolation_olarak bu hatayı rapor.

## <a name="conflicting-service-plans"></a>Çakışan hizmet planları

**Sorun:** Grupta belirtilen ürünlerden biri, kullanıcıya farklı bir ürün üzerinden atanmış başka bir hizmet planıyla çakışan bir hizmet planı içerir. Bazı hizmet planları, ilgili başka bir hizmet planıyla aynı kullanıcıya atanamayacağı şekilde yapılandırılır.

Aşağıdaki örneği inceleyin. Bir kullanıcı, tüm planlar etkinleştirilmiş, doğrudan atanmış Office 365 Enterprise *E1* için bir lisansa sahiptir. Kullanıcı, office 365 Enterprise *E3* ürüne atanmış bir gruba eklendi. E3 ürünü, E1'de yer alan planlarla çakışamayan hizmet planları içerir, bu nedenle grup lisans ataması "Çakışan hizmet planları" hatasıyla başarısız olur. Bu örnekte, çakışan hizmet planları şunlardır:

- SharePoint Online (Plan 2), SharePoint Online (Plan 1) ile çakışabilir.
- Exchange Online (Plan 2) Exchange Online (Plan 1) ile çakışma.

Bu çakışmayı çözmek için, iki planı devre dışı bmalısınız. Doğrudan kullanıcıya atanan E1 lisansını devre dışı kullanabilirsiniz. Veya, tüm grup lisans atamasını değiştirmeniz ve E3 lisansındaki planları devre dışı balmanız gerekir. Alternatif olarak, E3 lisansı bağlamında gereksiz ise E1 lisansını kullanıcıdan kaldırmaya karar verebilirsiniz.

Çakışan ürün lisanslarının nasıl çözüleceği hakkındaki karar her zaman yöneticiye aittir. Azure AD, lisans çakışmalarını otomatik olarak çözmez.

**PowerShell:** PowerShell cmdlets _MutuallyExclusiveViolation_olarak bu hatayı rapor.

## <a name="other-products-depend-on-this-license"></a>Diğer ürünler bu lisansa bağlıdır

**Sorun:** Grupta belirtilen ürünlerden biri, başka bir üründe başka bir hizmet planının çalışması için etkinleştirilmesi gereken bir hizmet planı içerir. Bu hata, Azure AD temel hizmet planını kaldırmaya çalıştığında oluşur. Örneğin, kullanıcıyı gruptan kaldırdığınızda bu durum olabilir.

Bu sorunu çözmek için, gerekli planın başka bir yöntemle kullanıcılara hala atandığından veya bağımlı hizmetlerin bu kullanıcılar için devre dışı bırakıldığından emin olmanız gerekir. Bunu yaptıktan sonra, grup lisansını bu kullanıcılardan düzgün bir şekilde kaldırabilirsiniz.

**PowerShell:** PowerShell cmdlets Bağımlılık _Ihlali_olarak bu hatayı rapor.

## <a name="usage-location-isnt-allowed"></a>Kullanım konumuna izin verilmiyor

**Sorun:** Bazı Microsoft hizmetleri, yerel yasalar ve düzenlemeler nedeniyle tüm konumlarda kullanılamaz. Bir kullanıcıya lisans atamadan önce, kullanıcı için **Kullanım konumu** özelliğini belirtmeniz gerekir. Azure portalında **Kullanıcı** > **Profili** > **Ayarları** bölümü altında konumu belirtebilirsiniz.

Azure AD, kullanım konumu desteklenmeyen bir kullanıcıya grup lisansı atamaya çalıştığında başarısız olur ve kullanıcıda bir hata kaydeder.

Bu sorunu çözmek için, kullanıcıları desteklenmeyen konumlardan lisanslı gruptan kaldırın. Alternatif olarak, geçerli kullanım konumu değerleri gerçek kullanıcı konumunu temsil etmiyorsa, bunları lisansların bir sonraki sefere doğru şekilde atanması için değiştirebilirsiniz (yeni konum desteklenirse).

**PowerShell:** PowerShell cmdlets Bu hatayı _ProhibitedInUsageLocationViolation olarak bildirin._

> [!NOTE]
> Azure AD grup lisansları atadığında, belirli bir kullanım konumu olmayan kullanıcılar dizinin konumunu devralır. Yöneticilerin, yerel yasa ve yönetmeliklere uymak için grup tabanlı lisanslama kullanmadan önce kullanıcılar üzerinde doğru kullanım konum değerlerini ayarlamalarını öneririz.

## <a name="duplicate-proxy-addresses"></a>Yinelenen proxy adresleri

Exchange Online kullanıyorsanız, kiracınızdaki bazı kullanıcılar aynı proxy adresi değeriyle yanlış yapılandırılmış olabilir. Grup tabanlı lisanslama, böyle bir kullanıcıya lisans atamaya çalıştığında başarısız olur ve "Proxy adresi zaten kullanılıyor" ifadesine işaret eder.

> [!TIP]
> Yinelenen bir proxy adresi olup olmadığını görmek için Exchange Online'a karşı aşağıdaki PowerShell cmdlet'i uygulayın:
> ```
> Get-Recipient -ResultSize unlimited | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
> ```
> Bu sorun hakkında daha fazla bilgi için [Exchange Online'da "Proxy adresi zaten kullanılıyor" hata iletisi](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online)içinbkz. Makale ayrıca uzaktan [PowerShell kullanarak Exchange Online bağlanmak için nasıl](https://technet.microsoft.com/library/jj984289.aspx)bilgi içerir.

Etkilenen kullanıcılar için herhangi bir proxy adresi sorunlarını çözdükten sonra, lisansların artık uygulanabilmesini sağlamak için grup üzerinde lisans işleme zorlamak için emin olun.

## <a name="azure-ad-mail-and-proxyaddresses-attribute-change"></a>Azure AD Mail ve ProxyAdresöz değişikliği

**Sorun:** Bir kullanıcı veya grup üzerindeki lisans atamasını güncellerken, bazı kullanıcıların Azure AD Postave Proxy Adresleri özniteliğinin değiştirildiğini görebilirsiniz.

Kullanıcıda lisans atamasını güncelleştirmek, proxy adresi hesaplamasının tetiklenmesine neden olur ve bu da kullanıcı özniteliklerini değiştirebilir. Değişikliğin tam nedenini anlamak ve sorunu çözmek için [proxyAdres özniteliğinin Azure AD'de nasıl dolduruldüğüne](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad)ilişkin bu makaleye bakın.

## <a name="licenseassignmentattributeconcurrencyexception-in-audit-logs"></a>LisansAtamaÖz ÖzellikDenetim günlüklerindeÖzel Durum

**Sorun:** Kullanıcı denetim günlüklerinde lisans atama için LisansAtamaAttributeConcurrencyException vardır.
Grup tabanlı lisanslama, aynı lisansın kullanıcıya eşzamanlı lisans atamasını işlemeye çalıştığında, bu özel durum kullanıcıya kaydedilir. Bu genellikle, bir kullanıcı aynı atanmış lisansa sahip birden fazla grubun üyesi olduğunda gerçekleşir. AZure AD, kullanıcı lisansını yeniden işlemeyi dener ve sorunu çözer. Bu sorunu gidermek için müşteriden herhangi bir işlem gerekmemektedir.

## <a name="more-than-one-product-license-assigned-to-a-group"></a>Bir gruba atanan birden fazla ürün lisansı

Bir gruba birden fazla ürün lisansı atayabilirsiniz. Örneğin, kullanıcılar için dahil edilen tüm hizmetleri kolayca etkinleştirmek için bir gruba Office 365 Enterprise E3 ve Enterprise Mobility + Security atayabilirsiniz.

Azure AD, grupta belirtilen tüm lisansları her kullanıcıya atamaya çalışır. Azure AD, iş mantığı sorunları nedeniyle ürünlerden birini atayamazsa, gruptaki diğer lisansları da atamaz. Bir örnek, herkes için yeterli lisans yoksa veya kullanıcı üzerinde etkin leştirilmiş diğer hizmetlerle çakışmaları varsa.

Atanamayan kullanıcıları görebilir ve bu sorundan hangi ürünlerin etkilendiğini denetleyebilirsiniz.

## <a name="when-a-licensed-group-is-deleted"></a>Lisanslı bir grup silindiğinde

Grubu silmeden önce bir gruba atanan tüm lisansları kaldırmanız gerekir. Ancak, gruptaki tüm kullanıcıların lisanslarını kaldırmak zaman alabilir. Bir gruptan lisans atamaları kaldırırken, kullanıcı nın atanmış bağımlı bir lisansı varsa veya lisans kaldırmayı yasaklayan bir proxy adresi çakışması sorunu varsa hatalar olabilir. Bir kullanıcının grup silme nedeniyle kaldırılan bir lisansa bağımlı bir lisansı varsa, kullanıcıya lisans ataması devralınan dan doğrudan dönüştürülür.

Örneğin, Skype kurumsal hizmet planı etkinleştirilmiş office 365 E3/E5 atanmış bir grubu düşünün. Ayrıca, grubun birkaç üyesinin doğrudan atanmış Ses Konferansı lisansları olduğunu düşünün. Grup silindiğinde, grup tabanlı lisanslama Office 365 E3/E5'i tüm kullanıcılardan kaldırmaya çalışır. Sesli Konferans Skype kurumsal olarak bağlı olduğundan, Sesli Konferans atanmış tüm kullanıcılar için grup tabanlı lisanslama, Office 365 E3/E5 lisanslarını doğrudan lisans atamasına dönüştürür.

## <a name="manage-licenses-for-products-with-prerequisites"></a>Ön koşula sahip ürünlerin lisanslarını yönetme

Sahip olabileceğiniz bazı Microsoft Online ürünleri *eklentilerdir.* Eklentiler, bir lisans atanır önce bir kullanıcı veya grup için etkinleştirilmesi için bir ön koşul hizmet planı gerektirir. Grup tabanlı lisanslama ile sistem, hem ön koşul hem de eklenti hizmet planlarının aynı grupta bulunmasını gerektirir. Bu, gruba eklenen kullanıcıların tam çalışan ürünü alabilmesi için yapılır. Aşağıdaki örnek üzerinde düşünelim:

Microsoft İşyeri Analytics bir eklenti ürünüdür. Aynı ada sahip tek bir hizmet planı içerir. Bu hizmet planını yalnızca aşağıdaki ön koşullardan biri atandığında bir kullanıcıya veya gruba atayabiliriz:

- Exchange Online (Plan 1)
- Exchange Online (Plan 2)

Bu ürünü bir gruba kendi başına atamaya çalışırsak, portal bir bildirim iletisi döndürür. Öğe ayrıntılarını seçersek, aşağıdaki hata iletisini gösterir:

  "Lisans işlemi başarısız oldu. Bağımlı bir hizmet eklemeden veya kaldırmadan önce grubun gerekli hizmetlere sahip olduğundan emin olun. **Microsoft İşyeri Analizi hizmetinin de etkinleştirilmesi Exchange Online (Plan 2) gerektirir.**

Bu eklenti lisansını bir gruba atamak için, grubun ön koşul hizmet planını da içerdiğinden emin olmalıyız. Örneğin, office 365 E3 ürünçinin tümünü içeren varolan bir grubu gızlaştırAbilir ve ek ürünü de buna ekleyebiliriz.

Eklentinin çalışması için yalnızca minimum gerekli ürünleri içeren bağımsız bir grup oluşturmak da mümkündür. Eklenti ürünü için yalnızca seçili kullanıcıları lisanslamak için kullanılabilir. Önceki örneğe göre, aşağıdaki ürünleri aynı gruba atarsınız:

- Yalnızca Exchange Online (Plan 2) hizmet planı etkin leştirilmiş Office 365 Enterprise E3
- Microsoft Workplace Analytics

Şu andan itibaren, bu gruba eklenen kullanıcılar E3 ürününün bir lisansını ve İşyeri Analitiği ürününün bir lisansını tüketir. Aynı zamanda, bu kullanıcılar onlara tam E3 ürün veren başka bir grubun üyesi olabilir ve hala bu ürün için sadece bir lisans tüketirler.

> [!TIP]
> Her ön koşul hizmet planı için birden çok grup oluşturabilirsiniz. Örneğin, kullanıcılarınız için hem Office 365 Kurumsal E1 hem de Office 365 Enterprise E3 kullanıyorsanız, Microsoft İşyeri Analizi'ni lisanslamak için iki grup oluşturabilirsiniz: biri E1'i ön koşul olarak, diğeri de E3 kullanan. Bu, eklentiyi ek lisanslar tüketmeden E1 ve E3 kullanıcılarına dağıtmanızı sağlar.

## <a name="force-group-license-processing-to-resolve-errors"></a>Hataları gidermek için grup lisans işleme zorlama

Hataları gidermek için hangi adımları attığınızbağlı olarak, kullanıcı durumunu güncelleştirmek için bir grubun işlenmesini el ile tetiklemek gerekebilir.

Örneğin, kullanıcılardan doğrudan lisans atamaları kaldırarak bazı lisansları serbest çıkarırsanız, daha önce tüm kullanıcı üyelerini tam olarak lisanslayamadığı grupların işlenmesini tetiklemeniz gerekir. Bir grubu yeniden işlemek için grup bölmesine gidin, **Lisansları**açın ve ardından araç çubuğundaki **Yeniden İşlem düğmesini** seçin.

## <a name="force-user-license-processing-to-resolve-errors"></a>Hataları gidermek için kullanıcı lisansı işleme zorlama

Hataları gidermek için hangi adımları attığınızbağlı olarak, kullanıcı durumunu güncelleştirmek için bir kullanıcının işlenmesini el ile tetiklemek gerekebilir.

Örneğin, etkilenen bir kullanıcı için yinelenen proxy adresi sorununu çözdükten sonra, kullanıcının işlenmesini tetiklemeniz gerekir. Bir kullanıcıyı yeniden işlemek için kullanıcı bölmesine gidin, **Lisansları**açın ve ardından araç çubuğundaki **Yeniden İşlem düğmesini** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Gruplar aracılığıyla lisans yönetimi yle ilgili diğer senaryolar hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Azure Active Directory'de grup tabanlı lisanslama nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de gruba lisans atama](licensing-groups-assign.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](licensing-groups-migrate-users.md)
* [Azure Active Directory'de grup tabanlı lisanslama yı kullanarak kullanıcıların ürün lisansları arasında geçiş ilertirme](licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](licensing-group-advanced.md)
* [Azure Active Directory'de grup tabanlı lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
