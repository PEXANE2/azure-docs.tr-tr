---
title: Azure Işlevleri ve diğer hizmetlerle sunucusuz bir mobil uygulama arka ucu oluşturun
description: Sağlam, sunucusuz bir mobil uygulama arka ucu oluşturmak için kullanılan işlem hizmetleri hakkında bilgi edinin.
author: codemillmatt
ms.service: mobile-services
ms.assetid: 444f0959-aa7f-472c-a6c7-9eecea3a34b9
ms.topic: conceptual
ms.date: 06/05/2020
ms.author: masoucou
ms.openlocfilehash: f27ef3868cb53c76ecd3ba5d3b24cd075f8a5531
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84485295"
---
# <a name="build-mobile-back-end-components-with-compute-services"></a>İşlem hizmetleri ile mobil arka uç bileşenleri oluşturun
Her mobil uygulamanın, veri depolama, iş mantığı ve güvenlik açısından sorumlu bir arka ucu olması gerekir. Altyapıyı ana bilgisayar ve yürütmeye yönelik olarak yönetmek, birden çok sunucuyu yazmanızı, sağlamanızı ve ölçeklendirmenizi gerektirir. Ayrıca, işletim sistemi güncelleştirmelerini ve ilgili donanımı yönetmeniz ve güvenlik düzeltme eklerini uygulamanız gerekir. Bu durumda, performans, kullanılabilirlik ve hataya dayanıklılık için bu altyapı bileşenlerinin tümünü izlemeniz gerekir. 

Yönetmek istediğiniz bir sunucunuz olmadığından ve yönetilecek bir işletim sistemi ya da donanım güncelleştirmesi olmadığından, sunucusuz mimari bu tür bir senaryo için yararlı olarak sunulur. Sunucusuz mimari, geliştirici süresini ve maliyetini kaydeder; bu da, uygulama oluşturmaya yönelik pazara ve odaklanmış enerji süresini daha hızlı sağlar.

## <a name="benefits-of-compute"></a>İşlem avantajları
- Sunucu soyutlama, barındırma, düzeltme eki uygulama ve güvenlik konusunda endişelenmenize gerek olmadığı anlamına gelir. Bu, yalnızca koda odaklanabilmenizi sağlar.
- Anında ve verimli ölçekleme, kaynakların otomatik olarak veya ihtiyacınız olan herhangi bir ölçeğe göre sağlanmış olmasını sağlar.
- Yüksek kullanılabilirlik ve hataya dayanıklılık.
- Mikro faturalandırma yalnızca kodunuzun gerçekten çalıştığı zaman için faturalandırılabilmenizi sağlar.
- Kod, seçtiğiniz dilde yazılmış bulutta çalışır.

Mobil uygulamalarınızda sunucusuz işlem yeteneklerini etkinleştirmek için aşağıdaki hizmetleri kullanın.

