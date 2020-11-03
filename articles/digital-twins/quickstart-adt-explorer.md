---
title: Hızlı Başlangıç-örnek senaryoyu araştırma
titleSuffix: Azure Digital Twins
description: Hızlı başlangıç-önceden oluşturulmuş bir senaryoyu görselleştirmek ve araştırmak için ADT Explorer örneğini kullanın.
author: baanders
ms.author: baanders
ms.date: 9/24/2020
ms.topic: quickstart
ms.service: digital-twins
ms.openlocfilehash: 466129e8435ef694821b078592a100a111a43f3a
ms.sourcegitcommit: 80034a1819072f45c1772940953fef06d92fefc8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93242288"
---
# <a name="quickstart---explore-a-sample-azure-digital-twins-scenario-using-adt-explorer"></a>Hızlı başlangıç-ADT Explorer kullanarak örnek bir Azure dijital TWINS senaryosunu araştırma

Azure dijital TWINS sayesinde gerçek ortamlarınızın canlı modellerini oluşturabilir ve bunlarla etkileşim kurabilirsiniz. Bu, ayrı öğelerin **dijital** bir şekilde modellenmesi ve sonra canlı olaylara yanıt verebilecek ve bilgi için sorgulanabilen bir bilgi **grafiğine** bağlanılarak yapılır.

Bu hızlı başlangıçta, [**Azure Digital TWINS (ADT) Gezgini**](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/)adlı örnek bir uygulamanın yardımıyla önceden oluşturulmuş bir Azure dijital TWINS grafiğini araştıracaktır. ADT Explorer bir ortamın dijital temsilini karşıya yüklemenize, Azure dijital TWINS 'teki ortamı göstermek için oluşturulan TWINS ve grafiğin görsel görüntülerini görüntülemenize ve tarayıcı tabanlı, görsel deneyimle diğer yönetim etkinliklerini gerçekleştirmenize olanak tanır.

Hızlı başlangıç aşağıdaki önemli adımları içerir:

1. Azure dijital TWINS örneğini ve ADT Gezginini ayarlama
1. Örnek senaryoyu oluşturmak için önceden oluşturulmuş modelleri ve grafik verilerini karşıya yükleme
1. Oluşturulan senaryo grafiğini keşfet
1. Grafikte değişiklikler yapma

İle birlikte çalıştığınız örnek grafik, iki kattan ve iki odaya sahip bir bina temsil eder. Grafik şöyle görünür:

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil.":::

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir Azure aboneliğine sahip olmanız gerekir. Henüz bir tane yoksa, şimdi **[ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)** .

Ayrıca makinenizde **Node.js** gerekir. Bu bağlantıdaki en son sürümü edinebilirsiniz: [Node.js](https://nodejs.org/).

Son olarak, hızlı başlangıç: **ADT Explorer** örnek uygulaması sırasında kullanmak üzere örneği de indirmeniz gerekir. Bu örnek, hızlı başlangıçta bir Azure dijital TWINS senaryosu ve örnek senaryo dosyalarını yüklemek ve araştırmak için kullandığınız uygulamayı içerir. Örneği almak için şuraya gidin: [Azure Digital TWINS (ADT) Explorer](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/). İndirmek için *posta indirme* düğmesine basın *.* Makinenize Bu örnek kodun ZIP dosyası. Bu, bir yükler. ZIP klasörünü makinenize _**Azure_Digital_Twins__ADT__explorer.zip**_. Klasörü sıkıştırmayı açın ve dosyaları ayıklayın.

## <a name="set-up-azure-digital-twins-and-adt-explorer"></a>Azure dijital TWINS ve ADT Gezginini ayarlama

Azure dijital TWINS ile çalışmanın ilk adımı, **bir Azure dijital TWINS örneği kurmak** için kullanılır. Hizmetin bir örneğini oluşturduktan ve ADT Explorer ile kimlik doğrulaması yapmak için **kimlik bilgilerinizi** ayarladıktan sonra, **ADT Explorer 'daki örneğe bağlanabilir** ve hızlı başlangıçtaki örnek verilerle doldurabilirsiniz.

Bu bölümün geri kalanında bu adımlarda adım adım gösterilmektedir.

### <a name="set-up-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini ayarlama

[!INCLUDE [digital-twins-prereq-instance.md](../../includes/digital-twins-prereq-instance.md)]

### <a name="set-up-local-azure-credentials"></a>Yerel Azure kimlik bilgilerini ayarlama

ADT Explorer uygulaması, [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) `Azure.Identity` Yerel makinenizde çalıştırdığınızda Azure dijital TWINS örneğiyle kullanıcıların kimliğini doğrulamak için DefaultAzureCredential (kitaplığın bir parçası) kullanır. Bir istemci uygulamasının Azure dijital TWINS ile kimlik doğrulaması yapabilme yolları hakkında daha fazla bilgi için bkz. [*nasıl yapılır: yazma uygulama kimlik doğrulaması kodu*](how-to-authenticate-client.md).

Bu tür bir kimlik doğrulamasıyla, ADT Explorer yerel ortamınızda yerel bir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) veya Visual Studio/Visual Studio Code Içindeki bir Azure oturumu gibi kimlik bilgilerini arar. Bu, ADT Explorer uygulamasının kimlik bilgilerini ayarlamak için bu mekanizmalardan biri aracılığıyla **yerel olarak Azure 'da oturum açmanız** gerektiği anlamına gelir.

