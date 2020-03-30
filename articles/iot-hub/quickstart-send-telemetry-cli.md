---
title: Azure IoT Hub'ına (CLI) hızlı başlatmaya telemetri gönderin
description: Bu hızlı başlangıç, IoT Hub'ına yeni başlayan geliştiricilere, bir IoT hub'ı oluşturmak, telemetri göndermek ve aygıt la hub arasındaki iletileri görüntülemek için Azure CLI'yi kullanarak nasıl başlayacaklarını gösterir.
ms.service: iot-hub
ms.topic: quickstart
ms.custom:
- iot-send-telemetry-cli
- iot-p0-scenario
ms.author: timlt
author: timlt
ms.date: 11/06/2019
ms.openlocfilehash: 711e15986265324bbb353fb2b4404cbfeb48dc84
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78851437"
---
# <a name="quickstart-send-telemetry-from-a-device-to-an-iot-hub-and-monitor-it-with-the-azure-cli"></a>Hızlı başlatma: Bir aygıttan bir IoT hub'ına telemetri gönderin ve Azure CLI ile izleyin

[!INCLUDE [iot-hub-quickstarts-1-selector](../../includes/iot-hub-quickstarts-1-selector.md)]

IoT Hub, IoT cihazlarınızdan buluta depolama veya işleme amacıyla yüksek hacimlerde telemetri almanızı sağlayan bir Azure hizmetidir. Bu hızlı başlatmada, Bir IoT Hub'ı ve simüle edilmiş bir aygıt oluşturmak, aygıt telemetrisini hub'a göndermek ve buluttan aygıta ileti göndermek için Azure CLI'yi kullanırsınız. Aygıt ölçümlerini görselleştirmek için Azure portalını da kullanırsınız. Bu, Bir IoT Hub uygulamasıyla etkileşimkurmak için CLI'yi kullanan geliştiriciler için temel bir iş akışıdır.

## <a name="prerequisites"></a>Ön koşullar
- Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir abonelik oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- Azure CLI. Tarayıcınızda çalışan etkileşimli bir CLI kabuğu olan Azure Bulut Kabuğu'nu kullanarak bu hızlı başlangıçta tüm komutları çalıştırabilirsiniz. Bulut Kabuğu'nu kullanıyorsanız, hiçbir şey yüklemeniz gerekmez. CLI'yi yerel olarak kullanmayı tercih ederseniz, bu hızlı başlatma azure CLI sürümü 2.0.76 veya daha sonra gerektirir. Sürümü bulmak için az --version komutunu çalıştırın. Yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye]( /cli/azure/install-azure-cli)bakın.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
https://portal.azure.com adresinden Azure portalında oturum açın.

İster yerel olarak ister Bulut Kabuğu'nda CLI çalıştırın, portalı tarayıcınızda açık tutun.  Daha sonra bu hızlı başlangıç kullanın.

