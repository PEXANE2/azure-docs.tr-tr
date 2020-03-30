---
title: API'ler gezinme - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins yönetim API'lerini sorgulama nın ortak kalıplarını öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/24/2020
ms.openlocfilehash: e9cdfd40a9672d19ef32dede0baadcdd56266bab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79265174"
---
# <a name="how-to-use-azure-digital-twins-management-apis"></a>Azure Digital Twins yönetim API'leri nasıl kullanılır?

Azure Digital Twins yönetim API'leri, IoT uygulamalarınız için güçlü işlevler sağlar. Bu makalede, API yapısında nasıl gezindiğinizi gösterilmektedir.  

## <a name="api-summary"></a>API özeti

Aşağıdaki liste, Digital Twins API'lerinin bileşenlerini gösterir.

* [/spaces](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces): Bu API'ler kurulumunuzdaki fiziksel konumlarla etkileşime girilir. Bunlar, fiziksel konumlarınızın dijital eşlemelerini [uzamsal](concepts-objectmodel-spatialgraph.md#spatial-intelligence-graph)bir grafik biçiminde oluşturmanıza, silmenize ve yönetmenize yardımcı olur.

* [/devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices): Bu API'ler kurulumunuzdaki aygıtlarla etkileşime girmektedir. Bu aygıtlar bir veya daha fazla sensörü yönetebilir. Örneğin, bir aygıt telefonunuz veya Raspberry Pi sensör bölmeniz veya Lora ağ geçidi niz olabilir.

* [/sensors](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Sensors): Bu API'ler cihazlarınızla ve fiziksel konumlarınızla ilişkili sensörlerle iletişim kurmanıza yardımcı olur. Sensörler, mekansal ortamınızı işlemek için kullanılabilecek ortam değerlerini kaydeder ve gönderir.  

* [/resources](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Resources): Bu API'ler, Dijital İkizler örneğiniz için IoT hub'ı gibi kaynakları ayarlamanıza yardımcı olur.

* [/türleri](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Types): Bu API'ler, genişletilmiş türleri Digital Twins nesnelerinizle ilişkilendirmenize ve bu nesnelere belirli özellikler eklemenize olanak sağlar. Bu türler, Kullanıcı Arabirimi'ndeki nesnelerin ve telemetri verilerinizi işleyen özel işlevlerin kolay filtrelenmesine ve gruplanmasına olanak sağlar. Genişletilmiş türleri örnekleri *DeviceType*, *SensorType*, *SensorDataType*, *SpaceType*, *SpaceSubType*, *SpaceBlobType*, *SpaceResourceType*, ve benzeri.

* [/ontolojiler](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Ontologies): Bu API'ler, genişletilmiş türlerin koleksiyonları olan ontolojileri yönetmenize yardımcı olur. Ontolojiler, temsil ettikleri fiziksel alana göre nesne türleri için adlar sağlar. Örneğin, *BACnet* ontolojisi *sensör türleri,* veri *türleri, veri*alt türleri ve *veri birimi türleri*için belirli *adlar*sağlar. Ontolojiler hizmet tarafından yönetilir ve oluşturulur. Kullanıcılar yükleyebilir ve yükleyebilir. Bir ontoloji yüklendiğinde, ilişkili tüm tür adları etkinleştirilir ve uzamsal grafiğinizde sağlanacak şekilde hazırdır. 

* [/propertyKeys](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/PropertyKeys): Bu API'leri, *alanlarınız,* *cihazlarınız,* *kullanıcılarınız*ve *sensörleriniz*için özel özellikler oluşturmak için kullanabilirsiniz. Bu özellikler anahtar/değer çiftleri olarak oluşturulur. Bu özelliklerin veri türünü *PrimitiveDataType'larını*ayarlayarak tanımlayabilirsiniz. Örneğin, sensörleriniz *için* *BasicTemperatureDeltaProcessingRefreshTime* adlı bir özellik tanımlayabilir ve ardından sensörlerinizin her biri için bu özellik için bir değer atayabilirsiniz. Ayrıca, *Min* ve *Max* aralıkları gibi özelliği oluştururken bu değerler için kısıtlamalar ve *Doğrulama Verileri*olarak izin verilen değerler ekleyebilirsiniz.

* [/matchers](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Matchers): Bu API'ler, gelen cihaz verilerinden değerlendirmek istediğiniz koşulları belirtmenize olanak tanır. Daha fazla bilgi için [bu makaleye](concepts-user-defined-functions.md#matchers) bakın. 

