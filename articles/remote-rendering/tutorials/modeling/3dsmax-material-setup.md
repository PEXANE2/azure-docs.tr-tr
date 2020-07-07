---
title: 3DSMax 'da fiziksel tabanlı işleme malzemeleri ayarlama
description: Fiziksel olarak temel işleme malzemelerinin 3DSMax 'da nasıl ayarlanacağını ve FBX biçimine nasıl dışarı aktarılacağını açıklar.
author: muxanickms
ms.author: misams
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: df4be8963c93199f9fad23ab3f709f691e1da768
ms.sourcegitcommit: 9b5c20fb5e904684dc6dd9059d62429b52cb39bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85880149"
---
# <a name="tutorial-set-up-physically-based-rendering-materials-in-3d-studio-max"></a>Öğretici: 3B Studio Max için fiziksel tabanlı işleme malzemeleri ayarlama

## <a name="overview"></a>Genel Bakış
Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
>
> * Sahnedeki nesnelere gelişmiş aydınlatma ile malzemeler atayın.
> * Nesnelerin ve malzemelerin örnek örneğini işleyin.
> * Bir sahneyi FBX biçimine aktarın ve önemli seçenekleri seçin.

[Fiziksel tabanlı işleme (PBR) materyalleri](../../overview/features/pbr-materials.md) Için en fazla (3dsmax), oldukça basit bir görevdir. Maya gibi diğer içerik oluşturma uygulamalarında PBR 'nin kurulumu için birçok yol benzerdir. Bu öğretici, Azure uzaktan Işleme projelerine yönelik temel bir PBR gölgelendirici kurulumu ve FBX dışa aktarma kılavuzudur.

Bu öğreticideki örnek sahnenin bir dizi Çokgen kutusu nesnesi vardır. Bunlar, Wood, metal, boyanmış metal, plastik ve lastik gibi farklı malzemelere atanır. Genel anlamda, her malzeme aşağıdaki dokuların tamamını veya çoğunu içerir:

* Malzemenin renk eşlemi olan ve ayrıca **dağıtma** veya **basecolor**olarak adlandırılan **Albedo**.
* **Metalness**, bir malzemenin metalik olup olmadığını ve hangi parçaların metalik olduğunu belirler. 
* Bir yüzeyin ne kadar kaba olduğunu veya düzgün çalıştığını belirleyen **Kablık**.
Ayrıca, bir yüzeydeki keskinlik ve önemli noktalara ilişkin netlik veya bulanıklaştırmalar da etkiler.
* **Normal**, daha fazla çokgen eklemek zorunda kalmadan bir yüzeye ayrıntı ekleyen normal. Ayrıntı örnekleri, bir metal yüzey veya tahta 'daki Grenler ve kanlar olabilir.
* Yumuşak gölgeleme eklemek ve bir modele iletişim kurmak için kullanılan **çevresel Occlusiyon**. Bu, bir modelin hangi alanlarının tam aydınlatma (beyaz) veya tam gölge (siyah) alacağını belirten gri tonlamalı bir eşlemedir.

## <a name="prepare-the-scene"></a>Sahneyi hazırlama
**3B Studio Max**sürümünde, bir PBR malzemesini ayarlama işlemi aşağıdaki gibidir.

İle başlamak için, örnek sahnede göreceğiniz gibi, her biri farklı bir tür malzemeyi temsil eden bir dizi Box nesnesi oluşturduk:

>[!TIP]
>Ölçü için **ölçüm kullandığından, ARR için varlıklar** oluşturmaya başlamadan önce dikkat edin.  
>Bu nedenle, sahne **sistem birimlerinizin** **ölçüm olarak ayarlanması**önerilir. Ayrıca, FBX dışa aktarma ayarlarındaki birimleri ölçü birimi olarak ayarlamak için dışa aktarma yapmanız önerilir.

Aşağıdaki görüntüde, System birimlerini, 3B Studio 'Nun maks. ölçü birimi olarak ayarlama adımları gösterilmektedir. Ana menüde, **Customize**  >  **birimleri ayarla kurulum**  >  **Sistem birimleri kurulum** ' a gidin ve **Sistem birimleri ölçek** açılan menüsünde ölçümler ' i **Meters**seçin. 
![Sistem birimleri](media/3dsmax/system-units.jpg)

