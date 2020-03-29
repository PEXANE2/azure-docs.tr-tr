---
title: Azure CDN ile büyük dosya indirme optimizasyonu
description: Bu makalede, büyük dosya karşıyüklemelerinin nasıl optimize edilebildiği açıklanmaktadır.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2018
ms.author: magattus
ms.openlocfilehash: 4fe72985a799595908a0ff6bceb1a73dca823c8f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593777"
---
# <a name="large-file-download-optimization-with-azure-cdn"></a>Azure CDN ile büyük dosya indirme optimizasyonu

Gelişmiş işlevsellik, gelişmiş grafikler ve zengin medya içeriği sayesinde internet üzerinden teslim edilen içeriğin dosya boyutları büyümeye devam eder. Bu büyüme birçok faktörtarafından yönlendirilir: geniş bant penetrasyonu, daha büyük ucuz depolama cihazları, yüksek tanımlı video yaygın artış, ve internete bağlı cihazlar (IoT). Büyük dosyalar için hızlı ve verimli bir dağıtım mekanizması, sorunsuz ve keyifli bir tüketici deneyimi sağlamak için çok önemlidir.

Büyük dosyaların teslimi çeşitli zorluklar vardır. İlk olarak, uygulamalar tüm verileri sırayla karşıdan yüklemeyebileceğinden, büyük bir dosyayı indirmek için ortalama süre önemli olabilir. Bazı durumlarda, uygulamalar ilk bölümden önce bir dosyanın son bölümünü karşıdan yükleyebilir. Yalnızca küçük bir dosya istendiğinde veya bir kullanıcı karşıdan yüklemeyi duraklattığında, karşıdan yükleme başarısız olabilir. İndirme, içerik dağıtım ağı (CDN) tüm dosyayı kaynak sunucudan alana kadar da gecikebilir. 

İkinci olarak, kullanıcının makinesi yle dosya arasındaki gecikme, içeriği görüntüleme hızını belirler. Buna ek olarak, ağ tıkanıklığı ve kapasite sorunları da iş buzunu etkiler. Sunucular ve kullanıcılar arasındaki daha büyük mesafeler, paket kaybının oluşması için ek fırsatlar oluşturur ve bu da kaliteyi azaltır. Sınırlı iş verime ve artan paket kaybından kaynaklanan kalite düşüşü, dosya karşıdan yüklemesinin tamamlanması için bekleme süresini artırabilir. 

Üçüncü olarak, birçok büyük dosya bunların bütünüyle teslim edilmez. Kullanıcılar, uzun bir MP4 videonun yalnızca ilk birkaç dakikasını yarıya indirebilir veya izleyebilir. Bu nedenle, yazılım ve medya dağıtım şirketleri yalnızca istenen bir dosyanın bölümünü teslim etmek istiyorum. İstenen bölümlerin verimli bir şekilde dağıtılması, kaynak sunucusundan çıkış trafiğini azaltır. Verimli dağıtım, kaynak sunucusundaki bellek ve G/Ç basıncını da azaltır. 


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-from-microsoft"></a>Microsoft'tan Azure CDN ile büyük dosyaların teslimi için optimize edin

Microsoft uç **noktalarından Azure CDN Standardı,** dosya boyutunda üst sınırı olmayan büyük dosyalar sunar. Büyük dosyaların teslimini daha hızlı yapmak için ek özellikler varsayılan olarak açık olur.

### <a name="object-chunking"></a>Nesne parçalama 

**Microsoft'tan Azure CDN Standard** nesne ötme adı verilen bir teknik kullanır. Büyük bir dosya istendiğinde, CDN dosyanın daha küçük parçalarını kaynaktan alır. CDN POP sunucusu tam veya bayt aralığı dosya isteği aldıktan sonra, CDN kenar sunucusu dosyayı 8 MB'lık parçalar halinde kaynağından ister. 

