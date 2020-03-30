---
title: Azure Depolama geçiş SSS | Microsoft Dokümanlar
description: Azure Depolama'yı geçirme yle ilgili sık sorulan soruların yanıtları
services: storage
author: genlin
manager: dcscontentpm
ms.service: storage
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.subservice: common
ms.openlocfilehash: 1445d74e3050ffd6da7c45037df552f4bee9acf5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77116678"
---
# <a name="frequently-asked-questions-about-azure-storage-migration"></a>Azure Depolama geçişi hakkında sık sorulan sorular

Bu makalede, Azure Depolama geçişi yle ilgili sık sorulan sorular yanıtlanmaktadır.

## <a name="copy-upload-or-download"></a>Kopyalama, yükleme veya indirme

**Dosyaları bir kapsayıcıdan diğerine kopyalamak için nasıl bir komut dosyası oluştururum?**

Kapsayıcılar arasında dosyaları kopyalamak için AzCopy'yi kullanabilirsiniz. Aşağıdaki örneğe bakın:

    AzCopy /Source:https://xxx.blob.core.windows.net/xxx
    /Dest:https://xxx.blob.core.windows.net/xxx /SourceKey:xxx /DestKey:xxx
    /S

AzCopy, kapsayıcıdaki her dosyayı kopyalamak için [Blob API](https://docs.microsoft.com/rest/api/storageservices/copy-blob) Kopyala'yı kullanır.  

AzCopy çalıştırmak için internet erişimi olan herhangi bir sanal makine veya yerel makine kullanabilirsiniz. Bunu otomatik olarak yapmak için bir Azure Toplu İşlem çizelgesi de kullanabilirsiniz, ancak bu daha karmaşıktır.  

Otomasyon komut dosyası, depolama içeriği işleme sağlık yerine Azure Kaynak Yöneticisi gönderilmesi için tasarlanmıştır. Daha fazla bilgi için Kaynak [Yöneticisi şablonları ve Azure PowerShell ile kaynakları dağıt'a](../../azure-resource-manager/templates/deploy-powershell.md)bakın.

**Aynı bölge içindeki aynı depolama hesabında iki dosya paylaşımı arasındaki verileri kopyalamak için herhangi bir ücret var mı?**

Hayır. Bu işlem için herhangi bir ücret alınmaz.

**Azure portalından 1-2 TB veri nasıl indirebilirim?**

Verileri indirmek için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Portaldaki indirme seçeneğini kullanmak dışında yerel bir makineye vhd nasıl indirebilirim?**

Bir VHD indirmek için [Depolama Gezgini'ni](https://azure.microsoft.com/features/storage-explorer/) kullanabilirsiniz.

**Azure depolama hesabından Linux tabanlı bir bilgisayara verileri nasıl karşıdan yüklerim veya bir Linux makinesinden nasıl veri yüklerim?**

Azure CLI'yi kullanabilirsiniz.

- Tek bir blob indirin:

      azure storage blob download -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -b "<Remote File Name>" -d "<Local path where the file will be downloaded to>"

- Tek bir blob yükleyin:

      azure storage blob upload -k "<Account Key>" -a "<Storage Account Name>" --container "<Blob Container Name>" -f "<Local File Name>"

**Blobs'u bir depolama hesabından diğerine nasıl geçirebilirim?**

 Bunu [Blob geçiş komut dosyamızı](../scripts/storage-common-transfer-between-storage-accounts.md)kullanarak yapabilirsiniz.
 
## <a name="migration-or-backup"></a>Geçiş veya yedekleme

**Verileri bir depolama kabından diğerine nasıl taşıyabilirim?**

Şu adımları uygulayın:

1.  Hedef blob'daki kapsayıcıyı (klasörü) oluşturun.

2.  İçeriği orijinal blob kapsayıcısından farklı bir blob kapsayıcısına kopyalamak için [AzCopy'yi](https://azure.microsoft.com/blog/azcopy-5-1-release/) kullanın.

**Azure Depolama'da verileri bir Azure dosyası paylaşımından diğerine taşımak için nasıl bir PowerShell komut dosyası oluştururum?**

Azure Depolama'da verileri bir Azure dosya paylaşımından diğerine taşımak için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Azure Depolama'ya büyük .csv dosyalarını nasıl yüklerim?**

Azure Depolama'ya büyük .csv dosyaları yüklemek için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Günlükleri her gün D sürücüsünden Azure depolama hesabıma taşımam gerekiyor. Bunu nasıl otomatikleştirebilirim?**

AzCopy'i kullanabilir ve Görev Zamanlayıcısı'nda bir görev oluşturabilirsiniz. AzCopy toplu komut dosyası kullanarak dosyaları Azure depolama hesabına yükleyin. Daha fazla bilgi için, [bir bulut hizmeti için başlangıç görevlerini nasıl yapılandırıp çalıştırılabilirsiniz.](../../cloud-services/cloud-services-startup-tasks.md)

**Depolama hesabımı abonelikler arasında nasıl taşıyabilirim?**

Depolama hesabınızı abonelikler arasında taşımak için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Yaklaşık 10 TB veriyi başka bir bölgede depolamaya nasıl taşıyabilirim?**

Verileri taşımak için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Şirket içi verileri şirket içinde Azure Depolama'ya nasıl kopyalayabilirim?**

Verileri kopyalamak için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Verileri şirket içinde Azure Dosyaları'na nasıl taşıyabilirim?**

Verileri taşımak için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Yönetilen diskleri başka bir depolama hesabına nasıl taşıyabilirim?**

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Şu adımları uygulayın:

1.  Yönetilen diskin bağlı olduğu sanal makineyi durdurun.

2.  Yönetilen disk VHD'yi aşağıdaki Azure PowerShell komut dosyasını çalıştırarak bir alandan diğerine kopyalayın:

    ```
    Connect-AzAccount

    Select-AzSubscription -SubscriptionId <ID>

    $sas = Grant-AzDiskAccess -ResourceGroupName <RG name> -DiskName <Disk name> -DurationInSecond 3600 -Access Read

    $destContext = New-AzStorageContext –StorageAccountName contosostorageav1 -StorageAccountKey <your account key>

    Start-AzStorageBlobCopy -AbsoluteUri $sas.AccessSAS -DestContainer 'vhds' -DestContext $destContext -DestBlob 'MyDestinationBlobName.vhd'
    ```

3.  VHD dosyasını kopyaladığınız başka bir bölgede VHD dosyasını kullanarak yönetilen bir disk oluşturun. Bunu yapmak için aşağıdaki Azure PowerShell komut dosyasını çalıştırın:  

    ```
    $resourceGroupName = 'MDDemo'

    $diskName = 'contoso\_os\_disk1'

    $vhdUri = 'https://contoso.storageaccou.com.vhd

    $storageId = '/subscriptions/<ID>/resourceGroups/<RG name>/providers/Microsoft.Storage/storageAccounts/contosostorageaccount1'

    $location = 'westus'

    $storageType = 'StandardLRS'

    $diskConfig = New-AzDiskConfig -AccountType $storageType -Location $location -CreateOption Import -SourceUri $vhdUri -StorageAccountId $storageId -DiskSizeGB 128

    $osDisk = New-AzDisk -DiskName $diskName -Disk $diskConfig -ResourceGroupName $resourceGroupName
    ```

Yönetilen bir diskten sanal makinenin nasıl dağıtılanöğretilen hakkında daha fazla bilgi için [CreateVmFromManagedOsDisk.ps1 'e](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/blob/master/CreateVmFromManagedOsDisk.ps1)bakın.

**Bir depolama hesabından verileri nasıl taşıyabilirim veya indiririm?**

Verileri indirmek için AzCopy'yi kullanın. Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı ve Linux'ta AzCopy](storage-use-azcopy.md) [ile veri aktarımı'na](storage-use-azcopy-linux.md)bakın.

**Premium depolama hesabından standart bir depolama hesabına nasıl taşınırım?**

Şu adımları uygulayın:

1.  Standart bir depolama hesabı oluşturun. (Veya aboneliğinizde mevcut bir standart depolama hesabı kullanın.)

2.  Azcopy indir. Aşağıdaki AzCopy komutlarından birini çalıştırın.

    Depolama hesabındaki tüm diskleri kopyalamak için:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /S

    Yalnızca bir disk kopyalamak **için, Desen'deki**diskin adını sağlayın:

        AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
        /Dest:https://destaccount.blob.core.windows.net/mycontainer2
        /SourceKey:key1 /DestKey:key2 /Pattern:abc.vhd

İşlemin tamamlanması birkaç saat sürebilir.

Aktarımın başarıyla tamamlandığından emin olmak için Azure portalındaki hedef depolama hesabı kapsayıcısını inceleyin. Diskler standart depolama hesabına kopyalandıktan sonra, bunları varolan bir disk olarak sanal makineye ekleyebilirsiniz. Daha fazla bilgi için, [Azure portalındaki bir Windows sanal makinesine yönetilen bir veri diskinin nasıl ekildiğini](../../virtual-machines/windows/attach-managed-disk-portal.md)öğrenin.  

**Klasik bir depolama hesabından Azure Kaynak Yöneticisi depolama hesabına nasıl geçerim?**

**Move-AzureStorageAccount** cmdlet'ini kullanabilirsiniz. Bu cmdlet birden çok adım (doğrulamak, hazırlamak, işlemek) vardır. Bunu yapmadan önce hareketi doğrulayabilirsiniz.

Sanal makineleriniz varsa, depolama hesabı verilerini geçirmeden önce ek adımlar atmanız gerekir. Daha fazla bilgi için Azure [PowerShell'i kullanarak klasikten Azure Kaynak Yöneticisi'ne IaaS kaynaklarını geçir'e](../..//virtual-machines/windows/migration-classic-resource-manager-ps.md)bakın.

**Tüm depolama hesabımı başka bir depolama hesabına nasıl yedeklerim?**

Tüm depolama hesabını doğrudan yedekleme seçeneği yoktur. Ancak, AzCopy veya Storage Explorer'ı kullanarak bu depolama hesabındaki kapsayıcıyı el ile başka bir hesaba taşıyabilirsiniz. Aşağıdaki adımlar, kapsayıcıyı taşımak için AzCopy'nin nasıl kullanılacağını gösterir:  

1.  [AzCopy](storage-use-azcopy.md) komut satırı aracını yükleyin. Bu araç, VHD dosyasını depolama hesapları arasında taşımanıza yardımcı olur.

2.  Yükleyiciyi kullanarak AzCopy'yi Windows'a yükledikten sonra bir Komut İstemi penceresi açın ve ardından bilgisayarınızdaki AzCopy yükleme klasörüne göz atın. Varsayılan olarak AzCopy **%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy** veya **%ProgramFiles%\Microsoft SDKs\Azure\AzCopy'ye**yüklenir.

3.  Kapsayıcıyı taşımak için aşağıdaki komutu çalıştırın. Metni gerçek değerlerle değiştirmeniz gerekir.   

            AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1
            /Dest:https://destaccount.blob.core.windows.net/mycontainer2
            /SourceKey:key1 /DestKey:key2 /S

    - `/Source`: Kaynak depolama hesabı (konteynere kadar) için URI sağlayın.  
    - `/Dest`: Hedef depolama hesabı (konteynere kadar) için URI sağlayın.  
    - `/SourceKey`: Kaynak depolama hesabı için birincil anahtarı sağlayın. Depolama hesabını seçerek bu anahtarı Azure portalından kopyalayabilirsiniz.  
    - `/DestKey`: Hedef depolama hesabı için birincil anahtarı sağlayın. Depolama hesabını seçerek bu anahtarı portaldan kopyalayabilirsiniz.

Bu komutu çalıştırdıktan sonra, kapsayıcı dosyaları hedef depolama hesabına taşınır.

> [!NOTE]
> AzCopy CLI, bir Azure blob'undan diğerine kopyaladiğinizde **Desen** anahtarıyla birlikte çalışmaz.
>
> AzCopy komutunu doğrudan kopyalayabilir ve düzenleyebilir ve **Desen'in** kaynakla eşleştiğinden emin olmak için çapraz kontrol yapabilirsiniz. Ayrıca **/S** joker kartlarının etkili olduğundan emin olun. Daha fazla bilgi için [AzCopy parametrelerine](storage-use-azcopy.md)bakın.

**Azure dosya depolamasını nasıl yedeklerim?**

Yedek çözüm yok. Ancak Azure Files, eşzamanlı kopyayı da destekler. Yani, dosyaları kopyalayabilirsiniz:

- Bir paylaşımdan bir depolama hesabındaki başka bir paya veya farklı bir depolama hesabına.

- Bir paylaşımdan, depolama hesabı içindeki blob kapsayıcısına veya farklı bir depolama hesabına.

Daha fazla bilgi için [Windows'da AzCopy ile veri aktarımı'na](storage-use-azcopy.md)bakın.
## <a name="configuration"></a>Yapılandırma

**Depolama hesabı için ikincil konumu Avrupa bölgesine nasıl değiştirebilirim?**

Bir depolama hesabı oluşturduğunuzda, hesabın birincil bölgesini seçersiniz. İkincil bölgenin seçimi birincil bölgeye dayanır ve değiştirilemez. Daha fazla bilgi için bkz: [Coğrafi yedekli depolama (GRS): Azure Depolama için bölgeler arası çoğaltma.](storage-redundancy.md)

**Azure Depolama Hizmeti Şifrelemesi (SSE) hakkında daha fazla bilgiyi nereden edinebilirim?**  

Aşağıdaki makalelere bakın:

-  [Azure Depolama güvenlik kılavuzu](../blobs/security-recommendations.md)

-  [Bekleyen Veri için Azure Storage Hizmeti Şifreleme](storage-service-encryption.md)

**Depolama hesabındaki verileri nasıl şifrelerim?**

Bir depolama hesabında şifrelemeyi etkinleştirdikten sonra, varolan veriler şifrelenmez. Varolan verileri şifrelemek için, bu verileri depolama hesabına yeniden yüklemeniz gerekir.

Verileri farklı bir depolama hesabına kopyalamak ve ardından verileri geri taşımak için AzCopy'yi kullanın. [Ayrıca istirahatte şifreleme](storage-service-encryption.md)kullanabilirsiniz.

**Bir depolama hesabının çoğaltılmasını coğrafi yedekli depolamadan yerel olarak yedekli depolamaya değiştirmek için herhangi bir ön koşul var mı?**

Hayır.

**Dosya paylaşımı için Azure Premium Depolama'ya nasıl dönüşüm yapabilirim?**

Azure dosya paylaşımında Premium Depolama'ya izin verilmez.

**Standart bir depolama hesabından premium depolama hesabına nasıl yükseltebilirim? Premium depolama hesabından standart bir depolama hesabına nasıl düşürülebilirim?**

Hedef depolama hesabını oluşturmanız, kaynak hesaptaki verileri hedef hesaba kopyalamanız ve ardından kaynak hesabı silmeniz gerekir. Verileri kopyalamak için AzCopy gibi bir araç kullanabilirsiniz.

Sanal makineleriniz varsa, depolama hesabı verilerini geçirmeden önce ek adımlar atmanız gerekir. Daha fazla bilgi için bkz: [Azure Premium Depolama alanına Geçiş (yönetilmeyen diskler).](storage-migration-to-premium-storage.md)

**Diğer kişilere depolama kaynaklarıma nasıl erişim sağlayabilirim?**

Diğer kişilere depolama kaynaklarına erişim sağlamak için:

-   Kaynağa erişim sağlamak için paylaşılan erişim imzası (SAS) belirteci kullanın.

-   Bir kullanıcıya depolama hesabı için birincil veya ikincil anahtarı sağlayın. Daha fazla bilgi için [bkz.](storage-account-keys-manage.md)

-   Anonim erişime izin vermek için erişim ilkesini değiştirin. Daha fazla bilgi için, [bkz.](../blobs/storage-manage-access-to-resources.md#grant-anonymous-users-permissions-to-containers-and-blobs)

**AzCopy nerede yüklenir?**

-   AzCopy'ye Microsoft Azure Depolama komut satırından eriştiyseniz, **AzCopy**yazın. Komut satırı AzCopy'nin yanına yüklenir.

-   32 bit sürümü yüklediyseniz, burada bulunur: **%ProgramFiles(x86)%\\Microsoft\\SDK'lar\\Azure AzCopy**.

-   64 bit sürümü yüklediyseniz, burada bulunur: **%ProgramFiles%\\Microsoft\\SDK'lar\\Azure AzCopy**.

**Depolama hesabımla birlikte HTTPS özel etki alanını nasıl kullanırım? Örneğin, "https: /mystorageaccountname.blob.core.windows.net/images/image.gif"yi "https:\/\//www.contoso.com/images/image.gif" olarak nasıl görünür hale getirebilirim?**

SSL şu anda özel etki alanları olan depolama hesaplarında desteklenmez.
Ancak, HTTPS olmayan özel etki alanlarını kullanabilirsiniz. Daha fazla bilgi için [bkz. Blob depolama bitiş noktanız için özel bir etki alanı adını yapılandırın.](../blobs/storage-custom-domain-name.md)

## <a name="access-to-storage"></a>Depolamaya erişim

**Sanal bir makinede konteyner klasörünü nasıl haritalakarşılarım?**

Azure dosya paylaşımı kullanın.

**Azure Dosyaları yedek depolama alanına nasıl erişebilirim?**

Yedekdepolamaya erişmek için okuma erişimi coğrafi yedekli depolama gereklidir. Ancak Azure Dosyaları yalnızca yerel olarak yedekli depolamayı ve salt okunur erişime izin vermeyen standart coğrafi yedekdepolamayı destekler.

**Çoğaltılmış bir depolama hesabı (bölge yedekli depolama, coğrafi yedekli depolama veya okuma erişimi coğrafi yedekli depolama gibi) için, ikincil bölgede depolanan verilere nasıl erişebilirim?**

-   Bölge yedekli depolama veya coğrafi yedekli depolama kullanıyorsanız, o bölgeye bir hata başlatmadığınız sürece ikincil bölgeden gelen verilere erişemezsiniz. Başarısız işlem hakkında daha fazla bilgi için [Azure Depolama'da Olağanüstü Durum kurtarma ve depolama hesabı nın başarısızlığına (önizleme)](storage-disaster-recovery-guidance.md)bakın.

-   Okuma-erişim coğrafi yedekli depolama kullanıyorsanız, istediğiniz zaman ikincil bölgeden verilere erişebilirsiniz. Aşağıdaki yöntemlerden birini kullanın:  

    - **AzCopy**: **-secondary** Url'deki depolama hesabı adına ikincil ek, ikincil bitiş noktasına erişmek için. Örnek:  

      https://storageaccountname-secondary.blob.core.windows.net/vhds/BlobName.vhd

    - **SAS belirteci**: Son noktadan verilere erişmek için bir SAS belirteci kullanın. Daha fazla bilgi için [bkz.](storage-sas-overview.md)

**Bir depolama hesabındaki verilere erişmek için FTP'yi nasıl kullanırım?**

FTP kullanarak doğrudan bir depolama hesabına erişmenin bir yolu yoktur. Ancak, bir Azure sanal makinesi ayarlayabilir ve ardından sanal makineye bir FTP sunucusu yükleyebilirsiniz. FTP sunucusunun dosyaları Azure Dosyaları paylaşımında veya sanal makinenin kullanabileceği bir veri diskinde saklamasını sağlayabilirsiniz.

Yalnızca Depolama Gezgini veya benzer bir uygulama kullanmak zorunda kalmadan veri indirmek istiyorsanız, bir SAS belirteci kullanabilirsiniz. Daha fazla bilgi için [bkz.](storage-sas-overview.md)

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Yine de yardıma ihtiyacınız varsa sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
