---
title: Uygulamalar için son kullanıcı deneyimleri - Azure Active Directory
description: Azure Etkin Dizin (Azure AD), uygulamaları kuruluşunuzdaki son kullanıcılara dağıtmak için birkaç özelleştirilebilir yol sağlar.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: mimart
ms.reviewer: arvindh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 72b3e37a423442194b81e3d10ecc7157201ff8be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266630"
---
# <a name="end-user-experiences-for-applications-in-azure-active-directory"></a>Azure Active Directory'deki uygulamalar için son kullanıcı deneyimleri

Azure Etkin Dizin (Azure AD), uygulamaları kuruluşunuzdaki son kullanıcılara dağıtmak için birkaç özelleştirilebilir yol sağlar:

* Azure AD erişim paneli
* Office 365 uygulama başlatıcısı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Kuruluşunuzda hangi yöntemi dağıtmayı seçtiğiniz sizin takdirinizdir.

## <a name="azure-ad-access-panel"></a>Azure AD erişim paneli

Erişim https://myapps.microsoft.com Paneli, Azure Active Directory'de kuruluş hesabı olan bir son kullanıcının Azure AD yöneticisi tarafından erişim hakkı verilen uygulamaları görüntülemesine ve başlatmasına olanak tanıyan web tabanlı bir portaldır. [Azure Active Directory Premium'a](https://azure.microsoft.com/pricing/details/active-directory/)sahip bir son kullanıcıysanız, Access Paneli aracılığıyla self servis grup yönetimi özelliklerini de kullanabilirsiniz.

![Ekran görüntüsü Azure AD Erişim Paneli portalını gösterir](media/what-is-single-sign-on/azure-ad-access-panel.png)

Varsayılan olarak, tüm uygulamalar tek bir sayfada birlikte listelenir. Ancak koleksiyonları, ilgili uygulamaları gruplandırmak ve ayrı bir sekmede sunmak için kullanabilirsiniz, bu da koleksiyonları bulmayı kolaylaştırır. Örneğin, belirli iş rolleri, görevler, projeler ve benzeri uygulamalar için mantıksal gruplandırmaları oluşturmak için koleksiyonları kullanabilirsiniz. Daha fazla bilgi için, [kullanıcı erişim panellerini özelleştirmek için Uygulamalarım koleksiyonlarını nasıl kullanacağınız](access-panel-collections.md)bilgisine bakın. 

Erişim Paneli Azure portalından ayrıdır ve kullanıcıların Azure aboneliğine veya Office 365 aboneliğine sahip olmasını gerektirmez.

Azure AD erişim paneli hakkında daha fazla bilgi için [erişim paneline giriş](../user-help/active-directory-saas-access-panel-introduction.md)e bakın.

## <a name="office-365-application-launcher"></a>Office 365 uygulama başlatıcısı

Office 365'i dağıtan kuruluşlar için, Azure AD aracılığıyla kullanıcılara atanan uygulamalar da [https://portal.office.com/myapps](https://portal.office.com/myapps)''in adresindeki Office 365 portalında görünür. Bu, bir kuruluştaki kullanıcıların ikinci bir portal kullanmak zorunda kalmadan uygulamalarını başlatmalarını kolay ve kullanışlı hale getirir ve Office 365'i kullanan kuruluşlar için önerilen uygulama başlatma çözümüdür.

![Ekran görüntüsü Office 365 portalını gösterir](./media/end-user-experiences/microsoft-365-portal-office-com.png)

Office 365 uygulama başlatıcısı hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher)

## <a name="direct-sign-on-to-federated-apps"></a>Birleştirilmiş uygulamalarda doğrudan oturum açma

SAML 2.0, WS-Federation veya OpenID bağlantılarını destekleyen çoğu federe uygulama, kullanıcıların uygulamadan başlama ve ardından otomatik yeniden yönlendirme yoluyla veya oturum açmak için bir bağlantıyı tıklatarak Azure AD üzerinden oturum açma olanağını da destekler. Bu, hizmet sağlayıcısı tarafından başlatılan oturum açma olarak bilinir ve Azure AD uygulama galerisindeki çoğu federe uygulama bunu destekler (ayrıntılar için Azure portalında uygulamanın tek oturum açma yapılandırma sihirbazından bağlanan belgelere bakın).

![Mobil uygulama oturum açma sayfası örneği](./media/end-user-experiences/workdaymobile.png)

## <a name="direct-sign-on-links"></a>Doğrudan oturum açma bağlantıları

Azure AD ayrıca, parola tabanlı tek oturum açma, bağlantılı tek oturum açma ve federe tek oturum açma biçimlerini destekleyen tek tek uygulamalara doğrudan tek oturum açma bağlantılarını da destekler.

Bu bağlantılar, kullanıcının Azure AD erişim panelinden veya Office 365'ten başlatmasını gerektirmeden, belirli bir uygulama için kullanıcıyı Azure AD oturum açma işlemi aracılığıyla gönderen özel olarak hazırlanmış URL'lerdir. Bu **Kullanıcı erişim URL'leri** kullanılabilir kurumsal uygulamaların özellikleri altında bulunabilir. Azure portalında **Azure Active Directory** > **Enterprise uygulamalarını**seçin. Uygulamayı seçin ve ardından **Özellikler'i**seçin.

![Twitter özelliklerinde Kullanıcı erişim URL örneği](media/end-user-experiences/direct-sign-on-link.png)

Bu bağlantılar, seçili uygulamaya oturum açma bağlantısı sağlamak istediğiniz her yerde kopyalanabilir ve yapıştırılabilir. Bu, bir e-postada veya kullanıcı uygulaması erişimi için ayarladığınız herhangi bir özel web tabanlı portalda olabilir. Aşağıda, Twitter için Azure AD doğrudan tek oturum açma URL'si örneği verilmiştir:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Erişim paneli için kuruluşa özgü URL'lere benzer şekilde, myapps.microsoft.com etki alanından sonra dizininiz için etkin veya doğrulanmış etki alanlarından birini ekleyerek bu URL'yi daha da özelleştirebilirsiniz. Bu, kullanıcının önce kullanıcı kimliğini girmelerine gerek kalmadan, kuruluş markalarının oturum açma sayfasına hemen yüklenmesini sağlar:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Yetkili bir kullanıcı bu uygulamaya özel bağlantılardan birini tıklattığında, önce kuruluş oturum açma sayfalarını görür (oturum açma da zaten oturum açmadıklarını varsayarsak) ve oturum açtıktan sonra erişim panelinde durmadan uygulamalarına yönlendirilir. Kullanıcı, parola tabanlı tek işaretli tarayıcı uzantısı gibi uygulamaya erişmek için ön koşulları yoksa, bağlantı kullanıcıdan eksik uzantıyı yüklemesini ister. Uygulama için tek oturum açma yapılandırması değişirse bağlantı URL'si de sabit kalır.

Bu bağlantılar erişim paneli ve Office 365 ile aynı erişim denetim mekanizmalarını kullanır ve yalnızca Azure portalında uygulamaya atanan kullanıcılar veya gruplar başarılı bir şekilde kimlik doğrulaması yapabilecektir. Ancak, yetkisiz olan herhangi bir kullanıcı, kendilerine erişim izni verilmediğini açıklayan bir ileti görür ve erişim eteklerine sahip oldukları kullanılabilir uygulamaları görüntülemek için erişim panelini yüklemek için bir bağlantı verilir.

## <a name="next-steps"></a>Sonraki adımlar

Dağıtım planları için Azure [Etkin Dizin dağıtım planlarına](../fundamentals/active-directory-deployment-plans.md) bakın
