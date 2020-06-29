---
title: Azure Içeri/dışarı aktarma verme işini onarma-v1 | Microsoft Docs
description: Azure Içeri/dışarı aktarma hizmeti kullanılarak oluşturulan ve çalıştırılan bir dışarı aktarma işini onarmayı öğrenin.
author: twooley
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/23/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: 10e209228ad12b377b729bc251eb761b51ff5378
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85514366"
---
# <a name="repairing-an-export-job"></a>Bir dışarı aktarma işini onarma
Bir dışarı aktarma işi tamamlandıktan sonra şirket içi Microsoft Azure İçeri/Dışarı Aktarma Aracı 'nı çalıştırmak için şunları yapabilirsiniz:  
  
1.  Azure Içeri/dışarı aktarma hizmeti 'nin dışarı aktaramadı tüm dosyaları indirin.  
  
2.  Sürücüdeki dosyaların doğru şekilde verildiğini doğrulayın.  
  
Bu işlevselliği kullanmak için Azure depolama bağlantısına sahip olmanız gerekir.  
  
Bir içeri aktarma işini onarmak için komutu **Repaırexport**' dir.

## <a name="repairexport-parameters"></a>Repaırexport parametreleri

Aşağıdaki parametreler **Repaırexport**ile belirtilebilir:  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|**/r: <RepairFile\>**|Gereklidir. Onarımın ilerlemesini izleyen ve kesilen bir onarımı sürdürmeye devam etmenizi sağlayan onarım dosyasının yolu. Her sürücünün bir ve yalnızca bir onarım dosyası olmalıdır. Belirli bir sürücü için onarım başlattığınızda, yolu henüz varolmayan bir onarım dosyasına geçitirsiniz. Kesilen bir onarımın sürdürülmesi için, var olan bir onarım dosyasının adını geçirmeniz gerekir. Hedef sürücüye karşılık gelen onarım dosyasının her zaman belirtilmesi gerekir.|  
|**/logdir: <LogDirectory\>**|İsteğe bağlı. Günlük dizini. Ayrıntılı günlük dosyaları, bu dizine yazılacak. Günlük dizini belirtilmemişse, geçerli dizin günlük dizini olarak kullanılacaktır.|  
|**/d: <TargetDirectory\>**|Gereklidir. Doğrulanacak ve onarılacak dizin. Bu genellikle dışa aktarma sürücüsünün kök dizinidir, ancak dışarı aktarılmış dosyaların bir kopyasını içeren bir ağ dosya paylaşma da olabilir.|  
|**/BK: <BitLockerKey\>**|İsteğe bağlı. Aracın, dışarıya kaydedilen dosyaların depolandığı bir şifrelenmiş kilit kilidini açmak istiyorsanız BitLocker anahtarını belirtmeniz gerekir.|  
|**/sn: <StorageAccountName\>**|Gereklidir. Dışarı aktarma işi için depolama hesabının adı.|  
|**/SK: <StorageAccountKey\>**|Yalnızca bir kapsayıcı SAS belirtilmemişse **gereklidir** . Dışarı aktarma işi için depolama hesabının hesap anahtarı.|  
|**/csas: <ContainerSas\>**|Yalnızca depolama hesabı anahtarı belirtilmemişse **gereklidir** . Dışarı aktarma işiyle ilişkili bloblara erişmek için kapsayıcı SAS.|  
|**/CopyLogFile: <DriveCopyLogFile\>**|Gereklidir. Sürücü kopyalama günlük dosyasının yolu. Dosya, Windows Azure Içeri/dışarı aktarma hizmeti tarafından oluşturulur ve işle ilişkili blob depolamadan indirilebilir. Kopyalama günlük dosyası, başarısız Bloblar veya onarılabilecek dosyalar hakkında bilgiler içerir.|  
|**/ManifestFile: <DriveManifestFile\>**|İsteğe bağlı. Dışa aktarma sürücüsünün bildirim dosyasının yolu. Bu dosya, Windows Azure Içeri/dışarı aktarma hizmeti tarafından oluşturulur ve dışa aktarma sürücüsüne ve isteğe bağlı olarak işle ilişkili depolama hesabındaki bir blobda depolanır.<br /><br /> Dışarı aktarma sürücüsündeki dosyaların içeriği bu dosyada bulunan MD5 karmalarıyla doğrulanır. Bozulmuş olduğu belirlenen tüm dosyalar, hedef dizinlere indirilir ve yeniden yazılır.|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a>Başarısız dışarı aktarmaları düzeltmek için Repaırexport modunu kullanma  
Dışarı aktarabileceğiniz dosyaları indirmek için Azure Içeri/dışarı aktarma aracını kullanabilirsiniz. Kopyalama günlük dosyası, dışarı aktarmayacak dosyaların bir listesini içerir.  
  
Dışarı aktarma hatalarının nedenleri aşağıdaki olasılıkları içerir:  
  
-   Hasarlı sürücüler  
  
-   Aktarım işlemi sırasında depolama hesabı anahtarı değişti  
  
Aracı **Repaırexport** modunda çalıştırmak için, önce, dosyaya verdiğiniz dosyaları içeren sürücüyü bilgisayarınıza bağlamanız gerekir. Ardından, parametresi ile bu sürücünün yolunu belirterek Azure Içeri/dışarı aktarma aracı 'nı çalıştırın `/d` . Ayrıca, indirdiğiniz sürücünün kopya günlük dosyasının yolunu da belirtmeniz gerekir. Aşağıdaki komut satırı örneği, dışarı aktarılmaya başarısız olan dosyaları onarmak için aracı çalıştırır:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
Aşağıda, blobdaki bir bloğun dışarı aktarılacağı bir blok olan bir kopya günlük dosyası örneği verilmiştir:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
  <DriveId>9WM35C2V</DriveId>  
  <Blob Status="CompletedWithErrors">  
    <BlobPath>pictures/wild/desert.jpg</BlobPath>  
    <FilePath>\pictures\wild\desert.jpg</FilePath>  
    <LastModified>2012-09-18T23:47:08Z</LastModified>  
    <Length>163840</Length>  
    <BlockList>  
      <Block Offset="65536" Length="65536" Id="AQAAAA==" Status="Failed" />  
    </BlockList>  
  </Blob>  
  <Status>CompletedWithErrors</Status>  