Yığın CDN kenarına geldikten sonra önbelleğe alınıp hemen kullanıcıya sunulur. CDN daha sonra bir sonraki parçayı paralel olarak önceden getirir. Bu ön alma, içeriğin kullanıcıdan bir yığın önde kalmasını sağlar ve bu da gecikme süresini azaltır. Bu işlem, tüm dosya indirilene (istenirse), tüm bayt aralıkları kullanılabilir olana (istenirse) veya istemci bağlantıyı sonlandırıncaya kadar devam eder. 

Bayt aralığı isteği hakkında daha fazla bilgi için [RFC 7233'e](https://tools.ietf.org/html/rfc7233)bakın.

CDN, alınan tüm parçaları önbelleğe almaz. Tüm dosyanın CDN önbelleğinde önbelleğe alınması gerekmez. Dosya veya bayt aralıkları için sonraki istekler CDN önbelleğinden sunulur. Tüm parçalar CDN'de önbelleğe alınmamışsa, prefetch kaynaktan parçalar istemek için kullanılır. Bu optimizasyon, başlangıç sunucusunun bayt aralığı isteklerini destekleme yeteneğine dayanır; Kaynak sunucu bayt aralığı isteklerini desteklemiyorsa, bu optimizasyon etkili değildir. 

### <a name="conditions-for-large-file-optimization"></a>Büyük dosya optimizasyonu için koşullar
Genel web teslim optimizasyonu türünü **kullandığınızda, Microsoft'tan Azure CDN Standardı** için büyük dosya optimizasyonu özellikleri varsayılan olarak açıktır. Maksimum dosya boyutunda sınır yoktur.


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-from-verizon"></a>Verizon'dan Azure CDN ile büyük dosyaların teslimi için optimize edin

**Verizon'dan Azure CDN Standardı** ve Verizon uç **noktalarından Azure CDN Premium,** dosya boyutunda bir kapak olmadan büyük dosyalar sunar. Büyük dosyaların teslimini daha hızlı yapmak için ek özellikler varsayılan olarak açık olur.

### <a name="complete-cache-fill"></a>Tam önbellek dolgusu

Varsayılan tam önbellek doldurma özelliği, cdn'nin ilk istek terk edildiğinde veya kaybolduğunda bir dosyayı önbelleğe çekmesini sağlar. 

Tam önbellek dolgusu büyük varlıklar için en yararlıdır. Genellikle, kullanıcılar bunları baştan sona indirmez. Onlar ilerici indirme kullanın. Varsayılan davranış, kenar sunucusunu kaynağı sunucudan varlığın arka plan getirmesini başlatmaya zorlar. Daha sonra, varlık kenar sunucusunun yerel önbelleğindedir. Tam nesne önbellekte olduktan sonra, kenar sunucusu önbelleğe alınmış nesne için CDN'ye bayt aralığı isteklerini yerine getirir.

Varsayılan davranış **Verizon'dan Azure CDN Premium'daki**kurallar altyapısı aracılığıyla devre dışı bilebilir.

### <a name="peer-cache-fill-hot-filing"></a>Eş önbelleği doldurma sıcak dosyalama

Varsayılan eş önbelleği doldurma özelliği, gelişmiş bir özel dosyalama algoritması kullanır. Büyük ve son derece popüler nesneler için istemci isteklerini yerine getirmek için bant genişliği ve toplu istek ölçümlerine dayalı ek kenar önbelleğe alma sunucuları kullanır. Bu özellik, çok sayıda ek isteğin kullanıcının başlangıç sunucusuna gönderildiği bir durumu önler. 

### <a name="conditions-for-large-file-optimization"></a>Büyük dosya optimizasyonu için koşullar

Genel web teslim optimizasyonu türünü kullandığınızda **Verizon'dan Azure CDN Standardı** ve **Verizon'dan Azure CDN Premium** için büyük dosya optimizasyonu özellikleri varsayılan olarak açıktır. Maksimum dosya boyutunda sınır yoktur. 


## <a name="optimize-for-delivery-of-large-files-with-azure-cdn-standard-from-akamai"></a>Akamai'den Azure CDN Standardı ile büyük dosyaların teslimi için optimize edin

Akamai profil uç **noktalarından Azure CDN Standardı,** büyük dosyaları ölçekte dünya çapındaki kullanıcılara verimli bir şekilde sunan bir özellik sunar. Özellik, kaynak sunuculardaki yükü azalttığından gecikme gevellerini azaltır.

Büyük dosya optimizasyonu türü özelliği, büyük dosyaları daha hızlı ve daha hızlı bir şekilde sunmak için ağ optimizasyonlarını ve yapılandırmaları açar. **Akamai uç noktalarından Azure CDN Standard** ile genel web teslimi dosyaları yalnızca 1,8 GB'ın altında önbelleğe alabilir ve 150 GB'a kadar dosya tünel (önbellek değil) olabilir. Büyük dosya optimizasyonu dosyaları 150 GB'a kadar önbelleğe aldayır.

Belirli koşullar yerine getirildiğinde büyük dosya optimizasyonu etkilidir. Koşullar, kaynak sunucunun nasıl çalıştığını ve istenen dosyaların boyutlarını ve türlerini içerir. 

### <a name="configure-an-akamai-cdn-endpoint-to-optimize-delivery-of-large-files"></a>Büyük dosyaların teslimini en iyi duruma getirmek için akamai CDN bitiş noktasını yapılandırma

Azure portalı üzerinden büyük dosyalar için teslimi optimize etmek için **Azure CDN Standardınızı Akamai** bitiş noktasından yapılandırabilirsiniz. Bunu yapmak için REST API'lerini veya istemci SDK'larından herhangi birini de kullanabilirsiniz. Aşağıdaki adımlar, **Akamai profilinden** bir Azure CDN Standardı için Azure portalı üzerinden işlemi gösterir:

1. Akamai **CDN profil** sayfasında yeni bir bitiş noktası eklemek için **Bitiş Noktası'nı**seçin.

    ![Yeni bitiş noktası](./media/cdn-large-file-optimization/cdn-new-akamai-endpoint.png)    
 
2. Açılan liste **için Optimize Edilmiş** **listede, Büyük dosya karşıdan yükleme'yi**seçin.

    ![Büyük dosya optimizasyonu seçili](./media/cdn-large-file-optimization/cdn-large-file-select.png)


CDN bitiş noktasını oluşturduktan sonra, belirli ölçütlerle eşleşen tüm dosyalar için büyük dosya optimizasyonları uygular. Aşağıdaki bölümde bu işlem açıklanmaktadır.

### <a name="object-chunking"></a>Nesne parçalama 

**Akamai'den Azure CDN Standard** ile büyük dosya optimizasyonu nesne öbeklenmesi adı verilen bir teknik kullanır. Büyük bir dosya istendiğinde, CDN dosyanın daha küçük parçalarını kaynaktan alır. CDN POP sunucusu tam veya bayt aralığı dosya isteği aldıktan sonra, dosya türünün bu optimizasyon için desteklenip desteklenmediğini denetler. Ayrıca, dosya türünün dosya boyutu gereksinimlerini karşılayıp karşılamadığını da denetler. Dosya boyutu 10 MB'dan büyükse, CDN kenar sunucusu dosyanın kaynağını 2 MB'lık parçalar halinde ister. 

Yığın CDN kenarına geldikten sonra önbelleğe alınıp hemen kullanıcıya sunulur. CDN daha sonra bir sonraki parçayı paralel olarak önceden getirir. Bu ön alma, içeriğin kullanıcıdan bir yığın önde kalmasını sağlar ve bu da gecikme süresini azaltır. Bu işlem, tüm dosya indirilene (istenirse), tüm bayt aralıkları kullanılabilir olana (istenirse) veya istemci bağlantıyı sonlandırıncaya kadar devam eder. 

Bayt aralığı isteği hakkında daha fazla bilgi için [RFC 7233'e](https://tools.ietf.org/html/rfc7233)bakın.

CDN, alınan tüm parçaları önbelleğe almaz. Tüm dosyanın CDN önbelleğinde önbelleğe alınması gerekmez. Dosya veya bayt aralıkları için sonraki istekler CDN önbelleğinden sunulur. Tüm parçalar CDN'de önbelleğe alınmamışsa, prefetch kaynaktan parçalar istemek için kullanılır. Bu optimizasyon, başlangıç sunucusunun bayt aralığı isteklerini destekleme yeteneğine dayanır; Kaynak sunucu bayt aralığı isteklerini desteklemiyorsa, bu optimizasyon etkili değildir.

### <a name="caching"></a>Önbelleğe alma
Büyük dosya optimizasyonu, genel web tesliminden farklı varsayılan önbelleğe alma süresi kullanır. HTTP yanıt kodlarına göre pozitif önbelleğe alma ve negatif önbelleğe alma arasında ayrım. Kaynak sunucu önbellek denetimi yoluyla bir son kullanma süresi belirtirse veya yanıtta üstbilginin süresi doluyorsa, CDN bu değeri onurlandırır. Kaynak belirtilmediğinde ve dosya bu optimizasyon türü için tür ve boyut koşullarıyla eşleşmiyorsa, CDN büyük dosya optimizasyonu için varsayılan değerleri kullanır. Aksi takdirde, CDN genel web teslimi için varsayılanları kullanır.


|    | Genel web | Büyük dosya iyileştirmesi 
--- | --- | --- 
Önbelleğe alma: Pozitif <br> HTTP 200, 203, 300, <br> 301, 302 ve 410 | 7 gün |1 gün  
Önbelleğe alma: Negatif <br> HTTP 204, 305, 404, <br> ve 405 | None | 1 saniye 

### <a name="deal-with-origin-failure"></a>Menşe hatasıyla başa çık

Genel web teslimi için iki saniyeden büyük dosya optimizasyonu türü için iki dakikaya kadar başlangıç okuma-zaman ayırma uzunluğu artar. Bu artış, erken zaman alamasını önlemek için daha büyük dosya boyutlarını hesaplar.

Bağlantı zaman ları bittiğinde, CDN istemciye "504 - Ağ Geçidi Zaman Kaçaması" hatası göndermeden önce birkaç kez yeniden çalışır. 

### <a name="conditions-for-large-file-optimization"></a>Büyük dosya optimizasyonu için koşullar

Aşağıdaki tabloda, büyük dosya optimizasyonu için karşılanacak ölçütler kümesi listelenebvardır:

Koşul | Değerler 
--- | --- 
Desteklenen dosya türleri | 3g2, 3gp, asf, avi, bz2, dmg, exe, f4v, flv, <br> gz, hdp, iso, jxr, m4v, mkv, mov, mp4, <br> mpeg, mpg, mts, pkg, qt, rm, swf, katran, <br> tgz, wdp, webm, webp, wma, wmv, zip  
Minimum dosya boyutu | 10 MB 
En büyük dosya boyutu | 150 GB 
Kaynak sunucu özellikleri | Bayt aralığı isteklerini desteklemeli 

## <a name="additional-considerations"></a>Diğer konular

Bu optimizasyon türü için aşağıdaki ek yönleri göz önünde bulundurun:

- Yığınlama işlemi, kaynak sunucuya ek istekler oluşturur. Ancak, kaynaktan teslim edilen verilerin toplam hacmi çok daha küçüktür. Yığınlama, CDN'de daha iyi önbelleğe alma özellikleriyle sonuçlanır.

- Dosyanın daha küçük parçaları teslim olduğundan bellek ve G/Ç basıncı kaynağında azalır.

- CDN'de önbelleğe alınan parçalar için, içeriğin süresi dolana veya önbellekten çıkarılana kadar kaynak için ek istek yoktur.

- Kullanıcılar, normal bir dosya gibi işlem gören CDN'ye aralık isteklerinde bulunabilir. Optimizasyon yalnızca geçerli bir dosya türüyse ve bayt aralığı 10 MB ile 150 GB arasındaysa geçerlidir. İstenen ortalama dosya boyutu 10 MB'dan küçükse, bunun yerine genel web teslimini kullanın.

