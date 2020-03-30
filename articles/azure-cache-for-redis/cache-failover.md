---
title: Yük devretme ve düzeltme eki uygulama - Redis için Azure Cache
description: Redis için Azure Önbelleği için düzeltme, düzeltme ve güncelleştirme işlemi hakkında bilgi edinin.
author: asasine
ms.service: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: adsasine
ms.openlocfilehash: 6ff33bd594181aabc4fd7d55ce33f780a0d06086
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74122193"
---
# <a name="failover-and-patching-for-azure-cache-for-redis"></a>Redis için Azure Önbelleği için başarısız lık ve düzeltme

Esnek ve başarılı istemci uygulamaları oluşturmak için Redis için Azure Önbelleği hizmeti bağlamında başarısızlığı anlamak çok önemlidir. Başarısız olmak, planlı yönetim işlemlerinin bir parçası olabilir veya planlanmamış donanım veya ağ hatalarından kaynaklanabilir. Önbellek başarısızlığı yaygın bir kullanım, yönetim hizmeti Redis ikilileri için Azure Önbelleği'ni yamaları ile gerçekleşir. Bu makalede, bir failover nedir, yama sırasında nasıl oluşur ve nasıl esnek bir istemci uygulaması oluşturmak için kapsar.

## <a name="what-is-a-failover"></a>Başarısız olmak nedir?

Redis için Azure Önbelleği için başarısızolmaya genel bir bakışla başlayalım.

### <a name="a-quick-summary-of-cache-architecture"></a>Önbellek mimarisinin hızlı bir özeti

Bir önbellek ayrı, özel IP adresleri ile birden çok sanal makineden oluşturulur. Düğüm olarak da bilinen her sanal makine, tek bir sanal IP adresine sahip paylaşılan bir yük dengeleyicisine bağlanır. Her düğüm Redis sunucu işlemini çalıştırZ ve ana bilgisayar adı ve Redis bağlantı noktaları ile erişilebilir. Her düğüm bir ana veya yineleme düğümü olarak kabul edilir. İstemci uygulaması önbelleğe bağlandığında, trafiği bu yük dengeleyicisi üzerinden geçer ve otomatik olarak ana düğüme yönlendirilir.

Temel önbellekte, tek düğüm her zaman bir ana dır. Standart veya Premium önbellekte iki düğüm vardır: biri ana karakter olarak seçilir, diğeri yinelemedir. Standart ve Premium önbelleklerinde birden çok düğüm olduğundan, diğerinin istekleri işlemeye devam ettiği bir düğüm kullanılamayabilir. Kümelenmiş önbellekler, her biri farklı ana ve yineleme düğümlerine sahip birçok parçadan oluşur. Diğerleri kullanılabilir durumda kalırken bir parça aşağı olabilir.

> [!NOTE]
> Temel önbelleğin birden çok düğümü yoktur ve kullanılabilirliği için hizmet düzeyi sözleşmesi (SLA) sunmaz. Temel önbellekler yalnızca geliştirme ve test amacıyla önerilir. Kullanılabilirliği artırmak için çok düğümlü dağıtım için Standart veya Premium önbellek kullanın.

### <a name="explanation-of-a-failover"></a>Bir başarısızlık açıklaması

Bir yineleme düğümü kendisini ana düğüm olarak tanıttığında ve eski ana düğüm varolan bağlantıları kapattığında bir hata oluşur. Ana düğüm geri geldikten sonra, rollerdeki değişikliği fark eder ve bir kopya haline gelmek için kendini düşürür. Daha sonra yeni ana bağlılanır ve verileri eşitler. Bir başarısızlık planlanmış veya planlanmamış olabilir.

Redis yama veya işletim sistemi yükseltmeleri gibi sistem güncelleştirmeleri ve ölçekleme ve yeniden başlatma gibi yönetim işlemleri sırasında planlanan bir *hata* gerçekleşir. Düğümler güncelleştirmehakkında önceden bildirimde bulunduklarından, rolleri işbirliğiiçinde değiştirebilir ve değişikliğin yük dengeleyicisini hızla güncelleyebilirler. Planlı bir hata genellikle 1 saniyeden kısa bir sürede sona ereb.

