---
title: Özel bir Aygıt Simülasyonu görüntüsü dağıtma - Azure| Microsoft Dokümanlar
description: Bu nasıl yapılmalı kılavuzunda, Aygıt Simülasyonu çözümünün özel docker görüntüsünü Azure'a nasıl dağıtabileceğinizi öğrenirsiniz.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: c1f321f452b65016c11cb66d08ebab108509cc62
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61448411"
---
# <a name="deploy-a-custom-device-simulation-docker-image"></a>Özel bir Aygıt Simülasyonu docker görüntüsü dağıtma

Özel özellikler eklemek için Aygıt Simülasyonu çözümlerini değiştirebilirsiniz. Örneğin, [Protokol Arabellekleri makalesini kullanarak Serialize telemetrisi,](iot-accelerators-device-simulation-protobuf.md) telemetri göndermek için Protokol Arabellekleri (Protobuf) kullanan çözüme nasıl özel bir aygıt ekleyeceğinizi gösterir. Değişikliklerinizi yerel olarak test ettikten sonra, bir sonraki adım değişikliklerinizi Azure'daki Aygıt Simülasyonu örneğine dağıtmaktır. Bu görevi tamamlamak için, değiştirilmiş hizmetinizi içeren bir Docker görüntüsü oluşturmanız ve dağıtmanız gerekir.

Bu nasıl yapılacağını gösteren adımlar, nasıl yapılacağını gösterir:

1. Geliştirme ortamı hazırlama
1. Yeni bir Docker görüntüsü oluşturma
1. Yeni Docker resminizi kullanmak için Aygıt Simülasyonunu yapılandırın
1. Yeni görüntüyü kullanarak simülasyon çalıştırın

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapIlen kılavuzundaki adımları tamamlamak için şunları yapmanız gerekir:

* Dağıtılan [aygıt simülasyon](quickstart-device-simulation-deploy.md) örneği.
* Docker, ne zaman? Platformunuz için [Docker Community Edition'ı](https://www.docker.com/products/docker-engine#/download) indirin.
* Docker resimlerinizi yükleyebileceğiniz bir [Docker Hub hesabı.](https://hub.docker.com/) Docker Hub hesabınızda, **aygıt simülasyonu**adı verilen genel bir depo oluşturun.
* Yerel makinenizde değiştirilmiş ve test edilmiş [aygıt simülasyonu çözümü.](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) Örneğin, [Protokolü Arabellekleri kullanarak telemetriyi Serihale](iot-accelerators-device-simulation-protobuf.md)için çözümü değiştirebilirsiniz.
* SSH çalıştırabilen bir kabuk. Windows için Git'i yüklerseniz, yüklemenin bir parçası olan **bash** kabuğunu kullanabilirsiniz. [Azure Bulut Shell'inizi](https://shell.azure.com/)de kullanabilirsiniz.

Bu makaledeki yönergeler Windows kullandığınızı varsayar. Başka bir işletim sistemi kullanıyorsanız, bazı dosya yollarını ve komutlarını ortamınıza uyacak şekilde ayarlamanız gerekebilir.

## <a name="create-a-new-docker-image"></a>Yeni bir Docker görüntüsü oluşturma

Aygıt Simülasyonu hizmetine kendi değişikliklerinizi dağıtmak için, kapsayıcıları docker-hub hesabınıza yüklemek için **komut dosyaları\docker** klasöründe yapı ve dağıtım komut dosyalarını değiştirmeniz gerekir

### <a name="modify-the-docker-scripts"></a>Docker komut dosyalarını değiştirme

Docker Hub depo bilgilerinizle Docker **build.cmd,** **publish.cmd**ve **run.cmd** komut **dosyalarını değiştirin.cmd** komut dosyaları. Bu **adımlar, aygıt simülasyonu**adı verilen genel bir depo oluşturduğunuzu varsayar:

`DOCKER_IMAGE={your-docker-hub-username}/device-simulation`

**Docker-compose.yml** dosyasını aşağıdaki gibi güncelleştirin:

`image: {your-docker-hub-username}/device-simulation`

### <a name="configure-the-solution-to-include-any-new-files"></a>Çözümü yeni dosyaları içerecek şekilde yapılandırın

Yeni aygıt modeli dosyaları eklediyseniz, bunları çözüme açıkça eklemeniz gerekir. Eklenecek her ek dosya için **hizmetlere/services.csproj'a** bir giriş ekleyin. Örneğin, Protokol Arabellekleri nasıl yapılsa nasıl yapılsa, [Serialize telemetrisini](iot-accelerators-device-simulation-protobuf.md) tamamladıysanız, aşağıdaki girişleri ekleyin:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

### <a name="generate-new-docker-images-and-push-to-docker-hub"></a>Yeni Docker görüntüleri oluşturun ve Docker Hub'a itin

Docker-hub hesabınızı kullanarak yeni Docker görüntüsünü Docker Hub'da yayımlayın:

1. Bir komut istemi açın ve aygıt simülasyon deposunun yerel kopyasına gidin.

1. **Docker** klasörüne gidin:

    ```cmd
    cd scripts\docker
    ```

1. Docker görüntüsünü oluşturmak için aşağıdaki komutu çalıştırın:

    ```cmd
    build.cmd
    ```

1. Docker görüntüsünü Docker Hub deponuza yayınlamak için aşağıdaki komutu çalıştırın. Docker Hub kimlik bilgilerinizle Docker ile oturum açın:

    ```cmd
    docker login
    publish.cmd
    ```

<!-- TODO fix heading levels working include -->

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="update-the-service"></a>Hizmeti güncelleştirme

Özel resminizi kullanmak için Aygıt Simülasyonu kapsayıcısını güncelleştirmek için aşağıdaki adımları tamamlayın:

* Aygıt Simülasyonu örneğini barındıran sanal makineye bağlanmak için SSH'yi kullanın. Bir önceki bölümde not aldığınız IP adresini ve parolasını kullanın:

    ```sh
    ssh azureuser@{your vm ip address}
    ```

* **/app** dizinine gidin:

    ```sh
    cd /app
    ```

* **Docker-compose.yml** dosyasını edin:

    ```sh
    sudo nano docker-compose.yml
    ```

    Docker Hub deponuza yüklediğiniz özel **aygıt simülasyonu** görüntüsünü işaret etmek için **resmi** değiştirin:

    ```yml
    image: {your-docker-hub-username}/device-simulation
    ```

    Yaptığınız değişiklikleri kaydedin.

* Mikro hizmetleri yeniden başlatmak için aşağıdaki komutu çalıştırın:

    ```sh
    sudo start.sh
    ```

## <a name="run-your-simulation"></a>Simülasyonunuzu Çalıştırın

Artık özelleştirilmiş Aygıt Simülasyonu çözümünüzü kullanarak bir simülasyon çalıştırabilirsiniz:

1. [Microsoft Azure IoT Çözüm Hızlandırıcılarından](https://www.azureiotsolutions.com/Accelerators#dashboard)Aygıt Simülasyonu web UI'nizi başlatın.

1. Bir simülasyonu yapılandırmak ve çalıştırmak için web UI'yi kullanın. [Protokol Arabellekleri kullanarak Serialize telemetrisini](iot-accelerators-device-simulation-protobuf.md)daha önce tamamladıysanız, özel aygıt modelinizi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şimdi nasıl özel bir Aygıt Simülasyonu görüntü dağıtmak için öğrendim, [Nasıl Aygıt Simülasyonu çözüm hızlandırıcı ile mevcut bir IoT hub kullanmayı](iot-accelerators-device-simulation-choose-hub.md)öğrenmek isteyebilirsiniz.