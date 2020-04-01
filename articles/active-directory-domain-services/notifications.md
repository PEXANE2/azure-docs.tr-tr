---
title: Azure AD Etki Alanı Hizmetleri için e-posta bildirimleri | Microsoft Dokümanlar'
description: Azure Active Directory Etki Alanı Hizmetleri yönetilen etki alanında sorunlar hakkında sizi uyarmak için e-posta bildirimlerini nasıl yapılandırabileceğinizi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: b9af1792-0b7f-4f3e-827a-9426cdb33ba6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: article
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: ff5b0b430de53a67f4de0dacbd76a38a7de6e284
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80475801"
---
# <a name="configure-email-notifications-for-issues-in-azure-active-directory-domain-services"></a>Azure Active Directory Etki Alanı Hizmetleri'ndeki sorunlar için e-posta bildirimlerini yapılandırma

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) yönetilen etki alanının durumu Azure platformu tarafından izlenir. Azure portalındaki sistem durumu durumu sayfası, yönetilen etki alanı için tüm uyarıları gösterir. Sorunların zamanında yanıtlandırıldığından emin olmak için, e-posta bildirimleri Azure AD DS yönetilen etki alanında algılanır algılanmadı sistem durumu uyarıları hakkında rapor verecek şekilde yapılandırılabilir.

Bu makalede, e-posta bildirimi alıcılarını Azure AD DS yönetilen bir etki alanı için nasıl yapılandırabileceğiniz gösterilmektedir.

## <a name="email-notification-overview"></a>E-posta bildirimine genel bakış

Azure AD DS yönetilen etki alanıyla ilgili sorunlar konusunda sizi uyarmak için e-posta bildirimlerini yapılandırabilirsiniz. Bu e-posta bildirimleri, uyarının mevcut olduğu Azure AD DS yönetilen etki alanını belirtirken, algılama zamanı ve Azure portalındaki sistem durumu sayfasına bağlantı verir. Daha sonra sorunları gidermek için sağlanan sorun giderme tavsiyelerini izleyebilirsiniz.

Aşağıdaki örnek e-posta bildirimi, Azure AD DS yönetilen etki alanında kritik bir uyarı veya uyarı oluşturulduğunu gösterir:

![Örnek e-posta bildirimi](./media/active-directory-domain-services-alerts/email-alert.png)

> [!WARNING]
> İletideki bağlantıları tıklatmadan önce her zaman e-postanın doğrulanmış bir Microsoft gönderenden geldiğinden emin olun. E-posta bildirimleri her `azure-noreply@microsoft.com` zaman adresten gelir.

### <a name="why-would-i-receive-email-notifications"></a>Neden e-posta bildirimleri alayım?

Azure AD DS, yönetilen etki alanı yla ilgili önemli güncelleştirmeler için e-posta bildirimleri gönderir. Bu bildirimler yalnızca hizmeti etkileyen acil sorunlar içindir ve hemen ele alınmalıdır. Her e-posta bildirimi, Azure AD DS yönetilen etki alanında bir uyarı tarafından tetiklenir. Uyarılar Azure portalında da görünür ve [Azure AD DS sistem durumu sayfasında][check-health]görüntülenebilir.

Azure AD DS reklam, güncelleştirme veya satış amacıyla e-posta göndermez.

### <a name="when-will-i-receive-email-notifications"></a>E-posta bildirimlerini ne zaman alacağım?

Azure AD DS yönetilen bir etki alanında yeni bir [uyarı][troubleshoot-alerts] bulunduğunda hemen bildirim gönderilir. Uyarı çözülmezse, her dört günde bir ek e-posta bildirimleri hatırlatıcı olarak gönderilir.

### <a name="who-should-receive-the-email-notifications"></a>E-posta bildirimlerini kimler almalı?

Azure AD DS için e-posta alıcılarının listesi, yönetilen etki alanını yönetebilen ve değişiklik yapabilen kişilerden oluşmalıdır. Bu e-posta listesi herhangi bir uyarı ve sorunlara "ilk yanıtlayanlar" olarak düşünülmelidir.

E-posta bildirimleri için en fazla beş ek e-posta alıcısı ekleyebilirsiniz. E-posta bildirimleri için beşten fazla alıcı istiyorsanız, bir dağıtım listesi oluşturun ve bunun yerine bildirim listesine ekleyin.

Ayrıca Azure AD dizininin tüm *Global Yöneticilerinin* ve *AAD DC Yöneticileri* grubunun her üyesinin e-posta bildirimleri almasını da seçebilirsiniz. Azure AD DS, genel yöneticilerin ve AAD DC yöneticilerinin listesi de dahil olmak üzere yalnızca en fazla 100 e-posta adresine bildirim gönderir.

## <a name="configure-email-notifications"></a>E-posta bildirimlerini yapılandırma

Varolan e-posta bildirimi alıcılarını gözden geçirmek veya ek alıcılar eklemek için aşağıdaki adımları tamamlayın:

1. Azure **portalında, Azure AD Etki Alanı Hizmetlerini**arayın ve seçin.
1. azure AD DS yönetilen etki alanınızı seçin( örneğin *aaddscontoso.com.*
1. Azure AD DS kaynak penceresinin sol tarafında **Bildirim ayarlarını**seçin. E-posta bildirimleri için varolan alıcılar gösterilir.
1. Bir e-posta alıcısı eklemek için ek alıcılar tablosuna e-posta adresini girin.
1. Bittiğinde, üstteki gezinmede **Kaydet'i** seçin.

> [!WARNING]
> Bildirim ayarlarını değiştirdiğinizde, yalnızca kendiniz değil, tüm Azure AD DS yönetilen etki alanının bildirim ayarları güncelleştirilir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="i-received-an-email-notification-for-an-alert-but-when-i-logged-on-to-the-azure-portal-there-was-no-alert-what-happened"></a>Bir uyarı için bir e-posta bildirimi aldım, ancak Azure portalında oturum açtığımda uyarı yoktu. Ne oldu?

Bir uyarı çözülürse, uyarı Azure portalından temizlenir. Bunun en olası nedeni, e-posta bildirimleri alan bir başkasının Azure AD DS yönetilen etki alanında uyarıyı çözmesi veya azure platformu tarafından otomatik olarak çözülmesidir.

### <a name="why-can-i-not-edit-the-notification-settings"></a>Bildirim ayarlarını neden düzeltamıyorum?

Azure portalındaki bildirim ayarları sayfasına erişemiyorsanız, Azure AD DS yönetilen etki alanını değiştirme izniniz yoktur. Azure AD DS kaynağını güncelleştirmek veya alıcı listesinden kaldırılmak için izin almak için genel bir yöneticiye başvurmanız gerekir.

### <a name="i-dont-seem-to-be-receiving-email-notifications-even-though-i-provided-my-email-address-why"></a>E-posta adresimi vermiş olmama rağmen e-posta bildirimleri alıyorum gibi görünmüyor. Neden?

Bildirim için e-postanızdaki spam veya önemsiz klasörünüzü kontrol edin ve gönderenin ''e izin verdiniz. `azure-noreply@microsoft.com`

## <a name="next-steps"></a>Sonraki adımlar

Bildirilebilecek bazı sorunları giderme hakkında daha fazla bilgi için Azure [AD DS yönetilen etki alanında uyarıları][troubleshoot-alerts]çözümle'ye bakın.

<!-- INTERNAL LINKS -->
[check-health]: check-health.md
[troubleshoot-alerts]: troubleshoot-alerts.md
