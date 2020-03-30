---
title: Azure Dev Spaces nasıl çalışır?
services: azure-dev-spaces
ms.date: 03/24/2020
ms.topic: conceptual
description: Azure Dev Spaces'e güç veren süreçleri açıklar
keywords: Azure Dev Alanlar, Dev Alanlar, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Servisi, konteynerler
ms.openlocfilehash: 99b0b3309d115b450bfca94871b6defd885349fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80234979"
---
# <a name="how-azure-dev-spaces-works"></a>Azure Dev Spaces nasıl çalışır?

Bir Kubernetes uygulaması geliştirmek zor olabilir. Docker ve Kubernetes yapılandırma dosyalarına ihtiyacınız var. Uygulamanızı yerel olarak nasıl sınayabileceğinizi ve diğer bağımlı hizmetlerle nasıl etkileşimde bulunduğunuzu bulmanız gerekir. Birden çok hizmeti aynı anda ve bir geliştirici ekibiyle aynı anda geliştirme ve sınama işlemlerini ele almanız gerekebilir.

Azure Geliştirme Alanları, Kubernetes uygulamalarını hızla tekrarlayıp hata ayıklamanız ve ekibinizle işbirliği yapmak için birden çok yol sağlar. Bu makalede, Azure Geliştirme Alanları'nın neler yapabileceği ve nasıl çalıştığı açıklanmaktadır.

## <a name="rapidly-iterate-and-debug-your-kubernetes-application"></a>Kubernetes uygulamanızı hızla yineleyin ve hata ayıklanın

Azure Dev Spaces, AKS kümeniz bağlamında Kubernetes uygulamanızı geliştirme, test etme ve yineleme çabasını azaltır. Bu çaba azaltma geliştiricilerin uygulamalarının iş mantığına odaklanmalarını ve hizmetlerini Kubernetes'te çalışacak şekilde yapılandırmamalarını sağlar.

### <a name="connect-your-development-machine-to-aks"></a>Geliştirme makinenizi AKS'ye bağlayın

Azure Dev Spaces ile geliştirme bilgisayarınızı AKS kümenize bağlayarak geliştirme bilgisayarınızda kümeüzerinde çalışıyormuş gibi kod çalıştırmanızı ve hata ayıklamanızı sağlayabilirsiniz. Azure Dev Spaces, geliştirme makineniz ile küme arasındaki trafiği yeniden yönlendirmek için uzak bir aracı görevi gören bir bölmeyi kümenizde çalıştırarak bağlı AKS kümeniz arasındaki trafiği yeniden yönlendirir. Bu trafik yeniden yönlendirmesi, geliştirme bilgisayarınızdaki kodun ve AKS kümenizde çalışan hizmetlerin aynı AKS kümesindeymüş gibi iletişim kurmasına olanak tanır. Geliştirme makinenizi AKS'ye bağlama hakkında daha fazla bilgi için [geliştirme bilgisayarınızı AKS kümenize bağlamanın nasıl çalıştığını][how-it-works-connect]görün.

### <a name="run-your-code-in-aks"></a>Kodunuzu AKS'de çalıştırın

Azure Geliştirme Alanları ile geliştirme bilgisayarınızla AKS kümeniz arasındaki trafiği yeniden yönlendirmenin yanı sıra kodunuzu doğrudan AKS'de yapılandırabilir ve hızlı bir şekilde çalıştırabilirsiniz. Visual Studio, Visual Studio Code veya Azure Dev Spaces CLI ile Azure Dev alanları kodunuzu kümelemek için yükler ve ardından oluşturur ve çalıştırır. Azure Dev alanları, kod değişikliklerini akıllıca eşitleyebilir ve değişiklikleri gerektiği gibi yansıtacak şekilde hizmetinizi yeniden başlatabilir. Kodunuzu çalıştırırken, yapı günlükleri ve HTTP izleri istemcinize geri aktarılır, böylece ilerlemeyi izleyebilir ve sorunları tanılayabilirsiniz. Visual Studio ve Visual Studio Code'daki hata ayıklayıcıyı Java, Node.js ve .NET Core hizmetlerine eklemek için Azure Dev Spaces'i de kullanabilirsiniz. Daha fazla bilgi için Azure [Dev Spaces için proje hazırlamanın nasıl çalıştığını,][how-it-works-prep]Azure Dev Spaces ile [kodunuzu çalıştırmanın nasıl çalıştığını][how-it-works-up]ve Azure Dev Spaces ile [kodunuzu uzaktan hata ayıklamanın nasıl çalıştığını][how-it-works-remote-debugging]öğrenin.

