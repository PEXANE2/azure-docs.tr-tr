---
title: VS Kodu Ile Azure Servis Kumaşı Başlıyor
description: Bu makale, Visual Studio Code kullanarak Service Fabric uygulamaları oluşturmaya genel bir bakıştır.
author: peterpogorski
ms.topic: article
ms.date: 06/29/2018
ms.author: pepogors
ms.openlocfilehash: 17bff459a0399d39c6bfdd772ad16d0b5b2f6771
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258492"
---
# <a name="service-fabric-for-visual-studio-code"></a>Visual Studio Code için Servis Kumaşı

[VS Code için Service Fabric Reliable Services uzantısı,](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-service-fabric-reliable-services) Windows, Linux ve macOS işletim sistemlerinde Service Fabric uygulamaları oluşturmak, oluşturmak ve hata ayıklamak için gerekli araçları sağlar.

Bu makalede, uzantı gereksinimleri ve kurulumu yanı sıra uzantısı tarafından sağlanan çeşitli komutların kullanımı genel bir bakış sağlar. 

> [!IMPORTANT]
> Service Fabric Java uygulamaları Windows makinelerde geliştirilebilir, ancak yalnızca Azure Linux kümelerine dağıtılabilir. Java uygulamalarının hata ayıklanması Windows'da desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki ön koşullar tüm ortamlara yüklenmelidir.

