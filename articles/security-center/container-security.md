---
title: Azure Güvenlik Merkezinde Konteyner Güvenliği | Microsoft Dokümanlar
description: Azure Güvenlik Merkezi'nin kapsayıcı güvenlik özellikleri hakkında bilgi edinin.
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
ms.openlocfilehash: d46e2a9820ec0c45d197f135428f1ace712b2fb8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80125137"
---
# <a name="container-security-in-security-center"></a>Güvenlik Merkezinde Konteyner Güvenliği

Azure Güvenlik Merkezi, kapsayıcı güvenliği için Azure'un yerel çözümüdür. Güvenlik Merkezi aynı zamanda bulut iş yüklerinizin, VM'lerinizin, sunucularınızın ve kapsayıcılarınızın güvenliği için en uygun tek cam deneyimi bölmesidir.

Bu makalede, Güvenlik Merkezi'nin kapsayıcılarınızın ve uygulamalarının güvenliğini iyileştirmenize, izlemenize ve korumanıza nasıl yardımcı olduğu açıklanmaktadır. Güvenlik Merkezi'nin konteyner güvenliğinin bu temel yönlerine nasıl yardımcı olduğunu öğreneceksiniz:

* Güvenlik açığı yönetimi
* Konteynerin çevresinin sertleşme
* Çalışma zamanı koruması

