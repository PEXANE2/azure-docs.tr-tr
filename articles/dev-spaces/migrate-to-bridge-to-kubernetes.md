---
title: Kubernetes 'e geçiş
services: azure-dev-spaces
ms.date: 09/21/2020
ms.topic: conceptual
description: Güç Azure Dev Spaces olan süreçler açıklanmaktadır
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Kubernetes ile Köprü oluşturma
ms.openlocfilehash: b585ee20efb7b377a041152996ef41d8c59c539e
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90998120"
---
# <a name="migrating-to-bridge-to-kubernetes"></a>Kubernetes 'e geçiş

Kubernetes Köprüsü, Azure Dev Spaces ile çalışan çoğu geliştirme senaryosunda daha hafif bir alternatif sağlar. Kubernetes Köprüsü, [Visual Studio][vs]   ve [Visual Studio Code][vsc]uzantıları kullanılarak yalnızca istemci tarafı bir deneyimdir.  

Kubernetes Köprüsü, kurulu bir Kubernetes uygulamasının yerel geliştirme iş istasyonunuzda çalışan bir hizmeti içermesini sağlayarak geliştirme deneyiminize yardımcı olur. Geliştirme alanlarının aksine, Kubernetes Köprüsü, Docker ve Kubernetes yapılandırmalarının oluşturulması gereksinimini ortadan kaldırarak iç döngü karmaşıklıklarını azaltır. böylece geliştiricilerin, mikro hizmet kodunun iş mantığına odaklanmasını sağlar.

Kubernetes Köprüsü, Kubernetes ortamınızdan bağımlılıklar ve mevcut yapılandırma kullanılırken geliştirme bilgisayarınızda çalışan kodda yineleme yapmanıza yardımcı olur. Buna karşılık, Azure Dev Spaces hizmetinize uzaktan hata ayıklamanıza ve kodunuzda tekrarlanmadan önce mikro hizmetinizi Kubernetes ortamına dağıtır.

Bu makalede, Kubernetes 'e Azure Dev Spaces ile Köprü oluşturma ile Kubernetes arasında Azure Dev Spaces geçiş yönergeleri arasında bir karşılaştırma sağlanır.

## <a name="development-approaches"></a>Geliştirme yaklaşımları

![Geliştirme yaklaşımları](media/migrate-to-btk/development-approaches.svg)

Kubernetes geliştiricileri, doğrudan kendi AKS kümesinde çalışan kodla çalışarak, dağıtılan ortama benzemeyen bir yerel ortam gereksinimini ortadan kaldırma. Azure Dev Spaces Bu yaklaşım, geliştirmenin belirli yönlerini geliştirmektedir, ancak Azure Dev Spaces kullanmaya başlayabilmeniz için Docker, Kubernetes ve Held gibi ek kavramların öğrenmesinin ve sürdürülmesi için de bir önkoşul getirmiştir.

Kubernetes Köprüsü, geliştiricilerin, kümesinin geri kalanı ile etkileşim kurarken doğrudan geliştirme bilgisayarlarında çalışmasına izin verir. Bu yaklaşım, kümesi tarafından sunulan bağımlılıkları ve ortamı paylaşırken doğrudan geliştirme bilgisayarlarında kod çalıştırmanın öğrenilmesi ve hızlarından yararlanır. Bu yaklaşım ayrıca, Kubernetes 'de çalışmaya gelen Aslına göre ve ölçeklendirmeden faydalanır.

## <a name="feature-comparison"></a>Özellik Karşılaştırması

Kubernetes 'in Azure Dev Spaces ve Köprüsü benzer özelliklere sahiptir ve ayrıca çeşitli alanlarda farklılık gösterir:

| Gereksinim  | Azure Dev Spaces  | Kubernetes 'e köprü oluşturma  |
|---------------|-------------------|--------------------------------|
| Azure Kubernetes Service | 15 Azure bölgesinde | Herhangi bir AKS hizmet bölgesi    |
| **Güvenlik** |
| Kümenizde güvenlik erişimi gerekiyor  | AKS kümesi Katılımcısı  | Kubernetes RBAC-dağıtım güncelleştirmesi   |
| Geliştirme bilgisayarınızda güvenlik erişimi gerekli  | Yok  | Yerel yönetici/sudo   |
| **Stillerin** |
| Kubernetes ve Docker yapılarından bağımsız  | Hayır  | Yes   |
| Değişiklikleri otomatik geri alma, hata ayıklama sonrası  | Hayır  | Yes   |
| **Ortamlar** |
| Visual Studio 2019 ile birlikte çalışma  | Yes  | Yes   |
| Visual Studio Code ile birlikte çalışma  | Yes  | Yes   |
| CLı ile birlikte çalışma  | Yes  | Hayır   |
| **İşletim sistemi uyumluluğu** |
| Windows 10 ' da geçerlidir  | Yes  | Yes  |
| Linux üzerinde çalışma  | Yes  | Yes  |
| MacOS 'ta çalışma  | Yes  | Yes  |
| **Özellikler** |
| Geliştirici yalıtımı veya takım geliştirme  | Yes  | Yes  |
| Ortam değişkenlerinin seçmeli olarak üzerine yaz  | Hayır  | Yes  |
| Dockerfile ve Held grafiğinin oluşturulması  | Yes  | Hayır  |
| Kubernetes 'e kalıcı kod dağıtımı  | Yes  | Hayır  |
| Bir Kubernetes Pod 'da uzaktan hata ayıklama  | Yes  | Hayır  |
| Yerel hata ayıklama, Kubernetes 'e bağlanıldı  | Hayır  | Yes  |
| Aynı iş istasyonunda aynı anda birden fazla hizmetin hatalarını ayıklama  | Yes  | Yes  |

