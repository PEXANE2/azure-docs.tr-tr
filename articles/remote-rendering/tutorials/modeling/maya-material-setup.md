---
title: Maya içinde fiziksel tabanlı işleme malzemeleri ayarlama
description: Maya ' de fiziksel tabanlı işleme malzemelerinin nasıl ayarlanacağını ve bunları FBX biçimine dışarı aktarmayı açıklar.
author: muxanickms
ms.author: misams
ms.date: 06/16/2020
ms.topic: tutorial
ms.openlocfilehash: 72742ff4f6aa19fda092b44d8d2237e7d49dd816
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "85373259"
---
# <a name="tutorial-set-up-physically-based-rendering-materials-in-maya"></a>Öğretici: Maya 'de fiziksel tabanlı işleme malzemeleri ayarlama

## <a name="overview"></a>Genel Bakış
Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
>
> * Sahnedeki nesnelere gelişmiş aydınlatma ile malzemeler atayın.
> * Nesnelerin ve malzemelerin örnek örneğini işleyin.
> * Bir sahneyi FBX biçimine aktarın ve önemli seçenekleri seçin.

Maya içinde [fiziksel tabanlı işleme (PBR) malzemeleri](../../overview/features/pbr-materials.md) oluşturmak nispeten daha basit bir görevdir. 3DS Max gibi diğer içerik oluşturma uygulamalarında PBR 'nin kurulumu için birçok yol benzerdir. Bu öğretici, Azure uzaktan Işleme projelerine yönelik temel bir PBR gölgelendirici kurulumu ve FBX dışa aktarma kılavuzudur. 

Bu öğreticideki örnek sahnenin bir dizi Çokgen kutusu nesnesi vardır. Bunlar, Wood, metal, boyanmış metal, plastik ve lastik gibi farklı malzemelere atanır. Genel anlamda, her malzeme aşağıdaki dokuların tamamını veya çoğunu içerir:

* Malzemeler renk eşlemi olan **Albedo**, ayrıca **dağıtma** veya **basecolor**olarak da adlandırılır.
* **Metalness**, bir malzemenin metalik olup olmadığını ve hangi parçaların metalik olduğunu belirler. 
* Bir yüzeyin kaba veya düzgün bir şekilde nasıl olduğunu belirleyen ve bir yüzey üzerinde önemli noktalar ve vurguların keskinliğini veya bulanıklaştırmasını belirleyen bir **kabın**.
* **Normal**, daha fazla çokgen eklemek zorunda kalmadan bir yüzeye ayrıntı ekleyen normal. Ayrıntı örnekleri, bir metal yüzey veya tahta 'daki Grenler ve kanlar olabilir.
* Yumuşak gölgeleme eklemek ve bir modele iletişim kurmak için kullanılan **çevresel Occlusiyon**. Bu, bir modelin hangi alanlarının tam aydınlatma (beyaz) veya tam gölge (siyah) alacağını belirten gri tonlamalı bir eşlemedir. 

## <a name="prerequisites"></a>Ön koşullar
* Autodesk Maya 2017 veya üzeri

## <a name="set-up-materials-in-the-scene"></a>Sahnede malzemeleri ayarlama
Maya içinde bir PBR malzemesini ayarlama hakkında daha fazla bilgiyi burada bulabilirsiniz.

Örnek sahnede gördüğünüz gibi, bir dizi Box nesnesi oluşturduk. Her nesne farklı bir malzeme türünü temsil eder. Görüntüde gösterildiği gibi, bu nesnelerin her birine kendi uygun adı verilmemiştir.

Azure uzaktan Işleme, ölçüm için ölçüleri kullanır ve yukarı yön Y eksenindedir. Varlık oluşturmaya başlamadan önce, Maya 'de sahne birimlerinizi ölçüm olarak ayarlamanızı öneririz. Dışarı aktarma için, FBX dışa aktarma ayarlarındaki birimleri ölçüm olarak ayarlayın.

