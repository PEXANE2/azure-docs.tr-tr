---
title: Power Apps'tan Azure Bilişsel Arama nasıl sorgulanır?
titleSuffix: Azure Cognitive Search
description: Bilişsel Arama'ya özel bağlayıcı nın nasıl oluşturulması ve Bir Güç Uygulaması'ndan nasıl görselleştirilen hakkında adım adım kılavuz
author: luiscabrer
manager: eladz
ms.author: luisca
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: c246f8652227a5ad2c0798880e530d6039cdeea8
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385121"
---
# <a name="how-to-query-a-cognitive-search-index-from-power-apps"></a>Power Apps'tan Bilişsel Arama dizini nasıl sorgulanır?

Bu belge, arama sonuçlarınızı bir arama dizininden alabilmeniz için Power Apps özel bağlayıcısı nasıl oluşturulabileceğinizi gösterir. Ayrıca, bir arama sorgusunun nasıl yayınlanır ve bir Power App'in sonuçlarını nasıl görselleştirin. 

## <a name="prerequisites"></a>Ön koşullar
*    Özel bağlayıcılar oluşturma özelliğiyle Power Apps hesabına erişim.
*    Azure Arama Dizini oluşturduğunuzu varsayıyoruz.

## <a name="create-a-custom-connector-to-query-azure-search"></a>Azure Arama sorgusu için özel bir bağlayıcı oluşturma

Azure Bilişsel Arama sonuçlarını gösteren bir PowerApp'a sahip olmanın iki ana adımı vardır. İlk olarak, arama dizini sorgulayabilir bir bağlayıcı oluşturalım. Bir [sonraki bölümde,](#visualize-results-from-the-custom-connector) konektör tarafından döndürülen sonuçları görselleştirmek için Power Apps uygulamanızı güncelleştireceğiz.

