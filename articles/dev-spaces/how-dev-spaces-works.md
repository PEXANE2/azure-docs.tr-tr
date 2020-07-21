---
title: Azure Dev Spaces nasıl çalışır?
services: azure-dev-spaces
ms.date: 06/02/2020
ms.topic: conceptual
description: Güç Azure Dev Spaces olan süreçler açıklanmaktadır
keywords: Azure Dev Spaces, dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar
ms.openlocfilehash: 536918e1b1db88dcae6985b853b3cb0a6c91a52a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86522287"
---
# <a name="how-azure-dev-spaces-works"></a>Azure Dev Spaces nasıl çalışır?

Bir Kubernetes uygulamasının geliştirilmesi zor olabilir. Docker ve Kubernetes yapılandırma dosyalarına ihtiyacınız vardır. Uygulamanızı yerel olarak test etme ve diğer bağımlı hizmetlerle etkileşim kurma hakkında bilgi almanız gerekir. Birden çok hizmeti aynı anda ve bir geliştirici ekibi ile geliştirmeyi ve test etmeyi işlemeniz gerekebilir.

Azure Dev Spaces, Kubernetes uygulamalarını hızla yinelemek ve hata ayıklamanın yanı sıra ekibinizle işbirliği yapmak için birden çok yol sağlar. Bu makalede Azure Dev Spaces ne yapabilecekleri ve nasıl çalıştığı açıklanmaktadır.

## <a name="rapidly-iterate-and-debug-your-kubernetes-application"></a>Kubernetes uygulamanızda hızlıca yineleme yapın ve hata ayıklayın

Azure Dev Spaces, Kubernetes uygulamanızı geliştirme, test etme ve AKS kümeniz bağlamında yineleme çabaları azaltır. Çabadaki Bu azalma, geliştiricilerin kendi uygulamalarının iş mantığına odaklanmasını ve hizmetlerini Kubernetes 'de çalışacak şekilde yapılandırmamasını sağlar.

### <a name="local-process-with-kubernetes"></a>Kubernetes ile Yerel İşlem

Kubernetes ile yerel Işlem sayesinde, geliştirme bilgisayarınızı Kubernetes kümenize bağlanarak, geliştirme bilgisayarınızda küme üzerinde çalışıyor gibi kod çalıştırmanıza ve hata ayıklamanıza olanak sağlayabilirsiniz. Azure Dev Spaces, kuruluşunuzda geliştirme bilgisayarınız ve küme arasında trafiği yeniden yönlendirmek için uzak bir aracı görevi gören bir pod çalıştırarak bağlı kümeniz arasında trafiği yeniden yönlendirir. Bu trafik yeniden yönlendirme, kümenizde çalışan geliştirme bilgisayarınızdaki ve hizmetinizdeki kodların aynı kümelemiş gibi iletişim kurmasına olanak tanır. Geliştirme bilgisayarınızı bir Kubernetes kümesine bağlama hakkında daha fazla bilgi için, [Kubernetes Ile yerel Işlemin nasıl çalıştığı hakkında][how-it-works-local-process-kubernetes]daha fazla bilgi için bkz..

### <a name="run-your-code-in-aks"></a>Kodunuzu AKS 'de çalıştırın

Geliştirme bilgisayarınız ile AKS kümeniz arasında trafiği yeniden yönlendirmeye ek olarak, Azure Dev Spaces, kodunuzu doğrudan AKS içinde yapılandırabilir ve hızlı bir şekilde çalıştırabilirsiniz. Visual Studio, Visual Studio Code veya Azure Dev Spaces CLı ile Azure dev Spaces, kodunuzu kümenize yükleyecek, sonra derleyip çalıştıracak. Azure dev Spaces Ayrıca kod değişikliklerini açabilir ve gerekirse değişiklikleri yansıtacak şekilde hizmetinizi yeniden başlatabilir. Kodunuzu çalıştırırken, derleme günlükleri ve HTTP izlemeleri, ilerleme durumunu izleyip sorunları tanılamanıza olanak tanıyacak şekilde istemcisine geri akışı sağlar. Visual Studio 'da hata ayıklayıcıyı eklemek ve Java, Node.js ve .NET Core hizmetlerine Visual Studio Code Azure Dev Spaces de kullanabilirsiniz. Daha fazla bilgi için bkz. [bir projenin Azure dev Spaces nasıl][how-it-works-prep]çalıştığı, [kodunuzun Azure dev Spaces nasıl çalıştığı][how-it-works-up]ve [Azure dev Spaces ile kodunuzda uzaktan hata ayıklamanın][how-it-works-remote-debugging]nasıl çalıştığı.

