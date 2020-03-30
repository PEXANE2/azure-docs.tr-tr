---
title: Güvenli bağlantılar sağlamak için güvenli aktarım gerektirir
titleSuffix: Azure Storage
description: Azure Depolama'daki istekler için güvenli aktarım için nasıl gerekli olduğunu öğrenin. Bir depolama hesabı için güvenli aktarım gerektirdiğinde, güvenli olmayan bir bağlantıdan kaynaklanan tüm istekler reddedilir.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 12/12/2019
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 3b2d78bd929e23d49a57f337022f6678114bb5fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75457437"
---
# <a name="require-secure-transfer-to-ensure-secure-connections"></a>Güvenli bağlantılar sağlamak için güvenli aktarım gerektirir

Depolama hesabınızı yalnızca depolama hesabı için **Güvenli aktarım gerekli** özelliğini ayarlayarak güvenli bağlantılardan gelen istekleri kabul etmek üzere yapılandırabilirsiniz. Güvenli aktarım gerektirdiğinde, güvenli olmayan bir bağlantıdan kaynaklanan tüm istekler reddedilir. Microsoft, tüm depolama hesaplarınız için her zaman güvenli aktarım aparatını gerektirmenizi önerir.

Güvenli aktarım gerektiğinde, Https üzerinden Azure Depolama REST API işlemine çağrı yapılmalıdır. HTTP üzerinden yapılan tüm istek reddedilir.

Depolama hesabı için güvenli aktarım gerektiğinde, SMB üzerinden Bir Azure Dosyası paylaşımına şifreleme olmadan bağlanmak başarısız olur. Güvenli olmayan bağlantılara örnek olarak, SMB 2.1, SMB 3.0 şifreleme olmadan yapılanlar veya Linux SMB istemcisinin bazı sürümleri verilebilir.

Varsayılan olarak, Azure portalında bir depolama hesabı oluşturduğunuzda **Güvenli aktarım özelliği** etkinleştirilir. Ancak, SDK ile bir depolama hesabı oluşturduğunuzda devre dışı bırakılır.

> [!NOTE]
> Azure Depolama özel alan adları için HTTPS'yi desteklemediği için, özel bir etki alanı adı kullanırken bu seçenek uygulanmaz. Ve klasik depolama hesapları desteklenmez.

## <a name="require-secure-transfer-in-the-azure-portal"></a>Azure portalında güvenli aktarım gerektirir

[Azure portalında](https://portal.azure.com)bir depolama hesabı oluşturduğunuzda **Güvenli aktarım özelliğini** açabilirsiniz. Varolan depolama hesapları için de etkinleştirebilirsiniz.

### <a name="require-secure-transfer-for-a-new-storage-account"></a>Yeni bir depolama hesabı için güvenli aktarım gerektirir

1. Azure portalında **depolama hesabı oluştur** bölmesini açın.
1. **Güvenli aktarım gerekli**altında , **Etkin**seçin.

   ![Depolama hesap bıçağı oluşturma](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_1.png)

### <a name="require-secure-transfer-for-an-existing-storage-account"></a>Varolan bir depolama hesabı için güvenli aktarım gerektirir

1. Azure portalında varolan bir depolama hesabı seçin.
1. DEPOLAMA hesabı menüsü bölmesinde, **AYARLAR**altında **Configuration'ı**seçin.
1. **Güvenli aktarım gerekli**altında , **Etkin**seçin.

   ![Depolama hesabı menüsü bölmesi](./media/storage-require-secure-transfer/secure_transfer_field_in_portal_en_2.png)

## <a name="require-secure-transfer-from-code"></a>Koddan güvenli aktarım gerektirir

Programlı olarak güvenli aktarım gerektirmek için, depolama hesabında _destekhttpsTrafficOnly_ özelliğini ayarlayın. Depolama Kaynak Sağlayıcısı REST API'sini, istemci kitaplıklarını veya araçlarını kullanarak bu özelliği ayarlayabilirsiniz:

* [REST API](/rest/api/storagerp/storageaccounts)
* [Powershell](/powershell/module/az.storage/set-azstorageaccount)
* [CLI](/cli/azure/storage/account)
* [NodeJS](https://www.npmjs.com/package/azure-arm-storage/)
* [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Management.Storage)
* [Python SDK'sı](https://pypi.org/project/azure-mgmt-storage)
* [Ruby SDK](https://rubygems.org/gems/azure_mgmt_storage)

## <a name="require-secure-transfer-with-powershell"></a>PowerShell ile güvenli transfer gerektirir

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu örnek, Azure PowerShell modülü Az sürüm 0.7 veya daha sonra gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-Az-ps).

Azure ile bağlantı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

 Ayarı denetlemek için aşağıdaki komut satırını kullanın:

```powershell
Get-AzStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}"
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : False
...

```

Ayarı etkinleştirmek için aşağıdaki komut satırını kullanın:

```powershell
Set-AzStorageAccount -Name "{StorageAccountName}" -ResourceGroupName "{ResourceGroupName}" -EnableHttpsTrafficOnly $True
StorageAccountName     : {StorageAccountName}
Kind                   : Storage
EnableHttpsTrafficOnly : True
...

```

## <a name="require-secure-transfer-with-azure-cli"></a>Azure CLI ile güvenli aktarım gerektirir

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 Ayarı denetlemek için aşağıdaki komutu kullanın:

```azurecli-interactive
az storage account show -g {ResourceGroupName} -n {StorageAccountName}
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": false,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

Ayarı etkinleştirmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az storage account update -g {ResourceGroupName} -n {StorageAccountName} --https-only true
{
  "name": "{StorageAccountName}",
  "enableHttpsTrafficOnly": true,
  "type": "Microsoft.Storage/storageAccounts"
  ...
}

```

## <a name="next-steps"></a>Sonraki adımlar

[Blob depolama için güvenlik önerileri](../blobs/security-recommendations.md)
