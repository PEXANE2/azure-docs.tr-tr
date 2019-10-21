---
title: Yük devretme ve düzeltme eki uygulama-Redsıs için Azure önbelleği | Microsoft Docs
description: Redsıs için Azure önbelleği için yük devretme, düzeltme eki uygulama ve güncelleştirme süreci hakkında bilgi edinin.
services: cache
author: asasine
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: adsasine
ms.openlocfilehash: 305511efe86d2b241ef5014d9c3f0501cfd3fbdc
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675908"
---
# <a name="failover-and-patching-for-azure-cache-for-redis"></a>Redsıs için Azure önbelleği için yük devretme ve düzeltme eki uygulama

Redsıs hizmeti için Azure önbelleği ile yük devretme işleminin bağlamını anlamak, dayanıklı ve başarılı istemci uygulamaları oluşturmak için kritik öneme sahiptir. Önbellek yük devretmesinin yaygın bir nedeni, Redsıs ikililerinin düzeltme eki uygulama yönetim hizmetinden gelir. Bu makalede, yük devretme işleminin ne olduğu, düzeltme eki sırasında nasıl oluştuğu ve dayanıklı bir istemci uygulamasının nasıl oluşturulacağı ele alınmaktadır.

## <a name="what-is-a-failover"></a>Yük devretme nedir?

### <a name="a-quick-summary-of-our-architecture"></a>Mimarimizin hızlı bir özeti

Önbellek, ayrı özel IP 'Leri olan birden çok sanal makineden oluşturulur. Düğüm olarak da bilinen her sanal makine, tek bir sanal IP 'ye sahip paylaşılan yük dengeleyiciye bağlanır. Her düğüm Redsıs sunucu sürecini çalıştırır ve ana bilgisayar adı ve Redsıs bağlantı noktaları aracılığıyla erişilebilir. Her düğüm, bir ana veya bir çoğaltma düğümü olarak kabul edilir. Bir istemci uygulaması bir önbelleğe bağlanırsa, trafiği bu yük dengeleyiciden geçer ve otomatik olarak ana düğüme yönlendirilir.

Temel bir önbellekte tek düğüm her zaman bir yöneticisidir. Standart veya Premium önbelleğinde, birinin ana öğe seçildiği ve diğeri çoğaltma olduğu iki düğüm vardır. Standart ve Premium önbellekler birden çok düğüme sahip olduğu için, diğeri istekleri işlemeye devam ederken bir düğüm kullanılamaz olabilir. Kümelenmiş önbellekler, her biri ayrı ana ve çoğaltma düğümlerine sahip birçok parça oluşur. Diğerleri kullanılabilir kaldığı sürece bir parça daha düşük olabilir.

> [!NOTE]
> Temel bir önbellekte birden fazla düğüm yoktur ve kullanılabilirlik üzerinde SLA sunmaz. Temel önbellekler yalnızca geliştirme ve test amaçları için önerilir. Kullanılabilirliği artırmak için çok düğümlü bir dağıtım için standart veya Premium önbellek kullanın.

### <a name="a-failover-explained"></a>Açıklanan yük devretme

Yük devretme, bir çoğaltma düğümü kendisini ana düğüm olacak şekilde yükseltir ve eski ana düğüm varolan bağlantıları kapattığında oluşur. Ana düğüm geri alındıktan sonra, rollerdeki değişikliği fark eder ve çoğaltma haline gelmesi için kendisini indirgeyin. Daha sonra yeni ana sunucuya bağlanır ve verileri eşitler. Yük devretme planlanmış veya planlanmamış olabilir.

Redis düzeltme eki uygulama ve işletim sistemi yükseltmeleri ve ölçeklendirme ve yeniden başlatma gibi yönetim işlemleri gibi sistem güncelleştirmeleri sırasında planlanmış bir yük devretme gerçekleşir. Düğümlere güncelleştirme hakkında gelişmiş bir bildirim verildiğinden, bu kişiler, bu roller arasında değişiklik yapabilir ve değişikliğin yük dengeleyiciyi hızla güncelleştirebilir. Planlı Yük devretme 1 saniyeden az bir sürede tamamlanmalıdır.

