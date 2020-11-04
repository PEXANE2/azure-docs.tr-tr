---
title: Azure 'da paylaşılan görüntülerle ilgili sorunları giderme
description: Paylaşılan görüntü galerileriyle ilgili sorunları nasıl giderebileceğinizi öğrenin.
author: olayemio
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: troubleshooting
ms.workload: infrastructure
ms.date: 10/27/2020
ms.author: olayemio
ms.reviewer: cynthn
ms.openlocfilehash: 189fa12b1fc11e79ab64231a7ecd453113b8771a
ms.sourcegitcommit: fa90cd55e341c8201e3789df4cd8bd6fe7c809a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93336019"
---
# <a name="troubleshooting-shared-image-galleries-in-azure"></a>Azure 'da paylaşılan görüntü galerileri sorunlarını giderme

Paylaşılan görüntü galerileri, görüntü tanımları ve görüntü sürümleri ile ilgili işlem yaparken sorunlarla karşılaşıyorsanız başarısız olan komutları hata ayıklama modunda yeniden çalıştırın. `--debug`Anahtar CLI ve anahtarı PowerShell ile geçirerek hata ayıklama modu etkinleştirilir `-Debug` . Hatayı bulduktan sonra, hataları gidermek için bu belgeyi izleyin.


## <a name="issues-with-creating-or-modifying-a-gallery"></a>Galeri oluşturma veya değiştirme sorunları ##

*Galeri adı geçersiz. İzin verilen karakterler, en fazla 80 karakter uzunluğunda olan Ingilizce alfasayısal karakter, alt çizgi ve nokta ile kullanılabilir. Tireler dahil diğer tüm özel karakterlere izin verilmez.*  
**Neden** : Galeri için verilen ad, adlandırma gereksinimlerini karşılamıyor.  
**Geçici çözüm** : aşağıdaki koşullara uyan bir ad seçin: 1) 80-karakter sınırı, 2) yalnızca İngilizce harfler, rakamlar, alt çizgiler ve noktalar içerir, 3), İngilizce harfler veya rakamlar ile başlar ve biter.

*' Gallername ' varlık adı, kendi doğrulama kuralına göre geçersiz: ^ [^ \_ \w] [\w-. \_ ] {0,79} (? <! [-.]) $.*  
**Neden** : Galeri adı adlandırma gereksinimlerini karşılamıyor.  
**Geçici çözüm** : Galeri için şu koşullara uyan bir ad seçin: 1) 80-karakter sınırı, 2) yalnızca İngilizce harfler, rakamlar, alt çizgiler ve noktalar içerir, 3), İngilizce harfler veya rakamlar ile başlar ve biter.

*Belirtilen kaynak adı <Gallername \> Şu geçersiz karakterleri içeriyor: <karakteri \> . Ad şu karakterlerle bitemez: <karakteri\>*  
**Neden** : galerinin adı nokta veya alt çizgi ile sona erer.  
**Geçici çözüm** : Galeri için şu koşullara uyan bir ad seçin: 1) 80-karakter sınırı, 2) yalnızca İngilizce harfler, rakamlar, alt çizgiler ve noktalar içerir, 3), İngilizce harfler veya rakamlar ile başlar ve biter.

*Sağlanan konum <bölgesi \> ' Microsoft. COMPUTE/galeriler ' kaynak türü için kullanılamıyor. Kaynak türü için kullanılabilir bölgelerin listesi...*  
**Neden** : Galeri için belirtilen bölge yanlış veya bir erişim isteği gerektiriyor.  
**Geçici çözüm** : bölge adının doğru yazıldığından emin olun. Hangi bölgeleri erişiminizin olduğunu görmek için bu komutu çalıştırabilirsiniz. Bölge listede listelenmiyorsa, [bir erişim isteği](/troubleshoot/azure/general/region-access-request-process)gönderebilirsiniz.

*İç içe geçmiş kaynaklar silinmeden önce kaynak silinemiyor.*  
**Neden** : var olan en az bir görüntü tanımı içeren bir galeriyi silmeye çalıştınız. Bir galeri silinmeden önce boş olmalıdır.  
**Geçici çözüm** : galerinin içindeki tüm görüntü tanımlarını silin ve sonra galeriyi silmeye devam edin. Görüntü tanımı görüntü sürümlerini içeriyorsa, görüntü tanımları silinmeden önce görüntü sürümlerinin silinmesi gerekir.

*<Gallername kaynağı \> kaynak grubundaki <bölge 1 konumunda zaten \_ mevcut \> <resourceGroup \> . <bölge 2 konumunda aynı ada sahip bir kaynak oluşturulamıyor \_ \> . Lütfen yeni bir kaynak adı seçin.*  
**Neden** : kaynak grubunda aynı ada sahip bir galeri zaten var ve aynı ada ancak farklı bir bölgede başka bir galeri oluşturmaya çalıştınız.  
**Geçici çözüm** : farklı bir galeri ya da farklı bir kaynak grubu kullanın.

## <a name="issues-with-creating-or-modifying-image-definitions"></a>Görüntü tanımları oluşturma veya değiştirme sorunları ##

