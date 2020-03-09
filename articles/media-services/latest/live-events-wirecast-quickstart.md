---
title: Portal ve kablolu dönüştürme ile Azure Media Services canlı bir akış oluşturma
description: Azure Media Service Live Stream oluşturmayı öğrenin
services: media-services
ms.service: media-services
ms.topic: quickstart
ms.author: inhenkel
author: IngridAtMicrosoft
ms.date: 03/06/2020
ms.openlocfilehash: c0eaf3907cbfcd86424b1d2cbc03930a7af72786
ms.sourcegitcommit: 9cbd5b790299f080a64bab332bb031543c2de160
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2020
ms.locfileid: "78927629"
---
# <a name="create-a-azure-media-services-live-stream-with-the-portal-and-wirecast"></a>Portal ve kablolu dönüştürme ile Azure Media Services canlı bir akış oluşturma

Bu başlangıç kılavuzunda, bir Azure aboneliğiniz olduğunu ve bir Azure Media Services hesabı oluşturduğunuzu varsayılmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

Web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

Bu hızlı başlangıçta şunları ele alınacaktır:

- Ücretsiz Telestream Kablobir deneme sürümü ile şirket içi kodlayıcı ayarlama
- Canlı akış ayarlama
- Canlı akış çıkışlarını ayarlama
- Varsayılan akış uç noktası çalıştırma
- Canlı akışı ve isteğe bağlı çıktıyı görüntülemek için Azure Media Player kullanma

Şeyleri basit tutmak için, kablolu atama, geçişli bulut kodlaması ve RTMP 'de Azure Media Services için bir kodlama ön ayarı kullanacağız.

## <a name="setting-up-an-on-premises-encoder-with-wirecast"></a>Kablolu dönüştürme ile şirket içi kodlayıcı ayarlama

1. https://www.telestream.net adresinden işletim sisteminiz için kablolu dönüştürme 'yi indirip yükleyin
1. Uygulamayı başlatın ve ürünü kaydetmek için sık kullandığınız e-posta adresinizi kullanın.  Uygulamayı açık tutun.
1. E-posta adresinizi doğrulamanızı isteyen bir e-posta alacaksınız, ardından uygulama ücretsiz denemeyi başlatacak.
1. ÖNERILIR: uygulama açma ekranında video öğreticisini Izleyin.

## <a name="setting-up-an-azure-media-services-live-stream"></a>Azure Media Services canlı akış ayarlama

1. Portalda Azure Media Services hesabına gittikten sonra, Media Services listesinden **canlı akış** ' ı seçin.
1. Yeni bir canlı akış olayı oluşturmak için **canlı etkinlik Ekle** ' ye tıklayın.
1. Canlı olay **adı** alanına *testliveevent* gibi yeni olaylarınız için bir ad girin.
1. **Açıklama** alanına olay için isteğe bağlı bir açıklama girin.
1. **Doğrudan geçiş – bulut kodlaması yok** radyo düğmesini seçin.
1. **RTMP** radyo düğmesini seçin. 
1. Canlı etkinlik için ücretsiz olarak faturalandırılmaya engel olmak üzere canlı etkinlik için **hiçbir** radyo düğmesinin seçili olmadığından emin olun.  (Faturalandırma, canlı olay başlatıldıktan sonra başlatılır.)
1. Ayarları gözden geçirmek için **gözden geçir + oluştur** düğmesine tıklayın.
1. Canlı etkinliği oluşturmak için **Oluştur** düğmesine tıklayın. Daha sonra canlı etkinlik listesi görünümüne döndürülecektir.
1. Az önce oluşturduğunuz **canlı etkinliğin bağlantısına** tıklayın. Olaylarınızın durdurulduğundan emin olun.
1. Bu sayfayı tarayıcınızda açık tutun.  Daha sonra bu yüklemeye geri döneceksiniz.

## <a name="setting-up-a-live-stream-with-wirecast-studio"></a>Kablolu yayın Studio ile canlı akış ayarlama

