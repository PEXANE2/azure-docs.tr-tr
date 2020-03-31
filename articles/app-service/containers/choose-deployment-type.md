---
title: Linux kapsayıcıları için dağıtım seçenekleri
description: Özel Docker konteyner dağıtımı, çoklu kapsayıcı ve Linux'ta Uygulama Hizmeti için yerleşik uygulama çerçevesi arasında karar verin.
keywords: azure uygulama hizmeti, web uygulaması, linux, oss
author: msangapu-msft
ms.topic: article
ms.date: 05/04/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 7c4025e393c196fb98455bac26e94e5f252840b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74687506"
---
# <a name="custom-image-multi-container-or-built-in-platform-image"></a>Özel görüntü, çoklu kapsayıcı veya yerleşik platform görüntüsü?

[Linux'taki Uygulama Hizmeti,](app-service-linux-intro.md) uygulamanızın web'de yayınlanması için üç farklı yol sunar:

- **Özel görüntü dağıtımı**: Uygulamanızı, tüm dosyalarınızı ve bağımlılıklarınızı kullanıma hazır bir pakette içeren bir Docker görüntüsüne "Dockerize" haline getirin.
- **Çok kapsayıcılı dağıtım**: Docker Compose yapılandırma dosyalarını kullanarak uygulamanızı birden çok kapsayıcıda "Dockerize" edin.
- **Yerleşik platform görüntüsüne sahip uygulama dağıtımı**: Yerleşik platform görüntülerimiz, Düğüm ve PHP gibi yaygın web uygulaması çalışma saatleri ve bağımlılıklar içerir. Uygulamanızı web uygulamanızın depolama alanına dağıtmak için [Azure Uygulama Hizmeti dağıtım yöntemlerinden](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) herhangi birini kullanın ve ardından çalıştırmak için yerleşik bir platform görüntüsü kullanın.

## <a name="which-method-is-right-for-your-app"></a>Uygulamanız için hangi yöntem doğru? 

Göz önünde bulundurulması gereken başlıca faktörler şunlardır:

- **Docker'ın geliştirme iş akışınızda kullanılabilirliği**: Özel görüntü geliştirme, Docker geliştirme iş akışı hakkında temel bilgiler gerektirir. Özel bir resmin bir web uygulamasına dağıtılması, özel resminizin Docker Hub gibi bir depo ana bilgisayara yayınlanmasını gerektirir. Docker'ı tanıyorsanız ve yapı iş akışınıza Docker görevleri ekleyebilirseniz veya uygulamanızı zaten Docker görüntüsü olarak yayınlıyorsanız, özel bir görüntü kesinlikle en iyi seçimdir.
- **Çok katmanlı mimari**: Çok kapsayıcı kullanarak yetenekleri ayırmak için web uygulama katmanı ve API katmanı gibi birden çok kapsayıcı yı dağıtın. 
- **Uygulama performansı**: Redis gibi bir önbellek katmanı kullanarak çok kapsayıcı uygulamanızın performansını artırın. Bunu başarmak için çoklu kapsayıcı seçin.
- **Benzersiz çalışma süresi gereksinimleri**: Yerleşik platform görüntüleri çoğu web uygulamasının gereksinimlerini karşılamak üzere tasarlanmıştır, ancak özelleştirilebilirlikleri sınırlıdır. Uygulamanız, yerleşik görüntülerin yapabileceklerini aşan benzersiz bağımlılıklara veya diğer çalışma süresi gereksinimlerine sahip olabilir.
- **Yapı gereksinimleri**: [Sürekli dağıtım](../deploy-continuous-deployment.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)ile uygulamanızı doğrudan kaynak kodundan Azure'da çalıştırabilirsiniz. Harici yapı veya yayın işlemi gerekmez. Ancak, [Kudu](https://github.com/projectkudu/kudu/wiki) dağıtım motoru içinde yapı araçlarının özelleştirilebilirliği ve kullanılabilirliğinin bir sınırı vardır. Uygulamanız, bağımlılıkları veya özel yapı mantığı gereksinimleri nde büyüdükçe Kudu'nun yeteneklerini aşabilir.
- **Disk okuma/yazma gereksinimleri**: Tüm web uygulamaları web içeriği için bir depolama birimi olarak ayrılır. Azure Depolama tarafından desteklenen bu birim, `/home` uygulamanın dosya sistemine monte edilir. Kapsayıcı dosya sistemindeki dosyaların aksine, içerik hacmindeki dosyalara bir uygulamanın tüm ölçek örneklerinde erişilebilir ve uygulamanın yeniden başlatılmasında değişiklikler devam eder. Ancak, içerik biriminin disk gecikmesi yerel kapsayıcı dosya sisteminin gecikmesinden daha yüksek ve daha değişkendir ve erişim platform yükseltmeleri, planlanmamış kapalı kalma süresi ve ağ bağlantısı sorunlarından etkilenebilir. İçerik dosyalarına salt okunur şekilde okunması gereken uygulamalar, dosyaları içerik hacmi yerine görüntü dosya sistemine yerleştiren özel görüntü dağıtımından yararlanabilir.
- **Yapı kaynak kullanımı**: Bir uygulama kaynaktan dağıtıldığında, Kudu tarafından işletilen dağıtım komut dosyaları, çalışan uygulamayla aynı Uygulama Hizmet Planı bilgi işlem ve depolama kaynaklarını kullanır. Büyük uygulama dağıtımları, istenenden daha fazla kaynak veya zaman tüketebilir. Özellikle, birçok dağıtım iş akışı, uygulama içeriği hacminde bu tür etkinlikler için optimize edilmedi, ağır disk etkinliği oluşturur. Özel bir resim, ek dosya aktarımlarına veya dağıtım işlemlerine gerek kalmadan uygulamanızın tüm dosyalarını ve Azure'a olan bağımlılıklarını tek bir pakette sunar.
- **Hızlı yineleme ye ihtiyaç:** Bir uygulamayı dockerizing ek yapı adımları gerektirir. Değişikliklerin etkili olması için, yeni resminizi her güncelleştirmeyle birlikte bir depoya itmeniz gerekir. Bu güncelleştirmeler daha sonra Azure ortamına çekilir. Yerleşik kapsayıcılardan biri uygulamanızın gereksinimlerini karşılıyorsa, kaynaktan dağıtmak daha hızlı bir geliştirme iş akışı sunabilir.

## <a name="next-steps"></a>Sonraki adımlar

Özel konteyner:
* [Özel kapsayıcı çalıştırma](quickstart-docker-go.md)

Çoklu konteyner:
* [Çok kapsayıcılı uygulama oluşturma](quickstart-multi-container.md)

Aşağıdaki makaleler, Yerleşik bir platform görüntüsüyle Linux'taki App Service ile başlamanızı sağlar:

* [.NET Core](quickstart-dotnetcore.md)
* [PHP](quickstart-php.md)
* [Node.js](quickstart-nodejs.md)
* [Java](quickstart-java.md)
* [Python](quickstart-python.md)
* [Ruby](quickstart-ruby.md)