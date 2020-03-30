---
title: Azure Monitör'de etkinlik günlüğü uyarıları oluşturma, görüntüleme ve yönetme
description: Azure portalı, Azure Kaynak Yöneticisi şablonu ve Azure PowerShell'i kullanarak etkinlik günlüğü uyarıları oluşturun.
ms.topic: conceptual
ms.subservice: alerts
ms.date: 06/25/2019
ms.openlocfilehash: bfbe2bc3ae3edf9285d3ec006ab0451f070cabd6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132400"
---
# <a name="create-view-and-manage-activity-log-alerts-by-using-azure-monitor"></a>Azure Monitör'ü kullanarak etkinlik günlüğü uyarıları oluşturma, görüntüleme ve yönetme  

## <a name="overview"></a>Genel Bakış

Etkinlik günlüğü uyarıları, uyarıda belirtilen koşullarla eşleşen yeni bir etkinlik günlüğü olayı oluştuğunda etkinleştirilen uyarılardır.

Bu uyarılar Azure kaynakları içindir ve bir Azure Kaynak Yöneticisi şablonu kullanılarak oluşturulabilir. Ayrıca Azure portalında oluşturulabilir, güncellenebilir veya silinebilir. Genellikle, Azure aboneliğinizdeki kaynaklarda belirli değişiklikler olduğunda bildirim almak için etkinlik günlüğü uyarıları oluşturursunuz. Uyarılar genellikle belirli kaynak gruplarına veya kaynaklara doğru kapsamlıdır. Örneğin, örnek kaynak grubu **myProductionResourceGroup'taki** herhangi bir sanal makine silindiğinde bilgilendirilmek isteyebilirsiniz. Veya aboneliğinizdeki bir kullanıcıya yeni roller atanmışsa bilgilendirilmek isteyebilirsiniz.

