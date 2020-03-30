---
title: Koşullu Erişim ilkeleri dışında olan kullanıcıları yönetme - Azure AD
description: Koşullu Erişim ilkeleri dışında bırakılmış kullanıcıları yönetmek için Azure Active Directory (Azure AD) erişim yorumlarını nasıl kullanacağınızı öğrenin
services: active-directory
documentationcenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 09/25/2018
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: d1d7bce940f2b614c239e8b5e5719d96da10a6c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75422706"
---
# <a name="use-azure-ad-access-reviews-to-manage-users-excluded-from-conditional-access-policies"></a>Koşullu Erişim ilkeleri dışında olan kullanıcıları yönetmek için Azure AD erişim yorumlarını kullanma

İdeal bir dünyada, kuruluşunuzun kaynaklarına erişimi sağlamak için tüm kullanıcılar erişim polislerini takip eder. Ancak, bazen özel durumlar yapmanızı gerektiren iş durumları vardır. Bu makalede, dışlamaların gerekli olabileceği bazı örnekler ve BT yöneticisi olarak bu görevi nasıl yönetebileceğiniz, ilke özel durumlarının denetlenmesini önleyebileceğiniz ve denetçilere bu özel durumların Azure kullanılarak düzenli olarak incelendiğine dair kanıt sağlamanız bazı örnekler açıklanmaktadır Etkin Dizin (Azure AD) erişim değerlendirmeleri.

> [!NOTE]
> Geçerli bir Azure AD Premium P2, Enterprise Mobility + Security E5 ücretli veya deneme lisansı, Azure AD erişim değerlendirmelerini kullanmak için gereklidir. Daha fazla bilgi için [Azure Etkin Dizin sürümlerine](../fundamentals/active-directory-whatis.md)bakın.

## <a name="why-would-you-exclude-users-from-policies"></a>Kullanıcıları neden ilkelerden dışlıyorsun?

