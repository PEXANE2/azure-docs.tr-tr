---
title: Azure AD 'de, passwordless kimlik doğrulama yöntemlerini kaydetmek için geçici bir erişim geçişi yapılandırın
description: Kullanıcıların geçici bir erişim geçişi (dokunarak) kullanarak parolasız kimlik doğrulama yöntemlerini kaydettirerek nasıl yapılandırılacağını ve etkinleştireceğinizi öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/19/2021
ms.author: justinha
author: inbarckms
manager: daveba
ms.reviewer: inbarckms
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0f49f39e6bc291c3242fe739866a015ac154a8b
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101651170"
---
# <a name="configure-temporary-access-pass-in-azure-ad-to-register-passwordless-authentication-methods-preview"></a>Azure AD 'de geçici erişim geçişini, passwordless kimlik doğrulama yöntemlerini (Önizleme) kaydetmek için yapılandırma

Microsoft Authenticator uygulamasında FIDO2 ve passwordless telefon oturum açma gibi parolasız kimlik doğrulama yöntemleri, kullanıcıların parolasız güvenli bir şekilde oturum açmasını sağlar. Kullanıcılar passwordless yöntemlerini iki yöntemden biriyle önyükleyebilir:

- Mevcut Azure AD Multi-Factor Authentication yöntemlerini kullanma 
- Geçici erişim geçişi kullanma (TAP) 

TAP, güçlü kimlik doğrulama gereksinimlerini karşılayan bir yönetici tarafından verilen zaman sınırlı bir geçiş kodu olur ve parolasız olanlar dahil diğer kimlik doğrulama yöntemlerini eklemek için kullanılabilir. Ayrıca, bir Kullanıcı bir FIDO2 güvenlik anahtarı veya Microsoft Authenticator uygulaması gibi güçlü kimlik doğrulama faktörünü kaybettiği veya unutduktan sonra, ancak yeni güçlü kimlik doğrulama yöntemlerini kaydetmek için oturum açması gereken durumlarda kurtarmayı kolaylaştırır.


Bu makalede, Azure AD 'de Azure portal kullanarak bir dokunun nasıl etkinleştirileceği ve kullanılacağı gösterilmektedir. REST API 'Lerini kullanarak da bu eylemleri gerçekleştirebilirsiniz. 

>[!NOTE]
>Geçici erişim geçişi Şu anda genel önizlemededir. Bazı özellikler desteklenmeyebilir veya sınırlı özelliklere sahip olabilir. 

## <a name="enable-the-tap-policy"></a>DOKUNMA ilkesini etkinleştir

Bir dokunma ilkesi, kiracıda oluşturulan geçiş ömrü veya oturum açmak için bir dokunma kullanabilecek kullanıcılar ve gruplar gibi ayarları tanımlar. Bir dokunarak oturum açmadan önce, kimlik doğrulama yöntemi ilkesini etkinleştirmeniz ve bir dokunma kullanarak hangi kullanıcıların ve grupların oturum açabileceği belirlemeniz gerekir.
Herhangi bir kullanıcı için bir dokunma oluşturabilseniz de yalnızca ilkeye dahil olanlar onunla oturum açabilir.

Genel yönetici ve kimlik doğrulama yöntemi Ilkesi yönetici rolü sahipleri, dokunma kimlik doğrulama yöntemi ilkesini güncelleştirebilir.
DOKUNMA kimlik doğrulama yöntemi ilkesini yapılandırmak için:

1. Azure Portal genel yönetici olarak oturum açın ve **Azure Active Directory**  >  **güvenlik**  >  **kimlik doğrulama yöntemleri**  >  **geçici erişim geçişi**' ne tıklayın.
1. İlkeyi etkinleştirmek için **Evet** ' e tıklayın, ilkenin hangi kullanıcılara uygulandığını ve tüm **genel** ayarları seçin.

   ![DOKUNMA kimlik doğrulama yöntemi ilkesini etkinleştirme ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/policy.png)

   Varsayılan değer ve izin verilen değer aralığı aşağıdaki tabloda açıklanmıştır.


   | Ayar          | Varsayılan değerler | İzin verilen değerler               | Yorumlar                                                                                                                                                                                                                                                                 |   |
   |------------------|----------------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    En düşük ömür | 1 saat         | 10 – 43200 dakika (30 gün) | TAP 'ın geçerli olduğu en az dakika sayısı.                                                                                                                                                                                                                         |   |
   | Maksimum ömür | 24 saat       | 10 – 43200 dakika (30 gün) | TAP 'ın geçerli olduğu en fazla dakika sayısı.                                                                                                                                                                                                                         |   |
   | Varsayılan ömür | 1 saat         | 10 – 43200 dakika (30 gün) | Varsayılan değerler, ilke tarafından yapılandırılan minimum ve maksimum ömür dahilinde tek tek geçişler tarafından geçersiz kılınabilir                                                                                                                                                |   |
   | Bir kerelik kullanım     | Yanlış          | Doğru/yanlış                 | İlke false olarak ayarlandığında, Kiracıdaki geçişler bir veya birden çok kez (en fazla ömür süresi) bir kez kullanılabilir. DOKUNMA ilkesinde tek seferlik kullanımı zorlayarak, kiracıda oluşturulan tüm geçişler tek seferlik kullanım olarak oluşturulur. |   |
   | Uzunluk           | 8              | 8-48 karakter              | Geçiş kodunun uzunluğunu tanımlar.                                                                                                                                                                                                                                      |   |

