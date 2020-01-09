---
title: Hızlı Başlangıç, Windows üzerinde Azure IOT Edge cihazı oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta bir IOT Edge cihazı oluşturma ve ardından önceden oluşturulmuş kod Azure Portalı'ndan uzaktan dağıtma öğrenin.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/06/2019
ms.topic: quickstart
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: ab3805e39112d4d37635571d8aa43030a1896951
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552449"
---
# <a name="quickstart-deploy-your-first-iot-edge-module-to-a-virtual-windows-device"></a>Hızlı başlangıç: ilk IoT Edge modülünüzü sanal bir Windows cihazına dağıtma

Kapsayıcılı kodu bir sanal IoT Edge cihazına dağıtarak bu hızlı başlangıçta Azure IoT Edge test edin. IoT Edge, iş yüklerinizi kenarda daha fazla gönderebilmeniz için cihazlarınızda kodu uzaktan yönetmenizi sağlar. Bu hızlı başlangıçta, IoT Edge cihazınız için bir Azure sanal makinesi kullanmanızı öneririz. Bu, hızlı bir şekilde bir test makinesi oluşturmanıza, önkoşulları yüklemenize ve sonra işiniz bittiğinde silmeniz sağlar.

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

1. IoT Hub 'ı oluşturun.
2. Bir IoT Edge cihazını IoT hub'ınıza kaydetme.
3. IoT Edge çalışma zamanını sanal cihazınıza yükleyip başlatın.
4. IoT Edge cihazına uzaktan modül dağıtma ve IoT Hub'a telemetri verileri gönderme.

![Diyagram - cihaz ve buluta yönelik hızlı başlangıç mimarisi](./media/quickstart/install-edge-full.png)

Bu hızlı başlangıç, bir Windows sanal makinesi oluşturma ve bunu IoT Edge bir cihaz olarak yapılandırma konusunda size yol gösterir. Bu işlemin ardından Azure portalından cihazınıza bir modül dağıtabilirsiniz. Bu hızlı başlangıçta oluşturduğunuz modül; sıcaklık, nem ve basınç verileri üreten bir sensör simülasyonudur. Diğer Azure IoT Edge öğreticileri, burada iş içgörüsü için simülasyon verilerini analiz eden modüller dağıtarak yaptığınız çalışmayı temel alır.

Etkin bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu hızlı başlangıçtaki adımların çoğunu gerçekleştirmek için Azure CLı 'yi kullanırsınız. Azure IoT, ek işlevselliği etkinleştirmek için bir uzantıya sahiptir.

Azure IoT uzantısını cloud shell örneğine ekleyin.

   ```azurecli-interactive
   az extension add --name azure-cli-iot-ext
   ```

## <a name="prerequisites"></a>Ön koşullar

Bulut kaynakları:

