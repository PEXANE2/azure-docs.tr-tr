---
title: Quickstart Linux'ta bir Azure IoT Edge cihazı oluşturmak | Microsoft Dokümanlar
description: Bu hızlı başlangıçta, bir IoT Edge aygıtının nasıl oluşturulup azure portalından uzaktan önceden oluşturulmuş kodu dağıtılamayı öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/06/2019
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 52258a8bc287df36158ec143e4aad74c34455ea4
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80236083"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-virtual-linux-device"></a>Quickstart: İlk IoT Edge modülünüzü sanal bir Linux cihazına dağıtın

Kapsayıcı kodu sanal bir IoT Edge aygıtına dağıtarak azure IoT Edge'i bu hızlı başlatmada test edin. IoT Edge, iş yüklerinizden daha fazlasını kenara gönderebilmeniz için aygıtlarınızdaki kodları uzaktan yönetmenize olanak tanır. Bu hızlı başlangıç için, IoT Edge aygıtınız için hızlı bir şekilde tüm ön koşullar yüklü bir test makinesi oluşturmanıza ve işiniz bittiğinde silmenize olanak tanıyan bir Azure sanal makinesi kullanmanızı öneririz.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT Hub oluşturma.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma süresini sanal cihazınıza yükleyin ve başlatın.
4. Bir IoT Edge cihazına uzaktan modül dağıtma.

![Diyagram - Aygıt ve bulut için Quickstart mimarisi](./media/quickstart-linux/install-edge-full.png)

Bu hızlı başlangıç, IoT Edge aygıtı olarak yapılandırılan bir Linux sanal makine oluşturmada size yol alar. Bu işlemin ardından Azure portalından cihazınıza bir modül dağıtabilirsiniz. Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıçtaki birçok adımı tamamlamak için Azure CLI kullanacaksınız. Azure IoT de ek işlevleri etkinleştirmek için bir uzantıya sahiptir.

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

* IoT Edge cihazınız olacak bir Linux cihazı veya sanal makinesi. IoT Edge'i bir aygıtta çalıştırmak için ihtiyacınız olan her şeyi önceden yükleyen [Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu) sanal makinede Microsoft tarafından sağlanan Azure IoT Edge'i kullanmalısınız. Kullanım koşullarını kabul edin ve aşağıdaki komutları kullanarak bu sanal makineyi oluşturun:

   ```azurecli-interactive
   az vm image terms accept --urn microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys
   ```

   Yeni sanal makinenin oluşturulup başlatılması birkaç dakika sürebilir.

   Yeni bir sanal makine oluşturduğunuzda, create komut çıktısının bir parçası olarak sağlanan **publicIpAddress**'e not alın. Bu genel IP adresini daha sonra hızlı bir şekilde sanal makineye bağlanmak için kullanırsınız.

