---
title: Kullanım DıŞı Azure DC/OS kümesini izleme-Dataköpek
description: Veri Köbir Azure Container Service kümesini izleyin. Dataköpek aracılarını kümenize dağıtmak için DC/OS Web Kullanıcı arabirimini kullanın.
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 530092dfabacb0b07f4002a82078dd3535cd7e8f
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76275246"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Kullanım DıŞı Dataköpek ile Azure Container Service DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Container Service kümenizdeki tüm aracı düğümlerine Dataköpek aracılarını dağıtacağız. Bu yapılandırma için veri Köbir hesaba ihtiyacınız olacaktır. 

## <a name="prerequisites"></a>Ön koşullar
Azure Container Service tarafından yapılandırılmış bir kümeyi [dağıtın](container-service-deployment.md) ve [bağlayın](../container-service-connect.md). [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Bir Dataköpek hesabı ayarlamak için [https://datadoghq.com](https://datadoghq.com) gidin. 

## <a name="datadog"></a>Datadog
Dataköpek, Azure Container Service kümenizin içindeki kapsayıcılarınızdaki izleme verilerini toplayan bir izleme hizmetidir. Dataköpek, kapsayıcılarınızın içindeki belirli ölçümleri görebileceğiniz bir Docker tümleştirme panosuna sahiptir. Kapsayıcılardan toplanan ölçümler CPU, bellek, ağ ve g/ç tarafından düzenlenir. Dataköpek ölçümleri kapsayıcılara ve görüntülere böler. Kullanıcı arabiriminin CPU kullanımı gibi görünme hakkında bir örnek aşağıda verilmiştir.

![Dataköpek Kullanıcı arabirimi](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Marathon ile bir Dataköpek dağıtımı yapılandırma
Bu adımlar, Marathon ile Dataköpek uygulamalarının nasıl yapılandırılacağını ve kümenize dağıtılacağını gösterir. 

DC/OS Kullanıcı arabirimine [http://localhost:80/](http://localhost:80/)aracılığıyla erişin. DC/OS Kullanıcı arabiriminde, sol alt kısımdaki "Universe" adresine gidin ve "Dataköpek" araması yapın ve "Install" a tıklayın.

![DC/OS Universe içindeki dataköpek paketi](./media/container-service-monitoring/datadog1.png)

Yapılandırmayı tamamlamaya yönelik olarak bir Dataköpek hesabı veya ücretsiz bir deneme hesabı gerekir. Dataköpek Web sitesinde oturum açtıktan sonra, sola gidin ve tümleştirmeler-> ardından [API 'ler](https://app.datadoghq.com/account/settings#api)' e gidin. 

![Dataköpek API anahtarı](./media/container-service-monitoring/datadog2.png)

Ardından, DC/OS Universe içindeki Dataköpek yapılandırmasına API anahtarınızı girin. 

![DC/OS Universe 'de dataköpek yapılandırması](./media/container-service-monitoring/datadog3.png) 

Yukarıdaki yapılandırma örneklerinde, kümeye yeni bir düğüm eklendiğinde, bu düğüme otomatik olarak bir aracı dağıtılacağı için 10000000 olarak ayarlanacaktır. Bu, geçici bir çözümdür. Paketi yükledikten sonra, Dataköpek Web sitesine geri gitmeniz ve "[panolar](https://app.datadoghq.com/dash/list)" bulmalısınız. Buradan, özel ve tümleştirme panoları görürsünüz. [Docker panosu](https://app.datadoghq.com/screen/integration/docker) , kümenizi izlemek için ihtiyaç duyduğunuz tüm kapsayıcı ölçümlerine sahip olacaktır. 

