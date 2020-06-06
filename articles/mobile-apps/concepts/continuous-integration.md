---
title: Visual Studio App Center ve Azure hizmetleriyle uygulamalarınızın yaşam döngüsünü otomatikleştirin
description: Mobil uygulamalarınız için sürekli derlemeyi ve tümleştirmeyi ayarlamaya yardımcı olan App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 34a8a070-9b3c-4faf-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 54103ad999d3e310d53e84f29d41aad733abfe93
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450881"
---
# <a name="automate-the-lifecycle-of-your-apps-with-continuous-build-and-integration"></a>Sürekli derleme ve tümleştirme sayesinde uygulamalarınızın yaşam döngüsünü otomatikleştirin

Geliştiriciler olarak kod yazar ve kod deposuna iade edersiniz, ancak depoya iade edilen işlemeler her zaman tutarlı olmayabilir. Birden çok geliştirici aynı proje üzerinde çalışırken, sorunlar tümleştirme ile birlikte gelebilir. Ekipler, işlerin işe madığı, hata oluşturan ve proje geliştirmenin gecikdüğü durumlarda çalışabilir. Geliştiricilerin, hataları denetlemek için tüm yazılım kodunun oluşturulup Test edilene kadar beklemesi gerekir. Bu işlem, işlemi yavaşlatır ve daha az yinelemeli hale getirir. 

Sürekli derleme ve tümleştirmeyle, geliştiriciler, değişiklikleri kaynak kodu deposuna kaydederek ve test ve doğrulamaları derleme ortamına yerleştirerek, derlemeleri basitleştirebilir ve kodlarını test edebilir. Bu şekilde, her zaman kendi kodlarına göre testler çalıştırırlar. Kaynak kodda yapılan tüm değişiklikler, depoya yapılan bir kayıt olduğunda sürekli olarak oluşturulur. Her iadede sürekli tümleştirme (CI) sunucusu, geliştiricinin oluşturduğu tüm testleri doğrular ve yürütür. Testler geçmezse, daha fazla değişiklik için kod geri gönderilir. Bu şekilde, geliştiriciler oluşturulan yapıları bozmayın. Ayrıca, geliştirici üretkenliğini artıran tüm testleri bilgisayarlarında yerel olarak çalıştırmak zorunda kalmaz. 

## <a name="key-benefits"></a>Önemli avantajlar
- İşlem hatları için derlemelerinizi, testlerinizi ve dağıtımlarınızı otomatikleştirin.
- Daha hızlı yayın ücretleri sağlamak için hataları tespit edin ve sorunları erken giderin.
- Kodu daha sık işleyin ve uygulamaları hızla oluşturun.
- Herhangi bir sorunla karşılaşmadan kodu hızla değiştirme esnekliği elde edin.
- Yalnızca iyi kalitede kodun tüm şekilde kullanmasını sağlamak için pazara daha hızlı vakit kazanın.
- Küçük kod değişikliklerini daha verimli hale getirin çünkü küçük kod parçaları tek seferde tümleşiktir.
- Müşterilerinizin ve takımınızdan sürekli geri bildirim alabilmeniz için ekip saydamlığını ve sorumlulutürei artırın.

Mobil uygulamalarınızda sürekli bir tümleştirme işlem hattını etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center derlemesi](/appcenter/build/) , takımınızın güvenli bir bulut altyapısı kullanarak üzerinde çalıştığı yerel ve platformlar arası uygulamalar oluşturmanıza yardımcı olur. Deponuzu kolayca bağlanarak Visual Studio App Center, her yürütmede uygulamanızı bulutta oluşturmaya başlayabilirsiniz. Yapı sunucularını yerel olarak yapılandırma konusunda endişelenmenize gerek yoktur, karmaşık yapılandırmalar ve bir iş arkadaşınızın makinesinde bulunan ancak sizinkiyle ilgili bir kod oluşturur.

Visual Studio App Center hizmetlerinin gücü sayesinde iş akışınızı daha da otomatik hale getirebilirsiniz. App Center dağıtımı ile otomatik olarak, test ediciler ve genel uygulama mağazalarına otomatik olarak yayın yapabilirsiniz. Ayrıca, App Center Test ile buluttaki binlerce gerçek cihaz ve işletim sistemi yapılandırmasında otomatik UI testleri de çalıştırabilirsiniz.

