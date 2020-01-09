---
title: Azure Multi-Factor Authentication veri yerleşimi
description: Azure Multi-Factor Authentication kişisel ve kurumsal verileri, siz ve kullanıcılarınız hakkında, kaynak ülke dahilinde hangi verilerin kaldığını öğrenin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 12/16/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: sasubram
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c4556ece7faaacb8657a1012344f2263ee84214
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480144"
---
# <a name="data-residency-and-customer-data-for-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication veri yerleşimi ve müşteri verileri

Müşteri verileri, Azure AD tarafından, Office 365 ve Azure gibi bir Microsoft Online hizmetine abone olurken kuruluşunuzun verdiği adrese bağlı olarak coğrafi bir konumda depolanır. Müşteri verilerinizin depolandığı hakkında daha fazla bilgi için, Microsoft Güven Merkezi ' nin [veri konumunu nerede bulabilirsiniz?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünde kullanabilirsiniz.

Bulut tabanlı Azure Multi-Factor Authentication ve Azure Multi-Factor Authentication Sunucusu işleme ve bazı kişisel verileri ve kurumsal verileri depolama. Bu makalede, verilerin depolandığı ve nerede saklandığı özetlenmektedir.

Aşağıdaki Multi-Factor Authentication etkinlikleri Şu anda yukarıda belirtilenler dışında ABD veri merkezlerinden kaynaklanmakta:

* Telefon çağrılarını veya SMS 'yi kullanan iki öğeli kimlik doğrulama genellikle ABD veri merkezlerinden kaynaklanarak küresel sağlayıcılar tarafından yönlendirilir.
    * Avrupa veya Avustralya gibi diğer bölgelerden gelen genel amaçlı kullanıcı kimlik doğrulama istekleri şu anda ilgili bölgedeki veri merkezleri tarafından işlenir. Self servis parola sıfırlama, Azure B2C olayları ya da NPS uzantısı veya AD FS bağdaştırıcısı kullanan Karma senaryolar gibi diğer olaylar şu anda ABD veri merkezleri tarafından işlenir.
* Microsoft Authenticator uygulamasını kullanarak anında iletme bildirimleri ABD veri merkezlerinden kaynaklarından geliyor. Buna ek olarak, cihaz satıcısına özgü hizmetler de farklı bölgelerden oynatabilir.
* OATH kodları genellikle ABD 'de Şu anda onaylanır
    * Bu durumda, Avrupa veya Avustralya gibi diğer bölgelerde yer alan genel amaçlı kullanıcı kimlik doğrulama olayları da söz konusu bölgedeki veri merkezleri tarafından işlenir. Diğer olaylar şu anda ABD veri merkezleri tarafından işlenir.

## <a name="personal-data-stored-by-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication tarafından depolanan kişisel veriler

Kişisel veriler, belirli bir kişiyle ilişkili kullanıcı düzeyi bilgiler. Aşağıdaki veri depoları kişisel bilgiler içerir:

* Engellenen kullanıcılar
* Atlanan kullanıcılar
* Microsoft Authenticator cihaz belirteci değişiklik istekleri
* Etkinlik raporları Multi-Factor Authentication
* Microsoft Authenticator etkinleştirmeleri

Bu bilgiler 90 gün boyunca tutulur.

Azure Multi-Factor Authentication, Kullanıcı adı, telefon numarası veya IP adresi gibi kişisel verileri günlüğe kaydetmez, ancak kullanıcılara Multi-Factor Authentication girişimlerini tanımlayan bir *Userobjectıd* vardır. Günlük verileri 30 gün boyunca depolanır.

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

Azure B2C kimlik doğrulaması, NPS uzantısı ve Windows Server 2016 ya da 2019 AD FS bağdaştırıcısı dışında Azure genel bulutları için aşağıdaki kişisel veriler depolanır:

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Multi-Factor Authentication günlüklerde     |
| Tek yönlü SMS                          | Multi-Factor Authentication günlüklerde     |
| Sesli çağrı                           | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar<br />Microsoft Authenticator cihaz belirteci değiştiğinde istekleri değiştirme |