## <a name="kubernetes-inner-loop-development"></a>Kubernetes iç döngüsü geliştirme

Kubernetes 'e Azure Dev Spaces ve köprü arasındaki en büyük fark kodunuzun çalıştığı yerdir. Azure Dev Spaces, mikro hizmet kodunuzda geliştirme ve hata ayıklamanıza yardımcı olur, ancak bu kodu kümenizde çalıştırmanızı gerektirir. Kubernetes Köprüsü, Kubernetes 'te çalışan büyük uygulamanın bağlamında, mikro hizmet kodunuzu doğrudan geliştirme bilgisayarınızda geliştirmenize ve hata ayıklamanıza olanak tanır. Kubernetes Köprüsü, Kubernetes kümesinin çevre düzeyini genişletir ve yerel işlemlerin Kubernetes yapılandırmalarını devralmasını sağlar.

![İç döngü geliştirme](media/migrate-to-btk/btk-graphic-non-isolated.gif)

Kubernetes için köprü kullanırken, geliştirme bilgisayarınız ile kümeniz arasında bir ağ bağlantısı oluşturulur.Bu bağlantının kullanım ömrü boyunca, istekleri hizmete geliştirme bilgisayarınıza yönlendiren Kubernetes dağıtımınızın yerine kümenize bir ara sunucu eklenir. Bağlantıyı kestiğinizde uygulama dağıtımı, küme üzerinde çalışan dağıtımın orijinal sürümünü kullanmaya döndürülür. Bu yaklaşım, Azure Dev Spaces kod ile hangi kodun eşitlendiğinde çalışır, oluşturulur ve çalıştırılır. Azure Dev Spaces Ayrıca kodunuzu geri almaz.

Kubernetes Köprüsü, dağıtım yöntemlerinden bağımsız olarak Kubernetes 'te çalışan uygulamalarla çalışma esnekliği sunar. Uygulamanızı derlemek ve çalıştırmak için CI/CD kullanıyorsanız, sağlanan araçları veya özel betikleri kullanıp kullanmayacağınızı bağımsız olarak, kodunuzu geliştirmek ve hatalarını ayıklamak için Kubernetes ile Köprü kullanmaya devam edebilirsiniz.

> [!TIP]
>  [Microsoft Kubernetes uzantısı][kubernetes-extension] , IntelliSense Ile Kubernetes bildirimlerini hızlı bir şekilde geliştirmenize olanak tanır.  

### <a name="use-visual-studio-to-transition-to-bridge-to-kubernetes-from-azure-dev-spaces"></a>Azure Dev Spaces 'den Kubernetes 'e geçiş yapmak için Visual Studio 'Yu kullanın

1. Visual Studio IDE 'nizi 16,7 veya üzeri bir sürüme güncelleştirin ve köprüyü [Visual Studio Market][vs-marketplace]Kubernetes uzantısına yüklersiniz.
1. Azure portal veya [Azure dev Spaces CLI][azds-delete]kullanarak Azure dev Spaces denetleyiciyi devre dışı bırakın.
1. `azds.yaml`Dosyayı projenizden kaldırın.
1. Uygulamanızı yeniden dağıtın.
1. Dağıtılan uygulamanızda Kubernetes için köprü yapılandırın. Visual Studio 'da Kubernetes ile Köprü kullanma hakkında daha fazla bilgi için bkz. [Kubernetes Için köprü kullanma][use-btk-vs].
1. Yeni oluşturulan köprü ile Kubernetes hata ayıklama profilini kullanarak Visual Studio 'da hata ayıklamayı başlatın.

### <a name="use-visual-studio-code-to-transition-to-bridge-to-kubernetes-from-azure-dev-spaces"></a>Azure Dev Spaces ile Kubernetes köprüsüne geçiş yapmak için Visual Studio Code kullanın

