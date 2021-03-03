---
title: Gilar kullanarak CI/CD Iş akışı-Azure Arc etkin Kubernetes
services: azure-arc
ms.service: azure-arc
ms.date: 02/26/2021
ms.topic: conceptual
author: tcare
ms.author: tcare
description: Bu makalede, Gilar kullanarak bir CI/CD iş akışına kavramsal bir genel bakış sunulmaktadır
keywords: Gilar, Kubernetes, K8s, Azure, Held, Arc, AKS, Azure Kubernetes hizmeti, kapsayıcılar, CI, CD, Azure DevOps
ms.openlocfilehash: 044275db0977a20474aa1451324486ad1750a7f9
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101693277"
---
# <a name="overview"></a>Genel Bakış

Modern Kubernetes dağıtımları birden çok uygulama, küme ve ortam barındırır. Gilar sayesinde, bu karmaşık kurulumları daha kolay yönetebilirsiniz ve Kubernetes ortamlarının istenen durumunu el ile git ile izleyebilirsiniz. Küme durumunu izlemek için ortak git araçları 'nı kullanarak, sorumluluğu artırabilir, hata araştırmayı kolaylaştırabilir ve ortamları yönetmek için Otomasyonu etkinleştirebilirsiniz.

Bu makalede, Azure Arc, Azure Repos ve Azure Pipelines kullanarak bir uygulamanın tam yaşam döngüsünde, Gilar 'ın bir gerçekliği sağlama hakkında kavramsal bir genel bakış sunulmaktadır. Geliştiriciden gide denetimli Kubernetes ortamlarına kadar bir uygulamadaki tek bir değişikliğin uçtan uca bir örneğini adım adım inceleyin.

## <a name="architecture"></a>Mimari

Bir veya daha fazla Kubernetes ortamına dağıtılan bir uygulamayı düşünün.