> [!IMPORTANT]
> Hizmet durumu bildirimindeki uyarılar, etkinlik günlüğü uyarısı oluşturma arabirimi üzerinden oluşturulamaz. Hizmet durumu bildirimleri oluşturma ve kullanma hakkında daha fazla bilgi edinmek için, [hizmet durumu bildirimlerinde etkinlik günlüğü uyarıları alın'a](alerts-activity-log-service-notifications.md)bakın.

Uyarı kuralları oluşturduğunuzda aşağıdakileri sağlayın:

- Kapsamdaki abonelik, uyarının oluşturulduğu abonelikten farklı değildir.
- Ölçütler, uyarının yapılandırıldığı düzey, durum, arayan, kaynak grubu, kaynak kimliği veya kaynak türü olay kategorisi olmalıdır.
- Uyarı yapılandırması JSON hiçbir "anyOf" koşulu veya iç içe koşullar var. Temel olarak, sadece bir "allOf" koşulu daha fazla "allOf" veya "anyOf" koşulları ile izin verilir.
- Kategori "yönetim" olduğunda, uyarınızda önceki ölçütlerden en az birini belirtmeniz gerekir. Etkinlik günlüklerinde her olay oluşturulduğunda etkinleştirilen bir uyarı oluşturamazsınız.

## <a name="azure-portal"></a>Azure portalında

Etkinlik günlüğü uyarı kuralları oluşturmak ve değiştirmek için Azure portalını kullanabilirsiniz. Bu deneyim, belirli ilgi çekici olaylar için sorunsuz uyarı oluşturulmasını sağlamak için bir Azure etkinlik günlüğüyle tümleştirilir.

### <a name="create-with-the-azure-portal"></a>Azure portalı ile oluşturma

Aşağıdaki yordamı kullanın.

1. Azure portalında, **Uyarıları İzle'yi** > **Alerts**seçin.
2. **Uyarılar** penceresinin sol üst köşesinde **Yeni uyarı kuralını** seçin.

     ![Yeni uyarı kuralı](media/alerts-activity-log/AlertsPreviewOption.png)

     **Oluştur kuralı** penceresi görüntülenir.

      ![Yeni uyarı kuralı seçenekleri](media/alerts-activity-log/create-new-alert-rule-options.png)

3. **Tanım uyarı koşulu**altında, aşağıdaki bilgileri sağlayın ve **Bitti'yi**seçin:

   - **Uyarı hedefi:** Yeni uyarının hedefini görüntülemek ve seçmek için kaynak türüne göre **aboneye** / Göre**Filtre Filtresi'ni**kullanın. Görüntülenen listeden kaynak veya kaynak grubunu seçin.

     > [!NOTE]
     > 
     > Etkinlik günlüğü sinyali için yalnızca [Azure Kaynak Yöneticisi](../../azure-resource-manager/management/overview.md) izlenen kaynağı, kaynak grubu veya tüm aboneliği seçebilirsiniz. 

     **Uyarı hedef örnek görünümü**

     ![Hedef seçin](media/alerts-activity-log/select-target.png)

   - **Hedef ölçütleri**altında, **ölçüt ekle'yi**seçin. Hedef için kullanılabilir tüm sinyaller görüntülenir, bu da etkinlik **günlüğünün**çeşitli kategorilerinden olanları içerir. Kategori adı **Monitör Hizmeti** adına eklenir.

   - **Tür Aktivite Günlüğü**için olası çeşitli işlemlerin görüntülenen listeden sinyali seçin.

     Bu hedef sinyal için günlük geçmişi zaman çizelgesini ve ilgili uyarı mantığını seçebilirsiniz:

     **Ölçüt ekranı ekleme**

     ![Ölçüt ekleme](media/alerts-activity-log/add-criteria.png)

     - **Geçmiş saati**: Seçili işlem için kullanılabilir olaylar son 6, 12 veya 24 saat içinde veya son hafta içinde çizilebilir.

     - **Uyarı mantığı**:

       - **Olay düzeyi**: Olayın önem düzeyi: _Verbose_, _Informational_, _Uyarı_, _Hata_, veya _Kritik_.
       - **Durum**: Olayın durumu: _Başlatıldı_, _Başarısız oldu_veya Başarılı _oldu._
       - **Başlatan Olay**: Arayarak olarak da bilinir. E-posta adresi veya işlemi gerçekleştiren kullanıcının Azure Etkin Dizin tanımlayıcısı.

       Bu örnek sinyal grafiğinde uygulanan uyarı mantığı vardır:

       ![Seçilen ölçütler](media/alerts-activity-log/criteria-selected.png)

4. **Uyarı ayrıntılarını tanımla**altında, aşağıdaki ayrıntıları sağlayın:

    - **Uyarı kuralı adı**: Yeni uyarı kuralının adı.
    - **Açıklaması**: Yeni uyarı kuralının açıklaması.
    - **Kaynak grubuna uyarı kaydet**: Bu yeni kuralı kaydetmek istediğiniz kaynak grubunu seçin.

5. Açılan menüden **Eylem grubu**altında, bu yeni uyarı kuralına atamak istediğiniz eylem grubunu belirtin. Veya [yeni bir eylem grubu oluşturun](../../azure-monitor/platform/action-groups.md) ve onu yeni kurala atayın. Yeni bir grup oluşturmak için **+ Yeni grubu**seçin.

6. Kuralları oluşturduktan sonra etkinleştirmek **için, oluşturma seçeneğinde Etkinleştir kuralı** için **Evet'i** seçin.
7. **Uyarı kuralı oluştur**’u seçin.

    Etkinlik günlüğü için yeni uyarı kuralı oluşturulur ve pencerenin sağ üst köşesinde bir onay iletisi görüntülenir.

    Bir kuralı etkinleştirebilir, devre dışı edebilir, düzenleyebilir veya silebilirsiniz. Etkinlik günlüğü kurallarını yönetme hakkında daha fazla bilgi edinin.


Etkinlik günlüğünde uyarı kurallarının oluşturulabileceği koşulları anlamak için basit bir benzetme, [Azure portalındaki etkinlik günlüğü](activity-log-view.md#azure-portal)aracılığıyla olayları keşfetmek veya filtrelemektir. Azure **Monitörü - Etkinlik günlüğü** ekranında, gerekli olayı filtreleyebilir veya bulabilir ve etkinlik **günlüğü uyarı ekle** düğmesini kullanarak bir uyarı oluşturabilirsiniz. Daha sonra daha önce gösterildiği gibi 7 adımları 4 izleyin.
    
 ![Etkinlik günlüğünden uyarı ekleme](media/alerts-activity-log/add-activity-log.png)
    

### <a name="view-and-manage-in-the-azure-portal"></a>Azure portalında görüntüleme ve yönetme

1. Azure portalında, **Uyarıları İzle'yi** > **Alerts**seçin. Pencerenin sol üst **köşesindeki uyarı kurallarını yönet'i** seçin.

    ![Uyarı kurallarını yönetin](media/alerts-activity-log/manage-alert-rules.png)

    Kullanılabilir kuralların listesi görüntülenir.

2. Değiştirmek için etkinlik günlüğü kuralını arayın.

    ![Arama etkinliği günlüğü uyarı kuralları](media/alerts-activity-log/searth-activity-log-rule-to-edit.png)

    Kullanılabilir filtreleri, _Abonelik,_ _Kaynak grubu,_ _Kaynak,_ _Sinyal türünü_veya _Durumu,_ yönetmek istediğiniz etkinlik kuralını bulmak için kullanabilirsiniz.

   > [!NOTE]
   > 
   > Yalnızca **Açıklama,** Hedef **ölçütleri**ve **Eylem gruplarını**edinebilirsiniz.

3. Kuralı seçin ve kural seçeneklerini yönetmek için çift tıklatın. Gerekli değişiklikleri yapın ve sonra **Kaydet'i**seçin.

   ![Uyarı kurallarını yönetin](media/alerts-activity-log/activity-log-rule-edit-page.png)

4. Bir kuralı etkinleştirebilir, devre dışı edebilir veya silebilirsiniz. Adım 2'de açıklandığı gibi kuralı seçtikten sonra pencerenin üst kısmındauygun seçeneği seçin.


## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Azure Kaynak Yöneticisi şablonu kullanarak bir etkinlik günlüğü uyarı kuralı oluşturmak `microsoft.insights/activityLogAlerts`için, türünün bir kaynağını oluşturursunuz. Sonra ilgili tüm özellikleri doldurun. Etkinlik günlüğü uyarı kuralını oluşturan bir şablon aşağıda verilmiştir:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "activityLogAlertName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Activity log alert."
      }
    },
    "activityLogAlertEnabled": {
      "type": "bool",
      "defaultValue": true,
      "metadata": {
        "description": "Indicates whether or not the alert is enabled."
      }
    },
    "actionGroupResourceId": {
      "type": "string",
      "metadata": {
        "description": "Resource Id for the Action group."
      }
    }
  },
  "resources": [   
    {
      "type": "Microsoft.Insights/activityLogAlerts",
      "apiVersion": "2017-04-01",
      "name": "[parameters('activityLogAlertName')]",      
      "location": "Global",
      "properties": {
        "enabled": "[parameters('activityLogAlertEnabled')]",
        "scopes": [
            "[subscription().id]"
        ],        
        "condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "operationName",
              "equals": "Microsoft.Resources/deployments/write"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        },
        "actions": {
          "actionGroups":
          [
            {
              "actionGroupId": "[parameters('actionGroupResourceId')]"
            }
          ]
        }
      }
    }
  ]
}
```
Önceki örnek JSON, örneğin, bu geçiş amacıyla sampleActivityLogAlert.json olarak kaydedilebilir ve [Azure portalında Azure Kaynak Yöneticisi](../../azure-resource-manager/templates/deploy-portal.md)kullanılarak dağıtılabilir.

Aşağıdaki alanlar, koşullar alanları için Azure Kaynak Yöneticisi şablonunda kullanabileceğiniz seçeneklerdir: "Kaynak Durumu", "Danışman" ve "Hizmet Durumu"nun özel alanları için ek özellikler alanlarına sahip olduğuna dikkat edin. 
1. resourceId: Uyarının oluşturulması gereken etkinlik günlüğü olayında etkilenen kaynağın kaynak kimliği.
2. kategori: Etkinlik günlüğü olayındaki kategorisi. Örneğin: Yönetim, ServiceHealth, ResourceHealth, Autoscale, Güvenlik, Öneri, İlke.
3. arayan: Etkinlik günlüğü olayının çalışmasını gerçekleştiren kullanıcının e-posta adresi veya Azure Etkin Dizin tanımlayıcısı.
4. düzey: Uyarının oluşturulması gereken etkinlik günlüğü olayındaki etkinlik düzeyi. Örneğin: Kritik, Hata, Uyarı, Bilgilendirme, Ayrıntılı.
5. operationName: Etkinlik günlüğü olayındaki işlemin adı. Örneğin: Microsoft.Resources/deployments/write
6. resourceGroup: Etkinlik günlüğü olayında etkilenen kaynak için kaynak grubunun adı.
7. resourceProvider: [Azure kaynak sağlayıcıları ve türleri açıklama](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fresource-providers-and-types&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373543634&sdata=4RjpTkO5jsdOgPdt%2F%2FDOlYjIFE2%2B%2BuoHq5%2F7lHpCwQw%3D&reserved=0). Kaynak sağlayıcılarını Azure hizmetleriyle eşleyen bir liste için Azure [hizmetleri için Kaynak sağlayıcılarına](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-resource-manager%2Fmanagement%2Fazure-services-resource-providers&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373553639&sdata=0ZgJPK7BYuJsRifBKFytqphMOxMrkfkEwDqgVH1g8lw%3D&reserved=0)bakın.
8. durum: Etkinlik olayında işlemin durumunu açıklayan dize. Örneğin: Başlatılan, Devam Eden, Başarılı, Başarısız, Etkin, Çözümlü
9. subStatus: Genellikle ilgili REST çağrısının HTTP durum kodu, ancak bir alt durumu açıklayan diğer dizeleri de içerebilir.   Örneğin: Tamam (HTTP Durum Kodu: 200), Oluşturuldu (HTTP Durum Kodu: 201), Kabul Edildi (HTTP Durum Kodu: 202), İçerik Yok (HTTP Durum Kodu: 204), Kötü İstek (HTTP Durum Kodu: 400), Bulunamadı (HTTP Durum Kodu: 404), Çakışma (HTTP Durum Kodu: 409), Dahili Sunucu Hata (HTTP Durum Kodu: 500), Hizmet Kullanılamıyor (HTTP Durum Kodu: 503), Ağ Geçidi Zaman Ası (HTTP Durum Kodu: 504).
10. resourceType: Olaydan etkilenen kaynağın türü. Örneğin: Microsoft.Resources/deployments

Örnek:

```json
"condition": {
          "allOf": [
            {
              "field": "category",
              "equals": "Administrative"
            },
            {
              "field": "resourceType",
              "equals": "Microsoft.Resources/deployments"
            }
          ]
        }

