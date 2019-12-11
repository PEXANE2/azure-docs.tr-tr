---
title: Azure Data Lake Storage 2. & ACL 'Leri için PowerShell kullanma (Önizleme)
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için PowerShell cmdlet 'lerini kullanın.
services: storage
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 11/24/2019
ms.author: normesta
ms.reviewer: prishet
ms.openlocfilehash: f2a2eaa3224fff117a30dfb742b4f8a35196dba4
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74973909"
---
# <a name="use-powershell-for-files--acls-in-azure-data-lake-storage-gen2-preview"></a>Azure Data Lake Storage 2. & ACL 'Leri için PowerShell kullanma (Önizleme)

Bu makalede, PowerShell kullanarak hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında Dizin, dosya ve izinleri oluşturma ve bunları yönetme işlemi gösterilmektedir. 

> [!IMPORTANT]
> Bu makalede tanıtılan PowerShell modülü şu anda genel önizleme aşamasındadır.

[Gen1 to Gen2 mapping](#gen1-gen2-map) | [geri bildirim verme](https://github.com/Azure/azure-powershell/issues)

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.
> * .NET Framework, 4.7.2 veya üzeri bir sürümü yüklendi. Bkz. [indirme .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).
> * PowerShell sürüm `5.1` veya üzeri.

## <a name="install-powershell-modules"></a>PowerShell modüllerini yükler

1. Yüklü olan PowerShell sürümünün aşağıdaki komutu kullanarak `5.1` veya daha yüksek olduğunu doğrulayın. 

    ```powershell
    echo $PSVersionTable.PSVersion.ToString() 
    ```
    
    PowerShell sürümünüzü yükseltmek için bkz. [var olan Windows PowerShell 'ı yükseltme](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell)
    
2. En son **PowerShellGet** modülünü yükler. Ardından, PowerShell konsolunu kapatıp yeniden açın.

    ```powershell
    install-Module PowerShellGet –Repository PSGallery –Force 
    ```

3.  Install **az. Storage** Preview Module.

    ```powershell
    install-Module Az.Storage -Repository PSGallery -RequiredVersion 1.9.1-preview –AllowPrerelease –AllowClobber –Force 
    ```

    PowerShell modüllerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz [. Azure PowerShell modülünü Install](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0)

## <a name="connect-to-the-account"></a>Hesaba Bağlan

1. Bir Windows PowerShell komut penceresi açın.

2. `Connect-AzAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

   ```powershell
   Connect-AzAccount
   ```

3. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi ' de dizin oluşturup yönetmek istediğiniz depolama hesabının aboneliğine ayarlayın.

   ```powershell
   Select-AzSubscription -SubscriptionId <subscription-id>
   ```

   `<subscription-id>` yer tutucu değerini aboneliğinizin KIMLIĞIYLE değiştirin.

4. Depolama hesabını alın.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   ```

   * `<resource-group-name>` yer tutucu değerini kaynak grubunuzun adıyla değiştirin.

   * `<storage-account-name>` yer tutucu değerini depolama hesabınızın adıyla değiştirin.

5. Depolama hesabı bağlamını alın.

   ```powershell
   $ctx = $storageAccount.Context
   ```

## <a name="create-a-file-system"></a>Dosya sistemi oluşturma

Dosya sistemi dosyalarınız için bir kapsayıcı olarak davranır. `New-AzDatalakeGen2FileSystem` cmdlet 'ini kullanarak bir tane oluşturabilirsiniz. 

Bu örnek `my-file-system`adlı bir dosya sistemi oluşturur.

```powershell
$filesystemName = "my-file-system"
New-AzDatalakeGen2FileSystem -Context $ctx -Name $filesystemName
```

## <a name="create-a-directory"></a>Dizin oluşturma

`New-AzDataLakeGen2Item` cmdlet 'ini kullanarak bir dizin başvurusu oluşturun. 

Bu örnek, `my-directory` adlı bir dizini bir dosya sistemine ekler.

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

Bu örnek, `Get-AzDataLakeGen2Item` cmdlet 'ini kullanarak bir dizin alır ve sonra özellik değerlerini konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dir =  Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
$dir.Permissions
$dir.Directory.PathProperties.Group
$dir.Directory.PathProperties.Owner
$dir.Directory.Metadata
$dir.Directory.Properties
```

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

`Move-AzDataLakeGen2Item` cmdlet 'ini kullanarak bir dizini yeniden adlandırın veya taşıyın.

Bu örnek `my-directory` adı bir dizini ad `my-new-directory`olarak adlandırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-new-directory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -DestFileSystem $filesystemName -DestPath $dirname2
```

Bu örnek, `my-directory` adlı bir dizini `my-subdirectory`adlı `my-directory-2` alt dizinine taşır. Bu örnek alt dizine umask de uygular.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$dirname2 = "my-directory-2/my-subdirectory/"
Move-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname1 -DestFileSystem $filesystemName -DestPath $dirname2 -Umask --------x -PathRenameMode Posix
```

## <a name="delete-a-directory"></a>Bir dizini silme

`Remove-AzDataLakeGen2Item` cmdlet 'ini kullanarak bir dizini silin.

Bu örnek, `my-directory`adlı bir dizini siler. 

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $dirname 
```

Dosyayı sormadan kaldırmak için `-Force` parametresini kullanabilirsiniz.

## <a name="download-from-a-directory"></a>Bir dizinden indir

`Get-AzDataLakeGen2ItemContent` cmdlet 'ini kullanarak bir dizinden dosya indirin.

Bu örnek, `my-directory`adlı bir dizinden `upload.txt` adlı bir dosyayı indirir. 

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$downloadFilePath = "download.txt"
Get-AzDataLakeGen2ItemContent -Context $ctx -FileSystem $filesystemName -Path $filePath -Destination $downloadFilePath
```

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

`Get-AzDataLakeGen2ChildItem` cmdlet 'ini kullanarak bir dizinin içeriğini listeleyin.

Bu örnek, `my-directory`adlı bir dizinin içeriğini listeler. 

Bir dosya sisteminin içeriğini listelemek için, komutundan `-Path` parametresini atlayın.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname
```

Bu örnek, `my-directory` adlı bir dizinin içeriğini listeler ve listedeki ACL 'Leri içerir. Ayrıca, tüm alt dizinlerin içeriğini listelemek için `-Recurse` parametresini kullanır.

Bir dosya sisteminin içeriğini listelemek için, komutundan `-Path` parametresini atlayın.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Path $dirname -Recurse -FetchPermission
```

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

`New-AzDataLakeGen2Item` cmdlet 'ini kullanarak bir dosyayı dizine yükleyin.

Bu örnek, `upload.txt` adlı bir dosyayı `my-directory`adlı dizine yükler. 

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
$file1.File.Metadata
$file1.File.Properties
```

## <a name="show-file-properties"></a>Dosya özelliklerini göster

Bu örnek, `Get-AzDataLakeGen2Item` cmdlet 'ini kullanarak bir dosya alır ve sonra özellik değerlerini konsola yazdırır.

```powershell
$filepath =  "my-directory/upload.txt"
$filesystemName = "my-file-system"
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filepath
$file
$file.ACL
$file.Permissions
$file.File.PathProperties.Group
$file.File.PathProperties.Owner
$file.File.Metadata
$file.File.Properties
```

## <a name="delete-a-file"></a>Dosyayı silme

`Remove-AzDataLakeGen2Item` cmdlet 'ini kullanarak bir dosyayı silin.

Bu örnek `upload.txt`adlı bir dosyayı siler. 

```powershell
$filesystemName = "my-file-system"
$filepath = "upload.txt"
Remove-AzDataLakeGen2Item  -Context $ctx -FileSystem $filesystemName -Path $filepath 
```

Dosyayı sormadan kaldırmak için `-Force` parametresini kullanabilirsiniz.

## <a name="manage-access-permissions"></a>Erişim izinlerini Yönet

Dizinler ve dosyalar için erişim izinlerini alabilir, ayarlayabilir ve güncelleştirebilirsiniz.

### <a name="get-directory-and-file-permissions"></a>Dizin ve dosya izinlerini alın

`Get-AzDataLakeGen2Item`cmdlet 'ini kullanarak bir dizin veya dosyanın ACL 'sini alın.

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

![ACL çıkışı al](./media/data-lake-storage-directory-file-acl-powershell/get-acl.png)

Bu örnekte, sahip olan kullanıcının okuma, yazma ve yürütme izinleri vardır. Sahip olan grubun yalnızca okuma ve yürütme izinleri vardır. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="set-directory-and-file-permissions"></a>Dizin ve dosya izinlerini ayarlama

Sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir ACL oluşturmak üzere `New-AzDataLakeGen2ItemAclObject` cmdlet 'ini kullanın. Ardından, ACL 'yi yürütmek için `Update-AzDataLakeGen2Item` cmdlet 'ini kullanın.

Bu örnek, ACL 'yi sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir **dizinde** ayarlar ve ardından ACL 'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$dir = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname
$dir.ACL
```
Bu örnek, sahip olan Kullanıcı, sahip olan grup veya diğer kullanıcılar için bir **DOSYADAKI** ACL 'yi ayarlar ve ardından ACL 'yi konsola yazdırır.

```powershell
$filesystemName = "my-file-system"
$filePath = "my-directory/upload.txt"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath -Acl $acl
$file = Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $filePath
$file.ACL
```

Aşağıdaki görüntüde, bir dosyanın ACL 'sini ayarlamadıktan sonra çıkış gösterilmektedir.

![ACL çıkışı al](./media/data-lake-storage-directory-file-acl-powershell/set-acl.png)

Bu örnekte, sahip olan Kullanıcı ve sahip olan Grup yalnızca okuma ve yazma izinlerine sahiptir. Diğer tüm kullanıcılar yazma ve yürütme izinlerine sahiptir. Erişim denetim listeleri hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. Access Control](data-lake-storage-access-control.md).

### <a name="update-directory-and-file-permissions"></a>Dizin ve dosya izinlerini güncelleştirme

Bir dizin veya dosyanın ACL 'sini almak için `Get-AzDataLakeGen2Item` cmdlet 'ini kullanın. Ardından, yeni bir ACL girişi oluşturmak için `New-AzDataLakeGen2ItemAclObject` cmdlet 'ini kullanın. Yeni ACL 'yi uygulamak için `Update-AzDataLakeGen2Item` cmdlet 'ini kullanın.

Bu örnek, bir dizin üzerinde Kullanıcı yazma ve yürütme izni verir.

```powershell
$filesystemName = "my-file-system"
$dirname = "my-directory/"
$Id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname).ACL
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $id -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
```
Bu örnek, bir dosya üzerinde Kullanıcı yazma ve yürütme izni verir.

```powershell
$filesystemName = "my-file-system"
$fileName = "my-directory/upload.txt"
$Id = "xxxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
$acl = (Get-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $fileName).ACL
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $id -Permission "-wx" -InputObject $acl
Update-AzDataLakeGen2Item -Context $ctx -FileSystem $filesystemName -Path $fileName -Acl $acl
```

### <a name="set-permissions-on-all-items-in-a-file-system"></a>Dosya sistemindeki tüm öğelerde izinleri ayarla

Bir dosya sistemindeki tüm dizinlerin ve dosyaların ACL 'sini ayarlamak için, `Get-AzDataLakeGen2Item` ve `-Recurse` parametresini `Update-AzDataLakeGen2Item` cmdlet 'i ile birlikte kullanabilirsiniz. 

```powershell
$filesystemName = "my-file-system"
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rw- 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission rw- -InputObject $acl 
$acl = New-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "-wx" -InputObject $acl
Get-AzDataLakeGen2ChildItem -Context $ctx -FileSystem $filesystemName -Recurse | Update-AzDataLakeGen2Item -Acl $acl
```
<a id="gen1-gen2-map" />

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
* [Azure depolama ile Azure PowerShell kullanma](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
* [Depolama PowerShell cmdlet 'leri](/powershell/module/az.storage).