![Giüstci CI/CD mimarisi](./media/gitops-arch.png)
### <a name="application-repo"></a>Uygulama deposu
Uygulama deposu, geliştiricilerin iç döngülerinde üzerinde çalıştığı uygulama kodunu içerir. Uygulamanın dağıtım şablonları, bu depoda, Helm veya Kustomize gibi genel bir formda canlı olarak yer oluşturur. Ortama özgü değerler depolanmaz. Bu depoya yapılan değişiklikler, dağıtım işlemini başlatan bir PR veya CI işlem hattı çağırır.
### <a name="container-registry"></a>Container Registry
Kapsayıcı kayıt defteri, Kubernetes ortamlarında kullanılan tüm birinci ve üçüncü taraf görüntülerini barındırır. Resmi oluşturmak için kullanılan, insanların okunabilir etiketleriyle ve git yürütmesiyle birinci taraf uygulama görüntülerini etiketleyin. Güvenlik, hız ve esnekliği için üçüncü taraf görüntülerini önbelleğe alma. Güvenlik güncelleştirmelerinin zamanında sınanması ve tümleştirilmesi için bir plan ayarlayın. Daha fazla bilgi için bkz. bir örnek için [ACR kullanma ve ortak içeriği koruma](https://docs.microsoft.com/azure/container-registry/tasks-consume-public-content) Kılavuzu.
### <a name="pr-pipeline"></a>PR işlem hattı
PR 'ler to Application deposunda, PR işlem hattının başarılı bir şekilde çalıştırılması alınır. Bu işlem hattı, uygulama kodunda ve birim testleri gibi temel kalite kapılarını çalıştırır. İşlem hattı, bir Kubernetes ortamına dağıtım için kullanılan uygulamayı ve Lints Dockerfiles ve Held şablonlarını sınar. Docker görüntüleri oluşturulup test edilmelidir, ancak gönderilemez. Ardışık düzen süresini, Hızlı yineleme için izin verecek şekilde nispeten daha kısa tutun.
### <a name="ci-pipeline"></a>CI işlem hattı
Uygulama CI işlem hattı, tüm PR işlem hattı adımlarını çalıştırır ve test ve dağıtım denetimlerini genişletir. İşlem hattı her bir yürütme için ya da bir işleme grubu olan düzenli bir temposunda için çalıştırılabilir. Bu aşamada, bir PR işlem hattı için çok uzun olan uygulama testi gerçekleştirin. Dağıtım hazırlığı sırasında derleme sonrasında Docker görüntülerini Container Registry gönderin. Değiştirilen şablon bir test değerleri kümesiyle ayarlanabilir. Hizmet çalışma zamanında kullanılan görüntülerin bu noktada oluşturulması, oluşturulması ve test olması gerekir. CI derlemesinde, dağıtım hazırlığı sırasında kullanmak üzere CD adımındaki yapıtlar yayımlanır.
### <a name="flux"></a>Flux
Flox, her kümede çalışan ve istenen durumun korunmasından sorumlu bir hizmettir. Hizmet, büyük bir depoyu kümedeki değişiklikler için sık sık yoklar ve uygular.
### <a name="cd-pipeline"></a>CD işlem hattı
CD işlem hattı başarılı CI derlemeleri tarafından otomatik olarak tetiklenir. Önceden yayınlanan şablonları kullanır, ortam değerlerini değiştirir ve bir veya daha fazla Kubernetes kümesinin istenen durumunda bir değişiklik istemek üzere Gila deposuna bir PR açar. Küme yöneticileri durum değişikliği PR 'yi gözden geçirdikten sonra, giler deposu ile birleştirme işlemini onaylar. İşlem hattı daha sonra çekme isteğinin tamamlanmasını bekler ve bu da Flox 'in durum değişikliğini almasına olanak tanır.
### <a name="gitops-repo"></a>Gilar deposu
Gilar deposu, kümeler genelinde tüm ortamların geçerli istenen durumunu temsil eder. Bu depoya yapılan herhangi bir değişiklik, her kümedeki Flox hizmeti tarafından alınır ve dağıtılır. PR 'ler, istenen durumda, gözden geçirilmiş ve birleştirilmiş değişikliklerle oluşturulur. Bu PR 'ler hem dağıtım şablonlarına hem de elde edilen işlenen Kubernetes bildirimlerine yapılan değişiklikleri içerir. Düşük düzey işlenmiş bildirimler, şablon düzeyinde genellikle görünmeyen değişiklikler konusunda dikkatli bir denetim sağlayarak şablon değiştirme 'nin arkasındaki tüm sürprizleri önleyin.
### <a name="kubernetes-clusters"></a>Kubernetes kümeleri
Bir veya daha fazla Azure yay etkinleştirilmiş Kubernetes kümesi, uygulamanın gerektirdiği farklı ortamları sunar. Örneğin, tek bir küme, farklı ad alanları aracılığıyla hem geliştirme hem de QA ortamına hizmeti verebilir. İkinci bir küme, ortamların ve daha ayrıntılı denetimin daha kolay ayrılmasını sağlayabilir.
## <a name="example-workflow"></a>Örnek iş akışı
Uygulama geliştiricisi olarak, Gamze:
* Uygulama kodunu yazar.
* Uygulamanın bir Docker kapsayıcısında nasıl çalıştırılacağını belirler.
* Bir Kubernetes kümesinde kapsayıcıyı ve bağımlı hizmetleri çalıştıran şablonları tanımlar.

Gamze, uygulamanın birden çok ortamda çalışmasına ihtiyaç duyurken, her ortam için belirli ayarları bilmez.

Çiğdem 'in uygulama dağıtım şablonunda kullanılan Docker görüntüsünü değiştiren bir uygulama değişikliği yapmak istediğini varsayalım.

1. Çiğdem Dağıtım şablonunu değiştirir, uygulamayı uygulama deposundaki bir uzak dala gönderir ve gözden geçirme için bir PR açar.
2. Çiğdem, takımdaki değişikliği gözden geçirmesini ister.
    * PR işlem hattı doğrulamayı çalıştırır.
    * Başarılı bir işlem hattı çalıştıktan sonra, takım kapatılır ve değişiklik birleştirilir.
3. CI işlem hattı, Çiğdem 'in değişikliğini doğrular ve başarıyla tamamlanır.
    * Değişikliğin kümeye dağıtılması güvenlidir ve yapıtlar CI işlem hattı çalıştırmasına kaydedilir.
4. Çiğdem 'in değişikliği, CD işlem hattını birleştirir ve tetikler.
    * CD işlem hattı, Gamze 'nin CI işlem hattı çalıştırması tarafından depolanan yapıtları seçer.
    * CD işlem hattı, şablonları ortama özgü değerlerle değiştirir ve tüm değişiklikleri, Gilar deposunda mevcut küme durumuna göre aşamalar.
    * CD işlem hattı, küme durumunda istenen değişikliklerle birlikte Gilar deposuna bir çekme isteği oluşturur.
5. Çiğdem 'in ekibi, PR 'yi gözden geçirir ve onaylar.
    * Değişiklik, ortama karşılık gelen hedef Dalla birleştirilir.
6. Dakikalar içinde, Flox, gide depodaki bir değişikliği fark eder ve Çiğdem 'in değişikliğini çeker.
    * Docker görüntü değişikliği nedeniyle, Pod uygulaması bir güncelleştirme gerektirir.
    * Flox, değişikliği kümeye uygular.
7. Çiğdem, dağıtımın başarıyla tamamlandığını doğrulamak için uygulama uç noktasını sınar.
   > [!NOTE]
   > Dağıtıma yönelik daha fazla ortam için, CD işlem hattı sonraki ortam için bir PR oluşturup 4-7 adımlarını yineleyerek yinelenir. İşlemin, güvenlikle ilgili bir değişiklik veya üretim ortamı gibi riskier dağıtımları veya ortamları için ek onay ihtiyacı vardır.
8.  Tüm ortamlar başarılı dağıtımlar aldıktan sonra işlem hattı tamamlanır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Arc etkin Kubernetes ile yapılandırma ve Gilar](./conceptual-configurations.md)