Bu yollarla Azure 'da zaten oturum açtıysanız bir [sonraki bölüme](#run-and-configure-adt-explorer)atlayabilirsiniz.

Aksi takdirde, yerel **Azure CLI** 'yi şu adımlarla yükleyebilirsiniz:
1. İşletim sistemi ile eşleşen yüklemeyi tamamlamaya yönelik [**Bu yükleme bağlantısındaki**](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) işlemi izleyin.
2. Makinenizde bir konsol penceresi açın.
3. `az login`' İ çalıştırın ve Azure hesabınızda oturum açmak için kimlik doğrulama istemlerini izleyin.

Bunu yaptıktan sonra, bir sonraki bölümde çalıştırdığınızda ADT Gezgini, Azure kimlik bilgilerinizi otomatik olarak göstermelidir.

İsterseniz kimlik doğrulama konsolu penceresini kapatabilir veya bir sonraki adımda kullanmak üzere açık tutabilirsiniz.

### <a name="run-and-configure-adt-explorer"></a>ADT Gezginini çalıştırma ve yapılandırma

Sonra, ADT Explorer uygulamasını çalıştırın ve Azure dijital TWINS örneğiniz için yapılandırın.

İndirilen ve sıkıştırılmış olmayan _**Azure_Digital_Twins__ADT__explorer**_ klasörüne gidin. */Client/src Azure_Digital_Twins__ADT__explorer* klasör konumunda bir konsol penceresi açın.

`npm install`Tüm gerekli bağımlılıkları indirmek için ' i çalıştırın.

Ardından, çalıştırarak uygulamayı başlatın `npm run start` .

Birkaç saniye sonra, bir tarayıcı penceresi açılır ve uygulama tarayıcıda görüntülenir.

:::image type="content" source="media/quickstart-adt-explorer/explorer-blank.png" alt-text="Localhost: 3000 konumunda çalışan bir uygulamayı gösteren tarayıcı penceresi. Uygulama ADT Explorer olarak adlandırılır ve bir sorgu Gezgini, model görünümü, grafik görünümü ve Özellik Gezgini için kutular içerir. Henüz bir ekran verisi yok." lightbox="media/quickstart-adt-explorer/explorer-blank.png":::

ADT Gezginini ayarladığınız örnekle çalışacak şekilde yapılandırmak için pencerenin üst kısmındaki *oturum aç* düğmesine (aşağıdaki görüntüde gösterildiği gibi) basın. 

:::image type="content" source="media/quickstart-adt-explorer/sign-in.png" alt-text="Pencerenin üst kısmındaki oturum açma simgesini vurgulayan ADT Explorer. Simge, bir anahtarın silueti içeren bir kişinin basit silueti gösterir." lightbox="media/quickstart-adt-explorer/sign-in.png":::

