---
title: Azure Medya Hizmetleri canlı akışı oluşturma
description: Portalı ve Wirecast'i kullanarak Azure Medya Hizmetleri canlı akışını nasıl oluşturabileceğinizi öğrenin
services: media-services
ms.service: media-services
ms.topic: quickstart
ms.author: inhenkel
author: IngridAtMicrosoft
ms.date: 03/25/2020
ms.openlocfilehash: 459f11844f873a911b3e5702e8c768b1cd22e504
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985006"
---
# <a name="create-an-azure-media-services-live-stream"></a>Azure Medya Hizmetleri canlı akışı oluşturma

Bu hızlı başlangıç, Azure portalını ve Telestream Wirecast'ı kullanarak bir Azure Medya Hizmetleri canlı akışı oluşturmanıza yardımcı olur. Azure aboneliğiniz olduğunu ve bir Medya Hizmetleri hesabı oluşturduğunuzu varsayar.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Web tarayıcınızı açın ve [Microsoft Azure portalına](https://portal.azure.com/)gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

Bu hızlı başlangıçta, şunları ele alacağız:

- Telestream Wirecast'in ücretsiz denemesi ile şirket içi bir kodlayıcı kurmak.
- Canlı yayın ayarlama.
- Canlı akış çıktılarını ayarlama.
- Varsayılan akış bitiş noktası çalıştırın.
- Canlı akışı ve isteğe bağlı çıktıyı görüntülemek için Azure Media Player'ı kullanma.

İşleri basit tutmak için, Wirecast, pass-through bulut kodlama ve RTMP'de Azure Medya Hizmetleri için bir kodlama ön kümesi kullanırız.

## <a name="set-up-an-on-premises-encoder-by-using-wirecast"></a>Wirecast'i kullanarak şirket içi kodlayıcı yı ayarlama

