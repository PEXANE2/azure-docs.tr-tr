---
title: Portal ve Wirecast ile Azure Medya Hizmetleri Canlı akışı oluşturun
description: Azure Media Hizmeti Canlı akışı oluşturma yı öğrenin
services: media-services
ms.service: media-services
ms.topic: quickstart
ms.author: inhenkel
author: IngridAtMicrosoft
ms.date: 03/25/2020
ms.openlocfilehash: e5bdd75ca61d53a64f003633d74e3d8f7992a98b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80336550"
---
# <a name="create-a-azure-media-services-live-stream-with-the-portal-and-wirecast"></a>Portal ve Wirecast ile Azure Medya Hizmetleri Canlı akışı oluşturun

Bu Başlangıç Kılavuzu, bir Azure aboneliğiniz olduğunu ve bir Azure Medya Hizmetleri hesabı oluşturduğunuzu varsayar.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

Web tarayıcınızı açıp [Microsoft Azure portalı](https://portal.azure.com/)’na gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

Bu hızlı başlangıçta, şunları ele alacağız:

- Telestream Wirecast ücretsiz deneme ile bir şirket içi kodlayıcı kurma
- Canlı akış ayarlama
- Canlı akış çıktılarını ayarlama
- Varsayılan akış bitiş noktası çalıştırma
- Canlı akışı ve isteğe bağlı çıktıyı görüntülemek için Azure Media Player'ı kullanma

İşleri basit tutmak için, Wirecast, pass-through bulut kodlama ve RTMP'de Azure Medya Hizmetleri için bir kodlama ön kümesi kullanacağız.

## <a name="setting-up-an-on-premises-encoder-with-wirecast"></a>Wirecast ile şirket içi kodlayıcı ayarlama

1. İşletim sisteminiz için Wirecast'ı indirin ve kurunhttps://www.telestream.net
1. Uygulamayı başlatın ve ürünü kaydetmek için en sevdiğiniz e-posta adresini kullanın.  Uygulamayı açık tutun.
1. E-posta adresinizi doğrulamanızı isteyen bir e-posta alacaksınız, ardından uygulama ücretsiz deneme sürümünü başlatacaktır.
1. ÖNERİlEN: Açılış uygulama ekranında video eğitimi izleyin.

## <a name="setting-up-an-azure-media-services-live-stream"></a>Azure Medya Hizmetleri canlı akışı ayarlama

1. Portaldaki Azure Medya Hizmetleri hesabına geçtikten sonra, Medya Hizmetleri listesinden **Canlı akış'ı** seçin.<br/>
![Canlı akış bağlantısını seçin](media/live-events-wirecast-quickstart/select-live-streaming.png)<br/>
1. Yeni bir canlı akış etkinliği oluşturmak için **Canlı Ekle etkinliğini** tıklatın.<br/>
![Canlı etkinlik simgesi ekleme](media/live-events-wirecast-quickstart/add-live-event.png)<br/>
1. Canlı etkinlik **Adı** alanına *TestLiveEvent* gibi yeni etkinliğiniz için bir ad girin.<br/>
![Canlı olay adı metin alanı](media/live-events-wirecast-quickstart/live-event-name.png)<br/>
1. **Açıklama** alanına olayın isteğe bağlı açıklamasını girin.
1. **Pass-through 'ı** seçin – bulut kodlama radyo düğmesini yok.<br/>
![Bulut kodlama radyo düğmesi](media/live-events-wirecast-quickstart/cloud-encoding.png)
1. **RTMP** radyo düğmesini seçin.
1. Canlı etkinlik hazır olmadan önce faturalandırılmayı önlemek için Canlı Etkinliği Başlat için **Radyo Yok** düğmesinin seçildiğinden emin olun.  (Canlı etkinlik başladıktan sonra faturalandırma başlar.) ![Canlı olay radyo düğmesini başlat](media/live-events-wirecast-quickstart/start-live-event-no.png)<br/>
1. Ayarları incelemek için **Gözden Geçir + oluştur** düğmesini tıklatın.
1. Canlı etkinliği oluşturmak için **Oluştur** düğmesini tıklatın. Daha sonra canlı etkinlik giriş görünümüne döndürülür.
1. Az önce oluşturduğunuz **canlı etkinliğin bağlantısını** tıklayın. Etkinliğinizin durdurulduğuna dikkat edin.
1. Bu sayfayı tarayıcınızda açık tutun.  Daha sonra geri döneriz.

## <a name="setting-up-a-live-stream-with-wirecast-studio"></a>Wirecast Studio ile canlı yayın kurma

1. Wirecast uygulamasını hala açık olarak gördüğünüzden, ana menüden **Boş Belge Oluştur'u** seçin ve ardından **Devam Et'i**tıklatın.
![Wirecast başlangıç ekranı](media/live-events-wirecast-quickstart/open-empty-document.png)
1. Wirecast katmanları alanında ilk katman üzerinde hover.  Görünen **Ekle** simgesini tıklatın ve akış yapmak istediğiniz video girişini seçin.  Ana Katman 1 diyalog kutusu açılacaktır.<br/>
![Wirecast simge ekle](media/live-events-wirecast-quickstart/add-icon.png)
1. Menüden **Video yakalama'yı** seçin ve ardından kullanmak istediğiniz kamerayı seçin. Kamera seçilirken, kameranın görünümü Önizleme alanında görünür.
![Wirecast video çekimi seçim ekranı](media/live-events-wirecast-quickstart/video-shot-selection.png)
1. Wirecast katmanları alanında ikinci katmanın üzerine titreyin. Görünen **Ekle** simgesini tıklatın ve akış yapmak istediğiniz ses girişini seçin.  Ana Katman 2 diyalog kutusu açılacaktır.
1. Menüden **Ses yakalama'yı** seçin ve ardından kullanmak istediğiniz ses girişini seçin.
![Wirecast ses çekimi seçim ekranı](media/live-events-wirecast-quickstart/audio-shot-select.png)
1. Ana menüden **Çıktı ayarlarını**seçin.  Çıktı iletişim kutusu görüntülenir.
1. Çıktı açılır tarihinden **Azure Medya Hizmetleri'ni** seçin.  Azure Medya Hizmetleri'nin çıktı ayarı, çıktı ayarlarının *çoğunu* otomatik olarak dolduracaktır.<br/>
![Wirecast çıkış ayarları ekranı](media/live-events-wirecast-quickstart/azure-media-services.png)
1. Bir sonraki bölümde, çıktı ayarlarına girmek için *Giriş URL'sini* kopyalamak için tarayıcınızdaki Azure Medya Hizmetleri'ne geri döneceksiniz.

### <a name="copy-and-paste-the-input-url"></a>Giriş URL'sini kopyalayıp yapıştırın

1. Portalın Azure Medya Hizmetleri sayfasında canlı akış etkinliğini başlatmak için **Başlat'ı** tıklatın. (Faturalandırma şimdi başlar.)<br/>
![Başlat simgesi](media/live-events-wirecast-quickstart/start.png)
2. Güvenli **Değil'e**ayarlamak için **Güvenli/Güvenli Değil** geçişini tıklatın.  Bu, protokolü RTMPS yerine RTMP olarak ayarlar.
3. Giriş **URL'sini** panonuza kopyalayın.
![Giriş URL'si](media/live-events-wirecast-quickstart/input-url.png)
4. Wirecast uygulamasına geçin ve **Giriş URL'sini** Çıkış ayarlarındaki **Adres** alanına yapıştırın.<br/>
![Wirecast giriş URL'si](media/live-events-wirecast-quickstart/input-url-wirecast.png)
5. **Tamam'ı**tıklatın.

## <a name="setting-up-outputs"></a>Çıktıları ayarlama

Bu bölüm çıktılarınızı ayarlar ve canlı akışınızın kaydını kaydetmenizi sağlar.  

> [!NOTE]
> Bu çıktıyı akışla aktarabilmek için akış bitiş noktasının çalışıyor olması gerekir.  Bkz. Aşağıdaki varsayılan akış bitiş noktası bölümünü çalıştırma.

1. Çıktılar video görüntüleyicinin altındaki **Çıktıoluştur** bağlantısına tıklayın.
1. İsterseniz, daha sonra bulmak kolay böylece daha kullanıcı dostu bir şey **Için Ad** alanında çıktının adını edin.
![Çıktı adı alanı](media/live-events-wirecast-quickstart/output-name.png)
1. Şimdilik tüm tarlaları yalnız bırak.
1. **Sonraki** akış bulucu ekle'yi tıklatın.
1. İstenirse, yer bulucuadını daha kullanıcı dostu bir şeyle değiştirin.
![Konum belirleyici ad alanı](media/live-events-wirecast-quickstart/live-event-locator.png)
1. Şimdilik bu ekranda her şeyi yalnız bırakın.
1. **Oluştur'u**tıklatın.

## <a name="starting-the-broadcast"></a>Yayını başlatma

1. Wirecast'te, Ana menüden **Azure Medya Hizmetleri: Azure Medya Hizmetleri > Başlat / Yayını Durdur> Çıktı'yı** seçin.  Akış canlı etkinliğe gönderildiğinde, Wirecast'teki Live penceresi Azure Medya Hizmetleri'ndeki canlı etkinlik sayfasında canlı etkinlik video oynatıcısında gösterilecek.

   ![Yayın menüsü öğelerini başlat](media/live-events-wirecast-quickstart/start-broadcast.png)

1. Wirecast katmanları için seçtiğiniz videoyu ve sesi yayınlamaya başlamak için önizleme penceresinin altındaki **Git** düğmesini tıklatın.

   ![Wirecast Go düğmesi](media/live-events-wirecast-quickstart/go-button.png)

   > [!TIP]
   > Bir hata varsa, oyuncunun üstündeki Oyuncuyu Yeniden Yükle bağlantısını tıklayarak oyuncuyu yeniden yüklemeyi deneyin.

## <a name="running-the-default-streaming-endpoint"></a>Varsayılan akış bitiş noktasını çalıştırma

1. Medya Hizmetleri listesinde **Akış uç noktalarını** seçerek akış bitiş noktanızın çalışmadığından emin olun. Akış uç noktaları sayfasına götürülür.<br/>
![Akış uç noktası menü öğesi](media/live-events-wirecast-quickstart/streaming-endpoints.png)
1. Varsayılan akış uç noktası durumu durdurulursa, **varsayılan** akış bitiş noktasını tıklatın. Bu, sizi bu bitiş noktası için sayfaya götürür.
1. **Başlat**'a tıklayın.  Bu, akış bitiş noktasını başlatır.<br/>
![Akış uç noktası menü öğesi](media/live-events-wirecast-quickstart/start.png)

## <a name="play-the-output-broadcast-with-azure-media-player"></a>Çıktı yayınını Azure Media Player ile oynatın

1. Akış **URL'sini** Çıktı video oynatıcısının altında kopyalayın.
1. Bir web tarayıcısında Azure Media Player demosuhttps://ampdemo.azureedge.net/azuremediaplayer.html
1. **Akış URL'sini** Azure Media Player'ın URL alanına yapıştırın.
1. Oyuncuyu **Güncelleştir** düğmesini tıklatın.
1. Canlı akışınızı görmek için videodaki **oynat simgesini** tıklatın.

## <a name="stopping-the-broadcast"></a>Yayını durdurma

Yeterli içerik akışı yaptığınızı düşündüğünüzde yayını durdurun.

1. Wirecast'te **yayın** düğmesine tıklayın.  Bu Wirecast'ten yayını durduracak.
1. **Portalda, Stop'a**tıklayın. Canlı yayının durduğunu ancak çıktının artık isteğe bağlı bir varlık haline geldiğini belirten bir uyarı iletisi alırsınız.
1. Uyarı iletisinde **Durdur'u** tıklatın. Azure Media Player da artık canlı akış kullanılamadığı için bir hata gösterir.

## <a name="play-the-on-demand-output-with-the-azure-media-player"></a>İsteğe bağlı çıktıyı Azure Media Player ile oynayın

Oluşturduğunuz çıktı, akış bitiş noktanız çalışmaya devam ediyorsa isteğe bağlı akış için kullanılabilir.

1. Medya Hizmetleri listesine gidin ve **Varlıklar'ı**seçin.
1. Daha önce oluşturduğunuz olay çıktısını bulun ve **varlığa**olan bağlantıyı tıklayın. Varlık çıktı sayfası açılır.
1. Varlık için video oynatıcının altındaki **Akış URL'sini** kopyalayın.
1. Tarayıcıdaki Azure Media Player'a dönün ve **Akış URL'sini** Azure Media Player'ın URL alanına yapıştırın.
1. **Player'ı Güncelle'yi**tıklatın.
1. İsteğe bağlı varlığı görüntülemek için videodaki **oynatma** simgesini tıklatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

> [!IMPORTANT]
> Hizmetleri durdurun! Bu hızlı başlangıçtaki adımları tamamladıktan sonra, canlı olayı ve akış bitiş noktasını durdurduğunuzdan emin olun, yoksa çalışmaya devam ettikleri süre için faturalandırılamaya devam edin. Canlı etkinliği durdurmak için yayını durdurma, yukarıdaki adım 2 ve 3'e bakın.

### <a name="stopping-the-streaming-endpoint"></a>Akış bitiş noktasını durdurma

1. Medya Hizmetleri listesinden **Akış uç noktalarını**seçin.
2. Daha önce başlattığınız **varsayılan** akış bitiş noktasını tıklatın. Bu, bitiş noktasının sayfasını açar.
3. **Stop'a**tıklayın.  Bu, akış bitiş noktasını durdurur.

> [!TIP]
> Varlıkların bu olaydan uzak tutulmasını istemiyorsanız, depolama için faturalandırılmayı önlemek için bunları sildiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Sırayla sırada sırada hangi makale](./live-events-outputs-concept.md)
