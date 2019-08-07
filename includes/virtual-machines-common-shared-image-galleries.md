---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/06/2019
ms.author: akjosh; cynthn
ms.custom: include file
ms.openlocfilehash: e43b8c951d8c7d5d60904fe49d8639efaba5c89f
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775938"
---
Paylaşılan görüntü Galerisi, yönetilen görüntülerinizin etrafında yapı ve kuruluş oluşturmanıza yardımcı olan bir hizmettir. Paylaşılan görüntü galerileri şunları sağlar:

- Görüntülerin yönetilen genel çoğaltması.
- Daha kolay yönetim için görüntülerin sürümü oluşturma ve gruplama.
- Kullanılabilirlik Alanları destekleyen bölgelerde bölge yedekli depolama (ZRS) hesaplarıyla yüksek düzeyde kullanılabilir görüntüler. ZRS, bölgesel arızalara karşı daha iyi koruma sağlar.
- RBAC kullanarak abonelikler arasında ve hatta Active Directory (AD) kiracılar arasında paylaşım.
- Her bölgedeki görüntü çoğaltmalarıyla dağıtımlarınızı ölçeklendirin.

Paylaşılan görüntü galerisini kullanarak, görüntülerinizi kuruluşunuzdaki farklı kullanıcılara, hizmet sorumlularına veya AD gruplarına paylaşabilirsiniz. Paylaşılan görüntüler, dağıtımlarınızın daha hızlı ölçeklendirilmesi için birden çok bölgeye çoğaltılabilir.

Yönetilen görüntü, görüntüyü oluşturma yönteminize bağlı olarak tam VM 'nin (bağlı veri diskleri dahil) veya yalnızca işletim sistemi diskinin bir kopyasıdır. Görüntüden bir VM oluşturduğunuzda, yeni VM 'nin disklerini oluşturmak için görüntüdeki VHD 'lerin bir kopyası kullanılır. Yönetilen görüntü depolamada kalır ve yeni VM 'Ler oluşturmak için tekrar tekrar kullanılabilir.

Korumanız gereken çok sayıda yönetilen görüntünüz varsa ve bunları şirketiniz genelinde kullanılabilir hale getirmek istiyorsanız, paylaşılan bir görüntü galerisini bir depo olarak kullanarak görüntülerinizi paylaşmayı kolaylaştırır. 

Paylaşılan görüntü Galerisi özelliği birden çok kaynak türüne sahiptir:

| Resource | Açıklama|
|----------|------------|
| **Yönetilen görüntü** | Tek başına kullanılabilen veya bir görüntü galerisinde **görüntü sürümü** oluşturmak için kullanılan temel bir görüntü. Yönetilen görüntüler Genelleştirilmiş VM 'lerden oluşturulur. Yönetilen görüntü, birden çok VM oluşturmak için kullanılabilen ve artık paylaşılan görüntü sürümleri oluşturmak için kullanılabilen özel bir VHD türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** , görüntüleri yönetmek ve paylaşmak için bir depodur, ancak kimlerin erişimi olduğunu kontrol edersiniz. |
| **Görüntü tanımı** | Görüntüler, bir galeri içinde tanımlanır ve bu görüntüyü kuruluşunuzda kullanmaya yönelik gereksinimler hakkında bilgi taşır. Görüntünün Windows veya Linux, en düşük ve en yüksek bellek gereksinimleri ve sürüm notları gibi bilgileri ekleyebilirsiniz. Bu, bir görüntü türünün tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** , galerı kullanılırken VM oluşturmak için kullandığınız şeydir. Ortamınız için gerektiğinde bir görüntünün birden fazla sürümüne sahip olabilirsiniz. Yönetilen bir görüntü gibi, bir sanal makine oluşturmak için bir **görüntü sürümü** kullandığınızda, sanal makine için yeni diskler oluşturmak üzere görüntü sürümü kullanılır. Görüntü sürümleri birden çok kez kullanılabilir. |

<br>


