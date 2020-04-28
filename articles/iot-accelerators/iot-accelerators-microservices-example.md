---
title: Mikro hizmeti değiştirme ve yeniden dağıtma-Azure | Microsoft Docs
description: Bu öğreticide, uzaktan Izlemede bir mikro hizmetin nasıl değiştirileceği ve yeniden dağıtılması gösterilmektedir
author: dominicbetts
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 04/19/2018
ms.topic: conceptual
ms.openlocfilehash: 1552c54afe2195d58a032e9cc7bfa5aa70c844b1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61447650"
---
# <a name="customize-and-redeploy-a-microservice"></a>Bir mikro hizmeti özelleştirme ve yeniden dağıtma

Bu öğreticide, uzaktan Izleme çözümünde [mikro hizmetlerden](https://azure.com/microservices) birini düzenleme, mikro hizmetinizin bir görüntüsünü oluşturma, görüntüyü Docker Hub 'ınıza dağıtma ve sonra uzaktan izleme çözümünde kullanma işlemlerinin nasıl yapılacağı gösterilir. Bu kavramı tanıtmak için öğretici, mikro hizmet API 'sini çağırdığınızda ve durum iletisini "canlı ve Iyi" olarak "yeni düzenlemeler" olarak değiştiren temel bir senaryoyu kullanır.

Uzaktan Izleme çözümü, bir Docker Hub 'ından çekilen Docker görüntüleri kullanılarak oluşturulan mikro hizmetleri kullanır. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Uzaktan Izleme çözümünde bir mikro hizmet düzenleme ve derleme
> * Docker görüntüsü oluşturma
> * Docker Hub 'ınıza bir Docker görüntüsü gönderin
> * Yeni Docker görüntüsünü çekme
> * Değişiklikleri görselleştirin 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi izlemek için şunlar gerekir:

>[!div class="checklist"]
> * [Uzaktan Izleme çözümü hızlandırıcıyı yerel olarak dağıtma](iot-accelerators-remote-monitoring-deploy-local.md)
> * [Bir Docker hesabı](https://hub.docker.com/)
> * [Postman](https://www.getpostman.com/) -API yanıtını görüntülemek için gereklidir

## <a name="call-the-api-and-view-response-status"></a>API 'YI çağırma ve yanıt durumunu görüntüleme

Bu bölümde, varsayılan IoT Hub Manager Mikro hizmet API 'sini çağırabilirsiniz. API, mikro hizmeti özelleştirerek daha sonra değiştirdiğiniz bir durum iletisi döndürür.

1. Uzaktan Izleme çözümünün makinenizde yerel olarak çalıştığından emin olun.
2. Postman 'ı indirdiğiniz yeri bulun ve açın.
3. Postman 'da GET: öğesine şunu girin: `http://localhost:8080/iothubmanager/v1/status`.
4. Dönüşü görüntüleyin, "durum": "Tamam: canlı ve Iyi" seçeneğini görmeniz gerekir.

    ![Canlı ve Iyi Postman Iletisi](./media/iot-accelerators-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Durumu değiştirme ve görüntüyü derleme

Şimdi IoT Hub Manager Mikro hizmetinin durum iletisini "yeni düzenlemeler yapıldı!" olarak değiştirin sonra Docker görüntüsünü bu yeni durumla yeniden derleyin. Burada sorunlarla karşılaşırsanız [sorun giderme](#Troubleshoot) bölümüne bakın.

1. Terminalizin açık olduğundan ve uzaktan Izleme çözümünü Klonladığınız dizine değişdiğinizden emin olun. 
1. Dizininizi "Azure-IoT-PCs-Remote-Monitoring-DotNet/Services/ıothub-Manager/Services" olarak değiştirin.
1. İstediğiniz herhangi bir metin düzenleyicisinde veya IDE 'de StatusService.cs açın. 
1. Aşağıdaki kodu bulun:

    ```csharp
    var result = new StatusServiceModel(true, "Alive and well!");
    ```

    ve bunu aşağıdaki kodla değiştirin ve kaydedin.

    ```csharp
    var result = new StatusServiceModel(true, "New Edits Made Here!");
    ```

5. Terminalinize geri dönün ancak şu dizine geçin: "Azure-IoT-PCs-Remote-Monitoring-DotNet/Services/ıothub-Manager/betikler/Docker".
6. Yeni Docker görüntünüzü oluşturmak için şunu yazın

    ```sh
    sh build
    ```
    
    veya Windows üzerinde:
    
    ```cmd
    ./build.cmd
    ```

7. Yeni görüntünüzün başarıyla oluşturulduğunu doğrulamak için, şunu yazın

    ```cmd/sh
    docker images 
    ```

Deponun "azureiotpcs/ıothub-Manager-DotNet" olması gerekir.

![Başarılı Docker görüntüsü](./media/iot-accelerators-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Görüntüyü etiketleme ve gönderme
Yeni Docker görüntünüzü bir Docker Hub 'ına gönderebilmeniz için Docker, görüntülerinizin etiketlenmesi bekler. Burada sorunlarla karşılaşırsanız [sorun giderme](#Troubleshoot) bölümüne bakın.

1. Yazarak oluşturduğunuz Docker görüntüsünün görüntü KIMLIĞINI bulun:

    ```cmd/sh
    docker images
    ```

2. Görüntünüzü "test" türüyle etiketlemek için

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Yeni etiketli görüntünüzü Docker Hub 'ınıza göndermek için şunu yazın

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. Internet tarayıcınızı açın ve [Docker Hub 'ınıza](https://hub.docker.com/) gidin ve oturum açın.
5. Artık Docker Hub 'ınızda yeni gönderilen Docker görüntünüzü görmeniz gerekir.
![Docker Hub 'da Docker görüntüsü](./media/iot-accelerators-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Uzaktan Izleme çözümünüzü güncelleştirme
Şimdi, yeni Docker görüntünüzü Docker Hub 'ından çekmek için yerel Docker-Compose. yıml 'nizi güncelleştirmeniz gerekir. Burada sorunlarla karşılaşırsanız [sorun giderme](#Troubleshoot) bölümüne bakın.

1. Terminale geri dönün ve şu dizine geçin: "Azure-IoT-PCs-Remote-Monitoring-DotNet/Services/betikler/Local".
2. İstediğiniz metin düzenleyicisinde veya IDE 'de Docker-Compose. yıml 'yi açın.
3. Aşağıdaki kodu bulun:

    ```yml
    image: azureiotpcs/iothub-manager-dotnet:testing
    ```

    ve bunu aşağıdaki görüntü gibi görünecek şekilde değiştirin ve kaydedin.

    ```yml
    image: [docker ID]/iothub-manager-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>Yeni yanıt durumunu görüntüleyin
Uzaktan Izleme çözümünün yerel bir örneğini yeniden dağıtarak ve yeni durum yanıtını Postman 'da görüntüleyerek son ' a ayarlayın.

1. Terminalinize geri dönün ve şu dizine geçin: "Azure-IoT-PCs-Remote-Monitoring-DotNet/KomutDosyaları/Local".
2. Terminale aşağıdaki komutu yazarak uzaktan Izleme çözümünün yerel örneğinizi başlatın:

    ```cmd/sh
    docker-compose up
    ```

3. Postman 'ı indirdiğiniz yeri bulun ve açın.
4. Postman 'da, GET: `http://localhost:8080/iothubmanager/v1/status`öğesine aşağıdaki isteği girin. Şimdi "durum": "Tamam: buradan yeni düzenlemeler yapıldı!" görmeniz gerekir.

![Burada yeni düzenlemeler Postman iletisinde yapıldı](./media/iot-accelerators-microservices-example/new-postman-message.png)

## <a name="troubleshoot"></a><a name="Troubleshoot"></a>Sorun giderme

Sorun yaşıyorsanız, yerel makinedeki Docker görüntülerini ve kapsayıcılarını kaldırmayı deneyin.

1. Tüm kapsayıcıları kaldırmak için öncelikle tüm çalışan kapsayıcıları durdurmanız gerekir. Terminalinizi açın ve yazın

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Tüm görüntüleri kaldırmak için terminalinizi açın ve yazın 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. Yazarak makinede herhangi bir kapsayıcı olup olmadığını kontrol edebilirsiniz.

    ```cmd/sh
    docker ps -aq 
    ```

    Tüm kapsayıcıları başarıyla kaldırdıysanız hiçbir şeyin gösterilmesi gerekir.

4. Yazarak makinede görüntü olup olmadığını kontrol edebilirsiniz.

    ```cmd/sh
    docker images
    ```

    Tüm kapsayıcıları başarıyla kaldırdıysanız hiçbir şeyin gösterilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Uzaktan Izleme çözümünde bir mikro hizmet düzenleme ve derleme
> * Docker görüntüsü oluşturma
> * Docker Hub 'ınıza bir Docker görüntüsü gönderin
> * Yeni Docker görüntüsünü çekme
> * Değişiklikleri görselleştirin 

Deneyebileceğiniz bir sonraki şey, [Uzaktan izleme çözümünde cihaz simülatörü mikro hizmetini özelleştiriliyor](iot-accelerators-microservices-example.md)

Uzaktan Izleme çözümü hakkında daha fazla geliştirici bilgisi için bkz.:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

