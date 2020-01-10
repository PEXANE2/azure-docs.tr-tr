---
title: Visual Studio Code ile Java uygulamaları geliştirme
description: Bu makalede, Visual Studio Code kullanılarak Java Service Fabric uygulamalarının nasıl oluşturulacağı, dağıtılacağı ve hata ayıklamanın nasıl yapılacağı gösterilir.
author: peterpogorski
ms.topic: article
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 999dbb8c36c4e0413f287b2a73cf39ab4acd15f5
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75610055"
---
# <a name="develop-java-service-fabric-applications-with-visual-studio-code"></a>Visual Studio Code ile Java Service Fabric uygulamaları geliştirme

[Vs Code için Service Fabric Reliable Services uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) , Windows, Linux ve MacOS Işletim sistemlerinde Java Service Fabric uygulamaları oluşturmayı kolaylaştırır.

Bu makalede Visual Studio Code kullanarak bir Java Service Fabric uygulamasının nasıl oluşturulacağı, dağıtılacağı ve hata ayıklamanın nasıl yapılacağı gösterilir.

> [!IMPORTANT]
> Service Fabric Java uygulamaları Windows makinelerde geliştirilebilir, ancak yalnızca Azure Linux kümelerine dağıtılabilir. Java uygulamalarında hata ayıklama Windows üzerinde desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, VS Code için Service Fabric Reliable Services uzantısı ve geliştirme ortamınız için gereken tüm bağımlılıklar VS Code, zaten yüklemiş olduğunuz varsayılır. Daha fazla bilgi için bkz [. Başlarken](./service-fabric-get-started-vs-code.md#prerequisites).

## <a name="download-the-sample"></a>Örneği indirme
Bu makale, [Service Fabric Java uygulama hızlı başlangıç örneği GitHub deposunda](https://github.com/Azure-Samples/service-fabric-java-quickstart)oylama uygulamasını kullanır. 

Depoyu geliştirme makinenize kopyalamak için, bir Terminal penceresinde aşağıdaki komutu çalıştırın (Windows 'da komut penceresi):

```sh
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="open-the-application-in-vs-code"></a>Uygulamayı VS Code açın

VS Code açın.  **Etkinlik çubuğunda** gezgin simgesine tıklayın ve **klasörü aç**' a tıklayın veya **Dosya > klasörü aç**' a tıklayın. Depoyu kopyaladığınız klasördeki *./Service-Fabric-Java-QuickStart/oylama* dizinine gidin ve ardından **Tamam**' a tıklayın. Çalışma alanı, aşağıdaki ekran görüntüsünde gösterilen dosyaları içermelidir.

![Çalışma alanında Java oylama uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/java-voting-application.png)

## <a name="build-the-application"></a>Uygulama oluşturma

1. VS Code içinde **komut paletini** açmak Için (Ctrl + Shift + p) tuşuna basın.
2. **Service Fabric: Build Application** komutunu arayın ve seçin. Derleme çıkışı tümleşik terminale gönderilir.

   ![VS Code uygulama komutu oluştur](./media/service-fabric-develop-java-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Uygulamayı yerel kümeye dağıtma
Uygulamayı oluşturduktan sonra yerel kümeye dağıtabilirsiniz. 

> [!IMPORTANT]
> Java uygulamalarını yerel kümeye dağıtmak Windows makinelerinde desteklenmez.

1. **Komut paletinden** **Service Fabric: Deploy Application (localhost) komutunu**seçin. Install işleminin çıktısı tümleşik terminale gönderilir.

   ![Uygulama komutunu VS Code içinde dağıt](./media/service-fabric-develop-java-applications-with-vs-code/sf-deploy-application.png)

4. Dağıtım tamamlandığında bir tarayıcı başlatın ve Service Fabric Explorer: `http://localhost:19080/Explorer`açın. Uygulamanın çalıştığını görmeniz gerekir. Bu işlem biraz zaman alabilir, bu nedenle sabırlı olun. 

   ![Service Fabric Explorer 'da oylama uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/sfx-localhost-java.png)

4. Uygulamanın çalıştığını doğruladıktan sonra bir tarayıcı başlatın ve şu sayfayı açın: `http://localhost:8080`. Bu, uygulamanın Web ön ucu olur. Öğe ekleyebilir ve oylamaya tıklayabilirsiniz.

   ![Tarayıcıda oylama uygulaması](./media/service-fabric-develop-java-applications-with-vs-code/voting-sample-in-browser.png)

5. Uygulamayı kümeden kaldırmak için, **komut paletinden** **Service Fabric: uygulamayı kaldır** komutunu seçin. Kaldırma işleminin çıktısı tümleşik terminale gönderilir. Uygulamanın yerel kümeden kaldırıldığını doğrulamak için Service Fabric Explorer kullanabilirsiniz.

## <a name="debug-the-application"></a>Uygulamada hata ayıklama
VS Code içindeki uygulamalarda hata ayıklarken, uygulamanın yerel bir kümede çalışıyor olması gerekir. Kesme noktaları daha sonra koda eklenebilir.

> [!IMPORTANT]
> Java uygulamalarında hata ayıklama Windows makinelerde desteklenmez.

VotingDataService ve oylama uygulamasını hata ayıklama için hazırlamak için aşağıdaki adımları izleyin:

1. *Oylama/VotingApplication/VotingDataServicePkg/Code/EntryPoint. sh* dosyasını güncelleştirin.
6\. satırdaki komutu açıklama (' # ' kullanın) ve aşağıdaki komutu dosyanın altına ekleyin:

   ```
   java -Xdebug -Xrunjdwp:transport=dt_socket,address=8001,server=y,suspend=n -Djava.library.path=$LD_LIBRARY_PATH -jar VotingDataService.jar
   ```

2. *Oylama/VotingApplication/ApplicationManifest. xml* dosyasını güncelleştirin. **Minreplicasetsize** ve **targetreplicasetsize** özniteliklerini **statefulservice** öğesinde "1" olarak ayarlayın:
   
   ```xml
         <StatefulService MinReplicaSetSize="1" ServiceTypeName="VotingDataServiceType" TargetReplicaSetSize="1">
   ```

3. VS Code hata ayıklayıcı görünümünü açmak için **etkinlik çubuğundaki** hata ayıkla simgesine tıklayın. Hata ayıklayıcı görünümünün en üstündeki dişli simgesine tıklayın ve açılan ortam menüsünden **Java** ' yı seçin. Launch. JSON dosyası açılır. 

   ![VS Code çalışma alanında hata ayıklama simgesi](./media/service-fabric-develop-java-applications-with-vs-code/debug-icon-workspace.png)

3. Launch. json dosyasında, **Debug (Attach)** adlı yapılandırmadaki bağlantı noktası değerini **8001**olarak ayarlayın. Dosyayı kaydedin.

   ![Launch. JSON için hata ayıklama yapılandırması](./media/service-fabric-develop-java-applications-with-vs-code/launch-json-java.png)

4. **Service Fabric: Deploy Application (localhost)** komutunu kullanarak uygulamayı yerel kümeye dağıtın. Service Fabric Explorer ' de uygulamanın çalıştığını doğrulayın. Uygulamanız artık ayıklanmaya hazırdır.

Bir kesme noktası ayarlamak için aşağıdaki adımları izleyin:

1. Gezgin 'de, */Voting/VotingDataService/src/statefulservice/VotingDataService.Java* dosyasını açın. `addItem` yönteminde (satır 80) `try` bloğundaki kodun ilk satırında bir kesme noktası ayarlayın.
   
   ![Oylama veri hizmetinde kesme noktası ayarlama](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-set.png)

   > [!IMPORTANT]
   > Kod noktalarından yürütülebilir satırlarda kesme noktaları ayarladığınızdan emin olun. Örneğin, yöntem bildirimlerinde ayarlanan kesme noktaları, `try` deyimlerini veya `catch` deyimlerini hata ayıklayıcı tarafından kaçıralınacaktır.
2. Hata ayıklamaya başlamak için **etkinlik çubuğundaki**hata ayıklama simgesine tıklayın, Hata Ayıkla menüsünden **Hata Ayıkla (Ekle)** yapılandırmasını seçin ve Çalıştır düğmesine (yeşil ok) tıklayın.

   ![Hata ayıklama (Iliştirme) yapılandırması](./media/service-fabric-develop-java-applications-with-vs-code/debug-attach-java.png)

3. Bir Web tarayıcısında `http://localhost:8080`' a gidin. Metin kutusuna yeni bir öğe yazın ve **+ Ekle**' ye tıklayın. Kesme noktası isabet etmelidir. Yürütmeye devam etmek, satırlarda adım adım veya geçerli yöntemin dışına geçmek için VS Code üstündeki hata ayıklama araç çubuğunu kullanabilirsiniz. 
   
   ![İsabet kesme noktası](./media/service-fabric-develop-java-applications-with-vs-code/breakpoint-hit.png)
       
4. Hata ayıklama oturumunu sonlandırmak için VS Code üstündeki hata ayıklama araç çubuğundaki tak simgesine tıklayın.
   
   ![Hata ayıklayıcıyla bağlantıyı kes](./media/service-fabric-develop-java-applications-with-vs-code/debug-bar-disconnect.png)
       
5. Hata ayıklamayı bitirdiğinizde, yerel kümeinizden oylama uygulamasını kaldırmak için **Service Fabric: uygulamayı kaldır** komutunu kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [Vs Code ile Service Fabric uygulamaları geliştirmeyi ve C# hata ayıklamayı](./service-fabric-develop-csharp-applications-with-vs-code.md)öğrenin.
