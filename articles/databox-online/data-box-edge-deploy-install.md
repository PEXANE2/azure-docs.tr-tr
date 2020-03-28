---
title: Yükleme eğitimi - Açma, raf, kablo Azure Data Box Edge fiziksel cihaz | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarını yükleme yle ilgili ikinci öğretici, fiziksel aygıtın nasıl açılaştırılacılıp rafa çıkarılacıla
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/17/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to install Data Box Edge in datacenter so I can use it to transfer data to Azure.
ms.openlocfilehash: fe74db34e62a80935954c6cfc2e591d49a84b0b7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76263957"
---
# <a name="tutorial-install-azure-data-box-edge"></a>Öğretici: Azure Veri Kutusu Kenarı Yükle

Bu öğreticide Data Box Edge fiziksel cihazı kurulum adımları anlatılmaktadır. Kurulum yordamı, aygıtın boşaltılma, raf montajı ve kablolama içerir. 

Yüklemenin tamamlanması yaklaşık iki saat sürebilir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa monte edin
> * Cihazın kablolarını bağlama

## <a name="prerequisites"></a>Ön koşullar

Fiziksel bir aygıtı n için ön koşullar aşağıdaki gibidir:

### <a name="for-the-data-box-edge-resource"></a>Data Box Edge kaynağı için

Başlamadan önce aşağıdakilerden emin olun:

* [Azure Veri Kutusu Kenarını dağıtmaya hazırla'daki](data-box-edge-deploy-prep.md)tüm adımları tamamladınız.
    * Cihazınızı dağıtmak için bir Veri Kutusu Kenarı kaynağı oluşturdunuz.
    * Cihazınızı Data Box Edge kaynağıyla etkinleştirmek için etkinleştirme anahtarını oluşturdunuz.

 
### <a name="for-the-data-box-edge-physical-device"></a>Data Box Edge fiziksel cihazı için

Cihazı dağıtmadan önce:

- Aygıtın düz, kararlı ve düz bir çalışma yüzeyinde güvenli bir şekilde dayandığından emin olun.
- Kurulumu gerçekleştirmeyi planladığınız yerde şunların bulunduğundan emin olun:
    - Bağımsız bir kaynaktan standart AC gücü

        -VEYA-
    - Kesintisiz güç kaynağına (UPS) sahip bir raf güç dağıtım ünitesi (PDU)
    - Cihazı monte etmeyi planladığınız rafta mevcut bir 1U yuvası

### <a name="for-the-network-in-the-datacenter"></a>Veri merkezindeki ağ için

Başlamadan önce:

- Veri Kutusu Kenarı dağıtmak için ağ gereksinimlerini gözden geçirin ve gereksinimlere göre veri merkezi ağını yapılandırın. Daha fazla bilgi için bkz. [Data Box Edge ağ gereksinimleri](data-box-edge-system-requirements.md#networking-port-requirements).

- Aygıtın en iyi çalışması için en az Internet bant genişliğinin 20 Mbps olduğundan emin olun.


## <a name="unpack-the-device"></a>Cihazı kutusundan çıkarma

Cihaz tek bir kutuda gönderilir. Cihazınızı kutusundan çıkarmak için aşağıdaki adımları gerçekleştirin. 

1. Kutuyu düz ve sabit bir yüzeye yerleştirin.
2. Kutuda ve ambalajda ezik, kesik, su hasarı veya gözle görülür herhangi bir hasar olup olmadığını kontrol edin. Kutu veya ambalaj ciddi şekilde hasar görmüşse, açmayın. Cihazın iyi durumda olup olmadığının değerlendirilmesi için Microsoft Desteği ile iletişim kurun.
3. Kutuyu açın. Kutuyu açtıktan sonra aşağıdakilerin bulunduğundan emin olun:
    - Tek bir kasa Data Box Edge cihazı
    - İki güç kablosu
    - Bir ray kiti montajı
    - Güvenlik, Çevre ve Mevzuat Bilgisi kitapçığı

Burada listelenen öğelerin tümlerini almadıysanız, Veri Kutusu Kenarı desteğine başvurun. Bir sonraki adım, cihazınıza monte raf etmektir.


## <a name="rack-the-device"></a>Cihazı rafa yerleştirme

Cihaz standart 19 inç rafa monte edilmelidir. Cihazınızı standart 19 inç rafa monte etmek için aşağıdaki yordamı kullanın.

> [!IMPORTANT]
> Data Box Edge cihazlarının düzgün çalışması için rafa yerleştirilmeleri gerekir.


### <a name="prerequisites"></a>Ön koşullar

- Başlamadan önce, Güvenlik, Çevre ve Düzenleyici Bilgiler kitapçığınızdaki güvenlik talimatlarını okuyun. Bu kitapçık cihazla birlikte gönderildi.
- Rayları raf muhafazasının altına en yakın olan ayrılmış alana yerleştirmeye başlayın.
- Takımlı ray montaj konfigürasyonu için:
    -  Sekiz vida tedarik etmek gerekir: #10-32, #12-24, #M5 veya #M6. Vidaların baş çapı 10 mm'den (0,4") az olmalıdır.
    -  Düz uçlu bir tornavida ya ihtiyacın var.

### <a name="identify-the-rail-kit-contents"></a>Demiryolu kiti içeriğini belirleme

Ray kiti montajını kurmak için bileşenleri bulun:
1. İki A7 Dell ReadyRailS II sürgülü ray montajı
2. İki kanca ve döngü kayışları

    ![Demiryolu kiti içeriğini belirleme](./media/data-box-edge-deploy-install/identify-rail-kit-contents.png)

### <a name="install-and-remove-tool-less-rails-square-hole-or-round-hole-racks"></a>Aletsiz rayları kurun ve çıkarın (Kare delikli veya yuvarlak delikli raflar)

> [!TIP]
> Bu seçenek, rayları raflarda iş parçacığı olmayan kare veya yuvarlak deliklere takmak ve kaldırmak için araçlar gerektirmediği için aletsizdir.

1. Sol ve sağ raylı uç parçaları içe bakacak **ŞEKILDE ÖN** etiketli yerleştirin ve dikey raf flanşlarının ön tarafındaki deliklerde oturmak için her bir uç parçayı yönlendirin.
2. Her bir uç parçayı istenilen U boşluklarının alt ve üst deliklerinde hizalayın.
3. Rayın arka ucunu, dikey raf flanşına tamamen oturana ve mandal yerine tıklayana kadar titreyin. Ön uç parçayı dikey raf flanşına oturtmak için bu adımları tekrarlayın.
4. Rayları çıkarmak için, son parçanın orta noktasındaki mandal bırakma düğmesini çekin ve her rayı kaldırın.

    ![Aletsiz rayları kurun ve çıkarın](./media/data-box-edge-deploy-install/installing-removing-tool-less-rails.png)

### <a name="install-and-remove-tooled-rails-threaded-hole-racks"></a>Takımlı rayları kurun ve çıkarın (Dişli delik rafları)

> [!TIP]
> Bu seçenek, raflarda dişli yuvarlak delikler içine rayları yüklemek ve kaldırmak için bir araç _(düz uçlu tornavida)_ gerektirdiğinden araçlıdır.

1. Pimleri düz uçlu bir tornavida kullanarak ön ve arka montaj braketlerinden çıkarın.
2. Montaj braketlerinden çıkarmak için ray mandal alt montajlarını çekin ve döndürün.
3. İki çift vida kullanarak sol ve sağ montaj raylarını ön dikey raf flanşlarına takın.
4. Sol ve sağ arka braketleri arka dikey raf flanşlarına doğru ileri doğru kaydırın ve iki çift vida kullanarak takın.

    ![Takımlı rayları kurun ve çıkarın](./media/data-box-edge-deploy-install/installing-removing-tooled-rails.png)

### <a name="install-the-system-in-a-rack"></a>Sistemi rafa yükleme

1. İç slayt raylarını yerine kilitlenene kadar raftan çekin.
2. Sistemin her iki tarafındaki arka ray standoff'unu bulun ve slayt montajlarındaki arka J yuvalarına indirin. Tüm ray soğuklukları J yuvalarında oturana kadar sistemi aşağı doğru döndürün.
3. Kilit kolları yerine tıklayana kadar sistemi içe doğru itin.
4. Her iki rayüzerindeki slayt yayın kilidi düğmelerine basın ve sistemi rafa kaydırın.

    ![Sistemi rafa yükleme](./media/data-box-edge-deploy-install/installing-system-rack.png)

### <a name="remove-the-system-from-the-rack"></a>Sistemi raftan kaldırın

1. İç rayların yanlarında kilit kollarını bulun.
2. Her kolu serbest bırakma konumuna getirerek kilidini açın.
3. Sistemin kenarlarını sıkıca kavrayın ve ray soğuklukları J yuvalarının önünde olana kadar öne çekin. Sistemi raftan yukarı ve uzağa kaldırın ve düz bir yüzeye yerleştirin.

    ![Sistemi raftan kaldırma](./media/data-box-edge-deploy-install/removing-system-rack.png)

### <a name="engage-and-release-the-slam-latch"></a>Çarpışma ve slam mandalını serbest bırakın

> [!NOTE]
> Slam mandalları ile donatılmış olmayan sistemler için, bu prosedürün 3.

1. Ön tarafa bakacak şekilde, sistemin her iki tarafındaki slam mandalını bulun.
2. Sistem rafa itilir ve mandallar yukarı çekerek serbest bırakılır gibi mandallar otomatik olarak devreye girer.
3. Rafta veya diğer kararsız ortamlarda sevkiyat için sistemi güvence altına almak için, her mandalın altındaki sert vidayı bulun ve her vidayı #2 bir Phillips tornavidayla sıkın.

    ![Devreye alın ve slam mandalını bırakın](./media/data-box-edge-deploy-install/engaging-releasing-slam-latch.png)


## <a name="cable-the-device"></a>Cihazın kablolarını bağlama

Kabloları yönlendirin ve ardından cihazınızı kablolayın. Aşağıdaki yordamlar, Veri Kutusu Edge cihazınızı güç ve ağ için nasıl kablolayacağınızı açıklar.

Cihazınızı kablolamaya başlamadan önce aşağıdakileri yapmanız gerekir:

- Veri Kutusu Kenarı fiziksel aygıtınız, ambalajı boşaltılmış ve rafa monte edilmiş.
- İki güç kablosu.
- Yönetim arabirimine bağlamak için en az bir 1-GbE RJ-45 ağ kablosu. Cihazda biri yönetim ve diğeri veri olmak üzere iki 1-GbE ağ arabirimi vardır.
- Yapılandırılacak her veri ağı arabirimi için bir 25-GbE SFP+ bakır kablo. PORT 2, PORT 3, PORT 4, PORT 5 veya PORT 6 arasında en az bir veri ağı arabiriminin Internet'e bağlanması gerekir (Azure bağlantısıyla).  
- İki güç dağıtım ünitesine erişim (önerilir).

> [!NOTE]
> - Yalnızca bir veri ağı arabirimi bağlıyorsanız, Azure'a veri göndermek için PORT 3, PORT 4, PORT 5 veya PORT 6 gibi 25/10 GbE ağ arabirimi kullanmanızı öneririz. 
> - En iyi performansı elde etmek ve büyük miktarda veriyi işlemek için tüm veri bağlantı noktalarını bağlamak isteyebilirsiniz.
> - Veri Kutusu Kenarı aygıtı, veri kaynağı sunucularından veri alabilmeleri için veri merkezi ağına bağlanmalıdır.

Veri Kutusu Kenarı cihazınızda:

- Ön panelde disk sürücüleri ve güç düğmesi vardır.

    - Cihazınızın önünde 10 disk yuvası vardır.
    - Slot 0 işletim sistemi diski olarak kullanılan 240 GB SATA sürücüsüne sahiptir. Yuva 1 boş ve yuvaları 2-9 veri diski olarak kullanılan NVMe SSD'ler vardır.
- Arka düzlemde yedek güç kaynağı üniteleri (PSU) bulunur.
- Arka düzlemde altı ağ arabirimi vardır:

    - İki 1 Gbps arabirimi.
    - 10 Gbps arabirimi olarak da hizmet verebilen dört adet 25 Gbps arabirimi.
    - Bir taban tahtası yönetim denetleyicisi (BMC).

- Arka düzlemde 6 bağlantı noktasıyla karşılık gelen iki ağ kartı vardır:

    - QLogic FastLinQ 41264
    - QLogic FastLinQ 41262

Desteklenen kabloların, anahtarların ve bu ağ kartlarının alıcı-vericilerinin tam listesi için [Cavium FastlinQ 41000 Serisi Birlikte Çalışabilirlik](https://www.marvell.com/documents/xalflardzafh32cfvi0z/)Matrisi'ne gidin.
 
Cihazınızı güç ve ağ için kablolamak için aşağıdaki adımları izleyin.

1. Cihazınızın arka düzlemindeki çeşitli bağlantı noktalarını tanımlayın.

    ![Kablolu aygıtın arka düzlemi](./media/data-box-edge-deploy-install/backplane-cabled.png)

2. Disk yuvalarını ve aygıtın önündeki güç düğmesini bulun.

    ![Bir aygıtın ön düzlemi](./media/data-box-edge-deploy-install/device-front-plane-labeled-1.png)

3. Güç kablolarını kasadaki PSU'lara bağlayın. Yüksek kullanılabilirlik için iki PSU'yu da takın ve ayrı güç kaynaklarına bağlayın.
4. Güç kablolarını raf güç dağıtım birimlerine (PDU) takın. İki PSU'nun ayrı güç kaynaklarını kullandığından emin olun.
5. Aygıtı açmak için güç düğmesine basın.
6. 1 GbE ağ arabirimi PORT 1'i fiziksel aygıtı yapılandırmak için kullanılan bilgisayara bağlayın. PORT 1, yönetim için ayrılmış arabirimdir.
7. PORT 2, PORT 3, PORT 4, PORT 5 veya PORT 6 bağlantı noktalarından birini veya birkaçını veri merkezi ağına/İnternete bağlayın.

    - PORT 2’yi bağlıyorsanız, RJ-45 ağ kablosunu kullanın.
    - 10/25 GbE ağ arabirimleri için SFP+ bakır kabloları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Nasıl Veri Kutusu Edge konular hakkında öğrendim nasıl:

> [!div class="checklist"]
> * Cihazı kutusundan çıkarma
> * Cihazı rafa yerleştirme
> * Cihazın kablolarını bağlama

Cihazınıza bağlanma, kurulumunu yapma ve etkinleştirme adımları için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Veri Kutusu Kenarını bağlayın ve kurulayın](./data-box-edge-deploy-connect-setup-activate.md)
