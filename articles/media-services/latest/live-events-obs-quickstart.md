---
title: OBS Studio ile Azure Medya Hizmetleri canlı akışı oluşturun
description: Portalı ve OBS Studio'yu kullanarak Azure Medya Hizmetleri canlı akışını nasıl oluşturabileceğinizi öğrenin
services: media-services
ms.service: media-services
ms.topic: quickstart
ms.author: inhenkel
author: IngridAtMicrosoft
ms.date: 04/16/2020
ms.openlocfilehash: 74aa17fb17d682449bd817945c3b8bbf3f95363e
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726616"
---
# <a name="create-an-azure-media-services-live-stream-with-obs"></a>OBS ile Azure Medya Hizmetleri canlı akışı oluşturun

Bu hızlı başlangıç, Azure portalı ve Açık Yayın Stüdyosu 'nu (OBS) kullanarak bir Azure Medya Hizmetleri canlı akışı oluşturmanıza yardımcı olur. Azure aboneliğiniz olduğunu ve bir Medya Hizmetleri hesabı oluşturduğunuzu varsayar.

Bu hızlı başlangıçta, şunları ele alacağız:

- OBS ile şirket içi kodlayıcı ayarlama.
- Canlı yayın ayarlama.
- Canlı akış çıktılarını ayarlama.
- Varsayılan akış bitiş noktası çalıştırın.
- Canlı akışı ve isteğe bağlı çıktıyı görüntülemek için Azure Media Player'ı kullanma.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Web tarayıcınızı açın ve [Microsoft Azure portalına](https://portal.azure.com/)gidin. Portalda oturum açmak için kimlik bilgilerinizi girin. Varsayılan görünüm hizmet panonuzu içerir.

## <a name="set-up-an-on-premises-encoder-by-using-obs"></a>OBS kullanarak şirket içi kodlayıcı yı ayarlama

1. İşletim sisteminiz için OBS'yi [Open Broadcaster Software web sitesinden](https://obsproject.com/)indirin ve kurun.
1. Uygulamayı başlatın ve açık tutun.

## <a name="run-the-default-streaming-endpoint"></a>Varsayılan akış bitiş noktasını çalıştırma

1. Medya Hizmetleri listesinde **Akış uç noktalarını** seçin.

   ![Akış uç noktaları menü öğesi](media/live-events-obs-quickstart/streaming-endpoints.png)
1. Varsayılan akış uç noktası durumu durdurulursa, bunu seçin. Bu adım, bu bitiş noktası için sayfaya götürür.
1. **Başlat**'ı seçin.

   ![Akış bitiş noktası için başlat düğmesi](media/live-events-obs-quickstart/start.png)

## <a name="set-up-an-azure-media-services-live-stream"></a>Azure Medya Hizmetleri canlı yayınını ayarlama

1. Portaldaki Azure Medya Hizmetleri hesabına gidin ve ardından **Medya Hizmetleri** listesinden **Canlı akış'ı** seçin.

   ![Canlı akış bağlantısı](media/live-events-obs-quickstart/select-live-streaming.png)
1. Yeni bir canlı akış olayı oluşturmak için **canlı olay ekle'yi** seçin.

   ![Canlı etkinlik simgesi ekleme](media/live-events-obs-quickstart/add-live-event.png)
1. Canlı etkinlik adı kutusuna *TestLiveEvent*gibi yeni **etkinliğiniz** için bir ad girin.

   ![Canlı etkinlik adı kutusu](media/live-events-obs-quickstart/live-event-name.png)
1. **Açıklama** kutusuna olayın isteğe bağlı açıklamasını girin.
1. **Pass-through 'ı** seçin – bulut kodlama seçeneği yok.

   ![Bulut kodlama seçeneği](media/live-events-obs-quickstart/cloud-encoding.png)
1. **RTMP** seçeneğini seçin.
1. Canlı etkinlik hazır olmadan önce faturalandırılmaktan kaçınmak için **Canlı Başlat etkinliği**için **Hayır** seçeneğinin seçildiğinden emin olun. (Faturalandırma, canlı etkinlik başlatıldığında başlar.)

   ![Canlı etkinlik seçeneğini başlatın](media/live-events-obs-quickstart/start-live-event-no.png)
