---
title: Windows Server Sanal Makinelerde Azure IoT Edge çalıştırın | Microsoft Dokümanlar
description: Windows Server Marketplace Sanal Makinelerde Azure IoT Edge kurulum talimatları
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: philmea
ms.openlocfilehash: 5f88a21efd04c9dd24fe31e925a3b911b5ec9df2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77045884"
---
# <a name="run-azure-iot-edge-on-windows-server-virtual-machines"></a>Windows Server Sanal Makinelerde Azure IoT Edge çalıştırın

Azure IoT Edge çalışma zamanı, aygıtı IoT Edge aygıtına dönüştüren şeydir. Çalışma süresi Raspberry Pi kadar küçük veya endüstriyel sunucu kadar büyük aygıtlarda dağıtılabilir. Bir aygıt IoT Edge çalışma zamanı ile yapılandırıldıktan sonra, buluttan iş mantığı dağıtmaya başlayabilirsiniz.

IoT Edge çalışma zamanı nın nasıl çalıştığı ve hangi bileşenlerin dahil edildiği hakkında daha fazla bilgi edinmek için azure [IoT Edge çalışma süresini ve mimarisini anlayın.](iot-edge-runtime.md)

Bu makalede, [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview) Azure Market teklifini kullanarak Bir Windows Server 2019 sanal makinede Azure IoT Edge çalışma süresini çalıştırma adımları listelenmiştir. Diğer sürümlerle birlikte kullanılmak üzere Windows'daki [Azure IoT Edge çalışma saatini](how-to-install-iot-edge-windows.md) yükleyin'deki yönergeleri izleyin.

## <a name="deploy-from-the-azure-marketplace"></a>Azure Marketinden Dağıtma

1. [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview) Azure Marketi teklifine gidin veya [Azure Marketi'nde](https://azuremarketplace.microsoft.com/) "Windows Server" araması yaparak
2. **ŞİmDİ AL'ı** seçin
3. **Yazılım planında**, "Windows Server 2019 Datacenter Server Core with Containers" adlı aracını bulun ve ardından bir sonraki iletişim kutusunda **Devam et'i** seçin.
    * Bu yönergeleri, Windows Server'ın Kapsayıcılı diğer sürümleri için de kullanabilirsiniz
4. Azure portalına girdiğinde, VM'yi dağıtmak için **Oluştur'u** seçin ve sihirbazı izleyin.
    * İlk kez bir VM deniyorsanız, parola kullanmak ve gelen ortak bağlantı noktası menüsünde RDP ve SSH'yi etkinleştirmek en kolayıdır.
    * Kaynak yoğun bir iş yükünüz varsa, daha fazla CPU ve/veya bellek ekleyerek sanal makine boyutunu yükseltmeniz gerekir.
5. Sanal makine dağıtıldıktan sonra, IoT Hub'ınıza bağlanacak şekilde yapılandırın:
    1. Aygıt bağlantı dizenizi IoT Hub'ınızda oluşturulan IoT Edge aygıtınızdan kopyalayın. Yordamı görün [Azure portalındaki bağlantı dizesini alın.](how-to-register-device.md#retrieve-the-connection-string-in-the-azure-portal)
    1. Azure portalından yeni oluşturulan sanal makine kaynağınızı seçin ve **çalıştır komut seçeneğini** açın
    1. **RunPowerShellScript** seçeneğini seçin
    1. Bu komut dosyasını aygıt bağlantı dizenizle komut penceresine kopyalayın:

        ```powershell
        . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
        Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'
        ```

    1. IoT Edge çalışma süresini yüklemek için komut dosyasını çalıştırın ve **Çalıştır'ı** seçerek bağlantı dizenizi ayarlayın
    1. Bir veya iki dakika sonra, Edge çalışma zamanının yüklenmiş ve başarıyla sağlandığını belirten bir ileti görmeniz gerekir.

## <a name="deploy-from-the-azure-portal"></a>Azure portalından dağıtma

1. Azure portalından "Windows Server"ı arayın ve VM oluşturma iş akışını başlatmak için **Windows Server 2019 Datacenter'ı** seçin.
2. **Bir yazılım planı seçin'den** "Windows Server 2019 Datacenter Server Core with Containers" seçeneğini belirleyin ve ardından **Oluştur'u** seçin
3. Yukarıdaki "Azure Marketinden Dağıt" yönergelerinde 5.

## <a name="deploy-from-azure-cli"></a>Azure CLI'den dağıtma

1. Masaüstünüzde Azure CLI kullanıyorsanız, oturum açarak başlayın:

   ```azurecli-interactive
   az login
   ```

1. Birden çok aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin:
   1. Aboneliklerinizi listeleyin:

      ```azurecli-interactive
      az account list --output table
      ```

   1. Kullanmak istediğiniz abonelik için SubscriptionID alanını kopyalama
   1. Bu komutu kopyaladiğiniz kimlikle çalıştırın:

      ```azurecli-interactive
      az account set -s {SubscriptionId}
      ```

1. Yeni bir kaynak grubu oluşturun (veya sonraki adımlarda varolan bir tane belirtin):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

1. Yeni bir sanal makine oluşturun:

   ```azurecli-interactive
   az vm create -g IoTEdgeResources -n EdgeVM --image MicrosoftWindowsServer:WindowsServer:2019-Datacenter-Core-with-Containers:latest  --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```

   * Bu komut bir parola için istenir, ancak `--admin-password` bir komut dosyasında daha kolay ayarlamak için seçeneğini ekleyebilirsiniz
   * Windows Server Core görüntüsünde yalnızca uzak masaüstü ile komut satırı desteği vardır, `MicrosoftWindowsServer:WindowsServer:2019-Datacenter-with-Containers:latest` bu nedenle tam masaüstü deneyimini istiyorsanız, resim olarak belirtin

1. Aygıt bağlantı dizesini ayarlayın (Bu işleme aşina değilseniz [Azure CLI yordamı ile bağlantı dizesini al'ı](how-to-register-device.md#retrieve-the-connection-string-with-the-azure-cli) takip edebilirsiniz):

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunPowerShellScript --script ". {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'"
   ```

## <a name="next-steps"></a>Sonraki adımlar

Artık çalışma zamanı yüklü bir IoT Edge aygıtınız olduğuna göre, [IoT Edge modüllerini dağıtabilirsiniz.](how-to-deploy-modules-portal.md)

Edge çalışma zamanının düzgün yüklenmesiyle ilgili sorunlar yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Varolan bir yüklemeyi IoT Edge'in en yeni sürümüne güncelleştirmek için [bkz.](how-to-update-iot-edge.md)

[Windows Virtual Machines belgelerinde](https://docs.microsoft.com/azure/virtual-machines/windows/)Windows sanal makineleri kullanma hakkında daha fazla bilgi edinin.

Kurulumdan sonra bu VM'ye SSH'yi sokmak istiyorsanız, uzak masaüstü veya uzak powershell kullanarak [Windows Server için OpenSSH yüklemesini](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse#installing-openssh-with-powershell) izleyin.
