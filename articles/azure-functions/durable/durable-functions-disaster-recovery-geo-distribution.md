---
title: Olağanüstü durum kurtarma ve coğrafi dağıtım Azure Dayanıklı Fonksiyonlar
description: Dayanıklı Fonksiyonlar'da olağanüstü durum kurtarma ve coğrafi dağılım hakkında bilgi edinin.
author: MS-Santi
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 7951f216143bef0d48a6b751beff3f8f4316b9bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75433343"
---
# <a name="disaster-recovery-and-geo-distribution-in-azure-durable-functions"></a>Azure Dayanıklı İşlevlerde olağanüstü durum kurtarma ve coğrafi dağıtım

Dayanıklı İşlevler'de tüm durum Azure Depolama'da kalıcıdır. [Görev](durable-functions-task-hubs.md) hub'ı, düzenleme için kullanılan Azure Depolama kaynakları için mantıksal bir kapsayıcıdır. Orkestratör ve etkinlik işlevleri yalnızca aynı görev hub'ına ait olduklarında birbirleriyle etkileşimkurabilir.
Açıklanan senaryolar, kullanılabilirliği artırmak ve olağanüstü durum kurtarma etkinlikleri sırasında kapalı kalma süresini en aza indirmek için dağıtım seçenekleri önerir.