## <a name="team-development"></a>Takım geliştirme

Azure Geliştirme Alanları, ekiplerin uygulamaları üzerinde aynı AKS kümesiüzerinde kesintiye uğramadan verimli bir şekilde çalışmalarına yardımcı olur.

### <a name="intelligent-routing-between-dev-spaces"></a>Dev boşluklar arasında akıllı yönlendirme

Azure Dev Spaces ile bir takım bulut yerel bir uygulama çalıştıran tek bir AKS kümesini paylaşabilir ve takımın diğer geliştirme alanlarına müdahale etmeden geliştirebileceği, test edebileceği ve hata ayıklayabileceği yalıtılmış dev alanları oluşturabilir. Uygulamanın temel sürümü kök geliştirme alanında çalışır. Ekip üyeleri daha sonra, uygulamadaki geliştirme, test ve hata ayıklama değişiklikleri için kök alanını temel alana göre bağımsız alt geliştirme alanları oluşturur. Dev Spaces'teki yönlendirme özellikleri sayesinde, alt geliştirme alanları isteklerini alt geliştirme alanında çalışan hizmetler ile üst dev alanı arasında yönlendirebilir. Bu yönlendirme, geliştiricilerin üst alandaki bağımlı hizmetleri yeniden kullanırken bir hizmetin kendi sürümünü çalıştırmasına olanak tanır. Her alt alanın, işbirliği için başkaları tarafından paylaşılabilen ve erişilebilen benzersiz bir URL'si vardır. Azure Dev Spaces'te yönlendirmenin nasıl çalıştığı hakkında daha fazla bilgi için Azure [Dev Spaces'te yönlendirmenin nasıl çalıştığını][how-it-works-routing]öğrenin.

### <a name="live-testing-an-open-pull-request"></a>Açık çekme isteğini canlı test etme

Azure Dev Alanları ile GitHub Eylemleri'ni, birleştirmeden önce doğrudan kümenizde çekme isteğinde uygulamanızda yapılan değişiklikleri test etmek için de kullanabilirsiniz. Azure Dev Spaces, uygulamanın gözden geçirici sürümünü kümenize otomatik olarak dağıtarak yazarın ve diğer ekip üyelerinin tüm uygulama bağlamında değişiklikleri gözden geçirmesine olanak tanır. Azure Dev Spaces yönlendirme özelliklerini kullanarak, uygulamanın bu inceleme sürümü diğer geliştirme alanlarını etkilemeden kümenize de dağıtılır. Tüm bu özellikler çekme isteklerini güvenle onaylamanızı ve birleştirmenizi sağlar. GitHub Eylemleri ve Azure Dev Alanları örneğini görmek için, [Azure Kubernetes Hizmeti & GitHub Eylemleri'ne][pr-flow]bakın.

## <a name="next-steps"></a>Sonraki adımlar

Yerel geliştirme makinenizi AKS kümenize bağlamaya başlamak [için][connect]bkz.

Ekip geliştirme için Azure Geliştirme Alanları'nı kullanmaya başlamak için [Azure Dev Spaces'te ekip geliştirmeye][quickstart-team] bakın.

[connect]: how-to/connect.md
[how-it-works-connect]: how-dev-spaces-works-connect.md
[how-it-works-prep]: how-dev-spaces-works-prep.md
[how-it-works-remote-debugging]: how-dev-spaces-works-remote-debugging.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[how-it-works-up]: how-dev-spaces-works-up.md
[pr-flow]: how-to/github-actions.md
[quickstart-team]: quickstart-team-development.md
[routing]: #team-development