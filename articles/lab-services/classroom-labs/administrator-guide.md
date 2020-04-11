---
title: Azure Lab Hizmetleri - Yönetici kılavuzu | Microsoft Dokümanlar
description: Bu kılavuz, Azure Laboratuvar Hizmetlerini kullanarak laboratuvar hesapları oluşturan ve yöneten yöneticilere yardımcı olur.
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
ms.openlocfilehash: 7ce7ef15f0bf13182e4799fb640e83136d0d4695
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115017"
---
# <a name="azure-lab-services---administrator-guide"></a>Azure Lab Hizmetleri - Yönetici kılavuzu
Bir üniversitenin bulut kaynaklarını yöneten Bilgi Teknolojisi (BT) yöneticileri genellikle okullarının laboratuvar hesabını kurmakla yükümlüdür. Bir laboratuvar hesabı kurulduktan sonra, yöneticiler veya eğitimciler laboratuvar hesabında bulunan sınıf laboratuvarları oluşturur. Bu makalede, söz konusu Azure kaynaklarının üst düzey bir genel bakışı ve bunları oluşturma kılavuzu sağlanmaktadır.

![Laboratuvar hesabındaki Azure kaynaklarının üst düzey görünümü](../media/administrator-guide/high-level-view.png)

- Sınıf laboratuvarları, Azure Lab Services'e ait bir Azure aboneliği nde barındırılır.
- Laboratuvar hesapları, paylaşılan resim galerisi ve resim sürümleri aboneliğiniz içinde barındırılır.
- Laboratuvar hesabınızı ve paylaşılan resim galerisini aynı kaynak grubunda bulabilirsiniz. Bu diyagramda, farklı kaynak gruplarında dırlar. 

## <a name="subscription"></a>Abonelik
Üniversitenizde bir veya daha fazla Azure aboneliği vardır. Abonelik, laboratuvar hesapları da dahil olmak üzere içinde kullanılan tüm Azure kaynakları\hizmetleri için faturalandırma ve güvenliği yönetmek için kullanılır.

Bir laboratuvar hesabı ile aboneliği arasındaki ilişki önemlidir, çünkü:

- Faturalandırma, laboratuvar hesabını içeren abonelik aracılığıyla bildirilir.
- Aboneliğin Azure Etkin Dizini (AD) kiracısı tarafından Azure Lab Hizmetleri'ne erişme izni verebilirsiniz. Bir kullanıcıyı laboratuvar hesabı sahibi\katılımcı, sınıf laboratuarı oluşturucusu veya sınıf laboratuarı sahibi olarak ekleyebilirsiniz.

