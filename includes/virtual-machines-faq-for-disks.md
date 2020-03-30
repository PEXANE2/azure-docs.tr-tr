---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/13/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 53bbee6dd75e045c2a7e95c88a0138c9859d12db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80374021"
---
Bu makalede, Azure Yönetilen Diskler ve Azure Premium SSD diskleri hakkında sık sorulan bazı sorular yanıtlanmaktadır.

## <a name="managed-disks"></a>Yönetilen Diskler

**Azure Yönetilen Diskler nedir?**

Yönetilen Diskler, azure iaas vm'leri için depolama hesabı yönetimini sizin için işleyerek disk yönetimini basitleştiren bir özelliktir. Daha fazla bilgi için [Yönetilen Diskler'e genel bakış](../articles/virtual-machines/windows/managed-disks-overview.md)bilgisine bakın.

**Mevcut bir VHD'den 80 GB'lık standart yönetilen bir disk oluşturursam, bu bana ne kadara mal olur?**

80 GB VHD'den oluşturulan standart yönetilen disk, s10 diski olan bir sonraki kullanılabilir standart disk boyutu olarak kabul edilir. S10 disk fiyatlandırmasına göre ücretlendirilirsiniz. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**Standart yönetilen diskler için işlem maliyeti var mı?**

Evet. Her işlem için ücretlendirilirsiniz. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**Standart yönetilen bir disk için, diskteki verilerin gerçek boyutu veya diskin sağlanan kapasitesi için ücretlendirilir miyim?**

Diskin sağlanan kapasitesine göre ücretlendirilirsiniz. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**Premium yönetilen disklerin fiyatlandırmasının yönetilmeyen disklerden farkı nedir?**

Premium yönetilen disklerin fiyatlandırması, yönetilmeyen premium disklerle aynıdır.

**Yönetilen disklerimin depolama hesabı türünü (Standart veya Premium) değiştirebilir miyim?**

Evet. Azure portalını, PowerShell'i veya Azure CLI'yi kullanarak yönetilen disklerinizin depolama hesabı türünü değiştirebilirsiniz.

**Farklı bir aboneye sahip yönetilen bir disk oluşturmak için Azure depolama hesabındaki bir VHD dosyasını kullanabilir miyim?**

Evet.

**Farklı bir bölgede yönetilen bir disk oluşturmak için Azure depolama hesabındaki bir VHD dosyasını kullanabilir miyim?**

Hayır.

**Yönetilen diskleri kullanan müşteriler için ölçek sınırlamaları var mı?**

Yönetilen Diskler, depolama hesaplarıyla ilişkili sınırları ortadan kaldırır. Ancak, en büyük sınır, bir abonelik için bölge başına ve disk türü başına 50.000 yönetilen disktir.

**Kullanılabilirlik kümesindeki VM'ler yönetilen ve yönetilmeyen disklerin bir birleşiminden oluşabilir mi?**

Hayır. Kullanılabilirlik kümesindeki VM'ler yönetilen tüm diskleri veya tüm yönetilmeyen diskleri kullanmalıdır. Kullanılabilirlik kümesi oluşturduğunuzda, hangi tür diskleri kullanmak istediğinizi seçebilirsiniz.

**Yönetilen Diskler Azure portalındaki varsayılan seçenek midir?**

Evet.

**Boş yönetilen bir disk oluşturabilir miyim?**

Evet. Boş bir disk oluşturabilirsiniz. Yönetilen bir disk, örneğin vm'ye takmadan vm'den bağımsız olarak oluşturulabilir.

**Yönetilen Diskler kullanan bir kullanılabilirlik kümesi için desteklenen hata etki alanı sayısı nedir?**

Yönetilen Diskler kullanan kullanılabilirlik kümesinin bulunduğu bölgeye bağlı olarak, desteklenen hata etki alanı sayısı 2 veya 3'tur.

**Tanılama için standart depolama hesabı nasıl ayarlanır?**

VM tanılama için özel bir depolama hesabı açtınız.

**Yönetilen Diskler için ne tür Rol Tabanlı Erişim Denetimi desteği kullanılabilir?**

Yönetilen Diskler üç temel varsayılan rolü destekler:

* Sahibi: Erişim de dahil olmak üzere her şeyi yönetebilirsiniz
* Katkıda bulunan: Erişim dışında her şeyi yönetebilir
* Okuyucu: Her şeyi görüntüleyebilir, ancak değişiklik yapamaz

**Yönetilen bir diski özel bir depolama hesabına kopyalamanın veya dışa aktarmamın bir yolu var mı?**