## <a name="launch-the-cloud-shell"></a>Bulut Kabuğunu Başlat
Bu bölümde, Azure Bulut Kabuğu'nun bir örneğini başlatAbilirsiniz. CLI'yi yerel olarak kullanıyorsanız, [iki CLI oturumu hazırla](#prepare-two-cli-sessions)bölümüne atlayın.

Bulut Kabuğunu başlatmak için:

1. Azure portalındaki sağ üst menü çubuğundaki **Bulut Kabuğu** düğmesini seçin. 

    ![Azure portalı Bulut Shell düğmesi](media/quickstart-send-telemetry-cli/cloud-shell-button.png)

    > [!NOTE]
    > Bulut Kabuğu'nu ilk kez kullanıyorsanız, Bulut Kabuğu'nu kullanmanız gereken depolama alanı oluşturmanızı ister.  Depolama hesabı oluşturmak ve Microsoft Azure Dosyaları paylaşmak için bir abonelik seçin. 

2. **Select ortamı** açılır düşüşünde tercih ettiğiniz CLI ortamını seçin. Bu hızlı **başlatma, Bash** ortamını kullanır. Aşağıdaki Tüm CLI komutları Powershell ortamında da çalışır. 

    ![CLI ortamını seçin](media/quickstart-send-telemetry-cli/cloud-shell-environment.png)

## <a name="prepare-two-cli-sessions"></a>İki CLI oturumu hazırlayın

Bu bölümde, iki Azure CLI oturumu hazırlayın. Bulut Kabuğu'nu kullanıyorsanız, iki oturumu ayrı tarayıcı sekmelerinde çalıştırın. Yerel bir CLI istemcisi kullanıyorsanız, iki ayrı CLI örneği çalıştırın. İlk oturumu benzetimli aygıt, ikinci oturumu ise iletileri izlemek ve göndermek için kullanırsınız. Bir komutu çalıştırmak **Copy** için, bu hızlı başlatmada bir kod bloğunu kopyalamak, kabuk oturumunuza yapıştırmak ve çalıştırmak için Kopyala'yı seçin.

Azure CLI, Azure hesabınıza oturum açmanızı gerektirir. Azure CLI kabuk oturumunuz la IoT hub'ınız arasındaki tüm iletişimin kimliği doğrulanır ve şifrelenir. Sonuç olarak, bu hızlı başlatma, bağlantı dizesi gibi gerçek bir aygıtla kullanacağınız ek kimlik doğrulaması gerektirmez.

*  CLI kabuğunuza Azure CLI için Microsoft Azure IoT Uzantısı eklemek için [az uzantı ekleme](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-add) komutunu çalıştırın. IOT Uzantı, Azure CLI'ye IoT Hub, IoT Edge ve IoT Aygıt Sağlama Hizmeti (DPS) özel komutlarını ekler.

   ```azurecli
   az extension add --name azure-iot
   ```
   
   Azure IOT uzantısını yükledikten sonra, herhangi bir Bulut Bulutu oturumunda yeniden yüklemeniz gerekmez. 

   [!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

*  İkinci bir CLI oturumu açın.  Bulut Kabuğu'nu kullanıyorsanız, **yeni oturum aç'ı**seçin. CLI'yi yerel olarak kullanıyorsanız, ikinci bir örnek açın. 

    >[!div class="mx-imgBorder"]
    >![Yeni Cloud Shell oturumunu açın](media/quickstart-send-telemetry-cli/cloud-shell-new-session.png)

## <a name="create-an-iot-hub"></a>IoT Hub'ı oluşturma
Bu bölümde, bir kaynak grubu ve bir IoT Hub'ı oluşturmak için Azure CLI'yi kullanırsınız.  Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. IoT Hub, IoT uygulamanız ve aygıtlar arasındaki çift yönlü iletişim için merkezi bir ileti merkezi görevi görür. 

> [!TIP]
> İsteğe bağlı olarak, [Azure portalı,](iot-hub-create-through-portal.md) [Visual Studio Kodu](iot-hub-create-use-iot-toolkit.md)veya diğer programlı yöntemleri kullanarak bir Azure kaynak grubu, bir IoT Hub ve diğer kaynaklar oluşturabilirsiniz.  

1. Bir kaynak grubu oluşturmak için [az grubu oluşturma](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create) komutunu çalıştırın. Aşağıdaki *komut, doğudaki* konumda *MyResourceGroup* adında bir kaynak grubu oluşturur. 

    ```azurecli
    az group create --name MyResourceGroup --location eastus
    ```

1. Bir IoT hub'ı oluşturmak için [az iot hub](https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest#az-iot-hub-create) oluşturma komutunu çalıştırın. Bir IoT hub'ı oluşturmak birkaç dakika sürebilir. 

    *YourIotHubName*. Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin. Azure'da bir IoT hub adı genel olarak benzersiz olmalıdır. Bu yer tutucu, ioT hub adınızı temsil etmek için bu hızlı başlatmanın geri kalanında kullanılır.

    ```azurecli
    az iot hub create --resource-group MyResourceGroup --name {YourIoTHubName}
    ```

## <a name="create-and-monitor-a-device"></a>Aygıt oluşturma ve izleme
Bu bölümde, ilk CLI oturumunda benzetimli bir aygıt oluşturursunuz. Benzetimli cihaz, aygıt telemetrisini IoT hub'ınıza gönderir. İkinci CLI oturumunda, olayları ve telemetriyi izler ve simüle edilen aygıta buluttan aygıta ileti gönderirsiniz.

Benzetimli bir aygıt oluşturmak ve başlatmak için:
1. İlk CLI oturumunda [az iot hub aygıt-kimlik oluşturma](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-device-identity-create) komutunu çalıştırın. Bu, benzetilen aygıt kimliğini oluşturur. 

    *YourIotHubName*. Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin. 

    *simDevice*. Bu hızlı başlatmanın geri kalanında bu adı doğrudan benzetilen aygıt için kullanabilirsiniz. İsteğe bağlı olarak, farklı bir ad kullanın. 

    ```azurecli
    az iot hub device-identity create --device-id simDevice --hub-name {YourIoTHubName} 
    ```

1. İlk CLI oturumunda [az iot aygıtını çalıştırın](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/device?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-device-simulate) komutu benzetin.  Bu, simüle edilen aygıtı başlatır. Cihaz IoT hub'ınıza telemetri gönderir ve ondan iletiler alır.  

    *YourIotHubName*. Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin. 

    ```azurecli
    az iot device simulate -d simDevice -n {YourIoTHubName}
    ```

Bir aygıtı izlemek için:
1. İkinci CLI oturumunda, [az iot hub monitör-olaylar](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-monitor-events) komutunu çalıştırın. Bu, simüle edilen aygıtı izlemeye başlar. Çıktı, benzetilen aygıtın IoT hub'ına gönderdiği telemetriyi gösterir.

    *YourIotHubName*. Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin. 

    ```azurecli
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

    ![Cloud Shell izleme etkinlikleri](media/quickstart-send-telemetry-cli/cloud-shell-monitor.png)

1. İkinci CLI oturumunda simüle edilen aygıtı izledikten sonra izlemeyi durdurmak için Ctrl+C tuşuna basın. 

## <a name="use-the-cli-to-send-a-message"></a>İleti göndermek için CLI'yi kullanma
Bu bölümde, benzetilen aygıta ileti göndermek için ikinci CLI oturumunu kullanırsınız.

1. İlk CLI oturumunda, benzetilen aygıtın çalıştığını doğrulayın. Aygıt durduysa, başlatmak için aşağıdaki komutu çalıştırın:

    *YourIotHubName*. Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin. 

    ```azurecli
    az iot device simulate -d simDevice -n {YourIoTHubName}
    ```

1. İkinci CLI oturumunda, [az iot cihazı c2d-message gönderme](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/device/c2d-message?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-device-c2d-message-send) komutunu çalıştırın. Bu, IoT hub'ınızdan simüle edilen aygıta buluttan cihaza ileti gönderir. İleti, bir dize ve iki anahtar değeri çifti içerir.  

    *YourIotHubName*. Aşağıdaki yer tutucuyu IoT hub'ınız için seçtiğiniz adla değiştirin. 

    ```azurecli
    az iot device c2d-message send -d simDevice --data "Hello World" --props "key0=value0;key1=value1" -n {YourIoTHubName}
    ```
    İsteğe bağlı olarak, Azure portalını kullanarak buluttan cihaza iletigönderebilirsiniz. Bunu yapmak için, IoT Hub'ınız için genel bakış sayfasına göz atın, **IoT Aygıtlarını**seçin, simüle edilen aygıtı seçin ve **Aygıta İleti'yi**seçin. 

1. İlk CLI oturumunda, benzetilen aygıtın iletiyi aldığını doğrulayın. 

    ![Bulut Shell buluttan cihaza ileti](media/quickstart-send-telemetry-cli/cloud-shell-receive-message.png)

1. İletiyi görüntüledikten sonra ikinci CLI oturumunu kapatın. İlk CLI oturumunu açık tutun. Kaynakları daha sonraki bir adımda temizlemek için kullanırsınız.

## <a name="view-messaging-metrics-in-the-portal"></a>Portaldaki mesajlaşma ölçümlerini görüntüleme
Azure portalı, IoT Hub'ınızın ve cihazlarınızın tüm yönlerini yönetmenize olanak tanır. Aygıtlardan telemetri yiyerek tipik bir IoT Hub uygulamasında, aygıtları izlemek veya aygıt telemetrisindeki ölçümleri görüntülemek isteyebilirsiniz. 

Azure portalında mesajlaşma ölçümlerini görselleştirmek için:
1. PortalDaki sol navigasyon menüsünde **Tüm Kaynaklar'ı**seçin. Bu, oluşturduğunuz IoT hub'ı da dahil olmak üzere aboneliğinizdeki tüm kaynakları listeler. 

1. Oluşturduğunuz IoT hub'ındaki bağlantıyı seçin. Portal, hub için genel bakış sayfasını görüntüler.

1. IoT Hub'ınızın sol bölmesinde **Ölçümler'i** seçin. 

    ![IoT Hub mesajlaşma ölçümleri](media/quickstart-send-telemetry-cli/iot-hub-portal-metrics.png)

1. **Kapsam'a**IoT hub adınızı girin.

2. Metrik Ad Alanında *Iot Hub Standart* Ölçümleri'ni seçin. **Metric Namespace**

3. **Metrik'te** *kullanılan toplam ileti sayısını* seçin. 

4. Fare işaretçinizi cihazınızın ileti gönderdiği zaman çizelgesi alanının üzerine getirin. Zaman içinde bir noktadaki toplam ileti sayısı zaman çizelgesinin sol alt köşesinde görüntülenir.

    ![Azure IoT Hub ölçümlerini görüntüleme](media/quickstart-send-telemetry-cli/iot-hub-portal-view-metrics.png)

5. İsteğe bağlı olarak, simüle edilen aygıtınızdaki diğer ölçümleri görüntülemek için **Metrik** açılır bırakma yı kullanın. Örneğin, *C2d ileti teslimleri tamamlandı* veya *Toplam aygıtlar (önizleme)*. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlatmada oluşturulan Azure kaynaklarına artık ihtiyacınız yoksa, bunları silmek için Azure CLI'yi kullanabilirsiniz.

Bir sonraki önerilen makaleye devam ederseniz, oluşturduğunuz kaynakları saklayabilir ve yeniden kullanabilirsiniz. 

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. 

Bir kaynak grubunu adıyla silmek için:
1. az [grubu silme](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu çalıştırın. Bu, kaynak grubunu, IoT Hub'ını ve oluşturduğunuz aygıt kaydını kaldırır.

    ```azurecli
    az group delete --name MyResourceGroup
    ```
1. Kaynak grubunun silinir onaylamak için [az grup liste](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-list) komutunu çalıştırın.  

    ```azurecli
    az group list
    ```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir IoT hub'ı oluşturmak, simüle edilmiş bir aygıt oluşturmak, telemetri göndermek, telemetriyi izlemek, buluttan aygıta ileti göndermek ve kaynakları temizlemek için Azure CLI'yi kullandınız. Cihazınızdaki mesajlaşma ölçümlerini görselleştirmek için Azure portalını kullandınız.

Bir aygıt geliştiricisiyseniz, önerilen sonraki adım, C için Azure IoT Device SDK'yı kullanan telemetri hızlı başlatmasını görmek, tercih ettiğiniz dilde veya SDK'da kullanılabilir Azure IoT Hub telemetrisi hızlı başlatma makalelerinden birini görmektir.

> [!div class="nextstepaction"]
> [Hızlı başlatma: Bir aygıttan IoT hub'ına (C) telemetri gönderme](quickstart-send-telemetry-c.md)
