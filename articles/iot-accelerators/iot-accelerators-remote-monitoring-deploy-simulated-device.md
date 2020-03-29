---
title: IoT özel simüle edilmiş aygıtları dağıtma - Azure | Microsoft Dokümanlar
description: Bu nasıl yapılılır kılavuzu, özel simüle edilmiş aygıtları Uzaktan İzleme çözüm hızlandırıcısına nasıl dağıtabileceğinizi gösterir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 08/15/2018
ms.topic: conceptual
ms.openlocfilehash: 7cbab38db859935c9f4490d79a131d6c9a7e302b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66427574"
---
# <a name="deploy-a-new-simulated-device"></a>Yeni bir simüle aygıtı dağıtma

Uzaktan İzleme ve Aygıt Simülasyonu çözüm hızlandırıcıları, kendi simüle edilmiş cihazlarınızı tanımlamanızı sağlar. Bu makalede, uzaktan izleme çözüm hızlandırıcısına özelleştirilmiş bir soğutucu aygıt türü ve yeni bir ampul aygıt türü nasıl dağıtılacağın bir gösteridir.

Bu makaledeki adımlar, Create'i tamamladığınızı [varsayar ve yeni bir simüle edilmiş aygıtın](iot-accelerators-remote-monitoring-create-simulated-device.md) nasıl yapılacağını test ettiğinizi ve özelleştirilmiş soğutucu ve yeni ampul aygıtı türlerini tanımlayan dosyalara sahip olduğunuzu varsayalım.

Bu nasıl yapılacağını gösteren bu kılavuzdaki adımlar nasıl yapılacağını gösterir:

1. Uzaktan İzleme çözüm hızlandırıcısını barındıran sanal makinenin dosya sistemine erişmek için SSH'yi kullanın.

1. Cihaz modellerini Docker kapsayıcısının dışındaki bir konumdan yükecek şekilde Docker'ı yapılandırın.

1. Özel aygıt modeli dosyalarını kullanarak Uzaktan İzleme çözüm hızlandırıcısını çalıştırın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için etkin bir Azure aboneliğine ihtiyacınız var.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını kılavuzunu takip etmek için şunları yapmanız gerekir:

- Uzaktan İzleme çözüm [hızlandırıcısının](https://www.azureiotsolutions.com/Accelerators#solutions/types/RM2)dağıtılmış bir örneği.
- Yerel bir **bash** kabuk `ssh` `scp` çalıştırmak ve komutları. Windows'da, **bash** yüklemek için kolay bir yolu [git](https://git-scm.com/download/win)yüklemektir.
- [Yeni bir benzetimli aygıt oluştur ve test](iot-accelerators-remote-monitoring-create-simulated-device.md)inde açıklananlar gibi özel aygıt modeli dosyalarınız.

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="configure-docker"></a>Docker'ı yapılandır

Bu bölümde, Docker'ı cihaz modeli dosyalarını Docker kapsayıcısının içinden değil, sanal makinedeki **/tmp/devicemodels** klasöründen yükleecek şekilde yapılandırAbilirsiniz. Bu bölümdeki komutları yerel makinenizde bir **bash** kabuğunda çalıştırın:

Bu bölümde, Docker'ı cihaz modeli dosyalarını Docker kapsayıcısının içinden değil, sanal makinedeki **/tmp/devicemodels** klasöründen yükleecek şekilde yapılandırAbilirsiniz. Bu bölümdeki komutları yerel makinenizde bir **bash** kabuğunda çalıştırın:

1. Azure'daki sanal makineye yerel makinenizden bağlanmak için SSH'yi kullanın. Aşağıdaki komut sanal makine **vm-vikxv** genel IP adresi **varsayar 104.41.128.108** - önceki bölümden sanal makinenin ortak IP adresi ile bu değeri değiştirin:

   ```sh
    ssh azureuser@104.41.128.108
    ```

    Önceki bölümde belirlediğiniz parolayla sanal makinede oturum açma istemlerini izleyin.

1. Cihaz simülasyon hizmetini, aygıt modellerini kapsayıcının dışından yüklemek için yapılandırın. Önce Docker yapılandırma dosyasını açın:

    ```sh
    sudo nano /app/docker-compose.yml
    ```

    **Aygıt simülasyon** kapsayıcısının ayarlarını bulun ve aşağıdaki parçacıkta gösterildiği gibi **birim** ayarını düzeltin:

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

1. Varolan aygıt modeli dosyalarını kapsayıcıdan yeni konuma kopyalayın. İlk olarak, aygıt simülasyon konteyneri için konteyner kimliğini bulun:

    ```sh
    sudo docker ps
    ```

    Ardından aygıt modeli dosyalarını sanal makinedeki **tmp** klasörüne kopyalayın. Aşağıdaki komut, konteyner kimliğinin c378d6878407 olduğunu varsayar -- bu değeri aygıt simülasyon konteyner kimliğinizle değiştirin:

    ```sh
    sudo docker cp c378d6878407:/app/webservice/data/devicemodels /tmp
    sudo chown -R azureuser /tmp/devicemodels/
    ```

    SSH oturumunuz açık olan **bash** penceresini açık tutun.

1. Özel aygıt model dosyalarınızı sanal makineye kopyalayın. Bu komutu, özel aygıt modellerinizi oluşturduğunuz makinede başka bir **bash** kabuğunda çalıştırın. İlk olarak, aygıt modeli JSON dosyalarını içeren yerel klasöre gidin. Aşağıdaki komutlar, sanal makinenin genel IP adresinin **104.41.128.108** olduğunu varsayar — bu değeri sanal makinenizin genel IP adresiyle değiştirin. İstendiğinde sanal makine şifrenizi girin:

    ```sh
    scp *json azureuser@104.41.128.108:/tmp/devicemodels
    cd scripts
    scp *js azureuser@104.41.128.108:/tmp/devicemodels/scripts
    ```

1. Yeni aygıt modellerini kullanmak için cihaz simülasyonu Docker konteynerini yeniden başlatın. Açık SSH oturumu yla **bash** kabuğundaki aşağıdaki komutları sanal makineye çalıştırın:

    ```sh
    sudo /app/start.sh
    ```

    Çalışan Docker kapsayıcılarının ve konteyner kimliklerinin durumunu görmek istiyorsanız, aşağıdaki komutu kullanın:

    ```sh
    sudo docker ps
    ```

    Aygıt simülasyon konteynerindeki günlüğü görmek istiyorsanız, aşağıdaki komutu çalıştırın. Konteyner kimliğini cihazınızın simülasyon kabının kimliğiyle değiştirin:

    ```sh
    sudo docker logs -f 5d3f3e78822e
    ```

## <a name="run-simulation"></a>Çalıştır simülasyonu

Artık özel cihaz modellerinizi Uzaktan İzleme çözümünde kullanabilirsiniz:

1. Uzaktan İzleme panonuzu [Microsoft Azure IoT Çözüm Hızlandırıcılarından](https://www.azureiotsolutions.com/Accelerators#dashboard)başlatın.

1. Benzetimli aygıtlar eklemek için **Cihazlar** sayfasını kullanın. Yeni bir simüle aygıt eklediğinizde, yeni cihaz modelleri seçmek için kullanılabilir.

1. Aygıt telemetrisini ve çağrı aygıtı yöntemlerini görüntülemek için panoyu kullanabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Daha fazla araştırma yapmayı planlıyorsanız, Uzaktan İzleme çözüm hızlandırıcısını dağıtılmış bırakın.

Artık çözüm hızlandırıcısına ihtiyacınız yoksa, çözüm seçerek ve ardından **Çözüm Sil'i** [tıklatarak, Verilen çözümler](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasından silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu kılavuz, uzaktan izleme çözüm hızlandırıcısına özel aygıt modellerini nasıl dağıtabileceğinizi göstermiştir. Önerilen bir sonraki adım, [gerçek bir aygıtı Uzaktan İzleme çözümünüze nasıl bağlayacaklarını](iot-accelerators-connecting-devices-node.md)öğrenmektir.