Yönetilen disk için salt okunur paylaşılan erişim imzası (SAS) URI oluşturabilir ve içeriği özel bir depolama hesabına veya şirket içi depolama alanına kopyalamak için kullanabilirsiniz. SAS URI'yi Azure portalını, Azure PowerShell'i, Azure CLI'yi veya [AzCopy'yi](../articles/storage/common/storage-use-azcopy.md) kullanarak kullanabilirsiniz

**Yönetilen diskimin bir kopyasını oluşturabilir miyim?**

Müşteriler, yönetilen disklerinin anlık görüntüsünü alabilir ve ardından anlık görüntüden başka bir yönetilen disk oluşturmak için kullanabilir.

**Yönetilmeyen diskler hala destekleniyor mu?**

Evet, hem yönetilmeyen hem de yönetilen diskler desteklenir. Yönetilen diskleri yeni iş yükleri için kullanmanızı ve geçerli iş yüklerinizi yönetilen disklere geçirmenizi öneririz.

**Aynı VM'de yönetilmeyen ve yönetilen diskleri birlikte bulabilir miyim?**

Hayır.

**128 GB'lık bir disk oluşturup boyutu 130'a (GiB) çıkarsam, bir sonraki disk boyutu (256 GiB) için ücretlendirilir miyim?**

Evet.

**Yerel olarak yedekli depolama, coğrafi yedekli depolama ve bölge yedekli depolama yönetilen diskler oluşturabilir miyim?**

Azure Yönetilen Diskler şu anda yalnızca yerel olarak yedekli depolama yı yönetilen diskleri destekler.

**Yönetilen disklerimi küçültebilir veya küçültebilir miyim?**

Hayır. Bu özellik şu anda desteklenmez.

**Diskimin kira kontratını kırabilir miyim?**

Hayır. Disk kullanılırken yanlışlıkla silinmesini önlemek için bir kiralama mevcut olduğundan, bu şu anda desteklenmez.

**Özel bir (Sistem Hazırlama aracı kullanılarak oluşturulmadı veya genelleştirilmiş) işletim sistemi diski VM sağlamak için kullanıldığında bilgisayar adı özelliğini değiştirebilir miyim?**

Hayır. Bilgisayar adı özelliğini güncelleştiremezsiniz. Yeni VM, işletim sistemi diskini oluşturmak için kullanılan ana VM'den devralır. 

