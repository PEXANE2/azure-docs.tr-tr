---
title: Azure Dosyaları nasıl dağıtılır | Microsoft Dokümanlar
description: Azure Dosyalarını baştan sona nasıl dağıtılayabilirsiniz öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 05/22/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 38339defc9d06f3e809bc24f957ebbb30abb46d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77598791"
---
# <a name="how-to-deploy-azure-files"></a>Azure Dosyaları’nı dağıtma
[Azure Files,](storage-files-introduction.md) endüstri standardı SMB protokolü aracılığıyla erişilebilen bulutta tam olarak yönetilen dosya paylaşımları sunar. Bu makalede, kuruluşunuz içinde Azure Dosyaları'nın nasıl pratik olarak dağıtılanacağı gösterilecek.

Bu makaledeki adımları izlemeden önce [Azure Dosyaları dağıtımı için Planlama'yı](storage-files-planning.md) okumanızı şiddetle öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, aşağıdaki adımları zaten tamamladığınızı varsayar:

- İstediğiniz bölgede istediğiniz esneklik ve şifreleme seçenekleriyle bir Azure Depolama Hesabı oluşturdunuz. Bkz. Depolama Hesabı oluşturma hakkında adım adım yol tarifi için [Bir Depolama Hesabı](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) Oluştur.
- Depolama Hesabınızda istediğiniz kotayla bir Azure dosya paylaşımı oluşturdu. Bkz. Dosya [paylaşımının](storage-how-to-create-file-share.md) nasıl oluşturulabildiğini anlatan adım adım yol tarifi için dosya paylaşımı oluştur.

## <a name="transfer-data-into-azure-files"></a>Verileri Azure Dosyalarına aktarma
Şirket içinde depolananlar gibi varolan dosya paylaşımlarını yeni Azure dosya paylaşımınıza geçirmek isteyebilirsiniz. Bu bölümde, [planlama kılavuzundan](storage-files-planning.md#migration) ayrıntılı olarak belirtilen birkaç popüler yöntem le verileri Azure dosya paylaşımına nasıl taşıyacağınızı gösterecektir

### <a name="azure-file-sync"></a>Azure Dosya Eşitleme
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu, Windows Sunucularınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Azure Dosya Eşitlemi, eşitleme mekanizması uzun süreli kullanım için istenmese bile verileri Azure dosya paylaşımına geçirmek için kullanılabilir. Verileri Azure dosya paylaşımına aktarmak için Azure Dosya Eşitlemi'nin nasıl kullanılacağı hakkında daha fazla bilgi, [Azure Dosya Eşitleme dağıtımı nın planlanması](storage-sync-files-planning.md) ve [Azure Dosya Eşitleme'nin nasıl dağıtılankullanılacağı](storage-sync-files-deployment-guide.md)hakkında daha fazla bilgi bulabilirsiniz.

### <a name="azure-importexport"></a>Azure İçeri/Dışarı Aktarma
Azure İçe Alma/Dışa Aktarma hizmeti, sabit disk sürücülerini bir Azure veri merkezine göndererek büyük miktarda veriyi Azure dosya paylaşımına güvenli bir şekilde aktarmanıza olanak tanır. Bkz. Hizmetin daha ayrıntılı bir özeti için verileri Azure depolama alanına aktarmak için [Microsoft Azure İçe Alma/Dışa Aktarma hizmetini kullanın.](../common/storage-import-export-service.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

> [!Note]  
> Azure İçe Alma/Dışa Aktarma hizmeti, şu anda bir Azure dosya paylaşımından dosya dışa aktarmayı desteklemez.

Aşağıdaki adımlar, şirket içi bir konumdan Azure dosya paylaşımınıza veri aktarılacaktır.

1. Azure'a postayla gönderebilmek için gerekli sayıda sabit disk satın al. Sabit diskler herhangi bir disk boyutunda olabilir, ancak SATA II veya SATA III standardını destekleyen 2,5" veya 3,5" SSD veya HDD olmalıdır. 

2. Veri aktarımını yapan sunucu/bilgisayardaki her diski bağlayın ve monte edin. En iyi performans için, verileri içeren sunucuda şirket içi dışa aktarma işini yerel olarak çalıştırmanızı öneririz. Verilere hizmet veren dosya sunucusu bir NAS aygıtı olduğunda olduğu gibi bazı durumlarda bu mümkün olmayabilir. Bu durumda, her diski bir bilgisayara monte etmek kesinlikle kabul edilebilir.

3. Her sürücünün çevrimiçi olduğundan, parayla baş harfe getirilip bir sürücü mektubu adadığından emin olun. Sürücüyü çevrimiçi duruma getirmek, bir sürücü mektubu nu başlatmave atamak için Disk Yönetimi MMC snap-in'i (diskmgmt.msc) açın.

    - Bir diski çevrimiçi duruma getirmek için (zaten çevrimiçi değilse), Disk Yönetimi MMC'nin alt bölmesindeki diske sağ tıklayın ve "Çevrimiçi"yi seçin.
    - Bir diski başlatmayı sağlamak için alt bölmedeki diske sağ tıklayın (disk çevrimiçi olduktan sonra) ve "Initialize" seçeneğini belirleyin. Sorulduğunda "GPT" seçtiğinizden emin olun.

        ![Disk Yönetimi MMC'deki Initialize Disk menüsünün ekran görüntüsü](media/storage-files-deployment-guide/transferdata-importexport-1.PNG)

    - Diske sürücü mektubu atamak için, çevrimiçi ve başlatılan diskin "ayrılmamış" alanına sağ tıklayın ve "Yeni Basit Ses Düzeyi"ni tıklatın. Bu, sürücü mektubu atamanızı sağlar. Daha sonra yapılacağı için ses düzeyini biçimlendirmeniz gerekmediğini unutmayın.

        ![Disk Yönetimi MMC'deki Yeni Basit Hacim sihirbazının ekran görüntüsü](media/storage-files-deployment-guide/transferdata-importexport-2.png)

4. Veri kümesi CSV dosyasını oluşturun. Veri kümesi CSV dosyası, şirket içi verilere giden yol ile kopyalanması gereken verileri paylaşmak istenen Azure dosyası arasında bir eşlemedir. Örneğin, aşağıdaki veri kümesi CSV dosyası, bir azure dosya paylaşımıyla ("MyAzureFileShare"):
    
    ```
    BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
    "F:\shares\scratch\","MyAzureFileShare/",file,rename,"None",None
    ```

    Depolama Hesabı ile birden çok paylaşım belirtilebilir. Bkz. Daha fazla bilgi için [veri kümesi CSV dosyasını hazırlayın.](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

5. Driveset CSV dosyasını oluşturun. Driveset CSV dosyası, şirket içi dışa aktarma aracısının kullanabileceği diskleri listeler. Örneğin, aşağıdaki driveset CSV `X:`dosya `Y:`listeleri `Z:` ve şirket içi dışa aktarma işinde kullanılacak sürücüler:

    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    X,Format,SilentMode,Encrypt,
    Y,Format,SilentMode,Encrypt,
    Z,Format,SilentMode,Encrypt,
    ```
    
    Bkz. Daha fazla bilgi için [driveset CSV dosyasını hazırlayın.](../common/storage-import-export-tool-preparing-hard-drives-import.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

6. Verilerinizi bir veya daha fazla sabit diske kopyalamak için [WAImportExport Aracı'nı](https://www.microsoft.com/download/details.aspx?id=55280) kullanın.

    ```
    WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
    ```

    > [!Warning]  
    > Disk hazırlamayı tamamladıktan sonra sabit disk sürücülerindeki veya günlük dosyasındaki verileri değiştirmeyin.

7. [Bir alma işi oluşturun.](../common/storage-import-export-data-to-files.md#step-2-create-an-import-job)
    
### <a name="robocopy"></a>Robocopy
Robocopy, Windows ve Windows Server ile birlikte gelen iyi bilinen bir kopyalama aracıdır. Robocopy, dosya paylaşımını yerel olarak monte ederek ve ardından Robocopy komutundaki hedef olarak monte edilen konumu kullanarak verileri Azure Dosyalarına aktarmak için kullanılabilir. Robocopy kullanmak oldukça basittir:

1. [Azure dosya paylaşımınızı monte edin.](storage-how-to-use-files-windows.md) En iyi performans için, Azure dosya paylaşımını verileri içeren sunucuya yerel olarak monte etmemi öneririz. Verilere hizmet veren dosya sunucusu bir NAS aygıtı olduğunda olduğu gibi bazı durumlarda bu mümkün olmayabilir. Bu durumda, Azure dosya paylaşımını bir bilgisayara monte etmek kesinlikle kabul edilebilir. Bu örnekte, `net use` dosya paylaşımını monte etmek için komut satırında kullanılır:

    ```
    net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
    ```

2. Verileri `robocopy` Azure dosya paylaşımına taşımak için komut satırında kullanın:

    ```
    robocopy <path-to-local-share> <path-to-azure-file-share> /E /Z /MT:32
    ```
    
    Robocopy, kopyalama davranışını istediğiniz gibi değiştirmek için önemli sayıda seçenağa sahiptir. Daha fazla bilgi için [Robocopy](https://technet.microsoft.com/library/cc733145.aspx) manuel sayfasını inceleyin.

### <a name="azcopy"></a>AzCopy
AzCopy, Azure Dosyaları'na ve Azure Blob depolama sına veri kopyalamak için tasarlanmış bir komut satırı yardımcı programıdır ve en iyi performansa sahip basit komutları kullanır. AzCopy kullanmak kolaydır:

1. [AzCopy'nin en son sürümünü Windows](https://aka.ms/downloadazcopy) veya [Linux'a](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux#download-and-install-azcopy)indirin.
2. Verileri `azcopy` Azure dosya paylaşımına taşımak için komut satırında kullanın. Windows'daki sözdizimi aşağıdaki gibidir: 

    ```
    azcopy /Source:<path-to-local-share> /Dest:https://<storage-account>.file.core.windows.net/<file-share>/ /DestKey:<storage-account-key> /S
    ```

    Linux'ta komut sözdizimi biraz farklıdır:

    ```
    azcopy --source <path-to-local-share> --destination https://<storage-account>.file.core.windows.net/<file-share>/ --dest-key <storage-account-key> --recursive
    ```

    AzCopy, kopyalama davranışını istediğiniz gibi değiştirmek için önemli sayıda seçenağa sahiptir. Daha fazla bilgi için [Windows'da AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) ve [Linux'ta AzCopy'e](../common/storage-use-azcopy-linux.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)göz at.

## <a name="automatically-mount-on-needed-pcsservers"></a>Gerekli bilgisayarlara/sunuculara otomatik olarak monte edin
Şirket içi dosya paylaşımını değiştirmek için, kullanılacak makinelerdeki paylaşımları önceden monte etmek yararlı olacaktır. Bu, makineler listesinde otomatik olarak yapılabilir.

> [!Note]  
> Azure dosya paylaşımı nı oluşturmak için depolama hesabı anahtarının parola olarak kullanılması gerekmektedir, bu nedenle yalnızca güvenilir ortamlarda montaj yapmanızı öneririz. 

### <a name="windows"></a>Windows
PowerShell birden fazla pCs montaj komutu çalıştırın kullanılabilir. Aşağıdaki örnekte, `$computers` el ile doldurulur, ancak otomatik olarak monte etmek için bilgisayarların listesini oluşturabilirsiniz. Örneğin, bu değişkeni Active Directory'den gelen sonuçlarla doldurabilirsiniz.

```powershell
$computer = "MyComputer1", "MyComputer2", "MyComputer3", "MyComputer4"
$computer | ForEach-Object { Invoke-Command -ComputerName $_ -ScriptBlock { net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name> /PERSISTENT:YES } }
```

### <a name="linux"></a>Linux
SSH ile birlikte basit bir bash komut dosyası aşağıdaki örnekte aynı sonucu verebilir. `$computer` Değişken de benzer şekilde kullanıcı tarafından doldurulmak üzere bırakılır:

```
computer = ("MyComputer1" "MyComputer2" "MyComputer3" "MyComputer4")
for item in "${computer[@]}"
do
    ssh $item "sudo bash -c 'echo \"//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino\" >> /etc/fstab'", "sudo mount -a"
done
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme dağıtımı için plan](storage-sync-files-planning.md)
- [Windows’ta Azure Dosyaları Sorunlarını Giderme](storage-troubleshoot-windows-file-connection-problems.md)
- [Linux’ta Azure Dosyaları Sorunlarını Giderme](storage-troubleshoot-linux-file-connection-problems.md)
