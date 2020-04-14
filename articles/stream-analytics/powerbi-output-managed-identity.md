---
title: Azure Akış Analizi işinizi Power BI çıktısına doğrulamak için Yönetilen Kimliği kullanın
description: Bu makalede, Azure Akış Analizi işinizi Power BI çıktısına doğrulamak için yönetilen kimliklerin nasıl kullanılacağı açıklanmaktadır.
author: cedarbaum
ms.author: sacedarb
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 3/10/2020
ms.openlocfilehash: 31a5195038ef25acadc08e2acbedf8471b25833c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261423"
---
# <a name="use-managed-identity-to-authenticate-your-azure-stream-analytics-job-to-power-bi"></a>Azure Akış Analizi işinizi Power BI için doğrulamak için Yönetilen Kimliği kullanın

Power BI çıktısı için [yönetilen Kimlik kimlik doğrulaması,](../active-directory/managed-identities-azure-resources/overview.md) Stream Analytics iş yerlerine Power BI hesabınızdaki bir çalışma alanına doğrudan erişim sağlar. Bu özellik, bir kullanıcının Azure portalı üzerinden Power BI'de etkileşimli olarak oturum açması artık gerekmediğinden, Akış Analizi işlerinin dağıtımlarının tam otomatik olmasını sağlar. Ayrıca, Power BI'ye yazan uzun süreli işler artık daha iyi desteklenir, çünkü işi düzenli olarak yeniden yetkilendirmeniz gerekmez.

Bu makalede, Bir Akış Analizi işinin Power BI çıktısı için Yönetilen Kimlik(ler) için Azure portalı ve Azure Kaynak Yöneticisi dağıtımı aracılığıyla nasıl etkinleştirilebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu özelliği kullanmak için aşağıdakiler gereklidir:

