---
title: Azure 'da ortak VM hata kodları | Microsoft Docs
description: Azure 'da sanal makineler sağlarken ve yönetirken karşılaşılan bazı yaygın hata kodlarından bazılarını anlayın
services: virtual-machines
documentationcenter: ''
author: xujing-ms
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure
ms.date: 5/22/2017
ms.author: xujing
ms.openlocfilehash: 52508a6820ce0cbbbe3a0341a99894f8b92b1645
ms.sourcegitcommit: 2ff0d073607bc746ffc638a84bb026d1705e543e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87831235"
---
# <a name="understand-common-error-messages-when-you-manage-virtual-machines-in-azure"></a>Azure'da sanal makineleri yönetirken sık karşılaşılan hata iletilerini anlama

Bu makalede, Azure 'da sanal makineler (VM) oluştururken veya yönetirken karşılaşabileceğiniz en yaygın hata kodlarından ve iletilerden bazıları açıklanmaktadır.

>[!NOTE]
> Bu sayfada geri bildirimde bulunmak veya #azerrormessage etiketiyle [Azure geri bildirimleri](https://feedback.azure.com/forums/216843-virtual-machines) aracılığıyla yorumlarınızı bırakabilirsiniz.

## <a name="error-response-format"></a>Hata yanıtı biçimi 
Azure VM 'Leri hata yanıtı için aşağıdaki JSON biçimini kullanır:

```json
{
  "status": "status code",
  "error": {
    "code":"Top level error code",
    "message":"Top level error message",
    "details":[
     {
      "code":"Inner level error code",
      "message":"Inner level error message"
     }
    ]
   }
}
```

Bir hata yanıtı her zaman bir durum kodu ve bir hata nesnesi içerir. Her hata nesnesi her zaman bir hata kodu ve ileti içerir. VM bir şablonla oluşturulduysa, hata nesnesi bir iç hata kodları ve ileti düzeyi içeren bir ayrıntılar bölümü de içerir. Normalde, hata iletisinin en iç düzeyi kök hatadır.


## <a name="common-virtual-machine-management-errors"></a>Ortak sanal makine yönetimi hataları

Bu bölümde, VM 'Leri yönetirken karşılaşabileceğiniz yaygın hata iletileri listelenmektedir:

|  Hata Kodu  |  Hata İletisi  |  
|  :------| :-------------|  
|  AcquireDiskLeaseFailed  |  URI ile blob kullanılarak ' ' diski oluşturulurken kira elde edilemedi {0} {1} . Blob zaten kullanımda.  |  
|  AllocationFailed  |  Ayırma başarısız oldu. Lütfen VM boyutunu veya VM sayısını azaltmayı deneyin, daha sonra yeniden deneyin veya farklı bir kullanılabilirlik kümesine veya farklı Azure konumuna dağıtım yapmayı deneyin.  |  
|  AllocationFailed  |  VM ayırması bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin veya farklı bir konuma dağıtımı deneyin.  |
|  ArtifactNotFound  |  Yayımcı ' ' {0} ve türü ' ' olan VM Uzantısı {1} ' ' konumunda bulunamadı {2} .  |
|  ArtifactNotFound  |  Uzantı deposunda Yayımcı ' {0} ', tür ' {1} ' ve tür işleyicisi sürümü ' ' olan uzantı {2} bulunamadı.  |
|  ArtifactVersionNotFound  |  Yapıt deposunda, istenen ' ' sürümünü karşılayan bir sürüm bulunamadı {0} .  |
|  ArtifactVersionNotFound  |  Yapıt deposunda, {0} Yayımcı ' {1} ' ve türü ' ' olan VM uzantısı için istenen ' ' sürümünü karşılayan bir sürüm bulunamadı {2} .  |
|  Attachdiskwhilebeingayrıldı  |  {0} {1} Disk şu anda ayrılmakta olduğundan ' ' sanal makinesine ' ' veri diski iliştirilemiyor. Lütfen disk tamamen ayrılana kadar bekleyip yeniden deneyin.  |
|  Işlemindeki hatalı istek  |  Hizalanmış ' kullanılabilirlik kümeleri bu bölgede henüz desteklenmiyor.  |
|  Işlemindeki hatalı istek  |  Yönetilmeyen kullanılabilirlik kümesine veya blob tabanlı disklere sahip VM 'nin yönetilen kullanılabilirlik kümesine eklenmesi için yönetilen disklere sahip bir VM 'nin eklenmesi desteklenmez. Yönetilen disklere sahip bir VM eklemek için lütfen ' Managed ' özelliği ayarlanmış bir kullanılabilirlik kümesi oluşturun.  |
|  Işlemindeki hatalı istek  |  Yönetilen diskler bu bölgede desteklenmez.  |
|  Işlemindeki hatalı istek  |  ' ' İşletim sistemi türü için işleyici başına birden çok VMExtensions desteklenmiyor {0} . ' ' İşleyicisiyle ' ' adlı Vmexgerilim, {1} {2} girişte zaten eklenmiş veya belirtilmiş.  |
|  Işlemindeki hatalı istek  |  ' {0} ' İşlemi, {1} yönetilen disklerle ' ' kaynağında desteklenmez.  |
|  Certificateımproperlybiçimlendirildi  |  Parolanın öğesinden alınan JSON gösterimi, {0} düzgün şekilde biçimlendirilen BIR pfx dosyası olmayan bir veri alanına sahip veya belirtilen parola PFX dosyasının doğru şekilde şifresini çözemedi.  |
|  Certificateımproperlybiçimlendirildi  |  Öğesinden alınan veriler {0} JSON 'a seri hale getirilemez.  |
|  Çakışma  |  Yalnızca bir VM oluşturulurken veya VM serbest bırakıldığında disk yeniden boyutlandırılmasına izin verilir.  |
|  Conflictinguserınput  |  {0}Disk, ' ' sanal makinesine ait olduğundan ' ' diski iliştirilemiyor {1} .  |
|  Conflictinguserınput  |  Kaynak ve hedef kaynak grupları aynıdır.  |
|  Conflictinguserınput  |  Disk için kaynak ve hedef depolama hesapları {0} farklı.  |
|  ContainerAlreadyOnLease  |  Blob 'u URI ile tutan depolama kapsayıcısı üzerinde zaten bir kira var {0} .  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Kaynakları taşıma isteği, istekteki bir veya daha fazla tarafından başvurulan Anahtar Kasası kaynaklarını içerir {0} . Bu, şu anda çapraz abonelik taşıması içinde desteklenmez. Anahtar Kasası kaynak kimlikleri için lütfen hata ayrıntılarını kontrol edin.  |
|  Diagnosticsoperationınternalerror  |  VM 'nin tanılama profili işlenirken bir iç hata oluştu {0} .  |
|  Diskblobalreadınusebyanotherdisk  |  Blob {0} , ' ' sanal makinesine ait başka bir disk tarafından zaten kullanılıyor {1} . Disk başvuru bilgileri için blob meta verilerini inceleyebilirsiniz.  |
|  DiskBlobNotFound  |  {0}' ' Diski IÇIN URI Ile VHD blobu bulunamıyor {1} .  |
|  DiskBlobNotFound  |  URI ile VHD blobu bulunamıyor {0} .  |
|  DiskEncryptionKeySecretMissingTags  |  {0}gizli dizi etiketleri yok {1} . Lütfen gizli sürümü güncelleştirin, gerekli etiketleri ekleyin ve yeniden deneyin.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  {0}Anahtar kullanılarak parola değerinin sarmalaması geri {1} alınamadı.  |
|  DiskImageNotReady  |  Disk görüntüsü {0} {1} durumunda. Görüntü hazırlanıyor, lütfen yeniden deneyin.  |
|  DiskPreparationError  |  VM diskleri hazırlanırken bir veya daha fazla hata oluştu. Ayrıntılar için bkz. disk örneği görünümü.  |
|  DiskProcessingError  |  VM, başarısız disklerde başka diskler içerdiğinden disk işleme durduruldu.  |
|  Imageblobnotfound  |  {0}' ' Diski IÇIN URI Ile VHD blobu bulunamıyor {1} .  |
|  Imageblobnotfound  |  URI ile VHD blobu bulunamıyor {0} .  |
|  IncorrectDiskBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. {0}' ' Diski için blob {1} , Blok Blobu türünde.  |
|  IncorrectDiskBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. Blob {0} ' {1} ' türünde.  |
|  IncorrectImageBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. {0}' ' Diski için blob {1} , Blok Blobu türünde.  |
|  IncorrectImageBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. Blob {0} ' {1} ' türünde.  |
|  Internaloperationerror  |  Depolama hesabı çözümlenemedi {0} . Lütfen işlem kaynağıyla aynı konumdaki depolama kaynak sağlayıcısı üzerinden oluşturulduğundan emin olun.  |
|  Internaloperationerror  |  {0}hedef arama görevi başarısız oldu.  |
|  Internaloperationerror  |  ' ' Sanal makinesinin ağ profili doğrulanırken hata oluştu {0} .  |
|  Invalidaccounttype  |  AccountType {0} geçersiz.  |
|  Geçersiz parametre  |  Parametrenin değeri {0} geçersiz.  |
|  Geçersiz parametre  |  Belirtilen yönetici parolasına izin verilmiyor.  |
|  Geçersiz parametre  |  "Sağlanan parola {0} {1} karakter uzunluğunda olmalıdır ve şunlardan en az {2} parola karmaşıklığı gereksinimlerinden oluşmalıdır: <ol><li> Büyük harfli bir karakter içerir</li><li>Küçük harfli bir karakter içerir</li><li>Sayısal bir basamak içerir</li><li>Özel bir karakter içerir.</li></ol>  |
|  Geçersiz parametre  |  Belirtilen yönetici kullanıcı adına izin verilmiyor.  |
|  Geçersiz parametre  |  VM bir platform veya Kullanıcı görüntüsünden oluşturulduysa, var olan bir işletim sistemi diski iliştirilemez.  |
|  Geçersiz parametre  |  Kapsayıcı adı {0} geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalı ve yalnızca küçük harfli alfasayısal karakterler ve kısa çizgi içerebilir. Kısa çizgi öncesinde ve sonra alfasayısal bir karakter gelmelidir.  |
|  Geçersiz parametre  |  {0}URL 'de kapsayıcı adı {1} geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalı ve yalnızca küçük harfli alfasayısal karakterler ve kısa çizgi içerebilir. Kısa çizgi öncesinde ve sonra alfasayısal bir karakter gelmelidir.  |
|  Geçersiz parametre  |  URL 'deki blob adı {0} eğik çizgi içeriyor. Bu, şu anda diskler için desteklenmez.  |
|  Geçersiz parametre  |  URI {0} doğru blob URI 'si olarak görünmüyor.  |
|  Geçersiz parametre  |  ' ' Adlı bir disk {0} zaten aynı LUN 'u kullanıyor: {1} .  |
|  Geçersiz parametre  |  ' ' Adlı bir disk {0} zaten var.  |
|  Geçersiz parametre  |  Belirtilen görüntü başvurusunda zaten tanımlı olan bir disk için Kullanıcı görüntüsü geçersiz kılmaları belirtilemez.  |
|  Geçersiz parametre  |  ' ' Adlı bir disk {0} zaten aynı VHD URL 'sini kullanıyor {1} .  |
|  Geçersiz parametre  |  Belirtilen hata etki alanı sayısı {0} , aralığında olmalıdır {1} {2} .  |
|  Geçersiz parametre  |  Lisans türü {0} geçersiz. Geçerli lisans türleri şunlardır: Windows_Client veya Windows_Server, büyük/küçük harfe duyarlı.  |
|  Geçersiz parametre  |  Linux ana bilgisayar adı {0} karakter uzunluğunu aşamaz veya şu karakterleri içeremez: {1} .  |
|  Geçersiz parametre  |  {0}Linux sağlama aracısındaki bilinen bir sorun nedeniyle SSH ortak anahtarları için hedef yol şu anda varsayılan değeriyle sınırlıdır.  |
|  Geçersiz parametre  |  LUN 'daki bir disk {0} zaten var.  |
|  Geçersiz parametre  |  İsteğin aboneliği, {0} {1} yönetilen disk kimliğinde bulunan abonelikle aynı olmalıdır.  |
|  Geçersiz parametre  |  OSProfile içindeki özel verilerin base64 kodlamalı ve en fazla karakter uzunluğunda olması gerekir {0} .  |
|  Geçersiz parametre  |  URL 'deki blob adı {0} ' {1} ' uzantısıyla bitmelidir.  |
|  Geçersiz parametre  |  {0}', geçerli bir yakalanan VHD blob adı ön eki değil. Geçerli bir önek, Regex ' {1} ' ile eşleşiyor.  |
|  Geçersiz parametre  |  VM Aracısı sağlanmadıysa sanal makinenize sertifikalar eklenemiyor.  |
|  Geçersiz parametre  |  LUN 'daki bir disk {0} zaten var.  |
|  Geçersiz parametre  |  İstenen boyut, {0} kullanılabilirlik kümesinin Şu anda ayrıldığı kümede KULLANıLAMADıĞıNDAN VM oluşturulamıyor. Kullanılabilir Boyutlar: {1} . Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin https://aka.ms/azure-resizevm .  |
|  Geçersiz parametre  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Geçerli bölgede bulunan Boyutlar: {1} . Konumundaki her bir bölgedeki kullanılabilir VM boyutları hakkında daha fazla bilgi edinin https://aka.ms/azure-regions .  |
|  Geçersiz parametre  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Konumundaki her bir bölgedeki kullanılabilir VM boyutları hakkında daha fazla bilgi edinin https://aka.ms/azure-regions .  |
|  Geçersiz parametre  |  Windows Yönetici Kullanıcı adı {0} karakterden uzun olamaz, nokta (.) ile bitemez veya şu karakterleri içeremez: {1} .  |
|  Geçersiz parametre  |  Windows bilgisayar adı {0} karakterden uzun olamaz, tamamen sayısal olamaz veya şu karakterleri içeremez: {1} .  |
|  Missingmovedependentkaynakları  |  Kaynakları taşıma isteği tüm bağımlı kaynakları içermez. Lütfen eksik kaynak kimlikleri için hata ayrıntılarını denetleyin.  |
|  Moveresourceshaveınvalidstate  |  Kaynakları taşıma isteği, geçersiz depolama hesaplarıyla ilişkili VM 'Leri içerir. Lütfen bu kaynak kimliklerinin ayrıntılarını ve başvurulan depolama hesabı adlarını denetleyin.  |
|  MoveResourcesHavePendingOperations  |  Kaynakları taşıma isteği, bir işlemin beklediği kaynakları içerir. Lütfen bu kaynak kimliklerinin ayrıntılarını kontrol edin. Bekleyen işlemler tamamlandıktan sonra işleminizi yeniden deneyin.  |
|  MoveResourcesNotFound  |  Kaynakları taşıma isteği bulunamayan kaynakları içerir. Lütfen bu kaynak kimliklerinin ayrıntılarını kontrol edin.  |
|  Networkingınternaloperationerror  |  Bilinmeyen ağ ayırma hatası.  |
|  Networkingınternaloperationerror  |  Bilinmeyen ağ ayırma hatası  |
|  Networkingınternaloperationerror  |  VM 'nin ağ profilini işlerken bir iç hata oluştu.  |
|  NotFound  |  Kullanılabilirlik kümesi {0} bulunamıyor.  |
|  NotFound  |  İstekte belirtilen ' ' kaynak sanal makinesi {0} Bu Azure konumunda yok.  |
|  NotFound  |  Kimliği olan kiracı {0} bulunamadı.  |
|  NotFound  |  Görüntü {0} bulunamıyor.  |
|  NotSupported  |  Lisans türü {0} , ancak görüntü blobu {1} Şirket içinden değildir.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi {0} silinemiyor. Bir kullanılabilirlik kümesini silmeden önce lütfen herhangi bir VM içermediğinden emin olun.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi SKU 'SU ' hizalı ' değerinden ' klasik ' öğesine değiştirilmesine izin verilmez.  |
|  OperationNotAllowed  |  VM çalışmadığı zaman VM 'deki uzantılar değiştirilemez.  |
|  OperationNotAllowed  |  Yakalama eylemi yalnızca blob tabanlı disklere sahip bir sanal makinede desteklenir. Yönetilen bir sanal makineden bir görüntü oluşturmak için lütfen ' görüntü ' kaynak API 'Lerini kullanın.  |
|  OperationNotAllowed  |  {0} {1} Görüntü başarıyla oluşturuluncaya kadar görüntüden kaynak oluşturulamıyor.  |
|  OperationNotAllowed  |  VM ayrıldığında encryptionSettings güncelleştirmelerine izin verilmiyor, lütfen VM serbest bırakıldıktan sonra yeniden deneyin  |
|  OperationNotAllowed  |  Blob tabanlı diskler içeren bir VM 'ye yönetilen disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  Bu boyuttaki bir VM 'ye bağlı olmasına izin verilen en fazla veri diski sayısı {0} .  |
|  OperationNotAllowed  |  Yönetilen disklerle VM 'ye blob tabanlı bir disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  {0}Görüntü silinmek üzere işaretlendiğinden ' ' görüntüsünde ' ' işlemine izin verilmiyor {1} . Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  VM Genelleştirilmiş olduğundan ' ' {0} sanal makinesinde ' ' işlemine izin verilmiyor {1} .  |
|  OperationNotAllowed  |  ' {0} ' Geri yükleme noktası koleksiyonu {1} silinmek üzere işaretlendiğinden ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  ' ' İşlemi {0} {1} silinmek üzere işaretlendiğinden ' ' VM uzantısında izin verilmiyor. Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  ' ' {0} Sanal makineleri ' ' {1} görüntüsü kullanılarak sağlanmakta olduğundan ' ' işlemine izin verilmiyor {2} .  |
|  OperationNotAllowed  |  ' ' {0} Sanal makine ölçek kümesi {1} Şu anda ' ' görüntüsünü kullandığından ' ' işlemine izin verilmiyor {2} .  |
|  OperationNotAllowed  |  {0}VM silinmek üzere işaretlendiğinden ' ' sanal makinesinde ' ' işlemine izin verilmiyor {1} . Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  {0} {1} VM serbest bırakılmış veya serbest bırakılmış olarak işaretlenmiş olduğundan ' ' sanal makinesinde ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  VM çalıştığından, ' ' {0} sanal makinesinde ' ' işlemine izin verilmiyor {1} . Lütfen VM 'yi konuk işletim sisteminin içinden kapattığınız durumda açık bir şekilde kapatın.  |
|  OperationNotAllowed  |  {0}VM serbest bırakılmadığı için ' ' sanal makinesinde ' ' işlemine izin verilmiyor {1} .  |
|  OperationNotAllowed  |  VM 'nin {0} {1} ' ' uzantısı başarısız durumunda olduğundan ' ' sanal makinesinde ' ' işlemine izin verilmiyor {2} .  |
|  OperationNotAllowed  |  {0} {1} Devam eden başka bir işlem olduğundan ' ' sanal makinesinde ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  ' ' İşlemi, {0} ' ' sanal makinesinin {1} genelleştirilmesi gerektiriyor.  |
|  OperationNotAllowed  |  İşlem için VM 'nin çalışıyor olması gerekir (veya çalıştırılacak şekilde ayarlanır).  |
|  OperationNotAllowed  |  {0}Görüntüde karşılık gelen disk boyutundan daha küçük olan boyut GB olan diske {1} izin verilmez.  |
|  OperationNotAllowed  |  ' ' İşleyicisinin VM Ölçek kümesi uzantıları {0} yalnızca VM Ölçek kümesi oluşturma sırasında eklenebilir.  |
|  OperationNotAllowed  |  ' ' İşleyicisinin VM Ölçek kümesi uzantıları {0} yalnızca VM Ölçek kümesi silme sırasında silinebilir.  |
|  OperationNotAllowed  |  VM ' {0} ' zaten yönetilen diskler kullanıyor.  |
|  OperationNotAllowed  |  ' ' Sanal makinesi ' {0} ' klasik ' kullanılabilirlik kümesine aittir {1} . Lütfen kullanılabilirlik kümesini ' hizalı ' SKU 'SU kullanacak şekilde güncelleştirin ve sonra dönüştürmeyi yeniden deneyin.  |
|  OperationNotAllowed  |  Görüntüden oluşturulan VM 'de blob tabanlı diskler olamaz. Tüm disklerin yönetilen diskler olması gerekir.  |
|  OperationNotAllowed  |  VM Genelleştirilmiş olmadığından yakalama işlemi tamamlanamıyor.  |
|  OperationNotAllowed  |  {0}VM diskleri yönetilen disklere dönüştürülemediğinden ' ' sanal makinesinde yönetim işlemlerine izin verilmiyor.  |
|  OperationNotAllowed  |  Devam eden bir işlem, sanal makinenin güç durumunu ' {0} e değiştiriyor {1} . Lütfen {2} bir süre sonra işlemi gerçekleştirin.  |
|  OperationNotAllowed  |  VM eklenemiyor veya güncelleştirilemiyor. İstenen VM boyutu {0} mevcut ayırma biriminde bulunmayabilir. Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin https://aka.ms/azure-resizevm .  |
|  OperationNotAllowed  |  İstenen boyut, {0} kullanılabilirlik kümesinin Şu anda ayrıldığı kümede KULLANıLAMADıĞıNDAN VM yeniden boyutlandırılamıyor. Kullanılabilir Boyutlar: {1} . Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin https://aka.ms/azure-resizevm .  |
|  OperationNotAllowed  |  İstenen boyut {0} VM 'nin şu anda ayrıldığı kümede KULLANıLAMADıĞıNDAN VM yeniden boyutlandırılamıyor. VM 'nizi yeniden boyutlandırmak için {1} (bu Azure Portal durdurma işlemi) ve yeniden boyutlandırma işlemini yeniden deneyin. Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin https://aka.ms/azure-resizevm .  |
|  OSProvisioningClientError  |  {0}Konuk işletim sistemi şu anda sağlanmakta olduğundan, ' ' sanal makinesi için işletim sistemi sağlama başarısız oldu.  |
|  OSProvisioningClientError  |  ' ' Sanal makinesi için işletim sistemi sağlama {0} başarısız oldu. Hata ayrıntıları: {1} görüntünün düzgün hazırlanmış (Genelleştirilmiş) olduğundan emin olun. <ul><li>Windows için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  SSH ana bilgisayar anahtarı oluşturulamadı. Hata ayrıntıları: {0} . Bu sorunu gidermek için, Linux aracısının düzgün ayarlanıp ayarlanmadığını doğrulayın. <ul><li>Yönergeleri şu adreste bulabilirsiniz:https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux/ </li></ul> |
|  OSProvisioningClientError  |  VM için belirtilen Kullanıcı adı bu Linux dağıtımı için geçersiz. Hata ayrıntıları: {0} .  |
|  Osprovisioningınternalerror  |  Bir iç hata nedeniyle VM ' ' için işletim sistemi sağlama başarısız oldu {0} .  |
|  Osprovisioningtime,  |  ' ' Sanal makinesi için işletim sistemi sağlama {0} , ayrılan sürede tamamlanmadı. VM sağlamayı başarıyla bitirirebilir. Lütfen sağlama durumunu daha sonra denetleyin.  |
|  Osprovisioningtime,  |  ' ' Sanal makinesi için işletim sistemi sağlama {0} , ayrılan sürede tamamlanmadı. VM sağlamayı başarıyla bitirirebilir. Lütfen sağlama durumunu daha sonra denetleyin. Ayrıca, görüntünün düzgün hazırlanmış (Genelleştirilmiş) olduğundan emin olun.   <ul><li>[Windows]( ../windows/upload-generalized-managed.md)için yönergeler.</li><li> [Linux](../linux/capture-image.md) için yönergeler</li></ul>  |
|  Osprovisioningtime,  |  ' ' Sanal makinesi için işletim sistemi sağlama {0} , ayrılan sürede tamamlanmadı. Ancak, VM Konuk Aracısı çalışıyor olarak algılandı. Bu, Konuk işletim sisteminin bir VM görüntüsü (CreateOption = FromImage) olarak kullanılmak üzere düzgün şekilde hazırlandığını önerir. Bu sorunu çözmek için, VHD 'yi CreateOption = Attach ile birlikte kullanın veya görüntü olarak kullanmak üzere doğru şekilde hazırlayın:   <ul><li>Windows için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Gerekli VM boyutu şu anda seçili konumda kullanılamıyor.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Devam eden platform güncelleştirmesi nedeniyle kaynak şu anda güncelleştirilemiyor. Lütfen daha sonra yeniden deneyin.  |
|  Storageaccountsınırlamanın  |  ' ' Depolama hesabı, {0} disk oluşturmak için gerekli olan sayfa bloblarını desteklemiyor.  |
|  Storageaccountsınırlamanın  |  ' ' Depolama hesabı {0} , ayrılmış kotasını aştı.  |
|  Storageaccountlocationuyuşmazlığıdır  |  Depolama hesabı çözümlenemedi {0} . Lütfen işlem kaynağıyla aynı konumdaki depolama kaynak sağlayıcısı üzerinden oluşturulduğundan emin olun.  |
|  StorageAccountNotFound  |  Depolama hesabı {0} bulunamadı. Depolama hesabının silinmediğinden ve VM ile aynı Azure konumuna ait olduğundan emin olun.  |
|  Storageaccountnottanınmış  |  Lütfen depolama kaynak sağlayıcısı tarafından yönetilen bir depolama hesabı kullanın. Kullanılması {0} desteklenmez.  |
|  Storageaccountoperationınternalerror  |  Depolama hesabına erişilirken iç hata oluştu {0} .  |
|  Storageaccountsubscriptionuyuşmazlığı  |  Depolama hesabı {0} aboneliğe ait değil {1} .  |
|  StorageAccountTooBusy  |  ' ' Depolama hesabı {0} Şu anda çok meşgul. Başka bir hesap kullanmayı düşünün.  |
|  StorageAccountTypeNotSupported  |  {0} {1} BLOB depolama hesabı olan disk kullanılır. Lütfen genel amaçlı depolama hesabı ile yeniden deneyin.  |
|  StorageAccountTypeNotSupported  |  Depolama hesabı {0} {1} türündedir. Önyükleme tanılaması {2} depolama hesabı türlerini destekler.  <ul><li>Bu hata, önyükleme tanılaması için Premium depolama hesabı kullanırsanız oluşur. Daha fazla bilgi için bkz. [önyükleme tanılamayı kullanma](boot-diagnostics.md). </li></ul> |
|  SubscriptionNotAuthorizedForImage  |  Abonelik yetkili değil.  |
|  TargetDiskBlobAlreadyExists var  |  Blob {0} zaten var. Lütfen yeni boş bir ' ' veri diski oluşturmak için farklı bir blob URI 'SI sağlayın {1} .  |
|  TargetDiskBlobAlreadyExists var  |  Hedef görüntü blobu {0} zaten varolduğundan ve VHD bloblarının üzerine yazılacak bayrak ayarlanmadığından yakalama işlemi devam edemiyor. Blobu silin ya da bayrağı VHD bloblarının üzerine yazacak şekilde ayarlayın ve yeniden deneyin.  |
|  TargetDiskBlobAlreadyExists var  |  Hedef görüntü {0} blobunun üzerinde etkin bir kiralama olduğundan yakalama işlemi devam edemiyor.   |
|  TargetDiskBlobAlreadyExists var  |  Blob {0} zaten var. Lütfen ' ' diski için hedef olarak farklı bir blob URI 'SI sağlayın {1} .  |
|  TooManyVMRedeploymentRequests  |  VM ' ' için çok fazla yeniden dağıtım isteği alındı {0} veya bu VM ile aynı Kullanılabilirlik kümesindeki VM 'ler. Lütfen daha sonra yeniden deneyin.  |
|  Vhdsizegeçersiz  |  {0}BLOB ile ' ' diskinin belirtilen disk boyutu değeri {1} {2} geçersiz. Disk boyutu ile arasında olmalıdır {3} {4} .  |
|  VMAgentStatusCommunicationError  |  VM ' {0} ', VM Aracısı veya uzantıları için durum bildirmedi. Lütfen VM 'nin çalışan bir VM aracısına sahip olduğunu ve Azure depolama 'ya giden bağlantılar kurabildiğini doğrulayın.  |
|  Vmartifactrepositoryınternalerror  |  VM yapıt ayrıntılarını almak için yapıt deposu ile iletişim kurulurken bir hata oluştu.  |
|  Vmartifactrepositoryınternalerror  |  Yapıt deposundan VM yapıt verileri alınırken bir iç hata oluştu.  |
|  Vmextensionhandlernongeçişli Enterror  |  ' ' İşleyicisi, ' ' {0} ADLı VM uzantısı için ' {1} ' Terminal hata kodu {2} ve ' ' hata iletisiyle ilgili bir hata bildirdi. {3}  |
|  Vmextensionmanagementınternalerror  |  ' ' VM Uzantısı işlenirken iç hata oluştu {0} .  |
|  Vmextensionmanagementınternalerror  |  VM uzantıları hazırlanırken birden çok hata oluştu. Ayrıntılar için bkz. VM uzantı örneği görünümü.  |
|  VMExtensionProvisioningError  |  VM, ' ' uzantısını işlerken bir hata bildirdi {0} . Hata iletisi: " {1} ".  |
|  VMExtensionProvisioningError  |  VM 'de birden çok VM Uzantısı sağlanamadı. Ayrıntılar için lütfen VM uzantısı örnek görünümüne bakın.  |
|  VMExtensionProvisioningTimeout  |  ' ' VM uzantısının sağlanması {0} zaman aşımına uğradı. Uzantı yüklemesi çok uzun sürüyor veya uzantı durumu elde edilemedi.  |
|  Vmmarketplaceınvalidınput  |  Market olmayan bir görüntüden sanal makine oluşturmak için plan bilgileri gerekmez, lütfen istekteki plan bilgilerini kaldırın. İşletim sistemi diski adı {0} .  |
|  Vmmarketplaceınvalidınput  |  Satın alma bilgileri eşleşmiyor. Market görüntüsünden dağıtım yapılamıyor. İşletim sistemi diski adı {0} .  |
|  Vmmarketplaceınvalidınput  |  Market görüntüsünden bir sanal makine oluşturmak istekte plan bilgileri gerektirir. İşletim sistemi diski adı {0} .  |
|  VMNotFound  |  ' ' Sanal makinesi {0} bulunamıyor.  |
|  VMRedeploymentFailed  |  VM ' {0} ' yeniden dağıtımı bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin.  |
|  Vmredeploymenttimeın  |  ' ' Sanal makinesinin yeniden dağıtımı {0} ayrılan sürede tamamlanmadı. Bir süre içinde başarıyla bitebilirler. Aksi takdirde, isteği yeniden deneyebilirsiniz.  |
|  Vmstarttimepr  |  ' ' VM 'si {0} ayrılan süre içinde başlatılmadı. VM hala başarıyla başlayabilir. Lütfen daha sonra güç durumunu kontrol edin.  |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.