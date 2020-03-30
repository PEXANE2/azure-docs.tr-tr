---
title: Uzaktan İzleme'den Data Lake Store'a veri akışı - Azure | Microsoft Dokümanlar
description: Bir Azure Akışı Analizi işini kullanarak Uzaktan İzleme çözümünü Azure Veri Gölü Deposu ile nasıl entegre edebilirsiniz öğrenin.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 0a684151e01b298c60ff17ef1470e0648a425850
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73889247"
---
# <a name="integrate-the-remote-monitoring-solution-with-azure-data-lake-store"></a>Uzaktan İzleme çözümünü Azure Veri Gölü Deposu ile tümleştirme

Uzaktan İzleme çözümünde sunulanın ötesinde gelişmiş analiz gereksinimlerine sahip olabilirsiniz. Azure Veri Gölü Deposu, büyük ve çeşitli veri kümelerinden veri depolayabildiği ve isteğe bağlı analitik sağlamak için Azure Data Lake Analytics ile tümleştirebildiği için bu uygulama için idealdir.

Bu şekilde, Uzaktan İzleme çözümünüzdeki IoT hub'ından bir Azure Veri Gölü Mağazası'na veri akışı sağlamak için bir Azure Akış Analizi işini kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını tamamlamak için aşağıdakilere ihtiyacınız olacaktır:

* [Uzaktan İzleme çözüm hızlandırıcısını dağıtın.](quickstart-remote-monitoring-deploy.md)
  * Uzaktan İzleme çözümü, bu makalede kullanılan IoT hub'ını ve Azure Akışı Analizi işini Azure aboneliğinize dağıtır.
