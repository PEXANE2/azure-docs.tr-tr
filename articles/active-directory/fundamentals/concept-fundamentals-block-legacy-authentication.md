---
title: Azure AD'de eski kimlik doğrulama protokollerini engelleme
description: Kuruluşların eski kimlik doğrulama protokollerini nasıl ve neden engellemesi gerektiğini öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63271567e70955f6dfb0b10a5c882b6dce9545ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74932495"
---
# <a name="blocking-legacy-authentication"></a>Eski kimlik doğrulamayı engelleme
 
Kullanıcılarınıza bulut uygulamalarınıza kolay erişim sağlamak için Azure Active Directory (Azure AD), eski kimlik doğrulama dahil olmak üzere çok çeşitli kimlik doğrulama protokollerini destekler. Eski kimlik doğrulaması, şu lar tarafından yapılan bir kimlik doğrulama isteğini ifade eden bir terimdir:

- Modern kimlik doğrulaması kullanmayan eski Office istemcileri (örneğin, Office 2010 istemcisi)
- IMAP/SMTP/POP3 gibi eski posta protokollerini kullanan tüm istemciler

Bugün, tüm ödün veren oturum açma girişimlerinin çoğu eski kimlik doğrulamasından gelir. Eski kimlik doğrulaması çok faktörlü kimlik doğrulamasını (MFA) desteklemez. Dizininizde etkin leştirilmiş bir MFA ilkesi olsa bile, kötü bir aktör eski bir protokol kullanarak kimlik doğrulaması yapabilir ve MFA'yı atlayabilir. Hesabınızı eski protokoller tarafından yapılan kötü amaçlı kimlik doğrulama isteklerine karşı korumanın en iyi yolu, bu girişimleri tamamen engellemektir.

## <a name="identify-legacy-authentication-use"></a>Eski kimlik doğrulama kullanımını belirleme

Dizininizde eski kimlik doğrulamasını engelleyebilmeniz için önce, kullanıcılarınızın eski kimlik doğrulaması kullanan uygulamaları olup olmadığını ve bunun genel dizininizi nasıl etkilediğini anlamanız gerekir. Azure AD oturum açma günlükleri, eski kimlik doğrulaması kullanıp kullanmadığınızı anlamak için kullanılabilir.

1. Azure Active Directory > Oturum Açma > Azure portalına gidin.
1. Sütunlar > İstemci Uygulaması'na tıklayarak gösterilmezse İstemci Uygulaması sütununa ekleyin.
1. İstemci Uygulamasına göre filtre uygulayın > sunulan tüm Diğer İstemci seçeneklerini kontrol edin ve Uygula'yı tıklatın.
1. Duruma göre filtre > Başarı'yı ve Uygula'yı tıklatın. 
1. Tarih filtresini kullanarak gerekirse tarih aralığınızı genişletin.

Filtreleme yalnızca seçilen eski kimlik doğrulama protokolleri tarafından yapılan başarılı oturum açma denemelerini gösterir. Her bir oturum açma girişimine tıkladığınızda ek ayrıntılar gösterecektir. Tek bir veri satırı seçtikten sonra Temel Bilgiler sekmesi altındaki İstemci Uygulaması sütunu veya İstemci Uygulaması alanı hangi eski kimlik doğrulama protokolünün kullanıldığını gösterir. Bu günlükler, hangi kullanıcıların hala eski kimlik doğrulamasına bağlı olduğunu ve kimlik doğrulama isteklerini gerçekleştirmek için hangi uygulamaların eski protokolleri kullandığını gösterir. Bu günlüklerde görünmeyen ve eski kimlik doğrulaması kullanmadığı doğrulanan kullanıcılar için Koşullu Erişim ilkesi uygulayın veya Temel ilkesini etkinleştirin: yalnızca bu kullanıcılar için eski kimlik doğrulamasını engelleyin.

## <a name="moving-away-from-legacy-authentication"></a>Eski kimlik doğrulamasından uzaklaşma 

Dizininizde eski kimlik doğrulamasını kimin kullandığı ve hangi uygulamaların buna bağlı olduğu hakkında daha iyi bir fikriniz olduğunda, bir sonraki adım kullanıcılarınızı modern kimlik doğrulamasını kullanacak şekilde yükseltmektir. Modern kimlik doğrulama, daha güvenli kullanıcı kimlik doğrulaması ve yetkilendirme sunan bir kimlik yönetimi yöntemidir. Dizininizde bir MFA ilkesi varsa, modern kimlik doğrulaması gerektiğinde kullanıcıdan MFA istendiğini sağlar. Eski kimlik doğrulama protokollerine daha güvenli bir alternatiftir.

