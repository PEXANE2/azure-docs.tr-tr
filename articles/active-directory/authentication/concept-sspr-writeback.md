---
title: Self servis parola sıfırlama ile şirket içi parola yazma - Azure Active Directory
description: Azure Etkin Dizin'deki parola değişikliği veya sıfırlama etkinliklerinin şirket içi dizin ortamına nasıl yazıldığını öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 04/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rhicock
ms.collection: M365-identity-device-management
ms.openlocfilehash: 787c15c11c995c7eb30662131302658175c7f877
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393014"
---
# <a name="how-does-self-service-password-reset-writeback-work-in-azure-active-directory"></a>Azure Active Directory'de self servis parola sıfırlama geri yazma nasıl çalışır?

Azure Etkin Dizin (Azure AD) self servis parola sıfırlama (SSPR), kullanıcıların parolalarını bulutta sıfırlamasına olanak tanır, ancak çoğu şirketin kullanıcılarının bulunduğu şirket içi Active Directory Domain Services (AD DS) ortamı da vardır. Parola yazma, [Buluttaki](../hybrid/whatis-hybrid-identity.md) parola değişikliklerinin varolan şirket içi dizine gerçek zamanlı olarak geri yazılmasını sağlayan Azure AD Connect ile etkinleştirilen bir özelliktir. Bu yapılandırmada, kullanıcılar bulutta SSPR kullanarak parolalarını değiştirdikçe veya sıfırladıkça, güncelleştirilmiş parolalar şirket içi AD DS ortamına da geri yazıldı

Parola yazma, aşağıdaki karma kimlik modellerini kullanan ortamlarda desteklenir:

* [Parola karması eşitleme](../hybrid/how-to-connect-password-hash-synchronization.md)
* [Doğrudan kimlik doğrulama](../hybrid/how-to-connect-pta.md)
* [Active Directory Federasyon Hizmetleri](../hybrid/how-to-connect-fed-management.md)

Parola yazma aşağıdaki özellikleri sağlar:

* **Şirket içi Active Directory Domain Services (AD DS) parola ilkelerinin uygulanması**: Bir kullanıcı parolasını sıfırladığında, bu dizine işlemeden önce şirket içi AD DS politikanızı karşıladığından emin olmak için denetlenir. Bu inceleme, AD DS'de tanımladığınız geçmiş, karmaşıklık, yaş, parola filtreleri ve diğer parola kısıtlamalarını denetlemeyi içerir.
* **Sıfır gecikmegeribildirim**: Şifre yazma senkron bir işlemdir. Parolaları ilkeyi karşılamazsa veya herhangi bir nedenle sıfırlanamazsa veya değiştirilemiyorsa kullanıcılara hemen bildirilir.
* **Erişim panelinden ve Office 365'ten parola değişikliklerini destekler**: Federe veya parola karma senkronize edilmiş kullanıcılar süresi dolmuş veya süresi dolmamış parolalarını değiştirmeye geldiklerinde, bu parolalar AD DS'ye geri yazılır.
* **Bir yönetici bunları Azure portalından sıfırladığında parola yazmayı destekler**: Bir yönetici [azure portalında](https://portal.azure.com)bir kullanıcının parolasını sıfırladığında , bu kullanıcı federe veya parola karma senkronize edilirse, parola şirket içinde geri yazılır. Bu işlev şu anda Office yönetici portalında desteklenmez.
* **Gelen güvenlik duvarı kuralları gerektirmez**: Parola yazma, temel iletişim kanalı olarak Azure Hizmet Veri Servisi rölesi kullanır. Tüm iletişim bağlantı noktası 443 üzerinden giden.

> [!NOTE]
> Şirket içi AD'de korumalı gruplar içinde bulunan yönetici hesapları parola yazma ile kullanılabilir. Yöneticiler bulutta parolalarını değiştirebilir, ancak unutulan bir parolayı sıfırlamak için parola sıfırlamayı kullanamaz. Korumalı gruplar hakkında daha fazla bilgi [için, Etkin Dizini'ndeki Korumalı hesaplar ve gruplara](/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory)bakın.

## <a name="how-password-writeback-works"></a>Parola geri yazma nasıl çalışır?

