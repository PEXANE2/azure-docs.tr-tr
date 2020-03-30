---
title: Özel verileri raster haritasında oluşturma | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar statik görüntü Hizmetini kullanarak özel verileri bir raster haritasında nasıl işleştireceğinizi öğreneceksiniz.
author: philmea
ms.author: philmea
ms.date: 01/23/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b8d47b69b4aba14c86fb09176b662aee7d5482d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335509"
---
# <a name="render-custom-data-on-a-raster-map"></a>Raster haritasında özel verileri oluşturma

Bu makalede, bir raster haritasının üstüne bindirmeler izin vermek için, görüntü kompozisyonu işlevselliği ile [statik görüntü hizmeti](https://docs.microsoft.com/rest/api/maps/render/getmapimage)nasıl kullanılacağı açıklanmaktadır. Görüntü kompozisyonu, özel raptler, etiketler ve geometri kaplamaları gibi ek verilerle bir raster döşemesini geri alma özelliğini içerir.

Özel pırtörtüleri, etiketleri ve geometri kaplamalarını işlemek için Postacı uygulamasını kullanabilirsiniz. Bindirmeleri depolamak ve işlemek için Azure Haritalar [Veri Hizmeti API'lerini](https://docs.microsoft.com/rest/api/maps/data) kullanabilirsiniz.

> [!Tip]
> Azure Haritalar Web SDK'sını bir web sayfasında basit bir harita yı göstermek için kullanmak statik görüntü hizmetini kullanmaktan çok daha uygun maliyetlidir. Web SDK harita kutucukları kullanır ve kullanıcı haritayı kaydırmadıkça ve yakınlaştırmadıkça, genellikle harita yükü başına bir işlemin yalnızca bir kısmını oluştururlar. Azure Haritalar web SDK'nın kaydırma ve yakınlaştırmayı devre dışı bırakma seçenekleri olduğunu unutmayın. Ayrıca, Azure Haritalar web SDK statik bir harita web hizmetidaha veri görselleştirme seçenekleri daha zengin bir dizi sağlar.  

## <a name="prerequisites"></a>Ön koşullar

### <a name="create-an-azure-maps-account"></a>Azure Haritalar hesabı oluşturma

Bu makaledeki yordamları tamamlamak için öncelikle bir Azure Haritalar hesabı oluşturmanız ve haritalar hesabı anahtarınızı almanız gerekir. Azure Haritalar hesabı aboneliği oluşturmak için [hesap oluştur'daki](quick-demo-map-app.md#create-an-account-with-azure-maps) yönergeleri izleyin ve hesabınızın birincil anahtarını almak için [birincil anahtarı al](quick-demo-map-app.md#get-the-primary-key-for-your-account) adımlarını izleyin. Azure Haritalar'da kimlik doğrulama hakkında daha fazla bilgi için Azure [Haritalar'da kimlik doğrulamayı yönet'e](./how-to-manage-authentication.md)bakın.


## <a name="render-pushpins-with-labels-and-a-custom-image"></a>Etiketlerle ve özel bir görüntüyle rendeleri işleme

> [!Note]
> Bu bölümdeki yordam, s0 veya S1 fiyatlandırmasında bir Azure Haritalar hesabı gerektirir.

Azure Haritalar hesabı S0 katmanı, parametrenin yalnızca tek bir örneğini `pins` destekler. URL isteğinde belirtilen en fazla beş retamağı özel bir resimle oluşturmanıza olanak tanır.

Etiketlerve özel bir görüntüyle rendeleri işlemek için şu adımları tamamlayın:

1. İstekleri depolamak için bir koleksiyon oluşturun. Postacı uygulamasında **Yeni'yi**seçin. Yeni **Oluştur** penceresinde **Koleksiyon'u**seçin. Koleksiyonu adlandırın ve **Oluştur** düğmesini seçin. 

2. İstek oluşturmak için Yeniden **Yeni'yi** seçin. Yeni **Oluştur** penceresinde **İstek'i**seçin. Pırtbazın **isteği adını** girin. İsteği kaydetmek için konum olarak önceki adımda oluşturduğunuz koleksiyonu seçin. Ardından **Kaydet**’i seçin.
    
    ![Postacı'da istek oluşturma](./media/how-to-render-custom-data/postman-new.png)

3. Oluşturucu sekmesinde HTTP'yi AL yöntemini seçin ve GET isteği oluşturmak için aşağıdaki URL'yi girin.

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.98,%2040.77&pins=custom%7Cla15+50%7Cls12%7Clc003b61%7C%7C%27CentralPark%27-73.9657974+40.781971%7C%7Chttps%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2FAzureMapsCodeSamples%2Fmaster%2FAzureMapsCodeSamples%2FCommon%2Fimages%2Ficons%2Fylw-pushpin.png
    ```
    İşte ortaya çıkan görüntü:

    ![Etiketli özel bir şınav](./media/how-to-render-custom-data/render-pins.png)


## <a name="get-data-from-azure-maps-data-storage"></a>Azure Haritalar veri depolamasından veri alma

> [!Note]
> Bu bölümdeki yordam, fiyatlandırma katmanı S1'de bir Azure Haritalar hesabı gerektirir.

Ayrıca [Veri Yükleme API'sini](https://docs.microsoft.com/rest/api/maps/data/uploadpreview)kullanarak yol ve pin konum bilgilerini de elde edebilirsiniz. Yolu yüklemek ve verileri sabitlemek için aşağıdaki adımları izleyin.

1. Postacı uygulamasında, önceki bölümde oluşturduğunuz koleksiyonda yeni bir sekme açın. Oluşturucu sekmesinde POST HTTP yöntemini seçin ve POST isteğinde bulunmak için aşağıdaki URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/mapData/upload?subscription-key={subscription-key}&api-version=1.0&dataFormat=geojson
    ```

2. **Params** sekmesinde, POST istek URL'si için kullanılan aşağıdaki anahtar/değer çiftlerini girin. Değeri `subscription-key` Azure Haritalar abonelik anahtarınızla değiştirin.
    
    ![Postacı'da anahtar/değer paramları](./media/how-to-render-custom-data/postman-key-vals.png)

3. **Gövde** sekmesinde, ham giriş biçimini seçin ve açılır listeden giriş biçimi olarak JSON'u seçin. Bu JSON'ı yüklenecek veri olarak sağlayın:
    
    ```JSON
    {
      "type": "FeatureCollection",
      "features": [
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "Polygon",
            "coordinates": [
              [
                [
                  -73.98235,
                  40.76799
                ],
                [
                  -73.95785,
                  40.80044
                ],
                [
                  -73.94928,
                  40.7968
                ],
                [
                  -73.97317,
                  40.76437
                ],
                [
                  -73.98235,
                  40.76799
                ]
              ]
            ]
          }
        },
        {
          "type": "Feature",
          "properties": {},
          "geometry": {
            "type": "LineString",
            "coordinates": [
              [
                -73.97624731063843,
                40.76560773817073
              ],
              [
                -73.97914409637451,
                40.766826609362575
              ],
              [
                -73.98513078689575,
                40.7585866048861
              ]
            ]
          }
        }
      ]
    }
    ```

4. Yanıt üstbilgisini **gönder'i** seçin ve gözden geçirin. Başarılı bir istek üzerine, Konum üstbilgisi yükleme isteğinin geçerli durumunu denetlemek için URI durumunu içerir. Uri durumu aşağıdaki biçimde olacaktır.  

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0
   ```

5. Durum URI'nizi kopyalayın ve Azure Haritalar hesap abonelik anahtarınızın değeriyle abonelik anahtarı parametresini ona ekleyin. Verileri yüklemek için kullandığınız hesap abonelik anahtarını kullanın. Durum URI biçimi aşağıdaki gibi görünmelidir:

   ```HTTP
   https://atlas.microsoft.com/mapData/{uploadStatusId}/status?api-version=1.0&subscription-key={Subscription-key}
   ```

6. UdId'i almak için Postacı uygulamasında yeni bir sekme açın. Oluşturucu sekmesinde HTTP'yi AL yöntemini seçin. URI durumunda get isteğinde bulunun. Veri yüklemeniz başarılı olduysa, yanıt gövdesinde bir udId alırsınız. UdId'i kopyala.

   ```JSON
   {
      "udid" : "{udId}"
   }
   ```

7. Haritadaki `udId` özellikleri işlemek için Veri Yükleme API'sinden alınan değeri kullanın. Bunu yapmak için, önceki bölümde oluşturduğunuz koleksiyonda yeni bir sekme açın. Oluşturucu sekmesinde HTTP'yi AL yöntemini seçin, {abonelik tuşu} ve {udId} değerlerinizle değiştirin ve GET isteğinde bulunmak için bu URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?subscription-key={subscription-key}&api-version=1.0&layer=basic&style=main&zoom=12&center=-73.96682739257812%2C40.78119135317995&pins=default|la-35+50|ls12|lc003C62|co9B2F15||'Times Square'-73.98516297340393 40.758781646381024|'Central Park'-73.96682739257812 40.78119135317995&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.30||udid-{udId}
    ```

    İşte yanıt görüntüsü:

    ![Azure Haritalar veri depolamasından veri alma](./media/how-to-render-custom-data/uploaded-path.png)

## <a name="render-a-polygon-with-color-and-opacity"></a>Çokgeni renk ve opaklıkla oluşturma

> [!Note]
> Bu bölümdeki yordam, fiyatlandırma katmanı S1'de bir Azure Haritalar hesabı gerektirir.


[Yol parametresi](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters)ile stil değiştiriciler kullanarak çokgen görünümünü değiştirebilirsiniz.

1. Postacı uygulamasında, daha önce oluşturduğunuz koleksiyonda yeni bir sekme açın. Oluşturucu sekmesinde HTTP'yi AL yöntemini seçin ve renk ve opaklık içeren bir çokgen işlemek için GET isteğini yapılandırmak için aşağıdaki URL'yi girin:
    
    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&sku=S1&zoom=14&height=500&Width=500&center=-74.040701, 40.698666&path=lc0000FF|fc0000FF|lw3|la0.80|fa0.50||-74.03995513916016 40.70090237454063|-74.04082417488098 40.70028420372218|-74.04113531112671 40.70049568385827|-74.04298067092896 40.69899904076542|-74.04271245002747 40.69879568992435|-74.04367804527283 40.6980961582905|-74.04364585876465 40.698055487620714|-74.04368877410889 40.698022951066996|-74.04168248176573 40.696444909137|-74.03901100158691 40.69837271818651|-74.03824925422668 40.69837271818651|-74.03809905052185 40.69903971085914|-74.03771281242369 40.699340668780984|-74.03940796852112 40.70058515602143|-74.03948307037354 40.70052821920425|-74.03995513916016 40.70090237454063
    &subscription-key={subscription-key}
    ```

    İşte yanıt görüntüsü:

    ![Opak bir çokgen oluşturma](./media/how-to-render-custom-data/opaque-polygon.png)


## <a name="render-a-circle-and-pushpins-with-custom-labels"></a>Özel etiketlerle daire ve rendeoluşturma oluşturma

> [!Note]
> Bu bölümdeki yordam, fiyatlandırma katmanı S1'de bir Azure Haritalar hesabı gerektirir.


Stil değiştiriciler ekleyerek pimlerin görünümünü değiştirebilirsiniz. Örneğin, puştürleri ve etiketlerini daha büyük `sc` veya daha küçük yapmak için "ölçek stili" değiştiricisini kullanın. Bu değiştirici sıfırdan büyük bir değer alır. 1 değeri standart ölçektir. 1'den büyük değerler pimleri büyütür ve 1'den küçük değerler onları küçültür. Stil değiştiriciler hakkında daha fazla bilgi için [statik görüntü hizmeti yolu parametrelerine](https://docs.microsoft.com/rest/api/maps/render/getmapimage#uri-parameters)bakın.


Bir daire ve özel etiketlerle rendeler işlemek için aşağıdaki adımları izleyin:

1. Postacı uygulamasında, daha önce oluşturduğunuz koleksiyonda yeni bir sekme açın. Oluşturucu sekmesinde HTTP'yi AL yöntemini seçin ve GET isteğinde bulunmak için bu URL'yi girin:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co002D62||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

    İşte yanıt görüntüsü:

    ![Özel pırtarak bir daire oluşturma](./media/how-to-render-custom-data/circle-custom-pins.png)

2. Son adımdaki tokmakların rengini değiştirmek için "co" stili değiştirin. Bak, `pins=default|la15+50|al0.66|lc003C62|co002D62|`geçerli renk CSS #002D62 olarak belirtilir. #41d42a olarak değiştirmek istediğinizi varsayalım. "Co" belirticisonra yeni renk değeri yazın, `pins=default|la15+50|al0.66|lc003C62|co41D42A|`aşağıdaki gibi: . Yeni bir GET isteği nde bulunun:

    ```HTTP
    https://atlas.microsoft.com/map/static/png?api-version=1.0&style=main&layer=basic&zoom=14&height=700&Width=700&center=-122.13230609893799,47.64599069048016&path=lcFF0000|lw2|la0.60|ra1000||-122.13230609893799 47.64599069048016&pins=default|la15+50|al0.66|lc003C62|co41D42A||'Microsoft Corporate Headquarters'-122.14131832122801  47.64690503939462|'Microsoft Visitor Center'-122.136828 47.642224|'Microsoft Conference Center'-122.12552547454833 47.642940335653996|'Microsoft The Commons'-122.13687658309935  47.64452336193245&subscription-key={subscription-key}
    ```

    İşte pinlerin renklerini değiştirdikten sonra yanıt görüntüsü:

    ![Güncelleştirilmiş tokalarla bir daire oluşturma](./media/how-to-render-custom-data/circle-updated-pins.png)

Benzer şekilde, diğer stil değiştiricilerini değiştirebilir, ekleyebilir ve kaldırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar


* Azure [Haritalar Harita Resim API belgelerini](https://docs.microsoft.com/rest/api/maps/render/getmapimage) inceleyin.
* Azure Haritalar Veri Hizmeti hakkında daha fazla bilgi edinmek için [hizmet belgelerine](https://docs.microsoft.com/rest/api/maps/data)bakın.