Bu bölümde, ortamınızı modern kimlik doğrulamasına nasıl güncelleştireceğine ilişkin adım adım genel bir bakış sunulur. Kuruluşunuzdaki eski kimlik doğrulama engelleme ilkesini etkinleştirmeden önce aşağıdaki adımları okuyun.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>Adım 1: Dizininizde modern kimlik doğrulamayı etkinleştirme

Modern kimlik doğrulamayı etkinleştirmedeki ilk adım, dizinizin modern kimlik doğrulamasını desteklediğinden emin olmaktır. 1 Ağustos 2017 tarihinde veya sonrasında oluşturulan dizinler için varsayılan olarak modern kimlik doğrulaması etkinleştirilir. Dizininiz bu tarihten önce oluşturulduysa, aşağıdaki adımları kullanarak dizininiz için modern kimlik doğrulamasını el ile etkinleştirmeniz gerekir:

1. Dizininizin `Get-CsOAuthConfiguration` Skype for Business Online [PowerShell modülünden](https://docs.microsoft.com/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell)çalıştırarak modern kimlik doğrulamasını zaten destekleyip desteklemediğini kontrol edin.
1. Komutunuz boş `OAuthServers` bir özelliği döndürürse, Modern Kimlik Doğrulama devre dışı bırakılır. Kullanarak modern kimlik doğrulamasını `Set-CsOAuthConfiguration`etkinleştirmek için ayarı güncelleştirin. Mülkünüzde `OAuthServers` bir giriş varsa, gitmeniz uygun olur.

İlerlemeden önce bu adımı tamamladıktan emin olun. Tüm Office istemcileri tarafından hangi protokolün kullanılacağını dikte ettikleri için önce dizin yapılandırmalarınızın değiştirilmesi çok önemlidir. Modern kimlik doğrulamasını destekleyen Office istemcilerini kullanıyor olsanız bile, dizininizde modern kimlik doğrulama devre dışı bırakılırsa eski protokolleri kullanmayı varsayılan olarak kullanırlar.

### <a name="step-2-office-applications"></a>Adım 2: Office uygulamaları

Dizininizde modern kimlik doğrulamasını etkinleştirdikten sonra, Office istemcileri için modern kimlik doğrulamasını etkinleştirerek uygulamaları güncelleştirmeye başlayabilirsiniz. Office 2016 veya sonraki istemciler varsayılan olarak modern kimlik doğrulamasını destekler. İlave adım gerekmez.

Office 2013 Windows istemcilerini veya daha eskilerini kullanıyorsanız, Office 2016 veya sonraki lere yükseltmenizi öneririz. Dizininizde modern kimlik doğrulamayı etkinleştirme önceki adımını tamamladıktan sonra bile, eski Office uygulamaları eski kimlik doğrulama protokollerini kullanmaya devam eder. Office 2013 istemcilerini kullanıyorsanız ve Hemen Office 2016'ya veya daha sonra yükseltemiyorsanız, [Windows aygıtlarında Office 2013 için Modern Kimlik Doğrulamasını Etkinleştir](https://docs.microsoft.com/office365/admin/security-and-compliance/enable-modern-authentication)için aşağıdaki makaledeki adımları izleyin. Eski kimlik doğrulamasını kullanırken hesabınızın korunmasına yardımcı olmak için dizininizde güçlü parolalar kullanmanızı öneririz. Dizininizdeki zayıf parolaları yasaklamak için [Azure AD parola korumasına](../authentication/concept-password-ban-bad.md) göz atın.

Office 2010, modern kimlik doğrulamasını desteklemez. Office 2010'a sahip tüm kullanıcıları Office'in daha yeni bir sürümüne yükseltmeniz gerekir. Varsayılan olarak eski kimlik doğrulamasını engellediğimiz için Office 2016'ya veya daha sonrasına yükseltmenizi öneririz.

MacOS kullanıyorsanız, Office for Mac 2016 veya sonraki bir süreye yükseltmenizi öneririz. Yerel posta istemcisini kullanıyorsanız, macos sürüm 10.14 veya daha sonra tüm cihazlarda olması gerekir.

### <a name="step-3-exchange-and-sharepoint"></a>Adım 3: Exchange ve SharePoint

Windows tabanlı Outlook istemcilerinin modern kimlik doğrulamasını kullanabilmesi için Exchange Online'ın da modern kimlik doğrulama özelliği etkin olması gerekir. Exchange Online için modern kimlik doğrulaması devre dışı bırakılırsa, modern kimlik doğrulamayı destekleyen Windows tabanlı Outlook istemcileri (Outlook 2013 veya sonraki) Exchange Online posta kutularına bağlanmak için temel kimlik doğrulamasını kullanır.

SharePoint Online, modern kimlik doğrulama varsayılanı için etkinleştirilir. 1 Ağustos 2017'den sonra oluşturulan dizinler için Exchange Online'da varsayılan olarak modern kimlik doğrulaması etkinleştirilir. Ancak, daha önce modern kimlik doğrulamasını devre dışı bıraktıysanız veya bu tarihten önce oluşturulmuş bir dizin kullanıyorsanız, [Exchange Online'da modern kimlik doğrulamayı etkinleştirmek](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online)için aşağıdaki makaledeki adımları izleyin.

### <a name="step-4-skype-for-business"></a>Adım 4: Skype kurumsal

Skype for Business tarafından yapılan eski kimlik doğrulama isteklerini önlemek için, Skype for Business Online için modern kimlik doğrulamasını etkinleştirmek gerekir. 1 Ağustos 2017'den sonra oluşturulan dizinler için Skype kurumsal için modern kimlik doğrulaması varsayılan olarak etkinleştirilir.

Varsayılan olarak modern kimlik doğrulamasını destekleyen Microsoft Teams'e geçiş yapmanızı öneririz. Ancak, şu anda geçiş yapamıyorsanız, Skype kurumsal müşteriler için modern kimlik doğrulaması kullanmaya başlamak için Skype for Business Online için modern kimlik doğrulamasını etkinleştirmeniz gerekir. Skype for Business için Modern Kimlik Doğrulama'yı etkinleştirmek için [Modern Kimlik Doğrulama ile desteklenen Skype kurumsal topolojileri](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported)için bu makaledeki adımları izleyin.

Skype for Business Online için modern kimlik doğrulamasını etkinleştirmeye ek olarak, Skype for Business için modern kimlik doğrulamasını etkinleştirirken Exchange Online için modern kimlik doğrulamasını etkinleştirmenizi öneririz. Bu işlem, Exchange Online ve Skype for Business'taki modern kimlik doğrulama durumunu çevrimiçi olarak senkronize etmeye yardımcı olur ve Skype for Business istemcileri için birden fazla oturum açma istemlerini önler.

### <a name="step-5-using-mobile-devices"></a>Adım 5: Mobil cihazları kullanma

Mobil cihazınızdaki uygulamaların eski kimlik doğrulamasını da engellemesi gerekir. Mobil cihazlar için Outlook'u kullanmanızı öneririz. Outlook for Mobile varsayılan olarak modern kimlik doğrulamayı destekler ve diğer MFA temel koruma ilkelerini karşılar.

Yerel iOS posta istemcisini kullanmak için, posta istemcisinin eski kimlik doğrulamasını engellemek için güncelleştirildiğinden emin olmak için iOS sürüm 11.0 veya daha sonra çalışıyor olmanız gerekir.

### <a name="step-6-on-premises-clients"></a>Adım 6: Şirket içi müşteriler

Şirket içinde Exchange Server ve Skype for Business'ı kullanan karma bir müşteriyseniz, modern kimlik doğrulamasını etkinleştirmek için her iki hizmetin de güncellenmesi gerekir. Karma bir ortamda modern kimlik doğrulaması kullanırken, kullanıcıların şirket içinde kimlik doğrulamasını kullanmaya devam emzebilirsiniz. Kaynaklara (dosyalar veya e-postalar) erişimlerine izin verme hikayesi değişir.

Şirket içinde modern kimlik doğrulamayı etkinleştirmeye başlamadan önce, lütfen ön koşulları yerine getirebildiğinizden emin olun. Artık şirket içinde modern kimlik doğrulamayı etkinleştirmeye hazırsınız.

Modern kimlik doğrulamasını etkinleştirme adımları aşağıdaki makalelerde bulunabilir:

* [Exchange Server'ı karma modern kimlik doğrulamasını kullanmak üzere şirket içinde yapılandırma](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [Skype for Business ile Modern Kimlik Doğrulama (ADAL) nasıl kullanılır?](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)

## <a name="next-steps"></a>Sonraki adımlar

- [Exchange Server'ı karma modern kimlik doğrulamasını kullanmak üzere şirket içinde yapılandırma](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
- [Skype for Business ile Modern Kimlik Doğrulama (ADAL) nasıl kullanılır?](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)
- [Eski kimlik doğrulamasını engelleme](../conditional-access/block-legacy-authentication.md)
