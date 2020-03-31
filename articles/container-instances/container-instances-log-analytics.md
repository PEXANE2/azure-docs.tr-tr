---
title: Kaynak günlüklerini toplama & analiz edin
description: Azure Kapsayıcı Örnekleri'ndeki kapsayıcı gruplarından kaynak günlüklerini ve olay verilerini Azure Monitörgünlüklerine nasıl göndereceğinizi öğrenin
ms.topic: article
ms.date: 01/08/2020
ms.author: danlep
ms.openlocfilehash: 304e98fff386911b878877d2f03d489d0eef5dd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75770552"
---
# <a name="container-group-and-instance-logging-with-azure-monitor-logs"></a>Azure Monitor günlükleriyle kapsayıcı grubu ve örnek günlüğe kaydetme

Günlük Analizi çalışma alanları, günlük verilerini yalnızca Azure kaynaklarından değil, diğer bulutlardaki şirket içi kaynakları ve kaynakları depolamak ve sorgulamak için merkezi bir konum sağlar. Azure Kapsayıcı Örnekleri, günlükleri ve olay verilerini Azure Monitor günlüklerine göndermek için yerleşik destek içerir.

Kapsayıcı grup günlüğü ve olay verilerini Azure Monitor günlüklerine göndermek için, kapsayıcı grubu oluştururken varolan bir Log Analytics çalışma alanı kimliği ve çalışma alanı anahtarını belirtin. Aşağıdaki bölümlerde, günlüğe kaydetme özelliğine göre kapsayıcı grubunun nasıl oluşturulup sorgulanmayı anlatılıyor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
> Şu anda, etkinlik verilerini yalnızca Linux kapsayıcı örneklerinden Log Analytics'e gönderebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Kapsayıcı örneklerinizde oturum açmayı etkinleştirmek için aşağıdakiler gerekir:

* [Log Analytics çalışma alanı](../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (veya [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Log Analytics kimlik bilgilerini alma

Azure Container Instances, Log Analytics çalışma alanınıza veri göndermek için izne ihtiyaç duyuyor. Bu izni vermek ve günlüğe kaydetmeyi etkinleştirmek için, Log Analytics çalışma alanını veya anahtarlarından birini (birincil veya ikincil) sağlamanız gerekir.

Günlük analizi çalışma alanı kimliğini ve birincil anahtarı elde etmek için:

1. Azure portalında Log Analytics çalışma alanınıza gidin
1. **Ayarlar**altında **Gelişmiş ayarları** seçin
1. **Bağlantılı Kaynaklar** > **Windows Sunucuları'nı** seçin (veya Linux **Sunucuları**--kimlik ve anahtarlar her ikisi için de aynıdır)
1. Şunları not edin:
   * **ÇALIŞMA ALANI KİMLİĞİ**
   * **BİRİNCİL ANAHTAR**

## <a name="create-container-group"></a>Kapsayıcı grubu oluştur

Artık günlük analizi çalışma alanı kimliğine ve birincil anahtarına sahip olduğunuza göre, günlüğe kaydetme özellikli bir kapsayıcı grubu oluşturmaya hazırsınız.

Aşağıdaki örnekler, tek bir [akıcı][fluentd] kapsayıcıdan oluşan bir kapsayıcı grubu oluşturmanın iki yolunu gösterir: YaML şablonu olan Azure CLI ve Azure CLI. Akıcı kapsayıcı, varsayılan yapılandırmasında birkaç çıkış satırı üretir. Bu çıkış Log Analytics çalışma alanınıza gönderileceğinden, günlükleri görüntüleme ve sorgulamayı göstermek için kullanışlıdır.

### <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Azure CLI ile dağıtmak için [az container create][az-container-create] komutunda `--log-analytics-workspace` ve `--log-analytics-workspace-key` parametrelerini belirtin. Aşağıdaki komutu çalıştırmadan önce, iki çalışma alanı değerini önceki adımda aldığınız değerlerle değiştirin (ve kaynak grubu adını güncelleştirin).

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

Ardından, kapsayıcı grubunu dağıtmak için aşağıdaki komutu uygulayın. Aboneliğinizde bir kaynak grubuyla değiştirin `myResourceGroup` (veya önce "myResourceGroup" adlı bir kaynak grubu oluşturun):

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Komutu verdikten kısa bir süre sonra, Azure’dan dağıtım ayrıntılarını içeren bir yanıt almanız gerekir.

## <a name="view-logs"></a>Günlükleri görüntüleme

Kapsayıcı grubunu dağıttıktan sonra, ilk günlük girdilerinin Azure portalında görünmesi birkaç dakika (en fazla 10) alabilir. Kapsayıcı grubunun günlüklerini tabloda `ContainerInstanceLog_CL` görüntülemek için:

1. Azure portalında Log Analytics çalışma alanınıza gidin
1. **Genel**altında, **Günlükleri** seçin  
1. Aşağıdaki sorguyu yazın:`ContainerInstanceLog_CL | limit 50`
1. **Çalıştır'ı** seçin

Sorgu tarafından görüntülenen birkaç sonuç görmeniz gerekir. İlk başta herhangi bir sonuç görmüyorsanız, birkaç dakika bekleyin ve sorguyu yeniden yürütmek için **Çalıştır** düğmesini seçin. Varsayılan olarak, günlük girişleri **Tablo** biçiminde görüntülenir. Daha sonra ayrı bir günlük girdisinin içeriğini görmek için bir satırı genişletebilirsiniz.

![Azure portalında Günlük Araması sonuçları][log-search-01]

## <a name="view-events"></a>Etkinlikleri görüntüleme

Azure portalında kapsayıcı örnekleriiçin olayları da görüntüleyebilirsiniz. Olaylar, örneğin oluşturulduğu ve başlatıldığı zamanı içerir. `ContainerEvent_CL` Tablodaki olay verilerini görüntülemek için:

1. Azure portalında Log Analytics çalışma alanınıza gidin
1. **Genel**altında, **Günlükleri** seçin  
1. Aşağıdaki sorguyu yazın:`ContainerEvent_CL | limit 50`
1. **Çalıştır'ı** seçin

Sorgu tarafından görüntülenen birkaç sonuç görmeniz gerekir. İlk başta herhangi bir sonuç görmüyorsanız, birkaç dakika bekleyin ve sorguyu yeniden yürütmek için **Çalıştır** düğmesini seçin. Varsayılan olarak, girişler **Tablo** biçiminde görüntülenir. Daha sonra, tek bir girişin içeriğini görmek için satırı genişletebilirsiniz.

![Azure portalında Olay Arama sonuçları][log-search-02]

## <a name="query-container-logs"></a>Kapsayıcı günlüklerini sorgulama

Azure Monitor günlükleri, günlük çıktısının binlerce satırından bilgi çekmek için kapsamlı bir [sorgu dili][query_lang] içerir.

Sorgunun temel yapısı kaynak tablodur (bu `ContainerInstanceLog_CL` makalede `ContainerEvent_CL`veya) ardından boru karakteriyle ayrılmış`|`bir dizi işleç ( ). Sonuçları daraltmak ve gelişmiş işlevleri gerçekleştirmek için çeşitli işleçleri zincirleyebilirsiniz.

Örnek sorgu sonuçlarını görmek için aşağıdaki sorguyu sorgu metin kutusuna yapıştırın ve sorguyu yürütmek için **Çalıştır** düğmesini seçin. Bu sorgu "Message" alanı "warn" sözcüğünü içeren tüm günlük girdilerini gösterir:

```query
ContainerInstanceLog_CL
| where Message contains "warn"
```

Daha karmaşık sorgular da desteklenir. Örneğin, bu sorgu yalnızca son saat içinde "mycontainergroup001" kapsayıcı grubu için oluşturulan günlük girdilerini görüntüler:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Azure Monitor günlüklerinde günlükleri sorgulama ve uyarıları yapılandırma hakkında daha fazla bilgi için bkz:

* [Azure Monitor günlüklerinde günlük aramalarını anlama](../log-analytics/log-analytics-log-search.md)
* [Azure İzleyici’de birleştirilmiş uyarılar](../azure-monitor/platform/alerts-overview.md)


### <a name="monitor-container-cpu-and-memory"></a>Kapsayıcı CPU ve belleğini izleme

Kapsayıcı örneği CPU ve bellek kaynaklarını izleme hakkında daha fazla bilgi için bkz.

* [Azure Container Instances’taki kapsayıcı kaynaklarını izleyin](container-instances-monitor.md).

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png
[log-search-02]: ./media/container-instances-log-analytics/portal-query-02.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://aka.ms/LogAnalyticsLanguage

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create