* [Azure Veri Gölü Deposu'na Dağıtma](../data-lake-store/data-lake-store-get-started-portal.md)
  * Veri Gölü Mağazanız, Uzaktan İzleme çözümünizle aynı bölgeye dağıtılmalıdır.
  * Hesabınızda "akış" adlı [bir klasör oluşturun.](../data-lake-store/data-lake-store-get-started-portal.md#createfolder)

## <a name="create-a-consumer-group"></a>Tüketici grubu oluşturma

Uzaktan İzleme çözümünüzün IoT merkezinde özel bir tüketici grubu oluşturun. Bu, Veri Gölü Mağazanıza veri akışı için Akış Analizi işi tarafından kullanılacaktır.

> [!NOTE]
> Tüketici grupları, uygulamalar tarafından Azure IoT Hub'ından veri çekmek için kullanılır. Her beş çıktı tüketicisi için yeni bir tüketici grubu oluşturmalısınız. En fazla 32 tüketici grubu oluşturabilirsiniz.

1. Azure Portal’da oturum açın.

1. Azure portalında Bulut **Kabuğu** düğmesini tıklatın.

    ![Portal Başlatma Simgesi](./media/iot-accelerators-integrate-data-lake/portal-launch-icon.png)

1. Yeni bir tüketici grubu oluşturmak için bu komutu uygulayın:

```azurecli-interactive
az iot hub consumer-group create --hub-name contoso-rm30263 --name streamanalyticsjob --resource-group contoso-rm
```

> [!NOTE]
> Uzaktan İzleme çözümünüzdeki kaynak grubunu ve IoT hub adlarını kullanın.

## <a name="create-stream-analytics-job"></a>Akış Analizi İşi Oluştur

Verileri IoT merkezinizden Azure Veri Gölü mağazanıza aktarmak için bir Azure Akış Analizi işi oluşturun.

1. **Kaynak Oluştur'u**tıklatın, Pazar Yeri'nden Nesnelerin İnterneti'ni seçin ve Akış **Analizi işini**tıklatın.

    ![Yeni Akış Analizi İş](./media/iot-accelerators-integrate-data-lake/new-stream-analytics-job.png)

1. Bir iş adı girin ve uygun Abonelik ve Kaynak grubunu seçin.

1. Veri Gölü Mağazanızın yakınında veya aynı bölgede bir Konum seçin. Burada Doğu ABD kullanıyoruz.

1. Barındırma ortamını varsayılan **Bulut**olarak bıraktığından emin olun.

1. **Oluştur'u**tıklatın.

    ![Akış Analizi İşi Oluştur](./media/iot-accelerators-integrate-data-lake/create-stream-analytics-job.png)

## <a name="configure-the-stream-analytics-job"></a>Akış Analizi işini yapılandırma

1. Uzaktan İzleme çözüm kaynak grubunuzdaki **Stream Analytics işine** gidin.

1. Genel Bakış **sayfasında, Girişler'i**tıklatın.

    ![Genel Bakış Sayfası](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview.png)

1. **Akış girişi ekle'yi** tıklatın ve açılır kaynaktan **IoT Hub'ını** seçin.

    ![Giriş Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-add-input.png)

1. Yeni giriş sekmesine, **IoTHub'ın**giriş takma adını girin.

1. Tüketici grubu açılır tarafından, daha önce oluşturduğunuz tüketici grubunu seçin. Burada **streamanalyticsjob**kullanıyoruz.

    ![Giriş'i Seçin](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-input.png)

1. **Kaydet**'e tıklayın.

1. Genel Bakış sayfasında **Çıktılar'ı**tıklatın.

    ![Veri Gölü Deposu Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-overview-2.png)

1. Açılan dosyadan Veri Gölü Deposu **Ekle'yi** tıklatın ve **Veri Gölü Deposu'nu** seçin.

    ![Çıktı Ekle](./media/iot-accelerators-integrate-data-lake/stream-analytics-output.png)

1. Yeni çıktı sekmesine **DataLakeStore'un**Çıktı takma adını girin.

1. Önceki adımlarda oluşturduğunuz Veri Gölü Deposu hesabını seçin ve depoya veri akışı için klasör yapısı sağlayın.

1. Tarih biçimi alanına **gir /streaming/{date}/{time}** girin. YYYY/MM/DD'nin varsayılan Tarih biçimini ve HH'nin Saat biçimini bırakın.

    ![Klasör Yapısını Sağlama](./media/iot-accelerators-integrate-data-lake/stream-analytics-new-output.png)

1. **Yetkilendir'i**tıklatın.

    Akış analizi iş dosya sistemine erişim yazmak vermek için Data Lake Store ile yetkilendirmeniz gerekir.

    ![Akış Analizini Data Lake Store'a Yetkilendirme](./media/iot-accelerators-integrate-data-lake/stream-analytics-out-authorize.png)

    Bir açılır pencere görürsünüz ve açılır pencere kapanında Yetkilendirme düğmesi yetkilendirme tamamlandıktan sonra soluk lakaplanır.

    > [!NOTE]
    > Açılır pencerede bir hata görürseniz, Gizli Mod'da yeni bir tarayıcı penceresi açın ve yeniden deneyin.

1. **Kaydet**'e tıklayın.

## <a name="edit-the-stream-analytics-query"></a>Akış Analizi sorgusunu düzenle

Azure Akış Analizi, verileri akışlayan, bu verileri istenilen şekilde dönüştüren ve çeşitli depolama veya işleme hedeflerine çıktı veren bir giriş kaynağı belirtmek için SQL benzeri bir sorgu dili kullanır.

1. Genel Bakış sekmesinde sorguyu **edit'i**tıklatın.

    ![Sorguyu Düzenle](./media/iot-accelerators-integrate-data-lake/stream-analytics-edit-query.png)

1. Sorgu düzenleyicisinde, [YourOutputAlias] ve [YourInputAlias] yer tutucularını daha önce tanımladığınız değerlerle değiştirin.

    ```sql
    SELECT
        *, System.Timestamp as time
    INTO
        DataLakeStore
    FROM
        IoTHub
    ```

    ![Akış Analizi Sorgusu](./media/iot-accelerators-integrate-data-lake/stream-analytics-query.png)

1. **Kaydet**'e tıklayın.
1. Değişiklikleri kabul etmek için **Evet'i** tıklatın.

## <a name="start-the-stream-analytics-job"></a>Akış Analizi işini başlatın

1. Genel Bakış sekmesinde **Başlat'ı**tıklatın.

    ![Akış Analizi İşini Başlat](./media/iot-accelerators-integrate-data-lake/stream-analytics-start.png)

1. İşi Başlat **sekmesinde, Özel'i**tıklatın.

1. Cihazınızın akışa başladığı tarihten itibaren verileri almak için birkaç saat geriye gitmek için özel zaman ayarlayın.

1. **Başlat**'a tıklayın.

    ![Özel Tarih Seçin](./media/iot-accelerators-integrate-data-lake/stream-analytics-start-custom.png)

    İş çalışma durumuna girene kadar bekleyin, sorgunuzdan olabilecek hatalar görürseniz, sözdiziminin doğru olduğundan emin olun.

    ![İş çalıştırma](./media/iot-accelerators-integrate-data-lake/stream-analytics-running.png)

    Akış işi, IoT Hub'ınızdaki verileri okumaya ve verileri Veri Gölü Deposu'nda depolamaya başlar. Verilerin Veri Gölü Deposu'nda görünmeye başlaması birkaç dakika sürebilir.

## <a name="explore-the-streaming-data"></a>Akış verilerini keşfedin

1. Data Lake Store'unuza gidin.

1. Genel Bakış sekmesinde, **Veri gezgini'ni**tıklatın.

1. Veri gezgininde, /streaming klasörüne ayrıntısına kadar **inin.** YYYY/MM/DD/HH formatında oluşturulan klasörleri görürsünüz.

    ![Akış Verilerini Keşfedin](./media/iot-accelerators-integrate-data-lake/data-lake-store-data-explorer.png)

    Saatte bir dosya içeren json dosyalarını göreceksiniz.

    ![Akış Verilerini Keşfedin](./media/iot-accelerators-integrate-data-lake/data-lake-store-file-preview.png)

## <a name="next-steps"></a>Sonraki Adımlar

Azure Veri Gölü Analizi, Veri Gölü Deposu veri kümelerinizde büyük veri analizi gerçekleştirmek için kullanılabilir. Data Lake [Analytics Dokümantasyon](https://docs.microsoft.com/azure/data-lake-analytics)hakkında daha fazla bilgi edinin.