Donanım hatası, ağ arızası veya ana düğüme yönelik diğer beklenmedik kesintiler nedeniyle planlanmamış yük devretme gerçekleşebilir. Çoğaltma düğümü kendisini ana olacak şekilde yükseltir, ancak işlem daha uzun sürer. Bir çoğaltma düğümü, yük devretme işlemini başlatmak için önce ana düğümünü algılamamalıdır. Yük devretmeyi önlemek için, çoğaltma düğümü Ayrıca bu planlanmamış hatanın geçici veya yerel olmadığından emin olmalıdır. Bu algılama gecikmesi, planlanmamış bir yük devretmenin genellikle 10 ila 15 saniye içinde tamamlandığı anlamına gelir.

## <a name="how-does-patching-occur"></a>Düzeltme eki uygulama nasıl yapılır?

Redsıs hizmeti için Azure önbelleği, en son platform özellikleri ve düzeltmeleriyle önbelleğinizi güncelleştirmek için düzenli olarak bakım yapar. Bir önbelleğe yama yapmak için hizmet aşağıdaki adımları izler:

1. Yönetim hizmeti düzeltme için bir düğüm seçer.
1. Seçili düğüm bir ana düğümse, çoğaltma düğümü birlikte kendi kendisini yükseltir. Bu promosyon planlı yük devretme olarak kabul edilir.
1. Seçili düğüm, yeni değişiklikleri yapmak için yeniden başlatılır ve bir çoğaltma düğümü olarak geri gönderilir. Çoğaltma düğümleri ana düğüme bağlanır ve verileri eşitler.
1. Veri eşitleme işlemi tamamlandığında, düzeltme eki uygulama, kalan düğümler için yinelenir.

Düzeltme eki uygulama planlı bir yük devretme olduğundan, çoğaltma düğümü bir ana olacak şekilde kendisini hızlı bir şekilde yükseltir ve istekleri ve yeni bağlantıları başlatır. Temel önbellekler bir çoğaltma düğümüne sahip değildir ve güncelleştirme tamamlanana kadar kullanılamaz. Kümelenmiş bir önbelleğin her parçası ayrı düzeltme eki uygulanmış olur ve başka bir parça bağlantılarını kapatmaz.

> [!IMPORTANT]
> Düğümler, veri kaybını engellemek için tek seferde düzeltme eki uygulanmış. Temel önbelleklerin veri kaybı olur. Kümelenmiş önbellekler tek seferde bir parça düzeltme hallenir.

Aynı kaynak grubunda ve bölgede bulunan birden çok önbellek de aynı anda bir tane düzeltme eki uygulanmış.  Farklı kaynak gruplarında veya farklı bölgelerde bulunan önbellekler aynı anda düzeltme olabilir.

