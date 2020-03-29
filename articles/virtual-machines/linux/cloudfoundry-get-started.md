---
title: Microsoft Azure'da Bulut Dökümhanesi ile Başlarken
description: Microsoft Azure'da OSS veya Pivotal Cloud Foundry'yi çalıştırma
author: seanmck
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.date: 01/19/2017
ms.author: seanmck
ms.openlocfilehash: ba0b4a636d8f785f1c169e4066c9cf6d25ba81db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970043"
---
# <a name="cloud-foundry-on-azure"></a>Azure’da Cloud Foundry

Cloud Foundry, çeşitli dillerde ve çerçevelerde geliştirilen 12 faktör uygulamaları oluşturmaya, dağıtmaya ve işletmeye yönelik açık kaynak bir hizmet olarak platformdur (PaaS). Bu belge, Azure'da Bulut Dökümhanesi'ni çalıştırma seçeneklerini ve nasıl başolabileceğinizi açıklar.

## <a name="cloud-foundry-offerings"></a>Bulut Döküm teklifleri

Azure'da çalıştırılabilen iki Bulut Dökümhanesi türü vardır: açık kaynak Cloud Foundry (OSS CF) ve Pivotal Cloud Foundry (PCF). OSS CF, Cloud Foundry Foundation tarafından yönetilen Cloud Foundry'nin tamamen [açık kaynak](https://github.com/cloudfoundry) kodlu bir sürümüdür. Pivotal Cloud Foundry, Pivotal Software Inc.'in Cloud Foundry şirketinin kurumsal dağıtımıdır. Biz iki teklifleri arasındaki bazı farklar bakmak.

### <a name="open-source-cloud-foundry"></a>Açık kaynak Cloud Foundry

GitHub'da sağlanan yönergeleri kullanarak önce bir BOSH yönetmenini dağıtArak ve ardından Cloud Foundry'yi dağıtarak OSS Cloud Foundry'yi [Azure'da](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md)dağıtabilirsiniz. OSS CF'yi kullanma hakkında daha fazla bilgi edinmek için Cloud Foundry Foundation tarafından sağlanan [belgelere](https://docs.cloudfoundry.org/) bakın.

Microsoft, OSS CF için aşağıdaki topluluk kanalları aracılığıyla en iyi çabayı sağlar:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slack"></a>[Bulut Döküm Bolluk](https://slack.cloudfoundry.org/) bosh-azure-cpi kanal
- [cf-bosh posta listesi](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- [TÜFE](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) ve [servis komisyoncusu](https://github.com/Azure/meta-azure-service-broker/issues) için GitHub sorunları

>[!NOTE]
> Cloud Foundry'yi çalıştırdığınız sanal makineler gibi Azure kaynaklarınız için destek düzeyi, Azure destek sözleşmenize bağlıdır. En iyi çaba topluluğunun desteği yalnızca Bulut Dökümhanesine özgü bileşenler için geçerlidir.

### <a name="pivotal-cloud-foundry"></a>Pivotal Bulut Dökümhanesi

Pivotal Cloud Foundry, OSS dağıtımıyla aynı temel platformun yanı sıra bir dizi özel yönetim aracı ve kurumsal desteği içerir. AZURE'da PCF çalıştırmak için Pivotal'dan bir lisans almanız gerekir. Azure pazar yerinde sunulan PCF teklifinde 90 günlük deneme lisansı bulunmaktadır.

Araçlar arasında, Bulut Dökümhanesi temelinin dağıtımını ve yönetimini kolaylaştıran bir web uygulaması olan [Pivotal Operations Manager](https://docs.pivotal.io/pivotalcf/customizing/)ve kullanıcıları ve uygulamaları yönetmek için bir web uygulaması olan [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/)yer almaktadır.

Yukarıda OSS CF için listelenen destek kanallarına ek olarak, BIR PCF lisansı destek için Pivotal ile iletişim kurma hakkı verir. Microsoft ve Pivotal, yardım için taraflardan herhangi biriyle iletişim kurmanızı ve sorunun nerede olduğuna bağlı olarak sorunuuygun şekilde yönlendiremenizi sağlayan destek iş akışlarını da etkinleştirdi.

## <a name="azure-service-broker"></a>Azure Servis Aracısı

Cloud Foundry, devletsiz uygulama süreçlerinin ve devlet destekli destek hizmetlerinin temiz bir şekilde ayrılmasını teşvik eden ["on iki faktörlü uygulama"](https://12factor.net/) metodolojisini teşvik eder. [Servis brokerları,](https://docs.cloudfoundry.org/services/api.html) destek hizmetlerini uygulamalara sağlamak ve bağlamak için tutarlı bir yol sunar. [Azure servis aracısı,](https://github.com/Azure/meta-azure-service-broker) Azure depolama ve Azure SQL gibi önemli Azure hizmetlerinden bazılarını bu kanal aracılığıyla sağlar.

Pivotal Cloud Foundry kullanıyorsanız, servis aracısı Pivotal Network'ten bir döşeme olarak da [kullanılabilir.](https://docs.pivotal.io/azure-sb/installing.html)

## <a name="related-resources"></a>İlgili kaynaklar

### <a name="azure-devops-services-plugin"></a>Azure DevOps Hizmetleri eklentisi

Cloud Foundry, sürekli tümleştirme (CI) ve sürekli teslimat (CD) kullanımı da dahil olmak üzere çevik yazılım geliştirme için uygundur. Projelerinizi yönetmek için Azure DevOps Hizmetlerini kullanıyorsanız ve Bulut Dökümhane'yi hedefleyen bir CI/CD ardışık hatlar oluşturmak istiyorsanız, [Azure DevOps Hizmetleri Bulut Döküm oluşturma uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension)kullanabilirsiniz. Eklenti, ister Azure'da ister başka bir ortamda çalışan Bulut Dökümhanesi'nde dağıtımları yapılandırmayı ve otomatikleştirmeyi kolaylaştırır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Marketinden Önemli Bulut Dökümhanesini Dağıtma](https://azure.microsoft.com/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)
- [Azure'da Bulut Dökümhanesi'ne uygulama dağıtma](./cloudfoundry-deploy-your-first-app.md)