1. Ayarları gözden geçirmek için **Gözden Geçir + oluştur** düğmesini seçin.
1. Canlı etkinliği oluşturmak için **Oluştur** düğmesini seçin. Daha sonra canlı etkinlik listesine döndürülür.
1. Oluşturduğunuz canlı etkinliğin bağlantısını seçin. Etkinliğinizin durdurulduğuna dikkat edin.
1. Bu sayfayı tarayıcınızda açık tutun. Daha sonra geri döneriz.

## <a name="set-up-a-live-stream-by-using-obs-studio"></a>OBS Studio'u kullanarak canlı akış ayarlama

OBS varsayılan bir sahneyle başlar, ancak giriş leri seçilmez.

   ![OBS varsayılan ekranı](media/live-events-obs-quickstart/live-event-obs-default-screen.png)

### <a name="add-a-video-source"></a>Video kaynağı ekleme

1. **Kaynaklar** panelinden, yeni bir kaynak aygıtı seçmek için **ekle** simgesini tıklatın. **Kaynaklar** menüsü açılacaktır.

1. Kaynak aygıt menüsünden **Video Yakalama Aygıtı'nı** seçin. **Kaynak Oluştur/Seç** menüsü açılır.

   ![Video aygıtı seçili OBS kaynakları menüsü](media/live-events-obs-quickstart/live-event-obs-video-device-menu.png)

1. **Varolan** Radyo Ekle düğmesini seçin ve **ardından Tamam'ı**tıklatın. **Video Aygıtı özellikleri** menüsü açılır.

   ![OBS yeni video kaynak menüsü ile mevcut seçili ekle](media/live-events-obs-quickstart/live-event-obs-new-video-source.png)

1. **Cihaz** açılır listesinden, yayınınız için kullanmak istediğiniz video girişini seçin. Ayarların geri kalanını şimdilik yalnız bırakın ve **Tamam'ı**tıklatın. Giriş kaynağı **Kaynaklar** paneline eklenir ve video giriş görünümü **Önizleme** alanında görünür.

   ![OBS kamera ayarları](media/live-events-obs-quickstart/live-event-surface-camera.png)

### <a name="add-an-audio-source"></a>Ses kaynağı ekleme

1. **Kaynaklar** panelinden, yeni bir kaynak aygıtı seçmek için **ekle** simgesini tıklatın. Kaynak Aygıt menüsü açılır.

1. Kaynak aygıt menüsünden **Ses Girişi Yakalama'yı** seçin. **Kaynak Oluştur/Seç** menüsü açılır.

   ![Ses aygıtı seçili OBS kaynakları menüsü](media/live-events-obs-quickstart/live-event-obs-audio-device-menu.png)

1. **Varolan** Radyo Ekle düğmesini seçin ve **ardından Tamam'ı**tıklatın. **Ses Girişi Yakalama** özellikleri menüsü açılır.

   ![MEVCUT seçili ekle ile OBS ses kaynağı ](media/live-events-obs-quickstart/live-event-obs-new-audio-source.png)

1. **Cihaz** açılır listesinden, yayınınız için kullanmak istediğiniz ses yakalama aygıtını seçin. Ayarların geri kalanını şimdilik rahat bırakın ve Tamam'ı tıklatın. Ses yakalama cihazı ses karıştırıcı paneline eklenir.

   ![OBS ses cihazı seçimi açılır listesi](media/live-events-obs-quickstart/live-event-select-audio-device.png)

### <a name="set-up-streaming-in-obs"></a>OBS'de akış ayarlama

Bir sonraki yordamda, çıktı ayarlarına girmek için giriş URL'sini kopyalamak için tarayıcınızdaki Azure Medya Hizmetleri'ne geri dönersiniz:

1. Portalın Azure Medya Hizmetleri sayfasında canlı akış olayını başlatmak için **Başlat'ı** seçin. (Faturalandırma şimdi başlar.)

   ![Başlat simgesi](media/live-events-obs-quickstart/start.png)
