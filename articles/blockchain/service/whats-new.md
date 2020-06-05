---
title: Yenilikler Sürüm notları-Azure blok zinciri hizmeti
description: En son sürüm notları, sürümler, bilinen sorunlar ve yaklaşan değişiklikler gibi Azure blok zinciri hizmeti ile nelerin yeni olduğunu öğrenin.
ms.date: 06/03/2020
ms.topic: conceptual
ms.reviewer: ravastra
ms.openlocfilehash: c5316aa387de28fe1a78b336eb2e9e010c624b02
ms.sourcegitcommit: b55d1d1e336c1bcd1c1a71695b2fd0ca62f9d625
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84435432"
---
# <a name="whats-new-in-azure-blockchain-service"></a>Azure blok zinciri hizmetindeki yenilikler nelerdir?

> Bu URL 'YI kopyalayıp yapıştırarak güncelleştirmeler için bu sayfanın ne zaman geri alınacağını öğrenin: RSS Akış `https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Blockchain+Service%22&locale=en-us` Okuyucusu [ ![ RSS akışı okuyucu simgesine](./media/whats-new/feed-icon-16x16.png)](https://docs.microsoft.com/api/search/rss?search=%22Release+notes+-+Azure+Blockchain+Service%22&locale=en-us).

Azure blok zinciri hizmeti, iyileştirmeleri sürekli olarak alır. En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

- Yeni özellikler
- Sürüm yükseltmeleri
- Bilinen sorunlar

---

## <a name="may-2020"></a>Mayıs 2020

### <a name="version-upgrades"></a>Sürüm yükseltmeleri

- Ubuntu sürümüne yükseltme 18,04
- 2.5.0 'e çekirdek sürümü yükseltme
- Tessera sürüm yükseltme 0.10.4

### <a name="azure-blockchain-service-supports-sending-rawprivate-transactions"></a>Azure blok zinciri hizmeti, Rawprıvate işlem gönderilmesini destekler

**Şunu yazın:** Özellik

Müşteriler, düğümdeki hesap dışında özel işlemleri imzalayabilir.

### <a name="two-phase-member-provisioning"></a>İki aşamalı üye sağlama

**Şunu yazın:** Geliştirmesini

İki aşama, bir üyenin var olan bir konsorsiyumun üzerinde oluşturulduğu senaryoların iyileştirmenize yardımcı olur. Üye altyapısı ilk aşamada sağlanıyor. İkinci aşamada, üye blok zinciriyle eşitlenir. İki aşamalı sağlama zaman aşımları nedeniyle üye oluşturma hatasından kaçınmanıza yardımcı olur.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="mining-stops-if-fewer-than-four-validator-nodes"></a>Dört Doğrulayıcı düğümü daha az olursa araştırma durduruluyor

Üretim ağlarında en az dört Doğrulayıcı düğümü olmalıdır. Çekirdek, IBFT kilitlenme hata toleransını (3F + 1) karşılamak için en az dört Doğrulayıcı düğümü gerektirir. Dört Doğrulayıcı düğümü almak için en az iki Azure blok zinciri hizmeti *Standart* katman düğümünüz olmalıdır. Standart bir düğüm, iki Doğrulayıcı düğümüyle birlikte sağlanır.  

Azure blok zinciri hizmeti üzerindeki blok zinciri ağında dört Doğrulayıcı düğümü yoksa, araştırma ağ üzerinde durmayabilir. İşlenen bloklar üzerinde bir uyarı ayarlayarak, araştırma durdurulur. Sağlıklı bir ağda, işlenen blok beş dakika başına düğüm başına 60 blok olacaktır.

Risk azaltma olarak, Azure blok zinciri hizmeti ekibinin düğümü yeniden başlatması gerekir. Müşterilerin, düğümü yeniden başlatması için bir destek isteği açması gerekir. Azure blok zinciri hizmeti ekibi, araştırma sorunlarını otomatik olarak algılayıp düzeltme konusunda çalışmaktadır.

Üretim sınıfı dağıtımları için *Standart* katmanı kullanın. Geliştirme, test ve kavram kanıtı için *temel* katmanı kullanın. Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanını değiştirmek desteklenmez.

### <a name="blockchain-data-manager-requires-standard-tier-node"></a>Blok zinciri Veri Yöneticisi standart katman düğümü gerektiriyor

