---
title: Azure Monitör'de özel günlükleri toplama | Microsoft Dokümanlar
description: Azure Monitor, hem Windows hem de Linux bilgisayarlardaki metin dosyalarından olayları toplayabilir.  Bu makalede, yeni bir özel günlük ve Azure Monitor'da oluşturdukları kayıtların ayrıntıları nasıl tanımlanabilecekleri açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/26/2019
ms.openlocfilehash: 1e889aaef7cd01cd743e8063a8a1dd5138ba9d0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670602"
---
# <a name="custom-logs-in-azure-monitor"></a>Azure İzleyici'deki özel günlükler

Azure Monitor'daki Özel Günlükler veri kaynağı, hem Windows hem de Linux bilgisayarlardaki metin dosyalarından olay toplamanıza olanak tanır. Birçok uygulama, Windows Event günlüğü veya Syslog gibi standart günlük hizmetleri yerine metin dosyalarına bilgi günlüğe kaydeder. Once collected, you can either parse the data into individual fields in your queries or extract the data during collection to individual fields.

![Özel günlük koleksiyonu](media/data-sources-custom-logs/overview.png)

Toplanacak günlük dosyaları aşağıdaki ölçütlere uygun olmalıdır.

- Günlük satır başına tek bir girişe sahip olmalı veya her girişin başında aşağıdaki biçimlerden biriyle eşleşen bir zaman damgası kullanmalıdır.

    YYYY-MM-DD HH:MM:SS<br>M/D/YYYY HH:MM:SS AM/PM<br>Pzt DD, YYYY HH:MM:SS<br />yyMdd HH:mm:ss<br />ddMMyy HH:mm:ss<br />MMM d hh:mm:ss<br />dd/MMM/yyyy:HH:mm:ss zzz<br />yyyy-MM-ddTHH:mm:ssK

- Günlük dosyası, dosyanın yeni girişlerle üzerine yazıldığı dairesel günlüğe veya günlük döndürmeye izin vermemelidir.
- Günlük dosyası ASCII veya UTF-8 kodlama kullanmalıdır.  UTF-16 gibi diğer biçimler desteklenmez.

>[!NOTE]
> Günlük dosyasında yinelenen girişler varsa, Azure Monitor bunları toplar. Ancak, filtre sonuçlarının sonuç sayısından daha fazla olay gösterdiği durumlarda sorgu sonuçları tutarsız olacaktır. Bu davranışı oluşturan uygulama neden olup olmadığını belirlemek için günlük doğrulamak ve özel günlük koleksiyonu tanımı oluşturmadan önce mümkünse adresi önemlidir.  
>

>[!NOTE]
> Log Analytics çalışma alanı aşağıdaki sınırları destekler:
> 
> * Yalnızca 500 özel günlük oluşturulabilir.
> * Tablo yalnızca en fazla 500 sütunu destekler. 
> * Sütun adı için en fazla karakter sayısı 500'dür. 
>

>[!IMPORTANT]
>Özel günlük toplama, günlük dosyasını yazan uygulamanın günlük içeriğini düzenli aralıklarla diske çekmesini gerektirir. Bunun nedeni, özel günlük koleksiyonunun izlenen günlük dosyası için dosya sistemi değişikliği bildirimlerine dayanmasıdır.

## <a name="defining-a-custom-log"></a>Özel bir günlük tanımlama
Özel bir günlük dosyasını tanımlamak için aşağıdaki yordamı kullanın.  Özel bir günlük ekleme örneğinin gözden geçirimi için bu makalenin sonuna gidin.

### <a name="step-1-open-the-custom-log-wizard"></a>1. Adım. Özel Günlük Sihirbazı'nı aç
Özel Günlük Sihirbazı Azure portalında çalışır ve toplamak için yeni bir özel günlük tanımlamanıza olanak tanır.

1. Azure portalında, Çalışma alanınız > **Gelişmiş Ayarlar'>** **Günlük Analizi çalışma alanlarını** seçin.
2. **Data** > **Custom günlüklerine**tıklayın.
3. Varsayılan olarak, tüm yapılandırma değişiklikleri otomatik olarak tüm aracılara itilir. Linux aracıları için, bir yapılandırma dosyası Fluentd veri toplayıcısına gönderilir.
4. Özel Günlük Sihirbazı'nı açmak için **Ekle+'yı** tıklatın.

