---
title: Visual Studio'da uygulamanızı hata ayıklama
description: Yerel bir geliştirme kümesinde Visual Studio'da bunları geliştirerek ve hata ayıklayarak hizmetlerinizin güvenilirliğini ve performansını artırın.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: fff8a19d5643f7ce866c9eb9c57486340b6f8a50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624130"
---
# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>Visual Studio'yu kullanarak Service Fabric uygulamanızda hata ayıklama
> [!div class="op_single_selector"]
> * [Görsel Stüdyo/CSharp](service-fabric-debugging-your-application.md) 
> * [Tutulma /Java](service-fabric-debugging-your-application-java.md)
>


## <a name="debug-a-local-service-fabric-application"></a>Yerel Servis Kumaşı uygulamasını hata ayıklama
Azure Hizmet Kumaşı uygulamanızı yerel bir bilgisayar geliştirme kümesinde dağıtıp hata ayıklayarak zamandan ve paradan tasarruf edebilirsiniz. Visual Studio 2019 veya 2015, uygulamayı yerel kümeye dağıtabilir ve hata ayıklayıcıyı uygulamanızın tüm örneklerine otomatik olarak bağlayabilir. Hata ayıklamayı bağlamak için Visual Studio Yönetici olarak çalıştırılmalıdır.

1. [Hizmet Kumaşı geliştirme ortamınızı ayarlama](service-fabric-get-started.md)adımlarını izleyerek yerel bir geliştirme kümesi başlatın.
2. **F5 tuşuna** basın veya **Hata Ayıklama** > **Başlat Hata Ayıklama'yı**tıklatın.
   
    ![Uygulamanın hata ayıklama başlatma][startdebugging]
3. Kodunuzda kesme noktaları ayarlayın ve **Hata Ayıklama** menüsündeki komutları tıklatarak uygulama üzerinden adım atın.
   
   > [!NOTE]
   > Visual Studio uygulamanızın tüm örneklerine bağlanır. Kod üzerinden adım atarken, kesme noktaları eşzamanlı oturumlarla sonuçlanan birden çok işlem tarafından vurulabilir. Kopma noktalarını vurulduktan sonra, her kesme noktasını iş parçacığı kimliğinde koşullu hale getirerek veya tanılama olaylarını kullanarak devre dışı bırakmayı deneyin.
   > 
   > 
4. **Tanılama olaylarını** gerçek zamanlı olarak görüntüleyebilmeniz için Tanılama Olayları penceresi otomatik olarak açılır.
   
    ![Tanılama olaylarını gerçek zamanlı olarak görüntüleme][diagnosticevents]
5. Ayrıca Bulut Gezgini'nde **Tanılama Olayları** penceresini de açabilirsiniz.  **Hizmet Kumaşı**altında, herhangi bir düğüme sağ tıklayın ve **Akış İzini Görüntüle'yi**seçin.
   
    ![Tanılama olayları penceresini açma][viewdiagnosticevents]
   
    İzlemelerinizi belirli bir hizmete veya uygulamaya filtrelemek istiyorsanız, belirli bir hizmet veya uygulamada akış izlemelerini etkinleştirin.
