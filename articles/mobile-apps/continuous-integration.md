---
title: Visual Studio App Center ve Azure hizmetleriyle uygulamalarınızın yaşam döngüsünü otomatikleştirin
description: Mobil uygulamalarınız için sürekli oluşturma ve tümleştirme oluşturmaya yardımcı olan App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 0560f47b832ec2965d9b567e1aeff78baa9c247c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240933"
---
# <a name="automate-the-lifecycle-of-your-apps-with-continuous-build-and-integration"></a>Sürekli oluşturma ve tümleştirme yle uygulamalarınızın yaşam döngüsünü otomatikleştirin

Geliştiriciler olarak, kod yazar ve kod deposunda denetlersiniz, ancak repo'da denetlenen taahhütler her zaman tutarlı olmayabilir. Birden çok geliştirici aynı proje üzerinde çalışırken, tümleştirme sorunları yla gelebilir. Takımlar işlerin yürümediği, hataların biriktiği ve proje geliştirmenin geciktiği durumlara çarpabilir. Geliştiriciler, hataları denetlemek için tüm yazılım kodunun oluşturulmasını ve test edilemesini beklemek zorunda, bu da işlemi yavaş ve daha az yinelemeli hale getirir. 

Sürekli yapı ve tümleştirme ile geliştiriciler, kaynak kodu deposundaki değişikliklerini taahhüt ederek ve test ve doğrulamaları yapı ortamına koyarak yapıyı basitleştirebilir ve kodlarını test edebilir. Bu şekilde, her zaman kendi kodlarına karşı testler çalıştırıyorlar. Kaynak kodunda yapılan tüm değişiklikler, depoya bir taahhüt yapıldığında sürekli olarak oluşturulur. Her iadeile, sürekli tümleştirme (CI) sunucusu geliştiricinin oluşturduğu tüm sınamaları doğrular ve yürütür. Testler geçmezse, kod daha fazla değişiklik için geri gönderilir. Bu şekilde, geliştiriciler oluşturulan yapılar kırmak yok. Ayrıca tüm testleri bilgisayarlarında yerel olarak çalıştırmak zorunda değildir, bu da geliştirici üretkenliğini artırır. 

## <a name="key-benefits"></a>Önemli avantajlar
- Yapılarınızı, testlerinizi ve dağıtımlarınızı ardışık hatlar için otomatikleştirin.
- Daha hızlı sürüm hızları sağlamak için hataları algıla ve sorunları erken giderin.
- Kodu daha sık işleyip uygulamaları hızlı bir şekilde oluşturun.
- Herhangi bir sorun olmadan kodu hızlı bir şekilde değiştirme esnekliğielde edin.
- Sadece iyi kalite kodu tüm yol yapar, böylece daha hızlı zaman pazara kazanın.
- Küçük kod parçaları aynı anda tümleştirilebildiği için küçük kod değişikliklerini daha verimli hale getirin.
- Müşterilerinizden ve ekibinizden sürekli geri bildirim almak için ekip şeffaflığını ve hesap verebilirliğini artırın.

Mobil uygulamalarınızda sürekli bir tümleştirme ardışık hattını etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Build,](/appcenter/build/) güvenli bir bulut altyapısı kullanarak ekibinizin üzerinde çalıştığı yerel ve çapraz platform uygulamaları oluşturmanıza yardımcı olur. Reponuzu Visual Studio App Center'a kolayca bağlayabilir ve uygulamanızı her işlemede bulutta oluşturmaya başlayabilirsiniz. Yapı sunucularını yerel olarak yapılandırma konusunda endişelenmenize gerek yok, karmaşık yapılandırmalar ve iş arkadaşınızın makinesinde yapılandırılan ancak sizinkini değil kodu.

Visual Studio App Center hizmetlerinin ek gücüyle iş akışınızı daha da otomatikleştirebilirsiniz. App Center Distribute ile yapıyı test edenlere ve genel uygulama mağazalarına otomatik olarak serbest bırakabilirsiniz. Ayrıca, App Center Testi ile bulutta binlerce gerçek aygıt ve işletim sistemi yapılandırmasında otomatik ui testleri çalıştırabilirsiniz.

