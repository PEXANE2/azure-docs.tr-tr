---
title: Azure AD SSPR ile şirket içi parola yazma tümleştirmesi - Azure Active Directory
description: Şirket içi AD altyapısına bulut parolalarını geri yazma
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fe58ae95c8d9c6b93c7e92e093541af009781ce
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454440"
---
# <a name="what-is-password-writeback"></a>Parola yazma nedir?

Bulut tabanlı parola sıfırlama yardımcı programı olması harikadır, ancak çoğu şirketin kullanıcılarının bulunduğu şirket içi dizine sahip olması gerekir. Microsoft, geleneksel şirket içi Active Directory'nin (AD) buluttaki parola değişiklikleriyle eşit tutulmasını nasıl destekler? Parola yazma, [Buluttaki](../hybrid/whatis-hybrid-identity.md) parola değişikliklerinin varolan şirket içi dizine gerçek zamanlı olarak geri yazılmasını sağlayan Azure AD Connect ile etkinleştirilen bir özelliktir.

Parola yazma, aşağıdakileri kullanan ortamlarda desteklenir:

* [Active Directory Federasyon Hizmetleri](../hybrid/how-to-connect-fed-management.md)
* [Parola karması eşitleme](../hybrid/how-to-connect-password-hash-synchronization.md)
* [Doğrudan kimlik doğrulama](../hybrid/how-to-connect-pta.md)