Federe veya parola karma senkronize kullanıcı bulutta parolasını sıfırlamaya veya değiştirmeye çalıştığında aşağıdaki eylemler oluşur:

1. Kullanıcının ne tür bir parolaya sahip olduğunu görmek için bir denetim gerçekleştirilir. Parola şirket içinde yönetiliyorsa:
   * Geri yazma hizmetinin çalışır durumda olup olmadığını görmek için bir denetim gerçekleştirilir. Eğer varsa, kullanıcı devam edebilirsiniz.
   * Yazma hizmeti kapatılırsa, kullanıcı parolasının şu anda sıfırlanamayacağı konusunda bilgilendirilir.
1. Ardından, kullanıcı uygun kimlik doğrulama kapılarından geçer ve **parolayı sıfırla** sayfasına ulaşır.
1. Kullanıcı yeni bir parola seçer ve onaylar.
1. Kullanıcı **Gönder'i**seçtiğinde, düz metin parolası, geri yazma kurulum işlemi sırasında oluşturulan simetrik bir anahtarla şifrelenir.
1. Şifrelenmiş parola, bir HTTPS kanalı üzerinden kiracıya özel servis veri günü rölesine gönderilen bir yüke dahildir (geri yazma kurulum işlemi sırasında sizin için ayarlanır). Bu röle, yalnızca şirket içi yüklemenizin bildiği rasgele oluşturulan bir parolayla korunur.
1. İleti servis veri veri nesi'ne ulaştıktan sonra, parola sıfırlama bitiş noktası otomatik olarak uyanır ve bekleyen bir sıfırlama isteği olduğunu görür.
1. Hizmet daha sonra bulut bağlantı özelliğini kullanarak kullanıcıyı arar. Bu aramanın başarılı olabilmesi için aşağıdaki koşulların yerine getirilmesi gerekir:

   * Kullanıcı nesnesi Active Directory bağlayıcı alanında bulunmalıdır.
   * Kullanıcı nesnesi ilgili metaverse (MV) nesneye bağlı olmalıdır.
   * Kullanıcı nesnesinin ilgili Azure Etkin Dizin bağlayıcısı nesnesine bağlı olması gerekir.
   * Active Directory bağlayıcısı nesnesinden MV'ye gelen bağlantının `Microsoft.InfromADUserAccountEnabled.xxx` bağlantıda eşitleme kuralı olması gerekir.

   Arama buluttan geldiğinde, eşitleme altyapısı Azure Active Directory bağlayıcısı uzay nesnesini aramak için **bulutÇapa** özniteliğini kullanır. Daha sonra MV nesnesine olan bağlantıyı izler ve sonra Etkin Dizin nesnesine olan bağlantıyı izler. Aynı kullanıcı için birden çok Active Directory nesnesi (çok orman) olabileceğinden, eşitleme altyapısı doğru olanı seçmek için `Microsoft.InfromADUserAccountEnabled.xxx` bağlantıya güvenir.

1. Kullanıcı hesabı bulunduktan sonra, parolayı doğrudan uygun Active Directory ormanında sıfırlama girişiminde bulunulr.
1. Parola kümesi işlemi başarılı olursa, kullanıcıya parolasının değiştirildiğini niçin değiştirdiği söylenir.

   > [!NOTE]
   > Kullanıcının parola karma sıcağa parola karma lıleştirme kullanılarak Azure AD ile senkronize edilirse, şirket içi parola ilkesinin bulut parola ilkesinden daha zayıf olma olasılığı vardır. Bu durumda, şirket içi ilke uygulanır. Bu ilke, tek oturum açma sağlamak için parola karma eşitleme veya federasyon kullanıp kullanmadığınızönemli olursa olsun, şirket içi politikanızın bulutta uygulanmasını sağlar.

1. Parola kümesi işlemi başarısız olursa, bir hata kullanıcıyı yeniden denemeye sorar. İşlem aşağıdaki nedenlerden dolayı başarısız olabilir:
    * Servis iniyordu.
    * Seçtikleri parola kuruluşun ilkelerine uymuyor.
    * Yerel Active Directory'de kullanıcı yı bulamıyor.

   Hata iletileri, kullanıcılara yönetici müdahalesi olmadan çözmeye çalışabilmeleri için kılavuz sağlar.

