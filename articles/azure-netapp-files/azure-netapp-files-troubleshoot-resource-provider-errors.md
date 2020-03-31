---
title: Azure NetApp Dosyaları Kaynak Sağlayıcı hatalarını giderme | Microsoft Dokümanlar
description: Sık karşılaşılan Azure NetApp Dosyaları Kaynak Sağlayıcısı hatalarının nedenlerini, çözümlerini ve geçici çözümlerini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
tags: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: b-juche
ms.openlocfilehash: 62e67d4965444df0e731b4387808ed3b89e4673a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72597200"
---
# <a name="troubleshoot-azure-netapp-files-resource-provider-errors"></a>Azure NetApp Files Kaynak Sağlayıcısı hatalarını giderme 

Bu makalede, sık karşılaşılan Azure NetApp Dosyaları Kaynak Sağlayıcısı hataları, nedenleri, çözümleri ve geçici çözümler (varsa) açıklanmaktadır.

## <a name="common-azure-netapp-files-resource-provider-errors"></a>Ortak Azure NetApp Dosyaları Kaynak Sağlayıcı hataları

***BareMetalTenantId değiştirilemez.***  

Bu hata, bir birimi güncelleştirmeye veya düzeltmeye çalıştığınızda oluşur ve `BaremetalTenantId` özellik değişen bir değere sahiptir.

* Neden:   
Bir birimi güncelleştirmeye çalışıyorsunuz `BaremetalTenantId` ve özelliğin Azure'da depolanan değerden farklı bir değeri var.
* Çözüm:   
Yama ve `BaremetalTenantId` güncelleştirme (koymak) isteğine eklemeyin. Alternatif olarak, `BaremetalTenantId` istekte aynı olduğundan emin olun.

***ServiceLevel değiştirilemez.***  

Bu hata, kapasite havuzunda zaten birimler varken, farklı bir hizmet düzeyine sahip bir kapasite havuzunu güncelleştirmeye veya düzeltmeye çalıştığınızda oluşur.

* Neden:   
Havuz birimleri içerdiğinde kapasite havuzu hizmet düzeyini güncelleştirmeye çalışıyorsunuz.
* Çözüm:   
Kapasite havuzundaki tüm birimleri silin ve ardından hizmet düzeyini değiştirin.
* Geçici çözüm:   
Başka bir kapasite havuzu oluşturun ve yeni kapasite havuzunda birimleri yeniden oluşturun.

***PoolId değiştirilemez***  

Bu hata, değiştirilen `PoolId` bir özelliği olan bir kapasite havuzugüncelleştirme veya yama çalıştığınızda oluşur.

* Neden:   
Kapasite havuzu `PoolId` özelliğini güncelleştirmeye çalışıyorsunuz. Özellik `PoolId` salt okunur bir özelliktir ve değiştirilemez.
* Çözüm:   
Yama ve `PoolId` güncelleştirme (koymak) isteğine eklemeyin.  Alternatif olarak, `PoolId` istekte aynı olduğundan emin olun.

***CreationToken değiştirilemez.***

Bu hata, birim oluşturulduktan sonra dosya`CreationToken`yolunu ( ) değiştirmeye çalıştığınızda oluşur. Dosya yolu`CreationToken`( ) birim oluşturulduğunda ayarlanmalıdır ve daha sonra değiştirilemez.

* Neden:   
Ses düzeyi oluşturulduktan sonra`CreationToken`dosya yolunu ( ) değiştirmeye çalışıyorsunuz, bu da desteklenen bir işlem değil. 
* Çözüm:   
Dosya yolunu değiştirmek gerekli değilse, hata iletisini kapatmak için istekten parametreyi kaldırmayı düşünün.
* Geçici çözüm:   
Dosya yolunu değiştirmeniz gerekiyorsa`CreationToken`( ), yeni bir dosya yolu ile yeni bir birim oluşturabilir ve sonra verileri yeni birime geçirebilirsiniz.

***CreationToken en az 16 karakter uzunluğunda olmalıdır.***

Bu hata, dosya yolu`CreationToken`( ) uzunluk gereksinimini karşılamadığında oluşur. Dosya yolunun uzunluğu en az bir karakter uzunluğunda olmalıdır.

* Neden:   
Dosya yolu boş.  API'yi kullanarak bir birim oluşturduğunuzda, oluşturma belirteci gereklidir. Azure portalını kullanıyorsanız, dosya yolu otomatik olarak oluşturulur.
* Çözüm:   
Dosya yolu olarak en az`CreationToken`bir karakter girin ( ).

***Etki alanı adı değiştirilemez.***

Bu hata, Etkin Dizin'de etki alanı adını değiştirmeye çalıştığınızda oluşur.

* Neden:   
Etki alanı adı özelliğini güncelleştirmeye çalışıyorsunuz.
* Çözüm:    
Yok. Etki alanı adını değiştiremezsiniz.
* Geçici çözüm:   
Etkin Dizin yapılandırmasını kullanarak tüm birimleri silin. Ardından Active Directory yapılandırmasını silin ve birimleri yeniden oluşturun.

