---
title: Hesapları ve kimlik bilgilerini paylaşma - Azure Active Directory | Microsoft Dokümanlar
description: Azure Active Directory'nin kuruluşların şirket içi uygulamalar ve tüketici bulut hizmetleri için hesapları güvenli bir şekilde paylaşmasına nasıl olanak sağladığını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 08/01/2019
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 987c3ee7c90eb0bb793b96eb2771efbb258f16a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77565511"
---
# <a name="sharing-accounts-with-azure-ad"></a>Azure AD ile hesap paylaşma

## <a name="overview"></a>Genel Bakış

Bazen kuruluşların birden çok kişi için tek bir kullanıcı adı ve parola kullanması gerekir ve bu parola genellikle iki durumda gerçekleşir:

* Şirket içi uygulamalar veya tüketici bulut hizmetleri (örneğin, kurumsal sosyal medya hesapları) olsun, her kullanıcı için benzersiz bir oturum açma ve parola gerektiren uygulamalara erişirken.
* Çok kullanıcılı ortamlar oluştururken. Ayrıcalıkları yüksek olan ve temel kurulum, yönetim ve kurtarma etkinlikleri ni yapmak için kullanılan tek bir yerel hesabınız olabilir. Örneğin, Office 365'in yerel "global yöneticisi" hesabı veya Salesforce'taki kök hesap.

Geleneksel olarak, bu hesaplar kimlik bilgilerini (kullanıcı adı ve parola) doğru kişilere dağıtArak veya birden çok güvenilen aracının erişebileceği paylaşılan bir konumda depolayarak paylaşılır.

Geleneksel paylaşım modelinin çeşitli dezavantajları vardır:

* Yeni uygulamalara erişimi etkinleştirmek için kimlik bilgilerini erişmesi gereken herkese dağıtmanız gerekir.
* Paylaşılan her uygulama, kullanıcıların birden çok kimlik belgesi kümesini hatırlamasını gerektiren kendi benzersiz paylaşılan kimlik bilgileri kümesini gerektirebilir. Kullanıcılar birçok kimlik bilgilerini hatırlamak zorunda kaldığında, riskli uygulamalara başvurma riski artar. (örneğin, parolaları yazma).
* Bir uygulamaya kimin erişimi olduğunu söyleyemezsiniz.
* Bir uygulamaya kimin *eriştinin* söyleyemezsiniz.
* Bir uygulamaya erişimi kaldırmak istediğinizde, kimlik bilgilerini güncelleştirmeniz ve bunları bu uygulamaya erişmesi gereken herkese yeniden dağıtmanız gerekir.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory hesap paylaşımı

Azure AD, paylaşılan hesapları kullanmaya bu sakıncaları ortadan kaldıran yeni bir yaklaşım sağlar.

Azure AD yöneticisi, Access Panelini kullanarak ve bu uygulamaya en uygun tek oturum açma türünü seçerek bir kullanıcının hangi uygulamalara erişebileceğini yapılandırır. Bu türlerden biri olan *parola tabanlı tek oturum*açma, Azure AD'nin bu uygulamanın oturum açma işlemi sırasında bir tür "aracı" olarak hareket edinmesini sağlar.

Kullanıcılar kuruluş hesaplarıyla bir kez oturum açın. Bu hesap, masaüstü veya e-postalarına erişmek için düzenli olarak kullandıkları hesapla aynıdır. Yalnızca atandıkları uygulamaları keşfedebilir ve bunlara erişebilirler. Paylaşılan hesaplarda, bu uygulama listesi paylaşılan kimlik bilgilerini içerebilir. Son kullanıcının, kullanıyor olabileceği çeşitli hesapları hatırlaması veya yazması gerekmez.

Paylaşılan hesaplar yalnızca gözetimi artırmakla ve kullanılabilirliği artırmakla kalmıyor, aynı zamanda güvenliğinizi de artırır. Kimlik bilgilerini kullanma izni olan kullanıcılar paylaşılan parolayı görmez, daha çok düzenlenmiş bir kimlik doğrulama akışının bir parçası olarak parolayı kullanma izinleri alır. Ayrıca, bazı parola SSO uygulamaları, parolaları düzenli olarak rollover (güncelleştirmek) için Azure AD'yi kullanma seçeneği sunar. Sistem, hesap güvenliğini artıran büyük, karmaşık parolalar kullanır. Yönetici, bir uygulamaya erişimi kolayca verebilir veya iptal edebilir, hesaba kimin erişebileceğini ve geçmişte bu uygulamaya kimlerin erişdiğini bilir.

Azure AD, tüm parola tek oturum açma uygulamalarında, tüm Enterprise Mobility Suite (EMS) veya Azure AD Premium lisans planı için paylaşılan hesapları destekler. Uygulama galerisinde binlerce önceden entegre edilmiş uygulamanın hesaplarını paylaşabilir ve [özel SSO uygulamalarıyla](../manage-apps/configure-single-sign-on-non-gallery-applications.md)kendi parola doğrulama uygulamanızı ekleyebilirsiniz.

Hesap paylaşımını sağlayan Azure AD özellikleri şunlardır:

* [Parola tek oturum açma](../manage-apps/what-is-single-sign-on.md#password-based-sso)
* Parola tek oturum açma aracısı
* [Grup ataması](groups-self-service-management.md)
* Özel Parola uygulamaları
* [Uygulama kullanım panosu/raporları](../active-directory-passwords-get-insights.md)
* Son kullanıcı erişim portalları
* [Uygulama proxy](../manage-apps/application-proxy.md)
* [Aktif Dizin Pazarı](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Hesap paylaşma

Bir hesabı paylaşmak için Azure AD'yi kullanmak için şunları yapmanız gerekir:

* Uygulama [uygulaması galerisi](https://azure.microsoft.com/marketplace/active-directory/) veya [özel uygulama](https://cloudblogs.microsoft.com/enterprisemobility/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-now-in-preview/) ekleme
* Parola için uygulamayı yapılandırTek Oturum Açma (SSO)
* [Grup tabanlı atamayı](groups-saasapps.md) kullanın ve paylaşılan bir kimlik bilgisi girme seçeneğini seçin

Ayrıca, Çok Faktörlü Kimlik Doğrulama (MFA) [(Azure AD ile uygulamaların güvenliğini sağlama](../authentication/concept-mfa-whichversion.md)hakkında daha fazla bilgi edinin) ile paylaşılan hesabınızı daha güvenli hale getirebilir ve Azure AD self [servis](groups-self-service-management.md) grup yönetimini kullanarak uygulamaya kimlerin erişimi olduğunu yönetme özelliğini devredebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Koşullu Erişim ile uygulamaları koruma](../active-directory-conditional-access-azure-portal.md)
* [Self servis grup yönetimi/SSAA](groups-self-service-management.md)
