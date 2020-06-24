---
title: Maya üzerinde Fiziksel Temelli İşleme malzemelerini ayarlama
description: Maya içinde fiziksel olarak temel Işleme malzemelerinin nasıl ayarlanacağını ve FBX biçimine dışarı aktarmayı açıklar
author: muxanickms
ms.author: misams
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: 5579994b0746a2de4b0f2ca927027ac709940024
ms.sourcegitcommit: 9bfd94307c21d5a0c08fe675b566b1f67d0c642d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84977853"
---
# <a name="tutorial-setting-up-physically-based-rendering-materials-in-maya"></a>Öğretici: Maya 'de fiziksel tabanlı Işleme malzemeleri ayarlama

## <a name="overview"></a>Genel Bakış
Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
>
> * Sahnedeki nesnelere gelişmiş aydınlatma modeliyle malzemeler atayın.
> * nesnelerin ve malzemelerin örnek örneğini işleyin.
> * seçilecek bir sahneyi FBX biçimine ve önemli seçeneklere dışarı aktarın.

' De [fiziksel tabanlı işleme (PBR) malzemeleri](../../overview/features/pbr-materials.md) oluşturmak `Maya` , gibi diğer IÇERIK oluşturma uygulamalarında PBR 'nin çok çeşitli yollarına benzer şekilde daha basit bir ileriye dönük görevdir `3DS Max` . Aşağıdaki öğretici, ARR projelerine yönelik temel bir PBR gölgelendirici ayarlama ve FBX dışa aktarma kılavuzudur. 

Bu öğreticideki örnek sahnede bir dizi `Polygon Box` farklı malzemeden, metal, boyanmış metal, plastik ve lastik atanmış bir dizi nesne vardır. Genel anlamda, her malzeme aşağıdaki dokuların tamamını veya çoğunu içerir 

* `Albedo`, ya da olarak adlandırılan malzemeler renk eşlemeleridir `Diffuse``BaseColor`
* `Metalness`bir malzemenin metalik olup olmadığını ve hangi parçaların metalik olduğunu belirler. 
* `Roughness`bir yüzeyi kaba veya düzgün bir şekilde gösteren bir yüzey, bir yüzeydeki keskinlik veya bulanıklaştırılmaları ve vurgulamaları nasıl etkilediğini belirler.
* `Normals`bir yüzeye ayrıntı ekleyen, daha fazla poligon eklemek zorunda kalmadan, bir metal yüzey veya Wood 'teki bir çıplak yüzey ya da greni gibi.
* `Ambient Occlusion`, yazılım, esnek gölgeleme eklemek ve bir modele iletişim kurmak için kullanılır. Bir modelin hangi alanlarının tam aydınlatma (beyaz) veya tam gölge (siyah) alacağını belirten bir gri tona dönüştürme haritadır. 

## <a name="prerequisites"></a>Ön koşullar
* `Autodesk Maya 2017`veya daha yeni

## <a name="setting-up-materials-in-the-scene"></a>Sahnede malzemeleri ayarlama
Maya ' de, bir PBR malzemesini ayarlama işlemi aşağıdaki gibidir:

İle başlamak için, örnek sahnede göreceğiniz gibi, her biri farklı bir tür malzemeyi temsil eden bir dizi Box nesnesi oluşturduk. Aşağıdaki görüntüde gösterildiği gibi, bu nesnelerden her birine kendi uygun adı verildiğine göz önüne alın 

> Azure uzaktan Işleme için varlıklar oluşturmaya başlamadan önce (ARR), ölçü için ölçüm kullanır ve yukarı yön Y ekseni. Bu nedenle, Maya 'de sahne birimlerinizin ölçü birimi olarak ayarlanması önerilir. Ayrıca, FBX dışa aktarma ayarlarındaki birimleri ölçü birimi olarak ayarlamak için dışa aktarma yapmanız önerilir. 

