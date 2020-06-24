---
title: Depolama hesabı yük devretmesini başlatma
titleSuffix: Azure Storage
description: Depolama hesabınızın birincil uç noktasının kullanılamaz duruma gelmesi durumunda, hesap yük devretmesini nasıl başlatacağınızı öğrenin. Yük devretme, ikincil bölgeyi depolama hesabınız için birincil bölge olacak şekilde güncelleştirir.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 06/11/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: cbdeb1c55af157a0bf5160d2420974fd014ea3b3
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84807586"
---
# <a name="initiate-a-storage-account-failover"></a>Depolama hesabı yük devretmesini başlatma

Coğrafi olarak yedekli depolama hesabınız için birincil uç nokta herhangi bir nedenle kullanılamaz duruma gelirse, hesap yük devretmesini başlatabilirsiniz. Hesap yük devretmesi ikincil uç noktayı, depolama hesabınız için birincil uç nokta olacak şekilde güncelleştirir. Yük devretme işlemi tamamlandıktan sonra istemciler yeni birincil bölgeye yazmaya başlayabilir. Zorunlu yük devretme, uygulamalarınız için yüksek kullanılabilirlik sağlamanıza olanak sağlar.

Bu makalede, Azure portal, PowerShell veya Azure CLı kullanarak depolama hesabınız için hesap yük devretmesinin nasıl başlatılacağı gösterilmektedir. Hesap yük devretmesi hakkında daha fazla bilgi için bkz. [olağanüstü durum kurtarma ve depolama hesabı yük devretme](storage-disaster-recovery-guidance.md).

> [!WARNING]
> Hesap yük devretmesi genellikle bazı veri kaybıyla sonuçlanır. Hesap yük devretmesinin etkilerini anlamak ve veri kaybına hazırlanmak için, [Hesap yük devretme sürecini anlayın](storage-disaster-recovery-guidance.md#understand-the-account-failover-process).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Depolama hesabınızda hesap yük devretmesi gerçekleştirebilmeniz için, depolama hesabınızın coğrafi çoğaltma için yapılandırıldığından emin olun. Depolama Hesabınız aşağıdaki artıklık seçeneklerinden herhangi birini kullanabilir:

- Coğrafi olarak yedekli depolama (GRS) veya Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS)
- Coğrafi bölge yedekli depolama (GZRS) veya Okuma Erişimli Coğrafi bölge yedekli depolama (RA-GZRS)

Azure depolama artıklığı hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](storage-redundancy.md).

## <a name="initiate-the-failover"></a>Yük devretmeyi Başlat

## <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portal hesap yük devretmesini başlatmak için aşağıdaki adımları izleyin:

1. Depolama hesabınıza gidin.
1. **Ayarlar**altında **coğrafi çoğaltma**' yı seçin. Aşağıdaki görüntüde, bir depolama hesabının coğrafi çoğaltma ve yük devretme durumu gösterilmektedir.

    :::image type="content" source="media/storage-initiate-account-failover/portal-failover-prepare.png" alt-text="Coğrafi çoğaltma ve yük devretme durumunu gösteren ekran görüntüsü":::

1. Depolama hesabınızın coğrafi olarak yedekli depolama (GRS) veya Okuma Erişimli Coğrafi olarak yedekli depolama (RA-GRS) için yapılandırıldığını doğrulayın. Aksi takdirde, hesabınız coğrafi olarak yedekli olacak şekilde güncelleştirmek için **Ayarlar** ' ın altında **yapılandırma** ' yı seçin.
1. **Son eşitleme zamanı** özelliği, ikinc'nin birincili hedeften gerisinde olduğunu gösterir. **Son eşitleme zamanı** , yük devretme tamamlandıktan sonra karşılaşabileceğiniz veri kaybı kapsamını tahmin eder. **Son eşitleme zamanı** özelliğini denetleme hakkında daha fazla bilgi için bkz. [bir depolama hesabı Için Son eşitleme zamanı özelliğini denetleme](last-sync-time-get.md).
1. **Yük devretme Için hazırla**' yı seçin.
1. Onay iletişim kutusunu inceleyin. Hazırsanız, yük devretmeyi onaylamak ve başlatmak için **Evet** girin.

    :::image type="content" source="media/storage-initiate-account-failover/portal-failover-confirm.png" alt-text="Hesap yük devretmesi için onay iletişim kutusunu gösteren ekran görüntüsü":::