1. Kablolu atama uygulamasının hala açık olduğunu varsayarsak, ana menüden **boş belge oluştur** ' u seçin ve ardından **devam**' a tıklayın.
1. Kablolu atama katmanları alanındaki ilk katmanın üzerine gelin.  Görüntülenen **Ekle** simgesine tıklayın ve akışa almak istediğiniz video girişini seçin.  Ana katman 1 iletişim kutusu açılır.
1. Menüden **video yakalama** ' yı seçin ve ardından kullanmak istediğiniz kamerayı seçin. Kameradaki Görünüm önizleme alanında görüntülenir.
1. Kablolu atama katmanları alanındaki ikinci katmanın üzerine gelin. Görüntülenen **Ekle** simgesine tıklayın ve akışa almak istediğiniz ses girişini seçin.  Ana katman 2 iletişim kutusu açılır.
1. Menüden **ses yakalama** ' yı seçin ve ardından kullanmak istediğiniz ses girişini seçin. 
1. Ana menüden **Çıkış ayarları**' nı seçin.  Çıkış iletişim kutusu görüntülenir.
1. Çıkış açılan listesinden **Azure Media Services** seçin.  Azure Media Services için çıkış ayarı, çıkış ayarlarının *çoğunu* otomatik olarak doldurur.
1. Sonraki bölümde, çıkış ayarlarına girmek üzere *GIRIŞ URL* 'sini kopyalamak için tarayıcınızda Azure Media Services geri dönebilirsiniz.

### <a name="copy-and-paste-the-input-url"></a>Giriş URL 'sini kopyalayıp yapıştırın

1. Portalın Azure Media Services sayfasına döndüğünüzde canlı akış olayını başlatmak için **Başlat** ' a tıklayın. (Faturalandırma şimdi başlar.)
2. Güvenli **/güvenli değil** düğmesine tıklayarak **güvenli değil**olarak ayarlayın.  Bu, protokolü RTMPS yerine RTMP 'ye ayarlar.
3. **GIRIŞ URL** 'sini panonuza kopyalayın.
4. Kablolu atama uygulamasına geçin ve **GIRIŞ URL** 'sini çıkış ayarlarındaki **Adres** alanına yapıştırın.
5. **Tamam**' a tıklayın.

## <a name="setting-up-outputs"></a>Çıkışları ayarlama

Bu bölüm, çıktılarınızı ayarlar ve canlı akışınızın bir kaydını kaydetmenizi sağlar.  

> [!NOTE]
> Bu çıkışın akışını sağlamak için akış uç noktasının çalışıyor olması gerekir.  Bkz. varsayılan akış uç noktası bölümünü çalıştırma.

1. Çıktılar video görüntüleyicisinin altındaki **çıktılar oluştur** bağlantısına tıklayın.
1. İsterseniz, daha sonra bulmayı kolaylaştırmak için **ad** alanındaki çıkışın adını daha kolay bir şekilde düzenleyin.
1. Tüm kalan alanları şu anda yalnız bırakın.
1. **İleri** akış Bulucu Ekle ' ye tıklayın.
1. Eğer istenirse, bulucunun adını daha kolay bir şekilde değiştirin.
1. Bu ekranda bulunan her şeyi şimdilik yalnız bırakın.
1. **Oluştur**'a tıklayın.

## <a name="starting-the-broadcast"></a>Yayın başlatılıyor

1. Kabloda bulunan çıkış ' ı seçin **> yayını başlat/durdur > başlat Azure Media Services:** ana menüden Azure Media Services.  Akış canlı olaya gönderildiğinde, kablodaki canlı bir pencere, Azure Media Services 'teki canlı etkinlik sayfasında canlı olay video oynatıcı 'da görünür.

1. Kablolu atama katmanları için seçtiğiniz videoyu ve sesi yayınlamayı başlatmak için Önizleme penceresinin altındaki **Git** düğmesine tıklayın.

