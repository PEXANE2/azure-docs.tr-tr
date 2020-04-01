---
title: Çözüm eğitiminde kenardaki anormallikleri algılama - Azure | Microsoft Dokümanlar
description: Bu eğitimde Uzaktan İzleme çözüm hızlandırıcısını kullanarak IoT Edge cihazlarınızı nasıl izleyeceğinizi öğreneceksiniz.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: a812155474b244682613b38b9b9379fa6cdcdcd8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "66117695"
---
# <a name="tutorial-detect-anomalies-at-the-edge-with-the-remote-monitoring-solution-accelerator"></a>Öğretici: Uzaktan İzleme çözüm hızlandırıcısı ile kenardaki anormallikleri tespit etme

Bu eğitimde, Uzaktan İzleme çözümünü bir IoT Edge aygıtı tarafından algılanan anormalliklere yanıt verecek şekilde yapılandırırsınız. IoT Edge aygıtları, çözüme gönderilen telemetri hacmini azaltmak ve aygıttaki olaylara daha hızlı yanıt vermek için telemetriyi kenarda işlemenize olanak tanır. Kenar işlemenin avantajları hakkında daha fazla bilgi edinmek için [Azure IoT Edge nedir'e](../iot-edge/about-iot-edge.md)bakın.

Uzaktan izleme ile kenar işleme tanıtmak için, bu öğretici bir simüle yağ pompası jakı cihazı kullanır. Bu yağ pompası jakı Contoso adlı bir kuruluş tarafından yönetilir ve Uzaktan İzleme çözüm hızlandırıcısına bağlıdır. Yağ pompası jakındaki sensörler sıcaklığı ve basıncı ölçer. Contoso'daki operatörler, sıcaklıktaki anormal artışın yağ pompası jakının yavaşlamasına neden olabileceğini bilirler. Contoso'daki operatörlerin cihazın normal menzili içindeyken sıcaklığını izlemesi gerekmez.

Contoso, sıcaklık anormalliklerini algılayan yağ pompası jakına akıllı bir kenar modülü yerleştirmek istiyor. Başka bir kenar modülü Uzaktan İzleme çözümüne uyarılar gönderir. Bir uyarı alındığı zaman, bir Contoso operatörü bir bakım teknisyeni gönderebilirsiniz. Contoso, çözüm bir uyarı aldığında çalışacak e-posta gönderme gibi otomatik bir eylemi de yapılandırabilir.

Aşağıdaki diyagram, öğretici senaryodaki temel bileşenleri gösterir:

![Genel Bakış](media/iot-accelerators-remote-monitoring-edge/overview.png)

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Çözüme Bir IoT Edge cihazı ekleme
> * Kenar bildirimi oluşturma
> * Bildirimi aygıtta çalışacak modülleri tanımlayan bir paket olarak alma
> * Paketi IoT Edge cihazınıza dağıtın
> * Aygıttan gelen uyarıları görüntüleme

IoT Edge aygıtında:

* Çalışma süresi paketi alır ve modülleri yükler.
* Akış analizi modülü pompadaki sıcaklık anormalliklerini algılar ve sorunu çözmek için komutlar gönderir.
* Akış analizi modülü filtrelenmiş verileri çözüm hızlandırıcısına ileter.

Bu öğretici, Bir IoT Edge aygıtı olarak bir Linux sanal makine kullanır. Ayrıca yağ pompası jakı cihazı simüle etmek için bir kenar modülü yükleyin.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="add-an-iot-edge-device"></a>IoT Edge cihazı ekleme

Uzaktan İzleme çözüm hızlandırıcınıza bir IoT Edge aygıtı eklemek için iki adım vardır. Bu bölümde nasıl kullanılacağı gösterilmektedir:

* Uzaktan İzleme web Kullanıcı Aracı'ndaki **Aygıt Gezgini** sayfasına bir IoT Edge aygıtı ekleyin.
* IoT Edge çalışma süresini bir Linux sanal makinesine (VM) yükleyin.

### <a name="add-an-iot-edge-device-to-your-solution"></a>Çözümünüze bir IoT Edge cihazı ekleme

Uzaktan İzleme çözüm hızlandırıcısına bir IoT Edge aygıtı eklemek için web Kullanıcı Arabirimi'ndeki **Aygıt Gezgini** sayfasına gidin ve **+ Yeni aygıtı**tıklatın.

Yeni **cihaz** panelinde, **IoT Edge cihazını** seçin ve cihaz kimliği olarak **yağ pompası** girin. Varsayılan değerleri diğer ayarlar için bırakabilirsiniz. Ardından **Apply** (Uygula) öğesine tıklayın:

[![IoT Edge cihazı ekle](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-expanded.png#lightbox)

Aygıt bağlantı dizesini not alın, bu öğreticinin bir sonraki bölümünde bunu yapmanız gerekir.

Bir aygıtı Uzaktan İzleme çözüm hızlandırıcısında IoT hub'ına kaydettiğinizde, web Kullanıcı Arabirimi'ndeki **Aygıt Gezgini** sayfasında listelenir:

[![Yeni IoT Edge cihazı](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-expanded.png#lightbox)

Çözümdeki IoT Edge aygıtlarını yönetmeyi kolaylaştırmak için bir aygıt grubu oluşturun ve IoT Edge aygıtını ekleyin:

1. **Aygıt Gezgini** sayfasındaki listedeki **yağ pompası** aygıtını seçin ve ardından **İşler'i**tıklatın.

1. Aşağıdaki ayarları kullanarak aygıta **IsEdge** etiketini eklemek için bir iş oluşturun:

    | Ayar | Değer |
    | ------- | ----- |
    | İş     | Etiketler  |
    | İş Adı | AddEdgeTag |
    | Anahtar     | IsOilPump |
    | Değer   | E     |
    | Tür    | Metin  |

    [![Etiket ekle](./media/iot-accelerators-remote-monitoring-edge/addtag-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addtag-expanded.png#lightbox)

1. **Uygula'yı**tıklatın, ardından **Kapat.**

1. Aygıt **Gezgini** sayfasında, **aygıt gruplarını yönet'i**tıklatın.

1. **Yeni aygıt grubu oluştur'u**tıklatın. Aşağıdaki ayarlarla yeni bir aygıt grubu oluşturun:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı    | Yağ Pompaları |
    | Alan   | Etiketler.IsOilPump |
    | İşleç | = Eşittir |
    | Değer    | E |
    | Tür     | Metin |

    [![Aygıt grubu oluşturma](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-expanded.png#lightbox)

1. **Kaydet**'e tıklayın.

You IoT Edge cihazı artık **OilPumps** grubunda.

### <a name="install-the-edge-runtime"></a>Kenar çalışma süresini yükleme

Edge aygıtının yüklenmesi için Edge çalışma zamanı gerekir. Bu öğreticide, senaryoyu test etmek için Edge çalışma saatini bir Azure Linux VM'sine yüklersiniz. Aşağıdaki adımlar yüklemede Azure bulut kabuğunu kullanır ve VM'yi yapılandırın:

1. Azure'da bir Linux VM oluşturmak için aşağıdaki komutları çalıştırın. Bulunduğunuz yere yakın bir konum kullanabilirsiniz:

    ```azurecli-interactive
    az group create \
      --name IoTEdgeDevices \
      --location eastus
    az vm create \
      --resource-group IoTEdgeDevices \
      --name EdgeVM \
      --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest \
      --admin-username azureuser \
      --generate-ssh-keys \
      --size Standard_B1ms
    ```

1. Edge çalışma süresini aygıt bağlantı dizesiyle yapılandırmak için, daha önce not aldığınız aygıt bağlantı dizesini kullanarak aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az vm run-command invoke \
      --resource-group IoTEdgeDevices \
      --name EdgeVM \
      --command-id RunShellScript \
      --scripts 'sudo /etc/iotedge/configedge.sh "YOUR_DEVICE_CONNECTION_STRING"'
    ```

    Bağlantı dizenizi çift tırnak işaretlerine eklediğinden emin olun.

IoT Edge çalışma zamanını bir Linux aygıtına yüklediniz ve yapılandırıldınız. Bu eğitimin ilerleyen saatlerinde, IoT Edge modüllerini bu aygıta dağıtmak için Uzaktan İzleme çözümünü kullanırsınız.

## <a name="create-an-edge-manifest"></a>Kenar bildirimi oluşturma

Yağ jakı pompası cihazını simüle etmek için Edge cihazınıza aşağıdaki modülleri eklemeniz gerekir:

* Sıcaklık simülasyon modülü.
* Azure Akış Analizi anormallik algılama.

Aşağıdaki adımlar, bu modülleri içeren bir Kenar dağıtım bildiriminin nasıl oluşturulabileceğinizi gösterir. Daha sonra bu öğretici uzaktan izleme çözüm hızlandırıcı bir paket olarak bu bildirimi almak.

### <a name="create-the-azure-stream-analytics-job"></a>Azure Akış Analizi işini oluşturun

Portaldaki Stream Analytics işini Edge modülü olarak paketlemeden önce tanımlarsınız.

1. Azure portalında, **IoTEdgeDevices** kaynak grubundaki varsayılan seçenekleri kullanarak bir Azure depolama hesabı oluşturun. Seçtiğiniz adı not edin.

1. Azure portalında, **IoTEdgeDevices** kaynak grubunda bir **Akış Analizi İşi** oluşturun. Aşağıdaki yapılandırma değerlerini kullanın:

    | Seçenek | Değer |
    | ------ | ----- |
    | İş adı | EdgeDeviceJob |
    | Abonelik | Azure aboneliğiniz |
    | Kaynak grubu | IoTEdgeCihazlar |
    | Konum | Doğu ABD |
    | Barındırma ortamı | Edge |
    | Akış birimleri | 1 |

1. Portalda **EdgeDeviceJob** Stream Analytics işini açın, Girişleri tıklatın ve **telemetri**adı verilen bir **Edge Hub** akışı girişi ekleyin.

1. Portaldaki **EdgeDeviceJob** Stream Analytics işinde **Çıktılar'ı** tıklatın ve **çıktı**adı verilen bir **Edge Hub** çıktısı ekleyin.

1. Portaldaki **EdgeDeviceJob** Stream Analytics **işinde, Çıktılar'ı** tıklatın ve **uyarı**adı verilen ikinci bir **Edge Hub** çıkışı ekleyin.

1. Portaldaki **EdgeDeviceJob** Stream Analytics işinde **Sorgula'yı** tıklatın ve aşağıdaki **seçili** deyimi ekleyin:

    ```sql
    SELECT  
    avg(machine.temperature) as temperature, 
    'F' as temperatureUnit 
    INTO output 
    FROM telemetry TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second, 5) 
    SELECT 'reset' as command 
    INTO alert 
    FROM telemetry TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second, 3) 
    HAVING avg(machine.temperature) > 400
    ```

1. Portaldaki **EdgeDeviceJob** Stream Analytics işinde **Depolama hesabı ayarlarını**tıklatın. Bu bölümün başlangıcı olarak **IoTEdgeDevices** kaynak grubuna eklediğiniz depolama hesabını ekleyin. **Edgeconfig**adlı yeni bir kapsayıcı oluşturun.

Aşağıdaki ekran görüntüsü kaydedilen Akış Analizi işini gösterir:

[![Stream Analytics işi](./media/iot-accelerators-remote-monitoring-edge/streamjob-inline.png)](./media/iot-accelerators-remote-monitoring-edge/streamjob-expanded.png#lightbox)

Artık kenar cihazınızda çalıştırmak için bir Akış Analizi işi tanımladınız. İş, 5 saniyelik bir pencerenin ortalama sıcaklığını hesaplar. 3 saniyelik bir penceredeki ortalama sıcaklık 400'ün üzerine çıkarsa, iş de bir uyarı gönderir.

### <a name="create-the-iot-edge-deployment"></a>IoT Edge dağıtımını oluşturma

Ardından, Edge aygıtınızda çalışacak modülleri tanımlayan bir IoT Edge dağıtım bildirimi oluşturursunuz. Sonraki bölümde, bu bildirimi Uzaktan İzleme çözümünde bir paket olarak içeri aktarAbilirsiniz.

1. Azure portalında, Uzaktan İzleme çözümünüzdeki IoT hub'ına gidin. Uzaktan İzleme çözümünüzle aynı ada sahip kaynak grubunda IoT hub'ını bulabilirsiniz.

1. IoT hub'ında Otomatik **Aygıt Yönetimi** bölümünde **IoT Edge'i** tıklatın. **IoT Edge dağıtımı ekle'yi**tıklatın.

1. Dağıtım **> Ad ve Etiket Oluştur** sayfasında yağ pompası **aygıtı**adını girin. **İleri**'ye tıklayın.

1. **Modüller Ekle > Oluştur sayfasında** **+ Ekle'yi**tıklatın. **IoT Edge Modül'üni**seçin.

1. **IoT Edge Özel Modüller** panelinde, adı olarak **temperatureSensor** girin ve **asaedgedockerhubtest/asa-edge-test-module:sensor-ad-linux-amd64** görüntü URI olarak. **Kaydet**'e tıklayın.

1. **Modüller Ekle > Oluştur sayfasında,** ikinci bir modül eklemek için **+ Ekle'yi** tıklatın. **Azure Akış Analizi Modül'üni**seçin.

1. Kenar **dağıtım** panelinde, aboneliğinizi ve önceki bölümde oluşturduğunuz **EdgeDeviceJob'ı** seçin. **Kaydet**'e tıklayın.

1. Dağıtım **oluştur > Modülekle** sayfasında **İleri'yi**tıklatın.

1. Dağıtım **oluştur > Rotalar belirt** sayfasında aşağıdaki kodu ekleyin:

    ```sql
    {
      "routes": { 
        "alertsToReset": "FROM /messages/modules/EdgeDeviceJob/outputs/alert INTO BrokeredEndpoint(\"/modules/temperatureSensor/inputs/control\")", 
        "telemetryToAsa": "FROM /messages/modules/temperatureSensor/* INTO BrokeredEndpoint(\"/modules/EdgeDeviceJob/inputs/telemetry\")", 
        "ASAToCloud": "FROM /messages/modules/EdgeDeviceJob/* INTO $upstream" 
      } 
    }
    ```

    Bu kod, Akış Analizi modülünden çıktıyı doğru konumlara yönlendirir.

    **İleri**'ye tıklayın.

1. Dağıtım **Oluştur > Ölçümleri Belirt** sayfasında **İleri'yi**tıklatın.

1. Dağıtım **> Hedef Aygıtlar Oluştur** sayfasında öncelik olarak 10 girin. **İleri**'ye tıklayın.

1. Dağıtım **oluştur > Gözden Geçir dağıtım** sayfasında **Gönder'i**tıklatın:

    [![Dağıtımı gözden geçirme](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-expanded.png#lightbox)

1. Ana **IoT Edge** **sayfasında, IoT Edge dağıtımlarını**tıklatın. Dağıtımlar listesinde **yağ pompası cihazını** görebilirsiniz.

1. Yağ **pompası-cihaz** dağıtımını tıklatın ve ardından **IoT Edge bildirimini İndir'i**tıklatın. Dosyayı **yağ pompası-device.json** olarak yerel makinenizde uygun bir konuma kaydedin. Bu öğreticinin bir sonraki bölümünde bu dosyaya ihtiyacınız var.

Şimdi paket olarak Uzaktan İzleme çözümüne aktarmak için bir IoT Edge bildirimi oluşturdunuz. Genellikle, bir geliştirici IoT Edge modülleri ve bildirim dosyası oluşturur.

## <a name="import-a-package"></a>Paket alma

Bu bölümde, Uzaktan İzleme çözümünde Kenar bildirimini paket olarak içeri aktarAbilirsiniz.

1. Uzaktan İzleme web Kullanıcı Arama Sürümü'nde **Paketler** sayfasına gidin ve **+ Yeni Paket'e**tıklayın:

    [![Yeni paket](./media/iot-accelerators-remote-monitoring-edge/newpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newpackage-expanded.png#lightbox)

1. Yeni **Paket** panelinde, paket türü olarak **Edge Manifest'i** seçin, yerel makinenizdeki **yağ pompası-device.json** dosyasını bulmak için **Gözat'ı** tıklatın ve **Yükle'yi**tıklatın:

    [![Paketi yükleme](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-expanded.png#lightbox)

    Paketlerin listesi şimdi **yağ pompası-device.json** paketi içerir.

Sonraki bölümde, paketi Edge cihazınıza uygulayan bir dağıtım oluşturursunuz.

## <a name="deploy-a-package"></a>Paket dağıtma

Artık paketi cihazınıza dağıtmaya hazırsınız.

1. Uzaktan İzleme web Kullanıcı Arama Sürümü'nde **Dağıtımlar** sayfasına gidin ve **+ Yeni dağıtım'ı**tıklatın:

    [![Yeni dağıtım](./media/iot-accelerators-remote-monitoring-edge/newdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newdeployment-expanded.png#lightbox)

1. Yeni **dağıtım** panelinde, aşağıdaki ayarlara sahip bir dağıtım oluşturun:

    | Seçenek | Değer |
    | ------ | ----- |
    | Adı   | Yağ Pompası Cihazları |
    | Paket türü | Kenar Manifestosu |
    | Paket | yağ pompası-device.json |
    | Cihaz Grubu | Yağ Pompaları |
    | Öncelik | 10 |

    [![Dağıtım oluşturma](./media/iot-accelerators-remote-monitoring-edge/createdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdeployment-expanded.png#lightbox)

    **Uygula**’ya tıklayın.

Paketin cihazınıza dağıtılması ve telemetrinin cihazdan akmaya başlaması için birkaç dakika beklemeniz gerekir.

[![Dağıtım etkin](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-inline.png)](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-expanded.png#lightbox)

**Dağıtımlar** sayfası aşağıdaki ölçümleri gösterir:

* **Hedeflenen** aygıt grubundaki aygıt sayısını gösterir.
* **Uygulanan** dağıtım içeriği uygulanan aygıtların sayısını gösterir.
* **Başarılı,** Dağıtımdaki Edge aygıtlarının sayısını IoT Edge istemci çalışma saatinden başarı yı bildiren gösterir.
* **Başarısız,** IoT Edge istemci çalışma zamanından dağıtım raporlama hatasıedge aygıtlarının sayısını gösterir.

## <a name="monitor-the-device"></a>Cihazı izleme

Uzaktan İzleme web Kullanıcı Aracı'nda yağ pompası cihazınızdan sıcaklık telemetrisini görüntüleyebilirsiniz:

1. **Aygıt Gezgini** sayfasına gidin ve yağ pompası cihazınızı seçin.
1. **Cihaz ayrıntıları** panelinin **Telemetri** bölümünde **Sıcaklık'ı**tıklatın:

    [![Telemetri görüntüleme](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-expanded.png#lightbox)

Bir eşiğe ulaşana kadar sıcaklığın nasıl yükseldiğini görebilirsiniz. Stream Analytics Edge modülü, sıcaklığın bu eşiğe ne zaman ulaştığını algılar ve sıcaklığı hemen azaltmak için cihaza bir komut gönderir. Tüm bu işlemler bulutla iletişim kurmadan aygıtta gerçekleşir.

Eşiğe ulaşıldığında operatörleri bilgilendirmek istiyorsanız, Uzaktan İzleme web Kullanıcı Arama Hizmeti'nde bir kural oluşturabilirsiniz:

1. **Kurallar** sayfasına gidin ve **+ Yeni kuralını**tıklatın.
1. Aşağıdaki ayarlarla yeni bir kural oluşturun:

    | Seçenek | Değer |
    | ------ | ----- |
    | Kural adı | Yağ pompası sıcaklığı |
    | Açıklama | Yağ pompası sıcaklığı 300'ü aştı |
    | Cihaz grubu | Yağ Pompaları |
    | Hesaplama | Anında |
    | Alan | sıcaklık |
    | İşleç | > |
    | Değer | 300 |
    | Önem derecesi | Bilgi |

    [![Kural oluşturma](./media/iot-accelerators-remote-monitoring-edge/newrule-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newrule-expanded.png#lightbox)

    **Uygula**’ya tıklayın.

1. **Pano** sayfasına gidin. **Yağ pompası** cihazındaki sıcaklık 300'ün üzerine çıktığında **Uyarılar** panelinde bir uyarı gösterir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, Uzaktan İzleme çözüm hızlandırıcısında bir IoT Edge aygıtının nasıl eklendiğini ve yapılandırılabildiğini gösterdi. Uzaktan İzleme çözümünde IoT Edge paketleriyle çalışma hakkında daha fazla bilgi edinmek için aşağıdaki nasıl yapılacağını zedeleme kılavuzuna bakın:

> [!div class="nextstepaction"]
> [Uzaktan İzleme çözüm hızlandırıcınıza bir IoT Edge paketi alma](iot-accelerators-remote-monitoring-import-edge-package.md)

IoT Edge çalışma süresini yükleme hakkında daha fazla bilgi edinmek için Azure [IoT Edge çalışma saatini Linux'ta (x64) yükleyin.](../iot-edge/how-to-install-iot-edge-linux.md)

Edge cihazlarda Azure Akış Analizi hakkında daha fazla bilgi edinmek için Azure [Akış Analizi'ni IoT Edge modülü olarak dağıt'a](../iot-edge/tutorial-deploy-stream-analytics.md)bakın.