***Nesne ExportPolicy.Rules[RuleIndex]için yinelenen değer hatası.***

Bu hata, dışa aktarma ilkesi benzersiz bir dizin ile tanımlanmadığında oluşur. Dışa aktarma ilkeleri tanımladığınızda, tüm dışa aktarma ilkesi kurallarının 1 ile 5 arasında benzersiz bir dizini olması gerekir.

* Neden:   
Tanımlanan dışa aktarma ilkesi, ihracat ilkesi kuralları gereksinimini karşılamaz. En az bir ihracat ilkesi kuralına ve en fazla beş ihracat ilkesi kuralına sahip olmalısınız.
* Çözüm:   
Dizinin zaten kullanılmadığından ve 1 ile 5 arasında değiştiğinden emin olun.
* Geçici çözüm:   
Ayarlamaya çalıştığınız kural için farklı bir dizin kullanın.

***Hata {action} {resourceTypeName}***

Bu hata, bir kaynak üzerinde bir eylem gerçekleştirirken diğer hata işleme hatası işlemek için başarısız olduğunda görüntülenir.   'Hata' metnini içerir. Herhangi `{action}` biri olabilir`getting`( `creating` `updating`, `deleting`, , veya ).  `{resourceTypeName}` (örneğin, `resourceTypeName` `netAppAccount`, , `capacityPool` `volume`, ve benzeri).

* Neden:   
Bu hata, neden bilinmiyor bir işlenmemiş özel durumdur.
* Çözüm:   
Günlüklerde ayrıntılı nedeni bildirmek için Azure Destek Merkezi'ne başvurun.
* Geçici çözüm:   
Yok.

***Dosya yolu adı yalnızca harfleri, sayıları ve tireleri (""-"") içerebilir.***

