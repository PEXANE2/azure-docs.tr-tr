---
title: Uygulamalar için son kullanıcı deneyimleri-Azure Active Directory
description: Azure Active Directory (Azure AD), kuruluşunuzdaki son kullanıcılara uygulama dağıtmak için çeşitli özelleştirilebilir yollar sunar.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: kenwith
ms.reviewer: arvindh
ms.openlocfilehash: d3b7dbbb9aab29e083795025ad5bf7381a4fa0ee
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89231223"
---
# <a name="end-user-experiences-for-applications-in-azure-active-directory"></a>Azure Active Directory uygulamalar için son kullanıcı deneyimleri

Azure Active Directory (Azure AD), kuruluşunuzdaki son kullanıcılara uygulama dağıtmak için çeşitli özelleştirilebilir yollar sağlar:

* Azure AD uygulamalarım
* Office 365 uygulama başlatıcısı
* Birleştirilmiş uygulamalarda doğrudan oturum açma
* Birleştirilmiş, parola tabanlı veya var olan uygulamalara yönelik ayrıntılı bağlantılar

Kuruluşunuzda dağıtmayı seçtiğiniz Yöntem (ler) sizin için önemlidir.

## <a name="azure-ad-my-apps"></a>Azure AD uygulamalarım

Konumundaki uygulamalarım https://myapps.microsoft.com , Azure AD yöneticisi tarafından erişim izni verilen uygulamaları görüntülemek ve başlatmak için Azure Active Directory ' deki bir kurumsal hesaba sahip bir son kullanıcıya izin veren Web tabanlı bir portaldır. [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/)olan bir son kullanıcı Ise, Uygulamalarım aracılığıyla Self Servis Grup yönetimi yeteneklerini de kullanabilirsiniz.

Varsayılan olarak, tüm uygulamalar tek bir sayfada birlikte listelenir. Ancak, koleksiyonları kullanarak ilgili uygulamaları gruplandırabilir ve bunları ayrı bir sekmede sunabilir, daha kolay bulunmalarını sağlayabilirsiniz. Örneğin, koleksiyonları, belirli iş rolleri, görevler, projeler vb. için mantıksal uygulama gruplandırmaları oluşturmak üzere kullanabilirsiniz. Bilgi için bkz. [uygulamalarım portalındaki koleksiyonlar oluşturma](access-panel-collections.md). 

Uygulamalarım Azure portal ayrıdır ve kullanıcıların bir Azure aboneliğine veya Office 365 aboneliğine sahip olmasını gerektirmez.

Azure AD My Apps hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="office-365-application-launcher"></a>Office 365 uygulama başlatıcısı

Office 365 ' i dağıtmış olan kuruluşlar için, Azure AD aracılığıyla kullanıcılara atanan uygulamalar, konumundaki Office 365 portalında da görünür [https://portal.office.com/myapps](https://portal.office.com/myapps) . Bu, bir kuruluştaki kullanıcıların ikinci bir portal kullanmak zorunda kalmadan uygulamalarını başlatmasını kolaylaştırır ve Office 365 kullanan kuruluşlar için önerilen uygulama başlatma çözümüdür.

Office 365 uygulama Başlatıcısı hakkında daha fazla bilgi için bkz. [uygulamanızın Office 365 uygulama başlatıcısı 'nda görünmesini sağlamak](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

## <a name="direct-sign-on-to-federated-apps"></a>Birleştirilmiş uygulamalarda doğrudan oturum açma

SAML 2,0, WS-Federation veya OpenID Connect 'i destekleyen çoğu Federasyon uygulaması, kullanıcıların uygulamada başlatma yeteneğini de destekler ve sonra otomatik yeniden yönlendirme veya oturum açmak için bir bağlantıya tıklayarak Azure AD aracılığıyla oturum açabilirler. Bu, hizmet sağlayıcı tarafından başlatılan oturum açma olarak bilinir ve Azure AD uygulama galerisindeki çoğu Federasyon uygulaması bunu destekler (Ayrıntılar için Azure portal uygulamanın çoklu oturum açma yapılandırma sihirbazından bağlantılı belgelere bakın).

## <a name="direct-sign-on-links"></a>Doğrudan oturum açma bağlantıları

Azure AD Ayrıca, parola tabanlı çoklu oturum açma, bağlantılı çoklu oturum açma ve herhangi bir Federasyon çoklu oturum açma biçimini destekleyen tek tek uygulamalara doğrudan çoklu oturum açma bağlantılarını destekler.

Bu bağlantılar, bir kullanıcıyı Azure AD My Apps veya Office 365 ' den başlatmaya gerek kalmadan belirli bir uygulama için Azure AD oturum açma işlemi aracılığıyla gönderen özel olarak hazırlanmış URL 'lardır. Bu **Kullanıcı erişimi URL 'leri** , kullanılabilir kurumsal uygulamaların özellikleri altında bulunabilir. Azure Portal **Azure Active Directory**  >  **Kurumsal uygulamalar**Azure Active Directory ' i seçin. Uygulamayı seçin ve ardından **Özellikler**' i seçin.

![Twitter özelliklerindeki Kullanıcı erişimi URL 'SI örneği](media/end-user-experiences/direct-sign-on-link.png)

Bu bağlantılar, seçilen uygulamaya bir oturum açma bağlantısı sağlamak istediğiniz yere kopyalanabilir ve yapıştırılabilir. Bu bir e-postada veya Kullanıcı uygulama erişimi için ayarladığınız özel Web tabanlı portalda olabilir. Twitter için bir Azure AD doğrudan çoklu oturum açma URL 'SI örneği aşağıda verilmiştir:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Uygulamalarım için kuruluşa özgü URL 'Lere benzer şekilde, *myapps.Microsoft.com* etki alanından sonra dizininiz için etkin veya doğrulanmış etki alanlarından birini ekleyerek bu URL 'yi daha da özelleştirebilirsiniz. Bu, tüm kurumsal markalamayı, kullanıcının kullanıcı KIMLIĞINI öncelikle girmesi gerekmeden oturum açma sayfasına hemen yüklenmesini sağlar:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Yetkili bir Kullanıcı bu uygulamaya özgü bağlantılardan birine tıkladığında, önce kurumsal oturum açma sayfasını (henüz oturum açmamış olduğunu varsayarak) görür ve oturum açma işleminden önce uygulamamda durdurulmadan önce bu uygulamalara yönlendirilir. Kullanıcı, uygulamaya erişmek için parola tabanlı tek imza tarayıcısı uzantısı gibi önkoşulların önkoşulları eksikse, bağlantı kullanıcıdan eksik uzantıyı yüklemesini ister. Uygulamanın çoklu oturum açma yapılandırması değişirse bağlantı URL 'SI de sabit kalır.

Bu bağlantılar, Uygulamalarım ve Office 365 ile aynı erişim denetimi mekanizmalarını kullanır ve yalnızca Azure portal uygulamaya atanmış olan kullanıcılar veya gruplar başarıyla kimlik doğrulayabilecektir. Ancak yetkilendirilmemiş olan her Kullanıcı, erişim verilmediğini belirten bir ileti görür ve uygulamalarımı, erişimleri olan kullanılabilir uygulamaları görüntülemek için yükleme bağlantısı verildiğini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama yönetiminde hızlı başlangıç serisi](view-applications-portal.md)
* [Çoklu oturum açma nedir?](what-is-single-sign-on.md)
* [Azure Active Directory uygulamalar ile tümleştirme kılavuzunu kullanmaya başlama](plan-an-application-integration.md)
