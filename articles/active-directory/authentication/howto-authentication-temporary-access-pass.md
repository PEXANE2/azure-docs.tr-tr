---
title: Azure AD 'de, passwordless kimlik doğrulama yöntemlerini kaydetmek için geçici bir erişim geçişi yapılandırın
description: Kullanıcıların geçici bir erişim geçişi kullanarak parolasız kimlik doğrulama yöntemlerini nasıl kaydedeceğinizi ve etkinleştireceğinizi öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/03/2021
ms.author: justinha
author: inbarckms
manager: daveba
ms.reviewer: inbarckms
ms.collection: M365-identity-device-management
ms.openlocfilehash: 101e3ee9279d3560c0b561f0ea7ea695387bee15
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096471"
---
# <a name="configure-temporary-access-pass-in-azure-ad-to-register-passwordless-authentication-methods-preview"></a>Azure AD 'de geçici erişim geçişini, passwordless kimlik doğrulama yöntemlerini (Önizleme) kaydetmek için yapılandırma

Microsoft Authenticator uygulamasında FIDO2 ve passwordless telefon oturum açma gibi parolasız kimlik doğrulama yöntemleri, kullanıcıların parolasız güvenli bir şekilde oturum açmasını sağlar. Kullanıcılar passwordless yöntemlerini iki yöntemden biriyle önyükleyebilir:

- Mevcut Azure AD Multi-Factor Authentication yöntemlerini kullanma 
- Geçici erişim geçişi kullanma 

Geçici erişim geçişi, güçlü kimlik doğrulama gereksinimlerini karşılayan ve parolasız olanlar dahil diğer kimlik doğrulama yöntemlerini eklemek için kullanılabilen, bir yönetici tarafından verilen zaman sınırlı bir geçiş kodu olur. Ayrıca, bir Kullanıcı bir FIDO2 güvenlik anahtarı veya Microsoft Authenticator uygulaması gibi güçlü kimlik doğrulama faktörünü kaybettiği veya unuttuğunda, ancak yeni güçlü kimlik doğrulama yöntemlerini kaydetmek için oturum açması gereken durumlarda, geçici erişim geçişi de kurtarmayı kolaylaştırır.


Bu makalede, Azure AD 'de Azure portal kullanarak geçici bir erişim geçişinin nasıl etkinleştirileceği ve kullanılacağı gösterilmektedir. REST API 'Lerini kullanarak da bu eylemleri gerçekleştirebilirsiniz. 

>[!NOTE]
>Geçici erişim geçişi Şu anda genel önizlemededir. Bazı özellikler desteklenmeyebilir veya sınırlı özelliklere sahip olabilir. 

## <a name="enable-the-temporary-access-pass-policy"></a>Geçici erişim geçiş ilkesini etkinleştir

Geçici erişim geçiş ilkesi, kiracıda oluşturulan geçiş ömrü veya oturum açmak için geçici erişim geçişini kullanabilecek kullanıcılar ve gruplar gibi ayarları tanımlar. Herkesin geçici erişim geçişiyle oturum açabilmesi için, kimlik doğrulama yöntemi ilkesini etkinleştirmeniz ve geçici erişim geçişi kullanarak hangi kullanıcıların ve grupların oturum açabileceği belirlemeniz gerekir.
Herhangi bir kullanıcı için geçici bir erişim geçişi oluşturabilseniz de yalnızca ilkeye dahil olanlar onunla oturum açabilir.

Genel yönetici ve kimlik doğrulama yöntemi Ilkesi yönetici rolü sahipleri, geçici erişim geçişi kimlik doğrulama yöntemi ilkesini güncelleştirebilir.
Geçici erişim geçişi kimlik doğrulama yöntemi ilkesini yapılandırmak için:

1. Azure Portal genel yönetici olarak oturum açın ve **Azure Active Directory**  >  **güvenlik**  >  **kimlik doğrulama yöntemleri**  >  **geçici erişim geçişi**' ne tıklayın.
1. İlkeyi etkinleştirmek için **Evet** ' e tıklayın, ilkenin hangi kullanıcılara uygulandığını ve tüm **genel** ayarları seçin.

   ![Geçici erişim geçişi kimlik doğrulama yöntemi ilkesini etkinleştirme ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/policy.png)

   Varsayılan değer ve izin verilen değer aralığı aşağıdaki tabloda açıklanmıştır.


   | Ayar          | Varsayılan değerler | İzin verilen değerler               | Yorumlar                                                                                                                                                                                                                                                                 |   |
   |------------------|----------------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    En düşük ömür | 1 saat         | 10 – 43200 dakika (30 gün) | Geçici erişim geçişinin geçerli olduğu dakika sayısı alt sınırı.                                                                                                                                                                                                                         |   |
   | Maksimum ömür | 24 saat       | 10 – 43200 dakika (30 gün) | Geçici erişim geçişinin geçerli olduğu en fazla dakika sayısı.                                                                                                                                                                                                                         |   |
   | Varsayılan ömür | 1 saat         | 10 – 43200 dakika (30 gün) | Varsayılan değerler, ilke tarafından yapılandırılan minimum ve maksimum ömür dahilinde tek tek geçişler tarafından geçersiz kılınabilir                                                                                                                                                |   |
   | Bir kerelik kullanım     | Yanlış          | Doğru/yanlış                 | İlke false olarak ayarlandığında, Kiracıdaki geçişler bir veya birden çok kez (en fazla ömür süresi) bir kez kullanılabilir. Geçici erişim geçiş ilkesinde tek seferlik kullanımı zorlayarak kiracıda oluşturulan tüm geçişler tek seferlik kullanım olarak oluşturulur. |   |
   | Uzunluk           | 8              | 8-48 karakter              | Geçiş kodunun uzunluğunu tanımlar.                                                                                                                                                                                                                                      |   |

