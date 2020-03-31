---
title: Depolama hesabı başlatma (önizleme) - Azure Depolama
description: Depolama hesabınız için birincil bitiş noktasının kullanılamaması durumunda bir hesap başarısızlığına nasıl başsüreceğinizi öğrenin. Failover, depolama hesabınız için birincil bölge olmak için ikincil bölgeyi güncelleştirir.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/11/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 0c619224201d6225d5e5c127b342f71f2f7fced9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79535361"
---
# <a name="initiate-a-storage-account-failover-preview"></a>Depolama hesabı nın başarısız olduğunu başlatma (önizleme)

Coğrafi yedekli depolama hesabınızın birincil bitiş noktası herhangi bir nedenle kullanılamaz hale gelirse, bir hesap başarısızlığı (önizleme) başlatabilirsiniz. Bir hesap başarısız, depolama hesabınız için birincil bitiş noktası olmak için ikincil bitiş noktasını güncelleştirir. Başarısız lık tamamlandıktan sonra, istemciler yeni birincil bölgeye yazmaya başlayabilir. Zorunlu arıza, uygulamalarınız için yüksek kullanılabilirliği korumanızı sağlar.

Bu makalede, Azure portalı, PowerShell veya Azure CLI'yi kullanarak depolama hesabınız için nasıl bir hesap başarısızlığına başlanır. Hesap başarısız olması hakkında daha fazla bilgi edinmek için [Azure Depolama'da Olağanüstü Durum kurtarma ve hesap başarısız (önizleme) (önizleme) sayfasını](storage-disaster-recovery-guidance.md)görün.