**Önemli özellikler**
- Dakikalar içinde sürekli tümleştirme ayarlayın ve uygulamalar daha sık ve daha hızlı bir şekilde derleyin.
- GitHub, BitBucket, Azure DevOps ve GitLab ile tümleştirin.
- Yönetilen, bulutta barındırılan makinelerde hızlı ve güvenli derlemeler oluşturun.
- Yapılarınızı test başlatmak için etkinleştirin ve uygulamanın gerçek dünyada iOS ve Android cihazlarda oluşturulup oluşturulmayacağını doğrulayın.
- İOS, Android, macOS, Windows, Xamarin ve yerel olarak tepki verme için yerel ve platformlar arası destek elde edin.
- Kopyalama sonrası, derleme öncesi ve derleme sonrası betikleri ekleyerek derlemelerinizi özelleştirin.

**Başvurular**
- [Visual Studio App Center kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center derleme ile çalışmaya başlama](/appcenter/build/)

## <a name="azure-pipelines"></a>Azure Pipelines
 Azure DevOps 'daki bir hizmet olan [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/), tercih ettiğiniz git sağlayıcınızla birlikte çalışan tam özellikli bir sürekli tümleştirme ve sürekli teslım (CD) hizmetidir. Azure 'un dahil olduğu en büyük bulut hizmetlerine dağıtabilir. GitHub, GitHub Enterprise Server, GitLab, Bitbucket bulutu veya Azure Repos için kodunuzla başlayabilirsiniz. Daha sonra Microsoft Azure, Google Cloud Platform veya Amazon Web Services (AWS) için kodunuzun oluşturma, test etme ve dağıtımını otomatik hale getirebilirsiniz.

**Önemli özellikler**
- **CI sunucusunu ayarlamaya yönelik Basitleştirilmiş görev tabanlı deneyim:** Microsoft ve Microsoft olmayan (node. js, Java) tabanlı sunucu teknolojilerine ek olarak hem yerel (Android, iOS, hem de Windows) ve platformlar arası (Xamarin, Cordova ve tepki verme) mobil uygulamaları için bir CI sunucusu ayarlayın.
- **Herhangi bir dil, platform ve bulut:** Node. js, Python, Java, PHP, Ruby, Go, C/C++, C#, Android ve iOS uygulamaları oluşturun, test edin ve dağıtın. Linux, macOS ve Windows üzerinde paralel olarak çalıştırın. Azure, AWS ve Google Cloud Platform gibi bulut sağlayıcılarına dağıtın. Mobil uygulamaları Beta kanalları ve uygulama depoları aracılığıyla dağıtın.
- **Yerel kapsayıcı desteği:** Kolayca yeni kapsayıcılar oluşturun ve bunları herhangi bir kayıt defterine gönderin. Kapsayıcıları bağımsız konaklara veya Kubernetes 'e dağıtın.
- **Gelişmiş iş akışları:** Kolayca derleme zincirlerini ve çoğullanan derlemeleri oluşturun. YAML, test tümleştirmesi, yayın kapıları, raporlama ve daha fazlası için destek alın.
- **Genişletilebilir:** Bolluk 'ten SonarCloud 'a yüzlerce uzantı içeren, topluluk tarafından oluşturulan derleme, test ve dağıtım görevleri aralığını kullanın. Jenkins gibi diğer CI sistemlerinden de dağıtım yapabilirsiniz. Web kancaları ve REST API 'Leri tümleştirmenize yardımcı olabilir.
- **Bulutta barındırılan ücretsiz derlemeler:** Bu derlemeler ortak ve özel depolar için kullanılabilir.
- **Diğer bulut satıcılarına dağıtım desteği:** Satıcıların AWS ve Google Cloud Platform vardır.

**Başvurular**
- [Azure Pipelines Kılavuzu ile çalışmaya başlama](/azure/devops/pipelines/get-started/pipelines-get-started?view=azure-devops)
- [Azure DevOps’u kullanmaya başlama](https://app.vsaex.visualstudio.com/signup/) 
- [Hızlı Başlangıçlar](/azure/devops/pipelines/create-first-pipeline?view=azure-devops&tabs=tfs-2018-2)

Uygulama derlemelerinizin doğru hizmetini seçmenize yardımcı olmak için [App Center derlemeyi ve Azure Pipelines](/appcenter/build/choose-between-services)karşılaştıran makaleye bakın.
