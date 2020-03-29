---
title: Azure İçe Alma/Dışa Aktarma işini onarma - v1 | Microsoft Dokümanlar
description: Azure İçe Alma/Dışa Aktarma hizmetini kullanarak oluşturulan ve çalıştırılabilen bir dışa aktarma işini nasıl onarın.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: b2ba30bddfc6364c79e1bb01d30cde63b261a07f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74978024"
---
# <a name="repairing-an-export-job"></a>Bir dışarı aktarma işini onarma
Bir dışa aktarma işi tamamlandıktan sonra, Microsoft Azure İçe Aktarma/Dışa Aktarma Aracı'nı şirket içinde şu şekilde çalıştırabilirsiniz:  
  
1.  Azure İçe Aktarma/Dışa Aktarma hizmetinin dışa aktaramadığı dosyaları karşıdan yükleyin.  
  
2.  Sürücüdeki dosyaların doğru şekilde dışa aktarıldığını doğrulayın.  
  
Bu işlevi kullanabilmek için Azure Depolama bağlantısına sahip olmalısınız.  
  
Bir alma işini onarmak için komut **RepairExport'dur.**

## <a name="repairexport-parameters"></a>Repairİhracat parametreleri

**RepairExport**ile aşağıdaki parametreler belirtilebilir:  
  
|Parametre|Açıklama|  
|---------------|-----------------|  
|**/r:<RepairFile\>**|Gereklidir. Onarımın ilerlemesini izleyen ve kesintiye uğramış bir onarıma devam etmenizi sağlayan onarım dosyasına giden yol. Her sürücünün bir ve yalnızca bir onarım dosyası olmalıdır. Belirli bir sürücü için onarım başlattığınızda, henüz var olmayan bir onarım dosyasına giden yoldan geçersiniz. Kesintiye uğramış bir onarıma devam etmek için, varolan bir onarım dosyasının adına geçmeniz gerekir. Hedef sürücüye karşılık gelen onarım dosyası her zaman belirtilmelidir.|  
|**/logdir:<LogDirectory\>**|İsteğe bağlı. Günlük dizini. Verbose günlük dosyaları bu dizine yazılacaktır. Günlük dizini belirtilmemişse, geçerli dizini günlük dizini olarak kullanılır.|  
|**/d:<TargetDirectory\>**|Gereklidir. Doğrulamak ve onarmak için dizin. Bu genellikle dışa aktarma sürücüsünün kök dizinidir, ancak dışa aktarılan dosyaların bir kopyasını içeren bir ağ dosyası paylaşımı da olabilir.|  
|**/bk:<BitLockerKey\>**|İsteğe bağlı. Aracın dışa aktarılan dosyaların depolandığı şifreli bir dosyanın kilidini açmasını istiyorsanız BitLocker tuşunu belirtmelisiniz.|  
|**/sn:<StorageAccountAccountName\>**|Gereklidir. Dışa aktarma işi için depolama hesabının adı.|  
|**/sk:<StorageAccountKey\>**|**Bir** kapsayıcı SAS belirtilmemişse ve yalnızca gerekli. Dışa aktarma işinin depolama hesabının hesap anahtarı.|  
|**/csas:<KonteynerSas\>**|**Depolama** hesabı anahtarı belirtilmemişse ve yalnızca gerekliyse. Dışa aktarma işiyle ilişkili blobs erişmek için konteyner SAS.|  
|**/CopyLogFile:<DriveCopyLogFile\>**|Gereklidir. Sürücü kopyalama günlüğü dosyasına giden yol. Dosya, Windows Azure İçe Alma/Dışa Aktarma hizmeti tarafından oluşturulur ve işle ilişkili blob depolamasından indirilebilir. Kopyalama günlüğü dosyası, başarısız lekeler veya onarılması gereken dosyalar hakkında bilgi içerir.|  
|**/ManifestFile:<DriveManifestFile\>**|İsteğe bağlı. Dışa aktarma sürücüsünün bildirim dosyasına giden yol. Bu dosya, Windows Azure İçe Alma/Dışa Aktarma hizmeti tarafından oluşturulur ve dışa aktarma sürücüsünde ve isteğe bağlı olarak işle ilişkili depolama hesabında bir blob'da depolanır.<br /><br /> Dışa aktarma sürücüsündeki dosyaların içeriği, bu dosyada bulunan MD5 haşiyeleriyle doğrulanacaktır. Bozuk olduğu belirlenen tüm dosyalar indirilir ve hedef dizinlere yeniden yazılır.|  
  
