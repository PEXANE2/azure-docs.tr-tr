---
title: RADIUS kullanan RDG ve Azure MFA Server - Azure Active Directory
description: Bu, RADIUS kullanan Uzak Masaüstü (RD) Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu’nu dağıtmada yardımcı olacak Azure Multi-factor authentication sayfasıdır.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 23e2f7424464860b647883be2441e903900cb266
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80652878"
---
# <a name="remote-desktop-gateway-and-azure-multi-factor-authentication-server-using-radius"></a>RADIUS kullanan Uzak Masaüstü Ağ Geçidi ve Azure Multi-Factor Authentication Sunucusu

Genellikle, Uzak Masaüstü (RD) Ağ Geçidi, kullanıcıların kimliğini doğrulamak için yerel [Ağ İlkesi Hizmetlerini (NPS)](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_optionalfeatures) kullanır. Bu makale, Uzak Masaüstü Ağ Geçidi’ndeki RADIUS isteklerini (yerel NPS üzerinden) Multi-Factor Authentication Sunucusu’na yönlendirmeyi açıklar. Azure MFA ve RD Ağ Geçidi bileşimi, kullanıcılarınızın güçlü kimlik doğrulaması ile diledikleri yerden çalışma ortamlarına erişebileceği anlamına gelir.

Server 2012 R2’de terminal hizmetleri için Windows Kimlik Doğrulaması desteklenmediğinden, MFA Sunucusu ile tümleştirmek için RD Ağ Geçidi ve RADIUS kullanın.

Multi-Factor Authentication Sunucusu'nu, RADIUS isteğini Uzak Masaüstü Ağ Geçidi Sunucusu'ndaki NPS'ye sunan ayrı bir sunucuya yükleyin. NPS, kullanıcı adını ve parolayı doğruladıktan sonra Multi-Factor Authentication Sunucusu'na bir yanıt gönderir. Ardından MFA Sunucusu, ikinci kimlik doğrulama faktörünü uygular ve ağ geçidine bir sonuç gönderir.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

## <a name="prerequisites"></a>Ön koşullar

- Etki alanına katılmış bir Azure MFA Sunucusu. Henüz yüklü değilse, [Azure Multi-Factor Authentication Sunucusu’nu kullanmaya başlama](howto-mfaserver-deploy.md) bölümündeki adımları izleyin.
- Varolan yapılandırılmış Bir NPS Sunucusu.
- Ağ İlkesi Hizmetleri’nde kimlik doğrulaması yapan bir Uzak Masaüstü Ağ Geçidi.

> [!NOTE]
> Bu makale, Azure MFA (Bulut tabanlı) ile değil, yalnızca MFA Server dağıtımlarında kullanılmalıdır.

## <a name="configure-the-remote-desktop-gateway"></a>Uzak Masaüstü Ağ Geçidini yapılandırma

RD Ağ Geçidini, bir Azure Multi-Factor Authentication Sunucusu’na RADIUS kimlik doğrulaması gönderecek şekilde yapılandırın.

1. RD Ağ Geçidi Yöneticisi’nde sunucu adına sağ tıklayıp **Özellikler**’i seçin.
2. **RD CAP Deposu** sekmesine gidin ve **NPS çalıştıran merkezi sunucu** öğesini seçin.
3. Her bir sunucunun adını veya IP adresini girerek, RADIUS sunucuları olarak bir ya da daha fazla Azure Multi-Factor Authentication Sunucusu ekleyin.
4. Her sunucu için paylaşılan gizlilik oluşturun.

## <a name="configure-nps"></a>NPS yapılandırma

RD Ağ Geçidi, Azure Multi-Factor Authentication’a RADIUS isteği göndermek için NPS kullanır. NPS’yi yapılandırmak için, RD Ağ Geçidinin iki adımlı doğrulama tamamlanmadan zaman aşımına uğramasını önlemek üzere ilk olarak zaman aşımı ayarlarını değiştirin. Ardından, MFA sunucunuzdan RADIUS kimlik doğrulamalarını almak için NPS’yi güncelleştirin. NPS’yi yapılandırmak için aşağıdaki yordamı kullanın:

### <a name="modify-the-timeout-policy"></a>Zaman aşımı ilkesini değiştirme

1. NPS’de, sol sütundaki **RADIUS İstemcileri ve Sunucu** menüsünü açın ve **Uzak RADIUS Sunucu Grupları**’nı seçin.
2. **TS AĞ GEÇİDİ SUNUCU GRUBU**’nu seçin.
3. **Yük Dengeleme** sekmesine gidin.
4. **İstek düşmeden önce yanıt sız saniye sayısını** ve sunucunun 30 ile 60 saniye arasında **kullanılamadığı tespit edildiğinde istekler arasındaki saniye sayısını** değiştirin. (Kimlik doğrulaması sırasında sunucu yine de zaman aşımına uğrarsa, buraya geri dönerek saniye sayısını artırabilirsiniz.)
5. **Kimlik Doğrulama/Hesap** sekmesine gidin ve belirtilen RADIUS bağlantı noktalarının, Multi-Factor Authentication Sunucusu’nun dinlediği bağlantı noktalarıyla eşleştiğinden emin olun.

