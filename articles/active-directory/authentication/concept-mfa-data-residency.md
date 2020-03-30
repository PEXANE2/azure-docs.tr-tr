---
title: Azure Çok Faktörlü Kimlik Doğrulama veri ihtisası
description: Azure Çok Faktörlü Kimlik Doğrulama'nın siz ve kullanıcılarınız hakkında hangi kişisel ve kurumsal verileri saklar ve hangi verilerin menşe ülke içinde kaldığını öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75480144"
---
# <a name="data-residency-and-customer-data-for-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama için veri ihtisas ve müşteri verileri

Müşteri verileri Azure AD tarafından kuruluşunuzun Office 365 ve Azure gibi bir Microsoft Çevrimiçi hizmetine abone olurken sağladığı adrese dayalı coğrafi bir konumda depolanır. Müşteri verilerinizin nerede depolandığı hakkında bilgi almak [için, verilerinizin nerede bulunduğu?](https://www.microsoft.com/trustcenter/privacy/where-your-data-is-located) bölümünü kullanabilirsiniz.

Bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulama ve Azure Çok Faktörlü Kimlik Doğrulama Sunucusu işlemi ve bir miktar kişisel veri ve kuruluş verilerini depolama. Bu makalede, verilerin nerede ve nereler depolandığı özetlenir.

Aşağıdaki Çok Faktörlü Kimlik Doğrulama etkinlikleri şu anda belirtilmedikçe ABD veri merkezlerinden kaynaklanmaktadır:

* Telefon aramaları veya SMS kullanarak iki faktörlü kimlik doğrulama genellikle ABD veri merkezlerinden kaynaklanır ve küresel sağlayıcılar tarafından yönlendirilir.
    * Avrupa veya Avustralya gibi diğer bölgelerden gelen genel amaçlı kullanıcı kimlik doğrulama istekleri şu anda o bölgedeki veri merkezleri tarafından işlenir. Self servis parola sıfırlama, Azure B2C olayları veya NPS Uzantıveya AD FS bağdaştırıcısı kullanan karma senaryolar gibi diğer olayların tümü şu anda ABD veri merkezleri tarafından işlenmektedir.
* Microsoft Authenticator uygulamasını kullanarak anında iletme bildirimleri ABD veri merkezlerinden kaynaklanır. Buna ek olarak, aygıt satıcısına özel hizmetler de farklı bölgelerden devreye girebilir.
* OATH kodları genellikle şu anda ABD'de doğrulanır.
    * Yine, Avrupa veya Avustralya gibi diğer bölgelerden kaynaklanan genel amaçlı kullanıcı kimlik doğrulama olayları, o bölgedeki veri merkezleri tarafından işlenir. Ek olaylar şu anda ABD veri merkezleri tarafından işlenir.

## <a name="personal-data-stored-by-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulama tarafından depolanan kişisel veriler

Kişisel veriler, belirli bir kişiyle ilişkili kullanıcı düzeyinde bilgilerdir. Aşağıdaki veri depoları kişisel bilgiler içerir:

* Engellenen kullanıcılar
* Atlalı kullanıcılar
* Microsoft Authenticator aygıt belirteç değişikliği istekleri
* Çok Faktörlü Kimlik Doğrulama faaliyet raporları
* Microsoft Authenticator etkinleştirmeleri

Bu bilgiler 90 gün boyunca saklanır.

Azure Çok Faktörlü Kimlik Doğrulama, kullanıcı adı, telefon numarası veya IP adresi gibi kişisel verileri kaydetmez, ancak kullanıcılara yönelik Çok Faktörlü Kimlik Doğrulama girişimlerini tanımlayan bir *UserObjectId* vardır. Günlük verileri 30 gün boyunca saklanır.

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

Azure B2C kimlik doğrulaması, NPS Uzantı ve Windows Server 2016 veya 2019 AD FS Bağdaştırıcısı hariç olmak üzere Azure genel bulutları için aşağıdaki kişisel veriler depolanır:

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Çok Faktörlü Kimlik Doğrulama günlüklerinde     |
| Tek yönlü SMS                          | Çok Faktörlü Kimlik Doğrulama günlüklerinde     |
| Sesli arama                           | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu<br />Sahtekarlık rapor edilirse engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu<br />Sahtekarlık rapor edilirse engellenen kullanıcılar<br />Microsoft Authenticator aygıt belirteci değiştiğinde istekleri değiştirme |

> [!NOTE]
> Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu, kimlik doğrulama isteğini işleyen bölgeden bağımsız olarak tüm bulutlar için ABD'de depolanır. Microsoft Azure Almanya, Microsoft Azure Tarafından Işletilen 21Vianet ve Microsoft Public Cloud'un genel bulut bölgesi veri depolarından ayrı kendi bağımsız veri depoları vardır, ancak bu veriler her zaman ABD'de depolanır.

Microsoft Azure Hükümeti, Microsoft Azure Almanya, Microsoft Azure Tarafından Işletilen 21Vianet, Azure B2C kimlik doğrulaması, NPS Uzantı ve Windows Server 2016 veya 2019 AD FS Bağdaştırıcısı için aşağıdaki kişisel veriler depolanır:

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu |
| Tek yönlü SMS                          | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu |
| Sesli arama                           | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu<br />Sahtekarlık rapor edilirse engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu<br />Sahtekarlık rapor edilirse engellenen kullanıcılar<br />Microsoft Authenticator aygıt belirteci değiştiğinde istekleri değiştirme |

### <a name="multi-factor-authentication-server"></a>Multi-Factor Authentication Sunucusu

Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nu dağıtıp çalıştırıyorsanız, aşağıdaki kişisel veriler depolanır:

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık Çok Faktörlü Kimlik Doğrulama Sunucusu sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce Çok Faktörlü Kimlik Doğrulama Sunucusu'nu etkinleştirmiş olan mevcut müşteriler, en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

| Olay türü                           | Veri deposu türü |
|--------------------------------------|-----------------|
| OATH belirteci                           | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu |
| Tek yönlü SMS                          | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu |
| Sesli arama                           | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu<br />Sahtekarlık rapor edilirse engellenen kullanıcılar |
| Microsoft Authenticator bildirimi | Çok Faktörlü Kimlik Doğrulama günlüklerinde<br />Çok Faktörlü Kimlik Doğrulama faaliyet raporu veri deposu<br />Sahtekarlık rapor edilirse engellenen kullanıcılar<br />Microsoft Authenticator aygıt belirteci değiştiğinde istekleri değiştirme |

## <a name="organizational-data-stored-by-azure-multi-factor-authentication"></a>Azure Çok Faktörlü Kimlik Doğrulaması tarafından depolanan kuruluş verileri

Kuruluş verileri, yapılandırma yı veya ortam kurulumlarını ortaya çıkarabilecek kiracı düzeyindebilgidir. Aşağıdaki Azure portalı Çok Faktörlü Kimlik Doğrulama sayfalarındaki kiracı ayarları, gelen telefon kimlik doğrulama istekleri için kilitleme eşikleri veya arayan kimliği bilgileri gibi kuruluş verilerini depolayabilir:

* Hesap kilitleme
* Sahtekarlık uyarısı
* Bildirimler
* Telefon görüşmesi ayarları

Azure Çok Faktörlü Kimlik Doğrulama Sunucusu için aşağıdaki Azure portal sayfaları kuruluş verileri içerebilir:

* Sunucu ayarlar
* Tek seferlik baypas
* Önbelleğe alma kuralları
* Çok Faktörlü Kimlik Doğrulama Sunucusu durumu

## <a name="log-data-location"></a>Günlük veri konumu

Günlük bilgilerinin depolandığı yer, hangi bölgede işlendiğine bağlıdır. Çoğu coğrafyanın yerel Azure Çok Faktörlü Kimlik Doğrulama yetenekleri vardır, bu nedenle günlük verileri Çok Faktörlü Kimlik Doğrulama isteğini işleyen aynı bölgede depolanır. Yerel Azure Çok Faktörlü Kimlik Doğrulama desteği olmayan coğrafyalarda, bunlara ABD veya Avrupa coğrafyaları tarafından hizmet verilir ve günlük verileri Çok Faktörlü Kimlik Doğrulama isteğini işleyen aynı bölgede depolanır.

Bazı temel kimlik doğrulama günlüğü verileri yalnızca ABD'de depolanır. Microsoft Azure Almanya ve Microsoft Azure Tarafından Çalıştırılan 21Vianet her zaman kendi bulutlarında depolanır. Microsoft Government Cloud günlük verileri her zaman ABD'de depolanır.

## <a name="next-steps"></a>Sonraki adımlar

Bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulama ve Azure Çok Faktörlü Kimlik Doğrulama Sunucusu tarafından hangi kullanıcı bilgilerinin toplandığı hakkında daha fazla bilgi için Azure [Çok Faktörlü Kimlik Doğrulama kullanıcı veri koleksiyonuna](howto-mfa-reporting-datacollection.md)bakın.
