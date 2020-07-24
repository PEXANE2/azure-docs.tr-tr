---
title: Yük devretme ve düzeltme eki uygulama - Redis için Azure Cache
description: Redsıs için Azure önbelleği için yük devretme, düzeltme eki uygulama ve güncelleştirme süreci hakkında bilgi edinin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 10/18/2019
ms.openlocfilehash: 7cfa7257e64421c30c359bb34044988bbb5af1dd
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093094"
---
# <a name="failover-and-patching-for-azure-cache-for-redis"></a>Redsıs için Azure önbelleği için yük devretme ve düzeltme eki uygulama

Esnek ve başarılı istemci uygulamaları oluşturmak için, Redsıs hizmeti için Azure önbelleği bağlamında yük devretmeyi anlamak önemlidir. Yük devretme, planlı yönetim işlemlerinin bir parçası olabilir veya planlanmamış donanım ya da ağ hatalarından kaynaklanıyor olabilir. Önbellek yük devretmesinin yaygın bir kullanımı, yönetim hizmeti Redsıs ikilileri için Azure önbelleğini yayüklerken gelir. Bu makalede, yük devretme işleminin ne olduğu, düzeltme eki sırasında nasıl gerçekleştiği ve dayanıklı bir istemci uygulamasının nasıl oluşturulacağı ele alınmaktadır.

## <a name="what-is-a-failover"></a>Yük devretme nedir?

Redin için Azure önbelleği için yük devretmeye genel bakış ile başlayalım.

### <a name="a-quick-summary-of-cache-architecture"></a>Önbellek mimarisinin hızlı bir özeti

Önbellek, ayrı, özel IP adreslerine sahip birden çok sanal makineden oluşturulur. Düğüm olarak da bilinen her sanal makine, tek bir sanal IP adresine sahip paylaşılan yük dengeleyiciye bağlanır. Her düğüm Redsıs sunucu sürecini çalıştırır ve ana bilgisayar adı ve Redsıs bağlantı noktaları aracılığıyla erişilebilir. Her düğüm, birincil ya da çoğaltma düğümü olarak değerlendirilir. Bir istemci uygulaması bir önbelleğe bağlanırsa, trafiği bu yük dengeleyiciden geçer ve otomatik olarak birincil düğüme yönlendirilir.

Temel bir önbellekte tek düğüm her zaman birincil olur. Standart veya Premium önbelleğinde iki düğüm vardır: biri birincil olarak seçilir ve diğeri çoğaltmadır. Standart ve Premium önbellekler birden çok düğüme sahip olduğundan, diğeri istekleri işlemeye devam ederken bir düğüm kullanılamaz olabilir. Kümelenmiş önbellekler, her biri ayrı birincil ve çoğaltma düğümlerine sahip birçok parça oluşur. Bir parça, diğerleri kullanılabilir kaldığı sırada kalmış olabilir.

> [!NOTE]
> Temel bir önbellekte birden fazla düğüm yoktur ve kullanılabilirlik için hizmet düzeyi sözleşmesi (SLA) sunmaz. Temel önbellekler yalnızca geliştirme ve test amaçları için önerilir. Kullanılabilirliği artırmak için çok düğümlü bir dağıtım için standart veya Premium önbellek kullanın.

### <a name="explanation-of-a-failover"></a>Yük devretme açıklaması

Bir yük devretme işlemi, bir çoğaltma düğümü kendisini birincil düğüm olacak şekilde yükseltir ve eski birincil düğüm varolan bağlantıları kapattığında oluşur. Birincil düğüm geri alındıktan sonra, rollerdeki değişikliği fark eder ve bir çoğaltma haline gelmesi için kendisini indirger. Daha sonra yeni birincil ağa bağlanır ve verileri eşitler. Yük devretme planlanmış veya planlanmamış olabilir.

Redis düzeltme eki uygulama, IŞLETIM sistemi yükseltmeleri ve ölçekleme ve yeniden başlatma gibi yönetim işlemleri gibi sistem güncelleştirmeleri sırasında *planlanmış bir yük devretme* gerçekleşir. Düğümler güncelleştirme için öncelikli bir bildirim aldığından, bu roller, kolayca takas rolleri ve değişikliğin yük dengeleyiciyi hızlıca güncelleştirebilirler. Planlanmış bir yük devretme genellikle 1 saniyeden az bir sürede tamamlanır.

