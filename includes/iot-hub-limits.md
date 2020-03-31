---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 34de38e91d47457d215c7ebf65d04ed2dbae5324
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74224538"
---
Aşağıdaki tabloda farklı hizmet katmanları S1, S2, S3 ve F1 ile ilişkili sınırlar listelendir. Her katmandaki her *birimin* maliyeti hakkında bilgi için [Azure IoT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/)bölümüne bakın.

| Kaynak | S1 Standart | S2 Standart | S3 Standart | F1 Ücretsiz |
| --- | --- | --- | --- | --- |
| İleti/gün |400,000 |6,000,000 |300,000,000 |8,000 |
| En fazla birim |200 |200 |10 |1 |

> [!NOTE]
> S1 veya S2 katman hub'ı olan 200'den fazla birim veya S3 katman hub'ı olan 10 birim kullanmayı bekliyorsanız, Microsoft Destek'e başvurun.
> 
> 

Aşağıdaki tabloda IoT Hub kaynakları için geçerli olan sınırlar listelenistır.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına en fazla ücretli IoT hub’ı sayısı |100 |
| Azure aboneliği başına en fazla ücretsiz IoT hub’ı sayısı |1 |
| Aygıt kimliğindeki maksimum karakter sayısı | 128 |
| En fazla cihaz kimliği sayısı<br/> (tek bir çağrıda döndürülen) |1000 |
| Cihazdan buluta iletiler için IoT Hub en fazla ileti bekletme süresi |7 gün |
| Cihazdan bulut iletinin en büyük boyutu |256 KB |
| Cihazdan buluta toplu işin en büyük boyutu |AMQP ve HTTP: tüm toplu iş için 256 KB <br/>MQTT: Her ileti için 256 KB |
| Cihazdan buluta toplu işte en fazla ileti sayısı |500 |
| Buluttan cihaza iletinin en büyük boyutu |64 KB |
| Buluttan cihaza iletiler için en büyük TTL |2 gün |
| Buluttan cihaza iletiler için en fazla teslim <br/> sayısı |100 |
| Aygıt başına maksimum bulut-aygıt kuyruk derinliği |50 |
| Geri bildirim iletileri için en fazla teslim sayısı <br/> (buluttan cihaza iletiye yanıt olarak) |100 |
| Buluttan cihaza iletiye yanıt olarak <br/> geri bildirim iletileri için en fazla TTL |2 gün |
| [En büyük cihaz ikizi boyutu](../articles/iot-hub/iot-hub-devguide-device-twins.md#device-twin-size) | Etiketler bölümü için 8 KB ve istenilen ve bildirilen özellikler bölümleri için 32 KB her |
| Cihaz çift dize anahtarının maksimum uzunluğu | 1 KB |
| Cihaz ikiz dize değerinin maksimum uzunluğu | 4 KB |
| [Cihaz ikizindeki en büyük nesne derinliği](../articles/iot-hub/iot-hub-devguide-device-twins.md#tags-and-properties-format) | 10 |
| Doğrudan yöntem yükünün en büyük boyutu | 128 KB |
| En büyük iş geçmişi bekletme süresi | 30 gün |
| En fazla eşzamanlı iş sayısı | 10 (S3 için), 5 (S2 için), 1 (S1 için) |
| En fazla ek uç nokta sayısı | 10 (S1, S2 ve S3 için) |
| En fazla ileti yönlendirme kuralı sayısı | 100 (S1, S2 ve S3 için) |
| Aynı anda bağlı aygıt akışlarının maksimum sayısı | 50 (yalnızca S1, S2, S3 ve F1 için) |
| Maksimum aygıt akışı veri aktarımı | Günde 300 MB (yalnızca S1, S2, S3 ve F1 için) |

> [!NOTE]
> Azure aboneliğinde 100'den fazla ücretli IoT hub'ına ihtiyacınız varsa Microsoft Destek'e başvurun.

> [!NOTE]
> Şu anda, tek bir IoT hub'ına kaydedilebilen toplam aygıt sayısı ve modül sayısı 1.000.000'dir. Bu sınırı artırmak istiyorsanız, [Microsoft Destek'e](https://azure.microsoft.com/support/options/)başvurun.

Aşağıdaki kotalar aşıldığında IoT Hub isteklerini daraltır.

| Kısıtlama | Hub başına değer |
| --- | --- |
| Kimlik kayıt defteri işlemleri <br/> (oluşturmak, almak, listele, güncellemek ve silmek), <br/> tek tek veya toplu içeri/dışarı aktarma |83.33/sn/birim (5.000/dk/birim) (S3 için). <br/> 1.67/sn/ünite (100/dk/birim) (S1 ve S2 için). |
| Cihaz bağlantıları |6.000/sn/birim (S3 için), 120/sn/birim (S2 için), 12/sn/birim (S1 için). <br/>En az 100/sn. |
| Cihazdan buluta gönderim |6.000/sn/birim (S3 için), 120/sn/birim (S2 için), 12/sn/birim (S1 için). <br/>En az 100/sn. |
| Buluttan cihaza gönderim | 83.33/sn/ünite (5.000/dk/birim) (S3 için), 1.67/sn/birim (100/dk/birim) (S1 ve S2 için). |
| Buluttan cihaza alım |833.33/sn/ünite (50.000/dk/birim) (S3 için), 16.67/sn/birim (1.000/dk/birim) (S1 ve S2 için). |
| Dosya karşıya yükleme işlemleri |83.33 dosya yükleme başlatmaları/sn/birim (5.000/dk/birim) (S3 için), 1.67 dosya yükleme başlatmaları/sn/birim (100/dk/birim) (S1 ve S2 için). <br/> Aynı anda 10.000 SAS URI'sı Azure Depolama hesabı için kullanılabilir.<br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| Doğrudan yöntemler | 24 MB/sn/birim (S3 için), 480 KB/sn/birim (S2 için), 160 KB/sn/birim (S1 için).<br/> 8-KB azaltma metre boyutuna göre. |
| Cihaz ikizi okumaları | 500/sn/birim (S3 için), Maksimum 100/sn veya 10/sn/birim (S2 için), 100/sn (S1 için) |
| Cihaz ikizi güncelleştirmeleri | 250/sn/birim (S3 için), Maksimum 50/sn veya 5/sn/birim (S2 için), 50/sn (S1 için) |
| İş işlemleri <br/> (oluşturma, güncelleme, listele ve silme) | 83.33/sn/birim (5.000/dk/birim) (S3 için), 1.67/sn/birim (100/dk/birim) (S2 için), 1.67/sn/sn/birim (100/dk/birim) (S1 için). |
| İşler işlemleri için cihaz başına aktarım hızı | 50/sn/birim (S3 için), maksimum 10/sn veya 1/sn/birim (S2 için), 10/sn (S1 için). |
| Cihaz akışı başlatma oranı | 5 yeni akış/sn (yalnızca S1, S2, S3 ve F1 için). |