```
Etkinlik günlüğü alanları hakkında daha fazla bilgi [bulabilirsiniz.](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.microsoft.com%2Fen-us%2Fazure%2Fazure-monitor%2Fplatform%2Factivity-log-schema&data=02%7C01%7CNoga.Lavi%40microsoft.com%7C90b7c2308c0647c0347908d7c9a2918d%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637199572373563632&sdata=6QXLswwZgUHFXCuF%2FgOSowLzA8iOALVgvL3GMVhkYJY%3D&reserved=0)



> [!NOTE]
> Yeni etkinlik günlüğü uyarı kuralının etkin hale gelmesi 5 dakika kadar sürebilir.

## <a name="rest-api"></a>REST API 
[Azure Monitörü Etkinlik Günlüğü Uyarıları API](https://docs.microsoft.com/rest/api/monitor/activitylogalerts) bir REST API'sidir. Azure Kaynak Yöneticisi REST API ile tamamen uyumludur. PowerShell üzerinden Kaynak Yöneticisi cmdlet veya Azure CLI kullanılarak kullanılabilir.

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="deploy-the-resource-manager-template-with-powershell"></a>PowerShell ile Kaynak Yöneticisi şablonu dağıtma
Önceki Azure Kaynak [Yöneticisi şablonu](#azure-resource-manager-template) bölümünde gösterilen örnek Kaynak Yöneticisi şablonunu dağıtmak için PowerShell'i kullanmak için aşağıdaki komutu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName "myRG" -TemplateFile sampleActivityLogAlert.json -TemplateParameterFile sampleActivityLogAlert.parameters.json
```

