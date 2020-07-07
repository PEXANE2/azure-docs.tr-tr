---
title: IoT dağıtımı özel sanal cihazlar-Azure | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda, özel sanal cihazların uzaktan Izleme çözüm hızlandırıcısına nasıl dağıtılacağı gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/15/2018
ms.topic: conceptual
ms.openlocfilehash: 7cbab38db859935c9f4490d79a131d6c9a7e302b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "66427574"
---
# <a name="deploy-a-new-simulated-device"></a>Yeni bir sanal cihaz dağıtma

Uzaktan Izleme ve cihaz benzetimi çözüm hızlandırıcılarına her ikisi de kendi sanal cihazlarınızı tanımlamanızı sağlar. Bu makalede, özelleştirilmiş bir chilcihaz türü ve yeni bir lightampul cihaz türünün uzaktan Izleme çözüm hızlandırıcısına nasıl dağıtılacağı gösterilir.

Bu makaledeki adımlarda, [Yeni bir sanal cihaz oluşturma ve test etme ve](iot-accelerators-remote-monitoring-create-simulated-device.md) özelleştirilmiş chilve yeni ampul cihaz türlerini tanımlayan dosyaların bulunduğu varsayılmaktadır.

Bu nasıl yapılır kılavuzundaki adımlarda şu adımları nasıl kullanabileceğiniz gösterilmektedir:

1. Uzaktan Izleme çözüm Hızlandırıcısını barındıran sanal makinenin dosya sistemine erişmek için SSH kullanın.

1. Docker kapsayıcısı dışındaki bir konumdan cihaz modellerini yüklemek için Docker 'ı yapılandırın.

1. Özel cihaz modeli dosyalarını kullanarak uzaktan Izleme çözüm Hızlandırıcısını çalıştırın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için etkin bir Azure aboneliğine ihtiyacınız vardır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu izlemek için şunlar gerekir:

- [Uzaktan izleme çözümü hızlandırıcının](https://www.azureiotsolutions.com/Accelerators#solutions/types/RM2)dağıtılan bir örneği.
- Ve komutlarını çalıştırmak için yerel bir **Bash** kabuğu `ssh` `scp` . Windows 'ta **Bash** 'i yüklemenin kolay bir yolu da [Git](https://git-scm.com/download/win)'i yüklemektir.
- [Yeni bir sanal cihaz oluşturma ve test](iot-accelerators-remote-monitoring-create-simulated-device.md)etme bölümünde açıklananlar gibi özel cihaz modeli dosyalarınız.

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>Docker 'ı yapılandırma

Bu bölümde, Docker 'ın, Docker kapsayıcısının içinden değil, sanal makinedeki **/tmp/devicemodeller** klasöründen cihaz modeli dosyalarını yükleyecek şekilde yapılandıracaksınız. Bu bölümdeki komutları yerel makinenizde bir **Bash** kabuğu 'nda çalıştırın:

Bu bölümde, Docker 'ın, Docker kapsayıcısının içinden değil, sanal makinedeki **/tmp/devicemodeller** klasöründen cihaz modeli dosyalarını yükleyecek şekilde yapılandıracaksınız. Bu bölümdeki komutları yerel makinenizde bir **Bash** kabuğu 'nda çalıştırın:

1. Yerel makinenizden Azure 'daki sanal makineye bağlanmak için SSH kullanın. Aşağıdaki komut, VM 'nin genel IP adresini (sanal makine **-vikxv** ) **104.41.128.108** olduğunu varsayar; bu değerin önceki bölümde yer ALDıĞı sanal makinenizin genel IP adresiyle değiştirin:

   ```sh
    ssh azureuser@104.41.128.108
    ```

    Önceki bölümde ayarladığınız parolayla sanal makinede oturum açmak için istemleri izleyin.

1. Cihaz benzetimi hizmetini kapsayıcı dışından cihaz modellerini yükleyecek şekilde yapılandırın. Önce Docker yapılandırma dosyasını açın:

    ```sh
    sudo nano /app/docker-compose.yml
    ```

    **Devicesimülasyon** kapsayıcısının ayarlarını bulun ve aşağıdaki kod parçacığında gösterildiği gibi **birimler** ayarını düzenleyin:

    ```yml
    devicesimulation:
      image: azureiotpcs/device-simulation-dotnet:1.0.0
      networks:
        - default_net
      depends_on:
        - storageadapter
      environment:
        - PCS_KEYVAULT_NAME
        - PCS_AAD_APPID
        - PCS_AAD_APPSECRET
      # How one could mount custom device models
      volumes:
        - /tmp/devicemodels:/app/webservice/data/devicemodels:ro
    ```

    Değişiklikleri kaydedin.

1. Varolan cihaz modeli dosyalarını kapsayıcıdan yeni konuma kopyalayın. İlk olarak, cihaz benzetim kapsayıcısının kapsayıcı KIMLIĞINI bulun:

    ```sh
    sudo docker ps
    ```

    Ardından, cihaz modeli dosyalarını sanal makinedeki **tmp** klasörüne kopyalayın. Aşağıdaki komut, kapsayıcı KIMLIĞININ c378d6878407 olduğunu varsayar; bu değeri cihaz benzetimi kapsayıcı KIMLIĞINIZLE değiştirin:

    ```sh
    sudo docker cp c378d6878407:/app/webservice/data/devicemodels /tmp
    sudo chown -R azureuser /tmp/devicemodels/
    ```

    SSH oturumunuzla **Bash** penceresini açık tutun.

1. Özel cihaz modeli dosyalarınızı sanal makineye kopyalayın. Bu komutu, özel cihaz modellerinizi oluşturduğunuz makinede başka bir **Bash** kabuğu 'nda çalıştırın. İlk olarak, cihaz modeli JSON dosyalarını içeren yerel klasöre gidin. Aşağıdaki komutlar, sanal makinenin genel IP adresinin **104.41.128.108** olduğunu varsayar. bu değeri, sanal MAKINENIZIN genel IP adresiyle değiştirin. İstendiğinde, sanal makine parolanızı girin:

    ```sh
    scp *json azureuser@104.41.128.108:/tmp/devicemodels
    cd scripts
    scp *js azureuser@104.41.128.108:/tmp/devicemodels/scripts
    ```

1. Yeni cihaz modellerini kullanmak için cihaz benzetimi Docker kapsayıcısını yeniden başlatın. Aşağıdaki komutları, sanal makineye açık SSH oturumu ile **Bash** kabuğu 'nda çalıştırın:

    ```sh
    sudo /app/start.sh
    ```

    Çalışan Docker kapsayıcıları ve kapsayıcı kimliklerinin durumunu görmek istiyorsanız aşağıdaki komutu kullanın:

    ```sh
    sudo docker ps
    ```

    Günlüğü cihaz simülasyon kapsayıcısından görmek istiyorsanız aşağıdaki komutu çalıştırın. Kapsayıcı KIMLIĞINI cihaz benzetim kapsayıcının KIMLIĞIYLE değiştirin:

    ```sh
    sudo docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>Simülasyonu çalıştır

Artık, uzaktan Izleme çözümünde özel cihaz modellerinizi kullanabilirsiniz:

1. [Microsoft Azure IoT çözüm hızlandırıcılarından](https://www.azureiotsolutions.com/Accelerators#dashboard)uzaktan izleme panonuzu başlatın.

1. Sanal cihazlar eklemek için **cihazlar** sayfasını kullanın. Yeni bir sanal cihaz eklediğinizde yeni cihaz modelleriniz seçmek için kullanılabilir.

1. Cihaz telemetrisini görüntülemek ve cihaz yöntemlerini çağırmak için panoyu kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla keşfetmeye devam ederseniz, uzaktan Izleme çözüm Hızlandırıcısını dağıtıldı olarak bırakın.

Artık çözüm hızlandırıcısına ihtiyacınız yoksa, bunu seçip **çözümü Sil**' e tıklayarak [sağlanan çözümler](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasından silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, uzaktan Izleme çözüm hızlandırıcısına özel cihaz modellerinin nasıl dağıtılacağını gösterdi. Önerilen sonraki adım, [gerçek bir cihazı uzaktan izleme çözümünüze nasıl bağlayacağınızı](iot-accelerators-connecting-devices-node.md)öğrenmeye yönelik olarak belirlenir.
