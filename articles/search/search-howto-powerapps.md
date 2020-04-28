---
title: "Öğretici: Power Apps 'ten sorgu"
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama dizinine bağlanan, sorgular gönderen ve sonuçları işleyen bir Power App oluşturma hakkında adım adım yönergeler.
author: luiscabrer
manager: eladz
ms.author: luisca
ms.service: cognitive-search
ms.devlang: rest-api
ms.topic: tutorial
ms.date: 04/25/2020
ms.openlocfilehash: e4afa3c122fa6e21b29b6ad52a386096b20aa055
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82184506"
---
# <a name="tutorial-query-a-cognitive-search-index-from-power-apps"></a>Öğretici: Power Apps 'ten bir Bilişsel Arama dizini sorgulama

Azure Bilişsel Arama 'de aranabilir içeriğiniz için özel bir uygulama oluşturmak üzere Power Apps 'in hızlı uygulama geliştirme ortamından yararlanın.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Bilişsel Arama bağlanma
> * Sorgu isteği ayarlama
> * Bir tuval uygulamasında sonuçları görselleştirin

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) açın.

## <a name="prerequisites"></a>Ön koşullar

* [Power Apps hesabı](http://make.powerapps.com)

* [Oteller-örnek Dizin](search-get-started-portal.md)

* [API anahtarını sorgula](search-security-api-keys.md#find-existing-keys)

## <a name="1---create-a-custom-connector"></a>1-özel bağlayıcı oluşturma

Power Apps 'teki bir bağlayıcı, veri kaynağı bağlantısıdır. Bu adımda, buluttaki bir arama dizinine bağlanmak için özel bir bağlayıcı oluşturacaksınız.

1. Power Apps ['Te oturum açın](http://make.powerapps.com) .

1. Sol tarafta, **veri** > **özel bağlayıcıları**' nı genişletin.
 
    :::image type="content" source="./media/search-howto-powerapps/1-2-custom-connector.png" alt-text="Özel bağlayıcı menüsü" border="true":::

1. **+ Yeni özel bağlayıcı**' ı seçin ve **boş oluştur**' u seçin.

    :::image type="content" source="./media/search-howto-powerapps/1-3-create-blank.png" alt-text="Boş menüden oluştur" border="true":::

1. Özel bağlayıcınıza bir ad verin (örneğin, *AzureSearchQuery*) ve ardından **devam**' a tıklayın.

1. Genel sayfasına bilgi girin:

   * Simge arka plan rengi (örneğin, #007ee5)
   * Açıklama (örneğin, "Azure 'a bağlayıcı Bilişsel Arama")
   * Konakta, arama hizmeti URL 'nizi (gibi `<yourservicename>.search.windows.net`) girmeniz gerekir
   * Taban URL 'SI için, "/" girmeniz yeterlidir.

    :::image type="content" source="./media/search-howto-powerapps/1-5-general-info.png" alt-text="Genel bilgiler iletişim kutusu" border="true":::

1. Güvenlik sayfasında, *API anahtarını* **kimlik doğrulama türü**olarak ayarlayın, hem parametre etiketini hem de parametre adını *API anahtarı*olarak ayarlayın. **Parametre konumu**için aşağıda gösterildiği gibi *üst bilgi* ' yi seçin.

    :::image type="content" source="./media/search-howto-powerapps/1-6-authentication-type.png" alt-text="Kimlik doğrulama türü seçeneği" border="true":::

1. Tanımlar sayfasında, dizinde sorgulama yapılacak bir eylem oluşturmak için **+ yeni eylem** ' i seçin. Özet için "Query" değerini ve işlem KIMLIĞI adını girin. *"Arama dizinini sorgulama"* gibi bir açıklama girin.

    :::image type="content" source="./media/search-howto-powerapps/1-7-new-action.png" alt-text="Yeni eylem seçenekleri" border="true":::

1. Aşağı kaydırın. Istekler ' de, arama hizmetinize bir sorgu isteği yapılandırmak için **örnekten + Içeri aktar** ' ı seçin:

   * Fiili seçin`GET`

   * URL için, arama dizininiz için bir örnek sorgu girin (`search=*` tüm belgeler ' i döndürür `$select=` , alanlar seçmenizi sağlar). API sürümü gereklidir. Tam olarak belirtilen URL şöyle görünebilir:`https://mydemo.search.windows.net/indexes/hotels-sample-index/docs?search=*&$select=HotelName,Description,Address/City&api-version=2019-05-06`

   * Üst bilgiler için yazın `Content-Type`. 

     **Power Apps** , sorgudan parametreleri ayıklamak için söz dizimini kullanır. Arama alanını açık bir şekilde tanımladığımızda dikkat edin. 

       :::image type="content" source="./media/search-howto-powerapps/1-8-1-import-from-sample.png" alt-text="Örnekten içeri aktarma" border="true":::

1. Isteği otomatik olarak doldurmanız için **Içeri aktar** ' a tıklayın. **..** . Öğesine tıklayarak parametre meta verilerini ayarlamayı doldurun. Her parametrenin yanındaki simge. Her bir parametre güncelleştirmesinden sonra Istek sayfasına dönmek için **geri** ' ye tıklayın.

   :::image type="content" source="./media/search-howto-powerapps/1-8-2-import-from-sample.png" alt-text="Örnekten Al iletişim kutusu" border="true":::

1. *Arama*için `*` : **varsayılan değer**olarak ayarlayın, **gerekli** olarak ayarlayın ve **görünürlüğü** *hiçbiri* *olarak ayarlayın* . 

    :::image type="content" source="./media/search-howto-powerapps/1-10-1-parameter-metadata-search.png" alt-text="Arama parametresi meta verileri" border="true":::

1. *Seçim*için: `HotelName,Description,Address/City` **varsayılan değer**olarak ayarla, **gerekli** ayarını *false*olarak ayarlayın ve **görünürlüğü** *hiçbiri*olarak ayarlayın.  

    :::image type="content" source="./media/search-howto-powerapps/1-10-4-parameter-metadata-select.png" alt-text="Sürüm parametresi meta verileri" border="true":::

1. *Api sürümü*için `2019-05-06` : **varsayılan değer** **olarak ayarlayın,** *true*olarak ayarlayın ve **görünürlüğü** *iç*olarak ayarlayın.  

    :::image type="content" source="./media/search-howto-powerapps/1-10-2-parameter-metadata-version.png" alt-text="Sürüm parametresi meta verileri" border="true":::

1. *Içerik türü*için: olarak `application/json`ayarlayın.

1. Bu değişiklikleri yaptıktan sonra, **Swagger düzenleyici** görünümüne geçiş yapın. Parametreler bölümünde aşağıdaki yapılandırmayı görmeniz gerekir:

    ```JSON
    parameters:
      - {name: search, in: query, required: false, type: string, default: '*'}
      - {name: $select, in: query, required: false, type: string, default: 'HotelName,Description,Address/City'}
      - {name: api-version, in: query, required: true, type: string, default: '2019-05-06',
        x-ms-visibility: internal}
      - {name: Content-Type, in: header, required: false, type: string}
    ```

1. 3 ' e geri dönün **. Adım iste** ve yanıt bölümüne kaydır. **"Varsayılan yanıt Ekle"** ye tıklayın. Bu kritik öneme sahiptir çünkü Power Apps 'in yanıt şemasını anlamasına yardımcı olur. 

1. Örnek bir yanıt yapıştırın. Örnek bir yanıtı yakalamanın kolay bir yolu Azure portal arama Gezgini ' dir. Arama Gezgini ' nde, istek için aynı sorguyu girmeniz gerekir, ancak sonuçları yalnızca iki belgeyle sınırlamak için **$Top = 2** ' yi ekleyin:: `search=*&$select=HotelName,Description,Address/City&$top=2`. 

   Power Apps 'in yalnızca şemayı algılaması için bazı sonuçlara ihtiyacı vardır.

    ```JSON
    {
        "@odata.context": "https://mydemo.search.windows.net/indexes('hotels-sample-index')/$metadata#docs(*)",
        "value": [
            {
                "@search.score": 1,
                "HotelName": "Arcadia Resort & Restaurant",
                "Description": "The largest year-round resort in the area offering more of everything for your vacation – at the best value!  What can you enjoy while at the resort, aside from the mile-long sandy beaches of the lake? Check out our activities sure to excite both young and young-at-heart guests. We have it all, including being named “Property of the Year” and a “Top Ten Resort” by top publications.",
                "Address": {
                    "City": "Seattle"
                }
            },
            {
                "@search.score": 1,
                "HotelName": "Travel Resort",
                "Description": "The Best Gaming Resort in the area.  With elegant rooms & suites, pool, cabanas, spa, brewery & world-class gaming.  This is the best place to play, stay & dine.",
                "Address": {
                    "City": "Albuquerque"
                }
            }
        ]
    }
    ```

    > [!TIP] 
    > Girebileceğiniz JSON yanıtı için bir karakter sınırı vardır. bu nedenle, bunu yapıştırmadan önce JSON 'ı basitleştirmek isteyebilirsiniz. Yanıtın şeması ve biçimi değerlerden daha önemlidir. Örneğin, açıklama alanı yalnızca ilk cümleyi basitleşmelidir.

1. Sağ üst köşedeki **bağlayıcı oluştur** ' a tıklayın.

## <a name="2---test-the-connection"></a>2-bağlantıyı test etme

Bağlayıcı ilk oluşturulduğunda, test etmek için özel bağlayıcılar listesinden yeniden açmanız gerekir. Daha sonra, ek güncelleştirmeler yaparsanız, sihirbaz içinden test edebilirsiniz.

Bu görev için bir [sorgu API 'si anahtarına](search-security-api-keys.md#find-existing-keys) ihtiyacınız olacak. Bir bağlantı her oluşturulduğunda, bir uygulamaya bir test çalıştırması veya dahil edilmesi durumunda bağlayıcı, Azure Bilişsel Arama bağlanmak için kullanılan sorgu API anahtarına ihtiyaç duyuyor.

1. En solda **özel bağlayıcılar**' a tıklayın.

1. Bağlayıcıyı ada göre ara (Bu öğreticide, "AzureSearchQuery").

1. Bağlayıcıyı seçin, eylemler listesini genişletin ve **özellikleri görüntüle**' yi seçin.

    :::image type="content" source="./media/search-howto-powerapps/1-11-1-test-connector.png" alt-text="Özellikleri Görüntüle" border="true":::

1. Sağ üst köşedeki **Düzenle** ' yi seçin.

1. 4 seçeneğini belirleyin **. **Test sayfasını açmak için test edin.

1. Test Işleminde **+ Yeni bağlantı**' ya tıklayın.

1. Bir sorgu API anahtarı girin. Bu, bir dizine salt okuma erişimi için bir Azure Bilişsel Arama sorgusudur. Anahtarı Azure portal [bulabilirsiniz](search-security-api-keys.md#find-existing-keys) . 

1. Işlemler ' de **test işlemi** düğmesine tıklayın. Başarılı olursanız, 200 durumunda bir durum görmeniz ve yanıt gövdesinde arama sonuçlarını açıklayan JSON görmeniz gerekir.

    :::image type="content" source="./media/search-howto-powerapps/1-11-2-test-connector.png" alt-text="JSON yanıtı" border="true":::

## <a name="3---visualize-results"></a>3-sonuçları görselleştirin

Bu adımda, sonuçlar için bir arama kutusuyla, bir arama düğmesiyle ve bir görüntüleme alanına sahip bir Power App oluşturun. Power App, Azure Search verileri almak için son oluşturulan özel bağlayıcıya bağlanır.

1. Sol tarafta **uygulamalar** > **+ Yeni uygulama** > **tuvali**' ni genişletin.

    :::image type="content" source="./media/search-howto-powerapps/2-1-create-canvas.png" alt-text="Tuval uygulaması oluşturma" border="true":::

1. Uygulama türünü seçin. Bu öğreticide, **Telefon düzeninde** **boş bir uygulama** oluşturun. **Power Apps Studio** görüntülenir.

1. Studio 'da bir kez **veri kaynakları** sekmesini seçin ve yeni oluşturduğunuz bağlayıcıya tıklayın. Bu durumda, *AzureSearchQuery*olarak adlandırılır. **Bağlantı ekle**' ye tıklayın.

   Sorgu API anahtarını girin.

    :::image type="content" source="./media/search-howto-powerapps/2-3-connect-connector.png" alt-text="bağlayıcı bağlama" border="true":::

    Artık *AzureSearchQuery* , uygulamanızdan kullanılabilecek bir veri kaynağıdır.

1. **Ekle sekmesinde**tuvale birkaç denetim ekleyin.

    :::image type="content" source="./media/search-howto-powerapps/2-4-add-controls.png" alt-text="Denetimleri Ekle" border="true":::

1. Aşağıdaki öğeleri ekleyin:

   * "Query:" değerine sahip bir metin etiketi
   * Bir metin girişi öğesi ( *Txtquery*çağırın, varsayılan değer: "*")
   * "Ara" metnini içeren bir düğme 
   * Dikey bir galeri çağrıldı (Bu, bir *Gallerresults*çağrısı yapın)

    Tuval şuna benzer görünmelidir:

    :::image type="content" source="./media/search-howto-powerapps/2-5-controls-layout.png" alt-text="Denetim düzeni" border="true":::

1. **Arama düğmesine** bir sorgu vermesini sağlamak için, aşağıdaki eylemi **OnSelect**içine yapıştırın:

    ```
    If(!IsBlank(txtQuery.Text),
        ClearCollect(azResult, AzureSearchQuery.Query({search: txtQuery.Text}).value))
    ```

   Aşağıdaki ekran görüntüsünde, **OnSelect** eyleminin formül çubuğu gösterilmektedir.

    :::image type="content" source="./media/search-howto-powerapps/2-6-search-button-event.png" alt-text="Düğme OnSelect" border="true":::

   Bu eylem, düğmenin Sorgu terimi olarak *txtquery* metin kutusundaki metni kullanarak, *azresult* adlı yeni bir koleksiyonu, arama sorgusunun sonucuyla güncelleştirmesine neden olur.

   > [!NOTE]
   > Formül sözdizimi hatası alırsanız bunu deneyin "' ClearCollect ' işlevinin bazı geçersiz işlevleri vardır":
   > 
   > * İlk olarak bağlayıcı başvurusunun doğru olduğundan emin olun. Bağlayıcı adını temizleyin ve bağlayıcının adını yazmaya başlayın. IntelliSense, doğru bağlayıcıyı ve fiili 'i önermelidir.
   > 
   > * Hata devam ederse, bağlayıcıyı silin ve yeniden oluşturun. Bağlayıcının birden çok örneği varsa, uygulama yanlış bir tane kullanıyor olabilir.
   > 

1. Dikey Galeri denetimini, önceki adımı tamamladığınızda oluşturulan *Azresult* koleksiyonuna bağlayın. 

   Galeri denetimini seçin ve Özellikler bölmesinde aşağıdaki eylemleri gerçekleştirin.

   * **Veri kaynağını** *azresult*olarak ayarlayın.
   * Dizininizdeki verilerin türüne göre sizin için uygun bir **Düzen** seçin. Bu durumda, *Başlık, alt başlık ve gövde* düzeni kullandık.
   * **Alanları düzenleyin**ve görselleştirmek istediğiniz alanları seçin.

    Bağlayıcıyı tanımladığımızda bir örnek sonuç sağladığımızdan, uygulama dizininizdeki kullanılabilir alanları algılar.
    
    :::image type="content" source="./media/search-howto-powerapps/2-7-gallery-select-fields.png" alt-text="Galeri alanları" border="true":::   
 
1. Uygulamayı önizlemek için **F5** tuşuna basın.  

    :::image type="content" source="./media/search-howto-powerapps/2-8-3-final.png" alt-text="Uygulamanın son hali" border="true":::    

<!--     Remember that the fields can be set to calculated values.

    For the example, setting using the *"Image, Title and Subtitle"* layout and specifying the *Image* function as the concatenation of the root path for the data and the file name (for instance, `"https://mystore.blob.core.windows.net/multilang/" & ThisItem.metadata_storage_name`) will produce the result below.

    :::image type="content" source="./media/search-howto-powerapps/2-8-2-final.png" alt-text="Final app" border="true":::         -->

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Power Apps, özel uygulamalar için hızlı uygulama geliştirmeyi mümkün bir şekilde sunar. Artık bir arama dizinine nasıl bağlanacağınızı öğrenmiş olduğunuza göre, özel bir Power App 'te zengin bir görselleştirme deneyimi oluşturma hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Power Apps öğrenme kataloğu](https://docs.microsoft.com/powerapps/learning-catalog/get-started)

