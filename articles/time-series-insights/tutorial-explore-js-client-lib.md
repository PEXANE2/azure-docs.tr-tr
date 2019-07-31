---
title: 'Öğretici: JavaScript istemci kitaplığı Azure Time Series Insights araştırma | Microsoft Docs'
description: Azure Time Series Insights JavaScript istemci kitaplığı ve ilgili programlama modeli hakkında bilgi edinmek için öğretici.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 07/29/2019
ms.author: dpalled
ms.custom: seodec18
ms.openlocfilehash: 2c1013829a7e6e94712e22ae235a47490c8c5eaa
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68677653"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Öğretici: Azure Time Series Insights JavaScript istemci kitaplığını keşfetme

JavaScript D3 tabanlı Azure Time Series Insights istemci kitaplığı, Web geliştiricilerinin Time Series Insights depolanan verileri sorgulamasını ve görselleştirmesine yardımcı olmak için geliştirilmiştir. Bu öğretici, barındırılan bir örnek uygulama kullanarak Time Series Insights istemci kitaplığı ve programlama modeli boyunca size rehberlik eder.

Öğretici, kitaplıkla nasıl çalışılacağı, Time Series Insights verilere nasıl erişebileceğiniz ve verileri işlemek ve görselleştirmek için grafik denetimlerinin nasıl kullanılacağı hakkında bilgi sağlar. Ayrıca, verileri görselleştirmek için farklı türdeki grafiklerle nasıl denemeler yapacağınızı öğreneceksiniz. Öğreticinin sonunda, istemci kitaplığını kullanarak Time Series Insights özelliklerini kendi web uygulamanıza ekleyebilirsiniz.

Özellikle şunları öğreneceksiniz:

> [!div class="checklist"]
> * Time Series Insights örnek uygulama
> * JavaScript istemci kitaplığı Time Series Insights
> * Örnek uygulama Time Series Insights verileri görselleştirmek için kitaplığı nasıl kullanır

