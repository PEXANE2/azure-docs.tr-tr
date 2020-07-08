---
title: Azure Içeri/dışarı aktarma içeri aktarma işi onarılıyor-v1 | Microsoft Docs
description: Azure Içeri/dışarı aktarma hizmeti kullanılarak oluşturulan ve çalıştırılan bir içeri aktarma işini onarmayı öğrenin.
author: twooley
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 01/23/2017
ms.author: twooley
ms.subservice: common
ms.openlocfilehash: a5c0e9bf94a9953e107de148792af2e39f8bac24
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85512302"
---
# <a name="repairing-an-import-job"></a>Bir içeri aktarma işini onarma
Microsoft Azure İçeri/Dışarı Aktarma hizmeti, bazı dosyalarınızı veya bir dosyanın bazı bölümlerini Windows Azure Blob hizmetine kopyalayamayabilir. Hatalar şunlardan kaynaklanabilir:  
  
-   Bozuk dosyalar  
  
-   Hasarlı sürücüler  
  
-   Dosya aktarılırken depolama hesabı anahtarı değiştirildi.  
  
Microsoft Azure İçeri/Dışarı Aktarma aracını içeri aktarma işinin kopyalama günlüğü dosyalarıyla birlikte çalıştırabilir, aracın eksik dosyaları (veya dosya parçalarını) Windows Azure depolama hesabınıza yükleyerek içeri aktarma işini tamamlamasını sağlayabilirsiniz.  
  
## <a name="repairimport-parameters"></a>Repairımport parametreleri

Aşağıdaki parametreler **Repairımport**ile belirtilebilir: 
  
|||  
|-|-|  
|**/r:**<repairFile\>|**Gerekli.** Onarımın ilerlemesini izleyen ve kesilen bir onarımı sürdürmeye devam etmenizi sağlayan onarım dosyasının yolu. Her sürücünün bir ve yalnızca bir onarım dosyası olmalıdır. Belirli bir sürücü için onarım başlattığınızda, yolu henüz varolmayan bir onarım dosyasına geçirin. Kesilen bir onarımın sürdürülmesi için, var olan bir onarım dosyasının adını geçirmeniz gerekir. Hedef sürücüye karşılık gelen onarım dosyasının her zaman belirtilmesi gerekir.|  
|**/logdir:**<LogDirectory\>|**Seçim.** Günlük dizini. Ayrıntılı günlük dosyaları bu dizine yazılır. Günlük dizini belirtilmemişse, geçerli dizin günlük dizini olarak kullanılır.|  
|**/d:**<targetdizinler\>|**Gerekli.** İçeri aktarılan özgün dosyaları içeren bir veya daha fazla noktalı virgülle ayrılmış dizin. İçeri aktarma sürücüsü de kullanılabilir, ancak özgün dosyaların alternatif konumları varsa gerekli değildir.|  
|**/BK:**<BitLockerKey\>|**Seçim.** Aracın, özgün dosyaların kullanılabildiği şifreli bir sürücünün kilidini açmak istiyorsanız BitLocker anahtarını belirtmeniz gerekir.|  
|**/sn:**<storageAccountName\>|**Gerekli.** İçeri aktarma işinin depolama hesabının adı.|  
|**/SK:**<StorageAccountKey\>|Yalnızca bir kapsayıcı SAS belirtilmemişse **gereklidir** . İçeri aktarma işi için depolama hesabının hesap anahtarı.|  
|**/csas:**<containersas\>|Yalnızca depolama hesabı anahtarı belirtilmemişse **gereklidir** . İçeri aktarma işiyle ilişkili bloblara erişmek için kapsayıcı SAS.|  
|**/CopyLogFile:**<DriveCopyLogFile\>|**Gerekli.** Sürücü kopyalama günlük dosyasının yolu (ayrıntılı günlük veya hata günlüğü). Dosya, Windows Azure Içeri/dışarı aktarma hizmeti tarafından oluşturulur ve işle ilişkili blob depolamadan indirilebilir. Kopyalama günlük dosyası, başarısız Bloblar veya dosyalar hakkında onarılabilecek bilgiler içerir.|  
|**/Pathmapfile:**<DrivePathMapFile\>|**Seçim.** Aynı işte içeri aktardığınız aynı ada sahip birden fazla dosyanız varsa, belirsizlikleri çözümlemek için kullanılabilen bir metin dosyasının yolu. Araç ilk kez çalıştırıldığında, bu dosyayı tüm belirsiz adlarla doldurabilir. Aracın sonraki çalıştırmaları, belirsizlikleri çözümlemek için bu dosyayı kullanır.|  
  