**Önemli özellikler**
- Dakikalar içinde sürekli tümleştirme ayarlayın ve uygulamaları daha sık ve daha hızlı oluşturun.
- GitHub, BitBucket, Azure DevOps ve GitLab ile tümleştirin.
- Yönetilen, bulut barındırılan makinelerde hızlı ve güvenli yapılar oluşturun.
- Test başlatmak için yapılarınızın etkinleştirin ve uygulamanın gerçek dünyadaki iOS ve Android cihazlarda oluşturup oluşturmadığını doğrulayın.
- iOS, Android, macOS, Windows, Xamarin ve React Native için yerel ve çapraz platform desteği kazanın.
- Klon, önceden yapı ve yapı sonrası komut dosyaları ekleyerek yapılarınızı özelleştirin.

**Başvurular**
- [Visual Studio App Center ile kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center Build ile başlayın](/appcenter/build/)

## <a name="azure-pipelines"></a>Azure Pipelines
 Azure DevOps'te bir hizmet olan [Azure Pipelines,](https://azure.microsoft.com/services/devops/pipelines/)tercih ettiğiniz Git sağlayıcısıyla çalışan tam özellikli sürekli tümleştirme ve sürekli teslimat (CD) hizmetidir. Azure'u içeren en büyük bulut hizmetlerine dağıtılabilir. Kodunuzu GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud veya Azure Repos'ta başlatabilirsiniz. Ardından kodunuzu Microsoft Azure, Google Bulut Platformu veya Amazon Web Hizmetleri'ne (AWS) otomatikleştirebilirsiniz.

**Önemli özellikler**
- **BIR CI sunucusu kurmak için basitleştirilmiş görev tabanlı deneyim:** Microsoft ve Microsoft (Node.js, Java) tabanlı sunucu teknolojilerine ek olarak hem yerel (Android, iOS ve Windows) hem de çapraz platform (Xamarin, Cordova ve React Native) mobil uygulamalar için bir CI sunucusu ayarlayın.
- **Herhangi bir dil, platform ve bulut:** Düğüm.js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android ve iOS uygulamaları oluşturun, test edin ve dağıtın. Linux, macOS ve Windows'da paralel olarak çalıştırın. Azure, AWS ve Google Cloud Platformu gibi bulut sağlayıcılarına dağıtın. Mobil uygulamaları beta kanalları ve uygulama mağazaları aracılığıyla dağıtın.
- **Yerel konteyner desteği:** Kolayca yeni kapsayıcılar oluşturun ve bunları herhangi bir kayıt defterine itin. Kapsayıcıları bağımsız ana bilgisayarlara veya Kubernetes'e dağıtın.
- **Gelişmiş iş akışları:** Kolayca yapı zincirleri ve çok aşamalı yapılar oluşturun. YAML, test entegrasyonu, sürüm kapıları, raporlama ve daha fazlası için destek alın.
- **Genişletilebilir:** Slack'ten SonarCloud'a kadar yüzlerce uzantı içeren topluluk tarafından oluşturulmuş bir dizi oluşturma, test ve dağıtım görevi kullanın. Jenkins gibi diğer ci sistemlerinden bile konuşlandırabilirsiniz. Web kancaları ve REST API'leri tümleştirmenize yardımcı olabilir.
- **Ücretsiz bulut barındırılan yapılar:** Bu yapılar genel ve özel depolar için kullanılabilir.
- **Diğer bulut satıcılarına dağıtım desteği:** Satıcılar aws ve Google Cloud Platform içerir.

**Başvurular**
- [Azure Pipelines kılavuzunu kullanarak başlayın](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Azure DevOps’u kullanmaya başlama](https://app.vsaex.visualstudio.com/signup/) 
- [Hızlı başlangıçlar](/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)

Uygulama yapılarınız için doğru hizmeti seçmenize yardımcı olmak için, [App Center Build ile Azure Ardışık Hatları'nı](/appcenter/build/choose-between-services)karşılaştıran makaleye bakın.
