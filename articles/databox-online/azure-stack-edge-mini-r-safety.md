---
title: Azure Stack Edge Mini güvenlik kılavuzu | Microsoft Docs
description: Güvenlik kuralları, yönergeler, hususlar ve Azure Stack Edge Mini R cihazınızı güvenli bir şekilde yüklemeyi ve çalıştırmayı açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: conceptual
ms.date: 10/13/2020
ms.author: alkohli
ms.openlocfilehash: 507ceef0f13951eafdcb02d586f35c1d61764c4e
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2020
ms.locfileid: "96467992"
---
# <a name="azure-stack-edge-mini-r-safety-instructions"></a>Azure Stack Edge Mini güvenlik yönergeleri

![Uyarı simgesi 1 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ Okuma Güvenlik BILDIRIMI SIMGESI ](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png)
 **güvenlik ve durum bilgilerini oku**

Azure Stack Edge Mini R cihazınızı, tek bir pil paketinin bir oluşumunu, bir AC/DC takılmış güç kaynağını, bir modül güç bağdaştırıcısını ve bir sunucu modülünü kullanmadan önce bu makaledeki tüm güvenlik bilgilerini okuyun. Yönergeleri izlemeden hata tetiklenmesi, elektrik kesintisi, sakatlama veya özelliklerinizi zarar verebilir. Azure Stack Edge Mini R kullanmadan önce aşağıdaki tüm güvenlik bilgilerini okuyun.

## <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları

Hasar uyarısı işaretlerine yönelik aşağıdaki sinyal sözcükleri şunlardır:

| Simge | Açıklama |
|:--- |:--- |
| ![Hasar simgesi](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)| **Danger:** Önedilmediği durumlarda, ölüm veya ciddi bir yaralama oluşmasına neden olan tehlikeli bir durum belirtir. <br> **Uyarı:** Önedilmediği durumlarda, ölüm veya ciddi bir yaralama oluşmasına neden olabilecek tehlikeli bir durum belirtir. <br> **Dikkat:** Önedilmediği durumlarda küçük veya orta bir yaralanmaya neden olabilecek tehlikeli bir durum belirtir.|
|

Azure Stack Edge Mini R cihazınızı ayarlarken ve çalıştırırken aşağıdaki rastlantı simgeleri gözlemlenecek:

| Simge | Açıklama |
|:--- |:--- |
| ![Önce tüm yönergeleri okuyun](./media/azure-stack-edge-mini-r-safety/icon-safety-read-all-instructions.png) | Önce tüm yönergeleri okuyun |
| ![Hasar simgesi](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) | Hasar simgesi |
| ![Elektrik sarsıntı simgesi](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) | Elektrik şok hasar |
| ![Yalnızca iç kullanım](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) | Yalnızca iç kullanım |
| ![Kullanıcı Serviceable parçası yok simgesi](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) | Hiçbir Kullanıcı hizmet konusu parçası yok. Uygun şekilde Eğitilmediğiniz müddetçe bu erişimi kullanmayın. |
|

## <a name="handling-precautions-and-site-selection"></a>Önlemler ve site seçimini işleme

Azure Stack Edge Mini R cihazının aşağıdaki işleme önlemleri ve site seçim ölçütleri vardır:

