---
title: Bağlı Fabrika topolojisini yapılandırın - Azure | Microsoft Dokümanlar
description: Bu makalede, topolojisi de dahil olmak üzere Bağlı Fabrika çözüm hızlandırıcısının nasıl yapılandırılabildiğini açıklanmaktadır.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 5fa3d4d4fdfa0dd81cd8ab8772ffb3903dda289f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820127"
---
# <a name="configure-the-connected-factory-solution-accelerator"></a>Bağlı Fabrika çözüm hızlandırıcısını yapılandırın

Connected Factory çözüm hızlandırıcısı, kurgusal bir şirket Olan Contoso için simüle edilmiş bir pano gösterir. Bu şirketin dünya çapında çok sayıda küresel yerlerde fabrikaları vardır.

Bu makalede, Bağlı Fabrika çözümünün topolojisinin nasıl yapılandırılabildiğini açıklamak için Contoso bir örnek olarak kullanılmıştır.

## <a name="simulated-factories-configuration"></a>Simüle edilmiş fabrikalar yapılandırması

Her Contoso fabrikası, her biri üç istasyondan oluşan üretim hatlarına sahiptir. Her istasyon belirli bir rolü olan gerçek bir OPC UA sunucusudur:

* Montaj istasyonu
* Test istasyonu
* Paketleme istasyonu

Bu OPC UA sunucularında OPC UA düğümleri vardır ve [OPC Publisher](overview-opc-publisher.md) bu düğümlerin değerlerini Bağlı Fabrika'ya gönderir. Buna aşağıdakiler dahildir:

* Mevcut güç tüketimi gibi geçerli çalışma durumu.
* Üretilen ürün sayısı gibi üretim bilgileri.

Küresel bir görünümden istasyon düzeyinde görünüme kadar Contoso fabrika topolojisini delmek için gösterge panelini kullanabilirsiniz. Bağlı Fabrika panosu şunları sağlar:

* Topolojideki her katman için OEE ve KPI figürlerinin görselleştirilmesi.
* İstasyonlarda OPC UA düğümlerinin geçerli değerlerinin görselleştirilmesi.
* OEE ve KPI rakamlarının istasyon seviyesinden küresel düzeye toplanması.
* Değerler belirli eşiklere ulaşıyorsa gerçekleştirecek uyarıların ve eylemlerin görselleştirilmesi.

## <a name="connected-factory-topology"></a>Bağlı Fabrika topolojisi

Fabrikaların, üretim hatlarının ve istasyonların topolojisi hiyerarşiktir:

* Küresel düzeyde çocuk olarak fabrika düğümleri vardır.
* Fabrikalarda çocukken üretim hattı düğümleri vardır.
* Üretim hatlarında çocukken istasyon düğümleri vardır.
* İstasyonlarda (OPC UA sunucuları) çocuk olarak OPC UA düğümleri vardır.

Topolojideki her düğüm, aşağıdakileri tanımlayan ortak bir özellik kümesine sahiptir:

* Topoloji düğümü için benzersiz bir tanımlayıcı.
* Bir isim.
* Bir tanımlama.
* Bir görüntü.
* Topoloji düğümünün çocukları.
* OEE ve KPI rakamları ve yürütülecek uyarı eylemleri için minimum, hedef ve maksimum değerler.

## <a name="topology-configuration-file"></a>Topoloji yapılandırma dosyası

