---
title: Azure İşlevlerinde Dağıtım teknolojileri
description: Kodu Azure İşlevleri'ne dağıtmanın farklı yollarını öğrenin.
author: georgewallace
ms.custom: vs-azure
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: gwallace
ms.openlocfilehash: 43352117d149abbe41ba7bf49a1ffb68e46d2707
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277134"
---
# <a name="deployment-technologies-in-azure-functions"></a>Azure İşlevlerinde Dağıtım teknolojileri

Azure İşlevler proje kodunuzu Azure'a dağıtmak için birkaç farklı teknoloji kullanabilirsiniz. Bu makalede, bu teknolojilerin ayrıntılı bir listesini sağlar, hangi teknolojilerin Işlevlerin hangi tatlar için kullanılabilir açıklar, her yöntemi kullandığınızda ne olur açıklar ve çeşitli senaryolarda kullanmak için en iyi yöntem için öneriler sağlar . Azure İşlevlerine dağıtımı destekleyen çeşitli araçlar, bağlamlarına bağlı olarak doğru teknolojiye ayarlanır. Genel olarak, zip dağıtımı Azure İşlevler için önerilen dağıtım teknolojisidir.

## <a name="deployment-technology-availability"></a>Dağıtım teknolojisi kullanılabilirliği

Azure Fonksiyonları, Windows ve Linux'ta çapraz platform yerel geliştirme ve barındırmayı destekler. Şu anda, üç barındırma planları mevcuttur:

+ [Tüketim](functions-scale.md#consumption-plan)
+ [Premium](functions-scale.md#premium-plan)
+ [Özel (Uygulama Hizmeti)](functions-scale.md#app-service-plan)

Her planın farklı davranışları vardır. Azure İşlevlerinin her bir tadı için tüm dağıtım teknolojileri kullanılamaz. Aşağıdaki grafik, işletim sistemi ve barındırma planının her birleşimi için hangi dağıtım teknolojilerinin desteklenir olduğunu gösterir:

| Dağıtım teknolojisi | Windows Tüketimi | Windows Premium | Windows Özel  | Linux Tüketimi | Linux Premium | Linux Adanmış |
|-----------------------|:-------------------:|:-------------------------:|:------------------:|:---------------------------:|:-------------:|:---------------:|
| Harici paket URL<sup>1</sup> |✔|✔|✔|✔|✔|✔|
| Zip dağıtma |✔|✔|✔|✔|✔|✔|
| Docker konteyner | | | | |✔|✔|
| Web Dağıtımı |✔|✔|✔| | | |
| Kaynak denetimi |✔|✔|✔| |✔|✔|
| Yerel Git<sup>1</sup> |✔|✔|✔| |✔|✔|
| Bulut eşitleme<sup>1</sup> |✔|✔|✔| |✔|✔|
| FTP<sup>1</sup> |✔|✔|✔| |✔|✔|
| Portal düzenleme |✔|✔|✔| |✔<sup>2</sup>|✔<sup>2</sup>|

<sup>1</sup> [Manuel tetik eşitleme](#trigger-syncing)gerektiren dağıtım teknolojisi.  
<sup>2</sup> Portal düzenlemesi yalnızca Premium ve Özel planları kullanarak Linux'taki işlevler için HTTP ve Timer tetikleyicileri için etkinleştirilir.

## <a name="key-concepts"></a>Önemli kavramlar

Bazı temel kavramlar, Azure İşlevleri'nde dağıtımların nasıl çalıştığını anlamak için önemlidir.

### <a name="trigger-syncing"></a>Tetikleyici eşitleme

Tetikleyicilerinizden herhangi birini değiştirdiğinizde, Fonksiyonlar altyapısı değişikliklerin farkında olmalıdır. Eşitleme birçok dağıtım teknolojileri için otomatik olarak gerçekleşir. Ancak, bazı durumlarda tetikleyicilerinizi el ile eşitlemeniz gerekir. Güncelleştirmelerinizi harici bir paket URL'sine, yerel Git'e, bulut eşitlemesine veya FTP'ye başvurarak dağıtırken, tetikleyicilerinizi el ile eşitlemeniz gerekir. Tetikleyicileri üç şekilde eşitleyebilirsiniz:

* Azure portalında işlev uygulamanızı yeniden başlatın
* [Ana anahtarı](functions-bindings-http-webhook-trigger.md#authorization-keys)kullanmak `https://{functionappname}.azurewebsites.net/admin/host/synctriggers?code=<API_KEY>` için bir HTTP POST isteği gönderin.
* Bir HTTP POST `https://management.azure.com/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP_NAME>/providers/Microsoft.Web/sites/<FUNCTION_APP_NAME>/syncfunctiontriggers?api-version=2016-08-01`isteği gönderin. Yer tutucuları abonelik kimliğiniz, kaynak grup adınız ve işlev uygulamanızın adı ile değiştirin.

### <a name="remote-build"></a>Uzaktan yapı

Azure İşlevler, zip dağıtımlarından sonra aldığı kod üzerinde otomatik olarak oluşturma gerçekleştirebilir. Bu yapılar, uygulamanızın Windows veya Linux'ta çalışıp çalışmadığına bağlı olarak biraz farklı davranılır. Bir uygulama daha önce [Paketten Çalıştır](run-functions-from-deployment-package.md) modunda çalışacak şekilde ayarlandığında uzaktan yapılar gerçekleştirilmez. Uzaktan yapıyı kullanmayı öğrenmek için [zip dağıtımına](#zip-deploy)gidin.

> [!NOTE]
> Uzaktan yapıyla ilgili sorunlar yaşıyorsanız, bunun nedeni uygulamanızın özellik kullanıma sunulmadan önce oluşturulmuş olması olabilir (1 Ağustos 2019). Yeni bir işlev uygulaması oluşturmayı veya işlev uygulamanızı güncellemek için çalıştırmayı `az functionapp update -g <RESOURCE_GROUP_NAME> -n <APP_NAME>` deneyin. Bu komutun başarılı olması için iki deneme gerekebilir.

#### <a name="remote-build-on-windows"></a>Windows'da uzaktan yapı

Windows'da çalışan tüm işlev uygulamalarının küçük bir yönetim uygulaması vardır, SCM (veya [Kudu)](https://github.com/projectkudu/kudu)sitesi. Bu site, Azure İşlevleri için dağıtım ve oluşturma mantığının çoğunu işler.

Bir uygulama Windows'a dağıtıldığında, (C#) `dotnet restore` veya `npm install` (JavaScript) gibi dile özgü komutlar çalıştırılır.

#### <a name="remote-build-on-linux"></a>Linux üzerinde uzaktan yapı

Linux üzerinde uzaktan yapı oluşturmayı etkinleştirmek için aşağıdaki [uygulama ayarlarının](functions-how-to-use-azure-function-app-settings.md#settings) ayarlanabilmesi gerekir:

* `ENABLE_ORYX_BUILD=true`
* `SCM_DO_BUILD_DURING_DEPLOYMENT=true`

Varsayılan olarak, hem [Azure İşlevler Temel Araçları](functions-run-local.md) hem de Visual Studio Code için Azure [İşlevler Uzantısı,](functions-create-first-function-vs-code.md#publish-the-project-to-azure) Linux'a dağıtılırken uzaktan oluşturma gerçekleştirir. Bu nedenle, her iki araç da azure'da bu ayarları sizin için otomatik olarak oluşturur. 

Uygulamalar Linux üzerinde uzaktan oluşturulduğunda, [dağıtım paketinden çalışırlar.](run-functions-from-deployment-package.md) 

##### <a name="consumption-plan"></a>Tüketim planı

Tüketim planında çalışan Linux işlev uygulamalarının dağıtım seçeneklerini sınırlayan bir SCM/Kudu sitesi yoktur. Ancak, Tüketim planında çalışan Linux'taki işlev uygulamaları uzaktan yapıyı destekler.

##### <a name="dedicated-and-premium-plans"></a>Özel ve Premium planlar

[Özel (App Service) planında](functions-scale.md#app-service-plan) ve Premium [planında](functions-scale.md#premium-plan) Linux üzerinde çalışan işlev uygulamaları da sınırlı bir SCM/Kudu sitesine sahiptir.

## <a name="deployment-technology-details"></a>Dağıtım teknolojisi ayrıntıları

Aşağıdaki dağıtım yöntemleri Azure İşlevler'de kullanılabilir.

### <a name="external-package-url"></a>Harici paket URL'si

İşlev uygulamanızı içeren uzak bir paket (.zip) dosyasına başvurmak için harici bir paket URL'si kullanabilirsiniz. Dosya sağlanan URL'den indirilir ve uygulama [Paketten Çalıştır](run-functions-from-deployment-package.md) modunda çalışır.

>__Nasıl kullanılır:__ Uygulama `WEBSITE_RUN_FROM_PACKAGE` ayarlarınıza ekleyin. Bu ayarın değeri bir URL olmalıdır (çalıştırmak istediğiniz belirli paket dosyasının konumu). [Portalda](functions-how-to-use-azure-function-app-settings.md#settings) veya [Azure CLI'yi kullanarak](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set)ayarlar ekleyebilirsiniz. 
>
>Azure Blob depolama alanını kullanıyorsanız, Işlevler'in pakete erişmesini sağlamak için [paylaşılan erişim imzasına (SAS) sahip](../vs-azure-tools-storage-manage-with-storage-explorer.md#generate-a-sas-in-storage-explorer) özel bir kapsayıcı kullanın. Uygulama yeniden başlatılsa, içeriğin bir kopyasını getirir. Başvurunuz başvurunun kullanım ömrü boyunca geçerli olmalıdır.

>__Ne zaman kullanılır:__ Harici paket URL, kullanıcı [uzaktan bir yapının](#remote-build) gerçekleşmesini istemiyorsa, Tüketim planında Linux üzerinde çalışan Azure Fonksiyonları için desteklenen tek dağıtım yöntemidir. Bir işlev uygulamasının başvurulduğu paket dosyasını güncellediğinizde, Azure'a uygulamanızın değiştiğini söylemek için [tetikleyicileri el ile eşitlemeniz](#trigger-syncing) gerekir.

### <a name="zip-deploy"></a>Zip dağıtma

İşlev uygulamanızı içeren bir .zip dosyasını Azure'a itmek için zip dağıtımı'nı kullanın. İsteğe bağlı olarak, uygulamanızı [paketten çalışmaya](run-functions-from-deployment-package.md)başlayacak şekilde ayarlayabilir veya [uzaktan bir yapının](#remote-build) oluştuğunu belirtebilirsiniz.

>__Nasıl kullanılır:__ En sevdiğiniz istemci aracını kullanarak dağıtın: [Visual Studio Code](functions-develop-vs-code.md#publish-to-azure), Visual [Studio](functions-develop-vs.md#publish-to-azure), veya Komut satırından [Azure İşlerİ Temel Araçları'nı](functions-run-local.md#project-file-deployment)kullanarak. Varsayılan olarak, bu araçlar zip dağıtım kullanın ve [paketten çalıştırın.](run-functions-from-deployment-package.md) Core Tools ve Visual Studio Code uzantısı, Linux'a dağıtılırken [uzaktan yapı oluşturmayı](#remote-build) sağlar. İşlev uygulamanıza bir .zip dosyasını el ile dağıtmak [için,.zip dosyasından veya URL'den Dağıt'taki](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url)yönergeleri izleyin.

>Zip dağıtımı kullanarak dağıtırken, uygulamanızı [paketten çalışacak](run-functions-from-deployment-package.md)şekilde ayarlayabilirsiniz. Paketten çalıştırmak için `WEBSITE_RUN_FROM_PACKAGE` uygulama ayar `1`değerini ' e ayarlayın. Zip dağıtımı öneririz. Uygulamalarınız için daha hızlı yükleme süreleri sağlar ve VS Code, Visual Studio ve Azure CLI için varsayılan değerdir. 

>__Ne zaman kullanılır:__ Zip dağıtımı, Azure İşlevleri için önerilen dağıtım teknolojisidir.

### <a name="docker-container"></a>Docker konteyner

İşlev uygulamanızı içeren bir Linux kapsayıcı görüntüsü dağıtabilirsiniz.

>__Nasıl kullanılır:__ Premium veya Özel planda bir Linux işlev uygulaması oluşturun ve hangi kapsayıcı görüntüden çalıştırılanın belirtin. Bunu iki şekilde yapabilirsiniz:
>
>* Azure portalında bir Azure Uygulama Hizmeti planında bir Linux işlevi uygulaması oluşturun. **Yayımla**için **Docker Image'ı**seçin ve ardından kapsayıcıyı yapılandırın. Resmin barındırıldığı konumu girin.
>* Azure CLI'yi kullanarak Uygulama Hizmeti planında bir Linux işlevi uygulaması oluşturun. Nasıl yapılacağını öğrenmek için [bkz.](functions-create-function-linux-custom-image.md#create-supporting-azure-resources-for-your-function)
>
>[Azure İşlevler Çekirdek Araçları'nda](functions-run-local.md)özel bir kapsayıcı kullanarak [`func deploy`](functions-run-local.md#publish) varolan bir uygulamaya dağıtmak için komutu kullanın.

>__Ne zaman kullanılır:__ İşlev uygulamanızın çalıştığı Linux ortamı üzerinde daha fazla denetime ihtiyaç duyduğunuzda Docker kapsayıcı seçeneğini kullanın. Bu dağıtım mekanizması yalnızca Linux üzerinde çalışan işlevler için kullanılabilir.

### <a name="web-deploy-msdeploy"></a>Web Dağıtımı (MSDeploy)

Web Paketleri dağıtın ve Windows uygulamalarınızı Azure'da Windows'da çalışan işlev uygulamalarınız da dahil olmak üzere herhangi bir IIS sunucusuna dağıtın.

>__Nasıl kullanılır:__ [Azure İşlevleri için Visual Studio araçlarını](functions-create-your-first-function-visual-studio.md)kullanın. Paket **dosyasından Çalıştır'ı (önerilen)** onay kutusunu temizleyin.
>
>Ayrıca Web [Deploy 3.6'yı](https://www.iis.net/downloads/microsoft/web-deploy) indirebilir ve doğrudan arayabilirsiniz. `MSDeploy.exe`

>__Ne zaman kullanılır:__ Web Deploy desteklenir ve herhangi bir sorun vardır, ancak tercih edilen mekanizma [paketten çalıştır etkinleştirildiğinde zip dağıtımıdır.](#zip-deploy) Daha fazla bilgi için [Visual Studio geliştirme kılavuzuna](functions-develop-vs.md#publish-to-azure)bakın.

### <a name="source-control"></a>Kaynak denetimi

İşlev uygulamanızı git deposuna bağlamak için kaynak denetimini kullanın. Bu depodaki kod güncelleştirmesi dağıtımı tetikler. Daha fazla bilgi için [Kudu Wiki sayfasına](https://github.com/projectkudu/kudu/wiki/VSTS-vs-Kudu-deployments)bakın.

>__Nasıl kullanılır:__ Kaynak denetiminden yayımlamayı ayarlamak için portalın İşlevler alanında Dağıtım Merkezi'ni kullanın. Daha fazla bilgi için Azure [İşlevler için Sürekli dağıtım'a](functions-continuous-deployment.md)bakın.

>__Ne zaman kullanılır:__ Kaynak denetimini kullanmak, işlev uygulamaları üzerinde işbirliği yapan ekipler için en iyi yöntemdir. Kaynak denetimi, daha gelişmiş dağıtım ardışık hatlarına olanak tanıyan iyi bir dağıtım seçeneğidir.

### <a name="local-git"></a>Yerel Git

Yerel Git'i kullanarak yerel makinenizden Azure İşlevlerine kod itmek için kullanabilirsiniz.

>__Nasıl kullanılır:__ Azure Uygulama [Hizmeti'ne Yerel Git dağıtımındaki](../app-service/deploy-local-git.md)yönergeleri izleyin.

>__Ne zaman kullanılır:__ Genel olarak, farklı bir dağıtım yöntemi kullanmanızı öneririz. Yerel Git'den yayımladığınızda, [tetikleyicileri el ile eşitlemeniz](#trigger-syncing)gerekir.

### <a name="cloud-sync"></a>Bulut eşitleme

İçeriğinizi Dropbox ve OneDrive'dan Azure İşlerinden Azure Fonksiyonları'na senkronize etmek için bulut eşitlemesini kullanın.

>__Nasıl kullanılır:__ [Bulut klasöründen Eşitleme içeriğindeki](../app-service/deploy-content-sync.md)yönergeleri izleyin.

>__Ne zaman kullanılır:__ Genel olarak, diğer dağıtım yöntemlerini öneririz. Bulut eşitlemeyi kullanarak yayımlama yaptığınızda, [tetikleyicileri el ile eşitlemeniz](#trigger-syncing)gerekir.

### <a name="ftp"></a>FTP

Dosyaları doğrudan Azure İşlevleri'ne aktarmak için FTP'yi kullanabilirsiniz.

>__Nasıl kullanılır:__ [FTP/s kullanarak içeriği dağıt'taki](../app-service/deploy-ftp.md)yönergeleri izleyin.

>__Ne zaman kullanılır:__ Genel olarak, diğer dağıtım yöntemlerini öneririz. FTP kullanarak [yayımladığınızda, tetikleyicileri el ile eşitlemeniz](#trigger-syncing)gerekir.

### <a name="portal-editing"></a>Portal düzenleme

Portal tabanlı düzenleyicide, işlev uygulamanızdaki dosyaları doğrudan güncelleyebilirsiniz (değişikliklerinizi her kaydedişinizde dağıtAbilirsiniz).

>__Nasıl kullanılır:__ Azure portalındaki işlevlerinizi yeniden oluşturabilmek [için, işlevlerinizi portalda oluşturmuş](functions-create-first-azure-function.md)olmalısınız. Tek bir doğruluk kaynağını korumak için, başka bir dağıtım yöntemi kullanmak işlevinizi salt okunur hale getirir ve devam eden portal düzenlemesini engeller. Dosyalarınızı Azure portalında değiştirebileceğiniz bir duruma dönmek için, `Read/Write` dağıtımla ilgili uygulama ayarlarını (benzer şekilde) `WEBSITE_RUN_FROM_PACKAGE`el ile yeniden döndürebilir ve kaldırabilirsiniz. 

>__Ne zaman kullanılır:__ Portal, Azure İşlevleri ile başlamak için iyi bir yoldur. Daha yoğun geliştirme çalışmaları için aşağıdaki istemci araçlarından birini kullanmanızı öneririz:
>
>* [Visual Studio Code](functions-create-first-function-vs-code.md)
>* [Azure İşletim Temel Araçları (komut satırı)](functions-run-local.md)
>* [Visual Studio](functions-create-your-first-function-visual-studio.md)

Aşağıdaki tablo, portal düzenlemeyi destekleyen işletim sistemlerini ve dilleri gösterir:

| | Windows Tüketimi | Windows Premium | Windows Özel | Linux Tüketimi | Linux Premium | Linux Adanmış |
|-|:-----------------: |:----------------:|:-----------------:|:-----------------:|:-------------:|:---------------:|
| C# | | | | | |
| C# Komut Dosyası |✔|✔|✔| |✔<sup>\*</sup> |✔<sup>\*</sup>|
| F# | | | | | | |
| Java | | | | | | |
| JavaScript (Node.js) |✔|✔|✔| |✔<sup>\*</sup>|✔<sup>\*</sup>|
| Python (Önizleme) | | | | | | |
| PowerShell (Önizleme) |✔|✔|✔| | | |
| TypeScript (Düğüm.js) | | | | | | |

<sup>*</sup>Portal düzenleme, Premium ve Özel planları kullanarak Linux'taki işlevler için yalnızca HTTP ve Timer tetikleyicileri için etkinleştirilir.

## <a name="deployment-slots"></a>Dağıtım yuvaları

İşlev uygulamanızı Azure'a dağıttığınızda, doğrudan üretim yerine ayrı bir dağıtım yuvasına dağıtabilirsiniz. Dağıtım yuvaları hakkında daha fazla bilgi için ayrıntılar için [Azure İşlevler Dağıtım Yuvaları](../app-service/deploy-staging-slots.md) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar

İşlev uygulamalarınızı dağıtma hakkında daha fazla bilgi edinmek için bu makaleleri okuyun: 

+ [Azure İşlevler için sürekli dağıtım](functions-continuous-deployment.md)
+ [Azure DevOps kullanarak sürekli teslim](functions-how-to-azure-devops.md)
+ [Azure İşlevleri için zip dağıtımları](deployment-zip-push.md)
+ [Azure İşlevlerinizi bir paket dosyasından çalıştırma](run-functions-from-deployment-package.md)
+ [Azure İşlevlerinde işlev uygulamanız için kaynak dağıtımını otomatikleştirin](functions-infrastructure-as-code.md)