1. [Kubernetes uzantısına köprü][vsc-marketplace]yükler.
1. Azure portal veya [Azure dev Spaces CLI][azds-delete]kullanarak Azure dev Spaces denetleyiciyi devre dışı bırakın.
1. `azds.yaml`Dosyayı projenizden kaldırın.
1. Uygulamanızı yeniden dağıtın.
1. Dağıtılan uygulamanızda Kubernetes için köprü yapılandırın. Visual Studio Code 'de Kubernetes ile Köprü kullanma hakkında daha fazla bilgi için bkz. [Kubernetes Için köprü kullanma][use-btk-vsc].
1. Yeni oluşturulan köprü ile Kubernetes başlatma profilini kullanarak Visual Studio Code hata ayıklamayı başlatın.

## <a name="team-development-in-a-shared-cluster"></a>Paylaşılan kümede takım geliştirme

Ayrıca, Kubernetes ile bir Köprüle, geliştiriciye özgü yönlendirmeyi de kullanabilirsiniz. Azure Dev Spaces takım geliştirme senaryosu, üst ve alt ad alanları kavramını kullanarak bir hizmeti uygulamanın geri kalanından yalıtmak için birden çok Kubernetes ad alanı kullanır. Kubernetes Köprüsü, gelişmiş performans özellikleriyle ve aynı uygulama ad alanı içinde aynı özelliği sunar.

Hem Kubernetes hem de Azure Dev Spaces Köprüsü, HTTP üstbilgilerinin mevcut olmasını ve uygulamanın tamamında yayılmesini gerektirir. Uygulamanızı Azure Dev Spaces için üst bilgi yaymayı işleyecek şekilde yapılandırdıysanız üstbilginin güncelleştirilmesi gerekir. Azure Dev Spaces 'den Kubernetes 'e geçiş yapmak için, yapılandırılmış üstbilgiyi *azds* 'den, *Kubernetes*'e (as) güncelleştirin.

## <a name="evaluate-bridge-to-kubernetes"></a>Kubernetes için köprüyü değerlendir

Kümenizde Azure Dev Spaces devre dışı bırakmadan önce Kubernetes Köprüsü ile denemeler yapmak isterseniz, en kolay yöntem yeni bir küme kullanmaktır. Aynı kümedeki aynı anda Kubernetes 'i Azure Dev Spaces ve Köprüsü kullanmaya çalışırsanız, her ikisinde de yönlendirme özelliklerini kullanırken sorunlarla karşılaşacaktır.

### <a name="use-visual-studio-to-evaluate-bridge-to-kubernetes"></a>Kubernetes köprüsünü değerlendirmek için Visual Studio 'Yu kullanma

1. Visual Studio IDE 'nizi 16,7 veya üzeri bir sürüme güncelleştirin ve köprüyü [Visual Studio Market][vs-marketplace]Kubernetes uzantısına yüklersiniz.
1. Yeni bir AKS kümesi oluşturun ve uygulamanızı dağıtın. [Örnek bir uygulama][btk-sample-app]da kullanabilirsiniz.
1. Dağıtılan uygulamanızda Kubernetes için köprü yapılandırın. Visual Studio 'da Kubernetes ile Köprü kullanma hakkında daha fazla bilgi için bkz. [Kubernetes Için köprü kullanma][use-btk-vs].
1. Yeni oluşturulan köprü ile Kubernetes hata ayıklama profilini kullanarak Visual Studio 'da hata ayıklamayı başlatın.

### <a name="use-visual-studio-code-to-evaluate-bridge-to-kubernetes"></a>Kubernetes köprüsünü değerlendirmek için Visual Studio Code kullanın

1. [Kubernetes uzantısına köprü][vsc-marketplace]yükler.
1. Yeni bir AKS kümesi oluşturun ve uygulamanızı dağıtın. [Örnek bir uygulama][btk-sample-app]da kullanabilirsiniz.
1. Dağıtılan uygulamanızda Kubernetes için köprü yapılandırın. Visual Studio Code 'de Kubernetes ile Köprü kullanma hakkında daha fazla bilgi için bkz. [Kubernetes Için köprü kullanma][use-btk-vsc].
1. Yeni oluşturulan köprüyü Kubernetes başlatma profiline kullanarak Visual Studio 'da hata ayıklamayı başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes köprüsünün nasıl çalıştığı hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Kubernetes Köprüsü nasıl çalışacaktır?][how-it-works-bridge-to-kubernetes]


[azds-delete]: how-to/install-dev-spaces.md#remove-azure-dev-spaces-using-the-cli
[kubernetes-extension]: https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools
[btk-sample-app]: /visualstudio/containers/bridge-to-kubernetes?view=vs-2019#install-the-sample-application
[how-it-works-bridge-to-kubernetes]: /visualstudio/containers/overview-bridge-to-kubernetes
[use-btk-vs]: /visualstudio/containers/bridge-to-kubernetes?view=vs-2019#connect-to-your-cluster-and-debug-a-service
[use-btk-vsc]: https://code.visualstudio.com/docs/containers/bridge-to-kubernetes
[vs]: https://visualstudio.microsoft.com/
[vsc-marketplace]: https://marketplace.visualstudio.com/items?itemName=mindaro.mindaro
[vs-marketplace]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.mindaro
[vsc]: https://code.visualstudio.com/