Bu hata, dosya yolu desteklenmeyen karakterler içerdiğinde (örneğin, bir dönem ("."), virgül (","), alt puan ("_") veya dolar işareti ("$") oluşur.

* Neden:   
Dosya yolu, örneğin bir dönem ("."), virgül (","), alt puan ("_") veya dolar işareti ("$") gibi desteklenmeyen karakterler içerir.
* Çözüm:   
Alfabetik harf, sayı veya tire ("-") olmayan karakterleri girdiğiniz dosya yolundan kaldırın.
* Geçici çözüm:   
Alt bir alt sayıyı tire ile değiştirebilir veya yeni sözcüklerin başlangıcını belirtmek için boşluklar yerine büyük harf kullanabilirsiniz.  Örneğin, "yeni birim" yerine "NewVolume" kullanın.

***FileSystemId değiştirilemez.***

Bu hata, değiştirmeye `FileSystemId`çalıştığınızda oluşur.  Değiştirme `FileSystemdId` desteklenen bir işlem değildir. 

* Neden:   
Birim oluşturulduğunda dosya sisteminin kimliği ayarlanır. `FileSystemId`sonradan değiştirilemez.
* Çözüm:   
Bir yama `FileSystemId` ve güncelleştirme (koymak) isteğine eklemeyin.  Alternatif olarak, `FileSystemId` istekte aynı olduğundan emin olun.

***Id ile ActiveDirectory: '{string}' yok.***

Bölüm, `{string}` Etkin Dizin bağlantısı `ActiveDirectoryId` için özelliğe girdiğiniz değerdir.

* Neden:   
Etkin Dizin yapılandırmasına sahip bir hesap oluşturduğunuzda, `ActiveDirectoryId` boş olması gereken bir değer girmiş siniz.
* Çözüm:   
Oluşturma (koy) isteğine eklemeyin. `ActiveDirectoryId`

***Geçersiz api sürümü.***

API sürümü gönderilmez veya geçersiz bir değer içerir.

* Neden:   
Sorgu parametresindeki `api-version` değer geçersiz bir değer içerir.
* Çözüm:   
Doğru API sürüm değerini kullanın.  Kaynak sağlayıcısı birçok API sürümlerini destekler. Değer yyyy-mm-dd biçimindedir.

***Geçersiz bir değer '{value}' {1}için alındı.***

Bu `RuleIndex`ileti, , , `AllowedClients` `UnixReadOnly` `UnixReadWrite` `Nfsv3`, , ve `Nfsv4`.

* Neden:   
Giriş doğrulama isteği aşağıdaki alanlardan en az biri `RuleIndex`için `AllowedClients` `UnixReadOnly`başarısız `UnixReadWrite` `Nfsv`oldu: `Nfsv4`, , , , 3 ve .
* Çözüm:   
Komut satırında gerekli ve çakışmayan tüm parametreleri ayarladıktan emin olun. Örneğin, hem parametreleri `UnixReadOnly` hem `UnixReadWrite` de parametreleri aynı anda ayarlayamazsınız.
* Geçici çözüm:   
Yukarıdaki çözüme bakın.

***Vlan {0} {2} {1} için IP aralığı zaten kullanımda***

Kullanılan IP aralıklarının iç kayıtları yeni atanan IP adresiyle çakıştığı için bu hata oluşur.

* Neden:   
Birim oluşturma için atanan IP adresi zaten kayıtlı.
Bunun nedeni daha önce başarısız bir ses oluşturma olabilir.
* Çözüm:   
Azure Destek Merkezi'ne başvurun.

***'{özellik}' için eksik değer.***

Bu hata, istekte gerekli bir özelliğin eksik olduğunu gösterir. {özellik} dizesi eksik özelliğin adını içerir.

* Neden:   
Giriş doğrulama isteği, özelliklerden en az biri için başarısız oldu.
* Çözüm:   
İstekteki tüm gerekli ve çakışmayan özellikleri, özellikle hata iletisinden özelliği ayarladıktan emin olun.

***MountTargets değiştirilemez.***

Bu hata, bir kullanıcı ses birimi MountTargets özelliğini güncelleştirmeye veya düzeltmeye çalıştığında oluşur.

* Neden:   
Birim `MountTargets` özelliğini güncelleştirmeye çalışıyorsunuz. Bu özelliğin değiştirilmesi desteklenmez.
* Çözüm:   
Bir yama `MountTargets` ve güncelleştirme (koymak) isteğine eklemeyin.  Alternatif olarak, istekte aynı `MountTargets` olduğundan emin olun.

***Zaten kullanılmakta olan ad.***

Bu hata, kaynağın adının zaten kullanıldığını gösterir.

* Neden:   
Varolan bir kaynak için kullanılan bir ada sahip bir kaynak oluşturmaya çalışıyorsunuz.
* Çözüm:   
Kaynağı oluştururken benzersiz bir ad kullanın.

***Dosya yolu zaten kullanılıyor.***

Bu hata, birim için dosya yolunun zaten kullanımda olduğunu gösterir.

* Neden:   
Varolan bir birimle aynı olan bir dosya yolu içeren bir birim oluşturmaya çalışıyorsunuz.
* Çözüm:   
Birim oluştururken benzersiz bir dosya yolu kullanın.

***Çok uzun süre isim.***

Bu hata, kaynak adının en büyük uzunluk gereksinimini karşılamadığını gösterir.

* Neden:   
Kaynak adı çok uzun.
* Çözüm:   
Kaynak için daha kısa bir ad kullanın.

***Dosya yolu çok uzun.***

Bu hata, birim için dosya yolunun maksimum uzunluk gereksinimini karşılamadığını gösterir.

* Neden:   
Birim dosya yolu çok uzun.
* Çözüm:   
Daha kısa bir dosya yolu kullanın.

***İsim çok kısa.***

Bu hata, kaynak adının minimum uzunluk gereksinimini karşılamadığını gösterir.

* Neden:   
Kaynak adı çok kısa.
* Çözüm:   
Kaynak için daha uzun bir ad kullanın.

***Dosya yolu çok kısa.***

Bu hata, birim dosya yolunun minimum uzunluk gereksinimini karşılamadığını gösterir.

* Neden:   
Birim dosya yolu çok kısa.
* Çözüm:   
Birim dosya yolunun uzunluğunu artırın.

***Azure NetApp Files API'ye erişilmaz.***

Azure API'si, birimleri yönetmek için Azure NetApp Files API'sine güvenir. Bu hata, API bağlantısıyla ilgili bir sorunu gösterir.

* Neden:   
Temel API yanıt vermiyor, bu da bir iç hatayla sonuçlanır. Bu hata geçici olması muhtemeldir.
* Çözüm:   
Sorun geçici olması muhtemeldir. İstek bir süre sonra başarılı olmalıdır.
* Geçici çözüm:   
Yok. Temel API birimleri yönetmek için gereklidir.

***''{0}için işlem sonucu kimliği bulunamadı.***

Bu hata, bir iç hata işlemin tamamlanmasını engellediğini gösterir.

* Neden:   
Bir iç hata oluştu ve işlemin tamamlanmasını engelledi.
* Çözüm:   
Bu hata geçici olması muhtemeldir. Birkaç dakika bekleyin ve tekrar deneyin. Sorun devam ederse, sorunu araştırmak için teknik destek için bir bilet oluşturun.
* Geçici çözüm:   
Birkaç dakika bekleyin ve sorunun devam edip olmadığını kontrol edin.

***Protokol türleri CIFS ve NFS'nin karıştırılmasına izin verilmiyor***

Bu hata, bir Birim oluşturmaya çalışırken oluşur ve birim özelliklerinde hem CIFS (SMB) hem de NFS iletişim kuralı türleri vardır.

* Neden:   
Hem CIFS (SMB) hem de NFS protokol türleri birim özelliklerinde kullanılır.
* Çözüm:   
Protokol türlerinden birini kaldırın.
* Geçici çözüm:   
Protokol türü özelliğini boş veya boş bırakın.

***Öğe sayısı: nesne için {value}: ExportPolicy.Rules[RuleIndex] min-max aralığının dışındadır.***

Bu hata, dışa aktarma ilkesi kuralları minimum veya maksimum aralık gereksinimini karşılamadığında oluşur. Dışa aktarma ilkesini tanımlarsanız, en az bir dışa aktarma ilkesi kuralı ve en fazla beş ihracat ilkesi kuralı olmalıdır.

* Neden:   
Tanımladığınız dışa aktarma ilkesi gerekli aralığı karşılamaz.
* Çözüm:   
Dizinin zaten kullanılmadığından ve bu aralıkta 1 ile 5 arasında olduğundan emin olun.
* Geçici çözüm:   
Hacimlerde ihracat ilkesi nin kullanılması zorunlu değildir. Dışa aktarma ilkesi kurallarını kullanmanız gerekmiyorsa, dışa aktarma ilkesini tamamen atlayabilirsiniz.

***Yalnızca bir etkin dizine izin verildi***

Bu hata, Etkin Dizin yapılandırması oluşturmaya çalıştığınızda oluşur ve bir zaten bölgede abonelik için var. Birden fazla Active Directory yapılandırması oluşturmaya çalıştığınızda hata da oluşabilir.

* Neden:   
Etkin bir dizini oluşturmaya (güncelleştirmemeye) çalışıyorsunuz, ancak zaten varsınız.
* Çözüm:   
Active Directory yapılandırması kullanımda değilse, önce varolan yapılandırmayı silebilir ve ardından oluşturma işlemini yeniden deneyebilirsiniz.
* Geçici çözüm:   
Yok. Yalnızca bir Etkin Dizin'e izin verilir.

***'{operation}' işlemi desteklenmez.***

Bu hata, işlemin etkin abonelik veya kaynak için kullanılmadığını gösterir.

* Neden:   
İşlem abonelik veya kaynak için kullanılamaz.
* Çözüm:   
İşlemin doğru girildiğinden ve kullanmakta olduğunuz kaynak ve abonelik için kullanılabilir olduğundan emin olun.

***OwnerId değiştirilemez***

Bu hata, birimin OwnerId özelliğini değiştirmeye çalıştığınızda oluşur. OwnerId'i değiştirmek desteklenen bir işlem değildir. 

* Neden:   
Birim `OwnerId` oluşturulduğunda özellik ayarlanır. Özellik daha sonra değiştirilemez.
* Çözüm:   
Bir yama `OwnerId` ve güncelleştirme (koymak) isteğine eklemeyin. Alternatif olarak, istekte aynı `OwnerId` olduğundan emin olun.

***Üst havuz bulunamadı***

Bu hata, bir birim oluşturmaya çalıştığınızda oluşur ve birim oluşturduğunuz kapasite havuzu bulunamadı.

* Neden:   
Birimin oluşturulduğu kapasite havuzu bulunamadı.
* Çözüm:   
Büyük olasılıkla havuz tam olarak oluşturulmamadı veya birim oluşturma sırasında zaten silindi.

***Yama işlemi bu kaynak türü için desteklenmez.***

Bu hata, montaj hedefini veya anlık görüntüsünü değiştirmeye çalıştığınızda oluşur.

* Neden:   
Montaj hedefi oluşturulduğunda tanımlanır ve sonradan değiştirilemez.
Anlık görüntüler değiştirilebilen özellikler içermez.
* Çözüm:   
Yok. Bu kaynakların değiştirilebilen özellikleri yoktur.

***Toplam hacim boyutu için havuz boyutu çok küçük.***

Bu hata, kapasite havuzu boyutunu güncelleştirdiğinizde oluşur ve boyut, `usedBytes` bu kapasite havuzundaki tüm birimlerin toplam değerinden daha küçüktür.  Bu hata, yeni bir birim oluştururken veya varolan bir birimi yeniden boyutlandırırken ve yeni birim boyutu kapasite havuzundaki boş alanı aştığında da oluşabilir.

* Neden:   
Kapasite havuzunu, kapasite havuzundaki tüm birimlerde kullanılan Baytlardan daha küçük bir boyuta güncelleştirmeye çalışıyorsunuz.  Veya, kapasite havuzundaki boş alandan daha büyük bir hacim oluşturmaya çalışıyorsunuz.  Alternatif olarak, bir birimi yeniden boyutlandırmaya çalışıyorsunuz ve yeni boyut kapasite havuzundaki boş alanı aşıyor.
* Çözüm:   
Kapasite havuzu boyutunu daha büyük bir değere ayarlayın veya bir birim için daha küçük bir birim oluşturun.
* Geçici çözüm:   
Kapasite havuzu boyutunun bu boyuta güncelleştirilebilmeleri için yeterli birimleri kaldırın.

***Özellik: Anlık Görüntü konumu Birim ile aynı olmalıdır***

Bu hata, anlık görüntünün sahibi olan birim dışında konumiçeren bir anlık görüntü oluştururken oluşur.

* Neden:   
Anlık görüntü için Konum özelliğinde geçersiz değer.
* Çözüm:   
Konum özelliğinde geçerli dize ayarlayın.

***{resourceType} adı kaynak tanımlayıcı adı ile aynı olmalıdır.***

Bu hata, bir kaynak oluştururken oluşur ve ad özelliğini `resourceId`ad özelliğinden başka bir değerle doldurursunuz.

* Neden:   
Kaynak oluşturduğunuzda ad özelliğigeçersiz değer.
* Çözüm:   
Ad özelliğini boş bırakın veya ad özelliğiyle aynı değeri kullanmasına izin verin (son ters eğik `resourceId`çizgi "/" ile "?") arasında.

***Protokol türü {değer} bilinmiyor***

Bu hata, bilinmeyen bir iletişim kuralı türüne sahip bir birim oluştururken oluşur.  Geçerli değerler "NFSv3", "NFSv4" ve "CIFS"tir.

* Neden:   
Birim `protocolType` özelliğinde geçersiz bir değer ayarlamaya çalışıyorsunuz.
* Çözüm:   
Geçerli bir dize ayarla. `protocolType`
* Geçici çözüm:   
Null `protocolType` olarak ayarlayın.

***Protokol türleri değiştirilemez***

Bu hata, bir birim için `ProtocolType` güncelleştirmeyi veya düzeltmeyi denediğinizde oluşur.  ProtocolType değiştirme desteklenen bir işlem değildir.

* Neden:   
Birim `ProtocolType` oluşturulduğunda özellik ayarlanır.  Güncelleştirilemez.
* Çözüm:   
Yok.
* Geçici çözüm:   
Yeni iletişim kuralı türleri ile başka bir birim oluşturun.

***Türü {resourceType} kaynağı oluşturma türü {kota} kaynaklarının kotasını aşacak {resourceType} türü başına {resourceType} (parentResourceType} ) Geçerli kaynak sayısı {currentCount}, yeni bir kaynak oluşturmadan önce lütfen bu türdeki bazı kaynakları silin.***

