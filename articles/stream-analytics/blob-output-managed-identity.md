---
title: Yönetilen Kimlik Azure Akışı Analizi ile blob çıktısını doğrula
description: Bu makalede, Azure Akış Analizi işinizi Azure Blob depolama çıktısına doğrulamak için yönetilen kimliklerin nasıl kullanılacağı açıklanmaktadır.
author: cedarbaum
ms.author: sacedarb
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: 13f48a9e0bc3ed8f8c4d5f1b7da4b6c03f54cdf8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79129981"
---
# <a name="use-managed-identity-to-authenticate-your-azure-stream-analytics-job-to-azure-blob-storage-output"></a>Azure Akış Analizi işinizi Azure Blob Depolama çıktısına doğrulamak için Yönetilen Kimliği kullanın

Azure Blob depolamasına çıktı için [yönetilen Kimlik kimlik doğrulaması,](../active-directory/managed-identities-azure-resources/overview.md) Stream Analytics iş yerlerine bağlantı dizesini kullanmak yerine doğrudan bir depolama hesabına erişim sağlar. Geliştirilmiş güvenliğe ek olarak, bu özellik Azure'daki sanal ağdaki (VNET) bir depolama hesabına veri yazmanızı da sağlar.

Bu makalede, Azure portalı ve Azure Kaynak Yöneticisi dağıtımı aracılığıyla Bir Akış Analizi işinin Blob çıktısı için Yönetilen Kimlik(ler) nasıl etkinleştirilebilirsiniz.

## <a name="create-the-stream-analytics-job-using-the-azure-portal"></a>Azure portalını kullanarak Akış Analizi işini oluşturun

1. Azure portalında yeni bir Akış Analizi işi oluşturun veya varolan bir işi açın. Ekranın sol tarafında bulunan menü çubuğundan, **Yapıl'ın**altında bulunan **Yönetilen Kimlik'i** seçin. "Sistem le atanmış Yönetilen Kimliği Kullan" seçildiğinden emin olun ve ardından ekranın altındaki **Kaydet** düğmesini tıklatın.

   ![Akış Analizi yönetilen kimliğini yapılandırma](./media/common/stream-analytics-enable-managed-identity.png)

2. Azure Blob depolama çıkışı lavabonun çıktı özellikleri penceresinde, Kimlik Doğrulama modunu açılır pencereyi seçin ve **Yönetilen Kimlik'i**seçin. Diğer çıktı özellikleriyle ilgili bilgi için Azure [Akış Analizi'nden çıktıları anlayın'](./stream-analytics-define-outputs.md)a bakın. İşiniz bittiğinde **Kaydet**’e tıklayın.

   ![Azure Blob depolama çıktısını yapılandırma](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-blob-output-blade.png)

