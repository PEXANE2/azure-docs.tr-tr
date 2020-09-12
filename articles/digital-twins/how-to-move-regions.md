---
title: Örneği farklı bir Azure bölgesine taşıma
titleSuffix: Azure Digital Twins
description: Azure dijital TWINS örneğini bir Azure bölgesinden diğerine taşıma bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 08/26/2020
ms.topic: how-to
ms.custom: subject-moving-resources
ms.service: digital-twins
ms.openlocfilehash: e2cb8ee282666d7a9a567ca04762b26de3b3b9bd
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89443050"
---
# <a name="move-an-azure-digital-twins-instance-to-a-different-azure-region"></a>Azure dijital TWINS örneğini farklı bir Azure bölgesine taşıma

Azure dijital TWINS örneğinizi bir bölgeden diğerine taşımanız gerekiyorsa, geçerli işlem **kaynaklarınızın yeni bölgede yeniden oluşturulması**ve ardından orijinal kaynakları silmesi olacaktır. Bu işlemin sonunda, güncelleştirilmiş konum dışında, ilki ile özdeş olan yeni bir Azure dijital TWINS örneğiyle birlikte çalışmaya sunulacaktır.

Bu makalede, yeni örneği orijinal ile eşleştirmek için ihtiyacınız olan her şeyi kopyalamaya yönelik bir taşıma işleminin nasıl yapılacağı hakkında yönergeler sunulmaktadır.

Bu işlem aşağıdaki adımları içerir:
1. Hazırlama: özgün modellerinizi, TWINS 'i ve grafiği Indirin.
2. Taşı: yeni bir bölgede yeni bir Azure dijital TWINS örneği oluşturun.
3. Taşı: yeni Azure dijital TWINS örneğini yeniden doldurmak.
    - Özgün modelleri, TWINS 'i ve grafiği karşıya yükleyin.
    - Uç noktaları ve yolları yeniden oluşturun.
    - Bağlı kaynakları yeniden bağlayın.
4. Kaynak kaynaklarını Temizleme: özgün örneği silin.

## <a name="prerequisites"></a>Ön koşullar

Azure dijital TWINS örneğinizi yeniden oluşturmaya çalışmadan önce, özgün örneğinizin bileşenlerinin üzerine gidip, yeniden oluşturulması gereken tüm parçaların net bir fikrini edinmeniz iyi bir fikirdir.

Göz önünde bulundurmanız isteyebileceğiniz bazı sorular şunlardır:
* Örneğim ne tür **modeller** karşıya yüklendi? Kaç tane var?
* Örneğimde **TWINS** nedir? Kaç tane var?
* Örneðimde **grafiğin** genel şekli nedir? Kaç ilişki var?
* Örneğimde hangi **uç noktalara** sahibim?
* Örneğimde hangi **yollara** sahibim? Filtreler mi var?
* Örnek, **diğer Azure hizmetlerine nereden bağlanır**? Bazı ortak tümleştirme noktaları şunlardır...
    - Event Grid, Olay Hub 'ı veya Service Bus
    - Azure İşlevleri
    - Logic Apps
    - Time Series Insights
    - Azure Haritalar
    - Cihaz sağlama hizmeti (DPS)
* Örneğime hangi **kişisel veya Şirket uygulamaları** bağlanmalıyım?