* Bu hızlı başlangıçta kullandığınız tüm kaynakları yönetmek için kullanacağınız bir kaynak grubu.

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2 
   ```

IoT Edge cihazı:

* IoT Edge cihazınız olarak görev yapacak bir Windows sanal makinesi. Bu sanal makineyi aşağıdaki komutu kullanarak oluşturabilirsiniz ve `{password}` güvenli bir parolayla değiştirin:

  ```azurecli-interactive
  az vm create --resource-group IoTEdgeResources --name EdgeVM --image MicrosoftWindowsDesktop:Windows-10:rs5-pro:latest --admin-username azureuser --admin-password {password} --size Standard_DS1_v2
  ```

  Yeni sanal makinenin oluşturulup başlatılması birkaç dakika sürebilir. Daha sonra, sanal makinenize bağlanırken kullanmak üzere bir RDP dosyası indirebilirsiniz:

  1. Azure portal yeni Windows sanal makinenize gidin.
  1. **Bağlan**’ı seçin.
  1. **RDP** SEKMESINDE, **RDP dosyasını indir**' i seçin.

  `az vm create` komutuyla belirttiğiniz yönetici adı ve parolayı kullanarak Windows sanal makinenize bağlanmak için bu dosyayı Uzak Masaüstü Bağlantısı açın.

> [!NOTE]
> Bu hızlı başlangıçta basitlik için bir Windows Masaüstü sanal makinesi kullanılmaktadır. Üretim senaryolarında genel olarak hangi Windows işletim sistemlerinin kullanılabildiği hakkında bilgi için bkz. [Azure IoT Edge desteklenen sistemler](support.md).
>
> IoT Core çalıştıran cihazlar da dahil olmak üzere kendi Windows cihazınızı IoT Edge yapılandırmaya hazırsanız, [Windows 'a Azure IoT Edge çalışma zamanını yüklemek](how-to-install-iot-edge-windows.md)içindeki adımları izleyin.

## <a name="create-an-iot-hub"></a>Bir IoT Hub oluşturma

Azure CLı ile IoT Hub 'ı oluşturarak hızlı başlangıcı başlatın.

![Diyagram - bulutta IOT hub'ı oluşturma](./media/quickstart/create-iot-hub.png)

IoT Hub’ın ücretsiz düzeyi bu hızlı başlangıç için kullanılabilir. IoT Hub'ı daha önce kullandıysanız ve oluşturulmuş ücretsiz hub'ınız varsa bu IoT hub'ını kullanabilirsiniz. Her aboneliğin yalnızca bir ücretsiz IoT hub’ı olabilir.

Aşağıdaki kod `IoTEdgeResources`kaynak grubunda ücretsiz bir **F1** hub oluşturur. `{hub_name}`, IoT Hub 'ınız için benzersiz bir adla değiştirin.

   ```azurecli-interactive
   az iot hub create --resource-group IoTEdgeResources --name {hub_name} --sku F1 --partition-count 2
   ```

   Aboneliğinizde zaten bir ücretsiz hub olduğu için hata alırsanız, SKU değerini **S1** olarak değiştirin. IOT hub'ı adı kullanılamıyor, bir hata alırsanız, başka birisi zaten bu ada sahip bir hub'ı olduğu anlamına gelir. Yeni bir ad deneyin.

## <a name="register-an-iot-edge-device"></a>IoT Edge cihazı kaydetme

Yeni oluşturulan IoT hub'ına bir IoT Edge cihazı kaydedin.
![Diyagram: bir IOT hub'ı kimlik ile bir cihaz kaydı](./media/quickstart/register-device.png)

IoT hub'ınızla iletişim kurabilmesi amacıyla simülasyon cihazınız için bir cihaz kimliği oluşturun. Cihaz kimliği bulutta kalır ve fiziksel cihazla cihaz kimliği arasında bağlantı kurmak için benzersiz bir bağlantı dizesi kullanılır.

IOT Edge cihazları sınıflardır ve tipik bir IOT cihazlarında farklı yönetilebilir olduğundan, bu kimlik ile IOT Edge cihazı için bildirmek `--edge-enabled` bayrağı.

1. Azure Cloud Shell'de aşağıdaki komutu girerek hub'ınızda **myEdgeDevice** adlı bir cihaz oluşturun.

   ```azurecli-interactive
   az iot hub device-identity create --device-id myEdgeDevice --hub-name {hub_name} --edge-enabled
   ```

   İlke anahtarları iothubowner hakkında bir hata alırsanız, cloud shell azure CLI IOT ext uzantısı'nın en son sürümünü çalıştırdığından emin olun.

2. Fiziksel cihazınızla IoT Hub'daki kimliği arasında bağlantı oluşturan cihaz bağlantı dizesini alın.

   ```azurecli-interactive
   az iot hub device-identity show-connection-string --device-id myEdgeDevice --hub-name {hub_name}
   ```

3. `connectionString` anahtarının değerini JSON çıktısından kopyalayın ve kaydedin. Bu değer, cihaz bağlantı dizesidir. Bu bağlantı dizesini bir sonraki bölümde IoT Edge çalışma zamanını yapılandırmak için kullanacaksınız.

   ![CLı çıktısından bağlantı dizesini al](./media/quickstart/retrieve-connection-string.png)

## <a name="install-and-start-the-iot-edge-runtime"></a>IoT Edge çalışma zamanını yükleme ve başlatma

Azure IoT Edge çalışma zamanını IoT Edge cihazınıza yükleyin ve cihaz bağlantı dizesiyle yapılandırın.
![Diyagram - cihazda çalışma zamanını başlatma](./media/quickstart/start-runtime.png)

IoT Edge çalışma zamanı tüm IoT Edge cihazlarına dağıtılır. Üç bileşeni vardır. **IoT Edge güvenlik arka plan programı** , IoT Edge aracısını başlatarak IoT Edge bir cihaz her önyüklendiğinde ve cihazı önyükleme başlatılır. **IoT Edge Aracısı** , IoT Edge hub 'ı da dahil olmak üzere IoT Edge cihazdaki modüllerin dağıtımını ve izlenmesini yönetir. **IoT Edge hub** , IoT Edge cihazdaki modüller arasındaki iletişimleri ve cihaz ile IoT Hub yönetir.

Yükleme betiği Ayrıca, IoT Edge cihazınızdaki kapsayıcı görüntülerini yöneten Moby adlı bir kapsayıcı altyapısı içerir.

Çalışma zamanı yüklemesi sırasında cihaz bağlantı dizesi istenir. Azure CLI'den aldığınız dizeyi kullanın. Bu dize, fiziksel cihazınızı Azure'daki IoT Edge cihaz kimliğiyle ilişkilendirir.

### <a name="connect-to-your-iot-edge-device"></a>IOT Edge Cihazınızı bağlama

Bu bölümdeki adımlar IoT Edge cihazınızda gerçekleşirken, bu sanal makineye Uzak Masaüstü aracılığıyla bağlanmak istiyorsunuz.

### <a name="install-and-configure-the-iot-edge-service"></a>IoT Edge hizmetini yükleyip yapılandırma

PowerShell'i kullanarak IoT Edge çalışma zamanını indirin ve yükleyin. Cihazınızı yapılandırmak için IoT Hub'dan aldığınız cihaz bağlantı dizesini kullanın.

1. Henüz yapmadıysanız, [Yeni bir Azure IoT Edge cihaz kaydetme](how-to-register-device.md) bölümündeki adımları izleyerek cihazınızı kaydedin ve cihaz bağlantı dizesini alın.

2. Sanal makinede, PowerShell 'i yönetici olarak çalıştırın.

   >[!NOTE]
   >PowerShell (x86) değil IoT Edge yüklemek için PowerShell 'in AMD64 oturumunu kullanın. Hangi oturum türünü kullandığınızdan emin değilseniz, aşağıdaki komutu çalıştırın:
   >
   >```powershell
   >(Get-Process -Id $PID).StartInfo.EnvironmentVariables["PROCESSOR_ARCHITECTURE"]
   >```

3. **Deploy-ıotedge** komutu, Windows makinenizin desteklenen bir sürümde olup olmadığını denetler, kapsayıcılar özelliğini etkinleştirir, Moby çalışma zamanını indirir ve sonra IoT Edge çalışma zamanını indirir.

   ```powershell
   . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
   Deploy-IoTEdge -ContainerOs Windows
   ```

4. Makineniz otomatik olarak yeniden başlatılabilir. Yeniden başlatmak için Deploy-ıotedge komutu istenirse, şimdi bunu yapın.

5. PowerShell 'i yönetici olarak yeniden çalıştırın.

6. **Initialize-ıotedge** komutu, makinenizde IoT Edge çalışma zamanını yapılandırır. Komut, Windows kapsayıcıları ile el ile sağlamayı varsayılan olarak belirler. 

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

3. IoT Edge cihazınızda çalışan tüm modülleri görüntüleyin. Hizmet ilk kez başlatıldığı için yalnızca **edgeAgent** modülünün çalıştığını göreceksiniz. EdgeAgent modülü varsayılan olarak çalışır ve cihazınıza dağıttığınız ek modüllerin yüklenmesini ve başlamasını sağlar.

    ```powershell
    iotedge list
    ```

   ![Cihazınızda bir modülü görüntüleme](./media/quickstart/iotedge-list-1.png)

Yüklemenin tamamlanması birkaç dakika sürebilir ve IoT Edge aracı modülü başlatılır.

IoT Edge cihazınız yapılandırıldı. Bulutta dağıtılan modülleri çalıştırmak için hazır.

## <a name="deploy-a-module"></a>Modül dağıtma

IoT Hub 'ye telemetri verileri gönderen bir modül dağıtmak için Azure IoT Edge cihazınızı buluttan yönetin.
![Diyagram - modülü buluttan cihaza dağıtın](./media/quickstart/deploy-module.png)

[!INCLUDE [iot-edge-deploy-module](../../includes/iot-edge-deploy-module.md)]

## <a name="view-generated-data"></a>Oluşturulan verileri görüntüleme

Bu hızlı başlangıçta, IoT Edge bir cihaz kaydettiniz ve IoT Edge çalışma zamanını üzerine yüklediniz. Ardından, cihazın kendisinde değişiklik yapmak zorunda kalmadan cihazda çalıştırmak üzere bir IoT Edge modülünü dağıtmak için Azure portal kullandınız.

Bu durumda, gönderdiğiniz modül test için kullanabileceğiniz örnek veriler oluşturur. Sanal sıcaklık algılayıcı modülü, daha sonra test için kullanabileceğiniz ortam verileri oluşturur. Sanal algılayıcı hem makinenin hem de makinenin bulunduğu ortamın izlenmesini izler. Örneğin bu sensör bir sunucu odasında, fabrika sahasında veya rüzgar türbininde olabilir. İletide ortam sıcaklığı ve nemi, makine sıcaklığı ve basıncı ile bir zaman damgası bulunur. IoT Edge öğreticileri, bu modülle oluşturulan verileri analiz için test verileri olarak kullanır.

Buluttan dağıtılan modülün IoT Edge cihazınızda çalıştığından emin olun.

```powershell
iotedge list
```

   ![Cihazınızda üç modül görüntüleme](./media/quickstart/iotedge-list-2.png)

Sıcaklık algılayıcısı modülünden buluta gönderilen iletileri görüntüleyin.

```powershell
iotedge logs SimulatedTemperatureSensor -f
```

   >[!TIP]
   >IoT Edge komutlar modül adlarına başvururken büyük/küçük harfe duyarlıdır.

   ![Verileri modülünüzden görüntüleme](./media/quickstart/iotedge-logs.png)

Ayrıca, [Visual Studio Code Için azure IoT Hub araç seti uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (eskiden Azure IoT araç seti uzantısı) kullanarak IoT Hub 'ınıza gelen iletileri izleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

IoT Edge öğreticilerine devam etmek istiyorsanız bu hızlı başlangıçta kaydettiğiniz ve ayarladığınız cihazı kullanabilirsiniz. Aksi takdirde, ücretlendirmemek için oluşturduğunuz Azure kaynaklarını silebilirsiniz.

Sanal makinenizi ve IoT hub’ınızı yeni bir kaynak grubunda oluşturduysanız, bu grubu ve ilişkili tüm kaynaklarını silebilirsiniz. Çifte denetim var. emin olmak için kaynak grubunun içeriğini kullanıcının tutmak istediğiniz bir şey. Tüm bir grubu silmek istemiyorsanız, bunun yerine bu kaynakları tek tek silebilirsiniz.

**IoTEdgeResources** grubunu kaldırın.

```azurecli-interactive
az group delete --name IoTEdgeResources
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta bir IoT Edge cihaz oluşturdunuz ve cihaza kod dağıtmak için Azure IoT Edge bulut arabirimini kullandınız. Artık ortamı hakkında ham veri üreten bir test cihazınız var.

Bir sonraki adım, iş mantığınızı çalıştıran IoT Edge modüller oluşturmaya başlayabilmeniz için yerel geliştirme ortamınızı ayarlamanıza olanak sağlar. 

> [!div class="nextstepaction"]
> [Windows cihazları için IoT Edge modülleri geliştirmeye başlama](tutorial-develop-for-windows.md)
