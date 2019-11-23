---
title: 'Hızlı başlangıç: Azure Service Fabric bir Java uygulaması oluşturma'
description: Bu hızlı başlangıçta, bir Service Fabric güvenilir hizmetler örnek uygulaması kullanarak Azure için Java uygulaması oluşturursunuz.
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/29/2019
ms.author: suhuruli
ms.custom: mvc, devcenter, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 4ef0db5f72f5849942bb043261f1166cf7c046b1
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703288"
---
# <a name="quickstart--deploy-a-java-app-to-azure-service-fabric-on-linux"></a>Hızlı başlangıç: Linux üzerinde Azure Service Fabric Java uygulaması dağıtma

Bu hızlı başlangıçta, bir Linux geliştirici makinesinde tutulma IDE 'yi kullanarak Azure Service Fabric ilk Java uygulamanızı nasıl dağıtacağınız gösterilmektedir. Bitirdiğinizde, oylama sonuçlarını kümedeki durum bilgisi içeren arka uç hizmetine kaydeden bir Java web ön ucuna sahip oylama uygulaması sağlanır.

Azure Service Fabric; mikro hizmetleri ve kapsayıcıları dağıtmayı ve yönetmeyi sağlayan bir dağıtılmış sistemler platformudur.

![Azure Service Fabric oylama örneği](./media/service-fabric-quickstart-java/service-fabric-voting-sample.png)

Bu hızlı başlangıçta şunları yapmayı öğrenirsiniz:

* Service Fabric Java uygulamalarınız için Eclipse’ı kullanma
* Uygulamayı yerel kümenize dağıtma
* Birden çok düğüm arasında uygulamanın ölçeğini genişletme

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

1. [Service Fabric SDK ve Service Fabric Komut Satırı Arabirimi’ni (CLI) yükleme](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [Git'i yükleyin](https://git-scm.com/)
3. [Eclipse’i yükleyin](https://www.eclipse.org/downloads/)
4. [Java Environment’ı ayarlayın](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development), Eclipse eklentisini yüklemek için isteğe bağlı adımları izlediğinizden emin olun

## <a name="download-the-sample"></a>Örneği indirme

Komut penceresinde, örnek uygulama deposunu yerel makinenize kopyalamak için aşağıdaki komutu çalıştırın.

```git
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

1. Aşağıdaki komutu çalıştırarak yerel kümenizi başlatın:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    Yerel kümenin başlatılması biraz zaman alabilir. Kümenin sorunsuz çalıştığından emin olmak için, **http://localhost:19080** adresinden Service Fabric Explorer’a erişin. Sorunsuz beş düğüm, yerel kümenin çalıştığını belirtir.

    ![Azure Service Fabric Explorer sağlıklı düğümleri gösterir](./media/service-fabric-quickstart-java/service-fabric-explorer-healthy-nodes.png)

2. Eclipse’i açın.
3. **Mevcut Gradle projesi** > **Dosya** > **içeri aktar** > **Gradle** ' ı seçin ve Sihirbazı izleyin.
4. **Dizin** ' i seçin ve GitHub ' dan kopyaladığınız `service-fabric-java-quickstart` klasöründen `Voting` dizini seçin. **Son**’u seçin.

    ![Gradle projesini tutulma 'ya aktar](./media/service-fabric-quickstart-java/eclipse-import-gradle-project.png)

5. Artık, Package Explorer for Eclipse’de `Voting` projesine sahipsiniz.
6. Projeye sağ tıklayın ve **Service Fabric** açılan menüsünde **uygulamayı Yayımla** ' yı seçin. **Publishprofiles/Local. JSON** öğesini hedef profil olarak seçin ve **Yayımla**' yı seçin.

    ![Azure Service Fabric yerel JSON yayımlama](./media/service-fabric-quickstart-java/service-fabric-publish-local-json.png)

7. En sevdiğiniz web tarayıcınızı açın ve `http://localhost:8080`erişerek uygulamaya erişin.

    ![Azure Service Fabric yerel ana bilgisayarı](./media/service-fabric-quickstart-java/service-fabric-local-host.png)

Şimdi bir dizi oylama seçeneği ekleyebilir ve oyları almaya başlayabilirsiniz. Uygulama çalıştırılır ve ayrı bir veritabanına gerek kalmadan tüm verileri Service Fabric kümenizde depolar.

## <a name="scale-applications-and-services-in-a-cluster"></a>Bir kümedeki uygulamaları ve hizmetleri ölçeklendirme

Hizmet yükündeki bir değişikliği karşılamak için kümedeki hizmetler kolayca ölçeklendirilebilir. Kümede çalıştırılan örnek sayısını değiştirerek bir hizmeti ölçeklendirebilirsiniz. Hizmetlerinizi ölçeklendirmenin birçok yolu vardır; örneğin, Service Fabric CLI’den (sfctl) betikler veya komutlar kullanabilirsiniz. Aşağıdaki adımlarda Service Fabric Explorer kullanılmaktadır.

Service Fabric Explorer tüm Service Fabric kümelerinde çalıştırılır ve tarayıcıdan kümelerin HTTP yönetim bağlantı noktasına (19080) göz atılarak (örneğin, `http://localhost:19080`) erişilebilir.

Web ön uç hizmetini ölçeklendirmek için aşağıdakileri yapın:

1. Kümenizde Service Fabric Explorer'ı açın. Örneğin: `https://localhost:19080`.
2. TreeView 'daki **Fabric:/oylama/VotingWeb** düğümünün yanındaki üç nokta ( **...** ) simgesini seçin ve **hizmeti Ölçeklendir**' i seçin.

    ![Azure Service Fabric bir hizmeti ölçeklendirin](./media/service-fabric-quickstart-java/service-fabric-scale-service.png)

    Şimdi web ön uç hizmetindeki örnek sayısını ölçeklendirebilirsiniz.

3. Sayıyı **2** olarak değiştirin ve **hizmeti Ölçeklendir**' i seçin.
4. Ağaç görünümünde **Fabric:/oylama/VotingWeb** düğümünü seçin ve bölüm DÜĞÜMÜNÜ (GUID ile gösterilir) genişletin.

    ![Azure Service Fabric ölçeklenmiş hizmet](./media/service-fabric-quickstart-java/service-fabric-explorer-service-scaled.png)

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
