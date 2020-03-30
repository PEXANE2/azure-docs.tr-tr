---
title: Windows'da Azure Service Fabric Linux kümesini ayarlama
description: Bu makalede, Windows geliştirme makinelerinde çalışan Service Fabric Linux kümelerinin nasıl kurulacağına ilişkindir. Bu özellikle çapraz platform geliştirme için yararlıdır.
author: suhuruli
ms.topic: conceptual
ms.date: 11/20/2017
ms.author: suhuruli
ms.openlocfilehash: 806e77a928d25e30aed24147525f74507bc32795
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75462985"
---
# <a name="set-up-a-linux-service-fabric-cluster-on-your-windows-developer-machine"></a>Windows geliştirici makinenizde bir Linux Service Fabric kümesi ayarlama

Bu belge, Windows geliştirme makinelerinde yerel bir Linux Hizmet Dokusunun nasıl kurulabildiğini kapsar. Yerel bir Linux kümesi kurmak, Linux kümeleri için hedeflenen ancak Windows makinesinde geliştirilen uygulamaları hızlı bir şekilde test etmek için yararlıdır.

## <a name="prerequisites"></a>Ön koşullar
Linux tabanlı Service Fabric kümeleri Windows'da yerel olarak çalışmaz. Yerel bir Service Fabric kümesini çalıştırmak için önceden yapılandırılmış docker kapsayıcı görüntüsü sağlanır. Başlamadan önce şunlar gereklidir:

* En az 4 GB RAM
* [Docker](https://store.docker.com/editions/community/docker-ce-desktop-windows)'ın en son sürümü
* Docker Linux modunda çalışıyor olmalı

>[!TIP]
> * Windows'unuza Docker yüklemek için resmi Docker [belgelerinde](https://store.docker.com/editions/community/docker-ce-desktop-windows/plans/docker-ce-desktop-windows-tier?tab=instructions) belirtilen adımları izleyebilirsiniz. 
> * Yüklemeyi tamamladıktan sonra, [burada](https://docs.docker.com/docker-for-windows/#check-versions-of-docker-engine-compose-and-machine) anlatılan adımları izleyerek yüklemeyi doğru yaptığınızı onaylayın


## <a name="create-a-local-container-and-setup-service-fabric"></a>Yerel bir kapsayıcı oluşturma ve Service Fabric’i ayarlama
Yerel bir Docker kapsayıcısı kurmak ve üzerinde bir hizmet kumaş kümesi nin çalışmasını sağlamak için PowerShell'de aşağıdaki adımları gerçekleştirin:


1. Ana bilgisayarınızda Docker daemon yapılandırmasını aşağıdakiyle güncelleştirin ve Docker daemon programını yeniden başlatın: 

    ```json
    {
      "ipv6": true,
      "fixed-cidr-v6": "2001:db8:1::/64"
    }
    ```
    Güncellemenin tavsiye edilen yolu - Docker Icon > Ayarları > Daemon >'a gidin ve orada güncelleyin. Ardından, değişikliklerin etkili olması için Docker daemon'u yeniden başlatın. 

2. Service Fabric Görüntünüzü derlemek için yeni bir dizinde `Dockerfile` adlı bir dosya oluşturun:

    ```Dockerfile
    FROM microsoft/service-fabric-onebox
    WORKDIR /home/ClusterDeployer
    RUN ./setup.sh
    #Generate the local
    RUN locale-gen en_US.UTF-8
    #Set environment variables
    ENV LANG=en_US.UTF-8
    ENV LANGUAGE=en_US:en
    ENV LC_ALL=en_US.UTF-8
    EXPOSE 19080 19000 80 443
    #Start SSH before running the cluster
    CMD /etc/init.d/ssh start && ./run.sh
    ```

    >[!NOTE]
    >Kapsayıcınıza ek programlar veya bağımlılıklar eklemek için bu dosyayı uyarlayabilirsiniz.
    >Örneğin, `RUN apt-get install nodejs -y` komutu eklendiğinde, konuk yürütülebilir dosyaları olarak `nodejs` uygulamaları için destek sağlanır.
    
    >[!TIP]
    > Varsayılan olarak bu, görüntüyü Service Fabric’in en son sürümüyle çeker. Belirli düzeltmeler için lütfen [Docker Hub](https://hub.docker.com/r/microsoft/service-fabric-onebox/) sayfasını ziyaret edin

3. `Dockerfile` içinden yeniden kullanılabilir görüntünüzü derlemek için bir terminal açın ve doğrudan `Dockerfile` öğesini tutarak `cd` uygulayıp sonra şunu çalıştırın:

    ```powershell 
    docker build -t mysfcluster .
    ```
    
    >[!NOTE]
    >Bu işlem biraz zaman alır, ancak yalnızca bir kez gereklidir.

4. Şimdi aşağıdakini çalıştırarak her ihtiyaç duyduğunuzda hızla yerel bir Service Fabric kopyası başlatabilirsiniz:

    ```powershell 
    docker run --name sftestcluster -d -v //var/run/docker.sock:/var/run/docker.sock -p 19080:19080 -p 19000:19000 -p 25100-25200:25100-25200 mysfcluster
    ```

    >[!TIP]
    >Kapsayıcı örneğiniz için bir ad belirterek, örneğinizin daha okunaklı bir biçimde işlenebilmesini sağlayın. 
    >
    >Uygulamanız belirli bağlantı noktalarını dinliyorsa, bağlantı noktaları ek `-p` etiketleri kullanılarak belirtilmelidir. Örneğin, uygulamanız 8080 bağlantı noktasını dinliyorsa, şuradaki `-p` etiketini ekleyin:
    >
    >`docker run -itd -p 19080:19080 -p 8080:8080 --name sfonebox microsoft/service-fabric-onebox`
    >

5. Küme başlatmak için kısa bir süre alacak, aşağıdaki komutu kullanarak günlükleri görüntüleyebilir veya kümeler [http://localhost:19080](http://localhost:19080)sağlık görüntülemek için panoya atlamak:

    ```powershell 
    docker logs sftestcluster
    ```

6. Adım 5 başarıyla tamamlandıktan sonra, ``http://localhost:19080`` Windows'unuzdan gidebilirsiniz ve Service Fabric gezginini görebilirsiniz. Bu noktada, Windows geliştirici makinenizden herhangi bir aracı kullanarak bu kümeye bağlanabilir ve Linux Service Fabric kümeleri için hedeflenen uygulamayı dağıtabilirsiniz. 

    > [!NOTE]
    > Eclipse eklentisi Windows üzerinde şu anda desteklenmemektedir. 

7. İşi bittiğinde, bu komutla kapsayıcıyı durdurup temizleyin:

    ```powershell 
    docker rm -f sftestcluster
    ```

### <a name="known-limitations"></a>Bilinen Sınırlamalar 
 
 Mac’e yönelik kapsayıcıdaki yerel küme çalıştırmaya ilişkin bilinen sınırlandırmalar aşağıda verilmiştir: 
 
 * DNS hizmeti çalışmıyor ve desteklenmiyor [Sorun No. 132](https://github.com/Microsoft/service-fabric/issues/132)

## <a name="next-steps"></a>Sonraki adımlar
* [Eclipse](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-eclipse) ile başlayın
* Diğer [Java örneklerine](https://github.com/Azure-Samples/service-fabric-java-getting-started) göz atın


<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
