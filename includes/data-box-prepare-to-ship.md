---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 07/01/2020
ms.author: alkohli
ms.openlocfilehash: d099e33e7b35381f5404c9f8964d3ea90d4f3908
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959485"
---
Son adım cihazı göndermeye hazırlamaktır. Bu adımda tüm cihaz paylaşımları çevrimdışı duruma getirilir. Bu işlem başlatıldıktan sonra paylaşımlara erişilemez.

> [!IMPORTANT]
> Azure adlandırma kurallarıyla uyumlu olmayan verileri işaretlediği için göndermeye hazırlama adımı gereklidir. Bu adım, uyumsuz veriler nedeniyle olası veri yükleme hatalarını önler.

1. **Göndermeye hazırlama** sayfasına gidip **Hazırlamayı başlat**'a tıklayın. Varsayılan olarak, veriler kopyalanırken sağlama toplamları hesaplanır. Göndermeye hazırlama işlemi, sağlama toplamı hesaplamasını tamamlar ve dosya listesini ( *-BOM dosyaları*) oluşturur. Sağlama toplamı hesaplaması, verilerinizin boyutuna bağlı olarak birkaç saat veya birkaç gün sürebilir. 
   
    ![Göndermeye hazırlama 1](media/data-box-prepare-to-ship/prepare-to-ship1.png)

    Herhangi bir nedenle cihaz hazırlamayı durdurmak isterseniz **Hazırlamayı durdur**'a tıklayın. Daha sonra göndermeye hazırlama işlemine kaldığınız yerden devam edebilirsiniz.
        
    ![Göndermeye hazırlama 2](media/data-box-prepare-to-ship/prepare-to-ship2.png)
    
2. Göndermeye hazırlama işlemi başlar ve cihaz paylaşımları çevrimdışı olur. <!--You see a reminder to download the shipping label once the device is ready.--> Cihaz hazırlığı tamamlandıktan sonra cihaz durumu *Göndermeye hazır* olarak değişir ve cihaz kilitlenir.
        
    ![Göndermeye hazırlama 3](media/data-box-prepare-to-ship/prepare-to-ship3.png)

    Cihaza daha fazla veri kopyalamak istiyorsanız cihazın kilidini açabilir, daha fazla veri kopyalayabilir ve göndermeye hazırlamayı tekrar çalıştırabilirsiniz.

    Bu adımda hatalar varsa durum *Tarama hatalarla tamamlandı* olarak güncelleştirilir. Cihazın kilidini açıp **Bağlan ve kopyala** sayfasına gidin, sorunlar listesini indirin ve hataları çözün.

    ![Göndermeye hazırlama 1](media/data-box-prepare-to-ship/prepare-to-ship4.png)

    Hatalar çözümlendikten sonra **Göndermeye hazırlama** işlemini çalıştırın.

4. Göndermeye hazırlama aşaması başarıyla tamamlandığında (hatasız) şu adımları izleyin:

    1. Tamamlanma başvuru numarasını not alın. Bulunduğunuz ülkeye bağlı olarak bu numara farklı işlemler için gerekebilir.
    2. Bu işlem sırasında kopyalanan dosyaların (bildirim olarak da bilinir) listesini indirin. Daha sonra bu listeyi kullanarak Azure'a yüklenen dosyaları doğrulayabilirsiniz. Daha fazla bilgi için bkz. [Göndermeye hazırlama sırasında BOM dosyalarını inceleme](../articles/databox/data-box-logs.md#inspect-bom-during-prepare-to-ship).
        
        ![Göndermeye hazırlama 1](media/data-box-prepare-to-ship/prepare-to-ship5.png)
    3. Cihazın gönderim yönergelerini seçip indirin. Gönderim yönergeleri, bulunduğunuz ülkeye bağlı olarak farklı olabilir.
    4. E-mürekkep gönderim etiketini göstermiyorsa ters gönderim etiketini buradan indirebilirsiniz. 

5. Cihazı kapatın. **Kapat veya yeniden başlat** sayfasına gidip **Kapat**'a tıklayın. Onayınız istendiğinde devam etmek için **Tamam**'a tıklayın.

6. Kabloları sökün. Bir sonraki adım cihazı Microsoft'a göndermektir.
