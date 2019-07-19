---
title: Azure Izleyici günlükleri ile kapsayıcı örneği günlüğü
description: Azure Container Instances 'dan Azure Izleyici günlüklerine günlük gönderme hakkında bilgi edinin.
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: overview
ms.date: 07/09/2019
ms.author: danlep
ms.openlocfilehash: 4099bc0b15f02faade02f47aeb00fb7c4b4a3332
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325875"
---
# <a name="container-instance-logging-with-azure-monitor-logs"></a>Azure Izleyici günlükleri ile kapsayıcı örneği günlüğü

Log Analytics çalışma alanları, günlük verilerinin yalnızca Azure kaynaklarından değil, aynı zamanda diğer bulutlardaki kaynakları ve kaynakları depolama ve sorgulama için merkezi bir konum sağlar. Azure Container Instances, Azure Izleyici günlüklerine veri göndermeye yönelik yerleşik destek içerir.

Azure Izleyici günlüklerine kapsayıcı örneği verileri göndermek için bir kapsayıcı grubu oluştururken bir Log Analytics çalışma alanı KIMLIĞI ve çalışma alanı anahtarı belirtmeniz gerekir. Aşağıdaki bölümlerde, günlüğe yazma etkin bir kapsayıcı grubu ve sorgulama günlükleri oluşturma açıklanmaktadır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Önkoşullar

Kapsayıcı örneklerinizde oturum açmayı etkinleştirmek için aşağıdakiler gerekir:

* [Log Analytics çalışma alanı](../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (veya [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Log Analytics kimlik bilgilerini alma

Azure Container Instances, Log Analytics çalışma alanınıza veri göndermek için izne ihtiyaç duyuyor. Bu izni vermek ve günlüğe kaydetmeyi etkinleştirmek için, Log Analytics çalışma alanını veya anahtarlarından birini (birincil veya ikincil) sağlamanız gerekir.

Log Analytics çalışma alanı KIMLIĞINI ve birincil anahtarı almak için:

1. Azure portalında Log Analytics çalışma alanınıza gidin
1. **Ayarlar**altında **Gelişmiş ayarlar** ' ı seçin.
1. **Bağlı Kaynaklar** > **Windows Sunucuları**’nı seçin (**Linux Sunucuları**’nı da seçebilirsiniz--kimlik ve anahtarlar ikisi için de aynıdır)
1. Şunları not edin:
   * **ÇALIŞMA ALANI KİMLİĞİ**
   * **BİRİNCİL ANAHTAR**

## <a name="create-container-group"></a>Kapsayıcı grubu oluştur

Log Analytics çalışma alanı KIMLIĞINE ve birincil anahtara sahip olduğunuza göre, günlüğe kaydetme etkinleştirilmiş bir kapsayıcı grubu oluşturmaya hazırsınız demektir.

Aşağıdaki örneklerde tek bir [floentd][fluentd] kapsayıcısına sahip bir kapsayıcı grubu oluşturmanın iki yolu gösterilmektedir: Azure CLı ve bir YAML şablonuyla Azure CLı. Fluentd kapsayıcısı varsayılan yapılandırmasında birkaç çıkış satırı üretir. Bu çıkış Log Analytics çalışma alanınıza gönderileceğinden, günlükleri görüntüleme ve sorgulamayı göstermek için kullanışlıdır.

### <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI ile dağıtmak için, `--log-analytics-workspace` [az Container Create][az-container-create] komutunda `--log-analytics-workspace-key` ve parametrelerini belirtin. Aşağıdaki komutu çalıştırmadan önce, iki çalışma alanı değerini önceki adımda aldığınız değerlerle değiştirin (ve kaynak grubu adını güncelleştirin).

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainergroup001 \
    --image fluent/fluentd \
    --log-analytics-workspace <WORKSPACE_ID> \
    --log-analytics-workspace-key <WORKSPACE_KEY>
```

### <a name="deploy-with-yaml"></a>YAML ile dağıtma

Kapsayıcı gruplarını YAML ile dağıtmayı tercih ediyorsanız bu yöntemi kullanın. Aşağıdaki YAML, tek kapsayıcı içeren bir kapsayıcı grubunu gösterir. YAML şablonunu yeni bir dosyaya kopyalayın, ardından `LOG_ANALYTICS_WORKSPACE_ID` ve `LOG_ANALYTICS_WORKSPACE_KEY` değerlerini önceki adımda aldığınız değerlerle değiştirin. Dosyanızı **deploy-aci.yaml** olarak kaydedin.

```yaml
apiVersion: 2018-10-01
location: eastus
name: mycontainergroup001
properties:
  containers:
  - name: mycontainer001
    properties:
      environmentVariables: []
      image: fluent/fluentd
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
  diagnostics:
    logAnalytics:
      workspaceId: LOG_ANALYTICS_WORKSPACE_ID
      workspaceKey: LOG_ANALYTICS_WORKSPACE_KEY
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Ardından, kapsayıcı grubunu dağıtmak için aşağıdaki komutu yürütün. Aboneliğinizdeki bir kaynak grubuyla değiştirin `myResourceGroup` (veya önce "myresourcegroup" adlı bir kaynak grubu oluşturun):

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Komutu verdikten kısa bir süre sonra, Azure’dan dağıtım ayrıntılarını içeren bir yanıt almanız gerekir.

## <a name="view-logs-in-azure-monitor-logs"></a>Günlükleri Azure Izleyici günlüklerinde görüntüleme

Kapsayıcı grubunu dağıttıktan sonra, ilk günlük girdilerinin Azure portalında görünmesi birkaç dakika (en fazla 10) alabilir. Kapsayıcı grubunun günlüklerini görüntülemek için:

1. Azure portalında Log Analytics çalışma alanınıza gidin
1. **Genel**altında **Günlükler** ' i seçin  
1. Aşağıdaki sorguyu yazın:`search *`
1. **Çalıştır** 'ı seçin

`search *` sorgusu tarafından görüntülenen birkaç sonuç görmeniz gerekir. İlk olarak herhangi bir sonuç görmüyorsanız, birkaç dakika bekleyin ve sonra sorguyu yürütmek için **Çalıştır** düğmesini seçin. Varsayılan olarak, günlük girişleri **tablo** biçiminde görüntülenir. Daha sonra ayrı bir günlük girdisinin içeriğini görmek için bir satırı genişletebilirsiniz.

![Azure portalında Günlük Araması sonuçları][log-search-01]

## <a name="query-container-logs"></a>Kapsayıcı günlüklerini sorgulama

Azure Izleyici günlükleri, büyük olasılıkla binlerce günlük çıktı satırı üzerinden bilgi çekmek için kapsamlı bir [sorgu dili][query_lang] içerir.

Azure Container Instances günlüğe yazma aracı girdileri Log Analytics çalışma alanınızın `ContainerInstanceLog_CL` tablosuna gönderir. Bir sorgunun temel yapısı bir kaynak tablo (`ContainerInstanceLog_CL`) ve bunu izleyen dikey çizgi karakteriyle (`|`) ayrılmış bir işleç dizisidir. Sonuçları daraltmak ve gelişmiş işlevleri gerçekleştirmek için çeşitli işleçleri zincirleyebilirsiniz.

Örnek sorgu sonuçları görmek için, aşağıdaki sorguyu sorgu metni kutusuna yapıştırın ("Eski dil dönüştürücüyü göster" altında) ve sorguyu yürütmek için **ÇALIŞTIR** düğmesini seçin. Bu sorgu "Message" alanı "warn" sözcüğünü içeren tüm günlük girdilerini gösterir:

```query
ContainerInstanceLog_CL
| where Message contains("warn")
```

Daha karmaşık sorgular da desteklenir. Örneğin, bu sorgu yalnızca son saat içinde "mycontainergroup001" kapsayıcı grubu için oluşturulan günlük girdilerini görüntüler:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Azure Izleyici günlüklerinde günlükleri sorgulama ve Uyarıları yapılandırma hakkında daha fazla bilgi için bkz.:

* [Azure Izleyici günlüklerinde günlük aramalarını anlama](../log-analytics/log-analytics-log-search.md)
* [Azure İzleyici’de birleştirilmiş uyarılar](../azure-monitor/platform/alerts-overview.md)


### <a name="monitor-container-cpu-and-memory"></a>Kapsayıcı CPU ve belleğini izleme

Kapsayıcı örneği CPU ve bellek kaynaklarını izleme hakkında daha fazla bilgi için bkz.

* [Azure Container Instances’taki kapsayıcı kaynaklarını izleyin](container-instances-monitor.md).

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://aka.ms/LogAnalyticsLanguage

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create