Blok zinciri Veri Yöneticisi kullanıyorsanız *Standart* katmanı kullanın. *Temel* katmanda yalnızca 4 GB bellek bulunur. Bu nedenle, blok zinciri Veri Yöneticisi ve üzerinde çalışan diğer hizmetler için gereken kullanıma ölçeklenmez.

Geliştirme, test ve kavram kanıtı için *temel* katmanı kullanın. Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanını değiştirmek desteklenmez.

### <a name="large-volume-of-unlock-account-calls-causes-geth-to-crash"></a>Kilit açma hesabı çağrılarının büyük hacmi geth 'nin kilitlenmesine neden oluyor

İşlem gönderilirken bir dizi kilit açma hesabı çağrısı, bir işlem düğümünde çökme oluşmasına neden olabilir. Sonuç olarak, işlemleri durdurmanız gerekir. Aksi takdirde, bekleyen işlem kuyruğu artar.

Geth bir dakikadan kısa bir sürede otomatik olarak yeniden başlatılır. Düğüme bağlı olarak, eşitleme uzun zaman alabilir. Azure blok zinciri hizmeti ekibi, eşitleme süresini azaltacak bir arşivleme özelliğini etkinleştirir.

Geth kilitlenmelerini belirlemek için, uygulama günlüklerindeki blok zinciri iletilerinde herhangi bir hata iletisi için günlüklere bakabilirsiniz. Ayrıca, beklemede olan işlemlerin artması sırasında işlenen blokların azalmasını da denetleyebilirsiniz.

Sorunu azaltmak için, hesabın kilidini açmak üzere imzasız işlemleri bir komutla göndermek yerine imzalı işlemler gönderin. Dışarıdan imzalanmış işlemler için hesabın kilidini açmanız gerekmez.

İmzasız işlemleri göndermek istiyorsanız, Unlock komutunda Time parametresi olarak 0 göndererek hesabın sınırsız süre boyunca kilidini açın. Tüm işlemler gönderildikten sonra hesabı geri alabilirsiniz.  

Azure blok zinciri hizmeti 'nin kullandığı geth parametreleri aşağıda verilmiştir. Bu parametreleri ayarlayamezsiniz.

- Istanbul blok dönemi: 5 saniye
- Kat gaz sınırı: 700000000
- CEIL gaz sınırı: 800000000

### <a name="large-volume-of-private-transactions-reduces-performance"></a>Büyük özel işlem hacmi performansı azaltır

Azure blok zinciri hizmeti temel katmanı ve özel işlemler kullanıyorsanız Tessera kilitlenebilir.

Blok zinciri uygulama günlüklerini inceleyerek ve iletiyi arayarak Tessera kilitlenmesinin algılanmasını sağlayabilirsiniz `Tessera crashed. Restarting Tessera...` .

Azure blok zinciri hizmeti, kilitlenme olduğunda Tessera 'yi yeniden başlatır. Yeniden başlatma yaklaşık bir dakika sürer.

Yüksek hacimli özel işlemler gönderiyorsanız *Standart* katmanı kullanın. Geliştirme, test ve kavram kanıtı için *temel* katmanı kullanın. Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanını değiştirmek desteklenmez.

### <a name="calling-ethestimate-gas-function-reduces-performance"></a>ETH 'ın çağrılması. tahmin gaz işlevi performansı azaltır

*ETH. tahmin* işlevini birden çok kez çağırmak, saniye başına işlem azaltır. Her işlem gönderimi için *ETH. tahmin* gaz işlevini kullanmayın. *ETH. tahmin* işlevi bellek açısından yoğun.

Mümkünse, işlem göndermek için bir klasik gaz değeri kullanın ve *ETH. tahmin*kullanımını en aza indirin.

### <a name="unbounded-loops-in-smart-contracts-reduces-performance"></a>Akıllı sözleşmelerdeki sınırsız döngüler performansı azaltır

Performansı azaltabilmeleri için akıllı sözleşmelerdeki sınırsız döngüden kaçının. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Sınırsız döngüden kaçının](https://blog.b9lab.com/getting-loopy-with-solidity-1d51794622ad )
- [Akıllı sözleşme güvenlik en iyi uygulamaları](https://github.com/ConsenSys/smart-contract-best-practices)
- [Çekirdek tarafından sunulan akıllı sözleşme yönergeleri](http://docs.goquorum.com/en/latest/Security/Framework/Decentralized%20Application/Smart%20Contracts%20Security/)
- [Solidity tarafından sunulan gaz sınırları ve döngüleriyle ilgili yönergeler](https://solidity.readthedocs.io/en/develop/security-considerations.html#gas-limit-and-loops)