![Uyarı simgesi 2 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ elektrik sarsıntı simgesi ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
 ![ hiçbir Kullanıcı hizmet halinde bölüm simgesi yok ](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **:**

* Zararlar için *alınan as* cihazını inceleyin. Cihaz kasası hasar görmüşse, yerini almak için [Microsoft desteği başvurun](azure-stack-edge-placeholder.md) . Cihazı çalıştırmayı denemeyin.
* Cihazın arızalı olduğunu kuşkulanıyorsanız, bir değiştirme edinmek için [Microsoft desteği başvurun](azure-stack-edge-placeholder.md) . Cihaza bakım yapmayı denemeyin.
* Cihaz, hiçbir Kullanıcı-serviceable Bölümü içermiyor. Tehlikeli voltaj, geçerli ve enerji düzeyleri içinde mevcuttur. ' İ açmayın. Cihaza bakım için Microsoft 'a döndürün.

![Uyarı simgesi 3 Uyarı ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **:**

Sistemin çalışması önerilir:

* Doğrudan güneş ve kdiators dahil olmak üzere ısı kaynaklarından uzakta.
* Nemi veya yağmur olarak görünmeyen konumlarda.
* Titreşimi ve fiziksel sarsıntı en aza indiren bir alanda bulunur.  Sistem, MIL-STD-810G 'e göre darbe ve titreşim için tasarlanmıştır.
* Elektrik cihazları tarafından üretilen güçlü elektromanyetik alanlardan yalıtılmıştır.
* Herhangi bir likit veya herhangi bir yabancı nesnenin sisteme girmesine izin vermeyin. Meşruları veya herhangi bir diğer likit kapsayıcıyı sisteme veya yanına yerleştirmeyin.

![Uyarı simgesi 4 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ Kullanıcı Serviceable bölümü yok simgesi ](./media/azure-stack-edge-mini-r-safety/icon-safety-do-not-access.png) **Uyarı:**

* Bu ekipman bir lityum pil içerir. Pil paketine hizmet vermeyi denemeyin. Bu ekipmandaki piller Kullanıcı serviceable değildir. Pil, yanlış bir türle değiştirilirse açılım riski.

![Uyarı simgesi 5 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **Uyarı:**

Yalnızca Azure Stack Edge Mini R cihazının bir parçası olduğunda pil paketini ücretlendirin, ayrı bir cihaz olarak ücretlendirmeyin.

![Uyarı simgesi 6 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **dikkat:**

* Pil paketindeki açık/kapalı anahtarı, pili yalnızca sunucu modülüne borçlandırmayı sağlar. Güç bağdaştırıcısı pil paketine takılıysa, anahtar kapalı konumundayken bile sunucu modülüne güç geçirilir.

![Uyarı simgesi 7 Uyarı ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **:**

* Pil paketini yazma veya kısa devre vermeyin. Geri dönüştürülüp doğru bir şekilde atılmalıdır.

![Uyarı simgesi 8 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **Uyarı:**

* Bu sistemde, belirtilen AC/DC güç kaynağını kullanmanın yanı sıra, 2590 pil türünde bir alan kullanma seçeneği de bulunur. Bu durumda, son kullanıcı tüm ilgili güvenliği, ulaşım, çevresel ve diğer ulusal/yerel düzenlemeleri karşıladığını doğrular.
* Sistemi 2590 pil türü ile çalıştırırken, pil üreticisi tarafından belirtilen kullanım koşulları dahilinde pili çalışır.

![Uyarı simgesi 9 Uyarı ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) **:**

Bu cihazda, optik alıcı vericiler ile kullanılabilecek iki SFP + bağlantı noktası bulunur. Tehlikeli lazer yarıçapmasını önlemek için yalnızca sınıf 1 alıcı vericileri kullanın.

## <a name="electrical-precautions"></a>Elektrik önlemleri

Azure Stack Edge Mini R cihazının aşağıdaki elektrik önlemleri vardır:

![Uyarı simgesi 10 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) ![ elektrik sarsıntısı simgesi ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **uyarısı:**

Güç kaynağı bağdaştırıcısı ile birlikte kullanıldığında:

* Güç kaynağı kablosu ile güvenli bir elektrik dünya bağlantısı sağlayın. Değişen geçerli (AC) kabloda üç telli bir grounding eklentisine (bir grounding PIN 'i olan bir tak) sahiptir. Bu eklenti yalnızca bir topraklanmış AC prizine uyar. Grounding PIN 'inin amacını ertelemeyin.
* Güç kaynağı kablosu ana bağlantı kesme aygıtı olduğu için, dış cihazların cihazın yakınında bulunduğundan ve kolayca erişilebilir olduğundan emin olun.
* Aşağıdaki koşullardan herhangi biri mevcutsa, güç kablolarını (yani, bu eklentiyi çekerek) çıkarın ve tüm kabloların bağlantısını kesin:

  * Güç kablosu veya plug, Frayed veya hasarlı olur.
  * Cihaz yağmur, daha fazla Moisture veya diğer lılar tarafından sunulur.
  * Cihaz bırakılmış ve cihazın büyük küçük harfleri bozulmuş.
  * Cihazın hizmet veya onarım ihtiyacı olduğunu şüpheli.
* Birimi Taşımadan önce kalıcı olarak çıkarın veya herhangi bir şekilde hasar gördüğünü düşünün.

* Aşağıdaki güç belirtimlerini karşılamak için elektrik aşırı yüklemesi koruması ile uygun bir güç kaynağı sağlayın:

* Voltaj: 100-240 volt AC
* Geçerli: 1,7 Amperes
* Sıklık: 50 ila 60 Hz

![Uyarı simgesi 11 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ elektrik darbe simgesi ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png) **uyarısı:**

* Ekipman ile sağlandıklardan farklı AC güç kablosu (ler) i değiştirmeyi veya kullanmayı denemeyin.

![Uyarı simgesi 12 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png)
 ![ elektrik sarsıntı simgesi ](./media/azure-stack-edge-mini-r-safety/icon-safety-electric-shock.png)
 ![ yalnızca uyarı kullanımı ](./media/azure-stack-edge-mini-r-safety/icon-safety-indoor-use-only.png) **:**

* Bu sembolle etiketlenmiş güç kaynağı yalnızca iç kullanım için derecelendirilir.

## <a name="regulatory-information"></a>Mevzuat bilgileri

Aşağıda Azure Stack Edge Mini R cihazı, mevzuat model numarası: TMA01 için yasal bilgiler yer almaktadır.

Azure Stack Edge Mini R cihazı, NRTL listelenmiş (UL, CSA, ETL, vb.) ve ıEC/EN 60950-1 ya da ıEC/EN 62368-1 uyumlu (CE işaretli) Information Technology donatısı ile kullanılmak üzere tasarlanmıştır.

ABD ve Kanada dışındaki ülkelerde ağ kabloları (ekipman ile sağlanmayan), uzunlukları 3 metreden fazlaysa bu ekipmanla birlikte yüklenmez.

Ekipman, aşağıdaki ortamlarda çalışacak şekilde tasarlanmıştır:

| Ortam | Belirtimler |
|:---  |:--- |
| Sistem sıcaklık belirtimleri | <ul><li>Depolama sıcaklığı: – 20 &deg; c – 50 &deg; c (– 4 &deg; f-122 &deg; f)</li><li>Sürekli işlem: 0 &deg; c – 40 &deg; c (32 &deg; f – 104 &deg; F)</li></ul> |
| Bağıl nem (RH) belirtimleri | <ul><li>Depolama: %5 ile %95 bağıl nem</li><li>Çalışan: %90 bağıl nem %10</li></ul>|
| En fazla yükseklik belirtimleri | <ul><li>Çalışma: 15.000 fit (4.572 ölçüm)</li><li>Çalışma dışı: 40.000 fit (12.192 ölçü)</li></ul>|

> ![Not simgesi ](./media/azure-stack-edge-mini-r-safety/icon-safety-notice.png) **bildirimi:** &nbsp; Microsoft tarafından açıkça onaylanmamış ekipman üzerinde yapılan değişiklikler veya değişiklikler, kullanıcının donatımını çalıştırma yetkisini geçersiz edebilir.

Kanada ve ABD:

Uyarı: Bu ekipman test edilmiştir ve bir sınıftaki dijital bir cihaz için sınırlara uyum sağlamak ve bu da FCC kuralların 15. bölümünde yer Bu sınırlar, ekipman ticari bir ortamda çalıştırıldığında zararlı girişim için makul bir koruma sağlamak üzere tasarlanmıştır. Bu ekipman radyo sıklığı enerjisi oluşturur ve kullanır ve yönerge kılavuzuna uygun şekilde yüklenip kullanılmayacaksa radyo iletişimine zararlı girişim yapılmasına neden olabilir. Bu ekipmanın bir yöresel alanındaki işlemi, zararlı bir girişim oluşmasına neden olur ve bu durumda kullanıcının, girişim kendi masrafına göre düzeltilmesi gerekecektir.

Bu cihaz, FCC kuralları ve sektör Kanada lisans muafiyeti RSS standardının 15. bölümüne uyar. İşlem şu iki koşula tabidir: (1) Bu cihaz zararlı bir girişim oluşmasına neden olabilir ve (2) Bu cihaz, cihazın istenmeyen çalışmasına neden olabilecek girişim dahil olmak üzere, alınan bir girişimi kabul etmelidir.

![Mevzuat bilgileri uyarısı 1](./media/azure-stack-edge-mini-r-safety/regulatory-information-1.png)

ICES-3 (A)/NMB-3 (A) Microsoft Corporation, One Microsoft Way, Redmond, WA 98052, USA.
Birleşik Devletler: (800) 426-9400 Kanada: (800) 933-4750

Avrupa BIRLIĞI: uygunluk bildiriminin bir kopyasını Isteyin.

> ![Uyarı simgesi 13 ](./media/azure-stack-edge-mini-r-safety/icon-safety-warning.png) , bir ürün sınıfıdır. Yurtiçi bir ortamda, bu ürün radyo girişimine neden olabilir ve bu durumda kullanıcının yeterli ölçüler yapması gerekebilir.

Çöp pillerinin ve elektrik ve elektronik ekipmanın elden çıkarılması:

![Uyarı simgesi 14](./media/azure-stack-edge-mini-r-safety/icon-ewaste-disposal.png)

Üründe veya pillerinde bulunan bu sembol, bu ürünün ve içerdiği tüm pillerin, ev isverünüzle birlikte atılmayacağı anlamına gelir. Bunun yerine, pil ve elektrik ve elektronik ekipmanın geri dönüştürülmesi için bunu ilgili bir koleksiyon noktasına ele almak sizin sorumluluğunuzdadır. Bu ayrı koleksiyon ve geri dönüşüm, doğal kaynakların korunmasına yardımcı olur ve pillerde ve elektrik ve elektronik ekipmanda olası tehlikeli bir etkinlik olması nedeniyle, uygun olmayan bir elden çıkarılmasından kaynaklanabilir. Pillerinizi ve elektrik ve elektronik alımlarınızı nereden kapatabileceğiniz hakkında daha fazla bilgi için lütfen yerel şehriniz/municiplik ofisiniz, ev atık servisi hizmeti veya bu ürünü satın aldığınız mağaza ekibine başvurun. erecycle@microsoft.comWEEE hakkında daha fazla bilgi için iletişim kurun.

Bu ürün, para hücresi pili (lar) içerir.
Microsoft Irlanda Sandyford IND EST Dublin D18 KX32 Gu telefon numarası: + 353 1 295 3826 Faks numarası: + 353 1 706 4110

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Stack Edge Mini R 'yi dağıtmaya hazırlanma](azure-stack-edge-mini-r-deploy-prep.md)
