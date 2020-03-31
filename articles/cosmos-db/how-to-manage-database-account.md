---
title: Azure Cosmos DB'de veritabanı hesaplarını yönetmeyi öğrenin
description: Azure portalı, PowerShell, CLI ve Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB kaynaklarını nasıl yönetebilirsiniz öğrenin
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 61670d757611bd0c1dd11c389282b18edb3d7fa1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247390"
---
# <a name="manage-an-azure-cosmos-account"></a>Azure Cosmos hesabını yönetme

Bu makalede Azure portalını, Azure PowerShell’i, Azure CLI’yı ve Azure Resource Manager şablonlarını kullanarak Azure Cosmos hesabında çeşitli görevlerin nasıl yönetileceği açıklanır.

## <a name="create-an-account"></a>Hesap oluşturma

### <a name="azure-portal"></a><a id="create-database-account-via-portal"></a>Azure portalında

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

### <a name="azure-cli"></a><a id="create-database-account-via-cli"></a>Azure CLI

Lütfen [Azure CLI ile Azure Cosmos DB hesabı oluşturma](manage-with-cli.md#create-an-azure-cosmos-db-account)

### <a name="azure-powershell"></a><a id="create-database-account-via-ps"></a>Azure PowerShell

Powershell [ile Azure Cosmos DB hesabı oluşturma](manage-with-powershell.md#create-account)

### <a name="azure-resource-manager-template"></a><a id="create-database-account-via-arm-template"></a>Azure Resource Manager şablonu

Bu Azure Kaynak Yöneticisi şablonu, tutarlılık düzeyi, otomatik hata ve çoklu ana düzeyi seçmek için iki bölge ve seçenekle yapılandırılan SQL API için bir Azure Cosmos hesabı oluşturur. Bu şablonu dağıtmak için, okuma sayfasında Azure'a Dağıt'ı tıklatın, [Azure Cosmos hesabı oluşturun](https://github.com/Azure/azure-quickstart-templates/tree/master/101-cosmosdb-sql)

## <a name="addremove-regions-from-your-database-account"></a>Veritabanı hesabınızda bölge ekleme/çıkarma işlemi gerçekleştirme

### <a name="azure-portal"></a><a id="add-remove-regions-via-portal"></a>Azure portalında

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure Cosmos hesabınıza gidin ve **verileri genel olarak çoğalt** menüsünü açın.

1. Bölge eklemek için, haritadaki altıgenleri istediğiniz **+** bölgeye karşılık gelen etiketle seçin. Alternatif olarak, bölge eklemek için **+ Bölge Ekle** seçeneğini seçin ve açılan menüden bir bölge seçin.

1. Bölgeleri kaldırmak için, kontrol işaretli mavi altıgenleri seçerek bir veya daha fazla bölgeyi haritadan temizleyin. Veya sağ taraftaki bölgenin🗑yanındaki "çöp sepeti" simgesini seçin.

1. Değişikliklerinizi kaydetmek için **Tamam'ı**seçin.

   ![Bölgeler menüsü ekleme veya kaldırma](./media/how-to-manage-database-account/add-region.png)

Tek bölgeli yazma modunda yazma bölgesini kaldıramazsınız. Geçerli yazma bölgesini silebilmeniz için önce farklı bir bölgeye yük devretme gerçekleştirmeniz gerekir.

Çok bölgeli yazma modunda, en az bir bölgeniz varsa istediğiniz bölgeyi ekleyebilir veya kaldırabilirsiniz.

### <a name="azure-cli"></a><a id="add-remove-regions-via-cli"></a>Azure CLI

Azure [CLI ile bölgeler ekle veya kaldır'a](manage-with-cli.md#add-or-remove-regions) bakın

### <a name="azure-powershell"></a><a id="add-remove-regions-via-ps"></a>Azure PowerShell

Powershell [ile bölgeler ekle veya kaldır'a](manage-with-powershell.md#update-account) bakın

## <a name="configure-multiple-write-regions"></a><a id="configure-multiple-write-regions"></a>Birden fazla yazma bölgesi yapılandırma

### <a name="azure-portal"></a><a id="configure-multiple-write-regions-portal"></a>Azure portalında

Verileri **Genel Olarak Çoğaltma** sekmesini açın ve çok bölgeli yazmaları etkinleştirmek için **Etkinleştir'i** seçin. Çok bölgeli yazmaları etkinleştirdikten sonra, şu anda hesapta bulunan tüm okuma bölgeleri okuma ve yazma bölgeleri haline gelir.

![Azure Cosmos hesabı çok ekran lı ekran görüntüsü yapılandırır](./media/how-to-manage-database-account/single-to-multi-master.png)

### <a name="azure-cli"></a><a id="configure-multiple-write-regions-cli"></a>Azure CLI

Azure [CLI ile birden çok yazma yı etkinleştir meslekle](manage-with-cli.md#enable-multiple-write-regions) bakın

### <a name="azure-powershell"></a><a id="configure-multiple-write-regions-ps"></a>Azure PowerShell

Powershell [ile birden çok yazma bölgesini etkinleştir mesle](manage-with-powershell.md#multi-master)

### <a name="resource-manager-template"></a><a id="configure-multiple-write-regions-arm"></a>Kaynak Yöneticisi şablonu

Bir hesap, hesabı oluşturmak ve ayarlamak `enableMultipleWriteLocations: true`için kullanılan Kaynak Yöneticisi şablonu dağıtılarak tek ana kalıptan çoklu ana'ya geçirilebilir. Aşağıdaki Azure Kaynak Yöneticisi şablonu, iki bölge ve birden çok yazma konumu etkinleştirilmiş SQL API için bir Azure Cosmos hesabı dağıtacak en az şablondur.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "name": {
            "type": "String"
        },
        "location": {
            "type": "String",
            "defaultValue": "[resourceGroup().location]"
        },
        "primaryRegion":{
            "type":"string",
            "metadata": {
                "description": "The primary replica region for the Cosmos DB account."
            }
        },
        "secondaryRegion":{
            "type":"string",
            "metadata": {
              "description": "The secondary replica region for the Cosmos DB account."
          }
        }
    },
    "resources": [
        {
            "type": "Microsoft.DocumentDb/databaseAccounts",
            "kind": "GlobalDocumentDB",
            "name": "[parameters('name')]",
            "apiVersion": "2019-08-01",
            "location": "[parameters('location')]",
            "tags": {},
            "properties": {
                "databaseAccountOfferType": "Standard",
                "consistencyPolicy": { "defaultConsistencyLevel": "Session" },
                "locations":
                [
                    {
                        "locationName": "[parameters('primaryRegion')]",
                        "failoverPriority": 0,
                        "isZoneRedundant": false
                    },
                    {
                        "locationName": "[parameters('secondaryRegion')]",
                        "failoverPriority": 1,
                        "isZoneRedundant": false
                    }
                ],
                "enableMultipleWriteLocations": true
            }
        }
    ]
}
```

## <a name="enable-automatic-failover-for-your-azure-cosmos-account"></a><a id="automatic-failover"></a>Azure Cosmos hesabınız için otomatik yük devretmeyi etkinleştirme

Otomatik başarısız olma seçeneği, Azure Cosmos DB'nin bir bölgenin kullanılamaması halinde kullanıcı eylemi olmadan en yüksek başarısız lığa sahip bölgeye başarısız olmasını sağlar. Otomatik hata olduğunda, bölge önceliği değiştirilebilir. Hesabın otomatik olarak başarısız olmasını sağlamak için iki veya daha fazla bölgesi olmalıdır.

### <a name="azure-portal"></a><a id="enable-automatic-failover-via-portal"></a>Azure portalında

1. Azure Cosmos hesabınızdan, **verileri genel olarak çoğaltma** bölmesini açın.

2. Bölmenin üst kısmında Otomatik **Failover'ı**seçin.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Otomatik **Failover** bölmesinde, **Otomatik Failover'ı Etkinleştir'in** **AÇIYA**AYARLI OLDUĞUNDAN EMIN OLUN. 

4. **Kaydet'i**seçin.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

### <a name="azure-cli"></a><a id="enable-automatic-failover-via-cli"></a>Azure CLI

Azure [CLI ile otomatik arızayı etkinleştir mesuliyetini etkinleştir meslekle](manage-with-cli.md#enable-automatic-failover) bakın

### <a name="azure-powershell"></a><a id="enable-automatic-failover-via-ps"></a>Azure PowerShell

Powershell [ile otomatik arızayı etkinleştir mesuliye](manage-with-powershell.md#enable-automatic-failover) bakın

## <a name="set-failover-priorities-for-your-azure-cosmos-account"></a>Azure Cosmos hesabınız için yük devretme önceliklerini ayarlama

Cosmos hesabı otomatik hata için yapılandırıldıktan sonra, bölgeler için hata önceliği değiştirilebilir.

> [!IMPORTANT]
> Hesap otomatik hata için yapılandırıldıklarında yazma bölgesini (sıfırın yerine geçme önceliği) değiştiremezsiniz. Yazma bölgesini değiştirmek için otomatik başarısızolmayı devre dışı bırakıp el ile başarısız olmalısınız.

### <a name="azure-portal"></a><a id="set-failover-priorities-via-portal"></a>Azure portalında

1. Azure Cosmos hesabınızdan, **verileri genel olarak çoğaltma** bölmesini açın.

2. Bölmenin üst kısmında Otomatik **Failover'ı**seçin.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Otomatik **Failover** bölmesinde, **Otomatik Failover'ı Etkinleştir'in** **AÇIYA**AYARLI OLDUĞUNDAN EMIN OLUN.

4. Başarısız lık önceliğini değiştirmek için, okundukları bölgeleri, üzerlerinde gezinirken görünen satırın sol tarafındaki üç nokta üzerinden sürükleyin.

5. **Kaydet'i**seçin.

   ![Otomatik yük devretme portal menüsü](./media/how-to-manage-database-account/automatic-failover.png)

### <a name="azure-cli"></a><a id="set-failover-priorities-via-cli"></a>Azure CLI

Azure [CLI ile set başarısız önceliğine](manage-with-cli.md#set-failover-priority) bakın

### <a name="azure-powershell"></a><a id="set-failover-priorities-via-ps"></a>Azure PowerShell

Powershell [ile Set failover önceliğine](manage-with-powershell.md#modify-failover-priority) bakın

## <a name="perform-manual-failover-on-an-azure-cosmos-account"></a><a id="manual-failover"></a>Azure Cosmos hesabında el ile yük devretme gerçekleştirme

> [!IMPORTANT]
> Bu işlemin başarılı olabilmesi için Azure Cosmos hesabının el ile başarısız olması için yapılandırılmalıdır.

El ile hata gerçekleştirme işlemi, hesabın yazma bölgesinin (başarısız öncelik = 0) hesap için yapılandırılan başka bir bölgeye değiştirilmesini içerir.

> [!NOTE]
> Çok büyük hesaplar el ile başarısız olamaz. Azure Cosmos SDK'yı kullanan uygulamalarda, SDK bir bölgenin ne zaman kullanılamaz hale geldigini algılar, ardından SDK' da çok yönlü API kullanıyorsanız otomatik olarak bir sonraki en yakın bölgeye yönlendirir.

### <a name="azure-portal"></a><a id="enable-manual-failover-via-portal"></a>Azure portalında

1. Azure Cosmos hesabınıza gidin ve **verileri genel olarak çoğalt** menüsünü açın.

2. Menünün üst **kısmında, Manuel Failover'ı**seçin.

   ![Verileri genel olarak çoğaltma menüsü](./media/how-to-manage-database-account/replicate-data-globally.png)

3. Manuel **Failover** menüsünde yeni yazma bölgenizi seçin. Bu seçeneğin yazma bölgenizi değiştirdiğini anladığınızı belirtmek için onay kutusunu seçin.

4. Başarısız olmayı tetiklemek için **Tamam'ı**seçin.

   ![El ile yük devretme portal menüsü](./media/how-to-manage-database-account/manual-failover.png)

### <a name="azure-cli"></a><a id="enable-manual-failover-via-cli"></a>Azure CLI

Azure [CLI ile Tetikleme kılavuzu na](manage-with-cli.md#trigger-manual-failover) bakın

### <a name="azure-powershell"></a><a id="enable-manual-failover-via-ps"></a>Azure PowerShell

Powershell [ile Tetik manuel failover](manage-with-powershell.md#trigger-manual-failover) bakın

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabının yanı sıra veritabanı ve kapsayıcıların nasıl yönetilenlerine ilişkin daha fazla bilgi ve örnekler için aşağıdaki makaleleri okuyun:

* [Azure PowerShell'i kullanarak Azure Cosmos DB'yi yönetme](manage-with-powershell.md)
* [Azure CLI kullanarak Azure Cosmos DB'yi yönetme](manage-with-cli.md)
