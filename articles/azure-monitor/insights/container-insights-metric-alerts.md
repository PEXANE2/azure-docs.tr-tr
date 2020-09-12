---
title: Kapsayıcılar için Azure Izleyici 'den ölçüm uyarıları | Microsoft Docs
description: Bu makalede, Azure Izleyici 'de genel önizlemede bulunan kapsayıcılar için sunulan önerilen ölçüm uyarıları incelenir.
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: aace260ff22d63211424f2ce4a7319bf577436f4
ms.sourcegitcommit: 43558caf1f3917f0c535ae0bf7ce7fe4723391f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2020
ms.locfileid: "90019895"
---
# <a name="recommended-metric-alerts-preview-from-azure-monitor-for-containers"></a>Kapsayıcılar için Azure Izleyici 'den önerilen ölçüm uyarıları (Önizleme)

Azure izleyici günlüklerinde depolanan performans verilerine dayalı olarak, en üst düzey talep ve kapasite neredeyse kapasiteye sahip oldukları sırada sistem kaynak sorunları hakkında uyarı almak için bir günlük uyarısı oluşturursunuz. Kapsayıcılar için Azure Izleyici artık AKS kümeniz için genel önizlemede olan önceden yapılandırılmış ölçüm uyarısı kuralları içerir.

Bu makale, deneyimi inceler ve bu uyarı kurallarını yapılandırma ve yönetme hakkında rehberlik sağlar.

Azure Izleyici uyarılarını bilmiyorsanız, başlamadan önce bkz. [Microsoft Azure uyarılara genel bakış](../platform/alerts-overview.md) . Ölçüm uyarıları hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçüm uyarıları](../platform/alerts-metric-overview.md).

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdakileri onaylayın:

