---
title: Azure AD çok faktörlü kimlik doğrulaması verileri yerleşimi
description: Kişisel ve kurumsal verileri Azure AD çok faktörlü kimlik doğrulamasının ne olduğunu, sizin ve kullanıcılarınız hakkındaki verileri ve kaynak ülke/bölge dahilinde hangi verilerin kaldığını öğrenin.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: justinha
author: justinha
manager: daveba
ms.reviewer: inbarc
ms.collection: M365-identity-device-management
ms.openlocfilehash: 788a666e8ec509bbd29a8dbd503a60b3dddefd6b
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98208361"
---
# <a name="data-residency-and-customer-data-for-azure-ad-multifactor-authentication"></a>Azure AD çok faktörlü kimlik doğrulaması için veri yerleşimi ve müşteri verileri

Müşteri verileri, Azure AD tarafından, Microsoft 365 ve Azure gibi bir Microsoft Online hizmetine abone olurken kuruluşunuzun verdiği adrese bağlı olarak coğrafi bir konumda depolanır. Müşteri verilerinizin depolandığı hakkında daha fazla bilgi için, Microsoft Güven Merkezi ' nin [veri konumunu nerede bulabilirsiniz?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünde kullanabilirsiniz.

Bulut tabanlı Azure AD çok faktörlü kimlik doğrulaması ve Azure AD çok faktörlü kimlik doğrulama sunucusu işleme ve bazı kişisel veri ve kurumsal veri depolama. Bu makalede, verilerin depolandığı ve nerede saklandığı özetlenmektedir.

Azure AD çok faktörlü kimlik doğrulama hizmeti ABD, Avrupa ve Asya Pasifik veri merkezlerine sahiptir. Aşağıdaki etkinlikler, belirtilen durumlar dışında bölgesel veri merkezlerinden kaynaklanmalıdır:

* Telefon çağrılarını kullanan çok faktörlü kimlik doğrulaması ABD veri merkezlerinden kaynaklanarak genel sağlayıcılar tarafından yönlendirilir.
* Avrupa veya Avustralya gibi diğer bölgelerden gelen genel amaçlı kullanıcı kimlik doğrulama istekleri şu anda kullanıcının konumuna göre işlenir.
* Microsoft Authenticator uygulamasını kullanarak anında iletme bildirimleri, kullanıcının konumuna bağlı olarak bölgesel veri merkezlerinde işlenir.
    * Apple Anında Iletme bildirimleri gibi cihaz satıcısına özgü hizmetler, kullanıcının konumunun dışında olabilir.

## <a name="personal-data-stored-by-azure-ad-multifactor-authentication"></a>Azure AD çok faktörlü kimlik doğrulaması tarafından depolanan kişisel veriler

Kişisel veriler, belirli bir kişiyle ilişkili kullanıcı düzeyi bilgiler. Aşağıdaki veri depoları kişisel bilgiler içerir:

* Engellenen kullanıcılar
* Atlanan kullanıcılar
* Microsoft Authenticator cihaz belirteci değişiklik istekleri
* Çok faktörlü kimlik doğrulaması etkinlik raporları
* Microsoft Authenticator etkinleştirmeleri

Bu bilgiler 90 gün boyunca tutulur.

Azure AD çok faktörlü kimlik doğrulaması, Kullanıcı adı, telefon numarası veya IP adresi gibi kişisel verileri günlüğe kaydetmez, ancak kullanıcılara çok faktörlü kimlik doğrulaması girişimlerini tanımlayan bir *Userobjectıd* vardır. Günlük verileri 30 gün boyunca depolanır.

### <a name="azure-ad-multifactor-authentication"></a>Azure AD çok faktörlü kimlik doğrulaması

Azure B2C kimlik doğrulaması, NPS uzantısı ve Windows Server 2016 ya da 2019 AD FS bağdaştırıcısı dışında Azure genel bulutları için aşağıdaki kişisel veriler depolanır:

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Çok faktörlü kimlik doğrulama günlüklerinde     |
| Tek yönlü SMS                          | Çok faktörlü kimlik doğrulama günlüklerinde     |
| Sesli arama                           | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar<br />Microsoft Authenticator cihaz belirteci değiştiğinde istekleri değiştirme |

Microsoft Azure Kamu, Microsoft Azure Almanya, 21Vianet tarafından çalıştırılan Microsoft Azure, Azure B2C kimlik doğrulaması, NPS uzantısı ve Windows Server 2016 ya da 2019 AD FS bağdaştırıcısı için aşağıdaki kişisel veriler depolanır:

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu |
| Tek yönlü SMS                          | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu |
| Sesli arama                           | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar<br />Microsoft Authenticator cihaz belirteci değiştiğinde istekleri değiştirme |

