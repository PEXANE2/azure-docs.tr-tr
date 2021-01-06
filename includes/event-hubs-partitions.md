---
title: include dosyası
description: include dosyası
services: event-hubs
author: spelluru
ms.service: event-hubs
ms.topic: include
ms.date: 01/05/2021
ms.author: spelluru
ms.custom: include file
ms.openlocfilehash: 780da47e6f071d854a16ca1d1c5cd02dbdd6bef0
ms.sourcegitcommit: 19ffdad48bc4caca8f93c3b067d1cf29234fef47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97955662"
---
Olay Hub 'ı, olay dizilerini bir veya daha fazla bölüm halinde düzenler. Daha yeni olaylar geldikçe, bu sıranın sonuna eklenir. Bölüm bir "yürütme günlüğü" olarak düşünülebilir.

Bölümler, olayın gövdesini, olayı tanımlayan Kullanıcı tanımlı özellik paketini ve bölümdeki boşluğu, akış dizisindeki sayısını ve kabul edildiği hizmet tarafı zaman damgasını içeren olay verilerini tutar.

![Daha eski olay sırasını görüntüleyen diyagram.](./media/event-hubs-partitions/partition.png)

Event Hubs, büyük hacimden olayların işlenmesine yardımcı olmak üzere tasarlanmıştır ve bölümleme iki şekilde bu şekilde yardımcı olur:

İlk olarak, Event Hubs PaaS hizmeti olsa da, altında fiziksel bir gerçeklik vardır ve olayların sırasını koruyan bir günlüğün tutulması, bu olayların temel depolama ve çoğaltmalarda bir araya gelse ve bu tür bir günlük için üretilen iş üst sınırına neden olur. Bölümlendirme, aynı Olay Hub 'ı için birden çok paralel günlüklerin kullanılmasına izin verir ve bu nedenle kullanılabilir ham GÇ üretilen iş kapasitesini çarpar.

İkincisi, kendi uygulamalarınız bir olay hub 'ına gönderilen olayların hacminin işlenmesine devam edebilmelidir. Karmaşık olabilir ve önemli, ölçeği genişletilmiş ve paralel işleme kapasitesi gerektirir. Bölümler için ationale, yukarıdaki gibi aynıdır: olayları işlemek için tek bir işlemin kapasitesi sınırlıdır ve bu nedenle birkaç işlem yapmanız gerekir ve bölümler, her bir olayın açık bir işleme sahibine sahip olmasını sağlar. 

Event Hubs, tüm bölümler için geçerli olan yapılandırılmış bir saklama süresi için olayları tutar. Saklama süresine ulaşıldığında olaylar otomatik olarak kaldırılır. Bir günün bekletme dönemini belirtirseniz olay, kabul edildikten sonra tam 24 saat kullanılamaz hale gelir. Olayları açıkça silemezsiniz. 

