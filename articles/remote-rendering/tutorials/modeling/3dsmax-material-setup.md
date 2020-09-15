---
title: 3ds Max 'ta PBR malzemeleri ayarlama
description: 3ds Max ile fiziksel olarak temel işleme malzemelerinin nasıl ayarlanacağını ve bunları FBX biçimine dışarı aktarmayı açıklar.
author: FlorianBorn71
ms.author: flborn
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: e77379be863f4527081eeec6a0ee1f426d526527
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90084473"
---
# <a name="tutorial-set-up-physically-based-rendering-materials-in-3ds-max"></a>Öğretici: 3ds Max ile fiziksel tabanlı işleme malzemeleri ayarlama

## <a name="overview"></a>Genel Bakış
Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

>[!div class="checklist"]
>
> * Sahnedeki nesnelere gelişmiş aydınlatma ile malzemeler atayın.
> * Nesnelerin ve malzemelerin örnek örneğini işleyin.
> * Bir sahneyi FBX biçimine aktarın ve önemli seçenekleri seçin.

3ds Max 'ta [fiziksel tabanlı işleme (PBR) malzemeleri](../../overview/features/pbr-materials.md) oluşturmak, basit bir görevdir. Maya gibi diğer içerik oluşturma uygulamalarında PBR 'nin kurulumu için birçok yol benzerdir. Bu öğretici, Azure uzaktan Işleme projelerine yönelik temel bir PBR gölgelendirici kurulumu ve FBX dışa aktarma kılavuzudur.

Bu öğreticideki örnek sahnenin bir dizi Çokgen kutusu nesnesi vardır. Bunlar, Wood, metal, boyanmış metal, plastik ve lastik gibi farklı malzemelere atanır. Genel anlamda, her malzeme aşağıdaki dokuların tamamını veya çoğunu içerir:

* Malzemenin renk eşlemi olan **Albedo**ve ayrıca **dağıtma** ve **basecolor**olarak da adlandırılır.
* **Metalness**, bir malzemenin metalik olup olmadığını ve hangi parçaların metalik olduğunu belirler. 
* Bir yüzeyin ne kadar kaba olduğunu veya düzgün çalıştığını belirleyen **Kablık**.
Ayrıca, bir yüzeydeki keskinlik ve önemli noktalara ilişkin netlik veya bulanıklaştırmalar da etkiler.
* **Normal**, daha fazla Çokgen eklemeden bir yüzeye ayrıntı ekler. Ayrıntı örnekleri, bir metal yüzey veya tahta 'daki greler ve kandırıcı.
* Yumuşak gölgeleme eklemek ve bir modele iletişim kurmak için kullanılan **çevresel Occlusiyon**. Bu, modelin hangi alanlarının tam aydınlatma (beyaz) veya tam gölge (siyah) alacağını belirten gri tonlamalı bir eşlemedir.

## <a name="prepare-the-scene"></a>Sahneyi hazırlama
3ds Max sürümünde, bir PBR malzemesini ayarlama işlemi aşağıdaki gibidir.

Başlamak için, her biri farklı bir tür malzemeyi temsil eden bir dizi Box nesnesi oluşturacağız.

>[!TIP]
>Bu, ölçü için ölçüm kullandığından, uzak Işleme yönelik varlıklar oluşturmaya başlamadan önce dikkat etmeniz gerekir.  
>
>Böylece sahnenin sistem birimlerini ölçüm olarak ayarlamanız iyi bir fikirdir. Bir sahneyi dışa aktardığınızda FBX dışa aktarma ayarlarındaki **birimleri birimlere** ayarlamak da iyi bir fikirdir.

Aşağıdaki ekran görüntüsünde, sistem birimlerini 3ds Max cinsinden ölçü birimlerine ayarlama adımları gösterilmektedir. 