Azure Depolama'nın kullanımı tarafından yönlendirildikleri için bu senaryoların Etkin-Pasif yapılandırmalarına dayandığını fark etmek önemlidir. Bu desen, bir yedekleme (pasif) işlev uygulamasının farklı bir bölgeye dağıtılmasından oluşur. Trafik Yöneticisi kullanılabilirlik için birincil (etkin) fonksiyon uygulamasını izler. Birincil başarısız olursa yedekleme işlevi uygulaması üzerinde başarısız olur. Daha fazla bilgi için [Trafik Yöneticisi'nin](https://azure.microsoft.com/services/traffic-manager/) [Öncelikli Trafik Yönlendirme Yöntemi'ne bakın.](../../traffic-manager/traffic-manager-routing-methods.md#priority-traffic-routing-method)

>[!NOTE]
>
> - Önerilen Active-Passive yapılandırması, istemcinin HER zaman HTTP üzerinden yeni orkestrasyonları tetiklemesini sağlar. Ancak, aynı depolama alanını paylaşan iki işlevuygulamasının bir sonucu olarak, arka plan işleme her ikisi arasında dağıtılır ve aynı kuyruktaki iletiler için rekabet eder. Bu yapılandırma, ikincil işlev uygulaması için ek çıkış maliyetlerine neden olabilir.
> - Temel depolama hesabı ve görev hub'ı birincil bölgede oluşturulur ve her iki işlev uygulaması tarafından da paylaşılır.
> - Gereksiz bir şekilde dağıtılan tüm işlev uygulamaları, HTTP üzerinden etkinleştirilme durumunda aynı işlev erişim anahtarlarını paylaşmalıdır. Çalışma Zamanı İşlevleri, tüketicilerin işlev anahtarlarını programlı olarak eklemesini, silmesini ve güncelleştirmesini sağlayan bir [yönetim API'sini](https://github.com/Azure/azure-functions-host/wiki/Key-management-API) ortaya çıkarır.

## <a name="scenario-1---load-balanced-compute-with-shared-storage"></a>Senaryo 1 - Paylaşılan depolama ile dengeli bir işlem yükleme

Azure'daki işlem altyapısı başarısız olursa, işlev uygulaması kullanılamayabilir. Bu senaryo, bu tür kapalı kalma süresini en aza indirmek için farklı bölgelere dağıtılan iki işlev uygulaması kullanır.
Trafik Yöneticisi, birincil işlev uygulamasındaki sorunları algılamak ve trafiği otomatik olarak ikincil bölgedeki işlev uygulamasına yönlendirmek için yapılandırılır. Bu işlev uygulaması aynı Azure Depolama hesabını ve Görev Hub'ını paylaşır. Bu nedenle, işlev uygulamalarının durumu kaybolmaz ve çalışma normal şekilde devam edebilir. Sistem durumu birincil bölgeye geri yüklendikten sonra, Azure Trafik Yöneticisi istekleri otomatik olarak bu işlev uygulamasına yönlendirmeye başlar.

![Senaryo 1'i gösteren diyagram.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario01.png)

Bu dağıtım senaryosukullanırken çeşitli yararları vardır:

- İşlem altyapısı başarısız olursa, devlet kaybı olmadan bölge üzerinde başarısız işler devam edebilir.
- Trafik Yöneticisi otomatik olarak sağlıklı işlev uygulaması üzerinden otomatik başarısız ilgilenir.
- Trafik Yöneticisi, kesinti düzeltildikten sonra birincil işlev uygulamasına giden trafiği otomatik olarak yeniden kurar.

Ancak, bu senaryoyu kullanarak göz önünde bulundurun:

- İşlev uygulaması özel bir Uygulama Hizmeti planı kullanılarak dağıtılırsa, veri merkezi üzerinden başarısız olan bilgi işlem altyapısının çoğaltılması maliyetleri artırır.
- Bu senaryo, işlem altyapısındaki kesintileri kapsar, ancak depolama hesabı işlev Uygulaması için tek hata noktası olmaya devam eder. Depolama kesintisi varsa, uygulama nın bir kesintiden muzdarip olması.
- İşlev uygulaması başarısız olursa, bölgeler arasında depolama hesabına erişeceği için gecikme solunması artar.
- Depolama hizmetine, ağ çıkış trafiği nedeniyle daha yüksek maliyetle maruz kaldığı farklı bir bölgeden erişmek.
- Bu senaryo Trafik Yöneticisi'ne bağlıdır. Trafik [Yöneticisi'nin nasıl çalıştığını](../../traffic-manager/traffic-manager-how-it-works.md)göz önünde bulundurarak, Dayanıklı İşlev tüketen bir istemci uygulamasının Trafik Yöneticisi'nden işlev uygulaması adresini yeniden sorgulaması gerekebilir.

## <a name="scenario-2---load-balanced-compute-with-regional-storage"></a>Senaryo 2 - Bölgesel depolama ile dengeli hesaplama yı yükleyin

Önceki senaryo yalnızca işlem altyapısındaki hata durumunu kapsar. Depolama hizmeti başarısız olursa, işlev uygulamasının kesintisine neden olur.
Bu senaryo, dayanıklı işlevlerin sürekli çalışmasını sağlamak için, işlev uygulamalarının dağıtıldığı her bölgede yerel bir depolama hesabı kullanır.

![Senaryo 2'yi gösteren diyagram.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario02.png)

Bu yaklaşım, önceki senaryoda iyileştirmeler ekler:

- İşlev uygulaması başarısız olursa, Trafik Yöneticisi ikincil bölgeye geçememesine dikkat eder. Ancak, işlev uygulaması kendi depolama hesabına dayandığından, dayanıklı işlevler çalışmaya devam eder.
- Bir başarısızlık sırasında, işlev uygulaması ve depolama hesabı birlikte bulunduğundan, bölge üzerinde başarısız olduğunda ek bir gecikme süresi yoktur.
- Depolama katmanının arızalanması, dayanıklı işlevlerde hatalara neden olur ve bu da bölge üzerinde başarısızlığa yeniden yönlendirmeyi tetikler. Yine, işlev uygulaması ve depolama bölgeye göre izole olduğundan, dayanıklı işlevler çalışmaya devam edecektir.

Bu senaryo için önemli noktalar:

- İşlev uygulaması özel bir AppService planı kullanılarak dağıtılırsa, veri merkezi üzerinden başarısız olan bilgi işlem altyapısının çoğaltılması maliyetleri artırır.
- Geçerli durum üzerinde başarısız değil, bu da yürütme ve denetlenen işlevlerin başarısız olacağı anlamına gelir. Çalışmayı yeniden denemek/yeniden başlatmak istemci uygulamasına kalmış.

## <a name="scenario-3---load-balanced-compute-with-grs-shared-storage"></a>Senaryo 3 - GRS paylaşılan depolama ile dengeli bir işlem yük

Bu senaryo, paylaşılan bir depolama hesabı uygulayarak ilk senaryo üzerinde bir değişikliktir. Depolama hesabının coğrafi çoğaltma etkinken oluşturulduğu temel fark.
İşlevsel olarak, bu senaryo Senaryo 1 ile aynı avantajları sağlar, ancak ek veri kurtarma avantajları sağlar:

- Coğrafi yedekli depolama (GRS) ve Okuma erişimi GRS (RA-GRS) depolama hesabınız için kullanılabilirliği en üst düzeye çıkarır.
- Depolama hizmetinde bir bölge kesintisi varsa, veri merkezi işlemlerinin depolamanın ikincil bölgeye aktarılmasını belirleme olasılığından biri. Bu durumda, depolama hesabı erişimi, kullanıcı müdahalesi olmaksızın, depolama hesabının coğrafi olarak çoğaltılan kopyasına saydam olarak yönlendirilir.
- Bu durumda, dayanıklı işlevlerin durumu, birkaç dakikada bir oluşan depolama hesabının son çoğaltmasına kadar korunur.

Diğer senaryolarda olduğu gibi, önemli noktalar şunlardır:

- Çoğaltma üzerinde fail veri merkezi operatörleri tarafından yapılır ve biraz zaman alabilir. O zamana kadar, işlev uygulaması bir kesinti yaşayacaktır.
- Coğrafi olarak çoğaltılan depolama hesaplarını kullanmak için daha yüksek bir maliyet vardır.
- GRS eşzamanlı olarak oluşur. Çoğaltma işleminin gecikmesi nedeniyle en son işlemlerden bazıları kaybolabilir.

![Senaryo 3'ün çizilen diyagramı.](./media/durable-functions-disaster-recovery-geo-distribution/durable-functions-geo-scenario03.png)

## <a name="next-steps"></a>Sonraki adımlar

[RA-GRS kullanarak Yüksek Kullanılabilir Uygulamalar Tasarlama](../../storage/common/storage-designing-ha-apps-with-ragrs.md) hakkında daha fazla bilgi edinebilirsiniz
