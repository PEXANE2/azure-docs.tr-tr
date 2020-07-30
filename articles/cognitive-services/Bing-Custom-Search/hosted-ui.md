---
title: Bing Özel Arama için barındırılan bir kullanıcı arabirimi yapılandırma | Microsoft Docs
titleSuffix: Azure Cognitive Services
description: Bing Özel Arama için barındırılan bir kullanıcı arabirimini yapılandırmak ve bütünleştirmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.custom: devx-track-javascript
ms.openlocfilehash: 857195e0befb95035572b5b52a01d151b332518e
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406359"
---
# <a name="configure-your-hosted-ui-experience"></a>Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma

Bing Özel Arama, Web sayfalarınızı ve Web uygulamalarınızı JavaScript kod parçacığı olarak kolayca tümleştirebileceğinizi barındırılan bir kullanıcı arabirimi sağlar. Bing Özel Arama portalını kullanarak, Kullanıcı arabiriminin düzen, renk ve arama seçeneklerini yapılandırabilirsiniz.



## <a name="configure-the-custom-hosted-ui"></a>Özel barındırılan Kullanıcı arabirimini yapılandırma

Web uygulamalarınız için barındırılan bir kullanıcı arabirimi yapılandırmak için aşağıdaki adımları izleyin. Değişiklik yaparken, sağdaki bölme size, Kullanıcı arabiriminizi önizlemede bir önizleme sağlayacak. Görüntülenen arama sonuçları, örneğiniz için gerçek sonuçlar değildir.

