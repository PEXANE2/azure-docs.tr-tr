---
title: Visual Studio Code ile .NET Core uygulamalarını geliştirin
description: Bu makalede, Visual Studio Code kullanarak .NET Çekirdek Hizmet Kumaş uygulamalarının nasıl oluşturup dağıtılanın ve hata ayıklanınca gösterilmektedir.
author: peterpogorski
ms.topic: article
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 1d7478e6b81ef2c53ca6194197336e91d3ff250b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614532"
---
# <a name="develop-c-service-fabric-applications-with-visual-studio-code"></a>Visual Studio Code ile C# Service Fabric uygulamaları geliştirin

[VS Code için Service Fabric Reliable Services uzantısı,](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) Windows, Linux ve macOS işletim sistemlerinde .NET Core Service Fabric uygulamaları oluşturmayı kolaylaştırır.

Bu makalede, Visual Studio Code kullanarak bir .NET Çekirdek Hizmet Kumaş ı uygulaması nın nasıl oluşturup dağıtılacağını ve hata ayıklamasını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, VS Kodu, VS Kodu için Hizmet Kumaşı Güvenilir Hizmetler uzantısı ve geliştirme ortamınız için gerekli tüm bağımlılıkları zaten yüklediğiniz varsayar. Daha fazla bilgi için [bkz.](./service-fabric-get-started-vs-code.md#prerequisites)

## <a name="download-the-sample"></a>Örneği indirme
Bu makalede, Hizmet Kumaşı .NET Core'daki Karşı Servis uygulamasını [kullansın ve github deposu örneklerine başlar.](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) 

Depoyu geliştirme makinenize klonlamak için, terminal penceresinden (Windows'daki komut penceresi) aşağıdaki komutu çalıştırın:

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started.git
```

## <a name="open-the-application-in-vs-code"></a>VS Kodu'nda uygulamayı açın

### <a name="windows"></a>Windows
Başlat Menüsündeki VS Kodu simgesine sağ tıklayın ve **yönetici olarak Çalıştır'ı**seçin. Hata ayıklayıcıyı hizmetlerinize eklemek için VS Code'u yönetici olarak çalıştırmanız gerekir.

### <a name="linux"></a>Linux
Terminali kullanarak, uygulamanın yerel olarak klonlandığının dizininden /service-fabric-dotnet-core-getting-started/Services/CounterService yoluna gidin.

Hata ayıklayıcının hizmetlerinize ekleyebileceği şekilde VS Kodunu kök kullanıcı olarak açmak için aşağıdaki komutu çalıştırın.
```
sudo code . --user-data-dir='.'
```

Uygulama artık VS Kodu çalışma alanınızda görünmelidir.

![Çalışma Alanında Karşı Hizmet Uygulaması](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-application-in-workspace.png)

## <a name="build-the-application"></a>Uygulama oluşturma
1. VS Kodu'nda **Komut Paletini** açmak için (Ctrl + Shift + p) tuşuna basın.
2. Servis Dokusunu arayın ve **seçin: Uygulama** komutu oluşturun. Yapı çıktısı entegre terminale gönderilir.

   ![VS Kodu'nda Uygulama komutu oluşturma](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-build-application.png)

## <a name="deploy-the-application-to-the-local-cluster"></a>Uygulamayı yerel kümeye dağıtma
Uygulamayı yaptıktan sonra yerel kümeye dağıtabilirsiniz. 

1. Komut **Paleti'nden,** **Hizmet Dokusu'nu seçin: Uygulamayı (Localhost) dağıt komutunu**dağıtın. Yükleme işleminin çıktısı entegre terminale gönderilir.

   ![VS Kodunda Uygulama komutunu dağıt](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-deploy-application.png)

4. Dağıtım tamamlandığında, bir tarayıcı başlatın ve Hizmet Kumaş\/Explorer'ı açın: http: /localhost:19080/Explorer. Uygulamanın çalıştığını görmeniz gerekir. Bu biraz zaman alabilir, bu yüzden sabırlı olun. 

   ![Servis Kumaş Explorer'da Sayaç Servis uygulaması](./media/service-fabric-develop-csharp-applications-with-vs-code/sfx-verify-deploy.png)

4. Uygulamanın çalıştığını doğruladıktan sonra bir tarayıcı başlatın ve şu\/sayfayı açın: http: /localhost:31002. Bu uygulamanın web ön ucudur. Sayaç artışları olarak geçerli değerini görmek için sayfayı yenileyin.

   ![Tarayıcıda Karşı Servis uygulaması](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-running.png)

## <a name="publish-the-application-to-an-azure-service-fabric-cluster"></a>Uygulamayı Azure Hizmet Kumaşı kümesinde yayımlama
Uygulamayı yerel kümeye dağıtmanın yanı sıra, uygulamayı uzak bir Azure Hizmet Kumaşı kümesinde de yayımlayabilirsiniz. 

1. Yukarıdaki yönergeleri kullanarak uygulamanızı oluşturduğunuzdan emin olun. Yayımlanmak istediğiniz `Cloud.json` uzak kümenin ayrıntılarıyla oluşturulan yapılandırma dosyasını güncelleştirin.

2. Komut **Paleti'nden** **Hizmet Dokusu'nu seçin: Uygulama komutunu yayımla.** Yükleme işleminin çıktısı entegre terminale gönderilir.

   ![VS Kodu'nda Uygulama komutunu yayımla](./media/service-fabric-develop-csharp-applications-with-vs-code/sf-publish-application.png)

3. Dağıtım tamamlandığında, bir tarayıcı başlatın ve Hizmet `https:<clusterurl>:19080/Explorer`Kumaş Explorer'ı açın: . Uygulamanın çalıştığını görmeniz gerekir. Bu biraz zaman alabilir, bu yüzden sabırlı olun. 

## <a name="debug-the-application"></a>Uygulamada hata ayıklama
VS Code'da hata ayıklama uygulamaları yaparken, uygulama yerel bir kümeüzerinde çalışıyor olmalıdır. Kesme noktaları daha sonra koda eklenebilir.

Kesme noktası ve hata ayıklama ayarlamak için aşağıdaki adımları tamamlayın:
1. Explorer'da */src/CounterServiceApplication/CounterService/CounterService.cs* dosyasını açın ve `RunAsync` yöntemin içindeki 62.
3. VS Kodu'nda hata ayıklayıcı görünümünü açmak için **Etkinlik Çubuğu'ndaki** Hata Ayıklama simgesini tıklatın. Hata ayıklama görünümünün üst kısmındaki dişli simgesini tıklatın ve açılan ortam menüsünden **.NET Core'u** seçin. Launch.json dosyası açılır. Bu dosyayı kapatabilirsiniz. Şimdi çalıştır düğmesinin (yeşil ok) yanında bulunan hata ayıklama yapılandırma menüsünde yapılandırma seçeneklerini görmeniz gerekir.

   ![VS Kodu Çalışma Alanında Hata Ayıklama Simgesi](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-icon-workspace.png)

2. Hata ayıklama yapılandırma menüsünden **.NET Core Attach'ı** seçin.

   ![VS Kodu Çalışma Alanında Hata Ayıklama Simgesi](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-start.png)

3. Bir tarayıcıda Açık Servis Kumaş\/Explorer: http: /localhost:19080/Explorer. **Uygulamalar'ı** tıklatın ve Karşı Hizmetin üzerinde çalıştığı birincil düğümü belirlemek için ayrıntıya inin. Karşı Hizmet için birincil düğüm aşağıdaki resimde Düğüm 0.

   ![Karşı Hizmet için Birincil Düğüm](./media/service-fabric-develop-csharp-applications-with-vs-code/counter-service-primary-node.png)

4. VS Kodu'nda,.NET **Core Attach** hata ayıklama yapılandırmasının yanındaki çalıştır simgesini (yeşil ok) tıklatın. İşlem seçimi iletişim kutusunda, adım 4'te tanımladığınız birincil düğümüzerinde çalışan Karşı Hizmet işlemini seçin.

   ![Birincil Süreç](./media/service-fabric-develop-csharp-applications-with-vs-code/select-process.png)

5. *CounterService.cs* dosyasındaki kesme noktası çok hızlı bir şekilde vurulur. Daha sonra yerel değişkenlerin değerlerini keşfedebilirsiniz. Yürütmeyi devam etmek, satırları aşmak, yöntemlere adım atmak veya geçerli yöntemin dışına çıkmak için VS Kodunun üst kısmındaki Hata Ayıklama araç çubuğunu kullanın. 

   ![Hata Ayıklama Değerleri](./media/service-fabric-develop-csharp-applications-with-vs-code/breakpoint-hit.png)

6. Hata ayıklama oturumunu sona erdirmek için, HATA Ayıklama araç çubuğundaki fiş simgesini VS Kodu'nun üst kısmında tıklatın...
   
   ![Hata ayıklamayla bağlantıyı kesme](./media/service-fabric-develop-csharp-applications-with-vs-code/debug-bar-disconnect.png)
       
7. Hata ayıklamayı tamamladığınızda, **Servis Dokusu:** Karşı Servis uygulamasını yerel kümenizden kaldırmak için Uygulama komutunu kaldır komutunu kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* VS Code ile Java Service Fabric uygulamalarını nasıl [geliştirip hata](./service-fabric-develop-java-applications-with-vs-code.md)ayıklayarak hata ayıklamayı öğrenin.



