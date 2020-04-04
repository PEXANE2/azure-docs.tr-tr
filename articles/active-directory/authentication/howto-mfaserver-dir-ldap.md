---
title: LDAP Kimlik Doğrulama ve Azure MFA Server - Azure Active Directory
description: LDAP Kimlik Doğrulama ve Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nun dağıtımı.
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
ms.openlocfilehash: 2df89a764bc9b92a830b13e4216412694bb95523
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80653027"
---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP kimlik doğrulama ve Azure Çok Faktörlü Kimlik Doğrulama Sunucusu

Varsayılan olarak, Azure Multi-Factor Authentication Sunucusu kullanıcıları Active Directory'den aktarmak ya da eşitlemek üzere yapılandırılmıştır. Ancak, ADAM dizini veya belirli Active Directory etki alanı denetleyicisi gibi farklı LDAP dizinlerini bağlamak için yapılandırılabilir. LDAP üzerinden bir dizine bağlandığında, Azure Çok Faktörlü Kimlik Doğrulama Sunucusu kimlik doğrulamaları gerçekleştirmek için ldap proxy'si olarak hareket edebilir. Ayrıca, IIS Kimlik Doğrulaması ile ön kimlik doğrulaması için ya da Azure MFA kullanıcı portalında birincil kimlik doğrulaması için LDAP bağlamanın RADIUS hedefi olarak kullanılmasına izin verir.

Azure Çok Faktörlü Kimlik Doğrulama'yı LDAP proxy'si olarak kullanmak için, Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nu LDAP istemcisi (örneğin, VPN cihazı, uygulama) ile LDAP dizin sunucusu arasına ekleyin. Azure Multi-Factor Authentication doğrulama sunucusunun istemci sunucular ve LDAP dizini ile iletişim kurmak için yapılandırılmış olması gerekir. Bu yapılandırmada, Azure Multi-Factor Authentication Sunucusu istemci sunucularından uygulamalarından gelen LDAP isteklerini kabul eder ve bunları birincil kimlik bilgilerini doğrulamak amacıyla hedef LDAP dizini sunucusuna iletir. LDAP dizini birincil kimlik bilgilerini doğrularsa, Azure Çok Faktörlü Kimlik Doğrulaması ikinci bir kimlik doğrulama sıcatır ve LDAP istemcisine yanıt gönderir. Kimlik doğrulama sürecinin tamamının başarılı olması için hem LDAP sunucusu kimlik doğrulamasının hem de ikinci adım kimlik doğrulama işleminin başarılı olması gerekir.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

## <a name="configure-ldap-authentication"></a>LDAP kimlik doğrulamasını yapılandırma

LDAP kimlik doğrulamasını yapılandırmak için, bir Windows sunucusuna Azure Multi-Factor Authentication Sunucusu yükleyin. Aşağıdaki yordamı kullanın:

### <a name="add-an-ldap-client"></a>LDAP istemcisi ekleme

1. Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nda sol menüdeki LDAP Kimlik Doğrulama simgesini seçin.
2. **LDAP Kimlik Doğrulamasını etkinleştir** onay kutusunu işaretleyin.

   ![MFA Server'da LDAP Kimlik Doğrulaması](./media/howto-mfaserver-dir-ldap/ldap2.png)

3. İstemciler sekmesinde, Azure Çok Faktörlü Kimlik Doğrulama LDAP hizmetiLDAP isteklerini dinlemek için standart olmayan bağlantı noktalarına bağlanacaksa TCP bağlantı noktasını ve SSL (TLS) bağlantı noktasını değiştirin.
4. İstemciden Azure Çok Faktörlü Authentication Server'a LDAPS kullanmayı planlıyorsanız, MFA Server ile aynı sunucuya bir TLS/SSL sertifikası yüklenmesi gerekir. SSL (TLS) sertifika kutusunun yanındaki **Gözat'ı** tıklatın ve güvenli bağlantı için kullanılacak bir sertifika seçin.
5. **Ekle**’ye tıklayın.
6. LDAP İstemci Ekle iletişim kutusuna, sunucuya ve Uygulama adını doğrulayan cihazın, sunucunun veya uygulamanın IP adresini girin (isteğe bağlı). Uygulama adı Azure Multi-Factor Authentication raporlarında görünür ve SMS veya Mobil Uygulama kimlik doğrulama iletilerinde görüntülenebilir.
7. Tüm kullanıcılar Sunucu’ya aktarılmışsa ya da aktarılacaksa ve iki aşamalı doğrulamaya tabi olacaksa **Azure Multi-Factor Authentication kullanıcılarının eşleşmesini gerektir** kutusunu işaretleyin. Sunucuya henüz önemli sayıda kullanıcı alınmadıysa ve/veya iki aşamalı doğrulamadan muafsa, kutuyu işaretsiz bırakın. Bu özellik hakkında ek bilgi için MFA Server yardım dosyasına bakın.

Bu adımları tekrarlayarak diğer LDAP istemcilerini ekleyin.

### <a name="configure-the-ldap-directory-connection"></a>LDAP dizini bağlantısını yapılandırma

Azure Multi-Factor Authentication LDAP kimlik doğrulamaları almak üzere yapılandırıldığında, bu kimlik doğrulamalarını LDAP dizinine sunmalıdır. Bu nedenle, LDAP hedefi olarak kullanmak üzere, Hedef sekmesi yalnızca tek bir, gri seçenek görüntüler.

> [!NOTE]
> Dizin tümleştirmesi, Active Directory Domain Services dışındaki dizinlerle çalışmak için garanti edilmez.

1. LDAP dizini bağlantısını yapılandırmak için **Dizin Tümleştirme** simgesine tıklayın.
2. Ayarlar sekmesinde **Özel LDAP yapılandırması kullan** radyo düğmesini seçin.
3. **Düzenle…** seçeneğini belirleyin.
4. LDAP Yapılandırmasını Düzenle iletişim kutusunda, LDAP dizinine bağlanmak için gerekli bilgilerle alanları doldurun. Bu alanların açıklamaları Azure Multi-Factor Authentication Sunucusu yardım dosyasında da bulunmaktadır.

    ![Dizin Entegrasyonu LDAP config](./media/howto-mfaserver-dir-ldap/ldap.png)

5. **Test** düğmesine tıklayarak LDAP bağlantısını test edin.
6. LDAP bağlantı testi başarılı olduysa, **Tamam** düğmesini tıklatın.
7. **Filtreler** sekmesini tıklatın. Sunucu, Kapsayıcılar, güvenlik grupları ve Etkin Dizin kullanıcıları yüklemek için önceden yapılandırılmıştır. Farklı bir LDAP dizinine bağlanıyorsanız, büyük olasılıkla görüntülenen filtreleri düzenlemeniz gerekir. Filtreler hakkında daha fazla bilgi için **Yardım** bağlantısına tıklayın.
8. **Öznitelikler** sekmesini tıklatın. Sunucu, Active Directory'deki öznitelikleri eşlemek için önceden yapılandırılmıştır.
9. Farklı bir LDAP dizinine bağlıyorsanız veya önceden yapılandırılmış öznitelik eşlemelerini değiştirmek istiyorsanız için **Düzenle…** düğmesine tıklayın.
10. Öznitelikleri Düzenle iletişim kutusunda, dizininizin LDAP öznitelik eşlemelerini değiştirin. Öznitelik adlarını yazabilir veya **…** simgesine tıklayarak seçebilirsiniz. düğmesine tıklanarak seçilebilir. Öznitelikler hakkında daha fazla bilgi için **Yardım** bağlantısına tıklayın.
11. **Tamam** düğmesini tıklatın.
12. **Şirket Ayarları** simgesine tıklayın ve **Kullanıcı Adı Çözümleme** sekmesini seçin.
13. Active Directory’ye etki alanına katılmış bir sunucudan bağlanıyorsanız, **Kullanıcı adlarını eşlemek için Windows güvenlik tanımlayıcılarını (SID’ler) kullan** radyo düğmesini seçili bırakın. Diğer durumlarda **Kullanıcı adlarını eşlemek için LDAP benzersiz tanımlayıcı özniteliğini kullan** radyo düğmesini seçin.

**Kullanıcı adlarını eşlemek için LDAP benzersiz tanımlayıcı özniteliğini kullan** radyo düğmesi seçili olduğunda Azure Multi-Factor Authentication Sunucusu her kullanıcı adını LDAP dizinindeki benzersiz bir tanımlayıcıya çözümlemeyi dener. LdAP araması, Dizin Tümleştirmesi -> Öznitelikleri sekmesinde tanımlanan Kullanıcı adı öznitelikleri üzerinde gerçekleştirilir. Bir kullanıcı kimlik doğruladığında, kullanıcı adı LDAP dizinindeki benzersiz tanımlayıcıya çözülür. Benzersiz tanımlayıcı, Azure Çok Faktörlü Kimlik Doğrulama veri dosyasındaki kullanıcıyla eşleştirmek için kullanılır. Bu, büyük/küçük harf duyarsız karşılaştırmaları ve uzun ve kısa kullanıcı adı biçimleri sağlar.

Bu adımları tamamladıktan sonra, MFA Server yapılandırılan istemcilerden LDAP erişim istekleri için yapılandırılan bağlantı noktalarını dinler ve kimlik doğrulaması için LDAP dizinine gelen bu istekler için bir proxy görevi görür.

## <a name="configure-ldap-client"></a>LDAP istemcisi yapılandırma

LDAP istemcisini yapılandırmak için yönergeleri kullanın:

* LDAP aracılığıyla gerecinizi, sunucunuzu ya da uygulamanızı LDAP dizinindeymiş gibi Azure Multi-Factor Authentication Sunucusu için kimliğini doğrulamak üzere yapılandırın. Azure Multi-Factor Authentication Sunucusu’na ait olacak sunucu adı ya da IP adresi için olanlar dışında, LDAP dizininize normalde doğrudan bağlandığınızla aynı ayarları kullanın.
* LDAP zaman aşımını LDAP dizini ile kullanıcının kimlik bilgilerini doğrulamak, ikinci adım doğrulamayı gerçekleştirmek, yanıtlarını almak ve LDAP erişim isteğine yanıt vermek için zaman olacak şekilde 30-60 saniyeye yapılandırın.
* LDAPS kullanıyorsanız, LDAP sorgularını yapan cihaz veya sunucu, Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'na yüklenen TLS/SSL sertifikasına güvenmelidir.
