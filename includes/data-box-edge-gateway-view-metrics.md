---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: e02c0b86cd542b3ea12914e35a6577cf4e9b43d8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188822"
---
Ayrıca, cihazın performansını izlemek için ölçümleri ve cihaz sorunlarını gidermeye yönelik bazı örnekleri görüntüleyebilirsiniz.

Seçili cihaz ölçümleri için bir grafik oluşturmak üzere Azure portal aşağıdaki adımları uygulayın.

1. Azure portal kaynağınız için **izleme > ölçümleri** ' ne gidin ve **ölçüm Ekle**' yi seçin.

    ![Ölçüm ekleme](media/data-box-edge-gateway-view-metrics/view-metrics-1.png)

2. Kaynak otomatik olarak doldurulur.  

    ![Geçerli kaynak](media/data-box-edge-gateway-view-metrics/view-metrics-2.png)

    Başka bir kaynak belirtmek için kaynağı seçin. **Kaynak Seç** dikey penceresinde, ölçümü göstermek istediğiniz aboneliği, kaynak grubunu, kaynak türünü ve belirli kaynağı seçin ve **Uygula**' yı seçin.

    ![Başka bir kaynak seçin](media/data-box-edge-gateway-view-metrics/view-metrics-3.png)

3. Açılan listeden, cihazınızı izlemek için bir ölçüm seçin. Ölçümler **Kapasite ölçümleri** veya **işlem ölçümleri**olabilir. Kapasite ölçümleri, cihazın kapasitesiyle ilgilidir. İşlem ölçümleri, Azure depolama 'ya yönelik okuma ve yazma işlemleriyle ilgilidir.

    |Kapasite ölçümleri                     |Açıklama  |
    |-------------------------------------|-------------|
    |**Kullanılabilir kapasite**               | Cihaza yazılabilen verilerin boyutunu ifade eder. Diğer bir deyişle, bu, cihazda kullanılabilir hale getirilebilir kapasitedir. <br></br>Hem cihazda hem de bulutta bir kopyası olan dosyaların yerel kopyasını silerek cihaz kapasitesini serbest bırakabilirsiniz.        |
    |**Toplam kapasite**                   | Verilerin yazılacağı cihazdaki toplam bayt sayısını ifade eder. Bu, yerel önbelleğin toplam boyutu olarak da adlandırılır. <br></br> Artık bir veri diski ekleyerek var olan bir sanal cihazın kapasitesini artırabilirsiniz. VM için hiper yönetici yönetimi aracılığıyla bir veri diski ekleyin ve ardından sanal makineyi yeniden başlatın. Ağ Geçidi cihazının yerel depolama havuzu, yeni eklenen veri diskine uyum sağlayacak şekilde genişletilir. <br></br>Daha fazla bilgi için, [Hyper-V sanal makinesi için sabit sürücü ekleme](https://www.youtube.com/watch?v=EWdqUw9tTe4)bölümüne gidin. |
    
    |İşlem ölçümleri              | Açıklama         |
    |-------------------------------------|---------|
    |**Karşıya yüklenen bulut baytları (cihaz)**    | Cihazınızdaki tüm paylaşımlar genelinde karşıya yüklenen tüm baytların toplamı        |
    |**Karşıya yüklenen bulut baytları (paylaşma)**     | Her bir paylaşılan bayt karşıya yüklendi. Bu yük şunlardan biri olabilir: <br></br> Ort, (paylaşım başına karşıya yüklenen baytların toplamı/paylaşım sayısı),  <br></br>En fazla, bir paylaşımdan karşıya yüklenen en fazla bayt sayısıdır <br></br>En az, bir paylaşımdan karşıya yüklenen bayt sayısı alt sınırı      |
    |**Bulut indirme verimlilik (paylaşma)**| Her bir paylaşıma indirilen bayt. Bu yük şunlardan biri olabilir: <br></br> Ortalama, (okuma veya bir paylaşıma/paylaşım sayısına indirilen tüm baytların toplamı) <br></br> En fazla, bir paylaşımdan indirilen en fazla bayt sayısıdır<br></br> ve bir paylaşımdan indirilen en az bayt sayısı olan dk  |
    |**Bulut okuma performansı**            | Cihazınızdaki tüm paylaşımlar genelinde buluttan okunan tüm baytların toplamı     |
    |**Bulut karşıya yükleme performansı**          | Cihazınızdaki tüm paylaşımlar genelinde buluta yazılan tüm baytların toplamı     |
    |**Bulut karşıya yükleme üretilen işi (paylaşma)**  | Bir paylaşımdan buluta yazılan tüm baytların toplamı, paylaşım başına ortalama, en fazla ve en az      |
    |**Okuma performansı (ağ)**           | Buluttan okunan tüm baytlar için sistem ağı aktarım hızını içerir. Bu görünüm, paylaşımlarla sınırlı olmayan verileri içerebilir. <br></br>Bölmek, cihazdaki tüm ağ bağdaştırıcılarının trafiğini gösterir. Bu, bağlı veya etkin olmayan bağdaştırıcılar içerir.      |
    |**Yazma işleme (ağ)**       | Buluta yazılan tüm baytlar için sistem ağı aktarım hızını içerir. Bu görünüm, paylaşımlarla sınırlı olmayan verileri içerebilir. <br></br>Bölmek, cihazdaki tüm ağ bağdaştırıcılarının trafiğini gösterir. Bu, bağlı veya etkin olmayan bağdaştırıcılar içerir.          |
    |**Edge işlem-bellek kullanımı**      | Bu ölçüm Data Box Gateway için geçerli değildir, bu nedenle doldurulmuyor.          |
    |**Edge hesaplama-CPU yüzdesi**    | Bu ölçüm Data Box Gateway için geçerli değildir, bu nedenle doldurulmuyor.         |

4. Açılan listeden bir ölçüm seçildiğinde, toplama de tanımlanabilir. Toplama, belirli bir zaman aralığında toplanan gerçek değeri ifade eder. Toplanmış değerler ortalama, en düşük veya en büyük değer olabilir. AVG, Max veya min toplamını seçin.

    ![Grafiği görüntüle](media/data-box-edge-gateway-view-metrics/view-metrics-4.png)

5. Seçtiğiniz metriğin birden çok örneği varsa, bölme seçeneği kullanılabilir. **Bölmeyi Uygula** ' yı seçin ve ardından dökümü görmek istediğiniz değeri seçin.

    ![Bölmeyi Uygula](media/data-box-edge-gateway-view-metrics/view-metrics-5.png)

6. Artık dökümü yalnızca birkaç örnek için görmek istiyorsanız, verileri filtreleyebilirsiniz. Örneğin, bu durumda, yalnızca cihazınızdaki iki bağlı ağ arabirimi için ağ aktarım hızını görmek istiyorsanız, bu arabirimlere filtre uygulanabilir. **Filtre Ekle** ' yi seçin ve filtreleme için ağ arabirimi adını belirtin.

    ![Filtre ekle](media/data-box-edge-gateway-view-metrics/view-metrics-6.png)

7. Ayrıca, kolay erişim için grafiği panoya sabitleyebilirsiniz.

    ![Panoya sabitle](media/data-box-edge-gateway-view-metrics/view-metrics-7.png)

8. Grafik verilerini bir Excel elektronik tablosuna aktarmak veya paylaşabileceğiniz grafiğin bir bağlantısını almak için komut çubuğundan Share (paylaşma) seçeneğini belirleyin.

    ![Verileri dışarı aktarma](media/data-box-edge-gateway-view-metrics/view-metrics-8.png)