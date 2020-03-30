---
title: Azure Etkin Dizin Koşullu Erişim'de konum durumu
description: Bulut uygulamalarınız için kullanıcının ağ konumuna göre erişimi denetlemek için konum koşulunu nasıl kullanacağınızı öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.workload: identity
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 915675af1e646f2cb77e36c0018ed372ff9496fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263237"
---
# <a name="what-is-the-location-condition-in-azure-active-directory-conditional-access"></a>Azure Active Directory Koşullu Erişim'de konum koşulu nedir? 

[Azure Active Directory (Azure AD) Koşullu Erişim](../active-directory-conditional-access-azure-portal.md)ile yetkili kullanıcıların bulut uygulamalarınıza nasıl erişebileceğini kontrol edebilirsiniz. Koşullu Erişim ilkesinin konum koşulu, erişim denetimleri ayarlarını kullanıcılarınızın ağ konumlarına bağlamanızı sağlar.

Bu makalede, konum durumunu yapılandırmak için gereken bilgileri sağlar.

## <a name="locations"></a>Konumlar

Azure AD, genel internetin herhangi bir yerinden cihazlara, uygulamalara ve hizmetlere tek oturum açma olanağı sağlar. Konum koşuluyla, bulut uygulamalarınız için erişimi bir kullanıcının ağ konumuna göre denetleyebilirsiniz. Konum durumu için yaygın kullanım örnekleri şunlardır:

- Şirket ağı dışındayken bir hizmete erişen kullanıcılar için çok faktörlü kimlik doğrulaması gerektiren.
- Belirli ülkelerden veya bölgelerden bir hizmete erişen kullanıcılar için erişimi engelleme.

Konum, adlandırılmış bir konumu veya çok faktörlü kimlik doğrulama Güvenilir IP'leri temsil eden ağ konumu için bir etikettir.

## <a name="named-locations"></a>Adlandırılmış konumlar

Adlandırılmış konumlarla, IP adresi aralıklarının veya ülkelerin ve bölgelerin mantıksal gruplandırmalarını oluşturabilirsiniz.

Koşullu Erişim sayfasının **Yönet** bölümünden adlandırılmış konumlarınıza erişebilirsiniz.

