---
title: "Öğretici: Azure Haritalar'ı kullanarak mağaza bulucu uygulaması oluşturun | Microsoft Azure Haritaları"
description: Bu eğitimde, Microsoft Azure Maps web SDK'yı kullanarak mağaza bulucu web uygulaması oluşturmayı öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 01/14/2020
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 49b57b213a452d6c594bbc1ca537e68bd7a83864
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80333849"
---
# <a name="tutorial-create-a-store-locator-by-using-azure-maps"></a>Öğretici: Azure Haritalar'ı kullanarak mağaza bulucu oluşturma

Bu öğretici, Azure Haritalar'ı kullanarak basit bir mağaza bulucu oluşturma sürecinde size yol gösteriş eder. Mağaza bulucuları yaygındır. Bu tür uygulamalarda kullanılan kavramların çoğu diğer birçok uygulama türü için geçerlidir. Müşterilere bir mağaza bulucu sunan tüketicilere doğrudan satmak çoğu işletme için bir zorunluluktur. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
    
> [!div class="checklist"]
> * Azure Harita Denetimi API'sini kullanarak yeni bir web sayfası oluşturun.
> * Bir dosyadan özel verileri yükleyin ve haritada görüntüleyin.
> * Bir adres bulmak veya bir sorgu girmek için Azure Haritalar Arama hizmetini kullanın.
> * Kullanıcının konumunu tarayıcıdan alın ve haritada gösterin.
> * Haritada özel semboller oluşturmak için birden çok katmanı birleştirin.  
> * Veri noktalarını kümele.  
> * Haritaya yakınlaştırma denetimleri ekleyin.

<a id="Intro"></a>

