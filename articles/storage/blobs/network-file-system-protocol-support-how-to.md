---
title: NFS 3,0 protokolünü (Önizleme) kullanarak Azure Blob depolamayı bağlama | Microsoft Docs
description: Bir Azure sanal makinesinden (VM) veya NFS 3,0 protokolünü kullanarak şirket içinde çalışan bir istemciden BLOB depolama alanına bir kapsayıcı bağlama hakkında bilgi edinin.
author: normesta
ms.subservice: blobs
ms.service: storage
ms.topic: conceptual
ms.date: 08/04/2020
ms.author: normesta
ms.reviewer: yzheng
ms.custom: references_regions
ms.openlocfilehash: 09206b8189f03a37f8bd7d073238609a3f1bd3ad
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816108"
---
# <a name="mount-blob-storage-by-using-the-network-file-system-nfs-30-protocol-preview"></a>Ağ dosya sistemi (NFS) 3,0 protokolünü (Önizleme) kullanarak blob depolamayı bağlama

BLOB depolama alanına bir kapsayıcıyı Windows veya Linux tabanlı bir Azure sanal makinesi (VM) veya NFS 3,0 protokolünü kullanarak şirket içinde çalışan bir Windows veya Linux sisteminden bağlayabilirsiniz. Bu makalede adım adım yönergeler sunulmaktadır. Blob depolamada NFS 3,0 protokol desteği hakkında daha fazla bilgi edinmek için bkz. [Azure Blob Storage 'da (Önizleme) ağ dosya sistemi (NFS) 3,0 protokol desteği](network-file-system-protocol-support.md).

> [!NOTE]
> Azure Blob depolamada NFS 3,0 protokol desteği genel önizlemeye sunuldu ve şu bölgelerde kullanılabilir: ABD Doğu, ABD Orta, ABD Orta Batı, Avustralya Güneydoğu, Kuzey Avrupa, UK Batı, Kore Orta, Kore Güney ve Kanada Orta.

## <a name="step-1-register-the-nfs-30-protocol-feature-with-your-subscription"></a>1. Adım: NFS 3,0 protokol özelliğini aboneliğiniz ile kaydetme

1. Bir PowerShell komut penceresi açın. 

2. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

   ```powershell
   Connect-AzAccount
   ```

3. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi ayarlayın.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```
   
   `<subscription-id>`Yer tutucu değerini ABONELIĞINIZIN kimliğiyle değiştirin.

4. `AllowNFSV3`Aşağıdaki komutu kullanarak özelliği kaydettirin.

   ```powershell
   Register-AzProviderFeature -FeatureName AllowNFSV3 -ProviderNamespace Microsoft.Storage 
   ```

5. `PremiumHns`Aşağıdaki komutu kullanarak özelliği de kaydettirin.

   ```powershell
   Register-AzProviderFeature -FeatureName PremiumHns -ProviderNamespace Microsoft.Storage  
   ```

6. Aşağıdaki komutu kullanarak kaynak sağlayıcısını kaydedin.
    
   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Storage   
   ```

## <a name="step-2-verify-that-the-feature-is-registered"></a>2. Adım: özelliğin kaydedildiğini doğrulama 

Kayıt onayı bir saate kadar sürebilir. Kaydın tamamlandığını doğrulamak için aşağıdaki komutları kullanın.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNFSV3
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName PremiumHns  
```

## <a name="step-3-create-an-azure-virtual-network-vnet"></a>3. Adım: Azure sanal ağı oluşturma (VNet)

Depolama hesabınız bir sanal ağ içinde yer almalıdır. VNet, istemcilerin depolama hesabınıza güvenli bir şekilde bağlanmasını sağlar. VNet hakkında daha fazla bilgi edinmek ve bir oluşturma hakkında daha fazla bilgi için bkz. [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network/).

> [!NOTE]
> Aynı VNet 'teki istemciler, hesabınızdaki kapsayıcıları bağlayabilir. Ayrıca, şirket içi ağda çalışan bir istemciden bir kapsayıcı da bağlayabilirsiniz, ancak önce şirket içi ağınızı sanal ağınıza bağlamanız gerekir. Bkz. [desteklenen ağ bağlantıları](network-file-system-protocol-support.md#supported-network-connections).

## <a name="step-4-configure-network-security"></a>4. Adım: ağ güvenliğini yapılandırma

Hesabınızdaki verileri güvenli hale getirmenin tek yolu, VNet ve diğer ağ güvenlik ayarlarını kullanmaktır. Hesap anahtarı yetkilendirme, Azure Active Directory (AD) güvenliği ve erişim denetim listeleri (ACL 'Ler) dahil olmak üzere verilerin güvenliğini sağlamak için kullanılan diğer herhangi bir araç, NFS 3,0 protokol desteği etkinleştirilmiş hesaplarda henüz desteklenmemektedir. 

Hesabınızdaki verilerin güvenliğini sağlamak için şu önerilere bakın: [BLOB depolama Için ağ güvenlik önerileri](security-recommendations.md#networking).

## <a name="step-5-create-and-configure-a-storage-account"></a>5. Adım: depolama hesabı oluşturma ve yapılandırma

NFS 3,0 kullanarak bir kapsayıcı bağlamak için, özelliği aboneliğiniz ile **kaydettikten sonra** bir depolama hesabı oluşturmanız gerekir. Özelliği kaydetmeden önce var olan hesapları etkinleştiremezsiniz. 

Bu özelliğin önizleme sürümünde, NFS 3,0 protokolü yalnızca [Blockblobstorage](../blobs/storage-blob-create-account-block-blob.md) hesaplarında desteklenir.

Hesabı yapılandırırken şu değerleri seçin:

|Ayar | Değer|
|----|---|
|Konum|Şu bölgelerden biri: ABD Doğu, ABD Orta, ABD Orta Batı, Avustralya Güneydoğu, Kuzey Avrupa, UK Batı, Kore Orta, Kore Güney ve Kanada Orta |
|Performans|Premium|
|Hesap türü|BlockBlobStorage|
|Çoğaltma|Yerel olarak yedekli depolama (LRS)|
|Bağlantı yöntemi|Ortak uç nokta (seçili ağlar) veya özel uç nokta|
|Güvenli aktarım gerekir|Devre dışı|
|Hiyerarşik ad alanı|Etkin|
|NFS V3|Etkin|

Tüm diğer ayarlar için varsayılan değerleri kabul edebilirsiniz. 

## <a name="step-6-create-a-container"></a>6. Adım: Kapsayıcı oluşturma

Şu araçlardan veya SDK 'Lardan birini kullanarak depolama hesabınızda bir kapsayıcı oluşturun:

|Araçlar|SDK|
|---|---|
|[Azure Portal](https://portal.azure.com)|[.NET](data-lake-storage-directory-file-acl-dotnet.md#create-a-container)|
|[AzCopy](../common/storage-use-azcopy-blobs.md#create-a-container)|[Java](data-lake-storage-directory-file-acl-java.md#create-a-container)|
|[PowerShell](data-lake-storage-directory-file-acl-powershell.md#create-a-container)|[Python](data-lake-storage-directory-file-acl-python.md#create-a-container)|
|[Azure CLI](data-lake-storage-directory-file-acl-cli.md#create-a-container)|[JavaScript](data-lake-storage-directory-file-acl-javascript.md)|
||[REST](https://docs.microsoft.com/rest/api/storageservices/create-container)|

## <a name="step-7-mount-the-container"></a>7. Adım: kapsayıcıyı bağlama

Windows veya Linux sisteminizde bir dizin oluşturun ve ardından depolama hesabına bir kapsayıcı bağlayın.

### <a name="linux"></a>[Linux](#tab/linux)

1. Linux sisteminde bir dizin oluşturun.

   ```
   mkdir -p /mnt/test
   ```

2. Aşağıdaki komutu kullanarak bir kapsayıcı bağlayın.

   ```
   mount -o sec=sys,vers=3,nolock,proto=tcp <storage-account-name>.blob.core.windows.net:/<storage-account-name>/<container-name>  /mnt/test
   ```

   - `<storage-account-name>`Bu komutta görünen yer tutucuyu depolama hesabınızın adıyla değiştirin.  

   - `<container-name>`Yer tutucusunu kapsayıcının adıyla değiştirin.


### <a name="windows"></a>[Windows](#tab/windows)

1. **Windows özellikleri** iletişim kutusunu açın ve ardından **NFS istemcisi** özelliğini açın. 

   ![Ağ dosya sistemi için istemci özelliği](media/network-file-system-protocol-how-to/client-for-network-files-system-feature.png)

2. [Bağlama](https://docs.microsoft.com/windows-server/administration/windows-commands/mount) komutunu kullanarak bir kapsayıcı bağlayın.

   ```
   mount -o nolock <storage-account-name>.blob.core.windows.net:/<storage-account-name>/<container-name> *
   ```

   - `<storage-account-name>`Bu komutta görünen yer tutucuyu depolama hesabınızın adıyla değiştirin.  

   - `<container-name>`Yer tutucusunu kapsayıcının adıyla değiştirin.

3. Yazma izinlerine ihtiyacınız varsa, Windows 'un paylaşıma bağlanmak için kullandığı varsayılan UID ve GID 'yi değiştirmeniz gerekebilir. Bunu yapmak için aşağıdaki PowerShell komutlarını yönetici olarak çalıştırın:

   ```
   New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default -Name AnonymousUid -PropertyType DWord -Value 0
   New-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\ClientForNFS\CurrentVersion\Default -Name AnonymousGid -PropertyType DWord -Value 0
   ```
   
   - NFS istemci hizmetini yeniden başlatın veya bu değişikliği yaptıktan sonra sunucuyu yeniden başlatın.

---

## <a name="resolve-common-issues"></a>Yaygın sorunları çözme

|Sorun/hata | Çözüm|
|---|---|
|`Access denied by server while mounting`|İstemcinizin desteklenen bir alt ağ üzerinde çalıştığından emin olun. [Desteklenen ağ konumlarına](network-file-system-protocol-support.md#supported-network-connections)bakın.|
|`No such file or directory`| Bağladığınız kapsayıcının özelliğin kaydedildiğini doğruladıktan sonra oluşturulduğundan emin olun. Bkz. [2. Adım: özelliğin kayıtlı olduğunu doğrulayın](#step-2-verify-that-the-feature-is-registered). Ayrıca, Mount komutunu ve bu parametrenin parametrelerini doğrudan terminaline yazdığınızdan emin olun. Bu komutun herhangi bir bölümünü başka bir uygulamadan kopyalayıp terminale yapıştırırsanız, yapıştırdığınız bilgilerde bulunan gizli karakterler bu hatanın ortaya çıkmasına neden olabilir.|

## <a name="see-also"></a>Ayrıca bkz.

[Azure Blob depolamada ağ dosya sistemi (NFS) 3,0 protokol desteği (Önizleme)](network-file-system-protocol-support.md)







