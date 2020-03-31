---
title: Yerleşik kapsayıcıları çalıştırma SSS
description: Azure Uygulama Hizmeti'nde yerleşik Linux kapları hakkında sık sorulan soruların yanıtlarını bulun.
keywords: azure uygulama hizmeti, web uygulaması, sSS, linux, oss, konteynerler için web uygulaması, çoklu konteyner, multicontainer
author: msangapu-msft
ms.topic: article
ms.date: 10/30/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: f0a8b1758571a9473402d11a4d5141a11f76504d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80245829"
---
# <a name="azure-app-service-on-linux-faq"></a>Linux’ta Azure App Service hakkında SSS

Linux'ta App Service'in yayınlanmasıyla birlikte, özellikler ekleme ve platformumuza iyileştirmeler yapma üzerinde çalışıyoruz. Bu makale, müşterilerimizin son zamanlarda bize sorduğu soruların yanıtlarını sunmaktadır.

Bir sorunuz varsa, bu makaleye yorum yapın.

## <a name="built-in-images"></a>Dahili görüntüler

**Platformun sağladığı dahili Docker konteynerlerini çatallamak istiyorum. Bu dosyaları nerede bulabilirim?**

Tüm Docker dosyalarını [GitHub'da](https://github.com/azure-app-service)bulabilirsiniz. [Docker Hub'daki](https://hub.docker.com/u/appsvc/)tüm Docker konteynerlerini bulabilirsiniz.

<a id="#startup-file"></a>

**Çalışma zamanı yığınını yapılandırdığımda Başlangıç Dosyası bölümü için beklenen değerler nelerdir?**

| Yığın           | Beklenen Değer                                                                         |
|-----------------|----------------------------------------------------------------------------------------|
| Java SE         | JAR uygulamanızı başlatmak için komut `java -jar /home/site/wwwroot/app.jar --server.port=80`(örneğin, ) |
| Tomcat          | gerekli yapılandırmaları gerçekleştirmek için bir komut dosyasının `/home/site/deployments/tools/startup_script.sh`konumu (örneğin, )          |
| Node.js         | PM2 yapılandırma dosyası veya komut dosyası dosyanız                                |
| .NET Core       | olarak derlenmiş DLL adı`dotnet <myapp>.dll`                                 |
| Ruby            | uygulamanızı başlangıç olarak vermek istediğiniz Ruby komut dosyası                     |

Bu komutlar veya komut dosyaları yerleşik Docker kapsayıcısı başlatıldıktan sonra, ancak uygulama kodunuz başlatılmadan önce yürütülür.

## <a name="management"></a>Yönetim

**Azure portalındaki yeniden başlatma düğmesine bastığımda ne olur?**

Bu eylem Docker yeniden başlatma ile aynıdır.

**Uygulama konteynersanal makinesine (VM) bağlanmak için Secure Shell (SSH) kullanabilir miyim?**

Evet, bunu kaynak denetim yönetimi (SCM) sitesi aracılığıyla yapabilirsiniz.

> [!NOTE]
> SSH, SFTP veya Visual Studio Code (Node.js apps canlı hata ayıklaması için) kullanarak doğrudan yerel geliştirme makinenizden de uygulama kapsayıcısına bağlanabilirsiniz. Daha fazla bilgi için bkz. [Linux üzerinde App Service’te uzaktan hata ayıklama ve SSH](https://azure.github.io/AppService/2018/05/07/New-SSH-Experience-and-Remote-Debugging-for-Linux-Web-Apps.html).
>

**Bir SDK veya Azure Kaynak Yöneticisi şablonu aracılığıyla nasıl bir Linux Uygulama Hizmeti planı oluşturabilirim?**

Uygulama hizmetinin **ayrılmış** alanını *doğru*olarak ayarlayın.

## <a name="continuous-integration-and-deployment"></a>Sürekli tümleştirme ve dağıtım

**Web uygulamam, Docker Hub'daki resmi güncelledikten sonra hala eski bir Docker kapsayıcı görüntüsü kullanır. Sürekli tümleştirmeve özel kapsayıcıların dağıtımını destekliyor musunuz?**

Evet, Kapsayıcılar için [Web Uygulamasıyla Sürekli Dağıtım'ı](./app-service-linux-ci-cd.md)izleyerek Azure Kapsayıcı Kayıt Defteri veya DockerHub için sürekli tümleştirme/dağıtım ayarlamak için. Özel kayıt defterleri için, web uygulamanızı durdurup başlatarak kapsayıcıyı yenileyebilirsiniz. Veya kapsayıcınızın yenilenmesini zorlamak için sahte bir uygulama ayarını değiştirebilir veya ekleyebilirsiniz.

**Evreleme ortamlarını destekliyor musunuz?**

Evet.

**Web uygulamamı dağıtmak için *WebDeploy/MSDeploy'ı* kullanabilir miyim?**

Evet, `WEBSITE_WEBDEPLOY_USE_SCM` *false*adlı bir uygulama ayarı ayarlamanız gerekir.

**Linux web uygulamasını kullanırken uygulamamın Git dağıtımı başarısız olur. Sorunu nasıl çözebilirim?**

Git dağıtımı Linux web uygulamanızda başarısız olursa, uygulama kodunuzu dağıtmak için aşağıdaki seçeneklerden birini seçin:

- Sürekli Teslim (Önizleme) özelliğini kullanın: Azure Sürekli Teslim'i kullanmak için uygulamanızın kaynak kodunu bir Azure DevOps Git repo'sunda veya GitHub repo'sunda saklayabilirsiniz. Daha fazla bilgi için, [Linux web uygulaması için Sürekli Teslimat'ı nasıl yapılandırılatırınız.](https://blogs.msdn.microsoft.com/devops/2017/05/10/use-azure-portal-to-setup-continuous-delivery-for-web-app-on-linux/)

- ZIP [deploy API'yi](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)kullanın : Bu API'yi kullanmak için, [Web uygulamanızda SSH](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-ssh-support) ve kodunuzu dağıtmak istediğiniz klasöre gidin. Aşağıdaki kodu çalıştırın:

   ```bash
   curl -X POST -u <user> --data-binary @<zipfile> https://{your-sitename}.scm.azurewebsites.net/api/zipdeploy
   ```

   Komutun `curl` bulunamadıkbir hata alırsanız, önceki `apt-get install curl` `curl` komutu çalıştırmadan önce kullanarak kıvırma yüklediğinizden emin olun.

## <a name="language-support"></a>Dil desteği

**Düğüm uygulamamda, ayarlı özel ayarlarda veya yapılandırmalarımda web soketleri kullanmak istiyorum.**

Evet, sunucu `perMessageDeflate` tarafındaki Düğüm.js kodunuzda devre dışı kalım. Örneğin, socket.io kullanıyorsanız, aşağıdaki kodu kullanın:

```nodejs
const io = require('socket.io')(server,{
  perMessageDeflate :false
});
```

**Derlenmemiş .NET Core uygulamalarını destekliyor musunuz?**

Evet.

**Php uygulamaları için bağımlılık yöneticisi olarak Besteci'yi destekliyor musunuz?**

Evet, Git dağıtımı sırasında Kudu bir PHP uygulaması (bir composer.lock dosyasının varlığı sayesinde) dağıtdığınızı algılamalıdır ve Kudu daha sonra bir besteci yüklemesini tetikler.

## <a name="custom-containers"></a>Özel kapsayıcılar

**Kendi özel kabımı kullanıyorum. Platformun `/home/` dizine bir Kobİ payı monte etmesini istiyorum.**

`WEBSITES_ENABLE_APP_SERVICE_STORAGE` Ayar **belirtilmemişse** veya *doğru* `/home/` olarak ayarlanmışsa, dizin ölçek örnekleri arasında **paylaşılır** ve yazılan dosyalar yeniden **başlatmalarda devam eder.** Açıkça `WEBSITES_ENABLE_APP_SERVICE_STORAGE` *yanlış* ayarı montaj devre dışı bırakacaktır.

**Özel imletime başlamak uzun zaman alır ve platform kap bitmeden önce kapsayıcıyı yeniden başlatır.**

Platformun kapsayıcınızı yeniden başlatmadan önce bekleyeceği süreyi yapılandırabilirsiniz. Bunu yapmak için, `WEBSITES_CONTAINER_START_TIME_LIMIT` uygulama ayarını istediğiniz değere ayarlayın. Varsayılan değer 230 saniye, en büyük değer ise 1800 saniyedir.

**Özel kayıt defteri sunucusu URL'sinin biçimi nedir?**

Dahil olmak üzere tam kayıt `http://` `https://`defteri URL'sini sağlayın veya.

**Özel kayıt defteri seçeneğindeki resim adının biçimi nedir?**

Özel kayıt defteri URL'si de dahil olmak üzere tam resim adını ekleyin (örneğin, myacr.azurecr.io/dotnet:latest). Özel bir bağlantı noktası kullanan görüntü adları [portaldan girilemez.](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650) Ayarlamak `docker-custom-image-name` [ `az` için, komut satırı aracını](https://docs.microsoft.com/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set)kullanın.

**Özel kapsayıcı resmimde birden fazla bağlantı noktasını ortaya çıkarabilir miyim?**

Birden fazla limanın teşhir edilmesine destek değiliz.

**Kendi depomu getirebilir miyim?**

Evet, [kendi depolama getir](https://docs.microsoft.com/azure/app-service/containers/how-to-serve-content-from-azure-storage) önizleme de.

**Özel konteynerimin dosya sistemine veya SCM sitesinden çalışan işlemlere neden göz atamıyorum?**

SCM sitesi ayrı bir kapsayıcıda çalışır. Dosya sistemini veya uygulama kapsayıcısının çalışma işlemlerini denetleyemezsiniz.

**Benim özel konteyner port 80 dışında bir bağlantı noktası dinler. Uygulamamı istekleri bu bağlantı noktasına yönlendirecek şekilde nasıl yapılandırabilirim?**

Otomatik bağlantı noktası tespitimiz var. Ayrıca *WEBSITES_PORT* adlı bir uygulama ayarı belirtebilir ve beklenen bağlantı noktası numarasının değerini verebilirsiniz. Daha önce, platform *PORT* uygulama ayarı kullanılır. Bu uygulama ayarını amortismana kayırmayı ve *sadece WEBSITES_PORT* kullanmayı planlıyoruz.

**Özel konteynerimde HTTPS uygulamam gerekiyor mu?**

Hayır, platform paylaşılan ön uçlarda HTTPS sonlandırma işlemlerini işler.

## <a name="multi-container-with-docker-compose"></a>Docker Compose ile çoklu konteyner

**Azure Kapsayıcı Kayıt Defteri'ni (ACR) çoklu kapsayıcıyla kullanmak üzere nasıl yapılandırıyorum?**

ACR'yi çoklu kapsayıcılı kullanabilmek **için, tüm kapsayıcı görüntülerinin** aynı ACR kayıt defteri sunucusunda barındırılması gerekir. Aynı kayıt defteri sunucusuna geldiklerinde, uygulama ayarları oluşturmanız ve ardından Docker Comcreate yapılandırma dosyasını ACR görüntü adını içerecek şekilde güncelleştirmeniz gerekir.

Aşağıdaki uygulama ayarlarını oluşturun:

- DOCKER_REGISTRY_SERVER_USERNAME
- DOCKER_REGISTRY_SERVER_URL (tam URL, `https://<server-name>.azurecr.io`ör. )
- DOCKER_REGISTRY_SERVER_PASSWORD (ACR ayarlarında yönetici erişimini etkinleştirin)

Yapılandırma dosyasıiçinde, ACR resminize aşağıdaki örnek gibi başvurun:

```yaml
image: <server-name>.azurecr.io/<image-name>:<tag>
```

**Hangi konteynerin internete uygun olduğunu nasıl bilebilirim?**

- Yalnızca bir kapsayıcı erişim için açık olabilir
- Yalnızca 80 ve 8080 bağlantı noktası erişilebilir (açıkta kalan bağlantı noktaları)

Hangi kapsayıcının erişilebilir olduğunu belirleme kuralları şunlardır : öncelik sırasına göre:

- Kapsayıcı `WEBSITES_WEB_CONTAINER_NAME` adına ayarlanan uygulama ayarı
- Bağlantı noktası 80 veya 8080'i tanımlayan ilk kapsayıcı
- Yukarıdakilerin hiçbiri doğru değilse, dosyada tanımlanan ilk kapsayıcı erişilebilir (açıkta)


## <a name="web-sockets"></a>Web Soketleri

Web Soketleri Linux uygulamalarında desteklenir.

> [!IMPORTANT]
> Web Soketleri şu anda Ücretsiz Uygulama Hizmet Planları'ndaki Linux uygulamaları için desteklenmez. Bu sınırlamayı kaldırmak için çalışıyoruz ve Ücretsiz Uygulama Hizmeti planlarında en fazla 5 web soket bağlantısını desteklemeyi planlıyoruz.

## <a name="pricing-and-sla"></a>Fiyatlandırma ve SLA

**Hizmet genel olarak kullanılabilir hale geldiğiniz için fiyatlandırma nedir?**

Fiyatlandırma SKU ve bölgeye göre değişir ancak fiyatlandırma sayfamızda daha fazla ayrıntı görebilirsiniz: [App Service Fiyatlandırma.](https://azure.microsoft.com/pricing/details/app-service/linux/)

## <a name="other-questions"></a>Diğer sorular

**"İstenen özellik kaynak grubunda kullanılamıyor" ne anlama geliyor?**

Azure Kaynak Yöneticisi 'ni (ARM) kullanarak web uygulaması oluştururken bu iletiyi görebilirsiniz. Geçerli bir sınırlamaya bağlı olarak, aynı kaynak grubu için Windows ve Linux uygulamalarını aynı bölgede karıştıramazsınız.

**Uygulama ayarları adlarında desteklenen karakterler nelerdir?**

Uygulama ayarları için yalnızca harfleri (A-Z, a-z), sayılar (0-9) ve alt çizilme karakterini (_) kullanabilirsiniz.

**Yeni özellikleri nereden isteyebilirim?**

Fikrinizi Web Apps [geri bildirim forumunda](https://aka.ms/webapps-uservoice)gönderebilirsiniz. Fikrinizin başlığına "[Linux]" ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [Linux'ta Azure Uygulama Hizmeti nedir?](app-service-linux-intro.md)
- [Azure App Service’te hazırlık ortamları ayarlama](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Kapsayıcılar için Web Uygulaması ile Sürekli Dağıtım](./app-service-linux-ci-cd.md)
