---
title: Azure Monitör CLI hızlı başlangıç örnekleri
description: Azure Monitor özellikleri için örnek CLI komutları. Azure Monitor, uyarı bildirimleri göndermenize, yapılandırılmış telemetri verilerinin değerlerine göre web URL'lerini aramanızı ve otomatik Bulut Hizmetleri, Sanal Makineler ve Web Uygulamaları oluşturmanıza olanak tanıyan bir Microsoft Azure hizmetidir.
ms.subservice: ''
ms.topic: conceptual
ms.date: 05/16/2018
ms.openlocfilehash: 69687cee8b8a907b82f2c848242ac64d54dedb87
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248937"
---
# <a name="azure-monitor-cli-quick-start-samples"></a>Azure Monitör CLI hızlı başlangıç örnekleri
Bu makalede, Azure Monitor özelliklerine erişmenize yardımcı olmak için örnek komut satırı arabirimi (CLI) komutları gösterilmektedir. Azure Monitor, Bulut Hizmetlerini, Sanal Makineleri ve Web Uygulamalarını Otomatik Ölçeklendirmenize ve yapılandırılmış telemetri verilerinin değerlerine dayalı uyarı bildirimleri göndermenize veya web URL'lerini aramanıza olanak tanır.

## <a name="prerequisites"></a>Ön koşullar

Azure CLI'yi zaten yüklemediyseniz, [Azure CLI'yi yükleme yönergelerini](/cli/azure/install-azure-cli)izleyin. CLI'yi tarayıcınızda etkileşimli bir deneyim olarak çalıştırmak için [Azure Cloud Shell'i](/azure/cloud-shell) de kullanabilirsiniz. [Azure Monitor CLI başvurusundaki](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest)kullanılabilir tüm komutların tam başvurutarihine bakın. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma
İlk adım Azure hesabınıza giriş yapmaktır.

```azurecli
az login
```

Bu komutu çalıştırdıktan sonra, ekrandaki talimatlar aracılığıyla oturum açmanız gerekir. Tüm komutlar varsayılan aboneliğiniz bağlamında çalışır.

Geçerli aboneliğinizin ayrıntılarını listelemek için aşağıdaki komutu kullanın.

```azurecli
az account show
```

Çalışma bağlamını farklı bir aboneliğe değiştirmek için aşağıdaki komutu kullanın.

```azurecli
az account set -s <Subscription ID or name>
```

Desteklenen tüm Azure Monitor komutlarının listesini görüntülemek için aşağıdakileri gerçekleştirin.

```azurecli
az monitor -h
```

## <a name="view-activity-log-for-a-subscription"></a>Abonelik için etkinlik günlüğünü görüntüleme

Etkinlik günlüğü olaylarının listesini görüntülemek için aşağıdakileri gerçekleştirin.

```azurecli
az monitor activity-log list
```

Kullanılabilir tüm seçenekleri görüntülemek için aşağıdakileri deneyin.

```azurecli
az monitor activity-log list -h
```

Burada bir kaynak Grubu tarafından günlükleri listelemek için bir örnek

```azurecli
az monitor activity-log list --resource-group <group name>
```

Arayanın günlüklerini listelemek için örnek

```azurecli
az monitor activity-log list --caller myname@company.com
```

Bir tarih aralığında, bir kaynak türünde arayanın günlüklerini listelemek için örnek

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="work-with-alerts"></a>Uyarılarla çalışma 
> [!NOTE]
> Şu anda CLI'de yalnızca uyarılar (klasik) desteklenir. 

### <a name="get-alert-classic-rules-in-a-resource-group"></a>Kaynak grubunda uyarı (klasik) kuralları alma

```azurecli
az monitor activity-log alert list --resource-group <group name>
az monitor activity-log alert show --resource-group <group name> --name <alert name>
```

### <a name="create-a-metric-alert-classic-rule"></a>Metrik uyarı (klasik) kuralı oluşturma

```azurecli
az monitor alert create --name <alert name> --resource-group <group name> \
    --action email <email1 email2 ...> \
    --action webhook <URI> \
    --target <target object ID> \
    --condition "<METRIC> {>,>=,<,<=} <THRESHOLD> {avg,min,max,total,last} ##h##m##s"
```

### <a name="delete-an-alert-classic-rule"></a>Uyarı (klasik) kuralını silme

```azurecli
az monitor alert delete --name <alert name> --resource-group <group name>
```

## <a name="log-profiles"></a>Günlük profilleri

Günlük profilleriyle çalışmak için bu bölümdeki bilgileri kullanın.

### <a name="get-a-log-profile"></a>Günlük profili oluşturma

```azurecli
az monitor log-profiles list
az monitor log-profiles show --name <profile name>
```

### <a name="add-a-log-profile-with-retention"></a>Bekletme ile günlük profili ekleme

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true \
    --storage-account-id <storage account ID to store the logs in>
```

### <a name="add-a-log-profile-with-retention-and-eventhub"></a>Bekletme ve EventHub içeren bir günlük profili ekleme

```azurecli
az monitor log-profiles create --name <profile name> --location <location of profile> \
    --locations <locations to monitor activity in: location1 location2 ...> \
    --categories <categoryName1 categoryName2 ...> \
    --days <# days to retain> \
    --enabled true
    --storage-account-id <storage account ID to store the logs in>
    --service-bus-rule-id <service bus rule ID to stream to>
```

### <a name="remove-a-log-profile"></a>Günlük profilini kaldırma

```azurecli
az monitor log-profiles delete --name <profile name>
```

## <a name="diagnostics"></a>Tanılama

Tanılama ayarlarıyla çalışmak için bu bölümdeki bilgileri kullanın.

### <a name="get-a-diagnostic-setting"></a>Tanılama ayarı alın

```azurecli
az monitor diagnostic-settings list --resource <target resource ID>
```

### <a name="create-a-diagnostic-setting"></a>Tanılama ayarı oluşturma 

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --storage-account <storage account ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true,
        "retentionPolicy": {
            "days": <# days to retain>,
            "enabled": true
        }
    }]'
```

### <a name="delete-a-diagnostic-setting"></a>Tanılama ayarını silme

```azurecli
az monitor diagnostic-settings delete --name <diagnostic name> \
    --resource <target resource ID>
```

## <a name="autoscale"></a>Otomatik Ölçeklendirme

Otomatik ölçek ayarlarıyla çalışmak için bu bölümdeki bilgileri kullanın. Bu örnekleri değiştirmeniz gerekir.

### <a name="get-autoscale-settings-for-a-resource-group"></a>Kaynak grubu için otomatik ölçek lendirme ayarlarını alma

```azurecli
az monitor autoscale list --resource-group <group name>
```

### <a name="get-autoscale-settings-by-name-in-a-resource-group"></a>Kaynak grubunda ada göre otomatik ölçeklendirme ayarlarını alma

```azurecli
az monitor autoscale show --name <settings name> --resource-group <group name>
```

### <a name="set-autoscale-settings"></a>Otomatik ölçek ayarlarını ayarlama

```azurecli
az monitor autoscale create --name <settings name> --resource-group <group name> \
    --count <# instances> \
    --resource <target resource ID>
```

