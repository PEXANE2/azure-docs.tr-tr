---
title: Windows Server sanal makinelerinde Azure IoT Edge çalıştırma | Microsoft Docs
description: Windows Server Market sanal makinelerinde kurulum yönergelerini Azure IoT Edge
author: gregman-msft
manager: arjmands
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: philmea
ms.openlocfilehash: 5f88a21efd04c9dd24fe31e925a3b911b5ec9df2
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77045884"
---
# <a name="run-azure-iot-edge-on-windows-server-virtual-machines"></a>Windows Server sanal makinelerinde Azure IoT Edge çalıştırma

Azure IOT Edge çalışma zamanı, ne bir cihaz ile IOT Edge cihazı kapatır ' dir. Çalışma zamanı, cihaz olarak endüstriyel sunucusu olarak büyük veya küçük bir Raspberry Pi üzerinde dağıtılabilir. Bir cihaz IOT Edge çalışma zamanı ile yapılandırıldıktan sonra iş mantığı buluttan dağıttıktan başlayabilirsiniz.

IoT Edge çalışma zamanının nasıl çalıştığı ve hangi bileşenlerin dahil olduğu hakkında daha fazla bilgi edinmek için bkz. [Azure IoT Edge çalışma zamanını ve mimarisini anlayın](iot-edge-runtime.md).

Bu makalede, Windows [Server](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview) Azure Marketi teklifini kullanarak windows Server 2019 sanal makinesinde Azure IoT Edge çalışma zamanını çalıştırma adımları listelenir. Diğer sürümlerle kullanmak üzere Windows 'da [Azure IoT Edge çalışma zamanını yüklerken](how-to-install-iot-edge-windows.md) yönergeleri izleyin.

## <a name="deploy-from-the-azure-marketplace"></a>Azure Marketi 'nden dağıtma