## <a name="create-a-temporary-access-pass-in-the-azure-ad-portal"></a>Azure AD portalında geçici erişim geçişi oluşturma

Bir ilkeyi etkinleştirdikten sonra, Azure AD 'de bir kullanıcı için geçici bir erişim geçişi oluşturabilirsiniz. Bu roller, geçici erişim geçişleriyle ilgili aşağıdaki eylemleri gerçekleştirebilir.

- Genel yönetici, herhangi bir kullanıcının (kendileri hariç) geçici erişim geçişini oluşturabilir, silebilir, görüntüleyebilir
- Ayrıcalıklı kimlik doğrulama yöneticileri Yöneticiler ve Üyeler üzerinde (kendileri hariç) geçici bir erişim geçişi oluşturabilir, silebilir, görüntüleyebilir
- Kimlik doğrulama yöneticileri, Üyeler üzerinde geçici bir erişim geçişi oluşturabilir, silebilir, görüntüleyebilir (kendileri hariç)
- Genel yönetici, kullanıcının geçici erişim geçiş ayrıntılarını (kodun kendisini okumadan) görüntüleyebilir.

Geçici erişim geçişi oluşturmak için:

1. Portalda genel yönetici, ayrıcalıklı kimlik doğrulama Yöneticisi veya kimlik doğrulama Yöneticisi olarak oturum açın. 
1. **Azure Active Directory**' ye tıklayın, kullanıcılar ' a gidin, *Chris yeşil* gibi bir kullanıcı seçin, sonra **kimlik doğrulama yöntemleri**' ni seçin.
1. Gerekirse, **Yeni Kullanıcı kimlik doğrulama yöntemleri deneyimini deneyin** seçeneğini belirleyin.
1. **Kimlik doğrulama yöntemleri ekleme** seçeneğini belirleyin.
1. Aşağıdaki **yöntemi seçin**, **geçici erişim geçişi (Önizleme)** seçeneğine tıklayın.
1. Özel bir etkinleştirme saati veya süresi tanımlayın ve **Ekle**' ye tıklayın.

   ![Geçici erişim geçişinin nasıl oluşturulacağı ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/create.png)

1. Eklendikten sonra, geçici erişim geçişinin ayrıntıları gösterilir. Gerçek geçici erişim geçiş değerini bir yere getirin. Bu değeri kullanıcıya sağlarsınız. **Tamam**' a tıkladıktan sonra bu değeri görüntüleyemezsiniz.
   
   ![Geçici erişim geçiş ayrıntılarının ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/details.png)

## <a name="use-a-temporary-access-pass"></a>Geçici bir erişim geçişi kullanın

Geçici erişim geçişi için en yaygın kullanım, bir kullanıcının, ek güvenlik istemlerini tamamlamaya gerek olmadan ilk oturum açma sırasında kimlik doğrulama ayrıntılarını kaydetmesi içindir. Kimlik doğrulama yöntemleri konumunda kaydedilir [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) . Kullanıcılar ayrıca, mevcut kimlik doğrulama yöntemlerini buradan güncelleştirebilir.

1. İçin bir Web tarayıcısı açın [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) .
1. İçin geçici erişim geçişini oluşturduğunuz hesabın UPN 'sini girin (örneğin,) *tapuser@contoso.com* .
1. Kullanıcı geçici erişim geçiş ilkesine dahil ise, geçici erişim geçişini girmek üzere bir ekran görür.
1. Azure portal görüntülenen geçici erişim geçişini girin.

   ![Geçici erişim geçişinin nasıl girediğinin ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/enter.png)

>[!NOTE]
>Federasyon etki alanları için, Federasyon üzerinden geçici bir erişim geçişi tercih edilir. Geçici erişim geçişine sahip bir Kullanıcı, Azure AD 'de kimlik doğrulamasını tamamlar ve federal kimlik sağlayıcısına (IDP) yönlendirilmeyecektir.

Kullanıcı artık oturum açtı ve FIDO2 güvenlik anahtarı gibi bir yöntemi güncelleştirebilir veya kaydedebilir. Kimlik bilgilerinin veya cihazının kaybedilmesi nedeniyle kimlik doğrulama yöntemlerini güncelleştiren kullanıcılar eski kimlik doğrulama yöntemlerini kaldırdıklarından emin olmanızı sağlamalıdır.

