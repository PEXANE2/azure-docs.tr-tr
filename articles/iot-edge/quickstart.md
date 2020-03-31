---
title: Windows'da Azure IoT Edge aygıtı oluşturmaya hızlı başlangıç | Microsoft Dokümanlar
description: Bu hızlı başlangıçta, bir IoT Edge aygıtının nasıl oluşturulup azure portalından uzaktan önceden oluşturulmuş kodu dağıtılamayı öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/06/2019
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 54efe7b5c392ad2b4cc3a0de414e04951b268508
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78674237"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-virtual-windows-device"></a>Hızlı başlatma: İlk IoT Edge modülünüzü sanal bir Windows aygıtına dağıtma

Kapsayıcı kodu sanal bir IoT Edge aygıtına dağıtarak Azure IoT Edge'i deneyin. IoT Edge, iş yüklerinizden daha fazlasını kenara gönderebilmeniz için aygıtlarınızdaki kodları uzaktan yönetmenize olanak tanır. Bu hızlı başlangıç için, IoT Edge aygıtınız için bir Azure sanal makinesi kullanmanızı öneririz. Sanal bir makine kullanmak, hızlı bir şekilde bir test makinesi oluşturmanıza, ön koşulları yüklemenize ve bittiğinde silmenize olanak tanır.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT hub'ı oluşturun.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma süresini sanal cihazınıza yükleyin ve başlatın.
4. IoT Edge cihazına uzaktan modül dağıtma ve IoT Hub'a telemetri verileri gönderme.

![Diyagram - Aygıt ve bulut için Quickstart mimarisi](./media/quickstart/install-edge-full.png)

Bu hızlı başlatma, windows sanal makine oluşturma ve ioT Edge aygıtı olarak yapılandırma konusunda size yol alar. Bu işlemin ardından Azure portalından cihazınıza bir modül dağıtabilirsiniz. Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıçtaki adımların çoğunu tamamlamak için Azure CLI'yi kullanırsınız. Azure IoT'nin ek işlevselliği etkinleştirmek için bir uzantısı vardır.

Azure IoT uzantısını cloud shell örneğine ekleyin.

   ```azurecli-interactive
   az extension add --name azure-iot
   ```

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

## <a name="prerequisites"></a>Ön koşullar

Bulut kaynakları:

* Bu hızlı başlangıçta kullandığınız tüm kaynakları yönetmek için kullanacağınız bir kaynak grubu.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

IoT Edge cihazı:

* IoT Edge aygıtınız olarak hareket etmek için bir Windows sanal makine. Bu sanal makineyi aşağıdaki komutu kullanarak `{password}` güvenli bir parolayla değiştirerek oluşturabilirsiniz:

  ```azurecli-interactive
  az vm create --resource-group IoTEdgeResources --name EdgeVM --image MicrosoftWindowsDesktop:Windows-10:rs5-pro:latest --admin-username azureuser --admin-password {password} --size Standard_DS1_v2
  ```

  Yeni sanal makinenin oluşturulup başlatılması birkaç dakika sürebilir. Daha sonra sanal makinenize bağlanırken kullanılmak üzere bir RDP dosyası indirebilirsiniz:

  1. Azure portalındaki yeni Windows sanal makinenize gidin.
  1. **Bağlan**’ı seçin.
  1. **RDP** sekmesinde **RDP Dosyasını İndir'i**seçin.

  Komutla belirttiğiniz yönetici adını ve parolasını kullanarak Windows sanal makinenize `az vm create` bağlanmak için Uzak Masaüstü Bağlantısı ile bu dosyayı açın.