</DriveLog>  
```  
  
Kopyalama günlük dosyası, Microsoft Azure Içeri/dışarı aktarma hizmeti, blob 'un bloklarından birini dışarı aktarma sürücüsündeki dosyaya indirirken bir hata oluştuğunu belirtir. Dosyanın diğer bileşenleri başarıyla indirildi ve dosya uzunluğu doğru şekilde ayarlanmış. Bu durumda, araç dosyayı sürücüde açar, blok depolama hesabından indirir ve 65536 ' den başlayarak 65536 uzunluğunda bir dosya aralığına yazar.  
  
## <a name="using-repairexport-to-validate-drive-contents"></a>Sürücü içeriğini doğrulamak için Repaırexport kullanma  
Ayrıca, sürücüdeki içeriklerin doğru olduğunu doğrulamak için Azure Içeri/dışarı aktarma ile **Repaırexport** seçeneğini kullanabilirsiniz. Her dışarı aktarma sürücüsündeki bildirim dosyası, sürücünün içeriği için MD5s içerir.  
  
Azure Içeri/dışarı aktarma hizmeti, bildirim dosyalarını dışarı aktarma işlemi sırasında bir depolama hesabına de kaydedebilir. Bildirim dosyalarının konumu, iş tamamlandığında [Iş al](/rest/api/storageimportexport/jobs) işlemi yoluyla kullanılabilir. Bir sürücü bildirim dosyasının biçimi hakkında daha fazla bilgi için bkz. [içeri/dışarı aktarma hizmet bildirimi dosyası biçimi](storage-import-export-file-format-metadata-and-properties.md) .  
  
Aşağıdaki örnek, **/ManifestFile** ve **/CopyLogFile** parametreleriyle Azure Içeri/dışarı aktarma aracının nasıl çalıştırılacağını göstermektedir:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log /ManifestFile:G:\9WM35C3U.manifest  
```  
  
Aşağıda bir bildirim dosyası örneği verilmiştir:  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveManifest Version="2011-10-01">  
  <Drive>  
    <DriveId>9WM35C3U</DriveId>  
    <ClientCreator>Windows Azure Import/Export service</ClientCreator>  
    <BlobList>
      <Blob>  
        <BlobPath>pictures/city/redmond.jpg</BlobPath>  
        <FilePath>\pictures\city\redmond.jpg</FilePath>  
        <Length>15360</Length>  
        <PageRangeList>  
          <PageRange Offset="0" Length="3584" Hash="72FC55ED9AFDD40A0C8D5C4193208416" />  
          <PageRange Offset="3584" Length="3584" Hash="68B28A561B73D1DA769D4C24AA427DB8" />  
          <PageRange Offset="7168" Length="512" Hash="F521DF2F50C46BC5F9EA9FB787A23EED" />  
        </PageRangeList>  
        <PropertiesPath Hash="E72A22EA959566066AD89E3B49020C0A">\pictures\city\redmond.jpg.properties</PropertiesPath>  
      </Blob>  
      <Blob>  
        <BlobPath>pictures/wild/canyon.jpg</BlobPath>  
        <FilePath>\pictures\wild\canyon.jpg</FilePath>  
        <Length>10884</Length>  
        <BlockList>  
          <Block Offset="0" Length="2721" Id="AAAAAA==" Hash="263DC9C4B99C2177769C5EBE04787037" />  
          <Block Offset="2721" Length="2721" Id="AQAAAA==" Hash="0C52BAE2CC20EFEC15CC1E3045517AA6" />  
          <Block Offset="5442" Length="2721" Id="AgAAAA==" Hash="73D1CB62CB426230C34C9F57B7148F10" />  
          <Block Offset="8163" Length="2721" Id="AwAAAA==" Hash="11210E665C5F8E7E4F136D053B243E6A" />  
        </BlockList>  
        <PropertiesPath Hash="81D7F81B2C29F10D6E123D386C3A4D5A">\pictures\wild\canyon.jpg.properties</PropertiesPath>  
      </Blob> 
    </BlobList>  
 </Drive>  
</DriveManifest>  
``` 
  
Onarım sürecini tamamladıktan sonra, araç, bildirim dosyasında başvurulan her bir dosyayı okur ve dosyanın MD5 karmalarıyla bütünlüğünü doğrular. Yukarıdaki bildirim için aşağıdaki bileşenlere gidecektir.  

```  
G:\pictures\city\redmond.jpg, offset 0, length 3584  
  
G:\pictures\city\redmond.jpg, offset 3584, length 3584  
  
G:\pictures\city\redmond.jpg, offset 7168, length 3584  
  
G:\pictures\city\redmond.jpg.properties  
  
G:\pictures\wild\canyon.jpg, offset 0, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 2721, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 5442, length 2721  
  
G:\pictures\wild\canyon.jpg, offset 8163, length 2721  
  
G:\pictures\wild\canyon.jpg.properties  
```

Doğrulama başarısız olan herhangi bir bileşen araç tarafından indirilir ve sürücüdeki aynı dosyaya yeniden yazılır.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure Içeri/dışarı aktarma aracı 'nı ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
