---
title: Ağlar için Azure Monitörü (Önizleme)
description: Herhangi bir yapılandırma olmaksızın dağıtılan tüm ağ kaynakları için sistem durumu ve ölçümlerin kapsamlı bir görünümünü sağlayan Azure Monitor for Network için hızlı bir genel bakış.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/07/2019
ms.openlocfilehash: 0f5b2fbd13cb9658e255fde727e115df748aaed9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77654877"
---
# <a name="azure-monitor-for-networks-preview"></a>Ağlar için Azure Monitörü (Önizleme)
Ağ için Azure Monitor, herhangi bir yapılandırma olmaksızın dağıtılan tüm ağ kaynakları için sistem durumu ve ölçümlerin kapsamlı bir görünümünü sağlar. Gelişmiş arama özelliği, yalnızca barındırılan web sitesi adını arayarak web sitenize ev sahipliği yapan kaynakları tanımlama gibi senaryoları etkinleştirerek kaynak bağımlılıklarını belirlemeye yardımcı olur.

Ağlar için Azure Monitörü **Genel Bakış** sayfası, kaynak durumu ve uyarılarla birlikte ağ kaynaklarınızın envanterini görselleştirmek için zahmetsiz bir yol sağlar. Dört temel işlevsel alana ayrılır:

- Arama ve filtreleme
- Kaynak Sağlığı ve Ölçümleri
- Uyarılar 
- Bağımlılık görünümü

![Genel bakış sayfası](media/network-insights-overview/overview.png)

## <a name="search-and-filtering"></a>Arama ve filtreleme
Kaynak durumu ve uyarı görünümü **Abonelik,** **Kaynak Grubu** ve **Kaynak Türü**gibi filtreler kullanılarak özelleştirilebilir. Arama kutusu kaynak özellikleri arasında arama olanağı sağlar.

Arama kutusu kaynakları ve ilişkili kaynakları aramak için kullanılabilir. Örneğin, Ortak IP bir Uygulama Ağ Geçidi ile ilişkilidir. Ortak IP'ler DNS adının aranması hem Genel IP'yi hem de ilişkili Uygulama Ağ Geçidi'ni tanımlar.

![Search](media/network-insights-overview/search.png)


## <a name="resource-health-and-metric"></a>Kaynak Sağlığı ve Metrik
Her döşeme, kaynak durumu durumuyla birlikte seçilen tüm abonelikler arasında dağıtılan örnek sayısıyla birlikte bir kaynak türünü temsil eder. Aşağıdaki örnekte, 105 ER ve VPN Bağlantısı dağıtılır, 103'u sağlıklı ve 2'si kullanılamaz.

![Kaynak durumu](media/network-insights-overview/resource-health.png)

Kullanılamaz iki ER ve VPN bağlantısına tıkladığınızda, metrik bir görünüm başlatılır. 

![Metrik görünüm](media/network-insights-overview/metric-view.png)

Izgara görünümündeki her öğeyi tıklatabilirsiniz. Bu bağlantı için kaynak durumu yönlendirmeiçin Sistem Durumu simgesine tıklayın. Bu bağlantı için sırasıyla uyarılara ve ölçümler sayfasına yönlendirmek için Uyarılar'ı tıklatın. 

## <a name="alerts"></a>Uyarılar
Sağdaki **Uyarılar** ızgarası, tüm abonelikler arasında seçili kaynaklar için oluşturulan tüm uyarıların görünümünü sağlar. Ayrıntılı uyarılar sayfasına gitmek için uyarı sayımlarını tıklatın.

## <a name="dependency-view"></a>Bağımlılık görünümü
**Bağımlılık** görünümü, kaynağın nasıl yapılandırıldığı görselleştirmeye yardımcı olur. Şu anda bağımlılık görünümü yalnızca Uygulama Ağ Geçidi için desteklenir. Bağımlılık görünümüne, ölçümler ızgara görünümünden Uygulama Ağ Geçidi kaynak adını tıklayarak erişilebilir.

![Uygulama Ağ Geçidi görünümü](media/network-insights-overview/application-gateway.png)

Uygulama Ağ Geçidi için **Bağımlılık** görünümü, ön uç IP'lerin dinleyicilere, kurallara ve arka uç havuzuna nasıl bağlandığını basitleştirilmiş bir görünüm sağlar. Bağlantı kenarları renk kodludur ve arka uç havuzu durumuna göre ek ayrıntılar sağlar. Görünüm ayrıca, VMSS ve VM örnekleri gibi ilgili tüm arka uç havuzları için Uygulama Ağ Geçidi ölçümlerinin ve ölçümlerinin ayrıntılı bir görünümünü de sağlar.

![Bağımlılık görünümü](media/network-insights-overview/dependency-view.png)

Bağımlılık grafiği yapılandırma ayarlarına kolay gezinme sağlar. Diğer işlevlere erişmek için arka uç havuzuna sağ tıklayın. Örneğin, arka uç havuzu bir VM ise, bağlantı sorunlarını belirlemek için Doğrudan VM Öngörüleri ve Ağ İzleyicisi bağlantı sorun giderme sorununa erişebilirsiniz.

![Bağımlılık görünümü menüsü](media/network-insights-overview/dependency-view-menu.png)

Bağımlılık görünümündeki arama ve filtre çubuğu, grafikte arama yapmak için zahmetsiz bir yol sağlar. Örneğin, aşağıdaki örnekte *AppGWTestRule'i* aramak, grafik görünümünü *AppGWTestRule*ile bağlanan tüm düğümlere daraltır. 

![Arama örneği](media/network-insights-overview/search-example.png)

Farklı filtreler, belirli bir yol ve duruma daraltmak için yardım sağlar. Örneğin, durumun *sağlıksız*olduğu tüm kenarları göstermek için **yalnızca Sağlık Durumu'ndan** *Sağlıksız'ı* seçin.

Uygulama ağ geçidi, tüm arka uç havuzu kaynakları ve ön uç IP'leri için ayrıntılı ölçümler içeren önceden yapılandırılmış bir çalışma kitabı başlatmak için **Ayrıntılı Metrik Görünümü'ne** tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar 

- [Azure Ağ İzleyicisi nedir?](/azure/network-watcher/network-watcher-monitoring-overview)