> [!TIP]
> Model varlıklarınıza ilgili parça veya malzeme türüne göre uygun adlar verin. Anlamlı adlar, nesne ağır sahneler üzerinde gezinmeyi kolaylaştırır.

![Nesne adları](media/object-names.jpg)

Bazı dokular oluşturup aldıktan sonra benzersiz dokular da oluşturabilirsiniz. Tixel Suite, PhotoShop veya ya da diğer kaynaklardan genel döşeme dokuları gibi türsel uygulamaları kullanabilirsiniz.

Modelize doku uygulamak için:

1. Sahne görünüm penceresinde, modelinizi veya geometriyi seçin ve sağ tıklayın. Görüntülenen menüde **yeni malzeme ata**' yı seçin.
1. **Yeni malzeme ata** iletişim kutusunda **Maya**  >  **Stingray PBS**adresine gidin. Bu eylem modelinize bir PBR malzemesi atar. 

Maya 2020 ' de, bir dizi farklı PBR gölgelendiriciler mevcuttur. **Maya standart yüzeyi**, **Arnold standart yüzeyı**ve **Stingray PBR**içerir. **Maya standart yüzey gölgelendiricisi** , fbx 2020 eklentisi aracılığıyla henüz dışarı aktarılabilir değildir. **Arnold standart yüzey gölgelendiricisi** , FBX dosyalarıyla aktarılabilir. Çoğu diğer yönden, **Maya standart yüzey gölgelendiricisi**ile aynıdır. Bu **, 3B** Studio 'nun maks.

**STINGRAY PBR gölgelendiricisi** , diğer birçok uygulamayla uyumludur ve Azure uzaktan işleme gereksinimlerinden en yakından eşleşir. Bu, Maya 2017 tarihinden itibaren desteklenmektedir. Bu tür bir malzeme görünüm penceresinde görselleştirildiği zaman, daha sonra Azure uzaktan Işlemede görsellere benzer.

![Stingray malzemesi](media/stingray-material.jpg)

Malzemeniz, varlığınıza atandı ve uygun şekilde adlandırıldığından, artık çeşitli dokularınızı atayabilirsiniz. Aşağıdaki görüntüler, her doku türünün PBR malzemesine uygun olduğunu gösterir. Stingray PBR malzemeleri etkinleştirebileceğiniz öznitelikleri seçmenizi sağlar. Doku haritalarınızı eklemeden önce ilgili öznitelikleri etkinleştirmeniz gerekir.

![Malzeme kurulumu](media/material-setup.jpg)

Kullanım veya türlerini hesaba ayırarak malzemelerinizi uygun şekilde adlandırın. Benzersiz bir bölümde kullanılan bir malzeme, bu bölüm için adlandırılmış olabilir. Daha geniş bir alanda kullanılan bir malzeme, özellikleri veya türü için adlandırılmış olabilir.

Dokularınızı görüntüde gösterildiği gibi atayın.

![Doku kurulumu](media/texture-setup.jpg)

