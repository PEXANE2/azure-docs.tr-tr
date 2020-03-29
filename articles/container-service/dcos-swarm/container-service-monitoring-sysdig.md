---
title: (AmortismanA Uğradı) Sysdig ile Azure Kapsayıcı Hizmeti kümesini izleme
description: Sysdig ile bir Azure Container Service kümesini izleyin.
author: sauryadas
ms.service: container-service
ms.topic: conceptual
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: a22d48554573e2517b318f6172b759864bf46612
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277726"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-sysdig"></a>(AmortismanA Uğradı) Sysdig ile Azure Kapsayıcı Hizmeti kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Bu makalede, Azure Container Service kümenizdeki tüm aracı düğümlere Sysdig aracıları dağıtılır. Bu yapılandırma için bir Sysdig hesabınızın olması gerekir. 

## <a name="prerequisites"></a>Ön koşullar
Azure Container Service tarafından yapılandırılmış bir kümeyi [dağıtın](container-service-deployment.md) ve [bağlayın](../container-service-connect.md). [Marathon Kullanıcı Arabirimi](container-service-mesos-marathon-ui.md)’ni keşfedin. Bir [https://app.sysdigcloud.com](https://app.sysdigcloud.com) Sysdig bulut hesabı kurmak için gidin. 

## <a name="sysdig"></a>Sysdig
Sysdig, kümenizdeki kapsayıcıları izlemenize olanak tanıyan bir izleme hizmetidir. Sysdig, sorun gidermeye yardımcı olmasıyla bilinir, ayrıca CPU, Ağ, Bellek ve G/Ç izlemede kullandığınız temel ölçümleri de içerir. Sysdig, üzerinde en fazla çalıştığınız veya en fazla bellek ve CPU kullanan kapsayıcıları görmenizi kolaylaştırır. Bu görünüm, şu anda beta sürümünde olan “Genel Bakış” bölümünde yer alır. 

![Sysdig Kullanıcı Arabirimi](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Marathon ile bir Sysdig dağıtımı yapılandırma
Bu adımlarda Sysdig uygulamalarını Marathon ile yapılandırma ve kümenize dağıtma konuları açıklanmaktadır. 

DC/OS Kullanıcı Arabirimi'nize [http://localhost:80/](http://localhost:80/) bir kez DC/OS UI ile erişin sol altta bulunan "Universe"e gidin ve ardından "Sysdig"i arayın.

![DC/OS Evreninde Sysdig](./media/container-service-monitoring-sysdig/sysdig1.png)

Yapılandırmayı tamamlayabilmeniz için bir Sysdig bulut hesabınız veya ücretsiz deneme hesabınız olmalıdır. Sysdig bulut web sitesinde oturum açtıktan sonra kullanıcı adınıza tıklayın, sayfada “Erişim Anahtarınızı” göreceksiniz. 

![Sysdig API anahtarı](./media/container-service-monitoring-sysdig/sysdig2.png) 

Ardından DC/OS Evreninde Sysdig yapılandırmanıza Erişim Anahtarınızı girin. 

![DC/OS Evreninde Sysdig yapılandırması](./media/container-service-monitoring-sysdig/sysdig3.png)

Kümeye yeni bir düğüm eklendiğinde Sysdig’in bu yeni düğüme otomatik olarak bir aracı dağıtması için örnekleri 10000000 olarak ayarlayın. Bu, Sysdig’in küme içindeki tüm aracılara dağıtım yapmasını sağlayacak geçici bir çözümdür. 

![DC/OS Evreninde Sysdig yapılandırmasına örnekler](./media/container-service-monitoring-sysdig/sysdig4.png)

Paketi yükledikten sonra Sysdig kullanıcı arabirimine geri dönün. Buradan kümeniz içindeki kapsayıcılar için farklı ölçümlerin nasıl kullanıldığını keşfedebilirsiniz. 

Ayrıca [yeni pano sihirbazıyla](https://app.sysdigcloud.com/#/dashboards/new) Mesos ve Marathon'a özel panoları da yükleyebilirsiniz.
