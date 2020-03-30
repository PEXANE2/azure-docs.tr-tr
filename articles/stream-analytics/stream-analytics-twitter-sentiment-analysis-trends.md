---
title: Azure Stream Analytics ile gerçek zamanlı Twitter duygu analizi
description: Bu makalede, gerçek zamanlı Twitter duyarlılık analizi için Stream Analytics nasıl kullanılacağı açıklanmaktadır. Canlı bir panodaki etkinlik oluşturmadan verilere adım adım rehberlik.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/10/2020
ms.openlocfilehash: 4b265bb574895e4728ad93ee25c9dad0da226ea4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240293"
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Azure Stream Analytics’te gerçek zamanlı Twitter yaklaşım analizi

Bu makale, Azure Etkinlik Hub'larına gerçek zamanlı Twitter etkinlikleri getirerek sosyal medya duyarlılık analizi çözümlerini nasıl oluşturabileceğinizi öğretir. Verileri analiz etmek ve sonuçları daha sonra kullanmak üzere depolamak veya gerçek zamanlı olarak öngörüler sağlamak için bir [Power BI](https://powerbi.com/) panosu oluşturmak için bir Azure Akış Analizi sorgusu yazarsınız.

Sosyal medya analiz araçları, kuruluşların popüler konuları anlamalarına yardımcı olur. Popüler konular sosyal medyada yüksek hacimli mesajlara sahip konular ve tutumlardır. *Fikir madenciliği*olarak da adlandırılan duygu analizi, bir ürün veya fikre karşı tutumları belirlemek için sosyal medya analiz araçlarını kullanır. 

Gerçek zamanlı Twitter eğilim analizi, belirli anahtar kelimeleri (hashtag'ler) dinlemenize ve özet akışının duyarlılık analizini geliştirmenize olanak sağladığından, bir analiz aracının harika bir örneğidir.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Senaryo: Gerçek zamanlı olarak sosyal medya duyarlılık analizi

Bir haber medya web sitesi olan bir şirket hemen okuyucuları ile ilgili site içeriği sunarak rakiplerine karşı bir avantaj elde ilgileniyor. Şirket Twitter verilerinin gerçek zamanlı duygu analizi yaparak okuyucuları niçin ilgili konularda sosyal medya analizi kullanır.

Twitter'da trend konuları gerçek zamanlı olarak belirlemek için, şirketin önemli konular için tweet hacmi ve duyarlılık hakkında gerçek zamanlı analize ihtiyacı vardır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılabilir kılavuzunda, Twitter'a bağlanan ve belirli hashtag'leri olan (ayarlayabileceğiniz) tweet'leri arayan bir istemci uygulaması kullanırsınız. Uygulamayı çalıştırmak ve Azure Akış Analizi'ni kullanarak tweetleri analiz etmek için aşağıdakilere sahip olmalısınız:

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)oluşturun.

* Bir [Twitter](https://twitter.com) hesabı.

* Twitter beslemesini okuyan TwitterClientCore uygulaması. Bu uygulamayı almak [için, TwitterClientCore](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TwitterClientCore)indirin.

* [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x) sürümünü 2.1.0'a yükleyin.

## <a name="create-an-event-hub-for-streaming-input"></a>Akış girişi için bir olay hub'ı oluşturma

Örnek uygulama olayları oluşturur ve bunları bir Azure etkinlik merkezine iter. Azure Etkinlik Hub'ları, Akış Analizi için tercih edilen etkinlik oluşturma yöntemidir. Daha fazla bilgi için [Azure Etkinlik Hub'ları belgelerine](../event-hubs/event-hubs-what-is-event-hubs.md)bakın.

### <a name="create-an-event-hub-namespace-and-event-hub"></a>Olay merkezi ad alanı ve etkinlik merkezi oluşturma
Bu bölümde, bir olay hub ad alanı oluşturmak ve bu ad alanına bir olay hub'ı ekleyin. Olay hub ad alanları mantıksal olarak ilişkili olay veri ağacı örneklerini gruplandırmak için kullanılır. 

1. Azure portalına giriş yapın ve **kaynak oluştur'u**seçin. Sonra. Olay **Hub'larını** arayın ve **Oluştur'u**seçin.

2. Ad **Alanı Oluştur** sayfasında bir ad alanı adı girin. Ad alanı için herhangi bir ad kullanabilirsiniz, ancak ad bir URL için geçerli olmalı ve Azure genelinde benzersiz olmalıdır. 
    
3. Bir fiyatlandırma katmanı ve aboneliği seçin ve bir kaynak grubu oluşturun veya seçin. Ardından, bir konum seçin ve **Oluştur'u**seçin. 
 