1. Ana menüde, **Customize**  >  **birimleri ayarla kurulum**  >  **Sistem birimleri kurulumu**' na gidin. **Sistem birimi ölçeğinde** **ölçüm:** ![ sistem birimlerinin nasıl ayarlanacağını gösteren ekran görüntüsü ' nü seçin.](media/3dsmax/system-units.jpg)

1. Şimdi modelleri oluşturmaya başlayabiliriz. Örnek sahnede, her biri farklı bir malzeme türünü temsil eden çeşitli Box nesneleri oluşturacağız. Örneğin, metal, lastik ve plastik. 

   >[!TIP]
   >Varlıklar oluşturduğunuzda, bunları giderek uygun şekilde adlandırmak iyi bir uygulamadır. Bu, sahneye çok sayıda nesne içeriyorsa daha sonra bulmayı kolaylaştırır.

1. Nesneleri aşağıdaki ekran görüntüsünde gösterildiği gibi yeniden adlandırın: 

   ![Nesnelerin nasıl yeniden adlandırılacağı gösteren ekran görüntüsü.](media/3dsmax/rename-objects.jpg)

## <a name="assign-materials"></a>Malzemeleri ata

Artık sahnede bazı nesnelerimiz olduğuna göre, bu durumda bir dizi küp, PBR kurulumuna başlayabiliriz:

1. Ana araç çubuğunda, aşağıdaki ekran görüntüsünde gösterildiği gibi **malzeme düzenleyici** simgesini seçin. Düzenleyiciyi açmak için klavyenizde **M** 'yi de seçebilirsiniz. Malzeme Düzenleyicisi 'nde, **modlar** listesinde seçebileceğiniz iki mod vardır: **kompakt malzeme düzenleyici** modu ve **kurşun malzeme Düzenleyicisi** modu. Bu sahnenin görece basit olması nedeniyle kompakt modunu kullanacağız.

1. Malzeme düzenleyicisinde, bir dizi su görürsünüz. Bu kasalar, malzemelerdir. Bu malzemelerden birini sahnenin her bir nesnesine (her kutu) atayacağız. Malzemeleri atamak için öncelikle ana görünüm penceresinde nesnelerden birini seçin. Ardından, malzeme düzenleyicisinde ilk Sphere öğesini seçin. Bir nesneye atandıktan sonra, sonraki görüntüde gösterildiği gibi seçili malzeme vurgulanacaktır.

1. Gösterildiği gibi **Seçime malzeme ata**' yı seçin. Malzeme artık seçili nesnesine atanır.

   ![Malzemelerin nasıl atanacağını gösteren ekran görüntüsü.](media/3dsmax/assign-material.jpg)

    Malzeme düzenleyicisinde, gereksinimlerinize bağlı olarak çok sayıda malzeme türü arasından seçim yapabilirsiniz. Genellikle, malzeme türü varsayılan olarak **Standart** olarak ayarlanır. Bu malzeme, PBR kurulumu için uygun olmayan temel bir malzemedir. Bu nedenle, malzeme türünü bir PBR malzemesiyle değiştirmemiz gerekiyor. Fiziksel malzeme, Azure uzaktan Işleme projeleri için tercih edilen 3ds en fazla malzemedir.

1. Malzeme düzenleyicisinde **Standart** sekmesini seçin. **Malzeme/harita tarayıcısında** **fiziksel malzeme**' i seçin. Bu eylem, atanan **Standart** MALZEMEYI bir PBR fiziksel malzemesine dönüştürür.

   ![Malzemenin nasıl değiştirileceğini gösteren ekran görüntüsü.](media/3dsmax/physical-material.jpg)

    Malzeme düzenleyicisinde, aşağıdaki ekran görüntüsünde gösterildiği gibi, artık fiziksel malzemenin özelliklerini görürsünüz. Şimdi, varlığa dokular atamaya başlayabilirsiniz.

   ![Dokuların listesini gösteren ekran görüntüsü.](media/3dsmax/textures-list.jpg)

