---
title: Azure Uygulama Yapılandırma SSS
description: Azure Uygulama Yapılandırması hakkında sık sorulan sorular
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/19/2020
ms.author: lcozzens
ms.openlocfilehash: 25187fd055f40e8b32d840ead2a9c54882446b88
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80348782"
---
# <a name="azure-app-configuration-faq"></a>Azure Uygulama Yapılandırma SSS

Bu makalede, Azure Uygulama Yapılandırması hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="how-is-app-configuration-different-from-azure-key-vault"></a>Uygulama Yapılandırması'nın Azure Key Vault'tan farkı nedir?

Uygulama Yapılandırması, geliştiricilerin uygulama ayarlarını yönetmesine ve özellik kullanılabilirliğini denetlemesine yardımcı olur. Karmaşık yapılandırma verileriyle çalışma görevlerinin çoğunu basitleştirmeyi amaçlamaktadır.

Uygulama Yapılandırması destekler:

- Hiyerarşik ad alanları
- Etiketleme
- Kapsamlı sorgular
- Toplu alma
- Özel yönetim operasyonları
- Özellik yönetimi kullanıcı arabirimi

Uygulama Yapılandırması Key Vault'u tamamlar ve bu iki uygulama dağıtımında yan yana kullanılmalıdır.

## <a name="should-i-store-secrets-in-app-configuration"></a>Sırları App Configuration'da saklamalı mıyım?

App Configuration sertleştirilmiş güvenlik sağlasa da, Key Vault uygulama sırlarını depolamak için hala en iyi yerdir. Key Vault donanım düzeyinde şifreleme, parçalı erişim ilkeleri ve sertifika döndürme gibi yönetim işlemleri sağlar.

Key Vault'ta saklanan sırlara başvuran Uygulama Yapılandırma değerleri oluşturabilirsiniz. Daha fazla bilgi için, [ASP.NET Core uygulamasında Anahtar Kasası başvurularını kullanın'](./use-key-vault-references-dotnet-core.md)a bakın.

## <a name="does-app-configuration-encrypt-my-data"></a>Uygulama Yapılandırması verilerimi şifreler mi?

Evet. App Configuration sahip olduğu tüm temel değerleri ve ağ iletişimini şifreler. Anahtar adları ve etiketler yapılandırma verilerini almak için dizin olarak kullanılır ve şifrelenmez.

## <a name="how-is-app-configuration-different-from-azure-app-service-settings"></a>Uygulama Yapılandırması'nın Azure Uygulama Hizmeti ayarlarından farkı nedir?