Bu hata, bir kaynak oluşturmaya çalışırken`NetAppAccount`oluşur `CapacityPool` `Volume`( `Snapshot`, , , , veya ), ancak kotanız sınırına ulaştı.

* Neden:   
Kaynak oluşturmaya çalışıyorsunuz, ancak kota sınırına ulaşıldı `NetAppAccounts` (örneğin: abonelik başına veya `CapacityPools` başına). `NetAppAccount`
* Çözüm:   
Kota sınırını artırın.
* Geçici çözüm:   
Aynı türdeki kullanılmayan kaynakları silin ve yeniden oluşturun.

***Salt okunur özellik '{propertyName}' için bir değer aldı.***

Değiştirilemeyen bir özellik için değer tanımladığınız zaman bu hata oluşur. Örneğin, birim kimliğini değiştiremezsiniz.

* Neden:   
Değiştirilemeyen bir parametreyi (örneğin, birim kimliği) değiştirmeye çalışıyorsunuz.
* Çözüm:   
Özellik için bir değer değiştirmeyin.

***İstenen {kaynak} bulunamadı.***

Bu hata, var olmayan bir kaynağa (örneğin, bir birim veya anlık görüntü) başvurmaya çalıştığınızda oluşur. Kaynak silinmiş veya bir misspelt kaynak adı olabilir.

