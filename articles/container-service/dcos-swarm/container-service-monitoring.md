---
title: Kullanım DıŞı Azure DC/OS kümesini izleme-Dataköpek
description: Veri Köbir Azure Container Service kümesini izleyin. Dataköpek aracılarını kümenize dağıtmak için DC/OS Web Kullanıcı arabirimini kullanın.
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: fcb005e39f89298b35bf0f3a0ad1e19601ae4d13
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82166162"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Kullanım DıŞı Dataköpek ile Azure Container Service DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Container Service kümenizdeki tüm aracı düğümlerine Dataköpek aracılarını dağıtacağız. Bu yapılandırma için veri Köbir hesaba ihtiyacınız olacaktır. 

## <a name="prerequisites"></a>Önkoşullar
Azure Container Service tarafından yapılandırılmış bir kümeyi [dağıtın](container-service-deployment.md) ve [bağlayın](../container-service-connect.md). [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. [https://datadoghq.com](https://datadoghq.com)Bir Dataköpek hesabı kurmak için bölümüne gidin. 

## <a name="datadog"></a>Datadog
Dataköpek, Azure Container Service kümenizin içindeki kapsayıcılarınızdaki izleme verilerini toplayan bir izleme hizmetidir. Dataköpek, kapsayıcılarınızın içindeki belirli ölçümleri görebileceğiniz bir Docker tümleştirme panosuna sahiptir. Kapsayıcılardan toplanan ölçümler CPU, bellek, ağ ve g/ç tarafından düzenlenir. Dataköpek ölçümleri kapsayıcılara ve görüntülere böler. Kullanıcı arabiriminin CPU kullanımı gibi görünme hakkında bir örnek aşağıda verilmiştir.

![Dataköpek Kullanıcı arabirimi](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Marathon ile bir Dataköpek dağıtımı yapılandırma
Bu adımlar, Marathon ile Dataköpek uygulamalarının nasıl yapılandırılacağını ve kümenize dağıtılacağını gösterir. 

Aracılığıyla DC/OS Kullanıcı arabirimine erişin `http://localhost:80/` . DC/OS Kullanıcı arabiriminde, sol alt kısımdaki "Universe" adresine gidin ve "Dataköpek" araması yapın ve "Install" a tıklayın.

![DC/OS Universe içindeki dataköpek paketi](./media/container-service-monitoring/datadog1.png)

Yapılandırmayı tamamlamaya yönelik olarak bir Dataköpek hesabı veya ücretsiz bir deneme hesabı gerekir. Dataköpek Web sitesinde oturum açtıktan sonra, sola gidin ve tümleştirmeler-> ardından [API 'ler](https://app.datadoghq.com/account/settings#api)' e gidin. 

![Dataköpek API anahtarı](./media/container-service-monitoring/datadog2.png)

Ardından, DC/OS Universe içindeki Dataköpek yapılandırmasına API anahtarınızı girin. 

![DC/OS Universe 'de dataköpek yapılandırması](./media/container-service-monitoring/datadog3.png) 

Yukarıdaki yapılandırma örneklerinde, kümeye yeni bir düğüm eklendiğinde, bu düğüme otomatik olarak bir aracı dağıtılacağı için 10000000 olarak ayarlanacaktır. Bu, geçici bir çözümdür. Paketi yükledikten sonra, Dataköpek Web sitesine geri gitmeniz ve "[panolar](https://app.datadoghq.com/dash/list)" bulmalısınız. Buradan, özel ve tümleştirme panoları görürsünüz. [Docker panosu](https://app.datadoghq.com/screen/integration/docker) , kümenizi izlemek için ihtiyaç duyduğunuz tüm kapsayıcı ölçümlerine sahip olacaktır. 

