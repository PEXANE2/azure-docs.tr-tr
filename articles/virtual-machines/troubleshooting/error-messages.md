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
ms.openlocfilehash: f5639d1cf94c77d699dc6de9841698b045ac1f96
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76543027"
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
|  AcquireDiskLeaseFailed  |  URI{0} {1}ile blob kullanılarak ' ' diski oluşturulurken kira elde edilemedi. Blob zaten kullanımda.  |  
|  AllocationFailed  |  Ayırma başarısız oldu. Lütfen VM boyutunu veya VM sayısını azaltmayı deneyin, daha sonra yeniden deneyin veya farklı bir kullanılabilirlik kümesine veya farklı Azure konumuna dağıtım yapmayı deneyin.  |  
|  AllocationFailed  |  VM ayırması bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin veya farklı bir konuma dağıtımı deneyin.  |
|  ArtifactNotFound  |  Yayımcı '{0}' ve türü '{1}' olan VM Uzantısı '{2}' konumunda bulunamadı.  |
|  ArtifactNotFound  |  Uzantı deposunda Yayımcı '{0}', tür '{1}' ve tür işleyicisi sürümü '{2}' olan uzantı bulunamadı.  |
|  ArtifactVersionNotFound  |  Yapıt deposunda, istenen '{0}' sürümünü karşılayan bir sürüm bulunamadı.  |
|  ArtifactVersionNotFound  |  Yapıt deposunda, Yayımcı '{0}{1}' ve türü '{2}' olan VM uzantısı için istenen ' ' sürümünü karşılayan bir sürüm bulunamadı.  |
|  Attachdiskwhilebeingayrıldı  |  Disk şu anda ayrılmakta{0}olduğundan ' ' sanal{1}makinesine ' ' veri diski iliştirilemiyor. Lütfen disk tamamen ayrılana kadar bekleyip yeniden deneyin.  |
|  Işlemindeki hatalı istek  |  Hizalanmış ' kullanılabilirlik kümeleri bu bölgede henüz desteklenmiyor.  |
|  Işlemindeki hatalı istek  |  Yönetilmeyen kullanılabilirlik kümesine veya blob tabanlı disklere sahip VM 'nin yönetilen kullanılabilirlik kümesine eklenmesi için yönetilen disklere sahip bir VM 'nin eklenmesi desteklenmez. Yönetilen disklere sahip bir VM eklemek için lütfen ' Managed ' özelliği ayarlanmış bir kullanılabilirlik kümesi oluşturun.  |
|  Işlemindeki hatalı istek  |  Yönetilen diskler bu bölgede desteklenmez.  |
|  Işlemindeki hatalı istek  |  '{0}' İşletim sistemi türü için işleyici başına birden çok VMExtensions desteklenmiyor. '{2}' İşleyicisiyle{1}' ' adlı vmexgerilim, girişte zaten eklenmiş veya belirtilmiş.  |
|  Işlemindeki hatalı istek  |  '{0}' İşlemi, yönetilen disklerle '{1}' kaynağında desteklenmez.  |
|  Certificateımproperlybiçimlendirildi  |  Parolanın öğesinden {0} alınan JSON gösterimi, düzgün şekilde BIÇIMLENDIRILEN bir PFX dosyası olmayan bir veri alanına sahip veya BELIRTILEN parola PFX dosyasının doğru şekilde şifresini çözemedi.  |
|  Certificateımproperlybiçimlendirildi  |  Öğesinden {0} ALıNAN veriler JSON 'a seri hale getirilemez.  |
|  Çakışma  |  Yalnızca bir VM oluşturulurken veya VM serbest bırakıldığında disk yeniden boyutlandırılmasına izin verilir.  |
|  Conflictinguserınput  |  Disk,{0}'{1}' sanal makinesine ait olduğundan ' ' diski iliştirilemiyor.  |
|  Conflictinguserınput  |  Kaynak ve hedef kaynak grupları aynıdır.  |
|  Conflictinguserınput  |  Disk {0} için kaynak ve hedef depolama hesapları farklı.  |
|  ContainerAlreadyOnLease  |  Blob 'u URI {0}ile tutan depolama kapsayıcısı üzerinde zaten bir kira var.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Kaynakları taşıma isteği, istekteki bir veya daha fazla {0}tarafından başvurulan Anahtar Kasası kaynaklarını içerir. Bu, şu anda çapraz abonelik taşıması içinde desteklenmez. Anahtar Kasası kaynak kimlikleri için lütfen hata ayrıntılarını kontrol edin.  |
|  Diagnosticsoperationınternalerror  |  VM {0}'nin tanılama profili işlenirken bir iç hata oluştu.  |
|  Diskblobalreadınusebyanotherdisk  |  Blob {0} , '{1}' sanal makinesine ait başka bir disk tarafından zaten kullanılıyor. Disk başvuru bilgileri için blob meta verilerini inceleyebilirsiniz.  |
|  DiskBlobNotFound  |  ' {0} {1}' DISKI için URI ile VHD blobu bulunamıyor.  |
|  DiskBlobNotFound  |  URI {0}ile VHD blobu bulunamıyor.  |
|  DiskEncryptionKeySecretMissingTags  |  {0}gizli dizi {1} etiketleri yok. Lütfen gizli sürümü güncelleştirin, gerekli etiketleri ekleyin ve yeniden deneyin.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Anahtar {0} {1} kullanılarak parola değerinin sarmalaması geri alınamadı.  |
|  DiskImageNotReady  |  Disk görüntüsü {0} {1} durumunda. Görüntü hazırlanıyor, lütfen yeniden deneyin.  |
|  DiskPreparationError  |  VM diskleri hazırlanırken bir veya daha fazla hata oluştu. Ayrıntılar için bkz. disk örneği görünümü.  |
|  DiskProcessingError  |  VM, başarısız disklerde başka diskler içerdiğinden disk işleme durduruldu.  |
|  Imageblobnotfound  |  ' {0} {1}' DISKI için URI ile VHD blobu bulunamıyor.  |
|  Imageblobnotfound  |  URI {0}ile VHD blobu bulunamıyor.  |
|  IncorrectDiskBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. ' {0} {1}' Diski için blob, Blok Blobu türünde.  |
|  IncorrectDiskBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. Blob {0} '{1}' türünde.  |
|  IncorrectImageBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. ' {0} {1}' Diski için blob, Blok Blobu türünde.  |
|  IncorrectImageBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. Blob {0} '{1}' türünde.  |
|  Internaloperationerror  |  Depolama hesabı {0}çözümlenemedi. Lütfen işlem kaynağıyla aynı konumdaki depolama kaynak sağlayıcısı üzerinden oluşturulduğundan emin olun.  |
|  Internaloperationerror  |  {0}hedef arama görevi başarısız oldu.  |
|  Internaloperationerror  |  '{0}' Sanal makinesinin ağ profili doğrulanırken hata oluştu.  |
|  Invalidaccounttype  |  AccountType {0} geçersiz.  |
|  Geçersiz parametre  |  Parametrenin {0} değeri geçersiz.  |
|  Geçersiz parametre  |  Belirtilen yönetici parolasına izin verilmiyor.  |
|  Geçersiz parametre  |  "Sağlanan parola {0}{1} karakter uzunluğunda olmalıdır ve şunlardan en az {2} parola karmaşıklığı gereksinimlerinden oluşmalıdır: <ol><li> Büyük harfli bir karakter içerir</li><li>Küçük harfli bir karakter içerir</li><li>Sayısal bir basamak içerir</li><li>Özel bir karakter içerir.</li></ol>  |
|  Geçersiz parametre  |  Belirtilen yönetici kullanıcı adına izin verilmiyor.  |
|  Geçersiz parametre  |  VM bir platform veya Kullanıcı görüntüsünden oluşturulduysa, var olan bir işletim sistemi diski iliştirilemez.  |
|  Geçersiz parametre  |  Kapsayıcı adı {0} geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalı ve yalnızca küçük harfli alfasayısal karakterler ve kısa çizgi içerebilir. Kısa çizgi öncesinde ve sonra alfasayısal bir karakter gelmelidir.  |
|  Geçersiz parametre  |  URL {1} 'de {0} kapsayıcı adı geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalı ve yalnızca küçük harfli alfasayısal karakterler ve kısa çizgi içerebilir. Kısa çizgi öncesinde ve sonra alfasayısal bir karakter gelmelidir.  |
|  Geçersiz parametre  |  URL {0} 'deki blob adı eğik çizgi içeriyor. Bu, şu anda diskler için desteklenmez.  |
|  Geçersiz parametre  |  URI {0} doğru blob URI 'si olarak görünmüyor.  |
|  Geçersiz parametre  |  '{0}' Adlı bir disk zaten aynı LUN 'u kullanıyor: {1}.  |
|  Geçersiz parametre  |  '{0}' Adlı bir disk zaten var.  |
|  Geçersiz parametre  |  Belirtilen görüntü başvurusunda zaten tanımlı olan bir disk için Kullanıcı görüntüsü geçersiz kılmaları belirtilemez.  |
|  Geçersiz parametre  |  '{0}' Adlı bir disk zaten aynı VHD URL 'sini {1}kullanıyor.  |
|  Geçersiz parametre  |  Belirtilen hata etki alanı sayısı {0} , aralığında {1} olmalıdır {2}.  |
|  Geçersiz parametre  |  Lisans türü {0} geçersiz. Geçerli lisans türleri şunlardır: Windows_Client veya Windows_Server, büyük/küçük harfe duyarlı.  |
|  Geçersiz parametre  |  Linux ana bilgisayar adı karakter {0} uzunluğunu aşamaz veya şu karakterleri içeremez: {1}.  |
|  Geçersiz parametre  |  Linux sağlama aracısındaki bilinen bir sorun nedeniyle SSH ortak anahtarları için hedef {0} yol şu anda varsayılan değeriyle sınırlıdır.  |
|  Geçersiz parametre  |  LUN {0} 'daki bir disk zaten var.  |
|  Geçersiz parametre  |  İsteğin {0} aboneliği, yönetilen disk kimliğinde bulunan abonelikle {1} aynı olmalıdır.  |
|  Geçersiz parametre  |  OSProfile içindeki özel verilerin base64 kodlamalı ve en fazla {0} karakter uzunluğunda olması gerekir.  |
|  Geçersiz parametre  |  URL {0} 'deki blob adı '{1}' uzantısıyla bitmelidir.  |
|  Geçersiz parametre  |  {0}', geçerli bir yakalanan VHD blob adı ön eki değil. Geçerli bir önek, Regex '{1}' ile eşleşiyor.  |
|  Geçersiz parametre  |  VM Aracısı sağlanmadıysa sanal makinenize sertifikalar eklenemiyor.  |
|  Geçersiz parametre  |  LUN {0} 'daki bir disk zaten var.  |
|  Geçersiz parametre  |  İstenen boyut {0} , kullanılabilirlik kümesinin Şu anda ayrıldığı kümede kullanılamadığından VM oluşturulamıyor. Kullanılabilir Boyutlar: {1}. Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha https://aka.ms/azure-resizevmfazla bilgi edinin.  |
|  Geçersiz parametre  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Geçerli bölgede bulunan Boyutlar: {1}. Konumundaki https://aka.ms/azure-regionsher bir BÖLGEDEKI kullanılabilir VM boyutları hakkında daha fazla bilgi edinin.  |
|  Geçersiz parametre  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Konumundaki https://aka.ms/azure-regionsher bir BÖLGEDEKI kullanılabilir VM boyutları hakkında daha fazla bilgi edinin.  |
|  Geçersiz parametre  |  Windows Yönetici Kullanıcı adı {0} karakterden uzun olamaz, nokta (.) ile bitemez veya şu karakterleri içeremez:. {1}  |
|  Geçersiz parametre  |  Windows bilgisayar adı {0} karakterden uzun olamaz, tamamen sayısal olamaz veya şu karakterleri içeremez: {1}.  |
|  Missingmovedependentkaynakları  |  Kaynakları taşıma isteği tüm bağımlı kaynakları içermez. Lütfen eksik kaynak kimlikleri için hata ayrıntılarını denetleyin.  |
|  Moveresourceshaveınvalidstate  |  Kaynakları taşıma isteği, geçersiz depolama hesaplarıyla ilişkili VM 'Leri içerir. Lütfen bu kaynak kimliklerinin ayrıntılarını ve başvurulan depolama hesabı adlarını denetleyin.  |
|  MoveResourcesHavePendingOperations  |  Kaynakları taşıma isteği, bir işlemin beklediği kaynakları içerir. Lütfen bu kaynak kimliklerinin ayrıntılarını kontrol edin. Bekleyen işlemler tamamlandıktan sonra işleminizi yeniden deneyin.  |
|  MoveResourcesNotFound  |  Kaynakları taşıma isteği bulunamayan kaynakları içerir. Lütfen bu kaynak kimliklerinin ayrıntılarını kontrol edin.  |
|  Networkingınternaloperationerror  |  Bilinmeyen ağ ayırma hatası.  |
|  Networkingınternaloperationerror  |  Bilinmeyen ağ ayırma hatası  |
|  Networkingınternaloperationerror  |  VM 'nin ağ profilini işlerken bir iç hata oluştu.  |
|  NotFound  |  Kullanılabilirlik kümesi {0} bulunamıyor.  |
|  NotFound  |  İstekte belirtilen '{0}' kaynak sanal makinesi bu Azure konumunda yok.  |
|  NotFound  |  Kimliği {0} olan kiracı bulunamadı.  |
|  NotFound  |  Görüntü {0} bulunamıyor.  |
|  NotSupported  |  Lisans türü {0}, ancak görüntü blobu {1} Şirket içinden değildir.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi {0} silinemiyor. Bir kullanılabilirlik kümesini silmeden önce lütfen herhangi bir VM içermediğinden emin olun.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi SKU 'SU ' hizalı ' değerinden ' klasik ' öğesine değiştirilmesine izin verilmez.  |
|  OperationNotAllowed  |  VM çalışmadığı zaman VM 'deki uzantılar değiştirilemez.  |
|  OperationNotAllowed  |  Yakalama eylemi yalnızca blob tabanlı disklere sahip bir sanal makinede desteklenir. Yönetilen bir sanal makineden bir görüntü oluşturmak için lütfen ' görüntü ' kaynak API 'Lerini kullanın.  |
|  OperationNotAllowed  |  Görüntü başarıyla {0} oluşturuluncaya kadar görüntüden {1} kaynak oluşturulamıyor.  |
|  OperationNotAllowed  |  VM ayrıldığında encryptionSettings güncelleştirmelerine izin verilmiyor, lütfen VM serbest bırakıldıktan sonra yeniden deneyin  |
|  OperationNotAllowed  |  Blob tabanlı diskler içeren bir VM 'ye yönetilen disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  Bu boyuttaki bir VM 'ye bağlı olmasına izin verilen en fazla veri diski sayısı {0}.  |
|  OperationNotAllowed  |  Yönetilen disklerle VM 'ye blob tabanlı bir disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  Görüntü silinmek{0}üzere işaretlendiğinden '{1}' görüntüsünde ' ' işlemine izin verilmiyor. Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  VM Genelleştirilmiş{0}olduğundan ' ' sanal{1}makinesinde ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  '{1}'{0}Geri yükleme noktası koleksiyonu silinmek üzere işaretlendiğinden ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  {0}' ' İşlemi silinmek üzere işaretlendiğinden '{1}' VM uzantısında izin verilmiyor. Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  ' '{0}Sanal makineleri{1}' ' görüntüsü{2}kullanılarak sağlanmakta olduğundan ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  ' '{0}Sanal makine ölçek kümesi{1}Şu anda '{2}' görüntüsünü kullandığından ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  VM silinmek{0}üzere işaretlendiğinden '{1}' sanal makinesinde ' ' işlemine izin verilmiyor. Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  VM serbest{0}bırakılmış veya serbest bırakılmış olarak işaretlenmiş olduğundan{1}' ' sanal makinesinde ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  VM çalıştığından{0}, ' ' sanal{1}makinesinde ' ' işlemine izin verilmiyor. Lütfen VM 'yi konuk işletim sisteminin içinden kapattığınız durumda açık bir şekilde kapatın.  |
|  OperationNotAllowed  |  VM serbest{0}bırakılmadığı için ' ' sanal{1}makinesinde ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  VM 'nin{0}'{2}' uzantısı başarısız durumunda olduğundan '{1}' sanal makinesinde ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  Devam eden{0}başka bir işlem olduğundan ' ' sanal{1}makinesinde ' ' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  '{0}' İşlemi, '{1}' sanal makinesinin genelleştirilmesi gerektiriyor.  |
|  OperationNotAllowed  |  İşlem için VM 'nin çalışıyor olması gerekir (veya çalıştırılacak şekilde ayarlanır).  |
|  OperationNotAllowed  |  Görüntüde karşılık gelen {0}disk boyutundan {1}daha küçük olan boyut GB olan diske izin verilmez.  |
|  OperationNotAllowed  |  '{0}' Işleyicisinin VM Ölçek kümesi UZANTıLARı yalnızca VM Ölçek kümesi oluşturma sırasında eklenebilir.  |
|  OperationNotAllowed  |  '{0}' Işleyicisinin VM Ölçek kümesi UZANTıLARı yalnızca VM Ölçek kümesi silme sırasında silinebilir.  |
|  OperationNotAllowed  |  VM '{0}' zaten yönetilen diskler kullanıyor.  |
|  OperationNotAllowed  |  '{0}' Sanal makinesi '{1}' klasik ' kullanılabilirlik kümesine aittir. Lütfen kullanılabilirlik kümesini ' hizalı ' SKU 'SU kullanacak şekilde güncelleştirin ve sonra dönüştürmeyi yeniden deneyin.  |
|  OperationNotAllowed  |  Görüntüden oluşturulan VM 'de blob tabanlı diskler olamaz. Tüm disklerin yönetilen diskler olması gerekir.  |
|  OperationNotAllowed  |  VM Genelleştirilmiş olmadığından yakalama işlemi tamamlanamıyor.  |
|  OperationNotAllowed  |  VM diskleri yönetilen disklere dönüştürülemediğinden '{0}' sanal makinesinde yönetim işlemlerine izin verilmiyor.  |
|  OperationNotAllowed  |  Devam eden bir işlem, sanal makinenin {0} güç durumunu ' e {1}değiştiriyor. Lütfen bir süre {2} sonra işlemi gerçekleştirin.  |
|  OperationNotAllowed  |  VM eklenemiyor veya güncelleştirilemiyor. İstenen VM boyutu {0} mevcut ayırma biriminde bulunmayabilir. Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha https://aka.ms/azure-resizevmfazla bilgi edinin.  |
|  OperationNotAllowed  |  İstenen boyut {0} , kullanılabilirlik kümesinin Şu anda ayrıldığı kümede kullanılamadığından VM yeniden boyutlandırılamıyor. Kullanılabilir Boyutlar: {1}. Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha https://aka.ms/azure-resizevmfazla bilgi edinin.  |
|  OperationNotAllowed  |  İstenen boyut {0} VM 'nin şu anda ayrıldığı kümede kullanılamadığından VM yeniden boyutlandırılamıyor. VM 'nizi yeniden boyutlandırmak için {1} (Bu Azure Portal durdurma işlemi) ve yeniden boyutlandırma işlemini yeniden deneyin. Üzerinde VM yeniden boyutlandırma stratejisi hakkında daha https://aka.ms/azure-resizevmfazla bilgi edinin.  |
|  OSProvisioningClientError  |  Konuk işletim sistemi şu anda sağlanmakta olduğundan, '{0}' sanal makinesi için Işletim sistemi sağlama başarısız oldu.  |
|  OSProvisioningClientError  |  '{0}' Sanal makinesi için işletim sistemi sağlama başarısız oldu. Hata ayrıntıları: {1} görüntünün düzgün hazırlanmış (Genelleştirilmiş) olduğundan emin olun. <ul><li>Windows için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  SSH ana bilgisayar anahtarı oluşturulamadı. Hata ayrıntıları: {0}. Bu sorunu gidermek için, Linux aracısının düzgün ayarlanıp ayarlanmadığını doğrulayın. <ul><li>Yönergeleri şu adreste bulabilirsiniz:https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux/ </li></ul> |
|  OSProvisioningClientError  |  VM için belirtilen Kullanıcı adı bu Linux dağıtımı için geçersiz. Hata ayrıntıları: {0}.  |
|  Osprovisioningınternalerror  |  Bir iç hata nedeniyle VM '{0}' için Işletim sistemi sağlama başarısız oldu.  |
|  Osprovisioningtime,  |  '{0}' Sanal makinesi için Işletim sistemi sağlama, ayrılan sürede tamamlanmadı. VM sağlamayı başarıyla bitirirebilir. Lütfen sağlama durumunu daha sonra denetleyin.  |
|  Osprovisioningtime,  |  '{0}' Sanal makinesi için Işletim sistemi sağlama, ayrılan sürede tamamlanmadı. VM sağlamayı başarıyla bitirirebilir. Lütfen sağlama durumunu daha sonra denetleyin. Ayrıca, görüntünün düzgün hazırlanmış (Genelleştirilmiş) olduğundan emin olun.   <ul><li>Windows için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  Osprovisioningtime,  |  '{0}' Sanal makinesi için Işletim sistemi sağlama, ayrılan sürede tamamlanmadı. Ancak, VM Konuk Aracısı çalışıyor olarak algılandı. Bu, Konuk işletim sisteminin bir VM görüntüsü (CreateOption = FromImage) olarak kullanılmak üzere düzgün şekilde hazırlandığını önerir. Bu sorunu çözmek için, VHD 'yi CreateOption = Attach ile birlikte kullanın veya görüntü olarak kullanmak üzere doğru şekilde hazırlayın:   <ul><li>Windows için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler:https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Gerekli VM boyutu şu anda seçili konumda kullanılamıyor.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Devam eden platform güncelleştirmesi nedeniyle kaynak şu anda güncelleştirilemiyor. Lütfen daha sonra tekrar deneyin.  |
|  Storageaccountsınırlamanın  |  '{0}' Depolama hesabı, disk oluşturmak için gerekli olan sayfa bloblarını desteklemiyor.  |
|  Storageaccountsınırlamanın  |  '{0}' Depolama hesabı, ayrılmış kotasını aştı.  |
|  Storageaccountlocationuyuşmazlığıdır  |  Depolama hesabı {0}çözümlenemedi. Lütfen işlem kaynağıyla aynı konumdaki depolama kaynak sağlayıcısı üzerinden oluşturulduğundan emin olun.  |
|  StorageAccountNotFound  |  Depolama hesabı {0} bulunamadı. Depolama hesabının silinmediğinden ve VM ile aynı Azure konumuna ait olduğundan emin olun.  |
|  Storageaccountnottanınmış  |  Lütfen depolama kaynak sağlayıcısı tarafından yönetilen bir depolama hesabı kullanın. {0} Kullanılması desteklenmez.  |
|  Storageaccountoperationınternalerror  |  Depolama hesabına {0}erişilirken iç hata oluştu.  |
|  Storageaccountsubscriptionuyuşmazlığı  |  Depolama hesabı {0} aboneliğe {1}ait değil.  |
|  StorageAccountTooBusy  |  '{0}' Depolama hesabı şu anda çok meşgul. Başka bir hesap kullanmayı düşünün.  |
|  StorageAccountTypeNotSupported  |  Blob {0} depolama {1} hesabı olan disk kullanılır. Lütfen genel amaçlı depolama hesabı ile yeniden deneyin.  |
|  StorageAccountTypeNotSupported  |  Depolama hesabı {0} {1} türündedir. Önyükleme tanılaması depolama {2} hesabı türlerini destekler.  <ul><li>Bu hata, önyükleme tanılaması için Premium depolama hesabı kullanırsanız oluşur. Daha fazla bilgi için bkz. [önyükleme tanılamayı kullanma](boot-diagnostics.md). </li></ul> |
|  SubscriptionNotAuthorizedForImage  |  Abonelik yetkili değil.  |
|  TargetDiskBlobAlreadyExists var  |  Blob {0} zaten var. Lütfen yeni boş bir '{1}' veri diski oluşturmak için farklı bir blob URI 'si sağlayın.  |
|  TargetDiskBlobAlreadyExists var  |  Hedef görüntü blobu {0} zaten VAROLDUĞUNDAN ve VHD bloblarının üzerine yazılacak bayrak ayarlanmadığından yakalama işlemi devam edemiyor. Blobu silin ya da bayrağı VHD bloblarının üzerine yazacak şekilde ayarlayın ve yeniden deneyin.  |
|  TargetDiskBlobAlreadyExists var  |  Hedef görüntü blobunun {0} üzerinde etkin bir kiralama olduğundan yakalama işlemi devam edemiyor.   |
|  TargetDiskBlobAlreadyExists var  |  Blob {0} zaten var. Lütfen '{1}' diski için hedef olarak farklı bir blob URI 'si sağlayın.  |
|  TooManyVMRedeploymentRequests  |  VM '{0}' için çok fazla yeniden dağıtım isteği alındı veya bu VM ile aynı Kullanılabilirlik kümesindeki VM 'ler. Lütfen daha sonra yeniden deneyin.  |
|  Vhdsizegeçersiz  |  Blob {2} ile '{1}' diskinin belirtilen {0} disk boyutu değeri geçersiz. Disk boyutu ile {3} {4}arasında olmalıdır.  |
|  VMAgentStatusCommunicationError  |  VM '{0}', VM Aracısı veya uzantıları için durum bildirmedi. Lütfen VM 'nin çalışan bir VM aracısına sahip olduğunu ve Azure depolama 'ya giden bağlantılar kurabildiğini doğrulayın.  |
|  Vmartifactrepositoryınternalerror  |  VM yapıt ayrıntılarını almak için yapıt deposu ile iletişim kurulurken bir hata oluştu.  |
|  Vmartifactrepositoryınternalerror  |  Yapıt deposundan VM yapıt verileri alınırken bir iç hata oluştu.  |
|  Vmextensionhandlernongeçişli Enterror  |  '{0}' İşleyicisi, ' ' adlı VM Uzantısı{1}için '{2}' Terminal hata kodu ve '{3}' hata iletisiyle ilgili bir hata bildirdi.  |
|  Vmextensionmanagementınternalerror  |  '{0}' VM Uzantısı işlenirken iç hata oluştu.  |
|  Vmextensionmanagementınternalerror  |  VM uzantıları hazırlanırken birden çok hata oluştu. Ayrıntılar için bkz. VM uzantı örneği görünümü.  |
|  VMExtensionProvisioningError  |  VM, '{0}' uzantısını işlerken bir hata bildirdi. Hata iletisi: "{1}".  |
|  VMExtensionProvisioningError  |  VM 'de birden çok VM Uzantısı sağlanamadı. Ayrıntılar için lütfen VM uzantısı örnek görünümüne bakın.  |
|  VMExtensionProvisioningTimeout  |  '{0}' VM uzantısının sağlanması zaman aşımına uğradı. Uzantı yüklemesi çok uzun sürüyor veya uzantı durumu elde edilemedi.  |
|  Vmmarketplaceınvalidınput  |  Market olmayan bir görüntüden sanal makine oluşturmak için plan bilgileri gerekmez, lütfen istekteki plan bilgilerini kaldırın. İşletim sistemi diski adı {0}.  |
|  Vmmarketplaceınvalidınput  |  Satın alma bilgileri eşleşmiyor. Market görüntüsünden dağıtım yapılamıyor. İşletim sistemi diski adı {0}.  |
|  Vmmarketplaceınvalidınput  |  Market görüntüsünden bir sanal makine oluşturmak istekte plan bilgileri gerektirir. İşletim sistemi diski adı {0}.  |
|  VMNotFound  |  '{0}' Sanal makinesi bulunamıyor.  |
|  VMRedeploymentFailed  |  VM '{0}' yeniden dağıtımı bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin.  |
|  Vmredeploymenttimeın  |  '{0}' Sanal makinesinin yeniden dağıtımı ayrılan sürede tamamlanmadı. Bir süre içinde başarıyla bitebilirler. Aksi takdirde, isteği yeniden deneyebilirsiniz.  |
|  Vmstarttimepr  |  '{0}' VM 'si ayrılan süre içinde başlatılmadı. VM hala başarıyla başlayabilir. Lütfen daha sonra güç durumunu kontrol edin.  |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayı da oluşturabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
