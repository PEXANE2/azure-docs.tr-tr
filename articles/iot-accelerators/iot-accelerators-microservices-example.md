---
title: Bir mikro hizmeti değiştirin ve yeniden dağıtın - Azure | Microsoft Dokümanlar
description: Bu öğretici, Uzaktan İzleme'de bir mikro hizmeti nasıl değiştireceğiniz ve yeniden dağıtabileceğinizi gösterir
author: dominicbetts
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 04/19/2018
ms.topic: conceptual
ms.openlocfilehash: 1552c54afe2195d58a032e9cc7bfa5aa70c844b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447650"
---
# <a name="customize-and-redeploy-a-microservice"></a>Bir mikro hizmeti özelleştirme ve yeniden dağıtma

Bu öğretici, Uzaktan İzleme çözümündeki [mikro hizmetlerden](https://azure.com/microservices) birini nasıl dizediğinizi, mikro hizmetinizin bir görüntüsünü oluşturmanızı, görüntüyü docker hub'ınıza nasıl dağıtabileceğinizi ve uzaktan izleme çözümünde nasıl kullanacağınızı gösterir. Bu kavramı tanıtmak için, öğretici bir microservice API arama ve "Alive and Well" dan "Yeni Edits Made Here için durum iletisi değiştirmek temel bir senaryo kullanır!"

Uzaktan İzleme çözümü, docker hub'ından çekilen docker görüntüleri kullanılarak oluşturulmuş mikro hizmetleri kullanır. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

>[!div class="checklist"]
> * Uzaktan İzleme çözümünde bir mikro hizmeti edin ve oluşturun
> * Docker görüntüsü oluşturma
> * Docker görüntüsünü docker hub'ınıza itin
> * Yeni docker görüntüsünü çekin
> * Değişiklikleri görselleştirin 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi takip etmek için şunları yapmanız gerekir:

>[!div class="checklist"]
> * [Uzaktan İzleme çözüm hızlandırıcısını yerel olarak dağıtma](iot-accelerators-remote-monitoring-deploy-local.md)
> * [Docker hesabı](https://hub.docker.com/)
> * [Postacı](https://www.getpostman.com/) - API yanıtını görüntülemek için gerekli

## <a name="call-the-api-and-view-response-status"></a>API'yi arayın ve yanıt durumunu görüntüleyin

Bu bölümde, varsayılan IoT hub yöneticisi microservice API'yi çağırırsınız. API, daha sonra microservice'i özelleştirerek değiştirdiğiniz bir durum iletisini döndürür.

1. Uzaktan İzleme çözümünüzün makinenizde yerel olarak çalıştırdığından emin olun.
2. Postacı'yı indirdiğiniz yeri bulun ve açın.
3. Postman olarak, GET aşağıdaki girin: `http://localhost:8080/iothubmanager/v1/status`.
4. Dönüşü görüntüleyin ve "Durum": "Ok:Alive and Well"i görmelisiniz.

    ![Canlı ve İyi Postacı Mesajı](./media/iot-accelerators-microservices-example/postman-alive-well.png)

## <a name="change-the-status-and-build-the-image"></a>Durumu değiştirin ve görüntüyü oluşturun

Şimdi Iot Hub Manager microservice'in durum iletisini "Burada Yapılan Yeni Edonlar!" olarak değiştirin. ve sonra bu yeni durumla docker görüntüsünü yeniden oluşturun. Burada sorunlarla karşılaştıysanız, [Sorun Giderme](#Troubleshoot) bölümümüze bakın.

1. Terminalinizin açık olduğundan emin olun ve Uzaktan İzleme çözümünü klonladığınız dizine değiştirin. 
1. Dizininizi "azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/Services" olarak değiştirin.
1. Beğendiğiniz herhangi bir metin düzenleyicisinde veya IDE'de StatusService.cs açın. 
1. Aşağıdaki kodu bulun:

    ```csharp
    var result = new StatusServiceModel(true, "Alive and well!");
    ```

    ve aşağıdaki koda değiştirin ve kaydedin.

    ```csharp
    var result = new StatusServiceModel(true, "New Edits Made Here!");
    ```

5. Terminalinize geri dön, ancak şimdi aşağıdaki dizine geçin: "azure-iot-pcs-remote-monitoring-dotnet/services/iothub-manager/scripts/docker".
6. Yeni docker resminizi oluşturmak için

    ```sh
    sh build
    ```
    
    veya Windows'da:
    
    ```cmd
    ./build.cmd
    ```

7. Yeni resminizin başarıyla oluşturulduğunu doğrulamak için

    ```cmd/sh
    docker images 
    ```

Depo "azureiotpcs/iothub-manager-dotnet" olmalıdır.

![Başarılı docker görüntüsü](./media/iot-accelerators-microservices-example/successful-docker-image.png)

## <a name="tag-and-push-the-image"></a>Görüntüyü etiketleme ve gönderme
Docker, yeni docker resminizi docker hub'ına itmeden önce resimlerinizin etiketlenmesini bekler. Burada sorunlarla karşılaştıysanız, [Sorun Giderme](#Troubleshoot) bölümümüze bakın.

1. Yazarak oluşturduğunuz docker görüntüsünün Resim Kimliğini bulun:

    ```cmd/sh
    docker images
    ```

2. Resminizi "test" türüyle etiketlemek için

    ```cmd/sh
    docker tag [Image ID] [docker ID]/iothub-manager-dotnet:testing 
    ```

3. Yeni etiketlenen resminizi docker hub'ınıza itmek için

    ```cmd/sh
    docker push [docker ID]/iothub-manager-dotnet:testing
    ```

4. İnternet tarayıcınızı açın ve [docker hub'ınıza](https://hub.docker.com/) gidin ve oturum açın.
5. Şimdi docker hub üzerinde yeni itti docker görüntü görmelisiniz.
![Docker hub'ında Docker görüntüsü](./media/iot-accelerators-microservices-example/docker-image-in-docker-hub.png)

## <a name="update-your-remote-monitoring-solution"></a>Uzaktan İzleme çözümünüzü güncelleyin
Docker hub'ınızdan yeni docker imajınızı çekmek için artık yerel docker-compose.yml'inizi güncellemeniz gerekir. Burada sorunlarla karşılaştıysanız, [Sorun Giderme](#Troubleshoot) bölümümüze bakın.

1. Terminale geri dön ve aşağıdaki dizine geç: "azure-iot-pcs-remote-monitoring-dotnet/services/scripts/local".
2. İstediğinherhangi bir metin düzenleyicisinde veya IDE'de docker-compose.yml'i açın.
3. Aşağıdaki kodu bulun:

    ```yml
    image: azureiotpcs/iothub-manager-dotnet:testing
    ```

    ve aşağıdaki resim gibi görünmesi için değiştirin ve kaydedin.

    ```yml
    image: [docker ID]/iothub-manager-dotnet:testing
    ```

## <a name="view-the-new-response-status"></a>Yeni yanıt durumunu görüntüleme
Uzaktan İzleme çözümünün yerel bir örneğini yeniden dağıtarak ve Postman'da yeni durum yanıtını görüntüleyerek bitirin.

1. Terminalinize geri dön ve aşağıdaki dizine geçiş yapın: "azure-iot-pcs-remote-monitoring-dotnet/scripts/local".
2. Terminale aşağıdaki komutu yazarak Uzaktan İzleme çözümünün yerel örneğini başlatın:

    ```cmd/sh
    docker-compose up
    ```

3. Postacı'yı indirdiğiniz yeri bulun ve açın.
4. Postacı'da GET'e aşağıdaki isteği `http://localhost:8080/iothubmanager/v1/status`girin: . Şimdi görmelisiniz, "Durum": "Tamam: Yeni Editler Burada Made!".

![Yeni Edneler Burada postacı mesajı made made](./media/iot-accelerators-microservices-example/new-postman-message.png)

## <a name="troubleshoot"></a><a name="Troubleshoot"></a>Sorun giderme

Sorunlarla karşılaşıyorsanız, yerel makinedeki docker görüntülerini ve konteynerleri kaldırmayı deneyin.

1. Tüm kapsayıcıları kaldırmak için öncelikle çalışan tüm kapsayıcıları durdurmanız gerekir. Terminalinizi açın ve yazın

    ```cmd/sh
    docker stop $(docker ps -aq)
    docker rm $(docker ps -aq)
    ```
    
2. Tüm görüntüleri kaldırmak için terminalinizi açın ve 

    ```cmd/sh
    docker rmi $(docker images -q)
    ```

3. Yazarak makinede herhangi bir kap olup olmadığını kontrol edebilirsiniz

    ```cmd/sh
    docker ps -aq 
    ```

    Tüm kapsayıcıları başarıyla kaldırdıysanız, hiçbir şey görünmemelidir.

4. Yazarak makinede herhangi bir görüntü olup olmadığını kontrol edebilirsiniz

    ```cmd/sh
    docker images
    ```

    Tüm kapsayıcıları başarıyla kaldırdıysanız, hiçbir şey görünmemelidir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl gördüğünüz:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Uzaktan İzleme çözümünde bir mikro hizmeti edin ve oluşturun
> * Docker görüntüsü oluşturma
> * Docker görüntüsünü docker hub'ınıza itin
> * Yeni docker görüntüsünü çekin
> * Değişiklikleri görselleştirin 

Denemek için bir sonraki şey [Uzaktan İzleme çözümünde cihaz simülatörü microservice özelleştirme](iot-accelerators-microservices-example.md)

Uzaktan İzleme çözümü hakkında daha fazla geliştirici bilgisi için bkz:

* [Geliştirici Başvuru Kılavuzu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
<!-- Next tutorials in the sequence -->