## <a name="create-a-tap-in-the-azure-ad-portal"></a>Azure AD portalında bir dokunma oluşturma

Bir dokunma ilkesini etkinleştirdikten sonra, Azure AD 'de bir kullanıcı için bir dokunma oluşturabilirsiniz. Bu roller, DOKUNMAYLA ilgili aşağıdaki eylemleri gerçekleştirebilir.

- Genel yönetici, herhangi bir kullanıcı için (kendileri hariç) oluşturabilir, silebilir, bu görünüme DOKUNABILIR
- Ayrıcalıklı kimlik doğrulama yöneticileri Yöneticiler ve Üyeler üzerinde (kendileri hariç) bir görünüm oluşturabilir, silebilir, görüntüleme yapabilir
- Kimlik doğrulama yöneticileri, Üyeler üzerinde (kendileri hariç) bir görünüm oluşturabilir, silebilir, görüntüleyebilir
- Genel yönetici, Kullanıcı üzerindeki dokunma ayrıntılarını görüntüleyebilir (kodun kendisini okumadan).

Bir dokunma oluşturmak için:

1. Portalda genel yönetici, ayrıcalıklı kimlik doğrulama Yöneticisi veya kimlik doğrulama Yöneticisi olarak oturum açın. 
1. **Azure Active Directory**' ye tıklayın, kullanıcılar ' a gidin, *Chris yeşil* gibi bir kullanıcı seçin, sonra **kimlik doğrulama yöntemleri**' ni seçin.
1. Gerekirse, **Yeni Kullanıcı kimlik doğrulama yöntemleri deneyimini deneyin** seçeneğini belirleyin.
1. **Kimlik doğrulama yöntemleri ekleme** seçeneğini belirleyin.
1. Aşağıdaki **yöntemi seçin**, **geçici erişim geçişi (Önizleme)** seçeneğine tıklayın.
1. Özel bir etkinleştirme saati veya süresi tanımlayın ve **Ekle**' ye tıklayın.

   >[!NOTE]
   >Kiracıda oturum açanlar (KMSı) etkin olduğunda, tek seferlik dokunma kullanımı zorlanmaz. Tek seferlik bir dokunma oluşturuyorsanız, KMSı 'yi devre dışı bıraktığınızdan emin olun.

   ![DOKUNMA oluşturma ekranının ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/create.png)

1. Eklendikten sonra, dokunma ayrıntıları gösterilir. Gerçek dokunma değerini bir yere getirin. Bu değeri kullanıcıya sağlarsınız. **Tamam**' a tıkladıktan sonra bu değeri görüntüleyemezsiniz.
   
   ![DOKUNMA ayrıntıları ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/details.png)

## <a name="use-a-tap"></a>Bir dokunma kullanın

Bir dokunma için en yaygın kullanım, bir kullanıcının, ek güvenlik istemlerini tamamlamaya gerek kalmadan ilk oturum açma sırasında kimlik doğrulama ayrıntılarını kaydetmesi içindir. Kimlik doğrulama yöntemleri konumunda kaydedilir [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) . Kullanıcılar ayrıca, mevcut kimlik doğrulama yöntemlerini buradan güncelleştirebilir.

1. İçin bir Web tarayıcısı açın [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) .
1. İçin dokunun oluşturduğunuz hesabın UPN 'sini girin (örneğin,) *tapuser@contoso.com* .
1. Kullanıcı dokunma ilkesine dahil ediliyorlarsa, dokunma girişi için bir ekran görür.
1. Azure portal görüntülenen dokunun girin.

   ![DOKUNMA girme ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/enter.png)

>[!NOTE]
>Federasyon etki alanları için, Federasyon üzerinden bir dokunma tercih edilir. DOKUNARAK bir Kullanıcı Azure AD 'de kimlik doğrulamasını tamamlar ve federal kimlik sağlayıcısına (IDP) yönlendirilmeyecektir.

Kullanıcı artık oturum açtı ve FIDO2 güvenlik anahtarı gibi bir yöntemi güncelleştirebilir veya kaydedebilir. Kimlik bilgilerinin veya cihazının kaybedilmesi nedeniyle kimlik doğrulama yöntemlerini güncelleştiren kullanıcılar eski kimlik doğrulama yöntemlerini kaldırdıklarından emin olmanızı sağlamalıdır.