* Özel ölçümler yalnızca Azure bölgelerinin bir alt kümesinde kullanılabilir. Desteklenen bölgelerin listesi [burada](../platform/metrics-custom-overview.md#supported-regions)belgelenmiştir.

* Ölçüm uyarılarını ve ek ölçümlerin giriş bilgilerini desteklemek için gereken en düşük aracı sürümü **Microsoft/OMS: ciprod05262020**' dir.

    Kümenizin aracının daha yeni bir sürümünü çalıştırdığından emin olmak için şunlardan birini yapabilirsiniz:

    * Şu komutu çalıştırın: `kubectl describe <omsagent-pod-name> --namespace=kube-system` . Döndürülen durum ' da, çıkışın *kapsayıcılar* bölümünde omsagent için **görüntü** altındaki değeri aklınızda edin. 
    * **Düğümler** sekmesinde, küme düğümünü seçin ve sağdaki **Özellikler** bölmesinde, **Aracı görüntüsü etiketi**altındaki değeri aklınızda edin.

    Gösterilen değer **ciprod05262020**' den sonraki bir sürüm olmalıdır. Kümenizin daha eski bir sürümü varsa, en son sürümü almak için [AKS kümesi adımlarında yükseltme aracısını](container-insights-manage-agent.md#upgrade-agent-on-aks-cluster) izleyin.
    
    Aracı sürümü ile ilgili daha fazla bilgi için bkz. [Aracı yayınlama geçmişi](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). Ölçümlerin toplandığını doğrulamak için, Azure Izleyici ölçümleri Gezginini kullanabilir ve **Öngörüler** tarafından listelenen **ölçüm ad** alanından emin olabilirsiniz. Varsa, devam edebilir ve uyarıları ayarlamaya başlayabilirsiniz. Toplanan ölçümleri görmüyorsanız, küme hizmeti sorumlusu veya MSI 'nin gerekli izinleri yoktur. SPN veya MSI 'ın **Izleme ölçümleri yayımcı** rolünün bir üyesi olduğunu doğrulamak için, rol atamasını onaylamak ve ayarlamak üzere [Azure CLI kullanarak küme başına yükseltme](container-insights-update-metrics.md#upgrade-per-cluster-using-azure-cli) bölümünde açıklanan adımları izleyin.

## <a name="alert-rules-overview"></a>Uyarı kurallarına genel bakış

Önemli şeyler hakkında uyarı almak için, kapsayıcılar için Azure Izleyici, AKS kümeleriniz için aşağıdaki ölçüm uyarılarını içerir:

|Ad| Açıklama |Varsayılan eşik |
|----|-------------|------------------|
|Ortalama kapsayıcı CPU 'SU yüzdesi |Kapsayıcı başına kullanılan ortalama CPU 'YU hesaplar.|Kapsayıcı başına ortalama CPU kullanımı %95 ' den büyük olduğunda.| 
|Ortalama kapsayıcı çalışma kümesi belleği yüzdesi |Kapsayıcı başına kullanılan ortalama çalışma kümesi belleğini hesaplar.|Kapsayıcı başına ortalama çalışma kümesi bellek kullanımı %95 ' den büyük olduğunda. |
|Ortalama CPU % |Düğüm başına kullanılan ortalama CPU 'YU hesaplar. |Ortalama düğüm CPU kullanımı %80 ' den büyük olduğunda |
|Ortalama Disk kullanımı% |Bir düğüm için Ortalama disk kullanımını hesaplar.|Bir düğüm için disk kullanımı %80 ' den büyükse. |
|Ortalama çalışma kümesi belleği yüzdesi |Bir düğüm için Ortalama çalışma kümesi belleğini hesaplar. |Bir düğüm için Ortalama çalışma kümesi belleği %80 ' den fazla olduğunda. |
|Kapsayıcı sayısı yeniden başlatılıyor |Yeniden başlatma kapsayıcısı sayısını hesaplar. | Kapsayıcı yeniden başlatmalar 0 ' dan büyük olduğunda. |
|Hatalı Pod sayıları |Hatalı durumda bir pod varsa hesaplar.|Başarısız durumundaki bir dizi dizin 0 ' dan büyük olduğunda. |
|Düğüm NotReady durumu |Herhangi bir düğümün NotReady durumunda olup olmadığını hesaplar.|NotReady durumundaki bir dizi düğüm 0 ' dan büyükse. |
|OOM sonlandırılan kapsayıcılar |OOM sonlandırıldı kapsayıcıları sayısını hesaplar. |Bir dizi OOM sonlandırılan kapsayıcı 0 ' dan büyükse. |
|% Pods Ready |Pods 'nin ortalama Ready durumunu hesaplar. |Pod 'nin hazırlanma durumu %80 ' den az olduğunda.|
|Tamamlanan iş sayısı |Altı saatten uzun süre önce tamamlanan işlerin sayısını hesaplar. |Altı saatten eski olan eski işlerin sayısı 0 ' dan büyük olduğunda.|

Tüm bu uyarı kuralları genelinde ortak özellikler vardır:

* Tüm uyarı kuralları ölçüm tabanlıdır.

* Tüm uyarı kuralları varsayılan olarak devre dışıdır.

* Tüm uyarı kuralları dakikada bir kez değerlendirilir ve son 5 dakikalık verileri geri arar.

* Uyarı kurallarında varsayılan olarak bunlara atanmış bir eylem grubu yoktur. Uyarı kuralını düzenleyerek, var olan bir eylem grubunu seçerek veya yeni bir eylem grubu oluşturarak uyarıya bir [eylem grubu](../platform/action-groups.md) ekleyebilirsiniz.

* Uyarı kurallarının eşiğini doğrudan düzenleyerek değiştirebilirsiniz. Ancak, eşiğini değiştirmeden önce her bir uyarı kuralında sunulan kılavuza bakın.

Aşağıdaki uyarı tabanlı ölçümler, diğer ölçümler ile karşılaştırıldığında benzersiz davranış özelliklerine sahiptir:

* *Completedjobscount* ölçümü yalnızca altı saatten daha önce tamamlanan işler olduğunda gönderilir.

* *containerRestartCount* ölçümü yalnızca yeniden başlatma kapsayıcıları olduğunda gönderilir.

* *Oomkilledcontainercount* ölçümü yalnızca OOM tarafından sonlandırılan kapsayıcılar olduğunda gönderilir.

* CPU, bellek RSS ve bellek çalışma kümesi değerleri yapılandırılan eşiği aştığında (varsayılan eşik %95), *Cpuexceededpercentage*, *memoryrssexceededpercentage*ve *Memoryworkingsetexceededpercentage* ölçümleri gönderilir. Bu eşikler, karşılık gelen uyarı kuralı için belirtilen uyarı koşulu eşiğini dışlıyor. Anlamı, bu ölçümleri toplamak ve [Ölçüm Gezgini](../platform/metrics-getting-started.md)' nden çözümlemek istiyorsanız, eşiği uyarı eşiğinden daha düşük bir değere yapılandırmanızı öneririz. Kapsayıcı kaynak kullanımı eşikleri için koleksiyon ayarlarıyla ilişkili yapılandırma, bölümünde ConfigMaps dosyasında geçersiz kılınabilir `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]` . ConfigMap yapılandırma dosyanızı yapılandırma ile ilgili ayrıntılar için, [alertable ölçümleri ConfigMaps](#configure-alertable-metrics-in-configmaps) bölümüne bakın.

## <a name="metrics-collected"></a>Toplanan ölçümler

Aşağıdaki ölçümler, aksi belirtilmediği sürece, bu özelliğin bir parçası olarak etkinleştirilir ve toplanır:

|Ölçüm ad alanı |Ölçüm |Açıklama |
|---------|----|------------|
|Öngörüler. kapsayıcı/düğümler |Cpuusagemiliçekirdekler |Ana bilgisayara göre miliçekirdekte CPU kullanımı.|
|Öngörüler. kapsayıcı/düğümler |cpuUsagePercentage |Düğüme göre CPU kullanım yüzdesi.|
|Öngörüler. kapsayıcı/düğümler |memoryRssBytes |Ana bilgisayar tarafından bayt cinsinden bellek RSS kullanımı.|
|Öngörüler. kapsayıcı/düğümler |memoryRssPercentage |Konağa göre bellek RSS kullanım yüzdesi.|
|Öngörüler. kapsayıcı/düğümler |memoryWorkingSetBytes |Konağa göre bayt cinsinden bellek çalışma kümesi kullanımı.|
|Öngörüler. kapsayıcı/düğümler |memoryWorkingSetPercentage |Konağa göre bellek çalışma kümesi kullanım yüzdesi.|
|Öngörüler. kapsayıcı/düğümler |nodesCount |Duruma göre düğüm sayısı.|
|Öngörüler. kapsayıcı/düğümler |diskUsedPercentage |Düğümde cihaz tarafından kullanılan disk yüzdesi.|
|Öngörüler. kapsayıcı/pods |Pod sayısı |Denetleyici, ad alanı, düğüm ve aşamaya göre Pod sayısı.|
|Öngörüler. kapsayıcı/pods |completedJobsCount |Tamamlanan İşler, denetleyici, Kubernetes ad alanı tarafından daha eski kullanıcı yapılandırılabilir eşiğini (varsayılan altı saat) sayar. |
|Öngörüler. kapsayıcı/pods |restartingContainerCount |Denetleyici, Kubernetes ad alanı tarafından yeniden başlatmalar sayısı.|
|Öngörüler. kapsayıcı/pods |oomKilledContainerCount |Controller, Kubernetes ad alanı tarafından, Oomsonlandırılan kapsayıcıların sayısı.|
|Öngörüler. kapsayıcı/pods |Pod Readypercentage |Denetleyici, Kubernetes ad alanı için hazır durumda Pod yüzdesi.|
|Öngörüler. kapsayıcı/kapsayıcılar |cpuExceededPercentage |Kapsayıcı adı, denetleyici adı, Kubernetes ad alanı, Pod adı ve kullanıcı yapılandırılabilir eşiğini (varsayılan olarak 95,0) aşan kapsayıcılar için CPU kullanım yüzdesi.<br> Toplanıyor  |
|Öngörüler. kapsayıcı/kapsayıcılar |memoryRssExceededPercentage |Kapsayıcı adı, denetleyici adı, Kubernetes ad alanı, Pod adı ve kullanıcı yapılandırılabilir eşiğini (varsayılan olarak 95,0) aşan kapsayıcılar için bellek RSS yüzdesi.|
|Öngörüler. kapsayıcı/kapsayıcılar |memoryWorkingSetExceededPercentage |Kapsayıcı adı, denetleyici adı, Kubernetes ad alanı, Pod adı ve kullanıcı yapılandırılabilir eşiğini (varsayılan olarak 95,0) aşan kapsayıcılar için bellek çalışma kümesi yüzdesi.|

## <a name="enable-alert-rules"></a>Uyarı kurallarını etkinleştir

Azure portal Azure Izleyici 'de ölçüm uyarılarını etkinleştirmek için bu adımları izleyin. Kaynak Yöneticisi şablonu kullanmayı etkinleştirmek için, bkz. [Kaynak Yöneticisi şablonuyla etkinleştirme](#enable-with-a-resource-manager-template).

### <a name="from-the-azure-portal"></a>Azure portalından

Bu bölümde, Azure portal kapsayıcılar ölçüm Uyarısı (Önizleme) için Azure Izleyici 'nin etkinleştirilmesi gösterilmektedir.

1. [Azure portalında](https://portal.azure.com/) oturum açın.

2. Kapsayıcılar için Azure Izleyici ölçümleri Uyarısı (Önizleme) özelliğine erişim, Azure portal sol bölmeden **Öngörüler** ' i seçerek doğrudan bir aks kümesinden edinilebilir.

3. Komut çubuğundan **Önerilen uyarılar**' ı seçin.

    ![Kapsayıcılar için Azure Izleyici 'de önerilen uyarılar seçeneği](./media/container-insights-metric-alerts/command-bar-recommended-alerts.png)

4. **Önerilen uyarılar** özellik bölmesi otomatik olarak sayfanın sağ tarafında görüntülenir. Varsayılan olarak, listedeki tüm uyarı kuralları devre dışıdır. **Etkinleştir**' i seçtikten sonra, uyarı kuralı oluşturulur ve kural adı, uyarı kaynağına bir bağlantı içerecek şekilde güncelleştirilir.

    ![Önerilen uyarılar Özellikler bölmesi](./media/container-insights-metric-alerts/recommended-alerts-pane.png)

    Uyarıyı etkinleştirmek için **Etkinleştir/devre dışı bırak** seçeneğini belirledikten sonra bir uyarı kuralı oluşturulur ve kural adı gerçek uyarı kaynağına bir bağlantı içerecek şekilde güncelleştirilir.

    ![Uyarı kuralını etkinleştirme](./media/container-insights-metric-alerts/recommended-alerts-pane-enable.png)

5. Uyarı kuralları, kullanıcılara bir uyarının tetiklendiğini bildirmek için bir [eylem grubuyla](../platform/action-groups.md) ilişkili değildir. **Atanmış eylem grubu yok** ' u seçin ve **eylem grupları** sayfasında, **Ekle** veya **Oluştur**' u seçerek mevcut bir eylem grubu belirtin veya bir eylem grubu oluşturun.

    ![Bir eylem grubu seçin](./media/container-insights-metric-alerts/select-action-group.png)

### <a name="enable-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla etkinleştirin

Azure Izleyici 'de dahil edilen ölçüm uyarılarını oluşturmak için bir Azure Resource Manager şablonu ve parametreler dosyası kullanabilirsiniz.

Temel adımlar aşağıdaki gibidir:

1. [GitHub](https://github.com/microsoft/Docker-Provider/tree/ci_dev/alerts/recommended_alerts_ARM)'dan uyarının nasıl oluşturulacağını betimleyen kullanılabilir şablonların birini veya tümünü indirin.

2. Uyarı kuralını oluşturmak için gereken değerleri ayarlamak için bir [parametre dosyasını](../../azure-resource-manager/templates/parameter-files.md) JSON olarak oluşturun ve kullanın.

3. Azure portal, PowerShell veya Azure CLı 'dan şablonu dağıtın.

#### <a name="deploy-through-azure-portal"></a>Azure portal üzerinden dağıtma

1. Aşağıdaki komutları kullanarak uyarı kuralı oluşturmak için bir yerel klasöre, Azure Resource Manager şablonu ve parametre dosyasına indirip kaydedin:

2. Portal üzerinden özelleştirilmiş bir şablon dağıtmak için [Azure Portal](https://portal.azure.com) **kaynak oluştur** ' u seçin.

3. **Şablon**araması yapın ve ardından **şablon dağıtımı**' yi seçin.

4. **Oluştur**’u seçin.

5. Şablon oluşturmak için çeşitli seçenekler görürsünüz, **düzenleyicide kendi şablonunuzu oluşturun**seçeneğini belirleyin.

6. **Şablonu Düzenle sayfasında** **Dosya Yükle** ' yi seçin ve şablon dosyasını seçin.

7. **Şablonu Düzenle** sayfasında **Kaydet**' i seçin.

8. **Özel dağıtım** sayfasında, aşağıdakileri belirtin ve ardından tamamlandığında, şablonu dağıtmak ve uyarı kuralını oluşturmak Için **satın al** ' ı seçin.

    * Kaynak grubu
    * Konum
    * Uyarı Adı
    * Küme kaynak KIMLIĞI

#### <a name="deploy-with-azure-powershell-or-cli"></a>Azure PowerShell veya CLı ile dağıtma

1. Aşağıdaki komutları kullanarak uyarı kuralı oluşturmak için bir yerel klasöre, Azure Resource Manager şablonu ve parametre dosyasına indirip kaydedin:

2. PowerShell veya Azure CLı kullanarak şablon ve parametreler dosyasını kullanarak ölçüm uyarısı oluşturabilirsiniz.

    Azure PowerShell’i kullanma

    ```powershell
    Connect-AzAccount

    Select-AzSubscription -SubscriptionName <yourSubscriptionName>
    New-AzResourceGroupDeployment -Name CIMetricAlertDeployment -ResourceGroupName ResourceGroupofTargetResource `
    -TemplateFile templateFilename.json -TemplateParameterFile templateParameterFilename.parameters.json
    ```

    Azure CLI’yı kullanma

    ```azurecli
    az login

    az group deployment create \
    --name AlertDeployment \
    --resource-group ResourceGroupofTargetResource \
    --template-file templateFileName.json \
    --parameters @templateParameterFilename.parameters.json
    ```

    >[!NOTE]
    >Ölçüm uyarısı hedef kaynağa farklı bir kaynak grubunda oluşturularken, hedef kaynağınız ile aynı kaynak grubunu kullanmanızı öneririz.

## <a name="edit-alert-rules"></a>Uyarı kurallarını Düzenle

Kapsayıcılar için Azure Izleyici uyarı kurallarını görüntüleyebilir ve yönetebilir, eşiğini düzenleyebilir veya AKS kümeniz için bir [eylem grubu](../platform/action-groups.md) yapılandırabilirsiniz. Azure portal ve Azure CLı 'den bu işlemleri gerçekleştirebilmeniz mümkün olsa da, kapsayıcılar için Azure Izleyici 'deki AKS kümenizdeki doğrudan de yapılabilir.

1. Komut çubuğundan **Önerilen uyarılar**' ı seçin.

2. Eşiği değiştirmek için **Önerilen uyarılar** bölmesinde, etkin uyarıyı seçin. **Düzenle kuralında**, düzenlemek istediğiniz **uyarı ölçütünü** seçin.

    * Uyarı kuralı eşiğini değiştirmek için **koşulu**seçin.
    * Var olan veya bir eylem grubu oluşturmak için, **eylem grubu** altında **Ekle** veya **Oluştur** ' u seçin.

Etkin kurallar için oluşturulan uyarıları görüntülemek için **Önerilen uyarılar** bölmesinde **uyarılarda görüntüle**' yi seçin. Zaten kümeniz için oluşturulmuş olan tüm uyarıları görebileceğiniz AKS kümesinin uyarı menüsüne yönlendirilirsiniz.

## <a name="configure-alertable-metrics-in-configmaps"></a>ConfigMaps 'ta alertable ölçümlerini yapılandırma

ConfigMap yapılandırma dosyanızı varsayılan kapsayıcı kaynak kullanımı eşiklerini geçersiz kılmak üzere yapılandırmak için aşağıdaki adımları gerçekleştirin. Bu adımlar yalnızca aşağıdaki alertable ölçümleri için geçerlidir.

* *cpuExceededPercentage*
* *memoryRssExceededPercentage*
* *memoryWorkingSetExceededPercentage*

1. Bölümünde ConfigMap YAML dosyasını düzenleyin `[alertable_metrics_configuration_settings.container_resource_utilization_thresholds]` .

2. *Cpuexceededpercentage* eşiğini %90 olarak değiştirmek ve bu ölçümün karşılanması ve aşılması durumunda bu metriğin toplanmasını başlamak için aşağıdaki örneği kullanarak configmap dosyasını yapılandırın.

    ```
    container_cpu_threshold_percentage = 90.0
    # Threshold for container memoryRss, metric will be sent only when memory rss exceeds or becomes equal to the following percentage
    container_memory_rss_threshold_percentage = 95.0
    # Threshold for container memoryWorkingSet, metric will be sent only when memory working set exceeds or becomes equal to the following percentage
    container_memory_working_set_threshold_percentage = 95.0
    ```

3. Şu kubectl komutunu çalıştırın: `kubectl apply -f <configmap_yaml_file.yaml>` .

    Örnek: `kubectl apply -f container-azm-ms-agentconfig.yaml`.

Yapılandırma değişikliğinin, yürürlüğe girmeden önce tamamlanması birkaç dakika sürebilir ve kümedeki tüm omsagent 'lar yeniden başlatılır. Yeniden başlatma, tüm omsagent pods için aynı anda yeniden başlatma işlemi için bir yeniden başlatma işlemi yapılır. Yeniden başlatmalar tamamlandığında, aşağıdakine benzer bir ileti görüntülenir ve sonucu içerir: `configmap "container-azm-ms-agentconfig" created` .

## <a name="next-steps"></a>Sonraki adımlar

- Daha önceden tanımlanmış sorguları ve Uyarıları izlemek için değerlendirmek veya özelleştirmek üzere ön tanımlı sorguları ve örnekleri görmek için [günlük sorgusu örneklerini](container-insights-log-search.md#search-logs-to-analyze-data) görüntüleyin, kümelerinizi görselleştirmeyi veya çözümlemeyi inceleyin.

- Azure Izleyici hakkında daha fazla bilgi edinmek ve Kubernetes kümenizin diğer yönlerini izlemek için bkz. [Kubernetes kümesi performansını görüntüleme](container-insights-analyze.md).