Sınıf laboratuvarları ve sanal makineleri (VM'ler), Azure Lab Hizmetleri'ne ait bir abonelik içinde sizin için yönetilir ve barındırılır.

## <a name="resource-group"></a>Kaynak grubu
Abonelik bir veya daha fazla kaynak grubu içerir. Kaynak grupları, aynı çözüm içinde birlikte kullanılan Azure kaynaklarının mantıksal gruplandırmalarını oluşturmak için kullanılır.  

Bir laboratuvar hesabı oluşturduğunuzda, laboratuvar hesabını içeren kaynak grubunu yapılandırmanız gerekir. 

Paylaşılan bir [resim galerisi](#shared-image-gallery)oluştururken bir kaynak grubu da gereklidir. Laboratuvar hesabınızı ve paylaşılan resim galerini iki ayrı kaynak grubuna koymayı seçebilirsiniz, bu da resim galerisini farklı çözümler arasında paylaşmayı planlıyorsanız tipiktir. Veya, bunları aynı kaynak grubuna koymayı seçebilirsiniz.

Bir laboratuvar hesabı oluşturduğunuzda, paylaşılan bir resim galerisini aynı anda otomatik olarak oluşturabilir ve ekleyebilirsiniz.  Bu seçenek, laboratuvar hesabı ve paylaşılan resim galerisinin ayrı kaynak gruplarında oluşturulmasıyla sonuçlanır. Bu öğreticide açıklanan adımları kullanırken bu davranışı görürsünüz: [Laboratuvar hesabı oluşturma sırasında paylaşılan resim galerisini yapılandırın.](how-to-attach-detach-shared-image-gallery.md#configure-at-the-time-of-lab-account-creation) Bu makalenin üst kısmındaki görüntü de bu yapılandırmayı kullanır. 

Bir laboratuvar hesabınızın veya paylaşılan resim galerisinin kaynak grubunu oluşturulduktan sonra değiştirmenin mümkün *olmadığından,* kaynak gruplarınızın yapısını planlamak için önceden zaman ayırmanızı öneririz. Bu kaynakların kaynak grubunu değiştirmeniz gerekiyorsa, laboratuvar hesabınızı ve\veya paylaşılan resim galerisini silmeniz ve yeniden oluşturmanız gerekir.

## <a name="lab-account"></a>Laboratuvar hesabı
Laboratuvar hesabı, bir veya daha fazla sınıf laboratuarı için bir konteyner görevi görehizmet eder. Azure Laboratuvar Hizmetleri'ne başlarken, yalnızca tek bir laboratuvar hesabına sahip olmak yaygındır. Laboratuvar kullanımınız ölçeklendikçe, daha sonra daha fazla laboratuvar hesabı oluşturmayı seçebilirsiniz.

Aşağıdaki liste, birden fazla laboratuvar hesabının yararlı olabileceği senaryoları vurgular:

- **Sınıf laboratuvarları arasında farklı ilke gereksinimlerini yönetme** 
    
    Bir laboratuvar hesabı ayarladığınızda, laboratuvar hesabının altındaki *tüm* sınıf laboratuvarları için geçerli olan ilkeler ayarlarsınız, örneğin:
    - Sınıf laboratuvarının erişebileceği paylaşılan kaynaklara sahip Azure sanal ağı. Örneğin, sanal ağ içinde paylaşılan veri kümesine erişmeniz gereken bir sınıf laboratuarları kümeniz olabilir.
    - Sınıf laboratuvarları VM'ler oluşturmak için kullanabileceği sanal makine (VM) görüntüleri. Örneğin, Linux Marketplace görüntüsü [için Veri Bilimi VM'sine](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.linux-data-science-vm-ubuntu) erişmeniz gereken bir sınıf laboratuvarı setiniz olabilir. 
    
    Birbirinden benzersiz ilke gereksinimleri olan sınıf laboratuvarlarınız varsa, bu sınıf laboratuvarlarını ayrı ayrı yönetmek için ayrı laboratuvar hesapları oluşturmak yararlı olabilir.

- **Laboratuvar hesabına göre ayrı bütçe**
  
    Tüm sınıf laboratuvar maliyetlerini tek bir laboratuvar hesabı üzerinden raporlamak yerine, daha net bir şekilde ayrılmış bir bütçeye ihtiyacınız olabilir. Örneğin, bütçeyi bölümler arasında ayırmak için üniversitenizin Matematik bölümü, Bilgisayar Bilimleri bölümü vb. için laboratuvar hesapları oluşturabilirsiniz.  Daha sonra [Azure Maliyet Yönetimi'ni](https://docs.microsoft.com/azure/cost-management-billing/cost-management-billing-overview)kullanarak her bir laboratuvar hesabının maliyetini görüntüleyebilirsiniz.
    
- **Pilot laboratuvarları aktif üretim laboratuvarlarından izole edin**
  
    Etkin üretim laboratuvarlarını etkilemeden bir laboratuvar hesabı için ilke değişikliklerini pilot yapmak istediğiniz durumlar olabilir. Bu tür senaryolarda, pilot uygulama amacıyla ayrı bir laboratuvar hesabı oluşturmak değişiklikleri yalıtmanıza olanak tanır. 

## <a name="classroom-lab"></a>Sınıf laboratuvarı
Sınıf laboratuarı, her biri tek bir öğrenciye atanan sanal makineler (VM' ler) içerir. Genel olarak, şunları bekleyebilirsiniz:

- Her sınıf için bir sınıf laboratuvarınız olsun.
- Her dönem yeni bir sınıf laboratuvarı kümesi oluşturun (veya sınıfınızın sunulduğu her zaman dilimi için). Genellikle aynı görüntü gereksinimlerine sahip sınıflar için, görüntüleri laboratuarlar ve dönemler arasında yeniden kullanmak için paylaşılan bir [resim galerisi](#shared-image-gallery) kullanmanız gerekir.

Sınıf laboratuvarlarınızı nasıl yapılandırılacaklarını belirlerken aşağıdaki noktaları göz önünde bulundurun:

- **Sınıf laboratuarındaki tüm VM'ler, yayınlanan görüntüyle dağıtılır**

    Sonuç olarak, farklı laboratuvar görüntülerinin aynı anda yayınlanmasını gerektiren bir sınıfvarsa, her biri için ayrı sınıf laboratuarları oluşturulmalıdır.
  
- **Kullanım kotası laboratuvar düzeyinde ayarlanır ve laboratuardaki tüm kullanıcılar için geçerlidir**
    
    Kullanıcılar için farklı kotalar ayarlamak için ayrı sınıf laboratuarları oluşturmanız gerekir. Ancak, kotayı ayarladıktan sonra belirli bir kullanıcıya daha fazla saat eklemek mümkündür.
  
- **Başlatma veya kapatma zamanlaması laboratuar düzeyinde ayarlanır ve laboratuardaki tüm VM'ler için geçerlidir**

    Önceki noktaya benzer şekilde, kullanıcılar için farklı zamanlamalar ayarlamanız gerekiyorsa, ayrı sınıf laboratuarları oluşturmanız gerekir. 

## <a name="shared-image-gallery"></a>Paylaşılan resim galerisi
Paylaşılan bir resim galerisi bir laboratuar hesabına eklenir ve görüntüleri depolamak için merkezi bir depo görevi görehizmet eder. Bir eğitimci sınıf laboratuvarının şablon sanal makinesinden (VM) dışa aktarmayı seçtiğinde bir resim galeriye kaydedilir. Bir eğitimci vm şablonunda ve dışa aktarımlarda her değişiklik yaptığında, önceki sürümleri korurken görüntünün yeni sürümleri kaydedilir.

Eğitmenler, yeni bir sınıf laboratuvarı oluşturduklarında paylaşılan resim galerisinden bir resim sürümünü yayımlayabilir. Galeri bir resmin birden çok sürümünü depolayabilse de, eğitimciler yalnızca laboratuvar oluşturma sırasında en son sürümü seçebilir.

Paylaşılan resim galerisi, yalnızca birkaç sınıf laboratuvarıyla başlarken hemen ihtiyaç duymayabileceğiniz isteğe bağlı bir kaynaktır. Ancak, paylaşılan resim galerisini kullanmak, daha fazla sınıf laboratuvarına sahip olmak için ölçeklendikçe yararlı olan birçok avantaja sahiptir:

- **Şablon VM görüntüsünün sürümlerini kaydetmenizi ve yönetmenize olanak tanır**

    Genel Market galerisinden bir görüntüye özel bir görüntü oluşturmak veya değişiklikler yapmak (yazılım, yapılandırma vb.) yararlıdır.  Örneğin, eğitimcilerin farklı yazılım\araç yüklemesini gerektirmesi yaygındır. Öğrencilerin bu ön koşulları kendi başlarına el ile yüklemelerini gerektirmek yerine, vm şablonunun farklı sürümleri paylaşılan bir resim galerisine aktarılabilir. Bu görüntü sürümleri daha sonra yeni sınıf laboratuarları oluştururken kullanılabilir.
- **Şablon VM görüntülerinin sınıf laboratuarlarında paylaşılmasını\yeniden kullanılmasını sağlar**

    Yeni bir sınıf laboratuvarı oluşturduğunuzda görüntüyü sıfırdan yapılandırmak zorunda kalmamak için görüntüyü kaydedebilir ve yeniden kullanabilirsiniz. Örneğin, aynı görüntüye gereksinim duyan birden çok sınıf sunuluyorsa, bu görüntünün sınıf laboratuarlarında paylaşılabilmesi için yalnızca bir kez oluşturulması ve paylaşılan resim galerisine dışa aktarılması gerekir.
- **Çoğaltma yoluyla görüntü kullanılabilirliğini sağlar**

    Bir sınıf laboratuvarından paylaşılan görüntü galerisine kaydettiğinizde, resminiz otomatik olarak [aynı coğrafyadaki](https://azure.microsoft.com/global-infrastructure/regions/)diğer bölgelere çoğaltılır. Bir bölge için bir kesinti olması durumunda, başka bir bölgeden görüntü çoğaltma kullanılabildiğinden, görüntüyü sınıf laboratuvarınıza yayımlamak etkilenmez.  Birden çok yinelemeden VM yayımlama da performans ile yardımcı olabilir.

Paylaşılan görüntüleri mantıksal olarak gruplandırmak için birkaç seçeneğiniz vardır:

- Birden çok paylaşılan resim galerisi oluşturun. Her laboratuvar hesabı yalnızca paylaşılan bir resim galerisine bağlanabilir, bu nedenle bu seçenek birden çok laboratuvar hesabı oluşturmanızı da gerektirir.
- Veya, birden çok laboratuvar hesabı tarafından paylaşılan tek bir paylaşılan resim galerisi kullanabilirsiniz. Bu durumda, her laboratuvar hesabı yalnızca içerdiği sınıf laboratuarları için geçerli olan görüntüleri etkinleştirebilir.

## <a name="naming"></a>Adlandırma
Azure Laboratuvar Hizmetleri'ne başladığınızda, kaynak grupları, laboratuvar hesapları, sınıf laboratuarları ve paylaşılan resim galerisi için adlandırma kuralları oluşturmanızı öneririz. Oluşturduğunuz adlandırma kuralları kuruluşunuzun gereksinimlerine özgü olsa da, aşağıdaki tablo genel yönergeleri özetler.

| Kaynak türü | Rol | Önerilen düzen | Örnekler |
| ------------- | ---- | ----------------- | -------- | 
| Kaynak grubu | Bir veya daha fazla laboratuvar hesabı ve bir veya daha fazla paylaşılan resim galerisi içerir | \<organizasyon kısa\>-\<\>ad ortamı -rg<ul><li>**Kuruluş kısa adı,** kaynak grubunun desteklediği kuruluşun adını tanımlar</li><li>**Ortam,** Pilot veya Üretim gibi kaynağın ortamını tanımlar</li><li>**Rg** kaynak türünü temsil ediyor: kaynak grubu.</li></ul> | contosouniversitylabs-rg<br/>contosouniversitylabs-pilot-rg<br/>contosouniversitylabs-prod-rg |
| Laboratuvar hesabı | Bir veya daha fazla laboratuvar içerir | \<organizasyon kısa\>-\<\>ad ortamı -la<ul><li>**Kuruluş kısa adı,** kaynak grubunun desteklediği kuruluşun adını tanımlar</li><li>**Ortam,** Pilot veya Üretim gibi kaynağın ortamını tanımlar</li><li>**La** kaynak türü anlamına gelir: laboratuvar hesabı.</li></ul> | contosouniversitylabs-la<br/>mathdeptlabs-la<br/>sciencedeptlabs-pilot-la<br/>sciencedeptlabs-prod-la |
| Sınıf laboratuvarı | Bir veya daha fazla VM içerir |\<sınıf\>-\<adı\>-\<zaman dilimi eğitimci tanımlayıcısı\><ul><li>**Sınıf adı,** laboratuarın desteklediği sınıfın adını tanımlar.</li><li>**Zaman dilimi,** sınıfın sunulduğu zaman dilimini tanımlar.</li>**Eğitim tanımlayıcısı,** laboratuvarın sahibi olan eğiticiyi tanımlar.</li></ul> | CS1234-sonbahar2019-johndoe<br/>CS1234-spring2019-johndoe | 
| Paylaşılan resim galerisi | Bir veya daha fazla VM görüntü sürümü içerir | \<organizasyon kısa\>ad galerisi | contosouniversitylabsgallery |

Diğer Azure kaynaklarını adlandırma hakkında daha fazla bilgi için Azure [kaynakları için adlandırma kurallarına](/azure/architecture/best-practices/naming-conventions)bakın.

## <a name="regionslocations"></a>Bölgeler\yerler

Azure Laboratuvar Hizmetleri'nizin kaynaklarını ayarlarken, kaynağı barındıracak veri merkezinin bir bölgesini (veya konumunu) sağlamanız gerekir. Aşağıda, bölgenin bir laboratuvar kurma yla ilgili kaynakların her birini nasıl etkilediğine ilişkin daha fazla ayrıntı veredetir.

### <a name="resource-group"></a>Kaynak grubu

Bölge, kaynak grubu hakkındaki bilgilerin depolandığı veri merkezini belirtir. Kaynak grubunda bulunan azure kaynakları, üst lerinden farklı bölgelerde olabilir.

### <a name="lab-account"></a>Laboratuvar hesabı

Bir laboratuvar hesabının konumu, bu kaynağın bulunduğu bölgeyi gösterir.  

### <a name="classroom-lab"></a>Sınıf laboratuvarı
    
Bir sınıf laboratuvarının bulunduğu konum aşağıdaki etkenlere göre değişir:

  - **Laboratuvar hesabı sanal ağa (VNet) bakar**
  
    Bir laboratuvar hesabı, aynı bölgede olduklarında [bir VNet ile eşlenebilir.](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-connect-peer-virtual-network)  Bir laboratuvar hesabına VNet ile bakıldığında, sınıf laboratuarları hem laboratuvar hesabı hem de VNet ile aynı bölgede otomatik olarak oluşturulur.

    > [!NOTE]
    > Bir laboratuvar hesabına Bir VNet ile bakıldığında, **laboratuvar oluşturucusunun laboratuvar konumunu seçmesine izin** verme ayarı devre dışı bırakılır. Makalede bu ayar hakkında ek bilgiler bulunabilir: [Laboratuvar oluşturucusu laboratuvar için yer seçmek için izin ver.](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location)
    
  - **Hiçbir VNet bakılır ***ve*** laboratuvar oluşturucuları laboratuvar konumunu seçmek için izin verilmez**
  
    Laboratuvar hesabı **no** ile baktı *hiçbir* VNet ve [laboratuvar yaratıcıları laboratuvar konumu seçmek için izin **verilmez** ](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location)olduğunda, sınıf laboratuvarları otomatik olarak kullanılabilir VM kapasitesine sahip bir bölgede oluşturulur.  Azure Lab Hizmetleri [özellikle, laboratuvar hesabıyla aynı coğrafyada bulunan bölgelerde](https://azure.microsoft.com/global-infrastructure/regions)kullanılabilirlik arar.

  - **Hiçbir VNet bakılır ***ve*** laboratuvar oluşturucuları laboratuvar konumunu seçmek için izin verilir**
       
    **Hiçbir** VNet baktı ve [laboratuvar yaratıcıları laboratuvar konumunu seçmek için izin verildiğinde,](https://docs.microsoft.com/azure/lab-services/classroom-labs/allow-lab-creator-pick-lab-location)laboratuvar oluşturucu tarafından seçilebilir konumlar kullanılabilir kapasiteye göre bağlıdır.

> [!NOTE]
> Bir bölge için yeterli VM kapasitesi olduğundan emin olmak için, önce laboratuvar hesabı üzerinden veya laboratuvar oluştururken kapasite istemeniz önemlidir.

Genel kural, kaynağın bölgesini kullanıcılarına en yakın olana ayarlamaktır. Sınıf laboratuvarları için bu, öğrencilerinize en yakın sınıf laboratuarıoluşturmak anlamına gelir. Öğrencilerin tüm dünyada bulunduğu çevrimiçi kurslar için, merkezi bir şekilde bulunan bir sınıf laboratuvarı oluşturmak için en iyi kararınızı kullanmanız gerekir. Veya bir sınıfı öğrencilerinizin bölgesine göre birden çok sınıf laboratuvarına bölün.

### <a name="shared-image-gallery"></a>Paylaşılan resim galerisi

Bölge, hedef bölgelere otomatik olarak çoğaltılmadan önce ilk görüntü sürümünün depolandığı kaynak bölgeyi gösterir.

## <a name="vm-sizing"></a>VM boyutlandırma
Yöneticiler veya laboratuvar oluşturucuları bir sınıf laboratuvarı oluşturduklarında, sınıflarının gereksinimlerine göre aşağıdaki VM boyutları arasından seçim yapabilirler. Kullanılabilir işlem boyutlarının laboratuvar hesabınızın bulunduğu bölgeye bağlı olduğunu unutmayın:

| Boyut | Özellikler | Seriler | Önerilen kullanım |
| ---- | ----- | ------ | ------------- |
| Küçük| <ul><li>2 Çekirdek</li><li>3,5 GB RAM</li> | [Standard_A2_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | Bu boyut en iyi komut satırı için uygundur, web tarayıcısı açılış, düşük trafik web sunucuları, küçük ve orta veritabanları. |
| Orta | <ul><li>4 Çekirdek</li><li>7 GB RAM</li> | [Standard_A4_v2](https://docs.microsoft.com/azure/virtual-machines/av2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için en uygun uyrdu. |
| Orta (İç içe sanallaştırma) | <ul><li>4 Çekirdek</li><li>16 GB RAM</li></ul> | [Standard_D4s_v3](https://docs.microsoft.com/azure/virtual-machines/dv3-dsv3-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json#dsv3-series) | Bu boyut, ilişkisel veritabanları, bellek içi önbelleğe alma ve analiz için en uygun uyrdu.  Bu boyut iç içe sanallaştırmayı da destekler. |
| Büyük | <ul><li>8 Çekirdek</li><li>32 GB RAM</li></ul>  | [Standard_DC8_v2](https://docs.microsoft.com/azure/virtual-machines/dcv2-series?toc=/azure/virtual-machines/linux/toc.json&bc=/azure/virtual-machines/linux/breadcrumb/toc.json) | Bu boyut, daha hızlı CPU'lar, daha iyi yerel disk performansı, büyük veritabanları, büyük bellek önbellekleri gerektiren uygulamalar için en uygunudur.  Bu boyut iç içe sanallaştırmayı da destekler. |
| Küçük GPU (Görselleştirme) | <ul><li>6 Çekirdek</li><li>56 GB RAM</li>  | [Standard_NV6](https://docs.microsoft.com/azure/virtual-machines/nv-series) | Bu boyut en iyi uzaktan görselleştirme, akış, oyun, OpenGL ve DirectX gibi çerçeveleri kullanarak kodlama için uygundur. |
| Küçük GPU (İşlem) | <ul><li>6 Çekirdek</li><li>56 GB RAM</li></ul>  | [Standard_NC6](https://docs.microsoft.com/azure/virtual-machines/nc-series) |Bu boyut en iyi Yapay Zeka ve Derin Öğrenme gibi bilgisayar yoğun uygulamalar için uygundur. |
| Orta GPU (Görselleştirme) | <ul><li>12 Çekirdek</li><li>112 GB RAM</li></ul>  | [Standard_NC12](https://docs.microsoft.com/azure/virtual-machines/nc-series) | Bu boyut en iyi uzaktan görselleştirme, akış, oyun, OpenGL ve DirectX gibi çerçeveleri kullanarak kodlama için uygundur. |

## <a name="manage-identity"></a>Kimliği yönetme
[Azure'un rol tabanlı erişim denetimini](https://docs.microsoft.com/azure/role-based-access-control/overview)kullanarak, laboratuvar hesaplarına ve sınıf laboratuvarlarına erişim sağlamak için aşağıdaki roller atanabilir:

- **Laboratuvar hesap sahibi**

    Laboratuvar hesabını oluşturan yönetici otomatik olarak laboratuvar hesabının **Sahibi** rolüne eklenir.  **Sahibi** rolü atanan bir yönetici şunları yapabilir:
     - Laboratuvar hesabının ayarlarını değiştirin.
     - Diğer yöneticilerin laboratuvar hesabına sahip veya katkıda bulunan olarak erişmelerini saðlar. 
     - Eğitimcilere sınıf laboratuvarlarına yaratıcı, sahip veya katkıda bulunan olarak erişim hakkı verin.
     - Laboratuvar hesabındaki tüm sınıf laboratuvarlarını oluşturun ve yönetin.

- **Laboratuvar hesabı katılımcısı**

    **Katılımcı** rolüatanan bir yönetici şunları yapabilir:
    - Laboratuvar hesabının ayarlarını değiştirin.
    - Laboratuvar hesabı içindeki tüm sınıf laboratuvarlarını oluşturun ve yönetin.
    
    Ancak, diğer kullanıcılara laboratuvar hesaplarına veya sınıf laboratuarlarına erişim izni *veremezler.*

- **Sınıf laboratuvarı oluşturucusu**

    Bir laboratuvar hesabı içinde sınıf laboratuvarları oluşturmak için, bir eğitimci **Ninotor Oluşturucu** rolünün bir üyesi olmalıdır.  Bir eğitimci bir sınıf laboratuvarı oluşturduğunda, otomatik olarak laboratuvarın sahibi olarak eklenir.  [ **Laboratuvar Oluşturucurolüne** nasıl bir kullanıcı ekleyeceğiniz](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-lab-account#add-a-user-to-the-lab-creator-role)le ilgili öğreticiye bakın. 

- **Sınıf laboratuarı sahibi\katılımcı**
  
    Bir eğitimci, bir laboratuvarın **Sahibi** veya **Katılımcı** rolünün üyesi olduklarında sınıf laboratuvarının ayarlarını görüntüleyebilir ve değiştirebilir; ayrıca laboratuvar hesabının **Reader** rolünün bir üyesi olmalıdır.

    Bir laboratuvarın **Sahibi** ve **Katılımcı** rolleri arasındaki temel fark, bir katılımcının diğer kullanıcılara laboratuvarı yönetme erişimi *verememesidir* - yalnızca sahipleri diğer kullanıcılara laboratuvarı yönetme erişimi verebilir.
    
    Buna ek olarak, bir eğitimci, **Lab Creator** rolünün bir üyesi olmadıkça yeni sınıf laboratuvarları *oluşturamaz.*

- **Paylaşılan resim galerisi**
    
    Paylaşılan bir resim galerisini bir laboratuvar hesabına eklediğinizde, laboratuvar hesabı sahipleri\katkıda bulunanlar ve laboratuvar oluşturucuları\sahipleri\katkıda bulunanlara galerideki görüntüleri görüntüleme ve kaydetme erişimi otomatik olarak verilir. 

Rolleri atamaya yardımcı olacak bazı ipuçları aşağıda verilmiştir:
   - Genellikle, yalnızca yöneticiler bir laboratuvar hesabının **Sahibi** veya **Katılımcı** rollerinin üyeleri olmalıdır; birden fazla sahibi\katkıda bulunabilir.

   - Bir eğitimciye yeni sınıf laboratuvarları oluşturma ve oluşturdukları laboratuvarları yönetme becerisi kazandırmak; yalnızca **Lab Creator** rolüne erişim atamanız gerekir.
   
   - Bir eğitimciye belirli sınıf laboratuvarlarını yönetme yeteneği vermek, ancak yeni laboratuvarlar oluşturma becerisi *vermek;* yönetecekleri sınıf laboratuvarlarının her biri için **Sahibi** veya **Katılımcı** rolüne erişim atamalısınız.  Örneğin, hem bir profesörün hem de bir öğretim görevlisinin bir sınıf laboratuvarının ortak sahibi olması için izin vermek isteyebilirsiniz.  [Bir kullanıcıyı sınıf laboratuvarına sahip olarak](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-add-user-lab-owner)nasıl ekleyeceğiniz kılavuzuna bakın.

## <a name="pricing"></a>Fiyatlandırma

### <a name="azure-lab-services"></a>Azure Lab Services
Azure Lab Hizmetleri fiyatlandırması aşağıdaki makalede açıklanmıştır: [Azure Lab Hizmetleri fiyatlandırması.](https://azure.microsoft.com/pricing/details/lab-services/)

Ayrıca, görüntü sürümlerini depolamak ve yönetmek için kullanmayı planlıyorsanız, paylaşılan resim galerisinin fiyatlandırmasını da göz önünde bulundurmanız gerekir. 

### <a name="shared-image-gallery"></a>Paylaşılan resim galerisi
Paylaşılan bir resim galerisi oluşturmak ve laboratuvar hesabınıza eklemek ücretsizdir. Bir resim sürümünü galeriye kaydedene kadar maliyetler tahakkuk etmez. Genellikle, paylaşılan bir resim galerisini kullanma fiyatlandırması oldukça önemsizdir, ancak Azure Lab Hizmetleri fiyatlandırmasına dahil olmadığından nasıl hesaplandığı anlamak önemlidir.  

#### <a name="storage-charges"></a>Depolama ücretleri
Görüntü sürümlerini depolamak için paylaşılan bir resim galerisi standart HDD yönetilen diskler kullanır. Kullanılan HDD yönetilen diskin boyutu, depolanan görüntü sürümünün boyutuna bağlıdır. Fiyatlandırmayı görüntülemek için aşağıdaki makaleye bakın: [Yönetilen diskler fiyatlandırması.](https://azure.microsoft.com/pricing/details/managed-disks/)


#### <a name="replication-and-network-egress-charges"></a>Çoğaltma ve ağ çıkış ücretleri
Bir sınıf laboratuvarının şablon sanal makinesini (VM) kullanarak bir resim sürümünü kaydettiğinizde, Azure Lab Hizmetleri önce bir kaynak bölgede depolar ve ardından kaynak görüntü sürümünü otomatik olarak bir veya daha fazla hedef bölgeye çoğaltır. Azure Laboratuvar Hizmetleri'nin kaynak görüntü sürümünü sınıf laboratuvarının bulunduğu [coğrafyadaki](https://azure.microsoft.com/global-infrastructure/regions/) tüm hedef bölgelere otomatik olarak çoğalttığına dikkat etmek önemlidir. Örneğin, sınıf laboratuvarınız ABD coğrafyasındaysa, bir görüntü sürümü ABD içinde bulunan sekiz bölgenin her birine çoğaltılır.

Bir görüntü sürümü kaynak bölgeden ek hedef bölgelere çoğaltıldığında ağ çıkış ücreti oluşur. Tahsil edilen tutar, görüntünün verileri ilk olarak kaynak bölgeden giden olarak aktarıldığında görüntü sürümünün boyutuna bağlıdır.  Fiyatlandırma ayrıntıları için aşağıdaki makaleye bakın: [Bant genişliği fiyatlandırma ayrıntıları.](https://azure.microsoft.com/pricing/details/bandwidth/)

[Eğitim çözümleri](https://www.microsoft.com/licensing/licensing-programs/licensing-for-industries?rtc=1&activetab=licensing-for-industries-pivot:primaryr3) müşterilerin çıkış ücretleri ödemekten feragat edilebilir. Daha fazla bilgi edinmek için hesap yöneticinizle konuşun.  Daha fazla bilgi için, özellikle "Akademik müşteriler için hangi veri aktarım programları var ve nasıl hak kazanırım?" sorusuna bağlı belgedeki **SSS** bölümüne bakın.

#### <a name="pricing-example"></a>Fiyatlandırma örneği
Yukarıda açıklanan fiyatlandırmayı özetlemek için, şablon vm görüntümüzün paylaşılan resim galerisine kaydedilmesine bir örnek verelim. Aşağıdaki senaryoları varsayalım:

- Bir özel VM görüntü var.
- Görüntünün iki versiyonunu kaydediyorsun.
- Laboratuvarınız, toplam sekiz bölgesi olan Abd'de.
- Her görüntü sürümü boyutu 32 GB; sonuç olarak, HDD tarafından yönetilen disk fiyatı ayda 1,54 TL'dir.

Toplam maliyet olarak tahmin edilir:

Görüntü sayısı × sürüm sayısı × çoğaltma sayısı × yönetilen disk fiyatı

Bu örnekte, maliyet:

1 özel resim (32 GB) x 2 versiyonx 8 ABD bölgeleri x $1.54 = $24.64 aylık

#### <a name="cost-management"></a>Maliyet yönetimi
Laboratuvar hesabı yöneticisinin, gereksiz resim sürümlerini galeriden düzenli olarak silerek maliyetleri yönetmesi önemlidir. 

Maliyetleri azaltmanın bir yolu olarak çoğaltmayı belirli bölgelere silmemeniz gerekir (bu seçenek paylaşılan resim galerisinde bulunur). Çoğaltma değişiklikleri, Azure Lab Hizmeti'nin paylaşılan bir resim galerisinde kaydedilen resimlerden VM yayımlama becerisi üzerinde olumsuz etkilere yol açabilir.

## <a name="next-steps"></a>Sonraki adımlar
Bir laboratuvar hesabı ve laboratuvar oluşturmak için adım adım talimatlar için öğreticiye bakın: [Kılavuzu Ayarla](tutorial-setup-lab-account.md)
