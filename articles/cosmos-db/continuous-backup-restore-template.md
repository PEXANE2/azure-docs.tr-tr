---
title: Azure Cosmos DB ' de sürekli yedeklemeyi ve zaman içinde geri yüklemeyi yapılandırmak için ARM şablonunu kullanın.
description: Azure Resource Manager şablonları kullanarak sürekli yedekleme ve verileri geri yükleme ile bir hesap sağlamayı öğrenin.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 02/01/2021
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 6c388a08a589cc89d83b7178e31e3f4497b924bb
ms.sourcegitcommit: ea822acf5b7141d26a3776d7ed59630bf7ac9532
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2021
ms.locfileid: "99527810"
---
# <a name="configure-and-manage-continuous-backup-and-point-in-time-restore---using-azure-resource-manager-templates"></a>Sürekli yedeklemeyi yapılandırma ve yönetme zaman geri yükleme-Azure Resource Manager şablonları kullanma
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

Azure Cosmos DB zaman içinde geri yükleme özelliği, bir kapsayıcı içindeki yanlışlıkla yapılan bir değişikliği kurtarmanıza, silinen bir hesabı, veritabanını veya kapsayıcıyı geri yüklemeyi ya da herhangi bir bölgeye geri yüklemeyi (yedeklemelerin varolduğu) sağlar. Sürekli yedekleme modu, son 30 gün içinde herhangi bir zaman noktasına geri yükleme yapmanıza olanak sağlar.

Bu makalede, Azure Resource Manager şablonları kullanarak sürekli yedekleme ve verileri geri yükleme ile bir hesabın nasıl sağlanacağı açıklanır.

## <a name="provision-an-account-with-continuous-backup"></a><a id="provision"></a>Sürekli yedekleme ile hesap sağlama

Azure Cosmos DB bir hesabı sürekli mod ile dağıtmak için Azure Resource Manager şablonları kullanabilirsiniz. Bir hesap sağlamak için şablonu tanımlarken, aşağıdaki örnekte gösterildiği gibi "backupPolicy" parametresini ekleyin:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "ademo-pitr1",
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "apiVersion": "2016-03-31",
      "location": "West US",
      "properties": {
        "locations": [
          {
            "locationName": "West US"
          }
        ],
        "backupPolicy": {
          "type": "Continuous"
        },
        "databaseAccountOfferType": "Standard"
      }
    }
  ]
}
```

Daha sonra Azure PowerShell veya CLı kullanarak şablonu dağıtın. Aşağıdaki örnek, bir CLı komutuyla şablonun nasıl dağıtılacağını göstermektedir:

```azurecli-interactive
az group deployment create -g <ResourceGroup> --template-file <ProvisionTemplateFilePath>
```

## <a name="restore-using-the-resource-manager-template"></a><a id="restore"></a>Kaynak Yöneticisi şablonunu kullanarak geri yükleme

Kaynak Yöneticisi şablonu kullanarak da bir hesabı geri yükleyebilirsiniz. Şablonu tanımlarken aşağıdaki parametreleri içerir:

* "CreateMode" parametresini "geri yükle" olarak ayarlayın
* "RestoreParameters" tanımlayın, "restoreSource" değerinin, `az cosmosdb restorable-database-account list` kaynak hesabınız için komutun çıktısından ayıklandığına dikkat edin. Hesap adınızın örnek KIMLIĞI özniteliği geri yüklemeyi yapmak için kullanılır.
* "RestoreMode" parametresini "PointInTime" olarak ayarlayın ve "Restoretimestamp ınutc" değerini yapılandırın.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "vinhpitrarmrestore-kal3",
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "apiVersion": "2016-03-31",
      "location": "West US",
      "properties": {
        "locations": [
          {
            "locationName": "West US"
          }
        ],
        "databaseAccountOfferType": "Standard",
        "createMode": "Restore",
        "restoreParameters": {
            "restoreSource": "/subscriptions/2296c272-5d55-40d9-bc05-4d56dc2d7588/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/6a18ecb8-88c2-4005-8dce-07b44b9741df",
            "restoreMode": "PointInTime",
            "restoreTimestampInUtc": "6/24/2020 4:01:48 AM"
        }
      }
    }
  ]
}
```

Daha sonra Azure PowerShell veya CLı kullanarak şablonu dağıtın. Aşağıdaki örnek, bir CLı komutuyla şablonun nasıl dağıtılacağını göstermektedir:  

```azurecli-interactive
az group deployment create -g <ResourceGroup> --template-file <RestoreTemplateFilePath> 
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLI](continuous-backup-restore-command-line.md), [PowerShell](continuous-backup-restore-command-line.md)veya [Azure Portal](continuous-backup-restore-portal.md)kullanarak sürekli yedeklemeyi yapılandırın ve yönetin.
* [Sürekli yedekleme modunun kaynak modeli](continuous-backup-restore-resource-model.md)
* Sürekli yedekleme moduyla verileri geri yüklemek için gereken [Izinleri yönetin](continuous-backup-restore-permissions.md) .