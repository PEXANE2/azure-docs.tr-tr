---
title: NFS Share-Azure dosyaları oluşturma
description: Ağ dosya sistemi protokolü kullanılarak iliştirilenebilir bir Azure dosya paylaşımının nasıl oluşturulacağını öğrenin.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 2a848cb77336fc89172d55a6204d66b9e5be5976
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90705240"
---
# <a name="how-to-create-an-nfs-share"></a>NFS paylaşma oluşturma

Azure dosya paylaşımları, bulutta yaşayan tamamen yönetilen dosya paylaşımlardır. Bunlar, sunucu Ileti bloğu Protokolü veya ağ dosya sistemi (NFS) protokolü kullanılarak erişilebilirler. Bu makalede NFS protokolünü kullanan bir dosya paylaşımının oluşturulması ele alınmaktadır. Her iki protokol hakkında daha fazla bilgi için bkz. [Azure dosya paylaşma protokolleri](storage-files-compare-protocols.md).

## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Önkoşullar

- Bir [dosya depolama hesabı](storage-how-to-create-premium-fileshare.md)oluşturun.

    > [!IMPORTANT]
    > NFS paylaşımlarına yalnızca güvenilen ağlardan erişilebilir. NFS paylaşımınızla kurulan bağlantılar aşağıdaki kaynaklardan birinden kaynaklanmalıdır:

    - [Özel uç nokta oluşturun](storage-files-networking-endpoints.md#create-a-private-endpoint) (önerilir) ya da [genel uç noktanıza erişimi kısıtlayın](storage-files-networking-endpoints.md#restrict-public-endpoint-access).
    - [Azure dosyaları ile kullanmak üzere Linux üzerinde Noktadan siteye (P2S) VPN yapılandırın](storage-files-configure-p2s-vpn-linux.md).
    - [Azure dosyaları ile kullanmak Için siteden sıteye VPN yapılandırın](storage-files-configure-s2s-vpn.md).
    - [ExpressRoute](../../expressroute/expressroute-introduction.md)'ı yapılandırın.
- Azure CLı 'yı kullanmayı planlıyorsanız [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="register-the-nfs-41-protocol"></a>NFS 4,1 protokolünü kaydetme

Azure PowerShell modülünü veya Azure CLı kullanıyorsanız, aşağıdaki komutları kullanarak özelliğinizi kaydedin:

### <a name="powershell"></a>PowerShell

```azurepowershell
Connect-AzAccount
$context = Get-AzSubscription -SubscriptionId <yourSubscriptionIDHere>
Set-AzContext $context
Register-AzProviderFeature -FeatureName AllowNfsFileShares -ProviderNamespace Microsoft.Storage
Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az login
az feature register --name AllowNfsFileShares
                    --namespace Microsoft.Storage
                    --subscription <yourSubscriptionIDHere>
az provider register --namespace Microsoft.Storage
```

## <a name="verify-that-the-feature-is-registered"></a>Özelliğin kayıtlı olduğunu doğrulayın

Kayıt onayı bir saate kadar sürebilir. Kaydın tamamlandığını doğrulamak için aşağıdaki komutları kullanın:

### <a name="powershell"></a>PowerShell

```azurepowershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNfsFileShares
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az feature show --name AllowNfsFileShares --namespace Microsoft.Storage --subscription <yourSubscriptionIDHere>
```

## <a name="create-an-nfs-share"></a>NFS paylaşma oluşturma

# <a name="portal"></a>[Portal](#tab/azure-portal)

Bir dosya depolama hesabı oluşturup ağı yapılandırdığınıza göre, bir NFS dosya paylaşma oluşturabilirsiniz. İşlem SMB paylaşımının oluşturulmasına benzer, ancak paylaşımın oluşturulması sırasında **SMB** yerine **NFS** 'yi seçersiniz.

1. Depolama hesabınıza gidin ve **dosya paylaşımları**' nı seçin.
1. Yeni bir dosya paylaşma oluşturmak için **+ dosya paylaşma** ' yı seçin.
1. Dosya paylaşımınızı adlandırın, sağlanan kapasiteyi seçin.
1. **Protokol** için **NFS (Önizleme)** seçeneğini belirleyin.
1. **Kök Squash** için bir seçim yapın.

    - Kök sıkıştırma (varsayılan)-uzak Süper Kullanıcı (root) erişimi, UID (65534) ve GID (65534) ile eşleştirilir.
    - Kök sıkıştırarak-uzak Süper Kullanıcı (root), kök olarak erişim almaz.
    - Tüm sıkıştırarak-tüm Kullanıcı erişimi UID (65534) ve GID (65534) ile eşleştirilir.
    
1. **Oluştur**’u seçin.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/create-nfs-file-share.png" alt-text="Dosya paylaşma oluşturma dikey penceresinin ekran görüntüsü":::

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. .NET Framework 'ün yüklü olduğundan emin olun. Bkz. [indirme .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).
 
1. `5.1`Aşağıdaki komutu kullanarak yüklü PowerShell sürümünün veya daha yüksek olduğunu doğrulayın.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   PowerShell sürümünüzü yükseltmek için bkz. [var olan Windows PowerShell 'ı yükseltme](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)
    
1. PowershellGet modülünün en son sürümünü yükler.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force  
   ```

1. ' I kapatın ve ardından PowerShell konsolunu yeniden açın.

1. **Az. Storage** Preview Module sürüm **2.5.2 Parallel sections-Preview**' i yükler.

   ```powershell
   Install-Module Az.Storage -Repository PsGallery -RequiredVersion 2.5.2-preview -AllowClobber -AllowPrerelease -Force  
   ```

   PowerShell modüllerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz [. Azure PowerShell modülünü Install](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)
   
1. Azure PowerShell modülüyle Premium bir dosya paylaşımı oluşturmak için [New-Azrmstoragesshare](/powershell/module/az.storage/new-azrmstorageshare) cmdlet 'ini kullanın.

> [!NOTE]
> Sağlanan paylaşım boyutları paylaşım kotası ile belirtilir, dosya paylaşımları sağlanan boyutta faturalandırılır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

  ```powershell
  New-AzRmStorageShare `
   -ResourceGroupName $resourceGroupName `
   -StorageAccountName $storageAccountName `
   -Name myshare `
   -EnabledProtocol NFS `
   -RootSquash RootSquash `
   -Context $storageAcct.Context
  ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı ile Premium dosya paylaşma oluşturmak için [az Storage Share Create](/cli/azure/storage/share-rm) komutunu kullanın.

> [!NOTE]
> Sağlanan paylaşım boyutları paylaşım kotası ile belirtilir, dosya paylaşımları sağlanan boyutta faturalandırılır. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/files/).

```azurecli-interactive
az storage share-rm create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --enabled-protocol NFS \
    --root-access RootSquash \
    --name "myshare" 
```
---

## <a name="next-steps"></a>Sonraki adımlar

Artık bir NFS paylaşma oluşturduğunuza göre, bunu kullanmak için Linux istemcinizle bağlamanız gerekir. Ayrıntılar için bkz. [NFS paylaşma bağlama](storage-files-how-to-mount-nfs-shares.md).

Herhangi bir sorunla karşılaşırsanız bkz. [Azure NFS dosya paylaşımları sorunlarını giderme](storage-troubleshooting-files-nfs.md).