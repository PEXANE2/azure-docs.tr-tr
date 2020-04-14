---
title: ACD'ler & dosyalar için Azure Veri Gölü Depolama Gen2 PowerShell (önizleme)
description: Hiyerarşik ad alanı (HNS) etkin leştirilmiş depolama hesaplarında dizinleri ve dosya ve dizin erişim denetim listelerini (ACL) yönetmek için PowerShell cmdlets'i kullanın.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: b59c68e3f2edc0fbe5eee3c3861a3e5116d4fac6
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262392"
---
# <a name="use-powershell-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>Azure Veri Gölü Depolama Gen2'deki dizinleri, dosyaları ve ACD'leri yönetmek için PowerShell'i kullanın (önizleme)

Bu makalede, hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında dizinler, dosyalar ve izinler oluşturmak ve yönetmek için PowerShell'i nasıl kullanacağınızı gösterir. 

> [!IMPORTANT]
> Bu makalede yer alan PowerShell modülü şu anda genel önizlemededir.

[Gen1'den Gen2'ye haritalama](#gen1-gen2-map) | [Geri bildirim de verin](https://github.com/Azure/azure-powershell/issues)

## <a name="prerequisites"></a>Ön koşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkinleştirilmiş bir depolama hesabı. Oluşturmak için [bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * .NET Framework 4.7.2 veya daha fazla yüklü. Bkz. [İndir .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).
> * PowerShell `5.1` sürümü veya daha yüksek.

## <a name="install-powershell-modules"></a>PowerShell modüllerini yükleyin

1. Aşağıdaki komutu kullanarak PowerShell sürümünün `5.1` yüklü veya daha yüksek olduğunu doğrulayın.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   PowerShell sürümünüzü yükseltmek [için, mevcut Windows PowerShell'i Yükseltme](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell) bölümüne bakın
    
2. En son **PowershellGet** modüllerini yükleyin. Ardından PowerShell konsolu kapatın ve yeniden açın.

   ```powershell
   Install-Module PowerShellGet –Repository PSGallery –Force 
   ```

3. **Az.Storage** önizleme modüllerini yükleyin.

   ```powershell
   Install-Module az.storage -RequiredVersion 1.13.3-preview -Repository PSGallery -AllowClobber -AllowPrerelease -Force 
   ```

   PowerShell modüllerinin nasıl yüklenir hakkında daha fazla bilgi için Azure [PowerShell modüllerini yükleyin](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)

## <a name="connect-to-the-account"></a>Hesaba bağlanın

Bir Windows PowerShell komut penceresi açın ve ardından `Connect-AzAccount` komutla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi dizin oluşturmak ve yönetmek istediğiniz depolama hesabı aboneliğine ayarlayın. Bu örnekte, `<subscription-id>` yer tutucu değerini aboneliğinizin kimliğiyle değiştirin.

```powershell
Select-AzSubscription -SubscriptionId <subscription-id>
```

Ardından, komutlarınızın depolama hesabına nasıl yetki almasını istediğinizi seçin. 

### <a name="option-1-obtain-authorization-by-using-azure-active-directory-ad"></a>Seçenek 1: Azure Etkin Dizini (AD) kullanarak yetkilendirme edinin

Bu yaklaşımla sistem, kullanıcı hesabınızın uygun rol tabanlı erişim denetimi (RBAC) atamalarına ve ACL izinlerine sahip olmasını sağlar. 

```powershell
$ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
```

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>Seçenek 2: Depolama hesabı anahtarını kullanarak yetkilendirme elde etme

Bu yaklaşımla, sistem RBAC veya ACL izinlerini denetlemez.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
$ctx = $storageAccount.Context
```

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi, dosyalarınız için bir kapsayıcı görevi görür. `New-AzDatalakeGen2FileSystem` Cmdlet kullanarak bir oluşturabilirsiniz. 

Bu örnek, .. `my-file-system`

```powershell
$filesystemName = "my-file-system"
New-AzDatalakeGen2FileSystem -Context $ctx -Name $filesystemName
```

## <a name="create-a-directory"></a>Dizin oluşturma

`New-AzDataLakeGen2Item` Cmdlet kullanarak bir dizin başvurusu oluşturun. 

Bu örnek, dosya `my-directory` sistemine adlandırılmış bir dizin ekler.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory
```

Bu örnek aynı dizini ekler, ancak izinleri, umask, özellik değerlerini ve meta veri değerlerini de ayarlar. 

```powershell
$dir = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory -Permission rwxrwxrwx -Umask ---rwx---  -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
```

## <a name="show-directory-properties"></a>Dizin özelliklerini göster

Bu `Get-AzDataLakeGen2Item` örnek, cmdlet kullanarak bir dizin alır ve sonra konsola özellik değerlerini yazdırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir =  Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
$dir.Permissions
$dir.Group
$dir.Owner
$dir.Properties
$dir.Properties.Metadata
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

`Move-AzDataLakeGen2Item` Cmdlet'i kullanarak bir dizini yeniden adlandırın veya taşıyın.

Bu örnek, bir dizini `my-directory` adından `my-new-directory`ada yeniden adlandırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-new-directory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

> [!NOTE]
> İstemsiz olarak üzerine yazmak istiyorsanız parametreyi `-Force` kullanın.

Bu örnek, `my-directory` `my-directory-2` adlı `my-subdirectory`bir alt dizine adlı bir dizin taşır. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-directory-2/my-subdirectory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname1 -DestFileSystem $filesystemName -DestPath $dirname2
```

## <a name="delete-a-directory"></a>Bir dizini silme

`Remove-AzDataLakeGen2Item` Cmdlet kullanarak bir dizini silin.

Bu örnek, adlı `my-directory`bir dizini siler. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $dirname 
```

Bir istem `-Force` olmadan dosyayı kaldırmak için parametre kullanabilirsiniz.

## <a name="download-from-a-directory"></a>Bir dizinden indirin

Cmdlet kullanarak bir dizinden `Get-AzDataLakeGen2ItemContent` dosya indirin.

Bu örnek, adlı `upload.txt` `my-directory`bir dizinden adlı bir dosyayı karşıdan yükler. 

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$downloadFilePath = "download.txt"
Get-AzDataLakeGen2ItemContent -Context $ctx -FileSystem $filesystemName -Path $filePath -Destination $downloadFilePath
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

`Get-AzDataLakeGen2ChildItem` Cmdlet kullanarak bir dizinin içeriğini listelayın. İsteğe bağlı parametreyi `-OutputUserPrincipalName` kullanarak kullanıcıların adını (nesne kimliği yerine) alabilirsiniz.

Bu örnek, adlı `my-directory`dizinin içeriğini listeler.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -OutputUserPrincipalName
```

Aşağıdaki örnek, `ACL`dizindeki her öğenin , , `Permissions` `Group`ve `Owner` özelliklerini listeler. Parametre `-FetchProperty` `ACL` özelliği için değerleri almak için gereklidir. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$properties = Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -Recurse -FetchProperty
$properties.ACL
$properties.Permissions
$properties.Group
$properties.Owner
```

Bir dosya sisteminin içeriğini listelemek için, `-Path` komuttan parametreyi atlayın.

## <a name="upload-a-file-to-a-directory"></a>Dosyayı dizine yükleme

Cmdlet kullanarak bir dosyayı `New-AzDataLakeGen2Item` dizine yükleyin.

Bu örnek, adlı `upload.txt` `my-directory`bir dizine adlı bir dosya yükler. 

```powershell
$localSrcFile =  "upload.txt"
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$destPath = $dirname + (Get-Item $localSrcFile).Name
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Force 
```

Bu örnek aynı dosyayı yükler, ancak ardından hedef dosyanın izinlerini, umask'ı, özellik değerlerini ve meta veri değerlerini ayarlar. Bu örnek, bu değerleri konsola da yazdırır.

```powershell
$file = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Permission rwxrwxrwx -Umask ---rwx--- -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
$file1
$file1.Properties
$file1.Properties.Metadata

```

## <a name="show-file-properties"></a>Dosya özelliklerini göster

Bu `Get-AzDataLakeGen2Item` örnek, cmdlet kullanarak bir dosya alır ve sonra konsola özellik değerlerini yazdırır.

```powershell
$filepath =  "my-directory/upload.txt"
$filesystemName = "my-file-system"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filepath
$file
$file.ACL
$file.Permissions
$file.Group
$file.Owner
$file.Properties
$file.Properties.Metadata
```

## <a name="delete-a-file"></a>Dosyayı silme

Cmdlet kullanarak `Remove-AzDataLakeGen2Item` bir dosyayı silin.

Bu örnek, . `upload.txt` 

```powershell
$filesystemName = "my-file-system"
$filepath = "upload.txt"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $filepath 
```

Bir istem `-Force` olmadan dosyayı kaldırmak için parametre kullanabilirsiniz.

## <a name="manage-access-permissions"></a>Erişim izinlerini yönetme

Dosya sistemlerinin, dizinlerin ve dosyaların erişim izinlerini alabilir, ayarlayabilir ve güncelleştirebilirsiniz. Bu izinler erişim denetim listelerinde (ALA'lar) yakalanır.

> [!NOTE]
> Komutları yetkilendirmek için Azure Active Directory'yi (Azure AD) kullanıyorsanız, güvenlik yöneticinizin [Depolama Blob Veri Sahibi rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunları değiştirmenin etkileri hakkında daha fazla bilgi edinmek için [Azure Veri Gölü Depolama Gen2'de Erişim denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

### <a name="get-an-acl"></a>Bir ACL alın

`Get-AzDataLakeGen2Item`Cmdlet kullanarak bir dizin veya dosyanın ACL alın.

Bu örnek, bir **dosya sisteminin** ACL'sini alır ve acl'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$filesystem = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName
$filesystem.ACL
```

Bu örnek, bir **dizinin**ACL'sini alır ve ardından ACL'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```

Bu örnek, bir **dosyanın** ACL'sini alır ve acl'yi konsola yazdırır.

```powershell
$filePath = "my-directory/upload.txt"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

Aşağıdaki resim, bir dizinin ACL'sini aldıktan sonra çıktıyı gösterir.

![ACL çıktısı alın](./media/data-lake-storage-directory-file-acl-powershell/get-acl.png)

Bu örnekte, sahibi olan kullanıcı izinleri okumuş, yazar ve yürütemiştir. Sahip olan grup yalnızca izinleri okudu ve çalıştırıyor. Erişim denetimi listeleri hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen2'deki Erişim denetimine](data-lake-storage-access-control.md)bakın.

### <a name="set-an-acl"></a>Bir ACL ayarlama

Sahibi `set-AzDataLakeGen2ItemAclObject` kullanıcı, sahip grubu veya diğer kullanıcılar için bir ACL oluşturmak için cmdlet kullanın. Daha sonra, `Update-AzDataLakeGen2Item` ACL işlemek için cmdlet kullanın.

Bu örnek, sahip olan kullanıcı, grup sahibi veya diğer kullanıcılar için bir **dosya sisteminde** ACL ayarlar ve sonra konsola ACL yazdırır.

```powershell
$filesystemName = "my-file-system"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission -wx -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Acl $acl
$filesystem = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName
$filesystem.ACL
```

Bu örnek, ACL'yi sahibi kullanıcı, sahip grubu veya diğer kullanıcılar için bir **dizinde** ayarlar ve ardından ACL'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission -wx -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```
Bu örnek, sahip olan kullanıcı, grup sahibi veya diğer kullanıcılar için bir **dosyaüzerinde** ACL ayarlar ve sonra konsola ACL yazdırır.

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath -Acl $acl
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

Aşağıdaki resim, bir dosyanın ACL'sini ayarladıktan sonra çıktıyı gösterir.

![ACL çıktısı alın](./media/data-lake-storage-directory-file-acl-powershell/set-acl.png)

Bu örnekte, sahibi kullanıcı ve sahip grubu yalnızca okuma ve yazma izinleri var. Diğer tüm kullanıcıların yazma ve yürütme izinleri vardır. Erişim denetimi listeleri hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen2'deki Erişim denetimine](data-lake-storage-access-control.md)bakın.


### <a name="set-acls-on-all-items-in-a-file-system"></a>Dosya sistemindeki tüm öğelerin ALA'larını ayarlama

Bir dosya `Get-AzDataLakeGen2Item` sistemindeki `-Recurse` tüm dizin `Update-AzDataLakeGen2Item` ve dosyaların ACL'sini ayarlamak için cmdlet ile birlikte parametreyi kullanabilirsiniz. 

```powershell
$filesystemName = "my-file-system"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission -wx -InputObject $acl
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Recurse | Update-AzDataLakeGen2Item -Acl $acl
```
### <a name="add-or-update-an-acl-entry"></a>ACL girişi ekleme veya güncelleme

Önce ACL'yi al. Ardından, bir `set-AzDataLakeGen2ItemAclObject` ACL girişi eklemek veya güncellemek için cmdlet'i kullanın. ACL `Update-AzDataLakeGen2Item` işlemek için cmdlet kullanın.

Bu örnek, bir kullanıcı için bir **dizinde** ACL oluşturur veya güncelleştirir.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname).ACL
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityID xxxxxxxx-xxxx-xxxxxxxxxxx -Permission r-x -InputObject $acl 
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
```

### <a name="remove-an-acl-entry"></a>ACL girişini kaldırma

Bu örnek, varolan bir ACL'den bir girişi kaldırır.

```powershell
$id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

# Create the new ACL object.
[Collections.Generic.List[System.Object]]$aclnew =$acl

foreach ($a in $aclnew)
{
    if ($a.AccessControlType -eq "User"-and $a.DefaultScope -eq $false -and $a.EntityId -eq $id)
    {
        $aclnew.Remove($a);
        break;
    }
}
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $aclnew
```

<a id="gen1-gen2-map" />

## <a name="gen1-to-gen2-mapping"></a>Gen1 ' den Gen2 Haritalama

Aşağıdaki tablo, Data Lake Storage Gen1 haritası için kullanılan cmdlets'in Data Lake Storage Gen2 için cmdlets'e nasıl olduğunu göstermektedir.

|Data Lake Depolama Gen1 cmdlet| Data Lake Depolama Gen2 cmdlet| Notlar |
|--------|---------|-----|
|Get-AzDataLakeStoreChildItem|Get-AzDataLakeGen2ChildItem|Varsayılan olarak, Get-AzDataLakeGen2ChildItem cmdlet yalnızca birinci düzey alt öğeleri listeler. -Recurse parametresi alt öğeleri özyinelemeli olarak listeler. |
|Get-AzDataLakeStoreItem<br>Get-AzDataLakeStoreItemAclEntry<br>Get-AzDataLakeStoreItemSahibi<br>Get-AzDataLakeStoreItemPermission|Get-AzDataLakeGen2Item|Get-AzDataLakeGen2Item cmdlet çıktı öğeleri şu özelliklere sahiptir: Acl, Sahibi, Grubu, İzin.|
|Get-AzDataLakeStoreItemContent|Get-AzDataLakeGen2FileContent|Get-AzDataLakeGen2FileContent cmdlet dosya içeriğini yerel dosyaya indirin.|
|Move-AzDataLakeStoreItem|Move-AzDataLakeGen2Item||
|Yeni-AzDataLakeStoreItem|Yeni-AzDataLakeGen2Item|Bu cmdlet, yeni dosya içeriğini yerel bir dosyadan yükler.|
|Remove-AzDataLakeStoreItem|Kaldır-AzDataLakeGen2Item||
|Set-AzDataLakeStoreItemSahibi<br>Set-AzDataLakeStoreItemPermission<br>Set-AzDataLakeStoreItemAcl|Güncelleme-AzDataLakeGen2Item|Update-AzDataLakeGen2Item cmdlet yalnızca tek bir öğeyi güncelleştirir ve özyinelemeli olarak güncellemez. Yinelemeli olarak güncelleştirmek istiyorsanız, Get-AzDataLakeStoreChildItem cmdlet kullanarak öğeleri listeleyin, ardından Update-AzDataLakeGen2Item cmdlet'e boru hattı.|
|Test-AzDataLakeStoreItem|Get-AzDataLakeGen2Item|Get-AzDataLakeGen2Item cmdlet, öğe yoksa bir hata bildirir.|



## <a name="see-also"></a>Ayrıca bkz.

* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Azure Depolama ile Azure PowerShell'i kullanma.](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Depolama PowerShell cmdlets](/powershell/module/az.storage).

