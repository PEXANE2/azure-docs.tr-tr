---
title: Azure Marketi 'nde sanal makine teklifi için planlar oluşturma
description: Azure Marketi 'nde bir sanal makine teklifi için planlar oluşturmayı öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: mingshen-ms
ms.author: mingshen
ms.date: 10/19/2020
ms.openlocfilehash: bc5e98484560fcc15e0ea3e289069c84687f158c
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93040518"
---
# <a name="how-to-create-plans-for-a-virtual-machine-offer"></a>Bir sanal makine teklifi için plan oluşturma

Iş Ortağı Merkezi 'nde aynı teklif dahilinde çeşitli plan seçenekleri sağlayabilirsiniz. Bir teklif en az bir plan (daha önce SKU olarak adlandırılır) gerektirir; Bu, Monaya getirme seyirci kitlesi, Azure bölgesi, özellikleri veya VM görüntülerine göre farklılık gösterebilir.

Her teklif için en fazla 100 plan oluşturabilirsiniz: bunların en fazla 45 ' e kadar özel olabilir. [Microsoft ticari Market 'Teki özel tekliflerle](private-offers.md)ilgili özel planlar hakkında daha fazla bilgi edinin.

Planlarınızı oluşturduktan sonra, görüntülenecek **plana genel bakış** sekmesini seçin:

- Plan adları
- Lisans modelleri
- Hedef kitle (genel veya özel)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

**Plana genel bakış** bölmesinde bulunan eylemlere, planınızın geçerli durumuna bağlı olarak değişiklik gösterir.

- Plan durumu bir taslak ise, **taslağı Sil** ' i seçin.
- Plan durumu canlı olarak yayınlanıyorsa, **satışı durdur** veya **özel izleyiciyi Eşitle** seçeneğini belirleyin.

## <a name="create-a-new-plan"></a>Yeni bir plan oluşturun

En üstte **Yeni plan oluştur** ' u seçin. **Yeni plan** iletişim kutusu görüntülenir.

**Plan kimliği** kutusunda, bu teklifte her plan için benzersiz BIR plan kimliği oluşturun. Bu KIMLIK, ürün web adresinde müşterilere görünür olacaktır. Yalnızca küçük harf ve rakam, tire veya alt çizgi ve en fazla 50 karakter kullanın.

> [!NOTE]
> Plan KIMLIĞI, **Oluştur** ' u seçtikten sonra değiştirilemez.

**Plan adı** kutusuna bu plan için bir ad girin. Müşteriler teklifiniz dahilinde hangi planın seçeceğinize karar verirken bu adı görür. Planlar arasındaki farkları açıkça gösteren benzersiz bir ad oluşturun. Örneğin, *Kullandıkça Öde* , *KLG* , *Gelişmiş* ve *Kurumsal* planlar ile **Windows Server** girebilirsiniz.

**Oluştur** ’u seçin.

## <a name="plan-setup"></a>Planı ayarla

Plan türü için üst düzey yapılandırmayı ayarlayın, başka bir plandan bir teknik yapılandırmayı yeniden kullanıp kullanmayacağını belirtin ve planın kullanılabilir olması gereken Azure bölgelerini belirleyin. Burada yaptığınız seçimler, aynı plan için diğer bölmelerde hangi alanların görüntülendiğini belirlemektir.

### <a name="reuse-a-technical-configuration"></a>Teknik yapılandırmayı yeniden kullanma

Aynı türde birden fazla planınız varsa ve paketler aralarında aynıysa, **Bu planı başka bir plandaki teknik yapılandırmayı yeniden kullanır** ' ı seçebilirsiniz. Bu seçenek, bu teklif için aynı türdeki diğer planlardan birini seçmenizi sağlar ve teknik yapılandırmasını yeniden kullanmanıza olanak tanır.

> [!NOTE]
> Teknik yapılandırmayı başka bir plandan yeniden kullandığınızda, tüm **Teknik yapılandırma** sekmesi bu plandan kaybolur. Daha sonra yaptığınız tüm güncelleştirmeler dahil olmak üzere diğer plandaki teknik yapılandırma ayrıntıları, bu plan için de kullanılacaktır. Plan yayımlandıktan sonra bu ayar değiştirilemez.

### <a name="azure-regions"></a>Azure bölgeleri

Planınız en az bir Azure bölgesinde kullanılabilir duruma getirilmelidir.

Planınızı, ticari Market tümleştirmesi olan tüm Azure genel bölgelerindeki müşterilerin kullanımına sunmak için **Azure genel** seçeneğini belirleyin. Daha fazla bilgi için bkz. [coğrafi kullanılabilirlik ve para birimi desteği](marketplace-geo-availability-currencies.md).