* Neden:   
Zaten silinmiş veya yanlış yazılmış bir kaynak adı olan var olmayan bir kaynağa (örneğin, bir birim veya anlık görüntü) başvurmaya çalışıyorsunuz.
* Çözüm:   
Doğru başvuruldığından emin olmak için yazım hataları isteğini denetleyin.
* Geçici çözüm:   
Yukarıdaki Çözüm bölümüne bakın.

***Hizmet düzeyi '{volumeServiceLevel}' üst '{poolServiceLevel}' daha yüksektir***

Bu hata, bir birim oluştururken veya güncellerken ve hizmet düzeyini onu içeren kapasite havuzundan daha yüksek bir düzeye ayarladığınızda oluşur.

* Neden:   
Üst kapasite havuzundan daha yüksek sıralı hizmet düzeyine sahip bir birim oluşturmaya veya güncelleştirmeye çalışıyorsunuz.
* Çözüm:   
Hizmet düzeyini üst kapasite havuzundan aynı veya daha düşük bir dereceye ayarlayın.
* Geçici çözüm:   
Doğru hizmet düzeyine sahip başka bir kapasite havuzunda birim oluşturun. Alternatif olarak, kapasite havuzundaki tüm birimleri silin ve kapasite havuzu için hizmet düzeyini daha yüksek bir dereceye ayarlayın.

***SMB sunucu adı 10 karakterden uzun olmayabilir.***

Bu hata, bir hesap için Etkin Dizin yapılandırması oluştururken veya güncellerken oluşur.

* Neden:   
SMB sunucu adının uzunluğu 10 karakteri aşıyor.
* Çözüm:   
Daha kısa bir sunucu adı kullanın. Maksimum uzunluk 10 karakterdir.
* Geçici çözüm:   
Yok.  Yukarıdaki çözüme bakın. 