*' Gallerımage. Properties. <Property ' özelliğinin değiştirilmesine \> izin verilmiyor.*  
**Neden** : işletim sistemi türü, işletim sistemi durumu, Hyper V oluşturma, teklif, Yayımcı, SKU 'yu değiştirmeye çalıştınız. Bu özelliklerden herhangi birini değiştirmeye izin verilmez.  
**Geçici çözüm** : bunun yerine yeni bir görüntü tanımı oluşturun.

*<Gallername/ımagedefinitionname kaynağı \> kaynak grubundaki <bölge 1 konumunda zaten \_ mevcut \> <resourceGroup \> . <bölge 2 konumunda aynı ada sahip bir kaynak oluşturulamıyor \_ \> . Lütfen yeni bir kaynak adı seçin.*  
**Neden** : aynı Galeri ve kaynak grubunda aynı ada sahip bir görüntü tanımınız var ve aynı ada ve aynı galeride ancak farklı bir bölgede başka bir görüntü tanımı oluşturmaya çalıştık.  
**Geçici çözüm** : görüntü tanımı için farklı bir ad kullanın veya görüntü tanımını farklı bir galeri veya kaynak grubuna yerleştirin

*Belirtilen kaynak adı <Gallername \> /<ımagedefinitionname \> Şu geçersiz karakterler içeriyor: <karakter \> . Ad şu karakterlerle bitemez: <karakteri\>*  
**Neden** : verilen <ımagedefinitionname, \> nokta veya alt çizgiyle bitiyor.  
**Geçici çözüm** : aşağıdaki koşullara uyan görüntü tanımı için bir ad seçin: 1) 80-karakter sınırı, 2) yalnızca İngilizce harfler, rakamlar, tireler, alt çizgiler ve noktalar içerir, 3), İngilizce harfler veya rakamlar ile başlar ve biter.

*<ımagedefinitionname varlık adı, \> kendi doğrulama kuralına göre geçersiz: ^ [^ \_ \\ W] [ \\ W-. \_ ] {0,79} (? <! [-.]) $"*  
**Neden** : verilen <ımagedefinitionname, \> nokta veya alt çizgiyle bitiyor.  
**Geçici çözüm** : aşağıdaki koşullara uyan görüntü tanımı için bir ad seçin: 1) 80-karakter sınırı, 2) yalnızca İngilizce harfler, rakamlar, tireler, alt çizgiler ve noktalar içerir, 3), İngilizce harfler veya rakamlar ile başlar ve biter.

*Gallerımage. Properties. Identifier. <özelliği varlık adı \> geçerli değil. Boş olamaz. İzin verilen karakterler büyük veya küçük harfler, rakamlar, kısa çizgi (-), nokta (.), alt çizgi ( \_ ). Adların nokta (.) ile bitme izni yok. Adın uzunluğu <sayı \> karakteri aşamaz.*  
**Neden** : verilen Yayımcı, TEKLIF veya SKU değeri, adlandırma gereksinimlerini karşılamıyor.  
**Geçici çözüm** : aşağıdaki koşullara uyan bir değer seçin: 1) 128-yayımcı veya 64 için karakter sınırı; TEKLIF ve SKU için karakter sınırı, 2) yalnızca İngilizce harfler, rakamlar, tire, alt çizgi, nokta ve 3) noktayla bitmez.

*İç içe kaynak üzerinde istenen işlem gerçekleştirilemiyor. Gallermadı <üst kaynak \> bulunamadı.*  
**Neden** : \> geçerli abonelikte ve kaynak grubunda <gallername adlı bir galeri yok.  
**Geçici çözüm** : Galeri adının, aboneliğin ve kaynak grubunun doğru olduğundan emin olun. Aksi takdirde, <Gallername adlı yeni bir galeri oluşturun \> .

*Sağlanan konum <bölgesi \> ' Microsoft. COMPUTE/galeriler ' kaynak türü için kullanılamıyor. Kaynak türü için kullanılabilir bölgelerin listesi...*  
**Neden** : <bölgesi \> yanlış veya bir erişim isteği gerektiriyor  
**Geçici çözüm** : bölge adının doğru yazıldığından emin olun. Hangi bölgeleri erişiminizin olduğunu görmek için bu komutu çalıştırabilirsiniz. Bölge listede listelenmiyorsa, [bir erişim isteği](/troubleshoot/azure/general/region-access-request-process)gönderebilirsiniz.

*Değer seri hale getirilemiyor: <değeri \> tür: ' ISO-8601 '., ISO8601Error: ıso 8601 zaman göstergesi 't ' eksik. DateTime dize <değeri ayrıştırılamıyor\>*  
**Neden** : özelliğe girilen değer doğru şekilde bir tarih olarak biçimlendirilmedi.  
**Geçici çözüm** : yyyy-aa-gg, yyyy-mm-dd'T'HH: mm: Sszzz veya [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)-geçerli biçiminde bir tarih girin.

*Dize DateTimeOffset: <değerine dönüştürülemedi \> . ' Properties. <Property \> ' yolu*  
**Neden** : özelliğe girilen değer doğru şekilde bir tarih olarak biçimlendirilmedi.  
**Geçici çözüm** : yyyy-aa-gg, yyyy-mm-dd'T'HH: mm: Sszzz veya [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)-geçerli biçiminde bir tarih girin.

