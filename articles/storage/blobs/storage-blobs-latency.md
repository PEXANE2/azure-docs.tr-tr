---
title: Blob depolamada gecikme - Azure Depolama
description: Blob depolama işlemleri için gecikme yi anlayın ve ölçün ve blob depolama uygulamalarınızı düşük gecikme süresi için nasıl tasarlayacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: overview
ms.date: 09/05/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 78440b8150a0992bed2e2a3e597fdac8e7a1c7b0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75749719"
---
# <a name="latency-in-blob-storage"></a>Blob depolamada gecikme süresi

Bazen yanıt süresi olarak başvurulan gecikme süresi, bir uygulamanın isteği tamamlamasını beklemesi gereken süredir. Gecikme süresi bir uygulamanın performansını doğrudan etkileyebilir. Düşük gecikme, genellikle kredi kartı işlemleri yapmak veya web sayfalarını yüklemek gibi döngüdeki insanlarla ilgili senaryolar için önemlidir. Gelen olayları telemetri günlüğü veya IoT olayları gibi yüksek oranlarda işlemesi gereken sistemler de düşük gecikme sürelerini gerektirir. Bu makalede, blok lekeleri üzerindeki işlemler için gecikme süresinin nasıl anlayacağı ve ölçüldüğü ve düşük gecikme süresi için uygulamalarınızın nasıl tasarlayacağı açıklanmaktadır.

Azure Depolama, blok blob'ları için iki farklı performans seçeneği sunar: premium ve standart. Premium blok lekeleri, yüksek performanslı SSD diskler aracılığıyla standart blok lekelere göre önemli ölçüde daha düşük ve daha tutarlı gecikme alanı sunar. Daha fazla bilgi için Azure Blob depolama alanında **Premium performans bloğu blob depolama** alanına [bakın: sıcak, serin ve arşiv erişim katmanları.](storage-blob-storage-tiers.md)

## <a name="about-azure-storage-latency"></a>Azure Depolama gecikmesi hakkında

Azure Depolama gecikmesi, Azure Depolama işlemleri için istek oranlarıyla ilgilidir. İstek oranları saniyede giriş/çıkış işlemleri (IOPS) olarak da bilinir.

İstek oranını hesaplamak için, önce her isteğin tamamlanması için gereken süreyi belirleyin, ardından saniyede kaç isteğin işlenebileceğini hesaplayın. Örneğin, bir isteğin tamamlanmasının 50 milisaniye (ms) sürdüğünü varsayalım. Bir olağanüstü okuma veya yazma işlemi ile bir iş parçacığı kullanarak bir uygulama 20 IOPS (1 saniye veya 1000 ms / 50 ms istek başına) elde etmelidir. Teorik olarak, iş parçacığı sayısı ikiye ikiye katlanırsa, uygulama 40 IOPS elde etmek gerekir. Her iş parçacığı için olağanüstü asynchronous okuma veya yazma işlemleri ikiye iki katına çıkarılırsa, uygulama 80 IOPS elde etmek gerekir.

Uygulamada, istek oranları her zaman bu kadar doğrusal ölçeklendirmez, çünkü istemcide görev planlaması, bağlam değiştirme ve benzeri nedenlerden genel merkezler. Hizmet tarafında, Azure Depolama sistemi üzerindeki baskı, kullanılan depolama ortamındaki farklılıklar, diğer iş yüklerinden kaynaklanan gürültü, bakım görevleri ve diğer etkenler nedeniyle gecikme gecikmesinde değişkenlik olabilir. Son olarak, istemci ve sunucu arasındaki ağ bağlantısı, tıkanıklık, yeniden yönlendirme veya diğer aksaklıklar nedeniyle Azure Depolama gecikmesini etkileyebilir.

İş ortası olarak da adlandırılan Azure Depolama bant genişliği, istek oranıyla ilişkilidir ve istek hızının (IOPS) istek boyutuyla çarpılmasıyla hesaplanabilir. Örneğin, saniyede 160 istek varsayarsak, her 256 KiB veri saniyede 40.960 KiB veya saniyede 40 MiB iş artışı ile sonuçlanır.

## <a name="latency-metrics-for-block-blobs"></a>Blok lekeleri için gecikme ölçütleri

Azure Depolama, blok lekeleri için iki gecikme sonu ölçümü sağlar. Bu ölçümler Azure portalında görüntülenebilir:

- **Uçtan uca (E2E) gecikme,** Azure Depolama'nın isteğin ilk paketini aldığı tarihten, Azure Depolama'nın yanıtın son paketinde bir istemci bildirimi alana kadar olan aralığı ölçer.

- **Sunucu gecikmesi,** Azure Depolama'nın isteğin son paketini aldığı tarihten yanıtın ilk paketi Azure Depolama'dan döndürülene kadar olan aralığı ölçer.

