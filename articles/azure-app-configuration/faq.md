---
title: Azure uygulama yapılandırma hakkında SSS
description: Azure Uygulama yapılandırması hakkında sık sorulan sorular
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 25187fd055f40e8b32d840ead2a9c54882446b88
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80348782"
---
# <a name="azure-app-configuration-faq"></a>Azure uygulama yapılandırma hakkında SSS

Bu makalede, Azure Uygulama yapılandırması hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>Uygulama yapılandırması Azure Key Vault nasıl farklıdır?

Uygulama yapılandırması, geliştiricilerin uygulama ayarlarını yönetmesine ve özellik kullanılabilirliğini denetlemesine yardımcı olur. Karmaşık yapılandırma verileriyle çalışan pek çok görevi basitleştirecek.

Uygulama yapılandırması şunları destekler:

- Hiyerarşik ad alanları
- Kapatma
- Kapsamlı sorgular
- Toplu iş alımı
- Özel yönetim işlemleri
- Özellik yönetimi Kullanıcı arabirimi

Uygulama yapılandırması Key Vault tamamlar ve iki uygulama dağıtımında yan yana kullanılmalıdır.

## <a name="should-i-store-secrets-in-app-configuration"></a>Gizli dizileri uygulama yapılandırmasında depolamam gerekir mi?

Uygulama yapılandırması sıkı güvenlik sağlar, ancak Key Vault uygulama gizli dizileri depolamak için de en iyi yerdir. Key Vault, donanım düzeyinde şifreleme, ayrıntılı erişim ilkeleri ve sertifika döndürme gibi yönetim işlemleri sağlar.

Key Vault depolanan gizli dizileri referans eden uygulama yapılandırma değerleri oluşturabilirsiniz. Daha fazla bilgi için bkz. [ASP.NET Core uygulamasında Key Vault başvurularını kullanma](./use-key-vault-references-dotnet-core.md).

## <a name="does-app-configuration-encrypt-my-data"></a>Uygulama yapılandırması verilerimi şifreliyor mu?

Evet. Uygulama yapılandırması, tuttuğu tüm anahtar değerlerini şifreler ve ağ iletişimini şifreler. Anahtar adları ve Etiketler, yapılandırma verilerini almak için dizinler olarak kullanılır ve şifrelenmez.

## <a name="how-is-app-configuration-different-from-azure-app-service-settings"></a>Uygulama yapılandırması Azure App Service ayarlarından farklı midir?

