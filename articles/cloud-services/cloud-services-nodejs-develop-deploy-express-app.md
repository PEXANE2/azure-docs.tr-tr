---
title: Azure Bulut Hizmetleri için bir Düğüm.js Express uygulaması oluşturun ve dağıtın
titleSuffix: Azure Cloud Services
description: Node.js'de Bir Express.js uygulamasını Azure Bulut Hizmetleri'ne oluşturun ve dağıtın
services: cloud-services
documentationcenter: nodejs
author: tgore03
ms.service: cloud-services
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tagore
ms.openlocfilehash: 79a998930a384420b22add8825ee4b2269eb4539
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75360761"
---
# <a name="build-and-deploy-a-nodejs-web-application-using-express-on-an-azure-cloud-services"></a>Azure Bulut Hizmetlerinde Express'i kullanarak bir Düğüm web uygulaması oluşturma ve dağıtma

Node.js, çekirdek çalışma zamanında en az işlevsellik kümesi içerir.
Geliştiriciler genellikle bir Düğüm.js uygulaması geliştirirken ek işlevsellik sağlamak için üçüncü taraf modülleri kullanır. Bu eğitimde, Node.js web uygulamaları oluşturmak için bir MVC çerçevesi sağlayan [Express](https://github.com/expressjs/express) modüllerini kullanarak yeni bir uygulama oluşturacaksınız.

Tamamlanan uygulamanın ekran görüntüsü aşağıdadır:

![Azure'da Express'e Hoş Geldiniz'i görüntüleyen bir web tarayıcısı](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="create-a-cloud-service-project"></a>Bulut Hizmeti Projesi Oluşturma
[!INCLUDE [install-dev-tools](../../includes/install-dev-tools.md)]

'expressapp' adlı yeni bir bulut hizmeti projesi oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Başlat **Menüsünden** veya **Başlat Ekranından** **Windows PowerShell'i**arayın. Son olarak, **Windows PowerShell'e** sağ tıklayın ve **Yönetici Olarak Çalıştır'ı**seçin.
   
    ![Azure PowerShell simgesi](./media/cloud-services-nodejs-develop-deploy-express-app/azure-powershell-start.png)
2. Dizinleri **\\c:düğüm** dizinini değiştirin ve **ardından expressapp** adında yeni bir çözüm ve **WebRole1**adlı bir web rolü oluşturmak için aşağıdaki komutları girin:
   
        PS C:\node> New-AzureServiceProject expressapp
        PS C:\Node\expressapp> Add-AzureNodeWebRole
        PS C:\Node\expressapp> Set-AzureServiceProjectRole WebRole1 Node 0.10.21
   
    > [!NOTE]
    > Varsayılan olarak, **Add-AzureNodeWebRole,** Düğüm.js'nin eski bir sürümünü kullanır. Yukarıdaki **Set-AzureServiceProjectRole** deyimi Azure'a Düğüm'ün v0.10.21'ini kullanmasını bildirir.  Parametrelerin büyük/küçük harf duyarlı olduğuna dikkat edin.  **WebRole1\package.json'daki** **motor** özelliğini işaretleyerek Düğüm.js'nin doğru sürümünün seçildiğini doğrulayabilirsiniz.
    > 
    > 

## <a name="install-express"></a>Express yükleme
1. Aşağıdaki komutu vererek Express jeneratörü yükleyin:
   
        PS C:\node\expressapp> npm install express-generator -g
   
    npm komutunun çıktısı aşağıdaki sonuca benzer görünmelidir. 
   
    ![NPM yükleme express komutunun çıktısını görüntüleyen Windows PowerShell.](./media/cloud-services-nodejs-develop-deploy-express-app/express-g.png)
2. **WebRole1** dizininde dizinleri değiştirin ve yeni bir uygulama oluşturmak için express komutunu kullanın:
   
        PS C:\node\expressapp\WebRole1> express
   
    Önceki başvurunuzun üzerine yazmanız istenir. Devam etmek için **y** veya **evet** girin. Express, uygulamanızı oluşturmak için app.js dosyasını ve klasör yapısını oluşturur.
   
    ![Ekspres komutun çıktısı](./media/cloud-services-nodejs-develop-deploy-express-app/node23.png)
3. package.json dosyasında tanımlanan ek bağımlılıkları yüklemek için aşağıdaki komutu girin:
   
       PS C:\node\expressapp\WebRole1> npm install
   
   ![npm yükleme komutunun çıktısı](./media/cloud-services-nodejs-develop-deploy-express-app/node26.png)
4. **Bin/www** dosyasını **server.js'ye**kopyalamak için aşağıdaki komutu kullanın. Bu, bulut hizmetinin bu uygulamanın giriş noktasını bulabilmesi içindir.
   
       PS C:\node\expressapp\WebRole1> copy bin/www server.js
   
   Bu komut tamamlandıktan sonra, WebRole1 dizininde bir **server.js** dosyanız olmalıdır.
5. '.' karakterlerinden birini aşağıdaki satırdan kaldırmak için **server.js'yi** değiştirin.
   
       var app = require('../app');
   
   Bu değişikliği yaptıktan sonra, satır aşağıdaki gibi görünmelidir.
   
       var app = require('./app');
   
   Dosyayı (eski **bin/www**,) gerekli olan uygulama dosyasıyla aynı dizine taşıdığımız için bu değişiklik gereklidir. Bu değişikliği yaptıktan sonra **server.js** dosyasını kaydedin.
6. Uygulamayı Azure emülatöründe çalıştırmak için aşağıdaki komutu kullanın:
   
       PS C:\node\expressapp\WebRole1> Start-AzureEmulator -launch
   
    ![Eksprese hoş geldiniz içeren bir web sayfası.](./media/cloud-services-nodejs-develop-deploy-express-app/node28.png)

## <a name="modifying-the-view"></a>Görünümü Değiştirme
Şimdi görünümü değiştirerek "Azure'da Express'e hoş geldiniz" iletisini görüntüleyin.

1. index.jade dosyasını açmak için aşağıdaki komutu girin:
   
       PS C:\node\expressapp\WebRole1> notepad views/index.jade
   
   ![Index.jade dosyasının içeriği.](./media/cloud-services-nodejs-develop-deploy-express-app/getting-started-19.png)
   
   Jade, Express uygulamaları tarafından kullanılan varsayılan görünüm altyapısıdır. Yeşim görünüm altyapısı hakkında daha [http://jade-lang.com][http://jade-lang.com]fazla bilgi için bkz.
2. **Azure'a**ekleyerek son metin satırını değiştirin.
   
   ![Index.jade dosyası, son satır okur: \#p Azure'da {title} hoş geldiniz](./media/cloud-services-nodejs-develop-deploy-express-app/node31.png)
3. Dosyayı kaydedin ve Not Defteri'nden çıkın.
4. Tarayıcınızı yenilerseniz değişikliklerinizi görürsünüz.
   
   ![Bir tarayıcı penceresi, sayfa Azure Express hoş geldiniz içerir](./media/cloud-services-nodejs-develop-deploy-express-app/node32.png)

Uygulamayı test ettikten sonra, emülatördurdurmak için **Dur-AzureEmulator** cmdlet'i kullanın.

## <a name="publishing-the-application-to-azure"></a>Uygulamayı Azure'da Yayımlama
Azure PowerShell penceresinde, uygulamayı bir bulut hizmetine dağıtmak için **Publish-AzureServiceProject** cmdlet'ini kullanın

    PS C:\node\expressapp\WebRole1> Publish-AzureServiceProject -ServiceName myexpressapp -Location "East US" -Launch

Dağıtım işlemi tamamlandıktan sonra tarayıcınız açılır ve web sayfasını görüntüler.

![Express sayfasını görüntüleyen bir web tarayıcısı. URL, artık Azure'da barındırılan url'yi gösterir.](./media/cloud-services-nodejs-develop-deploy-express-app/node36.png)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](https://docs.microsoft.com/azure/javascript/).

[Node.js Web Application]: https://www.windowsazure.com/develop/nodejs/tutorials/getting-started/
[Express]: https://expressjs.com/
[http://jade-lang.com]: http://jade-lang.com