1. [Telestream web sitesinde](https://www.telestream.net)işletim sisteminiz için Wirecast'ı indirin ve kurun.
1. Uygulamayı başlatın ve ürünü kaydetmek için en sevdiğiniz e-posta adresini kullanın. Uygulamayı açık tutun.
1. Aldığınız e-postada e-posta adresinizi doğrulayın. Daha sonra uygulama ücretsiz deneme başlayacaktır.
1. Önerilen: Açılış uygulama ekranında video eğitimi izleyin.

## <a name="set-up-an-azure-media-services-live-stream"></a>Azure Medya Hizmetleri canlı yayınını ayarlama

1. Portaldaki Azure Medya Hizmetleri hesabına gidin ve ardından **Medya Hizmetleri** listesinden **Canlı akış'ı** seçin.

   ![Canlı akış bağlantısı](media/live-events-wirecast-quickstart/select-live-streaming.png)
1. Yeni bir canlı akış olayı oluşturmak için **canlı olay ekle'yi** seçin.

   ![Canlı etkinlik simgesi ekleme](media/live-events-wirecast-quickstart/add-live-event.png)
1. Canlı etkinlik adı kutusuna *TestLiveEvent*gibi yeni **etkinliğiniz** için bir ad girin.

   ![Canlı etkinlik adı kutusu](media/live-events-wirecast-quickstart/live-event-name.png)
1. **Açıklama** kutusuna olayın isteğe bağlı açıklamasını girin.
1. **Pass-through 'ı** seçin – bulut kodlama seçeneği yok.

   ![Bulut kodlama seçeneği](media/live-events-wirecast-quickstart/cloud-encoding.png)
1. **RTMP** seçeneğini seçin.
1. Canlı etkinlik hazır olmadan önce faturalandırılmaktan kaçınmak için **Canlı Başlat etkinliği**için **Hayır** seçeneğinin seçildiğinden emin olun. (Faturalandırma, canlı etkinlik başlatıldığında başlar.)

   ![Canlı etkinlik seçeneğini başlatın](media/live-events-wirecast-quickstart/start-live-event-no.png)
1. Ayarları gözden geçirmek için **Gözden Geçir + oluştur** düğmesini seçin.
1. Canlı etkinliği oluşturmak için **Oluştur** düğmesini seçin. Daha sonra canlı etkinlik listesine döndürülür.
1. Oluşturduğunuz canlı etkinliğin bağlantısını seçin. Etkinliğinizin durdurulduğuna dikkat edin.
1. Bu sayfayı tarayıcınızda açık tutun. Daha sonra geri döneriz.

## <a name="set-up-a-live-stream-by-using-wirecast-studio"></a>Wirecast Studio'u kullanarak canlı yayın ayarlama

1. Wirecast uygulamasında, ana menüden **Boş Belge Oluştur'u** ve ardından **Devam et'i**seçin.

   ![Wirecast başlangıç ekranı](media/live-events-wirecast-quickstart/open-empty-document.png)
1. **Wirecast katmanları** alanında ilk katman üzerinde hover.  Görünen **Ekle** simgesini seçin ve akış yapmak istediğiniz video girişini seçin.

   ![Wirecast simge ekle](media/live-events-wirecast-quickstart/add-icon.png)

   **Ana Katman 1** iletişim kutusu açılır.
1. Menüden **Video Yaslave** ardından kullanmak istediğiniz kamerayı seçin.

   ![Video çekimi için önizleme alanı](media/live-events-wirecast-quickstart/video-shot-selection.png)

   Kameranın görünümü önizleme alanında görünür.
1. **Wirecast katmanları** alanında ikinci katmanın üzerine titreyin. Görünen **Ekle** simgesini seçin ve akış yapmak istediğiniz ses girişini seçin. **Ana Katman 2** iletişim kutusu açılır.
1. Menüden **Ses yakalama'yı** seçin ve ardından kullanmak istediğiniz ses girişini seçin.

   ![Ses yakalama girişleri](media/live-events-wirecast-quickstart/audio-shot-select.png)
1. Ana menüden **Çıktı ayarlarını**seçin. **Çıktı Hedefi Seç** iletişim kutusu görüntülenir.
1. **Hedef** açılır listesinden **Azure Medya Hizmetleri'ni** seçin. Azure Medya Hizmetleri'nin çıktı ayarı, çıktı ayarlarının *çoğunu* otomatik olarak doldurur.

   ![Wirecast çıkış ayarları ekranı](media/live-events-wirecast-quickstart/azure-media-services.png)


Bir sonraki yordamda, çıktı ayarlarına girmek için giriş URL'sini kopyalamak için tarayıcınızdaki Azure Medya Hizmetleri'ne geri dönersiniz:

1. Portalın Azure Medya Hizmetleri sayfasında canlı akış olayını başlatmak için **Başlat'ı** seçin. (Faturalandırma şimdi başlar.)

   ![Başlat simgesi](media/live-events-wirecast-quickstart/start.png)
2. **Güvenli/Güvenli Olmayan** geçişini **güvenli değil**olarak ayarlayın. Bu adım, protokolü RTMPS yerine RTMP olarak ayarlar.
3. Giriş **URL** kutusunda, URL'yi panonuza kopyalayın.

   ![Giriş URL'si](media/live-events-wirecast-quickstart/input-url.png)
4. Wirecast uygulamasına geçin ve **Giriş URL'sini** çıkış ayarlarındaki **Adres** kutusuna yapıştırın.

   ![Wirecast giriş URL'si](media/live-events-wirecast-quickstart/input-url-wirecast.png)
5. **Tamam'ı**seçin.

## <a name="set-up-outputs"></a>Çıktıları ayarlama

Bu bölüm çıktılarınızı ayarlar ve canlı akışınızın kaydını kaydetmenizi sağlar.  

> [!NOTE]
> Bu çıktıyı akışla aktarabilmek için akış bitiş noktasının çalışıyor olması gerekir. Daha sonra varsayılan akış uç noktası bölümünü [çalıştır'a](#run-the-default-streaming-endpoint) bakın.

1. **Çıktılar** video görüntüleyicisinin altındaki **çıktı oluştur** bağlantısını seçin.
1. İsterseniz, daha sonra bulmak kolay böylece daha kullanıcı dostu bir şey **için Ad** kutusunda çıktı nın adını edin.
   
   ![Çıktı adı kutusu](media/live-events-wirecast-quickstart/output-name.png)
1. Şimdilik kutuların geri kalanını rahat bırak.
1. Akış bulucu eklemek için **İleri'yi** seçin.
1. İsterseni bulucunun adını daha kullanıcı dostu bir şeyle değiştirin.
   
   ![Konumlayıcı ad kutusu](media/live-events-wirecast-quickstart/live-event-locator.png)
1. Şimdilik bu ekranda her şeyi yalnız bırakın.
1. **Oluştur'u**seçin.

## <a name="start-the-broadcast"></a>Yayını başlatın

1. Wirecast'te, ana menüden **Çıktı** > **Başlat / Yayını** > **Başlat Azure Medya Hizmetleri: Azure Medya Hizmetleri'ni** seçin.

   ![Yayın menüsü öğelerini başlat](media/live-events-wirecast-quickstart/start-broadcast.png)

   Akış canlı etkinliğe gönderildiğinde, Wirecast'teki **Live** penceresi Azure Medya Hizmetleri'ndeki canlı etkinlik sayfasında video oynatıcıda görünür.

1. Wirecast katmanları için seçtiğiniz video ve sesi yayınlamaya başlamak için önizleme penceresinin altındaki **Git** düğmesini seçin.

   ![Wirecast Go düğmesi](media/live-events-wirecast-quickstart/go-button.png)

   > [!TIP]
   > Bir hata varsa, oyuncunun üstündeki Oyuncuyu Yeniden **Yükle** bağlantısını seçerek oyuncuyu yeniden yüklemeyi deneyin.

## <a name="run-the-default-streaming-endpoint"></a>Varsayılan akış bitiş noktasını çalıştırma

1. Medya Hizmetleri listesinde **Akış uç noktalarını** seçin.

   ![Akış uç noktaları menü öğesi](media/live-events-wirecast-quickstart/streaming-endpoints.png)
1. Varsayılan akış uç noktası durumu durdurulursa, bunu seçin. Bu adım, bu bitiş noktası için sayfaya götürür.
1. **Başlat**'ı seçin.
   
   ![Akış bitiş noktası için başlat düğmesi](media/live-events-wirecast-quickstart/start.png)

## <a name="play-the-output-broadcast-by-using-azure-media-player"></a>Azure Media Player kullanarak çıktı yayınını oynatma

1. Akış URL'sini **Çıktı** video oynatıcısının altında kopyalayın.
1. Bir web tarayıcısında [Azure Media Player demosu](https://ampdemo.azureedge.net/azuremediaplayer.html)açın.
1. Akış URL'sini Azure Media Player'ın **URL** kutusuna yapıştırın.
1. Oyuncuyu **Güncelleştir** düğmesini seçin.
1. Canlı akışınızı görmek için videodaki **Oynat** simgesini seçin.

## <a name="stop-the-broadcast"></a>Yayını durdurun

Yeterli içerik akışı yaptığınızı düşündüğünüzde yayını durdurun.

1. Wirecast'te **Yayın** düğmesini seçin. Bu adım Wirecast'ten yayını durdurur.
1. Portalda **Durdur'u**seçin. Daha sonra, canlı yayının duracağına, ancak çıktının artık isteğe bağlı bir varlık haline geleceğini belirten bir uyarı iletisi alırsınız.
1. Uyarı iletisinde **Durdur'u** seçin. Azure Media Player artık bir hata gösterir, çünkü canlı akış artık kullanılamıyor.

## <a name="play-the-on-demand-output-by-using-azure-media-player"></a>Azure Media Player'ı kullanarak isteğe bağlı çıktıyı oynayın

Oluşturduğunuz çıktı, akış bitiş noktanız çalışmaya devam ettiği sürece isteğe bağlı akış için kullanılabilir.

1. Medya Hizmetleri listesine gidin ve **Varlıklar'ı**seçin.
1. Daha önce oluşturduğunuz olay çıktısını bulun ve varlığa olan bağlantıyı seçin. Varlık çıktısayfası açılır.
1. Varlık için video oynatıcının altındaki akış URL'sini kopyalayın.
1. Tarayıcıda Azure Media Player'a dönün ve akışlı URL'yi URL kutusuna yapıştırın.
1. **Güncelle Player'ı**seçin.
1. İsteğe bağlı varlığı görüntülemek için videodaki **Oynat** simgesini seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

> [!IMPORTANT]
> Hizmetleri durdurun! Bu hızlı başlangıçtaki adımları tamamladıktan sonra, canlı olayı ve akış bitiş noktasını durdurduğunuzdan emin olun, yoksa çalışmaya devam ettikleri süre için faturalandırılırsınız. Canlı etkinliği durdurmak için [yayın prosedürünü durdurun,](#stop-the-broadcast) 2 ve 3.

Akış bitiş noktasını durdurmak için:

1. Medya Hizmetleri listesinden **Akış uç noktalarını**seçin.
2. Daha önce başlattığınız varsayılan akış bitiş noktasını seçin. Bu adım bitiş noktasının sayfasını açar.
3. **Durdur'u**seçin.

> [!TIP]
> Varlıkları bu olaydan uzak tutmak istemiyorsanız, depolama için faturalandırılmamanız için bunları sildiğinizden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Medya Hizmetlerinde canlı etkinlikler ve canlı çıktılar](./live-events-outputs-concept.md)
