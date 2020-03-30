---
title: Visual Studio Code ile Java uygulamaları geliştirin
description: Bu makalede, Visual Studio Code kullanarak Java Hizmet Kumaşı uygulamalarının nasıl oluşturup dağıtılanın ve hata ayıklanınca gösterilmektedir.
author: peterpogorski
ms.topic: article
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 999dbb8c36c4e0413f287b2a73cf39ab4acd15f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75610055"
---
# <a name="develop-java-service-fabric-applications-with-visual-studio-code"></a>Visual Studio Code ile Java Servis Kumaşı uygulamaları geliştirin

[VS Code için Service Fabric Güvenilir Hizmetler uzantısı,](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) Windows, Linux ve macOS işletim sistemlerinde Java Service Fabric uygulamaları oluşturmayı kolaylaştırır.

Bu makalede, Visual Studio Code kullanarak bir Java Hizmet Kumaşı uygulamasının nasıl oluşturup dağıtılacağını ve hata ayıklamasını gösterir.

> [!IMPORTANT]
> Service Fabric Java uygulamaları Windows makinelerde geliştirilebilir, ancak yalnızca Azure Linux kümelerine dağıtılabilir. Java uygulamalarının hata ayıklanması Windows'da desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, VS Kodu, VS Kodu için Hizmet Kumaşı Güvenilir Hizmetler uzantısı ve geliştirme ortamınız için gerekli tüm bağımlılıkları zaten yüklediğiniz varsayar. Daha fazla bilgi için [bkz.](./service-fabric-get-started-vs-code.md#prerequisites)