4. Ad alanı dağıtımı tamamlandığında, kaynak grubunuza gidin ve Azure kaynakları listenizdeki etkinlik merkezi ad alanını bulun. 

5. Yeni ad alanından ** + &nbsp;Olay Hub'ını**seçin. 

6. Yeni olay merkezi *socialtwitter-eh*adı . Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, bunu not edin, çünkü daha sonra isme ihtiyacınız var. Olay merkezi için başka seçenekler ayarlamanız gerekmez.
 
7. **Oluştur'u**seçin.

### <a name="grant-access-to-the-event-hub"></a>Etkinlik merkezine erişim izni verme

Bir işlem olay hub'ına veri gönderebilmesi için, olay merkezinin erişime izin veren bir ilke ye ihtiyacı vardır. Erişim ilkesi, yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1.  Etkinlik hub'larınızın ad alanının sol tarafındaki gezinti çubuğunda, **Varlıklar** bölümünde bulunan **Olay Hub'larını**seçin. Ardından, yeni oluşturduğunuz etkinlik merkezini seçin.

2.  Sol taraftaki gezinti çubuğunda, **Ayarlar**altında bulunan **Paylaşılan erişim ilkelerini** seçin.

    >[!NOTE]
    >Olay merkezi ad alanı ve olay merkezi için altında paylaşılan erişim ilkeleri seçeneği vardır. Genel etkinlik hub'ı ad alanı yla değil, etkinlik hub'ınız bağlamında çalıştığınızdan emin olun.

3.  Erişim ilkesi sayfasından **+ Ekle'yi**seçin. Ardından **İlke adı** için *socialtwitter erişimi* girin ve **Yönet** onay kutusunu işaretleyin.
 
4.  **Oluştur'u**seçin.

5.  İlke dağıtıldıktan sonra, paylaşılan erişim ilkeleri listesinden ilkeyi seçin.

6.  **Bağlantı dizesi etiketli** kutuyu birincil tuşla bulun ve bağlantı dizesinin yanındaki kopya düğmesini seçin.
 
7.  Bağlantı dizesini bir metin düzenleyicisine yapıştırın. Bazı küçük değişkentler yaptıktan sonra bir sonraki bölüm için bu bağlantı dizesi

   Bağlantı dizesi aşağıdaki gibi görünür:
   
   ```
   Endpoint=sb://EVENTHUBS-NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh
   ```

   Bağlantı dizesi, yarı iki nokta üst üste ayrılmış `Endpoint`birden `SharedAccessKeyName` `SharedAccessKey`çok `EntityPath`anahtar değeri çifti içerdiğine dikkat edin: , , ve .  

   > [!NOTE]
   > Güvenlik için, örnekteki bağlantı dizesinin bölümleri kaldırıldı.

## <a name="configure-and-start-the-twitter-client-application"></a>Twitter istemcisi uygulamasını yapılandırma ve başlatma

İstemci uygulaması doğrudan Twitter'dan tweet olayları alır. Bunu yapmak için, Twitter Streaming API'leri aramak için izin gerekir. Bu izni yapılandırmak için, Twitter'da benzersiz kimlik bilgileri (OAuth belirteci gibi) oluşturan bir uygulama oluşturursunuz. Ardından, istemci uygulamasını API çağrıları yaptığında bu kimlik bilgilerini kullanacak şekilde yapılandırabilirsiniz. 

### <a name="create-a-twitter-application"></a>Twitter uygulaması oluşturma
Zaten bu nasıl yapılabilir kılavuzu için kullanabileceğiniz bir Twitter uygulaması yoksa, bir oluşturabilirsiniz. Zaten bir Twitter hesabınız olmalı.

> [!NOTE]
> Bir uygulama oluşturmak ve anahtarları, sırları almak ve belirteç almak için Twitter tam süreci değişebilir. Bu talimatlar Twitter sitesinde gördüklerinizle eşleşmiyorsa, Twitter geliştirici belgelerine bakın.