Planınızı [Azure Kamu](../azure-government/documentation-government-welcome.md) bölgesinde kullanılabilir hale getirmek Için **Azure Kamu** seçeneğini belirleyin. Bu bölge, ABD Federal, eyalet, yerel veya üç aylık varlıklardan müşterilere denetimli erişim sağlar ve bunların yanı sıra onlara hizmeti sunmaya uygun olan iş ortakları için de çalışır. Yayımcı olarak tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan sorumludur. Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır.

[Azure Kamu](../azure-government/documentation-government-manage-marketplace-partners.md)'da yayımlamadan önce, belirli uç noktalar farklı olabileceğinden planınızı ortamda test edin ve doğrulayın. Planınızı ayarlamak ve test etmek için [Microsoft Azure Kamu deneme](https://azure.microsoft.com/global-infrastructure/government/request/) sayfasından bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayımlandıktan ve belirli bir Azure bölgesinde kullanılabilir olduktan sonra bu bölgeyi kaldıramazsınız.

### <a name="azure-government-certifications"></a>Azure Kamu sertifikaları

Bu seçenek yalnızca, önceki bölümde yer alan Azure bölgesi olarak **Azure Kamu** 'yu seçtiyseniz görünür.

Azure Kamu Hizmetleri, belirli kamu düzenlemelerine ve gereksinimlerine tabi olan verileri işler. Örneğin, Fedrampa, NıST 800,171 (DIB), ıTAR, ıRS 1075, DoD L4 ve CJıS. Bu programlara yönelik sertifikalarınıza yönelik olarak bir açıklama getirmek için, bunları tanımlayan 100 'e kadar bağlantı sağlayabilirsiniz. Bunlar, program üzerinde doğrudan listelemesine bağlantılar ya da kendi Web sitelerinizde uyumluluğun açıklamalarını bağlar olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="plan-listing"></a>Planı listeleme

Bu bölümde planın listeleme ayrıntılarını yapılandırırsınız. Bu bölme, aynı teklifteki diğer planlardan farklı olabilen belirli bilgileri görüntüler.

### <a name="plan-name"></a>Plan adı

Bu alan, oluşturduğunuz zaman planınızı verdiğiniz adla otomatik olarak doldurulur. Bu ad, Azure Marketi 'nde bu planın başlığı olarak görünür. 100 karakterle sınırlıdır.

### <a name="plan-summary"></a>Plan Özeti

Teklifinizi değil, planınızın kısa bir özetini sağlayın. Bu Özet 100 karakterle sınırlıdır.

### <a name="plan-description"></a>Plan açıklaması

Bu yazılım planının benzersiz olduğunu ve teklifinizdeki planlar arasındaki farkları açıkla. Teklifi değil yalnızca planı açıklama. Plan açıklaması en fazla 2.000 karakter içerebilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Bu bölmede şunları yapılandırırsınız:

- Bu planın kullanılabildiği pazarlar. Her plan en az bir [pazarda](marketplace-geo-availability-currencies.md)kullanılabilir olmalıdır.
- Saat başına fiyat.
- Planın herkese veya yalnızca belirli müşterilere (özel bir hedef kitle) görünür hale yapılıp yapılmayacağını belirtir.

### <a name="markets"></a>Pazar

