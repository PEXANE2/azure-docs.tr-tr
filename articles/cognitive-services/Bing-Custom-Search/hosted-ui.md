---
title: Bing Özel Arama için barındırılan kullanıcı arabirimi yapılandırma | Microsoft Dokümanlar
titleSuffix: Azure Cognitive Services
description: Bing Özel Arama için barındırılan bir kullanıcı arabirimi yapılandırmak ve tümleştirmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.openlocfilehash: 2cc89bf57167db75404c044f58d18ab48edfaf38
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68854078"
---
# <a name="configure-your-hosted-ui-experience"></a>Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma

Bing Özel Arama, web sayfalarınıza ve web uygulamalarınıza JavaScript kodu snippet'i olarak kolayca entegre edilebilen barındırılan bir kullanıcı arabirimi sağlar. Bing Özel Arama portalını kullanarak Kullanıcı Arabirimi'nin düzen, renk ve arama seçeneklerini yapılandırabilirsiniz.



## <a name="configure-the-custom-hosted-ui"></a>Özel barındırılan kullanıcı ayını yapılandırma

Web uygulamalarınız için barındırılan bir kullanıcı arabirimi yapılandırmak için aşağıdaki adımları izleyin. Değişiklik yaptığınızda, sağdaki bölme size uI'nizin önizlemesini verecektir. Görüntülenen arama sonuçları, örneğiniz için gerçek sonuçlar değildir.