*Endofyaşam tarihi, gelecekteki bir tarih olarak ayarlanmalıdır.*  
**Neden** : yaşam bitiş tarihi özelliği, bugünün tarihinden sonra gelen bir tarih olarak doğru biçimlendirilmedi.  
**Geçici çözüm** : yyyy-aa-gg, yyyy-mm-dd'T'HH: mm: Sszzz veya [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)-geçerli biçiminde bir tarih girin.

*bağımsız değişken--<özellik \> : geçersiz int değeri: <değeri\>*  
**Neden** : <özellik \> yalnızca tamsayı değerlerini kabul ediyor ve <değeri \> bir tamsayı değil.  
**Geçici çözüm** : bir tamsayı değeri seçin.

*<özelliğinin en küçük değeri \> <özelliğinin en büyük değerinden büyük olmamalıdır \> .*  
**Neden** : <özelliği için belirtilen minimum değer \> <özelliği için verilen en büyük değerden daha yüksek \> .  
**Geçici çözüm** : değerleri minimum veya en yüksek değere eşit olacak şekilde değiştirin.

*Galeri görüntüsü: <ımagedefinitionname \> tarafından tanımlanan (Yayımcı: <yayımcı \> , teklif: <teklif \> , SKU: <SKU \> ) zaten var. Farklı bir yayımcı, teklif, SKU birleşimi seçin.*  
**Neden** : aynı galerideki aynı yayımcı, teklif, SKU 'su ile yeni bir görüntü tanımı oluşturmaya çalıştınız.  
**Geçici çözüm** : belirli bir galeri içinde tüm görüntü tanımlarının benzersiz bir yayımcı, TEKLIF, SKU birleşimi olması gerekir. Benzersiz bir bileşim seçin veya yeni bir galeri seçin ve görüntü tanımını yeniden oluşturun.

*İç içe geçmiş kaynaklar silinmeden önce kaynak silinemiyor.*  
**Neden** : görüntü sürümlerini içeren bir görüntü tanımını silmeye çalıştınız. Bir görüntü tanımının silinebilmesi için önce boş olması gerekir.  
**Geçici çözüm** : görüntü tanımının içindeki tüm görüntü sürümlerini silin ve ardından görüntü tanımını silmeye devam edin.

*Parameter <özelliği bağlanamıyor \> . Değer <değeri \> <propertyType türüne dönüştürülemiyor \> . Tanımlayıcı adı <değeri \> geçerli bir Numaralandırıcı adı ile eşleşmiyor. Aşağıdaki Numaralandırıcı adlarından birini belirtip yeniden deneyin: <choice1 \> , <choice2 \> ,...*  
**Neden** : özellik, olası değerler listesine sahip ve <değeri \> bunlardan biri değil.  
**Geçici çözüm** : olası <seçim değerlerinden birini seçin \> .

*Parameter <özelliği bağlanamıyor \> . Değer <değeri \> &quot; System. DateTime türüne dönüştürülemiyor&quot;*  
**Neden** : özelliğe girilen değer doğru şekilde bir tarih olarak biçimlendirilmedi.  
**Geçici çözüm** : yyyy-aa-gg, yyyy-mm-dd'T'HH: mm: Sszzz veya [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)-geçerli biçiminde bir tarih girin.

*Parameter <özelliği bağlanamıyor \> . Değer <değeri \> &quot; System. Int32 türüne dönüştürülemiyor&quot;*  
**Neden** : <özellik \> yalnızca tamsayı değerlerini kabul ediyor ve <değeri \> bir tamsayı değil.  
**Geçici çözüm** : bir tamsayı değeri seçin.

