---
title: Zaman Serisi Öngörüleri ile simüle edilmiş telemetriyi görselleştirin - Azure | Microsoft Dokümanlar
description: Aygıt Simülasyonu çözüm hızlandırıcısı tarafından oluşturulan telemetriyi keşfetmek ve analiz etmek için Zaman Serisi Öngörüleri ortamınızı nasıl yapılandıracaklarınızı öğrenin.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 2bbd7911a40d6a256d478e2533ad2469b8fd6973
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73889338"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-device-simulation-solution-accelerator"></a>Aygıt Simülasyonu çözüm hızlandırıcısından gönderilen telemetriyi görselleştirmek için Zaman Serisi Öngörülerini kullanın

Cihaz Simülasyonu çözüm hızlandırıcısı, IoT çözümlerinizi test etmek için simüle edilmiş cihazlardan telemetri oluşturmanıza olanak tanır. Bu nasıl yapılsın kılavuzu, zaman serisi öngörüleri ortamını kullanarak simüle edilmiş telemetriyi nasıl görselleştirip analiz edebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları izlemek için etkin bir Azure aboneliğine ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

Bu nasıl yapılmalı kılavuzundaki adımlar, Aygıt Simülasyonu çözüm hızlandırıcısını Azure aboneliğinize dağıtdığınızı varsayar. Çözüm hızlandırıcısını dağıtmadıysanız, Dağıt'taki adımları izleyin [ve bulut tabanlı bir aygıt simülasyon çözümü](quickstart-device-simulation-deploy.md) hızlı başlatın.

Bu makalede çözüm hızlandırıcı adını **varsayar contoso-simülasyon**. Aşağıdaki adımları tamamlarken **solüsyon simülasyonunu** çözüm hızlandırıcınızın adı ile değiştirin.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-consumer-group"></a>Tüketici grubu oluşturma

Zaman Serisi Öngörüleri'ne telemetri akışı sağlamak için IoT merkezinizde özel bir tüketici grubu oluşturmanız gerekir. Time Series Insights'taki bir etkinlik kaynağı, bir IoT Hub tüketici grubunun özel kullanımına sahip olmalıdır.

Aşağıdaki adımlar, tüketici grubunu oluşturmak için Azure Bulut Kabuğu'ndaki Azure CLI'yi kullanır:

1. IoT hub'ı, Aygıt Simülasyonu çözüm hızlandırıcısını dağıttığınızda oluşturulan birkaç kaynaktan biridir. Aşağıdaki komutu uygulayın IoT hub'ınızın adını bulun - çözüm hızlandırıcınızın adını kullanmayı unutmayın:

    ```azurecli-interactive
    az resource list --resource-group contoso-simulation -o table
    ```

    IoT hub türü **Microsoft.Devices/IotHubs**kaynağıdır.

1. Hub'a **devicesimulationtsi** adlı bir tüketici grubu ekleyin. Aşağıdaki komutta hub ve çözüm hızlandırıcınızın adını kullanın:

    ```azurecli-interactive
    az iot hub consumer-group create --hub-name contoso-simulation7d894 --name devicesimulationtsi --resource-group contoso-simulation
    ```

    Artık Azure Bulut Kabuğu'nu kapatabilirsiniz.

## <a name="create-a-new-time-series-insights-environment"></a>Yeni bir Time Series Öngörüleri ortamı oluşturun

[Azure Time Series Öngörüleri,](../../articles/time-series-insights/time-series-insights-overview.md) bulutta IoT ölçeğinde zaman serisi verilerini yönetmek için tamamen yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Yeni bir Time Series Öngörüleri ortamı oluşturmak için:

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Things > **Zaman Serisi Insights**bir kaynak > **Internet** **oluştur'un**seçin:

    ![Yeni Zaman Serisi Öngörüleri](./media/iot-accelerators-device-simulation-time-series-insights/new-time-series-insights.png)

1. Zaman Serisi Öngörüleri ortamınızı çözüm hızlandırıcınızla aynı kaynak grubunda oluşturmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ortam Adı | Aşağıdaki ekran görüntüsü **Contoso-TSI**adını kullanır. Bu adımı tamamladığınızda kendi benzersiz adınızı seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **aksoso-simülasyon**. Çözüm hızlandırıcınızın adını kullanın. |
    | Konum | Bu örnekdoğu **ABD**kullanır. Ortamınızı Aygıt simülasyon hızlandırıcınızla aynı bölgede oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |

    ![Zaman Serisi Öngörüleri Oluştur](./media/iot-accelerators-device-simulation-time-series-insights/new-time-series-insights-create.png)

    > [!NOTE]
    > Çözüm hızlandırıcıile aynı kaynak grubuna Zaman Serisi Öngörüleri ortamının eklenmesi, çözüm hızlandırıcısını sildiğinizde silindiği anlamına gelir.

1. **Oluştur'u**tıklatın. Ortamın oluşturulması birkaç dakika sürebilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IoT hub'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturduğunuz tüketici grubunu kullanın. Bir Time Series Insights etkinlik kaynağı, başka bir hizmet tarafından kullanılmayan özel bir tüketici grubu gerektirir.

