---
title: (AmortismanA Uğradı) Azure DC/OS kümesini izleyin - Dynatrace
description: Dynatrace ile bir Azure Kapsayıcı Hizmeti DC/OS kümesini izleyin. DC/OS panosunu kullanarak Dynatrace OneAgent'ı dağıtın.
author: MartinGoodwell
ms.service: container-service
ms.topic: conceptual
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: a82481c5cb3d12b11179b41999f73e67583ec43b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277756"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>(AmortismanA Uğradı) Dynatrace SaaS/Yönetilen ile bir Azure Kapsayıcı Hizmeti DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Kapsayıcı Hizmeti kümenizdeki tüm aracı düğümlerini izlemek için [Dynatrace](https://www.dynatrace.com/) OneAgent'ı nasıl dağıtabileceğinizi göstereceğiz. Bu yapılandırma için Dynatrace SaaS/Managed'te bir hesaba ihtiyacınız vardır. 

## <a name="dynatrace-saasmanaged"></a>Dynatrace SaaS/Yönetilen
Dynatrace, son derece dinamik kapsayıcı ve küme ortamları için bulut ait bir izleme çözümüdür. Gerçek zamanlı kullanım verilerini kullanarak kapsayıcı dağıtımlarınızı ve bellek ayırmalarınızı daha iyi optimize etmenizi sağlar. Otomatik taban çizgileme, sorun korelasyon ve kök neden algılama sağlayarak uygulama ve altyapı sorunlarını otomatik olarak belirleyebilme yeteneğine sahiptir.

Aşağıdaki şekil Dynatrace UI gösterir:

![Dynatrace UI](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Ön koşullar 
Azure Kapsayıcı Hizmeti tarafından yapılandırılan bir kümeye [dağıtın](container-service-deployment.md) ve [bağlanın.](./../container-service-connect.md) [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Dynatrace SaaS hesabı açmak için [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) gidin.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Marathon ile Dynatrace dağıtımını yapılandırın
Bu adımlar, Marathon ile Dynatrace uygulamalarını kümenize nasıl yapılandırabileceğinizi ve dağıtabileceğinizi gösterir.

1. DC/OS Kullanıcı UI'nize . [http://localhost:80/](http://localhost:80/) DC/OS Kullanıcı Arabirimi'ne bir kez gidin, **Evren** sekmesine gidin ve ardından **Dynatrace'i**arayın.

    ![DC/OS Evreninde Dinamit](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. Yapılandırmayı tamamlamak için bir Dynatrace SaaS hesabına veya ücretsiz deneme hesabına ihtiyacınız vardır. Dynatrace panosuna giriş yaptıktan **sonra, Dynatrace'i dağıt'ı**seçin.

    ![Dynatrace PaaS entegrasyonunu ayarlama](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Sayfada, **PaaS tümleştirmesini ayarla'yı**seçin. 

    ![Dynatrace API belirteci](./media/container-service-monitoring-dynatrace/api-token.png) 

4. API belirtecinizi DC/OS Universe içindeki Dynatrace OneAgent yapılandırmasına girin. 

    ![DC/OS Universe'de Dynatrace OneAgent yapılandırması](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Örnekleri çalıştırmak istediğiniz düğüm sayısına ayarlayın. Daha yüksek bir sayı ayarlama da çalışır, ancak DC/OS bu sayıya gerçekten ulaşılana kadar yeni örnekler bulmaya çalışır. İsterseniz, bunu 1000000 gibi bir değere de ayarlayabilirsiniz. Bu durumda, kümeye ne zaman yeni bir düğüm eklendiğinde, Dynatrace otomatik olarak dc/os karşılığında sürekli olarak başka örnekleri dağıtmaya çalışırken bu yeni düğüme bir aracı dağıtır.

    ![DC/OS Evren örneklerinde dynatrace yapılandırması](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Sonraki adımlar

Paketi yükledikten sonra Dynatrace panosuna geri gidin. Kümenizdeki kapsayıcılar için farklı kullanım ölçümlerini keşfedebilirsiniz. 