### <a name="step-2-upload-and-parse-a-sample-log"></a>2. Adım Örnek bir günlüğü yükleme ve ayrıştama
Özel günlüğün bir örneğini yükleyerek başlarsınız.  Sihirbaz, doğrulamanız için bu dosyadaki girişleri ayrıştıracak ve görüntüler.  Azure Monitor, her kaydı tanımlamak için belirttiğiniz sınır dışı cihazını kullanır.

**Yeni Hat** varsayılan sınır dışı layıcıdır ve satır başına tek bir girişi olan günlük dosyaları için kullanılır.  Satır kullanılabilir biçimlerden birinde bir tarih ve saatle başlarsa, birden fazla satıra yayılan girişleri destekleyen bir **Zaman Damgası** sınırlayıcı belirtebilirsiniz.

Bir zaman damgası sınırlayıcı kullanılırsa, Azure Monitor'da depolanan her kaydın TimeGenerated özelliği, günlük dosyasında bu giriş için belirtilen tarih/saatle doldurulur.  Yeni bir satır sınırlayıcı kullanılırsa, TimeGenerated, Azure Monitor'un girişi topladığı tarih ve saatle doldurulur.

1. **Gözat'ı** tıklatın ve örnek bir dosyaya göz atın.  Bu düğmenin bazı tarayıcılarda **Dosya Seç** olarak etiketlenebileceğini unutmayın.
2. **İleri**'ye tıklayın.
3. Özel Günlük Sihirbazı dosyayı yükler ve tanımladığı kayıtları listeler.
4. Yeni bir kaydı tanımlamak için kullanılan sınır dışılayıcıyı değiştirin ve günlük dosyanızdaki kayıtları en iyi tanımlayan delimiter'ı seçin.
5. **İleri**'ye tıklayın.

### <a name="step-3-add-log-collection-paths"></a>3. Adım Günlük toplama yolları ekleme
Aracıda özel günlüğü bulabileceği bir veya daha fazla yol tanımlamanız gerekir.  Günlük dosyası için belirli bir yol ve ad sağlayabilir veya ad için joker karakteriçeren bir yol belirtebilirsiniz. Bu, her gün yeni bir dosya oluşturan veya bir dosya belirli bir büyüklüğe ulaştığında uygulamaları destekler. Ayrıca, tek bir günlük dosyası için birden çok yol sağlayabilirsiniz.

Örneğin, bir uygulama log20100316.txt olarak adına dahil tarih ile her gün bir günlük dosyası oluşturabilirsiniz. Böyle bir günlük için bir *desen,\** uygulamanın adlandırma düzenini izleyen herhangi bir günlük dosyasına uygulanacak log .txt olabilir.

Aşağıdaki tablo, farklı günlük dosyaları belirtmek için geçerli desen örnekleri sağlar.

| Açıklama | Yol |
|:--- |:--- |
| *C:\Logs'daki* tüm dosyalar Windows aracısında .txt uzantısı ile |C:\Günlükler\\\*.txt |
| *C:\Logs'daki* tüm dosyalar, günlük le başlayan bir ad ve Windows aracısında .txt uzantısı ile |C:\Logs\log\*.txt |
| Linux aracısında .txt uzantısı ile */var/log/audit'deki* tüm dosyalar |/var/log/audit/*.txt |
| Linux aracısında log ve .txt uzantısı ile başlayan bir adla */var/log/audit'deki* tüm dosyalar |/var/log/audit/log\*.txt |

1. Hangi yol biçimini eklediğinizi belirtmek için Windows veya Linux'u seçin.
2. Yolu yazın ve düğmeyi **+** tıklatın.
3. Ek yollar için işlemi yineleyin.

### <a name="step-4-provide-a-name-and-description-for-the-log"></a>4. Adım. Günlük için bir ad ve açıklama sağlama
Belirttiğiniz ad, yukarıda açıklandığı gibi günlük türü için kullanılacaktır.  Her zaman özel bir günlük olarak ayırt etmek için _CL ile sona erer.

