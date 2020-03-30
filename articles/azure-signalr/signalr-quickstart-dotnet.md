---
title: ASP.NET ile geliştirin - Azure SignalR Hizmeti
description: ASP.NET çerçeveye sahip bir sohbet odası oluşturmak için Azure SignalR Hizmetini kullanmak için hızlı bir başlangıç.
author: sffamily
ms.service: signalr
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/20/2019
ms.author: zhshang
ms.openlocfilehash: ec5b7a75bced4b7cd81a120925558b8c1be57818
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74158184"
---
# <a name="quickstart-create-a-chat-room-with-aspnet-and-signalr-service"></a>Quickstart: ASP.NET ve SignalR Servisi ile bir sohbet odası oluşturun

Azure SignalR Hizmeti, ASP.NET SignalR ile %100 uyumlu **olmayan** [ASP.NET Core 2.0 için SignalR'ı](https://docs.microsoft.com/aspnet/core/signalr/introduction)temel adamıştır. Azure SignalR Hizmeti, en son ASP.NET Core teknolojilerini temel alan ASP.NET SignalR veri protokolünü yeniden uyguladı. ASP.NET SignalR için Azure SignalR Hizmetini kullanırken, bazı ASP.NET SignalR özellikleri artık desteklenmez, örneğin, Azure SignalR istemci yeniden bağlandığında iletileri yeniden oynatmaz. Ayrıca, Forever Frame taşıma ve JSONP desteklenmez. SignalR uygulamasının SignalR Hizmeti ile çalışmasını ASP.NET sağlamak için bazı kod değişiklikleri ve bağımlı kitaplıkların uygun sürümü gereklidir. 

ASP.NET SignalR ve ASP.NET Core SignalR arasındaki özellik karşılaştırmasının tam listesi için [sürüm farkları dokümanına](https://docs.microsoft.com/aspnet/core/signalr/version-differences?view=aspnetcore-2.2) bakın.

Bu hızlı başlangıçta, benzer bir [Sohbet Odası uygulaması](./signalr-quickstart-dotnet-core.md)için ASP.NET ve Azure SinyalR Hizmeti ile nasıl başlayacağınızı öğreneceksiniz.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)
* [.NET 4.6.1](https://www.microsoft.com/net/download/windows)
* [ASP.NET Sinyalci 2.4.1](https://www.nuget.org/packages/Microsoft.AspNet.SignalR/)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

ASP.NET SignalR uygulamaları için *sunucusuz* mod desteklenmez. Azure Sinyal Hizmeti örneği için varsayılan *veya* *klasik* her zaman kullanın.

SignalR [Service komut dosyası oluştur](scripts/signalr-cli-create-service.md)ile bu hızlı başlatmada kullanılan Azure kaynaklarını da oluşturabilirsiniz.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Hizmet dağıtılırken kod ile çalışmaya geçiş yapalım. [GitHub'dan örnek uygulamayı](https://github.com/aspnet/AzureSignalR-samples/tree/master/aspnet-samples/ChatRoom) kopyalayın, SignalR Hizmetinin bağlantı dizesini ayarlayın ve uygulamayı yerel olarak çalıştırın.

1. Bir git terminal penceresi açın. Örnek projeyi kopyalamak istediğiniz klasöre gidin.

1. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/aspnet/AzureSignalR-samples.git
    ```

## <a name="configure-and-run-chat-room-web-app"></a>Chat Room web uygulamasını yapılandırma ve çalıştırma

1. Visual Studio'yu başlatın ve çözümü klonlanmış deponun *aspnet örnekleri/ChatRoom/klasöründe* açın.

1. Azure portalının açıldığı tarayıcıda, oluşturduğunuz örneği bulun ve seçin.

1. SignalR Hizmeti örneğinin bağlantı dizelerini görüntülemek için **Anahtarlar**’ı seçin.

1. Birincil bağlantı dizesini seçerek kopyalayın.

1. Şimdi web.config dosyasındaki bağlantı dizesini ayarlayın.

    ```xml
    <configuration>
    <connectionStrings>
        <add name="Azure:SignalR:ConnectionString" connectionString="<Replace By Your Connection String>"/>
    </connectionStrings>
    ...
    </configuration>
    ```

1. *Startup.cs*, aramak `MapSignalR()`yerine, uygulamanın `MapAzureSignalR({your_applicationName})` SignalR'ı tek başına barındırmak yerine hizmete bağlanmasını sağlamak için bağlantı dizesini aramanız ve geçmeniz gerekir. Başvurunuzun adını değiştirin. `{YourApplicationName}` Bu ad, bu uygulamayı diğer uygulamalarınızdan ayırt etmek için benzersiz bir addır. Değer olarak `this.GetType().FullName` kullanabilirsiniz.

    ```cs
    public void Configuration(IAppBuilder app)
    {
        // Any connection or hub wire up and configuration should go here
        app.MapAzureSignalR(this.GetType().FullName);
    }
    ```

    Ayrıca, bu API'leri kullanmadan önce Hizmet SDK'ya başvurmanız gerekir. Araçları Aç **| NuGet Paket Yöneticisi | Paket Yöneticisi Konsolu** ve çalıştır komutu:

    ```powershell
    Install-Package Microsoft.Azure.SignalR.AspNet
    ```

    Bu değişiklikler dışında, diğer her şey aynı kalır, iş mantığı yazmak için zaten aşina olduğunuz hub arabirimini kullanmaya devam edebilirsiniz.

    > [!NOTE]
    > Uygulamada, Azure SignalR Service SDK tarafından bir bitiş noktası `/signalr/negotiate` pazarlıştırılmaz. İstemciler bağlantı dizesinde tanımlanan hizmet bitiş noktasına bağlanmaya ve yeniden yönlendirmeye çalıştıklarında özel bir anlaşma yanıtı döndürecektir.

1. Projeyi hata ayıklama modunda çalıştırmak için **F5** tuşuna basın. Uygulamanın yerel olarak çalıştığını görebilirsiniz. Uygulamanın kendisi tarafından bir SignalR çalışma süresi barındırmak yerine, artık Azure SinyalR Hizmeti'ne bağlanır.

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]



> [!IMPORTANT]
> Bir kaynak grubunu silme işlemi geri alınamaz ve kaynak grubunun ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. Bu örneği, tutmak istediğiniz kaynakları içeren mevcut bir kaynak grubunda barındırmak için kaynaklar oluşturduysanız, kaynak grubunu silmek yerine her kaynağı kendi ilgili dikey penceresinden tek tek silebilirsiniz.
> 
> 

[Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

**Ada göre filtrele...** metin kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçtaki yönergelerde *SignalRTestResources* adlı bir kaynak grubu kullanılmıştır. Sonuç listesindeki kaynak grubunuzda **...** ve sonra **Kaynak grubunu sil**’e tıklayın.

   
![Sil](./media/signalr-quickstart-dotnet-core/signalr-delete-resource-group.png)

Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir Azure Sinyal Hizmeti kaynağı oluşturdunuz ve ASP.NET bir web uygulamasıyla kullandınız. Ardından, ASP.NET Core ile Azure SignalR Hizmetini kullanarak gerçek zamanlı uygulamaları nasıl geliştireceklerini öğrenin.

> [!div class="nextstepaction"]
> [ASP.NET Core ile Azure SignalR Hizmeti](./signalr-quickstart-dotnet-core.md)