* [/userDefinedFunctions](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/UserDefinedFunctions): Bu API'ler, kurulumunuzdan gelen verileri işlemek için *eşleştirme* ler tarafından tanımlanan koşullar oluştuğunda çalıştırılan özel bir işlev oluşturmanıza, silmenize veya güncelleştirmenize olanak sağlar. *Kullanıcı tanımlı işlevler*olarak da adlandırılan bu özel işlevler hakkında daha fazla bilgi için [bu makaleye](concepts-user-defined-functions.md#user-defined-functions) bakın. 

* [/endpoints](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/Endpoints): Bu API'ler, Digital Twins çözümünüzün veri depolama ve analiz için diğer Azure hizmetleriyle iletişim kurabilmesi için uç noktalar oluşturmanıza olanak tanır. Daha fazla bilgi için [bu makaleyi](concepts-events-routing.md) okuyun. 

* [/keyStores](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#/KeyStores): Bu API'ler, alanlarınız için güvenlik anahtar mağazalarını yönetmenize olanak sağlar. Bu mağazalar güvenlik anahtarları koleksiyonu tutabilir ve en son geçerli anahtarları kolayca almanıza olanak sağlar.

* [/users](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Users): Bu API'ler, kullanıcıları boşluklarınızla ilişkilendirmenize, gerektiğinde bu kişilerin yerini bulmanıza olanak sağlar. 

* [/system](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/System): Bu API'ler varsayılan boşluk ve sensör türleri gibi sistem genelindeki ayarları yönetmenize olanak tanır. 

* [/roleAssignments](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/RoleAssignments): Bu API'ler rolleri kullanıcı kimliği, kullanıcı tanımlı işlev kimliği vb. varlıklarla ilişkilendirmenize olanak sağlar. Her rol ataması, ilişkilendirecek varlığın kimliğini, kuruluş türünü, ilişkilendirecek rolün kimliğini, kiracı kimliğini ve varlığın bu ilişkilendirmeyle erişebileceği kaynağın üst sınırını tanımlayan yolu içerir. Daha fazla bilgi için [bu makaleyi](security-role-based-access-control.md) okuyun.


## <a name="api-navigation"></a>API navigasyon

Digital Twins API'leri, aşağıdaki parametreleri kullanarak uzamsal grafiğiniz boyunca filtreleme ve gezinmeyi destekler:

- **spaceId**: API, verilen boşluk kimliğine göre sonuçları filtreler. Ayrıca, boolean bayrak **kullanımıParentSpace** [/spaces](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Spaces) API'leri için geçerlidir, bu da verilen boşluk kimliğinin geçerli alan yerine üst alana atıfta bulunduğunu gösterir. 

- **minLevel** ve **maxLevel**: Kök boşlukları seviye 1 olarak kabul edilir. N *düzeyinde* üst boşluğu olan boşluklar *n+1*düzeyindedir. Bu değerler kümesiyle, sonuçları belirli düzeylerde filtreleyebilirsiniz. Bunlar ayarlandığında kapsayıcı değerlerdir. Aygıtlar, sensörler ve diğer nesneler en yakın uzayla aynı düzeyde kabul edilir. Tüm nesneleri belirli bir düzeyde almak için hem **minLevel'i** hem de **maxLevel'i** aynı değere ayarlayın.

- **minRelative** ve **maxRelative**: Bu filtreler verildiğinde, ilgili düzey verilen boşluk kimliği düzeyine göre:
   - Göreli düzey *0,* verilen boşluk kimliğiyle aynı düzeydedir.
   - Göreli düzey *1,* verilen boşluk kimliğinin alt kısmıyla aynı düzeydeki boşlukları temsil eder. Göreli düzey *n,* belirtilen boşluktan daha düşük olan *boşlukları n* düzeylerine göre temsil eder.
   - Göreli düzey *-1,* belirtilen alanın üst alanıyla aynı düzeydeki boşlukları temsil eder.

- **travers :** Aşağıdaki değerlerde belirtildiği gibi, belirli bir boşluk kimliğinden her iki yönde de geçiş yapmanızı sağlar.
   - **Yok**: Bu varsayılan değer verilen boşluk kimliğine filtre lenir.
   - **Aşağı**: Bu, verilen boşluk kimliği ve onun soyundan gelenler tarafından filtreler. 
   - **Yukarı**: Bu, verilen uzay kimliği ne kadar filtreye ve atalarına göre filtreler. 
   - **Yayılma**: Uzamsal grafiğin yatay bir kısmını, verilen boşluk kimliğiyle aynı düzeyde filtreler. Bunun doğru ayarlanabilmesi için **minRelative** veya **maxRelative** gerekir. 


### <a name="examples"></a>Örnekler

Aşağıdaki liste [, /devices](https://docs.westcentralus.azuresmartspaces.net/management/swagger/ui/index#!/Devices) API'leri arasında gezinmeye bazı örnekler gösterir. Yer tutucunun, `YOUR_MANAGEMENT_API_URL` Azure Dijital İkizler örneğinizin adının bulunduğu `https://YOUR_INSTANCE_NAME.YOUR_LOCATION.azuresmartspaces.net/management/api/v1.0/`ve `YOUR_INSTANCE_NAME` örneğinizin barındırıldığı bölge `YOUR_LOCATION` olduğu biçimindeki Dijital İkizler API'lerinin URI'sini ifade ettiğini unutmayın.

- `YOUR_MANAGEMENT_API_URL/devices?maxLevel=1`kök boşluklarına bağlı tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?minLevel=2&maxLevel=4`düzey 2, 3 veya 4'teki boşluklara bağlı tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId`doğrudan mySpaceId'e bağlı tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down`mySpaceId'e veya onun soyundan birine bağlı tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true`mySpaceId hariç mySpaceId'in torunlarına bağlı tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&minLevel=1&minRelative=true&maxLevel=1&maxRelative=true`mySpaceId'in hemen çocuklarına bağlı tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Up&maxLevel=-1&maxRelative=true`mySpaceId'in atalarından birine bağlı tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Down&maxLevel=5`mySpaceId'in soyundan gelen ve 5'ten küçük veya eşit olan tüm aygıtları döndürür.
- `YOUR_MANAGEMENT_API_URL/devices?spaceId=mySpaceId&traverse=Span&minLevel=0&minRelative=true&maxLevel=0&maxRelative=true`mySpaceId ile aynı düzeydeki boşluklara bağlı tüm aygıtları döndürür.


## <a name="odata-support"></a>OData desteği

/spaces'teki GET çağrısı gibi koleksiyonları döndüren API'lerin çoğu, genel [OData](https://www.odata.org/getting-started/basic-tutorial/#queryData) sistem sorgu seçeneklerinin aşağıdaki alt kümesini destekler:  

* **$filter**
* **$orderby** 
* **$top**
* **$skip** - Koleksiyonun tamamını görüntülemek istiyorsanız, tek bir aramada ayarlanmış bir bütün olarak bunu talep etmeli ve ardından uygulamanızda sayfalama gerçekleştirmelisiniz. 

> [!NOTE]
> Bazı OData seçenekleri **($count,** **$expand**ve **$search**gibi) şu anda desteklenmez.

### <a name="examples"></a>Örnekler

Aşağıdaki liste, geçerli OData sözdizimi içeren birkaç sorguyu gösteriş vegösterilmiştir:

- `YOUR_MANAGEMENT_API_URL/devices?$top=3&$orderby=Name desc`
- `YOUR_MANAGEMENT_API_URL/keystores?$filter=endswith(Description,'space')`
- `YOUR_MANAGEMENT_API_URL/devices?$filter=TypeId eq 2`
- `YOUR_MANAGEMENT_API_URL/resources?$filter=StatusId ne 1`
- `YOUR_MANAGEMENT_API_URL/users?$top=4&$filter=endswith(LastName,'k')&$orderby=LastName`
- `YOUR_MANAGEMENT_API_URL/spaces?$orderby=Name desc&$top=3&$filter=substringof('Floor',Name)`
 
## <a name="next-steps"></a>Sonraki adımlar

Bazı yaygın API sorgu desenleri öğrenmek [için, ortak görevler için Azure Digital Twins API'lerini nasıl sorgular'ı](./how-to-query-common-apis.md)okuyun.

API uç noktalarınız hakkında daha fazla bilgi edinmek [için Digital Twins Swagger'ı nasıl kullanacağınızı](./how-to-use-swagger.md)okuyun.

OData sözdizimini ve kullanılabilir karşılaştırma işleçlerini incelemek için [Azure Bilişsel Arama'da OData karşılaştırma işleçlerini](../search/search-query-odata-comparison-operators.md)okuyun.