## <a name="using-repairexport-mode-to-correct-failed-exports"></a>Başarısız dışa aktarmayı düzeltmek için RepairExport modunu kullanma  
Dışa aktarmayı başaramayan dosyaları indirmek için Azure İçe Aktarma/Verme Aracı'nı kullanabilirsiniz. Kopyalama günlüğü dosyası, dışa aktarmayı başaramayan dosyaların listesini içerir.  
  
Dışa aktarma hatalarının nedenleri arasında aşağıdaki olasılıklar yer almaktadır:  
  
-   Hasarlı sürücüler  
  
-   Depolama hesabı anahtarı aktarım işlemi sırasında değiştirildi  
  
Aracı **RepairExport** modunda çalıştırmak için öncelikle dışa aktarılan dosyaları içeren sürücüyü bilgisayarınıza bağlamanız gerekir. Ardından, parametreyle bu sürücüye giden yolu belirten Azure `/d` İçe Aktarma/Dışa Aktarma Aracı'nı çalıştırın. Ayrıca, karşıdan yüklediğiniz sürücünün kopya günlüğü dosyasına giden yolu da belirtmeniz gerekir. Aşağıdaki komut satırı örneği, dışa aktarmayı başaramayan dosyaları onarmak için aracı çalıştırur:  
  
```  
WAImportExport.exe RepairExport /r:C:\WAImportExport\9WM35C3U.rep /d:G:\ /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C3U.log  
```  
  
Aşağıda, blob'daki bir bloğun dışa aktarılamadığını gösteren bir kopya günlüğü dosyası örneği verilmiştir:  
  
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
  
Kopyalama günlüğü dosyası, Windows Azure İçe Alma/Dışa Aktarma hizmeti blob bloklarından birini dışa aktarma sürücüsündeki dosyaya indirirken bir hata oluştuğunu gösterir. Dosyanın diğer bileşenleri başarıyla karşıdan yüklendi ve dosya uzunluğu doğru şekilde ayarlandı. Bu durumda, araç sürücüdeki dosyayı açar, depolama hesabından bloğu indirir ve 65536 uzunluğunda ofset 65536'dan başlayarak dosya aralığına yazar.  
  
## <a name="using-repairexport-to-validate-drive-contents"></a>Sürücü içeriğini doğrulamak için RepairExport'u kullanma  
Sürücüdeki içeriğin doğru olduğunu doğrulamak için **RepairExport** seçeneğiyle Azure İçe Aktarma/Dışa Aktarma seçeneğini de kullanabilirsiniz. Her dışa aktarma sürücüsündeki bildirim dosyası, sürücünün içeriği için MD5'ler içerir.  
  
Azure İçe Alma/Dışa Aktarma hizmeti, dışa aktarma işlemi sırasında bildirim dosyalarını bir depolama hesabına da kaydedebilir. Bildirim dosyalarının konumu, iş tamamlandığında [İş Icabı Alma](/rest/api/storageimportexport/jobs) işlemi aracılığıyla kullanılabilir. Sürücü bildirimi dosyasının biçimi hakkında daha fazla bilgi için [İçe/Dışa](storage-import-export-file-format-metadata-and-properties.md) Aktar hizmeti Bildirimi Dosya Biçimi'ne bakın.  
  
Aşağıdaki örnekte, **/ManifestFile** ve **/CopyLogFile** parametreleri ile Azure İçe Alma/Dışa Aktarma Aracı'nın nasıl çalıştırılanılışı gösterilmektedir:  
  
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
  
Onarım işlemini tamamladıktan sonra, araç bildirim dosyasında başvurulan her dosyayı okur ve dosyanın bütünlüğünü MD5 işleriyle doğrular. Yukarıdaki bildirim için, aşağıdaki bileşenlerden geçecektir.  

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

Doğrulamada başarısız olan tüm bileşen araç tarafından indirilir ve sürücüdeki aynı dosyaya yeniden yazılır.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure İçe Alma/Dışa Aktarma Aracını Ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir içeri aktarma işini onarma](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