Gördüğünüz gibi, malzemeye ekleyebileceğiniz çok çeşitli haritalar ve dokular vardır. Bu öğreticide, malzemeden yalnızca beş doku yuvası kullanıyoruz.

>[!TIP]
>Önceki ekran görüntüsünde gösterildiği gibi, malzemelerinizi uygun şekilde adlandırmak iyi bir uygulamadır.

Dokularınızı nasıl oluşturabileceğiniz tercih veya kullanıma göre değişiklik gösterebilir. Örneğin, herhangi bir varlığa uygulanabilecek döşeme dokuları kullanmak isteyebilirsiniz. Ya da bir projenin veya varlığın özel dokularına sahip olmak için kendi özel dokularına sahip olmanız gerekebilir. Çevrimiçi olarak alabileceğiniz, genel döşeme dokuları kullanmak isteyebilirsiniz. Ayrıca bunları Photoshop, Tixel Suite ve kayıt paketi gibi uygulamalarda da oluşturabilirsiniz.

Dokuları atamaya başlamadan önce varlığın doku koordinatlarını (UıVW) göz önünde bulundurmanız gerekir. Modelin sarmalanmamış olduğundan emin olmak için bir modele doku uyguladığınızda bu en iyi uygulamadır. (Dokular doğru UV sarmalama olmadan düzgün görüntülenmeyecek.) Modelimizin çevresel Occlusiyon (AO) haritasını kullanmak istiyoruz çünkü bu durum özellikle bizim için önemlidir. Maya içindeki Stingray gölgelendiricisinin aksine, 3ds Max 'daki fiziksel malzemenin ayrılmış bir AO doku yuvası yoktur. Bu nedenle, başka bir yuvaya AO eşlemesini uygulayacağız. Diğer dokulardan ayrı olarak kullanılmasına izin vermek için (örneğin dokularını döşeme), kendisine ait bir UıVW Map kanalı atayacağız. 

Aşağıdaki ekran görüntüsünde gösterildiği gibi, modele bir Unwrap UVW değiştiricisi atayarak başlayacağız. 

- Seçilen nesneler özellikler düzenleyicisinde, değiştirici listesini seçin. Görüntülenen aşağı açılan listede aşağı kaydırın ve **SARMALAMASı kaldır**' ı seçin. Bu eylem, varlığa bir Unwrap UVW değiştiricisi uygular.
![SARMALAMASı geri alma seçimini gösteren ekran görüntüsü.](media/3dsmax/unwrap-modifier.jpg)

  Harita kanalı 1 olarak ayarlanır. Genellikle, eşleme kanalı 1 ' de ana kaydırmasını gerçekleştirirsiniz. Bu durumda, nesne, çakışan doku koordinatları (UV) olmadan sarmalanmamış.
![Sarmalanmamış doku koordinatlarını (UıVW) gösteren ekran görüntüsü.](media/3dsmax/unwrapped-uvw.jpg)

Sonraki adım ikinci bir UV eşleme kanalı oluşturmaktır.

1. Açık ise UV düzenleyiciyi kapatın. **Uıvs 'Yi Düzenle** menüsünün **Kanal** bölümünde kanal numarasını **2**olarak değiştirin. Harita kanalı 2, AO haritaları için beklenen kanaldır. 

1. **Kanal değişikliği uyarısı** iletişim kutusunda, Kanal 1 ' deki mevcut UVS 'yi yeni kanal 2 ' ye **taşıyabilir** veya yeni bir UV geri sarmayı oluşturacak mevcut UVS 'yi **iptal** edebilirsiniz. Yalnızca **eşleme** kanalı 1 ' den UVS 'den farklı olan Ao eşlemesi için yenı bir UV geri sarması oluşturmayı planlıyorsanız bırak ' ı seçin. (Örneğin, Kanal 1 ' de döşeme dokuları kullanmak istiyorsanız) Bu öğreticide, yeni UV kanalını düzenlemediğimiz için kanal 2 ' ye ait UVs kanalını taşıyacağız.

   >[!NOTE]
   >Eşleme kanalı 1 ' den eşleme kanalı 2 ' ye bir UV geri sarmayı kopyaladıysanız (taşıdıysanız), özgün harita kanalını etkilemeden yeni kanal UVs 'ye gerekli düzenlemeleri yapabilirsiniz.

   ![Kanal değişikliği uyarısını gösteren ekran görüntüsü.](media/3dsmax/channel-change.jpg)

