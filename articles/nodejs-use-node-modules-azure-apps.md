---
title: Node.js Modülleri ile Çalışma
description: Azure Uygulama Hizmeti veya Bulut Hizmetleri kullanırken Node.js modülleriyle nasıl çalışacağınızı öğrenin.
services: ''
documentationcenter: nodejs
author: rloutlaw
manager: rloutlaw
editor: ''
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: routlaw
ms.openlocfilehash: 61be6bcd957a4e81147d5ef472b8f850e5605e41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70309268"
---
# <a name="using-nodejs-modules-with-azure-applications"></a>Azure uygulamalarıyla Node.js Modüllerini kullanma
Bu belge, Azure'da barındırılan uygulamalarla Node.js modüllerini kullanma konusunda kılavuz sağlar. Uygulamanızın bir modülün belirli bir sürümünü kullanmasını sağlamanın yanı sıra Azure ile yerel modülleri kullanma konusunda da kılavuzluk sağlar.

Node.js modüllerini, **package.json** ve **npm-shrinkwrap.json** dosyalarını kullanmaya zaten aşinaysanız, aşağıdaki bilgiler bu makalede tartışılanların hızlı bir özetini sağlar:

* Azure App Service **package.json** ve **npm-shrinkwrap.json** dosyalarını anlar ve bu dosyalardaki girişlere göre modüller yükleyebilir.

* Azure Bulut Hizmetleri, tüm modüllerin geliştirme ortamına yüklenmesini ve **\_düğüm modülleri** dizininin dağıtım paketinin bir parçası olarak eklenmesini bekler. Bulut Hizmetleri'nde **package.json** veya **npm-shrinkwrap.json** dosyalarını kullanarak modül yükleme desteği sağlamak mümkündür; ancak, bu yapılandırma Bulut Hizmeti projeleri tarafından kullanılan varsayılan komut dosyalarının özelleştirilmesini gerektirir. Bu ortamı nasıl yapılandırılabildiğini gösteren bir örnek için, [düğüm modüllerini dağıtmamak için npm yüklemesini çalıştırmak için Azure Başlangıç görevine](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown) bakın

> [!NOTE]
> Bir VM'deki dağıtım deneyimi Sanal Makine tarafından barındırılan işletim sistemine bağlı olduğundan, Azure Sanal Makineler bu makalede açıklanmaz.
> 
> 

## <a name="nodejs-modules"></a>Düğüm.js Modülleri
Modüller, uygulamanız için özel işlevler sağlayan yüklenebilir JavaScript paketleridir. Modüller genellikle **npm** komut satırı aracı kullanılarak yüklenir, ancak bazı modüller (http modülü gibi) çekirdek Node.js paketinin bir parçası olarak sağlanır.

Modüller yüklendiğinde, uygulama dizini yapınızın kökündeki **\_düğüm modülleri** dizininde depolanır. **Düğüm\_modülleri** dizinindeki her modül, bağlı olduğu tüm modülleri içeren kendi dizinini korur ve bu davranış bağımlılık zincirinin aşağısındaki her modül için tekrareder. Bu ortam, yüklü her modülün bağlı olduğu modüller için kendi sürüm gereksinimlerine sahip olmasını sağlar, ancak oldukça büyük bir dizin yapısına neden olabilir.

Uygulamanızın bir parçası olarak **düğüm\_modülleri** dizinini dağıtmak, **bir package.json** veya **npm-shrinkwrap.json** dosyasını kullanmaya kıyasla dağıtımBoyutunu artırır; ancak, üretimde kullanılan modüllerin geliştirmede kullanılan modüllerle aynı olduğunu garanti eder.

### <a name="native-modules"></a>Yerli Modüller
Çoğu modül sadece düz metin JavaScript dosyaları olsa da, bazı modüller platforma özgü ikili görüntülerdir. Bu modüller yükleme zamanında, genellikle Python ve düğüm-gyp kullanılarak derlenir. Azure Bulut Hizmetleri, uygulamanın bir parçası olarak dağıtılan **\_düğüm modülleri** klasörüne güvendiğinden, yüklenen modüllerin bir parçası olarak yer alan tüm yerel modüller, windows geliştirme sistemine yüklenmiş ve derlenmiş olduğu sürece bir bulut hizmetinde çalışmalıdır.

Azure Uygulama Hizmeti tüm yerel modülleri desteklemez ve modülleri belirli ön koşullara sahip derlerken başarısız olabilir. MongoDB gibi bazı popüler modüller isteğe bağlı yerel bağımlılıklara sahip ken ve onlar olmadan gayet iyi çalışırken, iki geçici çözüm bugün mevcut olan hemen hemen tüm yerel modüllerle başarılı oldu:

* Tüm yerel modülün ön koşulları yüklü olan bir Windows makinesinde **npm yükleme** çalıştırın. Ardından, oluşturulan **\_düğüm modülleri** klasörünü uygulamanın bir parçası olarak Azure Uygulama Hizmeti'ne dağıtın.

  * Derlemeden önce, yerel Düğüm.js yüklemenizin eşleşen mimariye sahip olup olmadığını ve sürümün Azure'da kullanılana mümkün olduğunca yakın olduğundan kontrol edin (geçerli değerler properties **process.arch** and **process.version'dan**çalışma zamanında denetlenebilir).

