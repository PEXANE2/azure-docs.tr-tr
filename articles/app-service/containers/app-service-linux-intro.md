---
title: Varsayılan Linux kapsayıcılarında kod çalıştırma
description: Azure Uygulama Hizmeti, kodunuzu önceden oluşturulmuş Linux kapsayıcılarında çalıştırabilir. Linux web uygulamalarınızı Azure'da nasıl çalıştırabileceğinizi öğrenin.
keywords: azure app service, linux, oss
author: msangapu-msft
ms.assetid: bc85eff6-bbdf-410a-93dc-0f1222796676
ms.topic: overview
ms.date: 1/11/2019
ms.author: msangapu
ms.custom: mvc, seodec18
ms.openlocfilehash: 27f085543869b1a77db9c97ca2e7ae7d3d3b7b88
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80046408"
---
# <a name="introduction-to-azure-app-service-on-linux"></a>Linux’ta Azure App Service’e Giriş

[Azure Uygulama Hizmeti,](../overview.md) web sitelerini ve web uygulamalarını barındırmak için optimize edilmiş tam olarak yönetilen bir bilgi işlem platformudur. Müşteriler Linux’ta App Service’i kullanarak desteklenen uygulama yığınları için Linux’ta yerel olarak web uygulamaları barındırabilir.

## <a name="languages"></a>Diller

Linux’ta App Service, geliştirici üretkenliğini artırmaya yönelik çeşitli Yerleşik görüntüleri destekler. Diller şunlardır: Node.js, Java (JRE 8 & JRE 11), PHP, Python, .NET Core ve Ruby. En [`az webapp list-runtimes --linux`](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-list-runtimes) son dilleri ve desteklenen sürümleri görüntülemek için çalıştırın. Yerleşik görüntülerde uygulamanızın gerektirdiği çalışma zamanı desteklenmiyorsa, [kendi Docker görüntünüzü](tutorial-custom-docker-image.md) oluşturarak Kapsayıcılar için Web App’e dağıtmaya yönelik yönergeler sunulmaktadır.

## <a name="deployments"></a>Dağıtımlar

* FTP
* Yerel Git
* GitHub
* Bitbucket

## <a name="devops"></a>DevOps

* Hazırlık ortamları
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro) ve DockerHub CI/CD

## <a name="console-publishing-and-debugging"></a>Konsol, Yayımlama ve Hata Ayıklama

* Ortamlar
* Dağıtımlar
* Temel konsol
* SSH

## <a name="scaling"></a>Ölçeklendirme

* Müşteriler [App Service planı](https://docs.microsoft.com/azure/app-service/overview-hosting-plans?toc=%2fazure%2fapp-service-web%2ftoc.json) katmanlarını değiştirerek web uygulamalarının ölçeğini büyütüp küçültebilirler

## <a name="locations"></a>Konumlar

[Azure Durum Panosu](https://azure.microsoft.com/status)’nu inceleyin.

## <a name="limitations"></a>Sınırlamalar

Azure portalı, yalnızca şu anda Kapsayıcılar için Web App ile kullanılabilen özellikleri gösterir. Yeni özellikler etkinleştirildikçe portalda görünmeye başlayacaktır.

Linux'taki Uygulama Hizmeti yalnızca [Ücretsiz, Temel, Standart ve Premium](https://azure.microsoft.com/pricing/details/app-service/plans/) uygulama hizmet planlarıyla desteklenir ve [Paylaşılan](https://azure.microsoft.com/pricing/details/app-service/plans/) bir katmanı yoktur. Linux olmayan Web Uygulamalarını barındıran bir Uygulama Hizmeti planında bir Linux Web Uygulaması oluşturamazsınız.  

Geçerli bir sınırlamaya bağlı olarak, aynı kaynak grubu için Windows ve Linux uygulamalarını aynı bölgede karıştıramazsınız.

## <a name="troubleshooting"></a>Sorun giderme

> [!NOTE]
> [Azure İzleme (önizleme)](https://docs.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs#send-logs-to-azure-monitor-preview) ile yeni entegre günlük özelliği vardır. 
>
>

Uygulamanız başlatılamazsa veya uygulamanızdan alınan günlük kayıtlarına bakmak isterseniz LogFiles dizinindeki Docker günlüklerini inceleyin. Bu dizine SCM siteniz üzerinden veya FTP aracılığıyla erişebilirsiniz. Ve konteyner `stdout` `stderr` gelen oturum açmak için, Uygulama **Hizmet Günlükleri**altında **Uygulama Günlüğü** etkinleştirmek gerekir. Ayar hemen etkili olur. Uygulama Hizmeti değişikliği algılar ve kapsayıcıyı otomatik olarak yeniden başlatır.

SCM sitesine **Geliştirme Araçları** menüsündeki **Gelişmiş Araçlar**’dan erişebilirsiniz.

![Kudu kullanarak Docker günlüklerini görüntüleme][1]

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleler farklı dillerde yazılmış web uygulamalarıyla Linux'ta App Service hizmetini kullanmaya başlamanızı sağlar:

* [.NET Core](quickstart-dotnetcore.md)
* [PHP](https://docs.microsoft.com/azure/app-service/containers/quickstart-php)
* [Node.js](quickstart-nodejs.md)
* [Java](quickstart-java.md)
* [Python](quickstart-python.md)
* [Ruby](quickstart-ruby.md)
* [Git](quickstart-docker-go.md)
* [Birden çok kapsayıcılı uygulamalar](quickstart-multi-container.md)

Linux'ta Uygulama Hizmeti hakkında daha fazla bilgi için bkz:

* [Linux’ta App Service hakkında SSS](app-service-linux-faq.md)
* [Linux'ta App Service için SSH desteği](app-service-linux-ssh-support.md)
* [App Service’te hazırlık ortamları ayarlama](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
* [Docker Hub sürekli dağıtımı](app-service-linux-ci-cd.md)

Sorularınızı ve çekincelerinizi [forumumuzda](https://docs.microsoft.com/answers/topics/azure-webapps.html) paylaşabilirsiniz.

<!--Image references-->
[1]: ./media/app-service-linux-intro/kudu-docker-logs.png
[2]: ./media/app-service-linux-intro/logging.png
