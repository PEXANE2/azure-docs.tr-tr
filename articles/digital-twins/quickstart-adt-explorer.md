---
title: Hızlı Başlangıç-örnek senaryoyu araştırma
titleSuffix: Azure Digital Twins
description: Hızlı başlangıç-önceden oluşturulmuş bir senaryoyu görselleştirmek ve araştırmak için ADT Explorer örneğini kullanın.
author: baanders
ms.author: baanders
ms.date: 9/24/2020
ms.topic: quickstart
ms.service: digital-twins
ms.openlocfilehash: 09d5ece0afdd5abe3da5ebdeb6a7ae290e1ccf3d
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92372336"
---
# <a name="quickstart---explore-a-sample-azure-digital-twins-scenario-using-adt-explorer"></a>Hızlı başlangıç-ADT Explorer kullanarak örnek bir Azure dijital TWINS senaryosunu araştırma

Azure dijital TWINS sayesinde gerçek ortamlarınızın canlı modellerini oluşturabilir ve bunlarla etkileşim kurabilirsiniz. Bu, ayrı öğelerin **dijital**bir şekilde modellenmesi ve sonra canlı olaylara yanıt verebilecek ve bilgi için sorgulanabilen bir bilgi **grafiğine** bağlanılarak yapılır.

Bu hızlı başlangıçta, [**Azure Digital TWINS (ADT) Gezgini**](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/)adlı örnek bir uygulamanın yardımıyla önceden oluşturulmuş bir Azure dijital TWINS grafiğini araştıracaktır. ADT Explorer bir ortamın dijital temsilini karşıya yüklemenize, Azure dijital TWINS 'teki ortamı göstermek için oluşturulan TWINS ve grafiğin görsel görüntülerini görüntülemenize ve tarayıcı tabanlı, görsel deneyimle diğer yönetim etkinliklerini gerçekleştirmenize olanak tanır.

Hızlı başlangıç aşağıdaki önemli adımları içerir:

1. Azure dijital TWINS örneğini ve ADT Gezginini ayarlama
1. Örnek senaryoyu oluşturmak için önceden oluşturulmuş modelleri ve grafik verilerini karşıya yükleme
1. Oluşturulan senaryo grafiğini keşfet
1. Grafikte değişiklikler yapma

İle birlikte çalıştığınız örnek grafik, iki kattan ve iki odaya sahip bir bina temsil eder. Grafik şöyle görünür:

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil.":::

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir Azure aboneliğine sahip olmanız gerekir. Henüz bir tane yoksa, şimdi **[ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)** .