6. Tanılama olayları otomatik olarak oluşturulan **ServiceEventSource.cs** dosyasında görülebilir ve uygulama kodundan çağrılır.
   
    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```
7. **Tanılama Olayları** penceresi olayları gerçek zamanlı olarak filtreletme, duraklatma ve denetlemeyi destekler.  Filtre, içeriği de dahil olmak üzere olay iletisinin basit bir dize aramasidır.
   
    ![Olayları gerçek zamanlı olarak filtreleme, duraklatma ve devam ettirme veya inceleme][diagnosticeventsactions]
8. Hata ayıklama hizmetleri, başka bir uygulamanın hata ayıklanması gibidir. Normalde kolay hata ayıklama için Visual Studio ile Breakpoints ayarlarsınız. Güvenilir Koleksiyonlar birden çok düğüm de çoğalsa da, yine de İzlenebilir uygularlar. Bu uygulama, içinde depoladığınız şeyi görmek için hata ayıklama sırasında Visual Studio'da Sonuç Görünümü'ni kullanabileceğiniz anlamına gelir. Bunu yapmak için, kodunuzda herhangi bir yere bir kesme noktası ayarlayın.
   
    ![Uygulamanın hata ayıklama başlatma][breakpoint]


### <a name="running-a-script-as-part-of-debugging"></a>Hata ayıklamanın bir parçası olarak komut dosyası çalıştırma
Bazı senaryolarda hata ayıklama oturumu başlatmanın bir parçası olarak (örneğin Varsayılan Hizmetleri kullanmadığınızda) bir komut dosyası çalıştırmanız gerekebilir.

Visual Studio'da, Service Fabric Application projesinin (.sfproj) **Komut Dosyaları** klasörüne **Start-Service.ps1** adlı bir dosya ekleyebilirsiniz. Bu komut dosyası, uygulama yerel kümede oluşturulduktan sonra çağrılacaktır.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>Uzaktan Servis Kumaşı uygulamasını hata ayıklama
Hizmet Kumaşı uygulamalarınız Azure'da bir Hizmet Kumaşı kümesinde çalışıyorsa, bu uygulamaları doğrudan Visual Studio'dan uzaktan hata ayıklayabilirsiniz.

> [!NOTE]
> Bu özellik için [Service Fabric SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ve [.NET 2.9 için Azure SDK'yı](https://azure.microsoft.com/downloads/)gerektirir.    

<!-- -->
> [!WARNING]
> Uzaktan hata ayıklama, işletim etüt uygulamaları üzerindeki etkisi nedeniyle geliştirme/test senaryoları içindir ve üretim ortamlarında kullanılmamalıdır.

1. **Bulut Gezgini'nde**kümenize gidin. Sağ tıklayın ve **Hata Ayıklama'yı Etkinleştir'i** seçin
   
    ![Uzaktan hata ayıklamayı etkinleştirme][enableremotedebugging]
   
    Bu eylem, küme düğümlerinizde ve gerekli ağ yapılandırmalarında uzaktan hata ayıklama uzantısını etkinleştirme işlemini başlatacaktır.
2. **Bulut Gezgini'nde**küme düğümüne sağ tıklayın ve **Hata Ayıklama Ekle'yi** seçin
   
    ![Hata ayıklama ekleme][attachdebugger]
3. **İşleme Ekle** iletişim kutusunda hata ayıklamak istediğiniz işlemi seçin ve **Ekle'yi** tıklatın
   
    ![İşlem seçin][chooseprocess]
   
    Eklemek istediğiniz işlemin adı, hizmet proje derleme adınızı eşittir.
   
    Hata ayıklama işlemi çalıştıran tüm düğümlere eklenir.
   
   * Bir hizmeti hata ayıklama durumunda, tüm düğümlerde hizmetin tüm örnekleri hata ayıklama oturumunun bir parçasıdır.
   * Bir hizmeti hata ayıklıyorsanız, yalnızca herhangi bir bölümün birincil kopyası etkin olur ve bu nedenle hata ayıklama tarafından yakalanır. Birincil yineleme hata ayıklama oturumu sırasında hareket ederse, bu yinelemenin işlenmesi yine de hata ayıklama oturumunun bir parçası olacaktır.
   * Yalnızca belirli bir hizmetin ilgili bölümlerini veya örneklerini yakalamak için, koşullu kesme noktalarını yalnızca belirli bir bölümü veya örneği kırmak için kullanabilirsiniz.
     
     ![Koşullu kesme noktası][conditionalbreakpoint]
     
     > [!NOTE]
     > Şu anda, aynı hizmet çalıştırılabilir adı birden çok örneği olan bir Hizmet Kumaşı kümesinin hata ayıklamasını destekliyoruz.
     > 
     > 
4. Uygulamanızı hata ayıklamayı tamamladıktan sonra, **Bulut Gezgini'nde** kümeyi sağ tıklayarak uzaktan hata ayıklama uzantısını devre dışı bırakıp Hata **Hata Ayıklama'yı** devre dışı bırakmayı seçebilirsiniz
   
    ![Uzaktan hata ayıklamayı devre dışı][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>Uzak küme düğümünden izleme akışı
Ayrıca, uzak küme düğümünden Visual Studio'ya doğrudan izleme akışı da edebilirsiniz. Bu özellik, Service Fabric küme düğümünde üretilen ETW izleme olaylarını akışla aktaranıza olanak tanır.

> [!NOTE]
> Bu [özellik, .NET 2.9 için](https://azure.microsoft.com/downloads/) [Hizmet Kumaşı SDK 2.0](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) ve Azure SDK'yı gerektirir.
> Bu özellik yalnızca Azure'da çalışan kümeleri destekler.
> 
> 

<!-- -->
> [!WARNING]
> Akış izleri, çalışan uygulamalar üzerindeki etkisi nedeniyle geliştirme/test senaryoları içindir ve üretim ortamlarında kullanılmamalıdır.
> Bir üretim senaryosunda, Azure Tanılama'yı kullanarak olayları iletme konusunda güvenmelisiniz.

1. **Bulut Gezgini'nde**kümenize gidin. Sağ tıklayın ve **Akış İzinleri Etkinleştir'i** seçin
   
    ![Uzaktan akış izlemelerini etkinleştirme][enablestreamingtraces]
   
    Bu eylem, küme düğümlerinizde akış izleme uzantısını ve gerekli ağ yapılandırmalarını etkinleştirme işlemini başlatacaktır.
2. **Bulut Gezgini'nde** **Düğümler** öğesini genişletin, izleme akışından istediğiniz düğüme sağ tıklayın ve **Akış İzini Görüntüle'yi** seçin
   
    ![Uzak akış izlerini görüntüleme][viewremotestreamingtraces]
   
    İzleri görmek istediğiniz kadar düğüm için 2. Her düğüm akışı özel bir pencerede gösterecektir.
   
    Artık Service Fabric tarafından yayılan izleri ve hizmetlerinizi görebilirsiniz. Olayları yalnızca belirli bir uygulamayı göstermek için filtrelemek istiyorsanız, filtredeki uygulamanın adını yazmanız yeterlidir.
   
    ![Akış izlerini görüntüleme][viewingstreamingtraces]
3. Kümenizden izleme akışı bittikten sonra, **Bulut Gezgini'nde** kümeyi sağ tıklayarak uzaktan akış izlerini devre dışı bırakıp **Akış İzlemelerini Devre Dışı** Bırakma'yı seçebilirsiniz
   
    ![Uzaktan akış izlemelerini devre dışı bırakma][disablestreamingtraces]

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Servis Kumaş ı hizmetini test edin.](service-fabric-testability-overview.md)
* [Service Fabric uygulamalarınızı Visual Studio'da yönetin.](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
