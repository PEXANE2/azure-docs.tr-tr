---
title: Yönetici Kılavuzu Azure Lab Services | Microsoft Docs
description: Bu kılavuz, Azure Lab Services kullanarak laboratuvar hesapları oluşturan ve yöneten yöneticilere yardımcı olur.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2019
ms.author: spelluru
ms.openlocfilehash: 638a90615d248b3c2829770432dd6a08eb4bb2fb
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75771743"
---
# <a name="azure-lab-services---administrator-guide"></a>Azure Lab Services Yönetici Kılavuzu
Bir kuruluşun bulut kaynaklarını yöneten bilgi teknolojisi (BT) yöneticileri, genellikle kuruluşları için laboratuvar hesabı ayarlamaktan sorumludur. Laboratuvar hesabında Yöneticiler veya eğitimciler sınıf laboratuvarları oluşturur. Bu makalede, söz konusu Azure kaynaklarına ve bunları oluşturmaya yönelik kılavuza yönelik yüksek düzeyde bir genel bakış sunulmaktadır.

![Laboratuvar hesabındaki Azure kaynaklarının üst düzey görünümü](../media/administrator-guide/high-level-view.png)

- Sınıf laboratuvarları Azure Lab Services sahip bir Azure aboneliği içinde barındırılır
- Laboratuvar hesapları, paylaşılan görüntü Galerisi ve görüntü sürümleri aboneliğiniz dahilinde barındırılır
- Aynı kaynak grubunda laboratuar hesabınız ve parça görüntüsü galeriniz olabilir. Bu diyagramda, farklı kaynak gruplarında yer alırlar. 

## <a name="subscription"></a>Abonelik
Kuruluşunuzun bir veya daha fazla Azure aboneliği vardır. Laboratuvar hesapları da dahil olmak üzere, içinde kullanılan tüm Azure resources\services için faturalandırma ve güvenliği yönetmek üzere bir abonelik kullanılır.

Laboratuvar hesabı ve aboneliği arasındaki ilişki önemlidir çünkü:

- Faturalandırma, laboratuvar hesabını içeren abonelikle raporlanır.
- Kullanıcılara Azure Lab Services erişimi abonelikle ilişkili Azure Active Directory (AD) kiracısında izin verebilirsiniz. Kullanıcıyı laboratuvar hesabı sahip\katkıda bulunan veya bir sınıf Laboratuvarı Oluşturucu olarak ekleyebilirsiniz.

