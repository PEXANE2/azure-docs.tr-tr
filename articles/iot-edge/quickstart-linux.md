---
title: Hızlı başlangıç Linux üzerinde Azure IoT Edge cihaz oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta, IoT Edge bir cihaz oluşturmayı ve ardından Azure portal önceden oluşturulmuş kodu uzaktan dağıtmayı öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/06/2019
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 52258a8bc287df36158ec143e4aad74c34455ea4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80236083"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-virtual-linux-device"></a>Hızlı başlangıç: ilk IoT Edge modülünüzü bir sanal Linux cihazına dağıtma

Kapsayıcılı kodu bir sanal IoT Edge cihazına dağıtarak bu hızlı başlangıçta Azure IoT Edge test edin. IoT Edge, iş yüklerinizi kenarda daha fazla gönderebilmeniz için cihazlarınızda kodu uzaktan yönetmenizi sağlar. Bu hızlı başlangıçta, IoT Edge cihazınız için bir Azure sanal makinesi kullanmanızı öneririz. Bu, tüm önkoşulların yüklü olduğu bir test makinesini hızlıca oluşturmanızı ve sonra işiniz bittiğinde silmenizi sağlar.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. Bir IoT Hub oluşturma.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma zamanını sanal cihazınıza yükleyip başlatın.
4. Bir IoT Edge cihazına uzaktan modül dağıtma.

![Diyagram-cihaz ve bulut için hızlı başlangıç mimarisi](./media/quickstart-linux/install-edge-full.png)

Bu hızlı başlangıçta, IoT Edge cihaz olarak yapılandırılmış bir Linux sanal makinesi oluşturma işlemi adım adım açıklanmaktadır. Bu işlemin ardından Azure portalından cihazınıza bir modül dağıtabilirsiniz. Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır.

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

* IoT Edge cihazınız olacak bir Linux cihazı veya sanal makinesi. [Ubuntu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft_iot_edge.iot_edge_vm_ubuntu) sanal makinesinde Microsoft tarafından sağlanmış Azure IoT Edge kullanmanız gerekir ve bu, bir cihazda IoT Edge çalıştırmak için ihtiyaç duyduğunuz her şeyi önceden kurar. Kullanım koşullarını kabul edin ve aşağıdaki komutları kullanarak bu sanal makineyi oluşturun:

   ```azurecli-interactive
   az vm image terms accept --urn microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest
   az vm create --resource-group IoTEdgeResources --name EdgeVM --image microsoft_iot_edge:iot_edge_vm_ubuntu:ubuntu_1604_edgeruntimeonly:latest --admin-username azureuser --generate-ssh-keys
   ```

   Yeni sanal makinenin oluşturulup başlatılması birkaç dakika sürebilir.

   Yeni bir sanal makine oluşturduğunuzda, Create komutu çıkışının bir parçası olarak belirtilen **Publicıpaddress**' i bir yere getirin. Bu genel IP adresini, daha sonra hızlı başlangıçta sanal makineye bağlanmak için kullanacaksınız.

* Azure IoT Edge çalışma zamanını kendi cihazınızda çalıştırmayı tercih ediyorsanız, [Linux üzerinde Azure IoT Edge çalışma zamanını yüklemek](how-to-install-iot-edge-linux.md)için yönergeleri izleyin.

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

Azure CLı ile IoT Hub 'ı oluşturarak hızlı başlangıcı başlatın.

