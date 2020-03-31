---
title: Azure İçe Aktarma/Dışa Aktarma işini onarma - v1 | Microsoft Dokümanlar
description: Azure İçe Alma/Dışa Aktarma hizmetini kullanarak oluşturulan ve çalıştırılabilen bir alma işini nasıl onarın.
author: twooley
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: f5db321d8c4a6e42591a82b0ed8eb6bc6e93bad4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74973892"
---
# <a name="repairing-an-import-job"></a>Bir içeri aktarma işini onarma
Microsoft Azure İçe Alma/Dışa Aktarma hizmeti, dosyanızın veya bir dosyanın bazı bölümlerinin Windows Azure Blob hizmetine kopyalanmasında başarısız olabilir. Hataların bazı nedenleri şunlardır:  
  
-   Bozuk dosyalar  
  
-   Hasarlı sürücüler  
  
-   Dosya aktarılırken depolama hesabı anahtarı değiştirildi.  
  
Microsoft Azure İçe Alma/Dışa Aktarma Aracı'nı içe aktarma işinin kopyalama günlüğü dosyalarıyla çalıştırabilirsiniz ve araç içe aktarma işini tamamlamak için eksik dosyaları (veya bir dosyanın parçalarını) Windows Azure depolama hesabınıza yükler.  
  
## <a name="repairimport-parameters"></a>Repairİthalat parametreleri

Aşağıdaki parametreler **RepairImport**ile belirtilebilir: 
  
|||  
|-|-|  
|**/r:**<RepairFile\>|**Gerekli.** Onarımın ilerlemesini izleyen ve kesintiye uğramış bir onarıma devam etmenizi sağlayan onarım dosyasına giden yol. Her sürücünün bir ve yalnızca bir onarım dosyası olmalıdır. Belirli bir sürücü için onarım başlattığınızda, henüz var olmayan bir onarım dosyasına giden yolda geçirin. Kesintiye uğramış bir onarıma devam etmek için, varolan bir onarım dosyasının adına geçmeniz gerekir. Hedef sürücüye karşılık gelen onarım dosyası her zaman belirtilmelidir.|  
|**/logdir:**<LogDirectory\>|**Isteğe bağlı.** Günlük dizini. Verbose günlük dosyaları bu dizine yazılır. Günlük dizini belirtilmemişse, geçerli dizin günlük dizini olarak kullanılır.|  
|**/d:**<TargetDirectories\>|**Gerekli.** Alınan özgün dosyaları içeren bir veya daha fazla yarı sütunayrılmış dizin. Alma sürücüsü de kullanılabilir, ancak özgün dosyaların alternatif konumları varsa gerekli değildir.|  
|**/bk:**<BitLockerKey\>|**Isteğe bağlı.** Aracın orijinal dosyaların bulunduğu şifreli bir sürücünün kilidini açmasını istiyorsanız BitLocker tuşunu belirtmelisiniz.|  
|**/sn:**<StorageAccountName\>|**Gerekli.** Alma işi için depolama hesabının adı.|  
|**/sk:**<StorageAccountKey\>|**Bir** kapsayıcı SAS belirtilmemişse ve yalnızca gerekli. Alma işinin depolama hesabının hesap anahtarı.|  
|**/csas:**<ContainerSas\>|**Depolama** hesabı anahtarı belirtilmemişse ve yalnızca gerekliyse. Alma işiyle ilişkili blobs erişmek için konteyner SAS.|  
|**/CopyLogFile:**<DriveCopyLogFile\>|**Gerekli.** Sürücü kopyalama günlüğü dosyasına giden yol (ayrıntılı günlük veya hata günlüğü). Dosya, Windows Azure İçe Alma/Dışa Aktarma hizmeti tarafından oluşturulur ve işle ilişkili blob depolamasından indirilebilir. Kopyalama günlüğü dosyası, onarılması gereken başarısız lekeler veya dosyalar hakkında bilgiler içerir.|  
|**/PathMapFile:**<DrivePathMapFile\>|**Isteğe bağlı.** Aynı işte içe aktardığınız aynı ada sahip birden çok dosyanız varsa, belirsizlikleri gidermek için kullanılabilecek bir metin dosyasına giden yol. Araç ilk çalıştırılda, bu dosyayı tüm belirsiz adlarla doldurabilir. Aracın sonraki çalıştırmaları belirsizlikleri gidermek için bu dosyayı kullanır.|  
  