Kullanıcılar, doğrudan Doğrulayıcı uygulamasından parolasız telefon oturum açma için kaydolmak üzere geçici erişim geçişini de kullanabilirler. Daha fazla bilgi için bkz. [Microsoft Authenticator uygulamasına iş veya okul hesabınızı ekleme](../user-help/user-help-auth-app-add-work-school-account.md).

![İş veya okul hesabını kullanarak geçici erişim geçişinin nasıl girediğinin ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/enter-work-school.png)

## <a name="delete-a-temporary-access-pass"></a>Geçici erişim geçişini silme

Zaman aşımına uğradı geçici erişim geçişi kullanılamaz. Bir kullanıcının **kimlik doğrulama yöntemlerinin** altında, **ayrıntı** sütunu, geçici erişim geçişinin ne zaman dolduğunu gösterir. Aşağıdaki adımları kullanarak, bir zaman aşımına uğradı geçici erişim geçişini silebilirsiniz:

1. Azure AD portalında, **Kullanıcılar**' a gidin, Kullanıcı ' *ya dokunun*, ardından **kimlik doğrulama yöntemleri**' ni seçin.
1. Listede gösterilen **geçici erişim geçişi (Önizleme)** kimlik doğrulama yönteminin sağ tarafında **Sil**' i seçin.

## <a name="replace-a-temporary-access-pass"></a>Geçici erişim geçişini değiştirme 

- Bir Kullanıcı yalnızca bir adet geçici erişim geçişine sahip olabilir. Geçiş kodu, geçici erişim geçişinin başlangıç ve bitiş saati sırasında kullanılabilir.
- Kullanıcı yeni bir geçici erişim geçişi gerektiriyorsa:
  - Mevcut geçici erişim geçişi geçerliyse, yöneticinin mevcut geçici erişim geçişini silmesi ve Kullanıcı için yeni bir geçiş oluşturması gerekir. Geçerli bir geçici erişim geçişini silmek kullanıcının oturumlarını iptal eder. 
  - Mevcut geçici erişim geçişinin süresi dolmuşsa, yeni bir geçici erişim geçişi mevcut geçici erişim geçişini geçersiz kılar ve kullanıcının oturumlarını iptal etmez.

Ekleme ve kurtarmaya yönelik NıST standartları hakkında daha fazla bilgi için bkz. [NIST özel yayını 800-63A](https://pages.nist.gov/800-63-3/sp800-63a.html#sec4).

## <a name="limitations"></a>Sınırlamalar

Bu sınırlamaları aklınızda bulundurun:

- FIDO2 veya Phone oturum açma gibi bir passwordless yöntemi kaydetmek için tek seferlik bir geçici erişim geçişi kullanırken, kullanıcının kaydı tek seferlik geçici erişim geçişine sahip 10 dakikalık bir oturum açma süresi içinde tamamlaması gerekir. Bu sınırlama, birden çok kez kullanılabilecek geçici bir erişim geçişine uygulanmaz.
- Konuk kullanıcılar geçici bir erişim geçişine oturum açamaz.
- Self servis parola sıfırlama (SSPR) kayıt ilkesi kapsamındaki kullanıcıların, bir geçici erişim geçişine oturum açtıktan sonra SSPR yöntemlerinden birini kaydetmesi gerekecektir. Kullanıcı yalnızca FIDO2 anahtarını kullanacaksanız, SSPR ilkesinden hariç tutun veya SSPR kayıt ilkesini devre dışı bırakın. 
- Geçici erişim geçişi, ağ Ilkesi sunucusu (NPS) uzantısı ve Active Directory Federasyon Hizmetleri (AD FS) (AD FS) bağdaştırıcısıyla birlikte kullanılamaz.
- Kiracıya sorunsuz SSO etkinleştirildiğinde, kullanıcılardan bir parola girmesi istenir. **Bunun yerine geçici erişim geçişinizi kullanın** bağlantısı, kullanıcının geçici bir erişim geçişiyle oturum açması için kullanılabilir olacaktır.

![Bunun yerine geçici erişim geçişi kullan ekran görüntüsü](./media/how-to-authentication-temporary-access-pass/alternative.png)

## <a name="troubleshooting"></a>Sorun giderme    

- Oturum açma sırasında bir kullanıcıya geçici erişim geçişi sunulmuyor ise, aşağıdakileri denetleyin:
  - Kullanıcı, geçici erişim geçişi kimlik doğrulama yöntemi ilkesinin kapsamındadır.
  - Kullanıcının geçerli bir geçici erişim geçişi vardır ve tek seferlik bir kullanım ise henüz kullanılmadı.
- Geçici erişim geçişinin oturum açması, oturum açma sırasında **Kullanıcı kimlik bilgileri ilkesi** , geçici erişim geçişi sırasında göründüğü için engellenirse, aşağıdakileri denetleyin:
  - Kimlik doğrulama yöntemi ilkesi, bir kerelik geçici erişim geçişi gerektirdiğinden kullanıcının çoklu kullanımı geçici erişim geçişi vardır.
  - Bir kerelik geçici erişim geçişi zaten kullanıldı.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Active Directory bir passwordless kimlik doğrulama dağıtımı planlayın](howto-authentication-passwordless-deployment.md)

