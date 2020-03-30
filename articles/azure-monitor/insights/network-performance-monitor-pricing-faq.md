---
title: Azure Ağ Performans Monitörü için Fiyatlandırma SSS | Microsoft Dokümanlar
description: Sık sorulan sorular - Azure Ağ Performans Monitörü
ms.subservice: logs
ms.topic: conceptual
author: agummadi
ms.author: agummadi
ms.date: 04/02/2018
ms.openlocfilehash: c5a80c7ff6d08ce601a6c2bbb0db0ef866d3b425
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654401"
---
# <a name="pricing-changes-for-azure-network-performance-monitor"></a>Azure Ağ Performans Monitörü için fiyatlandırma değişiklikleri

Geri bildiriminizi dinledik ve yakın zamanda Azure genelindeki çeşitli izleme hizmetleri için yeni bir [fiyatlandırma deneyimi](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/) sunduk. Bu makalede, Azure [Ağ Performans İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview) (NPM) ile ilgili fiyatlandırma değişiklikleri kolay okunan bir soru ve yanıt biçiminde yakalanmaktadır.

Ağ Performans Monitörü üç bileşenden oluşur:
* [Performans İzleyicisi](https://docs.microsoft.com/azure/networking/network-monitoring-overview#performance-monitor)
* [Servis Uç Noktası Monitörü](https://docs.microsoft.com/azure/networking/network-monitoring-overview)
* [ExpressRoute Monitör](https://docs.microsoft.com/azure/networking/network-monitoring-overview#expressroute-monitor)

Aşağıdaki bölümlerde NPM bileşenleriiçin fiyatlandırma değişiklikleri açıklayınız.

## <a name="performance-monitor"></a>Performans İzleyicisi

**Performans Monitörü'nün kullanımı eski modelde nasıl faturalandırıldı?**

NPM için faturalandırma, iki bileşenin kullanımına ve tüketimine dayanıyordu:
* **Düğümler**: Tüm sentetik işlemler düğümlerden kaynaklanır ve sonlandırır. Düğümler aracılar veya Microsoft Yönetim Aracıları olarak da adlandırılır.
* **Veri**: Çeşitli ağ testlerinin sonuçları Log Analytics çalışma alanında depolanır.

Eski modelde, fatura düğüm sayısına ve oluşturulan veri hacmine göre hesaplandı. 

**Performans Monitörü'nün kullanımı yeni model de nasıl ücretlendirilir?**

NPM'deki Performans İzleyicisi özelliği artık aşağıdakilerin bir kombinasyonuna göre faturalandırılır: 

* Alt ağ bağlantıları izlenir
* Veri hacmi

**Alt ağ bağlantısı nedir?**

Performans İzleyicisi ağdaki iki veya daha fazla konum arasındaki bağlantıyı izler. Bir alt ağdaki bir düğüm grubu veya aracı lar ile başka bir alt ağdaki düğüm ler grubu arasındaki bağlantıya alt ağ bağlantısı denir.

**İki alt ağ (A ve B) var ve her alt ağda birkaç aracım var. Performans Monitörü, A alt ağıüzerindeki tüm aracılardan B altindeki tüm aracılara bağlantı izler. Alt ağ bağlantısı sayısına göre ücretlendirilir miyim?**

Hayır. Faturalandırma amacıyla, A altağı B altağına kadar tüm bağlantılar tek bir alt net bağlantısında gruplandırılır. Tek bir bağlantı için faturalandırılırsınız. Performans İzleyicisi, her alt ağdaki çeşitli aracılar arasındaki bağlantıyı izlemeye devam ediyor.

**Bir alt net bağlantısını izlemenin maliyeti nedir?**

Tüm ay boyunca tek bir alt ağ bağlantısını izleme maliyeti için [Ping Mesh](https://azure.microsoft.com/pricing/details/network-watcher/) bölümüne bakın.

**Performans İzleyicisi'nin oluşturduğu veriler için ücretler nelerdir?**

Yutma ücreti (Azure Monitor'da Log Analytics çalışma alanına veri yükleme, işleme ve dizin oluşturma) Veri Alma bölümünde, Log Analytics için [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/log-analytics/) kullanılabilir. Veri saklama ücreti (diğer bir deyişle, ilk aydan sonra müşterinin tercihinde tutulan veriler) [fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/log-analytics/), Veri Saklama bölümünde de kullanılabilir.


## <a name="expressroute-monitor"></a>ExpressRoute Monitör

**ExpressRoute Monitor kullanımı için ücretler nelerdir?**

ExpressRoute Monitor ücretleri, izleme sırasında oluşturulan veri hacmine göre faturalandırılır. Daha fazla bilgi için bkz: "Performans İzleyicisi'nin oluşturduğu veriler için ücretler nelerdir?"

**Birden fazla ExpressRoute devresini izlemek için ExpressRoute Monitor'u kullanıyorum. İzlenen devre sayısına göre ücretlendirildim mi?**

Devre sayısına veya eşleme türüne (örneğin, özel akran, Microsoft eşleme) bağlı olarak ücretlendirilmezsiniz. Daha önce açıklandığı gibi, veri hacmine bağlı olarak ücretlendirilirsiniz.

**ExpressRoute tek bir devreyi izlediğinde oluşturulan veri hacmi nedir?**

ExpressRoute özel bir bakan bağlantı izlediğinde ayda oluşturulan veri hacmi aşağıdaki gibidir:

|Yüzdebirlik      |Veri/ay (MB)|
| :---:          |           ---:|
|50<sup>inci</sup> |            192|
|60<sup>inci</sup> |            256|
|70<sup>inci</sup> |            360|
|80<sup>inci</sup> |            498|
|90<sup>inci</sup> |            870|
|95<sup>inci</sup> |           1560|


Bu tabloya göre, yüzde 50'lik müşteriler 192 MB veri için ödeme yapmaktadır. İlk ay için $2.30/GB seviyesinde, bir devreyi izlemek için yapılan maliyet $0.43 'dir (= 192 * 2.30 / 1024).

**Veri hacmindeki değişimlerin bazı nedenleri nelerdir?**

Oluşturulan izleme verilerinin hacmi aşağıdakiler gibi çeşitli etkenlere bağlıdır:
* Ajan sayısı. Arıza yalıtımının doğruluğu, ajan sayısındaki artışla artar.
* Ağdaki atlama sayısı.
* Kaynak ve hedef arasındaki yol sayısı.

Daha yüksek yüzdelik yüzdelik müşteriler (önceki tabloda) genellikle kendi şirket içi ağ üzerinde birkaç bakış noktalarından devreleri izlemek. Birden çok aracı da ağdaha derin yerleştirilir, daha uzak servis sağlayıcı kenar yönlendirici. Aracılar genellikle veri merkezlerinde çeşitli kullanıcı sitelerine, şubelere ve raflara yerleştirilir.

## <a name="service-endpoint-monitor"></a>Servis Uç Noktası Monitörü

**Service Endpoint Monitor kullanımı için ücretler nelerdir?**

Service Endpoint Monitor'un kullanım ücretleri şu temele göre hesaplanır:
* Bağlantı sayısı
* Veri hacmi

**Bağlantı nedir?**

Bağlantı, tüm ay boyunca tek bir aracıdan tek bir uç noktaya (URL veya ağ hizmeti) erişilebilme testidir. Örneğin, üç aracıdan bing.com bağlantı izlemek üç bağlantı oluşturur.

**Service Endpoint Monitor'un maliyetleri nelerdir?**

Tüm ay için bir bitiş noktasını izleme maliyeti için [Bağlantı İzleme](https://azure.microsoft.com/pricing/details/network-watcher/) bölümüne bakın. Veri ücreti, Log Analytics'in [fiyatlandırma sayfasında,](https://azure.microsoft.com/pricing/details/log-analytics/) Veri Alımı bölümünde mevcuttur.

## <a name="references"></a>Başvurular

[Log Analytics Fiyatlandırma SSS](https://azure.microsoft.com/pricing/details/log-analytics/): SSS bölümünde ücretsiz katman, düğüm başına fiyatlandırma ve diğer fiyatlandırma ayrıntıları hakkında bilgiler bulunmaktadır.