## <a name="using-the-repairimport-command"></a>RepairImport komutunu kullanma  
Verileri ağ üzerinden aktararak alma verilerini onarmak için, `/d` parametreyi kullanarak içe aktardığınız özgün dosyaları içeren dizinleri belirtmeniz gerekir. Ayrıca, depolama hesabınızdan indirdiğiniz kopyalama günlüğü dosyasını da belirtmeniz gerekir. Kısmi hatalarla bir alma işini onarmak için tipik bir komut satırı gibi görünür:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
Bir kopyalama günlüğü dosyasının aşağıdaki örneğinde, bir dosyanın bir 64-K parçası alma işi için sevk edilen sürücüde bozuktu. Bu belirtilen tek başarısızlık olduğundan, iş lekeleri geri kalanı başarıyla ithal edildi.  
  
```xml
<?xml version="1.0" encoding="utf-8"?>  
<DriveLog>  
 <DriveId>9WM35C2V</DriveId>  
 <Blob Status="CompletedWithErrors">  
 <BlobPath>pictures/animals/koala.jpg</BlobPath>  
 <FilePath>\animals\koala.jpg</FilePath>  
 <Length>163840</Length>  
 <ImportDisposition Status="Overwritten">overwrite</ImportDisposition>  
 <PageRangeList>  
  <PageRange Offset="65536" Length="65536" Hash="AA2585F6F6FD01C4AD4256E018240CD4" Status="Corrupted" />  
 </PageRangeList>  
 </Blob>  
 <Status>CompletedWithErrors</Status>  
</DriveLog>  
```
  
Bu kopyalama günlüğü Azure İçe Alma/Dışa Aktarma Aracına geçirildiğinde, araç eksik içerikleri ağ üzerinden kopyalayarak bu dosyanın içe aktarmasını tamamlamaya çalışır. Yukarıdaki örneği izleyerek, araç iki `\animals\koala.jpg` dizin `C:\Users\bob\Pictures` içinde özgün `X:\BobBackup\photos`dosyayı arar ve . Dosya `C:\Users\bob\Pictures\animals\koala.jpg` varsa, Azure İçe Alma/Dışa Aktarma Aracı eksik veri `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg`aralığını ilgili blob'a kopyalar.  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>RepairImport kullanırken çakışmaları çözme  
Bazı durumlarda, araç aşağıdaki nedenlerden biri için gerekli dosyayı bulamayabilir veya açamayabilir: dosya bulunamadı, dosyaerişilebilir değil, dosya adı belirsiz veya dosyanın içeriği artık doğru değil.  
  
Araç bulmaya `\animals\koala.jpg` çalışıyorsa ve her iki `C:\Users\bob\pictures` ve `X:\BobBackup\photos`altında bu ada sahip bir dosya varsa belirsiz bir hata oluşabilir. Yani, hem `C:\Users\bob\pictures\animals\koala.jpg` `X:\BobBackup\photos\animals\koala.jpg` de ve alma iş sürücüleri var.  
  
Bu `/PathMapFile` seçenek, bu hataları çözmenizi sağlar. Aracın doğru şekilde tanımlayamayacağı dosyaların listesini içeren dosyanın adını belirtebilirsiniz. Aşağıdaki komut satırı örneği `9WM35C2V_pathmap.txt`çoğalır:  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
Araç daha sonra sorunlu dosya `9WM35C2V_pathmap.txt`yollarını her satıra bir e, bir e yazar. Örneğin, dosya komutu çalıştırdıktan sonra aşağıdaki girişleri içerebilir:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Listedeki her dosya için, aracın kullanılabilir olduğundan emin olmak için dosyayı bulup açmayı denemelisiniz. Araca bir dosyayı nerede bulacağınızı açıkça söylemek isterseniz, yol haritası dosyasını değiştirebilir ve bir sekme karakteriyle ayrılmış aynı satırdaki her dosyaya yol ekleyebilirsiniz:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Gerekli dosyaları araç için kullanılabilir hale getirdikten veya yol eşleme dosyasını güncelledikten sonra, alma işlemini tamamlamak için aracı yeniden çalıştırabilirsiniz.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure İçe Alma/Dışa Aktarma Aracını Ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