Bu bilgileri [Azure Portal](https://portal.azure.com), [Azure dijital TWINS API 'leri ve SDK](how-to-use-apis-sdks.md)'Ları, [Azure dijital TWINS CLI komutları](how-to-use-cli.md)veya [Azure Digital TWINS (ADT) Gezgini](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/) örneğini kullanarak toplayabilirsiniz.

## <a name="prepare"></a>Hazırlama

Bu bölümde, özgün **modellerinizi, TWINS 'i ve grafiği** özgün örnekten indirerek örneğinizi yeniden oluşturmaya hazırlanacaktır. Bu makalede, [Azure Digital TWINS (ADT) gezgin](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/) örneği kullanılarak yapılır.

>[!NOTE]
>Örnek olarak modeller ve/veya grafik içeren dosyalarınız olabilir. Bu durumda, her şeyi yeniden indirmeniz gerekmez; yalnızca eksik olan veya bu dosyaları (yeni verilerle güncelleştirilmiş olabilecek TWINS gibi) karşıya yüklediğiniz sürece değişmiş olabilecek parçalar.

### <a name="limitations-of-adt-explorer"></a>ADT Explorer sınırlamaları

[Azure dijital TWINS (ADT) gezgin örneği](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/) , grafiğinizin görsel gösterimini destekleyen ve örneğiniz ile görsel etkileşim sağlayan bir istemci uygulaması örneğidir. Bu makalede, modellerinizi, TWINS 'leri ve grafiklerinizi indirmek ve daha sonra yeniden yüklemek için nasıl kullanılacağı gösterilmektedir.

Bununla birlikte, bunun bir **örnek** olduğunu ve bunun bir araç olduğunu lütfen unutmayın. Yük testi sınamamıştır ve büyük boyuttaki grafikleri işlemek için derlenmedi. Sonuç olarak, lütfen aşağıdaki hazır olmayan örnek sınırlamaları aklınızda bulundurun:
* Örnek şu anda yalnızca 1000 düğüme ve 2000 ilişkiye kadar grafik boyutlarında test edilmiştir
* Örnek aralıklı hatalarda yeniden denemeyi desteklemez
* Karşıya yüklenen veriler tamamlanmamışsa, örnek kullanıcıya bildirim gerektirmez
* Örnek, bellek gibi kullanılabilir kaynakları aşan çok büyük grafiklerden kaynaklanan hataları işlemez

Örnek, grafiğinizin boyutunu işleyemeyebilir, diğer Azure dijital TWINS geliştirici araçlarını kullanarak grafiği dışarı ve içeri aktarabilirsiniz:
* [Azure dijital TWINS CLı komutları](how-to-use-cli.md)
* [Azure dijital TWINS API 'Leri ve SDK 'ları](how-to-use-apis-sdks.md)

### <a name="set-up-adt-explorer-application"></a>ADT gezgin uygulamasını ayarlama

ADT Gezgini ile devam etmek için, önce örnek uygulama kodunu indirin ve makinenizde çalışacak şekilde ayarlayın. 

Buradaki örneğe gidin: [Azure Digital TWINS (ADT) Explorer](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/). İndirmek için *posta indirme* düğmesine basın *. * Bu örnek kodun ZIP dosyası _**ADT_Explorer.zip**_ olarak makinenize. Dosyayı sıkıştırmayı açın.

Ardından, ADT Explorer için makinenizde çalıştırılacak izinleri ayarlayın. Bunu yapmak için, Azure Digital TWINS hızlı başlangıç 'nin [*set ADT gezgin izinleri*](quickstart-adt-explorer.md#set-adt-explorer-permissions) bölümündeki adımları izleyin.

Son olarak, ADT Gezginini çalıştırıp özgün Azure dijital TWINS örneğinizi bağlamak üzere çalıştırın. Hızlı başlangıçta bulunan [*ADT Explorer 'ı Çalıştır ve Yapılandır*](quickstart-adt-explorer.md#run-and-configure-adt-explorer) bölümündeki adımları izleyin.

Şimdi makinenizde bir tarayıcıda çalışan ADT Gezgini örnek uygulamasına sahip olmanız gerekir. Örnek, özgün Azure dijital TWINS örneğinize bağlanmalıdır.

:::image type="content" source="media/how-to-move-regions/explorer-blank.png" alt-text="Localhost: 3000 konumunda çalışan bir uygulamayı gösteren tarayıcı penceresi. Uygulama ADT Explorer olarak adlandırılır ve bir sorgu Gezgini, model görünümü, grafik görünümü ve Özellik Gezgini için kutular içerir. Henüz bir ekran verisi yok." lightbox="media/how-to-move-regions/explorer-blank.png":::

Bağlantıyı doğrulamak için, *Graph Explorer* kutusundaki grafikteki tüm TWINS ve ilişkileri görüntüleyen varsayılan sorguyu çalıştırmak üzere *Sorguyu Çalıştır* düğmesine basabilirsiniz.

:::image type="content" source="media/how-to-move-regions/run-query.png" alt-text="Pencerenin üst kısmına yakın bir şekilde ' Run Query ' okuyan bir düğme vurgulandı" lightbox="media/how-to-move-regions/run-query.png":::

Bu öğeleri hedef bölgede yeni örneğinize yeniden yüklemek için ADT Gezgini 'ni çalışır durumda bırakabilirsiniz.

### <a name="download-models-twins-and-graph"></a>Model, TWINS ve Graf 'yi indirin

Sonra, çözümünüzdeki modelleri, TWINS 'i ve grafiği makinenize indirin.

Bunların tümünü tek seferde indirmek için, önce grafiğin *GRAFIK görünümü* kutusunda tam grafiğin göründüğünden emin olun (bunu, `SELECT * FROM digitaltwins` *sorgu Gezgini* kutusunda varsayılan sorgusunu yeniden çalıştırarak yapabilirsiniz).
 
Sonra *GRAFIK görünümü* kutusunda *grafiği dışarı aktar* simgesine basın.

:::image type="content" source="media/how-to-move-regions/export-graph.png" alt-text="Grafik görünümü kutusunda bir simge vurgulanır. Bir bulutun dışına işaret eden bir ok gösterir." lightbox="media/how-to-move-regions/export-graph.png":::

Bu, *GRAFIK görünümünde*bir *indirme* bağlantısını etkinleştirir. Modelleriniz, TWINS ve ilişkiler dahil olmak üzere sorgu sonucunun JSON tabanlı temsilini indirmek için bunu seçin. Bu, makinenize bir *. JSON* dosyası indirmelidir.

>[!NOTE]
>İndirilen dosyanın farklı bir dosya uzantısı varsa, uzantıyı doğrudan düzenlemenizi ve *. JSON*olarak değiştirmeyi deneyin.

## <a name="move"></a>Taşı

Daha sonra, hedef bölgede yeni bir örnek oluşturup özgün örneğinizin verileri ve bileşenleri ile doldurarak örneğinizin "taşıma" bilgisini tamamlayacaksınız.

### <a name="create-a-new-instance"></a>Yeni örnek oluştur

İlk olarak, **hedef bölgenizde Azure dijital TWINS 'in yeni bir örneğini oluşturun**. Bunu yapmak için [*nasıl yapılır: örnek ve kimlik doğrulama ayarlama*](how-to-set-up-instance-scripted.md)bölümündeki adımları izleyerek bu işaretçileri aklınızda tutun:
* Farklı bir kaynak grubunda **yer alıyorsa, yeni örnek için** aynı adı koruyabilirsiniz. Özgün örneğinizi içeren aynı kaynak grubunu kullanmanız gerekiyorsa, yeni örneğiniz kendi farklı adına sahip olur.
* Bir konum sorulduğunda hedef yeni bölgeyi girin.
* Uygulama kaydını yeniden oluşturmanız **gerekmez** . Yeni örneğiniz, zaten sahip olduğunuz uygulama kaydını yeniden kullanabilir.
    - [Komut dosyalı](how-to-set-up-instance-scripted.md) kurulum makalesini kullanıyorsanız, istendiğinde yeni bir ad girmek yerine mevcut uygulama kaydlarınızın ayrıntılarını yeniden girebilirsiniz.
    - El ile [Portal](how-to-set-up-instance-portal.md) veya [CLI](how-to-set-up-instance-cli.md) kurulum makalelerini kullanıyorsanız, *Azure dijital TWINS örneğini oluşturma* ve *Kullanıcı erişim izinleri ayarlama* adımlarını tamamladıktan sonra durulabiliyor olabilirsiniz. *İstemci uygulamaları için erişim Izinlerini ayarla*ile devam etmeniz gerekmez.

Bu işlem tamamlandıktan sonra, verilerinizi verilerle ayarlamaya devam etmek için yeni örneğinizin **ana bilgisayar adına** ihtiyacınız olacaktır. Kurulum sırasında bunu bir yere görmüyorsanız, Azure portal şimdi almak için [Bu yönergeleri](how-to-set-up-instance-portal.md#verify-success-and-collect-important-values) izleyebilirsiniz.

### <a name="repopulate-old-instance"></a>Eski örneği yeniden doldurmak

Ardından, yeni örneği, özgün kopyanın bir kopyası olacak şekilde ayarlayacaksınız.

#### <a name="upload-original-models-twins-and-graph-using-adt-explorer"></a>ADT gezginini kullanarak özgün modelleri, TWINS 'leri ve grafiği karşıya yükleme

Bu bölümde, modellerinizi, TWINS 'i ve grafiği yeni örneğe yeniden yükleyebilirsiniz. Özgün Örneğinizde herhangi bir model, TWINS veya grafik yoksa veya yeni örneğe taşımak istemiyorsanız, bir [sonraki bölüme](#recreate-endpoints-and-routes)atlayabilirsiniz.

Aksi takdirde, devam etmek için **ADT Gezginini** çalıştıran tarayıcı penceresine dönün ve aşağıdaki adımları izleyin.

##### <a name="connect-to-the-new-instance"></a>Yeni örneğe Bağlan

Şu anda ADT Gezgini özgün Azure dijital TWINS örneğinizle bağlantılı. Pencerenin üst kısmındaki *oturum aç* düğmesine vurarak bağlantıyı yeni örneğinizi işaret etmek üzere değiştirin. 

:::image type="content" source="media/how-to-move-regions/sign-in.png" alt-text="Pencerenin üst kısmındaki oturum açma simgesini vurgulayan ADT Explorer. Simge, bir anahtarın silueti içeren bir kişinin basit silueti gösterir." lightbox="media/how-to-move-regions/sign-in.png":::

Uygulama kaydını yeniden kullandığınız için yalnızca *ADT URL 'sini*değiştirmeniz gerekir. Bu değeri *https://{yeni örnek konak adı}* okuduğu şekilde değiştirin.

*Connect*tuşuna basın. Azure kimlik bilgilerinizle yeniden oturum açmanız ve/veya bu uygulamaya izin vermeniz istenebilir.

##### <a name="upload-models-twins-and-graph"></a>Model, TWINS ve Graf yükleme

Ardından, daha önce indirdiğiniz çözüm bileşenlerini yeni örneğinize yükleyin.

**Modellerinizi, TWINS 'i ve Graf**'yi karşıya yüklemek IÇIN grafik *görünümü* kutusunda *Graf al* simgesine basın. Bu seçenek, bu bileşenlerin üçünü aynı anda (hatta grafikte kullanılmayan modeller) karşıya yükler.

:::image type="content" source="media/how-to-move-regions/import-graph.png" alt-text="Grafik görünümü kutusunda bir simge vurgulanır. Bir buluta işaret eden bir ok gösterir." lightbox="media/how-to-move-regions/import-graph.png":::

Dosya Seçicisi kutusunda, indirdiğiniz grafiğinize gidin. Graph *. JSON* dosyasını seçin ve *Aç*' a tıklayın.

Birkaç saniye sonra, ADT Gezgini yüklenecek Grafiğin önizlemesini görüntüleyen bir *Içeri aktarma* görünümü açar.

Grafik karşıya yüklemeyi onaylamak için, *GRAFIK görünümünün*sağ üst köşesindeki *Kaydet* simgesine basın:

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-move-regions/graph-preview-save.png" alt-text="Grafik Önizleme bölmesindeki Kaydet simgesini vurgulama" lightbox="media/how-to-move-regions/graph-preview-save.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

ADT Explorer artık modellerinizi ve Graf (TWINS ve ilişkiler dahil) yeni Azure dijital TWINS örneğinizi karşıya yükleyecek. Kaç tane model, TWINS ve ilişki yüklendiğini gösteren bir başarı iletisi görmeniz gerekir:

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-move-regions/import-success.png" alt-text="Graph import başarısını belirten iletişim kutusu. ' Içeri aktarma başarılı ' i okur. 2 model içeri aktarıldı. 4 TWINS içeri aktarıldı. 2 ilişki içeri aktarıldı. '" lightbox="media/how-to-move-regions/import-success.png":::
    :::column-end:::
    :::column:::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Her şeyin başarıyla karşıya yüklendiğini doğrulamak için Graph *Explorer* kutusundaki *sorgu Çalıştır* düğmesine basın ve grafikteki tüm TWINS ve ilişkileri görüntüleyen varsayılan sorguyu çalıştırın. Bu, *model görünümündeki*model listesini de yeniler.

:::image type="content" source="media/how-to-move-regions/run-query.png" alt-text="Pencerenin üst kısmına yakın bir şekilde, aynı ' çalıştırma sorgusu ' düğmesini etrafında Vurgula" lightbox="media/how-to-move-regions/run-query.png":::

Grafınızı *GRAFIK Gezgini* kutusunda görünen tüm TWINS ve ilişkilerle görmeniz gerekir. *Model görünümü* kutusunda modellerinizi de görmeniz gerekir.

:::image type="content" source="media/how-to-move-regions/post-upload.png" alt-text="' Model görünümü ' kutusunda vurgulanmış 2 model ve ' Graph Explorer ' kutusunda vurgulanmış bir grafik gösteren ADT Gezgini görünümü" lightbox="media/how-to-move-regions/post-upload.png":::

Bu, modellerinizin, TWINS 'in ve grafiğinin hedef bölgedeki yeni örneğe yeniden yüklendiğini onaylar.

#### <a name="recreate-endpoints-and-routes"></a>Uç noktaları ve yolları yeniden oluşturma

Özgün örneğiniz için **uç noktalarınız ve/veya rotalar** varsa, bunları yeni Örneğinizde yeniden oluşturmanız gerekir. Özgün Örneğinizde herhangi bir uç nokta veya yol yoksa veya yeni örneğe taşımak istemiyorsanız, [sonraki bölüme](#re-link-connected-resources)atlayabilirsiniz.

Aksi takdirde, şu işaretçileri aklınızda tutarak, [*nasıl yapılır: yeni örneği kullanarak uç noktaları ve yolları yönetme*](how-to-manage-routes-portal.md) bölümündeki adımları takip edin: 
* Uç nokta için kullanmakta olduğunuz Event Grid, Olay Hub 'ını veya Service Bus kaynağını yeniden oluşturmanız **gerekmez** (uç nokta yönergelerindeki*Önkoşul* bölümleri). Uç noktayı Azure dijital TWINS örneğinde yeniden oluşturmanız yeterlidir.
* Farklı bir örneğe kapsam yaptığından, uç nokta ve yol **adlarını**yeniden kullanabilirsiniz.
* Oluşturduğunuz yollara gerekli **filtreleri** eklemeyi unutmayın.

#### <a name="re-link-connected-resources"></a>Bağlı kaynakları yeniden bağla

Özgün Azure dijital TWINS örneğinizle bağlantılı başka uygulamalarınız veya Azure kaynaklarınız varsa, bağlantıyı yeni örneğinize ulaşacak şekilde düzenlemeniz gerekir. Bu, Azure dijital TWINS ile çalışmak üzere ayarlamış olduğunuz diğer Azure hizmetlerini veya kişisel veya şirket uygulamalarını içerebilir.

Özgün örneğinizle bağlantılı başka kaynaklarınız yoksa veya yeni örneğe taşımak istemiyorsanız, bir [sonraki bölüme](#verify)atlayabilirsiniz.

Aksi halde devam etmek için, senaryonuza bağlı kaynakları göz önünde bulundurun. Bağlı kaynakları silip yeniden oluşturmanız gerekmez; Bunun yerine, bir Azure dijital TWINS örneğine bağlandıkları noktaları, **ana bilgisayar adı**aracılığıyla düzenlemeniz ve bunu orijinal yerine yeni örneğin ana bilgisayar adını kullanacak şekilde güncelleştirmeniz gerekir.

Düzenlemeniz gereken tam kaynaklar senaryonuza bağlıdır, ancak bazı yaygın tümleştirme noktaları aşağıda verilmiştir:
* Azure Işlevleri. Kodu özgün örneğinin ana bilgisayar adını içeren bir Azure işleviniz varsa, bu değeri yeni örneğin ana bilgisayar adına güncelleştirmeniz ve işlevi yeniden yayımlamanız gerekir.
* Event Grid, Event Hubs veya Service Bus
* Logic Apps
* Time Series Insights
* Azure Haritalar
* Cihaz sağlama hizmeti (DPS)
* Azure dışındaki kişisel veya Şirket uygulamaları (öğreticide oluşturulan **istemci uygulaması** gibi), örneğe bağlanan ve Azure Digital TWINS API 'lerini çağıran [*bir istemci uygulamasını kodlayın*](tutorial-code.md)

Bu adımı tamamladıktan sonra, hedef bölgedeki yeni örneğinizin özgün örneğin bir kopyası olması gerekir.

## <a name="verify"></a>Doğrulama

Yeni örneğinizin doğru şekilde ayarlandığını doğrulamak için aşağıdaki araçları kullanabilirsiniz:
* [**Azure Portal**](https://portal.azure.com) (yeni örneğinizin var olduğunu ve doğru hedef bölgede olduğunu doğrulamak için iyidir; Ayrıca, uç noktaların ve yolların doğrulanması ve diğer Azure hizmetleriyle bağlantı için de iyidir)
* [Azure dijital TWINS **CLI komutları** ](how-to-use-cli.md) (yeni örneğinizin var olduğunu ve doğru hedef bölgede olduğunu doğrulamak için iyi, örnek verileri doğrulamak için de kullanılabilir)
* [**ADT Explorer**](https://docs.microsoft.com/samples/azure-samples/digital-twins-explorer/digital-twins-explorer/) (modeller, TWINS ve grafik gibi örnek verileri doğrulamak için iyi)
* [Azure dijital TWINS API 'leri ve SDK 'lar](how-to-use-apis-sdks.md) (modeller, TWINS ve grafik gibi örnek verileri doğrulamak için iyi) ve uç noktaların ve yolların doğrulanması için de iyidir)

Yeni örnekle doğru çalıştıklarından emin olmanıza yardımcı olması için, özgün örneğiniz ile çalıştırdığınız özel uygulamaları veya uçtan uca akışları çalıştırmayı da deneyebilirsiniz.

## <a name="clean-up-source-resources"></a>Kaynak kaynaklarını Temizleme

Yeni örneğiniz, özgün örneğin verilerinin ve bağlantılarının bir kopyasına sahip hedef bölgede ayarlandığına göre **özgün örneği silebilirsiniz**.

Bunu, [CLI](how-to-use-cli.md)ile veya [Denetim düzlemi API 'leriyle](how-to-use-apis-sdks.md#overview-control-plane-apis) [Azure Portal](https://portal.azure.com)yapabilirsiniz.

Azure portal kullanarak örneği silmek için, portalı bir tarayıcı penceresinde [açın](https://portal.azure.com) ve Portal arama çubuğunda adını arayarak özgün Azure dijital TWINS örneğinizi bulun.

*Sil* düğmesine basın ve istemleri izleyerek silme işlemini tamamlayın.

:::image type="content" source="media/how-to-move-regions/delete-instance.png" alt-text="Genel Bakış sekmesinde Azure portal Azure dijital TWINS örneği ayrıntılarının görünümü. Sil düğmesi vurgulanır":::