![Galerinizdeki bir görüntünün birden fazla sürümünü nasıl kullanabileceğinizi gösteren grafik](./media/shared-image-galleries/shared-image-gallery.png)

## <a name="image-definitions"></a>Görüntü tanımları

Görüntü tanımları bir görüntünün sürümleri için bir mantıksal gruplandırmadır. Görüntü tanımı, görüntünün neden oluşturulduğu, ne kadar işletim sistemi olduğu ve görüntüyü kullanma hakkında bilgi içeren bilgileri barındırır. Bir görüntü tanımı, belirli bir görüntü oluşturma ile ilgili tüm ayrıntılar için bir plan gibidir. Bir görüntü tanımından, ancak tanımdan oluşturulan görüntü sürümünden bir VM dağıtmazsınız.


Her görüntü tanımı için, kombinasyon- **Yayımcı**, **teklif** ve **SKU**'da kullanılan üç parametre vardır. Bunlar, belirli bir görüntü tanımını bulmak için kullanılır. Üç değerden birini veya ikisini birden paylaşan görüntü sürümlerine sahip olabilirsiniz.  Örneğin, aşağıda üç görüntü tanımı ve değerleri verilmiştir:

|Görüntü Tanımı|Yayımcı|Sunduğu|Sku|
|---|---|---|---|
|myImage1|Contoso|Finans|Arka uç|
|myImage2|Contoso|Finans|Ön uç|
|myImage3|Test Etme|Finans|Ön uç|

Bunların üçü de benzersiz değer kümelerine sahiptir. Bu biçim, bir market görüntüsünün en son sürümünü almak için Azure PowerShell ' de [Azure Market görüntüleri](../articles/virtual-machines/windows/cli-ps-findimage.md) için yayımcı, TEKLIF ve SKU 'yu nasıl belirteceğinize benzer. Her görüntü tanımının bu değerlerin benzersiz bir kümesine sahip olması gerekir.

Aşağıda, kaynaklarınızı daha kolay izleyebilmek için görüntü tanımınızda ayarlanabilme diğer parametreler verilmiştir:

* İşletim sistemi durumu-işletim sistemi durumunu genelleştirilemez veya özelleşmiş olarak ayarlayabilirsiniz, ancak şu anda yalnızca genelleştirilemez. Görüntülerin, Windows için Sysprep veya `waagent -deprovision` Linux için genelleştirilmiş kullanılarak genelleştirilmiş VM 'lerden oluşturulması gerekir.
* İşletim sistemi-Windows veya Linux olabilir.
* Açıklama-görüntü tanımının neden mevcut olduğuna ilişkin daha ayrıntılı bilgi için açıklama kullanın. Örneğin, ön uç sunucunuz için uygulamanın önceden yüklenmiş olduğu bir görüntü tanımınız olabilir.
* EULA-görüntü tanımına özgü bir son kullanıcı lisans anlaşmasını işaret etmek için kullanılabilir.
* Gizlilik bildirimi ve sürüm notları-Azure Storage 'da sürüm notlarını ve gizlilik bildirimlerini depolayın ve görüntü tanımının bir parçası olarak bunlara erişmek için bir URI sağlayın.
* Son geçerlilik tarihi-eski görüntü tanımlarını silmek üzere Otomasyonu kullanabilmeniz için görüntü tanımınıza bir yaşam sonu tarihi ekleyin.
* Etiket-görüntü tanımınızı oluştururken Etiketler ekleyebilirsiniz. Etiketler hakkında daha fazla bilgi için bkz. [kaynakları düzenlemek için etiketleri kullanma](../articles/azure-resource-manager/resource-group-using-tags.md)
* En düşük ve en yüksek vCPU ve bellek önerileri-görüntünüz vCPU ve bellek önerileri içeriyorsa, bu bilgileri görüntü tanımınıza ekleyebilirsiniz.
* İzin verilmeyen disk türleri-sanal makinenizin depolama gereksinimleriyle ilgili bilgiler sağlayabilirsiniz. Örneğin, görüntü standart HDD disklerine uygun değilse, bunları izin vermeme listesine eklersiniz.


