---
title: Azure Data Box/Azure Data Box Heavy aracılığıyla sık erişimli, soğuk, arşiv blobu katmanına veri gönderme
description: Etkin, soğuk veya arşiv gibi uygun bir Blok Blobu depolama katmanına veri göndermek için Azure Data Box veya Azure Data Box Heavy kullanmayı açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 05/24/2019
ms.author: alkohli
ms.openlocfilehash: 31178284313ad7dafb094b109a75d4550cabd39c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77560380"
---
# <a name="use-azure-data-box-or-azure-data-box-heavy-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Uygun Azure Storage blob katmanına veri göndermek için Azure Data Box veya Azure Data Box Heavy kullanın

Azure Data Box, size özel bir depolama cihazı göndererek büyük miktarlardaki verileri Azure 'a taşıttı. Cihazı verilerle doldurup geri döndürebilirsiniz. Data Box veriler, depolama hesabıyla ilişkili bir varsayılan katmana yüklenir. Daha sonra verileri başka bir depolama katmanına taşıyabilirsiniz.

Bu makalede, Data Box tarafından karşıya yüklenen verilerin sık erişimli, soğuk veya arşiv blob katmanına nasıl taşınabileceği açıklanır. Bu makale hem Azure Data Box hem de Azure Data Box Heavy için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Verileriniz için doğru depolama katmanını seçin

Azure depolama, üç farklı katmanın verileri en düşük maliyetli şekilde (sık erişimli, soğuk veya Arşiv) depolamasına olanak sağlar. Sık erişimli depolama katmanı, sık erişilen verileri depolamak için iyileştirilmiştir. Sık erişimli depolama, seyrek erişimli ve arşiv depolamadan daha yüksek depolama maliyetlerine sahip olmakla kalmaz.

Seyrek erişimli depolama katmanı, en az 30 gün içinde depolanması gereken seyrek erişimli verilere yöneliktir. Soğuk katmana yönelik depolama maliyeti, sık erişimli depolama katmanından daha düşüktür, ancak sık erişimli katmanla karşılaştırıldığında veri erişim ücretleri yüksektir.

Azure arşiv katmanı çevrimdışı ve en yüksek erişim maliyetlerini de en düşük depolama maliyetini sunar. Bu katman, arşiv depolamada en az 180 gün boyunca kalan veriler için tasarlanmıştır. Bu katmanların ve fiyatlandırma modelinin ayrıntıları için [depolama katmanlarının karşılaştırması](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)bölümüne gidin.

Data Box veya Data Box Heavy verileri depolama hesabıyla ilişkili bir depolama katmanına yüklenir. Bir depolama hesabı oluşturduğunuzda, erişim katmanını sık erişimli veya soğuk olarak belirtebilirsiniz. İş yükünüzün ve maliyetinin erişim düzenine bağlı olarak, bu verileri varsayılan katmandan başka bir depolama katmanına taşıyabilirsiniz.

Nesne depolama verilerinizi yalnızca blob Storage veya Genel Amaçlı v2 (GPv2) hesaplarında katmanalabilirsiniz. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Verileriniz için doğru depolama katmanını seçmek üzere Azure Blob depolama 'da ayrıntılı konuları gözden geçirin [: Premium, sık erişimli, seyrek erişimli ve arşiv depolama katmanları](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

## <a name="set-a-default-blob-tier"></a>Varsayılan bir blob katmanı ayarlama

Varsayılan blob katmanı, depolama hesabı Azure portal oluşturulduğunda belirtilir. Depolama türü GPv2 veya blob depolama alanı olarak seçildikten sonra, erişim katmanı özniteliği belirtilebilir. Varsayılan olarak, etkin katman seçilidir.

Bir Data Box veya Data Box Heavy sıralanırken yeni bir hesap oluşturmaya çalışıyorsanız katmanlar belirtilemez. Hesap oluşturulduktan sonra, portalda hesabı değiştirerek varsayılan erişim katmanını ayarlayabilirsiniz.

Alternatif olarak, önce belirtilen erişim katmanı özniteliğiyle bir depolama hesabı oluşturursunuz. Data Box veya Data Box Heavy sırasını oluştururken, var olan depolama hesabını seçin. Depolama hesabı oluşturma sırasında varsayılan blob katmanını ayarlama hakkında daha fazla bilgi için [Azure Portal ' de depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal)bölümüne gidin.

## <a name="move-data-to-a-non-default-tier"></a>Verileri varsayılan olmayan bir katmana taşıma

Data Box cihaz verileri varsayılan katmana yüklendikten sonra, verileri varsayılan olmayan bir katmana taşımak isteyebilirsiniz. Bu verileri varsayılan olmayan bir katmana taşımanın iki yolu vardır.

- **Azure Blob depolama yaşam döngüsü yönetimi** -verileri otomatik olarak katman veya yaşam döngüsünün sonunda sona ermek üzere ilke tabanlı bir yaklaşım kullanabilirsiniz. Daha fazla bilgi için [Azure Blob depolama yaşam döngüsünü yönetme](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts)sayfasına gidin.
- **Betik oluşturma** -blob düzeyi katmanlamayı etkinleştirmek için Azure PowerShell aracılığıyla betikleştirilmiş bir yaklaşım kullanabilirsiniz. Blob üzerinde katman ayarlamak `SetBlobTier` için işlemi çağırabilirsiniz.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Blob katmanını ayarlamak için Azure PowerShell kullanma

Aşağıdaki adımlarda, bir Azure PowerShell betiği kullanarak blob katmanını arşive nasıl ayarlayabilmeniz anlatılmaktadır.

1. Yükseltilmiş bir Windows PowerShell oturumu açın. Çalışan PowerShell 5,0 veya üzeri bir sürümü olduğundan emin olun. Şunu yazın:

   `$PSVersionTable.PSVersion`     

2. Azure PowerShell oturum açın. 

   `Login-AzAccount`  

3. Depolama hesabı, erişim anahtarı, kapsayıcı ve depolama bağlamı için değişkenleri tanımlayın.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Kapsayıcıdaki tüm Blobları alın.

    `$blobs = Get-AzStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. Kapsayıcıdaki tüm Blobların katmanını arşive ayarlayın.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    Örnek bir çıktı aşağıda gösterilmiştir:

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    PS C:\WINDOWS\system32> $PSVersionTable.PSVersion

    Major  Minor  Build  Revision
    -----  -----  -----  --------
    5      1      17763  134
    PS C:\WINDOWS\system32> Login-AzAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
   > [!TIP]
   > Verilerin alma sırasında arşivlenmesini istiyorsanız varsayılan hesap katmanını sık erişimli olarak ayarlayın. Varsayılan katman soğuk ise, veriler arşiv 'e hemen taşınırsa 30 günlük erken silme ceza puanı vardır.

## <a name="next-steps"></a>Sonraki adımlar

-  [Yaşam döngüsü ilke kurallarıyla ortak veri katmanlama senaryolarını](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples) nasıl ele alabileceğinizi öğrenin