Sistem birimleri ölçü olarak ayarlandığında modellerimizi oluşturmaya başlayabiliriz. Örnek sahümüzde, her biri farklı bir malzeme türünü temsil eden (örneğin, metal, lastik, plastik vb.) çeşitli Box nesneleri oluşturacağız. 

>[!TIP]
>Uygun şekilde ad vermek için Varlıklar oluştururken iyi bir uygulamadır. Bu, sahneye çok sayıda nesne varsa daha sonra bulmayı daha kolay hale getirir

![yeniden adlandırma nesneleri](media/3dsmax/rename-objects.jpg)

## <a name="assign-materials"></a>Malzemeleri ata

Sahnemiz içinde oluşturulan bazı nesneler sayesinde, bu durumda bir dizi küp, PBR kurulumuna başlayabiliriz:

* Ana araç çubuğunda, aşağıdaki görüntüde gösterildiği gibi **malzeme Düzenleyicisi** simgesine tıklayın. Düzenleyiciyi açmak için klavyenizde **M** tuşuna da basabilirsiniz. Malzeme Düzenleyicisi, **modlar** açılan menüsünde seçilebilir iki moda sahiptir ( **kompakt malzeme Düzenleyicisi** modu ve **kurşun malzemeler** modu). Bu sahnenin görece basit olduğu için **kompakt modunu**kullanacağız.

* Malzeme Düzenleyicisi 'nin içinde, bir dizi şumuza sahip olursunuz. bu sımlar malzemelerimiz olacaktır. Bu malzemelerden birini, sahemizdeki her bir nesne-kutuya atayacağız. Bu atamayı yapmak için önce ana görünüm penceresinde nesnelerden birini seçin. Bu seçim yapıldıktan sonra, malzeme Düzenleyicisi penceresindeki ilk sphere öğesine tıklayın. Bir nesneye atandıktan sonra, seçili malzemeniz sonraki görüntüde gösterildiği gibi vurgulanacaktır.

* Gösterildiği gibi, **seçimden malzeme ata** düğmesine tıklayın. Seçilen malzeme artık seçili nesneye atandı.
![ata-malzemeler](media/3dsmax/assign-material.jpg)

Malzeme düzenleyicisinde, kullanım durumunuza bağlı olarak, geniş bir seçimden malzeme türleri seçebilirsiniz. Genellikle, malzeme türü varsayılan olarak **Standart** olarak ayarlanır. Bu malzeme, PBR kurulumu için uygun olmayan temel bir malzemedir, bu nedenle malzeme türünü bir PBR malzemesiyle değiştirmemiz gerekecektir. Azure uzaktan işleme projeleri için tercih edilen **3Dsmax** malzemesi **fiziksel malzemedir**.

* Malzeme Düzenleyicisi 'nde **Standart** sekmesine ve ardından **fiziksel malzeme**Seç ' i açan malzeme/harita tarayıcısı ' na tıklayın. Bu eylem, atanan **Standart** MALZEMEYI bir PBR **fiziksel malzemesine**dönüştürür.
![fiziksel malzeme](media/3dsmax/physical-material.jpg)

* Malzeme düzenleyicisinde artık fiziksel malzemenin özelliklerini görürsünüz (aşağıda görebilirsiniz) ve varlığa dokular atamaya başlayabiliriz.
![dokular-liste](media/3dsmax/textures-list.jpg)

Yukarıdaki görüntüden görünebileceğini de, malzemeye eklenebilen çok çeşitli haritalar ve dokular vardır. Yine de, malzemelerde yalnızca beş doku yuvası kullanacağız.

>[!TIP]
>Yukarıdaki görüntüde gösterildiği gibi malzemelerinizi uygun şekilde adlandırmak iyi bir uygulamadır.

Şimdi malzememiz için dokuların atanmasını düşünmek için başlayabiliriz. Dokularınızı nasıl oluşturabileceğiniz, tercihe göre veya kullanıma göre farklılık gösterir. Örneğin, herhangi bir varlığa uygulanabilecek döşeme dokularını kullanmanın kutlu olması veya bir proje/varlığın belirli bölümlerinin kendi özel dokularına sahip olması gerekebilir. Çevrimiçi olarak alınan genel döşeme dokularını kullanmak veya **Photoshop**, **tixel Suite** **,,** ,,,,,,,,,,,,, ve gibi uygulamalarda kendiniz oluşturmanız 