> [!TIP]
Ad, nesne ağır sahneleri daha kolay gezecek şekilde, model varlıklarınızı genellikle ilgili parça veya malzeme türüyle uygun şekilde adlandırmak iyi bir uygulamadır.

![Nesne adları](media/object-names.jpg)

Dokularınızı oluşturduktan/aldıktan sonra, gereksinimlerinize bağlı olarak,, veya gibi bir model için benzersiz dokular oluşturmak isteyebilirsiniz veya `Quixel Suite` `Photoshop` `Substance Suite` diğer kaynaklardan genel döşeme dokuları kullanıyor olabilirsiniz, bunları modelinize aşağıdaki gibi uygulayabilirsiniz:

* Sahne görünüm penceresinde, modelinizi/geometriyi seçin ve sağ tıklayın. Görüntülenen menüde tıklama`Assign New Material`
* `Assign New Material`Seçenekler ' e gidin `Maya` > `Stingray PBS` . Bu eylem, modelinize bir PBR malzemesi atayacaktır. 

' De,, `Maya 2020` ve için kullanılabilir bir dizi farklı PBR gölgelendiriciler vardır `Maya Standard Surface` `Arnold Standard Surface` `Stingray PBR` . , `Maya Standard Surface Shader` Tarafından henüz dışarı aktarılabilir, `FBX plugin 2020` ancak `Arnold Standard Surface Shader` FBX dosyalarıyla dışarı aktarılabilir. Çoğu diğer yönden, `Maya Standard Surface Shader` ile aynıdır ve ' a benzerdir `Physical Material` `3D Studio Max` .

**`The Stingray PBR Shader`**, diğer birçok uygulamayla uyumludur ve gereksinimleriyle en yakından eşleşir `ARR` ve sonrasında desteklenir `Maya 2017` . Ayrıca, bu tür malzemelerde, daha sonra ARR 'de görselleştirilecektir.

![' Stingray ' malzeme](media/stingray-material.jpg)

Malzemeniz ve uygun şekilde adlandırıldığından, artık çeşitli dokularınızı atamaya devam edebilirsiniz. Aşağıdaki görüntüler her doku türünün PBR malzemesine uygun olduğu yerdir. `Stingray PBR`Malzeme, etkinleştirebileceğiniz öznitelikleri seçmenizi sağlar, böylece `plug in` doku haritalarınızı kullanabilmeniz için ilgili öznitelikleri etkinleştirmeniz gerekir: 

![Malzeme kurulumu](media/material-setup.jpg)

> [!TIP]
Malzemelerinizi uygun şekilde adlandırmak ve/veya türlerini hesaba katılarak iyi bir uygulamadır. Benzersiz bir bölümde kullanılacak bir malzeme, bu bölüm için adlandırılmış olabilir, ancak daha fazla sayıda alanda kullanılabilecek bir malzeme, özellikleri veya türü için adlandırılmış olabilir.

Dokularınızı aşağıdaki gibi atayın:

![Doku kurulumu](media/texture-setup.jpg)