Kullanıcılar, doğrudan Doğrulayıcı uygulamasından parolasız telefon oturum açma için kaydolmak üzere dokunarak da kullanabilirler. Daha fazla bilgi için bkz. [Microsoft Authenticator uygulamasına iş veya okul hesabınızı ekleme](../user-help/user-help-auth-app-add-work-school-account.md).

![İş veya okul hesabı kullanarak bir dokunma girme ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/enter-work-school.png)

## <a name="delete-a-tap"></a>DOKUNMA silme

Süre dolduğunda bir dokunma kullanılamaz. Bir kullanıcının **kimlik doğrulama yöntemlerinin** altında **ayrıntı** sütunu, dokunma ne zaman dolduğunu gösterir. Aşağıdaki adımları kullanarak bu zaman aşımına uğradı, bu dokunmalar silebilirsiniz:

1. Azure AD portalında, **Kullanıcılar**' a gidin, Kullanıcı ' *ya dokunun*, ardından **kimlik doğrulama yöntemleri**' ni seçin.
1. Listede gösterilen **geçici erişim geçişi (Önizleme)** kimlik doğrulama yönteminin sağ tarafında **Sil**' i seçin.

## <a name="replace-a-tap"></a>DOKUNMA değiştirme 

- Bir kullanıcının yalnızca bir DOKUNMASı olabilir. Geçiş kodu, dokunun başlangıç ve bitiş saati sırasında kullanılabilir.
- Kullanıcı yeni bir dokunma gerektiriyorsa:
  - Varolan dokunma geçerliyse, yöneticinin mevcut TAP 'ı silmesi ve Kullanıcı için yeni bir geçiş oluşturması gerekir. Geçerli bir dokunma silindiğinde kullanıcının oturumları iptal edilir. 
  - Varolan dokunma süresi dolmuşsa, yeni bir dokunma var olan DOKUNMAYı geçersiz kılar ve kullanıcının oturumlarını iptal etmez.

Ekleme ve kurtarmaya yönelik NıST standartları hakkında daha fazla bilgi için bkz. [NIST özel yayını 800-63A](https://pages.nist.gov/800-63-3/sp800-63a.html#sec4).

## <a name="limitations"></a>Sınırlamalar

Bu sınırlamaları aklınızda bulundurun:

- FIDO2 veya Phone oturum açma gibi bir passwordless yöntemi kaydetmek için tek seferlik bir dokunma kullandığınızda, kullanıcının kaydı tek seferlik dokunarak 10 dakikalık bir oturum açma işlemiyle tamamlaması gerekir. Bu sınırlama, birden çok kez kullanılabilecek bir dokunma için uygulanmaz.
- Konuk kullanıcılar bir dokunarak oturum açamaz.
- Self servis parola sıfırlama (SSPR) kayıt ilkesi kapsamındaki kullanıcıların, dokunarak oturum açtıktan sonra SSPR yöntemlerinden birini kaydetmesi gerekecektir. Kullanıcı yalnızca FIDO2 anahtarını kullanacaksanız, SSPR ilkesinden hariç tutun veya SSPR kayıt ilkesini devre dışı bırakın. 
- TAP, ağ Ilkesi sunucusu (NPS) uzantısı ve Active Directory Federasyon Hizmetleri (AD FS) (AD FS) bağdaştırıcısıyla birlikte kullanılamaz.
- Kiracıda KMSı etkinleştirildiğinde tek seferlik dokunma kullanımı zorlanmaz.
- Kiracıya sorunsuz SSO etkinleştirildiğinde, kullanıcılardan bir parola girmesi istenir. **Bunun yerine geçici erişim geçişinizi kullanın** bağlantısı, kullanıcının dokunarak oturum açması için kullanılabilir olacaktır.

![Bunun yerine dokunma kullan ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/alternative.png)

## <a name="troubleshooting"></a>Sorun giderme    

- Oturum açma sırasında bir kullanıcıya bir dokunma sunulmaz, aşağıdakileri denetleyin:
  - Kullanıcı, kimlik doğrulama yöntemi ilkesinin DOKıNDADıR.
  - Kullanıcının geçerli bir DOKUNMASı vardır ve tek seferlik bir kullanım ise henüz kullanılmadı.
- Kullanıcı kimlik bilgileri Ilkesi, dokunarak oturum açma sırasında göründüğünden, **geçici erişim geçiş oturumu açma engellendiyse** , aşağıdakileri denetleyin:
  - Kimlik doğrulama yöntemi ilkesi tek seferlik bir dokunma gerektirdiğinden kullanıcının birden çok kullanımda DOKUNMASı vardır.
  - Bir kerelik dokunma zaten kullanıldı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory bir passwordless kimlik doğrulama dağıtımı planlayın](howto-authentication-passwordless-deployment.md)

