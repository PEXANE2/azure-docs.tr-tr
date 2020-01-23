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
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76543027"
---
# <a name="understand-common-error-messages-when-you-manage-virtual-machines-in-azure"></a>Azure 'da sanal makineleri yönetirken yaygın hata iletilerini anlama

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
|  AcquireDiskLeaseFailed  |  URI {1}ile blob kullanılarak '{0}' diski oluşturulurken kira elde edilemedi. Blob zaten kullanımda.  |  
|  AllocationFailed  |  Ayırma başarısız oldu. Lütfen VM boyutunu veya VM sayısını azaltmayı deneyin, daha sonra yeniden deneyin veya farklı bir kullanılabilirlik kümesine veya farklı Azure konumuna dağıtım yapmayı deneyin.  |  
|  AllocationFailed  |  VM ayırması bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin veya farklı bir konuma dağıtımı deneyin.  |
|  ArtifactNotFound  |  Yayımcı '{0}' ve türü '{1}' olan VM Uzantısı '{2}' konumunda bulunamadı.  |
|  ArtifactNotFound  |  Uzantı deposunda '{0}' yayımcısına, '{1}' türüne ve '{2}' tür işleyicisi sürümüne sahip uzantı bulunamadı.  |
|  ArtifactVersionNotFound  |  Yapıt deposunda, istenen '{0}' sürümünü karşılayan bir sürüm bulunamadı.  |
|  ArtifactVersionNotFound  |  Yapıt deposunda, Yayımcı '{1}' ve türü '{2}' olan VM uzantısı için istenen '{0}' sürümünü karşılayan bir sürüm bulunamadı.  |
|  Attachdiskwhilebeingayrıldı  |  Disk şu anda ayrılmakta olduğundan '{0}' veri diski '{1}' sanal makinesine iliştirilemiyor. Lütfen disk tamamen ayrılana kadar bekleyip yeniden deneyin.  |
|  Işlemindeki hatalı istek  |  Hizalanmış ' kullanılabilirlik kümeleri bu bölgede henüz desteklenmiyor.  |
|  Işlemindeki hatalı istek  |  Yönetilmeyen kullanılabilirlik kümesine veya blob tabanlı disklere sahip VM 'nin yönetilen kullanılabilirlik kümesine eklenmesi için yönetilen disklere sahip bir VM 'nin eklenmesi desteklenmez. Yönetilen disklere sahip bir VM eklemek için lütfen ' Managed ' özelliği ayarlanmış bir kullanılabilirlik kümesi oluşturun.  |
|  Işlemindeki hatalı istek  |  Yönetilen diskler bu bölgede desteklenmez.  |
|  Işlemindeki hatalı istek  |  '{0}' işletim sistemi türü için işleyici başına birden çok VMExtensions desteklenmiyor. '{2}' işleyicisine sahip '{1}' Vmexgerilim, girişte zaten eklenmiş veya belirtilmiş.  |
|  Işlemindeki hatalı istek  |  Yönetilen disklerle '{1}' kaynağında '{0}' işlemi desteklenmiyor.  |
|  Certificateımproperlybiçimlendirildi  |  Gizli dizi {0} alınan JSON gösterimi, düzgün şekilde biçimlendirilen bir PFX dosyası olmayan bir veri alanına sahip veya belirtilen parola PFX dosyasının doğru şekilde şifresini çözemedi.  |
|  Certificateımproperlybiçimlendirildi  |  {0} alınan veriler JSON 'a seri hale getirilemez.  |
|  Çakışma  |  Yalnızca bir VM oluşturulurken veya VM serbest bırakıldığında disk yeniden boyutlandırılmasına izin verilir.  |
|  Conflictinguserınput  |  '{1}' sanal makinesi zaten sahip olduğundan '{0}' diski iliştirilemiyor.  |
|  Conflictinguserınput  |  Kaynak ve hedef kaynak grupları aynıdır.  |
|  Conflictinguserınput  |  Disk {0} için kaynak ve hedef depolama hesapları farklı.  |
|  ContainerAlreadyOnLease  |  Blob 'u URI {0}tutan depolama kapsayıcısı üzerinde zaten bir kira var.  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Kaynakları taşıma isteği, istekteki bir veya daha fazla {0}s tarafından başvurulan Anahtar Kasası kaynaklarını içerir. Bu, şu anda çapraz abonelik taşıması içinde desteklenmez. Anahtar Kasası kaynak kimlikleri için lütfen hata ayrıntılarını kontrol edin.  |
|  Diagnosticsoperationınternalerror  |  VM {0}tanılama profili işlenirken bir iç hata oluştu.  |
|  Diskblobalreadınusebyanotherdisk  |  Blob {0}, '{1}' VM 'sine ait başka bir disk tarafından zaten kullanılıyor. Disk başvuru bilgileri için blob meta verilerini inceleyebilirsiniz.  |
|  DiskBlobNotFound  |  '{1}' diski için URI {0} VHD blobu bulunamıyor.  |
|  DiskBlobNotFound  |  URI {0}VHD blobu bulunamıyor.  |
|  DiskEncryptionKeySecretMissingTags  |  {0} gizli dizi {1} etiketlere sahip değil. Lütfen gizli sürümü güncelleştirin, gerekli etiketleri ekleyin ve yeniden deneyin.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Anahtar {1} kullanarak gizli {0} değerinin sarmalaması geri alınamadı.  |
|  DiskImageNotReady  |  Disk görüntüsü {0} {1} durumunda. Görüntü hazırlanıyor, lütfen yeniden deneyin.  |
|  DiskPreparationError  |  VM diskleri hazırlanırken bir veya daha fazla hata oluştu. Ayrıntılar için bkz. disk örneği görünümü.  |
|  DiskProcessingError  |  VM, başarısız disklerde başka diskler içerdiğinden disk işleme durduruldu.  |
|  Imageblobnotfound  |  '{1}' diski için URI {0} VHD blobu bulunamıyor.  |
|  Imageblobnotfound  |  URI {0}VHD blobu bulunamıyor.  |
|  IncorrectDiskBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. '{1}' diski için blob {0}, Blok Blobu türünde.  |
|  IncorrectDiskBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. Blob {0} '{1}' türünde.  |
|  IncorrectImageBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. '{1}' diski için blob {0}, Blok Blobu türünde.  |
|  IncorrectImageBlobType  |  Disk blob 'ları yalnızca Sayfa Blobu türünde olabilir. Blob {0} '{1}' türünde.  |
|  Internaloperationerror  |  {0}depolama hesabı çözümlenemedi. Lütfen işlem kaynağıyla aynı konumdaki depolama kaynak sağlayıcısı üzerinden oluşturulduğundan emin olun.  |
|  Internaloperationerror  |  görevleri arayan {0} hedef başarısız oldu.  |
|  Internaloperationerror  |  '{0}' VM 'sinin ağ profili doğrulanırken hata oluştu.  |
|  Invalidaccounttype  |  AccountType {0} geçersiz.  |
|  Geçersiz parametre  |  {0} parametresi değeri geçersiz.  |
|  Geçersiz parametre  |  Belirtilen yönetici parolasına izin verilmiyor.  |
|  Geçersiz parametre  |  "Sağlanan parola {0}{1} karakter uzunluğunda olmalı ve aşağıdakiler için en az {2} parola karmaşıklığı gereksinimi karşılamalıdır: <ol><li> Büyük harfli bir karakter içerir</li><li>Küçük harfli bir karakter içerir</li><li>Sayısal bir basamak içerir</li><li>Özel bir karakter içerir.</li></ol>  |
|  Geçersiz parametre  |  Belirtilen yönetici kullanıcı adına izin verilmiyor.  |
|  Geçersiz parametre  |  VM bir platform veya Kullanıcı görüntüsünden oluşturulduysa, var olan bir işletim sistemi diski iliştirilemez.  |
|  Geçersiz parametre  |  {0} kapsayıcı adı geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalı ve yalnızca küçük harfli alfasayısal karakterler ve kısa çizgi içerebilir. Kısa çizgi öncesinde ve sonra alfasayısal bir karakter gelmelidir.  |
|  Geçersiz parametre  |  URL {1} {0} kapsayıcı adı geçersiz. Kapsayıcı adları 3-63 karakter uzunluğunda olmalı ve yalnızca küçük harfli alfasayısal karakterler ve kısa çizgi içerebilir. Kısa çizgi öncesinde ve sonra alfasayısal bir karakter gelmelidir.  |
|  Geçersiz parametre  |  URL 'deki blob adı {0} eğik çizgi içeriyor. Bu, şu anda diskler için desteklenmez.  |
|  Geçersiz parametre  |  URI {0} doğru blob URI 'SI olarak görünmüyor.  |
|  Geçersiz parametre  |  '{0}' adlı bir disk zaten aynı LUN 'U kullanıyor: {1}.  |
|  Geçersiz parametre  |  '{0}' adlı bir disk zaten var.  |
|  Geçersiz parametre  |  Belirtilen görüntü başvurusunda zaten tanımlı olan bir disk için Kullanıcı görüntüsü geçersiz kılmaları belirtilemez.  |
|  Geçersiz parametre  |  '{0}' adlı bir disk zaten aynı VHD URL 'sini {1}kullanıyor.  |
|  Geçersiz parametre  |  Belirtilen hata etki alanı sayısı {0} {2}için {1} aralığına denk gelmelidir.  |
|  Geçersiz parametre  |  {0} lisans türü geçersiz. Geçerli lisans türleri şunlardır: Windows_Client veya Windows_Server, büyük/küçük harfe duyarlı.  |
|  Geçersiz parametre  |  Linux ana bilgisayar adı {0} karakterden uzun olamaz veya şu karakterleri içeremez: {1}.  |
|  Geçersiz parametre  |  Linux sağlama aracısındaki bilinen bir sorun nedeniyle, SSH ortak anahtarları için hedef yol şu anda varsayılan değeriyle sınırlıdır {0}.  |
|  Geçersiz parametre  |  LUN {0} bir disk zaten var.  |
|  Geçersiz parametre  |  İsteğin abonelik {0}, yönetilen disk kimliğinde bulunan abonelikle {1} eşleşmelidir.  |
|  Geçersiz parametre  |  OSProfile içindeki özel verilerin Base64 kodlaması ve en fazla {0} karakter uzunluğunda olması gerekir.  |
|  Geçersiz parametre  |  URL {0} blob adı '{1}' uzantısıyla bitmelidir.  |
|  Geçersiz parametre  |  {0}', geçerli bir yakalanan VHD blob adı ön eki değil. Geçerli bir önek, Regex '{1}' ile eşleşiyor.  |
|  Geçersiz parametre  |  VM Aracısı sağlanmadıysa sanal makinenize sertifikalar eklenemiyor.  |
|  Geçersiz parametre  |  LUN {0} bir disk zaten var.  |
|  Geçersiz parametre  |  İstenen boyut {0}, kullanılabilirlik kümesinin Şu anda ayrıldığı kümede kullanılamadığından VM oluşturulamıyor. Kullanılabilir Boyutlar: {1}. https://aka.ms/azure-resizevm adresindeki VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin.  |
|  Geçersiz parametre  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. Geçerli bölgede bulunan Boyutlar: {1}. https://aka.ms/azure-regions adresindeki her bölgede kullanılabilir VM boyutları hakkında daha fazla bilgi edinin.  |
|  Geçersiz parametre  |  İstenen VM boyutu {0} geçerli bölgede kullanılamıyor. https://aka.ms/azure-regions adresindeki her bölgede kullanılabilir VM boyutları hakkında daha fazla bilgi edinin.  |
|  Geçersiz parametre  |  Windows Yönetici Kullanıcı adı {0} karakterden uzun olamaz, nokta (.) ile bitemez veya şu karakterleri içeremez: {1}.  |
|  Geçersiz parametre  |  Windows bilgisayar adı {0} karakterden uzun olamaz, tamamen rakamlardan oluşamaz veya şu karakterleri içeremez: {1}.  |
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
|  NotFound  |  {0} resim bulunamıyor.  |
|  NotSupported  |  Lisans türü {0}, ancak görüntü blobu {1} şirket içinde değil.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi {0} silinemiyor. Bir kullanılabilirlik kümesini silmeden önce lütfen herhangi bir VM içermediğinden emin olun.  |
|  OperationNotAllowed  |  Kullanılabilirlik kümesi SKU 'SU ' hizalı ' değerinden ' klasik ' öğesine değiştirilmesine izin verilmez.  |
|  OperationNotAllowed  |  VM çalışmadığı zaman VM 'deki uzantılar değiştirilemez.  |
|  OperationNotAllowed  |  Yakalama eylemi yalnızca blob tabanlı disklere sahip bir sanal makinede desteklenir. Yönetilen bir sanal makineden bir görüntü oluşturmak için lütfen ' görüntü ' kaynak API 'Lerini kullanın.  |
|  OperationNotAllowed  |  Görüntü başarıyla oluşturuluncaya kadar, kaynak {0} görüntüden oluşturulamıyor {1}.  |
|  OperationNotAllowed  |  VM ayrıldığında encryptionSettings güncelleştirmelerine izin verilmiyor, lütfen VM serbest bırakıldıktan sonra yeniden deneyin  |
|  OperationNotAllowed  |  Blob tabanlı diskler içeren bir VM 'ye yönetilen disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  Bu boyuttaki bir VM 'ye bağlı olmasına izin verilen en fazla veri diski sayısı {0}.  |
|  OperationNotAllowed  |  Yönetilen disklerle VM 'ye blob tabanlı bir disk eklenmesi desteklenmez.  |
|  OperationNotAllowed  |  '{0}' işlemine '{1}' görüntüsü silinmek üzere işaretlendiğinden bu görüntüde izin verilmiyor. Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  VM Genelleştirilmiş olduğundan '{1}' sanal makinesinde '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  '{1}' geri yükleme noktası koleksiyonu silinmek üzere işaretlendiğinden '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  '{0}' işlemine silinmek üzere işaretlendiğinden '{1}' VM uzantısında izin verilmiyor. Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  '{1}' sanal makineleri '{2}' görüntüsü kullanılarak sağlanmakta olduğundan '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  '{1}' sanal makine ölçek kümesi şu anda '{2}' görüntüsünü kullandığından '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  VM silinmek üzere işaretlendiğinden, '{1}' sanal makinesinde '{0}' işlemine izin verilmiyor. Yalnızca silme işlemini yeniden deneyebilir (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  OperationNotAllowed  |  VM serbest bırakılmış veya serbest bırakılmış olarak işaretlenmiş olduğundan '{1}' sanal makinesinde '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  VM 'nin çalıştığından, '{1}' sanal makinesinde '{0}' işlemine izin verilmiyor. Lütfen VM 'yi konuk işletim sisteminin içinden kapattığınız durumda açık bir şekilde kapatın.  |
|  OperationNotAllowed  |  VM serbest bırakılmadığından, '{1}' sanal makinesinde '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  VM 'nin başarısız durumda '{2}' uzantısı olduğundan '{1}' sanal makinesinde '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  Devam eden başka bir işlem olduğundan, '{1}' sanal makinesinde '{0}' işlemine izin verilmiyor.  |
|  OperationNotAllowed  |  '{0}' işlemi, '{1}' sanal makinesinin genelleştirilmesi gerektiriyor.  |
|  OperationNotAllowed  |  İşlem için VM 'nin çalışıyor olması gerekir (veya çalıştırılacak şekilde ayarlanır).  |
|  OperationNotAllowed  |  Görüntüde karşılık gelen disk {1}boyutundan daha küçük olan {0}GB boyutundaki disk, buna izin verilmez.  |
|  OperationNotAllowed  |  '{0}' işleyicisinin VM Ölçek kümesi uzantıları yalnızca VM Ölçek kümesi oluşturma sırasında eklenebilir.  |
|  OperationNotAllowed  |  '{0}' işleyicisinin VM Ölçek kümesi uzantıları yalnızca VM Ölçek kümesi silme sırasında silinebilir.  |
|  OperationNotAllowed  |  '{0}' VM 'si zaten yönetilen diskler kullanıyor.  |
|  OperationNotAllowed  |  '{0}' VM 'si '{1}' ' klasik ' kullanılabilirlik kümesine aittir. Lütfen kullanılabilirlik kümesini ' hizalı ' SKU 'SU kullanacak şekilde güncelleştirin ve sonra dönüştürmeyi yeniden deneyin.  |
|  OperationNotAllowed  |  Görüntüden oluşturulan VM 'de blob tabanlı diskler olamaz. Tüm disklerin yönetilen diskler olması gerekir.  |
|  OperationNotAllowed  |  VM Genelleştirilmiş olmadığından yakalama işlemi tamamlanamıyor.  |
|  OperationNotAllowed  |  VM diskleri yönetilen disklere dönüştürülemediğinden, '{0}' VM 'sinde yönetim işlemlerine izin verilmiyor.  |
|  OperationNotAllowed  |  Devam eden bir işlem, sanal makine {0} güç durumunu {1}olarak değiştiriyor. Lütfen bir süre sonra {2} işlemi gerçekleştirin.  |
|  OperationNotAllowed  |  VM eklenemiyor veya güncelleştirilemiyor. İstenen VM boyutu {0} var olan ayırma biriminde kullanılamıyor olabilir. https://aka.ms/azure-resizevm adresindeki VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin.  |
|  OperationNotAllowed  |  İstenen boyut {0}, kullanılabilirlik kümesinin Şu anda ayrıldığı kümede kullanılamadığından VM yeniden boyutlandırılamıyor. Kullanılabilir Boyutlar: {1}. https://aka.ms/azure-resizevm adresindeki VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin.  |
|  OperationNotAllowed  |  İstenen boyut {0} VM 'nin şu anda ayrıldığı kümede kullanılamadığından VM yeniden boyutlandırılamıyor. VM 'nizi yeniden boyutlandırmak {1} için lütfen serbest bırakın (Bu işlem Azure portal durdurulur) ve yeniden boyutlandırma işlemini yeniden deneyin. https://aka.ms/azure-resizevm adresindeki VM yeniden boyutlandırma stratejisi hakkında daha fazla bilgi edinin.  |
|  OSProvisioningClientError  |  Konuk işletim sistemi şu anda sağlanmakta olduğundan, '{0}' sanal makinesi için işletim sistemi sağlama başarısız oldu.  |
|  OSProvisioningClientError  |  '{0}' sanal makinesi için işletim sistemi sağlama başarısız oldu. Hata ayrıntıları: {1} görüntünün düzgün hazırlanmış (Genelleştirilmiş) olduğundan emin olun. <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  SSH ana bilgisayar anahtarı oluşturulamadı. Hata ayrıntıları: {0}. Bu sorunu gidermek için, Linux aracısının düzgün ayarlanıp ayarlanmadığını doğrulayın. <ul><li>Şu adreste bulunan yönergeleri denetleyebilirsiniz: https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux/ </li></ul> |
|  OSProvisioningClientError  |  VM için belirtilen Kullanıcı adı bu Linux dağıtımı için geçersiz. Hata ayrıntıları: {0}.  |
|  Osprovisioningınternalerror  |  Bir iç hata nedeniyle '{0}' VM 'si için işletim sistemi sağlama başarısız oldu.  |
|  Osprovisioningtime,  |  '{0}' VM 'si için işletim sistemi sağlama, ayrılan sürede tamamlanmadı. VM sağlamayı başarıyla bitirirebilir. Lütfen sağlama durumunu daha sonra denetleyin.  |
|  Osprovisioningtime,  |  '{0}' VM 'si için işletim sistemi sağlama, ayrılan sürede tamamlanmadı. VM sağlamayı başarıyla bitirirebilir. Lütfen sağlama durumunu daha sonra denetleyin. Ayrıca, görüntünün düzgün hazırlanmış (Genelleştirilmiş) olduğundan emin olun.   <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  Osprovisioningtime,  |  '{0}' VM 'si için işletim sistemi sağlama, ayrılan sürede tamamlanmadı. Ancak, VM Konuk Aracısı çalışıyor olarak algılandı. Bu, Konuk işletim sisteminin bir VM görüntüsü (CreateOption = FromImage) olarak kullanılmak üzere düzgün şekilde hazırlandığını önerir. Bu sorunu çözmek için, VHD 'yi CreateOption = Attach ile birlikte kullanın veya görüntü olarak kullanmak üzere doğru şekilde hazırlayın:   <ul><li>Windows için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için yönergeler: https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Gerekli VM boyutu şu anda seçili konumda kullanılamıyor.  |
|  ResourceUpdateBlockedOnPlatformUpdate  |  Devam eden platform güncelleştirmesi nedeniyle kaynak şu anda güncelleştirilemiyor. Lütfen daha sonra tekrar deneyin.  |
|  Storageaccountsınırlamanın  |  '{0}' depolama hesabı, disk oluşturmak için gerekli olan sayfa bloblarını desteklemiyor.  |
|  Storageaccountsınırlamanın  |  '{0}' depolama hesabı, ayrılmış kotasını aştı.  |
|  Storageaccountlocationuyuşmazlığıdır  |  {0}depolama hesabı çözümlenemedi. Lütfen işlem kaynağıyla aynı konumdaki depolama kaynak sağlayıcısı üzerinden oluşturulduğundan emin olun.  |
|  StorageAccountNotFound  |  {0} depolama hesabı bulunamadı. Depolama hesabının silinmediğinden ve VM ile aynı Azure konumuna ait olduğundan emin olun.  |
|  Storageaccountnottanınmış  |  Lütfen depolama kaynak sağlayıcısı tarafından yönetilen bir depolama hesabı kullanın. {0} kullanımı desteklenmez.  |
|  Storageaccountoperationınternalerror  |  {0}depolama hesabına erişilirken iç hata oluştu.  |
|  Storageaccountsubscriptionuyuşmazlığı  |  {0} depolama hesabı {1}aboneliğe ait değil.  |
|  StorageAccountTooBusy  |  '{0}' depolama hesabı şu anda çok meşgul. Başka bir hesap kullanmayı düşünün.  |
|  StorageAccountTypeNotSupported  |  Disk {0} bir BLOB depolama hesabı olan {1} kullanır. Lütfen genel amaçlı depolama hesabı ile yeniden deneyin.  |
|  StorageAccountTypeNotSupported  |  Depolama hesabı {0} {1} türüdür. Önyükleme tanılaması {2} depolama hesabı türlerini destekler.  <ul><li>Bu hata, önyükleme tanılaması için Premium depolama hesabı kullanırsanız oluşur. Daha fazla bilgi için bkz. [önyükleme tanılamayı kullanma](boot-diagnostics.md). </li></ul> |
|  SubscriptionNotAuthorizedForImage  |  Abonelik yetkili değil.  |
|  TargetDiskBlobAlreadyExists var  |  Blob {0} zaten var. Lütfen yeni boş bir '{1}' veri diski oluşturmak için farklı bir blob URI 'SI sağlayın.  |
|  TargetDiskBlobAlreadyExists var  |  Hedef görüntü blobu {0} zaten varolduğundan ve VHD bloblarının üzerine yazılacak bayrak ayarlanmadığından yakalama işlemi devam edemiyor. Blobu silin ya da bayrağı VHD bloblarının üzerine yazacak şekilde ayarlayın ve yeniden deneyin.  |
|  TargetDiskBlobAlreadyExists var  |  Hedef görüntü blobu {0} üzerinde etkin bir kiralama olduğundan yakalama işlemi devam edemiyor.   |
|  TargetDiskBlobAlreadyExists var  |  Blob {0} zaten var. Lütfen '{1}' diski için hedef olarak farklı bir blob URI 'SI sağlayın.  |
|  TooManyVMRedeploymentRequests  |  VM '{0}' veya bu VM ile aynı Kullanılabilirlik kümesindeki VM 'Ler için çok fazla yeniden dağıtım isteği alındı. Lütfen daha sonra yeniden deneyin.  |
|  Vhdsizegeçersiz  |  Blob {2} ile '{1}' diski için {0} belirtilen disk boyutu değeri geçersiz. Disk boyutu {3} ile {4}arasında olmalıdır.  |
|  VMAgentStatusCommunicationError  |  VM '{0}' VM Aracısı veya uzantıları için durum bildirmedi. Lütfen VM 'nin çalışan bir VM aracısına sahip olduğunu ve Azure depolama 'ya giden bağlantılar kurabildiğini doğrulayın.  |
|  Vmartifactrepositoryınternalerror  |  VM yapıt ayrıntılarını almak için yapıt deposu ile iletişim kurulurken bir hata oluştu.  |
|  Vmartifactrepositoryınternalerror  |  Yapıt deposundan VM yapıt verileri alınırken bir iç hata oluştu.  |
|  Vmextensionhandlernongeçişli Enterror  |  '{0}' işleyicisi, '{1}' VM uzantısı için '{2}' Terminal hata kodu ve '{3}' hata iletisiyle ilgili bir hata bildirdi.  |
|  Vmextensionmanagementınternalerror  |  '{0}' sanal makine uzantısı işlenirken iç hata oluştu.  |
|  Vmextensionmanagementınternalerror  |  VM uzantıları hazırlanırken birden çok hata oluştu. Ayrıntılar için bkz. VM uzantı örneği görünümü.  |
|  VMExtensionProvisioningError  |  VM, '{0}' uzantısını işlerken bir hata bildirdi. Hata iletisi: "{1}".  |
|  VMExtensionProvisioningError  |  VM 'de birden çok VM Uzantısı sağlanamadı. Ayrıntılar için lütfen VM uzantısı örnek görünümüne bakın.  |
|  VMExtensionProvisioningTimeout  |  '{0}' VM uzantısının sağlanması zaman aşımına uğradı. Uzantı yüklemesi çok uzun sürüyor veya uzantı durumu elde edilemedi.  |
|  Vmmarketplaceınvalidınput  |  Market olmayan bir görüntüden sanal makine oluşturmak için plan bilgileri gerekmez, lütfen istekteki plan bilgilerini kaldırın. İşletim sistemi diski adı {0}.  |
|  Vmmarketplaceınvalidınput  |  Satın alma bilgileri eşleşmiyor. Market görüntüsünden dağıtım yapılamıyor. İşletim sistemi diski adı {0}.  |
|  Vmmarketplaceınvalidınput  |  Market görüntüsünden bir sanal makine oluşturmak istekte plan bilgileri gerektirir. İşletim sistemi diski adı {0}.  |
|  VMNotFound  |  '{0}' VM 'si bulunamıyor.  |
|  VMRedeploymentFailed  |  VM '{0}' yeniden dağıtımı bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin.  |
|  Vmredeploymenttimeın  |  '{0}' VM 'sinin yeniden dağıtımı ayrılan sürede bitmedi. Bir süre içinde başarıyla bitebilirler. Aksi takdirde, isteği yeniden deneyebilirsiniz.  |
|  Vmstarttimepr  |  '{0}' VM 'si ayrılan süre içinde başlatılmadı. VM hala başarıyla başlayabilir. Lütfen daha sonra güç durumunu kontrol edin.  |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Stack Overflow forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişim kurun. Alternatif olarak, bir Azure destek olayına dosya. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