***SubnetId değiştirilemez.***

Bu hata, birim oluşturulduktan `subnetId` sonra değiştirmeye çalıştığınızda oluşur.  `SubnetId`birim oluşturulduğunda ayarlanmalıdır ve daha sonra değiştirilemez.

* Neden:   
Desteklenen bir işlem `subnetId` olmayan birim oluşturulduktan sonra değiştirmeye çalışıyorsunuz. 
* Çözüm:   
Değiştirme `subnetId` gerekli değilse, hata iletisini kapatmak için istekten parametreyi kaldırmayı düşünün.
* Geçici çözüm:   
`subnetId`Değiştirmeniz gerekirse, yeni `subnetId`bir ,'la yeni bir birim oluşturabilir ve sonra verileri yeni bir topluya geçirebilirsiniz.

***SubnetId geçersiz biçimdedir.***

Bu hata, yeni bir birim oluşturmaya çalıştığınızda oluşur, ancak bir alt ağ için bir `subnetId` `resourceId` hata değildir.

* Neden:   
Bu hata, yeni bir birim oluşturmaya çalıştığınızda `subnetId` oluşur, `resourceId` ancak bir alt ağ için değildir. 
* Çözüm:   
Kullanılan alt ağ `subnetId` için bir `resourceId` içerdiğinden emin olmak için değerini denetleyin.
* Geçici çözüm:   
Yok. Yukarıdaki çözüme bakın. 

***Subnet'in bir 'Microsoft.NetApp/volumes' delegasyonu olmalıdır.***

Bu hata, bir birim oluştururken oluşur ve seçili alt `Microsoft.NetApp/volumes`ağ .

* Neden:   
Birim oluşturmaya çalıştınız ve `Microsoft.NetApp/volumes`''ye devredilmedi.
* Çözüm:   
'ye `Microsoft.NetApp/volumes`devredilen başka bir alt ağ seçin
* Geçici çözüm:   
Alt ağa doğru bir delegasyon ekleyin.

***Belirtilen kaynak türü bilinmiyor/uygulanamaz.***

Bu hata, bir ad denetimi uygulanabilir olmayan bir kaynak türü nde veya bilinmeyen bir kaynak türü için istendiğinde oluşur.

* Neden:   
Bilinmeyen veya desteklenmeyen bir kaynak türü için ad denetimi istendi.
* Çözüm:   
İsteğini yaptığınız kaynağın desteklendiğini veya yazım hatası içermediğini denetleyin.
* Geçici çözüm:   
Yukarıdaki çözüme bakın.

***Bilinmeyen Azure NetApp Dosyaları Hatası.***

Azure API'si, birimleri yönetmek için Azure NetApp Files API'sine güvenir. Hata, API'ye iletilen iletişimde bir sorunu gösterir.

* Neden:   
Temel API bilinmeyen bir hata gönderiyor. Bu hata geçici olması muhtemeldir.
* Çözüm:   
Sorun geçici olması muhtemeldir ve istek bir süre sonra başarılı olmalıdır. Sorun devam ederse, sorunun araştırılması için bir destek bileti oluşturun.
* Geçici çözüm:   
Yok. Temel API birimleri yönetmek için gereklidir.

***Bilinmeyen bir özellik '{propertyName}' için alınan değer.***

Bu hata, birim, anlık görüntü veya montaj hedefi gibi bir kaynak için var olmayan özellikler sağlandığında oluşur.

* Neden:   
İstek, her kaynakla kullanılabilecek bir özellik kümesine sahiptir. İstekte var olmayan özellikleri dahil edemezsiniz.
* Çözüm:   
Tüm özellik adlarının doğru yazıldığından ve özelliklerin abonelik ve kaynak için kullanılabilir olduğundan emin olun.
* Geçici çözüm:   
Hataya neden olan özelliği ortadan kaldırmak için istekte tanımlanan özellik sayısını azaltın.

***Güncelleştirme işlemi bu kaynak türü için desteklenmez.***

Yalnızca birimler güncelleştirilebilir. Bu hata, örneğin anlık görüntü güncelleştirme gibi desteklenmeyen bir güncelleştirme işlemi gerçekleştirmeye çalıştığınızda oluşur.

* Neden:   
Güncelleştirmeye çalıştığınız kaynak güncelleştirme işlemini desteklemez. Yalnızca birimlerin özellikleri değiştirilebilir.
* Çözüm:   
Yok. Güncelleştirmeye çalıştığınız kaynak güncelleştirme işlemini desteklemez. Bu nedenle değiştirilemez.
* Geçici çözüm:   
Bir birim için, güncelleştirme yerinde yeni bir kaynak oluşturun ve verileri geçirin.

***Birim, başarılı olmayan bir havuzda oluşturulamaz.***

Bu hata, başarılı durumda olmayan bir havuzda bir birim oluşturmaya çalıştığınızda oluşur. Büyük olasılıkla, kapasite havuzu için oluşturma işlemi bazı nedenlerden dolayı başarısız oldu.

