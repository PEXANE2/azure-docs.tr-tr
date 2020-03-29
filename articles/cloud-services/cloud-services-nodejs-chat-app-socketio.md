---
title: Socket.io kullanarak Düğüm.js uygulaması - Azure
description: Azure'da barındırılan bir düğüm.js uygulamasında socket.io nasıl kullanacağınızı öğrenin.
services: cloud-services
documentationcenter: nodejs
author: tgore03
ms.service: cloud-services
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tagore
ms.openlocfilehash: 0b515c630d8a3539cdab1df64b1925e9fcaf206e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75360778"
---
# <a name="build-a-nodejs-chat-application-with-socketio-on-an-azure-cloud-service"></a>Azure Bulut Hizmeti'nde Socket.IO ile Bir Düğüm.js sohbet uygulaması oluşturun

Socket.IO, düğüm sunucunuz ve istemcileriniz arasında gerçek zamanlı iletişim sağlar. Bu öğretici bir soket barındırma ile size yol. Azure'da IO tabanlı sohbet uygulaması. Socket.IO hakkında daha fazla bilgi için [socket.io.](https://socket.io)

Tamamlanan uygulamanın ekran görüntüsü aşağıdadır:

![Azure'da barındırılan hizmeti görüntüleyen bir tarayıcı penceresi][completed-app]  

## <a name="prerequisites"></a>Ön koşullar
Bu makalede örneği başarıyla tamamlamak için aşağıdaki ürün ve sürümlerin yüklendiğinden emin olun:

* [Visual Studio'u](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) Yükleyin
* [Node.js](https://nodejs.org/download/) yükle
* [Python sürüm 2.7.10](https://www.python.org/) yükle

## <a name="create-a-cloud-service-project"></a>Bulut Hizmeti Projesi Oluşturma
Aşağıdaki adımlar, Socket.IO uygulamasını barındıracak bulut hizmeti projesini oluşturur.

1. Başlat **Menüsünden** veya **Başlat Ekranından** **Windows PowerShell'i**arayın. Son olarak, **Windows PowerShell'e** sağ tıklayın ve **Yönetici Olarak Çalıştır'ı**seçin.
   
    ![Azure PowerShell simgesi][powershell-menu]
2. c adlı bir dizin **oluşturun:\\düğüm**. 
   
        PS C:\> md node
3. Dizinleri **c'ye\\değiştirin: düğüm** dizini
   
        PS C:\> cd node
4. **Chatapp** adında yeni bir çözüm ve **WorkerRole1**adlı bir işçi rolü oluşturmak için aşağıdaki komutları girin:
   
        PS C:\node> New-AzureServiceProject chatapp
        PS C:\Node> Add-AzureNodeWorkerRole
   
    Aşağıdaki yanıtı görürsünüz:
   
    ![Yeni azureservice ve add-azurenodeworkerrolecmdlets çıktı](./media/cloud-services-nodejs-chat-app-socketio/socketio-1.png)

## <a name="download-the-chat-example"></a>Sohbet Örneğini İndir
Bu proje için, [Socket.IO GitHub deposundan]sohbet örneğini kullanacağız. Örneği indirmek ve daha önce oluşturduğunuz projeye eklemek için aşağıdaki adımları gerçekleştirin.

1. **Klon** düğmesini kullanarak deponun yerel bir kopyasını oluşturun. Projeyi indirmek için **ZIP** düğmesini de kullanabilirsiniz.
   
   ![Zip indirme https://github.com/LearnBoost/socket.io/tree/master/examples/chatsimgesi vurgulanmış bir tarayıcı penceresi görüntüleme](./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png)
2. **Örnekler\\sohbet** dizinine varana kadar yerel deponun dizin yapısında gezinin. Bu dizinin içeriğini **C:\\düğüm\\chatapp\\WorkerRole1** dizininin daha önce oluşturduğuna kopyalayın.
   
   ![Explorer, arşivden çıkarılan sohbet\\dizininin içeriğini görüntüleme][chat-contents]
   
   Yukarıdaki ekran görüntüsünde vurgulanan **öğeler, sohbet\\** dizinindeki örneklerden kopyalanan dosyalardır.
3. **C:\\\\düğüm chatapp\\WorkerRole1** dizini, **server.js** dosyasını silin ve sonra **server.js**için **app.js** dosyasını yeniden adlandırma . Bu, **Ekle-AzureNodeWorkerRole** cmdlet tarafından daha önce oluşturulan varsayılan **server.js** dosyasını kaldırır ve sohbet örneğinden uygulama dosyasıyla değiştirir.

### <a name="modify-serverjs-and-install-modules"></a>Server.js'yi değiştirin ve Modülleri Kurun
Uygulamayı Azure emülatöründe test etmeden önce bazı küçük değişiklikler yapmalıyız. server.js dosyasına aşağıdaki adımları gerçekleştirin:

1. Visual Studio'da veya herhangi bir metin düzenleyicisinde **server.js** dosyasını açın.
2. server.js'in **başındaki Modül bağımlılıkları** bölümünü bulun ve sio = require('... içeren satırı **değiştirin. //.. lib//socket.io')** to sio = aşağıda gösterildiği **gibi('socket.io') gerektirir:**
   
       var express = require('express')
         , stylus = require('stylus')
         , nib = require('nib')
       //, sio = require('..//..//lib//socket.io'); //Original
         , sio = require('socket.io');                //Updated
         var port = process.env.PORT || 3000;         //Updated
3. Uygulamanın doğru bağlantı noktasında dinlediğinden emin olmak için Notepad'de veya en sevdiğiniz düzenleyicide server.js'i açın ve aşağıdaki gösterildiği gibi **3000'i** **process.env.port** ile değiştirerek aşağıdaki satırı değiştirin:
   
       //app.listen(3000, function () {            //Original
       app.listen(process.env.port, function () {  //Updated
         var addr = app.address();
         console.log('   app listening on http://' + addr.address + ':' + addr.port);
       });

**değişiklikleri server.js'ye**kaydettikten sonra, gerekli modülleri yüklemek için aşağıdaki adımları kullanın ve uygulamayı Azure emülatöründe sınlayın:

1. **Azure PowerShell'i**kullanarak dizinleri **C:\\düğüm\\chatapp\\WorkerRole1** dizinine değiştirin ve bu uygulamanın gerektirdiği modülleri yüklemek için aşağıdaki komutu kullanın:
   
       PS C:\node\chatapp\WorkerRole1> npm install
   
   Bu, package.json dosyasında listelenen modülleri yükler. Komut tamamlandıktan sonra aşağıdakine benzer bir çıktı görmeniz gerekir:
   
   ![npm yükleme komutunun çıktısı][The-output-of-the-npm-install-command]
2. Bu örnek aslında Socket.IO GitHub deposunun bir parçası olduğundan ve Socket.IO kitaplığına göreli yol ile doğrudan başvurulduğundan, Socket.IO package.json dosyasında başvurulmadığını, bu nedenle aşağıdaki komutu vererek yüklememiz gerekir:
   
       PS C:\node\chatapp\WorkerRole1> npm install socket.io --save

### <a name="test-and-deploy"></a>Test edin ve dağıtın
1. Aşağıdaki komutu vererek emülatör başlatın:
   
       PS C:\node\chatapp\WorkerRole1> Start-AzureEmulator -Launch
   
   > [!NOTE]
   > Başlatma emülatörile ilgili sorunlarla karşılaşırsanız, örneğin: Başlangıç-AzureEmulator : Beklenmeyen bir hata oluştu.  Ayrıntılar: Beklenmeyen bir hatayla karşılaşılır İletişim nesnesi System.ServiceModel.Channels.ServiceChannel, Hatalı durumda olduğu için iletişim için kullanılamaz.
   > 
   > AzureAuthoringTools v 2.7.1 ve AzureComputeEmulator v 2.7'yi yeniden yükleyin - bu sürümün eşleştiğini unutmayın.

2. Bir tarayıcı açın **http://127.0.0.1**ve ''ye gidin.
3. Tarayıcı penceresi açıldığında, bir takma ad girin ve enter tuşuna basın.
   Bu, iletileri belirli bir takma ad olarak göndermenize olanak sağlar. Çok kullanıcılı işlevselliği test etmek için, aynı URL'yi kullanarak ek tarayıcı pencereleri açın ve farklı takma adlar girin.
   
   ![User1 ve User2'den gelen sohbet mesajlarını görüntüleyen iki tarayıcı penceresi](./media/cloud-services-nodejs-chat-app-socketio/socketio-8.png)
4. Uygulamayı test ettikten sonra, aşağıdaki komutu vererek emülatör durdurun:
   
       PS C:\node\chatapp\WorkerRole1> Stop-AzureEmulator
5. Uygulamayı Azure'a dağıtmak için **Publish-AzureServiceProject** cmdlet'ini kullanın. Örnek:
   
       PS C:\node\chatapp\WorkerRole1> Publish-AzureServiceProject -ServiceName mychatapp -Location "East US" -Launch
   
   > [!IMPORTANT]
   > Benzersiz bir ad kullandığınızdan emin olun, aksi takdirde yayımlama işlemi başarısız olur. Dağıtım tamamlandıktan sonra, tarayıcı açılır ve dağıtılan hizmete yönlendirilir.
   > 
   > Sağlanan abonelik adının içe aktarılan yayımlama profilinde bulunmadığını belirten bir hata alırsanız, Azure'a başlamadan önce aboneliğinizin yayımlama profilini indirmeniz ve almanız gerekir. Uygulamayı **Azure'a Dağıtma'yı** Yapı'nın Azure bölümüne görün [ve Bir Azure Bulut Hizmetine Düğüm.js uygulamasını dağıtın](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 
   
   ![Azure'da barındırılan hizmeti görüntüleyen bir tarayıcı penceresi][completed-app]
   
   > [!NOTE]
   > Sağlanan abonelik adının içe aktarılan yayımlama profilinde bulunmadığını belirten bir hata alırsanız, Azure'a başlamadan önce aboneliğinizin yayımlama profilini indirmeniz ve almanız gerekir. Uygulamayı **Azure'a Dağıtma'yı** Yapı'nın Azure bölümüne görün [ve Bir Azure Bulut Hizmetine Düğüm.js uygulamasını dağıtın](https://azure.microsoft.com/develop/nodejs/tutorials/getting-started/)
   > 
   > 

Uygulamanız artık Azure'da çalışıyor ve Socket.IO kullanarak farklı istemciler arasında sohbet iletileri aktarabilir.

> [!NOTE]
> Basitlik için, bu örnek aynı örneğe bağlı kullanıcılar arasında sohbet etmekle sınırlıdır. Bu, bulut hizmeti iki alt rol örneği oluşturursa, kullanıcıların yalnızca aynı alt rol örneğine bağlı başkalarıyla sohbet edebileceği anlamına gelir. Uygulamayı birden çok rol örneğiyle çalışacak şekilde ölçeklendirmek için, Socket.IO deposu durumunu örnekler arasında paylaşmak için Service Bus gibi bir teknoloji kullanabilirsiniz. Örneğin, [Node.js GitHub deposu için Azure SDK'daki](https://github.com/WindowsAzure/azure-sdk-for-node)Hizmet Veri Merkezi Kuyrukları ve Konular kullanım örneklerine bakın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Azure Bulut Hizmeti'nde barındırılan temel bir sohbet uygulamasını nasıl oluşturabileceğinizi öğrendiniz. Bu uygulamayı bir Azure Web Sitesinde nasıl barındırılacınız öğrenmek için, Azure [Web Sitesinde Socket.IO ile Bir Düğüm.js Sohbet Uygulaması Oluştur bölümüne][chatwebsite]bakın.

Daha fazla bilgi için, ayrıca [Node.js Geliştirici Merkezi'ne](https://docs.microsoft.com/azure/javascript/)bakın.

[chatwebsite]: https://docs.microsoft.com/azure/cloud-services/cloud-services-nodejs-develop-deploy-app

[Azure SLA]: https://www.windowsazure.com/support/sla/
[Azure SDK for Node.js GitHub repository]: https://github.com/WindowsAzure/azure-sdk-for-node
[completed-app]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-10.png
[Azure SDK for Node.js]: https://www.windowsazure.com/develop/nodejs/
[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Socket.IO GitHub deposu]: https://github.com/LearnBoost/socket.io/tree/0.9.14
[Azure Considerations]: #windowsazureconsiderations
[Hosting the Chat Example in a Worker Role]: #hostingthechatexampleinawebrole
[Summary and Next Steps]: #summary
[powershell-menu]: ./media/cloud-services-nodejs-chat-app-socketio/azure-powershell-start.png

[chat example]: https://github.com/LearnBoost/socket.io/tree/master/examples/chat
[chat-example-view]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-22.png


[chat-contents]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-5.png
[The-output-of-the-npm-install-command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-7.png
[The output of the Publish-AzureService command]: ./media/cloud-services-nodejs-chat-app-socketio/socketio-9.png