> [!TIP]
> Bir hata varsa, Player 'ın üzerindeki oynatıcıyı yeniden yükle bağlantısına tıklayarak oynatıcıyı yeniden yüklemeyi deneyin.

## <a name="running-the-default-streaming-endpoint"></a>Varsayılan akış uç noktasını çalıştırma

1. Media Services listesinde **akış uç noktaları** ' nı seçerek akış uç noktanızın çalıştığından emin olun. Akış uç noktaları sayfasına yönlendirilirsiniz.
1. Varsayılan akış uç noktası durumu durdurulmuşsa, **varsayılan** akış uç noktasına tıklayın. Bu, sizi bu uç noktanın sayfasına götürür.
1. **Başlat**’a tıklayın.  Bu işlem akış uç noktasını başlatacak.

## <a name="play-the-output-broadcast-with-azure-media-player"></a>Çıkış yayınını Azure Media Player oynatma

1. Çıkış video oyuncusunun altındaki **AKıŞ URL** 'sini kopyalayın. 
1. Bir Web tarayıcısında, demo Azure Media Player açın https://ampdemo.azureedge.net/azuremediaplayer.html
1. **AKıŞ URL** 'SINI Azure Media Player URL alanına yapıştırın.
1. **Oynatıcı Güncelleştir** düğmesine tıklayın.
1. Canlı akışınızı görmek için videoda **oynat** simgesine tıklayın.

## <a name="stopping-the-broadcast"></a>Yayın durduruluyor

Yeterli içerik akışı olduğunu düşündüğünüzde yayını durdurun.

1. Kabloda bulunan **yayın** düğmesine tıklayın.  Bu, yayını kablodan durduracak şekilde durdurur.
1. Portalda **Durdur**' a tıklayın. Canlı akışın durdığına ilişkin bir uyarı iletisi alırsınız, ancak çıktı artık isteğe bağlı bir varlık olur.
1. Uyarı iletisindeki **Durdur** ' a tıklayın. Canlı akış artık kullanılamadığından Azure Media Player de bir hata gösterir.

## <a name="play-the-on-demand-output-with-the-azure-media-player"></a>Azure Media Player ile isteğe bağlı çıktıyı yürütün

Oluşturduğunuz çıktı artık akış uç noktası çalıştığı sürece isteğe bağlı akış için kullanılabilir.

1. Media Services listesine gidin ve **varlıklar**' ı seçin.
1. Daha önce oluşturduğunuz olay çıktısını bulun ve **varlığın bağlantısına**tıklayın. Varlık çıkış sayfası açılır.
1. Varlık için video oynatıcı altındaki **AKıŞ URL** 'sini kopyalayın.
1. Tarayıcıdaki Azure Media Player geri dönüp **AKıŞ URL** 'SINI Azure Media Player URL alanına yapıştırın.
1. **Oynatıcıyı Güncelleştir**' e tıklayın.
1. İsteğe bağlı varlığı görüntülemek için videoda **oynat** simgesine tıklayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

  > [!IMPORTANT]
  > Hizmetleri durdurun! Bu hızlı başlangıçta bulunan adımları tamamladıktan sonra, canlı olay ve akış uç noktasını durdurmanız ve çalışır durumda kaldığı süre için faturalandırılmaya devam etmeniz gerekir. Canlı etkinliği durdurmak için, yukarıdaki yayını durdurma, adım 2 ve 3 ' e bakın.

### <a name="stopping-the-streaming-endpoint"></a>Akış uç noktası durduruluyor

1. Media Services listesinden **akış uç noktaları**' nı seçin.
2. Daha önce başlattığınız **varsayılan** akış uç noktasına tıklayın. Bu, uç noktanın sayfasını açar.
3. **Durdur**' a tıklayın.  Bu işlem akış uç noktasını durdurur.

>[!TIP]
>Varlıkları bu olaydan korumak istemiyorsanız, depolama için faturalandırılmaya engel olmak için bunları silmeyi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Sıradaki Makale](./live-events-outputs-concept.md)