Aygıt hatası, ağ hatası veya ana düğümdeki diğer beklenmeyen kesintiler nedeniyle *planlanmamış* bir hata olabilir. Yineleme düğümü kendisini ana olarak tanıtıyor, ancak işlem daha uzun sürüyor. Bir yineleme düğümü, başarısız lık işlemini başlatmadan önce ana düğümün kullanılmadığını algılamalıdır. Yineleme düğümü, gereksiz bir başarısızlığı önlemek için bu planlanmamış hatanın geçici veya yerel olmadığını da doğrulamalıdır. Algılamadaki bu gecikme, planlanmamış bir arızanın genellikle 10 ila 15 saniye içinde bittiği anlamına gelir.

## <a name="how-does-patching-occur"></a>Yama nasıl oluşur?

Redis için Azure Önbelleği hizmeti önbelleğinizi en son platform özellikleri ve düzeltmeleriyle düzenli olarak güncelleştirir. Önbelleği yamalamak için hizmet aşağıdaki adımları izler:

1. Yönetim hizmeti yamalı bir düğüm seçer.
1. Seçili düğüm bir ana düğümse, karşılık gelen yineleme düğümü birlikte kendini tanıtıyor. Bu promosyon planlı bir başarısızlık olarak kabul edilir.
1. Seçili düğüm, yeni değişiklikleri almak için yeniden başlatılır ve yineleme düğümü olarak geri gelir.
1. Yineleme düğümü ana düğüme bağlanır ve verileri eşitler.
1. Veri eşitleme tamamlandığında, kalan düğümler için düzeltme işlemi yinelenir.

Yama planlı bir başarısızlık olduğundan, yineleme düğümü hızla bir ana olmak için kendini teşvik eder ve hizmet istekleri ve yeni bağlantılar başlar. Temel önbelleklerin yineleme düğümü yoktur ve güncelleştirme tamamlanana kadar kullanılamaz. Kümelenmiş önbelleğin her parçası ayrı ayrı yamalı ve bağlantıları başka bir parçaya kapatmaz.

> [!IMPORTANT]
> Düğümler veri kaybını önlemek için birer birer yamalı. Temel önbellekler veri kaybına sahip olacaktır. Kümelenmiş önbellekler bir seferde bir parça yamalı.

Aynı kaynak grubundaki ve bölgedeki birden çok önbellek de birer birer yamalı.  Farklı kaynak gruplarında veya farklı bölgelerde bulunan önbellekler aynı anda yamalanabilir.

