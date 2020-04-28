---
title: Kullanım DıŞı Azure DC/OS kümesini izleme-dynaTrace
description: Bir Azure Container Service DC/OS kümesini dynaTrace ile izleyin. DC/OS panosunu kullanarak dynaTrace OneAgent 'ı dağıtın.
author: MartinGoodwell
ms.service: container-service
ms.topic: conceptual
ms.date: 12/13/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: ab6bb116c93aad8501da21dc5688d7e39f4195fe
ms.sourcegitcommit: 6a4fbc5ccf7cca9486fe881c069c321017628f20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "82166198"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-dynatrace-saasmanaged"></a>Kullanım DıŞı DynaTrace SaaS/yönetilen ile Azure Container Service DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Container Service kümenizdeki tüm aracı düğümlerini izlemek için [dynaTrace](https://www.dynatrace.com/) oneagent ' ı nasıl dağıtacağınızı göstereceğiz. Bu yapılandırma için dynaTrace SaaS/yönetilmiş bir hesabınız olması gerekir. 

## <a name="dynatrace-saasmanaged"></a>DynaTrace SaaS/yönetilen
DynaTrace, yüksek oranda dinamik kapsayıcı ve küme ortamları için bulut Yerel bir izleme çözümüdür. Gerçek zamanlı kullanım verilerini kullanarak kapsayıcı dağıtımlarınızı ve bellek ayırmalarını daha iyi en iyi hale getirmenize olanak tanır. Otomatik taban çizgisi, sorun bağıntısı ve kök neden algılamayı sağlayarak uygulama ve altyapı sorunlarını otomatik olarak işaret eder.

Aşağıdaki şekilde, dynaTrace Kullanıcı arabirimi gösterilmektedir:

![DynaTrace Kullanıcı arabirimi](./media/container-service-monitoring-dynatrace/dynatrace.png)

## <a name="prerequisites"></a>Ön koşullar 
Azure Container Service tarafından yapılandırılan bir kümeye [dağıtın](container-service-deployment.md) ve [bağlanın](./../container-service-connect.md) . [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Bir dynaTrace SaaS hesabı ayarlamak [https://www.dynatrace.com/trial/](https://www.dynatrace.com/trial/) için bölümüne gidin.  

## <a name="configure-a-dynatrace-deployment-with-marathon"></a>Marathon ile bir dynaTrace dağıtımı yapılandırma
Bu adımlarda, Marathon ile dynaTrace uygulamalarının nasıl yapılandırılacağı ve kümenize dağıtılacağı gösterilmektedir.

1. Aracılığıyla `http://localhost:80/`DC/OS Kullanıcı arabirimine erişin. DC/OS kullanıcı arabiriminden bir kez **Universe** sekmesine gidin ve ardından **dynaTrace**için arama yapın.

    ![DC/OS Universe 'de dynaTrace](./media/container-service-monitoring-dynatrace/dynatrace-universe.png)

2. Yapılandırmayı gerçekleştirmek için bir dynaTrace SaaS hesabına veya ücretsiz bir deneme hesabına sahip olmanız gerekir. DynaTrace panosunda oturum açtıktan sonra **dynaTrace dağıt**' ı seçin.

    ![DynaTrace PaaS tümleştirmesini ayarlama](./media/container-service-monitoring-dynatrace/setup-paas.png)

3. Sayfasında **PaaS tümleştirmesini ayarla**' yı seçin. 

    ![DynaTrace API belirteci](./media/container-service-monitoring-dynatrace/api-token.png) 

4. API belirtecinizi DC/OS Universe içindeki dynaTrace OneAgent yapılandırmasına girin. 

    ![DC/OS Universe üzerinde dynaTrace OneAgent yapılandırması](./media/container-service-monitoring-dynatrace/dynatrace-config.png)

5. Örnekleri çalıştırmayı düşündüğünüz düğüm sayısına ayarlayın. Daha yüksek bir sayı ayarlanması de çalışır, ancak DC/OS bu sayıya ulaşılıncaya kadar yeni örnekler bulmaya çalışacaktır. İsterseniz bunu 1000000 gibi bir değere de ayarlayabilirsiniz. Bu durumda, kümeye yeni bir düğüm eklendiğinde, dynaTrace bu yeni düğüme otomatik olarak bir aracı dağıtır ve daha fazla örnek dağıtmaya çalışan DC/OS fiyatına sürekli olarak bir aracı dağıtır.

    ![DC/OS Universe-Instances içindeki dynaTrace yapılandırması](./media/container-service-monitoring-dynatrace/dynatrace-config2.png)

## <a name="next-steps"></a>Sonraki adımlar

Paketi yükledikten sonra, dynaTrace panosuna geri gidin. Kümenizin içindeki kapsayıcılar için farklı kullanım ölçümlerini inceleyebilirsiniz. 
