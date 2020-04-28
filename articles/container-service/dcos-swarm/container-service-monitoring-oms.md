---
title: Kullanım DıŞı Azure DC/OS kümesi-Operations Management 'ı izleme
description: Bir Azure Container Service DC/OS kümesini Log Analytics ile izleyin.
author: keikhara
ms.service: container-service
ms.topic: conceptual
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 1ab8d1cf3eb38a17f0b3d6c8137e37237498a527
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76277333"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-log-analytics"></a>Kullanım DıŞı Log Analytics ile Azure Container Service DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Log Analytics, Microsoft 'un Şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan bulut tabanlı BT yönetimi çözümüdür.Kapsayıcı çözümü, kapsayıcı envanterini, performansı ve günlükleri tek bir konumda görüntülemenize yardımcı olan Log Analytics bir çözümdür. Günlükleri merkezi konuma görüntüleyerek denetleyebilir, sorun giderebilir ve bir konakta çok fazla kapsayıcı tüketen çok fazla kapsayıcı bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözümü hakkında daha fazla bilgi için bkz. [kapsayıcı çözümü Log Analytics](../../azure-monitor/insights/containers.md).

## <a name="setting-up-log-analytics-from-the-dcos-universe"></a>DC/OS Universe 'dan Log Analytics ayarlama


Bu makalede, bir DC/OS ayarladığınızı ve kümede basit Web kapsayıcı uygulamaları dağıttığınızı varsayılmaktadır.

### <a name="pre-requisite"></a>Önkoşul
- [Abonelik Microsoft Azure](https://azure.microsoft.com/free/) , ücretsiz bir abonelik edinebilirsiniz.  
- Log Analytics çalışma alanı kurulumu-aşağıda "Adım 3" bölümüne bakın
- [DC/OS CLI](https://docs.mesosphere.com/1.12/cli) yüklendi.

1. DC/OS panosunda, Universe ' a tıklayın ve aşağıda gösterildiği gibi ' OMS ' araması yapın.

   >[!NOTE]
   >OMS artık Log Analytics olarak adlandırılır.

   ![](media/container-service-monitoring-oms/image2.png)

2. **Install**'a tıklayın. Sürüm bilgileri ve **yükleme paketi** veya **Gelişmiş yükleme** düğmesini içeren bir açılır pencere görürsünüz. **Gelişmiş yükleme**' ye tıkladığınızda, **OMS 'ye özgü yapılandırma özellikleri** sayfasına yol açar.

   ![](media/container-service-monitoring-oms/image3.png)

   ![](media/container-service-monitoring-oms/image4.png)

3. Burada `wsid` (Log Analytics çalışma alanı kimliği) ve `wskey` (çalışma alanı kimliği için birincil anahtar) girmeniz istenir. Her ikisini de `wsid` almak `wskey` için, ' de bir hesap oluşturmanız <https://mms.microsoft.com>gerekir.
   Hesap oluşturmak için adımları izleyin. Hesabı oluşturmayı tamamladıktan sonra, aşağıda gösterildiği gibi `wsid` **Ayarlar**, **bağlı kaynaklar**ve **Linux sunucuları**' na tıklayarak ve `wskey` ' yi edinmeniz gerekir.

   ![](media/container-service-monitoring-oms/image5.png)

4. İstediğiniz örnek sayısını seçin ve ' ıncele ve yüklensin ' düğmesine tıklayın. Genellikle, aracı kümenizde sahip olduğunuz VM sayısına eşit sayıda örnek olması gerekir. Linux için Log Analytics Aracısı, izleme ve günlüğe kaydetme bilgilerini toplamak istediği her VM 'de ayrı kapsayıcılar olarak yüklenir.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

## <a name="setting-up-a-simple-log-analytics-dashboard"></a>Basit bir Log Analytics panosu ayarlama

VM 'Lere Linux için Log Analytics Aracısı 'nı yükledikten sonra, sonraki adım Log Analytics panosunu ayarlamaya yöneliktir. Panoyu Azure portal aracılığıyla ayarlayabilirsiniz.

### <a name="azure-portal"></a>Azure portal 

Azure portal için oturum açın <https://portal.microsoft.com/>. **Market**'e gidin, **izleme + yönetim** ' i seçin ve **Tümünü gör**' e tıklayın. Ardından arama `containers` yazın. Arama sonuçlarında "kapsayıcılar" ifadesini görürsünüz. **Kapsayıcılar** ' ı seçin ve **Oluştur**' a tıklayın.

![](media/container-service-monitoring-oms/image9.png)

**Oluştur**'a tıkladığınızda, sizden çalışma alanınızı sorar. Çalışma alanınızı seçin ya da bir tane yoksa, yeni bir çalışma alanı oluşturun.

![](media/container-service-monitoring-oms/image10.PNG)

Çalışma alanınızı seçtikten sonra **Oluştur**' a tıklayın.

![](media/container-service-monitoring-oms/image11.png)

Log Analytics kapsayıcı çözümü hakkında daha fazla bilgi için lütfen [kapsayıcı çözümü Log Analytics](../../azure-monitor/insights/containers.md)başvurun.

### <a name="how-to-scale-log-analytics-agent-with-acs-dcos"></a>Log Analytics aracısını ACS DC/OS ile ölçeklendirme 

Gerçek düğüm sayısının Log Analytics Aracısı ' nı yüklemiş olmanız veya daha fazla VM ekleyerek sanal makine ölçek kümesini ölçeklendirmeniz gerekiyorsa, `msoms` hizmeti ölçeklendirerek bunu yapabilirsiniz.

Marathon veya DC/OS UI Services sekmesine gidebilir ve düğüm sayılarınızın ölçeğini değiştirebilirsiniz.

![](media/container-service-monitoring-oms/image12.PNG)

Bu, henüz Log Analytics aracısını dağıtmayan diğer düğümlere dağıtılır.

## <a name="uninstall-ms-oms"></a>MS OMS 'yi kaldırma

MS OMS 'yi kaldırmak için aşağıdaki komutu girin:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Bize!!! bilgi verin
Ne işe yarar? Neler eksik? Bunun sizin için yararlı olması için başka ne gerekir? <a href="mailto:OMSContainers@microsoft.com">Omscontainers</a>'da bize bilgi verin.

## <a name="next-steps"></a>Sonraki adımlar

 Kapsayıcılarınızı izlemek için Log Analytics ayarladığınıza göre,[kapsayıcı panonuz için bkz](../../azure-monitor/insights/containers.md)..