Ve ayarlanmış olan PBR malzemelerinize göre, sahninizdeki [nesneleri örnek](../../how-tos/conversion/configure-model-conversion.md#instancing) oluşturma hakkında düşünmeye değecektir. Sahnedeki benzer nesneleri örneklerinizi örnek, örneğin, NSU, cıvatalar, screws yıkama yapanlar gibi, temelde aynı olan her türlü nesne dosya boyutu açısından önemli tasarruf elde edebilir. Bir ana nesnenin örnekleri kendi ölçek, döndürme ve dönüşümlerinin yanı sıra, sahneye gereken şekilde yerleştirilebilecek. Maya ' de, örnek oluşturma işlemi basittir.

* `Edit`Menüsünde öğesine gidin `Duplicate Special` ve `Options` öğesini açın, 
* Seçenekler içinde `Duplicate Special` `Geometry Type` `Copy` `Instance` , 
* Şuna tıklayın: `Duplicate Special`

![Örnek Oluşturma](media/instancing.jpg)

Bu eylem, nesnenin üst ve diğer örneklerinden bağımsız olarak döndürülen veya ölçeklendirilmemiş bir örnek oluşturur. 
>Ancak, bileşen modunda bir örnekte yaptığınız tüm değişiklikler, nesnenizin tüm örneklerine iletilir. bu nedenle, ınstanced nesneleri bileşenleriyle, çokgen yüzlerle çalışıyorsanız, bu örneklerin tümünü etkilemek için yaptığınız tüm değişiklikleri istediğinizden emin olun.

Örnek sahnede her bir kutu nesnesi ınstanmiş. Bu eylem, sahneyi FBX biçimine dışa aktardığınızda ilgiye sahip olacaktır.

![Sahneye genel bakış](media/scene-overview.jpg)

>Sahnedeki örnek oluşturma konusunda en iyi yöntem, daha sonra, ınstanced nesneleriyle ' kopya ' değiştirme son derece zordur. 

## <a name="fbx-export-process"></a>FBX dışarı aktarma işlemi

Şimdi sahnenin veya sahne varlıklarınızın FBX dışarı aktarmaya geçebilirsiniz. Genel olarak, varlıkları yalnızca sizin istediğiniz sahneyi/varlıkları istediğiniz şekilde dışarı aktarmak için yalnızca seçilecek şekilde dışa aktarırken anlamlı hale gelir. Bir sahnede 100 nesneleriniz varsa ancak yalnızca 30 ' u kullanmak istiyorsanız sahnenin tamamını dışarı aktarmanın hiçbir noktası yoktur. Yani sahnenin tamamını dışa aktarmadığınız için seçiminizi yapın ve şuraya gidin:

* `File` > `Export Selection`Dışarı Aktar iletişim kutusunda en alta gidin ve `Files of Type` olarak ayarlanır `FBX Export` . Bu pencere, FBX dışa aktarma ayarlarını kullanıma sunacaktır. FBX dışa aktarma için birincil ayarlar aşağıdaki görüntüde kırmızıyla vurgulanır.

![FBX dışarı aktarma](media/FBX-exporting.jpg)

Gereksinimlerinize bağlı olarak (örneğin, bir istemciye bir varlık göndermek ve varlık ile çok sayıda doku dosyası göndermek istemiyorsanız), dokuları, dışarıya eklenen FBX dosyasına eklemeyi seçebilirsiniz. Bu seçenek, paketlenecek yalnızca bir dosyanız olduğu, ancak bu FBX varlığının boyutunu önemli ölçüde artıracağı anlamına gelir. Aşağıda gösterildiği gibi seçenek üzerinde geçiş yaparak dokuları ekleme seçeneğini etkinleştirebilirsiniz `Embed Media` .

> [!TIP]
> Bu durumda dosyanın bu durumu yansıtacak şekilde adlandırıldığına dikkat edin. Bu, varlıklarınızı izlemek için iyi bir uygulamadır. 

Dışarı aktarma için yapılandırmanızı ayarlamayı bitirdikten sonra sağ alt köşedeki ' seçimi dışarı aktar ' düğmesine tıklayın.

![Medyayı katıştırma](media/embedding-media.jpg)

## <a name="conclusion"></a>Sonuç

Genel olarak, bu tür malzemeler, ışığın gerçek dünyasına göre daha gerçekçi bir şekilde görünür. Gerçek dünyada sahnenin bulunduğu ek bir derinlikli efekt oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir sahnedeki nesnelere gelişmiş aydınlatma ile malzemeleri ayarlamaya ve bunu ARR tarafından desteklenen FBX biçimine aktarmaya yönelik en önemli işlevleri öğrenmiş olursunuz. Sonraki adım, FBX dosyasını dönüştürmek ve ARR 'de görselleştirmek olacaktır.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: bir modeli işleme için dönüştürme](../../quickstarts\convert-model.md)