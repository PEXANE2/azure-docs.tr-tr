---
title: Çözüm öğreticisindeki anormallikleri algılama-Azure | Microsoft Docs
description: Bu öğreticide, uzaktan Izleme çözüm Hızlandırıcısını kullanarak IoT Edge cihazlarınızı izlemeyi öğreneceksiniz.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: a812155474b244682613b38b9b9379fa6cdcdcd8
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "66117695"
---
# <a name="tutorial-detect-anomalies-at-the-edge-with-the-remote-monitoring-solution-accelerator"></a>Öğretici: uzaktan Izleme çözümü hızlandırıcısına sahip olan anormallikleri Algıla

Bu öğreticide, uzaktan Izleme çözümünü bir IoT Edge cihaz tarafından algılanan anormallara yanıt verecek şekilde yapılandırırsınız. IoT Edge cihazlar, çözüme gönderilen telemetri hacmini azaltmak ve cihazlarda olaylara daha hızlı yanıt vermek için, en kenarda Telemetriyi işlemenizi sağlar. Edge işlemenin avantajları hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Edge nedir](../iot-edge/about-iot-edge.md).

Bu öğretici, uzaktan izleme ile kenar işleme tanıtmak için, sanal bir yağ pompa jakı kullanır. Bu yağ pompa jakı, contoso adlı bir kuruluş tarafından yönetilir ve uzaktan Izleme çözüm hızlandırıcısına bağlanır. Yağ pompa jakına ilişkin algılayıcılar sıcaklık ve basınç ölçer. Contoso 'daki operatörler, sıcaklığın anormal artışının, yağ pompa jakının yavaşlamasına neden olduğunu bilir. Contoso 'daki operatörler, normal aralığında olduğunda cihazın sıcaklığını izlemeye gerek yoktur.

Contoso, sıcaklık bozuklularını algılayan yağ pompa jakına akıllı bir sınır modülünü dağıtmak istiyor. Başka bir Edge modülü, uyarıları uzaktan Izleme çözümüne gönderir. Bir uyarı alındığında, contoso işleci bir bakım teknisyeni gönderebilir. Contoso Ayrıca, çözüm bir uyarı aldığında çalıştırılacak bir e-posta gönderme gibi otomatik bir eylem de yapılandırabilir.

Aşağıdaki diyagramda, öğretici senaryosunda anahtar bileşenleri gösterilmektedir:

![Genel Bakış](media/iot-accelerators-remote-monitoring-edge/overview.png)

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Çözüme IoT Edge bir cihaz ekleme
> * Kenar bildirimi oluşturma
> * Bildirimi cihazda çalıştırılacak modülleri tanımlayan bir paket olarak içeri aktarın
> * Paketi IoT Edge cihazınıza dağıtma
> * Cihazdaki uyarıları görüntüleme

IoT Edge cihazında:

* Çalışma zamanı paketi alır ve modülleri kurar.
* Stream Analytics modülü, pompa 'nin sıcaklık bozuklularını algılar ve bu sorunu çözmek için komut gönderir.
* Stream Analytics modülü filtrelenmiş verileri çözüm hızlandırıcısına iletir.