Yeni harita kanalını oluşturduğumuza göre, malzeme düzenleyicisinde fiziksel malzemeye dönebilir ve dokularımızı buna eklemeye başlayabilirsiniz. İlk olarak, doğru çalışmasına izin veren başka bir adım olduğundan AO haritasını ekleyeceğiz. AO Haritası malzemeden prize takılıyken, bunu eşleme kanalı 2 ' yi kullanacak şekilde yapılandırmamız gerekir.

Daha önce belirtildiği gibi, 3ds Max fiziksel malzemesindeki AO haritaları için adanmış bir yuva yoktur. Bunun yerine, diğer bir deyişle, **Dağıtılmış kabalık** yuvasına Ao eşlemesini uygulayacağız.

1. Fiziksel malzemenin **genel haritalar** listesinde, **kabalılığını dağıtma** ve Ao eşlemenizi yükleme ' nin yanındaki **eşleme** yuvası ' nı seçin.

1. AO dokuları özelliklerinde, eşleme kanalı varsayılan olarak **1** ' e ayarlanır. Bu değeri **2**olarak değiştirin. Bu eylem, AO eşlemenizi eklemek için gereken adımları tamamlar.

   >[!IMPORTANT]
   >Bu önemli bir adımdır, özellikle Kanal 2 ' deki UVs 'niz Kanal 1 ' de farklı ise, yanlış kanal seçilmişse AO doğru şekilde eşlenmeyeceği için önemli bir adımdır.

   ![Bir AO eşlemesinin nasıl atanacağını gösteren ekran görüntüsü.](media/3dsmax/assign-ao-map.jpg)

Şimdi, PBR malzemesine normal eşlemeyi atayacağız. Bu eylem, Maya 'deki işlemden biraz farklılık gösterir. Normal harita, çarpma eşleme yuvasına doğrudan uygulanmaz. (3DS maksimum fiziksel malzemelerde normal eşleme yuvası yoktur.) Bunun yerine, normal eşlemeyi normal bir harita değiştiricisine eklersiniz, bu, kendisi de normal yuvasına takılır.