1. Azure portalında yeni Zaman Serisi Ortamınıza gidin.

1. Solda, Olay **Kaynakları'nı**tıklatın:

    ![Etkinlik Kaynaklarını Görüntüle](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-sources.png)

1. **Ekle'yi**tıklatın :

    ![Etkinlik Kaynağı Ekle](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-sources-add.png)

1. IoT hub'ınızı yeni bir olay kaynağı olarak yapılandırmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Olay kaynağı Adı | Aşağıdaki ekran görüntüsü **contoso-iot-hub**adını kullanır. Bu adımı tamamladığınızda kendi benzersiz adınızı kullanın. |
    | Kaynak | **IoT Hub** |
    | İçeri aktarma seçeneği | **Kullanılabilir aboneliklerden IoT Hub'ı kullanma** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | Iot hub adı | **contoso-simulation7d894**. Aygıt Simülasyonu çözüm hızlandırıcınızdan IoT hub'ınızın adını kullanın. |
    | Iot hub ilke adı | **iothubowner** |
    | Iot hub ilkesi anahtarı | Bu alan otomatik olarak doldurulur. |
    | Iot hub tüketici grubu | **devicesimulationtsi** |
    | Olay serileştirme biçimi | **Json** |
    | Zaman damgası özellik adı | Boş bırakın |

    ![Etkinlik Kaynağı Oluşturma](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-source-create.png)

1. **Oluştur'u**tıklatın.

> [!NOTE]
> Zaman Serisi Öngörüler [gezginine ek kullanıcılara erişim izni](../../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) verebilirsiniz.

## <a name="start-a-simulation"></a>Simülasyon başlatın

Zaman Serisi Öngörüler explorer'ı kullanmadan önce, bazı telemetriler oluşturmak için Aygıt Simülasyonu çözüm hızlandırıcısını yapılandırın. Aşağıdaki ekran görüntüsü, 10 soğutucu cihazlar ile çalışan bir simülasyon gösterir:

![Çalışan cihaz simülasyonu](./media/iot-accelerators-device-simulation-time-series-insights/running-simulation.png)

## <a name="time-series-insights-explorer"></a>Time Series Insights gezgini

Time Series Insights explorer, telemetrinizi görselleştirmek için kullanabileceğiniz bir web uygulamasıdır.

1. Azure portalında, Zaman Serisi Öngörülere **Genel Bakış** sekmesini seçin.

1. Time Series Insights explorer web uygulamasını açmak **için Ortama Git'i**tıklatın:

    ![Time Series Insights gezgini](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-environment.png)

1. Zaman seçim panelinde, hızlı saatler menüsünden **Son 30 dakikayı** seçin ve **Ara'yı**tıklatın:

    ![Zaman Serisi Insights explorer arama](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-search-time.png)

1. Soldaki terimler panelinde, **ölçü** olarak **Measure** sıcaklığı ve **iothub-connection-device-id** değerini **Böl** olarak seçin:

    ![Zaman Serisi Öngörüler explorer sorgusu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-query1.png)

1. Grafiğe sağ tıklayın ve **olayları keşfedin**seçin:

    ![Zaman Serisi Insights explorer etkinlikleri](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-explore-events.png)

1. Olay verileri bir ızgarada gösterir:

    ![Zaman Serisi Insights explorer tablosu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-table.png)

1. Perspektif görünümü düğmesini tıklatın:

    ![Zaman Serisi Insights explorer perspektifi](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-explorer-perspective.png)

1. Perspektife yeni bir sorgu eklemek için tıklatın: **+**

    ![Zaman Serisi Öngörüler explorer Sorgu Ekle](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-new-query.png)

1. Zaman aralığı olarak **Son 30 dakikayı,** **Ölçü**olarak **Nem'i** ve **iothub-connection-device-id'i** de **Split By** değeri olarak seçin:

    ![Zaman Serisi Öngörüler explorer sorgusu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-query2.png)

1. Cihazınızın telemetri panosunu görüntülemek için perspektif görünümü düğmesini tıklatın:

    ![Zaman Serisi Insights explorer panosu](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-dashboard.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla araştırma yapmayı planlıyorsanız, çözüm hızlandırıcısını dağıtılmış bırakın.

Artık çözüm hızlandırıcısına ihtiyacınız yoksa, çözüm seçerek ve ardından **Çözüm Sil'i** [tıklatarak, Verilen çözümler](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasından silin.

Çözüm hızlandırıcının kaynak grubuna Time Series Öngörüleri ortamını eklediyseniz, çözüm hızlandırıcısını sildiğinizde otomatik olarak silinir. Aksi takdirde, Azure portalından Zaman Serisi Öngörüleri ortamını el ile kaldırmanız gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

Zaman Serisi Öngörüleri gezgininde verileri nasıl keşfedip sorgulayabilirsiniz hakkında daha fazla bilgi edinmek için [Azure Zaman Serisi Öngörüleri gezginine](../time-series-insights/time-series-insights-explorer.md)bakın.