[![Azure Güvenlik Merkezi'nin kapsayıcı güvenlik sekmesi](media/container-security/container-security-tab.png)](media/container-security/container-security-tab.png#lightbox)

Bu özelliklerin nasıl kullanılacağına ilişkin talimatlar için [bkz.](monitor-container-security.md)

## <a name="vulnerability-management---scanning-container-images"></a>Güvenlik açığı yönetimi - kapsayıcı görüntülerini tarayan
ARM tabanlı Azure Konteyner Kayıt Defterinizi izlemek için, Güvenlik Merkezi'nin standart katmanında olduğunuzdan emin olun [(fiyatlandırmaya](/azure/security-center/security-center-pricing)bakın). Ardından, isteğe bağlı Konteyner Kayıt Defterleri paketini etkinleştirin. Yeni bir görüntü itildiğinde, Güvenlik Merkezi, sektör lideri güvenlik açığı tarama satıcısı Qualys'ten gelen bir tarayıcı kullanarak görüntüyü tarar.

Qualys veya Güvenlik Merkezi tarafından sorunlar bulunduğunda, Güvenlik Merkezi panosunda size bildirilir. Güvenlik Merkezi, her güvenlik açığı için önem derecesi sınıflandırmasının yanı sıra eyleme geçirilebilir öneriler ve sorunu nasıl düzelteceklerine ilişkin kılavuz sağlar. Güvenlik Merkezi'nin kapsayıcılar için önerilerinin ayrıntıları [için, önerilerin başvuru listesine](recommendations-reference.md#recs-containers)bakın.

Güvenlik Merkezi, tarayıcıdaki bulguları filtreler ve sınıflar. Bir görüntü sağlıklı olduğunda, Güvenlik Merkezi görüntüyle işaretlenir. Güvenlik Merkezi, yalnızca çözülmesi gereken sorunları olan görüntüler için güvenlik önerileri oluşturur. Güvenlik Merkezi yalnızca sorun olduğunda bildirimde bulunarak istenmeyen bilgilendirme uyarıları nın potansiyelini azaltır.

## <a name="environment-hardening"></a>Çevre sertleştirme

### <a name="continuous-monitoring-of-your-docker-configuration"></a>Docker yapılandırmanızın sürekli izlenmesi
Azure Güvenlik Merkezi, IaaS Linux VM'lerinde barındırılan yönetilmeyen kapsayıcıları veya Docker kapsayıcılarını çalıştıran diğer Linux makinelerini tanımlar. Güvenlik Merkezi bu kapsayıcıların yapılandırmalarını sürekli olarak değerlendirir. Daha sonra [Internet Security (BDT) Docker Benchmark merkezi](https://www.cisecurity.org/benchmark/docker/)ile karşılaştırır.

Güvenlik Merkezi, BDT Docker Benchmark'ın tüm kural kümesini içerir ve kapsayıcılarınız denetimlerin hiçbirini karşılamazsa sizi uyarır. Yanlış yapılandırmalar bulduğunda, Güvenlik Merkezi güvenlik önerileri oluşturur. Önerileri görüntülemek ve sorunları düzeltmek için **öneriler sayfasını** kullanın. Ayrıca, Docker ile dağıtılan tüm sanal makineleri görüntüleyen **Kapsayıcılar** sekmesinde önerileri de görürsünüz. 

Bu özellik için görüntülenebilen ilgili Güvenlik Merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

Bir VM'nin güvenlik sorunlarını araştırırken, Güvenlik Merkezi makinedeki kapsayıcılar hakkında ek bilgiler sağlar. Bu tür bilgiler Docker sürümünü ve ana bilgisayarda çalışan görüntü sayısını içerir. 

>[!NOTE]
> Bu BDT kıyaslama denetimleri AKS tarafından yönetilen örneklerde veya Databricks tarafından yönetilen VM'lerde çalışmaz.

### <a name="continuous-monitoring-of-your-kubernetes-clusters"></a>Kubernetes kümelerinizin sürekli izlenmesi
Security Center, Microsoft'un kapsayıcı uygulamaları geliştirmek, dağıtmak ve yönetmek için yönetilen konteyner düzenleme hizmeti olan Azure Kubernetes Service (AKS) ile birlikte çalışır.

AKS, kümelerinizin güvenlik duruşuna güvenlik denetimleri ve görünürlük sağlar. Güvenlik Merkezi bu özellikleri şu şekilde kullanır:
* AKS kümelerinizin yapılandırmasını sürekli olarak izleyin
* Endüstri standartlarına uygun güvenlik önerileri oluşturma

Bu özellik için görüntülenebilen ilgili Güvenlik Merkezi önerilerinin ayrıntıları için, öneriler başvuru tablosunun [kapsayıcı bölümüne](recommendations-reference.md#recs-containers) bakın.

## <a name="run-time-protection---real-time-threat-detection"></a>Çalışma zamanı koruması - Gerçek zamanlı tehdit algılama

Güvenlik Merkezi, konteynerleştirilmiş ortamlarınız için gerçek zamanlı tehdit algılama sağlar ve şüpheli etkinlikler için uyarılar oluşturur. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

Ev sahibi ve AKS küme düzeyinde tehditleri tespit ediyoruz. Tüm ayrıntılar [için Azure kapsayıcıları için tehdit algılama](threat-protection.md#azure-containers)ya da tehdit algılama ya da


## <a name="container-security-faq"></a>Konteyner güvenliği SSS

### <a name="what-types-of-images-can-azure-security-center-scan"></a>Azure Güvenlik Merkezi ne tür görüntüleri tayabilir?
Güvenlik Merkezi, kabuk erişimi sağlayan Linux IŞLETIM Sistemi tabanlı görüntüleri tarar. 

Qualys tarayıcıdocker [kazıkazan](https://hub.docker.com/_/scratch/) görüntüleri veya yalnızca uygulamanızı ve bir paket yöneticisi, kabuk veya işletim sistemi olmadan çalışma zamanı bağımlılıkları içeren "Distroless" görüntüleri gibi süper minimalist görüntüleri desteklemez.

### <a name="how-does-azure-security-center-scan-an-image"></a>Azure Güvenlik Merkezi bir görüntüyü nasıl tazmır?
Görüntü kayıt defterinden çekilir. Daha sonra, bilinen güvenlik açıklarının bir listesini ayıklayan Qualys tarayıcısının olduğu izole bir kum havuzunda çalıştırılır.

Güvenlik Merkezi, tarayıcıdaki bulguları filtreler ve sınıflar. Bir görüntü sağlıklı olduğunda, Güvenlik Merkezi görüntüyle işaretlenir. Güvenlik Merkezi, yalnızca çözülmesi gereken sorunları olan görüntüler için güvenlik önerileri oluşturur. Güvenlik Merkezi yalnızca sorun olduğunda bildirimde bulunarak istenmeyen bilgilendirme uyarıları nın potansiyelini azaltır.

### <a name="how-often-does-azure-security-center-scan-my-images"></a>Azure Güvenlik Merkezi görüntülerimi ne sıklıkta tatıyor?
Görüntü taramaları her itme de tetiklenir.

### <a name="can-i-get-the-scan-results-via-rest-api"></a>REST API ile tarama sonuçlarını alabilir miyim?
Evet. Sonuçlar Alt [Değerlendirmeler Rest API](/rest/api/securitycenter/subassessments/list/)altındadır. Ayrıca, tüm kaynaklarınız için Kusto benzeri API olan Azure Kaynak Grafiği'ni (ARG) kullanabilirsiniz: bir sorgu belirli bir taramaya gelebilir.
 

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik Merkezi'nde kapsayıcı güvenliği hakkında daha fazla bilgi edinmek için ilgili makalelere bakın:

* Konteynerle ilgili kaynaklarınızın güvenlik duruşunu görüntülemek [için, makinelerinizi ve uygulamalarınızı koruyun kapsayıcılar bölümüne](security-center-virtual-machine-protection.md#containers)bakın.

* [Azure Kubernetes Hizmeti ile tümleştirmenin](azure-kubernetes-service-integration.md) ayrıntıları

* [Azure Kapsayıcı Kayıt Defteri ile tümleştirmenin](azure-container-registry-integration.md) ayrıntıları