* Azure IoT Edge çalışma süresini kendi cihazınızda çalıştırmayı tercih ediyorsanız, [Azure IoT Edge çalışma saatini Linux'ta yükleyin](how-to-install-iot-edge-linux.md)yönergeleri izleyin.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Azure CLI ile bir IoT hub'ı oluşturarak hızlı başlatın.

![Diyagram - Bulutta bir IoT hub'ı oluşturma](./media/quickstart-linux/create-iot-hub.png)

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir.

Aşağıdaki kod, **IoTEdgeResources** kaynak grubunda ücretsiz bir **F1** hub’ı oluşturur. IoT hub'ınız için benzersiz bir adla değiştirin. `{hub_name}`

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. IoT Hub adının kullanılamadığıyla ilgili bir hata alırsanız, başka birinin bu ada sahip bir hub'ı zaten olduğu anlamına gelir. Yeni bir isim dene.

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazını kaydetme

Yeni oluşturulan IoT hub'ına bir IoT Edge cihazı kaydedin.

![Diyagram - IoT Hub kimliğine sahip bir aygıt kaydettirme](./media/quickstart-linux/register-device.png)

IoT edge aygıtınız için bir aygıt kimliği oluşturun, böylece IoT hub'ınızla iletişim kurabilir. Cihaz kimliği bulutta kalır ve fiziksel cihazla cihaz kimliği arasında bağlantı kurmak için benzersiz bir bağlantı dizesi kullanılır.

IoT Edge aygıtları normal IoT aygıtlarına göre farklı şekilde davranabildiği ve yönetilebildiği `--edge-enabled` için, bu kimliği bayraklı bir IoT Edge aygıtı için beyan edin.

1. Azure Cloud Shell'de aşağıdaki komutu girerek hub'ınızda **myEdgeDevice** adlı bir cihaz oluşturun.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

   iothubowner ilke anahtarlarıyla ilgili bir hata alırsanız Cloud Shell ortamınızda azure-cli-iot-ext uzantısının son sürümünün çalıştığından emin olun.

2. Fiziksel cihazınızla IoT Hub'daki kimliği arasında bağlantı oluşturan cihaz bağlantı dizesini alın.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. `connectionString` Anahtarın değerini JSON çıkışından kopyalayın ve kaydedin. Bu değer, cihaz bağlantı dizesidir. Bu bağlantı dizesini bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız.

   ![CLI çıkışından bağlantı dizelerini alma](./media/quickstart/retrieve-connection-string.png)

## <a name="configure-your-iot-edge-device"></a>IoT Edge cihazınızı yapılandırın

IoT Edge cihazınızda Azure IoT Edge çalışma süresini başlatın.

![Diyagram - Cihazda çalışma süresini başlatın](./media/quickstart-linux/start-runtime.png)

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik daemonu, ioT** Edge aracısını başlatarak aygıtı her başlatınve önyüklemede bir IoT Edge aygıtı başlatın. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Çalışma zamanı yapılandırması sırasında cihaz bağlantı dizesi sağlamanız gerekir. Azure CLI'den aldığınız dizeyi kullanın. Bu dize, fiziksel cihazınızı Azure'daki IoT Edge cihaz kimliğiyle ilişkilendirir.

### <a name="set-the-connection-string-on-the-iot-edge-device"></a>IoT Edge aygıtındaki bağlantı dizesini ayarlama

Ön koşullarda açıklandığı gibi Ubuntu sanal makinesindeki Azure IoT Edge'i kullanıyorsanız, aygıtınız zaten IoT Edge çalışma zamanı yüklü. Cihazınızı bir önceki bölümde aldığınız aygıt bağlantı dizesiyle yapılandırmanız gerekir. Bu işlemi uzaktan, sanal makineye bağlanmadan gerçekleştirebilirsiniz. Kendi dizenizi değiştirerek `{device_connection_string}` aşağıdaki komutu çalıştırın.

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunShellScript --script "/etc/iotedge/configedge.sh '{device_connection_string}'"
   ```

IoT Edge'i yerel makinenizde veya ARM32 veya ARM64 aygıtınızda çalıştırıyorsanız, IoT Edge çalışma süresini ve ön koşulları cihazınıza yüklemeniz gerekir. Linux'ta [Azure IoT Edge çalışma saatini yükleyin](how-to-install-iot-edge-linux.md)yönergeleri izleyin ve ardından bu hızlı başlatmaya geri dönün.

### <a name="view-the-iot-edge-runtime-status"></a>IoT Edge çalışma zamanı durumunu görüntüleme

Bu hızlı başlatmadaki komutların geri kalanı IoT Edge cihazınızın kendisinde yer alır, böylece cihazda neler olduğunu görebilirsiniz. Sanal bir makine kullanıyorsanız, oluşturma komutu tarafından çıktı edilen genel IP adresini kullanarak o makineye şimdi bağlanın. Azure portalında sanal makinenizin genel bakış sayfasında genel IP adresini de bulabilirsiniz. Sanal makinenize bağlanmak için aşağıdaki komutu kullanın. Önkoşullarda önerilenden farklı bir kullanıcı adı kullandıysanız değiştirin. `{azureuser}` Makinenizin adresiyle değiştirin. `{publicIpAddress}`

   ```console
   ssh azureuser@{publicIpAddress}
   ```

Çalışma zamanının IoT Edge aygıtınıza başarıyla yüklendiğinden ve yapılandırıldığını doğrulayın.

>[!TIP]
>`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Makinenizdeki oturumu kapattıktan sonra IoT Edge çalışma zamanını yükleyip oturum açtığınızda izinleriniz otomatik olarak güncelleştirilir. O zamana `sudo` kadar, komutların önünde kullanın.

1. IoT Edge güvenlik daemon bir sistem hizmeti olarak çalışıyor olup olmadığını kontrol edin.

   ```bash
   sudo systemctl status iotedge
   ```

   ![IoT Edge daemon'un bir sistem hizmeti olarak çalışmasını görün](./media/quickstart-linux/iotedged-running.png)

2. Hizmetle ilgili sorunları gidermeniz gerekirse hizmet günlüklerini alın.

   ```bash
   journalctl -u iotedge
   ```

3. Cihazınızda çalışan modülleri görüntüleyin.

   ```bash
   sudo iotedge list
   ```

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart-linux/iotedge-list-1.png)

IoT Edge cihazınız yapılandırıldı. Bulutta dağıtılan modülleri çalıştırmak için hazır.

## <a name="deploy-a-module"></a>Modül dağıtma

Azure IoT Edge cihazınızı, IoT Hub'ına telemetri verileri gönderecek bir modül dağıtmak için buluttan yönetin.
![Diyagram - modülü buluttan cihaza dağıtma](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, aygıtın kendisinde değişiklik yapmak zorunda kalmadan aygıtta çalışmak üzere bir IoT Edge modülü dağıtmak için Azure portalını kullandınız.

Bu durumda, ittiğiniz modül, sınama için kullanabileceğiniz örnek veriler oluşturur. Simüle edilen sıcaklık sensörü modülü, daha sonra test etmek için kullanabileceğiniz ortam verileri oluşturur. Simüle sensör hem bir makineyi hem de makinenin etrafındaki çevreyi izliyor. Örneğin bu sensör bir sunucu odasında, fabrika sahasında veya rüzgar türbininde olabilir. İletide ortam sıcaklığı ve nemi, makine sıcaklığı ve basıncı ile bir zaman damgası bulunur. IoT Edge öğreticileri, bu modül tarafından oluşturulan verileri analiz için test verileri olarak kullanır.

IoT Edge cihazınızda komut istemini yeniden açın veya Azure CLI'den SSH bağlantısı kurun. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun:

   ```bash
   sudo iotedge list
   ```

   ![Cihazınızda üç modül görüntüleme](./media/quickstart-linux/iotedge-list-2.png)

Sıcaklık sensörü modülünden gönderilen iletileri görüntüleyin:

   ```bash
   sudo iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >IoT Edge komutları modül adlarına atıfta bulunulurken büyük/küçük harf duyarlıdır.

   ![Verileri modülünüzden görüntüleme](./media/quickstart-linux/iotedge-logs.png)

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
> [Linux cihazları için IoT Edge modülleri geliştirmeye başlayın](tutorial-develop-for-linux.md)