Canlı mağaza [bulucu örneğine](https://azuremapscodesamples.azurewebsites.net/?sample=Simple%20Store%20Locator) veya [kaynak koduna](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator)atlayın. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticideki adımları tamamlamak için öncelikle bir Azure Haritalar hesabı oluşturmanız ve birincil anahtarınızı (abonelik anahtarı) almanız gerekir. S1 fiyatlandırma katmanına sahip bir Azure Haritalar hesabı aboneliği oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin ve hesabınızın birincil anahtarını almak için [birincil anahtarı al'daki](quick-demo-map-app.md#get-the-primary-key-for-your-account) adımları izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](how-to-manage-authentication.md)bakın.

## <a name="design"></a>Tasarım

Koda atlamadan önce, bir tasarımla başlamak iyi bir fikirdir. Mağaza bulucunuz istediğiniz kadar basit veya karmaşık olabilir. Bu öğreticide, basit bir mağaza bulucu oluşturmak. İsterseniz bazı işlevleri genişletmenize yardımcı olmak için yol boyunca bazı ipuçları ekleriz. Contoso Coffee adında kurgusal bir şirket için bir mağaza bulucusu oluşturuyoruz. Aşağıdaki şekil, bu öğreticide oluşturduğumuz mağaza bulucusu genel düzeni bir tel çerçevesi gösterir:

<center>

![Contoso Coffee mağaza yerleri için mağaza bulucu uygulamasının tel kafesi](./media/tutorial-create-store-locator/SimpleStoreLocatorWireframe.png)</center>

Bu mağaza bulucunun kullanışlılığını en üst düzeye çıkarmak için, bir kullanıcının ekran genişliği 700 pikselden daha küçük olduğunda ayarlayan duyarlı bir düzen ekleriz. Duyarlı bir mizanpaj, mağaza bulucuyu mobil aygıtta olduğu gibi küçük bir ekranda kullanmayı kolaylaştırır. Burada küçük ekran düzeni bir tel çerçevesi' s:  

<center>

![Mobil cihazda Contoso Coffee mağaza bulucu uygulamasının tel kafesi](./media/tutorial-create-store-locator/SimpleStoreLocatorMobileWireframe.png)</center>

Tel kafesler oldukça basit bir uygulama göstermektedir. Uygulamanın bir arama kutusu, yakındaki mağazaların bir listesi ve semboller gibi bazı işaretçileri olan bir harita vardır. Ayrıca, kullanıcı bir işaretçi seçtiğinde ek bilgiler görüntüleyen bir açılır pencereye sahiptir. Daha ayrıntılı olarak, burada bu öğretici bu mağaza bulucu içine inşa özellikleri şunlardır:

* Alınan sekme-sınırlandırılmış veri dosyasından tüm konumlar haritaya yüklenir.
* Kullanıcı haritayı kaydırıp yakınlaştırabilir, arama yapabilir ve Konumum GPS düğmesini seçebilir.
* Sayfa düzeni, aygıt ekranının genişliğine göre ayarlanır.  
* Üstbilgi mağaza logosunu gösterir.  
* Kullanıcı, adres, posta kodu veya şehir gibi bir konumu aramak için bir arama kutusu ve arama düğmesini kullanabilir. 
* Arama `keypress` kutusuna eklenen bir olay, kullanıcı Enter tuşuna baslarsa aramayı tetikler. Bu işlevsellik genellikle göz ardı edilir, ancak daha iyi bir kullanıcı deneyimi oluşturur.
* Harita hareket ettiğinde, haritanın merkezinden her konuma olan uzaklık hesaplanır. Sonuç listesi, haritanın en üstündeki en yakın konumları görüntülemek için güncelleştirilir.  
* Sonuç listesinde bir sonuç seçtiğinizde, harita seçili konumun üzerine ortalanır ve konumla ilgili bilgiler açılır pencerede görünür.  
* Haritada belirli bir konumu n seçilmesi de açılır pencereyi tetikler.
* Kullanıcı uzaklaştırdığında, konumlar kümeler halinde gruplanır. Kümeler, daireiçinde bir sayı olan bir daire ile temsil edilir. Kullanıcı yakınlaştırma düzeyini değiştirdiğinde kümeler oluşur ve ayrılır.
* Bir küme seçmek, haritada iki düzeyde yakınlaştırır ve kümenin konumu üzerinde ortalar.

<a id="create a data-set"></a>

## <a name="create-the-store-location-dataset"></a>Mağaza konumu veri kümesini oluşturma

Bir mağaza bulucu uygulaması geliştirmeden önce, haritada görüntülemek istediğimiz mağazaların bir veri kümesini oluşturmamız gerekir. Bu eğitimde, Contoso Coffee adlı hayali bir kahve dükkanı için bir veri seti kullanıyoruz. Bu basit depo bulucuiçin veri kümesi bir Excel çalışma kitabında yönetilir. Veri seti, dokuz ülkeye/bölgeye yayılmış 10.213 Contoso Coffee kahve dükkanı yeri içerir: Amerika Birleşik Devletleri, Kanada, İngiltere, Fransa, Almanya, İtalya, Hollanda, Danimarka ve İspanya. Verilerin nasıl göründüğünün ekran görüntüsü aşağıda vereyim:

<center>

![Excel çalışma kitabındaki depo bulucu verilerinin ekran görüntüsü](./media/tutorial-create-store-locator/StoreLocatorDataSpreadsheet.png)</center>

Excel [çalışma kitabını indirebilirsiniz.](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator/data) 

Verilerin ekran görüntüsüne baktığımızda aşağıdaki gözlemleri yapabiliriz:
    
* Konum bilgileri **Adres Hattı,** **Şehir,** **Belediye** (ilçe), **Yönetici Bölümü** (eyalet/il), **Posta Kodu** (posta kodu) ve **Ülke** sütunları kullanılarak depolanır.  
* **Enlem** ve **Boylam** sütunları her Contoso Coffee kahve dükkanı nın konumunun koordinatlarını içerir. Koordinat bilgileriniz yoksa, konum koordinatlarını belirlemek için Azure Haritalar'daki Arama hizmetlerini kullanabilirsiniz.
* Bazı ek sütunlar kafelerle ilgili meta veriler içerir: telefon numarası, Boolean sütunları ve mağaza açılış ve kapanış saatleri 24 saat biçiminde. Boolean sütunları Wi-Fi ve tekerlekli sandalye erişilebilirliği içindir. Konum verilerinizle daha alakalı meta veriler içeren kendi sütunlarınızı oluşturabilirsiniz.

> [!Note]
> Azure Haritalar, küresel Mercator projeksiyonunda verileri "EPSG:3857" olarak işler, ancak WGS84 datum'u kullanan "EPSG:4325"deki verileri okur. 

Veri kümesini uygulamaya maruz bırakmanın birçok yolu vardır. Bir yaklaşım, verileri bir veritabanına yüklemek ve verileri sorgulayan bir web hizmetini ortaya çıkarmaktır. Daha sonra sonuçları kullanıcının tarayıcısına gönderebilirsiniz. Bu seçenek, büyük veri kümeleri veya sık sık güncelleştirilen veri kümeleri için idealdir. Ancak, bu seçenek daha fazla geliştirme çalışması gerektirir ve daha yüksek bir maliyeti vardır. 

Başka bir yaklaşım, bu veri kümesini tarayıcının kolayca ayrıştırabileceği düz bir metin dosyasına dönüştürmektir. Dosyanın kendisi, uygulamanın geri kalanıyla birlikte barındırılabilir. Bu seçenek her şeyi basit tutar, ancak kullanıcı tüm verileri karşıdan yüklediği için yalnızca daha küçük veri kümeleri için iyi bir seçenek. Veri dosyası boyutu 1 MB'dan küçük olduğundan, bu veri kümesi için düz metin dosyasını kullanırız.  

Çalışma kitabını düz bir metin dosyasına dönüştürmek için çalışma kitabını sekme yle sınırlandırılmış dosya olarak kaydedin. Her sütun, sütunların kodumuzda ayrıştırılamasını kolaylaştıran bir sekme karakteriyle sınırlandırılır. Virgülle ayrılmış değer (CSV) biçimini kullanabilirsiniz, ancak bu seçenek daha fazla ayrıştırma mantığı gerektirir. Etrafında virgül olan herhangi bir alan tırnak işaretleriile sarılı olacaktır. Bu verileri Excel'de sekme yle sınırlandırılmış dosya olarak dışa aktarmak için **Farklı Kaydet'i**seçin. Tür açılır listesinde **Kaydet'te** **Metin (Sekme delimited)(*.txt)** seçeneğini belirleyin. *DosyacontosoCoffee.txt*adı . 

<center>

![Tür iletişim kutusu olarak kaydet ekran görüntüsü](./media/tutorial-create-store-locator/SaveStoreDataAsTab.png)</center>

Not Defteri'nde metin dosyasını açarsanız, aşağıdaki şekilde benzer görünür:

<center>

![Sekme sınırlandırılabilen bir veri kümesini gösteren notepad dosyasının ekran görüntüsü](./media/tutorial-create-store-locator/StoreDataTabFile.png)</center>


## <a name="set-up-the-project"></a>Projeyi ayarlama

Projeyi oluşturmak için [Visual Studio'yu](https://visualstudio.microsoft.com) veya seçtiğiniz kod düzenleyicisini kullanabilirsiniz. Proje klasörünüzde üç dosya oluşturun: *index.html*, *index.css*ve *index.js*. Bu dosyalar, uygulamanın düzenini, stilini ve mantığını tanımlar. *Veri* adında bir klasör oluşturun ve *klasöre ContosoCoffee.txt* ekleyin. *Resim*adlı başka bir klasör oluşturun. Bu uygulamada haritadaki simgeler, düğmeler ve işaretçiler için 10 resim kullanıyoruz. Bu [resimleri indirebilirsiniz.](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator/data) Proje klasörünüz artık aşağıdaki şekilde görünmelidir:

<center>

![Basit Depo Bulucu proje klasörünün ekran görüntüsü](./media/tutorial-create-store-locator/StoreLocatorVSProject.png)</center>

## <a name="create-the-user-interface"></a>Kullanıcı arabirimini oluşturma

Kullanıcı arabirimi oluşturmak için *index.html*kodu ekleyin:

1. `meta` *Index.html'e* `head` aşağıdaki etiketleri ekleyin. Etiket `charset` karakter kümesini (UTF-8) tanımlar. Değeri, `http-equiv` Internet Explorer ve Microsoft Edge'e en son tarayıcı sürümlerini kullanmalarını söyler. Ve son `meta` etiket, duyarlı düzenler için iyi çalışan bir görünüm portu belirtir.

    ```HTML
    <meta charset="utf-8">
    <meta http-equiv="x-ua-compatible" content="IE=Edge">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

1. Azure Haritalar web denetimi JavaScript ve CSS dosyalarına referanslar ekleyin:

    ```HTML
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css">
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>
    ```

1. Azure Haritalar Hizmetleri modülüne bir başvuru ekleyin. Modül, Azure Haritalar REST hizmetlerini saran ve JavaScript'te kullanımını kolaylaştıran bir JavaScript kitaplığıdır. Modül, arama işlevselliğini güçlandırmak için kullanışlıdır.

    ```HTML
    <script src="https://atlas.microsoft.com/sdk/javascript/service/2/atlas-service.min.js"></script>
    ```

1. *index.js ve index.css'ye* referans lar ekleyin: *index.css*

    ```HTML
    <link rel="stylesheet" href="index.css" type="text/css">
    <script src="index.js"></script>
    ```

1. Belgenin gövdesine bir `header` etiket ekleyin. Etiketin `header` içine logoyu ve şirket adını ekleyin.

    ```HTML
    <header>
        <img src="images/Logo.png" />
        <span>Contoso Coffee</span>
    </header>
    ```

1. Bir `main` etiket ekleyin ve metin kutusu ve arama düğmesi olan bir arama paneli oluşturun. Ayrıca, `div` harita, liste paneli ve Konumum GPS düğmesi için referanslar ekleyin.

    ```HTML
    <main>
        <div class="searchPanel">
            <div>
                <input id="searchTbx" type="search" placeholder="Find a store" />
                <button id="searchBtn" title="Search"></button>
            </div>
        </div>
        <div id="listPanel"></div>
        <div id="myMap"></div>
        <button id="myLocationBtn" title="My Location"></button>
    </main>
    ```

Bitirdikten *sonra, index.html* bu [örnek index.html dosyası](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator/index.html)gibi görünmelidir.

Bir sonraki adım CSS stilleri tanımlamaktır. CSS stilleri, uygulama bileşenlerinin nasıl ortaya konulduruluşlarını ve uygulamanın görünümünü tanımlar. *Index.css'yi* açın ve aşağıdaki kodu ekleyin. Stil, `@media` ekran genişliği 700 pikselden küçükolduğunda kullanılacak alternatif stil seçeneklerini tanımlar.  

   ```CSS
    html, body {
        padding: 0;
        margin: 0;
        font-family: Gotham, Helvetica, sans-serif;
        overflow-x: hidden;
    }

    header {
        width: calc(100vw - 10px);
        height: 30px;
        padding: 15px 0 20px 20px;
        font-size: 25px;
        font-style: italic;
        font-family: "Comic Sans MS", cursive, sans-serif;
        line-height: 30px;
        font-weight: bold;
        color: white;
        background-color: #007faa;
    }

    header span {
        vertical-align: middle;
    }

    header img {
        height: 30px;
        vertical-align: middle;
    }

    .searchPanel {
        position: relative;
        width: 350px;
    }

    .searchPanel div {
        padding: 20px;
    }

    .searchPanel input {
        width: calc(100% - 50px);
        font-size: 16px;
        border: 0;
        border-bottom: 1px solid #ccc;
    }

    #listPanel {
        position: absolute;
        top: 135px;
        left: 0px;
        width: 350px;
        height: calc(100vh - 135px);
        overflow-y: auto;
    }

    #myMap { 
        position: absolute;
        top: 65px;
        left: 350px;
        width: calc(100vw - 350px);
        height: calc(100vh - 65px);
    }

    .statusMessage {
        margin: 10px;
    }

    #myLocationBtn, #searchBtn {
        margin: 0;
        padding: 0;
        border: none;
        border-collapse: collapse;
        width: 32px;
        height: 32px; 
        text-align: center;
        cursor: pointer;
        line-height: 32px;
        background-repeat: no-repeat;
        background-size: 20px;
        background-position: center center;
        z-index: 200;
    }

    #myLocationBtn {
        position: absolute;
        top: 150px;
        right: 10px;
        box-shadow: 0px 0px 4px rgba(0,0,0,0.16);
        background-color: white;
        background-image: url("images/GpsIcon.png");
    }

    #myLocationBtn:hover {
        background-image: url("images/GpsIcon-hover.png");
    }

    #searchBtn {
        background-color: transparent;
        background-image: url("images/SearchIcon.png");
    }

    #searchBtn:hover {
        background-image: url("images/SearchIcon-hover.png");
    }

    .listItem {
        height: 50px;
        padding: 20px;
        font-size: 14px;
    }

    .listItem:hover {
        cursor: pointer;
        background-color: #f1f1f1;
    }

    .listItem-title {
        color: #007faa;
        font-weight: bold;
    }

    .storePopup {
        min-width: 150px;
    }

    .storePopup .popupTitle {
        border-top-left-radius: 4px;
        border-top-right-radius: 4px;
        padding: 8px;
        height: 30px;
        background-color: #007faa;
        color: white;
        font-weight: bold;
    }

    .storePopup .popupSubTitle {
        font-size: 10px;
        line-height: 12px;
    }

    .storePopup .popupContent {
        font-size: 11px;
        line-height: 18px;
        padding: 8px;
    }

    .storePopup img {
        vertical-align:middle;
        height: 12px;
        margin-right: 5px;
    }

    /* Adjust the layout of the page when the screen width is less than 700 pixels. */
    @media screen and (max-width: 700px) {
        .searchPanel {
            width: 100vw;
        }

        #listPanel {
            top: 385px;
            width: 100%;
            height: calc(100vh - 385px);
        }

        #myMap {
            width: 100vw;
            height: 250px;
            top: 135px;
            left: 0px;
        }

        #myLocationBtn {
            top: 220px;
        }
    }

    .mapCenterIcon {
        display: block;
        width: 10px;
        height: 10px;
        border-radius: 50%;
        background: orange;
        border: 2px solid white;
        cursor: pointer;
        box-shadow: 0 0 0 rgba(0, 204, 255, 0.4);
        animation: pulse 3s infinite;
    }

    @keyframes pulse {
        0% {
            box-shadow: 0 0 0 0 rgba(0, 204, 255, 0.4);
        }

        70% {
            box-shadow: 0 0 0 50px rgba(0, 204, 255, 0);
        }

        100% {
            box-shadow: 0 0 0 0 rgba(0, 204, 255, 0);
        }
    }
   ```

Uygulamayı şimdi çalıştırın, üstbilgi, arama kutusu ve arama düğmesini görürsünüz. Ancak harita henüz yüklenmediğinden görünmüyor. Bir arama yapmaya çalışırsanız, hiçbir şey olmaz. Bir sonraki bölümde açıklanan JavaScript mantığını ayarlamamız gerekir. Bu mantık, mağaza bulucunun tüm işlevlerine erişir.

## <a name="wire-the-application-with-javascript"></a>Uygulamayı JavaScript ile kablola

Artık her şey kullanıcı arabiriminde ayarlandı. Verileri yüklemek ve ayrışdırmak ve ardından verileri harita üzerinde işlemek için JavaScript'i eklememiz gerekir. Başlamak için *index.js'yi* açın ve aşağıdaki adımlarda açıklandığı gibi kod ekleyin.

1. Ayarların güncelleştirilen kolay olmasını sağlamak için genel seçenekler ekleyin. Harita, açılan pencere, veri kaynağı, simge katmanı ve HTML işaretçisi için değişkenleri tanımlayın. HTML işaretçisini bir arama alanının merkezini gösterecek şekilde ayarlayın. Ayrıca, Azure Haritalar arama hizmeti istemcisinin bir örneğini tanımlayın.

    ```JavaScript
    //The maximum zoom level to cluster data point data on the map.
    var maxClusterZoomLevel = 11;

    //The URL to the store location data.
    var storeLocationDataUrl = 'data/ContosoCoffee.txt';

    //The URL to the icon image.
    var iconImageUrl = 'images/CoffeeIcon.png';
    var map, popup, datasource, iconLayer, centerMarker, searchURL;
    ```

1. *index.js'ye*kod ekleyin. Aşağıdaki kod haritayı başlatılmasını. Sayfa yükleme tamamlanana kadar beklemek için bir [olay dinleyicisi](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) ekledik. Daha sonra, haritanın yüklenmesini izlemek ve arama düğmesine ve Konumum düğmesine işlevsellik sağlamak için olayları bağladık.

   Kullanıcı arama düğmesini seçtiğinde veya arama kutusuna bir konum yazdığında enter tuşuna bastığında, kullanıcının sorgusuna karşı bulanık bir arama başlatılır. Arama sonuçlarını bu ülkelerle/bölgelerle `countrySet` sınırlama seçeneğine bir dizi ülke ISO 2 değeri aktarın. Arama yapmak için ülkelerin/bölgelerin sınırlandırılması, döndürülen sonuçların doğruluğunu artırmaya yardımcı olur. 
  
   Arama tamamlandıktan sonra, ilk sonucu alın ve harita kamerasını o alanın üzerine ayarlayın. Kullanıcı Konumum düğmesini seçtiğinde, HTML5 Coğrafi Konum API'sını kullanarak kullanıcının konumunu alın. Bu API tarayıcıda yerleşiktir. Ardından, haritayı konumlarının üzerine ortalayın.  

   > [!Tip]
   > Açılır pencereleri kullandığınızda, tek `Popup` bir örnek oluşturmak ve içeriğini ve konumunu güncelleştirerek örneği yeniden kullanmak en iyisidir. Kodunuza `Popup`eklediğiniz her örnek için sayfaya birden çok DOM öğesi eklenir. Bir sayfada ne kadar çok DOM öğesi varsa, tarayıcının izlemesi gereken o kadar çok şey olur. Çok fazla öğe varsa, tarayıcı yavaş olabilir.

    ```JavaScript
    function initialize() {
        //Initialize a map instance.
        map = new atlas.Map('myMap', {
            center: [-90, 40],
            zoom: 2,

            //Add your Azure Maps primary subscription key to the map SDK.
            authOptions: {
                authType: 'subscriptionKey',
                subscriptionKey: '<Your Azure Maps Key>'
            }
        });

        //Create a pop-up window, but leave it closed so we can update it and display it later.
        popup = new atlas.Popup();

        //Use SubscriptionKeyCredential with a subscription key
        const subscriptionKeyCredential = new atlas.service.SubscriptionKeyCredential(atlas.getSubscriptionKey());

        //Use subscriptionKeyCredential to create a pipeline
        const pipeline = atlas.service.MapsURL.newPipeline(subscriptionKeyCredential, {
            retryOptions: { maxTries: 4 } // Retry options
        });

        //Create an instance of the SearchURL client.
        searchURL = new atlas.service.SearchURL(pipeline);

        //If the user selects the search button, geocode the value the user passed in.
        document.getElementById('searchBtn').onclick = performSearch;

        //If the user presses Enter in the search box, perform a search.
        document.getElementById('searchTbx').onkeyup = function(e) {
            if (e.keyCode === 13) {
                performSearch();
            }
        };

        //If the user selects the My Location button, use the Geolocation API to get the user's location. Center and zoom the map on that location.
        document.getElementById('myLocationBtn').onclick = setMapToUserLocation;

        //Wait until the map resources are ready.
        map.events.add('ready', function() {

            //Add your post-map load functionality.

        });
    }

    //Create an array of country ISO 2 values to limit searches to. 
    var countrySet = ['US', 'CA', 'GB', 'FR','DE','IT','ES','NL','DK'];

    function performSearch() {
        var query = document.getElementById('searchTbx').value;

        //Perform a fuzzy search on the users query.
        searchURL.searchFuzzy(atlas.service.Aborter.timeout(3000), query, {
            //Pass in the array of country ISO2 for which we want to limit the search to.
            countrySet: countrySet
        }).then(results => {
            //Parse the response into GeoJSON so that the map can understand.
            var data = results.geojson.getFeatures();

            if (data.features.length > 0) {
                //Set the camera to the bounds of the results.
                map.setCamera({
                    bounds: data.features[0].bbox,
                    padding: 40
                });
            } else {
                document.getElementById('listPanel').innerHTML = '<div class="statusMessage">Unable to find the location you searched for.</div>';
            }
        });
    }

    function setMapToUserLocation() {
        //Request the user's location.
        navigator.geolocation.getCurrentPosition(function(position) {
            //Convert the Geolocation API position to a longitude and latitude position value that the map can interpret and center the map over it.
            map.setCamera({
                center: [position.coords.longitude, position.coords.latitude],
                zoom: maxClusterZoomLevel + 1
            });
        }, function(error) {
            //If an error occurs when the API tries to access the user's position information, display an error message.
            switch (error.code) {
                case error.PERMISSION_DENIED:
                    alert('User denied the request for geolocation.');
                    break;
                case error.POSITION_UNAVAILABLE:
                    alert('Position information is unavailable.');
                    break;
                case error.TIMEOUT:
                    alert('The request to get user position timed out.');
                    break;
                case error.UNKNOWN_ERROR:
                    alert('An unknown error occurred.');
                    break;
            }
        });
    }

    //Initialize the application when the page is loaded.
    window.onload = initialize;
    ```

1. Haritanın `ready` olay dinleyicisinde, arama alanının merkezini görüntülemek için bir yakınlaştırma denetimi ve HTML işaretçisi ekleyin.

    ```JavaScript
    //Add a zoom control to the map.
    map.controls.add(new atlas.control.ZoomControl(), {
        position: 'top-right'
    });

    //Add an HTML marker to the map to indicate the center to use for searching.
    centerMarker = new atlas.HtmlMarker({
        htmlContent: '<div class="mapCenterIcon"></div>',
        position: map.getCamera().center
    });

    map.markers.add(centerMarker);
    ```

1. Haritanın `ready` olay dinleyicisinde bir veri kaynağı ekleyin. Ardından, veri kümesini yüklemek ve ayrıştmak için bir arama yapın. Veri kaynağında kümele'yi etkinleştirin. Bir kümede çakışan noktaları bir araya getirerek veri kaynağı grupları üzerinde kümeleme. Kümeler, kullanıcı yakınlaştıkça ayrı ayrı noktalara ayrılır. Bu davranış daha iyi bir kullanıcı deneyimi sağlar ve performansı artırır.

    ```JavaScript
    //Create a data source, add it to the map, and then enable clustering.
    datasource = new atlas.source.DataSource(null, {
        cluster: true,
        clusterMaxZoom: maxClusterZoomLevel - 1
    });

    map.sources.add(datasource);

    //Load all the store data now that the data source is defined.  
    loadStoreData();
    ```

1. Haritanın `ready` olay dinleyicisine veri kümesini yükledikten sonra, verileri işlemek için bir katman kümesi tanımlayın. Kümelenmiş veri noktalarını işlemek için kabarcık katmanı kullanılır. Bir sembol katmanı kabarcık katmanı üzerinde her kümede nokta sayısını işlemek için kullanılır. İkinci bir sembol katmanı, haritadaki tek tek konumlar için özel bir simge oluşturur.

   Kullanıcı `mouseover` `mouseout` haritadaki bir küme veya simgenin üzerinde gezindiğinde fare imlecini değiştirmek için kabarcık ve simge katmanlarına ekleme ve olaylar ekleyin. Küme `click` kabarcığı katmanına bir olay ekleyin. Bu `click` olay haritada iki düzey yakınlaştırır ve kullanıcı herhangi bir küme seçtiğinde haritayı bir küme üzerinde ortalar. Simge `click` katmanına bir olay ekleyin. Bu `click` olay, bir kullanıcı tek bir konum simgesi ni seçtiğinde bir kafenin ayrıntılarını gösteren bir açılır pencere görüntüler. Haritanın hareket etmesini izlemek için haritaya bir olay ekleyin. Bu olay çıktığında, liste panelindeki öğeleri güncelleştirin.  

    ```JavaScript
    //Create a bubble layer to render clustered data points.
    var clusterBubbleLayer = new atlas.layer.BubbleLayer(datasource, null, {
        radius: 12,
        color: '#007faa',
        strokeColor: 'white',
        strokeWidth: 2,
        filter: ['has', 'point_count'] //Only render data points that have a point_count property; clusters have this property.
    });

    //Create a symbol layer to render the count of locations in a cluster.
    var clusterLabelLayer = new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            image: 'none' //Hide the icon image.
        },

        textOptions: {
            textField: ['get', 'point_count_abbreviated'],
            size: 12,
            font: ['StandardFont-Bold'],
            offset: [0, 0.4],
            color: 'white'
        }
    });

    map.layers.add([clusterBubbleLayer, clusterLabelLayer]);

    //Load a custom image icon into the map resources.
    map.imageSprite.add('myCustomIcon', iconImageUrl).then(function() {

    //Create a layer to render a coffee cup symbol above each bubble for an individual location.
    iconLayer = new atlas.layer.SymbolLayer(datasource, null, {
        iconOptions: {
            //Pass in the ID of the custom icon that was loaded into the map resources.
            image: 'myCustomIcon',

            //Optionally, scale the size of the icon.
            font: ['SegoeUi-Bold'],

            //Anchor the center of the icon image to the coordinate.
            anchor: 'center',

            //Allow the icons to overlap.
            allowOverlap: true
        },

        filter: ['!', ['has', 'point_count']] //Filter out clustered points from this layer.
    });

    map.layers.add(iconLayer);

    //When the mouse is over the cluster and icon layers, change the cursor to a pointer.
    map.events.add('mouseover', [clusterBubbleLayer, iconLayer], function() {
        map.getCanvasContainer().style.cursor = 'pointer';
    });

    //When the mouse leaves the item on the cluster and icon layers, change the cursor back to the default (grab).
    map.events.add('mouseout', [clusterBubbleLayer, iconLayer], function() {
        map.getCanvasContainer().style.cursor = 'grab';
    });

    //Add a click event to the cluster layer. When the user selects a cluster, zoom into it by two levels.  
    map.events.add('click', clusterBubbleLayer, function(e) {
        map.setCamera({
            center: e.position,
            zoom: map.getCamera().zoom + 2
        });
    });

    //Add a click event to the icon layer and show the shape that was selected.
    map.events.add('click', iconLayer, function(e) {
        showPopup(e.shapes[0]);
    });

    //Add an event to monitor when the map is finished rendering the map after it has moved.
    map.events.add('render', function() {
        //Update the data in the list.
        updateListItems();
    });
    ```

1. Kahve dükkanı veri seti yüklendiğinde, önce indirilmelidir. Ardından, metin dosyası satırlara bölünmelidir. İlk satır üstbilgi bilgilerini içerir. Kodun takibini kolaylaştırmak için, üstbilgiyi bir nesneye ayrıştırır, bu nedenle her özelliğin hücre dizinini aramak için kullanabiliriz. İlk satırdan sonra, kalan satırları niçin döngüye sokup bir nokta özelliği oluşturun. Nokta özelliğini veri kaynağına ekleyin. Son olarak, liste panelini güncelleştirin.

    ```JavaScript
    function loadStoreData() {

    //Download the store location data.
    fetch(storeLocationDataUrl)
        .then(response => response.text())
        .then(function(text) {

            //Parse the tab-delimited file data into GeoJSON features.
            var features = [];

            //Split the lines of the file.
            var lines = text.split('\n');

            //Grab the header row.
            var row = lines[0].split('\t');

            //Parse the header row and index each column to make the code for parsing each row easier to follow.
            var header = {};
            var numColumns = row.length;
            for (var i = 0; i < row.length; i++) {
                header[row[i]] = i;
            }

            //Skip the header row and then parse each row into a GeoJSON feature.
            for (var i = 1; i < lines.length; i++) {
                row = lines[i].split('\t');

                //Ensure that the row has the correct number of columns.
                if (row.length >= numColumns) {

                    features.push(new atlas.data.Feature(new atlas.data.Point([parseFloat(row[header['Longitude']]), parseFloat(row[header['Latitude']])]), {
                        AddressLine: row[header['AddressLine']],
                        City: row[header['City']],
                        Municipality: row[header['Municipality']],
                        AdminDivision: row[header['AdminDivision']],
                        Country: row[header['Country']],
                        PostCode: row[header['PostCode']],
                        Phone: row[header['Phone']],
                        StoreType: row[header['StoreType']],
                        IsWiFiHotSpot: (row[header['IsWiFiHotSpot']].toLowerCase() === 'true') ? true : false,
                        IsWheelchairAccessible: (row[header['IsWheelchairAccessible']].toLowerCase() === 'true') ? true : false,
                        Opens: parseInt(row[header['Opens']]),
                        Closes: parseInt(row[header['Closes']])
                    }));
                }
            }

            //Add the features to the data source.
            datasource.add(new atlas.data.FeatureCollection(features));

            //Initially, update the list items.
            updateListItems();
        });
    }
    ```

1. Liste paneli güncelleştirildiğinde, mesafe hesaplanır. Bu mesafe, haritanın merkezinden geçerli harita görünümündeki tüm nokta özelliklerine kadardır. Özellikler daha sonra mesafeye göre sıralanır. HTML, liste panelindeki her konumu görüntülemek için oluşturulur.

    ```JavaScript
    var listItemTemplate = '<div class="listItem" onclick="itemSelected(\'{id}\')"><div class="listItem-title">{title}</div>{city}<br />Open until {closes}<br />{distance} miles away</div>';

    function updateListItems() {
        //Hide the center marker.
        centerMarker.setOptions({
            visible: false
        });

        //Get the current camera and view information for the map.
        var camera = map.getCamera();
        var listPanel = document.getElementById('listPanel');

        //Check to see whether the user is zoomed out a substantial distance. If they are, tell the user to zoom in and to perform a search or select the My Location button.
        if (camera.zoom < maxClusterZoomLevel) {
            //Close the pop-up window; clusters might be displayed on the map.  
            popup.close(); 
            listPanel.innerHTML = '<div class="statusMessage">Search for a location, zoom the map, or select the My Location button to see individual locations.</div>';
        } else {
            //Update the location of the centerMarker property.
            centerMarker.setOptions({
                position: camera.center,
                visible: true
            });

            //List the ten closest locations in the side panel.
            var html = [], properties;

            /*
            Generating HTML for each item that looks like this:
            <div class="listItem" onclick="itemSelected('id')">
                <div class="listItem-title">1 Microsoft Way</div>
                Redmond, WA 98052<br />
                Open until 9:00 PM<br />
                0.7 miles away
            </div>
            */

            //Get all the shapes that have been rendered in the bubble layer. 
            var data = map.layers.getRenderedShapes(map.getCamera().bounds, [iconLayer]);

            //Create an index of the distances of each shape.
            var distances = {};

            data.forEach(function (shape) {
                if (shape instanceof atlas.Shape) {

                    //Calculate the distance from the center of the map to each shape and store in the index. Round to 2 decimals.
                    distances[shape.getId()] = Math.round(atlas.math.getDistanceTo(camera.center, shape.getCoordinates(), 'miles') * 100) / 100;
                }
            });

            //Sort the data by distance.
            data.sort(function (x, y) {
                return distances[x.getId()] - distances[y.getId()];
            });

            data.forEach(function(shape) {
                properties = shape.getProperties();
                html.push('<div class="listItem" onclick="itemSelected(\'', shape.getId(), '\')"><div class="listItem-title">',
                properties['AddressLine'],
                '</div>',
                //Get a formatted addressLine2 value that consists of City, Municipality, AdminDivision, and PostCode.
                getAddressLine2(properties),
                '<br />',

                //Convert the closing time to a format that is easier to read.
                getOpenTillTime(properties),
                '<br />',

                //Get the distance of the shape.
                distances[shape.getId()],
                ' miles away</div>');
            });

            listPanel.innerHTML = html.join('');

            //Scroll to the top of the list panel in case the user has scrolled down.
            listPanel.scrollTop = 0;
        }
    }

    //This converts a time that's in a 24-hour format to an AM/PM time or noon/midnight string.
    function getOpenTillTime(properties) {
        var time = properties['Closes'];
        var t = time / 100;
        var sTime;

        if (time === 1200) {
            sTime = 'noon';
        } else if (time === 0 || time === 2400) {
            sTime = 'midnight';
        } else {
            sTime = Math.round(t) + ':';

            //Get the minutes.
            t = (t - Math.round(t)) * 100;

            if (t === 0) {
                sTime += '00';
            } else if (t < 10) {
                sTime += '0' + t;
            } else {
                sTime += Math.round(t);
            }

            if (time < 1200) {
                sTime += ' AM';
            } else {
                sTime += ' PM';
            }
        }

        return 'Open until ' + sTime;
    }

    //Create an addressLine2 string that contains City, Municipality, AdminDivision, and PostCode.
    function getAddressLine2(properties) {
        var html = [properties['City']];

        if (properties['Municipality']) {
            html.push(', ', properties['Municipality']);
        }

        if (properties['AdminDivision']) {
            html.push(', ', properties['AdminDivision']);
        }

        if (properties['PostCode']) {
            html.push(' ', properties['PostCode']);
        }

        return html.join('');
    }
    ```

1. Kullanıcı liste panelinde bir öğe seçtiğinde, öğenin ilişkili olduğu şekil veri kaynağından alınır. Formda depolanan özellik bilgilerini temel alan bir açılır pencere oluşturulur. Harita şeklin üzerinde ortalanır. Harita 700 pikselden daha genişse, açılır pencerenin görünür olması için harita görünümü ofset edilir.

    ```JavaScript
    //When a user selects a result in the side panel, look up the shape by its ID value and display the pop-up window.
    function itemSelected(id) {
        //Get the shape from the data source by using its ID.  
        var shape = datasource.getShapeById(id);
        showPopup(shape);

        //Center the map over the shape on the map.
        var center = shape.getCoordinates();
        var offset;

        //If the map is less than 700 pixels wide, then the layout is set for small screens.
        if (map.getCanvas().width < 700) {
            //When the map is small, offset the center of the map relative to the shape so that there is room for the popup to appear.
            offset = [0, -80];
        }

        map.setCamera({
            center: center,
            centerOffset: offset
        });
    }

    function showPopup(shape) {
        var properties = shape.getProperties();

        /* Generating HTML for the pop-up window that looks like this:

            <div class="storePopup">
                <div class="popupTitle">
                    3159 Tongass Avenue
                    <div class="popupSubTitle">Ketchikan, AK 99901</div>
                </div>
                <div class="popupContent">
                    Open until 22:00 PM<br/>
                    <img title="Phone Icon" src="images/PhoneIcon.png">
                    <a href="tel:1-800-XXX-XXXX">1-800-XXX-XXXX</a>
                    <br>Amenities:
                    <img title="Wi-Fi Hotspot" src="images/WiFiIcon.png">
                    <img title="Wheelchair Accessible" src="images/WheelChair-small.png">
                </div>
            </div>
        */

         //Calculate the distance from the center of the map to the shape in miles, round to 2 decimals.
        var distance = Math.round(atlas.math.getDistanceTo(map.getCamera().center, shape.getCoordinates(), 'miles') * 100)/100;

        var html = ['<div class="storePopup">'];
        html.push('<div class="popupTitle">',
            properties['AddressLine'],
            '<div class="popupSubTitle">',
            getAddressLine2(properties),
            '</div></div><div class="popupContent">',

            //Convert the closing time to a format that's easier to read.
            getOpenTillTime(properties),

            //Add the distance information.  
            '<br/>', distance,
            ' miles away',
            '<br /><img src="images/PhoneIcon.png" title="Phone Icon"/><a href="tel:',
            properties['Phone'],
            '">',  
            properties['Phone'],
            '</a>'
        );

        if (properties['IsWiFiHotSpot'] || properties['IsWheelchairAccessible']) {
            html.push('<br/>Amenities: ');

            if (properties['IsWiFiHotSpot']) {
                html.push('<img src="images/WiFiIcon.png" title="Wi-Fi Hotspot"/>');
            }

            if (properties['IsWheelchairAccessible']) {
                html.push('<img src="images/WheelChair-small.png" title="Wheelchair Accessible"/>');
            }
        }

        html.push('</div></div>');

        //Update the content and position of the pop-up window for the specified shape information.
        popup.setOptions({

            //Create a table from the properties in the feature.
            content:  html.join(''),
            position: shape.getCoordinates()
        });

        //Open the pop-up window.
        popup.open(map);
    }
    ```

Şimdi, tamamen işlevsel bir mağaza bulucunuz var. Bir web tarayıcısında, mağaza bulucu için *index.html* dosyasını açın. Kümeler haritada göründüğünde, arama kutusunu kullanarak, Konumum düğmesini seçerek, bir küme seçerek veya tek tek konumları görmek için haritayı yakınlaştırarak bir konum arayabilirsiniz.

Bir kullanıcı Konumum düğmesini ilk kez seçtiğinde, tarayıcı kullanıcının konumuna erişmek için izin isteyen bir güvenlik uyarısı görüntüler. Kullanıcı konumunu paylaşmayı kabul ederse, harita kullanıcının konumunu yakınlaştırır ve yakındaki kafeler gösterilir. 

<center>

![Tarayıcının kullanıcının konumuna erişim isteğinin ekran görüntüsü](./media/tutorial-create-store-locator/GeolocationApiWarning.png)</center>

Kahve dükkanı konumları olan bir alanda yeterince yakınlaştırdığınızda, kümeler ayrı konumlara ayrılır. Haritadaki simgelerden birini seçin veya açılır pencereyi görmek için yan panelde bir öğe seçin. Açılır pencere, seçili konumun bilgilerini gösterir.

<center>

![Bitmiş mağaza bulucu ekran görüntüsü](./media/tutorial-create-store-locator/FinishedSimpleStoreLocator.png)</center>

Tarayıcı penceresini 700 pikselden daha kısa bir genişlikte yeniden boyutlandırAcak veya uygulamayı mobil bir cihazda açarsanız, düzen daha küçük ekranlar için daha uygun olacaktır. 

<center>

![Mağaza bulucuküçük ekran lı sürümüekran görüntüsü](./media/tutorial-create-store-locator/FinishedSimpleStoreLocatorSmallScreen.png)</center>

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Haritalar'ı kullanarak temel bir mağaza bulucusu oluşturmayı öğrenirsiniz. Bu öğreticide oluşturduğunuz mağaza bulucu, ihtiyacınız olan tüm işlevselliğe sahip olabilir. Mağaza bulucunuza özellikler ekleyebilir veya daha özel bir kullanıcı deneyimi için daha gelişmiş özellikler kullanabilirsiniz: 

> [!div class="checklist"]
> * Arama kutusuna [yazarken önerileri](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20Autosuggest%20and%20JQuery%20UI) etkinleştirin.  
> * [Birden çok dil için destek](https://azuremapscodesamples.azurewebsites.net/?sample=Map%20Localization)ekleyin. 
> * Kullanıcının [rota boyunca konumları filtrelemesine](https://azuremapscodesamples.azurewebsites.net/?sample=Filter%20Data%20Along%20Route)izin verin. 
> * [Filtreleri ayarlamak](https://azuremapscodesamples.azurewebsites.net/?sample=Filter%20Symbols%20by%20Property)için yeteneği ekleyin. 
> * Sorgu dizesini kullanarak ilk arama değerini belirtmek için destek ekleyin. Bu seçeneği mağaza bulucunuza eklediğinizde, kullanıcılar aramaları yer imi yapabilir ve paylaşabilir. Ayrıca, aramaları başka bir sayfadan bu sayfaya geçirmeniz için kolay bir yöntem de sağlar.  
> * Mağaza bulucunuzu Azure [Uygulaması Hizmeti Web Uygulaması](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-html)olarak dağıtın. 
> * Verilerinizi bir veritabanında saklayın ve yakındaki konumları arayın. Daha fazla bilgi edinmek için, [sql server uzamsal veri türlerine genel bakış](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-types-overview?view=sql-server-2017) ve en yakın komşu için [uzamsal verileri](https://docs.microsoft.com/sql/relational-databases/spatial/query-spatial-data-for-nearest-neighbor?view=sql-server-2017)sorgula'ya bakın.

> [!div class="nextstepaction"]
> [Tam kaynak kodunu görüntüleme](https://github.com/Azure-Samples/AzureMapsCodeSamples/tree/master/AzureMapsCodeSamples/Tutorials/Simple%20Store%20Locator)

> [!div class="nextstepaction"]
> [Canlı örneği görüntüle](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Simple%20Store%20Locator)

Azure Haritalar'ın kapsamı ve özellikleri hakkında daha fazla bilgi edinmek için:

> [!div class="nextstepaction"]
> [Yakınlaştırma düzeyleri ve döşeme ızgarası](zoom-levels-and-tile-grid.md)

Daha fazla kod örneği ve etkileşimli bir kodlama deneyimi için:

> [!div class="nextstepaction"]
> [Harita denetimini kullanma](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Veri temelli stil ifadeleri kullanma](data-driven-style-expressions-web-sdk.md)
