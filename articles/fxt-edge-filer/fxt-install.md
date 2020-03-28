---
title: 'Öğretici: Azure FXT Edge Filer fiziksel aygıtı yükleme'
description: Microsoft Azure FXT Edge Filer hibrit depolama önbelleğinin fiziksel aygıt bileşenini nasıl boşaltılır, rafa gerekir ve kabloyla kablosuzdur?
author: ekpgh
ms.author: rohogue
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 07/01/2019
ms.openlocfilehash: 7c77e25d0dcabc49f2e6672645c6bc41e8662ec8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75551038"
---
# <a name="tutorial-install-azure-fxt-edge-filer"></a>Öğretici: Azure FXT Edge Filer'ı yükleyin 

Bu öğretici, Azure FXT Edge Filer karma depolama önbelleği için donanım düğümünün nasıl yüklenir olduğunu açıklar. Bir Azure FXT Edge Filer kümesi oluşturmak için en az üç donanım düğümü yüklemeniz gerekir.

Kurulum prosedürü, cihazın ambalajını açma ve rafa takmayı ve kablo yönetim kolunu (CMA) ve ön çerçeveyi takmayı içerir. Ayrı bir öğretici ağ kabloları ve bağlantı gücü ekleme açıklar. 

Bir Azure FXT Edge Filer düğümü yüklemek yaklaşık bir saat sürer. 

Bu öğretici, şu kurulum adımlarını içerir: 

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa monte edin
> * Ön çerçeveyi kurun (isteğe bağlı)

## <a name="installation-prerequisites"></a>Yükleme önkoşulları 

Başlamadan önce, kullanacağınız veri merkezi nin ve rafın şu özelliklere sahip olduğundan emin olun:

* Cihazı monte etmeyi planladığınız rafta mevcut bir 1U yuvası.
* Azure FXT Edge Filer'ın gereksinimlerini karşılayan AC güç kaynağı ve soğutma sistemleri. (Kurulum planlama ve boyutlandırma yardımcı olmak için Güç ve [termal özellikleri](fxt-specs.md#power-and-thermal-specifications) okuyun.)  

  > [!NOTE] 
  > İki yedek güç kaynağı ünitesinden (PSU) tam olarak yararlanmak için AC güç takarken iki farklı dal devresindeki güç dağıtım ünitelerini kullanın. Ayrıntılar için [Connect güç kablolarını](fxt-network-power.md#connect-power-cables) okuyun.  

## <a name="unpack-the-hardware-node"></a>Donanım düğümündeki leri açma 

Her Azure FXT Kenar Filer düğümü tek bir kutuda sevk edilir. Aygıtı açmak için bu adımları tamamlayın.

1. Kutuyu düz ve sabit bir yüzeye yerleştirin.

2. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Kutu veya ambalaj ciddi şekilde hasar görmüşse, açmayın. Cihazın iyi durumda olup olmadığının değerlendirilmesi için Microsoft Desteği ile iletişim kurun.

3. Kutuyu açın. Aşağıdaki öğeleri içerdiğinden emin olun:
   * Tek bir muhafaza FXT cihazı
   * İki güç kablosu
   * Bir ön çerçeve ve anahtar
   * Bir ray kiti montajı
   * Bir kablo yönetim kolu (CMA)
   * CMA kurulum talimatları kitapçığı
   * Raf yükleme talimatları kitapçığı
   * Güvenlik, Çevre ve Mevzuat Bilgisi kitapçığı

Listelenen maddelerin tümünün alamamışsa, destek için aygıt satıcısına başvurun. 

Aygıtıyüklemeden veya açmadan önce odayla aynı sıcaklığa ulaşmak için yeterli zamanı olduğundan emin olun. Aygıtın herhangi bir yerinde yoğuşma fark ederseniz, yüklemeden önce en az 24 saat bekleyin.

Bir sonraki adım, cihazınıza monte raf etmektir.

## <a name="rack-the-device"></a>Cihazı rafa yerleştirme

Azure FXT Edge Filer aygıtı standart 19 inç rafa yüklenmelidir. 

Azure FXT Edge Filer hibrit depolama önbelleği üç veya daha fazla Azure FXT Edge Filer aygıtından oluşur. Sisteminizin bir parçası olan her cihaz için raf yükleme adımlarını tekrarlayın. 

### <a name="rack-install-prerequisites"></a>Raf yükleme ön koşulları

* Başlamadan önce, cihazınızla birlikte gönderilen Güvenlik, Çevre ve Düzenleyici Bilgiler kitapçığındaki güvenlik talimatlarını okuyun.

  > [!NOTE]
  > Düğümü kaldırırken, bir rafa taktığınız veya raftan kaldırdığınızda da dahil olmak üzere her zaman iki kişi kullanın. 

* Ekipman rafınızda kullanılan ray tesisatının türünü belirleyin. 
  * Kare veya yuvarlak delikli snap-in raflar için, aletsiz ray talimatlarına uyun.
  * Dişli delik rafları için, takımlı ray talimatlarına uyun. 
  
    Takımlı ray montaj konfigürasyonu için, tip 10-32, 12-24, M5 veya M6 olmak üzere sekiz vida sağlamanız gerekir. Vidaların baş çapı 10 mm'den (0,4") az olmalıdır.

### <a name="identify-the-rail-kit-contents"></a>Demiryolu kiti içeriğini belirleme

Ray kiti montajını kurmak için bileşenleri bulun:

1. İki Adet A7 Dell ReadyRailS II sürgülü ray montajı (1)
1. İki kanca ve döngü kayışları (2)

![Ray kiti içeriğinin numaralı çizimi](media/fxt-install/identify-rail-kit-contents-400.png)

### <a name="rail-assembly---tool-less-rails-square-hole-or-round-hole-racks"></a>Ray montajı - aletsiz raylar (Kare delikli veya yuvarlak delikli raflar)

Kare veya yuvarlak delikleri olan raflar için rayları monte etmek ve takmak için bu yordamı uygulayın. 

1. Sol ve sağ ray uç parçalarını öne bakacak **şekilde ÖN** etiketli konumlandırın. Her bir uç parçayı, dikey raf flanşlarının ön tarafındaki deliklere yer olacak şekilde yerleştirin. (1)

2. Her bir uç parçayı, monte etmek istediğiniz alanda rafın alt ve üst deliklerine hizalayın. (2)

3. Rayın arka ucunu, dikey raf flanşına tamamen oturana ve mandal yerine tıklayana kadar titreyin. Bu adımları dikey raf flanşına konumlandırmak ve ön uç parçasını oturtmak için tekrarlayın. (3)

> [!TIP]
> Rayları çıkarmak için, son parça orta noktasındaki mandal serbest bırakma düğmesini çekin (4) ve her rayı kaldırın.

![Aletsiz rayların takılması ve kaldırılması diyagramı, basamaklar numaralanmış](media/fxt-install/installing-removing-tool-less-rails-400.png)

### <a name="rail-assembly---tooled-rails-threaded-hole-racks"></a>Ray montajı - takımlı raylar (Dişli delik rafları)

Dişli delikleri olan raflar için, rayları monte etmek ve takmak için bu yordamı uygulayın.

1. Pimleri ön ve arka montaj braketlerinden düz uçlu bir tornavida ile çıkarın. (1)
1. Montaj braketlerinden çıkarmak için ray mandal alt montajlarını çekin ve döndürün. (2)
1. İki çift vida kullanarak sol ve sağ montaj raylarını ön dikey raf flanşlarına takın. (3)
1. Sol ve sağ arka braketleri arka dikey raf flanşlarına doğru ileri doğru kaydırın ve iki çift vida kullanarak takın. (4)

![Raylar numaralanmış rayların takılması ve çıkarılması diyagramı](media/fxt-install/installing-removing-tooled-rails-400.png)

### <a name="install-the-system-in-the-rack"></a>Sistemi rafa kurun

Azure FXT Edge Filer aygıtını rafa monte etmek için aşağıdaki adımları izleyin.

1. İç slayt raylarını yerine kilitlenene kadar raftan çekin. (1)
1. Cihazın her iki tarafındaki arka ray standoffunu bulun ve slayt montajlarındaki arka J yuvalarına indirin. (2) 
1. Tüm ray soğuklukları J yuvalarına oturana kadar cihazı aşağı doğru döndürün. (3)
1. Kilit kolları yerine tıklayana kadar cihazı içe doğru itin.
1. Her iki raydaki slayt yayın kilidi düğmelerine basın (4) ve cihazı rafa kaydırın.

![Adımları numaralanmış bir raf diyagramına sistemi yükleme](media/fxt-install/installing-system-rack-400.png)

### <a name="remove-the-system-from-the-rack"></a>Sistemi raftan kaldırın

Aygıtı raftan kaldırmak için bu yordamı uygulayın. 

1. İç rayların yanlarında kilit kollarını bulun (1).
2. Her kolu serbest bırakma konumuna getirerek kilidini açın (2).
3. Sistemin kenarlarını sıkıca kavrayın ve ray soğuklukları J yuvalarının önünde olana kadar öne çekin. Sistemi raftan yukarı ve uzağa kaldırın ve düz bir yüzeye yerleştirin (3).

![Bir sistemi raftan çıkarma, basamaklar numaralanmış çizim](media/fxt-install/removing-system-rack-400.png)

### <a name="engage-the-slam-latch"></a>Slam mandalı devreye alın

1. Ön tarafa bakacak şekilde, sistemin her iki tarafındaki slam mandalını (1) bulun.
2. Sistem rafa itildikçe mandallar otomatik olarak devreye girer. 

Sistemi çıkarırken mandalları serbest bırakmak için yukarı çekin (2).

İsteğe bağlı sabit vidalar sevkiyat için rafa veya diğer kararsız ortamlarda sistemi güvenli hale getirmek için sağlanır. Her mandal altında vida bulmak ve #2 Phillips tornavida (3) ile sıkın.

![Slam mandalının çekici ve serbest bırakılmasının numaralı çizimi](media/fxt-install/engaging-releasing-slam-latch-400.png)

### <a name="install-the-cable-management-arm"></a>Kablo yönetim kolunu tonuyla tonuyla 

FXT Edge Filer ile isteğe bağlı kablo yönetim kolu (CMA) sağlanır. Yüklemek için yazdırılan talimatlar pakette sağlanır. 

1. Kablo yönetim kolu kitinin bileşenlerini boşaltın ve tanımlayın:
   * CMA tepsi (1)
   * CMA (2)
   * Naylon kablo kravat sarar (3)
   * CMA eki braketleri (4)
   * Durum göstergesi kablosu (5) 

   > [!TIP] 
   > RAFTA sevkiyat için CMA'yı güvence altına almak için, kravatı hem sepetlerin hem de tepsinin etrafına sarıp sıkıca sıkıştırın. CMA'nın bu şekilde güvence altına alınması, sisteminizi kararsız ortamlarda da güvence altına alacaktır.

   ![CMA parçalarının illüstrasyonu](media/fxt-install/cma-kit-400.png)

2. CMA tepsisini töşeyin.

   CMA tepsisi destek sağlar ve CMA için bir tutucu görevi görür. 

   1. Rayların iç kenarlarındaki alıcı braketleri ile tepsinin her iki tarafını hizalayın ve titretin. 
   1. Tepsiyerine tıklanınceye kadar ileri itin. (1)
   1. Tepsiyi çıkarmak için mandal bırakma düğmelerini merkeze doğru sıkıştırın ve tepsiyi alıcı braketinden çıkarın (2).

   ![CMA tepsi kurulumu İllüstrasyonu](media/fxt-install/cma-tray-install-400.png)

3. CMA eki parantezini yükleyin. 

   > [!NOTE]
   >
   > * Kabloları sistemden nasıl yönlendirebileceğinize bağlı olarak CMA'yı sağ veya sol montaj rayına takabilirsiniz. 
   > * Kolaylık sağlamak için CMA'yı güç kaynaklarının (A tarafı) karşısındaki tarafa monte edin. B tarafına monte edilmişse, dış güç kaynağını çıkarmak için CMA'nın bağlantısı kesilmelidir. 
   > * Güç kaynaklarını çıkarmadan önce tepsiyi her zaman çıkarın. 

   ![CMA braket kurulumu İllüstrasyonu](media/fxt-install/cma-bracket-l-r-install-400.png)

   1. CMA'yı monte etmek istediğiniz taraf için uygun CMA eki ayraçını seçin (B veya A tarafı).
   1. CmA eki braketini, slayt rayının arkasına ilgili A veya yan B işaretiyle titretin.
   1. Braketteki delikleri kaydırma rayındaki iğnelerle hizala. Yerine kilitlenir kadar parantez aşağı itin. 

4. CMA'yı yükleyin.

   1. Sistemin arka kısmında, mandal devreye girene kadar cma'nın ön ucuna slayt tertibatının en iç teki braketi üzerine tonuyla tonuyla tonuyla tonuyla tonulayın (1). 
   1. Diğer mandayı en dıştaki braketin ucuna, mandal devreye girene kadar titretin (2). 
   1. CMA'yı kaldırmak için, iç ve dış mandal gövdelerinin üst kısmındaki CMA serbest bırakma düğmelerine basarak her iki mandalı da çıkarın (3).

   ![Ana CMA kurulumunun illüstrasyonu](media/fxt-install/cma-install-400.png)

   CMA erişim ve hizmet için sistemden uzağa döndürülebilir. Menteşeli ucunda, CMA'yı tepsiden uzaklaştırarak koltuğunu kaldırın (1). Tepsiden çıktıktan sonra CMA'yı sistemden uzaklaştırın (2).

   ![CMA'nın hizmete açık olarak döndürülmüş çizimi](media/fxt-install/cma-swing-over-tray-400.png)

## <a name="install-the-front-bezel-optional"></a>Ön çerçeveyi kurun (isteğe bağlı)

Bu bölümde, Azure FXT Edge Filer donanımı için ön çerçevenin (ön plaka) nasıl yüklenir ve kaldırılır. 

Ön çerçeveyi yüklemek için: 

1. Çerçeve paketinde bulunan çerçeve anahtarını bulun ve çıkarın. 
1. Çerçeveyi şasinin ön kısmıyla hizala ve çerçevenin sağ tarafındaki pimleri düğümün sağ tarafındaki raf montaj flanşındaki deliklere takın. 
1. Çerçevenin sol ucunu şasiye sığdırın. Sol taraftaki düğme yerine tıklayana kadar çerçeveye basın.
1. Çerçeveyi anahtarla kilitleyin.

Ön çerçeveyi kaldırmak için: 
1. Çerçeve anahtarını kullanarak çerçevenin kilidini açın.
1. Sol taraftaki serbest bırakma düğmesine basın ve çerçevenin sol ucunu şasiden çekin.
1. Sağ ucunu çıkarın ve çerçeveyi çıkarın.
   
   ![Çerçevenin solundaki serbest bırakma düğmesini gösteren görüntü ve sol taraftan dışa çekerek nasıl çıkarılabildiğini](media/fxt-install/remove-bezel-edited-600.png)

## <a name="next-steps"></a>Sonraki adımlar

Aygıtı boşaltıp rafa çıkardıktan sonra ağ kabloları takarak ve AC gücünü Azure FXT Edge Filer'e bağlayarak kuruluma devam edin.

> [!div class="nextstepaction"]
> [Şebeke bağlantı noktalarını ve besleme gücünü kablola](fxt-network-power.md)