## <a name="using-the-repairimport-command"></a>Repairımport komutunu kullanma  
Verileri ağ üzerinden akışa alarak içeri aktarma verilerini onarmak için, parametresini kullanarak içe aktardığınız özgün dosyaları içeren dizinleri belirtmeniz gerekir `/d` . Depolama hesabınızdan indirdiğiniz kopya günlük dosyasını da belirtmeniz gerekir. Kısmi hatalarla bir içeri aktarma işini onarmak için tipik bir komut satırı şöyle görünür:  
  
```  
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log  
```  
  
Aşağıdaki bir kopya günlük dosyası örneğinde, bir dosyanın 1 64-K parçası içeri aktarma işi için gönderilen sürücüdeki bozulmuş. Bu, belirtilen tek hata olduğundan, işteki Blobların geri kalanı başarıyla içeri aktarıldı.  
  
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
  
Bu kopyalama günlüğü Azure Içeri/dışarı aktarma aracına geçirildiğinde, araç eksik içeriği ağ üzerinden kopyalayarak bu dosyanın içeri aktarılmasını tamamlamayı dener. Yukarıdaki örnekte yer alan araç, bu `\animals\koala.jpg` iki dizin ve içindeki orijinal dosyayı arar `C:\Users\bob\Pictures` `X:\BobBackup\photos` . Dosya `C:\Users\bob\Pictures\animals\koala.jpg` varsa, Azure içeri/dışarı aktarma aracı eksik veri aralığını karşılık gelen bloba kopyalar `http://bobmediaaccount.blob.core.windows.net/pictures/animals/koala.jpg` .  
  
## <a name="resolving-conflicts-when-using-repairimport"></a>Repairımport kullanılırken çakışmalar çözümleniyor  
Bazı durumlarda, araç aşağıdaki nedenlerden biri için gerekli dosyayı bulamıyor veya açamıyor olabilir: dosya bulunamadı, dosya erişilebilir durumda değil, dosya adı belirsiz veya dosyanın içeriği artık doğru değil.  
  
Araç bulmaya çalışıyorsa `\animals\koala.jpg` ve hem hem de içinde bu ada sahip bir dosya varsa belirsiz bir hata oluşabilir `C:\Users\bob\pictures` `X:\BobBackup\photos` . Diğer bir deyişle, `C:\Users\bob\pictures\animals\koala.jpg` `X:\BobBackup\photos\animals\koala.jpg` içeri aktarma işi sürücülerinde her ikisi de mevcuttur.  
  
`/PathMapFile`Seçeneği bu hataları çözmenize olanak sağlar. Aracın doğru bir şekilde belirleyemediğinden dosya listesini içeren dosyanın adını belirtebilirsiniz. Aşağıdaki komut satırı örneği doldurulur `9WM35C2V_pathmap.txt` :  
  
```
WAImportExport.exe RepairImport /r:C:\WAImportExport\9WM35C2V.rep /d:C:\Users\bob\Pictures;X:\BobBackup\photos /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\WAImportExport\9WM35C2V.log /PathMapFile:C:\WAImportExport\9WM35C2V_pathmap.txt  
```
  
Araç daha sonra sorunlu dosya yollarını `9WM35C2V_pathmap.txt` her satıra bir olacak şekilde yazar. Örneğin, dosyası komutunu çalıştırdıktan sonra dosya aşağıdaki girişleri içerebilir:  
 
```
\animals\koala.jpg  
\animals\kangaroo.jpg  
```
  
 Listedeki her dosya için, aracın kullanılabilir olduğundan emin olmak için dosyayı bulmayı ve açmayı denemeniz gerekir. Araca bir dosyayı nerede bulacağınızı açıkça söylemek isterseniz, yol haritası dosyasını değiştirebilir ve yolu bir sekme karakteriyle ayırarak aynı satırdaki her bir dosyaya ekleyebilirsiniz:  
  
```
\animals\koala.jpg           C:\Users\bob\Pictures\animals\koala.jpg  
\animals\kangaroo.jpg        X:\BobBackup\photos\animals\kangaroo.jpg  
```
  
Gerekli dosyaları araç için kullanılabilir hale getirdikten veya yol haritası dosyasını güncelleştirdikten sonra, içeri aktarma işlemini gerçekleştirmek için aracı yeniden çalıştırabilirsiniz.  
  
## <a name="next-steps"></a>Sonraki adımlar
 
* [Azure Içeri/dışarı aktarma aracı 'nı ayarlama](storage-import-export-tool-setup-v1.md)   
* [Sabit sürücüleri içeri aktarma işine hazırlama](../storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Kopyalama günlük dosyalarıyla iş durumunu gözden geçirme](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Bir dışarı aktarma işini onarma](../storage-import-export-tool-repairing-an-export-job-v1.md)   
* [Azure İçeri/Dışarı Aktarma Aracı ile ilgili sorunları giderme](storage-import-export-tool-troubleshooting-v1.md)