![Koşullu Erişim'de adlandırılmış konumlar](./media/location-condition/02.png)

Adlandırılmış bir konum aşağıdaki bileşenleri içerir:

![Adlandırılmış yeni bir konum oluşturma](./media/location-condition/42.png)

- **Ad** - Adlandırılmış bir konumun görüntü adı.
- **IP aralıkları** - CIDR formatında bir veya daha fazla IPv4 adres aralığı. IPv6 adres aralığı belirtilmesi desteklenmez.

   > [!NOTE]
   > IPv6 adres aralıkları şu anda adlandırılmış bir konuma dahil edilemez. Bu, IPv6 aralıklarının Koşullu Erişim ilkesinin dışında tutulamayacağı anlamına gelir.

- **Güvenilen konum olarak işaretle** - Güvenilir bir konumu belirtmek için adlandırılmış bir konum için ayarlanabileceğiniz bayrak. Genellikle, güvenilen konumlar BT departmanınız tarafından denetlenen ağ alanlarıdır. Koşullu Erişim'e ek olarak, güvenilen adlandırılmış konumlar, azure kimlik koruması ve Azure REKLAM güvenlik raporları tarafından [yanlış pozitif leri](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations-1)azaltmak için de kullanılır.
- **Ülkeler/Bölgeler** - Bu seçenek, adlandırılmış bir konumu tanımlamak için bir veya daha fazla ülke veya bölge seçmenize olanak tanır.
- **Bilinmeyen alanları ekleyin** - Bazı IP adresleri belirli bir ülkeye veya bölgeye eşlenmez. Bu seçenek, bu IP adreslerinin adlandırılmış konuma eklenmemesi gerektiğini seçmenize olanak tanır. Adlandırılmış konumu kullanan ilkenin bilinmeyen konumlara uygulanması gerektiğinde bu ayarı kullanın.

Yapılandırabileceğiniz adlandırılmış konum sayısı, Azure AD'deki ilgili nesnenin boyutuyla sınırlandırılmıştır. Konumları aşağıdaki sınırlamaları temel alınabiliyor:

- 1200'e kadar IP aralığına sahip bir konum adlandırılmış konum.
- Her birine atanmış bir IP aralığına sahip en fazla 90 adlandırılmış konum.

Koşullu Erişim ilkesi IPv4 ve IPv6 trafiği için geçerlidir. Şu anda adlandırılmış konumlar IPv6 aralıklarının yapılandırılmasına izin vermez. Bu sınırlama aşağıdaki durumlara neden olur:

- Koşullu Erişim ilkesi belirli IPv6 aralıklarına hedeflenemez
- Koşullu Erişim ilkesi belirli IPV6 aralıklarını hariç tutamaz

Bir ilke "Herhangi bir konum" için geçerli olacak şekilde yapılandırılırsa, IPv4 ve IPv6 trafiği için geçerli olacaktır. Belirtilen ülkeler ve bölgeler için yapılandırılan adlandırılmış konumlar yalnızca IPv4 adreslerini destekler. IPv6 trafiği yalnızca "bilinmeyen alanları ekleme" seçeneği seçilirse dahildir.

## <a name="trusted-ips"></a>Güvenilen IP'ler

Ayrıca, [çok faktörlü kimlik doğrulama hizmeti ayarlarında](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx)kuruluşunuzun yerel intranetini temsil eden IP adresi aralıklarını yapılandırabilirsiniz. Bu özellik, en fazla 50 IP adres aralığı yapılandırmanızı sağlar. IP adres aralıkları CIDR formatındadır. Daha fazla bilgi için Bkz. [Güvenilmiş IP'ler.](../authentication/howto-mfa-mfasettings.md#trusted-ips)  

Güvenilir IP'ler yapılandırıldıysa, konum koşuluna ait konumlar listesinde **MFA Trusted IPS** olarak gösterilir.

### <a name="skipping-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulamayı atlama

Çok faktörlü kimlik doğrulama hizmeti ayarları sayfasında, **intranetimdeki federe kullanıcılardan gelen istekler için çok faktörlü kimlik doğrulamasını atla'yı**seçerek kurumsal intranet kullanıcılarını tanımlayabilirsiniz. Bu ayar, AD FS tarafından verilen iç şirket ağı talebinin güvenilir olması ve kullanıcının şirket ağında olduğunu tanımlamak için kullanılması gerektiğini gösterir. Daha fazla bilgi için [Koşullu Erişim'i kullanarak Güvenilir IP'leri Etkinleştir özelliğine](../authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access)bakın.

Bu seçeneği işaretledikten sonra, adlandırılmış konum **MFA Trusted IPS** de dahil olmak üzere, bu seçeneği seçilen tüm ilkeler için geçerli olacaktır.

Uzun süreli oturum ömrüne sahip mobil ve masaüstü uygulamaları için Koşullu Erişim düzenli olarak yeniden değerlendirilir. Varsayılan değer saatte birdir. Şirket içi ağ talebi yalnızca ilk kimlik doğrulaması sırasında yayımlandığında, Azure AD'de güvenilir IP aralıklarının bir listesi olmayabilir. Bu durumda, kullanıcının hala şirket ağında olup olmadığını belirlemek daha zordur:

1. Kullanıcının IP adresinin güvenilir IP aralıklarından birinde olup olmadığını kontrol edin.
2. Kullanıcının IP adresinin ilk üç sekizlisinin ilk kimlik doğrulamanın IP adresinin ilk üç sekizlisi ile eşleşip eşleşmediğini kontrol edin. IP adresi, şirket içi ağ talebi ilk olarak yayımlandığında ve kullanıcı konumu doğrulandığında ilk kimlik doğrulamasıyla karşılaştırılır.

Her iki adım da başarısız olursa, kullanıcı artık güvenilir bir IP'de olmadığı kabul edilir.

## <a name="location-condition-configuration"></a>Konum durumu yapılandırması

Konum koşulunu yapılandırdığınızda, aşağıdakileri ayırt etme seçeneğiniz var:

- Herhangi bir konum
- Tüm güvenilen konumlar
- Seçili konumlar

![Konum durumu yapılandırması](./media/location-condition/01.png)

### <a name="any-location"></a>Herhangi bir konum

Varsayılan olarak, **Herhangi** bir konumu seçmek, internetteki herhangi bir adres anlamına gelen tüm IP adreslerine bir ilke uygulanmasına neden olur. Bu ayar, adlandırılmış konum olarak yapılandırdığınız IP adresleriyle sınırlı değildir. **Herhangi bir konumu**seçtiğinizde, belirli konumları bir ilkenin dışında tutabilirsiniz. Örneğin, kapsamı şirket ağı dışındaki tüm konumlara ayarlamak için güvenilen konumlar dışındaki tüm konumlara bir ilke uygulayabilirsiniz.

### <a name="all-trusted-locations"></a>Tüm güvenilen konumlar

Bu seçenek aşağıdakiler için geçerlidir:

- Güvenilen konum olarak işaretlenmiş tüm konumlar
- **MFA Trusted IPS** (yapılandırılırsa)

### <a name="selected-locations"></a>Seçili konumlar

Bu seçenekle, bir veya daha fazla adlandırılmış konum seçebilirsiniz. Bu ayarın uygulanabilmesi için, kullanıcının seçili konumlardan herhangi birinden bağlanması gerekir. Adlandırılmış ağların listesini gösteren adlı ağ seçim denetimini **seç'i** tıklattığınızda açılır. Liste ayrıca ağ konumunun güvenilir olarak işaretlenip işaretlenmediğini de gösterir. **MFA Trusted IP'leri** adlı adlandırılmış konum, çok faktörlü kimlik doğrulama hizmeti ayarı sayfasında yapılandırılabilen IP ayarlarını eklemek için kullanılır.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="when-is-a-location-evaluated"></a>Bir konum ne zaman değerlendirilir?

Koşullu Erişim ilkeleri şu anda değerlendirilir:

- Kullanıcı başlangıçta bir web uygulaması, mobil veya masaüstü uygulaması nda yer kaplar.
- Modern kimlik doğrulamakullanan bir mobil veya masaüstü uygulaması, yeni bir erişim jetonu elde etmek için yeni bir belirteç kullanır. Varsayılan olarak bu denetim saatte bir kez yapılır.

Bu denetim, modern kimlik doğrulaması kullanan mobil ve masaüstü uygulamaları için, ağ konumunu değiştirdin sonra bir saat içinde konum değişikliği algılanacak anlamına gelir. Modern kimlik doğrulaması kullanmayan mobil ve masaüstü uygulamaları için ilke her belirteç isteğinde uygulanır. İsteğin sıklığı uygulamaya bağlı olarak değişebilir. Benzer şekilde, web uygulamaları için, ilk oturumda uygulanan ilke ve web uygulamasında oturum ömrü için iyidir. Uygulamalar arasındaki oturum yaşam ları farklılıkları nedeniyle, ilke değerlendirmesi arasındaki süre de değişir. Uygulama her yeni oturum açma belirteci istediğinde, ilke uygulanır.

Varsayılan olarak, Azure AD saatlik olarak bir belirteç yayınlar. Şirket ağından ayrıldıktan sonra, bir saat içinde modern kimlik doğrulaması kullanan uygulamalar için ilke uygulanır.

### <a name="user-ip-address"></a>Kullanıcı IP adresi

İlke değerlendirmesinde kullanılan IP adresi, kullanıcının genel IP adresidir. Özel bir ağdaki aygıtlar için bu IP adresi intranetteki kullanıcının aygıtının istemci IP'si değil, genel internete bağlanmak için ağ tarafından kullanılan adrestir.

> [!WARNING]
> Cihazınızda yalnızca bir IPv6 adresi varsa, konum koşulunun yapılandırılması desteklenmez.

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Adlandırılmış yerlerin toplu yükleme ve indirme

Toplu güncelleştirmeler için adlandırılmış konumlar oluşturduğunuzda veya güncelleştirdiğinizde, IP aralıklarına sahip bir CSV dosyası yükleyebilir veya indirebilirsiniz. Yükleme, listedeki IP aralıklarını dosyadakilerle değiştirir. Dosyanın her satırı CIDR biçiminde bir IP Adresi aralığı içerir.

### <a name="cloud-proxies-and-vpns"></a>Bulut yakınlıkları ve VPN'ler

Bulut barındırılan proxy veya VPN çözümünün kullanıldığında, Azure AD'nin bir ilkeyi değerlendirirken kullandığı IP adresi proxy'nin IP adresidir. Kullanıcının genel IP adresini içeren X-Forwarded-For (XFF) üstbilgisi, güvenilir bir kaynaktan geldiğini doğrulamadığından kullanılmaz, bu nedenle bir IP adresi sahtesini yapmak için bir yöntem sunar.

Bir bulut proxy'si yerindeyken, bir etki alanı birleştirilmiş aygıt gerektiren bir ilke veya AD FS'den içeriden corpnet talebi kullanılabilir.

### <a name="api-support-and-powershell"></a>API desteği ve PowerShell

API ve PowerShell henüz adlandırılmış konumlar veya Koşullu Erişim ilkeleri için desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

- Koşullu Erişim ilkesini nasıl yapılandırdığınızı bilmek istiyorsanız, [Azure Active Directory Koşullu Erişim'e sahip belirli uygulamalar için MFA'yı zorunlu kılmasını gerektir](app-based-mfa.md)ini.
- Ortamınız için Koşullu Erişim ilkelerini yapılandırmaya hazırsanız, [Azure Etkin Dizini'nde Koşullu Erişim için en iyi uygulamalara](best-practices.md)bakın.
