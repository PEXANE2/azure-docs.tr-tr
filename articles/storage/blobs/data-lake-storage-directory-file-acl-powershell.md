---
title: "& ACL 'Ler için Azure Data Lake Storage 2. PowerShell"
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için PowerShell cmdlet 'lerini kullanın.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: how-to
ms.date: 08/26/2020
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: 01706b3f6850d49240b9c84997cbbec528045200
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88923883"
---
# <a name="use-powershell-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>PowerShell kullanarak Azure Data Lake Storage 2. dizinleri, dosyaları ve ACL 'Leri yönetme

Bu makalede, PowerShell kullanarak hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında Dizin, dosya ve izinleri oluşturma ve bunları yönetme işlemi gösterilmektedir. 

[Başvuru](https://docs.microsoft.com/powershell/module/Az.Storage/?view=azps-4.5.0)  |  [Gen1 to Gen2 Mapping](#gen1-gen2-map)  |  [Geri bildirimde](https://github.com/Azure/azure-powershell/issues) bulunun

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * .NET Framework, 4.7.2 veya üzeri bir sürümü yüklendi. Bkz. [indirme .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).
> * PowerShell sürümü `5.1` veya üzeri.

## <a name="install-the-powershell-module"></a>PowerShell modülünü yükler

1. `5.1`Aşağıdaki komutu kullanarak yüklü PowerShell sürümünün veya daha yüksek olduğunu doğrulayın.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   PowerShell sürümünüzü yükseltmek için bkz. [var olan Windows PowerShell 'ı yükseltme](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)
    
2. **Az. Storage** modülünü yükler.

   ```powershell
   Install-Module Az.Storage -Repository PSGallery -Force  
   ```

   PowerShell modüllerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz [. Azure PowerShell modülünü Install](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)

## <a name="connect-to-the-account"></a>Hesaba Bağlan

Bir Windows PowerShell komut penceresi açın ve komutuyla Azure aboneliğinizde oturum açın `Connect-AzAccount` ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi ' de dizin oluşturup yönetmek istediğiniz depolama hesabının aboneliğine ayarlayın. Bu örnekte, `<subscription-id>` yer tutucu değerini ABONELIĞINIZIN kimliğiyle değiştirin.

```powershell
Select-AzSubscription -SubscriptionId <subscription-id>
```

Sonra, komutlarınızın depolama hesabı için nasıl yetkilendirme elde etmek istediğinizi seçin. 

### <a name="option-1-obtain-authorization-by-using-azure-active-directory-ad"></a>Seçenek 1: Azure Active Directory kullanarak yetkilendirme alma (AD)

Bu yaklaşımda sistem, Kullanıcı hesabınızın uygun rol tabanlı erişim denetimi (RBAC) atamaları ve ACL izinleri olmasını sağlar. 

```powershell
$ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
```

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>Seçenek 2: depolama hesabı anahtarını kullanarak yetkilendirme alma

Bu yaklaşımda sistem RBAC veya ACL izinlerini denetlemez.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
$ctx = $storageAccount.Context
```

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir kapsayıcı dosyalarınız için bir dosya sistemi görevi görür. Cmdlet 'ini kullanarak bir tane oluşturabilirsiniz `New-AzStorageContainer` . 

Bu örnek adlı bir kapsayıcı oluşturur `my-file-system` .

```powershell
$filesystemName = "my-file-system"
New-AzStorageContainer -Context $ctx -Name $filesystemName
```

## <a name="create-a-directory"></a>Dizin oluşturma

Cmdlet 'ini kullanarak bir dizin başvurusu oluşturun `New-AzDataLakeGen2Item` . 

Bu örnek, bir kapsayıcıya adlı bir dizin ekler `my-directory` .

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory
```

Bu örnek aynı dizini ekler, ancak izinler, umask, özellik değerleri ve meta veri değerlerini de ayarlar. 

```powershell
$dir = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Directory -Permission rwxrwxrwx -Umask ---rwx---  -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
```

## <a name="show-directory-properties"></a>Dizin özelliklerini göster

Bu örnek, cmdlet 'ini kullanarak bir dizin alır `Get-AzDataLakeGen2Item` ve sonra özellik değerlerini konsola yazdırır.

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

Cmdlet 'ini kullanarak bir dizini yeniden adlandırın veya taşıyın `Move-AzDataLakeGen2Item` .

Bu örnek, adından adı olan bir dizini yeniden adlandırır `my-directory` `my-new-directory` .

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-new-directory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

> [!NOTE]
> `-Force`Komut istemleri olmadan üzerine yazmak istiyorsanız parametresini kullanın.

Bu örnek adlı bir dizini `my-directory` adlı bir alt dizine taşır `my-directory-2` `my-subdirectory` . 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-directory-2/my-subdirectory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname1 -DestFileSystem $filesystemName -DestPath $dirname2
```

## <a name="delete-a-directory"></a>Bir dizini silme

Cmdlet 'ini kullanarak bir dizini silin `Remove-AzDataLakeGen2Item` .

Bu örnek adlı bir dizini siler `my-directory` . 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $dirname 
```

`-Force`Dosyayı sormadan kaldırmak için parametresini kullanabilirsiniz.

## <a name="download-from-a-directory"></a>Bir dizinden indir

Cmdlet 'ini kullanarak bir dizinden dosya indirin `Get-AzDataLakeGen2ItemContent` .

Bu örnek, `upload.txt` adlı bir dizinden adlı bir dosyayı indirir `my-directory` . 

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$downloadFilePath = "download.txt"
Get-AzDataLakeGen2ItemContent -Context $ctx -FileSystem $filesystemName -Path $filePath -Destination $downloadFilePath
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Cmdlet 'ini kullanarak bir dizinin içeriğini listeleyin `Get-AzDataLakeGen2ChildItem` . `-OutputUserPrincipalName`Kullanıcının adını (nesne kimliği yerine) almak için isteğe bağlı parametresini kullanabilirsiniz.

Bu örnek, adlı bir dizinin içeriğini listeler `my-directory` .

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -OutputUserPrincipalName
```

Aşağıdaki örnek, `ACL` `Permissions` `Group` dizindeki her öğenin,, ve `Owner` özelliklerini listeler. `-FetchProperty`Özelliği için değerleri almak için parametresi gereklidir `ACL` . 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$properties = Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -Recurse -FetchProperty
$properties.ACL
$properties.Permissions
$properties.Group
$properties.Owner
```

Bir kapsayıcının içeriğini listelemek için `-Path` komuttan parametreyi atlayın.

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

Cmdlet 'ini kullanarak bir dosyayı dizine yükleyin `New-AzDataLakeGen2Item` .

Bu örnek adlı bir dizine adlı bir dosyayı yükler `upload.txt` `my-directory` . 

```powershell
$localSrcFile =  "upload.txt"
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$destPath = $dirname + (Get-Item $localSrcFile).Name
New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Force 
```

Bu örnek, aynı dosyayı karşıya yükler, ancak ardından hedef dosyanın izinlerini, umaskesini, özellik değerlerini ve meta veri değerlerini ayarlar. Bu örnek ayrıca konsola bu değerleri de yazdırır.

```powershell
$file = New-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $destPath -Source $localSrcFile -Permission rwxrwxrwx -Umask ---rwx--- -Property @{"ContentEncoding" = "UDF8"; "CacheControl" = "READ"} -Metadata  @{"tag1" = "value1"; "tag2" = "value2" }
$file1
$file1.Properties
$file1.Properties.Metadata

```

## <a name="show-file-properties"></a>Dosya özelliklerini göster

Bu örnek, cmdlet 'ini kullanarak bir dosya alır `Get-AzDataLakeGen2Item` ve sonra özellik değerlerini konsola yazdırır.

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

Cmdlet 'ini kullanarak bir dosyayı silin `Remove-AzDataLakeGen2Item` .

Bu örnek adlı bir dosyayı siler `upload.txt` . 

```powershell
$filesystemName = "my-file-system"
$filepath = "upload.txt"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $filepath 
```

`-Force`Dosyayı sormadan kaldırmak için parametresini kullanabilirsiniz.

## <a name="manage-access-permissions"></a>Erişim izinlerini Yönet

Dizinler ve dosyalar için erişim izinlerini alabilir, ayarlayabilir ve güncelleştirebilirsiniz. Bu izinler, erişim denetim listeleri (ACL 'Ler) içinde yakalanır.

> [!NOTE]
> Komutları yetkilendirmek için Azure Active Directory (Azure AD) kullanıyorsanız, güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için  [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)' ne bakın.

### <a name="get-an-acl"></a>ACL al

Cmdlet 'ini kullanarak bir dizin veya dosyanın ACL 'sini alın `Get-AzDataLakeGen2Item` .

Bu örnek, bir **kapsayıcının** kök dizininin ACL 'sini alır ve ardından ACL 'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$filesystem = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName
$filesystem.ACL
```

Bu örnek, bir **DIZININ**ACL 'sini alır ve ardından ACL 'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```

Bu örnek, bir **DOSYANıN** ACL 'sini alır ve ardından ACL 'yi konsola yazdırır.

```powershell
$filePath = "my-directory/upload.txt"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

Aşağıdaki görüntüde bir dizinin ACL 'SI alındıktan sonra çıkış gösterilmektedir.

![Dizin için ACL çıkışı al](./media/data-lake-storage-directory-file-acl-powershell/get-acl.png)

Bu örnekte, sahip olan kullanıcının okuma, yazma ve yürütme izinleri vardır. Sahip olan grubun yalnızca okuma ve yürütme izinleri vardır. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="set-an-acl"></a>ACL ayarla

`set-AzDataLakeGen2ItemAclObject`Sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için BIR ACL oluşturmak üzere cmdlet 'ini kullanın. Ardından, ACL 'yi `Update-AzDataLakeGen2Item` yürütmek için cmdlet 'ini kullanın.

Bu örnek, sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir **kapsayıcının** kök dizinindeki ACL 'yi ayarlar ve ardından ACL 'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission -wx -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Acl $acl
$filesystem = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName
$filesystem.ACL
```

Bu örnek, ACL 'yi sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir **dizinde** ayarlar ve ardından ACL 'yi konsola yazdırır.

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
Bu örnek, sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir **DOSYADAKI** ACL 'yi ayarlar ve ardından ACL 'yi konsola yazdırır.

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

Aşağıdaki görüntüde, bir dosyanın ACL 'sini ayarlamadıktan sonra çıkış gösterilmektedir.

![Dosya için ACL çıkışı al](./media/data-lake-storage-directory-file-acl-powershell/set-acl.png)

Bu örnekte, sahip olan Kullanıcı ve sahip olan Grup yalnızca okuma ve yazma izinlerine sahiptir. Diğer tüm kullanıcılar yazma ve yürütme izinlerine sahiptir. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="add-or-update-an-acl-entry"></a>ACL girdisi ekleme veya güncelleştirme

İlk olarak, ACL 'yi alın. Ardından, `set-AzDataLakeGen2ItemAclObject` BIR ACL girdisi eklemek veya güncelleştirmek için cmdlet 'ini kullanın. `Update-AzDataLakeGen2Item`ACL 'yi yürütmek için cmdlet 'ini kullanın.

Bu örnek, bir kullanıcı için **Dizin** üzerinde ACL oluşturur veya güncelleştirir.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname).ACL
$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityID xxxxxxxx-xxxx-xxxxxxxxxxx -Permission r-x -InputObject $acl 
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
```

### <a name="remove-an-acl-entry"></a>ACL girişini kaldırma

Bu örnek, var olan bir ACL 'den bir girişi kaldırır.

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

### <a name="set-an-acl-recursively-preview"></a>Bir ACL 'yi yinelemeli olarak ayarlama (Önizleme)

Bu değişiklikleri her bir alt öğe için ayrı ayrı yapmak zorunda kalmadan, bir üst dizinin varolan alt öğelerinde ACL 'Leri yinelemeli olarak ekleyebilir, güncelleştirebilir ve kaldırabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Lake Storage 2. için erişim denetim listelerini (ACL 'ler) yinelemeli olarak ayarlama](recursive-access-control-lists.md).

<a id="gen1-gen2-map"></a>

## <a name="gen1-to-gen2-mapping"></a>Gen1 to Gen2 Mapping

Aşağıdaki tabloda, Data Lake Storage 2. için cmdlet 'lerle Data Lake Storage 1. eşleme için kullanılan cmdlet 'ler gösterilmektedir.

|Data Lake Storage 1. cmdlet 'i| Data Lake Storage 2. cmdlet 'i| Notlar |
|--------|---------|-----|
|Get-AzDataLakeStoreChildItem|Get-AzDataLakeGen2ChildItem|Varsayılan olarak, Get-AzDataLakeGen2ChildItem cmdlet 'i yalnızca ilk düzey alt öğeleri listeler. -Recurse parametresi, alt öğeleri yinelemeli olarak listeler. |
|Get-AzDataLakeStoreItem<br>Get-AzDataLakeStoreItemAclEntry<br>Get-AzDataLakeStoreItemOwner<br>Get-AzDataLakeStoreItemPermission|Get-AzDataLakeGen2Item|Get-AzDataLakeGen2Item cmdlet 'inin çıkış öğeleri şu özelliklere sahiptir: ACL, sahip, Grup, Izin.|
|Get-AzDataLakeStoreItemContent|Get-AzDataLakeGen2FileContent|Get-AzDataLakeGen2FileContent cmdlet 'i dosya içeriğini yerel dosyaya indirir.|
|Move-AzDataLakeStoreItem|Move-AzDataLakeGen2Item||
|New-AzDataLakeStoreItem|New-AzDataLakeGen2Item|Bu cmdlet yeni dosya içeriğini yerel bir dosyadan yükler.|
|Remove-AzDataLakeStoreItem|Remove-AzDataLakeGen2Item||
|Set-AzDataLakeStoreItemOwner<br>Set-AzDataLakeStoreItemPermission<br>Set-AzDataLakeStoreItemAcl|Update-AzDataLakeGen2Item|Update-AzDataLakeGen2Item cmdlet 'i yalnızca tek bir öğeyi güncelleştirir ve yinelemeli olarak içermez. Yinelemeli olarak güncelleştirmek istiyorsanız, Get-AzDataLakeStoreChildItem cmdlet 'ini kullanarak öğeleri listeleyin ve sonra da Update-AzDataLakeGen2Item cmdlet 'ine işlem hattı yapın.|
|Test-AzDataLakeStoreItem|Get-AzDataLakeGen2Item|Öğe yoksa Get-AzDataLakeGen2Item cmdlet 'i bir hata bildirir.|

## <a name="see-also"></a>Ayrıca bkz.

* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Depolama PowerShell cmdlet’leri](/powershell/module/az.storage)
