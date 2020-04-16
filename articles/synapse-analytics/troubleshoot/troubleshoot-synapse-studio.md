---
title: Azure Synapse Studio sorun giderme (önizleme)
description: Sorun Giderme Azure Synapse Studio
author: julieMSFT
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: ede38317762e5aac5562c9bb3494713c3edacc69
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431325"
---
# <a name="azure-synapse-studio-preview-troubleshooting"></a>Azure Synapse Studio (önizleme) sorun giderme

Bu sorun giderme kılavuzu, ağ bağlantısı sorunları hakkında destek bileti açarken hangi bilgilerin sağlandığı na ilişkin talimat sağlar. Doğru bilgilerle, sorunu daha hızlı çözebiliriz.

## <a name="sql-on-demand-preview-service-connectivity-issue"></a>SQL isteğe bağlı (önizleme) hizmet bağlantısı sorunu

### <a name="symptom-1"></a>Belirti 1

"Sql on-demand" seçeneği "Connect to" açılır açılır durumda gri renkte.

![belirti1](media/troubleshooting-synapse-studio/symptom1v2.png)

### <a name="symptom-2"></a>Belirti 2

Sorguyu "SQL on-demand" ile çalıştırmak size "Sunucuya bağlantı kurmakta başarısız oldu" hata iletisi verir.

![belirti2](media/troubleshooting-synapse-studio/symptom2.png)

## <a name="troubleshooting-steps"></a>Sorun giderme adımları

> [!NOTE] 
>    Aşağıdaki sorun giderme adımları Krom Kenar ve Chrome içindir. Aynı sorun giderme adımlarına sahip diğer tarayıcıları (FireFox gibi) kullanabilirsiniz, ancak "Geliştirici Aracı" penceresi bu TSG'deki ekran ekranlarından farklı bir düzene sahip olabilir. Mümkünse, belirli bir durumda yanlış bilgi gösterebileceğinden, sorun giderme için klasik Edge'i KULLANMAYIN.

"Tanılama Bilgileri" panelini açın, "Tanılamayı İndir" düğmesini seçin. İndirilen bilgileri hata bildirimi için saklayın. Bunun yerine, "Oturum Kimliği"ni kopyalayabilir ve destek biletini açarken ekleyebilirsiniz.

![tanı-bilgi](media/troubleshooting-synapse-studio/diagnostic-info-download.png)

Sorun gidermeye başlamak için Azure Synapse Studio'da gerçekleştirdiğiniz işlemi yeniden deneyin.

- Belirti 1 için, "SQL komut dosyası" sekmesinde "Veritabanını kullan" açılır tabanının sağındaki "Yenile" düğmesini seçin ve "SQL on-demand"i görüp göremeyeceğinizi kontrol edin.
- Belirti 2 için, başarılı bir şekilde yürütülüp yürütülmeyeceğini görmek için sorguyu yeniden çalıştırmayı deneyin.

Sorun hala varsa, "Geliştirici Araçları" (DevTools) açmak için tarayıcınızda F12 tuşuna basın.

"Geliştirici Araçları" penceresinde "Ağ" paneline geçin. Gerekirse "Ağ" panelindeki araç çubuğundaki "Temizle" düğmesini seçin.
"Ağ" panelindeki "Önbelleği devre dışı" denetiminin denetlendiğine emin olun.

Azure Synapse Studio'da gerçekleştirdiğiniz işlemi yeniden deneyin. "Geliştirici Araçları"nda "Ağ" listesinde gösterilen yeni öğeler görebilirsiniz. Destek biletini sağlamak için geçerli sistem zamanınızı not edin.

![ağ paneli](media/troubleshooting-synapse-studio/network-panel.png)

Url sütunu aşağıdaki desenle eşleşen öğeyi bulun:

`https://[*A*]-ondemand.database.windows.net:1443/databases/[*B*]/query?api-version=2018-08-01-preview&application=ArcadiaSqlOnDemandExplorer`

*[A]* çalışma alanı adınız, "-ondemand" "-sqlod" ve *[B]* veritabanı adı olmalıdır, örneğin "master" olmalıdır. Aynı URL değerine sahip ancak farklı yöntem değerlerine sahip en fazla iki öğe olmalıdır; SEÇENEKLER ve POST. Bu iki öğenin durum sütunu altında "x" tek basamaklı olabileceği "200" veya "20x" olup olmadığını kontrol edin.

Bunlardan biri "20x" dışında bir şey varsa ve:

- durum "(başarısız)" ile başlar, "Durum" sütununu genişletin veya metnin tamamını görmek için işaretçinizin üzerinde durum metninin üzerine gezin. Destek biletini açarken metni ve/veya ekran görüntüsünü ekleyin.

    ![durum-metin](media/troubleshooting-synapse-studio/status-text.png)

    - ERR_NAME_NOT_RESOLVED görürseniz ve çalışma alanınızı 10 dakika içinde oluşturduysanız, 10 dakika bekleyin ve sorunun hala var olup olmadığını görmek için yeniden deneyin.
    - ERR_INTERNET_DISCONNECTED veya ERR_NETWORK_CHANGED görürseniz, pc ağ bağlantınızın sorunlar yaşadığını gösterebilir. Ağ bağlantınızı kontrol edin ve işlemi yeniden deneyin.
    - "SSL" içeren ERR_CONNECTION_RESET, ERR_SSL_PROTOCOL_ERROR veya başka hata kodları görürseniz, yerel SSL yapılandırmanızın sorun yaşadığını veya ağ yöneticinizin isteğe bağlı SQL sunucusuna erişimi engellediğini gösterebilir. Bir destek bileti açın ve açıklamaya hata kodunu takın.
    - ERR_NETWORK_ACCESS_DENIED görüyorsanız, yerel güvenlik duvarı politikanızın *.database.windows.net etki alanına veya uzak bağlantı noktasına erişimi engelleyip engellemediğini yöneticiye kontrol etmeniz gerekebilir.
    - İsteğe bağlı olarak, bilgisayarınızdaki bir ağ yapılandırma sorununu ekarte etmek için aynı işlemi hemen farklı bir makine de ve/veya ağ ortamında deneyin.

- durum "40x", "50x" veya diğer sayılar, ayrıntıları görmek için öğe(ler) seçin. Sağdaki öğe ayrıntılarını görmelisiniz. "Yanıt Üstbilgi" bölümünü bulun; ardından "access-control-allow-allow-origin" adlı bir öğenin var olup olmadığını denetleyin. Bu nedenle, aşağıdaki değerlerden birine sahip olup olmadığını denetleyin:

    - `*`(tek yıldız işareti)
    - https://web.azuresynapse.net/(veya tarayıcı adres çubuğunuzdaki metnin başladığı diğer değer)

Yanıt üstbilgisi yukarıdaki değerlerden birini içeriyorsa, hata bilgilerini zaten toplamamız gerektiği anlamına gelir. Gerekirse bir destek bileti açabilir ve isteğe bağlı olarak öğe ayrıntılarının ekran görüntüsünü ekleyebilirsiniz.

Üstbilgigöremiyorsanız veya üstbilgide yukarıda listelenen değerlerden biri yoksa, bileti açtığınızda öğe ayrıntılarının ekran görüntüsünü takın.

![öğe ayrıntıları](media/troubleshooting-synapse-studio/item-details.png)

Yukarıdaki adımlar sorununuzu çözmezse, bir destek bileti açmanız gerekebilir. Destek biletinizi gönderirken, bu kılavuzun başında indirilen "Oturum Kimliği" veya "Tanılama Bilgileri"ni ekleyin.

Sorunu rapor ederken, isteğe bağlı olarak "Geliştirici Araçları"ndaki "Konsol" sekmenizin ekran görüntüsünü alabilir ve destek biletine takabilirsiniz. İçeriği kaydırın ve iletinin tamamını yakalamak için gerekirse birden fazla ekran görüntüsü alın.

![geliştirici-araç-konsol](media/troubleshooting-synapse-studio/developer-tool-console.png)

Ekran görüntüleri ekiyorsanız, ekran görüntülerini ne zaman aldığınıza (veya tahmini bir zaman aralığına) sahip olun. Bu sorun içine bakarken bize yardımcı olacaktır.

Bazı tarayıcılar "Konsol" sekmesinde zaman damgalarını göstermeyi destekler. Krom Kenar/Chrome için"Geliştirici Araçları"ndaki "Ayarlar" iletişim kutusunu açın ve "Tercihler" sekmesinde "Zaman damgalarını göster"i işaretleyin.

![geliştirici-araç-konsol-ayarları](media/troubleshooting-synapse-studio/developer-tool-console-settings.png)

![show-time-stamp](media/troubleshooting-synapse-studio/show-time-stamp.png)

## <a name="next-steps"></a>Sonraki adımlar
Önceki adımlar sorunu gidermek için yardımcı olursa [bir destek bileti oluştur](../../sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