Sınıf laboratuvarları ve bunların sanal makineleri (VM 'Ler) tamamen sizin için yönetilir. Özel olması için, Azure Lab Services sahip olduğu özel bir abonelikte barındırılır.

## <a name="resource-group"></a>Kaynak grubu
Bir abonelik bir veya daha fazla kaynak grubu içeriyor. Kaynak grupları, aynı çözüm içinde birlikte kullanılan Azure kaynakları mantıksal gruplandırmaları oluşturmak için kullanılır.  

Laboratuvar hesabı oluşturduğunuzda, laboratuvar hesabını içeren kaynak grubunu yapılandırmanız gerekir. 

[Paylaşılan görüntü Galerisi](#shared-image-gallery)oluştururken de bir kaynak grubu gereklidir. Laboratuvar hesabınızı ve paylaşılan görüntü galerinizi iki ayrı kaynak grubuna yerleştirerek, görüntü galerisini farklı çözümler arasında paylaşmayı planlıyorsanız bu durum tipik bir çözümdür. Ya da, bunları aynı kaynak grubuna koyabilirsiniz.

Bir laboratuvar hesabı oluşturduğunuzda ve aynı anda paylaşılan bir görüntü galerisi oluşturup iliştirerek, laboratuvar hesabı ve paylaşılan görüntü Galerisi varsayılan olarak ayrı kaynak gruplarında oluşturulur. Bu öğreticide açıklanan adımları kullanırken bu davranışı görürsünüz: [Laboratuvar hesabı oluşturma sırasında paylaşılan görüntü Galerisi 'Ni yapılandırma](how-to-attach-detach-shared-image-gallery.md#configure-at-the-time-of-lab-account-creation). Bu makalenin en üstündeki resim da bu yapılandırmayı kullanır. 

Bir laboratuvar hesabının veya paylaşılan görüntü galerisinin kaynak grubunu oluşturulduktan sonra değiştirmek mümkün olmadığından, kaynak gruplarınızın yapısını planlamak için zaman aşımına uğrar. Bu kaynaklar için kaynak grubunu değiştirmeniz gerekiyorsa, laboratuvar hesabınızı ve \ veya paylaşılan görüntü galerinizi silip yeniden oluşturmanız gerekir.

## <a name="lab-account"></a>Laboratuvar hesabı
Laboratuvar hesabı bir veya daha fazla sınıf Laboratuvarı için kapsayıcı görevi görür. Azure Lab Services kullanmaya başlarken, yalnızca tek bir laboratuar hesabı olması yaygındır. Laboratuvar kullanımınız ölçeklenirken daha sonra daha fazla laboratuvar hesabı oluşturmayı tercih edebilirsiniz.

Aşağıdaki listede birden çok laboratuvar hesabının faydalı olabileceği senaryolar vurgulanmıştır:

- **Sınıf laboratuvarları genelinde farklı ilke gereksinimlerini yönetme** 

    Bir laboratuvar hesabı ayarlarken, laboratuvar hesabı altındaki tüm sınıf laboratuvarları için uygulanan ilkeler ayarlarsınız, örneğin:
    - Sınıf laboratuvarının erişebileceği paylaşılan kaynaklarla Azure sanal ağı. Örneğin, bir sanal ağ içindeki paylaşılan bir veri kümesine erişmesi gereken bir derslik Laboratuvarı kümesine sahip olabilirsiniz.
    - Sınıf laboratuvarlarının VM oluşturmak için kullanabileceği sanal makine (VM) görüntüleri. Örneğin, Linux Market görüntüsü [için veri bilimi VM'si](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.linux-data-science-vm-ubuntu) erişmesi gereken bir derslik Laboratuvarı kümesine sahip olabilirsiniz.  

    Bir diğerinden benzersiz ilke gereksinimlerine sahip derslabs varsa, bu derslik laboratuvarlarını ayrı ayrı yönetmek için ayrı laboratuvar hesapları oluşturmak yararlı olabilir.
- **Laboratuvar oluşturucularının belirli sınıf laboratuvarlarına erişimini kısıtlama**  

    Bir Kullanıcı laboratuvar Oluşturucu olarak eklendiğinde, diğer laboratuvar oluşturucuları tarafından oluşturulan laboratuvarlar dahil olmak üzere laboratuvar hesabı içindeki tüm sınıf laboratuvarlarına erişim verilir. Laboratuvar oluşturucularını belirli laboratuvarları yönetmek üzere kısıtlamak için, erişiminin kapsamını kısıtlamak üzere ayrı laboratuvar hesapları oluşturabilirsiniz. Örneğin, bir üniversite içindeki her departman için ayrı bir laboratuvar hesabı oluşturabilirsiniz. Örneğin: Bilim Departmanı için bir laboratuar hesabı ve matematik departmanı için bir laboratuvar hesabı vb.   
- **Laboratuvar hesabına göre bütçeyi ayır**

    Tek bir laboratuar hesabı için tüm derslik Laboratuvarı maliyetlerine sahip olmak yerine, daha net bir şekilde ayrılmış bütçe yapmanız gerekebilir. Yukarıdaki madde işaretli örnekteki örnekle devam ederseniz, her bir üniversite departmanı için bütçeyi uygun şekilde ayırmak üzere bir laboratuvar hesabı oluşturabilirsiniz. Azure maliyet yönetimi 'ni kullanarak her bir laboratuvar hesabının maliyetini görüntüleyebilirsiniz.
- **Pilot laboratuvarları etkin laboratuvarlardan yalıtın**

    Etkin laboratuvarları etkilemeden laboratuvar hesabındaki ilke değişikliklerini pilot yapmak istediğiniz durumlar olabilir. Bu senaryoda, pilot çalışması amaçlarıyla ayrı bir laboratuvar hesabı oluşturmak, değişiklikleri yalıtmanızı sağlar. 

## <a name="classroom-lab"></a>Sınıf Laboratuvarı
Bir sınıf Laboratuvarı, her biri belirli bir öğrenciye atanan bir veya daha fazla sanal makine (VM) içerir. Genel olarak şunları yapabilirsiniz:

- Her sınıf için bir derslik Laboratuvarı olmalıdır.
- Her bir Yarıyılı: (veya sınıfınızın sunulduğu her zaman çerçevesinde) yeni bir sınıf Laboratuvarı kümesi oluşturun. Genellikle aynı görüntü gerektiren sınıflar için, laboratuvarlar ve seorlar arasında görüntü paylaşmak üzere [paylaşılan bir görüntü Galerisi](#shared-image-gallery) kullanırsınız.

Sınıf Laboratuvarlarınızı nasıl yapılandıracağınızı belirlerken aşağıdaki noktaları göz önünde bulundurun:

- **Bir sınıf laboratuvarında bulunan tüm VM 'ler yayımlanan aynı görüntüyle dağıtılır**. Sonuç olarak, farklı laboratuvar görüntülerinin aynı anda yayımlanmasını gerektiren bir sınıfınız varsa, her biri için ayrı sınıf laboratuvarları oluşturulmalıdır.
- **Kullanım kotası laboratuvar düzeyinde ayarlanır ve laboratuvardaki tüm kullanıcılar için geçerlidir**. Örneğin, öğreme hazırlanmak için bir sınıfın VM 'lerine erişmesi gereken bir eğitimciler kümesine sahip olabilirsiniz ancak eğitimciler yalnızca 10 saatlik bir kota gerektirirken, sınıfa kayıtlı öğrenciler 40 saatlik bir kota gerektirir. Kullanıcılara farklı kotalar ayarlamak için ayrı bir derslik Laboratuvarı oluşturmanız gerekir. Ancak, kotayı ayarladıktan sonra belirli bir kullanıcıya daha fazla saat eklemek mümkündür.
- **Başlatma veya kapatılma zamanlaması laboratuvar düzeyinde ayarlanır ve laboratuvardaki tüm VM 'lere uygulanır**. Önceki noktaya benzer şekilde, kullanıcılar için farklı zamanlamalar ayarlamanız gerekiyorsa ayrı sınıf laboratuvarları oluşturmanız gerekir. 

## <a name="shared-image-gallery"></a>Paylaşılan görüntü Galerisi
Paylaşılan görüntü Galerisi bir laboratuvar hesabına iliştirilir ve görüntüleri depolamak için merkezi bir depo işlevi görür. Bir eğitimci, bir ders laboratuvarının şablon sanal makinesinden (VM) kaydetmeyi seçtiğinde galeride bir görüntü kaydedilir. Bir eğitimci, şablon sanal makinesinde değişiklik yaptığında ve kaydettiğinde, önceki sürümler korunurken görüntünün yeni sürümleri kaydedilir.

Eğitmenler, yeni bir sınıf Laboratuvarı oluştururken paylaşılan görüntü galerisinden bir görüntü sürümü yayımlayabilir. Galeri bir görüntünün birden çok sürümünü depolayabilse de, eğitimciler yalnızca Laboratuvar oluşturma sırasında en son sürümü seçebilir.

Paylaşılan görüntü Galerisi yalnızca birkaç derslik laboratuvarından başlayarak hemen ihtiyacınız olmayan isteğe bağlı bir kaynaktır. Ancak, paylaşılan görüntü galerisinin kullanılması, daha fazla sınıf Laboratuvarı sağlamak için ölçeklendirerek yararlı olan birçok avantaja sahiptir:

- **Bir şablon VM görüntüsünün sürümlerini kaydetmenizi ve yönetmenizi sağlar**.

    Özel bir görüntü oluştururken veya genel Market galerisindeki bir görüntüye değişiklikler (yazılım, yapılandırma vb.) yapılırken faydalıdır.  Örneğin, eğitimciler 'in farklı software\araçları yüklenmesi gerekir. Öğrencilerin bu önkoşulların kendilerine el ile yüklenmesini gerektirmek yerine, şablon VM görüntüsünün farklı sürümleri paylaşılan görüntü galerisine kaydedilebilir. Bu görüntü sürümleri, yeni sınıf laboratuvarları oluştururken kullanılabilir.
- **Sınıf laboratuvarları genelinde şablon VM görüntülerinin sharing\yeniden kullanılmasını mümkün**.

    Yeni bir sınıf Laboratuvarı oluşturduğunuz her seferinde bir görüntüyü sıfırdan yapılandırmak zorunda olmanızı önler. Örneğin, aynı görüntüde olması gereken birden çok sınıf sunulursa, bu görüntünün yalnızca bir kez oluşturulması ve paylaşılan görüntü galerisine kaydedilmesi gerekir; böylece bu, sınıf laboratuvarları arasında paylaşılabilir.
- **Çoğaltma aracılığıyla görüntü kullanılabilirliğini sağlar**.

    Bir sınıf laboratuvarında paylaşılan görüntü galerisine kaydederken, görüntünüz aynı coğrafya içindeki diğer bölgelere otomatik olarak çoğaltılır. Bölge için bir kesinti olması durumunda, sınıf laboratuvarınızda şablon sanal makinesinin yayımlanması, diğer bölgelerde bir görüntü çoğaltması kullanılarak etkilenmez. Ayrıca, farklı çoğaltmalar kullanmak için yayarak çoklu VM yayımlama senaryolarında performans sağlanmasına yardımcı olur.

Paylaşılan görüntüleri mantıksal olarak gruplamak için birkaç seçeneğiniz vardır:

- Birden çok paylaşılan görüntü galerisi oluşturun. Her laboratuvar hesabı yalnızca bir paylaşılan görüntü galerisine bağlanabilir, bu nedenle bu seçenek ayrıca birden çok laboratuvar hesabı oluşturmanızı gerektirir.
- Veya, birden çok laboratuvar hesabı tarafından paylaşılan tek bir paylaşılan görüntü Galerisi de kullanabilirsiniz. Bu durumda, her laboratuvar hesabı yalnızca içerdiği sınıf laboratuvarları için geçerli olan görüntüleri etkinleştirebilir.

## <a name="naming"></a>Adlandırma
Azure Lab Services kullanmaya başlarken, kaynak grupları, laboratuvar hesapları, sınıf laboratuvarları ve paylaşılan görüntü Galerisi için adlandırma kuralları oluşturmanızı öneririz. Oluşturduğunuz adlandırma kuralları, kuruluşunuzun ihtiyaçlarına göre benzersiz olacaktır, ancak aşağıdaki tabloda genel yönergeler özetlenmektedir.

| Kaynak türü | Rol | Önerilen düzen | Örnekler |
| ------------- | ---- | ----------------- | -------- | 
| Kaynak grubu | Bir veya daha fazla laboratuvar hesabı ve bir veya daha fazla paylaşılan görüntü Galerisi içerir | \<kuruluş kısa adı\>-\<ortamı\>-RG<ul><li>**Kuruluş kısa adı** , kaynak grubunun desteklediği kuruluşun adını tanımlar</li><li>**Ortam** , kaynak için test veya üretim gibi ortamı tanımlar</li><li>**RG** , kaynak türü: kaynak grubu için bir temsil eder.</li></ul> | contosoüniversıtylabs-RG<br/>contosoüniversıtylabs-test-RG<br/>contosoüniversıtylabs-prod-RG |
| Laboratuvar hesabı | Bir veya daha fazla Laboratuvarı içerir | \<kuruluş kısa adı\>-\<ortamı\>-La<ul><li>**Kuruluş kısa adı** , kaynak grubunun desteklediği kuruluşun adını tanımlar</li><li>**Ortam** , kaynak için test veya üretim gibi ortamı tanımlar</li><li>**La** , kaynak türü: Laboratuvar hesabı için temsil eder.</li></ul> | contosoüniversıtylabs-La<br/>mathdeptlabs-La<br/>bilimsel deptlabs-test-La<br/>bilimsel deptlabs-prod-La |
| Sınıf Laboratuvarı | Bir veya daha fazla sanal makine içeriyor |\<sınıf adı\>-\<zaman dilimi\>-\<eğitimci tanımlayıcı\><ul><li>**Sınıf adı** , laboratuvarın desteklediği sınıfın adını tanımlar.</li><li>**Zaman çerçevesi** , sınıfın sunulduğu zaman dilimini tanımlar.</li>**Eğitim tanımlayıcısı** , laboratuvarın sahibi olan eğitimci 'yi tanımlar.</li></ul> | CS1234-fall2019-johntikan<br/>CS1234-spring2019-johntikan | 
| Paylaşılan görüntü Galerisi | Bir veya daha fazla VM görüntüsü sürümü içeriyor | \<kuruluş kısa adı\>Galerisi | contosoüniversıtylabsgallery |

Diğer Azure kaynaklarını adlandırma hakkında daha fazla bilgi için bkz. [Azure kaynakları Için adlandırma kuralları](/azure/architecture/best-practices/naming-conventions).

## <a name="regions-or-locations"></a>Bölgeler veya konumlar
Azure Lab Services ' kaynaklarınızı ayarlarken, kaynağı barındıracak veri merkezinin bir bölgesini veya konumunu sağlamanız gerekir. Bu konuda, region\location ' ın Laboratuvar Hizmetleri dağıtımınızda kullanılan aşağıdaki kaynakların her birini nasıl etkilediği hakkında daha fazla ayrıntı verilmiştir:

- **Kaynak grubu**

    Bölge, kaynak grubuyla ilgili bilgilerin depolandığı veri merkezini belirtir. Kaynak grubu içinde yer alan Azure kaynakları, üst öğesinden farklı bölgelerde olabilir.
- **Laboratuvar hesabı veya sınıf Laboratuvarı**

    Laboratuvar hesabının konumu, bu kaynağın bölgesini gösterir. Laboratuvar hesabında oluşturulan derslik Labs aynı coğrafya içindeki herhangi bir bölgeye dağıtılabilir. Laboratuvarın VM 'lerinin dağıtıldığı belirli bölge, bölgedeki kullanılabilir kapasiteye göre otomatik olarak seçilir.  
    Bir yönetici, laboratuvar oluşturucularının derslik laboratuvarının konumunu seçmesine izin veriyorsa, seçim için kullanılabilir konumlar laboratuvar oluştururken kullanılabilir bölgesel kapasiteye dayanır.

    Derslik laboratuvarının konumu Ayrıca hangi VM işlem boyutlarının seçilebilir olduğunu de belirler. Belirli bilgi işlem boyutları yalnızca belirli konumlarda kullanılabilir.
- **Paylaşılan görüntü Galerisi**

    Bölge, hedef bölgelere otomatik olarak çoğaltılmadan önce ilk görüntü sürümünün depolandığı kaynak bölgeyi gösterir.
    
Genel bir kural, bir kaynağın region\location değerini kullanıcılarına en yakın bir şekilde ayarlamak şeklindedir. Derslik laboratuvarları için öğrencilerinize en yakın derslik Laboratuvarı oluşturulması anlamına gelir. Öğrencilerin dünyanın her yerinden bulunduğu çevrimiçi kurslar için, merkezi olarak bulunan bir derslik laboratuvarı oluşturmak üzere en iyi kararlarınızı kullanmanız gerekir. Ya da bir sınıfı, öğrencilerinizin bölgesine göre birden çok sınıf laboratuvarlarına bölebilirsiniz.

## <a name="vm-sizing"></a>VM boyutlandırma
Yöneticiler veya laboratuvar oluşturucuları bir sınıf Laboratuvarı oluşturduklarında, derslerinden ihtiyaçlarına göre aşağıdaki VM boyutları arasından seçim yapabilir. Kullanılabilir işlem boyutlarının, laboratuvar hesabınızın bulunduğu bölgeye bağlı olduğunu unutmayın:

| Boyut | Özellikler | Önerilen kullanım |
| ---- | ----- | ------------- |
| Küçük| <ul><li>2 Çekirdek</li><li>3,5 GB RAM</li></ul> | Bu boyut, komut satırı, Web tarayıcısı, düşük trafikli web sunucuları, küçük ve orta ölçekli veritabanları için idealdir. |
| Orta | <ul><li>4 çekirdek</li><li>7 GB RAM</li></ul> | Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için idealdir. |
| Orta (Iç Içe sanallaştırma) | <ul><li>4 çekirdek</li><li>16 GB RAM</li></ul> | Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için idealdir.  Bu boyut, iç içe sanallaştırmayı da destekler. |
| Büyük | <ul><li>8 Çekirdek</li><li>32 GB RAM</li></ul> | Bu boyut daha hızlı CPU, daha iyi yerel disk performansı, büyük veritabanları, büyük bellek önbellekler gerektiren uygulamalar için idealdir.  Bu boyut, iç içe sanallaştırmayı da destekler. |
| Küçük GPU (görselleştirme) | <ul><li>6 çekirdek</li><li>56 GB RAM</li> | Bu boyut, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir. |
| Küçük GPU (Işlem) | <ul><li>6 çekirdek</li><li>56 GB RAM</li></ul> |Bu boyut, yapay zeka ve derin öğrenme gibi bilgisayar yoğunluklu uygulamalar için idealdir. |
| Orta ölçekli GPU (görselleştirme) | <ul><li>12 çekirdek</li><li>112 GB RAM</li></ul> | Bu boyut, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun ve kodlama için idealdir. |

## <a name="manage-identity"></a>Kimliği Yönet
Laboratuvar hesabı yöneticisinin sahip olabileceği iki tür rol vardır:

- **Sahip**

    **Sahip** rolüne atanan bir yöneticinin, diğer kullanıcılara laboratuvar hesabına erişim verme ve laboratuvar oluşturucuları ekleme hakkı dahil laboratuvar hesabına tam erişimi vardır. Laboratuvar hesabını oluşturan yönetici varsayılan olarak sahip olarak eklenir.
- **Katılımcı**

    Katkıda bulunan rolüne atanan bir yönetici, laboratuvar hesabı ayarlarını değiştirebilir, ancak diğer kullanıcılara erişim izni veremez; Ayrıca, laboratuvar oluşturucuları ekleyebilirler.

Bir laboratuvar hesabına paylaşılan bir görüntü Galerisi eklediğinizde, her iki yönetici ve laboratuvar oluşturucuları için erişim otomatik olarak verilir. böylece, Galeri 'de görüntüleri görüntüleyebilir ve kaydedebilir. 

## <a name="pricing"></a>Fiyatlandırma

### <a name="azure-lab-services"></a>Azure Lab Services
Azure Lab Services fiyatlandırması aşağıdaki makalede açıklanmıştır: [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).

Ayrıca, görüntü sürümlerini depolamak ve yönetmek için kullanmayı planlıyorsanız paylaşılan görüntü galerisinin fiyatlandırmasını göz önünde bulundurmanız gerekir. 

### <a name="shared-image-gallery"></a>Paylaşılan görüntü Galerisi
Paylaşılan bir görüntü galerisi oluşturup laboratuvar hesabınıza ekleme ücretsizdir. Bir görüntü sürümü galeriye kaydedilinceye kadar maliyetler tahakkuk etmemektedir. Genellikle, paylaşılan bir görüntü Galerisi kullanma fiyatlandırması oldukça göz ardı edilebilir, ancak Azure Lab Services fiyatlandırmasına dahil olmadığından nasıl hesaplanacağını anlamak önemlidir.  

### <a name="storage-charges"></a>Depolama ücretleri
Görüntü sürümlerini depolamak için, paylaşılan bir görüntü Galerisi standart HDD ile yönetilen diskleri kullanır. Kullanılan HDD tarafından yönetilen diskin boyutu, depolanan görüntü sürümünün boyutuna bağlıdır. Fiyatlandırma: [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/)' nı görüntülemek için aşağıdaki makaleye bakın.


### <a name="replication-and-network-egress-charges"></a>Çoğaltma ve ağ çıkış ücretleri
Bir sınıf laboratuvarının şablon sanal makinesini (VM) kullanarak bir görüntü sürümünü kaydettiğinizde, Laboratuvar Hizmetleri önce onu bir kaynak bölgede depolar ve ardından kaynak görüntü sürümünü otomatik olarak bir veya daha fazla hedef bölgeye çoğaltır. Azure Lab Services, kaynak görüntü sürümünü, sınıf laboratuvarının bulunduğu coğrafi konum içindeki tüm hedef bölgelere otomatik olarak çoğalttığını unutmayın. Örneğin, sınıf laboratuvarınız ABD coğrafi konumunda ise, bir görüntü sürümü ABD içinde mevcut olan sekiz bölgenin her birine çoğaltılır

Bir görüntü sürümü kaynak bölgeden ek hedef bölgelere çoğaltıldığında bir ağ çıkış ücreti oluşur. Ücretlendirilen miktar, görüntünün verileri ilk başta kaynak bölgeden dışarıya aktarıldığında görüntü sürümünün boyutunu temel alır.  Fiyatlandırma ayrıntıları için aşağıdaki makaleye bakın: [bant genişliği fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/bandwidth/).

[Eğitim çözümleri](https://www.microsoft.com/licensing/licensing-programs/licensing-for-industries?rtc=1&activetab=licensing-for-industries-pivot:primaryr3) müşterileri, çıkış ücretlerini ödemekten alınmaz olabilir. Daha fazla bilgi edinmek için hesap yöneticinize konuşun.  Daha fazla bilgi için bkz. bağlantılı belgedeki **SSS** bölümüne, özellikle "akademik müşteriler için hangi veri aktarımı programlarının var olduğunu ve nasıl hak alabilirim?" sorusuna bakın).

### <a name="pricing-example"></a>Fiyatlandırma örneği
Yukarıda açıklanan fiyatlandırmayı açıklamak için, şablon VM görüntümizi paylaşılan görüntü galerisine kaydetme örneğine bakalım. Aşağıdaki senaryolar varsayılmaktadır:

- Bir özel VM Görüntünüz vardır.
- Görüntünün iki sürümünü kaydediyorsunuz.
- Laboratuvarınız ABD 'de ve toplam sekiz bölgeye sahiptir.
- Her görüntü sürümü 32 GB boyutdir; Sonuç olarak, HDD ile yönetilen disk fiyatı ayda $1,54 ' dir.

Toplam maliyet Şu şekilde tahmin edilir:

Görüntü sayısı × sürüm sayısı × yönetilen disk fiyatı

Bu örnekte, maliyet Şu şekilde olur:

1 özel görüntü (32 GB) x 2 sürüm x 8 ABD Bölge x $1,54 = $24,64/ay

### <a name="cost-management"></a>Maliyet yönetimi
Laboratuvar hesabı yöneticisinin, gereksiz görüntü sürümlerini Galeriden düzenli olarak silerek maliyetleri yönetmesi önemlidir. 

Maliyetleri azaltmanın bir yolu olarak belirli bölgelere yönelik çoğaltmayı silmelisiniz (paylaşılan görüntü galerisinde Bu seçenek mevcuttur). Çoğaltma değişiklikleri, Azure laboratuvar hizmeti 'nin paylaşılan görüntü galerisinde kaydedilmiş görüntülerden VM yayımlama yeteneğinin olumsuz etkileri olabilir.

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvar hesabı ve laboratuvar oluşturmaya yönelik adım adım yönergeler için öğreticiye bakın: [öğretici: Laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md)
