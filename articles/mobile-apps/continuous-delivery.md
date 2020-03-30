---
title: Visual Studio App Center ve Azure hizmetleriyle mobil uygulamalarınızın dağıtımını ve serbest bırakılmasını otomatikleştirin
description: Mobil uygulamalarınız için sürekli teslimat boru hattı nın ayarlanınyardımcı olan App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: ccfd0fc0450a6fbd34192dce7e375fe2de7f47c1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235348"
---
# <a name="automate-the-deployment-and-release-of-your-mobile-applications-with-continuous-delivery-services"></a>Sürekli teslimat hizmetleriyle mobil uygulamalarınızın dağıtımını ve serbest bırakılmasını otomatikleştirin

Geliştiriciler olarak, kod yazar ve kod deposunda denetlersiniz, ancak repo'da denetlenen taahhütler her zaman tutarlı olmayabilir. Birden çok geliştirici aynı proje üzerinde çalışırken, tümleştirme sorunları yla gelebilir. Takımlar işlerin yürümediği, hataların biriktiği ve proje geliştirmenin geciktiği durumlara çarpabilir. Geliştiriciler, hataları denetlemek için tüm yazılım kodunun oluşturulmasını ve test edilemesini beklemek zorunda, bu da işlemi yavaş ve daha az yinelemeli hale getirir.

Sürekli teslimatla, mobil uygulamalarınızın dağıtımını ve serbest bırakılmasını otomatikleştirebilirsiniz. Uygulamayı test eden bir gruba veya şirket çalışanlarına (beta testi için) veya bir uygulama mağazasına (üretim için) dağıtıp dağıtmadığınız önemli değildir. Sürekli teslim dağıtımları daha az riskli hale getirir ve hızlı yinelemeleri teşvik eder. Müşterilerinize sürekli olarak yeni değişiklikler de sunabilirsiniz.

## <a name="distribute-application-binaries-to-beta-testers"></a>Uygulama ikililerini beta sınayıcılarına dağıtın
Mobil uygulamanızı beta testi, uygulama geliştirme sürecinde ki kritik adımlardan biridir. Bu erken uygulamahataları ve sorunları bulmak için yardımcı olur. Geri bildirim, üretim kullanımına hazır hale aldığınızda uygulama kalitenizi artırır.

Mobil uygulamalarınızda sürekli bir teslimat ardışık hattını etkinleştirmek için aşağıdaki hizmetleri kullanın.

### <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Distribute,](/appcenter/distribution/) geliştiricilerin yapılara hızla yayınlarını cihazlara yayınlaştırılması için bir araçtır. Eksiksiz bir yükleme portalı deneyimi yle App Center Distribute, beta uygulama sınayıcı dağıtımı için güçlü bir çözümdür. Ayrıca genel uygulama mağazaları aracılığıyla dağıtım için uygun bir alternatif. Geliştiriciler, App Center Build ve genel uygulama mağazası tümleştirmeleri ile dağıtım iş akışlarını daha da otomatikleştirebilir.

**Önemli özellikler**
- Uygulamanızı beta sınayıcılara ve kullanıcılara dağıtın ve tüm test yapanlarınızın uygulamanızın en son sürümünde olduğundan emin olun.
- Test edenler indownload akışından tekrar geçmeden yeni sürümleri test edenleri bildirin.
- Uygulamanızın farklı sürümleri için dağıtım gruplarını yönetin.
- Mağazalara dağıtın: 
    - [Apple](/appcenter/distribution/stores/apple)
    - [Google Play](/appcenter/distribution/stores/googleplay)
    - [Intune](/appcenter/distribution/stores/intune)
- iOS, Android, macOS, tvOS, Xamarin, React Native, Unity ve Cordova için platform desteği kazanın.
- iOS aygıtlarını otomatik olarak sağlama profilinize kaydedin.

**Başvurular**
- [Visual Studio App Center ile kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center Distribute ile başlayın](/appcenter/build/)

### <a name="azure-pipelines"></a>Azure Pipelines