Ayrıca makinenizde **Node.js** gerekir. Bu bağlantıdaki en son sürümü edinebilirsiniz: [Node.js](https://nodejs.org/).

Son olarak, hızlı başlangıç sırasında kullanmak üzere iki örnek de indirmeniz gerekir:
* **ADT Explorer** örnek uygulaması. Bu örnek, hızlı başlangıçta bir Azure dijital TWINS senaryosu yüklemek ve araştırmak için kullandığınız ana uygulamayı içerir. Uygulamayı almak için şuraya gidin: [Azure Digital TWINS (ADT) Explorer](/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/). İndirmek için *posta indirme* düğmesine basın *. * Makinenize Bu örnek kodun ZIP dosyası. Bu, bir yükler. ZIP klasörünü makinenize _**Azure_Digital_Twins__ADT__explorer.zip**_. Klasörü sıkıştırmayı açın ve dosyaları ayıklayın.
* **Örnek Azure dijital TWINS senaryosu**. Bu, ile çalışmak üzere ADT Gezgini 'ne yüklediğiniz önceden oluşturulmuş bir Azure dijital TWINS grafiğini içerir. Senaryoya ulaşmak için şuraya gidin: [Azure Digital TWINS uçtan uca örnekler](/samples/azure-samples/digital-twins-samples/digital-twins-samples). İndirmek için *posta indirme* düğmesine basın *. * Makinenize Bu örnek kodun ZIP dosyası. Bu, bir yükler. ZIP klasörünü makinenize _**Azure_Digital_Twins_end_to_end_samples.zip**_. Klasörü sıkıştırmayı açın ve dosyaları ayıklayın.

## <a name="set-up-azure-digital-twins-and-adt-explorer"></a>Azure dijital TWINS ve ADT Gezginini ayarlama

Azure dijital TWINS ile çalışmanın ilk adımı, bir **Azure dijital TWINS örneği**kurmak için kullanılır. Hizmetin bir örneğini oluşturduktan sonra, bunu hızlı başlangıçta örnek verilerle doldurabileceksiniz.

Ayrıca, ADT Explorer 'ın bilgisayarınızda çalışması ve Azure dijital TWINS örneğine erişmesi için izinler ayarlayacaksınız. Bu, örneğinizi ve verilerini araştırmak için örnek uygulamayı kullanmanıza imkan sağlar.

### <a name="set-up-azure-digital-twins-instance-and-app-registration"></a>Azure dijital TWINS örneği ve uygulama kaydı ayarlama

İlk olarak, **bir Azure dijital TWINS örneği** ve onunla çalışabilmeniz için gereken kimlik doğrulamasını ayarlayın. Bunu yapmak için [*nasıl yapılır: örnek ve kimlik doğrulama ayarlama*](how-to-set-up-instance-portal.md)konusundaki yönergeleri izleyin. Tercih ettiğiniz deneyiminize bağlı olarak, [Azure Portal](how-to-set-up-instance-portal.md), [clı](how-to-set-up-instance-cli.md)veya [Otomatik Cloud Shell dağıtım betiği örneği](how-to-set-up-instance-scripted.md)için kurulum makalesine sunulur. Yönergelerin tüm sürümleri, her adımı başarıyla tamamlayıp tamamlamadığınızı ve yeni örneğinizi kullanmaya başlamaya hazırlamış olduğunuzu doğrulamaya yönelik adımları da içerir.
* Azure dijital TWINS örneğinizi ayarladıktan sonra, örneğin **_ana bilgisayar adına_** ([portalda bul](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values)) ihtiyacınız olacaktır.

ADT Explorer uygulamasının kimliğini doğrulamak için, bir **uygulama kaydı**da ayarlamanız gerekir. Bunu ayarlamak için [*nasıl yapılır: uygulama kaydı oluşturma*](how-to-create-app-registration.md) ' daki yönergeleri izleyin. 
* Bir uygulama kaydınız olduktan sonra, kaydın **_uygulama (istemci) kimliği_** ve **_Dizin (kiracı) kimliği_** ([bunları portalda bulun](how-to-create-app-registration.md#collect-client-id-and-tenant-id)) gerekir.

### <a name="set-adt-explorer-permissions"></a>ADT gezgin izinlerini ayarla

Daha sonra, oluşturduğunuz Azure dijital TWINS örneğini, yerel olarak barındırılan bir Web uygulaması olan ADT Explorer ile çalışacak şekilde hazırlayın. Azure portal [uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) sayfasını ziyaret edin ve listeden önceki bölümde oluşturduğunuz **uygulama** kaydlarınızın adını seçin.

Kayıt menüsünden *kimlik doğrulaması* ' nı seçin ve *+ Platform Ekle*' ye basın.

:::image type="content" source="media/quickstart-adt-explorer/authentication-pre.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/authentication-pre.png":::

Aşağıdaki *platformları Yapılandır* sayfasında, *Web*' i seçin.
Yapılandırma ayrıntılarını aşağıdaki gibi girin:
* **Yeniden yönlendirme URI 'leri**: bir yeniden yönlendirme URI 'si ekleyin *http://localhost:3000* .
* **Örtük izin**: *erişim belirteçleri*kutusunu işaretleyin.

Son olarak *yapılandırma* ' ya basın.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/authentication-configure-web.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil.":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Artık ADT Explorer 'ın kullanacağı bir Web yapılandırması yapılandırdınız. Azure portal kimlik doğrulaması sekmesi bunu yansıtmalıdır. Aşağıdaki bölümler doğrulandıktan sonra *Kaydet*' i vurun.

:::image type="content" source="media/quickstart-adt-explorer/authentication-post.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil.":::

### <a name="run-and-configure-adt-explorer"></a>ADT Gezginini çalıştırma ve yapılandırma

Sonra, ADT Explorer uygulamasını çalıştırın ve Azure dijital TWINS örneğiniz için yapılandırın.

İndirilen ve sıkıştırılmış olmayan _**Azure_Digital_Twins__ADT__explorer**_ klasörüne gidin. */Client/src Azure_Digital_Twins__ADT__explorer*klasör konumunda bir komut istemi açın.

`npm install`Tüm gerekli bağımlılıkları indirmek için ' i çalıştırın.

Ardından, çalıştırarak uygulamayı başlatın `npm run start` .

Birkaç saniye sonra, bir tarayıcı penceresi açılır ve uygulama tarayıcıda görüntülenir.

:::image type="content" source="media/quickstart-adt-explorer/explorer-blank.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/explorer-blank.png":::

ADT Gezginini ayarladığınız örnekle çalışacak şekilde yapılandırmak için pencerenin üst kısmındaki *oturum aç* düğmesine (aşağıdaki görüntüde gösterildiği gibi) basın. 

:::image type="content" source="media/quickstart-adt-explorer/sign-in.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/sign-in.png":::

[Önkoşul](#prerequisites) bölümünde daha önce topladığınız önemli bilgileri girin:
* Uygulama (istemci) kimliği
* Dizin (kiracı) kimliği
* Azure dijital TWINS örneği URL 'SI, *https://{örnek konak adı}* biçiminde

>[!NOTE]
> Aynı simgeyi seçerek, oturum açma kutusunu tekrar çekmek için bu bilgileri dilediğiniz zaman yeniden ziyaret edebilir/düzenleyebilirsiniz. Bu, geçirilen değerleri tutacak.

> [!TIP]
> Bağlandığınızda bir `SignalRService.subscribe` hata iletisi gösteriliyorsa, Azure Digital TWINS URL 'nizin *https://* ile başladığından emin olun.

Microsoft 'tan bir *izin* iste açılan pencere görürseniz, bu uygulamaya izin verin ve devam etmek için kabul edin.

## <a name="add-the-sample-data"></a>Örnek verileri ekleme

Ardından, örnek senaryoyu ve grafiği ADT Explorer 'a içeri aktaracaksınız.

Örnek senaryo, indirdiğiniz ve daraltılmış  _**Azure_Digital_Twins_end_to_end_samples**_ klasörünüzde bulunur, bu nedenle şimdi klasöre gitmeniz gerekir.

### <a name="models"></a>Modeller

Bir Azure dijital TWINS çözümünün ilk adımı, ortamınız için sözlük tanımlıyor. Bu işlem, ortamınızda var olan varlık türlerini açıklayan özel [**modeller**](concepts-models.md)oluşturularak yapılır. 

Her model, **dijital Ikizi tanım dili (DTDL)** ADLı bir JSON-ld benzeri dilde yazılır ve *özellikleri*, *telemetrisi*, *ilişkileri*ve *bileşenleri*açısından tek bir varlık türünü tanımlar. Daha sonra bu modelleri, bu türlerin belirli örneklerini temsil eden dijital TWINS 'in temeli olarak kullanacaksınız.

Genellikle bir model oluştururken üç adımı tamamlayacaksınız:
1. Model tanımını yazın (örnek çözümün bir parçası olarak zaten yapılmış olan hızlı başlangıçta)
2. Sözdiziminin doğru olduğundan emin olmak için bunu doğrulayın (hızlı başlangıçta, örnek çözümün bir parçası olarak zaten yapılır)
3. Azure dijital TWINS örneğinizi karşıya yükleyin
 
Bu hızlı başlangıçta, model dosyaları sizin için zaten yazılmıştır ve doğrulanıp, indirdiğiniz çözüme dahil edilmiştir. Bu bölümde, derleme ortamının bu bileşenlerini tanımlamak için örneğinizle önceden yazılmış iki modeli karşıya yükleyeceksiniz:
* Floor
* Yı

#### <a name="upload-models"></a>Modelleri karşıya yükle

*Model görünümü* kutusunda *bir model yükle* simgesine basın.

:::image type="content" source="media/quickstart-adt-explorer/upload-model.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/upload-model.png":::
 
1. Görüntülenen dosya Seçicisi kutusunda, indirilen depodaki *Azure_Digital_Twins_end_to_end_samples/adtsampleapp/sampleclientapp/models* klasörüne gidin.
2. *ÜzerindeRoom.js* seçin ve *üzerindeFloor.js*ve Tamam 'a tıklayın. (İsterseniz diğer modelleri karşıya yükleyebilirsiniz, ancak bu hızlı başlangıçta kullanılmaz.)
3. Azure hesabınızda oturum açmanızı isteyen açılan menü iletişim kutusunu izleyin.

>[!NOTE]
>Şu hata iletisini görürseniz: :::image type="content" source="media/quickstart-adt-explorer/error-models-popup.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." border="false"::: 
> Açılır pencere engelleyicinizi devre dışı bırakmayı veya farklı bir tarayıcı kullanmayı deneyin.

ADT Gezgini şimdi bu model dosyalarını Azure dijital TWINS örneğinize yükleyecek. Bunların kolay adlarını ve tam model kimliklerini görüntüleyen *model görünümü* kutusunda gösterilmesi gerekir. Modeldeki DTDL kodunu görmek için model bilgi balonlarını *göster* ' e tıklayabilirsiniz.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/model-info.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/model-info.png":::
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

:::image type="content" source="media/quickstart-adt-explorer/import-graph.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/import-graph.png":::

Dosya Seçicisi kutusunda *Azure_Digital_Twins_end_to_end_samples/adtsampleapp/sampleclientapp* klasörüne gidin ve _**buildingScenario.xlsx**_ elektronik tablo dosyasını seçin. Bu dosya, örnek grafiğin bir açıklamasını içerir. Tamam 'a basın.

Birkaç saniye sonra, ADT Gezgini yüklenecek Grafiğin önizlemesini görüntüleyen bir *Içeri aktarma* görünümü açar.

Grafik karşıya yüklemeyi onaylamak için, *GRAFIK görünümünün*sağ üst köşesindeki *Kaydet* simgesine basın:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/graph-preview-save.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/graph-preview-save.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

ADT Explorer, istenen TWINS ve aralarında ilişkiler oluşturmak için karşıya yüklenen dosyayı kullanacaktır. Bittiğini göstermek için bir iletişim kutusu görüntülenir. *Close*'a basın.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/import-success.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/import-success.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Grafik şimdi ADT Explorer 'a yüklendi. Grafiği görmek için, *GRAFIK Gezgini* kutusundaki *sorgu Çalıştır* düğmesine, ADT gezgin penceresinin üst kısmına gidin. 

:::image type="content" source="media/quickstart-adt-explorer/run-query.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/run-query.png":::

Bu, tüm dijital TWINS 'i seçmek ve göstermek için varsayılan sorguyu çalıştırır. ADT Explorer, hizmetten tüm TWINS 'leri ve ilişkileri alır ve *GRAFIK görünümü* kutusunda bunlar tarafından tanımlanan grafiği çizecek.

## <a name="explore-the-graph"></a>Grafiği keşfet

Şimdi, örnek senaryonun karşıya yüklenen grafiğini görebilirsiniz:

:::image type="content" source="media/quickstart-adt-explorer/graph-view-full.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil.":::

Daireler (Graf "düğümleri") dijital TWINS 'i temsil eder ve satırlar ilişkileri temsil eder. *Floor0* ikizi *Room0*Içerdiğini ve *Floor1* ikizi *room1*içerdiğini görürsünüz.

Bir fare kullanıyorsanız, grafiğin parçalarını tıklatıp sürükleyerek taşıyabilirsiniz.

### <a name="view-twin-properties"></a>İkizi özelliklerini görüntüle 

*ÖZELLIK Gezgini* kutusunda, özelliklerinin ve bunların değerlerinin listesini görmek için bir ikizi seçebilirsiniz. 

*Room0*öğesinin özellikleri şunlardır:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room0.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

*Room0* 'ın **70**sıcaklığının olduğunu unutmayın.

*Room1*öğesinin özellikleri şunlardır:

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/properties-room1.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/properties-room1.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

*Room1* 'ın **80**sıcaklığının olduğunu unutmayın.

### <a name="query-the-graph"></a>Grafiği sorgulama

Azure dijital TWINS 'in ana özelliği, ortamınız hakkında soruları yanıtlamak için ikizi grafınızı kolayca ve verimli bir şekilde [sorgulayabilir](concepts-query-language.md) . 

Grafikteki TWINS 'i sorgulamak için bir yol, *özelliklerinden*biridir. Özellikleri temel alan sorgular, ortamınızda ilgilenilmesi gerekebilecek aykırı değerleri bulma dahil olmak üzere çeşitli soruların yanıtlanmasına yardımcı olabilir.

Bu bölümde, şu soruyu yanıtlamak için bir sorgu çalıştıracaksınız: _ **75 üzerinde bir sıcaklık ile ortamınızdaki tüm TWINS nedir?**_

Yanıtı görmek için *sorgu Gezgini* kutusunda aşağıdaki sorguyu çalıştırın:

```SQL
SELECT * FROM DigitalTwins T WHERE T.Temperature > 75
```

*Room0* 'ın **70** sıcaklığını ve *room1* **80**sıcaklığını içeren ikizi özelliklerini görüntülemeyi geri çekin. Sonuç olarak, sonuçlarda yalnızca _**room1**_ görünür.
    
:::image type="content" source="media/quickstart-adt-explorer/result-query-property-before.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/result-query-property-before.png":::

>[!TIP]
> Yukarıdaki sorgu içinde diğer karşılaştırma işleçleri ( *<* , *>* , *=* , veya *! =*) de desteklenir. Kendi sorularınızı yanıtlamayı denemek için bu, farklı değerleri veya sorguya farklı ikizi özelliklerini takmayı deneyebilirsiniz.

## <a name="edit-data-in-the-graph"></a>Grafikteki verileri düzenleme

Grafiğinizde temsil edilen TWINS özelliklerini düzenlemek için ADT Gezginini kullanabilirsiniz. Bu bölümde **Room0 _'in sıcaklığını 76 olarak tetikliyoruz_ **.

Bunu yapmak için, *ÖZELLIK Gezgini* kutusunda *Room0*' ı seçin.

Bu listedeki Özellikler düzenlenebilir. Yeni bir değer girmeyi etkinleştirmek için **70** sıcaklık değerini seçin. **76**girin ve sıcaklığın **76**olarak güncelleştirilmesi için *Kaydet* simgesine basın.

:::row:::
    :::column:::
        :::image type="content" source="media/quickstart-adt-explorer/new-properties-room0.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/new-properties-room0.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Başarılı kaydetme sırasında, güncelleştirmeyi yapmak için Azure dijital TWINS [API 'leri](how-to-use-apis-sdks.md) ile arka planda kullanılan düzeltme eki kodunu görüntüleyen bir *yama bilgileri* penceresi görürsünüz. *Close*'a basın.

### <a name="query-to-see-the-result"></a>Sonucu görmek için sorgu

Grafiğin, güncelleştirmenizi *Room0*'un sıcaklığını başarıyla kaydettirdiğini doğrulamak için daha önce sorguyu yeniden çalıştırarak **ortamdaki tüm twıns sürümlerini 75 üzerinde bir sıcaklığa sahip**olacak şekilde yeniden çalıştırın:

```SQL
SELECT * FROM DigitalTwins T WHERE T.Temperature > 75
```

*Room0* 'ın sıcaklığı **70** olarak **76**' e değiştiği için, her iki TWINS de sonuçta görünür.

:::image type="content" source="media/quickstart-adt-explorer/result-query-property-after.png" alt-text="Oklarla bağlı 4 dairesel düğüm oluşan bir grafik görünümü. ' Floor1 ' etiketli bir daire, ' Contains ' etiketli ve ' room1 ' etiketli bir daireye bağlandı; ' Floor0 ' etiketli bir daire, ' Contains ' etiketli bir okla ' Room0 ' etiketli bir daireye bağlanır. ' Floor1 ' ve ' Floor0 ' bağlı değil." lightbox="media/quickstart-adt-explorer/result-query-property-after.png":::

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

Ardından, aşağıdaki komutla istemci uygulamanız için oluşturduğunuz Azure Active Directory Uygulama kaydını silin:

```azurecli
az ad app delete --id <your-application-ID>
```

Son olarak, indirdiğiniz proje örnek klasörlerini yerel makinenize silin (_**Azure_Digital_Twins__ADT__explorer**_ ve _**Azure_Digital_Twins_end_to_end_samples**_). Hem daraltılmış hem de daraltılmış sürümleri silmeniz gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar 

Daha sonra, Azure dijital TWINS öğreticilerine devam ederek kendi Azure dijital TWINS senaryonuz ve etkileşim araçlarınızı oluşturun.

> [!div class="nextstepaction"]
> [*Öğretici: istemci uygulamasını kodlayın*](tutorial-code.md)