*ZRS depolama hesabı türü bu bölgede desteklenmiyor.*  
**Neden** : henüz desteklemeyen bir bölgede standart ZRS seçtiniz.  
**Geçici çözüm** : depolama hesabı türünü ' Premium \_ LRS ' veya ' standart \_ LRS ' olarak değiştirin. ZRS önizlemesinin etkinleştirildiği bölgelerin en son [listesi](/azure/storage/common/storage-redundancy#zone-redundant-storage) için belgelerimizi kontrol edin.

## <a name="issues-with-creating-or-updating-image-versions"></a>Görüntü sürümlerini oluşturma veya güncelleştirme sorunları ##

*Sağlanan konum <bölgesi \> ' Microsoft. COMPUTE/galeriler ' kaynak türü için kullanılamıyor. Kaynak türü için kullanılabilir bölgelerin listesi...*  
**Neden** : <bölgesi \> yanlış veya bir erişim isteği gerektiriyor  
**Geçici çözüm** : bölge adının doğru yazıldığından emin olun. Hangi bölgeleri erişiminizin olduğunu görmek için bu komutu çalıştırabilirsiniz. Bölge listede listelenmiyorsa, [bir erişim isteği](/troubleshoot/azure/general/region-access-request-process)gönderebilirsiniz.

*İç içe kaynak üzerinde istenen işlem gerçekleştirilemiyor. Gallername/ımagedefinitionname <üst kaynak \> bulunamadı.*  
**Neden** : \> geçerli abonelikte ve kaynak grubunda <Gallername/ımagedefinitionname adlı bir galeri yok.  
**Geçici çözüm** : Galeri adının, aboneliğin ve kaynak grubunun doğru olduğundan emin olun. Aksi takdirde, \> belirtilen kaynak grubunda <ımagedefinitionname adlı <gallername ve/veya image Definition adlı yeni bir galeri oluşturun \> .

*Parameter <özelliği bağlanamıyor \> . Değer <değeri \> &quot; System. DateTime türüne dönüştürülemiyor&quot;*  
**Neden** : özelliğe girilen değer doğru şekilde bir tarih olarak biçimlendirilmedi.  
**Geçici çözüm** : yyyy-aa-gg, yyyy-mm-dd'T'HH: mm: Sszzz veya [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)-geçerli biçiminde bir tarih girin.

*Parameter <özelliği bağlanamıyor \> . Değer <değeri \> &quot; System. Int32 türüne dönüştürülemiyor&quot;*  
**Neden** : <özellik \> yalnızca tamsayı değerlerini kabul ediyor ve <değeri \> bir tamsayı değil.  
**Geçici çözüm** : bir tamsayı değeri seçin.

*Galeri görüntüsü sürüm yayımlama profili bölgeleri <Publishingregion \> , görüntü sürümü <Sourceregilocation 'ın konumunu içermelidir\>*  
**Neden** : kaynak görüntünün konumu (<sourceRegion \> ) <publishingregion \> listesine eklenmelidir  
**Geçici çözüm** : \> <publishingregion listesine < sourceregidahil edin \> .

*\>Parametre <özelliğinin değeri <değer \> Aralık dışında. Değer <minValue \> ile <MaxValue \> (dahil) arasında olmalıdır.*  
**Neden** : <değeri \> <özelliği için olası değerler aralığının dışında \> .  
**Geçici çözüm** : <minValue ve <MaxValue aralığı içinde olan bir değer seçin \> \> .

*Kaynak <RESOURCEID \> bulunamadı. Lütfen kaynak olup olmadığını denetleyin ve oluşturulan Galeri görüntüsü sürümü ile aynı bölgede.*  
**Neden** : <RESOURCEID konumunda kaynak yok \> veya <RESOURCEID konumundaki kaynak, \> oluşturulmakta olan Galeri görüntüsüyle aynı bölgede değil.  
**Geçici çözüm** : <RESOURCEID 'nin \> doğru olduğundan ve Galeri görüntüsü sürümünün kaynak bölgesinin, RESOURCEID <bölgesiyle aynı olduğundan emin olun\>

*' Gallerımageversion. Properties. storageProfile. <DiskImage \> . Source.id ' özelliğinin değiştirilmesine izin verilmiyor.*  
**Neden** : bir galeri görüntüsü sürümünün kaynak kimliği, oluşturulduktan sonra değiştirilemez.  
**Geçici çözüm** : kaynak kimliğinin zaten var olan kaynak kimliğiyle aynı olduğundan emin olun veya görüntü sürümünün sürüm numarasını değiştirin.

*Giriş veri disklerinde yinelenen LUN numaraları algılandı. LUN numarası her bir veri diski için benzersiz olmalıdır.*  
**Neden** : bir disk ve/veya disk anlık görüntülerinin listesini kullanarak bir görüntü sürümü oluştururken, iki veya daha fazla disk ya da disk anlık görüntüsü aynı LUN numaralarına sahiptir.  
**Geçici çözüm** : yinelenen LUN numaralarını kaldırın veya değiştirin.

*Yinelenen kaynak kimlikleri, giriş disklerinde bulunur. Kaynak kimliği her disk için benzersiz olmalıdır.*  
**Neden** : bir disk ve/veya disk anlık görüntülerinin listesini kullanarak bir görüntü sürümü oluştururken, iki veya daha fazla disk ya da disk anlık görüntüsü aynı kaynak kimliğine sahiptir.  
**Geçici çözüm** : yinelenen disk kaynağı kimliklerini kaldırın veya değiştirin.

*\>' Properties. storageProfile. <Diskımages. Source.id ' yolundaki <ResourceId Özellik kimliği \> geçersiz. '/Subscriptions/{SubscriptionID} ' veya '/providers/{resourceProviderNamespace}/' ile başlayan tam kaynak kimliği beklenmelidir.*  
**Neden** : <RESOURCEID \> Hatalı biçimlendirilmiş.  
**Geçici çözüm** : RESOURCEID 'nin doğru olup olmadığını denetleyin.

*Kaynak kimliği: <RESOURCEID, \> yönetilen bir görüntü, sanal makine ya da başka bir galeri görüntüsü sürümü olmalıdır*  
**Neden** : <RESOURCEID \> Hatalı biçimlendirilmiş.  
**Geçici çözüm** : kaynak görüntü olarak VM, yönetilen görüntü veya Galeri görüntüsü sürümünü KULLANıYORSANıZ, VM 'nin, yönetilen görüntünün veya Galeri görüntüsü sürümünün kaynak kimliğinin doğru olduğundan emin olun.

*Kaynak kimliği: <RESOURCEID, \> yönetilen bir disk veya anlık görüntü olmalıdır.*  
**Neden** : <RESOURCEID \> Hatalı biçimlendirilmiş.  
**Geçici çözüm** : diskler ve/veya disk anlık görüntülerini görüntü sürümü için kaynaklar olarak kullanıyorsanız, disklerin ve/veya disk anlık görüntülerinin kaynak kimliklerinin doğru olduğundan emin olun.

*\>Üst Galeri görüntüsündeki (<osstate 1) işletim sistemi durumu \_ \> <osstate 2 olmadığından, bu öğeden Galeri görüntüsü sürümü oluşturulamıyor: <RESOURCEID \_ \> .*  
**Neden** : işletim sistemi durumu (Genelleştirilmiş veya özel) görüntü tanımında belirtilen işletim sistemi durumuyla eşleşmiyor.  
**Geçici çözüm** : <osstate 1 işletim sistemi durumuna sahıp bir VM 'yi temel alan bir kaynak seçin \_ \> ya da <osstate 2 ' ye göre VM 'ler için yeni bir görüntü tanımı oluşturun \_ \> .

*' <RESOURCEID ' kimliğine sahip kaynak, \> \_ \> üst Galeri görüntüsü hiper yönetici oluşturma [' <v # \_ 2 \> '] dışında farklı bir hiper yönetici üretimi [' <V # 1 '] içeriyor*  
**Neden** : görüntü sürümünün hiper yönetici nesli, görüntü tanımında belirtilen hiper yönetici oluşturma ile eşleşmiyor. Görüntü tanımı işletim sistemi <V # \_ 1 \> ve görüntü sürümü işletim sistemi <v # 2 ' dir \_ \> .  
**Geçici çözüm** : görüntü tanımıyla aynı hiper yönetici oluşturmaya sahip bir kaynak seçin ya da görüntü sürümüyle aynı hiper yönetici oluşturmaya sahip yeni bir görüntü tanımı oluşturun/seçin.

*' <RESOURCEID ' kimliğine sahip kaynak, \> \_ \> üst Galeri görüntüsü işletim sistemi türü oluşturma [' <OSType \_ 2 \> '] dışında farklı bir işletim sistemi türü [' <OSType 1 '] sahip.*  
**Neden** : görüntü sürümünün hiper yönetici nesli, görüntü tanımında belirtilen hiper yönetici oluşturma ile eşleşmiyor. Görüntü tanımı işletim sistemi <OsType \_ 1 \> ve görüntü sürümü işletim sistemi <OSType 2 ' dir \_ \> .  
**Geçici çözüm** : görüntü tanımıyla aynı işletim sistemine (Linux/Windows) sahip bir kaynak seçin ya da görüntü sürümü ile aynı işletim sistemi oluşturmaya sahip yeni bir görüntü tanımı oluşturun/seçin.

*Kaynak sanal makine <RESOURCEID \> , kısa ömürlü bir işletim sistemi diski içeremez.*  
**Neden** : ' <RESOURCEID ' konumundaki kaynak, \> kısa ömürlü bir işletim sistemi diski içeriyor. Paylaşılan görüntü Galerisi Şu anda kısa ömürlü işletim sistemi diskini desteklemiyor.  
**Geçici çözüm** : kısa ömürlü işletim sistemi diski kullanmayan bir VM 'yi temel alan farklı bir kaynak seçin.

*Kaynak sanal makine <RESOURCEID \> , bir UltraSSD hesabı türünde depolanan disk [' <DiskID \> '] içeremez.*  
**Neden** : disk ' <DiskID \> bir UltraSSD disk. Paylaşılan görüntü Galerisi Şu anda Ultra SSD diskleri desteklemiyor.  
**Geçici çözüm** : yalnızca Premium SSD, standart SSD ve/veya standart HDD tarafından yönetilen diskleri içeren bir kaynak kullanın.

*Kaynak sanal makine <RESOURCEID \> yönetilen disklerden oluşturulmalıdır.*  
**Neden** : RESOURCEID <sanal makine \> yönetilmeyen diskler kullanıyor.  
**Geçici çözüm** : yalnızca Premium SSD, standart SSD ve/veya standart HDD tarafından yönetilen diskleri IÇEREN bir VM 'yi temel alan bir kaynak kullanın.

*' <RESOURCEID ' kaynağında çok fazla istek yok \> . Lütfen yeniden denemeden önce kaynaktaki istek sayısını azaltın veya bir süre bekleyin.*  
**Neden** : Bu görüntü sürümünün kaynağı çok fazla istek nedeniyle şu anda kısıtlanıyor.  
**Geçici çözüm** : görüntü sürümü oluşturmayı daha sonra deneyin.

*' <Diskencryptionsetıd ' disk şifreleme kümesi, \> \> Galeri kaynağı olarak aynı ' <SubscriptionID ' aboneliğinde olmalıdır.*  
**Neden** : disk şifreleme kümeleri yalnızca oluşturuldukları abonelikte ve bölgede kullanılabilir.  
**Geçici çözüm** : görüntü sürümüyle aynı abonelikte ve bölgede bir şifreleme kümesi oluşturun veya kullanın

*Şifrelenmiş kaynak: ' <RESOURCEID \> ', geçerli galeri görüntü sürümü aboneliğinden (' <SubscriptionID 1 ') farklı bir ABONELIK kimliğinde \_ \> . \_ \> Galeri görüntüsü sürümünü oluşturmak için lütfen şifrelenmemiş bir kaynakla yeniden deneyin veya kaynağın ' <Subcriptionıd 2 ' aboneliğini kullanın.*  
**Neden** : paylaşılan görüntü Galerisi, kaynak görüntü şifrelenirse başka bir kaynak görüntüden başka bir abonelikte görüntü sürümlerini oluşturmayı desteklememektedir.  
**Geçici çözüm** : şifrelenmemiş bir kaynak kullanın veya kaynak ile aynı abonelikte görüntü sürümünü oluşturun.

*<Diskencryptionsetıd adlı disk şifreleme kümesi \> bulunamadı.*  
**Neden** : disk şifrelemesi yanlış olabilir.  
**Geçici çözüm** : disk şifreleme KÜMESININ kaynak kimliğinin doğru olduğundan emin olun.

*Görüntü sürümü adı geçersiz. Görüntü sürümü adı büyük (int) olmalıdır. Küçük (int). Patch (int) biçimi, örneğin: 1.0.0, 2018.12.1 vb.*  
**Neden** : görüntü sürümü için geçerli biçim bir noktayla ayrılmış 3 tamsayılardır. Görüntü sürümü adı geçerli biçime uymuyor.  
**Geçici çözüm** : Ana (int) biçimini izleyen bir görüntü sürümü adı kullanın. Küçük (int). Patch (int), örneğin: 1.0.0. or 2018.12.1.

*GalleryArtifactVersion. Properties. publishingProfile. Targetregion. encryption. Datadiskımages. Diskencryptionsetıd parametresinin değeri geçersiz*  
**Neden** : bir veri diski görüntüsünde kullanılan disk şifrelemesi KÜMESININ kaynak kimliği geçersiz bir biçim kullanıyor.  
**Geçici çözüm** : disk şifreleme KÜMESININ kaynak kimliğinin/Subscriptions/<SubscriptionID \> /ResourceGroups/<resourcegroupname \> /providers/Microsoft.COMPUTE/<diskencryptionsetname biçimini izlediği emin olun \> .

*GalleryArtifactVersion. Properties. publishingProfile. Targetregion. encryption. Osdiskımage. Diskencryptionsetıd parametresinin değeri geçersiz.* 
 **Neden** : işletim sistemi diski görüntüsünde kullanılan disk şifrelemesi KÜMESININ kaynak kimliği geçersiz bir biçim kullanıyor  
**Geçici çözüm** : disk şifreleme KÜMESININ kaynak kimliğinin/Subscriptions/<SubscriptionID \> /ResourceGroups/<resourcegroupname \> /providers/Microsoft.COMPUTE/<diskencryptionsetname biçimini izlediği emin olun \> .

*\> \> Güncelleştirme Galerisi görüntü sürümü isteği için [<Region] bölgesinde bir disk şifrelemesi kümesine sahip yeni veri diski görüntü şifreleme lun [<Number] belirtilemiyor. Bu sürümü güncelleştirmek için yeni LUN 'yi kaldırın. Veri diski görüntü şifreleme ayarlarını değiştirmeniz gerekiyorsa, doğru ayarlarla yeni bir galeri görüntüsü sürümü oluşturmanız gerekir.*  
**Neden** : var olan bir görüntü sürümünün veri diskine şifreleme eklendi. Varolan bir görüntü sürümüne şifreleme eklenemiyor.  
**Geçici çözüm** : yeni bir galeri görüntüsü sürümü oluşturun veya eklenen şifreleme ayarlarını kaldırın.

*Galeri yapıt sürümü kaynağı yalnızca doğrudan storageProfile veya bağımsız işletim sistemi ya da veri diskleri içinde belirtilebilir. Tek bir kaynak türü (Kullanıcı görüntüsü, anlık görüntü, disk, sanal makine) belirtilebilir.*  
**Neden** : kaynak kimliği eksik.  
**Geçici çözüm** : KAYNAĞıN kaynak kimliğinin mevcut olduğundan emin olun.

*Kaynak bulunamadı: RESOURCEID <\> . Lütfen kaynağın mevcut olduğundan emin olun.*  
**Neden** : kaynağın RESOURCEID yanlış olabilir.  
**Geçici çözüm** : kaynağın RESOURCEID 'nin doğru olduğundan emin olun.

*' \_ \> <Diskencryptionsetıd ' adlı disk şifreleme kümesi \> , ' <Region \_ 2 ' bölgesindeki karşılık gelen disk için kullanıldığından, ' <Region ' ' hedef bölgesindeki ' GalleryArtifactVersion. Properties. Publishingprofile. Targetbölgeleriyle. encryption. Osdiskımage. diskencryptionsetıd ' diski için bir disk şifreleme kümesi gerekir \>*  
**Neden** : şifreleme, <bölge 2 ' deki işletim sistemi diskinde kullanılmıştır \_ \> , ancak <bölge 1 ' de \_ kullanılamaz \> .  
**Geçici çözüm** : işletim sistemi diskinde şifreleme kullanılıyorsa, tüm bölgelerde şifreleme kullanın.

*' \> \_ \> \> <Region \_ 2 ' bölgesindeki karşılık gelen disk Için ' <diskencryptionsetıd ' disk şifreleme kümesi kullanıldığından, ' <Region 1 ' hedef bölgesindeki \> ' LUN <numarası ' diski için bir disk şifreleme kümesi gerekir*  
**Neden** : şifreleme \> \_ \> , <bölge 1 ' de değil, <bölge 2 ' deki LUN <numaralı veri diskinde kullanılır \_ \> .  
**Geçici çözüm** : bir veri diskinde şifreleme kullanıyorsanız, tüm bölgelerde şifreleme kullanın.

*\>ENCRYPTION. Datadiskımages içinde geçersiz bir LUN [<Number] belirtildi. LUN, şu değerlerden biri olmalıdır [' 0, 9 '].*  
**Neden** : şifreleme için belirtilen LUN numarası, VM 'ye bağlı disklerin LUN numaralarıyla eşleşmez.  
**Geçici çözüm** : şifrelemedeki LUN numarasını VM 'de mevcut bir veri diskinin LUN numarası olarak değiştirin.

*' \> <Region \> ' şifreleme. Datadiskımages ' ' <number ' yinelenen LUN 'lar belirtildi.*  
**Neden** : <bölgede kullanılan şifreleme ayarları \> en az iki kez bir LUN numarası belirtti.  
**Geçici çözüm** : \> tüm LUN numaralarının <bölgesinde benzersiz olduğundan emin olmak için <bölgesindeki LUN numarasını değiştirin \> .

*Osdiskımage ve Datadiskımage <SourceId ile aynı blob 'a işaret edemez\>*  
**Neden** : işletim sistemi diskinin kaynağı ve en az bir veri diski benzersiz değil.  
**Geçici çözüm** : işletim sistemi diskinin ve/veya veri disklerinin kaynağını değiştirerek, her bir veri diskinin da benzersiz olduğundan emin olun.

*Hedef yayımlama bölgelerinde yinelenen bölgelere izin verilmez.*  
**Neden** : bölge, yayımlama bölgeleri arasında birden fazla kez listeleniyor.  
**Geçici çözüm** : yinelenen bölgeyi kaldırın.

*Yeni veri diskleri ekleme veya var olan bir görüntüdeki veri diskinin LUN 'unu değiştirmeye izin verilmiyor.*  
**Neden** : görüntü sürümüne yönelik bir güncelleştirme çağrısı yeni bir veri diski içeriyor ya da bir disk için yenı bir LUN numarasına sahip.  
**Geçici çözüm** : mevcut görüntü sürümünün LUN numaralarını ve veri disklerini kullanın.

*<Diskencryptionsetıd disk şifreleme kümesi, \> Galeri kaynağıyla aynı abonelikte <aynı abonelikte olmalıdır \> .*  
**Neden** : paylaşılan görüntü Galerisi, farklı bir abonelikte bir disk şifrelemesi kümesini kullanmayı Şu anda desteklemiyor.  
**Geçici çözüm** : aynı abonelikte görüntü sürümünü ve disk şifreleme kümesini oluşturun.

## <a name="issues-creating-or-updating-a-vm-or-scale-sets-from-image-version"></a>Görüntü sürümünden VM veya ölçek kümeleri oluşturma veya güncelleştirme sorunları ##

*İstemci, <RESOURCEID kapsamındaki ' Microsoft. COMPUTE/galeriler/Images/Versions/Read ' eylemini gerçekleştirme iznine sahip \> , ancak geçerli kiracı <tenantId1 \> <subscriptionId2 bağlı aboneliğine erişim yetkisi yok \> .*  
**Neden** : sanal makine veya ölçek kümesi, başka bir KIRACıDA bir SIG görüntüsü kullanılarak oluşturulmuştur. Sanal makinede veya ölçek kümesinde değişiklik yapmaya çalıştınız, ancak görüntünün sahibi olan aboneliğe erişiminiz yok.  
**Geçici çözüm** : görüntü sürümüne okuma erişimi sağlamak için görüntü sürümüne ait aboneliğin sahibine başvurun.

*<RESOURCEID, \> <Region \> bölgesinde kullanılamaz. Lütfen bu bölgeye çoğaltmak için görüntü sahibine başvurun veya istediğiniz bölgeyi değiştirin.*  
**Neden** : VM, Galeri görüntüsü için yayımlanan bölgelerin listesi içinde olmayan bir bölgede oluşturuluyor.  
**Geçici çözüm** : görüntüyü bölgeye çoğaltın ya da Galeri görüntüsünün yayımlama bölgelerindeki bölgelerden bırınde bir VM oluşturun.

*' OsProfile ' parametresine izin verilmiyor.*  
**Neden** : özelleştirilmiş bir görüntü sürümünden oluşturulmuş bir VM için Yönetici Kullanıcı adı, parola veya SSH anahtarları sağlandı.  
**Geçici çözüm** : Bu GÖRÜNTÜDEN bir VM oluşturmak istiyorsanız yönetici kullanıcı adını, parolayı veya SSH anahtarlarını eklemeyin. Aksi takdirde, genelleştirilmiş bir görüntü sürümü kullanın ve Yönetici Kullanıcı adı, parola veya SSH anahtarlarını sağlayın.

*Gerekli ' osProfile ' parametresi eksik (null).*  
**Neden** : VM genelleştirilmiş bir görüntüden oluşturulmuş ve Yönetici Kullanıcı adı, parola veya SSH anahtarları eksik. Genelleştirilmiş görüntüler yönetici kullanıcı adını, parolayı veya SSH anahtarlarını korumadığından, bu alanların bir VM veya ölçek kümesi oluşturulması sırasında belirtilmesi gerekir.  
**Geçici çözüm** : yönetici kullanıcı adını, parolayı veya SSH anahtarlarını belirtin veya özel bir görüntü sürümü kullanın.

*\>Üst Galeri görüntüsündeki (' özelleştirilmiş ') işletim sistemi durumu ' Genelleştirilmiş ' olmadığından, bu öğeden Galeri görüntüsü sürümü oluşturulamıyor: <RESOURCEID.*  
**Neden** : görüntü sürümü genelleştirilmiş bir kaynaktan oluşturulur, ancak üst tanımı özelleştirilmiş olur.  
**Geçici çözüm** : özel bir kaynak kullanarak görüntü sürümü oluşturun ya da genelleştirilmiş bir üst tanımı kullanın.

*\>VM Ölçek kümesinin geçerli işletim sistemi durumu Genelleştirilmiş olduğundan, sanal makine ölçek kümesi <vmssName güncelleştirilemiyor.*  
**Neden** : ölçek kümesi için geçerli kaynak görüntüsü genelleştirilmiş bir kaynak görüntüdür, ancak özelleştirilmiş bir kaynak görüntüyle güncelleştiriliyor. Geçerli kaynak görüntüsü ve bir ölçek kümesi için yeni kaynak görüntüsü aynı durumda olmalıdır.  
**Geçici çözüm** : scaleset 'i güncelleştirmek için genelleştirilmiş bir görüntü sürümü kullanın.

*Paylaşılan görüntü galerisinde <Diskencryptionsetıd adlı disk şifreleme kümesi \> <VersionId, \> abonelik <subscriptionId1 aittir \> ve abonelik <' ' kaynağıyla kullanılamaz subscriptionId2\>*  
**Neden** : görüntü sürümünü şifrelemek için kullanılan disk şifreleme kümesi, görüntü sürümünü barındırmak için abonelikten farklı bir abonelikte bulunuyor.  
**Geçici çözüm** : görüntü sürümü ve disk şifreleme kümesi için aynı aboneliği kullanın.

*VM veya sanal makine ölçek kümesi oluşturma uzun bir süre sürer.*  
**Geçici çözüm** : VM 'yi ya da sanal makine ölçek kümesini oluşturmaya çalıştığınız görüntü sürümünün **OSType** öğesinin görüntü sürümünü oluşturmak Için kullandığınız kaynağın aynı **OSType** öğesine sahip olduğunu doğrulayın. 

## <a name="issues-creating-a-disk-from-an-image-version"></a>Görüntü sürümünden disk oluşturma sorunları ##

*ImageReference parametresinin değeri geçersiz.*  
**Neden** : bir SIG görüntü sürümünden bir diske aktarmaya çalıştınız, ancak görüntüde bulunmayan bir LUN konumu kullandınız.    
**Geçici çözüm** : hangi LUN konumlarının kullanımda olduğunu görmek için görüntü sürümünü denetleyin.

## <a name="unable-to-share-resources"></a>Kaynaklar paylaşılamıyor

[Azure rol tabanlı erişim denetimi (Azure RBAC)](../role-based-access-control/rbac-and-directory-admin-roles.md)kullanılarak, paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümü kaynakları abonelikler genelinde paylaşılır. 

## <a name="replication-is-slow"></a>Çoğaltma yavaş

Belirtilen tüm hedef bölgelere yönelik çoğaltmanın tamamlanıp tamamlanmadığını denetlemek için, ' **ı genişlet ReplicationStatus** bayrağını kullanın. Aksi takdirde, işin tamamlanabilmesi için 6 saate kadar bekleyin. Başarısız olursa, görüntü sürümünü oluşturmak ve çoğaltmak için komutu yeniden tetikleyin. Görüntü sürümünün çoğaltılacağı çok sayıda hedef bölge varsa, çoğaltmayı aşamalar halinde yapmayı göz önünde bulundurun.

## <a name="azure-limits-and-quotas"></a>Azure limitleri ve kotaları 

[Azure Limitleri ve kotaları](../azure-resource-manager/management/azure-subscription-service-limits.md) tüm paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümü kaynakları için geçerlidir. Aboneliklerinizin sınırları dahilinde olduğunuzdan emin olun. 


## <a name="next-steps"></a>Sonraki adımlar

[Paylaşılan görüntü galerileri](./linux/shared-image-galleries.md)hakkında daha fazla bilgi edinin.