* Neden:   
Yeni birimi içeren kapasite havuzu başarısız durumda.
* Çözüm:   
Kapasite havuzunun başarıyla oluşturulup oluşturulmadığını ve başarısız durumda olup olmadığını denetleyin.
* Geçici çözüm:   
Yeni bir kapasite havuzu oluşturun ve yeni havuzda birim oluşturun.

***Birim oluşturuluyor ve şu anda silinemiyor.***

Bu hata, hala oluşturulmakta olan bir birimi silmeye çalıştığınızda oluşur.

* Neden:   
Birimi silmeye çalıştığınızda bir birim hala oluşturuluyor.
* Çözüm:   
Birim oluşturma tamamlanana kadar bekleyin ve silme işlemini yeniden deneyin.
* Geçici çözüm:   
Yukarıdaki çözüme bakın.

***Ses düzeyi silinmektedir ve şu anda silinemez.***

Bu hata, zaten silinmiş bir ses düzeyini silmeye çalıştığınızda oluşur.

* Neden:   
Birim silmeye çalıştığınızda zaten bir birim silinmektedir.
* Çözüm:   
Geçerli silme işlemi tamamlanana kadar bekleyin.
* Geçici çözüm:   
Yukarıdaki çözüme bakın.

***Birim güncelleştiriliyor ve şu anda silinemiyor.***

Bu hata, güncelleştirilen bir birimi silmeye çalıştığınızda oluşur.

* Neden:   
Birim, sesi silmeye çalıştığınızda güncelleştiriliyor.
* Çözüm:   
Güncelleştirme işlemi tamamlanana kadar bekleyin ve silme işlemini yeniden deneyin.
* Geçici çözüm:   
Yukarıdaki çözüme bakın.

***Birim bulunamadı veya başarıyla oluşturulmamadı.***

Bu hata, birim oluşturma başarısız olduğunda oluşur ve birim değiştirmek veya birim için bir anlık görüntü oluşturmak için çalışıyoruz.

* Neden:   
Birim yok veya oluşturma başarısız oldu.
* Çözüm:   
Doğru birimi değiştirip değiştirmediğinizi ve birimin oluşturulmasının başarılı olduğundan denetleyin. Veya anlık görüntü oluşturduğunuz birimin var olup olmadığını denetleyin.
* Geçici çözüm:   
Yok.  Yukarıdaki çözüme bakın. 

***Belirtilen oluşturma belirteci zaten var***

Bu hata, bir birim oluşturmaya çalıştığınızda ve bir birimin zaten var olduğu bir oluşturma belirteci (dışa aktarma yolu) belirttiğinde oluşur.

* Neden:   
Birim oluşturma sırasında belirttiğiniz oluşturma belirteci (dışa aktarma yolu) zaten başka bir birimle ilişkilidir. 
* Çözüm:   
Farklı bir oluşturma belirteci seçin.  Alternatif olarak, diğer birim silin.

***Belirtilen oluşturma belirteci ayrılmıştır***

Bu hata, bir birim oluşturmaya çalıştığınızda ve dosya yolu (oluşturma belirteci) olarak "varsayılan" veya "yok" belirttiğiniz zaman oluşur.

* Neden:    
Bir birim oluşturmaya çalışıyorsunuz ve dosya yolu (oluşturma belirteci) olarak "varsayılan" veya "yok" belirtin.
* Çözüm:   
Farklı bir dosya yolu (oluşturma belirteci) seçin.
 
***Etkin Dizin kimlik bilgileri kullanılıyor***

Bu hata, Etkin Dizin yapılandırmasını en az bir Kobİ biriminin hala bulunduğu bir hesaptan silmeye çalıştığınızda oluşur.  Silmek için çalıştığınız Active Directory yapılandırması kullanılarak SMB birimi oluşturuldu.

* Neden:   
Etkin Dizin yapılandırmasını bir hesaptan silmeye çalışıyorsunuz, ancak Active Directory yapılandırması kullanılarak oluşturulan en az bir Kobİ birimi hala var. 
* Çözüm:   
İlk olarak, Active Directory yapılandırmasını kullanarak oluşturulan tüm Kobİ birimlerini silin.  Ardından yapılandırma silme işlemini yeniden deneyin.

***Kimlik bilgileri kullanılıyorsa Kuruluş Birimi atamasını değiştiremez***

Bu hata, Etkin Dizin yapılandırmasının Kuruluş Birimini değiştirmeye çalıştığınızda oluşur, ancak en az bir Kobİ birimi hala vardır.  Silmek için çalıştığınız Active Directory yapılandırması kullanılarak SMB birimi oluşturuldu.

* Neden:   
Etkin Dizin yapılandırmasının Kuruluş Birimini değiştirmeye çalışıyorsunuz.  Ancak, başlangıçta Active Directory yapılandırması kullanılarak oluşturulan en az bir Kobİ birimi hala vardır.
* Çözüm:   
 İlk olarak, Active Directory yapılandırmasını kullanarak oluşturulan tüm Kobİ birimlerini silin.  Ardından yapılandırma silme işlemini yeniden deneyin. 