## <a name="azure-functions"></a>Azure İşlevleri
[Azure işlevleri](https://azure.microsoft.com/services/functions/) , kodunuzu yürütmek için kullanabileceğiniz ve sunucu hakkında endişelenmeden, seçtiğiniz programlama dilinde yazılmış olay odaklı bir işlem deneyimidir. Uygulamayı ya da çalıştırmak için altyapıyı yönetmeniz gerekmez. İşlevler isteğe bağlı olarak ölçeklendirilir ve yalnızca kodunuzun çalıştığı süre için ödeme yaparsınız. Azure işlevleri, bir mobil uygulama için API uygulamanın harika bir yoludur. Kolayca uygulamanız ve bakımını yapmak ve HTTP üzerinden erişilebilir olmaları kolaydır.

**Önemli özellikler**
- Bir işlevin ne zaman çağrılabileceğini ve hangi verilerin bağlandığını tanımlamak için Tetikleyicileri ve bağlamaları kullanabileceğiniz olay odaklı ve ölçeklenebilir.
- Işlevler NuGet ve NPM 'yi desteklediğinden, en sevdiğiniz kitaplıkları kullanabilmeniz için kendi bağımlılıklarınızı getirin.
- Azure Active Directory, Facebook, Google, Twitter ve Microsoft hesabı gibi OAuth sağlayıcılarıyla HTTP ile tetiklenen işlevleri koruyabilmeniz için tümleşik güvenlik.
- Farklı [Azure hizmetleri](/azure/azure-functions/functions-overview) ve hizmet olarak yazılım (SaaS) teklifleri ile basitleştirilmiş tümleştirme.
- Esnek geliştirme, işlevlerinizi hemen Azure portal veya sürekli tümleştirme ayarlayıp kodunuzu GitHub, Azure DevOps Services ve diğer desteklenen geliştirme araçları aracılığıyla dağıtmanıza olanak sağlayabilir.
- İşlevler çalışma zamanı açık kaynaktır ve [GitHub](https://github.com/azure/azure-webjobs-sdk-script)'da kullanılabilir.
- Tümleşik araçlar ve yerleşik DevOps özellikleri ile izleme sayesinde tercih edilen düzenleyiciyi veya kullanımı kolay Web arabirimini kullanarak yerel olarak kod ve test etme ve hata ayıklama işlemlerini kullanabileceğiniz gelişmiş geliştirme deneyimi.
- C#, Node. js, Java, JavaScript veya Python gibi geliştirme için çeşitli programlama dilleri ve barındırma seçenekleri.
- Kullanım başına ödeme modeli, yalnızca kodunuzu çalıştırırken harcanan süre için ödeme yaptığınız anlamına gelir.

**Başvurular**
- [Azure portal](https://portal.azure.com)
- [Azure İşlevleri belgeleri](/azure/azure-functions/)
- [Azure İşlevleri geliştirici kılavuzu](/azure/azure-functions/functions-reference)
- [Hızlı başlangıçlar](/azure/azure-functions/functions-create-first-function-vs-code)
- [Örnekler](/samples/browse/?products=azure-functions&languages=csharp)

## <a name="azure-app-service"></a>Azure App Service
[Azure App Service](https://azure.microsoft.com/services/app-service/)sayesinde, altyapıyı yönetmeksizin Web uygulamaları ve tercih ettiğiniz programlama dilinde API 'ler oluşturabilir ve bunları barındırabilirsiniz. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux destekler ve GitHub, Azure DevOps veya herhangi bir git deposundan otomatik dağıtımları sağlar.

**Önemli özellikler**
- ASP.NET, ASP.NET Core, Java, Ruby, Node. js, PHP veya Python için birden çok dil ve çerçeve desteği. Ayrıca, PowerShell’i ve diğer betikleri ya da yürütülebilir hizmetleri arka plan hizmetleri olarak çalıştırabilirsiniz.
- Azure DevOps, GitHub, BitBucket, Docker Hub veya Azure Container Registry ile sürekli tümleştirme ve dağıtım aracılığıyla DevOps iyileştirmesi. Azure PowerShell veya platformlar arası komut satırı arabirimi (CLI) kullanarak uygulamalarınızı App Service’de yönetin.
- Ölçeği el ile veya otomatik olarak genişletmek için yüksek kullanılabilirliğe sahip küresel ölçek.
- SAP, Salesforce gibi SaaS Hizmetleri ve Facebook gibi İnternet Hizmetleri için 50 ' den fazla bağlayıcıdan seçim yapmak amacıyla SaaS platformları ve şirket içi verilere bağlantılar. Karma bağlantıları ve Azure sanal ağlarını kullanarak şirket içi verilere erişin.
- Azure App Service ISO, SOC ve PCI uyumludur. Google, Facebook, Twitter ve Microsoft gibi sosyal medya için Azure Active Directory veya oturum açma ile kullanıcıların kimliğini doğrulayın. IP adresi kısıtlamaları oluşturun ve hizmet kimliklerini yönetin.
- Azure Marketi 'nde WordPress, Joomla ve Drupal gibi kapsamlı uygulama şablonları listesinden seçim yapabileceğiniz uygulama şablonları.
- Visual Studio 'da adanmış araçlarla Visual Studio tümleştirmesi oluşturma, dağıtma ve hata ayıklama işini kolaylaştırır.

**Başvurular**
- [Azure portal](https://portal.azure.com/)
- [Azure App Service belgeleri](/azure/app-service/)
- [Hızlı başlangıçlar](/azure/app-service/app-service-web-get-started-dotnet)
- [Örnekler](/azure/app-service/samples-cli)
- [Öğreticiler](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service
[Azure Kubernetes hizmeti (AKS)](https://azure.microsoft.com/services/kubernetes-service/) , barındırılan Kubernetes ortamınızı yönetir. AKS, kapsayıcı düzenleme uzmanlığı olmadan Kapsayıcılı uygulamaları dağıtmayı ve yönetmeyi hızlı ve kolay hale getirir. Ayrıca, devam eden işlemlerin ve bakımın yükünü ortadan kaldırır. AKS, uygulamalarınızı çevrimdışı duruma getirmeden, kaynakları isteğe bağlı olarak sağlar, yükseltir ve ölçeklendirir.

**Önemli özellikler**
- Mevcut uygulamaları kapsayıcılara kolayca geçirin ve AKS içinde çalıştırın.
- Mikro hizmet tabanlı uygulamaların dağıtımını ve yönetimini kolaylaştırın.
- Hız ve güvenlik arasındaki dengeyi elde etmek ve ölçeği daha hızlı bir şekilde sunmak için AKS için güvenli DevOps.
- Saniyeler içinde başlayan Container Instances içinde Pod sağlamak için aks ve Azure Container Instances kullanarak kolayca ölçeklendirin.
- IoT cihazlarını isteğe bağlı olarak dağıtın ve yönetin.
- TensorFlow ve KubeFlow gibi araçların kullanımıyla makine öğrenimi modellerini eğitme.

**Başvurular**
- [Azure portal](https://portal.azure.com/)
- [Azure Kubernetes hizmeti belgeleri](/azure/aks/)
- [Hızlı başlangıçlar](/azure/aks/kubernetes-walkthrough-portal)
- [Öğreticiler](/azure/aks/tutorial-kubernetes-prepare-app)

## <a name="azure-container-instances"></a>Azure Container Instances
[Azure Container Instances](https://azure.microsoft.com/services/container-instances/) basit uygulamalar, görev otomasyonu ve derleme işleri gibi yalıtılmış kapsayıcılarda çalışabilen her senaryo için harika bir çözümdür. VM 'Leri yönetmeden uygulamaları hızlı bir şekilde geliştirin.

**Önemli özellikler**
- Container Instances hızlı başlangıç süreleri, sanal makineleri sağlamaya ve yönetmeye gerek kalmadan Azure 'daki kapsayıcıları Saniyeler içinde başlatabilir.
- Genel IP bağlantısı ve özel DNS adı.
- Uygulamanızı güvence altına alan hiper yönetici düzeyinde güvenlik, bir VM 'de olduğu gibi bir kapsayıcıda yalıtılmıştır.
- CPU çekirdeklerinin ve belleğin tam özelliklerine izin vererek en iyi kullanım için özel boyutlar. İhtiyaçlarınıza göre ödeme yapar ve saniye başına faturalandırılırsınız. Böylece harcamalarınızı ihtiyaçlarınıza uyacak şekilde ayarlayabilirsiniz.
- Durumu almak ve sürdürmek için kalıcı depolama. Container Instances Azure dosya paylaşımlarının doğrudan bağlanmasını sağlar.
- Linux ve Windows kapsayıcıları aynı API ile zamanlandı.

**Başvurular**
- [Azure portal](https://portal.azure.com/)
- [Azure Container Instances belgeleri](/azure/container-instances/)
- [Hızlı başlangıçlar](/azure/container-instances/container-instances-quickstart-portal)
- [Örnekler](https://azure.microsoft.com/resources/samples/?sort=0&term=aci)
- [Öğreticiler](/azure/container-instances/container-instances-tutorial-prepare-app)