[Önkoşullar](#prerequisites) bölümünde toplanan *Azure dijital TWıNS örneği URL 'sini* *https://{örnek ana bilgisayar adı}* biçiminde girin.

>[!NOTE]
> Aynı simgeyi seçerek, oturum açma kutusunu tekrar çekmek için bu bilgileri dilediğiniz zaman yeniden ziyaret edebilir/düzenleyebilirsiniz. Bu, geçirilen değerleri tutacak.

> [!TIP]
> Bağlandığınızda bir `SignalRService.subscribe` hata iletisi gösteriliyorsa, Azure Digital TWINS URL 'nizin *https://* ile başladığından emin olun.

Microsoft 'tan bir *izin* iste açılan pencere görürseniz, bu uygulamaya izin verin ve devam etmek için kabul edin.

## <a name="add-the-sample-data"></a>Örnek verileri ekleme

Ardından, örnek senaryoyu ve grafiği ADT Explorer 'a içeri aktaracaksınız. Örnek senaryo, daha önce indirdiğiniz **Azure_Digital_Twins__ADT__explorer** klasöründe de bulunur.

### <a name="models"></a>Modeller

Bir Azure dijital TWINS çözümünün ilk adımı, ortamınız için sözlük tanımlıyor. Bu işlem, ortamınızda var olan varlık türlerini açıklayan özel [**modeller**](concepts-models.md)oluşturularak yapılır. 

Her model, **dijital Ikizi tanım dili (DTDL)** ADLı bir JSON-ld benzeri dilde yazılır ve *özellikleri* , *telemetrisi* , *ilişkileri* ve *bileşenleri* açısından tek bir varlık türünü tanımlar. Daha sonra bu modelleri, bu türlerin belirli örneklerini temsil eden dijital TWINS 'in temeli olarak kullanacaksınız.

Genellikle bir model oluştururken üç adımı tamamlayacaksınız:
1. Model tanımını yazın (örnek çözümün bir parçası olarak zaten yapılmış olan hızlı başlangıçta)
2. Sözdiziminin doğru olduğundan emin olmak için bunu doğrulayın (hızlı başlangıçta, örnek çözümün bir parçası olarak zaten yapılır)
3. Azure dijital TWINS örneğinizi karşıya yükleyin
 
Bu hızlı başlangıçta, model dosyaları sizin için zaten yazılmıştır ve doğrulanıp, indirdiğiniz çözüme dahil edilmiştir. Bu bölümde, derleme ortamının bu bileşenlerini tanımlamak için örneğinizle önceden yazılmış iki modeli karşıya yükleyeceksiniz:
* Floor
* Yı

#### <a name="upload-models"></a>Modelleri karşıya yükle

*Model görünümü* kutusunda *bir model yükle* simgesine basın.

:::image type="content" source="media/quickstart-adt-explorer/upload-model.png" alt-text="Model görünümü kutusunda ortadaki simge vurgulanır. Bir buluta işaret eden bir ok gösterir." lightbox="media/quickstart-adt-explorer/upload-model.png":::
 
1. Görüntülenen dosya Seçicisi kutusunda, indirilen depodaki *Azure_Digital_Twins__ADT__explorer/Client/examples* klasörüne gidin.
2. *ÜzerindeRoom.js* seçin ve *üzerindeFloor.js* ve Tamam 'a tıklayın. (İsterseniz ek modeller yükleyebilirsiniz, ancak bu hızlı başlangıçta kullanılmaz.)
3. Azure hesabınızda oturum açmanızı isteyen açılan menü iletişim kutusunu izleyin.

>[!NOTE]
>Şu hata iletisini görürseniz: :::image type="content" source="media/quickstart-adt-explorer/error-models-popup.png" alt-text="bir açılan menü okuma ' hatası: modeller getirilirken hata: ClientAuthError: açılır pencere açılırken hata oluştu. Bu, IE kullanıyorsanız veya tarayıcıda açılan pencereler engelleniyorsa gerçekleşebilir. ' alt kısımdaki bir kapalı düğme ile" border="false"::: 
> Açılır pencere engelleyicinizi devre dışı bırakmayı veya farklı bir tarayıcı kullanmayı deneyin.

ADT Gezgini şimdi bu model dosyalarını Azure dijital TWINS örneğinize yükleyecek. Bunların kolay adlarını ve tam model kimliklerini görüntüleyen *model görünümü* kutusunda gösterilmesi gerekir. Modeldeki DTDL kodunu görmek için model bilgi balonlarını *göster* ' e tıklayabilirsiniz.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/model-info.png" alt-text="İki model tanımı olan ' model görünümü ' kutusunun bir görünümü, Floor (dtmı: örnek: Floor; 1) ve Oda (dtmı: örnek: Oda; 1). Bir daire içinde bir harf ' ı ' gösteren ' model görüntüle ' simgesi her model için vurgulanır." lightbox="media/quickstart-adt-explorer/model-info.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

### <a name="twins-and-the-twin-graph"></a>TWINS ve ikizi grafiği

Artık bazı modeller Azure dijital TWINS örneğinizi karşıya yüklediğinize göre, model tanımlarını izleyen [**dijital TWINS**](concepts-twins-graph.md) 'leri ekleyebilirsiniz. 

Dijital TWINS, iş ortamınızdaki gerçek varlıkları temsil eder: bir gruptaki algılayıcılar, bir otomobilde ışıklar gibi şeyler veya bu hızlı başlangıçta bir yapı katında odalar gibi şeyler. Belirli bir model türünün (hepsi *Oda* modelini kullanan birden fazla oda gibi) çok sayıda TWINS oluşturabilir ve bunları tam ortamı temsil eden bir **ikizi grafiğine** bağlayabilirsiniz.

Bu bölümde, önceden oluşturulmuş bir grafiğe bağlı olan önceden oluşturulmuş TWINS 'i karşıya yükleyeceksiniz. Grafik iki kat ve iki oda içerir ve aşağıdaki düzende bağlanır:
* *Floor0*
    - *Room0* içerir
* *Floor1*
    - *room1* içerir

#### <a name="import-the-graph"></a>Grafiği içeri aktarma

*GRAFIK görünümü* kutusunda, *grafiği içeri aktar* simgesine basın.

:::image type="content" source="media/quickstart-adt-explorer/import-graph.png" alt-text="Grafik görünümü kutusunda bir simge vurgulanır. Bir buluta işaret eden bir ok gösterir." lightbox="media/quickstart-adt-explorer/import-graph.png":::

Dosya Seçicisi kutusunda, *Azure_Digital_Twins__ADT__explorer/Client/examples* klasörüne gidin ve _**buildingScenario.xlsx**_ elektronik tablo dosyasını seçin. Bu dosya, örnek grafiğin bir açıklamasını içerir. Tamam 'a basın.

Birkaç saniye sonra, ADT Gezgini yüklenecek Grafiğin önizlemesini görüntüleyen bir *Içeri aktarma* görünümü açar.

Grafik karşıya yüklemeyi onaylamak için, *GRAFIK görünümünün* sağ üst köşesindeki *Kaydet* simgesine basın:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/graph-preview-save.png" alt-text="Grafik Önizleme bölmesindeki Kaydet simgesini vurgulama" lightbox="media/quickstart-adt-explorer/graph-preview-save.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

ADT Explorer, istenen TWINS ve aralarında ilişkiler oluşturmak için karşıya yüklenen dosyayı kullanacaktır. Bittiğini göstermek için bir iletişim kutusu görüntülenir. *Close* 'a basın.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/import-success.png" alt-text="Graph import başarısını belirten iletişim kutusu. ' Içeri aktarma başarılı ' i okur. 4 TWINS içeri aktarıldı. 2 ilişki içeri aktarıldı. '" lightbox="media/quickstart-adt-explorer/import-success.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Grafik şimdi ADT Explorer 'a yüklendi. Grafiği görmek için, *GRAFIK Gezgini* kutusundaki *sorgu Çalıştır* düğmesine, ADT gezgin penceresinin üst kısmına gidin. 

:::image type="content" source="media/quickstart-adt-explorer/run-query.png" alt-text="Pencerenin üst kısmına yakın bir şekilde ' Run Query ' okuyan bir düğme vurgulandı" lightbox="media/quickstart-adt-explorer/run-query.png":::

Bu, tüm dijital TWINS 'i seçmek ve göstermek için varsayılan sorguyu çalıştırır. ADT Explorer, hizmetten tüm TWINS 'leri ve ilişkileri alır ve *GRAFIK görünümü* kutusunda bunlar tarafından tanımlanan grafiği çizecek.

## <a name="explore-the-graph"></a>Grafiği keşfet

Şimdi, örnek senaryonun karşıya yüklenen grafiğini görebilirsiniz:

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="İçindeki bir ikizi Graf ile ' Graf görünümü ' kutusunun görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' room0 ' etiketli bir daireye bağlanır.":::

Daireler (Graf "düğümleri") dijital TWINS 'i temsil eder ve satırlar ilişkileri temsil eder. *Floor0* ikizi *Room0* Içerdiğini ve *Floor1* ikizi *room1* içerdiğini görürsünüz.

Bir fare kullanıyorsanız, grafiğin parçalarını tıklatıp sürükleyerek taşıyabilirsiniz.

### <a name="view-twin-properties"></a>İkizi özelliklerini görüntüle 

*ÖZELLIK Gezgini* kutusunda, özelliklerinin ve bunların değerlerinin listesini görmek için bir ikizi seçebilirsiniz. 

*Room0* öğesinin özellikleri şunlardır:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room0.png" alt-text="' Room0 ' $dtId alanı, 70 sıcaklık alanı ve 30 ' a bir nem alanı dahil olmak üzere, Room0 için özellikleri gösteren ' Özellik Gezgini ' kutusunun etrafında vurgulayın." lightbox="media/quickstart-adt-explorer/properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

*Room0* 'ın **70** sıcaklığının olduğunu unutmayın.

*Room1* öğesinin özellikleri şunlardır:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room1.png" alt-text="' Room1 ' $dtId alanı, 80 sıcaklık alanı ve 60 nem alanı dahil olmak üzere, room1 için özellikleri gösteren ' Özellik Gezgini ' kutusunun etrafında vurgulayın." lightbox="media/quickstart-adt-explorer/properties-room1.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

*Room1* 'ın **80** sıcaklığının olduğunu unutmayın.

### <a name="query-the-graph"></a>Grafiği sorgulama

Azure dijital TWINS 'in ana özelliği, ortamınız hakkında soruları yanıtlamak için ikizi grafınızı kolayca ve verimli bir şekilde [sorgulayabilir](concepts-query-language.md) . 

Grafikteki TWINS 'i sorgulamak için bir yol, *özelliklerinden* biridir. Özellikleri temel alan sorgular, ortamınızda ilgilenilmesi gerekebilecek aykırı değerleri bulma dahil olmak üzere çeşitli soruların yanıtlanmasına yardımcı olabilir.

Bu bölümde, şu soruyu yanıtlamak için bir sorgu çalıştıracaksınız: _**75 üzerinde bir sıcaklık ile ortamınızdaki tüm TWINS nedir?**_

Yanıtı görmek için *sorgu Gezgini* kutusunda aşağıdaki sorguyu çalıştırın:

```SQL
SELECT * FROM DigitalTwins T WHERE T.Temperature > 75
```

*Room0* 'ın **70** sıcaklığını ve *room1* **80** sıcaklığını içeren ikizi özelliklerini görüntülemeyi geri çekin. Sonuç olarak, sonuçlarda yalnızca _**room1**_ görünür.
    
:::image type="content" source="media/quickstart-adt-explorer/result-query-property-before.png" alt-text="Yalnızca room1 gösteren özellik sorgusunun sonuçları" lightbox="media/quickstart-adt-explorer/result-query-property-before.png":::

>[!TIP]
> Yukarıdaki sorgu içinde diğer karşılaştırma işleçleri ( *<* , *>* , *=* , veya *! =* ) de desteklenir. Kendi sorularınızı yanıtlamayı denemek için bu, farklı değerleri veya sorguya farklı ikizi özelliklerini takmayı deneyebilirsiniz.

## <a name="edit-data-in-the-graph"></a>Grafikteki verileri düzenleme

Grafiğinizde temsil edilen TWINS özelliklerini düzenlemek için ADT Gezginini kullanabilirsiniz. Bu bölümde **Room0 _'in sıcaklığını 76 olarak tetikliyoruz_**.

Bunu yapmak için, *ÖZELLIK Gezgini* kutusunda *Room0* ' ı seçin.

Bu listedeki Özellikler düzenlenebilir. Yeni bir değer girmeyi etkinleştirmek için **70** sıcaklık değerini seçin. **76** girin ve sıcaklığın **76** olarak güncelleştirilmesi için *Kaydet* simgesine basın.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/new-properties-room0.png" alt-text="Room0 özelliklerini gösteren ' Özellik Gezgini ' kutusu. Sıcaklık değeri, 76 gösteren düzenlenebilir bir kutusudur ve Kaydet simgesinin etrafında bir vurgu vardır." lightbox="media/quickstart-adt-explorer/new-properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Başarılı kaydetme sırasında, güncelleştirmeyi yapmak için Azure dijital TWINS [API 'leri](how-to-use-apis-sdks.md) ile arka planda kullanılan düzeltme eki kodunu görüntüleyen bir *yama bilgileri* penceresi görürsünüz. *Close* 'a basın.

### <a name="query-to-see-the-result"></a>Sonucu görmek için sorgu

Grafiğin, güncelleştirmenizi *Room0* 'un sıcaklığını başarıyla kaydettirdiğini doğrulamak için daha önce sorguyu yeniden çalıştırarak **ortamdaki tüm twıns sürümlerini 75 üzerinde bir sıcaklığa sahip** olacak şekilde yeniden çalıştırın:

```SQL
SELECT * FROM DigitalTwins T WHERE T.Temperature > 75
```

*Room0* 'ın sıcaklığı **70** olarak **76** ' e değiştiği için, her iki TWINS de sonuçta görünür.

:::image type="content" source="media/quickstart-adt-explorer/result-query-property-after.png" alt-text="Özellik sorgusunun sonuçları, hem Room0 hem de room1 gösteriliyor" lightbox="media/quickstart-adt-explorer/result-query-property-after.png":::

## <a name="review-and-contextualize-learnings"></a>İnceleme ve contextulaştır dersleri

Bu hızlı başlangıçta, bir Azure dijital TWINS örneği oluşturdunuz, bu dosyayı ADT Explorer 'a bağladınız ve örnek bir senaryoyla doldurmuş olursunuz. 

Daha sonra grafiği bu şekilde incelediniz...
1. Senaryo hakkında bir soruyu yanıtlamak için sorgu kullanma.
2. Dijital ikizi bir özelliği Düzenle.
3. Güncelleştirmenin bir sonucu olarak yanıtın nasıl değiştiğini görmek için sorguyu yeniden çalıştırın.

Bu alıştırmanın amacı, ortam değişmeye devam ettiği halde ortamınıza ilişkin soruları yanıtlamak için Azure dijital TWINS grafiğini nasıl kullanabileceğinizi gösterir. 

Bu hızlı başlangıçta, sıcaklık güncelleştirmesini el ile yapmış olursunuz, böylece Azure dijital TWINS 'de, telemetri verilerine bağlı olarak güncelleştirmeleri otomatik olarak almaları için dijital TWINS 'i gerçek IoT cihazlarına bağlama işlemi yaygındır. Bu, ortamınızın gerçek durumunu her zaman yansıtan canlı bir grafik oluşturmanızı sağlar ve ortamınızda gerçek zamanlı olarak neler olduğu hakkında bilgi almak için sorguları kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıçta çalışmayı kaydırmak için, önce çalışan konsol uygulamasını sonlandırın. Bu, tarayıcıda ADT gezgin uygulamasıyla bağlantıyı kapatacak ve artık canlı verileri tarayıcıda görüntüleyemeyeceksiniz. Tarayıcı sekmesini kapatabilirsiniz.

Azure dijital TWINS öğreticilerine devam etmeyi planlıyorsanız, bu hızlı başlangıçta kullanılan örnek bu makaleler için yeniden kullanılabilir ve bunu kaldırmanız gerekmez.
 
[!INCLUDE [digital-twins-cleanup-basic.md](../../includes/digital-twins-cleanup-basic.md)]

Son olarak, indirdiğiniz proje örnek klasörünü yerel makinenize ( _**Azure_Digital_Twins__ADT__explorer**_ ) silin. Hem daraltılmış hem de daraltılmış sürümleri silmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar 

Daha sonra, Azure dijital TWINS öğreticilerine devam ederek kendi Azure dijital TWINS senaryonuz ve etkileşim araçlarınızı oluşturun.

> [!div class="nextstepaction"]
> [*Öğretici: istemci uygulamasını kodlayın*](tutorial-code.md)