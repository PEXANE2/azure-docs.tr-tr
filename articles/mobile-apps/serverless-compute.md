---
title: Azure İşlevleri ve diğer hizmetlerle sunucusuz bir mobil uygulama arka uç oluşturma
description: Sağlam, sunucusuz bir mobil uygulama oluşturmak için kullanılan bilgi işlem hizmetleri hakkında bilgi edinin.
author: codemillmatt
ms.service: vs-appcenter
ms.assetid: 444f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: d6a2bbc984a8808d2cd8a9b292e55d80f43f2e9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240141"
---
# <a name="build-mobile-back-end-components-with-compute-services"></a>İşlem hizmetleriyle mobil arka uç bileşenleri oluşturun
Her mobil uygulamanın veri depolama, iş mantığı ve güvenlikiçin sorumlu bir arka uca ihtiyacı vardır. Arka uç kodunu barındıracak ve yürütecek altyapıyı yönetmek, birden çok sunucuyu boyutlandırmanızı, sağlamanızı ve ölçeklendirmenizi gerektirir. Ayrıca işletim sistemi güncelleştirmelerini ve ilgili donanımı yönetmeniz ve güvenlik yamaları uygulamanız gerekir. Ardından, performans, kullanılabilirlik ve hata toleransı için tüm bu altyapı bileşenlerini izlemeniz gerekir. 

Sunucusuz mimari, yönetecek sunucunuz ve yönetecek işletim sistemi veya ilgili yazılım veya donanım güncelleştirmeniz olmadığından, bu tür senaryolar için kullanışlıdır. Sunucusuz mimari, geliştiricinin zaman ve maliyet tasarrufu sağlar, bu da pazara daha hızlı zaman kazandırması ve enerjinin uygulama oluşturmaya odaklanması anlamına gelir.

## <a name="benefits-of-compute"></a>Hesaplamanın faydaları
- Sunucuların soyutlanması, yalnızca koda odaklanmanızı sağlayan barındırma, yama ve güvenlik konusunda endişelenmenize gerek olmadığı anlamına gelir.
- Anında ve verimli ölçeklendirme, kaynakların ihtiyacınız olan ölçekte otomatik olarak veya isteğe bağlı olarak sağlanmasını sağlar.
- Yüksek kullanılabilirlik ve arıza toleransı.
- Mikro faturalandırma, yalnızca kodunuz gerçekten çalışırken faturalandırılmanızı sağlar.
- Kod, seçtiğiniz dilde yazılmış bulutta çalışır.