1. **RTMP** geçişini **RTMPS**olarak ayarlayın.
1. Giriş **URL** kutusunda, URL'yi panonuza kopyalayın.

   ![Giriş URL'si](media/live-events-obs-quickstart/input-url.png)

1. OBS uygulamasına geçin.

1. **Denetimler** panelindeki **Ayarlar** düğmesini tıklatın. Ayarlar seçenekleri açılır.

   ![OBS Denetimleri paneli seçili ayarlarla](media/live-events-obs-quickstart/live-event-obs-settings.png)

1. **Ayarlar** menüsünden **Akış'ı** seçin.

1. **Hizmet** açılır listesinden tümünü göster'i seçin ve ardından **Özel...** seçeneğini belirleyin.

1. **Sunucu** alanında, kopyaladığınız RTMPS URL'sini panonuza yapıştırın.

1. **Stream tuşu** alanına bir şey girin.  Ne olduğu önemli değil, ama bir değeri olmalı.

    ![OBS akış ayarları](media/live-events-obs-quickstart/live-event-obs-stream-settings.png)

1. **Ayarlar** menüsünden **Çıktı'yı** seçin.

1. Anahtar kare **aralığı** alanına *2* girin. Bu, parça uzunluğunu 2 saniyeye ayarlar. Daha düşük gecikmeli canlı teslimat için 1 saniyedeğerini kullanın.

1. İstEĞE BAĞLI: İşlem gücü düşük bir bilgisayar **kullanıyorsanız, CPU Kullanım Ön Kümesini** çok *hızlı* olarak ayarlayın. İsteğe bağlı olarak, istenmeyen ağ koşulları varsa kbps'yi daha düşük bir şeye ayarlayabilirsiniz.

   ![OBS çıkış ayarları](media/live-events-obs-quickstart/live-event-obs-advanced-output-settings.png)

1. Ayarların geri kalanını şanssız bırakın ve **Tamam'ı**tıklatın.

### <a name="start-streaming"></a>Akışı başlatma

1. **Denetimler** panelinde, **Akışı Başlat'ı**tıklatın.

    ![OBS başlatma akış düğmesi](media/live-events-obs-quickstart/live-event-obs-start-streaming.png)

2. Tarayıcınızdaki Azure Media Services Live etkinlik ekranına geçin ve **Player'ı Yeniden Yükle** bağlantısını tıklayın. Artık akışınızı Önizleme oynatıcısında görmeniz gerekir.

## <a name="set-up-outputs"></a>Çıktıları ayarlama

Bu bölüm çıktılarınızı ayarlar ve canlı akışınızın kaydını kaydetmenizi sağlar.  

> [!NOTE]
> Bu çıktıyı akışla aktarabilmek için akış bitiş noktasının çalışıyor olması gerekir. Daha sonra varsayılan akış uç noktası bölümünü [çalıştır'a](#run-the-default-streaming-endpoint) bakın.

1. **Çıktılar** video görüntüleyicisinin altındaki **çıktı oluştur** bağlantısını seçin.
1. İsterseniz, daha sonra bulmak kolay böylece daha kullanıcı dostu bir şey **için Ad** kutusunda çıktının adını edin.

   ![Çıktı adı kutusu](media/live-events-wirecast-quickstart/output-name.png)
1. Şimdilik kutuların geri kalanını rahat bırak.
1. Akış bulucu eklemek için **İleri'yi** seçin.
1. İsterseni bulucunun adını daha kullanıcı dostu bir şeyle değiştirin.

   ![Konumlayıcı ad kutusu](media/live-events-wirecast-quickstart/live-event-locator.png)
1. Şimdilik bu ekranda her şeyi yalnız bırakın.
1. **Oluştur**’u seçin.

## <a name="play-the-output-broadcast-by-using-azure-media-player"></a>Azure Media Player kullanarak çıktı yayınını oynatma

1. Akış URL'sini **Çıktı** video oynatıcısının altında kopyalayın.
1. Bir web tarayıcısında [Azure Media Player demosu](https://ampdemo.azureedge.net/azuremediaplayer.html)açın.
1. Akış URL'sini Azure Media Player'ın **URL** kutusuna yapıştırın.
1. Oyuncuyu **Güncelleştir** düğmesini seçin.
1. Canlı akışınızı görmek için videodaki **Oynat** simgesini seçin.

## <a name="stop-the-broadcast"></a>Yayını durdurun

Yeterli içerik akışı yaptığınızı düşündüğünüzde yayını durdurun.

1. Portalda **Durdur'u**seçin.

1. OBS'de **Denetimler** panelindeki **Akışı Durdur** düğmesini seçin. Bu adım OBS'den yayını durdurur.

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