## <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Hesap yük devretme özelliği genel kullanıma sunulmuştur, ancak yine de PowerShell için bir önizleme modülünü kullanır. Bir hesap yük devretmesi başlatmak üzere PowerShell 'i kullanmak için, önce az. Storage [1.1.1-Preview](https://www.powershellgallery.com/packages/Az.Storage/1.1.1-preview) modülünü yüklemeniz gerekir. Modülünü yüklemek için şu adımları izleyin:

1. Azure PowerShell önceki tüm yüklemelerini kaldırın:

    - **Ayarlar**altındaki **uygulamalar & Özellikler** ayarını kullanarak Windows 'un önceki Azure PowerShell yüklemelerini kaldırın.
    - Tüm **Azure** modüllerini ' den kaldırın `%Program Files%\WindowsPowerShell\Modules` .

1. PowerShellGet 'in en son sürümüne sahip olduğunuzdan emin olun. Bir Windows PowerShell penceresi açın ve en son sürümü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. PowerShellGet yükledikten sonra PowerShell penceresini kapatın ve yeniden açın.

1. Azure PowerShell en son sürümünü yükler:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Hesap yük devretmesini destekleyen bir Azure depolama önizleme modülü yükler:

    ```powershell
    Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.1.1-preview –AllowPrerelease –AllowClobber –Force
    ```

PowerShell 'den bir hesap yük devretmesi başlatmak için aşağıdaki komutu yürütün:

```powershell
Invoke-AzStorageAccountFailover -ResourceGroupName <resource-group-name> -Name <account-name>
```

## <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Hesap yük devretmesi başlatmak için Azure CLı 'yı kullanmak üzere aşağıdaki komutları yürütün:

```azurecli-interactive
az storage account show \ --name accountName \ --expand geoReplicationStats
az storage account failover \ --name accountName
```

---

## <a name="important-implications-of-account-failover"></a>Hesap yük devretmesinin önemli etkileri

Depolama hesabınız için bir hesap yük devretmesi başlattığınızda, ikincil uç nokta için DNS kayıtları, ikincil uç nokta birincil uç nokta olacak şekilde güncelleştirilir. Yük devretme işlemine başlamadan önce depolama hesabınıza yönelik olası etkiyi anladığınızdan emin olun.

Yük devretme başlamadan önce büyük olasılıkla veri kaybı kapsamını tahmin etmek için **son eşitleme zamanı** özelliğini denetleyin. **Son eşitleme zamanı** özelliğini denetleme hakkında daha fazla bilgi için bkz. [bir depolama hesabı Için Son eşitleme zamanı özelliğini denetleme](last-sync-time-get.md).

Yük devretmenin ardından, depolama hesabı türü otomatik olarak yeni birincil bölgedeki yerel olarak yedekli depolama (LRS) olarak dönüştürülür. Hesap için coğrafi olarak yedekli depolamayı (GRS) veya Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) yeniden etkinleştirebilirsiniz. LRS 'den GRS 'ye veya RA-GRS ' ye dönüştürme ek bir maliyet doğurur. Daha fazla bilgi için bkz. [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/).

Depolama hesabınız için GRS 'yi yeniden etkinleştirdikten sonra Microsoft, hesabınızdaki verileri yeni ikincil bölgeye çoğaltmaya başlar. Çoğaltma süresi, çoğaltılan veri miktarına bağlıdır.  

## <a name="next-steps"></a>Sonraki adımlar

- [Olağanüstü durum kurtarma ve depolama hesabı yük devretme](storage-disaster-recovery-guidance.md)
- [Bir depolama hesabı için son eşitleme zamanı özelliğini denetleyin](last-sync-time-get.md)
- [Yüksek oranda kullanılabilir uygulamalar tasarlamak için coğrafi artıklığı kullanın](geo-redundant-design.md)
- [Öğretici: BLOB depolama ile yüksek oranda kullanılabilir bir uygulama oluşturma](../blobs/storage-create-geo-redundant-storage.md)
