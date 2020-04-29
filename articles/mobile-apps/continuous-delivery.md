---
title: Visual Studio App Center ve Azure hizmetleriyle mobil uygulamalarınızın dağıtımını ve sürümünü otomatikleştirin
description: Mobil uygulamalarınız için sürekli teslim işlem hattı ayarlamaya yardımcı olan App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: ccfd0fc0450a6fbd34192dce7e375fe2de7f47c1
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80235348"
---
# <a name="automate-the-deployment-and-release-of-your-mobile-applications-with-continuous-delivery-services"></a>Sürekli teslim hizmetleriyle mobil uygulamalarınızın dağıtımını ve sürümünü otomatikleştirin

Geliştiriciler olarak kod yazar ve kod deposuna iade edersiniz, ancak depoya iade edilen işlemeler her zaman tutarlı olmayabilir. Birden çok geliştirici aynı proje üzerinde çalışırken, sorunlar tümleştirme ile birlikte gelebilir. Ekipler, işlerin işe madığı, hata oluşturan ve proje geliştirmenin gecikdüğü durumlarda çalışabilir. Geliştiricilerin, hataları denetlemek için tüm yazılım kodunun oluşturulup Test edilene kadar beklemesi gerekir. Bu işlem, işlemi yavaşlatır ve daha az yinelemeli hale getirir.

Sürekli teslim sayesinde, mobil uygulamalarınızın dağıtımını ve sürümünü otomatik hale getirebilirsiniz. Uygulamayı bir test ediciler veya şirket çalışanları (Beta testi için) veya bir App Store (üretim için) grubuna dağıtmaktan bağımsız değildir. Sürekli teslim, dağıtımları daha az riskli hale getiriyor ve hızlı yinelemeleri öneriyor. Ayrıca, müşterileriniz için yeni değişiklikleri sürekli olarak bir şekilde serbest bırakabilirsiniz.

## <a name="distribute-application-binaries-to-beta-testers"></a>Uygulama ikili dosyalarını Beta sınayıcılarına dağıtma
Mobil uygulamanız, uygulama geliştirme sürecinde kritik adımlardan biridir. Uygulamanızdaki hataları ve sorunları önceden üzerinde bulmaya yardımcı olur. Geribildirim, üretim kullanımı için hazırlamak istediğinizde uygulamanızın kalitesini geliştirir.

Mobil uygulamalarınızda sürekli teslim işlem hattını etkinleştirmek için aşağıdaki hizmetleri kullanın.

### <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center dağıtımı](/appcenter/distribution/) , geliştiricilerin yapıları cihazlara hızlıca yayınlamasına yönelik bir araçtır. Portal 'ın tamamını bir yüklemeden App Center dağıtım, Beta uygulaması sınayıcı dağıtımı için güçlü bir çözümdür. Ayrıca, genel uygulama mağazalarından dağıtım yapmak için kullanışlı bir alternatiftir. Geliştiriciler, App Center derleme ve genel uygulama mağazası tümleştirmeleri sayesinde dağıtım iş akışını otomatik hale getirebilir.

**Önemli özellikler**
- Uygulamanızı Beta sınayıcılarına ve kullanıcılara dağıtın ve tüm test edenlerin uygulamanızın en son sürümünde bulunduğundan emin olun.
- Yeni yayınların sınayıcılarını karşıdan yükleme akışından tekrar ilerlemeden bildir.
- Uygulamanızın farklı sürümleri için dağıtım gruplarını yönetin.
- Mağazalara dağıt: 
    - [Apple](/appcenter/distribution/stores/apple)
    - [Google Play](/appcenter/distribution/stores/googleplay)
    - [Intune](/appcenter/distribution/stores/intune)
- İOS, Android, macOS, tvOS, Xamarin, bir yerel, Unity ve Cordova için platform desteği kazanın.
- İOS cihazlarını sağlama profilinize otomatik olarak kaydedin.

**Başvurular**
- [Visual Studio App Center kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center dağıtma ile çalışmaya başlama](/appcenter/build/)

### <a name="azure-pipelines"></a>Azure Pipelines

[Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/) , tercih ettiğiniz git sağlayıcınızla birlikte çalışarak tam özellikli bir sürekli TÜMLEŞTIRME (CI) ve sürekli teslım (CD) hizmetidir. Azure Pipelines, Azure hizmetleri gibi birçok ana bulut hizmetine dağıtılabilir. GitHub, GitHub Enterprise Server, GitLab, Bitbucket bulutu veya Azure Repos için kodunuzla başlayabilirsiniz. Daha sonra Microsoft Azure, Google Cloud Platform veya Amazon Web Services (AWS) için kodunuzun oluşturma, test etme ve dağıtımını otomatik hale getirebilirsiniz.

