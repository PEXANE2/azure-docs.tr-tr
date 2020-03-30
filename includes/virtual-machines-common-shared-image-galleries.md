---
title: include dosyası
description: include dosyası
services: virtual-machines
author: axayjo
ms.service: virtual-machines
ms.topic: include
ms.date: 05/06/2019
ms.author: akjosh
ms.custom: include file
ms.openlocfilehash: a477114bda7d138a6860d21f2fad75e27d968833
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80117082"
---
Paylaşılan Resim Galerisi, yönetilen resimlerinizin etrafında yapı ve kuruluş oluşturmanıza yardımcı olan bir hizmettir. Paylaşılan Resim Galerileri şunları sağlar:

- Görüntülerin yönetilen küresel çoğaltma.
- Daha kolay yönetim için görüntülerin sürümü ve gruplanması.
- Kullanılabilirlik Bölgelerini destekleyen bölgelerdeki Bölge Yedekli Depolama (ZRS) hesaplarına sahip yüksek kullanılabilir görüntüler. ZRS, zonal arızalara karşı daha iyi esneklik sağlar.
- RBAC'ı kullanarak abonelikler arasında ve hatta Active Directory (AD) kiracıları arasında paylaşım.
- Dağıtımlarınızı her bölgedeki görüntü yinelemeleriyle ölçekleme.

Paylaşılan Resim Galerisi'ni kullanarak resimlerinizi kuruluşunuzdaki farklı kullanıcılarla, hizmet müdürleriyle veya REKLAM gruplarına paylaşabilirsiniz. Paylaşılan görüntüler, dağıtımlarınızın daha hızlı ölçeklendirilmesi için birden çok bölgeye çoğaltılabilir.

Yönetilen görüntü, görüntüyü nasıl oluşturduğunuza bağlı olarak tam vm (ekli veri diskleri dahil) veya yalnızca işletim sistemi diskinin kopyasıdır. Görüntüden bir VM oluşturduğunuzda, yeni VM için diskler oluşturmak için görüntüdeki VH'lerin bir kopyası kullanılır. Yönetilen görüntü depolama da kalır ve yeni VM'ler oluşturmak için tekrar tekrar kullanılabilir.

Korumanız gereken çok sayıda yönetilen görseliniz varsa ve bunları şirketinizde kullanılabilir hale getirmek istiyorsanız, resimlerinizi paylaşmayı kolaylaştıran bir depo olarak Paylaşılan Resim Galerisi'ni kullanabilirsiniz. 

Paylaşılan Resim Galerisi özelliğinin birden çok kaynak türü vardır:

| Kaynak | Açıklama|
|----------|------------|
| **Yönetilen görüntü** | Tek başına kullanılabilen veya resim galerisinde **görüntü sürümü** oluşturmak için kullanılabilen temel bir resim. Yönetilen görüntüler [genelleştirilmiş](#generalized-and-specialized-images) VM'lerden oluşturulur. Yönetilen görüntü, birden çok VM yapmak için kullanılabilen ve şimdi paylaşılan görüntü sürümleri oluşturmak için kullanılabilen özel bir VHD türüdür. |
| **Anlık Görüntü** | Bir **görüntü sürümü**yapmak için kullanılabilecek bir VHD kopyası. Anlık görüntüler, özel leştirilmiş bir görüntü sürümü oluşturmak için tek başına veya veri disklerinin anlık görüntüleriyle kullanılan [özel](#generalized-and-specialized-images) bir VM'den (genelleştirilmemiş bir) alınabilir.
| **Resim galerisi** | Azure Marketi gibi, **resim galerisi** de görüntüleri yönetmek ve paylaşmak için bir depodur, ancak kimin erişimi olduğunu siz denetlersiniz. |
| **Resim tanımı** | Görüntüler bir galeri içinde tanımlanır ve görüntü ve kuruluşunuz içinde kullanmak için gereksinimleri hakkında bilgi taşır. Görüntünün genelleştirilmiş veya özelleştirilmiş olup olmadığı, işletim sistemi, minimum ve maksimum bellek gereksinimleri ve sürüm notları gibi bilgileri ekleyebilirsiniz. Bu bir görüntü türünün tanımıdır. |
| **Görüntü sürümü** | **Resim sürümü,** galeri kullanırken VM oluşturmak için kullandığınız sürümdür. Ortamınız için gerektiği gibi görüntünün birden çok sürümüne sahip olabilirsiniz. Yönetilen bir görüntü gibi, VM oluşturmak için bir **görüntü sürümü** kullandığınızda, görüntü sürümü VM için yeni diskler oluşturmak için kullanılır. Görüntü sürümleri birden çok kez kullanılabilir. |

<br>

![Galerinizde bir resmin birden çok sürümüne nasıl sahip olabileceğinizi gösteren grafik](./media/shared-image-galleries/shared-image-gallery.png)

## <a name="image-definitions"></a>Resim tanımları

Görüntü tanımları, görüntünün sürümleri için mantıksal bir gruplandırmadır. Görüntü tanımı, görüntünün neden oluşturulduğu, işletim sistemi nin ne için olduğu ve görüntünün kullanımı hakkında bilgi içerir. Görüntü tanımı, belirli bir görüntü oluşturma yla ilgili tüm ayrıntılar için bir plan gibidir. Görüntü tanımından değil, tanımdan oluşturulan görüntü sürümünden vm dağıtın.

Her görüntü tanımı için birlikte kullanılan üç parametre vardır - **Publisher**, **Teklif** ve **SKU**. Bunlar, belirli bir görüntü tanımını bulmak için kullanılır. Bir veya iki değeri paylaşan, ancak üç değeri paylaşan görüntü sürümlerine sahip olabilirsiniz.  Örneğin, burada üç görüntü tanımları ve değerleri şunlardır:

|Görüntü Tanımı|Yayımcı|Sunduğu|Sku|
|---|---|---|---|
|myImage1|Contoso|Finans|Arka uç|
|myImage2|Contoso|Finans|Ön uç|
|myImage3|Sınama|Finans|Ön uç|

Bunların üçünün de benzersiz değer kümeleri var. Biçim, Bir Market görüntüsünün en son sürümünü elde etmek için Azure PowerShell'deki [Azure Marketi görüntüleri](../articles/virtual-machines/windows/cli-ps-findimage.md) için yayımcı, teklif ve SKU'yu şu anda nasıl belirtebileceğinize benzer. Her görüntü tanımının bu değerlerden oluşan benzersiz bir kümeye sahip olması gerekir.

Kaynaklarınızı daha kolay izleyebilmeniz için görüntü tanımınızda ayarlanabilecek diğer parametreler şunlardır:

* İşletim sistemi durumu - İşletim sistemi durumunu [genelleştirilmiş veya özel](#generalized-and-specialized-images)olarak ayarlayabilirsiniz.
* İşletim sistemi - Windows veya Linux olabilir.
* Açıklama - resim tanımının neden var olduğu hakkında daha ayrıntılı bilgi vermek için açıklamayı kullanın. Örneğin, uygulama önceden yüklenmiş olan ön uç sunucunuz için bir görüntü tanımınız olabilir.
* Eula - görüntü tanımına özgü bir son kullanıcı lisans sözleşmesine işaret etmek için kullanılabilir.
* Gizlilik Bildirimi ve Sürüm notları - sürüm notlarını ve gizlilik bildirimlerini Azure depolama alanında saklayın ve resim tanımının bir parçası olarak bunlara erişmek için bir URI sağlayın.
* Kullanım ömrü sona erme tarihi - eski görüntü tanımlarını silmek için otomasyonu kullanabilmek için görüntü tanımınıza kullanım tarihi nizi iliştirin.
* Etiket - resim tanımınızı oluştururken etiket ekleyebilirsiniz. Etiketler hakkında daha fazla bilgi için [kaynaklarınızı düzenlemek için etiketleri kullanma](../articles/azure-resource-manager/management/tag-resources.md)
* Minimum ve maksimum vCPU ve bellek önerileri - görüntünüzde vCPU ve bellek önerileri varsa, bu bilgileri görüntü tanımınıza ekleyebilirsiniz.
* İzin verilmeyen disk türleri - VM'nizin depolama gereksinimleri hakkında bilgi verebilirsiniz. Örneğin, görüntü standart HDD diskler için uygun değilse, bunları izin verilenler listesine eklersiniz.

## <a name="generalized-and-specialized-images"></a>Genelleştirilmiş ve özel leştirilmiş görüntüler

Paylaşılan Resim Galerisi tarafından desteklenen iki işletim sistemi durumları vardır. Genellikle görüntüler, görüntüyü oluşturmak için kullanılan VM'nin görüntüyü almadan önce genelleştirilmiş olmasını gerektirir. Genelleme, makine ve kullanıcıya özgü bilgileri VM'den kaldıran bir işlemdir. Windows için, Sysprep çok kullanılır. Linux için [waagent](https://github.com/Azure/WALinuxAgent) `-deprovision` veya `-deprovision+user` parametreleri kullanabilirsiniz.

Özel Leştirilmiş VM'ler, makineye özgü bilgileri ve hesapları kaldırmak için bir işlem den geçmemiş. Ayrıca, özel leştirilmiş görüntülerden oluşturulan VM'lerin bunlarla ilişkili bir `osProfile` ilişkisi yoktur. Bu, özel leştirilmiş görüntülerin bazı sınırlamaları olacağı anlamına gelir.

- VM'ye giriş yapmak için kullanılabilecek hesaplar, bu VM'den oluşturulan özel görüntü kullanılarak oluşturulan herhangi bir VM'de de kullanılabilir.
- VM'ler, görüntünün alındığı VM'nin **Bilgisayar adını** kullanır. Çakışmaları önlemek için bilgisayar adını değiştirmeniz gerekir.
- Bazı `osProfile` hassas bilgilerin VM'ye nasıl `secrets`iletildiğidir. Bu KeyVault, WinRM ve diğer işlevleri `secrets` kullanarak `osProfile`sorunlara neden olabilir. Bazı durumlarda, bu sınırlamaları aşmak için yönetilen hizmet kimliklerini (MSI) kullanabilirsiniz.

> [!IMPORTANT]
> Özelleştirilmiş görüntüler şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
>
> **Bilinen önizleme sınırlamaları** VM'ler yalnızca portal veya API kullanılarak özel görüntülerden oluşturulabilir. Önizleme için CLI veya PowerShell desteği yoktur.


## <a name="regional-support"></a>Bölgesel Destek

Kaynak bölgeler aşağıdaki tabloda listelenmiştir. Tüm ortak bölgeler hedef bölgeler olabilir, ancak Avustralya Merkez ve Avustralya Merkez 2'ye çoğaltmak için aboneliğinizin beyaz listeye alınması gerekir. Beyaz liste istemek için şu na gidin:https://azure.microsoft.com/global-infrastructure/australia/contact/


| Kaynak bölgeler        |                   |                    |                    |
| --------------------- | ----------------- | ------------------ | ------------------ |
| Orta Avustralya     | Doğu Çin        | Güney Hindistan        | Batı Avrupa        |
| Orta Avustralya 2   | Çin Doğu 2      | Güneydoğu Asya     | Güney Birleşik Krallık           |
| Doğu Avustralya        | Kuzey Çin       | Doğu Japonya         | Batı Birleşik Krallık            |
| Güneydoğu Avustralya   | Çin Kuzey 2     | Batı Japonya         | Orta US DoD     |
| Güney Brezilya          | Doğu Asya         | Güney Kore - Orta      | Doğu US DoD        |
| Orta Kanada        | Doğu ABD           | Güney Kore - Güney        | US Gov Arizona     |
| Doğu Kanada           | Doğu ABD 2         | Orta Kuzey ABD   | US Gov Texas       |
| Orta Hindistan         | Doğu ABD 2 EUAP    | Kuzey Avrupa       | US Gov Virginia    |
| Orta ABD            | Orta Fransa    | Orta Güney ABD   | Batı Hindistan         |
| ORTA ABD EUAP       | Güney Fransa      | Orta Batı ABD    | Batı ABD            |
|                       |                   |                    | Batı ABD 2          |



## <a name="limits"></a>Sınırlar 

Paylaşılan Resim Galerileri'ni kullanarak kaynakları dağıtmak için abonelik başına sınırlar vardır:
- Her gün, abonelik başına 100 paylaşılan resim galerisi
- Her bölge için abonelik başına 1.000 görüntü tanımı
- Her gün abonelik başına 10.000 görüntü sürümü
- Görüntüye iliştirilen herhangi bir disk boyutu 1 TB'den küçük veya eşit olmalıdır

Daha fazla bilgi için, geçerli kullanımınızı nasıl denetleyeceksiniz etütleri için [kaynak kullanımını sınırlara karşı](https://docs.microsoft.com/azure/networking/check-usage-against-limits) denetle'ye bakın.
 
## <a name="scaling"></a>Ölçeklendirme
Paylaşılan Resim Galerisi, Azure'un görüntülerin tutulmasını istediğiniz yineleme sayısını belirtmenize olanak tanır. Bu, VM dağıtımları farklı yinelemelere yayılabilirken, tek bir yinelemenin aşırı yüklenmesi nedeniyle örnek oluşturma işleminin daraltılması olasılığını azalttığı için çoklu VM dağıtım senaryolarında yardımcı olur.

Paylaşılan Resim Galerisi ile artık sanal makine ölçeğinde (yönetilen görüntülerle 600'den fazla) 1.000 VM'e kadar örnek dağıtabilirsiniz. Görüntü yinelemeleri daha iyi dağıtım performansı, güvenilirlik ve tutarlılık sağlar. Her hedef bölgede, bölgenin ölçek gereksinimlerine göre farklı bir yineleme sayısı ayarlayabilirsiniz. Her yineleme görüntünüzün derin bir kopyası olduğundan, bu, dağıtımlarınızı her ekstra yinelemeyle doğrusal olarak ölçeklendirmenize yardımcı olur. İki görüntü veya bölgenin aynı olmadığını anlamakla birlikte, bir bölgede yinelemelerin nasıl kullanılacağına ilişkin genel kılavuzumuz aşağıda veda edilebiştir:

- Sanal Makine Ölçeği Kümesi (VMSS) Dağıtımları için - Aynı anda oluşturduğunuz her 20 VM için bir yineleme bulundurmanızı öneririz. Örneğin, bir bölgede aynı görüntüyü aynı anda kullanarak 120 VM oluşturuyorsanız, görüntünüzün en az 6 kopyasını saklamanızı öneririz. 
- Sanal Makine Ölçeği Kümesi (VMSS) dağıtımları için - 600'e kadar örnekiçeren her ölçek kurulumu dağıtımı için en az bir yineleme bulundurmanızı öneririz. Örneğin, her biri tek bir bölgede aynı görüntüyü kullanan 600 VM örneği olan 5 ölçek kümesi oluşturuyorsanız, görüntünüzün en az 5 kopyasını saklamanızı öneririz. 

Görüntü boyutu, içerik ve işletim sistemi türü gibi etkenler nedeniyle çoğaltma sayısını aşmanızı her zaman öneririz.

![Görüntüleri nasıl ölçeklendirebileceğinizi gösteren grafik](./media/shared-image-galleries/scaling.png)

## <a name="make-your-images-highly-available"></a>Resimlerinizi yüksek oranda kullanılabilir hale getirin

[Azure Bölgesi Yedek Depolama (ZRS),](https://azure.microsoft.com/blog/azure-zone-redundant-storage-in-public-preview/) bölgedeki Kullanılabilirlik Bölgesi arızasına karşı esneklik sağlar. Paylaşılan Resim Galerisi'nin genel kullanılabilirliği sayesinde, resimlerinizi Kullanılabilirlik Bölgeleri olan bölgelerdeki ZRS hesaplarında saklamayı seçebilirsiniz. 

Hedef bölgelerin her biri için hesap türünü de seçebilirsiniz. Varsayılan depolama hesabı türü Standard_LRS, ancak Kullanılabilirlik Bölgeleri olan bölgeler için Standard_ZRS seçebilirsiniz. ZRS'nin bölgesel kullanılabilirliğini buradan kontrol [edin.](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs)

![ZRS gösteren grafik](./media/shared-image-galleries/zrs.png)

## <a name="replication"></a>Çoğaltma
Paylaşılan Resim Galerisi, resimlerinizi diğer Azure bölgelerine otomatik olarak çoğaltmanızı da sağlar. Paylaşılan Resim sürümüher kuruluşunuz için neyin anlamlı olduğu bağlı olarak farklı bölgelere çoğaltılabilir. Bir örnek, tüm eski sürümler yalnızca 1 bölgede kullanılabilirken, en son görüntüyü her zaman çok bölgelerde çoğaltmaktır. Bu, Paylaşılan Resim sürümlerinin depolama maliyetlerinde tasarruf sağlar. 

Paylaşılan Resim sürümü çoğaltılacak şekilde çoğaltılan bölgeler oluşturma zamanından sonra güncelleştirilebilir. Farklı bölgelere çoğaltmak için gereken süre, kopyalanan veri miktarına ve sürümün çoğaltılan bölge sayısına bağlıdır. Bu bazı durumlarda birkaç saat sürebilir. Çoğaltma olurken, bölge başına çoğaltma durumunu görüntüleyebilirsiniz. Görüntü çoğaltma bir bölgede tamamlandıktan sonra, bölgedeki bu görüntü sürümünü kullanarak bir VM veya ölçek kümesi dağıtabilirsiniz.

![Görüntüleri nasıl kopyalayabildiğinizi gösteren grafik](./media/shared-image-galleries/replication.png)

## <a name="access"></a>Erişim

Paylaşılan Resim Galerisi, Resim Tanımı ve Resim sürümü tüm kaynaklar olduğundan, yerleşik azure RBAC denetimleri kullanılarak paylaşılabilir. RBAC'ı kullanarak bu kaynakları diğer kullanıcılarla, hizmet ilkeleriyle ve gruplarla paylaşabilirsiniz. Hatta, içinde oluşturuldukları kiracı nın dışındaki kişilere erişimi de paylaşabilirsiniz. Bir kullanıcı Paylaşılan Resim sürümüne eriştikten sonra, bir VM veya Sanal Makine Ölçeği Kümesi dağıtabilir.  Aşağıda, kullanıcının neye eriştiyi anlamanıza yardımcı olan paylaşım matrisi ve bu matris ve bu matris inanın ve erişimin

| Kullanıcı ile Paylaşılan     | Paylaşılan Görüntü Galerisi | Görüntü Tanımı | Görüntü sürümü |
|----------------------|----------------------|--------------|----------------------|
| Paylaşılan Görüntü Galerisi | Evet                  | Evet          | Evet                  |
| Görüntü Tanımı     | Hayır                   | Evet          | Evet                  |

En iyi deneyim için Galeri düzeyinde paylaşmanızı öneririz. Tek tek görüntü sürümlerini paylaşmanızı önermiyoruz. RBAC hakkında daha fazla bilgi için [bkz.](../articles/role-based-access-control/role-assignments-portal.md)

Görüntüler, çok kiracılı bir uygulama kaydı kullanarak kiracılar arasında bile ölçekte paylaşılabilir. Görüntüleri kiracılar arasında paylaşma hakkında daha fazla bilgi için, [Azure kiracılarında Galeri VM görüntülerini paylaş'a](../articles/virtual-machines/linux/share-images-across-tenants.md)bakın.

## <a name="billing"></a>Faturalandırma
Paylaşılan Resim Galerisi hizmetini kullanmak için ek ücret alınmaz. Aşağıdaki kaynaklar için ücretlendirilirsiniz:
- Paylaşılan Resim sürümlerini depolamanın depolama maliyetleri. Maliyet, görüntü sürümünün yineleme sayısına ve sürümün çoğaltılan bölge sayısına bağlıdır. Örneğin, 2 resminiz varsa ve her ikisi de 3 bölgeye çoğaltılırsa, boyutlarına bağlı olarak 6 yönetilen disk için ücretlendirilirsiniz. Daha fazla bilgi için Yönetilen [Diskler](https://azure.microsoft.com/pricing/details/managed-disks/)fiyatlandırması'na bakın.
- Ağ çıkış ücretleri, ilk görüntü sürümünün kaynak bölgeden çoğaltılan bölgelere çoğaltılması için kopyalanıyor. Sonraki yinelemeler bölge içinde işlenir, bu nedenle ek ücret lendirme yapılmaz. 

## <a name="updating-resources"></a>Kaynakları güncelleştirme

Oluşturulduktan sonra, resim galerisi kaynaklarında bazı değişiklikler yapabilirsiniz. Bunlar aşağıdakiile sınırlıdır:
 
Paylaşılan resim galerisi:
- Açıklama

Resim tanımı:
- Önerilen vCPUs
- Önerilen bellek
- Açıklama
- Yaşam sonu tarihi

Resim sürümü:
- Bölgesel yineleme sayısı
- Hedef bölgeler
- En son sürümlerden hariç tutma
- Yaşam sonu tarihi

## <a name="sdk-support"></a>SDK desteği

Aşağıdaki SDK'lar Paylaşılan Resim Galerileri oluşturmayı destekler:

- [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/virtualmachines/management?view=azure-dotnet)
- [Java](https://docs.microsoft.com/java/azure/?view=azure-java-stable)
- [Node.js](https://docs.microsoft.com/javascript/api/@azure/arm-compute)
- [Python](https://docs.microsoft.com/python/api/overview/azure/virtualmachines?view=azure-python)
- [Git](https://docs.microsoft.com/azure/go/)

## <a name="templates"></a>Şablonlar

Şablonları kullanarak Paylaşılan Resim Galerisi kaynağı oluşturabilirsiniz. Kullanılabilir birkaç Azure Quickstart Şablonu vardır: 

- [Paylaşılan Resim Galerisi Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan Resim Galerisinde Resim Tanımı Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan Resim Galerisinde Resim Sürümü Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resim Sürümünden VM Oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular 

* [Tüm Paylaşılan Resim Galerisi kaynaklarını abonelikler arasında nasıl listeleyebilirim?](#how-can-i-list-all-the-shared-image-gallery-resources-across-subscriptions) 
* [Varolan resmimi paylaşılan resim galerisine taşıyabilir miyim?](#can-i-move-my-existing-image-to-the-shared-image-gallery)
* [Özel bir diskten görüntü sürümü oluşturabilir miyim?](#can-i-create-an-image-version-from-a-specialized-disk)
* [Paylaşılan Resim Galerisi kaynağını oluşturulduktan sonra farklı bir aboneye taşıyabilir miyim?](#can-i-move-the-shared-image-gallery-resource-to-a-different-subscription-after-it-has-been-created)
* [Görüntü sürümlerimi Azure China 21Vianet veya Azure Almanya veya Azure Resmi Bulut gibi bulutlar da çoğaltabilir miyim?](#can-i-replicate-my-image-versions-across-clouds-such-as-azure-china-21vianet-or-azure-germany-or-azure-government-cloud)
* [Abonelikler arasında resim sürümlerimi çoğaltabilir miyim?](#can-i-replicate-my-image-versions-across-subscriptions)
* [Azure AD kiracıları arasında resim sürümlerini paylaşabilir miyim?](#can-i-share-image-versions-across-azure-ad-tenants)
* [Hedef bölgelerdeki görüntü sürümlerini çoğaltmak ne kadar sürer?](#how-long-does-it-take-to-replicate-image-versions-across-the-target-regions)
* [Kaynak bölge ile hedef bölge arasındaki fark nedir?](#what-is-the-difference-between-source-region-and-target-region)
* [Görüntü sürümünü oluştururken kaynak bölgeyi nasıl belirtebilirim?](#how-do-i-specify-the-source-region-while-creating-the-image-version)
* [Her bölgede oluşturulacak görüntü sürümü yinelemelerinin sayısını nasıl belirtebilirim?](#how-do-i-specify-the-number-of-image-version-replicas-to-be-created-in-each-region)
* [Paylaşılan resim galerisini, resim tanımı ve resim sürümündekinden farklı bir konumda oluşturabilir miyim?](#can-i-create-the-shared-image-gallery-in-a-different-location-than-the-one-for-the-image-definition-and-image-version)
* [Paylaşılan Resim Galerisi'ni kullanma ücretleri nelerdir?](#what-are-the-charges-for-using-the-shared-image-gallery)
* [Paylaşılan Resim Galerisi ve Resim Tanımı ve Resim Sürümü oluşturmak için hangi API sürümünü kullanmalıyım?](#what-api-version-should-i-use-to-create-shared-image-gallery-and-image-definition-and-image-version)
* [Görüntü Sürümünden Ayarlanmış Paylaşılan VM veya Sanal Makine Ölçeği oluşturmak için hangi API sürümünü kullanmalıyım?](#what-api-version-should-i-use-to-create-shared-vm-or-virtual-machine-scale-set-out-of-the-image-version)

### <a name="how-can-i-list-all-the-shared-image-gallery-resources-across-subscriptions"></a>Tüm Paylaşılan Resim Galerisi kaynaklarını abonelikler arasında nasıl listeleyebilirim?

Azure portalında erişiminiz olan abonelikler arasında paylaşılan resim galerisi kaynaklarını listelemek için aşağıdaki adımları izleyin:

1. Azure [portalını](https://portal.azure.com)açın.
1. Tüm **Kaynaklara**gidin.
1. Tüm kaynakları listelemek istediğiniz tüm abonelikleri seçin.
1. **Tür Özel galeri**kaynaklarını arayın.
 
   Resim tanımlarını ve resim sürümlerini görmek için **gizli türleri göster'i**de seçmeniz gerekir.
 
   İzinaldığınız abonelikler arasında paylaşılan resim galerisi kaynaklarını listelemek için Azure CLI'de aşağıdaki komutu kullanın:

   ```azurecli
   az account list -otsv --query "[].id" | xargs -n 1 az sig list --subscription
   ```

### <a name="can-i-move-my-existing-image-to-the-shared-image-gallery"></a>Varolan resmimi paylaşılan resim galerisine taşıyabilir miyim?
 
Evet. Sahip olabileceğiniz görüntü türlerine göre 3 senaryo vardır.

 Senaryo 1: SIG'inizle aynı abonelikte yönetilen bir görüntünüz varsa, ondan bir görüntü tanımı ve resim sürümü oluşturabilirsiniz.

 Senaryo 2: SIG'inizle aynı abonelikte yönetilmeyen bir görüntünüz varsa, ondan yönetilen bir görüntü oluşturabilir ve ondan bir görüntü tanımı ve resim sürümü oluşturabilirsiniz. 

 Senaryo 3: Yerel dosya sisteminizde bir VHD varsa, VHD'yi yönetilen bir görüntüye yüklemeniz gerekir, ardından ondan bir görüntü tanımı ve resim sürümü oluşturabilirsiniz.

- VHD Windows VM'dense, [vhd yükleyin'](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed)e bakın.
- VHD bir Linux VM içinise, [bkz.](https://docs.microsoft.com/azure/virtual-machines/linux/upload-vhd#option-1-upload-a-vhd)

### <a name="can-i-create-an-image-version-from-a-specialized-disk"></a>Özel bir diskten görüntü sürümü oluşturabilir miyim?

Evet, görseller önizlemede olduğu için özel diskler desteği. Portalı[(Windows](../articles/virtual-machines/linux/shared-images-portal.md) veya [Linux)](../articles/virtual-machines/linux/shared-images-portal.md)ve API'yi kullanarak yalnızca özel leştirilmiş bir görüntüden bir VM oluşturabilirsiniz. Önizleme için PowerShell desteği yoktur.

### <a name="can-i-move-the-shared-image-gallery-resource-to-a-different-subscription-after-it-has-been-created"></a>Paylaşılan Resim Galerisi kaynağını oluşturulduktan sonra farklı bir aboneye taşıyabilir miyim?

Hayır, paylaşılan resim galerisi kaynağını farklı bir aboneye taşıyamazsınız. Ancak, galerideki resim sürümlerini gerektiğinde diğer bölgelere çoğaltabilirsiniz.

### <a name="can-i-replicate-my-image-versions-across-clouds-such-as-azure-china-21vianet-or-azure-germany-or-azure-government-cloud"></a>Görüntü sürümlerimi Azure China 21Vianet veya Azure Almanya veya Azure Resmi Bulut gibi bulutlar da çoğaltabilir miyim?

Hayır, görüntü sürümlerini bulutlar arasında kopyalayamazsınız.

### <a name="can-i-replicate-my-image-versions-across-subscriptions"></a>Abonelikler arasında resim sürümlerimi çoğaltabilir miyim?

Hayır, bir abonelikteki bölgeler arasında görüntü sürümlerini çoğaltabilir ve RBAC aracılığıyla diğer aboneliklerde kullanabilirsiniz.

### <a name="can-i-share-image-versions-across-azure-ad-tenants"></a>Azure AD kiracıları arasında resim sürümlerini paylaşabilir miyim? 

Evet, rbac'ı kiracılar arasında bireylerle paylaşmak için kullanabilirsiniz. Ancak, ölçekte paylaşmak için [PowerShell](../articles/virtual-machines/windows/share-images-across-tenants.md) veya [CLI'yi](../articles/virtual-machines/linux/share-images-across-tenants.md)kullanarak "Galeri resimlerini Azure kiracıları arasında paylaşın" başlıklı ana bakın.

### <a name="how-long-does-it-take-to-replicate-image-versions-across-the-target-regions"></a>Hedef bölgelerdeki görüntü sürümlerini çoğaltmak ne kadar sürer?

Görüntü sürümü çoğaltma süresi tamamen görüntünün boyutuna ve çoğaltılan bölgelerin sayısına bağlıdır. Ancak, en iyi yöntem olarak, görüntüyü küçük tutmanız ve kaynak ve hedef bölgeleri en iyi sonuçlar için yakın tutmanız önerilir. -ReplicationStatus bayrağını kullanarak çoğaltma durumunu denetleyebilirsiniz.

### <a name="what-is-the-difference-between-source-region-and-target-region"></a>Kaynak bölge ile hedef bölge arasındaki fark nedir?

Kaynak bölge, resim sürümünüzün oluşturulacağı bölgedir ve hedef bölgeler, resim sürümünüzün bir kopyasının depolanacağı bölgelerdir. Her resim sürümü için yalnızca bir kaynak bölgeniz olabilir. Ayrıca, bir resim sürümü oluştururken kaynak bölge konumunu hedef bölgelerden biri olarak geçtiğinden emin olun.

### <a name="how-do-i-specify-the-source-region-while-creating-the-image-version"></a>Görüntü sürümünü oluştururken kaynak bölgeyi nasıl belirtebilirim?

Görüntü sürümü oluştururken, kaynak bölgeyi belirtmek için CLI'deki **konum** etiketini ve PowerShell'deki **Konum** etiketini kullanabilirsiniz. Lütfen görüntü sürümünü oluşturmak için temel görüntü olarak kullandığınız yönetilen görüntünün, görüntü sürümünü oluşturmak istediğiniz konumla aynı konumda olduğundan emin olun. Ayrıca, bir resim sürümü oluştururken kaynak bölge konumunu hedef bölgelerden biri olarak geçtiğinden emin olun.  

### <a name="how-do-i-specify-the-number-of-image-version-replicas-to-be-created-in-each-region"></a>Her bölgede oluşturulacak görüntü sürümü yinelemelerinin sayısını nasıl belirtebilirim?

Her bölgede oluşturulacak görüntü sürümü yinelemelerinin sayısını belirtmenin iki yolu vardır:
 
1. Bölge başına oluşturmak istediğiniz yineleme sayısını belirten bölgesel yineleme sayısı. 
2. Bölgesel yineleme sayısı belirtilmemişse, bölge sayısı başına varsayılan olan ortak yineleme sayısı. 

Bölgesel yineleme sayısını belirtmek için, konumu o bölgede oluşturmak istediğiniz yineleme sayısıyla birlikte geçirin: "Güney Orta ABD=2". 

Her konumla bölgesel yineleme sayısı belirtilmemişse, varsayılan yineleme sayısı belirttiğiniz ortak yineleme sayısı olacaktır. 

CLI'de ortak yineleme sayısını belirtmek için, komuttaki `az sig image-version create` **--çoğaltma sayısı** bağımsız değişkenini kullanın.

### <a name="can-i-create-the-shared-image-gallery-in-a-different-location-than-the-one-for-the-image-definition-and-image-version"></a>Paylaşılan resim galerisini, resim tanımı ve resim sürümündekinden farklı bir konumda oluşturabilir miyim?

Evet, olabilir. Ancak, en iyi uygulama olarak, kaynak grubunu, paylaşılan resim galerisini, resim tanımını ve resim sürümünü aynı konumda tutmanızı öneririz.

### <a name="what-are-the-charges-for-using-the-shared-image-gallery"></a>Paylaşılan Resim Galerisi'ni kullanma ücretleri nelerdir?

Görüntü sürümlerini depolamak için depolama ücretleri ve görüntü sürümlerini kaynak bölgeden hedef bölgelere çoğaltmak için ağ çıkış ücretleri dışında Paylaşılan Resim Galerisi hizmetini kullanmak için herhangi bir ücret yoktur.

### <a name="what-api-version-should-i-use-to-create-shared-image-gallery-and-image-definition-and-image-version"></a>Paylaşılan Resim Galerisi ve Resim Tanımı ve Resim Sürümü oluşturmak için hangi API sürümünü kullanmalıyım?

Paylaşılan resim galerileri, resim tanımları ve resim sürümleriyle çalışmak için API 2018-06-01 sürümünü kullanmanızı öneririz. Bölge Yedekli Depolama (ZRS) sürümü gerektirir 2019-03-01 veya daha sonra.

### <a name="what-api-version-should-i-use-to-create-shared-vm-or-virtual-machine-scale-set-out-of-the-image-version"></a>Görüntü Sürümünden Ayarlanmış Paylaşılan VM veya Sanal Makine Ölçeği oluşturmak için hangi API sürümünü kullanmalıyım?

VM ve Sanal Makine Ölçeği bir resim sürümünü kullanarak dağıtımları ayarlayın, API sürümünü 2018-04-01 veya daha yüksek kullanmanızı öneririz.