### <a name="multifactor-authentication-server"></a>Çok faktörlü kimlik doğrulama sunucusu

Azure AD çok faktörlü kimlik doğrulama sunucusunu dağıtıp çalıştırırsanız, aşağıdaki kişisel veriler depolanır:

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle, Microsoft artık Yeni dağıtımlar için çok faktörlü kimlik doğrulama sunucusu sunmaz. Kullanıcılardan çok faktörlü kimlik doğrulaması gerektirmek isteyen yeni müşterilerin bulut tabanlı Azure AD çok faktörlü kimlik doğrulamasını kullanması gerekir. 1 Temmuz 'dan önce çok faktörlü Authentication Server 'ı etkinleştiren mevcut müşteriler, en son sürümü ve gelecekteki güncelleştirmeleri indirebilir ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilir.

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu |
| Tek yönlü SMS                          | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu |
| Sesli arama                           | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Çok faktörlü kimlik doğrulama günlüklerinde<br />Çok faktörlü kimlik doğrulaması etkinlik raporu veri deposu<br />Sahtekarlık bildiriliyorsa engellenen kullanıcılar<br />Microsoft Authenticator cihaz belirteci değiştiğinde istekleri değiştirme |

## <a name="organizational-data-stored-by-azure-ad-multifactor-authentication"></a>Azure AD çok faktörlü kimlik doğrulaması tarafından depolanan Kurumsal veriler

Kuruluş verileri, yapılandırma veya ortam kurulumunu açığa çıkaran kiracı düzeyindeki bir bilgi. Aşağıdaki Azure portal çok faktörlü kimlik doğrulama sayfalarındaki kiracı ayarları, gelen telefon kimlik doğrulama istekleri için kilitleme eşikleri veya arayan KIMLIĞI bilgileri gibi kuruluş verilerini saklayabilir:

* Hesap kilitleme
* Sahtekarlık uyarısı
* Bildirimler
* Telefon araması ayarları

Azure AD çok faktörlü kimlik doğrulama sunucusu için aşağıdaki Azure portal sayfaları kurumsal verileri içerebilir:

* Sunucu ayarlar
* Bir kerelik geçiş
* Önbelleğe alma kuralları
* Çok faktörlü kimlik doğrulama sunucusu durumu

## <a name="multifactor-authentication-logs-location"></a>Çok faktörlü kimlik doğrulama günlükleri konumu

Aşağıdaki tabloda, Genel bulutların hizmet günlüklerinin konumu gösterilmektedir.

| Genel bulut| Oturum açma günlükleri | Çok faktörlü kimlik doğrulaması etkinlik raporu        | Çok faktörlü kimlik doğrulama hizmeti günlükleri       |
|-------------|--------------|----------------------------------------|------------------------|
| ABD          | ABD           | ABD                                     | ABD                     |
| Avrupa      | Avrupa       | ABD                                     | Avrupa <sup>2</sup>    |
| Avustralya   | Avustralya    | ABD<sup>1</sup>                         | Avustralya <sup>2</sup> |

<sup>1</sup> OATH kodu günlükleri Avustralya 'da depolanır

<sup>2</sup> Sesli çağrılar çok faktörlü kimlik doğrulama hizmeti günlükleri ABD 'de depolanır

Aşağıdaki tabloda, bağımsız bulutları için hizmet günlüklerinin konumu gösterilmektedir.

| Bağımsız bulut                      | Oturum açma günlükleri                         | Çok faktörlü kimlik doğrulaması etkinlik raporu (kişisel verileri içerir)| Çok faktörlü kimlik doğrulama hizmeti günlükleri |
|--------------------------------------|--------------------------------------|-------------------------------|------------------|
| Microsoft Azure Almanya              | Almanya                              | ABD                            | ABD               |
| 21Vianet tarafından Işletilen Microsoft Azure | Çin                                | ABD                            | ABD               |
| Microsoft kamu bulutu           | ABD                                   | ABD                            | ABD               |

Çok faktörlü kimlik doğrulama etkinliği rapor verileri, Kullanıcı asıl adı (UPN) ve tüm telefon numarası gibi kişisel verileri içerir.

Çok faktörlü kimlik doğrulama hizmeti günlükleri hizmeti çalıştırmak için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Bulut tabanlı Azure AD çok faktörlü kimlik doğrulaması ve Azure AD çok faktörlü kimlik doğrulama sunucusu tarafından hangi kullanıcı bilgilerinin toplandığı hakkında daha fazla bilgi için bkz. [Azure AD çok faktörlü kimlik doğrulama kullanıcı verileri koleksiyonu](howto-mfa-reporting-datacollection.md).