Azure Uygulama Hizmeti, her Uygulama Hizmeti örneği için uygulama ayarlarını tanımlamanıza olanak tanır. Bu ayarlar uygulama koduna ortam değişkeni olarak geçirilir. İsterseniz bir ayarı belirli bir dağıtım yuvasıyla ilişkilendirebilirsiniz. Daha fazla bilgi için uygulama [ayarlarını yapılandır'a](/azure/app-service/configure-common#configure-app-settings)bakın.

Buna karşılık, Azure Uygulama Yapılandırması, birden çok uygulama arasında paylaşılabilen ayarları tanımlamanıza olanak tanır. Buna, Uygulama Hizmeti'nde çalışan uygulamaların yanı sıra diğer platformlar da dahildir. Uygulama kodunuz bu ayarlara .NET ve Java yapılandırma sağlayıcıları, Azure SDK aracılığıyla veya doğrudan REST API'leri aracılığıyla erişir.

Ayrıca, Uygulama Hizmeti ve Uygulama Yapılandırması arasında ayarları içe aktarıp dışa aktarabilirsiniz. Bu özellik, mevcut Uygulama Hizmeti ayarlarını temel alan yeni bir Uygulama Yapılandırma mağazası kurmanızı sağlar. Yapılandırmayı, Uygulama Hizmeti ayarlarına dayanan mevcut bir uygulamayla da paylaşabilirsiniz.

## <a name="are-there-any-size-limitations-on-keys-and-values-stored-in-app-configuration"></a>Uygulama Yapılandırması'nda depolanan anahtarlar ve değerler üzerinde boyut sınırlaması var mı?

Tek bir anahtar değeri öğesi için 10 KB sınırı vardır.

## <a name="how-should-i-store-configurations-for-multiple-environments-test-staging-production-and-so-on"></a>Yapılandırmaları birden çok ortam (test, evreleme, üretim vb.) için nasıl depolamalıyım?

Uygulama Yapılandırması'na mağaza başına kimler erişebilir denetime siz siz. Farklı izinler gerektiren her ortam için ayrı bir mağaza kullanın. Bu yaklaşım en iyi güvenlik yalıtımı sağlar.

Ortamlar arasında güvenlik yalıtımına ihtiyacınız yoksa, yapılandırma değerleri arasında ayrım yapmak için etiketleri kullanabilirsiniz. [Farklı ortamlar için farklı yapılandırmaları etkinleştirmek için etiketleri kullanmak](./howto-labels-aspnet-core.md) tam bir örnek sağlar.

## <a name="what-are-the-recommended-ways-to-use-app-configuration"></a>Uygulama Yapılandırması'nı kullanmanın önerilen yolları nelerdir?

[En iyi uygulamalara](./howto-best-practices.md)bakın.

## <a name="how-much-does-app-configuration-cost"></a>Uygulama Yapılandırması ne kadartutar?

İki fiyatlandırma katmanı vardır:

- Ücretsiz katmanı
- Standart katman.

Standart katmanın piyasaya sürülmesinden önce bir mağaza oluşturduysanız, genel kullanılabilirlik nedeniyle otomatik olarak Free katmanına taşınır. Standart katmana yükseltmeyi seçebilir veya Serbest katmanda kalabilirsiniz.

Bir mağazayı Standart katmandan Serbest katmana indiremezsiniz. Serbest katmanda yeni bir mağaza oluşturabilir ve ardından yapılandırma verilerini bu mağazaya aktarabilirsiniz.

## <a name="which-app-configuration-tier-should-i-use"></a>Hangi Uygulama Yapılandırma katmanını kullanmalıyım?

Her iki Uygulama Yapılandırma katmanı da config ayarları, özellik bayrakları, Key Vault başvuruları, temel yönetim işlemleri, ölçümler ve günlükler dahil olmak üzere temel işlevsellik sunar.

Bir katman seçmek için göz önünde bulundurulması gereken hususlar şunlardır.

- **Abonelik başına kaynaklar**: Kaynak tek bir yapılandırma deposundan oluşur. Her abonelik, ücretsiz katmandaki bir yapılandırma mağazasıyla sınırlıdır. Aboneliklerde standart katmanda sınırsız sayıda yapılandırma mağazası olabilir.
- **Kaynak başına depolama**: Serbest katmanda, her yapılandırma deposu 10 MB depolama alanıyla sınırlıdır. Standart katmanda, her yapılandırma deposu en fazla 1 GB depolama alanı kullanabilir.
- **Anahtar geçmişi**: App Configuration tuşlarda yapılan tüm değişikliklerin geçmişini saklar. Serbest katmanda, bu tarih yedi gün boyunca saklanır. Standart katmanda, bu geçmiş 30 gün boyunca saklanır.
- **Günlük istekler**: Ücretsiz mağaza sayısı günlük 1.000 istekle sınırlıdır. Bir mağaza 1.000 isteklere ulaştığında, gece yarısı UTC'ye kadar tüm istekler için HTTP durum kodu 429'u döndürür.

    Standart katman mağazalar için, her gün ilk 200.000 istek günlük ücrete dahildir. Ek istekler fazlalık olarak faturalandırılır.

- **Hizmet düzeyi sözleşmesi**: Standart katman %99,9 kullanılabilirlik SLA'ya sahiptir. Serbest katmanda SLA yok.
- **Güvenlik özellikleri**: Her iki katman da Microsoft tarafından yönetilen anahtarlarla şifreleme, HMAC veya Azure Active Directory üzerinden kimlik doğrulama, RBAC desteği ve yönetilen kimlik gibi temel güvenlik işlevlerini içerir. Standart katman, Özel Bağlantı desteği ve müşteri tarafından yönetilen anahtarlarla şifreleme gibi daha gelişmiş güvenlik işlevleri sunar.
- **Maliyet**: Standart katman mağazalarında günlük kullanım ücreti vardır. Ayrıca günlük tahsisat geçmiş istekleri için bir fazlalık ücreti var. Ücretsiz bir mağaza kullanmak için hiçbir ücret yoktur.

## <a name="can-i-upgrade-a-store-from-the-free-tier-to-the-standard-tier-can-i-downgrade-a-store-from-the-standard-tier-to-the-free-tier"></a>Bir mağazayı Ücretsiz katmandan Standart katmana yükseltebilir miyim? Bir mağazayı Standart katmandan Serbest katmana düşürebilir miyim?

Ücretsiz katmandan Standart katmana istediğiniz zaman yükseltebilirsiniz.

Bir mağazayı Standart katmandan Serbest katmana indiremezsiniz. Serbest katmanda yeni bir mağaza oluşturabilir ve ardından [yapılandırma verilerini bu mağazaya aktarabilirsiniz.](howto-import-export-data.md)

## <a name="are-there-any-limits-on-the-number-of-requests-made-to-app-configuration"></a>App Configuration'a yapılan istek sayısında herhangi bir sınırlama var mı?

Ücretsiz katmandaki yapılandırma mağazaları günde 1.000 istekle sınırlıdır. Standart katmandaki yapılandırma depoları, istek oranı saatte 20.000 isteği aştığında geçici olarak azaltma yla karşılaşabilir.

Bir mağaza sınırına ulaştığında, süre sona erene kadar yapılan tüm istekler için HTTP durum kodu 429'u döndürecektir. Yanıttaki `retry-after-ms` üstbilgi, isteği yeniden denemeden önce önerilen bekleme süresini (milisaniye cinsinden) verir.

Uygulamanız düzenli olarak HTTP durum kodu 429 yanıtları ile karşılanırsa, yapılan istek sayısını azaltmak için yeniden tasarlamayı düşünün. Daha fazla bilgi için [bkz.](./howto-best-practices.md#reduce-requests-made-to-app-configuration)

## <a name="my-application-receives-http-status-code-429-responses-why"></a>Uygulamam HTTP durum kodu 429 yanıtları alır. Neden?

Bu koşullar altında bir HTTP durum kodu 429 yanıtı alırsınız:

* Ücretsiz katmandaki bir mağaza için günlük istek sınırını aşmak.
* Standart katmandaki bir mağaza için yüksek istek oranı nedeniyle geçici azaltma.
* Aşırı bant genişliği kullanımı.
* Depolama teklifi aşıldığında bir anahtar oluşturmaya veya değiştirmeye çalışmak.

İsteğin neden başarısız olduğunu belirli bir nedenle 429 yanıtının gövdesini kontrol edin.

## <a name="how-can-i-receive-announcements-on-new-releases-and-other-information-related-to-app-configuration"></a>Yeni sürümler ve Uygulama Yapılandırması ile ilgili diğer bilgiler le ilgili duyuruları nasıl alabiliyorum?

[Bizim GitHub duyurular repo](https://github.com/Azure/AppConfiguration-Announcements)abone olun.

## <a name="how-can-i-report-an-issue-or-give-a-suggestion"></a>Bir sorunu nasıl bildirebilirim veya öneride bulunabilirim?

Bize doğrudan [GitHub'dan](https://github.com/Azure/AppConfiguration/issues)ulaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Uygulama Yapılandırması hakkında](./overview.md)