Aşağıdaki resimde, `Get Blob` işlemi çağıran örnek bir iş yükü için Ortalama Başarı **E2E Gecikme si** ve Ortalama Başarı Sunucusu **Gecikmesi** gösterilmektedir:

![Blob işlemi için gecikme ölçümlerini gösteren ekran görüntüsü](media/storage-blobs-latency/latency-metrics-get-blob.png)

Normal koşullarda, uçuça gecikme si ile görüntünün örnek iş yükü için gösterdiği sunucu gecikmesi arasında çok az boşluk vardır.

Uçtan uca ve sunucu gecikme söktürme ölçümlerinizi gözden geçiriyorsanız ve uçtan uca gecikmenin sunucu gecikmesinden önemli ölçüde daha yüksek olduğunu fark ederseniz, ek gecikme nin kaynağını araştırın ve adreslendirin.

Uçtan uca ve sunucu gecikmeniz benzerse, ancak daha düşük gecikme ye ihtiyacınız varsa, premium blok blob depolamaalanına geçiş yapmayı düşünün.

## <a name="factors-influencing-latency"></a>Gecikmeyi etkileyen faktörler

Gecikmeyi etkileyen ana faktör işlem boyutudur. Ağ üzerinden aktarılan ve Azure Depolama tarafından işlenen veri miktarı nedeniyle daha büyük işlemlerin tamamlanması daha uzun sürer.

Aşağıdaki diyagram, çeşitli boyutlardaki işlemlerin toplam süresini gösterir. Küçük miktarda veri için gecikme aralığı, veri aktarmak yerine ağırlıklı olarak isteği işlemek için harcanmıştır. İşlem boyutu arttıkça gecikme aralığı sadece biraz artar (aşağıdaki diyagramda 1 işaretlenir). İşlem boyutu daha da arttıkça, toplam gecikme aralığının istek işleme ve veri aktarımı arasında bölünmesi için veri aktarımı için daha fazla zaman harcanmıştır (aşağıdaki diyagramda 2 işaretlenir). Daha büyük işlem boyutlarında, gecikme aralığı neredeyse yalnızca veri aktarımı için harcanmıştır ve istek işleme büyük ölçüde önemsizdir (aşağıdaki diyagramda 3 işaretlenmiştir).

![Toplam çalışma süresini çalışma boyutuna göre gösteren ekran görüntüsü](media/storage-blobs-latency/operation-time-size-chart.png)

Eşzamanlılık ve iş parçacığı gibi istemci yapılandırma faktörleri de gecikmeyi etkiler. Genel iş bilgililiği, herhangi bir zamanda kaç depolama isteğinin uçuşta olduğuna ve uygulamanızın iş parçacığı nı nasıl işlediğine bağlıdır. CPU, bellek, yerel depolama ve ağ arabirimleri de dahil olmak üzere istemci kaynakları gecikmeyi de etkileyebilir.

Azure Depolama isteklerini işleme, istemci CPU'su ve bellek kaynakları gerektirir. İstemci, yetersiz bir sanal makine veya sistemdeki bazı kaçak işlem nedeniyle baskı altındaysa, Azure Depolama isteklerini işlemek için daha az kaynak vardır. İstemci kaynaklarının herhangi bir çekişme veya eksikliği, sunucu gecikmesinde bir artış olmadan uçtan uca gecikmede artışa neden olur ve bu da iki ölçüm arasındaki boşluğu artırır.

Aynı derecede önemli istemci ve Azure Depolama arasındaki ağ arabirimi ve ağ borusu. Örneğin, istemci VM farklı bir Azure bölgesinde veya şirket içindeyse, fiziksel mesafe tek başına önemli bir faktör olabilir. Ağ atlamaları, ISS yönlendirmesi ve internet durumu gibi diğer etkenler genel depolama gecikmesi etkileyebilir.

Gecikme yi değerlendirmek için öncelikle senaryonuz için temel ölçümleri belirleyin. Temel ölçümler, iş yükü profilinize, uygulama yapılandırma ayarlarınıza, istemci kaynaklarını, ağ borusuna ve diğer etkenlere bağlı olarak uygulama ortamınız bağlamında beklenen uçtan uca ve sunucu gecikmesini sağlar. Temel ölçümlere sahip olduğunuzda, normal koşullara karşı anormal koşulları daha kolay tanımlayabilirsiniz. Temel ölçümler, uygulama yapılandırması veya VM boyutları gibi değiştirilen parametrelerin etkilerini gözlemlemenize de olanak tanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Blob depolama için ölçeklenebilirlik ve performans hedefleri](scalability-targets.md)
- [Blob depolama için performans ve ölçeklenebilirlik kontrol listesi](storage-performance-checklist.md)