İzin verilen saklama süresi, Event Hubs standart için 7 güne ve Event Hubs Ayrılmış için 90 güne kadar olur. Olayları, izin verilen saklama süresi ötesinde arşivlemeniz gerekiyorsa [Azure Storage 'da otomatik olarak depolanabilir veya Event Hubs yakalama özelliğini açıp Azure Data Lake](../articles/event-hubs/event-hubs-capture-overview.md), bu tür derinlemesine arşivleri aramanız veya analiz etmeniz gerekiyorsa, [bunları kolayca Azure SYNAPSE](../articles/event-hubs/store-captured-data-data-warehouse.md) veya diğer benzer depolar ve analiz platformları içine aktarabilirsiniz. 

Veri saklama sırasında Event Hubs ' sınırının nedeni, zaman damgası tarafından dizine alınmış ve yalnızca sıralı erişime izin veren derin bir depoda, geçmiş müşteri verilerinin büyük hacimlerini önlemektir. Burada, geçmiş verilerin daha zengin dizine sahip olması ve Event Hubs veya Kafka sağladığı gerçek zamanlı olay arabiriminden daha doğrudan erişim sağlaması gerekir. Olay akışı motorları, olay kaynağını belirlemek için veri klarının veya uzun vadeli arşivlerin rolünü yürütmeye uygun değildir. 

Bölümler birbirinden bağımsız olup kendi veri dizisini içerdiğinden genellikle farklı hızlarda büyürler. Event Hubs, örneğin, Apache Kafka gibi yönetim müdahalesini gerektiren bir sorun yoktur, ancak düzensiz dağıtım, aşağı akış olay işlemcilerinde düzensiz yüke neden olur.

![Event Hubs](./media/event-hubs-partitions/multiple-partitions.png)

Bölüm sayısı, oluşturma sırasında belirtilmiştir ve Event Hubs standardında 1 ile 32 arasında olmalıdır. Bölüm sayısı Event Hubs Ayrılmış Kapasite birimi başına en fazla 2000 bölüm olabilir. 

Bu belirli olay hub 'ı için uygulamanızın en yoğun yükü sırasında sürekli [üretilen iş birimleri (tu)](../articles/event-hubs/event-hubs-faq.md#what-are-event-hubs-throughput-units) için gereken en az sayıda bölüm seçmenizi öneririz. 1 TU işleme kapasitesine sahip tek bir bölümle hesaplamanız gerekir (1 Mbde, 2 Mbtu çıkış). Ad alanınızı veya kümenizin kapasite birimlerini bölüm sayısından bağımsız olarak ölçeklendirebilirsiniz. 32 bölümden oluşan bir olay hub 'ı veya 1 bölümlü bir olay hub 'ı, ad alanı 1 TU kapasitesine ayarlandığında tam olarak aynı maliyette olur. 

Uygulamalar, olayların üç farklı şekilde bölümlere eşlenmesini denetler:

- Sürekli olarak eşlenmiş (bir karma işlevi kullanılarak) bölüm anahtarını belirterek kullanılabilir bölümlerden birine göre. 
- Aracı, belirli bir olay için rastgele bir bölüm seçmek üzere bir bölüm anahtarı belirtmemelidir.
- Olayları belirli bir bölüme açıkça göndererek.

Bir bölüm anahtarı belirtilmesi, ilgili olayları aynı bölümde ve gönderildikleri kesin sırayla birlikte tutmaya izin vermez. Bölüm anahtarı, uygulama bağlamınızdan türetilen ve olayların ilişkilerini tanımlayan bir dizedir.

Bölüm anahtarı tarafından tanımlanan olaylar dizisi bir *akıştır*. Bölüm, birçok sayıda akış için çoğullanmış bir günlük deposudur. 

[Ayrılmış bir Event Hubs kümesindeki](../articles/event-hubs/event-hubs-dedicated-overview.md) bir olay hub 'ının bölüm sayısı, Olay Hub 'ı oluşturulduktan sonra [artırılabilir](../articles/event-hubs/dynamically-add-partitions.md) , ancak bölümler arasında akış dağıtımı, Bölüm anahtarlarının bölüm değişikliklerinin eşlenmesiyle yapıldığında, bu değişikliklerden kaçınmaya devam etmeniz gerekir. bu nedenle, olayların göreli sırası uygulamanızda önemli olduğunda bu değişikliklere engel olmak için katı çalışmanız gerekir.

Bölüm sayısının izin verilen maksimum değere ayarlanması, ancak her zaman birden çok bölümden faydalanmak için olay akışlarınızın yapılandırılması gerektiğini aklınızda bulundurun. Tüm olaylarda veya yalnızca bir alt akışlarda mutlak sipariş koruması gerekiyorsa, birçok bölümden faydalanabilmeyebilirsiniz. Ayrıca, birçok bölüm işleme tarafını daha karmaşık hale getirir. 

Bölümler doğrudan gönderilebilirler, ancak önerilmez. Bunun yerine, [olay yayımcıları](../articles/event-hubs/event-hubs-features.md#event-publishers) bölümünde tanıtılan daha yüksek düzey yapıları kullanabilirsiniz. 

Bölümleri ve kullanılabilirlikleri ile güvenilirlikleri arasındaki dengeleme hakkında daha fazla bilgi için, bkz: [Event Hubs programlama kılavuzu](../articles/event-hubs/event-hubs-programming-guide.md#partition-key) ve [Event Hubs’ta kullanılabilirlik ve tutarlılık](../articles/event-hubs/event-hubs-availability-and-consistency.md) makalesi.