**Yönetilen disklere sahip VM'ler oluşturmak için örnek Azure Kaynak Yöneticisi şablonlarını nerede bulabilirim?**
* [Yönetilen Diskler kullanan şablonlar listesi](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

**Bir blob bir disk oluştururken, bu kaynak blob ile herhangi bir sürekli varolan ilişki var mı?**

Hayır, yeni disk oluşturulduğunda o zaman bu lekenin tam bağımsız bir kopyasıdır ve ikisi arasında bağlantı yoktur. İsterseniz, diski oluşturduktan sonra, kaynak blob yeni oluşturulan diskherhangi bir şekilde etkilemeden silinebilir.

**Yönetilen veya yönetilmeyen bir diskoluşturulduktan sonra yeniden adlandırabilir miyim?**

Yönetilen diskler için bunları yeniden adlandıramazsınız. Ancak, şu anda bir VHD veya VM'ye bağlı olmadığı sürece yönetilmeyen bir diski yeniden adlandırabilirsiniz.

**Azure Disk'te GPT bölümleme kullanabilir miyim?**

Nesil 1 görüntüler, işletim sistemi disklerinde değil, yalnızca veri disklerinde GPT bölümleme kullanabilir. İşletim sistemi diskleri MBR bölüm stilini kullanmalıdır.

[Nesil 2 görüntüleri,](https://docs.microsoft.com/azure/virtual-machines/linux/generation-2) işletim sistemi diskinin yanı sıra veri disklerinde GPT bölümleme sini de kullanabilir.

**Hangi disk türleri anlık görüntüleri destekler?**

Premium SSD, standart SSD ve standart HDD destek anlık görüntüleri. Bu üç disk türü için anlık görüntüler tüm disk boyutları için desteklenir (32 TiB boyutuna kadar diskler dahil). Ultra diskler anlık görüntüleri desteklemez.

**Azure disk rezervasyonları nelerdir?**
Disk rezervasyonu, toplam maliyetinizi azaltarak bir yıllık disk depolama işlemini önceden satın alma seçeneğidir. Azure disk rezervasyonları ile ilgili ayrıntılar için konuyla ilgili makalemize bakın: [Azure Disk'e rezervasyon indiriminizin nasıl uygulandığını anlayın.](../articles/cost-management-billing/reservations/understand-disk-reservations.md)

**Azure disk rezervasyonu hangi seçenekleri sunar?**    
Azure disk rezervasyonu, belirtilen SK'lerde P30'dan (1 TiB) P80'e (32 TiB) kadar premium SSD'leri bir yıllık bir dönem için satın alma seçeneği sunar. Bir disk rezervasyonu satın almak için gereken en az disk miktarı üzerinde herhangi bir sınırlama yoktur. Ayrıca, tek bir peşin ödeme veya aylık ödeme ile ödemeyi seçebilirsiniz. Premium SSD Yönetilen Diskler için ek işlem maliyeti uygulanmaz.    

Rezervasyonlar kapasite de değil, disk şeklinde yapılır. Başka bir deyişle, bir P80 (32 TiB) disk ayırdığınızda, tek bir P80 diski alırsanız, bu özel rezervasyonu iki küçük P70 (16 TiB) diske bölemezsiniz. Elbette, iki ayrı P70 (16 TiB) diskde dahil olmak üzere istediğiniz kadar veya az sayıda disk rezerve edebilirsiniz.

**Azure disk rezervasyonu nasıl uygulanır?**    
Diskler rezervasyon ayrılmış sanal makine (VM) örnekleri benzer bir model izler. Fark, bir disk ayırma farklı SNU'lara uygulanamaz ken, bir VM örneği olabilir. VM örnekleri hakkında daha fazla bilgi için [Azure Ayrılmış VM Örnekleri ile maliyetleri](../articles/virtual-machines/linux/prepay-reserved-vm-instances.md) kaydedin.     

**Azure diskleri tarafından satın alınan veri depolama mı kullanabilirim?**    
Azure diskleri rezervasyonu belirli bir bölge ve SKU (Doğu ABD 2'deki P30 gibi) için satın alınır ve bu nedenle bu yapılar dışında kullanılamaz. Disk depolama gereksinimleriniz için diğer bölgelerdeki veya SK'lerde her zaman ek bir Azure Diskleri Rezervasyonu satın alabilirsiniz.    

**Azure diskleri rezervasyonumun süresi dolduğunda ne olur?**    
Son kullanma tarihinden 30 gün önce ve son kullanma tarihinden itibaren e-posta bildirimleri alırsınız. Rezervasyonun süresi dolduğunda, dağıtılan diskler çalışmaya devam eder ve en son [kullandıkça öde fiyatlarıyla](https://azure.microsoft.com/pricing/details/managed-disks/)faturalandırılır.

### <a name="azure-shared-disks"></a>Azure paylaşılan diskler

**Paylaşılan diskler özelliği yönetilmeyen diskler veya sayfa lekeleri için destekleniyor mu?**

Hayır, yalnızca premium SSD yönetilen diskler için desteklenir.

**Paylaşılan diskleri destekleyen bölgeler nelerdir?**

Şu anda sadece Batı Orta ABD.

**Paylaşılan diskler işletim sistemi diski olarak kullanılabilir mi?**

Hayır, paylaşılan diskler yalnızca veri diskleri için desteklenir.

**Hangi disk boyutları paylaşılan diskleri destekler?**

Yalnızca P15 veya daha fazla destek paylaşılan diskler olan premium SSD'ler.

**Varolan bir premium SSD'm varsa, üzerinde paylaşılan diskleri etkinleştirebilir miyim?**

API sürümü 2019-07-01 veya daha yüksek ile oluşturulan tüm yönetilen diskler paylaşılan diskleri etkinleştirebilir. Bunu yapmak için, bağlı olduğu tüm VM'lerden diski sökmeniz gerekir. Ardından, diskteki `maxShares` özelliği edin.

**Paylaşılan modda artık bir disk kullanmak istemiyorsam, diski nasıl devre dışı asıyım?**

Bağlı olduğu tüm VM'lerden diski sökün. Ardından diskteki maxShare özelliğini 1'e kadar edin.

**Paylaşılan bir diski yeniden boyutlandırabilir misiniz?**

Evet.

**Paylaşılan disklerin etkin olduğu bir diskte yazma hızlandırıcısını etkinleştirebilir miyim?**

Hayır.

**Paylaşılan disk etkinleştirilmiş bir disk için ana bilgisayar önbelleğe alma özelliğini etkinleştirebilir miyim?**

Desteklenen tek ana bilgisayar önbelleğe alma seçeneği 'Yok'dur.

## <a name="ultra-disks"></a>Ultra diskler

**Ultra disk elimi ne lere ayarlamalıyım?**
Disk iş inizi neye ayarladığınızdan emin değilseniz, 16 KiB'lik bir IO boyutu varsayarak başlamanızı ve uygulamanızı izlerken performansı oradan ayarlamanızı öneririz. Formül: MBps = # IOPS * 16 / 1000 throughput olduğunu.

**Ben 40000 IOPS benim disk yapılandırılan ama sadece 12800 IOPS görüyorum, neden diskin performansını göremiyorum?**
Disk gazına ek olarak, VM düzeyinde uygulanan bir IO gaz vardır. Lütfen kullanmakta olduğunuz VM boyutunun disklerinizde yapılandırılan düzeyleri desteklediğinden emin olun. VM'niz tarafından uygulanan IO limitleri ile ilgili ayrıntılar için [Azure'daki Windows sanal makineleri için Boyutlar'a](../articles/virtual-machines/windows/sizes.md)bakın.

**Ultra diskli önbelleğe alma düzeylerini kullanabilir miyim?**
Hayır, ultra diskler diğer disk türlerinde desteklenen farklı önbelleğe alma yöntemlerini desteklemez. Disk önbelleğe alma'yı Yok olarak ayarlayın.

**Mevcut VM'me ultra disk takabilir miyim?**
Belki, VM ultra diskleri destekleyen bir bölge ve kullanılabilirlik bölgesi çifti olmalıdır. Ayrıntılar için [ultra disklerle başlamaya](../articles/virtual-machines/windows/disks-enable-ultra-ssd.md) bakın.

**VM'im için işletim sistemi diski olarak ultra disk kullanabilir miyim?**
Hayır, ultra Diskler yalnızca veri diski olarak desteklenir ve yalnızca 4K yerel disk olarak desteklenir.

**Varolan bir diski ultra diske dönüştürebilir miyim?**
Hayır, ancak verileri varolan bir diskten ultra diske geçirebilirsiniz. Varolan bir diski ultra diske geçirmek için, her iki diski de aynı VM'ye takın ve diskin verilerini bir diskten diğerine kopyalayın veya veri geçişi için üçüncü taraf çözümden yararlanın.

**Ultra diskler için anlık görüntü oluşturabilir miyim?**
Hayır, anlık görüntüler henüz kullanılamıyor.

**Azure Yedekleme ultra diskler için kullanılabilir mi?**
Hayır, Azure Yedekleme desteği henüz kullanılamıyor.

**Kullanılabilirlik kümesinde çalışan bir VM'ye ultra disk ekleyebilir miyim?**
Hayır, bu henüz desteklenmedi.

**Ultra diskler kullanarak VM'ler için Azure Site Kurtarma'yı etkinleştirebilir miyim?**
Hayır, Azure Site Kurtarma henüz ultra diskler için desteklenmedi.

## <a name="uploading-to-a-managed-disk"></a>Yönetilen bir diske yükleme

**Varolan yönetilen bir diske veri yükleyebilir miyim?**

Hayır, yükleme yalnızca **ReadyToUpload** durumuna sahip yeni bir boş disk oluşturulması sırasında kullanılabilir.

**Yönetilen bir diske nasıl yüklenirim?**

[CreationData'nın](https://docs.microsoft.com/rest/api/compute/disks/createorupdate#creationdata) [createOption](https://docs.microsoft.com/rest/api/compute/disks/createorupdate#diskcreateoption) özelliği "Upload" olarak ayarlanmış yönetilen bir disk oluşturun, ardından ona veri yükleyebilirsiniz.

**Yükleme durumundayken VM'ye disk ekleyebilir miyim?**

Hayır.

**Yükleme durumundaki manged diskin anlık görüntüsünü alabilir miyim?**

Hayır.

## <a name="standard-ssd-disks"></a>Standart SSD diskler

**Azure Standart SSD diskleri nedir?**
Standart SSD diskler, daha düşük IOPS düzeylerinde tutarlı performans gerektiren iş yükleri için uygun maliyetli depolama alanı olarak optimize edilen katı hal ortamı tarafından desteklenen standart disklerdir.

<a id="standard-ssds-azure-regions"></a>**Şu anda Standart SSD diskler için desteklenen bölgeler nelerdir?**
Artık tüm Azure bölgeleri Standart SSD diskleri destekliyor.

**Azure Yedekleme Standart SSD'leri kullanırken kullanılabilir mi?**
Evet, Azure Yedekleme artık kullanılabilir.

**Standart SSD diskleri nasıl oluştururum?**
Azure Kaynak Yöneticisi şablonları, SDK, PowerShell veya CLI'yi kullanarak Standart SSD diskleri oluşturabilirsiniz. Standart SSD Diskleri oluşturmak için Kaynak Yöneticisi şablonunda gerekli parametreler aşağıda verilmiştir:

* Microsoft.Compute için *apiVersion* olarak `2018-04-01` ayarlanmalıdır (veya daha sonra)
* *managedDisk.storageAccountType'ı*`StandardSSD_LRS`

Aşağıdaki örnekte, Standart SSD Diskler kullanan bir VM için *properties.storageProfile.osDisk* bölümü gösterilmektedir:

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

Şablonlu bir Standart SSD diskinin nasıl oluşturulaca açık tam bir şablon örneği için [bkz.](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)

**Varolan disklerimi Standart SSD'ye dönüştürebilir miyim?**
Evet, bunu yapabilirsiniz. Azure [yönetilen diskdepolamayı standarttan premium'a dönüştürmeye](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage) bakın ve Yönetilen Diskleri dönüştürmeyle ilgili genel yönergeler için tam tersi. Ayrıca, disk türünü Standart SSD'ye güncelleştirmek için aşağıdaki değeri kullanın.
-Hesap Tipi StandardSSD_LRS

**HDD yerine Standart SSD diskler kullanmanın faydası nedir?**
Standart SSD diskler, HDD disklere kıyasla daha iyi gecikme süresi, tutarlılık, kullanılabilirlik ve güvenilirlik sunar. Uygulama iş yükleri, bu nedenle Standart SSD'de çok daha sorunsuz çalışır. Not: Premium SSD diskler, IO yoğun üretim iş yüklerinin çoğu için önerilen çözümdür.

**Standart SSD'leri Yönetilmeyen Diskler olarak kullanabilir miyim?**
Hayır, Standart SSD'ler diskleri yalnızca Yönetilen Diskler olarak kullanılabilir.

**Standart SSD Diskler "tek örnek VM SLA"yı destekliyor mu?**
Hayır, Standart SSD'lerde tek bir örnek VM SLA yoktur. Tek örnek VM SLA için Premium SSD diskleri kullanın.

## <a name="migrate-to-managed-disks"></a>Yönetilen Disklere geçme

**Geçişin Yönetilen Diskler performansı üzerinde herhangi bir etkisi var mı?**

Geçiş, Diskin bir Depolama konumundan diğerine hareketini içerir. Bu, disklerde veri miktarına bağlı olarak genellikle 24 Saat'ten daha az, tamamlanması birkaç saat sürebilecek arka plandaki veri kopyası aracılığıyla düzenlenmiştir. Bu süre zarfında uygulamanız normalden daha yüksek okuma gecikmesi yaşayabilir, çünkü bazı okumalar orijinal konuma yönlendirilebilir ve tamamlanması daha uzun sürebilir. Bu dönemde yazma gecikmesi üzerinde hiçbir etkisi yoktur.  

**Yönetilen Disklere geçişten önce/sonra önceden varolan bir Azure Yedekleme hizmeti yapılandırmasında hangi değişiklikler gereklidir?**

Değişiklik gerekmez.

**Geçiş çalışmaya devam etmeden önce Azure Yedekleme hizmeti aracılığıyla oluşturulan VM yedeklerim çalışır mı?**

Evet, yedeklemeler sorunsuz çalışır.

**Yönetilen Disklere geçişten önce/sonra önceden varolan bir Azure Diskşifreleme yapılandırmasında hangi değişiklikler gereklidir?**

Değişiklik gerekmez.

**Varolan sanal makine ölçeğinin otomatik geçişi yönetilmeyen disklerden Yönetilen Disklere ayarlanır mı?**

Hayır. Yönetilen Diskler ile yönetilen diskler ile eski ölçek kümesinden görüntüyü kullanarak yeni bir ölçek kümesi oluşturabilirsiniz.

**Yönetilen Diskler'e geçmeden önce çekilen bir sayfa blob anlık görüntüsünden Yönetilen Disk oluşturabilir miyim?**

Hayır. Bir sayfa blob anlık görüntüsünü bir sayfa blob olarak dışa aktarabilir ve ardından dışa aktarılan sayfa blob'undan yönetilen disk oluşturabilirsiniz.

**Azure Site Kurtarma tarafından korunan şirket içi makinelerim üzerinde Yönetilen Diskler ile vm'de başarısız olabilir miyim?**

Evet, Yönetilen Diskler ile bir VM'ye başarısız olmayı seçebilirsiniz.

**Azure Site Kurtarma yoluyla Azure çoğaltma yoluyla korunan Azure VM'leri üzerindeki geçişin herhangi bir etkisi var mı?**

Hayır. Yönetilen Diskler'e sahip VM'ler için Azure Site Kurtarma Azure'dan Azure'a koruma mevcuttur.

**Yönetilen diskler için önceden şifrelenmiş veya depolama hesaplarında bulunan yönetilmeyen disklerle VM'leri geçirtebilir miyim?**

Evet

## <a name="managed-disks-and-storage-service-encryption"></a>Yönetilen Diskler ve Depolama Hizmeti Şifrelemesi

**Yönetilen bir disk oluşturduğumda Azure Depolama Hizmeti Şifrelemesi varsayılan olarak etkinleştirilir mi?**

Evet.

**Önyükleme birimi yönetilen bir diskte varsayılan olarak şifrelenir mi?**

Evet. Varsayılan olarak, işletim sistemi diski de dahil olmak üzere yönetilen tüm diskler şifrelenir.

**Şifreleme anahtarlarını kim yönetiyor?**

Şifreleme anahtarlarını Microsoft yönetir.

**Yönetilen disklerim için Depolama Hizmeti Şifrelemesi'ni devre dışı kılabilir miyim?**

Hayır.

**Depolama Hizmeti Şifrelemesi yalnızca belirli bölgelerde mi kullanılabilir?**

Hayır. Yönetilen Disklerin kullanılabildiği tüm bölgelerde kullanılabilir. Yönetilen Diskler tüm ortak bölgelerde ve Almanya'da kullanılabilir. Ancak Çin'de de kullanılabilir, ancak müşteri yönetilen anahtarlar için değil, yalnızca Microsoft yönetilen anahtarlar için kullanılabilir.

**Yönetilen diskimin şifrelenip şifrelenmedığını nasıl öğrenebilirim?**

Azure portalı, Azure CLI ve PowerShell'den yönetilen bir diskin ne zaman oluşturulduğunu öğrenebilirsiniz. Saat 9 Haziran 2017'den sonraysa, diskiniz şifrelenir.

**10 Haziran 2017'den önce oluşturulan varolan disklerimi nasıl şifreleyebilirim?**

10 Haziran 2017 itibariyle, mevcut yönetilen disklere yazılan yeni veriler otomatik olarak şifrelenir. Ayrıca varolan verileri şifrelemeyi planlıyoruz ve şifreleme arka planda eşzamanlı olarak gerçekleşecektir. Varolan verileri şimdi şifrelemeniz gerekiyorsa, diskinizin bir kopyasını oluşturun. Yeni diskler şifrelenir.

* [Azure CLI'yi kullanarak yönetilen diskleri kopyalama](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [Yönetilen diskleri PowerShell kullanarak kopyalama](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

**Yönetilen anlık görüntüler ve görüntüler şifrelenir mi?**

Evet. 9 Haziran 2017'den sonra oluşturulan tüm yönetilen anlık görüntüler ve görüntüler otomatik olarak şifrelenir. 

**Yönetilen diskler için önceden şifrelenmiş veya depolama hesaplarında bulunan yönetilmeyen disklere sahip VM'leri dönüştürebilir miyim?**

Evet

**Yönetilen bir diskten veya anlık görüntüden dışa aktarılan Bir VHD de şifrelenir mi?**

Hayır. Ancak, şifreli yönetilen bir diskten veya anlık görüntüden şifreli bir depolama hesabına vhd dışa aktarırsanız, bu hesap şifrelenir. 

## <a name="premium-disks-managed-and-unmanaged"></a>Premium diskler: Yönetilen ve yönetilmeyen

**Bir VM, DSv2 gibi Premium SSD diskleri destekleyen bir boyut serisi kullanıyorsa, hem premium hem de standart veri diskleri ekleyebilir miyim?** 

Evet.

**D, Dv2, G veya F serisi gibi Premium SSD diskleri desteklemeyen bir boyut serisine hem premium hem de standart veri diskleri ekleyebilir miyim?**

Hayır. Premium SSD diskleri destekleyen bir boyut serisi kullanmayan VM'lere yalnızca standart veri diskleri ekleyebilirsiniz.

**80 GB olan mevcut bir VHD'den bir premium veri diski oluşturursam, bu ne kadara mal olur?**

80 GB VHD'den oluşturulan birinci sınıf veri diski, p10 diski olan bir sonraki kullanılabilir premium disk boyutu olarak kabul edilir. P10 disk fiyatlandırmasına göre ücretlendirilirsiniz.

**Premium SSD diskleri kullanmak için işlem maliyetleri var mı?**

IOPS ve iş maliyeti belirli limitler ile sağlanan her disk boyutu için sabit bir maliyet vardır. Diğer maliyetler, varsa giden bant genişliği ve anlık görüntü kapasitesidir. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage).

**IOPS ve disk önbelleğinden alabildiğim iş için sınırlar nelerdir?**

Bir DS serisi için önbellek ve yerel SSD için birleştirilmiş sınırlar çekirdek başına 4.000 IOPS ve çekirdek başına saniyede 33 MiB'dir. GS serisi çekirdek başına 5.000 IOPS ve çekirdek başına saniyede 50 MiB sunuyor.

**Yerel SSD Yönetilen Diskler VM için destekleniyor mu?**

Yerel SSD, Yönetilen Diskler VM ile birlikte verilen geçici depolama alanıdır. Bu geçici depolama için ekstra bir ücret yoktur. Azure Blob depolama alanında kalıcı olmadığı için uygulama verilerinizi depolamak için bu yerel SSD'yi kullanmamanızı öneririz.

**Premium disklerde TRIM kullanımının herhangi bir yan etkisi var mı?**

Azure disklerde TRIM kullanımının premium veya standart disklerde kullanılmasının hiçbir dezavantajı yoktur.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Yeni disk boyutları: Yönetilen ve yönetilmeyen

**Hangi bölgeler, geçerli premium SSD disk boyutu için patlama yeteneğini destekler?**

Patlama özelliği şu anda Azure West Central US'ta desteklenmiştir.

**4/8/16 GİB Yönetilen Disk boyutları (P1/P2/P3, E1/E2/E3) hangi bölgelerde desteklenir?**

Bu yeni disk boyutları şu anda Azure West Central US'ta destekleniyor.

**P1/P2/P3 disk boyutları yönetilmeyen diskler veya sayfa lekeleri için destekleniyor mu?**

Hayır, yalnızca premium SSD yönetilen disklerde desteklenir. 

**E1/E2/E3 disk boyutları yönetilmeyen diskler veya sayfa lekeleri için destekleniyor mu?**

Hayır, herhangi bir boyuttaki standart SSD yönetilen diskler yönetilmeyen disklerle veya sayfa lekeleriyle kullanılamaz.

**İşletim sistemi ve veri diskleri için desteklenen en büyük Yönetilen disk boyutu nedir?**

Azure'un bir işletim sistemi diski için desteklediği bölüm türü, ana önyükleme kaydıdır (MBR). MBR biçimi 2 TiB'e kadar bir disk boyutunu destekler. Azure'un bir işletim sistemi diski için desteklediği en büyük boyut 2 TiB'dir. Azure, yönetilen veri diskleri için 32 TiB'ye kadar destekler.

**İşletim sistemi ve veri diskleri için desteklenen en büyük Yönetilmeyen Disk boyutu nedir?**

Azure'un bir işletim sistemi diski için desteklediği bölüm türü, ana önyükleme kaydıdır (MBR). MBR biçimi 2 TiB'e kadar bir disk boyutunu destekler. Azure'un yönetilmeyen bir işletim sistemi için desteklediği en büyük boyut 2 TiB'dir. Azure, yönetilmeyen veriler için en fazla 4 TiB'yi destekler.

**Desteklenen en büyük sayfa blob boyutu nedir?**

Azure'un desteklediği en büyük sayfa blob boyutu 8 TiB 'dir (8.191 GiB). Veri veya işletim sistemi diskleri olarak bir VM'e eklendiğinde maksimum sayfa blob boyutu 4 TiB 'dir (4,095 GiB).

**1 TiB'den büyük diskler oluşturmak, eklemek, yeniden boyutlandırmak ve yüklemek için Azure araçlarının yeni bir sürümünü kullanmam gerekir mi?**

1 TiB'den büyük diskler oluşturmak, eklemek veya yeniden boyutlandırmak için varolan Azure araçlarınızı yükseltmeniz gerekmez. VHD dosyanızı şirket içinde doğrudan sayfa blob'u veya yönetilmeyen disk olarak Azure'a yüklemek için, aşağıda listelenen en son araç kümelerini kullanmanız gerekir. Biz sadece 8 TiB kadar VHD yüklemeleri destekler.

|Azure araçları      | Desteklenen sürümler                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Sürüm numarası 4.1.0: Haziran 2017 sürümü veya sonrası|
|Azure CLI v1     | Sürüm numarası 0.10.13: Mayıs 2017 sürümü veya sonrası|
|Azure CLI v2     | Sürüm numarası 2.0.12: Temmuz 2017 sürümü veya sonrası|
|AzCopy              | Sürüm numarası 6.1.0: Haziran 2017 sürümü veya sonrası|

**P4 ve P6 disk boyutları yönetilmeyen diskler veya sayfa lekeleri için destekleniyor mu?**

P4 (32 GiB) ve P6 (64 GiB) disk boyutları, yönetilmeyen diskler ve sayfa lekeleri için varsayılan disk katmanları olarak desteklenmez. Diskinizin bu katmanlara eşlenebilmek için [Blob Tier'i](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) açıkça P4 ve P6 olarak ayarlamanız gerekir. Disk boyutu veya içerik uzunluğu 32 GiB'den az veya Blob Tier'i ayarlamadan 32 GiB ile 64 GiB arasında olan yönetilmeyen bir disk veya sayfa blob'u dağıtırsanız, 500 IOPS ve 100 MiB/s ve eşlenen fiyatlandırma katmanıyla P10'a inmeye devam edersiniz.

**Mevcut premium yönetilen diskim küçük disk etkinleştirilmeden önce 64 GiB'den az oluşturulmuşsa (15 Haziran 2017 civarında), nasıl faturalandırılır?**

64 GiB'den küçük olan mevcut küçük premium diskler P10 fiyatlandırma katmanına göre faturalandırılamaya devam eder.

**64 GiB'den küçük premium disklerin disk katmanını P10'dan P4 veya P6'ya nasıl değiştirebilirim?**

Küçük disklerinizin anlık görüntüsünü alabilir ve ardından fiyatlandırma katmanını sağlanan boyuta bağlı olarak otomatik olarak P4 veya P6'ya geçirmek için bir disk oluşturabilirsiniz.

**Mevcut Yönetilen Diskleri 4 tebibayttan (TiB) daha az boyutlardan 32 TiB'ye kadar yeni tanıtılan yeni disk boyutlarına yeniden boyutlandırabilir misiniz?**

Evet.

**Azure Yedekleme ve Azure Site Kurtarma hizmeti tarafından desteklenen en büyük disk boyutları nelerdir?**

Azure Yedekleme tarafından desteklenen en büyük disk boyutu 32 TiB 'dir (şifreli diskler için 4 TiB). Azure Site Kurtarma tarafından desteklenen en büyük disk boyutu 8 TiB'dir. Azure Site Kurtarma'da 32 TiB'ye kadar olan büyük diskler için destek henüz kullanılamıyor.

**Optimize edilmiş disk IOPS ve Bant Genişliği elde etmek için Standart SSD ve Standart HDD diskler için daha büyük disk boyutları (>4 TiB) için önerilen VM boyutları nelerdir?**

Standart SSD ve Standart HDD büyük disk boyutlarının (>4 TiB) 500 IOPS ve 60 MiB/s'den daha fazla disk elde etmek için, Performansınızı optimize etmek için aşağıdaki VM boyutlarından birinden yeni bir VM dağıtmanızı öneririz: B serisi, DSv2 serisi, Dsv3 Serisi, ESv3 Serisi, Fs serisi, Fsv2 serisi, M serisi, GS serisi, NCv2 serisi, NCv3 serisi veya Ls serisi VM'ler. Yukarıda önerilen boyutları kullanmayan varolan VM'lere veya VM'lere büyük diskler takılmak daha düşük performansla karşılaşabilir.

**GA'da daha yüksek IOPS & bant genişliğini elde etmek için daha büyük disk boyutları önizlemesi sırasında dağıtılan disklerimi (>4 TiB) nasıl yükseltebilirim?**

Diskin bağlı olduğu VM'yi durdurup başlatabilir veya diskinizi ayırıp yeniden takabilirsiniz. GA'da hem premium SSD'ler hem de standart SSD'ler için daha büyük disk boyutlarının performans hedefleri artırıldı.

**8 TiB, 16 TiB ve 32 TiB'nin yönetilen disk boyutları hangi bölgelerde desteklenir?**

8 TiB, 16 TiB ve 32 TiB disk SUS'u global Azure, Microsoft Azure Hükümeti ve Azure China 21Vianet kapsamındaki tüm bölgelerde desteklenir.

**Tüm disk boyutlarında Ana Bilgisayar Önbelleğe Alma'yı etkinleştirme yi destekliyor muyum?**

ReadOnly'in Host Caching'ini destekliyoruz ve 4 TiB'den küçük disk boyutlarında Okuma/Yazma'yı destekliyoruz. 4 TiB'den fazla disk boyutları için, Hiçbiri dışında önbelleğe alma seçeneğini desteklemeyiz. VM'ye önbelleğe alınan verilerle daha iyi performans artışı gözlemleyebileceğiniz daha küçük disk boyutları için önbelleğe almanızı öneririz.

## <a name="what-if-my-question-isnt-answered-here"></a>Ya sorum burada cevaplandırılmazsa?

Sorunuz burada listelenmemişse, bize bildirin ve bir yanıt bulmanıza yardımcı olalım. Bu makalenin sonunda yorum bir soru gönderebilirsiniz. Azure Depolama ekibi ve diğer topluluk üyeleriyle bu makale hakkında etkileşimde bulunmak için MSDN [Azure Depolama forumundan](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)yararlanın.

Özellikler istemek için isteklerinizi ve fikirlerinizi [Azure Depolama geri bildirim forumuna](https://feedback.azure.com/forums/217298-storage)gönderin.