Önceki bölümde listelenen özellikleri yapılandırmak için, Bağlı Fabrika çözümü [ContosoTopologyDescription.json](https://github.com/Azure/azure-iot-connected-factory/blob/master/WebApp/Contoso/Topology/ContosoTopologyDescription.json)adlı bir yapılandırma dosyası kullanır.

Bu dosyayı klasördeki çözüm kaynak `WebApp/Contoso/Topology` kodunda bulabilirsiniz.

Aşağıdaki snippet yapılandırma dosyasının `ContosoTopologyDescription.json` bir anahat gösterir:

```json
{
  <global_configuration>,
  "Factories": [
    <factory_configuration>,
    "ProductionLines": [
      <production_line_configuration>,
      "Stations": [
        <station_configuration>,
        <more station_configurations>
      ],
      <more production_line_configurations>
    ]
    <more factory_configurations>
  ]
}
```

 `<global_configuration>`, , , `<factory_configuration>` `<production_line_configuration>`ve `<station_configuration>` ortak özellikleri şunlardır:

* **Ad** (tür dizesi)

  Panoda gösterilecek topoloji düğümü için yalnızca bir sözcük olması gereken açıklayıcı bir ad tanımlar.

* **Açıklama** (tür dizesi)

  Topoloji düğümlerini daha ayrıntılı olarak açıklar.

* **Resim** (yazı dizesi)

  Panoda topoloji düğümü hakkındaki bilgilerin gösterildiğinde WebApp çözümündeki bir görüntüye giden yol.

* **OeeGenel**, **OeePerformance**, **OeeAvailability**, **OeeQuality**, **Kpi1**, **Kpi2** (türü `<performance_definition>`)

  Bu özellikler, uyarılar oluşturmak için kullanılan operasyonel şeklin en az, hedef ve maksimal değerlerini tanımlar. Bu özellikler, bir uyarı algılanırsa yürütülecek eylemleri de tanımlar.

Ve `<factory_configuration>` `<production_line_configuration>` öğelerin bir özelliği vardır:

* **Kılavuz** (tip dize)

  Topoloji düğümlerini benzersiz bir şekilde tanımlar.

`<factory_configuration>`bir özelliği vardır:

* **Konum** (tür) `<location_definition>`

  Fabrikanın nerede olduğunu belirtir.

`<station_configuration>`özellikleri vardır:

* **OpcUri** (tip dize)

  Bu özellik, OPC UA sunucusunun OPC UA Uygulama URI olarak ayarlanmalıdır.
  OPC UA belirtimi tarafından küresel olarak benzersiz olması gerektiğinden, bu özellik istasyon topoloji düğümlerini tanımlamak için kullanılır.

* **OpcNodes**, OPC UA düğümleri bir dizi `<opc_node_description>`olan (türü)

`<location_definition>`özellikleri vardır:

* **Şehir** (tip dize)

  Konuma en yakın şehrin adı

* **Ülke** (tip dizesi)

  Konumun ülkesi

* **Enlem** (çift tip)

  Konumun enlemi

* **Boylam** (tip çift)

  Konumun boylam

`<performance_definition>`özellikleri vardır:

* **Minimum** (tip çift)

  Değerin ulaşabileceği daha düşük eşik. Geçerli değer bu eşiğin altındaysa, bir uyarı oluşturulur.

* **Hedef** (tip çift)

  İdeal hedef değeri.

* **Maksimum** (tip çift)

  Değerin ulaşabileceği üst eşik. Geçerli değer bu eşiğin üzerindeyse, bir uyarı oluşturulur.

* **MinimumAlertActions** (türü) `<alert_action>`

  Minimum uyarıya yanıt olarak alınabilecek eylem kümesini tanımlar.

* **MaximumAlertActions** (türü) `<alert_action>`

  Maksimum uyarıya yanıt olarak alınabilecek eylem kümesini tanımlar.

`<alert_action`> özellikleri vardır:

* **Türü** (tür dize)

  Uyarı eyleminin türü. Aşağıdaki türleri bilinmektedir:

  * **AcknowledgeAlert**: uyarının durumu kabul edilemeyecek şekilde değiştirilmelidir.
  * **CloseAlert**: Aynı türden tüm eski uyarılar artık panoda gösterilmemelidir.
  * **CallOpcMethod**: OPC UA yöntemi çağrılmalıdır.
  * **OpenWebPage**: ek bağlamsal bilgiler gösteren bir tarayıcı penceresi açılmalıdır.

* **Açıklama** (tür dizesi)

  Panoda gösterilen eylemin açıklaması.

* **Parametre** (tip dize)

  Eylemi yürütmek için gereken parametreler. Değer eylem türüne bağlıdır.

  * **AcknowledgeAlert**: parametre gerektirmez.
  * **CloseAlert**: parametre gerektirmez.
  * **CallOpcMethod**: Düğüm bilgileri ve OPC UA yönteminin parametreleri biçiminde aramak için "Ana düğümün Düğümü, NodeId metod, Uri OPC UA sunucusunun."
  * **OpenWebPage**: tarayıcı penceresinde gösterilen URL.

`<opc_node_description>`bir istasyondaki OPC UA düğümleri (OPC UA sunucusu) hakkında bilgi içerir. Varolan Hiçbir OPC UA düğümlerini temsil eden, ancak Bağlı Fabrika'nın hesaplama mantığında depolama olarak kullanılan düğümler de geçerlidir. Aşağıdaki özelliklere sahiptir:

* **Düğüm (tip** dizesi)

  İstasyonun (OPC UA sunucusunun) adres alanında ki OPC UA düğümünün adresi. Sözdizimi, Bir Düğüm için OPC UA belirtiminde belirtildiği gibi olmalıdır.

* **SymbolicName** (tip dizesi)

  Bu OPC UA düğümünün değeri gösterildiğinde panoda gösterilecek ad.

* **Alaka düzeyi** (tür dize dizisi)

  OEE veya KPI'nın hangi hesaplamaiçin OPC UA düğümü değerinin alakalı olduğunu gösterir. Her dizi öğesi aşağıdaki değerlerden biri olabilir:

  * **OeeAvailability_Running**: değer OEE Kullanılabilirliğinin hesaplanması için önemlidir.
  * **OeeAvailability_Fault**: değer OEE Kullanılabilirliğinin hesaplanması için önemlidir.
  * **OeePerformance_Ideal**: değer OEE Performansının hesaplanması için önemlidir ve genellikle sabit bir değerdir.
  * **OeePerformance_Actual**: değer OEE Performansının hesaplanması için önemlidir.
  * **OeeQuality_Good**: değer OEE Kalitesinin hesaplanması için önemlidir.
  * **OeeQuality_Bad**: değer OEE Kalitesinin hesaplanması için önemlidir.
  * **Kpi1**: değer KPI1 hesaplanması için ilgilidir.
  * **Kpi2**: değer KPI2 hesaplaması için ilgilidir.

* **OpCode** (tip dize)

  Zaman Serisi Öngörü sorgularında ve OEE/KPI hesaplamalarında OPC UA düğümünün değerinin nasıl işlendiğini gösterir. Her Zaman Serisi Öngörü sorgusu, sorgunun bir parametresi olan belirli bir zaman dilimini hedefler ve bir sonuç verir. OpCode, sonucun nasıl hesaplandığını denetler ve aşağıdaki değerlerden biri olabilir:

  * **Diff**: zaman bölmesindeki son ve ilk değer arasındaki fark.
  * **Avg**: zaman açısından tüm değerlerin ortalaması.
  * **Toplam**: zaman bölmesindeki tüm değerlerin toplamı.
  * **Son**: şu anda kullanılmaz.
  * **Count**: zaman bölmesindeki değer sayısı.
  * **Max**: zaman açısından maksimal değer.
  * **Min**: zaman açısından en az değer.
  * **Const**: sonuç özellik ConstValue tarafından belirtilen değerdir.
  * **SubMaxMin**: maksimal ve minimal değer arasındaki fark.
  * **Zaman gazetesi**: zaman gazetesi.

* **Birimler** (tip dize)

  Panoda görüntülenecek değerin bir birimini tanımlar.

* **Görünür** (tip boolean)

  Değerin panoda gösterilip gösterilmediğini denetler.

* **ConstValue** (tip çift)

  **OpCode** **Const**ise, bu özellik düğümün değeridir.

* **Minimum** (tip çift)

  Geçerli değer bu değerin altına düşerse, en az uyarı oluşturulur.

* **Maksimum** (tip çift)

  Geçerli değer bu değerin üzerinde yükselirse, en büyük uyarı oluşturulur.

* **MinimumAlertActions** (türü) `<alert_action>`

  Minimum uyarıya yanıt olarak alınabilecek eylem kümesini tanımlar.

* **MaximumAlertActions** (türü) `<alert_action>`

  Maksimum uyarıya yanıt olarak alınabilecek eylem kümesini tanımlar.

İstasyon düzeyinde, **Simülasyon** nesnelerini de görürsünüz. Bu nesneler yalnızca Bağlı Fabrika simülasyonu yapılandırmak için kullanılır ve gerçek bir topolojiyi yapılandırmak için kullanılmamalıdır.

## <a name="how-the-configuration-data-is-used-at-runtime"></a>Çalışma zamanında yapılandırma verilerinin nasıl kullanıldığı

Yapılandırma dosyasında kullanılan tüm özellikler, nasıl kullanıldıklarına bağlı olarak farklı kategorilerde gruplandırılabilir. Bu kategoriler şunlardır:

### <a name="visual-appearance"></a>Görsel görünüm

Bu kategorideki özellikler Bağlı Fabrika panosunun görsel görünümünü tanımlar. Örneklere şunlar dahildir:

* Adı
* Açıklama
* Görüntü
* Konum
* Birimler
* Görünür

### <a name="internal-topology-tree-addressing"></a>İç topoloji ağacı adresleme

WebApp, tüm topoloji düğümlerinin bilgilerini içeren bir dahili veri sözlüğü tutar. Özellikleri **Guid** ve **OpcUri** bu sözlük erişmek için anahtar olarak kullanılır ve benzersiz olması gerekir.

### <a name="oeekpi-computation"></a>OEE/KPI hesaplama

Bağlı Fabrika simülasyonu için OEE/KPI rakamları aşağıdakiler tarafından parametrelendirilmiştir:

* Hesaplamaya dahil edilecek OPC UA düğümü değerleri.
* Şeklin telemetri değerlerinden nasıl hesaplanır.

Bağlı Fabrika tarafından yayınlanan OEE formülleri [http://www.oeefoundation.org](http://www.oeefoundation.org)kullanır.

İstasyonlarda bulunan OPC UA düğümü nesneleri, OEE/KPI hesaplamasında kullanılmak için etiketlemeyi sağlar. **Alaka** özelliği, OEE/KPI figürü için OPC UA düğümü değerinin kullanılması gerektiğini gösterir. **OpCode** özelliği, değerin hesaplamaya nasıl dahil edildiğini tanımlar.

### <a name="alert-handling"></a>Uyarı işleme

Connected Factory basit bir minimum/maksimum eşik tabanlı uyarı oluşturma mekanizmasını destekler. Bu uyarılara yanıt olarak yapılandırabileceğiniz önceden tanımlanmış eylemler vardır. Aşağıdaki özellikler bu mekanizmayı denetler:

* Maksimum
* Minimum
* MaximumAlertActions
* MinimumAlertActions

## <a name="correlating-to-telemetry-data"></a>Telemetri verileriyle ilişkili

Son değeri görselleştirme veya Time Series Insight sorguları oluşturma gibi belirli işlemler için WebApp'ın yutulan telemetri verileri için bir adresleme şemasına ihtiyacı vardır. Bağlı Fabrika'ya gönderilen telemetrinin iç veri yapılarında da depolanması gerekir. Bu işlemleri etkinleştiren iki özellik istasyonda (OPC UA sunucusu) ve OPC UA düğümü düzeyindedir:

* **OpcUri**

  Telemetrinin geldiği OPC UA sunucusunu tanımlar (genel olarak benzersizdir). Yutulan iletilerde, bu özellik **ApplicationUri**olarak gönderilir.

* **Düğüm**

  OPC UA sunucusundaki düğüm değerini tanımlar. Özelliğin biçimi, OPC UA belirtiminde belirtildiği gibi olmalıdır. Yutulan iletilerde, bu özellik **Düğüm**olarak gönderilir.

Telemetri verilerinin Bağlı Fabrika'ya nasıl yutulduşu hakkında daha fazla bilgi için [OPC Publisher nedir](overview-opc-publisher.md) bilgisini görün.

## <a name="example-how-kpi1-is-calculated"></a>Örnek: KPI1 nasıl hesaplanır?

Dosyadaki yapılandırma, OEE/KPI rakamlarının `ContosoTopologyDescription.json` nasıl hesaplanır denetler. Aşağıdaki örnek, bu dosyadaki özelliklerin KPI1 hesaplamasını nasıl denetletolduğunu gösterir.

Bağlı Fabrika Da KPI1 son bir saat içinde başarıyla üretilen ürünlerin sayısını ölçmek için kullanılır. Bağlı Fabrika simülasyonundaki her istasyon (OPC UA sunucusu), bu`NodeId: "ns=2;i=385"`KPI'yi hesaplamak için telemetri sağlayan bir OPC UA düğümü sağlar.

Bu OPC UA düğümü için yapılandırma aşağıdaki snippet gibi görünür:

```json
{
  "NodeId": "ns=2;i=385",
  "SymbolicName": "NumberOfManufacturedProducts",
  "Relevance": [ "Kpi1", "OeeQuality_Good" ],
  "OpCode": "SubMaxMin"
},
```

Bu yapılandırma, Zaman Serisi Öngörüleri kullanarak bu düğümün telemetri değerlerinin sorgulanmasını sağlar. Zaman Serisi Öngörüleri sorgusu alır:

* Değer sayısı.
* En az değer.
* Maksimal değer.
* Tüm değerlerin ortalaması.
* Tüm benzersiz **OpcUri** **(ApplicationUri),** Belirli bir zaman açısından **Düğüm** çiftleri için tüm değerlerin toplamı.

**NumberOfManufactureredProducts** düğüm değerinin bir özelliği de yalnızca artmasıdır. Zaman açısından üretilen ürün sayısını hesaplamak için, Connected Factory **OpCode** **SubMaxMin**kullanır. Hesaplama, zaman bölmesinin başındaki minimum değeri ve zaman alanının sonundaki en büyük değeri alır.

Yapılandırmadaki **OpCode,** maksimum ve minimum değer farkının sonucunu hesaplamak için hesaplama mantığını yapılandırır. Bu sonuçlar daha sonra alt ta kök (genel) düzeyine kadar birikir ve panoda gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen bir sonraki adım, [Bağlı Fabrika çözümlerini](iot-accelerators-connected-factory-customize.md)nasıl özelleştirini öğrenmektir.