**Önemli özellikler**
- **CI sunucusunu ayarlamaya yönelik Basitleştirilmiş görev tabanlı deneyim:** Yerel (Android, iOS ve Windows) ve platformlar arası (Xamarin, Cordova ve yanıt verme) mobil uygulamaları için bir CI sunucusu ayarlayın.
- **Herhangi bir dil, platform ve bulut:** Node. js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android ve iOS uygulamaları oluşturun, test edin ve dağıtın. Linux, macOS ve Windows üzerinde paralel olarak çalıştırın. Azure, AWS ve Google Cloud Platform gibi bulut sağlayıcılarına dağıtın. Mobil uygulamaları Beta kanalları ve uygulama depoları aracılığıyla dağıtın.
- **Yerel kapsayıcı desteği:** Kolayca yeni kapsayıcılar oluşturun ve bunları herhangi bir kayıt defterine gönderin. Kapsayıcıları bağımsız konaklara veya Kubernetes 'e dağıtın.
- **Gelişmiş iş akışları ve Özellikler:** Kolayca derleme zincirlerini ve çoğullanan derlemeleri oluşturun. YAML, test tümleştirmesi, yayın kapıları, raporlama ve daha fazlası için destek alın.
- **Genişletilebilir:** Bolluk 'ten SonarCloud 'a yüzlerce uzantı içeren, topluluk tarafından oluşturulan derleme, test ve dağıtım görevleri aralığını kullanın. Jenkins gibi diğer CI sistemlerinden de dağıtım yapabilirsiniz. Web kancaları ve REST API 'Leri tümleştirmenize yardımcı olabilir.
- **Bulutta barındırılan ücretsiz derlemeler:** Bu derlemeler ortak ve özel depolar için kullanılabilir.
- **Diğer bulut satıcılarına dağıtım desteği:** Satıcıların AWS ve Google Cloud Platform vardır.

**Başvurular**
- [Azure Pipelines Kılavuzu ile çalışmaya başlama](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Azure DevOps’u kullanmaya başlama](https://app.vsaex.visualstudio.com/signup/)
  
## <a name="distribute-your-application-directly-to-app-stores"></a>Uygulamanızı doğrudan uygulama mağazalarına dağıtın
Uygulamanız üretim kullanımı için hazırsanız ve bu uygulamanın herkese açık bir şekilde kullanılmasını istiyorsanız, BT 'nin müşteriler tarafından indirilebileceği uygulama depolarına gönderilmesi gerekir. Uygulamanızı doğrudan uygulama depolarına dağıtmanın birden çok yolu vardır. 

### <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center dağıtımı](/appcenter/distribution/stores/)sayesinde, mobil uygulamalarınızı doğrudan uygulama mağazalarına yayımlayabilirsiniz. Uygulamanız kullanıcılar tarafından indirilmeye hazırsa, uygulama ikili dosyalarınızı doğrudan Visual Studio App Center portalından yayımlayabilirsiniz. 

Doğrudan dağıtım yapabilirsiniz:
- [Apple App Store](/appcenter/distribution/stores/apple)
- [Google Play Store](/appcenter/distribution/stores/googleplay)
- [Microsoft Intune](/appcenter/distribution/stores/intune)
    
### <a name="apple-app-store"></a>Apple App Store
Apple tarafından geliştirilen ve korunan uygulama mağazasındaki kullanıcılar iOS, MacOS, WatchOS ve tvOS cihazlarında geliştirilen uygulamalara gözatıp indirebilir. Geliştiricilerin iOS uygulamalarını genel kullanım için Apple App Store 'a göndermesi gerekir.

### <a name="google-play"></a>Intune Şirket Portalı

Google Play, Android işletim sistemi için resmi uygulama deposudur ve kullanıcıların Google aracılığıyla Yayınlanan Android cihazlarda geliştirilen uygulamalara gözatabilecekleri ve indirebileceği.

### <a name="intune"></a>Intune

[Microsoft Intune](/intune/app-management) , kurumsal hareketlilik yönetimi alanındaki bulut tabanlı bir hizmettir ve şirket verilerinizin korunmasını sağlarken iş gücünüzün üretken olmasını sağlar. Intune ile şunları yapabilirsiniz:
- Şirket verilerine erişmek için iş gücünüzün kullandığı mobil cihazları ve bilgisayarları yönetin.
- İş gücünüzün kullandığı mobil uygulamaları yönetin.
- İş gücünüzün eriştiği ve paylaştığı yöntemi denetleyerek şirket bilgilerinizi koruyun.
- Cihazların ve uygulamaların şirket güvenlik gereksinimleriyle uyumlu olduğundan emin olun.
    
## <a name="deploy-updates-directly-to-users-devices"></a>Güncelleştirmeleri doğrudan kullanıcıların cihazlarına dağıtma

### <a name="codepush"></a>CodePush
App Center [Apache Cordova Ile yerel](/appcenter/distribution/codepush/) geliştiriciler, mobil uygulama güncelleştirmelerini doğrudan kullanıcılarınızın cihazlarına dağıtabilir. Geliştiricilerin JavaScript, HTML, CSS ve resim değişiklikleri gibi bazı güncelleştirmeleri yayımlayabildiği merkezi bir depo gibi davranır. Ardından, uygulamalar, belirtilen istemci SDK 'larını kullanarak depodan güncelleştirme sorgulayabilir. Bu şekilde, hataları adreslarken veya küçük özellikler eklerken kullanıcılarınıza daha belirleyici ve doğrudan katılım modeli sağlayabilirsiniz. Bir ikiliyi yeniden oluşturmanız veya herhangi bir genel uygulama mağazalarından yeniden dağıtmanız gerekmez.

**Önemli özellikler**
- Cordova ve tepki verme yerel geliştiricileri, mobil uygulama güncelleştirmelerini bir mağazaya serbest bırakmadan kullanıcıların cihazlarına doğrudan dağıtabilir.
- İkiliyi yeniden oluşturmanız ve ilgili depolarla yeniden dağıtmanız gerekmeyen küçük özellikleri eklemek veya kaldırmak için faydalıdır.

**Başvurular**
- [Visual Studio App Center kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center 'de CodePush kullanmaya başlama](/appcenter/distribution/codepush/)
- [CodePush CLı](/appcenter/distribution/codepush/cli)