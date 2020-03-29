---
title: Azure'da yaygın VM hata kodları | Microsoft Dokümanlar
description: Azure'da sanal makineleri sağlamave yönetmede karşılaşılan yaygın hata kodlarından bazılarını anlayın
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76543027"
---
# <a name="understand-common-error-messages-when-you-manage-virtual-machines-in-azure"></a>Azure'da sanal makineleri yönetirken sık karşılaşılan hata iletilerini anlama

Bu makalede, Azure'da sanal makineler (VM' ler) oluştururken veya yönetirken karşılaşabileceğiniz en yaygın hata kodlarından ve iletilerinden bazıları açıklanmaktadır.

>[!NOTE]
> Bu sayfaya geri bildirim için veya [Azure geri bildirimi](https://feedback.azure.com/forums/216843-virtual-machines) aracılığıyla #azerrormessage etiketiyle yorum bırakabilirsiniz.

## <a name="error-response-format"></a>Hata Yanıt Biçimi 
Azure VM'ler hata yanıtı için aşağıdaki JSON biçimini kullanır:

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

Hata yanıtı her zaman bir durum kodu ve bir hata nesnesi içerir. Her hata nesnesi her zaman bir hata kodu ve bir ileti içerir. VM bir şablonla oluşturulursa, hata nesnesi ayrıca hata kodları ve iletinin iç düzeyini içeren bir ayrıntı bölümü de içerir. Normalde, hata iletisinin en iç düzeyi kök hatasıdır.


## <a name="common-virtual-machine-management-errors"></a>Yaygın sanal makine yönetimi hataları

Bu bölümde, VM'leri yönetirken karşılaşabileceğiniz yaygın hata iletileri listelenebilir:

|  Hata Kodu  |  Hata İletisi  |  
|  :------| :-------------|  
|  EdinmeDiskLeaseFailed  |  URI {1}ile blob kullanarak{0}disk ' ' oluştururken kira elde etmek için başarısız oldu. Blob zaten kullanımda.  |  
|  Tahsisat Başarısız Oldu  |  Ayırma başarısız oldu. Lütfen VM boyutunu veya VM sayısını azaltmayı deneyin, daha sonra yeniden deneyin veya farklı bir Kullanılabilirlik Kümesi'ne veya farklı Azure konumuna dağıtmayı deneyin.  |  
|  Tahsisat Başarısız Oldu  |  VM ayırma bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra yeniden deneyin veya farklı bir konuma dağıtmayı deneyin.  |
|  Eser Bulunamadı  |  Yayımcı ' '{0}ve type{1}' ' ' '{2}ile VM uzantısı ' ' konumunda bulunamadı.  |
|  Eser Bulunamadı  |  Uzantılı{0}uzantı '{1}', type '{2}' ve type işleyici sürümü ' ' uzantısı deposunda bulunamadı.  |
|  ArtefaktSürümBulunamadı  |  Talep edilen sürümü tatmin eden yapı deposunda hiçbir sürüm{0}bulunamadı.  |
|  ArtefaktSürümBulunamadı  |  ' ve ' type{0}{1}{2}' ile VM uzantısı için istenen sürümü ' ' tatmin eden yapı deposunda hiçbir sürüm bulunamadı.  |
|  AttachDiskWhileBeingDetached  |  Disk şu anda ayrıştırıldığı için VM '{0}{1}e ' veri diski ' ' ekleyemez. Lütfen disk tamamen kopana kadar bekleyin ve sonra yeniden deneyin.  |
|  BadRequest  |  Aligned' Kullanılabilirlik Kümeleri henüz bu bölgede desteklenmedi.  |
|  BadRequest  |  Yönetilen disklerle yönetilen disklerle vm eklenmesi yönetilen Kullanılabilirlik Kümesi'ne veya blob tabanlı disklere sahip bir VM'nin yönetilen Kullanılabilirlik Kümesi'ne eklenmesi desteklenmez. Lütfen, yönetilen diskleri olan bir VM eklemek için 'yönetilen' özellik kümesini içeren bir Kullanılabilirlik Kümesi oluşturun.  |
|  BadRequest  |  Yönetilen Diskler bu bölgede desteklenmez.  |
|  BadRequest  |  İşleyici başına birden çok VMExtensions os{0}türü ' için desteklenmeyen. VMExtension{1}' ' ile{2}işleyici ' ' zaten girdide eklenen veya belirtilen.  |
|  BadRequest  |  ''{0}işlemi Yönetilen disklerle{1}Kaynak ' ' üzerinde desteklenmez.  |
|  Sertifika Yanlış Biçimlendirilmiş  |  Alınan sırrın JSON gösterimi {0} düzgün biçimlendirilmiş pfx dosyası olmayan bir veri alanına sahiptir veya sağlanan parola PFX dosyasını doğru şekilde çözmüyor.  |
|  Sertifika Yanlış Biçimlendirilmiş  |  Alınan veriler {0} JSON'a aktarılamaz.  |
|  Çakışma  |  Disk yeniden boyutlandırmaya yalnızca VM oluşturulurken veya VM tahsis edildiğinde izin verilir.  |
|  ÇakışanUserInput  |  Disk{0}' ' ' ' disk zaten VM{1}' 'aağasahip olduğu için eklenemez.  |
|  ÇakışanUserInput  |  Kaynak ve hedef kaynak grupları aynıdır.  |
|  ÇakışanUserInput  |  Diskin {0} kaynak ve hedef depolama hesapları farklıdır.  |
|  KonteynerAlreadyOnLease  |  Uri {0}ile blob tutan depolama konteyner için de kira sırada kira sırada bir kira sürülü  |
|  CrossSubscriptionMoveWithKeyVaultResources  |  Kaynakları Taşı isteği, istekte bir veya daha {0}fazla s tarafından başvurulan KeyVault kaynaklarını içerir. Bu, şu anda Cross aboneliği Taşı'nda desteklenmez. Lütfen KeyVault kaynak kimlikleri için hata ayrıntılarını kontrol edin.  |
|  DiagnosticsOperationInternalError  |  VM {0}tanılama profili işlenirken bir iç hata oluştu.  |
|  DiskBlobAlreadyInUseByAnotherDisk  |  Blob {0} zaten VM '{1}'a ait başka bir disk tarafından kullanılmaktadır. Disk başvuru bilgileri için blob meta verilerini inceleyebilirsiniz.  |
|  DiskBlobNotFound  |  Disk için URI {0} ile VHD blob bulamıyor '{1}.  |
|  DiskBlobNotFound  |  URI {0}ile VHD blob bulamıyor.  |
|  DiskEncryptionKeySecretMissingEtiketler  |  {0}Gizli nin etiketleri {1} yok. Lütfen gizli sürümü güncelleyin, gerekli etiketleri ekleyin ve yeniden deneyin.  |
|  DiskEncryptionKeySecretUnwrapFailed  |  Anahtar {1} kullanarak {0} gizli değerin açılamaması başarısız oldu.  |
|  DiskImageNotReady  |  Disk {0} görüntüsü {1} durumda. Lütfen görüntü hazır olduğunda yeniden deneyin.  |
|  Disk Hazırlama Hatası  |  VM diskleri hazırlanırken bir veya daha fazla hata oluştu. Ayrıntılar için disk örneği görünümüne bakın.  |
|  DiskProcessingError  |  VM başarısız disklerde diğer diskler olduğu gibi disk işleme durduruldu.  |
|  ResimBlobNotFound  |  Disk için URI {0} ile VHD blob bulamıyor '{1}.  |
|  ResimBlobNotFound  |  URI {0}ile VHD blob bulamıyor.  |
|  YanlışDiskBlobType  |  Disk lekeleri yalnızca tür sayfa blob olabilir. Blob {0} for{1}disk ' ' türü blok blob'dur.  |
|  YanlışDiskBlobType  |  Disk lekeleri yalnızca tür sayfa blob olabilir. Blob {0} tip '{1}'.  |
|  YanlışGörüntüBlobType  |  Disk lekeleri yalnızca tür sayfa blob olabilir. Blob {0} for{1}disk ' ' türü blok blob'dur.  |
|  YanlışGörüntüBlobType  |  Disk lekeleri yalnızca tür sayfa blob olabilir. Blob {0} tip '{1}'.  |
|  InternalOperationError  |  Depolama hesabı {0}çözemedi. Lütfen bilgi işlem kaynağıyla aynı konumda Depolama Kaynak Sağlayıcısı aracılığıyla oluşturulduğundan emin olun.  |
|  InternalOperationError  |  {0}hedef arayan görevler başarısız oldu.  |
|  InternalOperationError  |  Hata VM{0}' in ağ profilini doğrulama oluştu.  |
|  Geçersiz Hesap Türü  |  AccountType {0} geçersizdir.  |
|  GeçersizParametre  |  Parametrenin {0} değeri geçersizdir.  |
|  GeçersizParametre  |  Belirtilen Yönetici parolasına izin verilmez.  |
|  GeçersizParametre  |  "Sağlanan parola arasında {0}olmalıdır{1} - karakterler uzun ve {2} aşağıdaki parola karmaşıklığı gereksinimleri en az karşılamak gerekir: <ol><li> Büyük harfli bir karakter içerir</li><li>Küçük bir karakter içerir</li><li>Sayısal basamak içerir</li><li>Özel bir karakter içerir.</li></ol>  |
|  GeçersizParametre  |  Belirtilen Yönetici Kullanıcı Adı'na izin verilmez.  |
|  GeçersizParametre  |  VM bir platformdan veya kullanıcı görüntüsünden oluşturulmuşsa varolan bir işletim sistemi diski eklenemez.  |
|  GeçersizParametre  |  Kapsayıcı {0} adı geçersizdir. Kapsayıcı adları uzunluğu 3-63 karakter olmalıdır ve yalnızca küçük harf alfasayısal karakterler ve tire içerebilir. Tire önce ve alfasayısal bir karakter takip edilmelidir.  |
|  GeçersizParametre  |  URL'deki {0} {1} kapsayıcı adı geçersizdir. Kapsayıcı adları uzunluğu 3-63 karakter olmalıdır ve yalnızca küçük harf alfasayısal karakterler ve tire içerebilir. Tire önce ve alfasayısal bir karakter takip edilmelidir.  |
|  GeçersizParametre  |  URL'deki {0} blob adı bir eğik çizgi içerir. Bu, şu anda diskler için desteklenmez.  |
|  GeçersizParametre  |  URI {0} doğru blob URI olarak görünmüyor.  |
|  GeçersizParametre  |  ' '{0}adlı bir disk zaten {1}aynı LUN kullanır: .  |
|  GeçersizParametre  |  ' '{0}adlı bir disk zaten var.  |
|  GeçersizParametre  |  Belirtilen görüntü başvurusunda zaten tanımlanmış bir disk için kullanıcı görüntüsünü geçersiz kılar belirtemez.  |
|  GeçersizParametre  |  ' '{0}adlı bir disk zaten {1}aynı VHD URL'sini kullanır.  |
|  GeçersizParametre  |  Belirtilen hata etki {0} alanı sayısı {1} aralıkta düşmesi {2}gerekir.  |
|  GeçersizParametre  |  Lisans türü {0} geçersizdir. Geçerli lisans türleri şunlardır: Windows_Client veya Windows_Server, servis talebine duyarlıdır.  |
|  GeçersizParametre  |  Linux ana bilgisayar {0} adı uzunluk karakterleri geçemez veya {1}aşağıdaki karakterleri içeremez: .  |
|  GeçersizParametre  |  Ssh ortak anahtarları için hedef yolu şu {0} anda Linux sağlama aracısında bilinen bir sorun nedeniyle varsayılan değeri ile sınırlıdır.  |
|  GeçersizParametre  |  LUN'da {0} bir disk zaten var.  |
|  GeçersizParametre  |  İsteğin aboneliği, {0} yönetilen {1} disk kimliğinde bulunan abonelikle eşleşmelidir.  |
|  GeçersizParametre  |  OSProfile'daki özel veriler Base64 kodlamasında ve maksimum {0} karakter uzunluğuna sahip olmalıdır.  |
|  GeçersizParametre  |  URL'deki {0} Blob adı{1}' ' uzantısı ile bitmelidir.  |
|  GeçersizParametre  |  {0}' geçerli bir yakalanan VHD blob adı öneki değildir. Geçerli bir önek regex ' 'eşleşir.{1}  |
|  GeçersizParametre  |  VM aracısı sağlanmıyorsa, Sertifikalar VM'nize eklenemez.  |
|  GeçersizParametre  |  LUN'da {0} bir disk zaten var.  |
|  GeçersizParametre  |  İstenen boyut, {0} kullanılabilirlik kümesinin şu anda tahsis edildiği kümede kullanılamadığı için VM oluşturulamıyor. Kullanılabilir boyutlar şunlardır: {1}. VM yeniden boyutlandırma stratejisi https://aka.ms/azure-resizevmhakkında daha fazla bilgi edinin.  |
|  GeçersizParametre  |  İstenen VM {0} boyutu geçerli bölgede kullanılamıyor. Geçerli bölgede bulunan boyutlar şunlardır: {1}. Her bölgedeki mevcut VM boyutları hakkında daha https://aka.ms/azure-regionsfazla bilgi edinin.  |
|  GeçersizParametre  |  İstenen VM {0} boyutu geçerli bölgede kullanılamıyor. Her bölgedeki mevcut VM boyutları hakkında daha https://aka.ms/azure-regionsfazla bilgi edinin.  |
|  GeçersizParametre  |  Windows admin kullanıcı adı {0} karakter den fazla uzun olamaz, bir dönem (.) {1}ile sona erer veya aşağıdaki karakterleri içeremez: .  |
|  GeçersizParametre  |  Windows bilgisayar adı uzun {0} karakterlerden fazla olamaz, tamamen sayısal olamaz veya {1}aşağıdaki karakterleri içeremez: .  |
|  EksikTaşıma Bağımlı Kaynaklar  |  Taşıma kaynakları isteği tüm bağımlı kaynakları içermez. Lütfen eksik kaynak kimlikleri için hata ayrıntılarını kontrol edin.  |
|  MoveResourcesHaveInvalidState  |  Kaynakları Taşı isteği, geçersiz depolama hesaplarıyla ilişkili VM'ler içerir. Lütfen bu kaynak kimlikleri ve başvurulan depolama hesabı adlarının ayrıntılarını kontrol edin.  |
|  MoveResourcesHavependingOperations  |  Taşıma kaynakları isteği, bir işlemin beklemede olduğu kaynakları içerir. Lütfen bu kaynak kimlikleri için ayrıntıları kontrol edin. Bekleyen işlemler tamamlandıktan sonra operasyonunuzu yeniden deneyin.  |
|  Taşınan KaynaklarBulunamadı  |  Taşıma kaynakları isteği bulunamayan kaynakları içerir. Lütfen bu kaynak kimlikleri için ayrıntıları kontrol edin.  |
|  AğInternalOperationError  |  Bilinmeyen ağ ayırma hatası.  |
|  AğInternalOperationError  |  Bilinmeyen ağ ayırma hatası  |
|  AğInternalOperationError  |  VM'nin işleme ağ profilinde bir iç hata oluştu.  |
|  NotFound  |  Kullanılabilirlik {0} Kümesi bulunamıyor.  |
|  NotFound  |  Kaynak Sanal{0}Makine ' ' belirtilen istek bu Azure konumunda bulunmamaktadır.  |
|  NotFound  |  Kimliği {0} bulunamayan kiracı.  |
|  NotFound  |  Görüntü {0} bulunamıyor.  |
|  Notsupported  |  Lisans {0}türü, ancak görüntü blob {1} şirket içinde değil.  |
|  İşleme İzin Verilmedi  |  Kullanılabilirlik {0} Kümesi silinemez. Kullanılabilirlik Kümesini silmeden önce lütfen VM içermediğinden emin olun.  |
|  İşleme İzin Verilmedi  |  Kullanılabilirlik kümesiSKU'nun 'Hizalanmış'dan 'Klasik'e değiştirilmesine izin verilmez.  |
|  İşleme İzin Verilmedi  |  VM çalışmıyorsa VM uzantılarını değiştiremezsiniz.  |
|  İşleme İzin Verilmedi  |  Yakalama eylemi yalnızca blob tabanlı disklere sahip sanal makinede desteklenir. Yönetilen bir Sanal Makine'den görüntü oluşturmak için lütfen 'Resim' kaynak API'lerini kullanın.  |
|  İşleme İzin Verilmedi  |  Kaynak, {0} Görüntü başarıyla {1} oluşturulana kadar Görüntü'den oluşturulamaz.  |
|  İşleme İzin Verilmedi  |  Şifreleme Ayarlarında güncellemeler VM tahsis edildiğinde izin verilmez, Lütfen VM ayrıldıktan sonra yeniden deneyin  |
|  İşleme İzin Verilmedi  |  Blob tabanlı diskler ile vm yönetilen bir disk eklenmesi desteklenmez.  |
|  İşleme İzin Verilmedi  |  Bu boyuttaki bir VM'ye eklenmesine izin verilen maksimum {0}veri diski sayısı.  |
|  İşleme İzin Verilmedi  |  Yönetilen disklerle VM'ye blob tabanlı bir disk eklenmesi desteklenmez.  |
|  İşleme İzin Verilmedi  |  Görüntü{0}silinmek için işaretli{1}olduğundan Görüntü ' ' nde ' ' işlemine izin verilmez. Yalnızca Silme işlemini yeniden deneyebilirsiniz (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  İşleme İzin Verilmedi  |  VM{0}genelleştirilediğinden VM{1}' de ' ' işlemine izin verilmez.  |
|  İşleme İzin Verilmedi  |  {0}Geri yükleme noktası koleksiyonu '{1}olarak ' işlemine izin verilmez' silinmek için işaretlenir.  |
|  İşleme İzin Verilmedi  |  İşlem{0}' ' ' ' vm uzantısı '{1}' ' ' silme için işaretli olduğundan izin verilmez. Yalnızca Silme işlemini yeniden deneyebilirsiniz (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  İşleme İzin Verilmedi  |  Sanal{0}Makineler{1}' ' Resmi{2}' kullanılarak sağlandığı için ' ' işlemine izin verilmemektedir.  |
|  İşleme İzin Verilmedi  |  Sanal{0}Makine ScaleSet '{1}' ' şu anda Görüntü{2}' 'yi kullandığından' işlemine izin verilmemektedir.  |
|  İşleme İzin Verilmedi  |  {0}VM silinmek için işaretli{1}olduğundan VM ' de ' işlemine izin verilmez. Yalnızca Silme işlemini yeniden deneyebilirsiniz (veya devam eden bir işlemin tamamlanmasını bekleyebilirsiniz).  |
|  İşleme İzin Verilmedi  |  {0}VM'nin ayrılması veya ayrılması{1}için işaretli olması nedeniyle VM ' de işlem ' ' ne izin verilmez.  |
|  İşleme İzin Verilmedi  |  VM{0}çalışırken VM '{1}' de işlem etüt edilmiyor. VM'yi konuk işletim sisteminin içinden kapatmanız durumunda lütfen açık bir şekilde kapatın.  |
|  İşleme İzin Verilmedi  |  {0}VM'nin ayrılması olmadığı için{1}VM ' de operasyona izin verilmez.  |
|  İşleme İzin Verilmedi  |  VM'de{0}{1}' ' ' ' ' '{2}işlemine izin verilmez, çünkü VM'nin uzantısı ' ' başarısız durumdadır.  |
|  İşleme İzin Verilmedi  |  Başka{0}bir işlem devam ettiği{1}için VM ' de ' işlemine izin verilmez.  |
|  İşleme İzin Verilmedi  |  ' '{0}' işlemi,{1}Sanal Makine ' nin Genelleştirilmesini gerektirir.  |
|  İşleme İzin Verilmedi  |  İşlem, VM'nin çalışmasını (veya çalışacak şekilde ayarlansın) gerektirir.  |
|  İşleme İzin Verilmedi  |  Resim'deki {0}ilgili diskin GB {1}boyutundan daha küçük olan GB boyutuna sahip diske izin verilmez.  |
|  İşleme İzin Verilmedi  |  VM Ölçeği Kümesi işleyici{0}' ' uzantıları yalnızca VM Ölçek Kümesi oluşturma sırasında eklenebilir.  |
|  İşleme İzin Verilmedi  |  VM Ölçeği Ayar uzantıları işleyici ' ' '{0}yalnızca VM Ölçeği Kümesi silme sırasında silinebilir.  |
|  İşleme İzin Verilmedi  |  VM{0}' ' zaten yönetilen diskler kullanıyor.  |
|  İşleme İzin Verilmedi  |  VM{0}' ' 'Klasik' kullanılabilirlik{1}seti ' 'ait. Lütfen 'Hizalanmış' SKU'yu kullanmak ve ardından Dönüşüm'ü yeniden denemek için kullanılabilirlik ayarını güncelleyin.  |
|  İşleme İzin Verilmedi  |  Image'dan oluşturulan VM'de blob tabanlı diskler olamaz. Tüm diskler diskyönetilmelidir.  |
|  İşleme İzin Verilmedi  |  VM genelleştirilmediği için yakalama işlemi tamamlanamaz.  |
|  İşleme İzin Verilmedi  |  VM diskleri{0}yönetilen disklere dönüştürüldüğü için VM ' ' üzerindeki yönetim işlemlerine izin verilmez.  |
|  İşleme İzin Verilmedi  |  Devam eden bir işlem Sanal {0} Makine'nin güç durumunu {1}. Lütfen bir {2} süre sonra operasyon gerçekleştirin.  |
|  İşleme İzin Verilmedi  |  VM eklenemiyor veya güncelleştiremiyor. İstenen VM {0} boyutu varolan ayırma biriminde kullanılamayabilir. VM yeniden boyutlandırma stratejisi https://aka.ms/azure-resizevmhakkında daha fazla bilgi edinin.  |
|  İşleme İzin Verilmedi  |  İstenen boyut, {0} kullanılabilirlik kümesinin şu anda tahsis edildiği kümede kullanılamadığı için VM yeniden boyutlandırılamıyor. Kullanılabilir boyutlar şunlardır: {1}. VM yeniden boyutlandırma stratejisi https://aka.ms/azure-resizevmhakkında daha fazla bilgi edinin.  |
|  İşleme İzin Verilmedi  |  İstenen boyut {0} VM'nin şu anda tahsis edildiği kümede kullanılamadığından VM yeniden boyutlandırılamıyor. Lütfen (bu Azure {1} portalındaki Çalışmayı Durdur) ve yeniden boyutlandırma işlemini yeniden denemek için VM'nizi yeniden boyutlandırmak için. VM yeniden boyutlandırma stratejisi https://aka.ms/azure-resizevmhakkında daha fazla bilgi edinin.  |
|  OSProvisioningClientError  |  Konuk işletim sistemi şu{0}anda sağlandığı için İşletim Sistemi Sağlama VM ' ' için başarısız oldu.  |
|  OSProvisioningClientError  |  VM ' '{0}için işletim sistemi sağlama başarısız oldu. Hata ayrıntıları: {1} Görüntünün düzgün şekilde hazırlandığından (genelleştirilmiş) olduğundan emin olun. <ul><li>Windows için talimatlar:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/  </li></ul> |
|  OSProvisioningClientError  |  SSH ana bilgisayar anahtar üretimi başarısız oldu. Hata ayrıntıları: {0}. Bu sorunu gidermek için Linux aracısı düzgün ayarlanmış olup olmadığını doğrulayın. <ul><li>Talimatları aşağıdaki noktadan kontrol edebilirsiniz:https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux/ </li></ul> |
|  OSProvisioningClientError  |  VM için belirtilen kullanıcı adı bu Linux dağıtımı için geçersizdir. Hata ayrıntıları: {0}.  |
|  OSProvisioningInternalError  |  İşletim Sistemi Sağlama,{0}bir iç hata nedeniyle VM ' ' için başarısız oldu.  |
|  OSProvisioningTimedOut  |  VM ' '{0}için İşletim Sistemi Sağlama, ayrılan süre içinde bitiremedi. VM yine de sağlamayı başarıyla tamamlayabilir. Lütfen daha sonra sağlama durumunu kontrol edin.  |
|  OSProvisioningTimedOut  |  VM ' '{0}için İşletim Sistemi Sağlama, ayrılan süre içinde bitiremedi. VM yine de sağlamayı başarıyla tamamlayabilir. Lütfen daha sonra sağlama durumunu kontrol edin. Ayrıca, görüntünün düzgün bir şekilde hazırlandığından (genelleştirilmiş) olduğundan emin olun.   <ul><li>Windows için talimatlar:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için talimatlar:https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OSProvisioningTimedOut  |  VM ' '{0}için İşletim Sistemi Sağlama, ayrılan süre içinde bitiremedi. Ancak, VM konuk aracı çalışırken algılandı. Bu, konuk işletim sistemi'nin VM görüntüsü olarak (CreateOption=FromImage ile) düzgün bir şekilde kullanılmak üzere hazırlanmadığını gösterir. Bu sorunu gidermek için, CreateOption=Attach'ta olduğu gibi VHD'yi kullanın veya görüntü olarak kullanmak üzere düzgün bir şekilde hazırlayın:   <ul><li>Windows için talimatlar:https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/ </li><li> Linux için talimatlar:https://azure.microsoft.com/documentation/articles/virtual-machines-linux-capture-image/</li></ul>  |
|  OverConstrainedAllocationRequest  |  Gerekli VM boyutu şu anda seçili konumda kullanılamıyor.  |
|  KaynakUpdateBlockedOnPlatformUpdate  |  Devam eden platform güncelleştirmesi nedeniyle kaynak şu anda güncelleştirilemez. Lütfen daha sonra tekrar deneyin.  |
|  Depolama Hesap Sınırlaması  |  Depolama hesabı{0}' ' ' disk oluşturmak için gerekli olan sayfa blobs desteklemez.  |
|  Depolama Hesap Sınırlaması  |  Depolama hesabı{0}' ' ' tahsis edilen kotayı aştı.  |
|  DepolamaHesapLocationMismatch  |  Depolama hesabı {0}çözemedi. Lütfen bilgi işlem kaynağıyla aynı konumda Depolama Kaynak Sağlayıcısı aracılığıyla oluşturulduğundan emin olun.  |
|  DepoHesabı Bulunamadı  |  Depolama {0} hesabı bulunamadı. Depolama hesabının silinmediğinden ve VM ile aynı Azure konumuna ait olduğundan emin olun.  |
|  StorageAccount Tanınmadı  |  Lütfen Depolama Kaynak Sağlayıcısı tarafından yönetilen bir depolama hesabı kullanın. {0} Kullanımı desteklenmez.  |
|  StorageAccountOperationInternalError  |  Depolama hesabına {0}erişirken iç hata oluştu.  |
|  DepolamaHesabıAbonelikMismatch  |  Depolama {0} hesabı aboneye {1}ait değildir.  |
|  Depolama HesabıÇok Meşgul  |  Depolama hesabı{0}' ' ' şu anda çok meşgul. Başka bir hesap kullanmayı düşünün.  |
|  StorageAccountTypeNotSupported  |  {0} Disk, {1} Blob depolama hesabı olan bir hesap kullanır. Lütfen Genel amaçlı depolama hesabı ile yeniden deneyin.  |
|  StorageAccountTypeNotSupported  |  Depolama {0} hesabı {1} türüdür. Önyükleme Tanılama depolama hesabı türlerini destekler. {2}  <ul><li>Bu hata, Önyükleme tanılama için premium depolama hesabı kullanırsanız oluşur. Daha fazla bilgi için [önyükleme tanılamanasıl kullanılır](boot-diagnostics.md)bakın. </li></ul> |
|  AbonelikNotAuthorizedForImage  |  Abonelik yetkili değildir.  |
|  TargetDiskBlobZatenVar  |  Blob {0} zaten var. Yeni bir boş veri diski{1}' 'oluşturmak için farklı bir blob URI sağlayın.  |
|  TargetDiskBlobZatenVar  |  Hedef görüntü blob {0} zaten var ve VHD lekeleri üzerine yazmak için bayrak ayarlanmadığından yakalama işlemi devam edemez. Ya blob silmek veya VHD lekeleri üzerine yazmak ve yeniden denemek için bayrağı ayarlayın.  |
|  TargetDiskBlobZatenVar  |  Hedef görüntü blob {0} üzerinde etkin bir kira olduğundan yakalama işlemi devam edemez.   |
|  TargetDiskBlobZatenVar  |  Blob {0} zaten var. Lütfen disk ' 'için{1}hedef olarak farklı bir blob URI sağlayın.  |
|  TooManyVMRedeploymentRequests  |  VM '{0}veya bu VM ile aynı kullanılabilirlik kümesindeki VM'ler için çok fazla yeniden dağıtım isteği alındı. Lütfen daha sonra tekrar deneyin.  |
|  VHDSizeGeçersiz  |  Blob {2} ile disk {0} '{1}' için belirtilen disk boyutu değeri geçersizdir. Disk boyutu arasında {3} {4}ve .  |
|  VMAgentStatusCommunicationError  |  VM{0}' ' VM aracısı veya uzantıları için durum bildirilmemiştir. Lütfen VM'nin çalışan bir VM aracısı olduğunu ve Azure depolama alanına giden bağlantılar kurabileceğini doğrulayın.  |
|  VMArtifactRepositoryInternalError  |  VM artifakı ayrıntılarını almak için yapı deposuyla iletişim kurarken bir hata oluştu.  |
|  VMArtifactRepositoryInternalError  |  Yapı deposundan VM artifakı verileri alırken bir iç hata oluştu.  |
|  VMExtensionHandlerNonTransientError  |  Handler{0}' ' ' terminal hata{1}kodu ' '{2}ve hata iletisi ile VM Extension ' ' için hata bildirdi: ' '{3}'  |
|  VMExtensionManagementInternalError  |  VM uzantısı{0}' 'işlenirken iç hata oluştu.  |
|  VMExtensionManagementInternalError  |  VM uzantıları hazırlanırken birden çok hata oluştu. Ayrıntılar için VM uzantı örnek görünümüne bakın.  |
|  VMExtensionProvisioningError  |  VM uzantısı ' 'işlenirken{0}bir hata bildirdi. Hata iletisi: "{1}".  |
|  VMExtensionProvisioningError  |  Birden çok VM uzantıları VM üzerinde sağlanamadı. Ayrıntılar için lütfen VM uzantıörneği görünümüne bakın.  |
|  VMExtensionProvisioningTimeout  |  VM uzantısı '{0}' sağlanması zaman doldu. Uzantı yüklemesi çok uzun sürebilir veya uzantı durumu elde edilemedi.  |
|  VMMarketplaceInvalidInput  |  Pazar Yeri olmayan bir görüntüden sanal bir makine oluşturmak Plan bilgisigerektirmez, lütfen istekteki Plan bilgilerini kaldırın. İşletim {0}sistemi disk adı .  |
|  VMMarketplaceInvalidInput  |  Satınalma bilgileri eşleşmez. Market görüntüsünden dağıtılamıyor. İşletim {0}sistemi disk adı .  |
|  VMMarketplaceInvalidInput  |  Market görüntüsünden sanal bir makine oluşturmak, istekteki Plan bilgilerini gerektirir. İşletim {0}sistemi disk adı .  |
|  VMNotFound  |  VM '{0}' ' bulunamadı.  |
|  VMRedeploymentFailed  |  VM{0}' ' ' yeniden dağıtım bir iç hata nedeniyle başarısız oldu. Lütfen daha sonra tekrar deneyin.  |
|  VMRedeploymentTimedOut  |  VM ' '{0}nin yeniden dağıtılması ayrılan sürede sona ermedi. Bir süre içinde başarılı bir şekilde bitirebilir. Aksi takdirde, isteği yeniden deneyebilirsiniz.  |
|  VMStartTimedOut  |  VM{0}' ' ' için ayrılan süre içinde başlamadı. VM hala başarılı bir şekilde başlayabilir. Lütfen daha sonra güç durumunu kontrol edin.  |


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