burada örnekActivityLogAlert.parameters.json uyarı kuralı oluşturma için gerekli parametreler için sağlanan değerleri içerir.

### <a name="use-activity-log-powershell-cmdlets"></a>Aktivite günlüğü PowerShell cmdlets kullanın

Etkinlik günlüğü uyarıları özel PowerShell cmdlets mevcuttur:

- [Set-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Set-AzActivityLogAlert): Yeni bir etkinlik günlüğü uyarısı oluşturur veya varolan bir etkinlik günlüğü uyarısını güncelleştirir.
- [Get-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Get-AzActivityLogAlert): Bir veya daha fazla etkinlik günlüğü uyarı kaynağı alır.
- [Enable-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Enable-AzActivityLogAlert): Varolan bir etkinlik günlüğü uyarısı sağlar ve etiketlerini ayarlar.
- [Devre Dışı-AzAktiviteLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Disable-AzActivityLogAlert): Varolan bir etkinlik günlüğü uyarısını devre dışı kayırır ve etiketlerini ayarlar.
- [Remove-AzActivityLogAlert](https://docs.microsoft.com/powershell/module/az.monitor/Remove-AzActivityLogAlert): Etkinlik günlüğü uyarısını kaldırır.

## <a name="azure-cli"></a>Azure CLI

Etkinlik günlüğü uyarı kurallarını yönetmek için az [monitör etkinlik günlüğü uyarısı](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert) altındaki özel Azure CLI komutları kullanılabilir.

Yeni bir etkinlik günlüğü uyarı kuralı oluşturmak için aşağıdaki komutları şu sırayla kullanın:

1. [az monitör etkinlik-günlük uyarısı oluşturma](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-create): Yeni bir etkinlik günlüğü uyarı kuralı kaynağı oluşturun.
1. [az monitör etkinlik-günlük uyarı kapsamı](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/scope): Oluşturulan etkinlik günlüğü uyarı kuralı için kapsam ekleyin.
1. [az monitör etkinlik-günlük uyarı eylem grubu](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert/action-group): Etkinlik günlüğü uyarı kuralına bir eylem grubu ekleyin.

Bir etkinlik günlüğü uyarı kuralı kaynağını almak için Azure CLI komutunu az [izleme etkinlik günlüğü uyarı gösterisini](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-show
)kullanın. Kaynak grubundaki tüm etkinlik günlüğü uyarı kuralı kaynaklarını görüntülemek için [az monitör etkinlik günlüğü uyarı listesini](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-list)kullanın.
Etkinlik günlüğü uyarı kuralı kaynakları Azure CLI komutu [az monitör aktivite-günlük uyarı silme](https://docs.microsoft.com/cli/azure/monitor/activity-log/alert#az-monitor-activity-log-alert-delete)kullanılarak kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Etkinlik [günlükleri için webhook şeması](../../azure-monitor/platform/activity-log-alerts-webhook.md)hakkında bilgi edinin.
- Etkinlik [günlüklerine genel bir bakış](../../azure-monitor/platform/activity-log-alerts.md)okuyun.
- [Eylem grupları](../../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.  
- Hizmet [durumu bildirimleri](../../azure-monitor/platform/service-notifications.md)hakkında bilgi edinin.
