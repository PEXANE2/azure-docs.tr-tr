---
title: Ödeme hesabınızı Azure portalında görüntüleme sorunlarını giderme
description: Bu makale, ödeme hesabınızı Azure portalında görüntülemeye çalışırken karşılaştığınız sorunları gidermenize yardımcı olur.
author: amberbhargava
ms.reviewer: banders
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: banders
ms.openlocfilehash: 71561f6ecdde09f259e54260e7bab4dcc0e85c98
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92132644"
---
# <a name="troubleshoot-viewing-your-billing-account-in-the-azure-portal"></a>Ödeme hesabınızı Azure portalında görüntüleme sorunlarını giderme

Azure’ı kullanmak için kaydolduğunuzda bir ödeme hesabı oluşturulur. Faturalarınızı, ödemelerinizi yönetmek ve maliyetleri izlemek için ödeme hesabınızı kullanırsınız. Birden çok ödeme hesabına erişiminiz olabilir. Örneğin, kişisel kullanım için Azure’a kaydolmuş olabilirsiniz. Ayrıca kuruluşunuzun Kurumsal Anlaşması veya Microsoft Müşteri Sözleşmesi aracılığıyla da Azure’a erişiminiz olabilir. Bu senaryoların her biri için ayrı bir ödeme hesabınız olur. Bu makale, ödeme hesabınızı Azure portalında görüntülemeye çalışırken karşılaştığınız sorunları gidermenize yardımcı olur.

Ödeme hesaplarınızı [Azure Maliyet Yönetimi + Faturalama](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade) sayfasında görüntüleyebilirsiniz.

Ödeme hesapları hakkında daha fazla bilgi edinmek ve ödeme hesabınızın türünü belirlemek için bkz. [Azure portalında ödeme hesaplarını görüntüleme](view-all-accounts.md).

Ödeme hesabınızı Azure portalında göremiyorsanız aşağıdaki seçenekleri deneyin:

## <a name="sign-in-to-a-different-tenant"></a>Farklı bir kiracıda oturum açma

Ödeme hesabınız tek bir Azure Active Directory kiracısıyla ilişkilidir. Yanlış kiracıda oturum açtıysanız ödeme hesabınızı Maliyet Yönetimi + Faturalandırma sayfasında görmezsiniz. Azure portalında başka bir kiracıya geçiş yapmak ve bu kiracıdaki ödeme hesaplarınızı görüntülemek için aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sayfanın sağ üst kısmındaki profilinizi (e-posta adresi) seçin.
1. **Dizini değiştir**’i seçin.  
    ![Portalda dizini değiştirme adımının seçildiğini gösteren ekran görüntüsü](./media/troubleshoot-account-not-found/select-switch-directory.png)
1. **Tüm dizinler** bölümünden bir dizin seçin.  
    ![Portalda dizin seçme adımını gösteren ekran görüntüsü](./media/troubleshoot-account-not-found/select-directory.png)

## <a name="sign-in-with-a-different-email-address"></a>Farklı bir e-posta adresiyle oturum açma

Bazı kullanıcıların [Azure portalında](https://portal.azure.com) oturum açabildiği birden çok e-posta adresi vardır. Tüm e-posta adreslerinin bir ödeme hesabına erişimi yoktur. Kaynakları yönetme izinlerine sahip ancak ödeme hesabı görüntüleme izni olmayan bir e-posta adresiyle oturum açarsanız Azure portalındaki [Maliyet Yönetimi + Faturalandırma](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade) sayfasında ödeme hesabını görmezsiniz.

Ödeme hesabınıza erişmek için, ödeme hesabına erişim izni olan bir e-posta adresiyle Azure portalında oturum açın.

## <a name="sign-in-with-a-different-identity"></a>Farklı bir kimlik ile oturum açma

Bazı kullanıcıların aynı e-posta adresinde iki kimliği vardır: iş veya okul hesabı ve kişisel hesap. Genellikle, kimliklerden yalnızca birinin ödeme hesabını görüntüleme izni vardır. Tek bir e-posta adresiyle ilişkili iki kimliğiniz olabilir. Ödeme hesabını görüntüleme izni olmayan bir kimlikle oturum açtığınızda, [Maliyet Yönetimi + Faturalandırma](https://portal.azure.com/#blade/Microsoft_Azure_GTM/ModernBillingMenuBlade) sayfasındaki ödeme hesabını göremezsiniz. Kimliğinizi değiştirmek için aşağıdaki adımları izleyin:

1. InPrivate/Gizli pencereden [Azure portalında](https://portal.azure.com) oturum açın.
1. E-posta adresinizde iki kimlik varsa kişisel bir hesap veya bir iş ya da okul hesabı seçmeye yönelik bir seçenek görürsünüz. Hesaplardan birini seçin.
1. Azure portalındaki Maliyet Yönetimi + Faturalandırma sayfasında ödeme hesabını göremiyorsanız diğer kimliği seçmek için 1. ve 2. adımları tekrarlayın.

## <a name="contact-us-for-help"></a>Yardım için bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

Sorunları gidermeye yardımcı olması için aşağıdaki faturalandırma ve abonelik makalelerini okuyun.

- [Reddedilen kart](./troubleshoot-declined-card.md)
- [Abonelikte oturum açma sorunları](./troubleshoot-sign-in-issue.md)
- [Abonelik bulunamadı](./no-subscriptions-found.md)
- [Kurumsal maliyet görünümü devre dışı](./enterprise-mgmt-grp-troubleshoot-cost-view.md)