Dokularımıza atamaya başlamadan önce, varlıklarımızı doku koordinatlarımıza (UıETDIĞIMIZ) göz önünde bulundurmanız gerekir. Modelin sarmalanmış hale geldiğinden emin olmak için bir modele herhangi bir doku uygulanırken en iyi yöntem olsa da, modelimizde bir **çevresel Occlusiyon** Haritası kullanmayı amaçladığımızda bizim için önemlidir. **Maya**Içindeki **Stingray gölgelendiricinin** aksine, **3Dsmax** Içindeki **fiziksel malzemenin** ayrılmış bir **çevresel occlusıda** doku yuvası yoktur. Bu nedenle, başka bir yuvaya AO eşlemesini uygulayacağız ve diğer dokulardan ayrı olarak kullanılmasına izin vereceğiz (örneğin dokularını döşeme), kendisine kendi UıVW Map kanalını atayacağız. 

Modelimize aşağıda gösterildiği gibi bir **Unsarmalama uıetü değiştirici** atayarak başlayacağız:

* Seçilen nesneler özellikler düzenleyicisinde, değiştirici listesine tıklayın ve açılan listede aşağı kaydırarak aşağı doğru kaydır ' ı seçin. Bu eylem, varlığımız için bir Unwrap UVW değiştiricisi uygular.
![kaydırmayı geri al-değiştirici](media/3dsmax/unwrap-modifier.jpg)

* Eşleme kanalı bir olarak ayarlanır. Bu, ana kaydırmalarınızın genellikle yapılacağı eşleme kanalalıdır. Bu durumda, nesne, çakışan doku koordinatları (UV) olmadan sarmalanmamış.
![sarmalanmamış-UVW](media/3dsmax/unwrapped-uvw.jpg)

Sonraki adım ikinci bir UV eşleme kanalı oluşturmaktır.

* Bu açık ise UV düzenleyiciyi kapatın ve **UV düzenleme** menüsünün kanal bölümünde kanal numarasını ikiye çevirin. Harita kanalı 2, çevresel Occlusiyon haritaları için beklenen kanaldır. 

* Açılan **kanal değişikliği uyarısı** iletişim kutusunda, mevcut UV ' i Kanal 1 ' in yeni kanal 2 ' ye **taşıma** veya yeni bir **UV geri SARMAYı** otomatik olarak oluşturacak mevcut UV ' i **bırakma** seçeneği sunulur. Yalnızca, harita kanalı 1 ' deki UV 'lerden farklı olan çevresel occlusiyon eşlemesi için yeni bir **UV geri sarması** oluşturmak istiyorsanız **Abandon** ' ı seçin (örneğin, Kanal 1 ' de döşeme dokuları kullanmak istiyorsanız). Yeni UV kanalını düzenlemediğimiz için, bizim amaçlarımız için UV ' ı Kanal 2 ' ye **taşıyacağız** .

>[!NOTE]
>Kopyalanmış ve **taşınmış** olsa da, eşleme kanalı 1 ' den eşleme kanalı 2 ' ye UV geri sarması, yenı kanal UV, özgün harita kanalını etkilemeden gerekli düzenlemeleri yapabilirsiniz.

![Kanal değişikliği](media/3dsmax/channel-change.jpg)

Yeni harita kanalı oluşturulduktan sonra, malzeme düzenleyicisinde fiziksel malzemeye dönebiliriz ve dokularımızı buna eklemeye başlayabilirsiniz. İlk olarak, doğru çalışmasına izin vermek için daha fazla bir adım olduğu için ortam occlusiyon (**Ao**) haritasını ekleyeceğiz. AO Haritası malzeme bağlı olduktan sonra, bunu eşleme kanalı 2 ' yi kullanmak üzere yönlendirmemiz gerekir.

* Daha önce belirtildiği gibi, **3Dsmax fiziksel malzemesindeki**Ao haritaları için adanmış bir yuva yoktur. Bunun yerine, bunu, **Dağıtılmış kabalık** yuvasına Ao eşlemesini uygulayacağız.

* Fiziksel malzemenin **genel haritalar** listesinde, **Dağıtılmış kabalığın** **eşleme** yuvası ' na tıklayın ve Ao eşlemenizi yükleyin.

* AO dokuları özelliklerinde, eşleme kanalı varsayılan olarak **1** ' e ayarlanır. Bu değeri **2**olarak değiştirin. Bu eylem, çevresel occlusiyon eşlemenizi eklemek için gereken adımları tamamlar.