> [!WARNING]
> [Azure Erişim Denetimi hizmeti (ACS) 7 Kasım 2018'de kullanımdan kaldırılınca,](../azuread-dev/active-directory-acs-migration.md)Azure AD Connect 1.0.8641.0 ve üzeri sürümlerini kullanan müşteriler için parola geri ödeme sayılmayı durdurur. Azure AD Connect sürümleri 1.0.8641.0 ve daha eski sürümleri, bu işlevsellik için ACS'ye bağlı oldukları için artık parola yazma izin vermez.
>
> Hizmette bir aksamayı önlemek için Azure AD Connect'in önceki sürümünden daha yeni bir sürüme yükseltin, [Azure AD Connect: Önceki sürümden en son sürümüne yükseltme makalesine](../hybrid/how-to-upgrade-previous-version.md) bakın
>

Parola yazma sağlar:

* **Şirket içi Active Directory parola ilkelerinin uygulanması**: Bir kullanıcı parolasını sıfırladığında, bu dizine işlemeden önce şirket içi Active Directory politikanızı karşıladığından emin olmak için denetlenir. Bu inceleme, yerel Active Directory'de tanımladığınız geçmiş, karmaşıklık, yaş, parola filtreleri ve diğer parola kısıtlamalarını denetlemeyi içerir.
* **Sıfır gecikmegeribildirim**: Şifre yazma senkron bir işlemdir. Parolaları ilkeyi karşılamadıysa veya herhangi bir nedenle sıfırlanamazsa veya değiştirilemediyse kullanıcılarınız derhal bildirilir.
* **Erişim panelinden ve Office 365'ten parola değişikliklerini destekler**: Federe veya parola karma senkronize edilmiş kullanıcılar süresi dolmuş veya süresi dolmamış parolalarını değiştirmeye geldiklerinde, bu parolalar yerel Active Directory ortamınıza geri yazılır.
* **Bir yönetici bunları Azure portalından sıfırladığında parola yazmayı destekler**: Bir yönetici [Azure portalında](https://portal.azure.com)bir kullanıcının parolasını sıfırladığında , bu kullanıcı federe veya parola karma senkronize edilirse, parola şirket içinde geri yazılır. Bu işlev şu anda Office yönetici portalında desteklenmez.
* **Gelen güvenlik duvarı kuralları gerektirmez**: Parola yazma, temel iletişim kanalı olarak Azure Hizmet Veri Servisi rölesi kullanır. Tüm iletişim bağlantı noktası 443 üzerinden giden.

> [!NOTE]
> Şirket içi AD'de korumalı gruplar içinde bulunan yönetici hesapları parola yazma ile kullanılabilir. Yöneticiler bulutta parolalarını değiştirebilir, ancak unutulan bir parolayı sıfırlamak için parola sıfırlamayı kullanamaz. Korumalı gruplar hakkında daha fazla bilgi [için, Etkin Dizini'ndeki Korumalı hesaplar ve gruplara](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-c--protected-accounts-and-groups-in-active-directory)bakın.

## <a name="licensing-requirements-for-password-writeback"></a>Parola yazma için lisans gereksinimleri

**Şirket içi geri yazma ile Self Servis Parola Sıfırlama/Değiştirme/Kilidini Açma Azure AD'nin bir premium özelliğidir.** Lisanslama hakkında daha fazla bilgi için [Azure Etkin Dizin fiyatlandırma sitesine](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

Parola yazma geri kullanmak için, kiracınızda aşağıdaki lisanslardan birine sahip olmalısınız:

* Azure AD Premium P1
* Azure AD Premium P2
* Kurumsal Mobilite + Güvenlik E3 veya A3
* Kurumsal Mobilite + Güvenlik E5 veya A5
* Microsoft 365 E3 veya A3
* Microsoft 365 E5 veya A5
* Microsoft 365 F1
* Microsoft 365 İş

> [!WARNING]
> Bağımsız Office 365 lisans lama planları *"Şirket içi yazma yla Self Servis Parola Sıfırlama/Değiştirme/Açma"yı desteklemez* ve bu işlevselliğin çalışması için önceki planlardan birine sahip olduğunuzu gerektirir.

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
1. Hizmet daha sonra bulut bağlantı özelliğini kullanarak kullanıcıyı arar. Bu aramanın başarılı olması için:

   * Kullanıcı nesnesi Active Directory bağlayıcı alanında bulunmalıdır.
   * Kullanıcı nesnesi ilgili metaverse (MV) nesneye bağlı olmalıdır.
   * Kullanıcı nesnesinin ilgili Azure Etkin Dizin bağlayıcısı nesnesine bağlı olması gerekir.
   * Active Directory bağlayıcısı nesnesinden MV'ye gelen bağlantının `Microsoft.InfromADUserAccountEnabled.xxx` bağlantıda eşitleme kuralı olması gerekir.
   
   Arama buluttan geldiğinde, eşitleme altyapısı Azure Active Directory bağlayıcısı uzay nesnesini aramak için **bulutÇapa** özniteliğini kullanır. Daha sonra MV nesnesine olan bağlantıyı izler ve sonra Etkin Dizin nesnesine olan bağlantıyı izler. Aynı kullanıcı için birden çok Active Directory nesnesi (çok orman) olabileceğinden, eşitleme altyapısı doğru olanı seçmek için `Microsoft.InfromADUserAccountEnabled.xxx` bağlantıya güvenir.

1. Kullanıcı hesabı bulunduktan sonra, parolayı doğrudan uygun Active Directory ormanında sıfırlama girişiminde bulunulr.
1. Parola kümesi işlemi başarılı olursa, kullanıcıya parolasının değiştirildiğini niçin değiştirdiği söylenir.
   > [!NOTE]
   > Kullanıcının parola karma sıcağa parola karma lıleştirme kullanılarak Azure AD ile senkronize edilirse, şirket içi parola ilkesinin bulut parola ilkesinden daha zayıf olma olasılığı vardır. Bu durumda, şirket içi ilke uygulanır. Bu ilke, tek oturum açma sağlamak için parola karma eşitleme veya federasyon kullanıp kullanmadığınızönemli olursa olsun, şirket içi politikanızın bulutta uygulanmasını sağlar.

1. Parola kümesi işlemi başarısız olursa, bir hata kullanıcıyı yeniden denemeye sorar. İşlem başarısız olabilir, çünkü:
    * Servis iniyordu.
    * Seçtikleri parola kuruluşun ilkelerini karşılamadı.
    * Yerel Active Directory'de kullanıcı yı bulamıyor.

      Hata iletileri, kullanıcılara yönetici müdahalesi olmadan çözmeye çalışabilmeleri için kılavuz sağlar.

## <a name="password-writeback-security"></a>Parola yazma güvenliği

Parola yazma son derece güvenli bir hizmettir. Bilgilerinizin korunduğundan emin olmak için, aşağıdaki gibi dört katmanlı bir güvenlik modeli etkinleştirilir:

* **Kiracıya özel servis-veri günü**
   * Hizmeti ayarladığınızda, Microsoft'un hiçbir zaman erişemeyeceği rasgele oluşturulmuş güçlü bir parolayla korunan kiracıya özel bir hizmet veri günü geçişi ayarlanır.
* **Kilitli, şifreleme güçlü, şifre şifreleme anahtarı**
   * Servis veri günü oluşturulduktan sonra, kablonun üzerinden gelirken parolayı şifrelemek için kullanılan güçlü bir simetrik anahtar oluşturulur. Bu anahtar yalnızca şirketinizin gizli mağazasında yaşar, dizindeki diğer parolalar gibi yoğun bir şekilde kilitlenir ve denetlenir.
* **Endüstri standardı Taşıma Katmanı Güvenliği (TLS)**
   1. Bulutta bir parola sıfırlama veya değiştirme işlemi gerçekleştiğinde, düz metin parolası ortak anahtarınızla şifrelenir.
   1. Şifrelenmiş parola, servis veri veri günü rölenize Microsoft TLS/SSL sertifikaları kullanılarak şifreli bir kanal üzerinden gönderilen bir HTTPS iletisine yerleştirilir.
   1. İleti servis otobüsüne geldikten sonra, şirket içi temsilciniz uyanır ve daha önce oluşturulmuş olan güçlü parolayı kullanarak servis otobüsüne doğrular.
   1. Şirket içi aracı, şifrelenmiş iletiyi alır ve özel anahtarı kullanarak şifresini çözer.
   1. Şirket içi aracı, parolayı AD DS SetPassword API aracılığıyla ayarlamaya çalışır. Bu adım, Etkin Dizin şirket içi parola ilkenizin (karmaşıklık, yaş, geçmiş ve filtreler gibi) bulutta uygulanmasına olanak sağlar.
* **İleti son kullanma ilkeleri**
   * Şirket içi hizmetiniz sayıldığından ileti servis otobüsünde yse, zaman ları söker ve birkaç dakika sonra kaldırılır. İletinin zaman ası ve kaldırılması güvenliği daha da artırır.

### <a name="password-writeback-encryption-details"></a>Parola yazma şifreleme ayrıntıları

Kullanıcı parola sıfırlama gönderdikten sonra, sıfırlama isteği şirket ortamınıza gelmeden önce birkaç şifreleme adımından geçer. Bu şifreleme adımları maksimum hizmet güvenilirliği ve güvenliği sağlar. Bunlar aşağıdaki gibi tanımlanır:

* **Adım 1: 2048 bit RSA Tuşu ile parola şifrelemesi**: Bir kullanıcı şirket içinde yazılacak bir parola gönderdikten sonra, gönderilen parolanın kendisi 2048 bit RSA tuşu ile şifrelenir.
* **Adım 2: AES-GCM ile paket düzeyinde şifreleme**: Paketin tamamı, parola ve gerekli meta veriler AES-GCM kullanılarak şifrelenir. Bu şifreleme, altta yatan ServiceBus kanalına doğrudan erişimi olan herkesin içeriği görüntülemesini veya buladeğiştirmesini önler.
* **Adım 3: Tüm iletişim TLS/SSL üzerinden gerçekleşir**: ServiceBus ile tüm iletişim bir SSL/TLS kanalında gerçekleşir. Bu şifreleme, içeriği yetkisiz üçüncü taraflardan güvence altına alar.
* **Her altı ayda bir otomatik anahtar devri**: Maksimum hizmet güvenliği ve güvenliğini sağlamak için tüm tuşlar her altı ayda bir devrilir veya parola geri yazılması devre dışı bırakıldığında ve Azure AD Connect'te yeniden etkinleştirildiğinde.

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
   * Herhangi bir son kullanıcı self-servis gönüllü değişiklik şifre işlemi
   * Herhangi bir son kullanıcı self servis kuvvet ibdeğiştirmesi parola işlemi, örneğin, parola sona ermesi
   * [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com) kaynaklanan son kullanıcı self servis parola sıfırlama
* **Desteklenen yönetici işlemleri**
   * Herhangi bir yönetici self-servis gönüllü değişiklik parola işlemi
   * Herhangi bir yönetici self servis kuvvet ibdeğiştirme parola işlemi, örneğin, parola sona ermesi
   * [Parola sıfırlama portalından](https://passwordreset.microsoftonline.com) kaynaklanan herhangi bir yönetici self servis parola sıfırlama
   * [Azure portalından](https://portal.azure.com) yönetici tarafından başlatılan tüm son kullanıcı parolası sıfırlama

## <a name="unsupported-writeback-operations"></a>Desteklenmeyen geri yazma işlemleri

Parolalar aşağıdaki durumların hiçbirinde geri *yazılmamışdır:*

* **Desteklenmeyen son kullanıcı işlemleri**
   * PowerShell sürüm 1, sürüm 2 veya Microsoft Graph API'yi kullanarak kendi parolasını sıfırlayan son kullanıcı
* **Desteklenmeyen yönetici işlemleri**
   * PowerShell sürüm 1, sürüm 2 veya Microsoft Graph API'den yönetici tarafından başlatılan tüm son kullanıcı parolası sıfırlama
   * [Microsoft 365 yönetici merkezinden](https://admin.microsoft.com) yönetici tarafından başlatılan tüm son kullanıcı parolası sıfırlama

> [!WARNING]
> Active Directory Users and Computers veya Active Directory Administrative Center gibi şirket içi Active Directory yönetim araçlarında "Kullanıcı bir sonraki oturumaçmada parolayı değiştirmeli" onay kutusunun kullanımı Azure AD Connect'in önizleme özelliği olarak desteklenir. Daha fazla bilgi için makaleye bakın, [Azure AD Connect eşitlemeile parola karma senkronizasyonu uygulayın.](../hybrid/how-to-connect-password-hash-synchronization.md)

## <a name="next-steps"></a>Sonraki adımlar

Öğretici'yi kullanarak parola yazmayı etkinleştirme: [Parola yazmayı etkinleştirme](tutorial-enable-writeback.md)