![Diyagram-bulutta IoT Hub 'ı oluşturma](./media/quickstart-linux/create-iot-hub.png)

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir.

Aşağıdaki kod, **IoTEdgeResources** kaynak grubunda ücretsiz bir **F1** hub’ı oluşturur. IoT `{hub_name}` hub 'ınız için benzersiz bir adla değiştirin.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. IoT Hub adının kullanılamadığı bir hata alırsanız, başka birinin zaten bu adı taşıyan bir hub 'ı olduğu anlamına gelir. Yeni bir ad deneyin.

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT hub'ına bir IoT Edge cihazı kaydedin.

![Diyagram-IoT Hub kimliği olan bir cihazı kaydetme](./media/quickstart-linux/register-device.png)

IoT Edge cihazınız için, IoT Hub 'ınız ile iletişim kurabilmesi için bir cihaz kimliği oluşturun. Cihaz kimliği bulutta kalır ve fiziksel cihazla cihaz kimliği arasında bağlantı kurmak için benzersiz bir bağlantı dizesi kullanılır.

IoT Edge cihazlar sıradan IoT cihazlarından farklı şekilde yönetilemediğinden, bu kimliği `--edge-enabled` bayrağıyla IoT Edge bir cihaz için olacak şekilde bildirin.

1. Azure Cloud Shell'de aşağıdaki komutu girerek hub'ınızda **myEdgeDevice** adlı bir cihaz oluşturun.

   ```azurecli-interactive
   az iot hub device-identity create --hub-name {hub_name} --device-id myEdgeDevice --edge-enabled
   ```

   iothubowner ilke anahtarlarıyla ilgili bir hata alırsanız Cloud Shell ortamınızda azure-cli-iot-ext uzantısının son sürümünün çalıştığından emin olun.

2. Fiziksel cihazınızla IoT Hub'daki kimliği arasında bağlantı oluşturan cihaz bağlantı dizesini alın.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. JSON çıktısından `connectionString` anahtarın değerini kopyalayın ve kaydedin. Bu değer, cihaz bağlantı dizesidir. Bu bağlantı dizesini bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız.

   ![CLı çıktısından bağlantı dizesini al](./media/quickstart/retrieve-connection-string.png)

## <a name="configure-your-iot-edge-device"></a>IoT Edge cihazınızı yapılandırma

IoT Edge cihazınızda Azure IoT Edge çalışma zamanını başlatın.

![Diyagram-çalışma zamanını cihazda Başlat](./media/quickstart-linux/start-runtime.png)

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik arka plan programı** , IoT Edge aracısını başlatarak IoT Edge bir cihaz her önyüklendiğinde ve cihazı önyükleme başlatılır. **IoT Edge aracısı**, IoT Edge hub'ı dahil olmak üzere IoT Edge cihazındaki modüllerin dağıtımını ve izlenmesini kolaylaştırır. **IoT Edge hub'ı** IoT Edge cihazındaki modüller ve cihaz ile IoT Hub'ı arasındaki iletişimi yönetir.

Çalışma zamanı yapılandırması sırasında cihaz bağlantı dizesi sağlamanız gerekir. Azure CLI'den aldığınız dizeyi kullanın. Bu dize, fiziksel cihazınızı Azure'daki IoT Edge cihaz kimliğiyle ilişkilendirir.

### <a name="set-the-connection-string-on-the-iot-edge-device"></a>IoT Edge cihazında bağlantı dizesini ayarlama

Önkoşul bölümünde açıklandığı gibi Ubuntu sanal makinesinde Azure IoT Edge kullanıyorsanız, cihazınızda IoT Edge çalışma zamanı zaten yüklüdür. Cihazınızı, önceki bölümde aldığınız cihaz bağlantı dizesi ile yapılandırmanız yeterlidir. Bu işlemi uzaktan, sanal makineye bağlanmadan gerçekleştirebilirsiniz. Aşağıdaki komutu çalıştırarak kendi dizeniz `{device_connection_string}` ile değiştirin.

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunShellScript --script "/etc/iotedge/configedge.sh '{device_connection_string}'"
   ```

Yerel makinenizde veya bir ARM32 ya da ARM64 cihazında IoT Edge çalıştırıyorsanız, IoT Edge çalışma zamanını ve önkoşul olan önkoşulları cihazınıza yüklemeniz gerekir. [Linux 'ta Azure IoT Edge çalışma zamanını yüklemek](how-to-install-iot-edge-linux.md)ve ardından bu hızlı başlangıca geri dönmek için yönergeleri izleyin.

### <a name="view-the-iot-edge-runtime-status"></a>IoT Edge çalışma zamanı durumunu görüntüleme

Bu hızlı başlangıçtaki komutların geri kalanı IoT Edge cihazınızın kendisinde gerçekleşirken cihazda neler olduğunu görmeniz için. Bir sanal makine kullanıyorsanız, oluşturma komutu tarafından çıktı olan genel IP adresini kullanarak o makineye şimdi bağlanın. Ayrıca, Azure portal sanal makinenizin Genel Bakış sayfasında genel IP adresini de bulabilirsiniz. Sanal makinenize bağlanmak için aşağıdaki komutu kullanın. Önkoşullarda önerilenden farklı bir Kullanıcı adı kullandıysanız değiştirin `{azureuser}` . Makinenizin `{publicIpAddress}` adresiyle değiştirin.

   ```console
   ssh azureuser@{publicIpAddress}
   ```

Çalışma zamanının IoT Edge cihazınıza başarıyla yüklenip yapılandırıldığını doğrulayın.

>[!TIP]
>`iotedge` komutlarını çalıştırmak için yükseltilmiş ayrıcalıklara ihtiyacınız olacaktır. Makinenizdeki oturumu kapattıktan sonra IoT Edge çalışma zamanını yükleyip oturum açtığınızda izinleriniz otomatik olarak güncelleştirilir. Bundan sonra komutların önünde `sudo` kullanın.

1. IoT Edge güvenlik arka plan programının sistem hizmeti olarak çalışıp çalışmadığını denetleyin.

   ```bash
   sudo systemctl status iotedge
   ```

   ![Sistem hizmeti olarak çalışan IoT Edge Daemon 'a bakın](./media/quickstart-linux/iotedged-running.png)

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
![Diyagram-modülü buluttan cihaza dağıt](./media/quickstart-linux/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, yeni bir IoT Edge cihazı oluşturdunuz ve üzerine IoT Edge çalışma zamanını yüklediniz. Ardından, cihazın kendisinde değişiklik yapmak zorunda kalmadan cihazda çalıştırmak üzere bir IoT Edge modülünü dağıtmak için Azure portal kullandınız.

Bu durumda, gönderdiğiniz modül test için kullanabileceğiniz örnek veriler oluşturur. Sanal sıcaklık algılayıcı modülü, daha sonra test için kullanabileceğiniz ortam verileri oluşturur. Sanal algılayıcı hem makinenin hem de makinenin bulunduğu ortamın izlenmesini izler. Örneğin bu sensör bir sunucu odasında, fabrika sahasında veya rüzgar türbininde olabilir. İletide ortam sıcaklığı ve nemi, makine sıcaklığı ve basıncı ile bir zaman damgası bulunur. IoT Edge öğreticileri, bu modülle oluşturulan verileri analiz için test verileri olarak kullanır.

IoT Edge cihazınızda komut istemini yeniden açın veya Azure CLI'den SSH bağlantısı kurun. Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun:

   ```bash
   sudo iotedge list
   ```

   ![Cihazınızda üç modül görüntüleme](./media/quickstart-linux/iotedge-list-2.png)

Sıcaklık Algılayıcı modülünden gönderilen iletileri görüntüleyin:

   ```bash
   sudo iotedge logs SimulatedTemperatureSensor -f
   ```

   >[!TIP]
   >IoT Edge komutlar modül adlarına başvururken büyük/küçük harfe duyarlıdır.

   ![Verileri modülünüzden görüntüleme](./media/quickstart-linux/iotedge-logs.png)

Ayrıca, [Visual Studio Code Için Azure IoT Hub uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak IoT Hub 'ınıza gelen iletileri izleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Edge öğreticilerine devam etmek istiyorsanız bu hızlı başlangıçta kaydettiğiniz ve ayarladığınız cihazı kullanabilirsiniz. Aksi takdirde, ücretlendirmemek için oluşturduğunuz Azure kaynaklarını silebilirsiniz.

Sanal makinenizi ve IoT hub’ınızı yeni bir kaynak grubunda oluşturduysanız, bu grubu ve ilişkili tüm kaynaklarını silebilirsiniz. Tutmak istediğiniz bir şey olduğundan emin olmak için kaynak grubunun içeriğini iki kez kontrol edin. Tüm grubu silmek istemiyorsanız, bunun yerine tek tek kaynakları silebilirsiniz.

**IoTEdgeResources** grubunu kaldırın.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Edge cihaz oluşturdunuz ve cihaza kod dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten bir test cihazınız var.

Bir sonraki adım, iş mantığınızı çalıştıran IoT Edge modüller oluşturmaya başlayabilmeniz için yerel geliştirme ortamınızı ayarlamanıza olanak sağlar.

> [!div class="nextstepaction"]
> [Linux cihazları için IoT Edge modülleri geliştirmeye başlama](tutorial-develop-for-linux.md)