Belirlenen ve ayarlanmış olan PBR malzemeleriniz sayesinde sahinizdeki [nesneleri örnek oluşturmayı](../../how-tos/conversion/configure-model-conversion.md#instancing) düşünün. Sahneinizdeki benzer nesneleri, örneğin nler, cıvatları, screws ve yıkama 'lar gibi örnek olarak, dosya boyutunda önemli tasarruf sağlar. Bir ana nesnenin örneklerinin kendi ölçeği, döndürme ve dönüştürmeleri olabilir. böylece, sahneye gereken şekilde yerleştirilebilecek. 

Maya ' de, örnek oluşturma işlemi basittir.

1. **Düzenle** menüsünde, **yinelenen özel** ' i açık Seçenekler ' e gidin.
1. **Yinelenen özel seçenekler** iletişim kutusunda, **geometri türü** için **örnek** seçeneğini belirleyin. 
1. **Yinelenen özel**' i seçin.

   ![Örnek Oluşturma](media/instancing.jpg)

Bu eylem, nesnenizin bir örneğini oluşturur. Üst öğesi ve o üst öğenin diğer örneklerinden bağımsız olarak taşıyabilir, döndürebilir veya ölçekleyebilirsiniz. 

Bileşen modlarında olduğu sırada bir örnekte yaptığınız tüm değişiklikler, nesnenizin tüm örneklerine iletilir. Örneğin, köşeler ve çokgen yüzler gibi, bir nesne bileşenleri ile çalışabilirsiniz. Bu örneklerin tümünü etkilemek için yaptığınız tüm değişiklikleri istediğinizden emin olun. 

Örnek sahnede her bir kutu nesnesi ınstanmiş. Bu eylem, sahneyi FBX biçimine dışa aktardığınızda ilgiye sahip olacaktır.

![Sahneye genel bakış](media/scene-overview.jpg)

> [!TIP]
> Sahnede ilerledikçe örnekler oluşturun. Daha sonra, ınstanced nesneleriyle kopyaları değiştirmek son derece zordur. 

## <a name="fbx-export-process"></a>FBX dışarı aktarma işlemi

Sahnenin veya sahnenin varlıklarınızın FBX dışa aktarılmasını de geçelim. Varlıkları dışa aktardığınızda, yalnızca sahneinizden dışarı aktarmak istediğiniz nesneleri veya varlıkları seçmek mantıklı olur. Örneğin, bir sahnede 100 nesneniz olabilir. Yalnızca 30 ' u kullanmak istiyorsanız sahnenin tamamını dışarı aktarmanın hiçbir noktası yoktur. 

Seçiminizi yapmak için:

1. **File**  >  **Seçimi dışarı aktar** iletişim kutusunu açmak için dosya**dışarı aktarma seçimi** ' ne gidin.
1. **Dosya türü** kutusunda, FBX **Dışa Aktar ' ı seçerek** FBX dışa aktarma ayarlarını görüntüleyin. FBX dışa aktarma için birincil ayarlar görüntüde kırmızı renkle vurgulanır.

   ![FBX dışarı aktarma](media/FBX-exporting.jpg)

Gereksinimlerinize bağlı olarak, örneğin bir istemciye bir varlık göndermek isteyebilirsiniz. Varlık ile çok sayıda doku dosyası göndermek istemiyor olabilirsiniz. Dokuları, dışarıya eklenen FBX dosyasına eklemeyi tercih edebilirsiniz. Bu seçenek, paketlenecek yalnızca bir dosyanız olduğu, ancak bu FBX varlığının boyutunun önemli ölçüde arttığı anlamına gelir. Gösterilen **Medya Ekle** seçeneğini belirleyerek dokuları ekleme seçeneğini etkinleştirebilirsiniz.

> [!TIP]
> Bu örnekte, dosya bu durumu yansıtacak şekilde adlandırılmıştı. Bu adlandırma stili, varlıklarınızı izlemenin iyi bir yoludur. 

Dışarı aktarma için yapılandırmanızı ayarladıktan sonra sağ alt köşedeki **seçimi dışarı aktar** ' ı seçin.

![Medyayı katıştırma](media/embedding-media.jpg)

## <a name="conclusion"></a>Sonuç

Genel olarak, bu tür bir malzeme, ışığın gerçek hayattai temel alınarak daha gerçekçi bir şekilde görünür. Sahnenin gerçek dünyada mevcut görünmesi için ek bir derinlikli efekt oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir sahnedeki nesneler için gelişmiş aydınlatma ile malzemeleri ayarlamayı öğrenirsiniz. Ayrıca, nesnelerin Azure uzaktan Işleme tarafından desteklenen FBX biçimine nasıl verileceğini de bilirsiniz. Sonraki adım, FBX dosyasını dönüştürmektir ve Azure uzaktan Işlemede görselleştirilecek.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: bir modeli işleme için dönüştürme](../../quickstarts\convert-model.md)