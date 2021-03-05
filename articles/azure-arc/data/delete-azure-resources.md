---
title: Azure'dan kaynakları silme
description: Azure'dan kaynakları silme
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: 60c5ddcc67db6e4a0649458cfbd5c2949aa9a32a
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102202051"
---
# <a name="delete-resources-from-azure"></a>Azure'dan kaynakları silme

> [!NOTE]
>  Bu makaledeki kaynakları silmeye yönelik seçenekler geri alınamaz!

> [!NOTE]
>  Şu anda Azure Arc etkin veri Hizmetleri için sunulan tek bağlantı modu, dolaylı olarak bağlı mod olduğundan, Kubernetes 'ten bir örneği silmek, Azure 'dan kaldırmayacak ve bir örneği Azure 'dan silmeden Kubernetes 'ten kaldırmayacaktır.  Şu anda bir kaynağı silmek iki adımlı bir işlemdir ve bu, gelecekte iyileştirilerek sunulacaktır.  İleri, Kubernetes, Truth kaynağı olacak ve Azure onu yansıtacak şekilde güncelleştirilecektir.

Bazı durumlarda, Azure Arc etkin veri Hizmetleri kaynaklarını Azure Resource Manager (ARM) içinde el ile silmeniz gerekebilir.  Aşağıdaki seçeneklerden herhangi birini kullanarak bu kaynakları silebilirsiniz.

- [Azure'dan kaynakları silme](#delete-resources-from-azure)
  - [Kaynak grubunun tamamını silme](#delete-an-entire-resource-group)
  - [Kaynak grubundaki belirli kaynakları Sil](#delete-specific-resources-in-the-resource-group)
  - [Azure CLı kullanarak kaynakları silme](#delete-resources-using-the-azure-cli)
    - [Azure CLı kullanarak SQL yönetilen örnek kaynaklarını silme](#delete-sql-managed-instance-resources-using-the-azure-cli)
    - [Azure CLı kullanarak PostgreSQL hiper ölçek sunucu grubu kaynaklarını silme](#delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli)
    - [Azure CLı 'yı kullanarak Azure Arc veri denetleyicisi kaynaklarını silme](#delete-azure-arc-data-controller-resources-using-the-azure-cli)
    - [Azure CLı kullanarak bir kaynak grubunu silme](#delete-a-resource-group-using-the-azure-cli)

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="delete-an-entire-resource-group"></a>Kaynak grubunun tamamını silme
Azure Arc etkin veri Hizmetleri için belirli ve ayrılmış bir kaynak grubu kullanıyorsanız ve kaynak grubunun içindeki *her şeyi* silmek istiyorsanız, içindeki her şeyi silecek kaynak grubunu silebilirsiniz.  

Azure portal bir kaynak grubunu, aşağıdakileri yaparak silebilirsiniz:

- Azure Arc 'ın etkinleştirildiği veri Hizmetleri kaynaklarının oluşturulduğu Azure portal kaynak grubuna gidin.
- **Kaynak grubunu sil** düğmesine tıklayın.
- Kaynak grubu adını girerek silmeyi onaylayın ve **Sil**' e tıklayın.

## <a name="delete-specific-resources-in-the-resource-group"></a>Kaynak grubundaki belirli kaynakları Sil

Aşağıdakileri yaparak Azure portal bir kaynak grubundaki belirli Azure yay etkin veri Hizmetleri kaynaklarını silebilirsiniz:

- Azure Arc 'ın etkinleştirildiği veri Hizmetleri kaynaklarının oluşturulduğu Azure portal kaynak grubuna gidin.
- Silinecek tüm kaynakları seçin.
- Sil düğmesine tıklayın.
- ' Evet ' yazarak silmeyi onaylayın ve **Sil**' e tıklayın.

## <a name="delete-resources-using-the-azure-cli"></a>Azure CLı kullanarak kaynakları silme

Azure CLı 'yı kullanarak belirli Azure Arc etkin veri Hizmetleri kaynaklarını silebilirsiniz.

### <a name="delete-sql-managed-instance-resources-using-the-azure-cli"></a>Azure CLı kullanarak SQL yönetilen örnek kaynaklarını silme

Azure CLı kullanarak Azure 'dan SQL yönetilen örnek kaynaklarını silmek için aşağıdaki komutta yer tutucu değerlerini değiştirin ve çalıştırın.

```azurecli
az resource delete --name <sql instance name> --resource-type Microsoft.AzureArcData/sqlManagedInstances --resource-group <resource group name>

#Example
#az resource delete --name sql1 --resource-type Microsoft.AzureArcData/sqlManagedInstances --resource-group rg1
```

### <a name="delete-postgresql-hyperscale-server-group-resources-using-the-azure-cli"></a>Azure CLı kullanarak PostgreSQL hiper ölçek sunucu grubu kaynaklarını silme

Azure CLı kullanarak Azure 'dan bir PostgreSQL hiper ölçek sunucu grubu kaynağını silmek için aşağıdaki komutta yer tutucu değerlerini değiştirin ve çalıştırın.

```azurecli
az resource delete --name <postgresql instance name> --resource-type Microsoft.AzureArcData/postgresInstances --resource-group <resource group name>

#Example
#az resource delete --name pg1 --resource-type Microsoft.AzureArcData/postgresInstances --resource-group rg1
```

### <a name="delete-azure-arc-data-controller-resources-using-the-azure-cli"></a>Azure CLı 'yı kullanarak Azure Arc veri denetleyicisi kaynaklarını silme

> [!NOTE]
> Bir Azure Arc veri denetleyicisi silinmeden önce, yönettiği tüm veritabanı örneği kaynaklarını silmelisiniz.

Azure CLı kullanarak Azure 'da bir Azure Arc veri denetleyicisini silmek için aşağıdaki komutta yer tutucu değerlerini değiştirin ve çalıştırın.

```azurecli
az resource delete --name <data controller name> --resource-type Microsoft.AzureArcData/dataControllers --resource-group <resource group name>

#Example
#az resource delete --name dc1 --resource-type Microsoft.AzureArcData/dataControllers --resource-group rg1
```

### <a name="delete-a-resource-group-using-the-azure-cli"></a>Azure CLı kullanarak bir kaynak grubunu silme

Ayrıca, [bir kaynak grubunu silmek](../../azure-resource-manager/management/delete-resource-group.md)IÇIN Azure CLI 'yi de kullanabilirsiniz.