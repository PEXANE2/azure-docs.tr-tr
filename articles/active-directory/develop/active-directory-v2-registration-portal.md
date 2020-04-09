---
title: Uygulama kayıt portalı başvurusu | Azure
titleSuffix: Microsoft identity platform
description: Microsoft uygulama kayıt portalındaki özelliklerin açıklaması.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: ryanwi
ms.reviewer: lenalepa
ms.custom: aaddev
ms.openlocfilehash: 910aaf84d3563d4410826d3c0bdfde3d2dfc75e3
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885642"
---
# <a name="app-registration-reference"></a>Uygulama kayıt başvurusu

Bu belge, Azure portalındaki [Uygulama kayıtları](https://aka.ms/appregistrations) deneyiminde bulunan çeşitli özelliklerin bağlamını ve açıklamalarını sağlar.

## <a name="my-applications-or-converged-applications"></a>Uygulamalarım veya Yakınsanmış Uygulamalarım

Bu liste, Microsoft kimlik platformu (v2.0) bitiş noktası ile kullanılmak üzere kayıtlı tüm uygulamalarınızı içerir. Bu uygulamalar, Azure Active Directory'den hem kişisel Microsoft hesapları na hem de iş/okul hesaplarıolan kullanıcılarda oturum açma özelliğine sahiptir. Kimlik platformu bitiş noktası hakkında daha fazla bilgi edinmek için [v2.0 genel bakış](active-directory-appmodel-v2-overview.md)ına bakın. Bu uygulamalar, Microsoft hesap kimlik doğrulama bitiş noktası ile `https://login.live.com`tümleştirmek için de kullanılabilir.

## <a name="azure-ad-only-applications"></a>Azure AD yalnızca uygulamaları

Bu liste, Azure AD v1.0 bitiş noktası ile kullanılmak üzere kayıtlı tüm uygulamalarınızı içerir. Bu uygulamalar yalnızca Azure Active Directory adresinden iş/okul hesabı olan kullanıcılarda oturum açma özelliğine sahiptir. Bu liste, [Azure portalındaki](https://portal.azure.com) **Uygulama kayıtları** deneyimini kullanarak kaydedilmiş uygulamaları içerir.

## <a name="live-sdk-applications"></a>Canlı SDK Uygulamaları

Bu liste, yalnızca Microsoft hesabında kullanılmak üzere kayıtlı tüm uygulamalarınızı içerir. Azure Active Directory ile kullanım için etkinleştirilirler. Burada daha önce MSA geliştirici portalına kayıtlı olan tüm `https://account.live.com/developers/applications`uygulamaları bulabilirsiniz. Daha önce gerçekleştirdiğiniz tüm `https://account.live.com/developers/applications` işlevler artık [Uygulama kayıtlarında](https://aka.ms/appregistrations)gerçekleştirilebilir.

## <a name="application-secrets"></a>Uygulama Sırları

Uygulama sırları, uygulamanızın Azure AD ile güvenilir [istemci kimlik doğrulaması](https://tools.ietf.org/html/rfc6749#section-2.3) gerçekleştirmesine olanak tanıyan kimlik bilgileridir. OAuth & OpenID Connect'te, bir uygulama sırrı `client_secret`genellikle . v2.0 protokolünde, web adresine ait bir konumda `https` (şema kullanarak) güvenlik belirteci alan herhangi bir uygulama, söz güvenlik belirteci nin itfası üzerine kendisini Azure AD'ye tanıtmak için bir uygulama sırrını kullanmalıdır. Ayrıca, bir aygıtta belirteçleri alan herhangi bir yerel istemciistemci kimlik doğrulaması gerçekleştirmek için bir uygulama gizli kullanarak yasaktır. Bu, sırların güvensiz ortamlarda depolanmasını engeller.

Her uygulama herhangi bir zamanda iki geçerli uygulama sırrı içerebilir. İki sırrı koruyarak, uygulamanızın tüm ortamında periyodik anahtar devri gerçekleştirme yeteneğine sahip olabilirsiniz. Uygulamanızın tamamını yeni bir gizliye geçirdiğinizde, eski sırrı silebilir ve yeni bir sır verebilirsiniz.

Şu anda, uygulama kayıt portalında yalnızca iki tür uygulama sırrına izin verilir. Yeni **Parola Oluştur'u** seçmek, uygulamanızda kullanabileceğiniz paylaşılan bir sırrı ilgili veri deposunda oluşturur ve depolar. Yeni **Anahtar Çifti Oluştur'u** seçmek, Azure AD'ye istemci kimlik doğrulaması için indirilebilen ve kullanılabilen yeni bir genel/özel anahtar çifti oluşturur. Upload **Public Key'i** seçmek, kendi ortak/özel anahtar çiftinizi kullanmanıza olanak tanır.
Ortak anahtar içeren bir sertifika yüklemeniz gerekir.

## <a name="profile"></a>Profil

Uygulama kayıt portalının profil bölümü, uygulamanız için oturum açma sayfasını özelleştirmek için kullanılabilir. Şu anda oturum açma sayfasının uygulama logosunu, hizmet şartları URL'sini ve gizlilik bildirimi URL'sini değiştirebilirsiniz. Logo, 15 KB veya daha küçük bir GIF, PNG veya JPEG dosyasında şeffaf 48 x 48 veya 50 x 50 piksel görüntü olmalıdır. Değerleri değiştirmeyi ve ortaya çıkan oturum açma sayfasını görüntülemeyi deneyin!

## <a name="live-sdk-support"></a>Canlı SDK Desteği

"Canlı SDK Desteği"ni etkinleştirdiğinizde, oluşturduğunuz tüm uygulama sırları hem Azure AD hem de Microsoft Hesabı veri depolarında karşılanır. Bu, uygulamanızın doğrudan Microsoft Hesabı hizmetiyle (login.live.com) tümleştirmesine olanak tanır. Doğrudan Microsoft Hesabı'nı kullanarak bir uygulama oluşturmak istiyorsanız (v2.0 bitiş noktasını kullanmanın aksine), Live SDK Desteğinin etkin olduğundan emin olmalısınız.

Live SDK desteğinin devre dışı bırakılması, uygulama sırrının yalnızca Azure AD veri deposuna yazılmasını sağlar. Azure AD veri deposu, FISMA uyumluluğu gibi belirli standartları karşılamasını sağlayan kurumsal dereceli düzenlemeler içerir. Live SDK desteğini etkinleştiriseniz, uygulamanız bu standartlardan bazılarına uymayabilir.

V2.0 bitiş noktasını kullanmayı planlıyorsanız, Live SDK desteğini güvenle devre dışı kullanabilirsiniz.