> [!NOTE]
> Multi-Factor Authentication etkinlik raporu veri deposu, kimlik doğrulama isteğini işleyen bölgeden bağımsız olarak tüm bulutlara yönelik Birleşik Devletler depolanır. Microsoft Azure Almanya, 21Vianet tarafından çalıştırılan Microsoft Azure ve Microsoft kamu bulutu, genel bulut bölgesi veri depolarından ayrı olarak kendi bağımsız veri depolarına sahiptir, ancak bu veriler her zaman Birleşik Devletler depolanır.

Microsoft Azure Kamu, Microsoft Azure Almanya, 21Vianet tarafından çalıştırılan Microsoft Azure, Azure B2C kimlik doğrulaması, NPS uzantısı ve Windows Server 2016 ya da 2019 AD FS bağdaştırıcısı için aşağıdaki kişisel veriler depolanır:

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu |
| Tek yönlü SMS                          | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu |
| Sesli çağrı                           | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar<br />Microsoft Authenticator cihaz belirteci değiştiğinde istekleri değiştirme |

### <a name="multi-factor-authentication-server"></a>Multi-Factor Authentication Sunucusu

Azure Multi-Factor Authentication Sunucusu dağıtıp çalıştırırsanız, aşağıdaki kişisel veriler depolanır:

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle, Microsoft artık Yeni dağıtımlar için Multi-Factor Authentication Sunucusu sunmayacaktır. Kullanıcılardan Multi-Factor Authentication istemek isteyen yeni müşteriler bulut tabanlı Azure Multi-Factor Authentication kullanmalıdır. 1 Temmuz 'dan önce Multi-Factor Authentication Sunucusu etkinleştiren mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri indirebilir ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilir.

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu |
| Tek yönlü SMS                          | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu |
| Sesli çağrı                           | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Multi-Factor Authentication günlüklerde<br />Multi-Factor Authentication etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar<br />Microsoft Authenticator cihaz belirteci değiştiğinde istekleri değiştirme |

## <a name="organizational-data-stored-by-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication tarafından depolanan Kurumsal veriler

Kuruluş verileri, yapılandırma veya ortam kurulumunu açığa çıkaran kiracı düzeyindeki bir bilgi. Aşağıdaki Azure portal Multi-Factor Authentication sayfalardaki kiracı ayarları, gelen telefon kimlik doğrulama istekleri için kilitleme eşikleri veya arayan KIMLIĞI bilgileri gibi kuruluş verilerini saklayabilir:

* Hesap kilitleme
* Sahtekarlık uyarısı
* Bildirimler
* Telefon görüşmesi ayarları

Azure Multi-Factor Authentication Sunucusu için aşağıdaki Azure portal sayfaları kuruluş verilerini içerebilir:

* Sunucu ayarları
* Bir kerelik geçiş
* Önbelleğe alma kuralları
* Multi-Factor Authentication Sunucusu durumu

## <a name="log-data-location"></a>Günlük veri konumu

Günlük bilgilerinin depolandığı yer, içinde işlendiği bölgeye bağlıdır. Çoğu geographıes yerel Azure Multi-Factor Authentication özelliklerine sahiptir, bu nedenle günlük verileri Multi-Factor Authentication isteğini işleyen bölgede depolanır. Yerel Azure Multi-Factor Authentication desteği olmadan coğrafi olarak, Birleşik Devletler veya Avrupa coğrafi ve günlük verileri Multi-Factor Authentication isteğini işleyen bölgede depolanır.

Bazı çekirdek kimlik doğrulama günlüğü verileri yalnızca Birleşik Devletler depolanır. 21Vianet tarafından çalıştırılan Microsoft Azure Almanya ve Microsoft Azure her zaman ilgili bulutta depolanır. Microsoft kamu bulutu günlük verileri her zaman Birleşik Devletler depolanır.

## <a name="next-steps"></a>Sonraki adımlar

Bulut tabanlı Azure Multi-Factor Authentication ve Azure Multi-Factor Authentication Sunucusu tarafından hangi kullanıcı bilgilerinin toplandığı hakkında daha fazla bilgi için bkz. [Azure Multi-Factor Authentication Kullanıcı verileri koleksiyonu](howto-mfa-reporting-datacollection.md).