## <a name="team-development"></a>Takım geliştirme

Azure Dev Spaces, takımların, uygulamaları kesintiye uğramadan aynı AKS kümesi üzerinde verimli bir şekilde çalışmasına yardımcı olur.

### <a name="intelligent-routing-between-dev-spaces"></a>Geliştirme alanları arasında akıllı yönlendirme

Azure Dev Spaces, bir takım bulut Yerel uygulaması çalıştıran tek bir AKS kümesini paylaşabilir ve takımın diğer geliştirme alanlarını etkilemeden, test ve hata ayıklayabilecekleri yalıtılmış dev alanları oluşturabilir. Uygulamanın temel bir sürümü kök dev alanında çalışır. Daha sonra ekip üyeleri, uygulamada geliştirme, test ve hata ayıklama değişiklikleri için kök alana göre bağımsız alt geliştirme alanları oluşturur. Geliştirme alanları 'ndaki yönlendirme özellikleri sayesinde, alt dev alanları, istekleri alt dev alanı ve üst dev alanı üzerinde çalışan hizmetler arasında yönlendirebilir. Bu yönlendirme, geliştiricilerin ana alandan bağımlı hizmetleri yeniden kullanırken bir hizmetin kendi sürümünü çalıştırmasına olanak tanır. Her alt alan kendi benzersiz URL 'sine sahiptir ve bu, başkaları tarafından işbirliği için paylaşılabilir ve erişilebilir. Yönlendirmenin Azure Dev Spaces nasıl çalıştığı hakkında daha fazla bilgi için, bkz. [yönlendirmenin Azure dev Spaces nasıl çalıştığı][how-it-works-routing].

### <a name="live-testing-an-open-pull-request"></a>Açık çekme isteğini canlı olarak test etme

Ayrıca, birleştirme işleminden önce doğrudan kümenizdeki bir çekme isteğinde uygulamanızdaki değişiklikleri test etmek için Azure Dev Spaces ile GitHub eylemleri de kullanabilirsiniz. Azure Dev Spaces, uygulamanın bir gözden geçirme sürümünü kümenize otomatik olarak dağıtabilir, bu da yazarın ve diğer takım üyelerinin tüm uygulamanın bağlamındaki değişiklikleri gözden geçirmesine izin verir. Azure Dev Spaces yönlendirme yeteneklerini kullanarak, uygulamanın bu gözden geçirme sürümü diğer geliştirme alanlarını etkilemeden kümenize de dağıtılır. Bu yeteneklerin hepsi, çekme isteklerini güvenle onaylayıp birleştirebilmeniz için izin verir. GitHub eylemleri ve Azure Dev Spaces örneğini görmek için bkz. [GitHub eylemleri & Azure Kubernetes Service][pr-flow].

## <a name="next-steps"></a>Sonraki adımlar

Yerel geliştirme bilgisayarınızı AKS kümenize bağlamaya başlamak için bkz. [geliştirme bilgisayarınızı BIR aks kümesine bağlama][connect].

Takım geliştirmesi için Azure Dev Spaces kullanmaya başlamak için Azure Dev Spaces hızlı başlangıçta [Takım geliştirme][quickstart-team] bölümüne bakın.

[connect]: how-to/local-process-kubernetes-vs-code.md
[how-it-works-local-process-kubernetes]: /visualstudio/containers/overview-local-process-kubernetes
[how-it-works-prep]: how-dev-spaces-works-prep.md
[how-it-works-remote-debugging]: how-dev-spaces-works-remote-debugging.md
[how-it-works-routing]: how-dev-spaces-works-routing.md
[how-it-works-up]: how-dev-spaces-works-up.md
[pr-flow]: how-to/github-actions.md
[quickstart-team]: quickstart-team-development.md
[routing]: #team-development