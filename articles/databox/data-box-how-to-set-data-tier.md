---
title: Azure Veri Kutusu/Azure Veri Kutusu Ağır üzerinden Sıcak, Soğuk, Arşiv blob katmanına veri gönderme
description: Verileri sıcak, soğuk veya arşiv gibi uygun bir blok blob depolama katmanına göndermek için Azure Veri Kutusu veya Azure Veri Kutusu Ağır'ı nasıl kullanacağımı açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 05/24/2019
ms.author: alkohli
ms.openlocfilehash: 31178284313ad7dafb094b109a75d4550cabd39c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77560380"
---
# <a name="use-azure-data-box-or-azure-data-box-heavy-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Verileri uygun Azure Depolama blob katmanına göndermek için Azure Veri Kutusu'na veya Azure Veri Kutusu Ağır'ı kullanın

Azure Veri Kutusu, size özel bir depolama aygıtı göndererek büyük miktarda veriyi Azure'a taşır. Cihazı verilerle doldurup döndürün. Veri Kutusu'ndan gelen veriler depolama hesabıyla ilişkili varsayılan bir katmana yüklenir. Daha sonra verileri başka bir depolama katmanına taşıyabilirsiniz.

Bu makalede, Veri Kutusu tarafından yüklenen verilerin Sıcak, Soğuk veya Arşiv blob katmanına nasıl taşınabileceği açıklanmaktadır. Bu makale, hem Azure Veri Kutusu hem de Azure Veri Kutusu Ağır için geçerlidir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Verileriniz için doğru depolama katmanını seçin

Azure depolama, üç farklı katmanın verileri en uygun maliyetli şekilde depolamasına olanak tanır ( Sıcak, Soğuk veya Arşiv). Sıcak depolama katmanı, sık erişilen verileri depolamak için optimize edilmiştir. Sıcak depolama, Cool ve Arşiv depolamadaha yüksek depolama maliyetleri, ancak en düşük erişim maliyetleri vardır.

Serin depolama katmanı, en az 30 gün boyunca depolanması gereken seyrek erişilen veriler içindir. Soğuk katman için depolama maliyeti sıcak depolama katmanından daha düşüktür, ancak Sıcak katmana kıyasla veri erişim ücretleri yüksektir.

Azure Arşiv katmanı çevrimdışıdır ve en düşük depolama maliyetlerinin değil, en yüksek erişim maliyetlerini de sunar. Bu katman, en az 180 gün arşiv depolamada kalan veriler için dir. Bu katmanların her birinin ayrıntıları ve fiyatlandırma modeli için [depolama katmanlarının Karşılaştırılması bölümüne](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)gidin.

Veri Kutusu veya Veri Kutusu Ağır'dan gelen veriler, depolama hesabıyla ilişkili bir depolama katmanına yüklenir. Bir depolama hesabı oluşturduğunuzda, erişim katmanını Sıcak veya Soğuk olarak belirtebilirsiniz. İş yükünüzün ve maliyetinizin erişim desenine bağlı olarak, bu verileri varsayılan katmandan başka bir depolama katmanına taşıyabilirsiniz.

Nesne depolama verilerinizi yalnızca Blob depolama veya Genel Amaçlı v2 (GPv2) hesaplarında katmanlababilirsiniz. Genel Amaçlı v1 (GPv1) hesaplar katman ayarlamayı desteklemez. Verileriniz için doğru depolama katmanını seçmek için Azure Blob depolamaalanında ayrıntılı olarak belirtilen hususları [inceleyin: Premium, Hot, Cool ve Arşiv depolama katmanları.](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers)

## <a name="set-a-default-blob-tier"></a>Varsayılan blob katmanı ayarlama

Varsayılan blob katmanı, Azure portalında depolama hesabı oluşturulduğunda belirtilir. GPv2 veya Blob depolama alanı olarak bir depolama türü seçildikten sonra Access katmanı özniteliği belirtilebilir. Varsayılan olarak, Sıcak katman seçilir.

Veri Kutusu veya Veri Kutusu Ağır siparişi verirken yeni bir hesap oluşturmaya çalışıyorsanız, katmanlar belirtilemez. Hesap oluşturulduktan sonra, varsayılan erişim katmanını ayarlamak için portaldaki hesabı değiştirebilirsiniz.

Alternatif olarak, önce belirtilen erişim katmanı özniteliğiyle bir depolama hesabı oluşturursunuz. Veri Kutusu veya Veri Kutusu Ağır siparişoluştururken, varolan depolama hesabını seçin. Depolama hesabı oluşturma sırasında varsayılan blob katmanının nasıl ayarlanılabildiğini hakkında daha fazla bilgi için [Azure portalında bir depolama hesabı oluşturma'ya](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal)gidin.

## <a name="move-data-to-a-non-default-tier"></a>Verileri varsayılan olmayan bir katmana taşıma

Veri Kutusu aygıtındaki veriler varsayılan katmana yüklendikten sonra, verileri varsayılan olmayan bir katmana taşımak isteyebilirsiniz. Bu verileri varsayılan olmayan bir katmana taşımanın iki yolu vardır.

- **Azure Blob depolama yaşam döngüsü yönetimi** - Verileri otomatik olarak katmanlamak veya yaşam döngüsünün sonunda sona ermek için ilke tabanlı bir yaklaşım kullanabilirsiniz. Daha fazla bilgi için [Azure Blob depolama yaşam döngüsünü yönetme'ye](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts)gidin.
- **Komut Dosyası -** Blob düzeyinde katmanlamayı etkinleştirmek için Azure PowerShell üzerinden komut dosyası yaklaşımını kullanabilirsiniz. Katmanı blob `SetBlobTier` üzerinde ayarlamak için işlemi arayabilirsiniz.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Blob katmanını ayarlamak için Azure PowerShell'i kullanın

Aşağıdaki adımlar, bir Azure PowerShell komut dosyası kullanarak blob katmanını Arşivolarak nasıl ayarlayabileceğinizi açıklar.

1. Yükseltilmiş bir Windows PowerShell oturumu açın. PowerShell 5.0 veya daha yüksek bir şekilde çalıştırdığından emin olun. Şunu yazın:

   `$PSVersionTable.PSVersion`     

2. Azure PowerShell'de oturum açın. 

   `Login-AzAccount`  

3. Depolama hesabı, erişim anahtarı, kapsayıcı ve depolama bağlamı için değişkenleri tanımlayın.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Tüm lekeleri konteynere koyun.

    `$blobs = Get-AzStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. Kapsayıcıdaki tüm lekelerin katmanını Arşiv'e ayarlayın.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    Örnek çıktı aşağıda gösterilmiştir:

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
   > Verilerin sindirimi nde arşivlebilmesini istiyorsanız, varsayılan hesap katmanını Hot olarak ayarlayın. Varsayılan katman Cool ise, veriler hemen Arşiv'e taşınırsa 30 günlük erken silme cezası vardır.

## <a name="next-steps"></a>Sonraki adımlar

-  [Yaşam döngüsü ilkesi kurallarıyla ortak veri katmanlama senaryolarını](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples) nasıl ele alabildiğini öğrenin