> [!WARNING]
> Bir hesap başarısız genellikle bazı veri kaybına neden olabilir. Bir hesap başarısızlığı üzerindeki etkilerini anlamak ve veri kaybına hazırlanmak için hesap [başarısızlığı işlemini inceleyin.](storage-disaster-recovery-guidance.md#understand-the-account-failover-process)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Depolama hesabınızda bir hesap başarısızlığı gerçekleştirmeden önce aşağıdaki adımı gerçekleştirdiğinizden emin olun:

- Depolama hesabınızın jeo-yedekli depolama (GRS) veya okuma erişimi coğrafi yedekli depolama (RA-GRS) kullanacak şekilde yapılandırıldığından emin olun. Coğrafi yedekdepolama hakkında daha fazla bilgi için [Azure Depolama artıklığı'na](storage-redundancy.md)bakın.

## <a name="important-implications-of-account-failover"></a>Hesap başarısızlığı önemli etkileri

Depolama hesabınız için bir hesap başarısızlığına başladığınızda, ikincil bitiş noktası nın DNS kayıtları güncelleştirilir, böylece ikincil bitiş noktası birincil bitiş noktası olur. Bir hata başlatmadan önce depolama hesabınızüzerindeki olası etkiyi anladığınızdan emin olun.

Bir hata başlatmadan önce olası veri kaybının boyutunu tahmin etmek için `Get-AzStorageAccount` PowerShell cmdlet'ini `-IncludeGeoReplicationStats` kullanarak Son **Eşitleme Süresi** özelliğini kontrol edin ve parametreyi ekleyin. Ardından hesabınız `GeoReplicationStats` için mülkü kontrol edin. \

Başarısız olduktan sonra, depolama hesabı türünüz otomatik olarak yeni birincil bölgede yerel olarak yedekli depolamaalanına (LRS) dönüştürülür. Hesap için coğrafi yedekli depolamayı (GRS) veya okuma erişimi coğrafi yedekli depolamayı (RA-GRS) yeniden etkinleştirebilirsiniz. LRS'den GRS veya RA-GRS'ye dönüştürmenin ek bir maliyete yol ladığını unutmayın. Daha fazla bilgi için [Bkz. Bant Genişliği Fiyatlandırma Ayrıntıları.](https://azure.microsoft.com/pricing/details/bandwidth/)

Depolama hesabınız için GRS'yi yeniden etkinleştirdikten sonra, Microsoft hesabınızdaki verileri yeni ikincil bölgeye çoğaltmaya başlar. Çoğaltma süresi çoğaltılan veri miktarına bağlıdır.  

## <a name="portal"></a>[Portal](#tab/azure-portal)

Azure portalından bir hesap başarısızı başlatmak için aşağıdaki adımları izleyin:

1. Depolama hesabınıza gidin.
2. **Ayarlar** **altında, Coğrafi çoğaltma'yı**seçin. Aşağıdaki resim, bir depolama hesabının coğrafi çoğaltma ve başarısız lık durumunu gösterir.

    ![Coğrafi çoğaltma ve başarısız lık durumunu gösteren ekran görüntüsü](media/storage-initiate-account-failover/portal-failover-prepare.png)

3. Depolama hesabınızın coğrafi yedekli depolama (GRS) veya okuma erişimi coğrafi yedekli depolama (RA-GRS) için yapılandırıldığından doğrulayın. Değilse, hesabınızı coğrafi yedekli olarak güncellemek için **Ayarlar** altında **Yapılandırma'yı** seçin. 
4. **Son Eşitleme Zamanı** özelliği, ikincil özelliğin birincilden ne kadar geride olduğunu gösterir. **Son Eşitleme Süresi,** başarısızlık tamamlandıktan sonra karşılaşacağınız veri kaybının boyutunun tahminini sağlar.
5. **Failover için Hazırla (önizleme) seçeneğini belirleyin.** 
6. Onay iletişim kutusunu gözden geçirin. Hazır olduğunuzda, başarısız olmayı onaylamak ve başlatmak için **Evet** girin.

    ![Bir hesap başarısızlığına yönelik onay iletişim kutusunu gösteren ekran görüntüsü](media/storage-initiate-account-failover/portal-failover-confirm.png)

## <a name="powershell"></a>[Powershell](#tab/azure-powershell)

Bir hesap başarısızmasını başlatmak için PowerShell'i kullanmak için öncelikle 6.0.1 önizleme modüllerini yüklemeniz gerekir. Modülü yüklemek için aşağıdaki adımları izleyin:

1. Azure PowerShell'in önceki yüklemelerini kaldırın:

    - **Ayarlar**altında ayarlar & **Uygulamalar'ı** kullanarak Azure PowerShell'in önceki tüm yüklemelerini Windows'dan kaldırın.
    - Tüm **Azure** modüllerini `%Program Files%\WindowsPowerShell\Modules`kaldırın.

1. PowerShellGet'in en son sürümünün yüklü olduğundan emin olun. Windows PowerShell penceresini açın ve en son sürümü yüklemek için aşağıdaki komutu çalıştırın:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. PowerShellGet'i yükledikten sonra PowerShell penceresini kapatın ve yeniden açın. 

1. Azure PowerShell'in en son sürümünü yükleyin:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Hesap başarısızlarını destekleyen bir Azure Depolama önizleme modülü yükleyin:

    ```powershell
    Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.1.1-preview –AllowPrerelease –AllowClobber –Force 
    ```

1. PowerShell penceresini kapatın ve yeniden açın.
 
PowerShell'den hesap başarısızolmasını başlatmak için aşağıdaki komutu uygulayın:

```powershell
Invoke-AzStorageAccountFailover -ResourceGroupName <resource-group-name> -Name <account-name> 
```

## <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir hesap başarısızolmasını başlatmak için Azure CLI'yi kullanmak için aşağıdaki komutları uygulayın:

```azurecli
az storage account show \ --name accountName \ --expand geoReplicationStats
az storage account failover \ --name accountName
```

---

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Depolama'da olağanüstü durum kurtarma ve hesap başarısız (önizleme)](storage-disaster-recovery-guidance.md)
- [RA-GRS’yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md)
- [Öğretici: Blob depolama ile son derece kullanılabilir bir uygulama oluşturun](../blobs/storage-create-geo-redundant-storage.md) 