* [Visual Studio Code](https://code.visualstudio.com/)
* [Node.js](https://nodejs.org/)
* [Git](https://git-scm.com/)
* [Servis Kumaş ı SDK](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)
* Yeoman Jeneratörler - uygulamanız için uygun jeneratörler yükleyin

   ```sh
   npm install -g yo
   npm install -g generator-azuresfjava
   npm install -g generator-azuresfcsharp
   npm install -g generator-azuresfcontainer
   npm install -g generator-azuresfguest
   ```

Java geliştirme için aşağıdaki ön koşullar yüklenmiş olmalıdır:

* [Java SDK](https://aka.ms/azure-jdks) (sürüm 1.8)
* [Gradle](https://gradle.org/install/)
* [Java VS Kodu uzantısı için Hata Ayıklama](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) Java hizmetlerini hata ayıklamak için gerekli. Java hizmetlerinin hata ayıklanması yalnızca Linux'ta desteklenir. VS Kodu'ndaki **Etkinlik Çubuğu'ndaki** Uzantılar simgesine tıklayarak ve uzantıyı arayarak veya VS Kodu Marketplace'ten yükleyebilirsiniz.

.NET Core/C# geliştirme için aşağıdaki ön koşullar yüklenmelidir:

* [.NET Core](https://www.microsoft.com/net/learn/get-started) (sürüm 2.0.0 veya sonrası)
* [C# Visual Studio Code (OmniSharp tarafından desteklenmektedir) VS Kod uzantısı için](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) C# hizmetlerini hata ayıklamak için gerekli. VS Kodu'ndaki **Etkinlik Çubuğu'ndaki** Uzantılar simgesine tıklayarak ve uzantıyı arayarak veya VS Kodu Marketplace'ten yükleyebilirsiniz.

## <a name="setup"></a>Kurulum

1. VS Kodunu açın.
2. VS Kodu'nun sol tarafındaki **Etkinlik Çubuğu'ndaki** Uzantılar simgesini tıklatın. "Servis Kumaşı" için arama yapın. Servis Kumaşı Güvenilir Hizmetler uzantısı için **Yükle'yi** tıklatın.

## <a name="commands"></a>Komutlar
VS Code için Hizmet Kumaşı Güvenilir Hizmetler uzantısı, geliştiricilerin Service Fabric projeleri oluşturmasına ve dağıtmasına yardımcı olmak için birçok komut sağlar. Komut Paleti'nden **Command Palette** komutları, giriş `(Ctrl + Shift + p)`çubuğuna komut adını yazarak ve istek listesinden istenen komutu seçerek arayabilirsiniz. 

* Hizmet Kumaşı: Uygulama Oluştur 
* Hizmet Kumaşı: Uygulama Yayınla 
* Hizmet Kumaşı: Uygulamayı Dağıt 
* Servis Kumaşı: Uygulamayı Kaldır  
* Servis Kumaşı: Yapı Uygulaması 
* Servis Kumaşı: Temiz Uygulama 

### <a name="service-fabric-create-application"></a>Hizmet Kumaşı: Uygulama Oluştur

**Hizmet Kumaşı: Uygulama Oluştur** komutu, geçerli çalışma alanınızda yeni bir Hizmet Kumaşı uygulaması oluşturur. Geliştirme makinenize hangi yeoman jeneratörlerin kurulduğuna bağlı olarak, Java, C#, Container ve Guest projeleri de dahil olmak üzere çeşitli Hizmet Kumaşı uygulamaları oluşturabilirsiniz. 

1.  Hizmet **Dokusunu Seçin: Uygulama** komutu oluştur
2.  Yeni Service Fabric uygulamanızın türünü seçin. 
3.  Oluşturmak istediğiniz uygulamanın adını girin
3.  Service Fabric uygulamanıza eklemek istediğiniz hizmet türünü seçin. 
4.  Hizmetin adını vermek için istemleri izleyin. 
5.  Yeni Hizmet Kumaşı uygulaması çalışma alanında görünür.
6.  Yeni uygulama klasörünü, çalışma alanındaki kök klasör olacak şekilde açın. Komutları buradan yürütmeye devam edebilirsiniz.

### <a name="service-fabric-add-service"></a>Servis Kumaşı: Hizmet Ekle
**Hizmet Kumaşı: Hizmet Ekle** komutu, varolan bir Hizmet Kumaşı uygulamasına yeni bir hizmet ekler. Hizmetin ekeceği uygulama, çalışma alanının kök dizini olmalıdır. 

1.  Hizmet **Kumaşı'nı seçin: Hizmet ekle** komutu.
2.  Geçerli Servis Kumaşı uygulamanızın türünü seçin. 
3.  Service Fabric uygulamanıza eklemek istediğiniz hizmet türünü seçin. 
4.  Hizmetin adını vermek için istemleri izleyin. 
5.  Yeni hizmet proje dizininizde görünür. 

### <a name="service-fabric-publish-application"></a>Hizmet Kumaşı: Uygulama Yayınla
**Hizmet Kumaşı: Uygulama Yayımla** komutu, Servis Kumaşı uygulamanızı uzak bir kümeye dağır. Hedef küme güvenli veya güvenli olmayan bir küme olabilir. Parametreler Cloud.json'da ayarlanmazsa, uygulama yerel kümeye dağıtılır.

1.  Uygulama ilk oluşturulduğunda, proje dizininde bir Cloud.json dosyası oluşturulur.
2.  Cloud.json dosyasına bağlanmak istediğiniz kümenin değerlerini girdi.
3.  Hizmet **Dokusunu seçin: Uygulama** komutunu yayımla.
4.  Uygulamanın yüklendiğini doğrulamak için Hizmet Kumaş Explorer ile hedef kümeyi görüntüleyin. 

### <a name="service-fabric-deploy-application-localhost"></a>Hizmet Kumaşı: Uygulama Dağıtma (Localhost)
**Hizmet Kumaşı: Uygulama Dağıtma** komutu, Service Fabric uygulamanızı yerel kümenize dağır. Komutu kullanmadan önce yerel kümenizin çalıştığından emin olun. 

1. Hizmet **Dokusunu Seçin: Uygulama** komutunu dağıt
2. Uygulamanın yüklendiğini doğrulamak için Service\/Fabric Explorer (http: /localhost:19080/Explorer) ile yerel kümeyi görüntüleyin. Bu biraz zaman alabilir, bu yüzden sabırlı olun.
3. Yerel bir kümeye dağıtmak için Cloud.json dosyasında parametreler ayarlı hizmet **dokusu: Uygulama** yayımla komutunu da kullanabilirsiniz.

> [!NOTE]
> Java uygulamalarını yerel kümeye dağıtmak Windows makinelerinde desteklenmez.

### <a name="service-fabric-remove-application"></a>Servis Kumaşı: Uygulamayı Kaldır
**Hizmet Kumaşını Kaldır: Uygulama** yı kaldır komutu, daha önce vs kodu uzantısını kullanmak için daha önce dağıtılmış olduğu kümeden bir Hizmet Kumaşı uygulamasını kaldırır. 

1.  Hizmet **Kumaşı'nı seçin: Uygulama** komutunu kaldırın.
2.  Uygulamanın kaldırıldığını doğrulamak için Cluster Fabric Explorer ile görüntüleyin. Bu biraz zaman alabilir, bu yüzden sabırlı olun.

### <a name="service-fabric-build-application"></a>Servis Kumaşı: Yapı Uygulaması
**Hizmet Kumaşı: Build Application** komutu Java veya C# Service Fabric uygulamaları oluşturabilir. 

1.  Bu komutu yürütmeden önce uygulama kök klasöründe olduğundan emin olun. Komut uygulama türünü (C# veya Java) tanımlar ve uygulamanızı buna göre oluşturur.
2.  Hizmet **Kumaşı'nı seçin: Uygulama** komutu oluşturun.
3.  Yapı işleminin çıktısı entegre terminale yazılır.

### <a name="service-fabric-clean-application"></a>Servis Kumaşı: Temiz Uygulama
**Hizmet Dokusu: Temiz Uygulama** komutu, yapı tarafından oluşturulan tüm kavanoz dosyalarını ve yerel kitaplıkları siler. Yalnızca Java uygulamaları için geçerlidir. 

1.  Bu komutu yürütmeden önce uygulama kök klasöründe olduğundan emin olun. 
2.  Servis Kumaşı'nı seçin: Uygulama komutunu **temizleyin.**
3.  Temiz işlemin çıktısı entegre terminale yazılır.

## <a name="next-steps"></a>Sonraki adımlar

* VS Code ile C# Service Fabric uygulamalarını nasıl [geliştirip hata ayıklayarak](./service-fabric-develop-csharp-applications-with-vs-code.md)hata ayıklamayı öğrenin.
* VS Code ile Java Service Fabric uygulamalarını nasıl [geliştirip hata](./service-fabric-develop-java-applications-with-vs-code.md)ayıklayarak hata ayıklamayı öğrenin.