Azure App Service, her bir App Service örneği için uygulama ayarlarını tanımlamanızı sağlar. Bu ayarlar, uygulama koduna ortam değişkenleri olarak geçirilir. İsterseniz bir ayarı belirli bir dağıtım yuvası ile ilişkilendirebilirsiniz. Daha fazla bilgi için bkz. [uygulama ayarlarını yapılandırma](/azure/app-service/configure-common#configure-app-settings).

Buna karşılık, Azure Uygulama yapılandırması, birden çok uygulama arasında paylaşılabilen ayarları tanımlamanızı sağlar. Bu, App Service çalıştıran uygulamaları ve diğer platformları da içerir. Uygulama kodunuz, bu ayarlara .NET ve Java için yapılandırma sağlayıcıları aracılığıyla, Azure SDK aracılığıyla veya doğrudan REST API 'Leri aracılığıyla erişir.

Ayrıca, App Service ile uygulama yapılandırması arasında ayarları içeri ve dışarı aktarabilirsiniz. Bu özellik, mevcut App Service ayarlarını temel alarak yeni bir uygulama yapılandırma deposu ayarlamanıza olanak sağlar. Ayrıca, yapılandırmayı App Service ayarlarını kullanan mevcut bir uygulamayla paylaşabilirsiniz.

## <a name="are-there-any-size-limitations-on-keys-and-values-stored-in-app-configuration"></a>Uygulama yapılandırmasında saklanan anahtarlar ve değerler üzerinde herhangi bir boyut sınırlaması var mı?

Tek bir anahtar-değer öğesi için 10 KB sınırı vardır.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>Birden çok ortam için yapılandırma (test, hazırlama, üretim vb.) nasıl depolamalıyım?

Uygulama yapılandırmasına kimin bir mağaza düzeyinde erişebileceğini denetlersiniz. Farklı izinler gerektiren her ortam için ayrı bir depo kullanın. Bu yaklaşım, en iyi güvenlik yalıtımını sağlar.

Ortamlar arasında güvenlik yalıtımına ihtiyacınız yoksa, yapılandırma değerlerini ayırt etmek için etiketleri kullanabilirsiniz. [Farklı ortamlara yönelik farklı yapılandırmaların etkinleştirilmesi için etiketleri kullanın](./howto-labels-aspnet-core.md) , tüm bir örnek sağlar.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Uygulama yapılandırmasını kullanmanın önerilen yolları nelerdir?

Bkz. [en iyi uygulamalar](./howto-best-practices.md).

## <a name="how-much-does-app-configuration-cost"></a>Uygulama yapılandırma maliyeti ne kadar?

İki fiyatlandırma katmanı vardır:

- Ücretsiz katmanı
- Standart katman.

Standart katmanın sunumundan önce bir mağaza oluşturduysanız, genel kullanıma sunulmadan otomatik olarak ücretsiz katmana taşınır. Standart katmana yükseltmeyi seçebilir veya ücretsiz katmanda kalabilirler.

Standart katmandan bir depoyu ücretsiz katmana indirgeyemezsiniz. Ücretsiz katmanda yeni bir mağaza oluşturup bu depoya yapılandırma verilerini içeri aktarabilirsiniz.

## <a name="which-app-configuration-tier-should-i-use"></a>Hangi uygulama yapılandırma katmanını kullanmalıyım?

Her iki uygulama yapılandırma katmanı da yapılandırma ayarları, özellik bayrakları, Key Vault başvuruları, temel yönetim işlemleri, ölçümler ve günlükler dahil olmak üzere çekirdek işlevler sunar.

Katman seçme konusunda dikkate alınması gereken noktalar aşağıda verilmiştir.

- **Abonelik başına kaynaklar**: bir kaynak tek bir yapılandırma deposundan oluşur. Her abonelik, ücretsiz katmandaki bir yapılandırma deposuyla sınırlıdır. Abonelikler Standart katmanda sınırsız sayıda yapılandırma deposu olabilir.
- **Kaynak başına depolama**: Ücretsiz katmanda, her yapılandırma deposu 10 MB depolama alanı ile sınırlıdır. Standart katmanda, her yapılandırma deposu en fazla 1 GB depolama kullanabilir.
- **Anahtar geçmişi**: uygulama yapılandırması anahtarlar üzerinde yapılan tüm değişikliklerin geçmişini depolar. Ücretsiz katmanda bu geçmiş yedi gün boyunca depolanır. Standart katmanda bu geçmiş 30 gün boyunca depolanır.
- **Gün başına istek**: ücretsiz katman depoları, günde 1.000 istek ile sınırlıdır. Bir mağaza 1.000 istek ulaştığında, gece yarısı UTC tarihine kadar tüm istekler için 429 HTTP durum kodunu döndürür.

    Standart katman depoları için her gün ilk 200.000 istek günlük ücretine dahil edilir. Ek istekler fazla kullanım olarak faturalandırılır.

- **Hizmet düzeyi sözleşmesi**: Standart katmanda% 99,9 kullanılabilirlik SLA 'sı vardır. Ücretsiz katmanda bir SLA yoktur.
- **Güvenlik özellikleri**: her iki katmanda da Microsoft tarafından yönetilen anahtarlarla ŞIFRELEME, HMAC veya Azure ACTIVE DIRECTORY, RBAC desteği ve yönetilen kimlik aracılığıyla kimlik doğrulaması dahil temel güvenlik işlevleri bulunur. Standart katmanda özel bağlantı desteği ve müşteri tarafından yönetilen Anahtarlarla Şifreleme dahil daha gelişmiş güvenlik işlevleri sunulmaktadır.
- **Maliyet**: Standart katman mağazalarında günlük kullanım ücreti vardır. Günlük ayırmanın ötesinde istekler için de fazla kullanım ücreti de vardır. Ücretsiz katman deposu kullanma maliyeti yoktur.

## <a name="can-i-upgrade-a-store-from-the-free-tier-to-the-standard-tier-can-i-downgrade-a-store-from-the-standard-tier-to-the-free-tier"></a>Bir mağazayı ücretsiz katmandan Standart katmana yükseltebilir miyim? Standart katmandaki bir deponun ücretsiz katmana indirgenmesini sağlayabilir miyim?

Ücretsiz katmandan Standart katmana dilediğiniz zaman yükseltebilirsiniz.

Standart katmandan bir depoyu ücretsiz katmana indirgeyemezsiniz. Ücretsiz katmanda yeni bir mağaza oluşturup [Bu depoya yapılandırma verilerini içeri aktarabilirsiniz](howto-import-export-data.md).

## <a name="are-there-any-limits-on-the-number-of-requests-made-to-app-configuration"></a>Uygulama yapılandırmasına yapılan istek sayısına yönelik herhangi bir sınırlama var mı?

Ücretsiz katmandaki yapılandırma depoları, günde 1.000 istek ile sınırlıdır. Standart katmandaki yapılandırma depoları, istek oranı saat başına 20.000 isteği aştığında geçici azaltmasına karşılaşabilir.

Bir depo sınıra ulaştığında, zaman aralığı sona erene kadar yapılan tüm istekler için 429 HTTP durum kodunu döndürür. Yanıttaki `retry-after-ms` üst bilgi, isteği yeniden denemeden önce önerilen bir bekleme süresi (milisaniye cinsinden) verir.

Uygulamanız düzenli olarak HTTP durum kodu 429 yanıtlarını yaşıyorsa, yapılan istek sayısını azaltmak için yeniden tasarlamayı düşünün. Daha fazla bilgi için bkz. [uygulama yapılandırmasına yapılan Istekleri azaltma](./howto-best-practices.md#reduce-requests-made-to-app-configuration)

## <a name="my-application-receives-http-status-code-429-responses-why"></a>Uygulamam HTTP durum kodu 429 yanıtlarını alıyor. Neden?

Şu koşullarda bir HTTP durum kodu 429 yanıtı alacaksınız:

* Ücretsiz katmandaki bir mağaza için günlük istek sınırı aşılıyor.
* Standart katmandaki bir mağaza için yüksek istek oranı nedeniyle geçici azaltma.
* Aşırı bant genişliği kullanımı.
* Depolama teklifi aşıldığında bir anahtar oluşturulmaya veya değiştirmeye çalışılıyor.

İsteğin neden başarısız olduğuna ilişkin belirli bir nedenden dolayı 429 yanıtının gövdesini kontrol edin.

## <a name="how-can-i-receive-announcements-on-new-releases-and-other-information-related-to-app-configuration"></a>Yeni yayınlar ve uygulama yapılandırmasıyla ilgili diğer bilgiler hakkında duyurular nasıl alabilirim?

[GitHub duyuruları](https://github.com/Azure/AppConfiguration-Announcements)depolarımıza abone olun.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Bir sorunu nasıl bildirebilirim veya öneride bulunmak için ne yapabilirim?

Doğrudan [GitHub](https://github.com/Azure/AppConfiguration/issues)üzerinde bizimle iletişime ulaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Uygulama Yapılandırması hakkında](./overview.md)
