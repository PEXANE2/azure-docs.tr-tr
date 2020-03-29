---
title: (AmortismanA Uğradı) Azure DC/OS kümesini izleyin - Datadog
description: Datadog ile bir Azure Kapsayıcı Hizmeti kümesini izleyin. Datadog aracılarını kümenize dağıtmak için DC/OS web Kullanıcı Aracı'nı kullanın.
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 530092dfabacb0b07f4002a82078dd3535cd7e8f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76275246"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>(AmortismanA Uğradı) Datadog ile Azure Kapsayıcı Hizmeti DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Kapsayıcı Hizmeti kümenizdeki tüm aracı düğümlerine Datadog aracıları dağıtacağız. Bu yapılandırma için Datadog'da bir hesaba ihtiyacınız olacaktır. 

## <a name="prerequisites"></a>Ön koşullar
Azure Container Service tarafından yapılandırılmış bir kümeyi [dağıtın](container-service-deployment.md) ve [bağlayın](../container-service-connect.md). [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Bir [https://datadoghq.com](https://datadoghq.com) Datadog hesabı ayarlamak için gidin. 

## <a name="datadog"></a>Datadog
Datadog, Azure Kapsayıcı Hizmeti kümenizdeki kapsayıcılarınızdan izleme verilerini toplayan bir izleme hizmetidir. Datadog'da, kapsayıcılarınızda belirli ölçümleri görebileceğiniz bir Docker Entegrasyon Panosu vardır. Kapsayıcılarınızdan toplanan ölçümler CPU, Memory, Network ve I/O tarafından düzenlenir. Datadog ölçümleri kapsayıcılara ve görüntülere böler. Cpu kullanımı için UI'nin nasıl göründüğüne bir örnek aşağıdadır.

![Datadog UI](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Marathon ile Datadog dağıtımını yapılandırın
Bu adımlar, Marathon ile Datadog uygulamalarını kümenize nasıl yapılandıracağınızı ve dağıtacağınızı gösterir. 

DC/OS Kullanıcı UI'nize . [http://localhost:80/](http://localhost:80/) DC/OS Kullanıcı Arabirimi'nde sol altta bulunan "Universe"e gidin ve ardından "Datadog"u arayın ve "Yükle"ye tıklayın.

![DC/OS Universe içinde Datadog paketi](./media/container-service-monitoring/datadog1.png)

Şimdi yapılandırmayı tamamlamak için bir Datadog hesabı veya ücretsiz bir deneme hesabı gerekir. Datadog web sitesine giriş yaptıktan sonra sola bakın ve Tümleştirmeler -> sonra [API'lere](https://app.datadoghq.com/account/settings#api)gidin. 

![Datadog API anahtarı](./media/container-service-monitoring/datadog2.png)

Ardından API anahtarınızı DC/OS Universe içindeki Datadog yapılandırmasına girin. 

![DC/OS Evreninde Veri Köpek yapılandırması](./media/container-service-monitoring/datadog3.png) 

Yukarıdaki yapılandırma örneklerinde 10000000 olarak ayarlanır, bu nedenle kümeye yeni bir düğüm eklendiğinde Datadog otomatik olarak bu düğüme bir aracı dağıtacaktır. Bu geçici bir çözümdür. Paketi yükledikten sonra Datadog web sitesine geri dönüp "[Panolar"ı](https://app.datadoghq.com/dash/list)bulmalısınız. Buradan Özel ve Entegrasyon Panoları göreceksiniz. [Docker panosu,](https://app.datadoghq.com/screen/integration/docker) kümenizi izlemek için gereken tüm kapsayıcı ölçümlerine sahip olacaktır. 