Her plan en az bir pazarda kullanılabilir olmalıdır. Bu planın satın alma için kullanılabilir olması gereken her Pazar konumunun onay kutusunu seçin. (Bu pazarlardaki kullanıcılar teklifi ["plan kurulumu"](#plan-setup) bölümünde seçilen tüm Azure bölgelerine dağıtmaya devam edebilir.) **Havale edilen vergi** düğmesi, Microsoft remits Sales ve sizin adınıza vergi tarafından kullanılan ülkeyi/bölgeleri gösterir. Çin 'de yayımlama, *ücretsiz* veya *kendi lisansını getir* (KLG) planlarla sınırlıdır.

Planınız için ABD Doları (USD) para biriminde zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Yayımlamadan önce her bir pazar için fiyatı her zaman gözden geçirin. Değişikliklerinizi kaydettikten sonra **fiyatları dışarı aktar (xlsx)** seçeneğini belirleyerek fiyatlandırmanızı gözden geçirin.

Bir pazarı kaldırdığınızda, bu pazardan etkin dağıtımlar kullanan müşteriler Yeni dağıtımlar oluşturamaz veya mevcut dağıtımlarını ölçeklendiremez. Mevcut dağıtımlar etkilenmez.

### <a name="pricing"></a>Fiyatlandırma

**Lisans modeli** için, bu plan için fiyatlandırmayı yapılandırmak üzere **Kullanım tabanlı aylık faturalandırılan plan** ' ı seçin veya müşterilerin bu planı mevcut lisansıyla kullanmasına izin vermek için **kendi lisansını getir** ' i seçin.

Kullanım tabanlı aylık faturalandırılan bir plan için aşağıdaki üç fiyatlandırma girişi seçeneğinden birini kullanın:

- **Çekirdek başına** : USD cinsinden çekirdek başına fiyatlandırma sağlar. Microsoft, çekirdek boyutu başına fiyatlandırmayı hesaplar ve geçerli döviz ücretini kullanarak yerel para birimlerine dönüştürür.
- **Çekirdek boyutu başına** : USD cinsinden çekirdek boyutu başına fiyatlandırma sağlar. Microsoft, fiyatlandırmayı hesaplar ve geçerli döviz ücretini kullanarak yerel para birimlerine dönüştürür.
- **Pazar ve çekirdek boyutu başına** : tüm pazarlar için her bir çekirdek boyutu için fiyatlandırma sağlayın. Fiyatları bir elektronik tablodan içeri aktarabilirsiniz.

> [!NOTE]
> Fiyatlandırma verilerinin dışa aktarılmasını sağlamak için fiyatlandırma değişikliklerini kaydedin. Planınızdaki bir pazar bedeli yayımlandıktan sonra, daha sonra değiştirilemez. Fiyatları yayımlamadan önce fiyatların doğru olduğundan emin olmak için, fiyatlandırma elektronik tablosunu dışarı aktarın ve her pazardaki fiyatları gözden geçirin.

### <a name="free-trial"></a>Ücretsiz Deneme

Müşterilerinize bir aylık veya üç aylık veya altı aylık *ücretsiz deneme sürümü* sunabilirsiniz.

### <a name="visibility"></a>Görüş Mesafesi

Her planı herkese görünür olacak şekilde veya yalnızca önceden seçilmiş bir kitleye tasarlayabilirsiniz. Azure abonelik kimliklerini kullanarak bu kısıtlanmış hedef kitlelere üyelik atayın.

**Ortak** : planınız herkes tarafından görülebilir.

**Özel hedef kitle** : planınızı yalnızca önceden seçilmiş bir hedef kitle için görünür hale getirin. Özel bir plan olarak yayımlandıktan sonra, hedef kitleyi güncelleştirebilir veya ortak olarak değiştirebilirsiniz. Planı herkese açık hale geçirdikten sonra, genel olarak kalması gerekir. Özel bir plana geri değiştirilemez.

> [!NOTE]
> Özel veya kısıtlanmış bir hedef kitle, **Önizleme** bölmesinde tanımladığınız önizleme izleyicilerinizden farklıdır. Bir önizleme hedef kitlesi, Azure Market 'Te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine açabilir. Özel hedef kitle seçimi yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi tüm özel ve genel planları doğrulama amaçlarıyla görüntüleyebilir.

**Kısıtlanmış hedef kitle (Azure abonelik kimlikleri)** : Azure abonelik kimliklerini kullanarak bu özel plana erişimi olacak hedef kitleyi atayın. İsteğe bağlı olarak, atadığınız her bir Azure abonelik KIMLIĞI için bir açıklama ekleyin. Bir CSV elektronik tablosu içeri aktarıyorsanız, en fazla 10 abonelik kimliğini el ile veya 20.000 kimlik olarak ekleyin. Azure abonelik kimlikleri GUID olarak temsil edilir ve tüm harflerin küçük harf olması gerekir.

>[!Note]
>Özel teklifler, bulut çözümü sağlayıcısı programı 'nın (CSP) satıcısı aracılığıyla oluşturulan Azure abonelikleri ile desteklenmez.

### <a name="hide-a-plan"></a>Planı gizleme

Sanal makineniz, başka bir çözüm şablonu veya yönetilen uygulama aracılığıyla başvurulduğu zaman dolaylı olarak yalnızca dolaylı olarak kullanılacaksa, sanal makineyi yayımlamak için bu onay kutusunu işaretleyin, ancak doğrudan arama veya tarama yapmakta olabilecek müşterilerden gizleyin.

> [!NOTE]
> Gizli planlar önizleme bağlantılarını desteklemez.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration"></a>Teknik yapılandırma

Bu planla ilişkili görüntüleri ve diğer teknik özellikleri sağlayın. Daha fazla bilgi için bkz. [VM teklif listesi ayrıntılarını yapılandırma](azure-vm-create-listing.md).

> [!NOTE]
> Bu planı, **plan kurulum** sekmesindeki başka bir plandan paketleri yeniden kullanmak üzere yapılandırdıysanız **Teknik yapılandırma** sekmesi gösterilmez.

### <a name="operating-system"></a>İşletim sistemi

**İşletim sistemi** bölmesinde şunları yapın:

- **İşletim sistemi ailesi** Için, **Windows** veya **Linux** işletim sistemini seçin.
- **Yayın** veya **satıcı** Için Windows sürümü veya Linux satıcısı ' nı seçin.
- **Işletim sistemi kolay adı** için kolay bir işletim sistemi adı girin. Bu ad, müşteriler tarafından görülebilir.

### <a name="recommended-vm-sizes"></a>Önerilen VM boyutları

Azure Marketi 'nde görüntülenmek üzere en fazla altı önerilen sanal makine boyutu seçin.

### <a name="open-ports"></a>Bağlantı noktalarını açma

Dağıtılmış bir sanal makinede ortak veya özel bağlantı noktalarını açın.

### <a name="storage-option-for-deployment"></a>Dağıtım için depolama seçeneği

**Disk dağıtımı seçeneği** için, müşterilerinizin sanal makine için kullanabileceği disk dağıtımı türünü seçin. Microsoft dağıtımı yalnızca **yönetilen disk dağıtımıyla** sınırlandırmanızı önerir.

### <a name="properties"></a>Özellikler

**Destek hızlandırılmış ağ iletişimi** IÇIN, sanal makinenizin [hızlandırılmış ağı](https://go.microsoft.com/fwlink/?linkid=2124513)destekleyip desteklemediğini seçin.

### <a name="generations"></a>İse

Bir sanal makine oluşturmak, kullandığı sanal donanımı tanımlar. Müşterinizin ihtiyaçlarına göre, 1. nesil bir VM, 2. nesil VM veya her ikisini de yayımlayabilirsiniz.

1. Yeni bir teklif oluştururken, bir **oluşturma türü** seçin ve istenen görüntü ayrıntılarını girin:

    :::image type="content" source="./media/create-vm/azure-vm-generations-image-details.png" alt-text="Oluşturma açılan kutusunun bir görünümü.":::

2. Plana bir oluşturma işlemi eklemek için **oluşturma oluştur** ' u seçin:

    :::image type="content" source="./media/create-vm/azure-vm-generations-add.png" alt-text="Oluşturma açılan kutusunun bir görünümü.":::

    Sonra oluşturma ayrıntılarını girin:

    :::image type="content" source="./media/create-vm/azure-vm-generations-details.png" alt-text="Oluşturma açılan kutusunun bir görünümü.":::

    Seçtiğiniz **oluşturma kimliği** , ürün URL 'LERI ve ARM şablonları (varsa) gibi yerlerde müşterilere görünür olacaktır. Yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi kullanın; yayımlandıktan sonra değiştirilemez.

3. 1. nesil zaten yayınlanmış var olan bir VM 'yi güncelleştirmek için **Teknik yapılandırma** sayfasında ayrıntıları düzenleyin:

    :::image type="content" source="./media/create-vm/azure-vm-generations-updating.png" alt-text="Oluşturma açılan kutusunun bir görünümü.":::

1. nesil ve 2. nesil yetenekler arasındaki farklar hakkında daha fazla bilgi edinmek için bkz. [Azure 'da 2. nesil sanal makineler Için destek](../virtual-machines/generation-2.md).

### <a name="vm-images"></a>VM görüntüleri

Sanal makine görüntüleri için bir disk sürümü ve paylaşılan erişim imzası (SAS) URI 'SI sağlayın. Her VM görüntüsü için en fazla 16 veri diski ekleyin. Belirtilen bir gönderideki plan başına yalnızca bir yeni görüntü sürümü sağlayın. Bir görüntü yayımlandıktan sonra düzenleyemezsiniz, ancak silebilirsiniz. Bir sürümü silmek, hem yeni hem de mevcut kullanıcıların silinen sürümün yeni bir örneğini dağıtmalarını engeller.

- **Disk sürümü** : sağlaettiğiniz görüntünün sürümü.
- **SAS URI 'si** : IŞLETIM sistemi VHD 'Sini depoladığınız Azure Storage hesabınızdaki konum. SAS URI 'sini alma hakkında bilgi edinmek için bkz. [VM Görüntünüz için paylaşılan erişim imzası URI 'Si alma](azure-vm-get-sas-uri.md).
- Veri diski görüntüleri Ayrıca, Azure depolama hesaplarında depolanan VHD paylaşılan erişim imza URI 'leridir.
- Bir plana gönderim başına yalnızca bir resim ekleyin.

Kullandığınız işletim sisteminden bağımsız olarak, yalnızca çözüm için gereken en az sayıda veri diski ekleyin. Dağıtım sırasında, müşteriler bir görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler.

Devam etmeden önce **Taslağı kaydet** ' i seçin ve **plana genel bakış** 'a geri dönün.

## <a name="next-steps"></a>Sonraki adımlar

- [Önizleme hedef kitlesi ekleme](azure-vm-create-preview.md)