* Azure Uygulama Hizmeti, dağıtım sırasında özel bash veya kabuk komutları yürütmek üzere yapılandırılabilir ve böylece özel komutları yürütme ve **npm yüklemenin** çalışma şeklini tam olarak yapılandırma fırsatı verir. Bu ortamın nasıl yapılandırılabildiğini gösteren bir video için [Kudu ile Özel Web Sitesi Dağıtım Komut Dosyaları'na](https://azure.microsoft.com/resources/videos/custom-web-site-deployment-scripts-with-kudu/)bakın.

### <a name="using-a-packagejson-file"></a>package.json dosyakullanma

**Package.json** dosyası, dağıtımın bir parçası olarak **\_düğüm modülleri** klasörünü eklemenizi gerektirmek yerine, barındırma platformunun bağımlılıkları yükleyebilmeleri için uygulamanızın gerektirdiği en üst düzey bağımlılıkları belirtmenin bir yoludur. Uygulama dağıtıldıktan sonra, **npm install** komutu **package.json** dosyasını ayrıştırmak ve listelenen tüm bağımlılıkları yüklemek için kullanılır.

Geliştirme sırasında, modülleri yüklerken otomatik olarak **package.json** dosyanıza modül girişi eklerken **--kaydet,** **--kaydet, -kaydet**veya **isteğe bağlı** parametreleri kaydet' i kullanabilirsiniz. Daha fazla bilgi için [npm-install'a](https://docs.npmjs.com/cli/install)bakın.

**package.json** dosyasıyla ilgili olası bir sorun, yalnızca üst düzey bağımlılıklar için sürümü belirtmesidir. Yüklü olan her modül, bağlı olduğu modüllerin sürümünü belirtebilir veya belirtmeyebilir ve bu nedenle geliştirmede kullanılandan farklı bir bağımlılık zincirine sahip olabilirsiniz.

> [!NOTE]
> Azure Uygulama Hizmeti'ne dağıtılırken, <b>package.json</b> dosyanız yerel bir modüle başvuruyorsa, uygulamayı Git kullanarak yayınlarken aşağıdaki örneğe benzer bir hata görebilirsiniz:
> 
> npm ERR! module-name@0.6.0yüklemek: 'düğüm-gyp yapıyapıyapı'
> 
> npm ERR! 'cmd "/c" "düğüm-gyp yapılandırma yapı"' ile başarısız oldu 1
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>npm-shrinkwrap.json dosyakullanma
**npm-shrinkwrap.json** **dosyası, package.json** dosyasının modül sürüm sınırlamalarını giderme girişimidir. **package.json** dosyası yalnızca üst düzey modüller için sürümler içerse de, **npm-shrinkwrap.json** dosyası tam modül bağımlılık zinciri için sürüm gereksinimlerini içerir.

Uygulamanız üretime hazır olduğunda, sürüm gereksinimlerini kilitleyebilir ve **npm shrinkwrap** komutunu kullanarak bir **npm-shrinkwrap.json** dosyası oluşturabilirsiniz. Bu komut, düğüm **\_modülleri** klasöründe şu anda yüklü sürümleri kullanır ve bu sürümleri **npm-shrinkwrap.json** dosyasına kaydeder. Uygulama barındırma ortamına dağıtıldıktan sonra, **npm yükleme** komutu **npm-shrinkwrap.json** dosyasını ayrıştırmak ve listelenen tüm bağımlılıkları yüklemek için kullanılır. Daha fazla bilgi için [npm-shrinkwrap'a](https://docs.npmjs.com/cli/shrinkwrap)bakın.

> [!NOTE]
> Azure Uygulama Hizmeti'ne dağıtılırken, <b>npm-shrinkwrap.json</b> dosyanız yerel bir modüle başvuruyorsa, uygulamayı Git kullanarak yayınlarken aşağıdaki örneğe benzer bir hata görebilirsiniz:
> 
> npm ERR! module-name@0.6.0yüklemek: 'düğüm-gyp yapıyapıyapı'
> 
> npm ERR! 'cmd "/c" "düğüm-gyp yapılandırma yapı"' ile başarısız oldu 1
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Artık Azure ile Node.js modüllerini nasıl kullanacağınızı anladığınıza göre, [Node.js sürümünü](https://github.com/squillace/staging/blob/master/articles/nodejs-specify-node-version-azure-apps.md)nasıl belirtin, [bir Node.js web uygulaması oluşturup dağıtabileceğinizi ve](app-service/app-service-web-get-started-nodejs.md)Mac ve Linux için Azure Komut Satırı [Arabirimini nasıl kullanacağınızı](https://azure.microsoft.com/blog/using-windows-azure-with-the-command-line-tools-for-mac-and-linux/)öğrenin.

Daha fazla bilgi için bkz. [Node.js Geliştirici Merkezi](/azure/javascript/).

[specify the Node.js version]: nodejs-specify-node-version-azure-apps.md
[How to use the Azure Command-Line Interface for Mac and Linux]:cli-install-nodejs.md
[Custom Website Deployment Scripts with Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