Donanım hatası, ağ arızası veya birincil düğüme yönelik diğer beklenmedik kesintiler nedeniyle *planlanmamış yük devretme* gerçekleşebilir. Çoğaltma düğümü kendisini birincili yükseltir, ancak işlem daha uzun sürer. Bir çoğaltma düğümünün, yük devretme işlemini başlatabilmesi için öncelikle birincil düğümünün mevcut olmadığından emin olması gerekir. Gereksiz bir yük devretmenin önüne geçmek için, çoğaltma düğümü bu planlanmamış hatanın geçici ya da yerel olmadığından emin olmalıdır. Bu algılama gecikmesi, planlanmamış bir yük devretmenin genellikle 10 ile 15 saniye içinde bittiği anlamına gelir.

## <a name="how-does-patching-occur"></a>Düzeltme eki uygulama nasıl yapılır?

Redsıs hizmeti için Azure önbelleği, en son platform özellikleri ve düzeltmeleriyle önbelleğinizi düzenli olarak güncelleştirir. Bir önbelleğe yama yapmak için hizmet aşağıdaki adımları izler:

1. Yönetim hizmeti düzeltme için bir düğüm seçer.
1. Seçili düğüm birincil düğümse, karşılık gelen çoğaltma düğümü birlikte kendisini yükseltir. Bu promosyon planlı yük devretme olarak kabul edilir.
1. Seçili düğüm, yeni değişiklikleri yapmak için yeniden başlatılır ve bir çoğaltma düğümü olarak geri gönderilir.
1. Çoğaltma düğümü birincil düğüme bağlanır ve verileri eşitler.
1. Veri eşitleme işlemi tamamlandığında, düzeltme eki uygulama, kalan düğümler için yinelenir.

Düzeltme eki uygulama planlı bir yük devretme olduğundan, çoğaltma düğümü birincil olacak ve hizmet isteklerine ve yeni bağlantılarla çalışmaya başlayacak şekilde kendisini hızla yükseltir. Temel önbellekler bir çoğaltma düğümüne sahip değildir ve güncelleştirme tamamlanana kadar kullanılamaz. Kümelenmiş bir önbelleğin her parçası ayrı düzeltme eki uygulanmış olur ve başka bir parça bağlantılarını kapatmaz.

> [!IMPORTANT]
> Düğümler, veri kaybını engellemek için tek seferde düzeltme eki uygulanmış. Temel önbelleklerin veri kaybı olur. Kümelenmiş önbellekler tek seferde bir parça düzeltme hallenir.

Aynı kaynak grubunda ve bölgede bulunan birden çok önbellek de aynı anda bir tane düzeltme eki uygulanmış.  Farklı kaynak gruplarında veya farklı bölgelerde bulunan önbellekler aynı anda düzeltme olabilir.