## <a name="download-the-sample"></a>Örneği indirme
Bu makalede, Service Fabric Java uygulamasında Oylama uygulamasını hızlı [başlatma örneği GitHub deposunda](https://github.com/Azure-Samples/service-fabric-java-quickstart)kullanır. 

Depoyu geliştirme makinenize klonlamak için, terminal penceresinden (Windows'daki komut penceresi) aşağıdaki komutu çalıştırın:

```sh
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="open-the-application-in-vs-code"></a>VS Kodu'nda uygulamayı açın

VS Kodunu açın.  **Etkinlik Çubuğu'ndaki** Gezgin simgesine tıklayın ve **Klasörü Aç'ı**tıklatın veya **Dosya -> Klasörü Aç'ı**tıklatın. Depoyu klonladığınız klasördeki *./service-fabric-java-quickstart/Voting* dizinine gidin ve **tamam'ı**tıklatın. Çalışma alanı, aşağıdaki ekran görüntüsünde gösterilen aynı dosyaları içermelidir.

![Çalışma Alanında Java Oylama Uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/java-voting-application.png)

## <a name="build-the-application"></a>Uygulama oluşturma

1. VS Kodu'nda **Komut Paletini** açmak için (Ctrl + Shift + p) tuşuna basın.
2. Servis Dokusunu arayın ve **seçin: Uygulama** komutu oluşturun. Yapı çıktısı entegre terminale gönderilir.

   ![VS Kodunda Uygulama Komutu Oluştur](./media/service-fabric-develop-java-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Uygulamayı yerel kümeye dağıtma
Uygulamayı yaptıktan sonra yerel kümeye dağıtabilirsiniz. 

> [!IMPORTANT]
> Java uygulamalarını yerel kümeye dağıtmak Windows makinelerinde desteklenmez.

1. Komut **Paleti'nden,** **Hizmet Dokusu'nu seçin: Uygulamayı (Localhost) dağıt komutunu**dağıtın. Yükleme işleminin çıktısı entegre terminale gönderilir.

   ![VS Kodunda Uygulama Komutunu Dağıt](./media/service-fabric-develop-java-applications-with-vs-code/sf-deploy-application.png)

4. Dağıtım tamamlandığında, bir tarayıcı başlatın ve Hizmet `http://localhost:19080/Explorer`Kumaş Explorer'ı açın: . Uygulamanın çalıştığını görmeniz gerekir. Bu biraz zaman alabilir, bu yüzden sabırlı olun. 

   ![Hizmet Kumaş Explorer'da Oylama Uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/sfx-localhost-java.png)

4. Uygulamanın çalıştığını doğruladıktan sonra bir tarayıcı başlatın ve şu `http://localhost:8080`sayfayı açın: . Bu uygulamanın web ön ucudur. Öğeleri ekleyebilir ve oy vermek için üzerlerine tıklayabilirsiniz.

   ![Tarayıcıda Oylama Uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/voting-sample-in-browser.png)

5. Uygulamayı kümeden kaldırmak için **Hizmet Dokusu: Uygulama** komutunu Komut **Paleti'nden**kaldırın' ı seçin. Kaldırma işleminin çıktısı entegre terminale gönderilir. Uygulamanın yerel kümeden kaldırıldığını doğrulamak için Service Fabric Explorer'ı kullanabilirsiniz.

## <a name="debug-the-application"></a>Uygulamada hata ayıklama
VS Code'da hata ayıklama uygulamaları yaparken, uygulama yerel bir kümeüzerinde çalışıyor olmalıdır. Kesme noktaları daha sonra koda eklenebilir.

> [!IMPORTANT]
> Java uygulamaları nın hata ayıklanması Windows makinelerde desteklenmez.

OylamaDataHizmeti'ni ve hata ayıklama için Oylama uygulamasını hazırlamak için aşağıdaki adımları tamamlayın:

1. *Oylama/OylamaUygulaması/OylamaDataServicePkg/Code/entryPoint.sh* dosyasını güncelleyin.
Satır 6'daki komutu yorumlayın ('#'''yi kullanın) ve dosyanın altına aşağıdaki komutu ekleyin:

   ```
   java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingDataService.jar
   ```

2. *Oylama/OylamaUygulaması/ApplicationManifest.xml* dosyasını güncelleyin. **MinReplicaSetSize** ve **TargetReplicaSetSize** özniteliklerini **StatefulService** öğesinde "1" olarak ayarlayın:
   
   ```xml
         <StatefulService MinReplicaSetSize="1" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="1">
   ```

3. VS Kodu'nda hata ayıklayıcı görünümünü açmak için **Etkinlik Çubuğu'ndaki** Hata Ayıklama simgesini tıklatın. Hata ayıklama görüntüsünün üst kısmındaki vites simgesine tıklayın ve açılır ortam menüsünden **Java'yı** seçin. Launch.json dosyası açılır. 

   ![VS Kodu Çalışma Alanında Hata Ayıklama Simgesi](./media/service-fabric-develop-java-applications-with-vs-code/debug-icon-workspace.png)

3. launch.json dosyasında, **Hata Ayıklama (Ekle)** adlı yapılandırmadaki bağlantı noktası değerini **8001**olarak ayarlayın. Dosyayı kaydedin.

   ![launch.json için Hata Ayıklama Yapılandırması](./media/service-fabric-develop-java-applications-with-vs-code/launch-json-java.png)

4. **Hizmet Kumaşı: Uygulamayı Dağıt (Localhost)** komutunu kullanarak uygulamayı yerel kümeye dağıtın. Uygulamanın Service Fabric Explorer'da çalıştığını doğrulayın. Başvurunuz artık debugged hazırdır.

Bir kesme noktası ayarlamak için aşağıdaki adımları tamamlayın:

1. Explorer'da */Voting/VotingDataService/src/statefulservice/VotingDataService.java* dosyasını açın. Yöntemde `try` (satır 80) bloktaki ilk `addItem` kod satırına bir kesme noktası ayarlayın.
   
   ![Oylama Veri Hizmeti'nde kesme noktası ayarlama](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-set.png)

   > [!IMPORTANT]
   > Yürütülebilir kod satırlarına kesme noktaları ayarladığınızdan emin olun. Örneğin yöntem bildirimleri, `try` deyimler veya `catch` deyimler üzerinde ayarlanan kesme noktaları hata ayıklayıcı tarafından özlenecektir.
2. Hata ayıklamayı başlatmak için **Etkinlik Çubuğu'ndaki**Hata Ayıklama simgesini tıklatın, hata ayıklama menüsünden **Hata Ayıklama (Ekle)** yapılandırmasını seçin ve çalıştır düğmesini (yeşil ok) tıklatın.

   ![Hata Ayıklama (Ekle) Yapılandırması](./media/service-fabric-develop-java-applications-with-vs-code/debug-attach-java.png)

3. Web tarayıcısını kullanarak `http://localhost:8080` adresine gidin. Metin kutusuna yeni bir öğe yazın ve **+ Ekle'yi**tıklatın. Kırılma noktanız vurulmalı. Yürütmeyi devam etmek, satırları aşmak, yöntemlere adım atmak veya geçerli yöntemin dışına çıkmak için VS Kodu'nun üst kısmındaki Hata Ayıklama araç çubuğunu kullanabilirsiniz. 
   
   ![Hit kesme noktası](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-hit.png)
       
4. Hata ayıklama oturumunu sona erdirmek için, HATA Ayıklama araç çubuğundaki fiş simgesini VS Kodu'nun üst kısmında tıklatın.
   
   ![Hata ayıklamayla bağlantıyı kesme](./media/service-fabric-develop-java-applications-with-vs-code/debug-bar-disconnect.png)
       
5. Hata ayıklamayı tamamladığınızda, Oylama uygulamasını yerel kümenizden kaldırmak için **Hizmet Dokusu: Uygulama** komutunu kaldır komutunu kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* VS Code ile C# Service Fabric uygulamalarını nasıl [geliştirip hata ayıklayarak](./service-fabric-develop-csharp-applications-with-vs-code.md)hata ayıklamayı öğrenin.