## <a name="password-writeback-security"></a>Parola yazma güvenliği

Parola yazma son derece güvenli bir hizmettir. Bilgilerinizin korunmasını sağlamak için dört katmanlı bir güvenlik modeli aşağıdaki gibi etkinleştirilir:

* **Kiracıya özel servis-veri günü**
   * Hizmeti ayarladığınızda, Microsoft'un hiçbir zaman erişemeyeceği rasgele oluşturulmuş güçlü bir parolayla korunan kiracıya özel bir hizmet veri günü geçişi ayarlanır.
* **Kilitli, şifreleme güçlü, şifre şifreleme anahtarı**
   * Servis veri günü oluşturulduktan sonra, kabloüzerinden gelirken parolayı şifrelemek için kullanılan güçlü bir simetrik anahtar oluşturulur. Bu anahtar yalnızca şirketinizin gizli mağazasında yaşar, dizindeki diğer parolalar gibi yoğun bir şekilde kilitlenir ve denetlenir.
* **Endüstri standardı Taşıma Katmanı Güvenliği (TLS)**
   1. Bulutta bir parola sıfırlama veya değiştirme işlemi gerçekleştiğinde, düz metin parolası ortak anahtarınızla şifrelenir.
   1. Şifrelenmiş parola, servis veri veri günü rölenize Microsoft TLS/SSL sertifikalarını kullanarak şifreli bir kanal üzerinden gönderilen bir HTTPS iletisine yerleştirilir.
   1. İleti servis otobüsüne geldikten sonra, şirket içi temsilciniz uyanır ve daha önce oluşturulmuş olan güçlü parolayı kullanarak servis otobüsüne doğrular.
   1. Şirket içi aracı, şifrelenmiş iletiyi alır ve özel anahtarı kullanarak şifresini çözer.
   1. Şirket içi aracı, parolayı AD DS SetPassword API aracılığıyla ayarlamaya çalışır. Bu adım, Etkin Dizin şirket içi parola ilkenizin (karmaşıklık, yaş, geçmiş ve filtreler gibi) bulutta uygulanmasına olanak sağlar.
* **İleti son kullanma ilkeleri**
   * Şirket içi hizmetiniz sayıldığından ileti servis otobüsünde yse, zaman ları söker ve birkaç dakika sonra kaldırılır. İletinin zaman ası ve kaldırılması güvenliği daha da artırır.

### <a name="password-writeback-encryption-details"></a>Parola yazma şifreleme ayrıntıları

Kullanıcı parola sıfırlama gönderdikten sonra, sıfırlama isteği şirket ortamınıza gelmeden önce birkaç şifreleme adımından geçer. Bu şifreleme adımları maksimum hizmet güvenilirliği ve güvenliği sağlar. Bunlar aşağıdaki gibi tanımlanır:

1. **2048 bit RSA Tuşu ile parola şifrelemesi**: Bir kullanıcı şirket içinde yazılacak bir parola gönderdikten sonra, gönderilen parolanın kendisi 2048 bit RSA tuşu ile şifrelenir.
1. **AES-GCM ile paket düzeyinde şifreleme**: Paketin tamamı, parola ve gerekli meta veriler AES-GCM kullanılarak şifrelenir. Bu şifreleme, altta yatan ServiceBus kanalına doğrudan erişimi olan herkesin içeriği görüntülemesini veya buladeğiştirmesini önler.
1. **Tüm iletişim TLS/SSL üzerinden gerçekleşir**: ServiceBus ile tüm iletişim bir SSL/TLS kanalında gerçekleşir. Bu şifreleme, içeriği yetkisiz üçüncü taraflardan güvence altına alar.
1. **Her altı ayda bir otomatik anahtar devri**: Maksimum hizmet güvenliği ve güvenliğini sağlamak için tüm tuşlar her altı ayda bir devrilir veya parola geri yazılması devre dışı bırakıldığında ve Azure AD Connect'te yeniden etkinleştirildiğinde.

### <a name="password-writeback-bandwidth-usage"></a>Parola yazma bant genişliği kullanımı

Parola yazma, istekleri yalnızca aşağıdaki koşullar altında şirket içi temsilciye geri gönderen düşük bant genişliğine sahip bir hizmettir:

* Özellik Azure AD Connect aracılığıyla etkinleştirildiğinde veya devre dışı bırakıldığında iki ileti gönderilir.
* Hizmet çalışırken hizmet sinyali olarak her beş dakikada bir bir ileti gönderilir.
* Her yeni parola gönderildiğinde iki ileti gönderilir:
   * İlk ileti, işlemi gerçekleştirmek için bir istektir.
   * İkinci ileti işlemin sonucunu içerir ve aşağıdaki durumlarda gönderilir:
      * Kullanıcı self servis parola sıfırlama sırasında her yeni parola gönderildiğinde.
      * Kullanıcı parolası değiştirme işlemi sırasında her yeni parola gönderildiğinde.
      * Yönetici tarafından başlatılan kullanıcı parolası sıfırlama sırasında her yeni parola gönderildiğinde (yalnızca Azure yönetici portallarından).

#### <a name="message-size-and-bandwidth-considerations"></a>İleti boyutu ve bant genişliği yle ilgili hususlar

Daha önce açıklanan iletinin her birinin boyutu genellikle 1 KB'nin altındadır. Hatta aşırı yükler altında, şifre yazma hizmeti kendisi bant genişliği saniyede birkaç kilobit tüketiyor. Her ileti gerçek zamanlı olarak gönderildiğinden, yalnızca bir parola güncelleştirme işlemi gerektiğinde ve ileti boyutu çok küçük olduğundan, geri yazma özelliğinin bant genişliği kullanımı ölçülebilir bir etkiye sahip olamayacak kadar küçüktür.

## <a name="supported-writeback-operations"></a>Desteklenen geri yazma işlemleri

Parolalar aşağıdaki tüm durumlarda geri yazılır:

* **Desteklenen son kullanıcı işlemleri**
   * Herhangi bir son kullanıcı self-servis gönüllü değişiklik parola işlemi.
   * Herhangi bir son kullanıcı self servis kuvveti parola işlemi, örneğin, parola sona erme değiştirin.
   * [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com)kaynaklanan son kullanıcı self servis parola sıfırlama.

* **Desteklenen yönetici işlemleri**
   * Herhangi bir yönetici self-servis gönüllü değişiklik parola işlemi.
   * Herhangi bir yönetici self servis kuvveti parola işlemi, örneğin, parola sona erme değiştirin.
   * [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com)kaynaklanan herhangi bir yönetici self servis parola sıfırlama.
   * [Azure portalından](https://portal.azure.com)yönetici tarafından başlatılan tüm son kullanıcı parolası sıfırlanır.

## <a name="unsupported-writeback-operations"></a>Desteklenmeyen geri yazma işlemleri

Parolalar aşağıdaki durumların hiçbirinde geri yazılmamışdır:

* **Desteklenmeyen son kullanıcı işlemleri**
   * PowerShell sürüm 1, sürüm 2 veya Microsoft Graph API'yi kullanarak kendi parolasını sıfırlayan herhangi bir son kullanıcı.
* **Desteklenmeyen yönetici işlemleri**
   * PowerShell sürüm 1, sürüm 2 veya Microsoft Graph API'den yönetici tarafından başlatılan tüm son kullanıcı parolası sıfırlanır.
   * [Microsoft 365 yönetici merkezinden](https://admin.microsoft.com)yönetici tarafından başlatılan son kullanıcı parolası sıfırlanır.

> [!WARNING]
> Active Directory Users and Computers veya Active Directory Administrative Center gibi şirket içi AD DS yönetim araçlarında "Kullanıcı bir sonraki oturumaçmada parolayı değiştirmeli" onay kutusunun kullanımı Azure AD Connect'in önizleme özelliği olarak desteklenir. Daha fazla bilgi için bkz: [Azure AD Connect eşitlemesi ile parola karma senkronizasyonu uygula.](../hybrid/how-to-connect-password-hash-synchronization.md)

## <a name="next-steps"></a>Sonraki adımlar

SSPR writeback ile başlamak için aşağıdaki öğreticiyi tamamlayın:

> [!div class="nextstepaction"]
> [Öğretici: Self servis parola sıfırlama (SSPR) yazma yı etkinleştirme](tutorial-enable-writeback.md)
