---
title: Azure Güvenlik Merkezi 'nde kapsayıcı güvenliği | Microsoft Docs
description: Azure Güvenlik Merkezi 'nin kapsayıcı güvenlik özellikleri hakkında bilgi edinin.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/11/2020
ms.author: memildin
ms.openlocfilehash: c18751d315af6da1a4b2f06aaca28c84746b7be5
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77470777"
---
# <a name="container-security-in-security-center"></a>Güvenlik Merkezi 'nde kapsayıcı güvenliği

Azure Güvenlik Merkezi, kapsayıcı güvenliği için Azure Native çözümüdür. Güvenlik Merkezi Ayrıca, bulut iş yüklerinizin, VM 'lerinin, sunucularınızın ve kapsayıcılarınızın güvenliği için en iyi cam deneyim bölmesidir.

Bu makalede Güvenlik Merkezi 'nin kapsayıcılarınızın ve uygulamalarının güvenliğini iyileştirmenize, izlemenize ve bunların güvenliğini sağlamanıza nasıl yardımcı olduğu açıklanır. Güvenlik Merkezi 'nin kapsayıcı güvenliğinin bu temel yönlerini nasıl yardımcı olduğunu öğreneceksiniz:

* Güvenlik açığı yönetimi
* Kapsayıcının ortamını sağlamlaştırma
* Çalışma zamanı koruması