1. Günlük için bir ad yazın.  CL soneki otomatik olarak sağlanır. ** \_**
2. İsteğe bağlı **Açıklama**ekle.
3. Özel günlük tanımını kaydetmek için **İleri'yi** tıklatın.

### <a name="step-5-validate-that-the-custom-logs-are-being-collected"></a>5. Adım. Özel günlüklerin toplandığını doğrulayın
Yeni bir özel günlükten gelen ilk verilerin Azure Monitor'da görünmesi bir saat kadar sürebilir.  Özel günlüğü tanımladığınız noktadan belirttiğiniz yolda bulunan günlüklerden girişleri toplamaya başlar.  Özel günlük oluşturma sırasında yüklediğiniz girişleri tutmaz, ancak bulunduğu günlük dosyalarında zaten varolan girişleri toplar.

Azure Monitor özel günlükten toplamaya başladığında, kayıtları bir günlük sorgusuyla kullanılabilir.  Özel günlüğe vermiş olduğunuz adı, sorgunuzdaki **Tür** olarak kullanın.

> [!NOTE]
> RawData özelliği sorguda eksikse, tarayıcınızı kapatıp yeniden açmanız gerekebilir.

### <a name="step-6-parse-the-custom-log-entries"></a>6. Adım. Özel günlük girişlerini ayrıştın
Tüm günlük girişi **RawData**adı verilen tek bir özellikte depolanır.  Büyük olasılıkla her kayıt için ayrı özellikler içine her girişbilgi farklı parçalarını ayırmak isteyeceksiniz. **RawData'yı** birden çok özelliğe ayrıştma seçenekleri için [Azure Monitor'daki Ayrışt metin verilerine](../log-query/parse-text.md) bakın.

## <a name="removing-a-custom-log"></a>Özel bir günlüğü kaldırma
Daha önce tanımladığınız özel bir günlüğü kaldırmak için Azure portalında aşağıdaki işlemi kullanın.

1. Çalışma alanınız için **Gelişmiş Ayarlar'daki** **Veri** menüsünden, tüm özel günlüklerinizi listelemek için **Özel Günlükler'i** seçin.
2. Kaldırmak için özel günlüğün yanındaki **Kaldır'ı** tıklatın.

## <a name="data-collection"></a>Veri toplama
Azure Monitor, her özel günlükten yaklaşık olarak her 5 dakikada bir yeni girişler toplar.  Aracı, topladığı her günlük dosyasına yerini kaydeder.  Aracı bir süre çevrimdışı kalırsa, bu girişler aracı çevrimdışıyken oluşturulmuş olsa bile Azure Monitor en son kaldığı yerden girişleri toplar.