- [Pro lisansı](https://docs.microsoft.com/power-bi/service-admin-purchasing-power-bi-pro)olan bir Power BI hesabı.

- Power BI hesabınızda yükseltilmiş bir çalışma alanı. Daha fazla bilgi için [Power BI'nin](https://powerbi.microsoft.com/blog/announcing-new-workspace-experience-general-availability-ga/) bu özelliği duyurusuna bakın.

## <a name="create-a-stream-analytics-job-using-the-azure-portal"></a>Azure portalını kullanarak Bir Akış Analizi işi oluşturun

1. Azure portalında yeni bir Akış Analizi işi oluşturun veya varolan bir işi açın. Ekranın sol tarafında bulunan menü çubuğundan, **Yapıl'ın**altında bulunan **Yönetilen Kimlik'i** seçin. "Sistem le atanmış Yönetilen Kimliği Kullan" seçeneğinin seçildiğinden emin olun ve ardından ekranın alt kısmındaki **Kaydet** düğmesini seçin.

   ![Akış Analizi yönetilen kimliğini yapılandırma](./media/common/stream-analytics-enable-managed-identity.png)

2. Çıktıyı yapılandırmadan önce, bu makalenin [Power BI çalışma alanı bölümünüze Akış Analizi iş erişimini ver'deki](#give-the-stream-analytics-job-access-to-your-power-bi-workspace) yönergeleri izleyerek Stream Analytics iş alanınıza Power BI çalışma alanınıza erişim verin.

3. Stream Analytic'inizin işinin **Çıktılar** bölümüne gidin, **+ Ekle'yi**seçin ve ardından **Power BI'yi**seçin. Ardından Yetki **düğmesini** seçin ve Power BI hesabınızla oturum açın.

   ![Power BI hesabı ile yetkilendirme](./media/stream-analytics-powerbi-output-managed-identity/stream-analytics-authorize-powerbi.png)

4. Onaylandıktan sonra, erişebildiğiniz tüm çalışma alanlarıyla birlikte bir açılır liste doldurulur. Önceki adımda yetki verdiğiniz çalışma alanını seçin. Ardından **Yönetilen Kimlik'i** "Kimlik Doğrulama modu" olarak seçin. Son olarak, **Kaydet** düğmesini seçin.

   ![Yönetilen Kimlik le Güç BI çıktısını yapılandırın](./media/stream-analytics-powerbi-output-managed-identity/stream-analytics-configure-powerbi-with-managed-id.png)

## <a name="azure-resource-manager-deployment"></a>Azure Resource Manager dağıtımı

Azure Kaynak Yöneticisi, Akış Analizi işinizin dağıtımını tam olarak otomatikleştirmenize olanak tanır. Kaynak Yöneticisi şablonlarını Azure PowerShell veya [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak dağıtabilirsiniz. Aşağıdaki örneklerde Azure CLI'yi kullanın.


1. Kaynak Yöneticisi şablonunuzun kaynak bölümüne aşağıdaki özelliği ekleyerek Yönetilen Kimliğe sahip bir **Microsoft.StreamAnalytics/streamingjobs** kaynağı oluşturabilirsiniz:

    ```json
    "identity": {
        "type": "SystemAssigned",
    }
    ```

   Bu özellik, Azure Kaynak Yöneticisi'ne Akış Analizi işinizin kimliğini oluşturmasını ve yönetmesini söyler. Aşağıda, Yönetilen Kimlik etkinleştirilmiş bir Akış Analizi işi ve Yönetilen Kimlik kullanan bir Power BI çıktısı lavabosu dağıtan örnek bir Kaynak Yöneticisi şablonu verilmiştir:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
            {
                "apiVersion": "2017-04-01-preview",
                "name": "pbi_managed_id",
                "location": "[resourceGroup().location]",
                "type": "Microsoft.StreamAnalytics/StreamingJobs",
                "identity": {
                    "type": "systemAssigned"
                },
                "properties": {
                    "sku": {
                        "name": "standard"
                    },
                    "outputs":[
                        {
                            "name":"output",
                            "properties":{
                                "datasource":{
                                    "type":"PowerBI",
                                    "properties":{
                                        "dataset": "dataset_name",
                                        "table": "table_name",
                                        "groupId": "01234567-89ab-cdef-0123-456789abcdef",
                                        "authenticationMode": "Msi"
                                    }
                                }
                            }
                        }
                    ]
                }
            }
        ]
    }
    ```

    Yukarıdaki işi aşağıdaki Azure CLI komutunu kullanarak Kaynak grubu **Örnek Grubu'na** dağıtın:

    ```azurecli
    az group deployment create --resource-group ExampleGroup -template-file StreamingJob.json
    ```

2. İş oluşturulduktan sonra, işin tam tanımını almak için Azure Kaynak Yöneticisi'ni kullanın.

    ```azurecli
    az resource show --ids /subscriptions/<subsription-id>/resourceGroups/<resource-group>/providers/Microsoft.StreamAnalytics/StreamingJobs/<resource-name>
    ```

    Yukarıdaki komut aşağıdaki gibi bir yanıt döndürecektir:

    ```json
    {
        "id": "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.StreamAnalytics/streamingjobs/<resource-name>",
        "identity": {
            "principalId": "<principal-id>",
            "tenantId": "<tenant-id>",
            "type": "SystemAssigned",
            "userAssignedIdentities": null
        },
        "kind": null,
        "location": "West US",
        "managedBy": null,
        "name": "<resource-name>",
        "plan": null,
        "properties": {
            "compatibilityLevel": "1.0",
            "createdDate": "2019-07-12T03:11:30.39Z",
            "dataLocale": "en-US",
            "eventsLateArrivalMaxDelayInSeconds": 5,
            "jobId": "<job-id>",
            "jobState": "Created",
            "jobStorageAccount": null,
            "jobType": "Cloud",
            "outputErrorPolicy": "Stop",
            "package": null,
            "provisioningState": "Succeeded",
            "sku": {
                "name": "Standard"
            }
        },
        "resourceGroup": "<resource-group>",
        "sku": null,
        "tags": null,
        "type": "Microsoft.StreamAnalytics/streamingjobs"
    }
    ```

    Stream Analytics işini Power BI çalışma alanınıza eklemek için Power BI'nin REST API'sini kullanmayı planlıyorsanız, döndürülen "principalId"e dikkat edin.

3. İş oluşturulduğuna göre, bu [makalenin Power BI çalışma alanı bölümünüze Akış Analizi iş erişimi vermeye](#give-the-stream-analytics-job-access-to-your-power-bi-workspace) devam edin.


## <a name="give-the-stream-analytics-job-access-to-your-power-bi-workspace"></a>Stream Analytics'e Power BI çalışma alanınıza iş erişimi verin

Akış Analizi işi oluşturulduğuna göre, bir Power BI çalışma alanına erişim hakkı verilebilir.

### <a name="use-the-power-bi-ui"></a>Power BI UI'ı kullanma

   > [!Note]
   > UI'yi kullanarak Stream Analytics işini Power BI çalışma alanınıza eklemek için, Power BI yönetici portalındaki **Geliştirici ayarlarında** hizmet temel erişimini de etkinleştirmeniz gerekir. Bkz. Daha fazla bilgi için [bir hizmet müdürüyle başlayın.](https://docs.microsoft.com/power-bi/developer/embed-service-principal)

1. Çalışma alanının erişim ayarlarına gidin. Daha fazla ayrıntı için bu makaleye bakın: [Çalışma alanınıza erişim verin.](https://docs.microsoft.com/power-bi/service-create-the-new-workspaces#give-access-to-your-workspace)

2. Metin kutusuna Akış Analizi işınızın adını yazın ve erişim düzeyi olarak **Katılımcı'yı** seçin.

3. **Bölmeyi Ekle** ve kapat'ı seçin.

   ![Power BI çalışma alanına Stream Analytics işini ekleme](./media/stream-analytics-powerbi-output-managed-identity/stream-analytics-add-job-to-powerbi-workspace.png)

### <a name="use-the-power-bi-powershell-cmdlets"></a>Power BI PowerShell cmdlets kullanın

1. Power BI `MicrosoftPowerBIMgmt` PowerShell cmdlets'i tökezle.

   > [!Important]
   > Lütfen cmdlets sürüm 1.0.821 veya daha sonra kullandığınızdan emin olun.

```powershell
Install-Module -Name MicrosoftPowerBIMgmt
```

2. Power BI'ye giriş yapın.

```powershell
Login-PowerBI
```

3. Akış Analizi işinizi Katılımcı olarak çalışma alanına ekleyin.

```powershell
Add-PowerBIWorkspaceUser -WorkspaceId <group-id> -PrincipalId <principal-id> -PrincipalType App -AccessRight Contributor
```

### <a name="use-the-power-bi-rest-api"></a>Power BI REST API'yi kullanın

Akış Analizi işi, doğrudan "Grup Kullanıcıekle" REST API'sını kullanarak çalışma alanına Katkıda Bulunan olarak da eklenebilir. Bu API için tam dokümantasyon burada bulunabilir: [Gruplar - Grup Kullanıcı ekle](https://docs.microsoft.com/rest/api/power-bi/groups/addgroupuser).

**Örnek İstek**
```http
POST https://api.powerbi.com/v1.0/myorg/groups/{groupId}/users
```
İstek Gövdesi
```json
{
    "groupUserAccessRight": "Contributor",
    "identifier": "<principal-id>",
    "principalType": "App"
}
```

## <a name="limitations"></a>Sınırlamalar
Aşağıda bu özelliğin sınırlamaları verilmiştir:

- Klasik Power BI çalışma alanları desteklenmez.

- Azure Etkin Dizini olmayan Azure hesapları.

- Çok kiracılı erişim desteklenmez. Belirli bir Akış Analizi işi için oluşturulan Hizmet ilkesi, işin oluşturulduğu aynı Azure Etkin Dizin kiracısında ikamet etmeli ve farklı bir Azure Etkin Dizin kiracısında bulunan bir kaynakla kullanılamaz.

- [Kullanıcı Atanan Kimlik](../active-directory/managed-identities-azure-resources/overview.md) desteklenmez. Bu, Stream Analytics işi tarafından kullanılmak üzere kendi hizmet müdürünüzü giremeyeceğiniz anlamına gelir. Hizmet sorumlusu Azure Akış Analizi tarafından oluşturulmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analizi ile Power BI pano entegrasyonu](./stream-analytics-power-bi-dashboard.md)
* [Azure Akış Analitiği'nden çıktıları anlama](./stream-analytics-define-outputs.md)