> [!NOTE]
> Bu hızlı başlatma, basitlik için bir Windows masaüstü sanal makine kullanır. Hangi Windows işletim sistemlerinin genellikle üretim senaryoları için kullanılabilir olduğu hakkında bilgi için Azure [IoT Edge destekli sistemlere](support.md)bakın.
>
> IoT Core çalıştıran aygıtlar da dahil olmak üzere IoT Edge için kendi Windows aygıtınızı yapılandırmaya hazırsanız, [Windows'ta Azure IoT Edge çalışma süresini yükleyin](how-to-install-iot-edge-windows.md)adımlarını izleyin.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Azure CLI ile bir IoT hub'ı oluşturarak hızlı başlatın.

![Diyagram - Bulutta bir IoT hub'ı oluşturma](./media/quickstart/create-iot-hub.png)

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir.

Aşağıdaki kod kaynak grubunda `IoTEdgeResources`ücretsiz bir **F1** hub'ı oluşturur. IoT hub'ınız için benzersiz bir adla değiştirin. `{hub_name}`

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. IoT Hub adının kullanılamadığıyla ilgili bir hata alırsanız, başka birinin bu ada sahip bir hub'ı zaten olduğu anlamına gelir. Yeni bir isim dene.

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazını kaydetme

Yeni oluşturulan IoT hub'ına bir IoT Edge cihazı kaydedin.
![Diyagram - IoT Hub kimliğine sahip bir aygıt kaydettirme](./media/quickstart/register-device.png)

IoT hub'ınızla iletişim kurabilmesi amacıyla simülasyon cihazınız için bir cihaz kimliği oluşturun. Cihaz kimliği bulutta kalır ve fiziksel cihazla cihaz kimliği arasında bağlantı kurmak için benzersiz bir bağlantı dizesi kullanılır.

IoT Edge aygıtları normal IoT aygıtlarına göre farklı şekilde davranabildiği ve yönetilebildiği `--edge-enabled` için, bu kimliği bayraklı bir IoT Edge aygıtı için beyan edin.

1. Azure Cloud Shell'de aşağıdaki komutu girerek hub'ınızda **myEdgeDevice** adlı bir cihaz oluşturun.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

   iothubowner ilke anahtarları hakkında bir hata alırsanız, bulut bulutunuzun azure-iot uzantısının en son sürümünü çalıştırdığından emin olun.

2. Fiziksel cihazınızla IoT Hub'daki kimliği arasında bağlantı oluşturan cihaz bağlantı dizesini alın.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. `connectionString` Anahtarın değerini JSON çıkışından kopyalayın ve kaydedin. Bu değer, cihaz bağlantı dizesidir. Bu bağlantı dizesini bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız.

   ![CLI çıkışından bağlantı dizelerini alma](./media/quickstart/retrieve-connection-string.png)

## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını IoT Edge cihazınıza yükleyin ve cihaz bağlantı dizesiyle yapılandırın.
![Diyagram - Cihazda çalışma süresini başlatın](./media/quickstart/start-runtime.png)

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemonu, ioT** Edge aracısını başlatarak aygıtı her başlatınve önyüklemede bir IoT Edge aygıtı başlatın. **IoT Edge aracısı,** IoT Edge hub'ı da dahil olmak üzere IoT Edge aygıtındaki modüllerin dağıtımını ve izlenmesini yönetir. **IoT Edge hub'ı, IoT** Edge aygıtındaki modüller ile aygıt ve IoT Hub arasındaki iletişimi yönetir.

Yükleme komut dosyası, IoT Edge aygıtınızdaki kapsayıcı görüntülerini yöneten Moby adında bir kapsayıcı motoru da içerir.

Çalışma zamanı yüklemesi sırasında cihaz bağlantı dizesi istenir. Azure CLI'den aldığınız dizeyi kullanın. Bu dize, fiziksel cihazınızı Azure'daki IoT Edge cihaz kimliğiyle ilişkilendirir.

### <a name="connect-to-your-iot-edge-device"></a>IoT Edge cihazınıza bağlanın

Bu bölümdeki adımların tümü IoT Edge aygıtınızda yer alır, bu nedenle artık uzak masaüstü üzerinden sanal makineye bağlanmak istersiniz.

### <a name="install-and-configure-the-iot-edge-service"></a>IoT Edge hizmetini yükleme ve yapılandırma

PowerShell'i kullanarak IoT Edge çalışma zamanını indirin ve yükleyin. Cihazınızı yapılandırmak için IoT Hub'dan aldığınız cihaz bağlantı dizesini kullanın.

1. Henüz yapmadıysanız, cihazınızı kaydetmek ve aygıt bağlantı dizesini almak için [yeni bir Azure IoT Edge aygıtı kaydet'teki](how-to-register-device.md) adımları izleyin.

2. Sanal makinede PowerShell'i yönetici olarak çalıştırın.

   >[!NOTE]
   >IoT Edge'i yüklemek için POWERShell'in AMD64 oturumunu kullanın, PowerShell'i (x86) değil. Hangi oturum türünü kullandığınızdan emin değilseniz aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Deploy-IoTEdge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini açar, Moby çalışma süresini indirir ve ardından IoT Edge çalışma süresini indirir.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Windows
   ```

4. Makineniz otomatik olarak yeniden başlatılabilir. Yeniden başlatmanız için Deploy-IoTEdge komutu tarafından istenirse, bunu şimdi yapın.

5. PowerShell'i yeniden yönetici olarak çalıştırın.

6. **Initialize-IoTEdge komutu,** Makinenizdeki IoT Edge çalışma süresini yapılandırır. Komut, Windows kapsayıcılarıyla el ile sağlama için varsayılandır.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Initialize-IoTEdge -ContainerOs Windows
   ```

7. **DeviceConnectionString** istendiğinde önceki bölümde kopyaladığınız dizeyi iletin. Bağlantı dizesinin etrafındaki tırnak işaretlerini dahil etmeyin.

### <a name="view-the-iot-edge-runtime-status"></a>IoT Edge çalışma zamanı durumunu görüntüleme

Çalışma zamanının başarıyla yüklendiğinden ve yapılandırıldığından emin olun.

1. IoT Edge hizmetinin durumunu kontrol edin.

   ```powershell
   Get-Service iotedge
   ```

2. Hizmetle ilgili sorunları gidermeniz gerekirse hizmet günlüklerini alın.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; Get-IoTEdgeLog
   ```

3. IoT Edge cihazınızda çalışan tüm modülleri görüntüleyin. Hizmet ilk kez başlatıldığı için yalnızca **edgeAgent** modülünün çalıştığını göreceksiniz. edgeAgent modülü varsayılan olarak çalışır ve cihazınıza dağıttığınız ek modülleri yüklemenize ve başlatmaya yardımcı olur.

    ```powershell
    iotedge list
    ```

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart/iotedge-list-1.png)

Yüklemenin tamamlanması ve IoT Edge aracı modülünün başlaması birkaç dakika sürebilir.

IoT Edge cihazınız yapılandırıldı. Bulutta dağıtılan modülleri çalıştırmak için hazır.

## <a name="deploy-a-module"></a>Modül dağıtma

IoT Hub'a telemetri verilerini gönderen bir modülü dağıtmak için Azure IoT Edge cihazınızı buluttan yönetin.
![Diyagram - modülü buluttan cihaza dağıtma](./media/quickstart/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, bir IoT Edge aygıtı kaydettiniz ve IoT Edge çalışma süresini yükledin. Ardından, aygıtın kendisinde değişiklik yapmak zorunda kalmadan aygıtta çalışmak üzere bir IoT Edge modülü dağıtmak için Azure portalını kullandınız.

Bu durumda, ittiğiniz modül, sınama için kullanabileceğiniz örnek veriler oluşturur. Simüle edilen sıcaklık sensörü modülü, daha sonra test etmek için kullanabileceğiniz ortam verileri oluşturur. Simüle sensör hem bir makineyi hem de makinenin etrafındaki çevreyi izliyor. Örneğin bu sensör bir sunucu odasında, fabrika sahasında veya rüzgar türbininde olabilir. İletide ortam sıcaklığı ve nemi, makine sıcaklığı ve basıncı ile bir zaman damgası bulunur. IoT Edge öğreticileri, bu modül tarafından oluşturulan verileri analiz için test verileri olarak kullanır.

Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun.

```powershell
iotedge list
```

   ![Cihazınızda üç modül görüntüleme](./media/quickstart/iotedge-list-2.png)

Sıcaklık sensörü modülünden buluta gönderilen iletileri görüntüleyin.

```powershell
iotedge logs SimulatedTemperatureSensor -f
```

   >[!TIP]
   >IoT Edge komutları modül adlarına atıfta bulunulurken büyük/küçük harf duyarlıdır.

   ![Verileri modülünüzden görüntüleme](./media/quickstart/iotedge-logs.png)

[Visual Studio Code için Azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak iletilerin IoT hub'ınıza ulaşmasını da izleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Edge öğreticilerine devam etmek istiyorsanız bu hızlı başlangıçta kaydettiğiniz ve ayarladığınız cihazı kullanabilirsiniz. Aksi takdirde, ücretlendirmeleri önlemek için oluşturduğunuz Azure kaynaklarını silebilirsiniz.

Sanal makinenizi ve IoT hub’ınızı yeni bir kaynak grubunda oluşturduysanız, bu grubu ve ilişkili tüm kaynaklarını silebilirsiniz. Saklamak istediğiniz hiçbir şey olmadığından emin olmak için kaynak grubunun içeriğini iki kez kontrol edin. Tüm grubu silmek istemiyorsanız, bunun yerine tek tek kaynakları silebilirsiniz.

**IoTEdgeResources** grubunu kaldırın.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlatmada, bir IoT Edge aygıtı oluşturdunuz ve kodu aygıta dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten bir test cihazınız var.

Bir sonraki adım, iş mantığınızı çalıştıran IoT Edge modülleri oluşturmaya başlayabilmeniz için yerel geliştirme ortamınızı ayarlamaktır.

> [!div class="nextstepaction"]
> [Windows aygıtları için IoT Edge modülleri geliştirmeye başlayın](tutorial-develop-for-windows.md)