BT yöneticisi olarak, kullanıcıların çok faktörlü kimlik doğrulama (MFA) kullanarak kimlik doğrulaması (MFA) veya güvenilir bir ağ veya aygıttan oturum açmalarını gerektirmek için [Azure AD Koşullu Erişim'i](../conditional-access/overview.md) kullanabilirsiniz. Dağıtım planlaması sırasında, bu gereksinimlerden bazılarının tüm kullanıcılar tarafından karşılanamayacağını fark esiniz. Örneğin, dahili ağınızın bir parçası olmayan uzak bir ofisten çalışan kullanıcılar veya desteklenmeyen eski bir telefon kullanan bir yönetici vardır. İşletme, bu kullanıcıların oturum açmalarına ve işlerini yapmalarına izin verilmesini gerektirir, bu nedenle Koşullu Erişim ilkelerinin dışında tutulurlar.

Başka bir örnek olarak, kullanıcıların kiracılarına erişmesine izin vermek istemediğiniz bir dizi ilçe ve bölgeyi yapılandırmak için Koşullu Erişim'de [adlandırılmış konumları](../conditional-access/location-condition.md) kullanabilirsiniz.

![Koşullu Erişim'de adlandırılmış konumlar](./media/conditional-access-exclusion/named-locations.png)

Ancak, bazı durumlarda, kullanıcıların bu engellenen ülkelerden/bölgelerde oturum açmaları için geçerli bir nedeni olabilir. Örneğin, kullanıcılar iş veya kişisel nedenlerle seyahat ediyor olabilir. Bu örnekte, bu ülkeleri/bölgeleri engellemek için Koşullu Erişim ilkesi, ilkenin dışında kalan kullanıcılar için özel bir bulut güvenlik grubuna sahip olabilir. Seyahat ederken erişime ihtiyaç duyan kullanıcılar, [Azure AD self servis Grup yönetimini](../users-groups-roles/groups-self-service-management.md)kullanarak kendilerini gruba ekleyebilirler.

Başka bir örnek, [kullanıcılarınızın büyük çoğunluğu için eski kimlik doğrulamasını engelleyen](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/07/azure-ad-conditional-access-support-for-blocking-legacy-auth-is-in-public-preview/)bir Koşullu Erişim ilkesine sahip olabilirsiniz. Microsoft, güvenlik duruşunuzu iyileştirmek için kiracınızdaki eski protokollerin kullanımını engellemenizi şiddetle önerir. Ancak, office 2010 veya IMAP/SMTP/POP tabanlı istemciler aracılığıyla kaynaklarına erişmek için kesinlikle eski kimlik doğrulama yöntemlerini kullanması gereken bazı kullanıcılarınız varsa, bu kullanıcıları eski kimlik doğrulama yöntemlerini engelleyen ilkeden hariç tutabilirsiniz.

## <a name="why-are-exclusions-challenging"></a>Dışlamalar neden zorlayıcı?

Azure AD'de, Koşullu Erişim ilkesini bir kullanıcı kümesine kapsamız. Azure AD rollerini, bireysel kullanıcıları veya kullanıcıların misafirlerini seçerek bu kullanıcıların bazılarını hariç tutabilirsiniz. Bu dışlamalar yapılandırıldığında, bu kullanıcılar için ilke amacının uygulanaamayacağı unutulmamalıdır. Bu dışlamalar tek tek kullanıcıların listesi veya eski bir şirket içi güvenlik grubu aracılığıyla yapılandırıldıysa, bu dışlama listesinin görünürlüğünü (kullanıcılar varlığını bilmeyebilir) ve BT yöneticisinin bu liste üzerindeki denetimini (kullanıcılar ilkeyi by-pass etmek için güvenlik grubu). Ayrıca, bir defada hariç tutulmaya hak kazanan kullanıcılar artık buna ihtiyaç duymayabilir veya buna uygun olmayabilir.

Bir dışlamanın başında, ilkeyi atlayan kullanıcıların kısa bir listesi vardır. Zamanla, daha fazla kullanıcı dışlanır ve liste büyür. Bir noktada, listeyi gözden geçirmek ve bu kullanıcıların her biri hala dışlanmış olması gerektiğini onaylamak için bir ihtiyaç vardır. Teknik açıdan listeyi yönetmek nispeten kolay olabilir, ancak iş kararlarını kim verir ve bunların denetlenebilir olduğundan nasıl emin olabilirsiniz?

Ancak, bir Azure REKLAM grubu kullanarak koşullu erişim ilkesine dışlamayı yapılandırırsanız, görünürlüğü artırmak ve özel durumu olan kullanıcı sayısını azaltmak için erişim değerlendirmelerini telafi denetimi olarak kullanabilirsiniz.

## <a name="how-to-create-an-exclusion-group-in-a-conditional-access-policy"></a>Koşullu Erişim ilkesinde dışlama grubu oluşturma

Yeni bir Azure REKLAM grubu ve bu grup için geçerli olmayan koşullu erişim ilkesi oluşturmak için aşağıdaki adımları izleyin.

### <a name="create-an-exclusion-group"></a>Dışlama grubu oluşturma

1. Azure Portal’da oturum açın.

1. Sol gezintide **Azure Etkin Dizin'i** tıklatın ve ardından **Gruplar'ı**tıklatın.

1. Üst menüde, grup bölmesini açmak için **Yeni Grup'u** tıklatın.

1. Grup **türü** listesinde **Güvenlik'i**seçin. Bir ad ve açıklama belirtin.

1. **Üyelik** türünü **Atanmış**olarak ayarladıklınız için emin olun.

1. Bu dışlama grubunun bir parçası olması gereken kullanıcıları seçin ve ardından **Oluştur'u**tıklatın.

    ![Azure Etkin Dizini'nde yeni grup bölmesi](./media/conditional-access-exclusion/new-group.png)

### <a name="create-a-conditional-access-policy-that-excludes-the-group"></a>Grubu dışlayan koşullu erişim ilkesi oluşturma

Artık bu dışlama grubunu kullanan bir Koşullu Erişim ilkesi oluşturabilirsiniz.

1. Sol gezintide **Azure Etkin Dizin'i** tıklatın ve ardından **İlkeler** çubuğunu açmak için **Koşullu Erişim'i** tıklatın.

1. **Yeni** bölmeyi açmak için **Yeni ilkeyi** tıklatın.

1. Bir ad belirtin.

1. Atamalar altında **Kullanıcılar ve gruplar**tıklatın.

1. **Ekle** sekmesinde Tüm **Kullanıcılar'ı**seçin.

1. **Dışla** sekmesinde, **Kullanıcılara ve gruplara** bir onay işareti ekleyin ve ardından **dışlanmış kullanıcıları seç'i**tıklatın.

1. Oluşturduğunuz dışlama grubunu seçin.

    > [!NOTE]
    > En iyi uygulama olarak, kiracınızın kilitsiz olmadığından emin olmak için sınama sırasında en az bir yönetici hesabının ilkenden hariç tutmanız önerilir.

1. Koşullu Erişim ilkesini kuruluş gereksinimlerinize göre ayarlamaya devam edin.

    ![Koşullu Erişim'de dışlanmış kullanıcı bölmesini seçme](./media/conditional-access-exclusion/select-excluded-users.png)

Koşullu Erişim ilkelerindeki hariç tutulmaları yönetmek için erişim değerlendirmelerini kullanabileceğiniz iki örneği ele alalım.

## <a name="example-1-access-review-for-users-accessing-from-blocked-countriesregions"></a>Örnek 1: Engellenen ülkelerden/bölgelerden erişen kullanıcılar için erişim incelemesi

Belirli ülkelerden/bölgelerden erişimi engelleyen bir Koşullu Erişim politikanız olduğunu varsayalım. İlkenin dışında tutulmuş bir grubu içerir. Burada, grup üyelerinin gözden geçirildiği önerilen bir erişim incelemesi vereme leri önerilir.

> [!NOTE]
> Erişim incelemeleri oluşturmak için Global yönetici veya Kullanıcı yöneticisi rolü gereklidir.

1. İnceleme her hafta tekrarlanacaktır.

2. Bu dışlama grubunu en güncel tuttuğunuzdan emin olmak için asla bitmez.

3. Bu grubun tüm üyeleri inceleme kapsamında olacaktır.

4. Her kullanıcı, engellenen bu ülkelerden/bölgelerden hala erişebilmeniz gerektiğini, bu nedenle yine de grubun bir üyesi olması gerektiğini kendi kendine kanıtlamak zorunda kalacaktır.

5. Kullanıcı inceleme isteğine yanıt vermezse, bu şirket otomatik olarak gruptan kaldırılır ve bu nedenle bu ülkelere/bölgelere seyahat ederken kiracıya artık erişemez.

6. Kullanıcıların erişim incelemesinin başlatılması ve tamamlanması hakkında bilgilendirilmeleri için posta bildirimlerini etkinleştirin.

    ![Örneğin bir erişim gözden geçirme bölmesi oluşturma 1](./media/conditional-access-exclusion/create-access-review-1.png)

## <a name="example-2-access-review-for-users-accessing-with-legacy-authentication"></a>Örnek 2: Eski kimlik doğrulamayla erişen kullanıcılar için erişim incelemesi

Eski kimlik doğrulaması ve eski istemci sürümlerini kullanan kullanıcıların erişimini engelleyen bir Koşullu Erişim ilkeniz olduğunu varsayalım. İlkenin dışında tutulmuş bir grubu içerir. Burada, grup üyelerinin gözden geçirildiği önerilen bir erişim incelemesi vereme leri önerilir.

1. Bu inceleme yinelenen bir inceleme olması gerekir.

2. Gruptaki herkesin gözden geçirilmesi gerekiyor.

3. Seçilen gözden geçirenler olarak iş birimi sahiplerini listelemek için yapılandırılabilir.

4. Sonuçları otomatik olarak uygulayın ve eski kimlik doğrulama yöntemlerini kullanmaya devam etmek için onaylanmamış kullanıcıları kaldırın.

5. Büyük grupların gözden geçirenlerinin kararlarını kolayca verebilmesi için önerilerin etkinhale getirilmesi yararlı olabilir.

6. Kullanıcıların erişim incelemesinin başlatılması ve tamamlanması hakkında bilgilendirilmeleri için posta bildirimlerini etkinleştirin.

    ![Örneğin 2 için bir erişim gözden geçirme bölmesi oluşturma](./media/conditional-access-exclusion/create-access-review-2.png)

**Pro İpucu**: Çok sayıda dışlama grubunuz varsa ve bu nedenle birden çok erişim incelemesi oluşturmanız gerekiyorsa, artık Microsoft Graph beta bitiş noktasında bunları programlı olarak oluşturmanıza ve yönetmenize olanak tanıyan bir API'miz var. Başlamak için, [Azure AD erişim incelemeleri API başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/accessreviews_root) ve Microsoft Graph üzerinden Azure AD erişim [incelemelerini alma örneği](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-of-retrieving-Azure-AD-access-reviews-via-Microsoft/td-p/236096)bölümüne bakın.

## <a name="access-review-results-and-audit-logs"></a>İnceleme sonuçlarına ve denetim günlüklerine erişin

Artık her şey yerinde, grup, Koşullu Erişim ilkesi ve erişim değerlendirmeleri var, izlemek ve bu değerlendirmeleri sonuçlarını izlemek için zamanı.

1. Azure portalında Access **incelemeleri** bıçağını açın.

1. Dışlama grubunu yönetmek için oluşturduğunuz denetim ve programı açın.

1. Listede kimlerin kalması onaylandı ve kimler kaldırıldı görmek için **Sonuçlar'ı** tıklatın.

    ![Erişim incelemeleri sonuçları kimin onaylandığını gösterir](./media/conditional-access-exclusion/access-reviews-results.png)

1. Ardından, bu inceleme sırasında gerçekleştirilen eylemleri görmek için **Denetim günlüklerini** tıklatın.

    ![Erişim, denetim günlüklerini listeleme eylemlerini gözden geçirir](./media/conditional-access-exclusion/access-reviews-audit-logs.png)

Bt yöneticisi olarak, dışlama gruplarını ilkelerinize göre yönetmenin bazen kaçınılmaz olduğunu bilirsiniz. Ancak, bu grupları korumak, işletme sahibi veya kullanıcıların kendileri tarafından düzenli olarak gözden geçirmek ve bu değişiklikleri denetlemek Azure AD erişim incelemeleri ile daha kolay hale getirebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Grupların veya uygulamaların erişim gözden geçirmesini oluşturma](create-access-review.md)
- [Azure Etkin Dizinde Koşullu Erişim Nedir?](../conditional-access/overview.md)