Tam veri eşitlemesi işlem tekrarından önce yapıldığından, standart veya Premium bir önbellek kullanılırken veri kaybı oluşması düşüktür. Verileri [dışarı](cache-how-to-import-export-data.md#export) aktarıp [kalıcılığı](cache-how-to-premium-persistence.md)etkinleştirerek veri kaybına karşı daha fazla koruma sağlayabilirsiniz.

### <a name="additional-cache-load"></a>Ek önbellek yükü

Yük devretme gerçekleştiğinde, standart ve Premium önbellekler verileri bir düğümden diğerine çoğaltmalıdır. Bu çoğaltma, sunucu belleğinde ve CPU 'da bazı yük artışına neden olur. Önbellek örneği zaten yüklüyse, istemci uygulamaları daha fazla gecikme yaşar. Olağanüstü durumlarda, istemci uygulamaları zaman aşımı özel durumları alabilir. Bu ek yükün etkisini azaltmaya yardımcı olmak için önbelleğin `maxmemory-reserved` ayarını [yapılandırın](cache-configure.md#memory-policies) .

## <a name="how-does-a-failover-impact-my-client-application"></a>Yük devretme, istemci uygulamamı nasıl etkiler?

İstemci uygulaması tarafından görülen hataların sayısı, yük devretme sırasında o bağlantıda kaç işlemin beklendiğini gösterir. Bağlantıları kapatan düğüm üzerinden yönlendirilen herhangi bir bağlantı, hataları görürler. Birçok istemci kitaplığı, bağlantıların kesilmesi sırasında zaman aşımı özel durumları, bağlantı özel durumları veya yuva özel durumları dahil farklı türlerde hatalar oluşturabilir. Özel durumların sayısı ve türü, isteğin, önbellek bağlantılarını kapatışında, isteğin bulunduğu yere bağlıdır. Örneğin, bir istek gönderen ancak yük devretme gerçekleştiğinde yanıt almamış bir işlem zaman aşımı özel durumu alabilir. Kapalı bağlantı nesnesindeki yeni istekler, yeniden bağlantı başarıyla gerçekleşene kadar bağlantı özel durumları alacaktır.

Çoğu istemci kitaplığı, bunu yapmak üzere yapılandırılmışsa önbelleğe yeniden bağlanmaya çalışır, ancak öngörülemeyen hatalar zaman zaman kurtarılamaz bir duruma yer alabilir. Hatalar önceden yapılandırılmış bir süreden daha uzun süre devam ediyorsa, bağlantı nesnesinin yeniden oluşturulması gerekir. .NET ve diğer nesne yönelimli dillerde, uygulamayı yeniden başlatmadan bağlantıyı yeniden oluşturmak [yavaş \<T \> bir model](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#reconnecting-with-lazyt-pattern)kullanılarak gerçekleştirilebilir.

### <a name="what-should-i-do-in-my-application"></a>Uygulamamda ne yapmam gerekir?

Yük devretmenin tamamen önlenemez olduğundan, istemci uygulamaların bağlantı kesmeleri ve başarısız istekler için bir esneklik için yazılması gerekir. Çoğu istemci kitaplığı otomatik olarak önbellek uç noktasına yeniden bağlanmamasına karşın, bazı istemci kitaplıkları başarısız istekleri yeniden denemeye çalışır. Uygulama senaryosuna bağlı olarak, yeniden deneme mantığı geri dönüş mantıklı olabilir.

İstemci uygulamasının esnekliğini test etmek için, bağlantı sonlarına yönelik el ile tetikleyici olarak [yeniden başlatma](cache-administration.md#reboot) kullanın. Ayrıca, Yönetim hizmetinde belirtilen haftalık pencereler sırasında Redsıs çalışma zamanı düzeltme eklerinin uygulanacağını bildirmek için bir önbellekte [güncelleştirme zamanlamanız](cache-administration.md#schedule-updates) önerilir. Bu pencereler genellikle, olası olayları önlemek için istemci uygulama trafiğinin düşük olduğu durumlarda dönemler için seçilir.

### <a name="client-network-configuration-changes"></a>İstemci ağ yapılandırması değişiklikleri

Bazı istemci tarafı ağ yapılandırması değişiklikleri "bağlantı kullanılamıyor" hatalarını tetikleyebilirsiniz.  Hazırlama ve üretim yuvaları arasında bir istemci uygulamasının sanal IP adresini değiştirme veya uygulamanızın örneklerinin boyutunu/sayısını ölçeklendirme, en son bir dakikadan kısa bir bağlantı sorununa neden olabilir. İstemci uygulamanız, redin 'e ek olarak diğer dış ağ kaynaklarıyla bağlantıyı kaybedebilir.

## <a name="next-steps"></a>Sonraki adımlar

- Önbelleğiniz için [güncelleştirmeleri zamanlayın](cache-administration.md#schedule-updates) .
- [Yeniden başlatma](cache-administration.md#reboot)kullanarak uygulama dayanıklılığı test edin.
- Bellek ayırmalarını ve ilkelerini [yapılandırın](cache-configure.md#memory-policies) .