1. [make.powerapps.com](http://make.powerapps.com) gidin ve **Oturum Açın.**

1. **Veri** > **Özel Bağlayıcıları** Ara
 
    :::image type="content" source="./media/search-howto-powerapps/1-2-custom-connector.png" alt-text="Özel konektör menüsü" border="true":::

1. **+ Yeni özel bağlayıcıyı** tıklatın ve sonra **boştan Oluştur'u**seçin.

    :::image type="content" source="./media/search-howto-powerapps/1-3-create-blank.png" alt-text="Boş menüden oluşturma" border="true":::

1. Özel konektörünüze bir ad verin. (diğer bir şey, *AzureSearchQuery*), ve sonra **Devam et'i**tıklatın. Bu, yeni bağlayıcınızı oluşturmak için bir sihirbaz getirir.

1. Bilgileri Genel Sayfaya girin.

    - Simge arka plan rengi (örneğin, #007ee5)
    - Açıklama (örneğin, "Azure Bilişsel Arama'nın bağlayıcısı")
    - Ana Bilgisayar'da arama hizmeti URL'nizi girmeniz `<yourservicename>.search.windows.net`gerekir (örneğin, )
    - Temel URL için "/"
    
    :::image type="content" source="./media/search-howto-powerapps/1-5-general-info.png" alt-text="Genel bilgi diyaloğu" border="true":::

1. Güvenlik Sayfasında, *API Anahtarını* **Kimlik Doğrulama Türü**olarak ayarlayın, parametre etiketini ve parametre ad alanlarını *api anahtarı*olarak ayarlayın. **Parametre konumu**için, aşağıda gösterildiği gibi *Üstbilgi'yi* seçin.
 
    :::image type="content" source="./media/search-howto-powerapps/1-6-authentication-type.png" alt-text="Kimlik doğrulama türü seçeneği" border="true":::

1. Tanımlar Sayfasında, dizini sorgulayacak bir eylem oluşturmak için **+ Yeni Eylem'i** seçin. Özet ve işlem kimliği adı için "Sorgu" değerini girin. *"Arama dizinini sorgular"* gibi bir açıklama girin.
 
    :::image type="content" source="./media/search-howto-powerapps/1-7-new-action.png" alt-text="Yeni eylem seçenekleri" border="true":::


1. Parametreleri ve üstbilgileri tanımlamak için **örnekten + İçe Aktar** düğmesine basın. Ardından, sorgu isteğini tanımlarsınız.  

    * Fiili seçin`GET`
    * URL için arama dizininiz için örnek bir sorgu girin, örneğin:
       
    >https://yoursearchservicename.search.windows.net/indexes/yourindexname/docs?search=*&api-version=2019-05-06-Önizleme
    

    **Power Apps** sorguparametrelerini ayıklamak için sözdizimini kullanır. Arama alanını açıkça tanımladığımıza dikkat edin. 

    :::image type="content" source="./media/search-howto-powerapps/1-8-1-import-from-sample.png" alt-text="Örnekten içeri aktarma" border="false":::

1.  İstek iletişim kutusunu otomatik olarak doldurmak için **İçe Aktar'ı** tıklatın.

    :::image type="content" source="./media/search-howto-powerapps/1-8-2-import-from-sample.png" alt-text="Örnek diyalogdan alma" border="false":::


1. Parametre meta verilerini tıklatarak tam ayarı **...** parametrelerin her birinin yanındaki sembolü.

    - Arama *search*için `*` : **Varsayılan değer**olarak ayarlayın, *yanlış* **olarak** ayarlayın ve **görünürlüğü** *hiçbirine*ayarlayın. 

    :::image type="content" source="./media/search-howto-powerapps/1-10-1-parameter-metadata-search.png" alt-text="Parametre meta verilerini arama" border="true":::

    - *api-version*için `2019-05-06-Preview` : **Varsayılan değer**olarak ayarlayın, **görünürlüğü** dahili olarak ayarlayın ve *True*için **gerekli** ayarlayın.  

    :::image type="content" source="./media/search-howto-powerapps/1-10-2-parameter-metadata-version.png" alt-text="Sürüm parametresi meta verileri" border="true":::

    - Benzer şekilde, *api-key*için , **gerektiği**gibi ayarlayın , *dahili* **görünürlük**ile . Varsayılan **değer**olarak arama hizmeti API anahtarınızı girin.
    
    Bu değişiklikleri yaptıktan **sonra, Swagger Düzenleyici** görünümüne geçiş yapın. Parametreler bölümünde aşağıdaki yapılandırmayı görmeniz gerekir:    

    ```
          parameters:
          - {name: search, in: query, required: false, type: string, default: '*'}
          - {name: api-version, in: query, required: true, type: string, default: 2019-05-06-Preview,
            x-ms-visibility: internal}
          - {name: api-key, in: header, required: true, type: string, default: YOURKEYGOESHERE,
            x-ms-visibility: internal}
    ```

1. Yanıt bölümünde **"Varsayılan yanıt ekle"** seçeneğini tıklayın. Bu çok önemlidir, çünkü **Power Apps'ın** yanıtın şemasını anlamalarına yardımcı olur. Örnek bir yanıt yapıştırın.

    > [!TIP] 
    > Girebileceğiniz JSON yanıtı için bir karakter sınırı vardır, bu nedenle JSON'u yapıştırmadan önce basitleştirmek isteyebilirsiniz. Yanıtın önemli yönü şeması/biçimi. Örnek yanıttaki gerçek değerler daha az önemlidir ve karakter sayısını azaltmak için basitleştirilmiş olabilir.
    

1.    Test etmeden önce ekranın sağ üst kısmındaki **Oluştur bağlayıcısı** düğmesini tıklatın.

1.  Test Sayfasında **+ Yeni Bağlantı'yı**tıklatın ve *api tuşu*için değer olarak arama hizmeti sorgu anahtarınızı girin.

    Bu adım sizi sihirbazın dışına ve Bağlantılar sayfasına götürebilir. Bağlantıyı gerçekten sınamak için Özel Bağlantılar düzenleyicisine geri dönmek isteyebilirsiniz. Özel **Bağlayıcı'ya** gidin > Yeni oluşturulan Bağlayıcıyı seçin > *...* > **Özellikleri Görüntüle** > **4.** > ** **Test sayfasına geri dönmek için test edin.

1.    Şimdi dizininizden sonuç aldığınızdan emin olmak için **Test işlemini** tıklatın. Başarılı olduysanız 200 durumu görmeniz gerekir ve yanıtın gövdesinde arama sonuçlarınızı açıklayan JSON'u görmeniz gerekir.




## <a name="visualize-results-from-the-custom-connector"></a>Özel konektörden sonuçları görselleştirin
Bu öğreticinin amacı, güç uygulamaları yla nasıl süslü kullanıcı deneyimleri oluşturacağınızı size göstermemektir, bu nedenle Kullanıcı Bira düzeni minimalist olacaktır. Arama kutusu, arama düğmesi içeren bir PowerApp oluşturalım ve sonuçları galeri kontrolünde görüntüleyelim.  PowerApp, Azure Arama'dan veri almak için yakın zamanda oluşturulan özel bağlayıcımıza bağlanır.

1. Yeni Power App oluşturun. **Uygulamalar** bölümüne gidin, **+ Yeni uygulamaya**tıklayın ve **Tuval'i**seçin.

    :::image type="content" source="./media/search-howto-powerapps/2-1-create-canvas.png" alt-text="Tuval uygulaması oluşturma" border="true":::

1. İstediğiniz uygulama türünü seçin. Bu öğretici için **Telefon Düzeni**ile boş bir **uygulama** oluşturun. **Power Apps Studio** görünecektir.

1. Stüdyoya girdiğimizde **Veri Kaynakları** sekmesini seçin ve yeni oluşturduğunuz yeni Bağlayıcı'yı tıklatın. Bizim durumumuzda, bu *AzureSearchQuery*denir. **Bağlantı ekle'yi**tıklatın.

    :::image type="content" source="./media/search-howto-powerapps/2-3-connect-connector.png" alt-text="connector'u bağlayın" border="true":::

    *AzureSearchQuery* artık uygulamanızdan kullanılabilecek bir veri kaynağıdır.
    
1. Formumuza birkaç denetim ekleyebilmemiz için **Ekle sekmesine**gidin.

    :::image type="content" source="./media/search-howto-powerapps/2-4-add-controls.png" alt-text="Denetim ekleme" border="true":::

1.  Aşağıdaki öğeleri ekleyin:
    -   "Sorgu:" değerine sahip bir Metin Etiketi
    -   Metin Giriş öğesi *(txtQuery*deyin, varsayılan değer: "*")
    -   "Arama" metninin yer alan bir düğmesi 
    -   Dikey Galeri adlı *(galeriSonuçları*deyin)
    
    Formunuz şuna benzer:

    :::image type="content" source="./media/search-howto-powerapps/2-5-controls-layout.png" alt-text="Düzeni denetler" border="true":::

1. **Arama düğmesini** sorgulamak için düğmeyi seçin ve **OnSelect'te**gerçekleştirmek için aşağıdaki eylemi yapıştırın:

    ```
    If(!IsBlank(txtQuery.Text),
        ClearCollect(azResult, AzureSearchQuery.Get({search: txtQuery.Text}).value))
    ```

    :::image type="content" source="./media/search-howto-powerapps/2-6-search-button-event.png" alt-text="Düğme OnSelect" border="true":::
 
    Bu eylem, *txtQuery* metin kutusundaki metni sorgu terimi olarak kullanarak, arama sorgusunun sonucuyla birlikte *azResult* adlı yeni bir koleksiyonu güncelleştirmek için düğmeye neden olur.
    
1.  Bir sonraki adım olarak, oluşturduğumuz dikey galeriyi *azResult* koleksiyonuna bağlayacaktır. Galeri denetimini seçin ve özellikler bölmesinde aşağıdaki eylemleri gerçekleştirin.

    -  **DataSource'u** *azResult*olarak ayarlayın.
    
    -  Dizininizdeki veri türüne bağlı olarak sizin için çalışan bir **Düzen** seçin. Bu durumda, *Başlık, altyazı ve vücut* düzeni kullanılır.
    
    -  Alanları ve görselleştirmek istediğiniz alanları **seçin.**

    Bağlayıcıyı tanımladığımızda örnek bir sonuç sağladığımız için, uygulama dizininizde bulunan alanların farkındadır.
    
    :::image type="content" source="./media/search-howto-powerapps/2-7-gallery-select-fields.png" alt-text="Galeri alanları" border="true":::   
 
1.  Uygulamayı önizlemek için **F5** tuşuna basın.  

    Alanların hesaplanan değerlere ayarlanabileceğini unutmayın.      
    Örneğin, *"Resim, Başlık ve Altyazı"* düzenini kullanarak ayar yapmak ve *Görüntü* işlevini veri ve dosya adı için kök yolunun (örneğin,) `"https://mystore.blob.core.windows.net/multilang/" & ThisItem.metadata_storage_name`biraraya getirmesi olarak belirtmek aşağıdaki sonucu oluşturur.

    :::image type="content" source="./media/search-howto-powerapps/2-8-2-final.png" alt-text="Uygulamanın son hali" border="true":::        

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi ve çevrimiçi eğitim için [Power Apps Öğrenme Kataloğu'na](https://docs.microsoft.com/powerapps/learning-catalog/get-started)bakın.