***Etkin Dizin güncelleştirmesi zaten devam ediyor***

Bu hata, bir düzenleme işleminin zaten devam etmekte olduğu bir Etkin Dizin yapılandırmasını düzenlemeyi denediğinizde oluşur.

* Neden:   
Etkin Dizin yapılandırmasını düzenlemeye çalışıyorsunuz, ancak başka bir düzenleme işlemi zaten devam ediyor.
* Çözüm:   
Şu anda çalışan edit işlemi tamamlanana kadar bekleyin.

***Önce seçili kimlik bilgilerini kullanarak tüm birimleri silme***

Bu hata, Etkin Dizin yapılandırmasını silmeye çalıştığınızda oluşur, ancak en az bir Kobİ birimi hala vardır.  Silmek için çalıştığınız Active Directory yapılandırması kullanılarak SMB birimi oluşturuldu.

* Neden:   
Etkin Dizin yapılandırmasını silmeye çalışıyorsunuz, ancak Active Directory yapılandırması kullanılarak başlangıçta oluşturulan en az bir Kobİ birimi hala var.
* Çözüm:   
İlk olarak, Active Directory yapılandırmasını kullanarak oluşturulan tüm Kobİ birimlerini silin.  Ardından yapılandırma silme işlemini yeniden deneyin. 

***Bölgede Etkin Dizin kimlik bilgileri bulunamadı***

Bu hata, bir Kobİ birimi oluşturmaya çalıştığınızda oluşur, ancak bölge hesabına Etkin Dizin yapılandırması eklenmemiştir.

* Neden:   
Bir SMB birimi oluşturmaya çalışıyorsunuz, ancak hesaba Etkin Dizin yapılandırması eklenmedi. 
* Çözüm:   
Bir Kobİ birimi oluşturmadan önce hesaba Etkin Dizin yapılandırması ekleyin.

***DNS sunucusu sorgulandı. Ağ yapılandırmasının doğru olduğunu ve DNS sunucularının kullanılabilir olduğunu doğrulayın.***

Bu hata, bir Kobİ birimi oluşturmaya çalıştığınızda oluşur, ancak bir DNS sunucusu (Etkin Dizin yapılandırmanızda belirtilir) erişilemez. 

* Neden:   
Bir SMB birimi oluşturmaya çalışıyorsunuz, ancak Bir DNS sunucusuna (Etkin Dizin yapılandırmanızda belirtilen) erişilemez.
* Çözüm:   
Active Directory yapılandırmanızı gözden geçirin ve DNS sunucu IP adreslerinin doğru ve ulaşılabilir olduğundan emin olun.
DNS sunucu IP adresleriyle ilgili herhangi bir sorun yoksa, erişimi engelleyen güvenlik duvarları olmadığını doğrulayın.

***Çok fazla eşzamanlı iş***

Bu hata, abonelik için üç anlık görüntü oluşturma işlemi zaten devam ederken anlık görüntü oluşturmaya çalıştığınızda oluşur.

* Neden:   
Abonelik için üç anlık görüntü oluşturma işlemi zaten devam ederken anlık görüntü oluşturmaya çalışıyorsunuz. 
* Çözüm:   
Anlık görüntü oluşturma işlerinin tamamlanması en fazla birkaç saniye sürer.  Birkaç saniye bekleyin ve anlık görüntü oluşturma işlemini yeniden deneyin.

***Ek işler doğuramaz. Lütfen devam eden işlerin bitmesini ve yeniden denemesini bekleyin***

Bu hata, belirli koşullar altında bir birim oluşturmaya veya silmeye çalıştığınızda oluşabilir.

* Neden:   
Belirli koşullar altında bir birim oluşturmaya veya silmeye çalışıyorsunuz.
* Çözüm:   
Bir dakika kadar bekleyin ve işlemi yeniden deneyin.

***Birim zaten durumlar arasında geçiş***

Bu hata, şu anda geçiş durumunda olan bir birimi silmeye çalıştığınızda (şu anda oluşturma, güncelleştirme veya silme durumunda) oluşabilir.

* Neden:   
Şu anda geçiş durumunda olan bir birimi silmeye çalışıyorsunuz.
* Çözüm:   
Şu anda çalışan (durum geçiş) işlemi tamamlanana kadar bekleyin ve sonra işlemi yeniden deneyin.

***Kaynak birim anlık görüntüden yeni birim bölmede başarısız oldu***

 Anlık görüntüden bir birim oluşturmaya çalıştığınızda bu hata oluşabilir.  

* Neden:   
Anlık görüntüden bir birim oluşturmaya çalışırsınız ve birim bir hata durumunda sona erer.
* Çözüm:   
Ses düzeyini silin ve anlık görüntüden birim oluşturma işlemini yeniden deneyin.

 
## <a name="next-steps"></a>Sonraki adımlar

* [REST API'li Azure NetApp Dosyaları için geliştirin](azure-netapp-files-develop-with-rest-api.md)