1. Bir web [tarayıcısından, Geliştiriciler için Twitter'a](https://developer.twitter.com/en/apps)gidin, bir geliştirici hesabı oluşturun ve uygulama **oluştur'u**seçin. Bir Twitter geliştirici hesabına başvurmanız gerektiğini belirten bir ileti görebilirsiniz. Bunu yapmak için çekinmeyin ve başvurunuz onaylandıktan sonra, bir onay e-postası görmelisiniz. Geliştirici hesabının onaylanması birkaç gün sürebilir.

   ![Twitter uygulama detayları](./media/stream-analytics-twitter-sentiment-analysis-trends/provide-twitter-app-details.png "Twitter uygulama detayları")

2. **Uygulama oluşturun** sayfasında yeni uygulamaya ilişkin ayrıntıları sağlayın ve **Kendi Twitter uygulamanızı oluşturun**’u seçin.

   ![Twitter uygulama detayları](./media/stream-analytics-twitter-sentiment-analysis-trends/provide-twitter-app-details-create.png "Twitter uygulama detayları")

3. Uygulama sayfasında, **Anahtarlar ve Belirteçler** sekmesini seçin ve **Tüketici API Anahtarı** ve Tüketici **API Gizli Anahtarı**değerlerini kopyalayın. Ayrıca, erişim belirteçlerini oluşturmak için **Access Token ve Access Token Secret** altında **Oluştur'u** seçin. **Erişim Belirteci** ve**Erişim Belirteci Parolası** değerlerini kopyalayın.

   Twitter uygulaması için aldığınız değerleri kaydedin. Değerlere daha sonra ihtiyacın olacak.

> [!NOTE]
> Twitter uygulamasının anahtarları ve sırları Twitter hesabınıza erişim sağlar. Bu bilgileri, Twitter şifrenizi yaptığınız gibi hassas olarak ele alın. Örneğin, bu bilgileri başkalarına verdiğiniz bir uygulamaya gömmeyin. 

### <a name="configure-the-client-application"></a>İstemci uygulamasını yapılandırma

Belirli bir konu kümesi yle ilgili tweet etkinlikleri toplamak için [Twitter'ın Akış API'lerini](https://dev.twitter.com/streaming/overview) kullanarak Twitter verilerine bağlanan bir istemci uygulaması oluşturduk.

Uygulama çalışmadan önce, Twitter anahtarları ve olay hub bağlantı dizesi gibi sizden belirli bilgiler gerektirir.

1. Ön koşullarda listelenen [TwitterClientCore](https://github.com/Azure/azure-stream-analytics/tree/master/DataGenerators/TwitterClientCore) uygulamasını indirdiğinizden emin olun.

2. *App.config* dosyasını açmak için bir metin düzenleyicisi kullanın. `<appSettings>` Öğede aşağıdaki değişiklikleri yapın:

   * Twitter `oauth_consumer_key` Tüketici Anahtarı (API anahtarı) olarak ayarlayın. 
   * Twitter `oauth_consumer_secret` Consumer Secret (API gizli anahtar) ayarlayın.
   * Twitter `oauth_token` Erişim jetonuna ayarlayın.
   * Twitter `oauth_token_secret` Access belirteci sırrına ayarlayın.
   * Bağlantı `EventHubNameConnectionString` dizesi ayarlayın.
   * Olay `EventHubName` hub adı (varlık yolunun değeri) ayarlayın.

3. Komut satırını açın ve TwitterClientCore uygulamanızın bulunduğu dizine gidin. Projeyi `dotnet build` oluşturmak için komutu kullanın. Ardından uygulamayı `dotnet run` çalıştırmak için komutu kullanın. Uygulama Tweetleri Etkinlik Hub'ınıza gönderir.

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

Artık tweet etkinlikleri Twitter'dan gerçek zamanlı olarak yayın yaptığına göre, bu olayları gerçek zamanlı olarak analiz etmek için bir Stream Analytics işi ayarlayabilirsiniz.

1. Azure portalında kaynak grubunuza gidin ve **+ Ekle'yi**seçin. Ardından **Stream Analytics işini** arayın ve **Oluştur'u**seçin.

2. İşi `socialtwitter-sa-job` adlandırın ve bir abonelik, kaynak grubu ve konum belirtin.

    En iyi performans için işi ve etkinlik merkezini aynı bölgeye yerleştirmek ve böylece bölgeler arasında veri aktarmak için ödeme yapmazsanız iyi bir fikirdir.

3. **Oluştur'u**seçin. Ardından dağıtım tamamlandığında işinize gidin.

## <a name="specify-the-job-input"></a>İş girişini belirtin

1. Akış Analizi işinizde, **İş Topolojisi**altındaki sol menüden **Girişler'i** seçin.

2. Akış girişi > Olay ** + &nbsp;Hub'ı ekle'yi**seçin.**Event Hub** **Yeni giriş** formunu aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı| *TwitterStream* | Giriş için bir takma ad girin. |
   |Abonelik  | \<Aboneliğiniz\> |  Kullanmak istediğiniz Azure aboneliğini seçin. |
   |Olay hub'ı ad alanı | *asa-twitter-eventhub* |
   |Olay Hub'ı adı | *socialtwitter-eh* | *Varolan Kullan'ı*seçin. Ardından oluşturduğunuz Etkinlik Hub'ını seçin.|
   |Olay sıkıştırma türü| Gzip | Veri sıkıştırma türü.|

   Kalan varsayılan değerleri bırakın ve **Kaydet'i**seçin.

## <a name="specify-the-job-query"></a>İş sorgusunu belirtin

Akış Analizi, dönüşümleri açıklayan basit, bildirimsel bir sorgu modelini destekler. Dil hakkında daha fazla bilgi edinmek için [Azure Akışı Analitik Sorgu Dili Başvurusu'na](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)bakın. Bu nasıl yap'ı kılavuzu, Twitter verileri üzerinden çeşitli sorgular yazmanıza ve test etmenize yardımcı olur.

Konular arasında söz sayısını karşılaştırmak için, her beş saniyede bir konuya göre söz sayısını almak için bir [Yuvarlanma penceresi](https://docs.microsoft.com/stream-analytics-query/tumbling-window-azure-stream-analytics) kullanabilirsiniz.

1. İşinize **Genel Bakış'ta**Sorgu kutusunun sağ üst **kısmındasorguyu edit'i** seçin. Azure, iş için yapılandırılan girdi ve çıktıları listeler ve çıktıya gönderilirken giriş akışını dönüştürmek için bir sorgu oluşturmanıza olanak tanır.

2. Sorgu düzenleyicisindeki sorguyu aşağıdakiyle değiştirin:

   ```sql
   SELECT *
   FROM TwitterStream
   ```

3. İletilerden gelen olay verileri, sorgunuzun altındaki **Giriş önizleme** penceresinde görünmelidir. **Görünümün** **JSON**olarak ayarlandığından emin olun. Herhangi bir veri görmüyorsanız, veri üretecinizin olay merkezinize olay merkezine olay gönderdiğinden ve giriş için sıkıştırma türü olarak **GZip'i** seçtiğinizden emin olun.

4. **Test sorgusunu** seçin ve sorgunuzun altındaki **Test sonuçları** penceresinde sonuçları fark edin.

5. Kod düzenleyicisindeki sorguyu aşağıdakiyle değiştirin ve **Test sorgusunu**seçin:

   ```sql
   SELECT System.Timestamp as Time, text
   FROM TwitterStream
   WHERE text LIKE '%Azure%'
   ```

6. Bu sorgu, *Azure*anahtar kelimesini içeren tüm tweetleri döndürür.

## <a name="create-an-output-sink"></a>Çıktı lavabosu oluşturma

Şimdi bir olay akışı, olayları yutmak için bir olay hub girişi ve akış üzerinde bir dönüşüm gerçekleştirmek için bir sorgu tanımladınız. Son adım, iş için bir çıktı lavabo tanımlamaktır.  

Bu nasıl yapılsa kılavuzunda, iş sorgusundan Azure Blob depolamasına toplu tweet olaylarını yazarsınız.  Ayrıca, uygulama gereksinimlerinize bağlı olarak sonuçlarınızı Azure SQL Veritabanı, Azure Tablo depolama alanı, Etkinlik Hub'ları veya Power BI'ye de taşıyabilirsiniz.

## <a name="specify-the-job-output"></a>İş çıktısını belirtin

1. Sol daki gezinme menüsündeki **İş Topolojisi** bölümünün altında **Çıktılar'ı**seçin. 

2. **Çıktılar** sayfasında, ** + &nbsp;Ekle** ve **Blob depolama/Veri Gölü Depolama Gen2'yi**tıklatın:

   * **Çıktı diğer adı**: `TwitterStream-Output`Adı kullanın. 
   * **Alma seçenekleri**: **Aboneliklerinizden depolama yı seç'i**seçin.
   * **Depolama hesabı**. Depolama hesabınızı seçin.
   * **Konteyner**. **Yeni Oluştur'u** seçin ve girin. `socialtwitter`
   
4. **Kaydet'i**seçin.   

## <a name="start-the-job"></a>İşi başlatma

İş girişi, sorgu ve çıktı belirtilir. Stream Analytics işine başlamaya hazırsınız.

1. TwitterClientCore uygulamasının çalışmadığından emin olun. 

2. İşe genel bakış olarak **Başlat'ı**seçin.

3. İş çıkışı **başlangıç saati**için **Başlat iş** sayfasında, **Şimdi'yi** seçin ve ardından **Başlat'ı**seçin.

## <a name="get-support"></a>Destek alın
Daha fazla yardım için [Azure Akışı Analizi forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