1. Bing Özel Arama [portalında](https://customsearch.ai)oturum açın.  
  
2. Bing Özel Arama örneğini seçin.

3. **Barındırılan kullanıcı arabirimi** sekmesine tıklayın.  
  
4. Bir düzen seçin.

    |  |  |
    |---------|---------|
    |Arama çubuğu ve sonuçları (varsayılan)    | Altında arama sonuçları olan bir arama kutusu görüntüler.         |
    |Yalnızca sonuçlar     | Arama kutusu olmadan yalnızca arama sonuçlarını görüntüler. Bu düzeni kullanırken, arama sorgusu`&q=<query string>`( ) sağlamanız gerekir. Sorgu parametresini JavaScript snippet'indeki istek URL'sine veya HTML bitiş noktası bağlantısına ekleyin.        |
    |Pop-over     | Bir arama kutusu sağlar ve arama sonuçlarını kayan bir bindirmede görüntüler.        |
    
5. Bir renk teması seçin. Özelleştir **temasını**tıklatarak renkleri uygulamanıza uyacak şekilde özelleştirebilirsiniz. Rengi değiştirmek için rengin RGB HEX değerini girin (örneğin), `#366eb8`veya renk önizlemesini tıklatın.

   Değişikliklerinizi portalın sağ tarafında önizleyebilirsiniz. Varsayılan **olarak Sıfırla'yı** tıklatmak, değişikliklerinizi seçili tema için varsayılan renklere geri ayarlar.

   > [!NOTE]
   > Renkleri seçerken erişilebilirliği göz önünde bulundurun.

6. **Ek Yapılandırmalar**altında, uygulamanız için uygun değerleri sağlayın. Bu ayarlar isteğe bağlıdır. Bunları uygulama nın veya kaldırmanın etkisini görmek için sağdaki önizleme bölmesine bakın. Kullanılabilir yapılandırma seçenekleri şunlardır:  

7. Arama abonelik anahtarını girin veya açılır listeden birini seçin. Açılır bırakma listesi, Azure hesabınızın aboneliklerinden gelen anahtarlarla doldurulur. [Bkz. Bilişsel Hizmetler API hesabı.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)  

8. Otomatik önermeyi etkinleştirdseniz, abonelik anahtarını otomatik olarak girin veya açılır listeden birini seçin. Açılır bırakma listesi, Azure hesabınızın aboneliklerinden gelen anahtarlarla doldurulur. Özel Otomatik Öner belirli bir abonelik katmanı gerektirir, [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)bakın.

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Özel Kullanıcı Bira Sıtkı tüket

Barındırılan UI'yi veya 

- Komut dosyasını web sayfanıza ekleme  
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- Veya bir Web tarayıcısında aşağıdaki URL'yi kullanın.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > Url'ye gerektiğinde aşağıdaki sorgu parametrelerini ekleyin. Bu parametreler hakkında bilgi için [Özel Arama API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters) başvurusuna bakın.
  >
  > - q
  > - mkt
  > - güvenli arama
  > - setlang

  > [!IMPORTANT]
  > Sayfa gizlilik bildiriminizi veya diğer bildirimleri ve koşulları görüntüleyemez. Kullanımınıza uygunluğu değişebilir.  

Özel Yapılandırma Kimliğiniz de dahil olmak üzere ek bilgi için **Üretim** sekmesi altındaki **Uç Noktaları'na** gidin.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Barındırdığınız Kişi Sayısı'nın davranışını Ek **Yapılandırmalar'ı**tıklatarak ve değerler sağlayarak yapılandırabilirsiniz. Bu ayarlar isteğe bağlıdır. Bunları uygulama nın veya kaldırmanın etkisini görmek için sağdaki önizleme bölmesine bakın. 

### <a name="web-search-configurations"></a>Web arama yapılandırmaları

|  |  |
|---------|---------|
|Web sonuçları etkin    | Web aramasının etkin olup olmadığını belirler (sayfanın üst kısmında bir Web sekmesi görürsünüz)        |
|Otomatik önermeyi etkinleştirme     | Özel otomatik önermenin etkin olup olmadığını belirler (ek maliyet için [fiyatlandırmaya](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) bakın).        |
|Sayfa başına web sonuçları    | Aynı anda görüntülenecek web arama sonuçlarının sayısı (sayfa başına en fazla 50 sonuç).        |
|Resim yazısı   | Görüntülerin arama sonuçlarıyla görüntülenip görüntülenmeyecektir belirler.|


**Gelişmiş yapılandırmaları göster'i**tıklattığınızda aşağıdaki yapılandırmalar gösterilir:


|  | |
|---------|---------|
|Sözcükleri vurgulama     | Sonuçların arama terimleriyle kalın olarak görüntülenip görüntülenmeyi belirler.         |
|Bağlantı hedefi    |  Kullanıcı bir arama sonucunu tıklattığında web sayfasının yeni bir tarayıcı sekmesinde (Boş) veya aynı tarayıcı sekmesinde (kendi kendine) açılıp açılmayanlarını belirler.        |

### <a name="image-search-configurations"></a>Resim arama yapılandırmaları

| | |
|---------|---------|
|Görüntü sonuçları etkin     | Resim aramasının etkin olup olmadığını belirler (sayfanın üst kısmında bir Resimler sekmesi görürsünüz).            |
|Sayfa başına resim sonuçları     | Bir seferde görüntülenecek resim arama sonuçlarının sayısı (sayfa başına en fazla 150 sonuç).          |

**Gelişmiş yapılandırmaları göster'i**tıklattığınızda aşağıdaki yapılandırma gösterilir.  
  
| | |
|---------|---------|
| Filtreleri etkinleştirme     | Kullanıcının Bing'in döndürdettiği görüntüleri filtrelemek için kullanabileceği filtreler ekler. Örneğin, kullanıcı sonuçları yalnızca animasyonlu GIF'ler için filtreleyebilir.|

### <a name="video-search-configurations"></a>Video arama yapılandırmaları

|  | |
|---------|---------|
|Video sonuçları etkin     | Video aramasının etkin olup olmadığını belirler (sayfanın üst kısmında videolar sekmesi görürsünüz).           |
|Sayfa başına video sonuçları   | Aynı anda görüntülenecek video arama sonuçlarının sayısı (sayfa başına en fazla 150 sonuç).        |

**Gelişmiş yapılandırmaları göster'i**tıklattığınızda aşağıdaki yapılandırma gösterilir.  
  
|  | |
|---------|---------|
|Filtreleri etkinleştirme    | Kullanıcının Bing'in döndürdettiği videoları filtrelemek için kullanabileceği filtreler ekler. Örneğin, kullanıcı son 24 saat içinde keşfedilen belirli bir çözünürlük veya videolar ile videoların sonuçlarını filtreleyebilir.          |

### <a name="miscellaneous-configurations"></a>Çeşitli yapılandırmalar


| |  |
|---------|---------|
|Sayfa başlığı   | Arama sonuçları sayfasının başlık alanında görüntülenen metin (açılır düzen için değil).        |
|Araç çubuğu teması    | Arama sonuçları sayfasının başlık alanının arka plan rengini belirler. |

**Gelişmiş yapılandırmaları göster'i**tıklattığınızda aşağıdaki yapılandırmalar gösterilir.  

|Sütun1  |Sütun2  |
|---------|---------|
|Kutu metin yer tutucusunda arama   | Girişten önce arama kutusunda görüntülenen metin.        |
|Başlık bağlantı url    |Başlık bağlantısı için hedef.         |
|Logo URL'si     | Başlığın yanında görüntülenen resim.         |
|Favicon    | Simge tarayıcının başlık çubuğunda görüntülenir.          |

Aşağıdaki yapılandırmalar yalnızca Barındırılan UI'yi HTML bitiş noktası üzerinden tüketirseniz uygulanır (JavaScript snippet'ini kullanıyorsanız geçerli değildir).

- Sayfa başlığı
- Araç çubuğu teması
- Başlık bağlantısı URL
- Logo URL'si
- Faviicon URL  

## <a name="next-steps"></a>Sonraki adımlar

- [Metni vurgulamak için süsleme işaretçilerini kullanma](../bing-web-search/hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