## <a name="regional-support"></a>Bölgesel destek

Kaynak bölgeler aşağıdaki tabloda listelenmiştir. Tüm ortak bölgeler hedef bölge olabilir, ancak Avustralya Orta ve Avustralya Orta 2 çoğaltmak için aboneliğinizi beyaz listeye almanız gerekir. Beyaz liste istemek için şuraya gidin: https://azure.microsoft.com/en-au/global-infrastructure/australia/contact/


| Kaynak bölgeler |
|---------------------|-----------------|------------------|-----------------|
| Avustralya Orta   | Orta ABD EUAP | Kore Orta    | Batı Orta ABD |
| Avustralya Orta 2 | Doğu Asya       | Kore Güney      | Batı Avrupa     |
| Avustralya Doğu      | East US         | Orta Kuzey ABD | Batı Hindistan      |
| Avustralya Güneydoğu | Doğu ABD 2       | Kuzey Avrupa     | Batı ABD         |
| Güney Brezilya        | Orta ABD 2 EUAP  | Orta Güney ABD | Batı ABD 2       |
| Orta Kanada      | Fransa Orta  | Güney Hindistan      |                 |
| Doğu Kanada         | Fransa Güney    | Güneydoğu Asya   |                 |
| Orta Hindistan       | Japonya Doğu      | Birleşik Krallık Güney         |                 |
| Orta ABD          | Japonya Batı      | Birleşik Krallık Batı          |                 |



## <a name="limits"></a>Sınırlar 

Paylaşılan görüntü galerileri kullanarak kaynak dağıtmak için abonelik başına sınırlar vardır:
- 100 paylaşılan görüntü galerileri, her bölge için abonelik başına
- 1\.000 görüntü tanımları, her bölge için abonelik başına
- 10.000 görüntü sürümü, her bölge için abonelik başına