Tam veri eşitlemesi işlem tekrardan önce yapıldığından, standart veya Premium bir önbellek kullandığınızda veri kaybı oluşması çok düşüktür. Verileri [dışarı](cache-how-to-import-export-data.md#export) aktarıp [kalıcılığı](cache-how-to-premium-persistence.md)etkinleştirerek veri kaybına karşı daha fazla koruma sağlayabilirsiniz.

## <a name="additional-cache-load"></a>Ek önbellek yükü

Yük devretme gerçekleştiğinde, standart ve Premium önbellekler verileri bir düğümden diğerine çoğaltmalıdır. Bu çoğaltma, sunucu belleğinde ve CPU 'da bazı yük artışına neden olur. Önbellek örneği zaten yüklüyse, istemci uygulamaları daha fazla gecikme yaşar. Olağanüstü durumlarda, istemci uygulamaları zaman aşımı özel durumları alabilir. Bu ek yükün etkisini azaltmaya yardımcı olmak için önbelleğin ayarını [yapılandırın](cache-configure.md#memory-policies) `maxmemory-reserved` .

## <a name="how-does-a-failover-affect-my-client-application"></a>Yük devretme istemci uygulamamı nasıl etkiler?

İstemci uygulaması tarafından görülen hataların sayısı, yük devretme sırasında o bağlantıda kaç işlemin beklendiğini gösterir. Bağlantılarını kapatan düğüm üzerinden yönlendirilen herhangi bir bağlantı, hataları görürler. Birçok istemci kitaplığı, bağlantı kesme sırasında zaman aşımı özel durumları, bağlantı özel durumları veya yuva özel durumları dahil farklı hata türleri oluşturabilir. Özel durumların sayısı ve türü, isteğin, önbellek bağlantılarını kapatışında, isteğin bulunduğu yere bağlıdır. Örneğin, bir istek gönderen ancak yük devretme gerçekleştiğinde yanıt almamış bir işlem zaman aşımı özel durumu alabilir. Kapalı bağlantı nesnesindeki yeni istekler, yeniden bağlantı başarıyla gerçekleşene kadar bağlantı özel durumları alıyor.

Çoğu istemci kitaplığı, bu şekilde yapılandırıldıysa önbelleğe yeniden bağlanmaya çalışır. Ancak, öngörülemeyen hatalar bazen kitaplık nesnelerini kurtarılamaz bir duruma yerleştirebilir. Hatalar önceden yapılandırılmış bir süreden daha uzun süre devam ediyorsa bağlantı nesnesi yeniden oluşturulmalıdır. Microsoft.NET ve diğer nesne yönelimli dillerde, uygulamayı yeniden başlatmadan bağlantıyı yeniden oluşturmak [yavaş bir \<T\> model](https://gist.github.com/JonCole/925630df72be1351b21440625ff2671f#reconnecting-with-lazyt-pattern)kullanılarak gerçekleştirilebilir.

### <a name="how-do-i-make-my-application-resilient"></a>Uygulamamı dayanıklı hale getirmek Nasıl yaparım??

Yük devretmeleri tamamen önlemenize izin vermekten, bağlantı kesme ve başarısız isteklere dayanıklılık sağlamak için istemci uygulamalarınızı yazın. Çoğu istemci kitaplığı otomatik olarak önbellek uç noktasına yeniden bağlansa da, başarısız istekleri yeniden denemeye çalışır. Uygulama senaryosuna bağlı olarak, yeniden deneme mantığını geri alma ile kullanmak mantıklı olabilir.

İstemci uygulamasının esnekliğini test etmek için, bağlantı sonlarına yönelik el ile tetikleyici olarak [yeniden başlatma](cache-administration.md#reboot) kullanın. Ayrıca, güncelleştirmeleri bir önbellekte [zamanlamanızı](cache-administration.md#schedule-updates) öneririz. Yönetim hizmetine, belirtilen haftalık pencereler sırasında Redsıs çalışma zamanı düzeltme ekleri uygulamayı söyleyin. Olası olayları önlemek için, bu pencereler genellikle istemci uygulama trafiğinin düşük olduğu dönemler olur.

### <a name="client-network-configuration-changes"></a>İstemci ağı-yapılandırma değişiklikleri

İstemci tarafı ağ yapılandırma değişiklikleri, "bağlantı kullanılamıyor" hatalarını tetikleyebilir. Bu değişiklikler şunları içerebilir:

- Hazırlama ve üretim yuvaları arasında bir istemci uygulamasının sanal IP adresini değiştirme.
- Uygulamanızın boyutunu veya örnek sayısını ölçeklendirin.

Bu değişiklikler, bir dakikadan kısa bir sürede bağlantı sorununa neden olabilir. İstemci uygulamanız, Redsıs hizmeti için Azure önbelleğinin yanı sıra diğer dış ağ kaynaklarıyla olan bağlantısını da kaybedecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Önbelleğiniz için [güncelleştirmeleri zamanlayın](cache-administration.md#schedule-updates) .
- [Yeniden başlatma](cache-administration.md#reboot)kullanarak uygulama dayanıklılığı test edin.
- Bellek ayırmalarını ve ilkelerini [yapılandırın](cache-configure.md#memory-policies) .