İşlem yinelenmeden önce tam veri eşitlemesi gerçekleştiğinden, Standart veya Premium önbellek kullandığınızda veri kaybı nın gerçekleşmesi olası değildir. Veri [dışa aktararak](cache-how-to-import-export-data.md#export) ve [kalıcılığı](cache-how-to-premium-persistence.md)sağlayarak veri kaybına karşı daha fazla korunabilirsiniz.

## <a name="additional-cache-load"></a>Ek önbellek yükü

Bir hata olduğunda, Standart ve Premium önbelleklerinin verileri bir düğümden diğerine çoğaltması gerekir. Bu çoğaltma hem sunucu belleğinde hem de CPU'da bazı yük artışına neden olur. Önbellek örneği zaten yüklüyse, istemci uygulamaları daha fazla gecikme süresi yaşayabilir. Ekstrem durumlarda, istemci uygulamaları zaman ayarı özel durumları alabilir. Bu ek yükün etkisini azaltmaya yardımcı olmak için önbelleğin `maxmemory-reserved` ayarını [yapılandırın.](cache-configure.md#memory-policies)

## <a name="how-does-a-failover-affect-my-client-application"></a>Bir arıza müşteri başvurumu nasıl etkiler?

İstemci uygulaması tarafından görülen hata sayısı, hata nın sona erme sırasında bu bağlantıda bekleyen kaç işlem olduğuna bağlıdır. Bağlantılarını kapatan düğümden yönlendirilen herhangi bir bağlantıda hatalar görür. Birçok istemci kitaplığı, zaman sonu özel durumları, bağlantı özel durumları veya soket özel durumları da dahil olmak üzere bağlantılar kırıldığında farklı türde hatalar atabilir. Özel durumların sayısı ve türü, önbelleğin bağlantılarını kapattığında isteğin kod yolunda nerede olduğuna bağlıdır. Örneğin, istek gönderen ancak başarısız olduğunda yanıt alamayan bir işlem zaman ayarı özel durumu alabilir. Kapalı bağlantı nesnesindeki yeni istekler, yeniden bağlantı başarılı bir şekilde gerçekleşene kadar bağlantı özel durumları alır.

İstemci kitaplıkların çoğu, bunu yapmak üzere yapılandırılırsa önbelleğe yeniden bağlanmayı dener. Ancak, öngörülemeyen hatalar bazen kitaplık nesnelerini kurtarılamaz bir duruma yerleştirilebilir. Hatalar önceden yapılandırılmış bir süreden daha uzun süre devam ederse, bağlantı nesnesi yeniden oluşturulmalıdır. Microsoft.NET ve diğer nesne yönelimli dillerde, uygulamayı yeniden başlatmadan bağlantıyı yeniden oluşturmak [Tembel\<T\> deseni](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#reconnecting-with-lazyt-pattern)kullanılarak gerçekleştirilebilir.

### <a name="how-do-i-make-my-application-resilient"></a>Başvurumu nasıl esnek hale getirebilirim?

Arızaları tamamen önleyemediğiniz için, bağlantı molalarına ve başarısız isteklere esneklik sağlamak için istemci uygulamalarınızı yazın. İstemci kitaplıklarının çoğu önbellek bitiş noktasına otomatik olarak yeniden bağlansa da, bunların çok azı başarısız istekleri yeniden denemeyi dener. Uygulama senaryosuna bağlı olarak, geri çekilme ile yeniden deneme mantığını kullanmak mantıklı olabilir.

İstemci uygulamasının esnekliğini sınamak için, bağlantı sonu için el ile tetikleyici olarak [yeniden başlatma](cache-administration.md#reboot) yı kullanın. Ayrıca, güncelleştirmeleri bir önbellekte [zamanlamanızı](cache-administration.md#schedule-updates) öneririz. Yönetim hizmetine, belirtilen haftalık pencereler sırasında Redis çalışma zamanı yamaları uygulamalarını söyleyin. Bu pencereler genellikle istemci uygulama trafiğinin düşük olduğu dönemlerdir, olası olayları önlemek için.

### <a name="client-network-configuration-changes"></a>İstemci ağ yapılandırma değişiklikleri

Bazı istemci tarafı ağ yapılandırma değişiklikleri "Bağlantı yok" hatalarını tetikleyebilir. Bu tür değişiklikler şunları içerebilir:

- Bir istemci uygulamasının sanal IP adresini evreleme ve üretim yuvaları arasında değiştirme.
- Uygulamanızın boyutunu veya örnek sayısını ölçekleme.

Bu tür değişiklikler, bir dakikadan kısa süren bir bağlantı sorununa neden olabilir. İstemci uygulamanız büyük olasılıkla Redis için Azure Önbelleği hizmetine ek olarak diğer dış ağ kaynaklarına olan bağlantısını kaybeder.

## <a name="next-steps"></a>Sonraki adımlar

- Önbelleğiniz için [güncelleştirmeleri zamanlayın.](cache-administration.md#schedule-updates)
- [Yeniden başlatma](cache-administration.md#reboot)kullanarak uygulama esnekliğini test edin.
- Bellek rezervasyonlarını ve ilkelerini [yapılandırın.](cache-configure.md#memory-policies)