Daha fazla bilgi için bkz. geçerli kullanımınızı nasıl denetabileceğine ilişkin örnekler için [sınırlara karşı kaynak kullanımını denetleme](https://docs.microsoft.com/azure/networking/check-usage-against-limits) .
 

## <a name="scaling"></a>Ölçeklendirme
Paylaşılan görüntü Galerisi, Azure 'un görüntüleri tutmasını istediğiniz çoğaltma sayısını belirtmenize olanak tanır. Bu, sanal makine dağıtımları farklı çoğaltmalara yayılabilecek olduğundan, tek bir çoğaltmanın aşırı yüklemesi nedeniyle örnek oluşturma işleminin kısıtlanıyor olma olasılığını azaltmak için çoklu VM dağıtım senaryolarında yardımcı olur.


Paylaşılan görüntü Galerisi ile artık bir sanal makine ölçek kümesindeki bir 1.000 VM örneğine (yönetilen görüntülerle 600 ' den fazla) dağıtabilirsiniz. Görüntü çoğaltmaları daha iyi dağıtım performansı, güvenilirlik ve tutarlılık sağlar.  Bölgenin ölçek ihtiyaçlarına göre her bir hedef bölgede farklı bir çoğaltma sayısı ayarlayabilirsiniz. Her çoğaltma görüntünüzün derin bir kopyası olduğundan, bu, her bir ekstra çoğaltma ile dağıtımlarınızın ölçeğini daha erken ölçeklendirmenize yardımcı olur. İki görüntü veya bölgenin aynı olmadığını anladığımızda, bir bölgede çoğaltmaların nasıl kullanılacağına ilişkin genel kılavuzumuz aşağıda verilmiştir:

- Aynı anda oluşturduğunuz her 20 VM için bir çoğaltmayı tutmanız önerilir. Örneğin, bir bölgedeki aynı görüntüyü kullanarak aynı anda 120 VM oluşturuyorsanız, resminizin en az 6 çoğaltmasını tutmanız önerilir. 
- En fazla 600 örneği olan her ölçek kümesi dağıtımı için en az bir çoğaltma tutmanız önerilir. Örneğin, her biri tek bir bölgedeki aynı görüntüyü kullanan 600 VM örneğiyle aynı anda 5 ölçek kümesi oluşturuyorsanız, resminizin en az 5 çoğaltmasını tutmanızı öneririz. 

Görüntü boyutu, içerik ve işletim sistemi türü gibi etkenlere bağlı olarak her zaman çoğaltma sayısını fazla temin etmenizi öneririz.


![Görüntüleri nasıl ölçeklendirekullanabileceğinizi gösteren grafik](./media/shared-image-galleries/scaling.png)



## <a name="make-your-images-highly-available"></a>Görüntülerinizi yüksek oranda kullanılabilir yapma

[Azure bölgesel olarak yedekli depolama (ZRS)](https://azure.microsoft.com/blog/azure-zone-redundant-storage-in-public-preview/) , bölgedeki bir kullanılabilirlik bölgesi hatasına karşı esnekliği sağlar. Paylaşılan görüntü galerisinin genel kullanıma sunulmasıyla birlikte, görüntülerinizi Kullanılabilirlik Alanları bölgelerde ZRS hesaplarında depolamayı tercih edebilirsiniz. 

Hedef bölgelerin her biri için hesap türünü de seçebilirsiniz. Varsayılan depolama hesabı türü Standard_LRS ' dir, ancak Kullanılabilirlik Alanları bölgeler için Standard_ZRS ' yi seçebilirsiniz. ZRS 'nin bölgesel kullanılabilirliğini [burada](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs)denetleyin.

![ZRS gösteren grafik](./media/shared-image-galleries/zrs.png)


## <a name="replication"></a>Çoğaltma
Paylaşılan görüntü Galerisi, görüntülerinizi diğer Azure bölgelerine otomatik olarak çoğaltmanıza da olanak tanır. Her paylaşılan görüntü sürümü, kuruluşunuz için anlamlı hale göre farklı bölgelere çoğaltılabilir. Bir örnek, tüm eski sürümler yalnızca 1 bölgede kullanılabilir durumdayken çok bölgeli en son görüntüyü her zaman çoğaltmalıdır. Bu, paylaşılan görüntü sürümleri için depolama maliyetlerinde tasarruf etmenize yardımcı olabilir. 

Paylaşılan görüntü sürümünün çoğaltılacağı bölgeler, oluşturma zamanından sonra güncelleştirilebilirler. Farklı bölgelere çoğaltılmak için gereken süre, kopyalanan veri miktarına ve sürümün çoğaltılan bölge sayısına bağlıdır. Bu, bazı durumlarda birkaç saat sürebilir. Çoğaltma gerçekleşirken, her bölge için çoğaltmanın durumunu görüntüleyebilirsiniz. Bir bölgede görüntü çoğaltma tamamlandıktan sonra, bölgedeki bu görüntü sürümünü kullanarak bir VM veya ölçek kümesi dağıtabilirsiniz.

![Görüntüleri nasıl çoğaltacağınızı gösteren grafik](./media/shared-image-galleries/replication.png)


## <a name="access"></a>Access

Paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümü tüm kaynaklar olduğundan, yerleşik yerel Azure RBAC denetimleri kullanılarak paylaşılabilir. RBAC kullanarak bu kaynakları diğer kullanıcılar, hizmet sorumluları ve gruplar ile paylaşabilirsiniz. Hatta, içinde oluşturuldukları kiracı dışındaki bireylere erişim de paylaşabilirsiniz. Bir kullanıcının paylaşılan görüntü sürümüne erişimi olduktan sonra, bir VM veya bir sanal makine ölçek kümesi dağıtabilirler.  Kullanıcının ne erişimi olduğunu anlamanıza yardımcı olan paylaşım matrisi aşağıda verilmiştir:

| Kullanıcıyla paylaşıldı     | Paylaşılan görüntü galerisi | Görüntü Tanımı | Görüntü sürümü |
|----------------------|----------------------|--------------|----------------------|
| Paylaşılan görüntü galerisi | Evet                  | Evet          | Evet                  |
| Görüntü Tanımı     | Hayır                   | Evet          | Evet                  |

En iyi deneyim için Galeri düzeyinde paylaşım yapmanızı öneririz. Ayrı görüntü sürümlerinin paylaşılmasını önermiyoruz. RBAC hakkında daha fazla bilgi için bkz. [RBAC kullanarak Azure kaynaklarına erişimi yönetme](../articles/role-based-access-control/role-assignments-portal.md).

Görüntüler aynı zamanda çok kiracılı bir uygulama kaydı kullanan kiracılar arasında bile ölçeklenebilir şekilde paylaşılabilir. Kiracılar arasında görüntü paylaşma hakkında daha fazla bilgi için bkz. [Azure kiracılar genelinde galerı VM görüntülerini paylaşma](../articles/virtual-machines/linux/share-images-across-tenants.md).

## <a name="billing"></a>Faturalandırma
Paylaşılan Görüntü Galerisi hizmetini kullanırken ekstra ücret ödemezsiniz. Aşağıdaki kaynaklar için ücretlendirilirsiniz:
- Paylaşılan görüntü sürümlerini depolamanın depolama maliyeti. Maliyet, görüntü sürümünün yineleme sayısına ve sürümün çoğaltılacağı bölge sayısına bağlıdır. Örneğin, 2 görüntünüz varsa ve her ikisi de 3 bölgeye çoğaltılırsa, boyutları temelinde 6 yönetilen disk için değişiklik yapılır. Daha fazla bilgi için bkz. [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/).
- Kaynak bölgeden çoğaltılan bölgelere ilk görüntü sürümünün çoğaltılmasıyla ilgili ağ çıkış ücretleri. Sonraki çoğaltmalar bölge içinde işlenir, bu nedenle ek ücret alınmaz. 

## <a name="updating-resources"></a>Kaynaklar güncelleştiriliyor

Oluşturulduktan sonra, görüntü Galerisi kaynaklarında bazı değişiklikler yapabilirsiniz. Bunlarla sınırlı:
 
Paylaşılan görüntü Galerisi:
- Açıklama

Görüntü tanımı:
- Önerilen vCPU 'Lar
- Önerilen bellek
- Açıklama
- Yaşam tarihi sonu

Görüntü sürümü:
- Bölgesel çoğaltma sayısı
- Hedef bölgeler
- En son dışında tut
- Yaşam tarihi sonu


## <a name="sdk-support"></a>SDK desteği

Aşağıdaki SDK 'lar paylaşılan görüntü galerileri oluşturmayı destekler:

- [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/virtualmachines/management?view=azure-dotnet)
- [Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable)
- [Node.js](https://docs.microsoft.com/javascript/api/azure-arm-compute/?view=azure-node-latest)
- [Python](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)
- [Go](https://docs.microsoft.com/go/azure/)

## <a name="templates"></a>Şablonlar

Şablonlar kullanarak paylaşılan görüntü Galerisi kaynağı oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan görüntü galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan görüntü galerisinde görüntü tanımı oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Görüntü sürümünden VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

**S.** Tüm paylaşılan görüntü Galerisi kaynaklarını abonelikler arasında nasıl listeleyebilirim? 
 
 A. Azure portal erişiminiz olan aboneliklerdeki tüm paylaşılan görüntü Galerisi kaynaklarını listelemek için aşağıdaki adımları izleyin:

1. [Azure portalı](https://portal.azure.com) açın.
1. **Tüm kaynaklara**gidin.
1. Tüm kaynakları listelemek istediğiniz tüm abonelikleri seçin.
1. **Özel Galeri**türündeki kaynakları arayın.
 
   Görüntü tanımlarını ve görüntü sürümlerini görmek için **gizli türleri göster**' i de seçmeniz gerekir.
 
   İzinleriniz olan aboneliklerdeki tüm paylaşılan görüntü Galerisi kaynaklarını listelemek için Azure CLı 'de aşağıdaki komutu kullanın:

   ```bash
   az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
   ```


**S.** Var olan görüntümi paylaşılan görüntü galerisine taşıyabilir miyim?
 
 A. Evet. Sahip olduğunuz görüntü türlerine göre 3 senaryo vardır.

 Senaryo 1: Yönetilen bir görüntünüz varsa, bundan sonra bir görüntü tanımı ve görüntü sürümü oluşturabilirsiniz.

 Senaryo 2: Yönetilmeyen bir Genelleştirilmiş görüntünüz varsa, bundan yönetilen bir görüntü oluşturabilir ve bundan sonra bir görüntü tanımı ve görüntü sürümü oluşturabilirsiniz. 

 Senaryo 3: Yerel dosya sisteminizde bir VHD 'niz varsa, VHD 'yi karşıya yüklemeniz, yönetilen bir görüntü oluşturmanız ve bundan sonra buradan görüntü tanımı ve görüntü sürümü oluşturmanız gerekir.
- VHD bir Windows sanal makinesi ise bkz. [Genelleştirilmiş BIR VHD 'Yi karşıya yükleme](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed).
- VHD bir Linux sanal makinesi için ise bkz. [BIR VHD 'Yi karşıya yükleme](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd)


**S.** Özel bir diskten görüntü sürümü oluşturabilir miyim?

 A. Hayır, şu anda özel diskleri görüntü olarak desteklemiyoruz. Özel bir diskiniz varsa, özel diski yeni bir sanal makineye ekleyerek [VHD 'den BIR VM oluşturmanız](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal#create-a-vm-from-a-disk) gerekir. Çalışan bir sanal makine oluşturduktan sonra, [WINDOWS VM](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-custom-images) veya [Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-custom-images)'den yönetilen bir görüntü oluşturmak için yönergeleri izlemeniz gerekir. Genelleştirilmiş bir yönetilen görüntünüz olduktan sonra, paylaşılan bir görüntü açıklaması ve görüntü sürümü oluşturmak için işlemi başlatabilirsiniz.

 
**S.** Oluşturulduktan sonra paylaşılan görüntü Galerisi kaynağını farklı bir aboneliğe taşıyabilir miyim?

 A. Hayır, paylaşılan görüntü Galerisi kaynağını farklı bir aboneliğe taşıyamazsınız. Bununla birlikte, galerideki görüntü sürümlerini, gereken şekilde diğer bölgelere çoğaltabileceksiniz.

**S.** My-Azure Çin 21Vianet, Azure Almanya ve Azure Kamu Bulutu bulutları genelinde görüntü sürümlerimi çoğaltabilir miyim? 

 A. Hayır, bulut genelinde görüntü sürümlerini çoğaltamaz.

**S.** Görüntü sürümlerimi abonelikler arasında çoğaltabilir miyim? 

 A. Hayır, bir abonelikteki bölgeler arasında görüntü sürümlerini çoğaltabilir ve RBAC aracılığıyla diğer aboneliklerde kullanabilirsiniz.

**S.** Azure AD kiracılar arasında görüntü sürümlerini paylaşabilir miyim? 

 A. Evet, kiracılar genelinde bireyler paylaşmak için RBAC kullanabilirsiniz. Ancak, ölçekte paylaşmak için, [PowerShell](../articles/virtual-machines/windows/share-images-across-tenants.md) veya [CLI](../articles/virtual-machines/linux/share-images-across-tenants.md)kullanarak "Azure Kiracılarında Galeri görüntülerini paylaşma" konusuna bakın.


**S.** Hedef bölgeler arasında görüntü sürümlerini çoğaltmak ne kadar sürer?

 A. Görüntü sürümü çoğaltma süresi, görüntünün boyutuna ve çoğaltılan bölge sayısına bağlıdır. Bununla birlikte, en iyi uygulama olarak, görüntüyü küçük tutmanız ve kaynak ve hedef bölgelerin en iyi sonuçlar için kapalı olması önerilir. -ReplicationStatus bayrağını kullanarak çoğaltmanın durumunu kontrol edebilirsiniz.


**S.** Kaynak bölgesi ve hedef bölge arasındaki fark nedir?

 A. Kaynak bölge, görüntü sürümünüzün oluşturulacağı bölgedir ve hedef bölgeler, görüntü sürümünüzün bir kopyasının depolanacağı bölgelerdir. Her görüntü sürümü için yalnızca bir kaynak bölgeniz olabilir. Ayrıca, bir görüntü sürümü oluştururken kaynak bölge konumunu hedef bölgelerden biri olarak geçirdiğinizden emin olun.  


**S.** Görüntü sürümünü oluştururken kaynak bölgeyi belirtmek Nasıl yaparım? mı?

 A. Görüntü sürümü oluştururken, kaynak bölgeyi belirtmek için PowerShell 'deki **--Location** etiketini ve **-Location** etiketini kullanabilirsiniz. Görüntü sürümünü oluşturmak için temel görüntü olarak kullandığınız yönetilen görüntünün, görüntü sürümünü oluşturmayı düşündüğünüz konumla aynı konumda olduğundan emin olun. Ayrıca, bir görüntü sürümü oluştururken kaynak bölge konumunu hedef bölgelerden biri olarak geçirdiğinizden emin olun.  


**S.** Nasıl yaparım? her bölgede oluşturulacak görüntü sürümü çoğaltmalarının sayısını belirtin.

 A. Her bölgede oluşturulacak görüntü sürümü çoğaltmalarının sayısını iki şekilde belirtebilirsiniz:
 
1. Bölge başına oluşturmak istediğiniz çoğaltmaların sayısını belirten bölgesel çoğaltma sayısı. 
2. Bölgesel çoğaltma sayısında bölge sayısı başına varsayılan olan ortak çoğaltma sayısı belirtilmemiş. 

Bölgesel çoğaltma sayısını belirtmek için, konumu bu bölgede oluşturmak istediğiniz çoğaltmaların sayısıyla birlikte geçirin: "Orta Güney ABD = 2". 

Bölgesel çoğaltma sayısı her konum ile belirtilmediyse, varsayılan çoğaltma sayısı belirttiğiniz ortak çoğaltma sayısı olacaktır. 

CLI 'de ortak çoğaltma sayısını belirtmek için, `az sig image-version create` komutta **--Replica-Count** bağımsız değişkenini kullanın.


**S.** Paylaşılan görüntü galerisini, görüntü tanımını ve görüntü sürümünü oluşturmak istediğimizden farklı bir konumda oluşturabilir miyim?

 A. Evet, olabilir. Ancak, en iyi uygulama olarak, kaynak grubu, paylaşılan görüntü Galerisi, görüntü tanımı ve görüntü sürümünü aynı konumda tutmanız önerilir.


**S.** Paylaşılan görüntü galerisini kullanma ücretleri nelerdir?

 A. Görüntü sürümlerini depolama ücretleri ve kaynak bölgeden hedef bölgelere çoğaltma için ağ çıkış ücretleri dışında, paylaşılan görüntü Galerisi hizmeti kullanımı için herhangi bir ücret alınmaz.

**S.** Görüntü sürümünden paylaşılan görüntü Galerisi, görüntü tanımı, görüntü sürümü ve VM/VMSS oluşturmak için hangi API sürümünün kullanılması gerekir?

 A. Bir görüntü sürümü kullanan VM ve sanal makine ölçek kümesi dağıtımları için API sürüm 2018-04-01 veya üstünü kullanmanızı öneririz. Paylaşılan görüntü galerileri, görüntü tanımları ve görüntü sürümleriyle çalışmak için API sürüm 2018-06-01 ' i kullanmanızı öneririz. Bölge yedekli depolama (ZRS) sürüm 2019-03-01 veya üstünü gerektirir.
