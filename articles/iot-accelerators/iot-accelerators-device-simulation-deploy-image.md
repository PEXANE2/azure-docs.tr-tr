---
title: Özel cihaz simülasyonu görüntüsü dağıtma-Azure | Microsoft Docs
description: Bu nasıl yapılır kılavuzunda, cihaz benzetimi çözümünün özel bir Docker görüntüsünü Azure 'a dağıtmayı öğreneceksiniz.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/06/2018
ms.author: dobett
ms.openlocfilehash: c1f321f452b65016c11cb66d08ebab108509cc62
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61448411"
---
# <a name="deploy-a-custom-device-simulation-docker-image"></a>Özel cihaz benzetimi Docker görüntüsü dağıtma

Özel özellikler eklemek için cihaz benzetimi çözümünü değiştirebilirsiniz. Örneğin, [protokol arabellekleri kullanarak Telemetriyi serileştirme](iot-accelerators-device-simulation-protobuf.md) makalesinde, telemetri göndermek Için protokol arabellekleri (protobellek) kullanan çözüme nasıl özel bir cihaz ekleyeceğiniz gösterilmektedir. Değişikliklerinizi yerel olarak test ettikten sonra, bir sonraki adım Azure 'daki cihaz simülasyonu Örneğinizde yaptığınız değişiklikleri dağıtmaktır. Bu görevi gerçekleştirmek için, değiştirdiğiniz hizmeti içeren bir Docker görüntüsü oluşturmanız ve dağıtmanız gerekir.

Bu nasıl yapılır kılavuzundaki adımlarda şu adımları nasıl kullanabileceğiniz gösterilmektedir:

1. Geliştirme ortamı hazırlama
1. Yeni bir Docker görüntüsü oluştur
1. Yeni Docker görüntünüzü kullanmak için cihaz simülasyonu yapılandırma
1. Yeni görüntüyü kullanarak benzetim çalıştırma

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için şunlar gerekir:

* Dağıtılan bir [cihaz benzetim](quickstart-device-simulation-deploy.md) örneği.
* Docker. Platformunuz için [Docker Community Edition](https://www.docker.com/products/docker-engine#/download) 'ı indirin.
* Docker görüntülerinizi karşıya yükleyebileceğiniz bir [Docker Hub hesabı](https://hub.docker.com/) . Docker Hub hesabınızda, **cihaz benzetimi**adlı ortak bir depo oluşturun.
* Yerel makinenizde değiştirilmiş ve test edilmiş bir [cihaz benzetimi çözümü](https://github.com/Azure/device-simulation-dotnet/archive/master.zip) . Örneğin, [protokol arabellekleri kullanarak telemetri serileştirme](iot-accelerators-device-simulation-protobuf.md)için çözümü değiştirebilirsiniz.
* SSH çalıştıran bir kabuk. Windows Için git 'i yüklerseniz, yükleme 'nin bir parçası olan **Bash** kabuğunu kullanabilirsiniz. [Azure Cloud Shell](https://shell.azure.com/)de kullanabilirsiniz.

Bu makaledeki yönergeler Windows kullandığınızı varsayar. Başka bir işletim sistemi kullanıyorsanız, ortamınıza uyacak şekilde bazı dosya yollarını ve komutları ayarlamanız gerekebilir.

## <a name="create-a-new-docker-image"></a>Yeni bir Docker görüntüsü oluşturma

Kendi değişikliklerinizi cihaz benzetimi hizmetine dağıtmak için, **scripts\docker** klasöründeki derleme ve Dağıtım betiklerini düzenleyerek kapsayıcıları Docker-Hub hesabınıza yükleyebilirsiniz

### <a name="modify-the-docker-scripts"></a>Docker betiklerini değiştirme

Docker **Build. cmd**, **Publish. cmd**dosyasını değiştirin ve **. cmd** komut dosyalarını, Docker Hub depo bilgileriniz ile **scripts\docker** klasöründe çalıştırın. Bu adımlarda, **cihaz benzetimi**adlı bir genel depo oluşturdunuz:

`DOCKER_IMAGE={your-docker-hub-username}/device-simulation`

**Docker-Compose. yml** dosyasını aşağıdaki gibi güncelleştirin:

`image: {your-docker-hub-username}/device-simulation`

### <a name="configure-the-solution-to-include-any-new-files"></a>Çözümü yeni dosya içerecek şekilde yapılandırma

Yeni bir cihaz modeli dosyası eklediyseniz, bunları çözüme açıkça eklemeniz gerekir. Dahil edilecek her ek dosya için **Services/Services. csproj** öğesine bir giriş ekleyin. Örneğin, [protokol arabelleklerini kullanarak serileştirme telemetrisini](iot-accelerators-device-simulation-protobuf.md) tamamladıysanız, aşağıdaki girişleri ekleyin:

```xml
<None Update="data\devicemodels\assettracker-01.json">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
<None Update="data\devicemodels\scripts\assettracker-01-state.js">
    <CopyToOutputDirectory>Always</CopyToOutputDirectory>
</None>
```

### <a name="generate-new-docker-images-and-push-to-docker-hub"></a>Yeni Docker görüntüleri oluşturma ve Docker Hub 'a gönderme

Docker Hub hesabınızı kullanarak yeni Docker görüntüsünü Docker Hub 'a yayımlayın:

1. Bir komut istemi açın ve cihaz simülasyon deposunun yerel kopyasına gidin.

1. **Docker** klasörüne gidin:

    ```cmd
    cd scripts\docker
    ```

1. Docker görüntüsünü derlemek için aşağıdaki komutu çalıştırın:

    ```cmd
    build.cmd
    ```

1. Docker görüntüsünü Docker Hub deponuza yayımlamak için aşağıdaki komutu çalıştırın. Docker Hub kimlik bilgilerinizle Docker 'da oturum açın:

    ```cmd
    docker login
    publish.cmd
    ```

<!-- TODO fix heading levels working include -->

[!INCLUDE [iot-solution-accelerators-access-vm](../../includes/iot-solution-accelerators-access-vm.md)]

## <a name="update-the-service"></a>Hizmeti güncelleştirme

Özel görüntünüzü kullanmak üzere cihaz benzetimi kapsayıcısını güncelleştirmek için aşağıdaki adımları izleyin:

* Cihaz benzetim örneğinizi barındıran sanal makineye bağlanmak için SSH kullanın. Önceki bölümde ' ın bulunduğu IP adresini ve parolayı kullanın:

    ```sh
    ssh azureuser@{your vm ip address}
    ```

* **/App** dizinine gidin:

    ```sh
    cd /app
    ```

* **Docker-Compose. yml** dosyasını düzenleyin:

    ```sh
    sudo nano docker-compose.yml
    ```

    **Resmi** , Docker Hub deponuza yüklediğiniz özel **cihaz benzetimi** görüntüsünü işaret etmek üzere değiştirin:

    ```yml
    image: {your-docker-hub-username}/device-simulation
    ```

    Yaptığınız değişiklikleri kaydedin.

* Mikro hizmetleri yeniden başlatmak için aşağıdaki komutu çalıştırın:

    ```sh
    sudo start.sh
    ```

## <a name="run-your-simulation"></a>Simülasyonu çalıştırın

Artık özelleştirilmiş cihaz benzetim çözümünüzü kullanarak bir benzetim çalıştırabilirsiniz:

1. [Microsoft Azure IoT çözüm hızlandırıcılarından](https://www.azureiotsolutions.com/Accelerators#dashboard)cihaz benzetimi Web Kullanıcı arabirimini başlatın.

1. Bir simülasyonu yapılandırmak ve çalıştırmak için Web Kullanıcı arabirimini kullanın. Daha önce [protokol arabellekleri kullanarak serileştirme telemetrisini](iot-accelerators-device-simulation-protobuf.md)tamamladıysanız özel cihaz modelinizi kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Özel bir cihaz benzetimi görüntüsünü dağıtmayı öğrendiniz, [cihaz benzetimi çözüm hızlandırıcısında mevcut bir IoT Hub 'ını](iot-accelerators-device-simulation-choose-hub.md)nasıl kullanacağınızı öğrenmek isteyebilirsiniz.