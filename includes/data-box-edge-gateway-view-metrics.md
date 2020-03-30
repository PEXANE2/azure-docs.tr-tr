---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/15/2019
ms.author: alkohli
ms.openlocfilehash: e02c0b86cd542b3ea12914e35a6577cf4e9b43d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188822"
---
Ayrıca, aygıtın performansını izlemek için ölçümleri ve bazı durumlarda sorun giderme aygıtı sorunlarını da görüntüleyebilirsiniz.

Seçili aygıt ölçümleri için bir grafik oluşturmak için Azure portalında aşağıdaki adımları izleyin.

1. Azure portalındaki kaynağınız için **İzleme >** Ölçümleri'ne gidin ve **metrik ekle'yi**seçin.

    ![Ölçüm ekleme](media/data-box-edge-gateway-view-metrics/view-metrics-1.png)

2. Kaynak otomatik olarak doldurulur.  

    ![Geçerli kaynak](media/data-box-edge-gateway-view-metrics/view-metrics-2.png)

    Başka bir kaynak belirtmek için kaynağı seçin. Kaynak bıçağı **seçin'** de, abonelik, kaynak grubu, kaynak türü ve ölçümleri göstermek istediğiniz belirli kaynağı seçin ve **Uygula'yı**seçin.

    ![Başka bir kaynak seçin](media/data-box-edge-gateway-view-metrics/view-metrics-3.png)

3. Açılan listeden cihazınızı izlemek için bir metrik seçin. Ölçümler **Kapasite ölçümleri** veya **Hareket ölçümleri**olabilir. Kapasite ölçümleri aygıtın kapasitesiyle ilgilidir. İşlem ölçümleri, Azure Depolama'daki okuma ve yazma işlemleriyle ilgilidir.

    |Kapasite ölçümleri                     |Açıklama  |
    |-------------------------------------|-------------|
    |**Kullanılabilir kapasite**               | Aygıta yazılabilir verilerin boyutunu ifade eder. Başka bir deyişle, bu aygıtta kullanılabilir hale getirilebilir kapasitedir. <br></br>Hem aygıtta hem de bulutta kopyası olan dosyaların yerel kopyasını silerek aygıt kapasitesini boşaltabilirsiniz.        |
    |**Toplam kapasite**                   | Veri yazmak için aygıttaki toplam baytları ifade eder. Buna, yerel önbelleğin toplam boyutu da denir. <br></br> Artık bir veri diski ekleyerek varolan bir sanal aygıtın kapasitesini artırabilirsiniz. VM için hipervizör yönetimi aracılığıyla bir veri diski ekleyin ve ardından VM'nizi yeniden başlatın. Ağ Geçidi aygıtının yerel depolama havuzu, yeni eklenen veri diskini barındıracak şekilde genişletilir. <br></br>Daha fazla bilgi için [Hyper-V sanal makine için sabit disk ekle'ye](https://www.youtube.com/watch?v=EWdqUw9tTe4)gidin. |
    
    |İşlem ölçümleri              | Açıklama         |
    |-------------------------------------|---------|
    |**Yüklenen bulut baytları (aygıt)**    | Cihazınızdaki tüm paylaşımlara yüklenen tüm baytların toplamı        |
    |**Yüklenen bulut baytları (paylaş)**     | Hisse başına yüklenen baytlar. Bu yük şunlardan biri olabilir: <br></br> Avg, (Hisse başına yüklenen tüm baytların toplamı / Hisse sayısı),  <br></br>Max, bir paylaşımdan yüklenen maksimum bayt sayısıdır <br></br>Min, bir paylaşımdan yüklenen en az bayt sayısıdır      |
    |**Bulut indirme iş girişi (paylaş)**| Baytlar hisse başına indirildi. Bu yük şunlardan biri olabilir: <br></br> Avg, hangi (Tüm baytların toplamı okumak veya bir paya indirilen / Hisse Sayısı) <br></br> Max, bir paylaşımdan indirilen en fazla bayt sayısıdır<br></br> ve Min, bir hisseden indirilen bayt ların en az sayısıdır  |
    |**Bulut okuma iş özeti**            | Cihazınızdaki tüm paylaşımlarda buluttan okunan tüm baytların toplamı     |
    |**Bulut yükleme girişi**          | Cihazınızdaki tüm paylaşımlarda buluta yazılan tüm baytların toplamı     |
    |**Bulut yükleme iş girişi (paylaş)**  | Bir hisse / # hisse buluta yazılan tüm baytların toplamı ortalama, max ve hisse başına min      |
    |**Okuma iş sayısı (ağ)**           | Buluttan okunan tüm baytlar için sistem ağı iş bilgililiğini içerir. Bu görünüm, paylaşımlar ile sınırlı olmayan verileri içerebilir. <br></br>Bölme, aygıttaki tüm ağ bağdaştırıcıları üzerindeki trafiği gösterir. Buna bağlı olmayan veya etkin olmayan bağdaştırıcılar dahildir.      |
    |**Yazma iş (ağ)**       | Buluta yazılan tüm baytlar için sistem ağı masını içerir. Bu görünüm, paylaşımlar ile sınırlı olmayan verileri içerebilir. <br></br>Bölme, aygıttaki tüm ağ bağdaştırıcıları üzerindeki trafiği gösterir. Buna bağlı olmayan veya etkin olmayan bağdaştırıcılar dahildir.          |
    |**Kenar bilgi işlem - bellek kullanımı**      | Bu metrik Veri Kutusu Ağ Geçidi için geçerli değildir ve bu nedenle doldurulmaz.          |
    |**Kenar hesaplama - yüzde CPU**    | Bu metrik Veri Kutusu Ağ Geçidi için geçerli değildir ve bu nedenle doldurulmaz.         |

4. Açılan listeden bir metrik seçildiğinde, toplama da tanımlanabilir. Toplama, belirli bir zaman aralığında toplanan gerçek değeri ifade eder. Toplanan değerler ortalama, minimum veya maksimum değer olabilir. Avg, Max veya Min'den Toplama'yı seçin.

    ![Grafiği görüntüle](media/data-box-edge-gateway-view-metrics/view-metrics-4.png)

5. Seçtiğiniz metrikbirden çok örneği varsa, bölme seçeneği kullanılabilir. **Bölme uygula'yı** seçin ve ardından dökümü görmek istediğiniz değeri seçin.

    ![Bölme uygula](media/data-box-edge-gateway-view-metrics/view-metrics-5.png)

6. Şimdi yalnızca birkaç örnek için dökümü görmek istiyorsanız, verileri filtreleyebilirsiniz. Örneğin, bu durumda, yalnızca aygıtınızdaki iki bağlı ağ arabirimi için ağ çıktısını görmek istiyorsanız, bu arabirimleri filtreleyebilirsiniz. **Filtre Ekle'yi** seçin ve filtreleme için ağ arabirimi adını belirtin.

    ![Filtre ekle](media/data-box-edge-gateway-view-metrics/view-metrics-6.png)

7. Ayrıca, kolay erişim için grafiği panoya sabitleyebilir.

    ![Panoya sabitle](media/data-box-edge-gateway-view-metrics/view-metrics-7.png)

8. Grafik verilerini bir Excel elektronik tablosuna aktarmak veya paylaşabileceğiniz grafiğe bağlantı almak için komut çubuğundan paylaşım seçeneğini seçin.

    ![Verileri dışarı aktarma](media/data-box-edge-gateway-view-metrics/view-metrics-8.png)