1. Bing Özel Arama [portalında](https://customsearch.ai)oturum açın.  
  
2. Bing Özel Arama örneğinizi seçin.

3. **Barındırılan kullanıcı arabirimi** sekmesine tıklayın.  
  
4. Bir düzen seçin.

    - Arama çubuğu ve sonuçları (varsayılan): altında arama sonuçlarıyla birlikte bir arama kutusu görüntüler.
    - Yalnızca sonuçlar: arama kutusu olmadan yalnızca arama sonuçlarını görüntüler. Bu düzeni kullanırken, arama sorgusu () sağlamanız gerekir `&q=<query string>` . Sorgu parametresini JavaScript kod parçacığındaki istek URL 'sine veya HTML uç nokta bağlantısını ekleyin.
    - Açılır pencere: bir arama kutusu sağlar ve arama sonuçlarını kayan bir kaplama içinde görüntüler.

5. Bir renk teması seçin. **Temayı Özelleştir**' e tıklayarak renkleri uygulamanıza uyacak şekilde özelleştirebilirsiniz. Bir rengi değiştirmek için, rengin RGB ONALTıLıK değerini girin (örneğin, `#366eb8` ) veya renk önizlemeye tıklayın.

   Portalın sağ tarafında yaptığınız değişikliklerin önizlemesini yapabilirsiniz. **Varsayılana sıfırla** ' ya tıkladığınızda seçili temanın varsayılan renkleriyle yaptığınız değişiklikler geri alınır.

   > [!NOTE]
   > Renk seçerken erişilebilirliği göz önünde bulundurun.

6. **Ek konfigürasyonlar**' ın altında, uygulamanız için uygun değerleri sağlayın. Bu ayarlar isteğe bağlıdır. Uygulama veya kaldırma etkisini görmek için sağdaki önizleme bölmesine bakın. Kullanılabilir yapılandırma seçenekleri şunlardır:  

7. Arama aboneliği anahtarını girin veya açılan listeden bir tane seçin. Açılan liste, Azure hesabınızın aboneliklerinizdeki anahtarlarla doldurulur. Bilişsel [HIZMETLER API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)' na bakın.  

8. Otomatik öner 'i etkinleştirdiyseniz, otomatik öneri aboneliği anahtarını girin veya açılan listeden bir tane seçin. Açılan liste, Azure hesabınızın aboneliklerinizdeki anahtarlarla doldurulur. Özel otomatik öneri için belirli bir abonelik katmanı gerekir, bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Özel Kullanıcı arabirimini tüketme

Barındırılan Kullanıcı arabirimini kullanmak için şunlardan birini yapın: 

- Web sayfanıza betiği ekleyin  
  
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

- Veya bir Web tarayıcısında aşağıdaki URL 'YI kullanın.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > Aşağıdaki sorgu parametrelerini URL 'ye gerektiği şekilde ekleyin. Bu parametreler hakkında daha fazla bilgi için bkz. [özel arama API'si](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters) Reference.
  >
  > - q
  > - mkt
  > - Güvenli Arama
  > - setlang

  > [!IMPORTANT]
  > Sayfa Gizlilik Bildirimimizi veya diğer bildirimleri ve koşulları görüntüleyemiyor. Kullanım için uygunluk farklılık gösterebilir.  

Özel yapılandırma KIMLIĞINIZ dahil olmak üzere ek bilgiler için **Üretim** sekmesinin altındaki **uç noktalar** ' a gidin.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

**Ek konfigürasyonlar**' a tıklayarak ve değer sağlayarak barındırılan Kullanıcı arabiriminin davranışını yapılandırabilirsiniz. Bu ayarlar isteğe bağlıdır. Uygulama veya kaldırma etkisini görmek için sağdaki önizleme bölmesine bakın. 

### <a name="web-search-configurations"></a>Web araması yapılandırması

- Web sonuçları etkin: Web aramasının etkinleştirilip etkinleştirilmediğini belirler (sayfanın üst kısmında bir Web sekmesi görürsünüz)
- Otomatik öneri etkinleştir: özel otomatik önerin etkinleştirilip etkinleştirilmediğini belirler (ek maliyet için [fiyatlandırmayı](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) görüntüleyin).
- Sayfa başına Web sonucu: tek seferde görüntülenecek web araması sonuçlarının sayısı (sayfa başına en fazla 50 sonuç).
- Resim başlığı: görüntülerin arama sonuçlarıyla görüntülenip görüntülenmediğini belirler.

**Gelişmiş yapılandırma göster**' e tıklarsanız aşağıdaki konfigürasyonlar gösterilir:

- Sözcükleri vurgula: sonuçların, arama terimleriyle kalın olarak gösterilip gösterilmeyeceğini belirler.
- Bağlantı hedefi: Kullanıcı bir arama sonucuna tıkladığında, Web sayfasının yeni bir tarayıcı sekmesinde (boş) veya aynı tarayıcı sekmesinde (kendi) açılıp açılamayacağını belirler.

### <a name="image-search-configurations"></a>Görüntü arama yapılandırması

- Görüntü sonuçları etkin: görüntü aramasının etkinleştirilip etkinleştirilmediğini belirler (sayfanın üst kısmında bir görüntüler sekmesi görürsünüz).
- Sayfa başına görüntü sonucu: tek seferde görüntülenecek görüntü arama sonuçlarının sayısı (sayfa başına en fazla 150 sonuç).

**Gelişmiş yapılandırmaları göster**' e tıkladığınızda aşağıdaki yapılandırma gösterilir.  
  
- Filtreleri etkinleştir: kullanıcının Bing tarafından döndürülen görüntüleri filtrelemek için kullanabileceği filtreleri ekler. Örneğin, Kullanıcı yalnızca animasyonlu GIF 'Ler için sonuçlara filtre uygulayabilir.

### <a name="video-search-configurations"></a>Video arama yapılandırması

- Video sonuçları etkin: video aramasının etkinleştirilip etkinleştirilmediğini belirler (sayfanın üst kısmında bir videolar sekmesi görürsünüz).
- Sayfa başına video sonucu: tek seferde görüntülenecek video arama sonuçlarının sayısı (sayfa başına en fazla 150 sonuç).

**Gelişmiş yapılandırmaları göster**' e tıkladığınızda aşağıdaki yapılandırma gösterilir.  
  
- Filtreleri etkinleştir: kullanıcının, Bing tarafından döndürülen videoları filtrelemek için kullanabileceği filtreleri ekler. Örneğin Kullanıcı, son 24 saat içinde bulunan belirli bir çözünürlük veya videoların bulunduğu videoların sonuçlarını filtreleyebilir.

### <a name="miscellaneous-configurations"></a>Çeşitli yapılandırma

- Sayfa başlığı: arama sonuçları sayfasının başlık alanında görüntülenen metin (açılır düzen için değil).
- Toolbar teması: arama sonuçları sayfasının başlık alanının arka plan rengini belirler.

**Gelişmiş yapılandırma göster**' e tıklarsanız aşağıdaki konfigürasyonlar gösterilir.  

|Sütun1  |Sütun2  |
|---------|---------|
|Arama kutusu metin yer tutucusu   | Girişten önce arama kutusunda görüntülenecek metin.        |
|Başlık bağlantısı URL 'si    |Başlık bağlantısı için hedef.         |
|Logo URL 'SI     | Başlığın yanında görüntülenecek resim.         |
|Öncelik simgesi    | Tarayıcının başlık çubuğunda görünen simge.          |

Aşağıdaki yapılandırma yalnızca, HTML uç noktası aracılığıyla barındırılan Kullanıcı arabirimini kullanacaksanız geçerlidir (JavaScript kod parçacığını kullanırsanız uygulanmaz).

- Sayfa başlığı
- Araç çubuğu teması
- Başlık bağlantısı URL 'SI
- Logo URL 'SI
- Faviıcon URL 'SI  

## <a name="next-steps"></a>Sonraki adımlar

- [Metni vurgulamak için süsleme işaretçilerini kullanma](../bing-web-search/hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