[Azure Pipelines,](https://azure.microsoft.com/services/devops/pipelines/) tercih ettiğiniz Git sağlayıcısıyla çalışan tam özellikli sürekli tümleştirme (CI) ve sürekli teslimat (CD) hizmetidir. Azure Ardışık Hatları, Azure hizmetleri gibi en büyük bulut hizmetlerine dağıtılabilir. Kodunuzu GitHub, GitHub Enterprise Server, GitLab, Bitbucket Cloud veya Azure Repos'ta başlatabilirsiniz. Ardından kodunuzu Microsoft Azure, Google Bulut Platformu veya Amazon Web Hizmetleri'ne (AWS) otomatikleştirebilirsiniz.

**Önemli özellikler**
- **BIR CI sunucusu kurmak için basitleştirilmiş görev tabanlı deneyim:** Hem yerel (Android, iOS ve Windows) hem de çapraz platform (Xamarin, Cordova ve React Native) mobil uygulamaları için bir CI sunucusu ayarlayın.
- **Herhangi bir dil, platform ve bulut:** Düğüm.js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android ve iOS uygulamaları oluşturun, test edin ve dağıtın. Linux, macOS ve Windows'da paralel olarak çalıştırın. Azure, AWS ve Google Cloud Platformu gibi bulut sağlayıcılarına dağıtın. Mobil uygulamaları beta kanalları ve uygulama mağazaları aracılığıyla dağıtın.
- **Yerel konteyner desteği:** Kolayca yeni kapsayıcılar oluşturun ve bunları herhangi bir kayıt defterine itin. Kapsayıcıları bağımsız ana bilgisayarlara veya Kubernetes'e dağıtın.
- **Gelişmiş iş akışları ve özellikler:** Kolayca yapı zincirleri ve çok aşamalı yapılar oluşturun. YAML, test entegrasyonu, sürüm kapıları, raporlama ve daha fazlası için destek alın.
- **Genişletilebilir:** Slack'ten SonarCloud'a kadar yüzlerce uzantı içeren topluluk tarafından oluşturulmuş bir dizi oluşturma, test ve dağıtım görevi kullanın. Jenkins gibi diğer ci sistemlerinden bile konuşlandırabilirsiniz. Web kancaları ve REST API'leri tümleştirmenize yardımcı olabilir.
- **Ücretsiz bulut barındırılan yapılar:** Bu yapılar genel ve özel depolar için kullanılabilir.
- **Diğer bulut satıcılarına dağıtım desteği:** Satıcılar aws ve Google Cloud Platform içerir.

**Başvurular**
- [Azure Pipelines kılavuzunu kullanarak başlayın](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Azure DevOps’u kullanmaya başlama](https://app.vsaex.visualstudio.com/signup/)
  
## <a name="distribute-your-application-directly-to-app-stores"></a>Uygulamanızı doğrudan App Store'lara dağıtın
Uygulamanız üretim kullanımına hazır olduktan ve genel olarak kullanılmasını istedikten sonra, uygulamanın müşteriler tarafından indirilebildiği uygulama mağazalarına gönderilmesi gerekir. Uygulamanızı doğrudan uygulama mağazalarına dağıtmanın birden çok yolu vardır. 

### <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Distribute](/appcenter/distribution/stores/)ile mobil uygulamalarınızı doğrudan uygulama mağazalarına yayınlayabilirsiniz. Uygulamanız kullanıcılar tarafından indirilmeye hazır olduktan sonra, uygulama ikililerinizi doğrudan Visual Studio App Center portalından yayınlayabilirsiniz. 

Doğrudan şu adresine dağıtabilirsiniz:
- [Apple App Store](/appcenter/distribution/stores/apple)
- [Google Play Store](/appcenter/distribution/stores/googleplay)
- [Microsoft Intune](/appcenter/distribution/stores/intune)
    
### <a name="apple-app-store"></a>Apple App Store
Apple tarafından geliştirilen ve bakımı yapılan uygulama mağazasında kullanıcılar iOS, MacOS, WatchOS ve tvOS aygıtları için geliştirilen uygulamalara göz atabilir ve indirebilir. Geliştiricilerin iOS uygulamalarını genel kullanım için Apple App Store'a göndermeleri gerekir.

### <a name="google-play"></a>Intune Şirket Portalı

Google Play, kullanıcıların Google üzerinden yayınlanan Android cihazlar için geliştirilen uygulamalara göz atabileceği ve indirebildiği Android Işletim Sistemi için resmi uygulama mağazasıdır.

### <a name="intune"></a>Intune

[Microsoft Intune,](/intune/app-management) kurumsal mobilite yönetimi alanında, iş gücünüzün üretken olmasını ve şirket verilerinizi korumasını sağlayan bulut tabanlı bir hizmettir. Intune ile şunları yapabilirsiniz:
- Şirket verilerine erişmek için iş gücünüzün kullandığı mobil cihazları ve bilgisayarları yönetin.
- İş gücünüzün kullandığı mobil uygulamaları yönetin.
- İş gücünüzün bu bilgilere erişme ve paylaşma şeklini kontrol ederek şirket bilgilerinizi koruyun.
- Aygıtların ve uygulamaların şirket güvenlik gereksinimleriyle uyumlu olduğundan emin olun.
    
## <a name="deploy-updates-directly-to-users-devices"></a>Güncelleştirmeleri doğrudan kullanıcıların cihazlarına dağıtma

### <a name="codepush"></a>CodePush
App Center'daki [CodePush](/appcenter/distribution/codepush/) ile Apache Cordova ve React Native geliştiricileri mobil uygulama güncellemelerini doğrudan kullanıcılarının cihazlarına dağıtabilir. Geliştiricilerin JavaScript, HTML, CSS ve görüntü değişiklikleri gibi belirli güncelleştirmeleri yayımlayabildiği merkezi bir depo görevi görür. Ardından uygulamalar, sağlanan istemci SDK'larını kullanarak depodan güncelleştirmeleri sorgulayabilir. Bu şekilde, hataları ele alırken veya küçük özellikler eklerken kullanıcılarınızla daha belirleyici ve doğrudan etkileşim modeline sahip olabilirsiniz. Bir ikiliyi yeniden oluşturmanız veya ortak uygulama mağazalarında yeniden dağıtmanız gerekmez.

**Önemli özellikler**
- Cordova ve React Native geliştiricileri, mobil uygulama güncelleştirmelerini mağazada yayınlamadan doğrudan kullanıcılarının cihazlarına dağıtabilir.
- Hataları düzeltmek veya ikili yeniden oluşturmanızı ve ilgili mağazalarda yeniden dağıtmanızı gerektirmeyen küçük özellikler eklemek ve kaldırmak için kullanışlıdır.

**Başvurular**
- [Visual Studio App Center ile kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center'da CodePush ile başlayın](/appcenter/distribution/codepush/)
- [CodePush CLI](/appcenter/distribution/codepush/cli)