Günlük girişinin tüm içeriği **RawData**adı verilen tek bir özelliğe yazılır.  Alınan her günlük girişini birden çok özelliğe ayrıştma yöntemleri için [Azure Monitor'daki ayrıştirma metin verilerine](../log-query/parse-text.md) bakın.

## <a name="custom-log-record-properties"></a>Özel günlük kayıt özellikleri
Özel günlük kayıtları, sağladığınız günlük adı ve aşağıdaki tablodaki özellikleri içeren bir türü vardır.

| Özellik | Açıklama |
|:--- |:--- |
| TimeGenerated |Kaydın Azure Monitor tarafından toplandığı tarih ve saat.  Günlük zaman tabanlı bir sınırlayıcı kullanıyorsa, bu girişten toplanan süredir. |
| SourceSystem |Kaydın toplandığı aracı türü. <br> OpsManager – Windows aracısı, doğrudan bağlantı veya Sistem Merkezi İşlemleri Yöneticisi <br> Linux – Tüm Linux ajanları |
| Ham Veriler |Toplanan girişin tam metni. Büyük olasılıkla bu [verileri tek tek özelliklere ayrıştırmak isteyeceksiniz.](../log-query/parse-text.md) |
| ManagementGroupName |Sistem Merkezi Operasyon Yönetimi aracıları için yönetim grubunun adı.  Diğer aracılar için bu AOI-\<çalışma alanı kimliğidir\> |


## <a name="sample-walkthrough-of-adding-a-custom-log"></a>Özel bir günlük ekleme nin örnek walkthrough
Aşağıdaki bölüm, özel bir günlük oluşturma örneğinin üzerinden geçer.  Toplanan örnek günlük, tarih ve saatle başlayan her satırda tek bir girişe sahiptir ve ardından kod, durum ve ileti için virgülle sınırlandırılmış alanlara sahiptir.  Birkaç örnek girişleri aşağıda gösterilmiştir.

    2019-08-27 01:34:36 207,Success,Client 05a26a97-272a-4bc9-8f64-269d154b0e39 connected
    2019-08-27 01:33:33 208,Warning,Client ec53d95c-1c88-41ae-8174-92104212de5d disconnected
    2019-08-27 01:35:44 209,Success,Transaction 10d65890-b003-48f8-9cfc-9c74b51189c8 succeeded
    2019-08-27 01:38:22 302,Error,Application could not connect to database
    2019-08-27 01:31:34 303,Error,Application lost connection to database

### <a name="upload-and-parse-a-sample-log"></a>Örnek bir günlüğü yükleme ve ayrıştama
Günlük dosyalarından birini sağlarız ve toplayacakları olayları görebiliriz.  Bu durumda Yeni Hat yeterli bir sınır dışı layıcıdır.  Günlükteki tek bir giriş birden çok satıra yayılabilirse, zaman damgası sınırlayıcısının kullanılması gerekir.

![Örnek bir günlüğü yükleme ve ayrıştama](media/data-sources-custom-logs/delimiter.png)

### <a name="add-log-collection-paths"></a>Günlük toplama yolları ekleme
Günlük dosyaları *C:\MyApp\Logs*adresinde yer alır.  Yeni bir dosya desen *appYYYYMMDD.log*tarih içeren bir ad ile her gün oluşturulur.  Bu günlük için yeterli desen *C:\MyApp\Logs .logs\\\** olacaktır.

![Günlük toplama yolu](media/data-sources-custom-logs/collection-path.png)

### <a name="provide-a-name-and-description-for-the-log"></a>Günlük için bir ad ve açıklama sağlama
*Biz bir* **Açıklama**MyApp_CL ve yazın bir ad kullanın.

![Günlük adı](media/data-sources-custom-logs/log-name.png)

### <a name="validate-that-the-custom-logs-are-being-collected"></a>Özel günlüklerin toplandığını doğrulayın
Toplanan günlükteki tüm kayıtları döndürmek için basit bir *MyApp_CL* sorgusu kullanırız.

![Özel alan olmayan günlük sorgusu](media/data-sources-custom-logs/query-01.png)


## <a name="alternatives-to-custom-logs"></a>Özel günlüklere alternatifler
Verileriniz listelenen ölçütlere uyuyorsa özel günlükler kullanışlı olsa da, başka bir stratejiye ihtiyaç duyduğunuz durumlar şunlardır:

- Veriler, zaman damgasının farklı bir biçimde olması gibi gerekli yapıya uymuyor.
- Günlük dosyası, dosya kodlama veya desteklenmeyen klasör yapısı gibi gereksinimlere uymaz.
- Veriler, toplamadan önce ön işleme veya filtreleme gerektirir. 

Verilerinizin özel günlüklerle toplanamayacağı durumlarda, aşağıdaki alternatif stratejileri göz önünde bulundurun:

- Azure Monitor tarafından toplanan [Windows Events](data-sources-windows-events.md) veya [Syslog'a](data-sources-syslog.md) veri yazmak için özel bir komut dosyası veya başka bir yöntem kullanın. 
- [HTTP Veri Toplayıcı API'sini](data-collector-api.md)kullanarak verileri doğrudan Azure Monitor'a gönderin. 

## <a name="next-steps"></a>Sonraki adımlar
* Alınan her günlük girişini birden çok özelliğe ayrıştma yöntemleri için [Azure Monitor'daki ayrıştirma metin verilerine](../log-query/parse-text.md) bakın.
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin.