Bu öğretici bir Linux sanal makinesini IoT Edge bir cihaz olarak kullanır. Ayrıca yağ pompa jakı cihazının benzetimini yapmak için bir Edge modülü de yüklersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [iot-accelerators-tutorial-prereqs](../../includes/iot-accelerators-tutorial-prereqs.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="add-an-iot-edge-device"></a>IoT Edge cihazı ekleme

Uzaktan Izleme çözüm hızlandırıcısına IoT Edge bir cihaz eklemenin iki adımı vardır. Bu bölümde, ' nin nasıl kullanılacağı gösterilmektedir:

* Uzaktan Izleme Web Kullanıcı arabirimindeki **Device Explorer** sayfasına IoT Edge bir cihaz ekleyin.
* IoT Edge çalışma zamanını bir Linux sanal makinesine (VM) yükler.

### <a name="add-an-iot-edge-device-to-your-solution"></a>Çözümünüze IoT Edge bir cihaz ekleyin

Uzaktan Izleme çözüm hızlandırıcısına IoT Edge bir cihaz eklemek için Web Kullanıcı arabirimindeki **Device Explorer** sayfasına gidin ve **+ yeni cihaz**' a tıklayın.

**Yeni cihaz** panelinde **IoT Edge cihaz** ' ı seçin ve cihaz kimliği olarak **yağ-pompa** girin. Diğer ayarlar için varsayılan değerleri bırakabilirsiniz. Ardından **Apply** (Uygula) öğesine tıklayın:

[![IoT Edge cihaz ekle](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addedgedevice-expanded.png#lightbox)

Cihaz bağlantı dizesini, Bu öğreticinin sonraki bölümünde gerekli olduğunu unutmayın.

Bir cihazı uzaktan Izleme çözüm hızlandırıcısında IoT Hub 'ına kaydettiğinizde, Web Kullanıcı arabirimindeki **Device Explorer** sayfasında listelenir:

[![Yeni IoT Edge cihaz](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newedgedevice-expanded.png#lightbox)

Çözümdeki IoT Edge cihazları yönetmeyi kolaylaştırmak için bir cihaz grubu oluşturun ve IoT Edge cihazı ekleyin:

1. **Device Explorer** sayfasındaki listeden **yağ-pompa** cihazını seçin ve ardından **işler**' e tıklayın.

1. Aşağıdaki ayarları kullanarak IBir **ıdge** etiketini cihaza eklemek için bir iş oluşturun:

    | Ayar | Değer |
    | ------- | ----- |
    | İş     | Etiketler  |
    | İş Adı | AddEdgeTag |
    | Anahtar     | Izeilpompa |
    | Değer   | Y     |
    | Tür    | Metin  |

    [![Etiket ekle](./media/iot-accelerators-remote-monitoring-edge/addtag-inline.png)](./media/iot-accelerators-remote-monitoring-edge/addtag-expanded.png#lightbox)

1. **Uygula**' ya ve ardından **Kapat**' a tıklayın.

1. **Device Explorer** sayfasında, **cihaz gruplarını yönet**' e tıklayın.

1. **Yeni cihaz grubu oluştur**' a tıklayın. Aşağıdaki ayarlarla yeni bir cihaz grubu oluşturun:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad    | OilPumps |
    | Alan   | Tags. ısoilpompa |
    | İşleç | = Eşittir |
    | Değer    | Y |
    | Tür     | Metin |

    [![Cihaz grubu oluştur](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdevicegroup-expanded.png#lightbox)

1. **Kaydet**’e tıklayın.

Cihaz artık **Oilpumps** grubunda IoT Edge.

### <a name="install-the-edge-runtime"></a>Edge çalışma zamanını yükler

Sınır cihazı, Edge çalışma zamanının yüklenmesini gerektirir. Bu öğreticide, senaryoyu test etmek için Edge çalışma zamanını bir Azure Linux sanal makinesine yüklersiniz. Aşağıdaki adımlar, VM 'yi yükleyip yapılandırmak için Azure Cloud Shell 'i kullanır:

1. Azure 'da bir Linux VM oluşturmak için aşağıdaki komutları çalıştırın. Şu konumda bir konum kapatabilirsiniz:

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

1. Uç çalışma zamanını cihaz bağlantı dizesiyle yapılandırmak için, daha önce bir değişiklik yaptığınız cihaz bağlantı dizesini kullanarak aşağıdaki komutu çalıştırın:

    ```azurecli-interactive
    az vm run-command invoke \
      --resource-group IoTEdgeDevices \
      --name EdgeVM \
      --command-id RunShellScript \
      --scripts 'sudo /etc/iotedge/configedge.sh "YOUR_DEVICE_CONNECTION_STRING"'
    ```

    Bağlantı dizenizi çift tırnak işaretleri içine eklediğinizden emin olun.

Şimdi bir Linux cihazında IoT Edge çalışma zamanını yüklediniz ve yapılandırdınız. Bu öğreticide daha sonra, bu cihaza IoT Edge modülleri dağıtmak için uzaktan Izleme çözümünü kullanırsınız.

## <a name="create-an-edge-manifest"></a>Kenar bildirimi oluşturma

Yağ jak pompa cihazının benzetimini yapmak için Edge cihazınıza aşağıdaki modülleri eklemeniz gerekir:

* Sıcaklık simülasyonu modülü.
* Anomali algılama Azure Stream Analytics.

Aşağıdaki adımlarda, bu modülleri içeren bir Edge dağıtım bildiriminin nasıl oluşturulacağı gösterilmektedir. Bu öğreticide daha sonra bu bildirimi, uzaktan Izleme çözüm hızlandırıcısında bir paket olarak içeri aktarırsınız.

### <a name="create-the-azure-stream-analytics-job"></a>Azure Stream Analytics işini oluşturma

Stream Analytics işini, bir Edge modülü olarak paketlemeden önce portalda tanımlarsınız.

1. Azure portal, **ıotedgedevices** kaynak grubundaki varsayılan seçenekleri kullanarak bir Azure depolama hesabı oluşturun. Seçtiğiniz adı bir yere getirin.

1. Azure portal, **ıotedgedevices** kaynak grubunda bir **Stream Analytics iş** oluşturun. Aşağıdaki yapılandırma değerlerini kullan:

    | Seçenek | Değer |
    | ------ | ----- |
    | İş adı | EdgeDeviceJob |
    | Abonelik | Azure aboneliğiniz |
    | Kaynak grubu | Iotedgedevices |
    | Konum | Doğu ABD |
    | Barındırma ortamı | Edge |
    | Akış birimleri | 1 |

1. Portalda **Edgedevicejob** Stream Analytics işini açın, girişler ' e tıklayın ve **telemetri**adlı bir **Edge hub** akış girişi ekleyin.

1. Portaldaki **Edgedevicejob** Stream Analytics Işinde, **çıktılar** ' e tıklayın ve **Çıkış**adlı bir **uç hub** çıkışı ekleyin.

1. Portaldaki **Edgedevicejob** Stream Analytics Işinde, **çıktılar** ' e tıklayın ve **Uyarı**adlı ikinci bir **Edge hub** çıkışı ekleyin.

1. Portalda **Edgedevicejob** Stream Analytics işi ' ne tıklayın, **sorgu** ' ya tıklayın ve aşağıdaki **Select** ifadesini ekleyin:

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

1. Portalda **Edgedevicejob** Stream Analytics işi ' nde, **depolama hesabı ayarları**' na tıklayın. Bu bölümün başlangıcı olarak **ıotedgedevices** kaynak grubuna eklediğiniz depolama hesabını ekleyin. **Edgeconfig**adlı yeni bir kapsayıcı oluşturun.

Aşağıdaki ekran görüntüsünde, kaydedilen Stream Analytics işi gösterilmektedir:

[![Stream Analytics işi](./media/iot-accelerators-remote-monitoring-edge/streamjob-inline.png)](./media/iot-accelerators-remote-monitoring-edge/streamjob-expanded.png#lightbox)

Artık Edge cihazınızda çalıştırmak üzere bir Stream Analytics işi tanımladınız. İş, ortalama sıcaklığı 5 saniyelik bir pencere üzerinden hesaplar. Ayrıca, 3 saniyelik bir penceredeki ortalama sıcaklık 400 üzerinde kaldığında iş bir uyarı gönderir.

### <a name="create-the-iot-edge-deployment"></a>IoT Edge dağıtımını oluşturma

Daha sonra, Edge cihazınızda çalıştırılacak modülleri tanımlayan bir IoT Edge dağıtım bildirimi oluşturursunuz. Sonraki bölümde, bu bildirimi uzaktan Izleme çözümünde bir paket olarak içeri aktarırsınız.

1. Azure portal, uzaktan Izleme çözümünüzdeki IoT Hub 'ına gidin. IoT Hub 'ını, uzaktan Izleme çözümünüz ile aynı ada sahip kaynak grubunda bulabilirsiniz.

1. IoT Hub 'ında **otomatik cihaz yönetimi** bölümünde **IoT Edge** ' a tıklayın. **IoT Edge dağıtımı Ekle**' ye tıklayın.

1. **Dağıtım > adı ve etiketi oluştur** sayfasında, **yağ-pompa-cihazı**adını girin. **İleri**’ye tıklayın.

1. **Dağıtım > modülleri Ekle** sayfasında **+ Ekle**' ye tıklayın. **IoT Edge modülünü**seçin.

1. **IoT Edge özel modüller** panelinde ad olarak **sıcaklık algılayıcısı** girin ve **asaedgedockerhubtest/asa-Edge-test-Module: algılayıcı-ad-Linux-AMD64** ' i görüntü URI 'si olarak girin. **Kaydet**’e tıklayın.

1. **Dağıtım > Modül Ekle** sayfasında, ikinci bir modül eklemek Için **+ Ekle** ' ye tıklayın. **Azure Stream Analytics modülünü**seçin.

1. **Kenar dağıtım** panelinde, aboneliğinizi ve önceki bölümde oluşturduğunuz **Edgedevicejob** ' ı seçin. **Kaydet**’e tıklayın.

1. **Dağıtım > modülleri Ekle** sayfasında **İleri**' ye tıklayın.

1. **Yol belirtin > dağıtım oluştur** sayfasında, aşağıdaki kodu ekleyin:

    ```sql
    {
      "routes": { 
        "alertsToReset": "FROM /messages/modules/EdgeDeviceJob/outputs/alert INTO BrokeredEndpoint(\"/modules/temperatureSensor/inputs/control\")", 
        "telemetryToAsa": "FROM /messages/modules/temperatureSensor/* INTO BrokeredEndpoint(\"/modules/EdgeDeviceJob/inputs/telemetry\")", 
        "ASAToCloud": "FROM /messages/modules/EdgeDeviceJob/* INTO $upstream" 
      } 
    }
    ```

    Bu kod, çıktıyı Stream Analytics modülünden doğru konumlara yönlendirir.

    **İleri**’ye tıklayın.

1. **Dağıtım oluştur > ölçümleri belirt** sayfasında **İleri**' ye tıklayın.

1. **Dağıtım > hedef cihazları oluştur** sayfasında, öncelik olarak 10 girin. **İleri**’ye tıklayın.

1. **Dağıtım > dağıtım oluşturmayı gözden geçir** sayfasında **Gönder**' e tıklayın:

    [![Dağıtımı gözden geçir](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/reviewdeployment-expanded.png#lightbox)

1. Ana **IoT Edge** sayfasında, **IoT Edge dağıtımlar**' a tıklayın. Bir dağıtım listesinde **yağ-pompa-cihazını** görebilirsiniz.

1. **Yağ-pompa-cihaz** dağıtımına ve ardından **IoT Edge bildirimi indir**' e tıklayın. Dosyayı **oil-pump-device.js** olarak yerel makinenizde uygun bir konuma kaydedin. Bu öğreticinin sonraki bölümünde bu dosyanın olması gerekir.

Artık bir paket olarak uzaktan Izleme çözümüne içeri aktarmak için bir IoT Edge bildirimi oluşturdunuz. Genellikle, geliştirici IoT Edge modüller ve bildirim dosyası oluşturur.

## <a name="import-a-package"></a>Bir paketi içeri aktar

Bu bölümde, uç bildirimini uzaktan Izleme çözümünde bir paket olarak içeri aktarırsınız.

1. Uzaktan Izleme Web Kullanıcı arabiriminde, **paketler** sayfasına gidin ve **+ yeni paket**' e tıklayın:

    [![Yeni paket](./media/iot-accelerators-remote-monitoring-edge/newpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newpackage-expanded.png#lightbox)

1. **Yeni paket** panelinde, paket türü olarak **Edge bildirimi** ' ni seçin, yerel makinenizde dosya **oil-pump-device.js** bulmak için, **Araştır** ' a tıklayın ve **karşıya yükle**' ye tıklayın:

    [![Paketi karşıya yükle](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-inline.png)](./media/iot-accelerators-remote-monitoring-edge/uploadpackage-expanded.png#lightbox)

    Paket listesi artık paketteki **oil-pump-device.js** içerir.

Sonraki bölümde, paketini Edge cihazınıza uygulayan bir dağıtım oluşturursunuz.

## <a name="deploy-a-package"></a>Paket dağıtma

Şimdi, paketi cihazınıza dağıtmaya hazırsınız demektir.

1. Uzaktan Izleme Web Kullanıcı arabiriminde **dağıtımlar** sayfasına gidin ve **+ yeni dağıtım**' ye tıklayın:

    [![Yeni dağıtım](./media/iot-accelerators-remote-monitoring-edge/newdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newdeployment-expanded.png#lightbox)

1. **Yeni dağıtım** panelinde, aşağıdaki ayarlarla bir dağıtım oluşturun:

    | Seçenek | Değer |
    | ------ | ----- |
    | Ad   | Oilbaldevices |
    | Paket türü | Edge bildirimi |
    | Paket | Üzerinde oil-pump-device.js |
    | Cihaz grubu | OilPumps |
    | Öncelik | 10 |

    [![Dağıtım oluşturma](./media/iot-accelerators-remote-monitoring-edge/createdeployment-inline.png)](./media/iot-accelerators-remote-monitoring-edge/createdeployment-expanded.png#lightbox)

    **Uygula**’ya tıklayın.

Paketin cihazınıza dağıtılması ve Telemetriyi cihazdan akışa başlaması için birkaç dakika beklemeniz gerekir.

[![Dağıtım etkin](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-inline.png)](./media/iot-accelerators-remote-monitoring-edge/deploymentactive-expanded.png#lightbox)

**Dağıtımlar** sayfasında aşağıdaki ölçümler gösterilmektedir:

* **Hedeflenen** cihaz grubundaki cihazların sayısını gösterir.
* **Uygulanan** , dağıtım içeriğinin uygulandığı cihazların sayısını gösterir.
* **Başarılı** , IoT Edge istemci çalışma zamanından dağıtım raporlama başarısı içindeki uç cihazların sayısını gösterir.
* **Başarısız oldu** , IoT Edge istemci çalışma zamanından dağıtım raporlama başarısızlığının uç cihaz sayısını gösterir.

## <a name="monitor-the-device"></a>Cihazı izleme

Yağ pompa cihazınızdan, uzaktan Izleme Web Kullanıcı arabirimindeki sıcaklık telemetrisini görüntüleyebilirsiniz:

1. **Device Explorer** sayfasına gidin ve yağ pompa cihazınızı seçin.
1. **Cihaz ayrıntıları** panelinin **telemetri** bölümünde **sıcaklık**' ya tıklayın:

    [![Telemetri görüntüleme](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-edge/viewtelemetry-expanded.png#lightbox)

Bir eşiğe ulaşıncaya kadar sıcaklığın nasıl yapıldığını görebilirsiniz. Stream Analytics Edge modülü, sıcaklığın Bu eşiğe ulaştığını algılar ve sıcaklığın hemen azaltılması için cihaza bir komut gönderir. Bu işleme, bulutta iletişim kurmadan cihazda gerçekleşir.

Eşiğe ulaşıldığında işleçleri bilgilendirmek istiyorsanız, uzaktan Izleme Web Kullanıcı arabiriminde bir kural oluşturabilirsiniz:

1. **Kurallar** sayfasına gidin ve **+ Yeni kural**' a tıklayın.
1. Aşağıdaki ayarlarla yeni bir kural oluşturun:

    | Seçenek | Değer |
    | ------ | ----- |
    | Kural adı | Yağ pompa sıcaklığı |
    | Açıklama | Yağ pompa sıcaklığı 300 |
    | Cihaz grubu | OilPumps |
    | Hesaplama | Anında |
    | Alan | sıcaklık |
    | İşleç | > |
    | Değer | 300 |
    | Önem derecesi | Bilgi |

    [![Kural Oluştur](./media/iot-accelerators-remote-monitoring-edge/newrule-inline.png)](./media/iot-accelerators-remote-monitoring-edge/newrule-expanded.png#lightbox)

    **Uygula**’ya tıklayın.

1. **Pano** sayfasına gidin. **Yağ-pompa** cihazındaki sıcaklık 300 üzerinden geçtiğinde **Uyarılar** panelinde bir uyarı gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, uzaktan Izleme çözüm hızlandırıcısında bir IoT Edge cihazının nasıl ekleneceğini ve yapılandırılacağını gösterdi. Uzaktan Izleme çözümünde IoT Edge paketleriyle çalışma hakkında daha fazla bilgi edinmek için aşağıdaki nasıl yapılır kılavuzuna bakın:

> [!div class="nextstepaction"]
> [Uzaktan Izleme çözüm hızlandırıcısına IoT Edge paketini içeri aktarın](iot-accelerators-remote-monitoring-import-edge-package.md)

IoT Edge çalışma zamanını yükleme hakkında daha fazla bilgi için bkz. [Linux üzerinde Azure IoT Edge çalışma zamanını yükleme (x64)](../iot-edge/how-to-install-iot-edge-linux.md).

Uç cihazlarda Azure Stream Analytics hakkında daha fazla bilgi edinmek için bkz. [Azure Stream Analytics IoT Edge modül olarak dağıtma](../iot-edge/tutorial-deploy-stream-analytics.md).
