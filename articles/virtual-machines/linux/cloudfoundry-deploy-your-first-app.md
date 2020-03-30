---
title: İlk uygulamanızı Microsoft Azure'daki Bulut Dökümhanesi'ne dağıtın
description: Azure'da Bulut Dökümhanesi'ne uygulama dağıtma
author: seanmck
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.date: 06/14/2017
ms.author: seanmck
ms.openlocfilehash: 793a8f291be4fcca6fad19d486849253dddc089f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294799"
---
# <a name="deploy-your-first-app-to-cloud-foundry-on-microsoft-azure"></a>İlk uygulamanızı Microsoft Azure'daki Bulut Dökümhanesi'ne dağıtın

[Cloud Foundry,](https://cloudfoundry.org) Microsoft Azure'da kullanılabilen popüler bir açık kaynak uygulama platformudur. Bu makalede, Bulut Dökümhanesi'ndeki bir uygulamayı Azure ortamında nasıl dağıtabileceğimizi ve yönetebileceğimizi gösteriyoruz.

## <a name="create-a-cloud-foundry-environment"></a>Bulut Döküm ortamı oluşturma

Azure'da Bulut Döküm ortamı oluşturmak için birkaç seçenek vardır:

- PCF Ops Manager ve Azure Servis Aracısını içeren standart bir ortam oluşturmak için Azure Marketi'ndeki [Pivotal Cloud Foundry teklifini][pcf-azuremarketplace] kullanın. Pazar yeri teklifini Pivotal belgelerinde dağıtmak için [tam talimatları][pcf-azuremarketplace-pivotaldocs] bulabilirsiniz.
- [Pivotal Cloud Foundry'yi el ile dağıtarak][pcf-custom]özelleştirilmiş bir ortam oluşturun.
- Açık kaynak cloud [foundry paketlerini doğrudan][oss-cf-bosh] bir [BOSH](https://bosh.io) yöneticisi, Bulut Dökümhanesi ortamının dağıtımını koordine eden bir VM ayarlayarak dağıtın.

> [!IMPORTANT] 
> Azure Marketi'nden PCF dağıtıyorsanız, her ikisi de pazar yeri dağıtım kılavuzunda açıklanan Pivotal Apps Yöneticisi'ne erişmek için gerekli SYSTEMDOMAINURL ve yönetici kimlik bilgilerini not edin. Bu öğretici tamamlamak için gereklidir. Pazar yeri dağıtımları için SYSTEMDOMAINURL `https://system.*ip-address*.cf.pcfazure.com`şeklindedir.

## <a name="connect-to-the-cloud-controller"></a>Bulut Denetleyicisine bağlanın

Bulut Denetleyicisi, uygulamaları dağıtmak ve yönetmek için Bulut Dökümhanesi ortamının birincil giriş noktasıdır. Çekirdek Bulut Denetleyicisi API (CCAPI) bir REST API, ancak çeşitli araçlar aracılığıyla erişilebilir. Bu durumda, [bulut döküm cli][cf-cli]üzerinden onunla etkileşim . CLI'yi Linux, macOS veya Windows'a yükleyebilirsiniz, ancak hiç yüklememeyi tercih ederseniz, Azure Bulut [Kabuğu'nda][cloudshell-docs]önceden yüklenmiş olarak kullanılabilir.

Oturum açmak `api` için, pazar yeri dağıtımından elde ettiğiniz SYSTEMDOMAINURL'ye hazırlayın. Varsayılan dağıtım kendi imzalı bir sertifika kullandığından, `skip-ssl-validation` anahtarı da eklemeniz gerekir.

```bash
cf login -a https://api.SYSTEMDOMAINURL --skip-ssl-validation
```

Bulut Denetleyicisi'nde oturum açmanız istenir. Pazar yeri dağıtım adımlarından elde ettiğiniz yönetici hesap kimlik bilgilerini kullanın.

Cloud Foundry, paylaşılan bir dağıtımdaki ekipleri ve ortamları yalıtmak için ad alanı olarak *orglar* ve *boşluklar* sağlar. PCF pazar yeri dağıtımı varsayılan *sistem* org ve otomatik ölçekleme hizmeti ve Azure servis aracısı gibi temel bileşenleri içerecek şekilde oluşturulan bir dizi boşluk içerir. Şimdilik, *sistem* alanını seçin.


## <a name="create-an-org-and-space"></a>Bir org ve alan oluşturma

Yazarsanız, `cf apps`sistem org içinde sistem alanında dağıtılan bir dizi sistem uygulamaları görürsünüz. 

*Sistem* org sistem uygulamaları için ayrılmış tutmak gerekir, bu yüzden örnek uygulama ev için bir org ve alan oluşturun.

```bash
cf create-org myorg
cf create-space dev -o myorg
```

Yeni org ve alana geçmek için hedef komutunu kullanın:

```bash
cf target -o testorg -s dev
```

Şimdi, bir uygulama dağıttığınızda, otomatik olarak yeni org ve uzayda oluşturulur. Yeni org/space'te şu anda uygulama olmadığını `cf apps` doğrulamak için yeniden yazın.

> [!NOTE] 
> Orglar ve alanlar ve bunların rol tabanlı erişim denetimi (RBAC) için nasıl kullanılabileceğini hakkında daha fazla bilgi için [Bulut Dökümhanesi belgelerine][cf-orgs-spaces-docs]bakın.

## <a name="deploy-an-application"></a>Uygulama dağıtma

Java'da yazılmış ve [Bahar Çerçevesi](https://spring.io) ve [Bahar Önyükleme'ye](https://projects.spring.io/spring-boot/)dayanan Hello Spring Cloud adlı örnek bir Bulut Dökümhanesi uygulamasını kullanalım.

### <a name="clone-the-hello-spring-cloud-repository"></a>Hello Spring Cloud deposunu klonla

Hello Spring Cloud örnek uygulaması GitHub'da mevcuttur. Ortamınıza kopyalayın ve yeni dizine dönüştürin:

```bash
git clone https://github.com/cloudfoundry-samples/hello-spring-cloud
cd hello-spring-cloud
```

### <a name="build-the-application"></a>Uygulama oluşturma

[Apache Maven](https://maven.apache.org)kullanarak uygulama oluşturun.

```bash
mvn clean package
```

### <a name="deploy-the-application-with-cf-push"></a>Cf push ile uygulamayı dağıtın

Çoğu uygulamayı Bulut Dökümhanesi'ne komutu `push` kullanarak dağıtabilirsiniz:

```bash
cf push
```

Bir uygulamayı *itdiğinizde,* Cloud Foundry uygulama türünü algılar (bu durumda, bir Java uygulaması) ve bağımlılıklarını tanımlar (bu durumda, Yay çerçevesi). Daha sonra, kodunuzu tek başına bir kapsayıcı görüntüsüne çalıştırmak için gereken her şeyi *bir damlacık*olarak bilinen paketler. Son olarak, Cloud Foundry uygulamayı ortamınızdaki kullanılabilir makinelerden birinde planlar ve komutun çıktısında bulunan bir URL oluşturur.

![Cf push komutundan çıkış][cf-push-output]

Hello-spring-cloud uygulamasını görmek için tarayıcınızda sağlanan URL'yi açın:

![Hello Spring Cloud için Varsayılan UI][hello-spring-cloud-basic]

> [!NOTE] 
> Sırasında `cf push`neler olduğu hakkında daha fazla bilgi edinmek için [bkz.][cf-push-docs]

## <a name="view-application-logs"></a>Uygulama günlüklerini görüntüleme

Bir uygulamanın günlüklerini adıyla görüntülemek için Bulut Döküm CLI'sini kullanabilirsiniz:

```bash
cf logs hello-spring-cloud
```

Varsayılan olarak, günlükleri komutu, yazıldığı gibi yeni günlükleri gösteren *kuyruk*kullanır. Yeni günlüklerin görüntülenmesini görmek için tarayıcıdaki hello-spring-cloud uygulamasını yenileyin.

Daha önce yazılmış günlükleri görüntülemek için `recent` anahtarı ekleyin:

```bash
cf logs --recent hello-spring-cloud
```

## <a name="scale-the-application"></a>Uygulamayı ölçeklendirin

Varsayılan olarak, `cf push` uygulamanızın yalnızca tek bir örneğini oluşturur. Yüksek kullanılabilirlik sağlamak ve daha yüksek iş elde etmek için ölçeklendirmeyi etkinleştirmek için genellikle uygulamalarınızın birden fazla örneğini çalıştırmak istersiniz. Zaten dağıtılan uygulamaları komutu `scale` kullanarak kolayca ölçeklendirebilirsiniz:

```bash
cf scale -i 2 hello-spring-cloud
```

Uygulamadaki `cf app` komutun çalıştırılsa, Cloud Foundry'nin uygulamanın başka bir örneğini oluşturduğunu gösterir. Uygulama başladıktan sonra, Cloud Foundry otomatik olarak yük dengeleme trafiğine başlar.


## <a name="next-steps"></a>Sonraki adımlar

- [Bulut Döküm belgeleri okuyun][cloudfoundry-docs]
- [Bulut Dökümhanesi için Azure DevOps Hizmetleri eklentisini ayarlama][vsts-plugin]
- [Bulut Dökümhanesi için Microsoft Log Analytics Nozzle'ı yapılandırın][loganalytics-nozzle]

<!-- LINKS -->

[pcf-azuremarketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/pivotal.pivotal-cloud-foundry
[pcf-custom]: https://docs.pivotal.io/pivotalcf/1-10/customizing/azure.html
[oss-cf-bosh]: https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/tree/master/docs
[pcf-azuremarketplace-pivotaldocs]: https://docs.pivotal.io/pivotalcf/customizing/pcf_azure.html
[cf-cli]: https://github.com/cloudfoundry/cli
[cloudshell-docs]: https://docs.microsoft.com/azure/cloud-shell/overview
[cf-orgs-spaces-docs]: https://docs.cloudfoundry.org/concepts/roles.html
[spring-boot]: https://projects.spring.io/spring-boot/
[spring-framework]: https://spring.io
[cf-push-docs]: https://docs.cloudfoundry.org/concepts/how-applications-are-staged.html
[cloudfoundry-docs]: https://docs.cloudfoundry.org
[vsts-plugin]: https://github.com/Microsoft/vsts-cloudfoundry
[loganalytics-nozzle]: https://github.com/Azure/oms-log-analytics-firehose-nozzle

<!-- IMAGES -->
[cf-push-output]: ./media/cloudfoundry-deploy-your-first-app/cf-push-output.png
[hello-spring-cloud-basic]: ./media/cloudfoundry-deploy-your-first-app/hello-spring-cloud-basic.png
