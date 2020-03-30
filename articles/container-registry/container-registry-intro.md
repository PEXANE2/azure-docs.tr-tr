---
title: Yönetilen konteyner kayıtları
description: Bulut tabanlı, yönetilen, özel Docker kayıt defterleri sağlayan Azure Container Kayıt Defteri hizmetine giriş.
author: stevelas
ms.topic: overview
ms.date: 02/10/2020
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: 1992a2a63d16a955d136459f5dbaece7df815c71
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77132023"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Azure'da özel Docker kapsayıcısı kayıt defterlerine giriş

Azure Konteyner Kayıt Defteri, açık kaynak Docker Registry 2.0'ı temel alan yönetilen, özel bir Docker kayıt hizmetidir. Özel Docker kapsayıcı resimlerinizi ve ilgili yapılarınızı depolamak ve yönetmek için Azure kapsayıcı kayıtları oluşturun ve koruyun.

Mevcut kapsayıcı geliştirme ve dağıtım ardışık hatlarınızla Azure kapsayıcı kayıt defterlerini kullanın veya Azure'da kapsayıcı resimleri oluşturmak için Azure Konteyner Kayıt Defteri Görevleri'ni kullanın. İsteğe bağlı olarak oluşturun veya kaynak kodu taahhütleri ve temel görüntü güncelleştirmeleri gibi tetikleyicilerle yapıyı tamamen otomatikleştirin.

Docker ve kayıt defteri kavramları hakkında daha fazla bilgi için [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/) ve [kayıt defterleri, depolar ve resimler hakkında](container-registry-concepts.md)bilgi bakın.

## <a name="use-cases"></a>Uygulama alanları

Azure kapsayıcısı kayıt defterinden çeşitli dağıtım hedeflerine görüntü çekme:

* [Kubernetes](https://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/) ve [Docker Swarm](https://docs.docker.com/swarm/) dahil olmak üzere konak kümeleri arasında kapsayıcı haline getirilmiş uygulamaları yöneten **ölçeklenebilir düzenleme sistemleri**.
* [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) ve diğerleri gibi uygun ölçekte uygulama oluşturulmasını ve çalıştırılmasını destekleyen **Azure hizmetleri**.

Geliştiriciler bir kapsayıcı geliştirme iş akışı kapsamında bir kapsayıcı kayıt defterine de öğe itebilir. Örneğin, bir konteyner kayıt defterini [Azure Pipelines](/azure/devops/pipelines/ecosystems/containers/acr-template) veya [Jenkins](https://jenkins.io/)gibi sürekli bir tümleştirme ve teslim aracından hedefleyin.

Temel görüntüleri güncelleştirildiğinde uygulama görüntülerini otomatik olarak yeniden oluşturmak için ACR Görevlerini yapılandırın veya takımınız bir Git deposuna kod işlediğinde görüntü oluşturmayı otomatikleştirin. Bulutta paralel olarak birden çok kapsayıcı görüntüsünü otomatikleştirmek, test etmek ve yamalamak için çok aşamalı görevler oluşturun.

Azure, Azure konteyner kayıt defterlerinizi yönetmek için Azure Komut Satırı Arabirimi, Azure portalı ve API desteği gibi araçlar sağlar. Azure konteyner kayıtlarınızla çalışmak için Visual [Studio Kodu için Docker Uzantısı'nı](https://code.visualstudio.com/docs/azure/docker) ve Azure [Hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısını isteğe bağlı olarak yükleyin. Görüntüleri bir Azure kapsayıcı kayıt defterine çekin ve itin veya Tümü Visual Studio Kodu'nda bulunan ACR Görevleri'ni çalıştırın.

## <a name="key-features"></a>Önemli özellikler

* **Kayıt Defteri SKU'ları** - Azure aboneliğinizde bir veya daha fazla kapsayıcı kayıt defteri oluşturun. Kayıt defterleri, her biri webhook tümleştirmesini, Azure Active Directory ile kayıt defteri kimlik doğrulamasını ve silme işlevlerini destekleyen üç SKU'da kullanılabilir: [Temel, Standart ve Premium.](container-registry-skus.md) Kapsayıcı görüntülerinizin yerel, kapalı bir ağda depolanmasının avantajlarından yararlanmak için dağıtımlarınızla aynı Azure konumunda bir kayıt defteri oluşturun. Gelişmiş çoğaltma ve kapsayıcı görüntüsü dağıtma senaryoları için Premium kayıt defterlerinin [coğrafi çoğaltma](container-registry-geo-replication.md) özelliğini kullanın. 

* **Güvenlik ve erişim** - Azure CLI veya standart `docker login` komutu kullanarak bir kayıt defterine giriş yapın. Azure Kapsayıcı Kayıt Defteri kapsayıcı görüntülerini HTTPS üzerinden aktarıyor ve istemci bağlantılarını güvence altına almak için TLS'yi destekler. 

  > [!IMPORTANT]
  > Azure Konteyner Kayıt Defteri, 13 Ocak 2020'den itibaren sunuculardan ve uygulamalardan tls 1.2 kullanmak için tüm güvenli bağlantıları gerektirir. TLS 1.2'yi yeni bir docker istemcisi (sürüm 18.03.0 veya sonrası) kullanarak etkinleştirin. TLS 1.0 ve 1.1 desteği emekli yedirilecektir. 

  Azure kimliğini, Azure Etkin Dizin destekli [hizmet yöneticisini](../active-directory/develop/app-objects-and-service-principals.md)veya sağlanan bir yönetici hesabını kullanarak konteyner kayıt defterine [erişimi denetlersiniz.](container-registry-authentication.md) Kullanıcıları veya sistemleri bir kayıt defterine ince taneli izinler atamak için rol tabanlı erişim denetimini (RBAC) kullanın.

  Premium SKU'nun güvenlik özellikleri arasında görüntü etiketi imzalama için [içerik güveni](container-registry-content-trust.md) ve kayıt defterine erişimi kısıtlamak için güvenlik duvarları ve sanal [ağlar (önizleme)](container-registry-vnet.md) yer alıyor. Azure Güvenlik Merkezi, bir resim kayıt defterine itildiğinde [görüntüleri tarayabilmek](../security-center/azure-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json) için isteğe bağlı olarak Azure Kapsayıcı Kayıt Defteri ile tümleşir.

* **Desteklenen görüntüler ve yapılar** - Bir depoda gruplandırılan her görüntü, Docker uyumlu bir kapsayıcının salt okunur görüntüsüdür. Azure kapsayıcısı kayıt defterleri hem Windows hem de Linux görüntüleri içerebilir. Tüm kapsayıcı dağıtımlarınız için görüntü adlarını siz denetlersiniz. Bir depoya görüntü itmek ya da bir depodan görüntü çekmek için standart [Docker komutlarını](https://docs.docker.com/engine/reference/commandline/) kullanın. Azure Konteyner Kayıt Defteri, Docker kapsayıcı resimlerine ek olarak, Açık Kapsayıcı [Girişimi (OCI) Görüntü Biçimi Belirtimi'nde](https://github.com/opencontainers/image-spec/blob/master/spec.md)oluşturulmuş [Miğfer grafikleri](container-registry-helm-repos.md) ve resimler gibi [ilgili içerik biçimlerini](container-registry-image-formats.md) depolar.

* **Otomatik görüntü oluşturma -** [Azure'da](container-registry-tasks-overview.md) görüntüleri oluşturma, test etme, zorlama ve dağıtmayı kolaylaştırmak için Azure Kapsayıcı Kayıt Defteri Görevlerini (ACR Görevleri) kullanın. Örneğin, işlemleri Azure'a boşaltarak `docker build` geliştirme iç döngünüzün buluta genişletilmesi için ACR Görevleri'ni kullanın. Kapsayıcı işletim sisteminizi ve çerçeve düzeltme eki uygulama işlem hattınızı otomatikleştirmek ve ekibiniz kaynak denetiminde kod yürüttüğünde otomatik olarak görüntü oluşturmak için oluşturma görevleri yapılandırın.

  [Çok adımlı görevler,](container-registry-tasks-overview.md#multi-step-tasks) bulutta kapsayıcı görüntüleri oluşturmak, test etmek ve yamalama için adım tabanlı görev tanımı ve yürütme sağlar. Görev adımları, tek tek kapsayıcı derleme ve gönderme işlemlerini tanımlar. Ayrıca, her adımın kapsayıcıyı kendi yürütme ortamı olarak kullanmasıyla bir veya daha fazla kapsayıcının yürütülmesini de tanımlayabilirler.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure portalını kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-portal.md)
* [Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma](container-registry-get-started-azure-cli.md)
* [ACR Görevleri ile konteyner yapılarını ve bakımlarını otomatikleştirin](container-registry-tasks-overview.md)