>[!IMPORTANT]
>Bu önemli bir adımdır, özellikle de UV, Kanal 2 ' deki UV, yanlış kanal seçiliyken doğru bir şekilde eşlenmeyecektir.

![ata-Ao-Map](media/3dsmax/assign-ao-map.jpg)

Şimdi, PBR malzememiz için normal haritamızın atanmasını sağlıyoruz. Bu eylem, normal haritanın çarpmaya doğrudan **bir şekilde (** Örneğin, **3Dsmax fiziksel malzemelerde** normal eşleme yuvası yoktur) değil, ancak normal eşleme değiştiricisine (Bu nedenle, buna benzer şekilde) göre farklılık gösterir. **normals**

* Fiziksel malzeme özelliklerindeki **özel haritalar** bölümünde (malzeme Düzenleyicisi 'nde), **kabartma haritasının** **eşleme** yuvası ' na tıklayın. 

* Malzeme/harita tarayıcısında, **normal kabartma**' i bulup tıklayın. Bu eylem, malzemeimize **normal bir kabartma** değiştiricisi ekler.

* **Normal kabartma** değiştiricide, **normal** **eşleme yok** ' a tıklayın ve normal eşlemenizi bulun ve ardından yükleyin.

* Yöntemin **tanjant** olarak ayarlandığından (varsayılan olarak bu değer olmalıdır) ve gerekirse **yeşil çevir (Y)** arasında geçiş yapın.

![normal-kabartma ](media/3dsmax/normal-bump.jpg)
 ![ yükü-normal-eşleme](media/3dsmax/load-normal-map.jpg)

Normal haritamız doğru şekilde atandığında, fiziksel malzeme kurulumumuzu tamamlamaya yönelik kalan dokuları atamaya devam edebiliriz. Bu işlem, dikkate alınması gereken özel ayarları olmayan basit bir işlemdir. Aşağıdaki görüntüde, malzemeimize atanan dokuların tam kümesi gösterilmektedir: ![ Tümü-dokular](media/3dsmax/all-textures.jpg)

Ve ayarlanmış olan PBR malzemelerinize göre, sahninizdeki nesneleri örnek oluşturma hakkında düşünmeye değecektir. Sahnedeki benzer nesneleri örneklerinizi örnek, örneğin, NSU, cıvatalar, screws yıkama yapanlar gibi, temelde aynı olan her türlü nesne dosya boyutu açısından önemli tasarruf elde edebilir. Bir ana nesnenin örnekleri kendi ölçek, döndürme ve dönüşümlerinin yanı sıra, sahneye gereken şekilde yerleştirilebilecek. **3B Studio Max**sürümünde **örnek** oluşturma işlemi basittir.

* Ana görünüm penceresinde, dışarı aktarmak istediğiniz nesne/nesneleri seçin.

* Dönüştür (taşı) aracını kullanarak **Üst Krktr** ve varlıkları yukarı doğru sürükleyin 

* Açılan **kopya seçenekleri** iletişim kutusunda, **nesneyi** **örnek** olarak ayarlayın ve **Tamam**' a tıklayın. 
![örnek nesnesi](media/3dsmax/instance-object.jpg)

Bu eylem, nesnenin üst ve diğer örneklerinden bağımsız olarak döndürülen veya ölçeklendirilmemiş bir örnek oluşturur.

>[!IMPORTANT]
>Ancak, alt nesne modundaki bir örnekte yaptığınız tüm değişiklikler, nesnenizin tüm örneklerine iletilir. bu nedenle, ınstanced nesneleri bileşenleriyle, çokgen yüzleriyle çalışıyorsanız ve bu örneklerin tümünü etkilemek için yaptığınız herhangi bir değişikliği istediğinizden emin olun. Herhangi bir zamanda, tüm ınstanced nesneleri için benzersiz bir nesne olarak getirilebilir olduğunu unutmayın. 

>[!TIP]
>Sahnedeki örnek oluşturma konusunda en iyi yöntem, ilerleyen nesneler ile **kopyaları** değiştirme son derece zordur. 

Dışarı aktarma işlemine geçmeden önce dikkate alınması gereken bir nihai şey, sahneye/varlığınızı paylaşım için nasıl paketlemek isteyebileceğiniz bir şeydir. İdeal olarak varlığı bir istemciye veya ekip üyesine geçirirseniz, en az bir Fuss miktarı ile görülebilmesi için varlığı açabilmelerini ve görüntüleyebilmesini isteyeceksiniz. Bu nedenle, varlıkların doku yollarını sahne dosyası ile ilişkili tutmak önemlidir. Varlığınızın doku yolları yerel bir sürücüye veya mutlak yola/konuma işaret ediyor ise, **. Max** dosyası dokularla aynı klasörde otursa bile, bu, farklı bir bilgisayarda açılırsa sahneye yüklenmeyecektir. Doku yollarının 3B Studio ile göreli olarak en fazla olması bu sorunu çözer ve oldukça basittir.

* Ana araç çubuğunda **Dosya**  >  **başvurusu**  >  **varlık izleme geçişi**' ne gidin. 

* Açılan varlık izleme tarayıcısında, **haritalar/gölgelendiriciler** sütununun altında listelenen PBR malzemelerinize uyguladığınız dokuların tamamını veya çoğunu görürsünüz.

* **Tam yol** sütununda bunların yanında, dokularınızın konumunun, büyük olasılıkla yerel makinenizde konumlarına ait dosya yolunu görürsünüz.

* Son olarak, **durum**adlı bir sütun görürsünüz. Bu sütun, belirli bir dokunun sahneye yüklenip uygulanmadığını ve uygulanıp uygulanmadığını ve bu dokuyu şu şartlarla birlikte işaretle **Tamam**, **bulundu**veya **dosya yok**olarak işaretlecektir. İlk iki, dosyanın bulunduğunu ve yüklendiğini gösterir, son olarak, izleyici bir dosyayı konumlandıramadığını açıkça gösterir.
![doku yolları](media/3dsmax/texture-paths.jpg)

İlk kez açtığınızda dokularınızın tümünün varlık izleyicide listelenmediğine dikkat edin. Yol bulma işlemi ile, genellikle tüm sahne dokularını bulduğunda, bu işlem hakkında endişe edilecek bir şey yoktur. Yol bulma işlemi aşağıdaki gibidir: 

* Varlık İzleyicisi **penceresinde,** + **haritalar/gölgelendiriciler** listesinde en üstteki dokuyu**tıklatın** ve Shift tuşunu basılı tutmaya devam edin, listedeki son dokuya tıklayın. Bu eylem listedeki tüm dokuları seçer. Seçili dokular artık mavi renkle vurgulanacaktır (yukarıdaki görüntüye bakın).

* Seçime sağ tıklayın ve açılan menüden **yolu ayarla**' yı seçin.

* Açılan **varlık yolunu belirtin** kutusunda, gösterilen dokularınızın yerel yolunu seçin ve aşağıdaki ile değiştirin `.\` ve **Tamam**' a tıklayın. 

* Bir süre sonra (sahnıza kaç dokuya sahip olduğuna ve sahsitenizin ne kadar büyük olduğuna bağlı olarak değişir) varlık İzleyicisi kendisini şu şekilde çözmelidir (bkz. görüntü).
![Çözümle-dokular](media/3dsmax/resolve-textures.jpg)

**Tam yol** sütununun artık boş olduğuna dikkat edin. Bu, sahnenin artık ilgili dokuları belirli (mutlak) bir konumda bulmayı, ancak en büyük dosya veya ilgili FBX dosyası dokularla aynı klasörde olduğu sürece her zaman bulacağı anlamına gelir. 

>[!NOTE]
>Bu işlem bazen tüm dokuların ve yolların bulunması ve çözümlenmesi için birkaç kez tekrarlamanız gerekebilir. İşbu konuyla ilgili hiçbir şey yoktur, yalnızca tüm ilgili varlıkların hesaba dahil edilene kadar yinelenir. Ayrıca, bazı dosyaların artık bulunamadığı durum da olabilir. Bu durumda, yalnızca listedeki tüm varlıkları seçin ve **eksik yolları kaldır** ' a (yukarıdaki görüntüye bakın) tıklayın

## <a name="fbx-export"></a>FBX dışa aktarma

Varlık izlemenin tamamlandığına göre artık FBX dışarı aktarmaya geçebilirsiniz. Yine, işlem basittir ve birkaç yolla yapılabilir. 

>[!TIP]
>Tüm sahnesinizi dışarı aktarmak istemediğiniz ve yalnızca gerekli olan varlıkları dışarı aktarma seçeneğini belirlediğiniz için bu iyi bir uygulamadır. Özellikle kaynak kullanımı yoğun görünümlerde dışarı aktarma işlemi uzun sürebilir, böylelikle yalnızca ihtiyacınız olanları dışarı aktarmak mantıklı olur
>
>**Türbodüzgünleştir** veya **Open alt div** gibi değiştiriciler kullandıysanız dışa aktarma sırasında sorunlara neden olabileceği için bunları dışarı aktarmadan önce daraltmanız önerilir. Bunu yapmadan önce her zaman sahneyi kaydedin! 

* Sahnede dışarı aktarmak istediğiniz varlıkları seçin ve ana araç çubuğunda **Dosya**  >  **dışarı**aktarma  >  **Seçili** öğesine gidin

* **Dışarı aktarılacak dosyayı seçin** iletişim kutusunda çıktı dosyası adı yazın veya seçin ve **farklı kaydet tür** seçeneklerinde **Autodesk (*. fbx)** öğesini seçin. Bu eylem, FBX dışa aktarma menüsünü açar. 

* Sahnede örnekler oluşturduysanız, **örnekleri** korumanın, FBX dışa aktarma ayarlarında açık bir şekilde, önemli olduğunu unutmayın. 
![FBX-dışarı aktarma](media/3dsmax/fbx-export.jpg)

Daha önce, dosyayı dışarı aktarmanın birkaç yolu olduğunu unutmayın. Dışarı aktarma işlemi, FBX 'in bir klasör/dizinde bulunan dokularıyla birlikte paylaşılmasıyla, aşağıdaki görüntüde gösterilen ayarların uygulanması ve iyi çalışmamaları gerekir. Ayarlarınızı seçtikten sonra, **Tamam**' a tıklayın.
![FBX ayarları](media/3dsmax/fbx-settings.jpg)

Bununla birlikte, büyük klasörler/dokuların dizin ve dizinlerini FBX ile **paylaşmamayı** tercih ediyorsanız, dokuları FBX içine eklemeyi seçebilirsiniz. Tüm varlık dokuları dahil, tek bir FBX 'e eklenecek anlamına gelir. Aynı şekilde, dışarı aktarmayı, FBX dosyasının sonuç olarak çok daha büyük olacağı tek bir varlığa birleştirirken göz önünde bulundurun.

>[!IMPORTANT]
>Sonuç FBX dosyanız 2,4 GB 'den büyükse, en düşük FBX dışa aktarma ayarları (yukarıya bakın) 2016 veya daha yeni bir sürüm olmalıdır. Daha yeni sürümlerde 64 bit desteği olduğundan ve bu nedenle daha büyük dosyaları destekledikleri için.

* FBX dışa aktarma ayarları ' nda, * * medyayı ekleme ' yi açın ve dahil edilen dokularla dışa aktarmak için **Tamam** ' a tıklayın. 

Fiziksel malzemeyi kullanırken FBX 'e dışa aktarmada, dışarı aktarma iletişim kutusunda ' Tamam 'a tıkladıktan sonra aşağıdaki uyarı açılır penceresini görürsünüz: ![ Export-uyarılar](media/3dsmax/export-warnings.jpg)

Bu uyarı, kullanıcıya, ihraç edilen malzemelerin diğer yazılım paketleriyle uyumlu olabileceğini bildirir. Fiziksel malzeme Azure uzaktan Işleme ile uyumlu olduğundan endişelenmek için bir şey yoktur. İşlemi tamamlayıp pencereyi kapatmak için **Tamam** ' a tıklamanız yeterlidir.

## <a name="conclusion"></a>Sonuç

Genel olarak, bu tür bir malzeme, ışığın gerçek hayattai temel alınarak daha gerçekçi bir şekilde görünür. Sahnenin gerçek dünyada mevcut görünmesi için ek bir derinlikli efekt oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir sahnedeki nesneler için gelişmiş aydınlatma ile malzemeleri ayarlamayı öğrenirsiniz. Ayrıca, nesnelerin Azure uzaktan Işleme tarafından desteklenen FBX biçimine nasıl verileceğini de bilirsiniz. Sonraki adım, FBX dosyasını dönüştürmektir ve Azure uzaktan Işlemede görselleştirilecek.

>[!div class="nextstepaction"]
>[Hızlı başlangıç: bir modeli işleme için dönüştürme](../../quickstarts\convert-model.md)