### <a name="prepare-nps-to-receive-authentications-from-the-mfa-server"></a>NPS’yi MFA Sunucusundan kimlik doğrulamaları almaya hazırlama

1. Sol sütundaki RADIUS İstemcileri ve Sunucuları altında bulunan **RADIUS İstemcileri**’ne sağ tıklayıp **Yeni**’yi seçin.
2. RADIUS istemcisi olarak Azure Multi-Factor Authentication Sunucusu ekleme Kolay bir ad seçin ve paylaşılan gizlilik belirtin.
3. Sol sütundaki **İlkeler** menüsünü açıp **Bağlantı İsteği İlkeleri**’ni seçin. RD Ağ Geçidi yapılandırıldığında oluşturulan TS AĞ GEÇİDİ KİMLİK DOĞRULAMA İLKESİ adlı bir ilke görürsünüz. Bu ilke RADIUS isteklerini Multi-Factor Authentication Sunucusu’na iletir.
4. **TS AĞ GEÇİDİ KİMLİK DOĞRULAMA İLKESİ**’ne sağ tıklayıp **İlkeyi Yinele**’yi seçin.
5. Yeni ilkeyi açıp **Koşullar** sekmesine gidin.
6. Azure Multi-Factor Authentication Sunucusu RADIUS istemcisi için 2. adımda ayarladığınız Kolay adla eşleşen bir İstemci Kolay Adı koşulu ekleyin.
7. **Ayarlar** sekmesine gidin ve **Kimlik Doğrulaması**’nı seçin.
8. Kimlik Doğrulama Sağlayıcısı’nı **Bu sunucu üzerindeki isteklerin kimliğini doğrula** olarak değiştirin. Bu ilke, NPS Azure MFA Sunucusu’ndan bir RADIUS isteği aldığında, döngü koşuluna neden olacak şekilde RADIUS isteğini Azure Multi-Factor Authentication Sunucusu’na geri göndermek yerine, kimlik doğrulamasının yerel olarak gerçekleştirilmesini sağlar.
9. Döngü koşulunu önlemek için yeni ilkenin **Bağlantı İsteği İlkeleri** bölmesindeki özgün ilkenin ÜZERİNDE bulunduğundan emin olun.

## <a name="configure-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication’ı yapılandırma

Azure Multi-Factor Authentication Sunucusu, RD Ağ Geçidi ile NPS arasında bir RADIUS proxy olarak yapılandırılmıştır.  RD AĞ Geçidi sunucusundan ayrı bir etki alanına katılmış sunucuya yüklenmelidir. Azure Multi-Factor Authentication Sunucusu’nu yapılandırmak için aşağıdaki yordamı uygulayın.

1. Azure Multi-Factor Authentication Sunucusu’nu açın ve RADIUS Kimlik Doğrulaması simgesini seçin.
2. RADIUS **kimlik doğrulama onay** kutusunu etkinleştir'i denetleyin.
3. İstemciler sekmesinde, bağlantı noktalarının NPS’de yapılandırılanlarla eşleştiğinden emin olun ve **Ekle** düğmesini seçin.
4. RD Ağ Geçidi sunucusu IP adresi, uygulama adı (isteğe bağlı) ve paylaşılan gizlilik ekleyin. Paylaşılan gizliliğin Azure Multi-Factor Authentication Sunucusu’nda ve RD Ağ Geçidinde aynı olması gerekir.
3. **Hedef** sekmesine gidin ve **RADIUS sunucuları** radyo düğmesini seçin.
4. **Ekle**’yi seçip IP adresi, paylaşılan gizlilik ve NPS sunucusu bağlantı noktalarını girin. Merkezi NPS kullanmadığınız sürece, RADIUS istemcisi ile RADIUS hedefi aynıdır. Paylaşılan gizliliğin, NPS sunucusunun RADIUS istemcisi bölümündekiyle eşleşmesi gerekir.

![MFA Server'da Yarıçap Kimlik Doğrulaması](./media/howto-mfaserver-nps-rdg/radius.png)

## <a name="next-steps"></a>Sonraki adımlar

- Azure MFA ve [IIS web uygulamalarını](howto-mfaserver-iis.md) tümleştirme

- [Azure Multi-Factor Authentication SSS](multi-factor-authentication-faq.md) bölümünde sorularınızın yanıtlarını alın