1. Fiziksel malzeme özelliklerinin **özel haritalar** bölümünde (malzeme Düzenleyicisi 'nde), **kabartma Haritası**' nın yanındaki **eşleme** yuvasını seçin. 

1. **Malzeme/harita tarayıcısında**, **normal kabartma**' i bulun ve seçin. Bu eylem, malzemeye **normal bir kabartma** değiştiricisi ekler.

1. **Normal kabartma** değiştiricide, **normal**' ın yanında **eşleme yok** ' u seçin. Normal eşlemenizi bulun ve yükleyin.

1. Yöntemin **tanjant**olarak ayarlandığından emin olun. (Varsayılan olarak, olmalıdır.) Gerekirse, **yeşil çevir (Y)** seçeneğini değiştirin.

   ![Normal kabartma seçimini gösteren ekran görüntüsü. ](media/3dsmax/normal-bump.jpg)
    ![ Normal haritanın yüklenmesini gösteren ekran görüntüsü.](media/3dsmax/load-normal-map.jpg)

Normal harita doğru şekilde atandığında, fiziksel malzeme kurulumunu tamamlamaya yönelik kalan dokuları atayabiliriz. Bu işlem basittir. Göz önünde bulundurmanız gereken özel bir ayar yok. Aşağıdaki ekran görüntüsünde, malzemeye atanan dokuların tam kümesi gösterilmektedir: 

![Malzemeye atanan dokuların tam kümesini gösteren ekran görüntüsü.](media/3dsmax/all-textures.jpg)

Artık PBR malzemeleri oluşturulup ayarlandığına göre, sahnedeki örnek oluşturma nesnelerini düşünmek de önemlidir. Sahnede, cıvatalar, screws ve yıkama gibi benzer nesneleri örnekle. Aynı olan tüm nesneler, dosya boyutu açısından önemli tasarruf elde edebilir. Bir ana nesnenin örnekleri kendi ölçek, döndürme ve dönüştürmelerine sahip olabilir, böylece bunları sahneye gereken şekilde yerleştirebilirsiniz. 3ds Max sürümünde örnek oluşturma işlemi basittir.

1. Ana görünüm penceresinde, dışarı aktarmak istediğiniz nesne veya nesneleri seçin.

1. **Shift** tuşunu basılı tutarak Dönüştür (taşı) aracını kullanarak varlıkları yukarı doğru sürükleyin. 

1. **Kopyalama seçenekleri** iletişim kutusunda, **nesneyi** **örnek** olarak ayarlayın ve ardından **Tamam**' ı seçin:

   ![Kopyalama seçenekleri iletişim kutusunun ekran görüntüsü.](media/3dsmax/instance-object.jpg)

Bu eylem, kendi üst öğesinden ve bu üst öğenin diğer örneklerinden bağımsız olarak taşıyabileceğiniz, döndürebileceğiniz veya ölçekleyerek nesnenizin bir örneğini oluşturur.

>[!IMPORTANT]
>Alt nesne modundayken bir örnekte yaptığınız tüm değişiklikler nesnenin tüm örneklerine iletilir. Bu nedenle, köşeler ve çokgen yüzleri gibi bir nesne bileşenleri ile çalışıyorsanız, tüm örnekleri etkilemek için yaptığınız tüm değişiklikleri istediğinizden emin olun. Herhangi bir zamanda, tüm ınstanced nesneleri için benzersiz bir nesne olarak getirilebilir olduğunu unutmayın. 

>[!TIP]
>Sahnede örneklerinizi yaparken örnek oluşturmanız iyi bir fikirdir. Daha sonra, ınstanced nesneleriyle kopyaları değiştirme zordur. 

Dışa aktarma işlemine geçmeden önce göz önünde bulundurmanız gereken bir son şey, sahnın/varlığınızı paylaşım için nasıl paketlemek isteyebileceğiniz bir şeydir. İdeal olarak, varlığı istemcilere veya ekip üyelerine geçirirseniz, kıymetin en düşük miktarda Fuss görülebilmesi için varlığı açabilmelerini ve görüntüleyebilmesini isteyeceksiniz. Bu nedenle, varlıkların doku yollarını sahne dosyası ile ilişkili tutmanız önemlidir. Varlığınızın doku yolları yerel bir sürücüye veya mutlak yola/konuma işaret ettikten sonra,. max dosyası dokularla aynı klasörde olsa bile, farklı bir bilgisayarda açılırsa, sahneye yüklenmezler. Doku yollarının 3ds ile göreli olarak en fazla olması bu sorunu çözer ve oldukça basittir.

1. Ana araç çubuğunda, **Dosya**  >  **başvurusu**  >  **varlık izleme geçişi**' ne gidin. 

1. Varlık Izleme penceresinde, **haritalar/gölgelendiriciler** sütununda listelenen PBR malzemelerinize uyguladığınız dokuların tamamını veya çoğunu görürsünüz.

1. Bunların yanında, **tam yol** sütununda dokularınızın konumunun, büyük olasılıkla yerel makinenizde bulunduğu konumun yolunu görürsünüz.

1. Son olarak, **durum**adlı bir sütun görürsünüz. Bu sütun, belirli bir dokunun sahneye yüklenip uygulanmadığını gösterir. Dokuyu şu terimlerden biriyle işaretler: **Tamam**, **bulundu**veya **dosya eksik**. İlk iki, dosyanın bulunduğunu ve yüklendiğini gösterir. En son açıkça, izleyici dosyanın yerini belirleyemediği anlamına gelir.
 
   ![Varlık Izleme penceresini gösteren ekran görüntüsü.](media/3dsmax/texture-paths.jpg)

İlk kez açtığınızda dokularınızın tümünün varlık Izleme penceresinde listelendiğini fark edebilirsiniz. Bu, ilgisi olacak hiçbir şey değildir. Yol bulma işlemi ile, genellikle bir sahnenin dokularını bir kez veya iki kez bulur. Yol bulma işlemi aşağıdaki gibidir: 

1. Varlık Izleme penceresinde, **SHIFT** tuşunu basılı tutun ve **haritalar/gölgelendiriciler** listesinden en üstteki dokuyu seçin ve ardından **SHIFT**'i tutmaya devam edin, listedeki son dokuyu seçin. Bu eylem listedeki tüm dokuları seçer. Seçili dokular mavi renkle vurgulanır. (Önceki ekran görüntüsüne bakın.)

1. Seçimi sağ tıklatın ve **yolu ayarla**' yı seçin.

1. **Varlık yolunu belirtin** kutusunda dokularınızın yerel yolunu seçin ve bunu ile değiştirin `.\` .  **Tamam**’ı seçin. 

    Varlık Izleme penceresi, aşağıdaki ekran görüntüsünde gösterildiği gibi güncelleşmeyecektir. Bu güncelleştirme, sahnede kaç dokuya sahip olduğuna ve sahnenin ne kadar büyük olduğuna bağlı olarak biraz zaman alabilir.
![Güncelleştirilmiş varlık Izleme penceresini gösteren screensthot.](media/3dsmax/resolve-textures.jpg)

**Tam yol** sütununun artık boş olduğuna dikkat edin. Bu, sahnenin artık belirli (mutlak) bir konumdaki ilgili dokuları araymayacağı anlamına gelir. Bu,. max dosyası veya ilgili FBX dosyası dokularla aynı klasörde olduğu sürece her zaman onları bulur. 

>[!NOTE]
>Tüm dokuların ve yolların bulunması ve çözümlenmesi için bu işlemi birkaç kez tekrarlamanız gerekebilir. Bu, ilgisi olacak hiçbir şey değildir. Tüm ilgili varlıklar için hesap oluşturuluncaya kadar işlemi tekrarlamanız yeterlidir. Bazı durumlarda bazı dosyalar bulunmayacaktır. Bu durumda, yalnızca listedeki tüm varlıkları seçin ve **eksik yolları kaldır**' ı seçin. (Önceki resme bakın.)

## <a name="fbx-export"></a>FBX dışa aktarma

Doku yollarını göreli olarak yaptığımız için, FBX dışa aktarmaya geçiş yapabilirsiniz. Yine, işlem basittir ve birkaç yolla yapabilirsiniz. 

>[!TIP]
>Sahnenin tamamını dışarı aktarmak istemediğiniz müddetçe, yalnızca ihtiyacınız olan varlıkları dışa aktarma ' yı seçmeniz iyi bir fikirdir. Kaynak yoğunluklu sahneler bölümünde dışarı aktarma işlemi uzun sürebilir.
>
>Turbodüzgünleştir ya da açık alt div gibi değiştiriciler kullandıysanız dışarı aktarma sırasında sorunlara neden olabileceğinden dışarı aktarma işleminden önce bunları daraltmak iyi bir fikir olabilir. Sahnelerinizi daralmadan önce kaydettiğinizden emin olun. 

1. Sahnede dışarı aktarmak istediğiniz varlıkları seçin. Ana araç çubuğunda **Dosya**  >  **dışarı**aktarma  >  **Seçili**öğesine gidin.

1. **Dışarı aktarılacak dosyayı seçin** iletişim kutusunda bir çıkış dosyası adı yazın veya seçin. **Farklı kaydet türü** listesinde **Autodesk (*. fbx)** öğesini seçin. Bu eylem, FBX dışa aktarma penceresini açar.

  >[!IMPORTANT] 
  >Sahnede örnekler oluşturduysanız, FBX dışa aktarma ayarlarındaki **örnekleri koru** ' yı seçmeniz önemlidir. 

  ![FBX 'e nasıl dışarı aktarılacağını gösteren ekran görüntüsü.](media/3dsmax/fbx-export.jpg)

  Dosyayı dışarı aktarmanın birkaç yolu olduğunu unutmayın. Amaç, FBX 'in yanı sıra bir klasör/dizinde doku dosyalarıyla paylaşıp, aşağıdaki ekran görüntüsünde gösterilen ayarların iyi bir şekilde çalışması gerekir. 

   FBX ile birlikte büyük klasörler/dokuların dizinlerini paylaşmamayı tercih ediyorsanız, dokuları FBX içine eklemeyi seçebilirsiniz. Dokuları eklerseniz, dokular dahil olmak üzere tüm varlık tek bir FBX öğesine eklenir. Bunun yapılması, dışa aktarma işlemini tek bir varlığa birleştirir, ancak bir sonuç olarak FBX dosyası büyük ölçüde daha büyük olacaktır.

   >[!IMPORTANT]
   >Elde edilen FBX dosyası 2,4 GB 'den büyükse, FBX dışa aktarma ayarlarında belirtilen en düşük sürüm 2016 veya üzeri olmalıdır. (Önceki ekran görüntüsüne bakın.) Daha yeni sürümler 64 bit desteğine sahiptir, bu nedenle daha büyük dosyaları destekler.

1. Sahneyi dahil edilen dokularla dışa aktarmak istiyorsanız, * FBX dışa aktarma penceresinde **medyayı Ekle**' yi seçin. 

1. Ayarlarınızı geri kalanını seçin ve ardından **Tamam**' ı seçin:

    ![FBX dışa aktarma ayarlarını gösteren ekran görüntüsü.](media/3dsmax/fbx-settings.jpg)


   Fiziksel bir malzeme kullanırken FBX 'e aktardığınızda, FBX dışa aktarma penceresinde **Tamam** ' ı seçtikten sonra aşağıdaki uyarıyı görürsünüz: 

   ![Malzeme dışa aktarma başarısız uyarısını gösteren ekran görüntüsü.](media/3dsmax/export-warnings.jpg)

   Bu uyarı, ihraç edilen malzemelerin diğer yazılım paketleriyle uyumlu olabileceğini bildiriyor. Fiziksel malzeme Azure uzaktan Işleme ile uyumlu olduğundan, Bu uyarıyla endişelenmeniz gerekmez. 

1. İşlemi tamamlayıp pencereyi kapatmak için **Tamam** ' ı seçin.

## <a name="conclusion"></a>Sonuç

Genel olarak, bu tür bir malzeme, ışığın gerçek hayattai temel alınarak daha gerçekçi bir şekilde görünür. Sahnenin gerçek dünyada mevcut görünmesi için ek bir derinlikli efekt oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir sahnedeki nesneler için gelişmiş aydınlatma ile malzemeleri ayarlamayı öğrenirsiniz. Ayrıca, nesnelerin Azure uzaktan Işleme tarafından desteklenen FBX biçimine nasıl verileceğini de bilirsiniz. Sonraki adım, FBX dosyasını dönüştürmektir ve Azure uzaktan Işlemede görselleştirilecek.

>[!div class="nextstepaction"]
>[Hızlı başlangıç: bir modeli işleme için dönüştürme](../../quickstarts\convert-model.md)
