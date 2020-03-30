---
title: "Quickstart: Azure Hizmet Kumaşı'nda Java uygulaması oluşturma"
description: Bu hızlı başlangıçta, bir Service Fabric güvenilir hizmetler örnek uygulaması kullanarak Azure için Java uygulaması oluşturursunuz.
author: suhuruli
ms.topic: quickstart
ms.date: 01/29/2019
ms.author: suhuruli
ms.custom: mvc, devcenter, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: fc615149b092aebfdde767fb3b716fb897bfd551
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77121493"
---
# <a name="quickstart--deploy-a-java-app-to-azure-service-fabric-on-linux"></a>Quickstart: Linux'ta Azure Hizmet Dokusu'na Java uygulaması dağıtma

Bu hızlı başlangıçta, Bir Linux geliştirici makinesinde Eclipse IDE'yi kullanarak Azure Hizmet Kumaşı'na bir Java uygulaması dağıtabilirsiniz. Bitirdiğinizde, oylama sonuçlarını kümedeki durum bilgisi içeren arka uç hizmetine kaydeden bir Java web ön ucuna sahip oylama uygulaması sağlanır.

Azure Service Fabric; mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur.

## <a name="prerequisites"></a>Ön koşullar

- [Java çevre](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development) ve [Yeoman](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-yeoman-generators-for-containers-and-guest-executables)
- [Eclipse Neon (4.6)+](https://www.eclipse.org/downloads/packages/) ve [Eclipse plug-in Servis Kumaş için](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#install-the-eclipse-plug-in-optional)
- [Servis Kumaş ı SDK ve Komut Hattı Arabirimi (CLI)](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
- [Git](https://git-scm.com/downloads)

## <a name="download-the-sample"></a>Örneği indirme

Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

1. Aşağıdaki komutu çalıştırarak yerel kümenizi başlatın:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Yerel kümenin başlatılması biraz zaman alabilir. Kümenin sorunsuz çalıştığından emin olmak için, `http://localhost:19080` adresinden Service Fabric Explorer’a erişin. Sorunsuz beş düğüm, yerel kümenin çalıştığını belirtir.

    ![Azure Hizmet Kumaş Explorer sağlıklı düğümleri gösterir](./media/service-fabric-quickstart-java/service-fabric-explorer-healthy-nodes.png)

2. Eclipse’i açın.
3. **Dosya** > **Alma** > **Gradle** > **Varolan Gradle Projesi'ni** seçin ve sihirbazı izleyin.
4. **Dizin'i** seçin ve GitHub'dan klonladığınız **hizmet-kumaş-java-quickstart** klasöründen **Oylama** dizinini seçin. **Bitiş'i**seçin.

    ![Eclipse içine Gradle proje İthalat](./media/service-fabric-quickstart-java/eclipse-import-gradle-project.png)

5. Artık, Package Explorer for Eclipse’de `Voting` projesine sahipsiniz.
6. Projeye sağ tıklayın ve **Hizmet Kumaşı** açılır altında **Uygulamayı Yayımla'yı** seçin. Hedef Profil olarak **PublishProfiles/Local.json'u** seçin ve **Yayımla'yı**seçin.

    ![Azure Hizmet Kumaşı yerel JSON yayımlıyor](./media/service-fabric-quickstart-java/service-fabric-publish-local-json.png)

7. Sık kullandığınız web tarayıcısını açın ve`http://localhost:8080` adresine giderek uygulamaya erişin.

    ![Azure Hizmet Kumaşı yerel ana bilgisayar](./media/service-fabric-quickstart-java/service-fabric-local-host.png)

Şimdi bir dizi oylama seçeneği ekleyebilir ve oyları almaya başlayabilirsiniz. Uygulama çalıştırılır ve ayrı bir veritabanına gerek kalmadan tüm verileri Service Fabric kümenizde depolar.

![Azure Hizmet Kumaşı oylama örneği](./media/service-fabric-quickstart-java/service-fabric-voting-sample.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme

Hizmet yükündeki bir değişikliği karşılamak için kümedeki hizmetler kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizi ölçeklemenin birçok yolu vardır. Örneğin, Service Fabric CLI ()`sfctl`komutları veya komutları kullanabilirsiniz. Aşağıdaki adımlarda Service Fabric Explorer kullanılmaktadır.

Service Fabric Explorer tüm Service Fabric kümelerinde çalışır ve kümenin HTTP yönetim bağlantı noktasına (19080) göz atarak bir tarayıcıdan erişilebilir. Örneğin, `http://localhost:19080`.

Web ön uç hizmetini ölçeklendirmek için aşağıdakileri yapın:

1. Kümenizde Servis Kumaş Explorer'ı açın. Örneğin, `https://localhost:19080`.
2. Kumaşın yanındaki elipsleri (**...**) **seçin:/Oylama/OylamaWeb** düğümü ağaç görünümünde ve **Ölçek Hizmeti'ni**seçin.

    ![Azure Hizmet Kumaşı'nda bir hizmeti ölçeklendirin](./media/service-fabric-quickstart-java/service-fabric-scale-service.png)

    Şimdi web ön uç hizmetindeki örnek sayısını ölçeklendirebilirsiniz.

3. Sayıyı **2** olarak değiştirin ve **Ölçek Hizmeti'ni**seçin.
4. **Kumaşı seçin:/Ağaç görünümünde Oy/VotingWeb** düğümü ve bölüm düğümünü genişletin (GUID tarafından temsil edilir).

    ![Azure Hizmet Kumaşı'nda ölçeklenmiş hizmet](./media/service-fabric-quickstart-java/service-fabric-explorer-service-scaled.png)

    Hizmette üç örnek olduğunu ve ağaç görünümünde örneklerin çalıştığı düğümleri görebilirsiniz.

Bu basit yönetim görevi sayesinde ön uç hizmetinin kullanıcı yükünü işlemek için kullanabileceği kaynakları iki katına çıkarmış oldunuz. Bir hizmetin güvenilir bir şekilde çalışması için birden fazla örneğe ihtiyaç duymadığınızı anlamanız önemlidir. Bir hizmet başarısız olursa Service Fabric, kümede yeni bir hizmet örneği çalışmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunları öğrendiniz:

* Service Fabric Java uygulamalarınız için Eclipse’ı kullanma
* Java uygulamalarını yerel kümenize dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme

Service Fabric’te Java uygulamalarıyla çalışma hakkında daha fazla bilgi için Java uygulamaları öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Java uygulaması dağıtma](./service-fabric-tutorial-create-java-app.md)