Mobil uygulamalarınızda sunucusuz bilgi işlem özelliklerini etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="azure-functions"></a>Azure İşlevleri
[Azure İşlevler,](https://azure.microsoft.com/services/functions/) sunucular hakkında endişelenmeden seçtiğiniz programlama dilinde yazılmış kodunuzu yürütmek için kullanabileceğiniz olay odaklı bir işlem deneyimidir. Çalıştırmak için uygulamayı veya altyapıyı yönetmeniz gerekyoktur. İşlevler isteğe bağlı olarak ölçeklendirilir ve yalnızca kodunuzu çalıştırdığınız süre için ödeme yaptığınızda, bu süre yiyebilirsiniz. Azure işlevleri, mobil uygulama için API uygulamanın harika bir yoludur. Bunlar uygulamak ve korumak kolaydır ve HTTP üzerinden erişilebilir.

**Önemli özellikler**
- Bir işlevin ne zaman çağrıldığını ve hangi verileri bağlayacaklarını tanımlamak için tetikleyicileri ve bağlamaları kullanabileceğiniz olay odaklı ve ölçeklenebilir.
- İşlevler NuGet ve NPM'yi desteklediği için kendi bağımlılıklarınızı getirin, böylece en sevdiğiniz kitaplıkları kullanabilirsiniz.
- Azure Active Directory, Facebook, Google, Twitter ve Microsoft Hesabı gibi OAuth sağlayıcılarıyla HTTP tarafından tetiklenen işlevleri koruyabilmeniz için entegre güvenlik.
- Hizmet (SaaS) teklifleri olarak farklı [Azure hizmetleri](/azure/azure-functions/functions-overview) ve yazılımlarla basitleştirilmiş tümleştirme.
- İşlevlerinizi Azure portalında doğru şekilde kodlayabilmeniz veya sürekli tümleştirme ayarlayabilmeniz ve kodunuzu GitHub, Azure DevOps Hizmetleri ve diğer desteklenen geliştirme araçları aracılığıyla dağıtabilmeniz için esnek geliştirme.
- İşlevler çalışma zamanı açık kaynak kodalır ve [GitHub'da](https://github.com/azure/azure-webjobs-sdk-script)kullanılabilir.
- Tercih ettikleri düzenleyiciyi veya kullanımı kolay web arabirimini kullanarak, entegre araçlar ve yerleşik DevOps özellikleriyle izleme ile yerel olarak kodlayabileceğiniz, test edilebildiğiniz ve hata ayıklaabileceğiniz gelişmiş geliştirme deneyimi.
- C#, Node.js, Java, JavaScript veya Python gibi çeşitli programlama dilleri ve geliştirme seçenekleri.
- Kullanım başına ödeme fiyatlandırma modeli, yalnızca kodunuzu çalıştırmak için harcadığınız süre için ödeme yaptığınız anlamına gelir.

**Başvurular**
- [Azure portalında](https://portal.azure.com)
- [Azure Fonksiyonları belgeleri](/azure/azure-functions/)
- [Azure İşlevleri geliştirici kılavuzu](/azure/azure-functions/functions-reference)
- [Hızlı başlangıçlar](/azure/azure-functions/functions-create-first-function-vs-code)
- [Örnekler](/samples/browse/?products=azure-functions&languages=csharp)

## <a name="azure-app-service"></a>Azure App Service
[Azure Uygulama Hizmeti](https://azure.microsoft.com/services/app-service/)ile, altyapıyı yönetmeden web uygulamaları ve RESTful API'leri seçtiğiniz programlama dilinde oluşturabilir ve barındırabilirsiniz. Otomatik ölçekleme ve yüksek kullanılabilirlik sunar, hem Windows'u hem de Linux'u destekler ve GitHub, Azure DevOps veya herhangi bir Git repo'dan otomatik dağıtımlar sağlar.

**Önemli özellikler**
- ASP.NET, ASP.NET Core, Java, Ruby, Node.js, PHP veya Python için birden fazla dil ve çerçeve desteği. Ayrıca, PowerShell’i ve diğer betikleri ya da yürütülebilir hizmetleri arka plan hizmetleri olarak çalıştırabilirsiniz.
- DevOps optimizasyonu, Azure DevOps, GitHub, BitBucket, Docker Hub veya Azure Konteyner Kayıt Defteri ile sürekli tümleştirme ve dağıtım yoluyla optimizasyon. Azure PowerShell veya platformlar arası komut satırı arabirimi (CLI) kullanarak uygulamalarınızı App Service’de yönetin.
- El ile veya otomatik olarak ölçeklendirmek veya çıkarmak için yüksek kullanılabilirlik ile küresel ölçek.
- SAP, Salesforce gibi kurumsal sistemler için 50'den fazla konektör ve Facebook gibi internet hizmetleri arasından seçim yapmak için SaaS platformlarına ve şirket içi verilere bağlantılar. Karma bağlantıları ve Azure Sanal Ağları kullanarak şirket içi verilere erişin.
- Azure Uygulama Hizmeti ISO, SOC ve PCI uyumludur. Azure Active Directory veya Google, Facebook, Twitter ve Microsoft gibi sosyal medya için oturum açma ile kullanıcıların kimliğini doğrulayın. IP adresi kısıtlamaları oluşturun ve hizmet kimliklerini yönetin.
- WordPress, Joomla ve Drupal gibi Azure Marketi'ndeki kapsamlı uygulama şablonları listesinden seçim yapmak için uygulama şablonları.
- Visual Studio'daki özel araçlarla Visual Studio entegrasyonu oluşturma, dağıtma ve hata ayıklama işini kolaylaştırır.

**Başvurular**
- [Azure portalında](https://portal.azure.com/)
- [Azure Uygulama Hizmeti belgeleri](/azure/app-service/)
- [Hızlı başlangıçlar](/azure/app-service/app-service-web-get-started-dotnet)
- [Örnekler](/azure/app-service/samples-cli)
- [Öğreticiler](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service
[Azure Kubernetes Service (AKS),](https://azure.microsoft.com/services/kubernetes-service/) barındırılan Kubernetes ortamınızı yönetir. AKS, konteyner düzenleme uzmanlığı olmadan konteyner uygulamalarının hızlı ve kolay bir şekilde dağıtılmasını ve yönetilmesini sağlar. Ayrıca devam eden operasyon ve bakım yükünü ortadan kaldırır. AKS hükümleri, yükseltmeleri ve uygulamalarınızı çevrimdışı olmadan talep üzerine kaynakları ölçekler.

**Önemli özellikler**
- Varolan uygulamaları kolayca kapsayıcılara geçirin ve AKS içinde çalıştırın.
- Mikro hizmetlere dayalı uygulamaların dağıtımını ve yönetimini basitleştirin.
- Hız ve güvenlik arasında dengeyi sağlamak ve kodu ölçekte daha hızlı sunmak için AKS için DevOps'leri güvenli hale getirin.
- Saniyeler içinde başlayan Kapsayıcı Örnekleri içinde bölme sağlamak için AKS ve Azure Kapsayıcı Örnekleri'ni kullanarak kolaylıkla ölçeklendirin.
- IoT aygıtlarını isteğe bağlı olarak dağıtın ve yönetin.
- TensorFlow ve KubeFlow gibi araçların kullanımı ile makine öğrenme modellerini eğitin.

**Başvurular**
- [Azure portalında](https://portal.azure.com/)
- [Azure Kubernetes Hizmet belgeleri](/azure/aks/)
- [Hızlı başlangıçlar](/azure/aks/kubernetes-walkthrough-portal)
- [Öğreticiler](/azure/aks/tutorial-kubernetes-prepare-app)

## <a name="azure-container-instances"></a>Azure Container Instances
[Azure Kapsayıcı Örnekleri,](https://azure.microsoft.com/services/container-instances/) basit uygulamalar, görev otomasyonu ve iş oluşturma gibi yalıtılmış kapsayıcılarda çalışabilen tüm senaryolar için harika bir çözümdür. VM'leri yönetmeden uygulamaları hızlı geliştirin.

**Önemli özellikler**
- Kapsayıcı Örnekleri olarak hızlı başlatma süreleri, VM'leri sağlama ve yönetmeye gerek kalmadan Azure'da kapsayıcıları saniyeler içinde başlatabilir.
- Genel IP bağlantısı ve özel DNS adı.
- Uygulamanızın vm'de olduğu kadar bir kapta izole olmasını garanti eden hipervizör düzeyinde güvenlik.
- CPU çekirdekleri ve bellek tam özellikleri sağlayarak optimum kullanım için özel boyutlar. İhtiyaçlarınıza göre ödeme yapar ve saniye başına faturalandırılırsınız. Böylece harcamalarınızı ihtiyaçlarınıza uyacak şekilde ayarlayabilirsiniz.
- Kalıcı depolama almak ve durumu devam. Kapsayıcı Örnekleri, Azure Dosyaları paylaşımlarının doğrudan montajını sunar.
- Linux ve Windows kapsayıcıları aynı API ile zamanlanır.

**Başvurular**
- [Azure portalında](https://portal.azure.com/)
- [Azure Kapsayıcı Örnekleri belgeleri](/azure/container-instances/)
- [Hızlı başlangıçlar](/azure/container-instances/container-instances-quickstart-portal)
- [Örnekler](https://azure.microsoft.com/resources/samples/?sort=0&term=aci)
- [Öğreticiler](/azure/container-instances/container-instances-tutorial-prepare-app)