1. [Windows Server](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftwindowsserver.windowsserver?tab=Overview) Azure Marketi teklifine veya [Azure Marketi](https://azuremarketplace.microsoft.com/) 'nde "Windows Server" arayarak gidin
2. **Şimdi al** seçeneğini belirleyin
3. **Yazılım planında**, "kapsayıcılarla Windows Server 2019 Datacenter Server Core" öğesini bulun ve ardından İleri Iletişim kutusunda **devam** ' ı seçin.
    * Bu yönergeleri, kapsayıcılarla Windows Server 'ın diğer sürümleri için de kullanabilirsiniz
4. Azure portal bir kez **Oluştur** ' u seçin ve VM 'yi dağıtmak için Sihirbazı izleyin.
    * Bir VM 'yi ilk kez deniyorsanız, bir parolanın kullanılması ve genel gelen bağlantı noktası menüsünde RDP ve SSH 'yi etkinleştirmek en kolay yoldur.
    * Yoğun kaynak iş yükünüz varsa, daha fazla CPU ve/veya bellek ekleyerek sanal makine boyutunu yükseltmeniz gerekir.
5. Sanal makine dağıtıldıktan sonra, IoT Hub bağlanacak şekilde yapılandırın:
    1. Cihaz Bağlantı dizenizi, IoT Hub oluşturulan IoT Edge cihazınızdan kopyalayın. [Azure Portal bağlantı dizesini alma](how-to-register-device.md#retrieve-the-connection-string-in-the-azure-portal)yordamına bakın.
    1. Azure portal yeni oluşturduğunuz sanal makine kaynağını seçin ve **komutu Çalıştır** seçeneğini açın
    1. **Runpowershellscript** seçeneğini belirleyin
    1. Bu betiği, cihaz bağlantı dizeniz ile komut penceresine kopyalayın:

        ```powershell
        . {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `
        Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'
        ```

    1. IoT Edge çalışma zamanını yüklemek ve **Çalıştır** 'ı seçerek Bağlantı dizenizi ayarlamak için betiği yürütün
    1. Bir veya iki dakika sonra, Edge çalışma zamanının başarıyla yüklendiğini ve sağlandığını belirten bir ileti görmeniz gerekir.

## <a name="deploy-from-the-azure-portal"></a>Azure portal dağıtma

1. Azure portal, "Windows Server" araması yapın ve sanal makine oluşturma iş akışını başlatmak için **Windows server 2019 Datacenter** ' ı seçin.
2. **Yazılım planı seç** ' den "kapsayıcı Içeren Windows Server 2019 Datacenter Server Core" öğesini seçin ve ardından **Oluştur** ' u seçin.
3. Yukarıdaki "Azure Marketi 'nden dağıtma" yönergelerindeki 5. adımı uygulayın.

## <a name="deploy-from-azure-cli"></a>Azure CLı 'dan dağıtma

1. Masaüstünüzde Azure CLı kullanıyorsanız, oturum açarak başlayın:

   ```azurecli-interactive
   az login
   ```

1. Birden çok aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin:
   1. Aboneliklerinizi listeleyin:

      ```azurecli-interactive
      az account list --output table
      ```

   1. Kullanmak istediğiniz aboneliğin abonelik alanını kopyalayın
   1. Kopyaladığınız KIMLIKLE bu komutu çalıştırın:

      ```azurecli-interactive
      az account set -s {SubscriptionId}
      ```

1. Yeni bir kaynak grubu oluşturun (veya sonraki adımlarda mevcut bir tane belirtin):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

1. Yeni bir sanal makine oluşturun:

   ```azurecli-interactive
   az vm create -g IoTEdgeResources -n EdgeVM --image MicrosoftWindowsServer:WindowsServer:2019-Datacenter-Core-with-Containers:latest  --admin-username azureuser --generate-ssh-keys --size Standard_DS1_v2
   ```

   * Bu komut sizden bir parola ister, ancak bir betikte daha kolay bir şekilde ayarlamak için `--admin-password` seçeneğini ekleyebilirsiniz
   * Windows Server çekirdek görüntüsünün yalnızca uzak masaüstü ile komut satırı desteği vardır. bu nedenle, tam masaüstü deneyimi istiyorsanız görüntü olarak `MicrosoftWindowsServer:WindowsServer:2019-Datacenter-with-Containers:latest` belirtin

1. Cihaz bağlantı dizesini ayarlayın (Bu işleme alışkın değilseniz [Azure CLI ile bağlantı dizesini alma](how-to-register-device.md#retrieve-the-connection-string-with-the-azure-cli) yordamını izleyebilirsiniz):

   ```azurecli-interactive
   az vm run-command invoke -g IoTEdgeResources -n EdgeVM --command-id RunPowerShellScript --script ". {Invoke-WebRequest -useb aka.ms/iotedge-win} | Invoke-Expression; `Install-IoTEdge -Manual -DeviceConnectionString '<connection-string>'"
   ```

## <a name="next-steps"></a>Sonraki adımlar

Çalışma zamanının yüklü olduğu bir IoT Edge cihazınıza sahip olduğunuza göre, [IoT Edge modülleri dağıtabilirsiniz](how-to-deploy-modules-portal.md).

Edge çalışma zamanının düzgün şekilde yüklenmesiyle ilgili sorun yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Mevcut bir yüklemeyi en yeni IoT Edge sürümüne güncelleştirmek için bkz. [IoT Edge güvenlik cini ve çalışma zamanını güncelleştirme](how-to-update-iot-edge.md).

[Windows sanal makineleri belgelerinde](https://docs.microsoft.com/azure/virtual-machines/windows/)Windows sanal makinelerini kullanma hakkında daha fazla bilgi edinin.

Kurulumdan sonra bu VM 'ye SSH eklemek istiyorsanız, uzak masaüstü veya uzak PowerShell kullanarak [Windows Server Için OpenSSH yükleme](https://docs.microsoft.com/windows-server/administration/openssh/openssh_install_firstuse#installing-openssh-with-powershell) kılavuzunu izleyin.
