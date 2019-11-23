---
title: 'Tutorial: Migrate on-premises data to Azure Storage with AzCopy| Microsoft Docs'
description: Bu öğreticide AzCopy komutunu kullanarak blob, tablo ve dosya içeriğine/içeriğinden verileri geçirecek veya kopyalayacaksınız. Yerel depolama alanınızdan Azure Depolama’ya kolayca verileri geçirin.
author: normesta
ms.service: storage
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: normesta
ms.reviewer: seguler
ms.subservice: common
ms.openlocfilehash: f7155053072b3533503765dc6f4fbf185d21f0d4
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74327511"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-with-azcopy"></a>Tutorial: Migrate on-premises data to cloud storage with AzCopy

AzCopy; basit komutlar kullanılarak Azure Blob depolamaya, Azure Dosyaları’na ve Azure Tablosu depolama alanına veya bunlardan veri kopyalamaya yönelik bir komut satırı aracıdır. Komutlar, en iyi performans için tasarlanmıştır. AzCopy ile bir dosya sistemi ile depolama hesabı arasında veya depolama hesapları arasında verileri kopyalayabilirsiniz. AzCopy, yerel verileri (şirket içi) bir depolama hesabına kopyalamak için kullanılabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Depolama hesabı oluşturma. 
> * AzCopy komutunu kullanarak tüm verilerinizi karşıya yükleme.
> * Test amacıyla verileri değiştirme.
> * Karşıya yüklenecek yeni dosyaları belirlemek için zamanlanmış bir görev veya sıralanmış iş oluşturma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

To complete this tutorial, download the latest version of AzCopy. See [Get started with AzCopy](storage-use-azcopy-v10.md).

Windows kullanıyorsanız bu öğreticide görev zamanlamak için kullanılan [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) uygulamasını edinmeniz gerekir. Linux kullanıcıları bunun yerine crontab komutunu kullanacaktır.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

İlk adım bir kapsayıcı oluşturmaktır. Blobların her zaman bir kapsayıcı içine yüklenmesi gerekir. Bilgisayarınızdaki dosyaları düzenlemek için klasörleri kullandığınız gibi blob gruplarını düzenlemek için de kapsayıcılardan faydalanabilirsiniz.

Kapsayıcı oluşturmak için şu adımları izleyin:

1. Ana sayfadan **Depolama hesapları** düğmesini seçin ve oluşturduğunuz depolama hesabını seçin.
2. **Hizmetler** bölümünden **Bloblar**’ı seçin ve sonra **Kapsayıcı**’yı seçin.

   ![Bir kapsayıcı oluşturma](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
Kapsayıcı harfleri bir harf veya sayı ile başlamalıdır. Bunlar yalnızca harf, sayı ve kısa çizgi karakterini (-) içerebilir. Kapsayıcıları ve blobları adlandırma kuralları hakkında daha fazla bilgi için bkz. [Kapsayıcıları, blobları ve meta verileri adlandırma ve bunlara başvurma](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="download-azcopy"></a>Download AzCopy

Download the AzCopy V10 executable file.

- [Windows](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (tar)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

Place the AzCopy file anywhere on your computer. Add the location of the file to your system path variable so that you can refer to this executable file from any folder on your computer.

## <a name="authenticate-with-azure-ad"></a>Azure AD ile kimlik doğrulaması

First, assign the [Storage Blob Data Contributor](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor) role to your identity. See [Grant access to Azure blob and queue data with RBAC in the Azure portal](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal).

Then, open a command prompt, type the following command, and press the ENTER key.

```azcopy
azcopy login
```

This command returns an authentication code and the URL of a website. Open the website, provide the code, and then choose the **Next** button.

![Bir kapsayıcı oluşturma](media/storage-use-azcopy-v10/azcopy-login.png)

A sign-in window will appear. In that window, sign into your Azure account by using your Azure account credentials. After you've successfully signed in, you can close the browser window and begin using AzCopy.

## <a name="upload-contents-of-a-folder-to-blob-storage"></a>Bir klasörün içeriğini Blob depolama alanına yükleme

AzCopy komutunu kullanarak, bir klasördeki tüm dosyaları, [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) veya [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) üzerindeki Blob depolama alanına yükleyebilirsiniz. Bir klasördeki tüm blobları karşıya yüklemek için aşağıdaki AzCopy komutunu girin:

```AzCopy
azcopy copy "<local-folder-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive=true
```

* Replace the `<local-folder-path>` placeholder with the path to a folder that contains files (For example: `C:\myFolder` or `/mnt/myFolder`).

* Replace the `<storage-account-name>` placeholder with the name of your storage account.

* Replace the `<container-name>` placeholder with the name of the container that you created.

To upload the contents of the specified directory to Blob storage recursively, specify the `--recursive` option. When you run AzCopy with this option, all subfolders and their files are uploaded as well.

## <a name="upload-modified-files-to-blob-storage"></a>Değiştirilen dosyaları Blob depolama alanına yükleme

You can use AzCopy to upload files based on their last-modified time. 

Bunu denemek için, test amacıyla kaynak dizininizde yeni dosyalar oluşturun veya değiştirin. Then, use the AzCopy `sync` command.

```AzCopy
azcopy sync "<local-folder-path>" "https://<storage-account-name>.blob.core.windows.net/<container-name>" --recursive=true
```

* Replace the `<local-folder-path>` placeholder with the path to a folder that contains files (For example: `C:\myFolder` or `/mnt/myFolder`.

* Replace the `<storage-account-name>` placeholder with the name of your storage account.

* Replace the `<container-name>` placeholder with the name of the container that you created.

To learn more about the `sync` command, see [Synchronize files](storage-use-azcopy-blobs.md#synchronize-files).

## <a name="create-a-scheduled-task"></a>Zamanlanmış görev oluşturma

AzCopy komut betiğini çalıştıran bir zamanlanmış görev veya sıralanmış iş oluşturabilirsiniz. Betik, belirli bir zaman aralığında yeni şirket içi verileri belirler ve bulut depolama alanına yükler.

AzCopy komutunu bir metin düzenleyiciye kopyalayın. AzCopy komutunun parametre değerlerini uygun değerlerle güncelleştirin. Dosyayı, AzCopy için `script.sh` (Linux) veya `script.bat` (Windows) olarak kaydedin. 

These examples assume that your folder is named `myFolder`, your storage account name is `mystorageaccount` and your container name is `mycontainer`.

> [!NOTE]
> The Linux example appends a SAS token. You'll need to provide one in your command. The current version of AzCopy V10 doesn't support Azure AD authorization in cron jobs.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy sync "/mnt/myfiles" "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-05-30T06:57:40Z&st=2019-05-29T22:57:40Z&spr=https&sig=BXHippZxxx54hQn%2F4tBY%2BE2JHGCTRv52445rtoyqgFBUo%3D" --recursive=true

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    azcopy sync "C:\myFolder" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive=true

---

Bu öğreticide, Windows üzerinde zamanlanmış görev oluşturmak için [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) kullanılır. Linux üzerinde bir sıralanmış iş oluşturmak için [Crontab](http://crontab.org/) komutu kullanılır.

 **Schtasks**, bir yöneticinin yerel veya uzak bilgisayarda zamanlanmış görevler oluşturmasına, silmesine, sorgulamasına, değiştirmesine, çalıştırmasına ve sonlandırmasına olanak sağlar. **Cron**, Linux ve Unix kullanıcılarının [sıralanmış iş ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression) kullanarak belirtilen bir tarih ve saatte komutları veya betikleri çalıştırmasına olanak sağlar.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Linux üzerinde sıralanmış iş oluşturmak için bir terminale aşağıdaki komutu girin:

```bash
crontab -e
*/5 * * * * sh /path/to/script.sh
```

Komutta `*/5 * * * *` sıralanmış iş ifadesi belirtilmesi, `script.sh` kabuk betiğinin beş dakikada bir çalıştırılacağını belirtir. Betiği, günlük, aylık veya yıllık olarak belirli bir saatte çalıştırılacak şekilde zamanlayabilirsiniz. İş yürütme için tarih ve saati ayarlama hakkında daha fazla bilgi edinmek istiyorsanız bkz. [sıralanmış iş ifadeleri](https://en.wikipedia.org/wiki/Cron#CRON_expression).

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Windows üzerinde zamanlanmış görev oluşturmak için, komut satırına veya PowerShell’e aşağıdaki komutu girin:

This example assumes that your script is located in the root drive of your computer, but your script can be anywhere that you want.

```cmd
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\script.bat
```

Komut:
- Bir dakikalık zamanlamayı belirtmek için `/SC` parametresini kullanır.
- Beş dakikalık zaman aralığını belirtmek için `/MO` parametresini kullanır.
- Görev adını belirtmek için `/TN` parametresini kullanır.
- `script.bat` dosyasının yolunu belirtmek için `/TR` parametresini kullanır.

Windows üzerinde zamanlanmış görev oluşturma hakkında daha fazla bilgi için bkz. [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---

Zamanlanmış görevin/sıralanmış işin düzgün şekilde çalıştığını doğrulamak için `myFolder` dizininizde yeni dosyalar oluşturun. Yeni dosyaların depolama hesabınıza yüklendiğini onaylamak için beş dakika bekleyin. Zamanlanmış görevin veya sıralanmış işin çıktı günlüklerini görüntülemek için günlük dizininize gidin.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi verileri Azure Depolama’ya (veya tam tersi) taşıma yolları hakkında daha fazla bilgi edinmek için bkz.:

* [Azure Depolamadan/Depolamaya veri taşıma](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).  

For more information about AzCopy, see any of these articles:

* [Get started with AzCopy](storage-use-azcopy-v10.md)

* [Transfer data with AzCopy and blob storage](storage-use-azcopy-blobs.md)

* [Transfer data with AzCopy and file storage](storage-use-azcopy-files.md)

* [Transfer data with AzCopy and Amazon S3 buckets](storage-use-azcopy-s3.md)
 
* [Configure, optimize, and troubleshoot AzCopy](storage-use-azcopy-configure.md)