> [!NOTE]
> * Öğretici, ücretsiz, barındırılan [Time Series Insights Web tanıtımı](https://insights.timeseries.azure.com/clientsample)kullanır.
> * Time Series Insights örnek uygulama kaynak dosyaları [GitHub örnek deposunda](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)verilmiştir.
> * [Time Series Insights istemci başvurusu belgelerini](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)okuyun.

Henüz yoksa ücretsiz bir [Azure aboneliğine](https://azure.microsoft.com/free/) kaydolun.

## <a name="prerequisites"></a>Önkoşullar

* Bu öğretici, tarayıcınızın **Geliştirici Araçları** özelliğini kullanır. Modern Web tarayıcıları ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [Firefox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/)ve diğerleri), genellikle klavyenizde F12 kısayol tuşu aracılığıyla **Web denetçisi görünümüne** erişim sağlar. Görünüme erişmenin başka bir yolu da bir Web sayfasına sağ tıklayıp ardından **öğeyi İncele**' yi seçeceksiniz.

## <a name="video"></a>Video

Bu videoda, açık kaynaklı Time Series Insights JavaScript SDK 'sını sunuyoruz:
<br /><br />

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="time-series-insights-sample-application"></a>Time Series Insights örnek uygulaması

Bu öğreticide, uygulamanın arkasındaki kaynak kodu araştırmak ve Time Series Insights JavaScript istemci kitaplığını araştırmak için, ücretsiz ve barındırılan Time Series Insights örnek bir uygulama kullanırız. Örnek uygulamayı kullanarak JavaScript 'te Time Series Insights etkileşim kurmayı ve grafikleri ve grafikleri kullanarak verileri görselleştirmeyi öğreneceksiniz.

1. [Time Series Insights örnek uygulamasına](https://insights.timeseries.azure.com/clientsample)gidin. Aşağıdaki oturum açma istemi görüntülenir:

   [![Time Series Insights istemci örnek oturum açma istemi](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Kimlik bilgilerinizi girmek veya seçmek için **oturum aç '** ı seçin. Kurumsal kuruluş hesabı (Azure Active Directory) veya kişisel hesap (Microsoft hesabı) kullanın.

   [![Time Series Insights istemci örnek kimlik bilgileri istemi](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Oturum açtıktan sonra, Time Series Insights verilerle doldurulmuş grafikleri görüntüleyen bir sayfa gösterilir. Kullanıcı hesabınız ve **Oturumu kapat** seçeneği, sağ üst köşede görünür:

   [![Oturum açtıktan sonra istemci örnek ana sayfası Time Series Insights](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Sayfa kaynağı ve yapısı

İlk olarak, işlenmiş Web sayfasının [HTML ve JavaScript kaynak kodunu](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) görüntüleyelim:

1. Tarayıcınızda **Geliştirici Araçları**’nı açın. Geçerli sayfayı oluşturan HTML öğelerini inceleyin (HTML veya DOM ağacı olarak da bilinir).

1. `<head>` Ve`<body>` öğelerini genişletin ve aşağıdaki bölümleri inceleyin:

   * `<head>` Öğesi altında, uygulamanın çalışmasını sağlayan sayfa meta verilerini ve bağımlılıklarını bulacaksınız:
     * Azure Active Directory `<script>` kimlik doğrulama kitaplığı (ADAL) dosyası *adal. min. js*' ye başvurmak için kullanılan bir öğe. ADAL, API’lere erişim için OAuth 2.0 kimlik doğrulaması (oturum açma) ve belirteç alımı sağlayan bir JavaScript kitaplığıdır.
     * `<link>` *Samplestyles. css* ve *tsiclient. css*gibi stil sayfaları ( *CSS*olarak da bilinir) için birden çok öğe. Stil sayfaları, renkler, yazı tipleri ve boşluklar gibi görsel sayfa stil ayrıntılarını denetler.
     * Time Series Insights `<script>` JavaScript istemci kitaplığı *tsiclient. js*' ye başvurmak için kullanılan bir öğe. Sayfa, Time Series Insights hizmeti API 'Lerini çağırmak ve sayfadaki grafik denetimlerini işlemek için kitaplığı kullanır.

     >[!NOTE]
     > * ADAL JavaScript kitaplığı kaynak kodu, [Azure-ActiveDirectory-Library-for-js deposunda](https://github.com/AzureAD/azure-activedirectory-library-for-js)bulunur.
     > * Time Series Insights JavaScript istemci kitaplığının kaynak kodu [tsiclient deposunda](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)bulunabilir.

   * Öğesi altında, sayfada öğelerin yerleşimini ve başka bir `<script>` öğesini tanımlamaya yardımcı olan öğeleri bulabilirsiniz `<div>`. `<body>`
     * İlk `<div>` öğe, **oturum açma** iletişim kutusunu (`id="loginModal"`) belirtir.
     * İkinci `<div>` öğesi şunlar için üst öğe işlevi görür:
       * Sayfanın üst kısmında (`class="header"`) doğru durum iletileri ve oturum açma bilgileri için kullanılan üst bilgi `<div>` öğesi.
       * Grafik `<div>` (`class="chartsWrapper"`) dahil olmak üzere sayfa gövdesi öğelerinin geri kalanı için bir öğe.
       * Sayfayı `<script>` denetlemek için kullanılan JavaScript 'i içeren bir bölüm.

   [![Geliştirici Araçları ile Istemci örneğini Time Series Insights](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Öğesini genişletin `<div class="chartsWrapper">` , daha fazla alt `<div>` öğe bulacaksınız. Bu öğeler her bir grafik denetimi örneğini konumlandırmak için kullanılır. Her bir grafik örneği için `<div>` bir tane olmak üzere birkaç öğe çifti vardır:

   * İlk (`class="rowOfCardsTitle"`) öğe, grafiğin neyi gösterdiğini özetleyen açıklayıcı bir başlık içerir. Örneğin, `Static Line Charts With Full-Size Legends.`
   * Second (`class="rowOfCards"`) öğesi, bir satır içindeki gerçek grafik denetimlerini konumlandırın ek alt `<div>` öğelerini içeren bir üst öğedir.

   [![Gövde div öğeleri](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Öğenindoğrudan`<div class="chartsWrapper">` altındaki öğesini genişletin. `<script type="text/javascript">` Sayfa düzeyi JavaScript bölümünün başı, tüm sayfa mantığını (kimlik doğrulaması, Time Series Insights hizmeti API 'Leri çağırma, grafik denetimlerini işleme ve daha fazlasını) işlemek için kullanılır:

   [![Gövde betiği](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="time-series-insights-javascript-client-library-concepts"></a>JavaScript istemci kitaplığı kavramlarını Time Series Insights

Time Series Insights istemci kitaplığı (*tsiclient. js*) Iki önemli JavaScript işlevi için soyut olanaklar sağlar:

* **Time Series Insights sorgu API 'lerini çağırmak Için sarmalayıcı yöntemleri**: Toplama ifadelerini kullanarak Time Series Insights verilerini sorgulamak için kullanabileceğiniz REST API 'Leri. Yöntemler, kitaplığın TsiClient. Server ad alanı altında düzenlenir.

* **Çeşitli grafik denetimleri türlerini oluşturma ve doldurma yöntemleri**: Bir Web sayfasında Time Series Insights toplam verileri işlemek için kullanabileceğiniz yöntemler. Yöntemler, kitaplığın TsiClient. UX ad alanı altında düzenlenir.

Bu basitleştiriciler sayesinde, geliştiriciler Time Series Insights verilerle desteklenen UI grafını ve grafik bileşenlerini daha kolay bir şekilde oluşturabilir.

### <a name="authentication"></a>Authentication

[Time Series Insights örnek uygulama](https://insights.timeseries.azure.com/clientsample) , ADAL OAuth 2,0 Kullanıcı kimlik doğrulama desteği olan tek sayfalı bir uygulamadır:

1. Kimlik doğrulaması için ADAL kullandığınızda, istemci uygulamanın Azure Active Directory (Azure AD) kayıtlı olması gerekir. Aslında, tek sayfalı uygulama, [OAuth 2,0 örtük izin akışını](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow)kullanmak üzere kaydedilir.
1. Uygulamanın çalışma zamanında bazı kayıt özelliklerini belirtmesi gerekir. Özellikler, istemci GUID (`clientId`) ve yeniden yönlendirme URI 'si (`postLogoutRedirectUri`) içerir.
1. Daha sonra, uygulama Azure AD 'den bir *erişim belirteci* ister. Erişim belirteci, belirli bir hizmet veya API tanımlayıcısı (https:\//api.timeseries.Azure.com) için sınırlı bir izin kümesi için verilir. Belirteç izinleri, oturum açmış kullanıcı adına verilir. Hizmet veya API için tanımlayıcı, uygulamanın Azure AD kaydında yer alan başka bir özelliktir.
1. ADAL, uygulamaya erişim belirtecini döndürtikten sonra, Time Series Insights hizmeti API 'Lerine eriştiğinde bir *taşıyıcı belirteci* olarak geçirilir.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Microsoft tarafından desteklenen Azure AD kimlik doğrulama kitaplıkları hakkında daha fazla bilgi edinmek için [Azure Active Directory kimlik doğrulaması kitaplığı başvuru belgelerine](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries)bakın.

### <a name="control-identification"></a>Denetim tanımlama

Belirtilen örnekte `<div>` öğeler üst `<body>` öğede düzenlenir ve sayfada işlenen grafik denetimleri için daha erişilebilir bir düzen sağlar.

Her `<div>` öğe, grafik denetimlerinin yerleştirme ve görsel özniteliklerinin özelliklerini belirler. HTML öğesi `id` özellikleri, görselleştirilen verileri işlemek ve güncelleştirmek üzere belirli denetimlere bağlamak için benzersiz tanımlayıcılar sunar.

### <a name="aggregate-expressions"></a>Toplama ifadeleri

Time Series Insights istemci kitaplığı API 'Leri toplam ifadeler kullanır:

* Toplama ifadesi bir veya daha fazla *arama terimi*oluşturma yeteneği sağlar.

* İstemci API 'leri, arama yayma, `where` koşullar, ölçüler ve `splitBy` değerler kullanan başka bir demo uygulamasına ( [Time Series Insights Gezgini](https://insights.timeseries.azure.com/demo)) benzer işlevler sağlamak üzere tasarlanmıştır.

* Çoğu istemci kitaplığı API 'Leri, hizmetin bir Time Series Insights veri sorgusu oluşturmak için kullandığı bir dizi toplama ifadesi alır.

### <a name="call-pattern"></a>Çağrı deseni

Grafik denetimlerini doldurma ve işleme genel bir model izler. Örnek uygulamanın tamamında genel bir model gözlemleyebilirsiniz ve istemci kitaplığını kullandığınızda size yardımcı olabilir:

1. Bir veya `array` daha fazla Time Series Insights toplama ifadesi tutmak için bir bildirir:

   ```javascript
   var aes =  [];
   ```

1. Derleme *1* - *n* toplam ifade nesneleri. Ardından, bunları toplam ifade dizisine ekleyin:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **aggregateExpression parametreleri**

   | Parametre | Açıklama | Örnek |
   | --------- | ----------- | ------- |
   | `predicateObject` | Veri filtreleme ifadesi |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | Kullanılan ölçünün Özellik adı | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | İstediğiniz ölçü özelliğinin toplamaları | `['avg', 'min']` |
   | `searchSpan`      | Toplam ifadesinin süre ve Aralık boyutu | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | Ayırmak istediğiniz dize özelliği (isteğe bağlı: null olabilir) | `{property: 'Station', type: 'String'}` |
   | `color`         | İşlemek istediğiniz nesnelerin rengi | `'pink'` |
   | `alias`           | Toplama ifadesi için kolay bir ad | `'Factory3Temperature'` |
   | `contextMenuActions` | Görselleştirmedeki zaman serisi nesnelerine bağlanacak bir eylem dizisi (isteğe bağlı) | Daha fazla bilgi için bkz. [açılır bağlam menüleri](#pop-up-context-menus). |

1. Toplam verileri istemek için TsiClient. Server API 'Lerini kullanarak bir Time Series Insights sorgusu çağırın:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **getAggregates parametreleri**

   | Parametre | Açıklama | Örnek |
   | --------- | ----------- | ------- |
   | `token`     | Time Series Insights API 'SI için erişim belirteci |  `authContext.getTsiToken()`<br />Daha fazla bilgi için bkz. [Kimlik doğrulaması](#authentication). |
   | `envFQDN`   | Time Series Insights ortamı için tam etki alanı adı (FQDN) | Azure portal. Örneğin: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com` |
   | `aeTsxArray` | Time Series Insights sorgu ifadeleri dizisi | Daha önce açıklandığı gibi `aes.map(function(ae){return ae.toTsx()}` değişkenikullanın:.`aes` |

1. Görselleştirme için Time Series Insights sorgusundan döndürülen sıkıştırılmış sonucu JSON 'ye dönüştürün:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. TsiClient. UX API 'Lerini kullanarak bir grafik denetimi oluşturun. Sayfadaki `<div>` öğelerden birine bağlayın:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Grafik denetimini dönüştürülmüş JSON veri nesneleriyle doldurun ve denetimi sayfada işle:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="render-controls"></a>İşleme denetimleri

Time Series Insights istemci kitaplığı, sekiz benzersiz, kullanıma hazır analiz denetimi sağlar:

* **çizgi grafik**
* **pasta grafik**
* **çubuk grafik**
* **ısı haritasını**
* **hiyerarşi denetimleri**
* **erişilebilir kılavuz**
* **ayrık olay zaman çizelgeleri**
* **durum geçişi zaman çizelgeleri**

### <a name="line-chart-bar-chart-and-pie-chart-examples"></a>Çizgi grafik, çubuk grafik ve pasta grafik örnekleri

Standart grafik denetimlerinden bazılarını işlemek için kullanılan tanıtım koduna bakın. Bu denetimleri oluşturmak için programlama modelini ve düzenlerini aklınızda yapın. `// Example 3/4/5` Özellikle, HTML `id` değerleri `chart3`, ve`chart5`ile denetimleri işleyen yorum altında HTML 'yi inceleyin. `chart4`

Grafik denetimlerinin sayfadaki satırlarda düzenlendiği [sayfa kaynağı ve yapısı bölümünün](#page-source-and-structure) 3. adımından geri çekin. Her grafik denetiminin açıklayıcı bir başlık satırı vardır. Bu örnekte, üç grafik `Multiple Chart Types From the Same Data` başlık `<div>` öğesinin altında doldurulur ve başlığın altında olan üç `<div>` öğeye bağlanır:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

JavaScript kodunun aşağıdaki bölümü, daha önce özetlenen desenleri kullanır: derleme Time Series Insights toplama ifadeleri oluşturun, Time Series Insights verileri sorgulamak için bunları kullanın ve ardından üç grafiği işleme. Tsiclient. ux ad alanından üç grafik türü kullanılır: `LineChart`, `BarChart`ve `PieChart`. Grafik türleri ilgili grafikleri oluşturmak ve işlemek için kullanılır. Üç grafik de aynı toplama ifadesi verilerini `transformedResult`kullanabilir:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Üç grafik işlendiğinde aşağıdaki gibi görünür:

[![Aynı verilerle birden çok grafik türü](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Gelişmiş özellikler

Time Series Insights istemci kitaplığı, veri görselleştirmeleri yürütmek için kullanabileceğiniz çeşitli ek özelliklere sahiptir.

### <a name="states-and-events"></a>Durumlar ve olaylar

Gelişmiş bir işlevsellik, grafiklere durum geçişleri ve ayrık olaylar ekleme olanağıdır. Bu özellik olayları vurgulama, uyarma ve durum anahtarları oluşturma (örneğin, açık/kapalı anahtarlar) için kullanışlıdır.

`// Example 10` Yorumu çevreleyen koda bakın. Kod, başlık `Line Charts with Multiple Series Types` altında bir çizgi denetimi işler ve `id` HTML değeri `chart10`olan `<div>` öğesine bağlar.

Aşağıdaki adımlarda işlem açıklanır:

1. Adlı `events4` bir yapı, izlemek üzere durum değişikliği öğelerini tutmak için tanımlanır. Yapı şunları içerir:

   * `Component States` adlı bir dize anahtarı.
   * Durumları temsil eden bir değer nesneleri dizisi. Her nesne şunları içerir:
     * JavaScript ISO zaman damgası içeren bir dize anahtarı.
     * Durumun özelliklerini içeren bir dizi: bir renk ve bir açıklama.

1. Yapısı, izlemek üzere bir `Incidents`olay öğeleri dizisini tutan için tanımlanır. `events5` Dizi yapısı, `events4` için belirtilen yapıyla aynı şekildedir.

1. Çizgi grafik işlenir ve grafik seçenekleri parametreleriyle iki yapıya geçirilir: `events:` ve. `states:` , `tooltip:` `theme:`Veya belirtmekiçindiğerseçenekparametrelerinegözönündekoyun.`grid:`

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Zaman ölçeği durum değişikliğini göstermek için olayları ve renkli çubukları/açılır pencereleri göstermek için kullanılan elmas işaretçileri/açılır pencereler:

[![Birden çok seri türüne sahip çizgi grafikler](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Açılır menüler

Diğer gelişmiş işlevler, özel bağlam menüleri oluşturma (açılır menüler ' e sağ tıklama) özelliğidir. Özel açılır menüler, uygulamanızın kapsamı dahilinde sıradaki eylemleri ve mantıksal adımları etkinleştirmek için yararlıdır.

`// Example 13/14/15` Açıklamanın etrafındaki koda bakın. Bu kod başlangıçta başlık `Line Chart with Context Menu to Create Pie/Bar Chart`altında bir çizgi grafik oluşturur. Grafik, HTML `<div>` `id` değeri `chart13`olan öğesine bağlanır.

Bağlam menülerini kullanarak, çizgi grafik dinamik olarak `chart14` ve `chart15` kimlikleriyle `<div>` öğelerine bağlı bir pasta ve çubuk grafik oluşturma olanağı sağlar. Hem pasta grafiği hem de çubuk grafik Ayrıca kendi özelliklerini etkinleştirmek için bağlam menülerini kullanır: pasta grafiğinden çubuk grafiğe veri kopyalama ve çubuk grafik verilerini sırasıyla tarayıcı konsolu penceresine yazdırma özelliği.

Aşağıdaki adımlarda işlem açıklanır:

1. Bir dizi özel eylem tanımlanmıştır. Her eylem bir veya daha fazla öğe ile bir dizi içerir. Her öğe tek bir bağlam menüsü öğesini tanımlar:

   * `barChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeren pasta grafiğinin bağlam menüsünü tanımlar:
     * `name`: Menü öğesi için kullanılan metin: "Parametreleri konsola yazdır."
     * `action`: Menü öğesiyle ilişkili eylem. Eylem her zaman anonim bir işlevdir ve grafiği oluşturmak için kullanılan toplama ifadesi temelinde üç bağımsız değişken alır. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
       * `ae`: Toplam ifade dizisi.
       * `splitBy`: `splitBy` Değer.
       * `timestamp`: Zaman damgası.

   * `pieChartActions`: Bu eylem, tek bir öğeyi tanımlamak için bir öğe içeren çubuk grafiğinin bağlam menüsünü tanımlar. Şekil ve şema, daha önce açıklanan `barChartActions` öğeyle aynıdır, `action` ancak işlev önemli ölçüde farklıdır: çubuk grafiği oluşturur ve oluşturur. `ae` Bağımsız değişkeni, çalışma zamanında geçirilen toplam ifade dizisini menü öğesi açıldığında belirtmek için kullanılır. İşlev, `barChartActions` bağlam menüsüyle birlikte `ae.contextMenu` özelliğini de ayarlar.
   * `contextMenuActions`: Bu eylem, üç menü öğesini tanımlamak için üç öğe içeren çizgi grafiğinin bağlam menüsünü tanımlar. Her bir öğenin şekli ve şeması, daha önce açıklanan öğelerle aynıdır. Aynı `barChartActions` öğesi gibi, ilk öğe tarayıcı konsol penceresine üç işlev bağımsız değişkenini yazar. `pieChartActions` Öğesine benzer şekilde ikinci iki öğe, sırasıyla pasta grafiği ve çubuk grafiği oluşturur ve işler. İkinci iki öğe sırasıyla `pieChartActions` ve `barChartActions` bağlam menüleriyle `ae.contextMenu` özelliklerini de ayarlar.

1. `aes` Toplam ifade dizisine iki toplama ifadesi itilir. Her öğe için `contextMenuActions` diziyi belirtirler. Bu ifadeler, çizgi grafik denetimiyle kullanılır.

1. Yalnızca çizgi grafik başlangıçta işlendiğinde, hem pasta grafiğin hem de çubuk grafiğin çalışma zamanında işlenebileceği.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

Aşağıdaki ekran görüntüsünde, ilgili açılan pencere bağlam menülerinin bulunduğu grafikler gösterilmektedir. Pasta grafik ve çubuk grafik, çizgi grafik bağlam menüsü seçenekleri kullanılarak dinamik olarak oluşturulmuştur.

[![Pasta grafik ve çubuk grafik oluşturmak için bağlam menüsü olan çizgi grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Fırçalar

Yakınlaştırma ve araştırma gibi eylemleri tanımlamak üzere bir zaman aralığı kapsamını belirlemek için fırçaları kullanabilirsiniz.

Fırçaları göstermek için kullanılan kod, açılan bağlam menülerini açıklayan `Line Chart with Context Menu to Create Pie/Bar Chart` örnekte gösterilmiştir.

* Fırça eylemleri, fırça için bir dizi özel eylem tanımlaması bakımından bağlam menüsüne benzer. Her eylem bir veya daha fazla öğesi olan bir dizi içerir. Her öğe tek bir bağlam menüsü öğesini tanımlar:
   * `name`: Menü öğesi için kullanılan metin: "Parametreleri konsola yazdır."
   * `action`: Her zaman iki bağımsız değişken alan bir anonim işlev olan menü öğesiyle ilişkili eylem. Bu örnekte, bağımsız değişkenler tarayıcı konsol penceresine yazılır:
     * `fromTime`: Fırça `from` seçiminin zaman damgası.
     * `toTime`: Fırça `to` seçiminin zaman damgası.

* Fırça eylemleri başka bir grafik seçeneği özelliği olarak eklenir. `brushContextMenuActions: brushActions` Özelliği `linechart.Render` çağrıya geçirilir.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Fırçalar kullanarak pasta grafik ve çubuk grafik oluşturmak için bağlam menüsü olan çizgi grafik](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticiyi tamamladığınıza göre, oluşturduğunuz kaynakları temizleyin:

1. [Azure Portal](https://portal.azure.com)sol menüden **tüm kaynaklar**' ı seçin, Azure Time Series Insights kaynak grubunuzu bulun.
1. Her kaynağı ayrı ayrı **Sil** veya Kaldır ' a tıklayarak kaynak grubunun tamamını (ve içinde yer alan tüm kaynakları) silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Oturum açın ve Time Series Insights örnek uygulamayı ve kaynağını araştırın
> * Time Series Insights JavaScript istemci kitaplığındaki API 'Leri kullanma
> * Grafik denetimlerini Time Series Insights verilerle oluşturmak ve doldurmak için JavaScript kullanma

Time Series Insights örnek uygulama bir demo veri kümesi kullanır. Kendi Time Series Insights ortamınızı ve veri kümenizi nasıl oluşturabileceğiniz hakkında bilgi edinmek için aşağıdaki makaleyi okuyun:

> [!div class="nextstepaction"]
> [Öğretici: Azure Time Series Insights ortamı oluşturma](tutorial-create-populate-tsi-environment.md)

Veya örnek uygulama kaynak dosyalarını Time Series Insights görüntüleyin:

> [!div class="nextstepaction"]
> [Örnek uygulama deposunu Time Series Insights](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

İstemci API 'SI başvuru belgelerini Time Series Insights okuyun:

> [!div class="nextstepaction"]
> [Time Series Insights API başvuru belgeleri](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
