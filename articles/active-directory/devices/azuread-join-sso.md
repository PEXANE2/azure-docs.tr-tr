---
title: SSO'nun şirket içi kaynakları Azure AD'de nasıl çalışır? Microsoft Dokümanlar
description: Hibrit Azure Active Directory'ye katılmış cihazları elle nasıl yapılandıracağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9d8c0cd803424e117bd4dc7a3382b7b32df2d05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672714"
---
# <a name="how-sso-to-on-premises-resources-works-on-azure-ad-joined-devices"></a>SSO'nun şirket içi kaynakları Azure AD'de nasıl çalışır?

Azure Active Directory (Azure AD) birleştirilmiş bir aygıtın kiracınızın bulut uygulamalarında size tek bir oturum açma (SSO) deneyimi sunması büyük olasılıkla sürpriz değildir. Ortamınızda şirket içi Active Directory (AD) varsa, bu cihazlardaki SSO deneyimini bu programa genişletebilirsiniz.

Bu makalede, bu nasıl çalıştığını açıklar.

## <a name="prerequisites"></a>Ön koşullar

 Azure AD'ye katılan makineler kuruluşunuzun ağına bağlı değilse, VPN veya başka bir ağ altyapısı gerekir. Şirket içi SSO, şirket içi AD DS etki alanı denetleyicilerinizle görüş alanı iletişimi gerektirir.

## <a name="how-it-works"></a>Nasıl çalışır? 

Yalnızca tek bir kullanıcı adı ve parola hatırlamanız gerektiğinden, SSO kaynaklarınıza erişimi kolaylaştırır ve ortamınızın güvenliğini artırır. Azure AD'ye katılan bir cihazla, kullanıcılarınız ortamınızdaki bulut uygulamalarında zaten bir SSO deneyimine sahiptir. Ortamınızda bir Azure REKLAMı ve şirket içi BIR REKLAM varsa, büyük olasılıkla SSO deneyiminizin kapsamını şirket içi Line Business (LOB) uygulamalarınız, dosya paylaşımları ve yazıcılarınıza genişletmek isteyebilirsiniz.

Azure AD'ye katılan aygıtların şirket içi REKLAM ortamınız hakkında hiçbir bilgisi yoktur, çünkü bunlara katılmaz. Ancak, Azure AD Connect ile bu aygıtlara şirket içi REKLAM'ınız hakkında ek bilgiler sağlayabilirsiniz.

Her ikisine de sahip bir ortam, bir Azure REKLAMı ve şirket içi AD, karma ortama sahip olduğu da bilinir. Karma bir ortamınız varsa, şirket içi kimlik bilgilerinizi bulutla senkronize etmek için Azure AD Connect'e zaten sahip sinizdir. Eşitleme işleminin bir parçası olarak Azure AD Connect, şirket içi kullanıcı bilgilerini Azure AD ile eşitler. Bir kullanıcı bir Azure AD aygıtında giriş yaptığında, karma bir ortamda aygıta katıldı:

1. Azure AD, kullanıcının üyesi olduğu şirket içi etki alanının adını aygıta geri gönderir.
1. Yerel güvenlik yetkilisi (LSA) hizmeti, cihazda Kerberos kimlik doğrulaması sağlar.

Kullanıcının şirket içi ortamında Kerberos'u isteyen bir kaynağa erişim girişimi sırasında, aygıt:

1. Kullanıcının kimliğinin doğrulanması için şirket içi etki alanı bilgilerini ve kullanıcı kimlik bilgilerini bulunan DC'ye gönderir.
1. AD'nin birleştirilmiş kaynaklarına erişmek için kullanılan Bir Kerberos [Bilet Verme Bileti (TGT)](/windows/desktop/secauthn/ticket-granting-tickets) alır. AAD connect etki alanı için TGT'yi alma girişimi başarısız olursa (ilgili DCLocator zaman ayarı gecikmeye neden olabilir), Kimlik Bilgisi Yöneticisi girişleri denenir veya kullanıcı hedef kaynak için kimlik bilgileri isteyen bir kimlik doğrulama açılır penceresi alabilir.

**Windows-Integrated kimlik doğrulama** için yapılandırılan tüm uygulamalar, bir kullanıcı bunlara erişmeye çalıştığında SSO'ya sorunsuz bir şekilde sahip olur.

Windows Hello for Business, Azure AD'ye katılan bir aygıttan şirket içi SSO'yu etkinleştirmek için ek yapılandırma gerektirir. Daha fazla bilgi için [bkz.](/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso-base) 

## <a name="what-you-get"></a>Ne alacaksınız

SSO ile Azure AD'ye katılan bir cihazda şunları yapabilirsiniz: 

- AD üye sunucusunda UNC yoluna erişin
- Windows tümleşik güvenliği için yapılandırılmış bir AD üyesi web sunucusuna erişin 

Şirket içi REKLAMınızı bir Windows aygıtından yönetmek istiyorsanız, [Windows 10 için Uzak Sunucu Yönetim Araçlarını](https://www.microsoft.com/download/details.aspx?id=45520)yükleyin.

Şunu kullanabilirsiniz:

- Etkin Dizin Kullanıcıları ve Bilgisayarları (ADUC) tüm AD nesnelerini yönetmek için tutturun. Ancak, el ile bağlanmak istediğiniz etki alanını belirtmeniz gerekir.
- AD'ye bağlı bir DHCP sunucusunu yönetmek için DHCP snap-in.. Ancak, DHCP sunucu adını veya adresini belirtmeniz gerekebilir.
 
## <a name="what-you-should-know"></a>Bilmeniz gerekenler

Gerekli etki alanlarıyla ilgili verilerin eşitlendirilmesini sağlamak için Azure AD Connect'te [etki alanına dayalı filtrelemenizi](../hybrid/how-to-connect-sync-configure-filtering.md#domain-based-filtering) ayarlamanız gerekebilir.

Azure AD'ye katılan aygıtların AD'de bir bilgisayar nesnesi olmadığından, Etkin Dizin makine kimlik doğrulaması'na bağlı uygulamalar ve kaynaklar çalışmaz. 

Azure AD'ye bağlı bir aygıtta dosyaları diğer kullanıcılarla paylaşamazsınız.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için Azure [Etkin Dizin'de aygıt yönetimi nedir?](overview.md) 
