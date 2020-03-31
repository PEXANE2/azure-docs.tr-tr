---
title: (AmortismanA Uğradı) Azure DC/OS kümesini izleyin - İşlem Yönetimi
description: Log Analytics ile bir Azure Kapsayıcı Hizmeti DC/OS kümesini izleyin.
author: keikhara
ms.service: container-service
ms.topic: conceptual
ms.date: 11/17/2016
ms.author: keikhara
ms.custom: mvc
ms.openlocfilehash: 1ab8d1cf3eb38a17f0b3d6c8137e37237498a527
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277333"
---
# <a name="deprecated-monitor-an-azure-container-service-dcos-cluster-with-log-analytics"></a>(AmortismanA Uğradı) Log Analytics ile Azure Kapsayıcı Hizmeti DC/OS kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

Log Analytics, Microsoft'un şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan bulut tabanlı BT yönetimi çözümüdür.Kapsayıcı Çözümü, Konteyner envanterini, performansını ve günlüklerini tek bir konumda görüntülemenize yardımcı olan Log Analytics'teki bir çözümdür. Merkezi konumdaki günlükleri görüntüleyerek kapsayıcıları denetleyebilir, sorun giderebilir ve bir ana bilgisayarda gürültülü aşırı kapsayıcı tüketen bir kutucağ bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı Çözümü hakkında daha fazla bilgi [için, Konteyner Çözüm Günlüğü Analizi'ne](../../azure-monitor/insights/containers.md)bakın.

## <a name="setting-up-log-analytics-from-the-dcos-universe"></a>DC/OS evreninden Log Analytics'i ayarlama


Bu makalede, bir DC/OS ayarladınız ve küme üzerinde basit web kapsayıcı uygulamaları dağıttınız varsayar.

### <a name="pre-requisite"></a>Önkoşul
- [Microsoft Azure Aboneliği](https://azure.microsoft.com/free/) - Ücretsiz abonelik alabilirsiniz.  
- Günlük Analitik Çalışma Alanı Kurulumu - aşağıdaki "Adım 3" bölümüne bakın
- [DC/OS CLI](https://docs.mesosphere.com/1.12/cli) yüklendi.

1. DC/OS panosunda, Evren'e tıklayın ve aşağıda gösterildiği gibi 'OMS' araması yapın.

   >[!NOTE]
   >OMS artık Log Analytics olarak adlandırılır.

   ![](media/container-service-monitoring-oms/image2.png)

2. **Yükle'yi**tıklatın. Sürüm bilgilerini ve **Yükleme Paketini** veya **Gelişmiş Yükleme** düğmesini içeren bir açılır pencere görürsünüz. **Gelişmiş Yükleme'yi**tıklattığınızda, bu da sizi **OMS'ye özgü yapılandırma özellikleri** sayfasına yönlendirir.

   ![](media/container-service-monitoring-oms/image3.png)

   ![](media/container-service-monitoring-oms/image4.png)

3. Burada, `wsid` (Log Analytics çalışma alanı kimliği) ve `wskey` (çalışma alanı kimliği için birincil anahtar) girmeniz istenir. Her ikisini `wsid` `wskey` de elde etmek için <https://mms.microsoft.com>ve 'de bir hesap oluşturmanız gerekir.
   Hesap oluşturmak için adımları izleyin. Bir kez hesap oluşturma yapılır, sizin `wsid` ve `wskey` **ayarlar**tıklayarak almak gerekir , sonra **Bağlı Kaynaklar**, ve daha sonra **Linux Sunucuları**, aşağıda gösterildiği gibi.

   ![](media/container-service-monitoring-oms/image5.png)

4. İstediğiniz örnek sayısını seçin ve 'Gözden Geçir ve Yükle' düğmesini tıklatın. Genellikle, aracı kümenizde bulunan VM sayısına eşit örnek sayısına sahip olmak istersiniz. Linux için Log Analytics aracısı, bilgileri izlemek ve kaydetmek için bilgi toplamak istediği her VM'ye tek tek kapsayıcı olarak yükler.

   [!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

## <a name="setting-up-a-simple-log-analytics-dashboard"></a>Basit bir Log Analytics panosu ayarlama

Linux için Log Analytics aracısını VM'lere yükledikten sonra, bir sonraki adım Log Analytics panosunu kurmaktır. Panoyu Azure portalı üzerinden ayarlayabilirsiniz.

### <a name="azure-portal"></a>Azure portalında 

Azure portalında oturum <https://portal.microsoft.com/>aç. **Market'e**gidin, **İzleme + yönetimi** seçin ve **Tümünü Gör'e**tıklayın. Sonra `containers` arama yazın. Arama sonuçlarında "kapsayıcılar" göreceksiniz. **Kapsayıcılar'ı** seçin ve **Oluştur'u**tıklatın.

![](media/container-service-monitoring-oms/image9.png)

**Oluştur'u**tıklattığınızda, çalışma alanınızı ister. Çalışma alanınızı seçin veya çalışma alanınız yoksa, yeni bir çalışma alanı oluşturun.

![](media/container-service-monitoring-oms/image10.PNG)

Çalışma alanınızı seçtikten sonra **Oluştur'u**tıklatın.

![](media/container-service-monitoring-oms/image11.png)

Log Analytics Konteyner Çözümü hakkında daha fazla bilgi için lütfen [Konteyner Çözüm Günlüğü Analizi'ne](../../azure-monitor/insights/containers.md)bakın.

### <a name="how-to-scale-log-analytics-agent-with-acs-dcos"></a>ACS DC/OS ile Log Analytics aracısı nasıl ölçeklendirilir? 

Log Analytics aracısını gerçek düğüm sayısının altında yüklemeniz gerekiyorsa veya daha fazla VM ekleyerek sanal makine ölçeğini ölçeklendiriyorsanız, `msoms` bunu hizmeti ölçeklendirerek yapabilirsiniz.

Maraton'a veya DC/OS Kullanıcı Bira Hizmetleri sekmesine gidip düğüm sayınızı ölçeklendirebilirsiniz.

![](media/container-service-monitoring-oms/image12.PNG)

Bu işlem, Log Analytics aracısını henüz dağıtmamış diğer düğümlere dağıtılır.

## <a name="uninstall-ms-oms"></a>MS OMS'yi kaldır

MS OMS'u kaldırmak için aşağıdaki komutu girin:

```bash
$ dcos package uninstall msoms
```

## <a name="let-us-know"></a>Bize bildirin!!!
Ne işe yarıyor? Eksik olan ne? Bunun sizin için yararlı olması için başka neye ihtiyacınız var? <a href="mailto:OMSContainers@microsoft.com">Bize OMSContainers</a>de bildirin.

## <a name="next-steps"></a>Sonraki adımlar

 Artık kapsayıcılarınızı izlemek için Log Analytics'i kurduğunuza[göre, konteyner panonuza bakın.](../../azure-monitor/insights/containers.md)