3. İş oluşturulduğuna göre, bu [makalenin depolama hesabı bölümünüze Akış Analizi iş erişimi verin bölümüne](#give-the-stream-analytics-job-access-to-your-storage-account) bakın.

## <a name="azure-resource-manager-deployment"></a>Azure Resource Manager dağıtımı

Azure Kaynak Yöneticisi'ni kullanmak, Akış Analizi işinizin dağıtımını tam olarak otomatikleştirmenize olanak tanır. Kaynak Yöneticisi şablonlarını Azure PowerShell veya [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanarak dağıtabilirsiniz. Aşağıdaki örneklerde Azure CLI'yi kullanın.


1. Kaynak Yöneticisi şablonunuzun kaynak bölümüne aşağıdaki özelliği ekleyerek Yönetilen Kimliğe sahip bir **Microsoft.StreamAnalytics/streamingjobs** kaynağı oluşturabilirsiniz:

    ```json
    "Identity": {
      "Type": "SystemAssigned",
    },
    ```

   Bu özellik, Azure Kaynak Yöneticisi'ne Akış Analizi işinizin kimliğini oluşturmasını ve yönetmesini söyler. Aşağıda, Yönetilen Kimlik etkinleştirilmiş bir Akış Analizi işi ve Yönetilen Kimlik kullanan bir Blob çıktı sıcağına dağıtan örnek bir Kaynak Yöneticisi şablonu verilmiştir:

    ```json
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "resources": [
            {
                "apiVersion": "2017-04-01-preview",
                "name": "MyStreamingJob",
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
                                "serialization": {
                                    "type": "JSON",
                                    "properties": {
                                        "encoding": "UTF8"
                                    }
                                },
                                "datasource":{
                                    "type":"Microsoft.Storage/Blob",
                                    "properties":{
                                        "storageAccounts": [
                                            { "accountName": "MyStorageAccount" }
                                        ],
                                        "container": "test",
                                        "pathPattern": "segment1/{date}/segment2/{time}",
                                        "dateFormat": "yyyy/MM/dd",
                                        "timeFormat": "HH",
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

    Yukarıdaki iş, aşağıdaki Azure CLI komutunu kullanarak Kaynak grubu **Örnek Grubu'na** dağıtılabilir:

    ```azurecli
    az group deployment create --resource-group ExampleGroup -template-file StreamingJob.json
    ```

2. İş oluşturulduktan sonra, işin tam tanımını almak için Azure Kaynak Yöneticisi'ni kullanabilirsiniz.

    ```azurecli
    az resource show --ids /subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.StreamAnalytics/StreamingJobs/{RESOURCE_NAME}
    ```

    Yukarıdaki komut aşağıdaki gibi bir yanıt döndürecektir:

    ```json
    {
        "id": "/subscriptions/{SUBSCRIPTION_ID}/resourceGroups/{RESOURCE_GROUP}/providers/Microsoft.StreamAnalytics/streamingjobs/{RESOURCE_NAME}",
        "identity": {
            "principalId": "{PRINCIPAL_ID}",
            "tenantId": "{TENANT_ID}",
            "type": "SystemAssigned",
            "userAssignedIdentities": null
        },
        "kind": null,
        "location": "West US",
        "managedBy": null,
        "name": "{RESOURCE_NAME}",
        "plan": null,
        "properties": {
            "compatibilityLevel": "1.0",
            "createdDate": "2019-07-12T03:11:30.39Z",
            "dataLocale": "en-US",
            "eventsLateArrivalMaxDelayInSeconds": 5,
            "jobId": "{JOB_ID}",
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
        "resourceGroup": "{RESOURCE_GROUP}",
        "sku": null,
        "tags": null,
        "type": "Microsoft.StreamAnalytics/streamingjobs"
    }
    ```

   İşinizin Azure Etkin Dizini içinde Yönetilen Kimliğini tanımlayan ve akış Analizi iş erişimini depolama hesabına vermek için bir sonraki adımda kullanılacak olan işin tanımından **principalId'e** dikkat edin.

3. İş oluşturulduğuna göre, bu [makalenin depolama hesabı bölümünüze Akış Analizi iş erişimi verin bölümüne](#give-the-stream-analytics-job-access-to-your-storage-account) bakın.


## <a name="give-the-stream-analytics-job-access-to-your-storage-account"></a>Akış Analizi'ne depolama hesabınıza iş erişimi verme

Akış Analizi işinizi vermeyi seçebileceğiniz iki erişim düzeyi vardır:

1. **Kapsayıcı düzeyi erişimi:** Bu seçenek, iş için belirli bir varolan kapsayıcıya erişim sağlar.
2. **Hesap düzeyi erişimi:** Bu seçenek, yeni kapsayıcılar oluşturma olanağı da dahil olmak üzere, iş alanına depolama hesabına genel erişim sağlar.

Sizin adınıza kapsayıcılar oluşturmak için işe ihtiyacınız yoksa, bu seçenek işe gereken minimum erişim düzeyini verecektir, çünkü **Kapsayıcı düzeyi erişimini** seçmeniz gerekir. Azure portalı ve komut satırı için her iki seçenek de aşağıda açıklanmıştır.

### <a name="grant-access-via-the-azure-portal"></a>Azure portalı üzerinden erişim izni verme

#### <a name="container-level-access"></a>Konteyner düzeyinde erişim

1. Depolama hesabınızdaki kapsayıcının yapılandırma bölmesine gidin.

2. Sol tarafta **KiErişim Denetimi'ni (IAM)** seçin.

3. "Rol ataması ekle" bölümünün altında **Ekle'yi**tıklatın.

4. Rol atama bölmesinde:

    1. **Rolü** "Depolama Blob Veri Katkıda Bulunan" olarak ayarlama
    2. Açılan **girişe Atama erişiminin** "Azure AD kullanıcısı, grubu veya hizmet sorumlusu" olarak ayarlandığından emin olun.
    3. Arama alanına Stream Analytics işünüzün adını yazın.
    4. Akış Analizi işinizi seçin ve **Kaydet'i**tıklatın.

   ![Konteyner erişimini hibe](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-container-access-portal.png)

#### <a name="account-level-access"></a>Hesap düzeyi erişimi

1. Depolama hesabınıza gidin.

2. Sol tarafta **KiErişim Denetimi'ni (IAM)** seçin.

3. "Rol ataması ekle" bölümünün altında **Ekle'yi**tıklatın.

4. Rol atama bölmesinde:

    1. **Rolü** "Depolama Blob Veri Katkıda Bulunan" olarak ayarlama
    2. Açılan **girişe Atama erişiminin** "Azure AD kullanıcısı, grubu veya hizmet sorumlusu" olarak ayarlandığından emin olun.
    3. Arama alanına Stream Analytics işünüzün adını yazın.
    4. Akış Analizi işinizi seçin ve **Kaydet'i**tıklatın.

   ![Hesap erişimini hibe](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-account-access-portal.png)

### <a name="grant-access-via-the-command-line"></a>Komut satırı üzerinden erişim izni verme

#### <a name="container-level-access"></a>Konteyner düzeyinde erişim

Belirli bir kapsayıcıya erişim sağlamak için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın:

   ```azurecli
   az role assignment create --role "Storage Blob Data Contributor" --assignee <principal-id> --scope /subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/blobServices/default/containers/<container-name>
   ```

#### <a name="account-level-access"></a>Hesap düzeyi erişimi

Hesabın tamamına erişim sağlamak için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın:

   ```azurecli
   az role assignment create --role "Storage Blob Data Contributor" --assignee <principal-id> --scope /subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>
   ```

## <a name="enable-vnet-access"></a>VNET erişimini etkinleştirme

Depolama hesabınızın **Güvenlik Duvarlarını ve sanal ağlarını**yapılandırırken, isteğe bağlı olarak diğer güvenilen Microsoft hizmetlerinden ağ trafiğine izin verebilirsiniz. Akış Analizi Yönetilen Kimlik kullanarak kimlik doğrulaması yaptığında, isteğin güvenilir bir hizmetten kaynaklandığına dair kanıt sağlar. Aşağıda bu VNET erişim özel durum etkinleştirmek için talimatlar verilmiştir.

1.  Depolama hesabının yapılandırma bölmesindeki "Güvenlik Duvarları ve sanal ağlar" bölmesine gidin.
2.  "Güvenilen Microsoft hizmetlerine bu depolama hesabına erişmesine izin ver" seçeneğinin etkinleştirildiğinden emin olun.
3.  Etkinleştirdiyseniz **Kaydet'i**tıklatın.

   ![VNET erişimini etkinleştirme](./media/stream-analytics-managed-identities-blob-output-preview/stream-analytics-vnet-exception.png)

## <a name="limitations"></a>Sınırlamalar
Aşağıda bu özelliğin geçerli sınırlamaları verilmiştir:

1. Klasik Azure Depolama hesapları.

2. Azure Etkin Dizini olmayan Azure hesapları.

3. Çok kiracılı erişim desteklenmez. Belirli bir Akış Analizi işi için oluşturulan Hizmet ilkesi, işin oluşturulduğu aynı Azure Etkin Dizin kiracısında ikamet etmeli ve farklı bir Azure Etkin Dizin kiracısında bulunan bir kaynakla kullanılamaz.

4. [Kullanıcı Atanan Kimlik](../active-directory/managed-identities-azure-resources/overview.md) desteklenmez. Bu, kullanıcının Stream Analytics işi tarafından kullanılmak üzere kendi hizmet ilkesini giremeyecekleri anlamına gelir. Hizmet sorumlusu Azure Akış Analizi tarafından oluşturulmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Akış Analitiği'nden çıktıları anlama](./stream-analytics-define-outputs.md)
* [Azure Stream Analytics özel blob çıktı bölümleme](./stream-analytics-custom-path-patterns-blob-storage-output.md)