[Azure Güvenlik Merkezi 'nin kapsayıcı Güvenlik sekmesini ![](media/container-security/container-security-tab.png)](media/container-security/container-security-tab.png#lightbox)

Bu özellikleri kullanma hakkında yönergeler için bkz. [kapsayıcılarınızın güvenliğini izleme](monitor-container-security.md).

## <a name="vulnerability-management---scanning-container-images-preview"></a>Güvenlik açığı yönetimi-kapsayıcı görüntülerini tarama (Önizleme)
ARM tabanlı Azure Container Registry izlemek için Güvenlik Merkezi 'nin standart katmanında olduğunuzdan emin olun (bkz. [fiyatlandırma](/azure/security-center/security-center-pricing)). Sonra isteğe bağlı kapsayıcı kayıt defterleri paketini etkinleştirin. Yeni bir görüntü gönderildiğinde, Güvenlik Merkezi, sektör lideri güvenlik açığı tarama satıcısı, Qualys 'den bir tarayıcı kullanarak görüntüyü tarar.

Sorunlar bulunduğunda: Qualys veya Güvenlik Merkezi tarafından – Güvenlik Merkezi panosunda bildirim alırsınız. Güvenlik Merkezi her güvenlik açığı için, eyleme dönüştürülebilir öneriler, bir önem sınıflandırması ve sorunun nasıl düzeltileceği ile ilgili rehberlik sağlar. Güvenlik Merkezi 'nin kapsayıcılar için önerilerin ayrıntıları için bkz. [önerilerin başvuru listesi](recommendations-reference.md#recs-containers).

## <a name="environment-hardening"></a>Ortam sağlamlaştırma

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Docker yapılandırmanızın sürekli izlenmesi
Azure Güvenlik Merkezi, IaaS Linux VM 'lerinde barındırılan yönetilmeyen kapsayıcıları veya Docker Kapsayıcıları çalıştıran diğer Linux makinelerini tanımlar. Güvenlik Merkezi, bu kapsayıcıların yapılandırmalarının sürekli değerlendirir. Daha sonra bunları [Internet güvenliği (CIS) Docker kıyaslaması Için merkezi](https://www.cisecurity.org/benchmark/docker/)ile karşılaştırır.

Güvenlik Merkezi, CIS Docker kıyaslaması 'nın kural kümesinin tamamını içerir ve kapsayıcılarınız denetimlerden herhangi birini karşılamadığı takdirde sizi uyarır. Güvenlik Merkezi, yanlış yapılandırma bulduğunda güvenlik önerileri oluşturur. Önerileri görüntülemek ve sorunları düzeltmek için **öneriler sayfasını** kullanın. Ayrıca, Docker ile dağıtılan tüm sanal makineleri görüntüleyen **kapsayıcılar** sekmesinde öneriler görürsünüz. 

Bu özellik için görünebilen ilgili güvenlik merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

Bir VM 'nin güvenlik sorunlarını araştırırken, güvenlik merkezi makinedeki kapsayıcılar hakkında ek bilgiler sağlar. Bu tür bilgiler, Docker sürümünü ve konakta çalışan görüntü sayısını içerir. 

>[!NOTE]
> Bu CIS kıyaslama denetimleri, AKS tarafından yönetilen örneklerde veya Databricks tarafından yönetilen VM 'lerde çalışmaz.

### <a name="continuous-monitoring-of-your-kubernetes-clusters-preview"></a>Kubernetes kümelerinizi sürekli izleme (Önizleme)
Güvenlik Merkezi Azure Kubernetes hizmeti (AKS) ile birlikte çalışarak Kapsayıcılı uygulamalar geliştirmek, dağıtmak ve yönetmek için Microsoft 'un yönetilen kapsayıcı düzenleme hizmetidir.

AKS, kümelerinizin güvenlik duruşuna güvenlik denetimleri ve görünürlük sağlar. Güvenlik Merkezi şu özellikleri kullanır:
* AKS kümelerinizin yapılandırmasını sürekli izleyin
* Endüstri standartlarına göre hizalanmış güvenlik önerileri oluşturun

Bu özellik için görünebilen ilgili güvenlik merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

## <a name="run-time-protection---real-time-threat-detection"></a>Çalışma zamanı koruması-gerçek zamanlı tehdit algılama

Güvenlik Merkezi, Kapsayıcılı ortamlarınız için gerçek zamanlı tehdit algılama sağlar ve şüpheli etkinlikler için uyarı oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Konuk ve AKS küme düzeyindeki tehditleri tespit ediyoruz. Tüm ayrıntılar için bkz. [Azure kapsayıcıları için tehdit algılama](https://docs.microsoft.com/azure/security-center/security-center-alerts-compute#azure-containers-).


## <a name="container-security-faq"></a>Kapsayıcı güvenliği SSS

### <a name="what-types-of-images-can-azure-security-center-scan"></a>Azure Güvenlik Merkezi tarayabilmesi gereken görüntü türleri nelerdir?
Güvenlik Merkezi, kabuk erişimi sağlayan Linux işletim sistemi tabanlı görüntüleri tarar. 

Qualys tarayıcısı, [Docker karalama](https://hub.docker.com/_/scratch/) görüntüleri gibi süper minimuz görüntüleri veya yalnızca uygulamanızı ve çalışma zamanı bağımlılıklarını (Paket Yöneticisi, kabuk veya işletim sistemi olmadan) Içeren "distroor" görüntülerini desteklemez.

### <a name="how-does-we-scan-azure-security-center-scan-an-image"></a>Azure Güvenlik Merkezi 'nin bir görüntüyü taramasını nasıl tarayıyoruz?
Görüntü, kayıt defterinden ayıklanır. Daha sonra, bilinen güvenlik açıklarının listesini ayıklayan Qualys tarayıcısı ile yalıtılmış bir korumalı alanda çalıştırılır.

### <a name="how-often-does-azure-security-center-scan-my-images"></a>Azure Güvenlik Merkezi görüntülerimi ne sıklıkla taraysın?
Yansıma taramaları her gönderim üzerinde tetiklenir.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>Tarama sonuçlarını REST API aracılığıyla alabilir miyim?
Evet. Sonuçlar [alt değerlendirmeler REST API 'si](/rest/api/securitycenter/subassessments/list/)altındadır. Ayrıca, tüm kaynaklarınız için kusto benzeri API olan Azure Kaynak Grafiği 'ni (ARG) kullanabilirsiniz: bir sorgu belirli bir taramayı getirebilir.
 

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik Merkezi 'nde kapsayıcı güvenliği hakkında daha fazla bilgi edinmek için aşağıdaki ilgili makalelere göz atın:

* Kapsayıcılarla ilgili kaynaklarınızın güvenlik duruşunu görüntülemek için, [makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md#containers)konusunun kapsayıcılar bölümüne bakın.

* [Azure Kubernetes hizmeti ile tümleştirmenin](azure-kubernetes-service-integration.md) ayrıntıları

* [Azure Container Registry ile tümleştirmenin](azure-container-registry-integration.md) ayrıntıları