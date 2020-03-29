---
title: Azure'da Ethereum Proof-of-Authority konsorsiyum uyrmaci çözüm şablonu dağıtma
description: Azure'da çok üyeli bir ethereum konsorsiyumu ethereum ağını dağıtmak ve yapılandırmak için Ethereum Proof-of-Authority konsorsiyumu çözümünün kullanın
ms.date: 12/18/2019
ms.topic: article
ms.reviewer: coborn
ms.openlocfilehash: 7e9af5c501b58f6828360ee280440ea85698bf16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75387676"
---
# <a name="deploy-ethereum-proof-of-authority-consortium-solution-template-on-azure"></a>Azure'da Ethereum yetki belgesi konsorsiyumu çözüm şablonu dağıtma

[Ethereum Proof-of-Authority Konsorsiyumu önizleme Azure çözüm şablonu'nu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-blockchain.azure-blockchain-ethereum) kullanarak çok üyeli bir konsorsiyum un yetki belgesi ethereum ağını en az Azure ve Ethereum bilgisine sahip olarak dağıtabilirsiniz.

Çözüm şablonu, Microsoft Azure hesaplama, ağ ve depolama hizmetlerini kullanarak bir blockchain ağ ayak izi sağlamak için her konsorsiyum üyesi tarafından kullanılabilir. Her konsorsiyum üyesinin ağ ayak izi, bir uygulamanın veya kullanıcının Ethereum işlemlerini göndermek için etkileşimkurabileceği bir dizi yük dengeli doğrulayıcı düğümden oluşur.

## <a name="choose-an-azure-blockchain-solution"></a>Azure Blockchain çözümü seçin

Ethereum yetki belgesi konsorsiyum çözümü şablonunu kullanmayı seçmeden önce, senaryonuzu kullanılabilir Azure Blockchain seçeneklerinin yaygın kullanım örnekleriyle karşılaştırın.

Seçenek | Servis modeli | Ortak kullanım örneği
-------|---------------|-----------------
Çözüm şablonları | IaaS | Çözüm şablonları, tam olarak yapılandırılmış blockchain ağ topolojisini sağlamak için kullanabileceğiniz Azure Kaynak Yöneticisi şablonlarıdır. Şablonlar, belirli bir blockchain ağ türü için Microsoft Azure bilgi işlem, ağ ve depolama hizmetlerini dağıtAbilir ve yapılandırır.
[Azure Blok Zinciri Hizmeti](../service/overview.md) | PaaS | Azure Blockchain Service Preview, konsorsiyum blockchain ağlarının oluşumunu, yönetimini ve yönetimini kolaylaştırır. PaaS, konsorsiyum yönetimi veya sözleşme ve işlem gizliliği gerektiren çözümler için Azure Blockchain Hizmetini kullanın.
[Azure Blockchain Workbench](../workbench/overview.md) | IaaS ve PaaS | Azure Blockchain Workbench Preview, iş süreçlerini ve verileri diğer kuruluşlarla paylaşmak için blockchain uygulamaları oluşturmanıza ve dağıtmanıza yardımcı olmak üzere tasarlanmış bir Azure hizmetleri ve yetenekleri topluluğudur. Blockchain çözümünün prototipini yapmak veya blockchain uygulama kavramı nın kanıtı için Azure Blockchain Workbench'i kullanın.

## <a name="solution-architecture"></a>Çözüm mimarisi

Ethereum çözüm şablonu'nu kullanarak, tek veya çok bölgeli tabanlı çok üyeli Ethereum yetki belgesi konsorsiyum ağı dağıtabilirsiniz.

![dağıtım mimarisi](./media/ethereum-poa-deployment/deployment-architecture.png)

Her konsorsiyum üye dağıtımı şunları içerir:

* PoA doğrulayıcılarını çalıştırmak için Sanal Makineler
* RPC, peering ve yönetim DApp isteklerini dağıtmak için Azure Yük Dengeleyicisi
* Geçerli kimliklerin güvenliğini sağlamak için Azure Anahtar Kasası
* Kalıcı ağ bilgilerini barındırmak ve kiralamayı koordine etmek için Azure Depolama
* Günlükleri ve performans istatistiklerini bir araya kaydetmek için Azure Monitörü
* Özel VNet'lerde VPN bağlantılarına izin vermek için VNet Ağ Geçidi (isteğe bağlı)

Varsayılan olarak, RPC ve bakan uç noktalarına, abonelikler ve bulutlar arasında basitleştirilmiş bağlantı sağlamak için genel IP üzerinden erişilebilir. Uygulama düzeyi erişim denetimleri için [Parity'nin izin sözleşmelerini](https://wiki.parity.io/Permissioning)kullanabilirsiniz. Çapraz abonelik bağlantısı için VNet ağ geçitlerinden yararlanan VPN'lerin arkasına dağıtılan ağlar desteklenir. VPN ve VNet dağıtımları daha karmaşık olduğundan, bir çözüm prototipi yaparken genel bir IP modeliyle başlamak isteyebilirsiniz.

Docker konteynerleri güvenilirlik ve modülerlik için kullanılır. Azure Kapsayıcı Kayıt Defteri, her dağıtımın bir parçası olarak sürümlenmiş görüntüleri barındırmak ve sunmak için kullanılır. Kapsayıcı görüntüleri aşağıdakilerden oluşur:

* Orchestrator - Kimlikler ve yönetim sözleşmeleri oluşturur. Kimlikleri bir kimlik mağazasında saklar.
* Eşlik istemcisi - Kimlik mağazasından kimlik kiralar. Eşleri keşfeder ve eşlere bağlar.
* EthStats Agent - RPC aracılığıyla yerel günlükleri ve istatistikleri toplar ve bilgileri Azure Monitor'a doğru iter.
* Governance DApp - Yönetim sözleşmeleri ile etkileşim için web arayüzü.

### <a name="validator-nodes"></a>Doğrulayıcı düğümler

Yetki kanıtı protokolünde, geçerli düğümler geleneksel madenci düğümlerinin yerini alır. Her doğrulayıcı, blok oluşturma işlemine katılmasına izin veren benzersiz bir Ethereum kimliğine sahiptir. Her konsorsiyum üyesi, coğrafi artıklık için beş bölgede iki veya daha fazla doğrulayıcı düğüm sağlayabilir. Geçerli düğümler, altta yatan dağıtılmış defterin durumu üzerinde fikir birliğine varmak için diğer doğrulayıcı düğümlerle iletişim kurar. Ağa adil katılım sağlamak için, her konsorsiyum üyesinin ağdaki ilk üyeden daha fazla doğrulayıcı kullanması yasaktır. Örneğin, ilk üye üç doğrulayıcı dağıtırsa, her üyenin en fazla üç doğrulayıcısı olabilir.

### <a name="identity-store"></a>Kimlik deposu

Oluşturulan Ethereum kimliklerini güvenli bir şekilde tutan her üyenin aboneliğinde bir kimlik deposu dağıtılır. Her doğrulama için, orkestrasyon konteyneri bir Ethereum özel anahtarı oluşturur ve Azure Key Vault'ta saklar.

## <a name="deploy-ethereum-consortium-network"></a>Ethereum konsorsiyum ağını dağıtın

Bu yürüyüşte, çok partili bir Ethereum konsorsiyum ağı oluşturduğunuzu varsayalım. Aşağıdaki akış, çok taraflı bir dağıtıma örnektir:

1. Üç üye her MetaMask kullanarak bir Ethereum hesabı oluşturmak
1. *Üye A,* Ethereum genel adresini sağlayarak Ethereum PoA'yı konuşlandırıyor
1. *Üye A,* konsorsiyum URL'sini *Üye B* ve *Üye C'ye* sağlar
1. *Üye B* ve *Üye C* dağıtmak, Ethereum PoA, kendi Ethereum Kamu Adresi ve *Üye A*'konsorsiyum URL sağlayan
1. *Üye A,* *Üye B'de* yönetici olarak oy kullanır
1. *Üye A* ve *Üye B* her ikisi de bir yönetici olarak *Üye C* oy

Sonraki bölümlerde ilk üyenin ağdaki ayak izini nasıl yapılandırabileceğiniz gösterin.

### <a name="create-resource"></a>Kaynak oluşturma

Azure [portalında,](https://portal.azure.com)sol üst köşede **kaynak oluştur'u** seçin.

**Blockchain** > **Ethereum Proof-of-Authority Konsorsiyumu 'yu seçin (önizleme)**.

### <a name="basics"></a>Temel Bilgiler

**Temel Bilgiler**altında, herhangi bir dağıtım için standart parametreler için değerleri belirtin.

![Temel Bilgiler](./media/ethereum-poa-deployment/basic-blade.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Yeni bir ağ oluşturma veya varolan ağa katılma | Yeni bir konsorsiyum ağı oluşturabilir veya önceden varolan bir konsorsiyum ağına katılabilirsiniz. Varolan bir ağa katılmak ek parametreler gerektirir. | Yeni oluştur
E-posta Adresi | Dağıtımınız dağıtımınızla ilgili bilgilerle tamamlandığında bir e-posta bildirimi alırsınız. | Geçerli bir e-posta adresi
VM kullanıcı adı | Dağıtılan her VM'nin yönetici kullanıcı adı | 1-64 alfanümerik karakter
Kimlik doğrulaması türü | Sanal makineye kimlik doğrulama yöntemi. | Parola
Parola | Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Tüm VM'ler başlangıçta aynı parolaya sahiptir. Hükmü aldıktan sonra parolayı değiştirebilirsiniz. | 12-72 karakter 
Abonelik | Konsorsiyum ağının dağıtılabilmek için abonelik |
Kaynak Grubu| Konsorsiyum ağını dağıtacak kaynak grubu. | myResourceGroup
Konum | Kaynak grubu için Azure bölgesi. | Batı ABD 2

**Tamam'ı**seçin.

### <a name="deployment-regions"></a>Dağıtım bölgeleri

*Dağıtım bölgeleri*altında, her biri için bölge ve konum sayısını belirtin. En fazla beş bölgede dağıtabilirsiniz. İlk *bölge, Temeller* bölümündeki kaynak grubu konumuyla eşleşmelidir. Geliştirme veya test ağları için üye başına tek bir bölge kullanabilirsiniz. Üretim için, yüksek kullanılabilirlik için iki veya daha fazla bölgeye dağıtın.

![dağıtım bölgeleri](./media/ethereum-poa-deployment/deployment-regions.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Bölge(ler) sayısı|Konsorsiyum ağını dağıtacak bölge sayısı| 2
İlk bölge | Konsorsiyum ağını dağıtacak ilk bölge | Batı ABD 2
İkinci bölge | Konsorsiyum ağını dağıtacak ikinci bölge. Bölge sayısı iki veya daha büyük olduğunda ek bölgeler görünür. | Doğu ABD 2

**Tamam'ı**seçin.

### <a name="network-size-and-performance"></a>Ağ boyutu ve performansı

*Ağ boyutu ve performansı*altında, konsorsiyum ağının boyutu için girdileri belirtin. Geçerlidüğüm depolama boyutu blockchain'in olası boyutunu belirler. Dağıtımdan sonra boyutu değiştirilebilir.

![Ağ boyutu ve performansı](./media/ethereum-poa-deployment/network-size-and-performance.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Yük dengeli doğrulayıcı düğümsayısı | Ağın bir parçası olarak hükme validator düğümlerinin sayısı. | 2
Geçerli düğüm depolama performansı | Dağıtılan geçerlilik düğümlerinin her biri için yönetilen disk türü. Fiyatlandırma hakkında ayrıntılı bilgi [için, depolama alanı fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/) | Standart SSD
Geçerlidüğüm sanal makine boyutu | Doğrulayıcı düğümler için kullanılan sanal makine boyutu. Fiyatlandırma hakkında ayrıntılı bilgi için [sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) | Standart D2 v3

Sanal makine ve depolama katmanı ağ performansını etkiler.  Maliyet verimliliğini seçmenize yardımcı olmak için aşağıdaki tabloyu kullanın:

Sanal Makine SKU|Depolama Katmanı|Fiyat|Aktarım hızı|Gecikme süresi
---|---|---|---|---
F1|Standart SSD|Düşük|Düşük|yüksek
D2_v3|Standart SSD|orta|orta|orta
F16'lar|Premium SSD|yüksek|yüksek|Düşük

**Tamam'ı**seçin.

### <a name="ethereum-settings"></a>Ethereum ayarları

*Ethereum Ayarları*altında, Ethereum ile ilgili yapılandırma ayarlarını belirtin.

![Ethereum ayarları](./media/ethereum-poa-deployment/ethereum-settings.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Konsorsiyum Üye Kimliği | Konsorsiyum ağına katılan her üyeyle ilişkili kimlik. Çarpışmayı önlemek için IP adresi boşluklarını yapılandırmak için kullanılır. Özel bir ağ için Üye Kimliği, aynı ağdaki farklı kuruluşlar arasında benzersiz olmalıdır.  Aynı kuruluş birden çok bölgeye dağıldığında bile benzersiz bir üye kimliği gereklidir. Çarpışma olmadığından emin olmak için bu parametrenin değerini not edin. Geçerli aralık 0 ile 255 arasındadır. | 0
Ağ Kimliği | Ethereum konsorsiyumunun ağ kimliği dağıtılıyor. Her Ethereum ağının kendi Ağ Kimliği vardır ve 1'i ortak ağ kimliğidir. Geçerli aralık 5 ile 999.999.999 | 10101010
Admin Ethereum Adresi | PoA yönetimine katılmak için kullanılan Ethereum hesap adresi. Bir Ethereum adresi oluşturmak için MetaMask'i kullanabilirsiniz. |
Gelişmiş Seçenekler | Ethereum ayarları için gelişmiş seçenekler | Etkinleştirme
Genel IP kullanarak dağıtma | Private VNet seçilirse, ağ bir VNet Ağ Geçidi'nin arkasına dağıtılır ve eşleme erişimini kaldırır. Özel VNet için, bağlantının uyumlu olabilmesi için tüm üyelerin bir VNet Ağ Geçidi kullanması gerekir. | Genel IP
Blok Gaz Limiti | Ağın başlangıç bloğu gaz sınırı. | 50000000
Blok Rseal Dönemi (sn) | Ağda işlem olmadığında boş blokların oluşturulma sıklığı. Daha yüksek bir frekans daha hızlı kesinlik ancak depolama maliyetleri artacaktır. | 15
İşlem İzni Sözleşmesi | İşlem İzni sözleşmesi için bytecode. Akıllı sözleşme dağıtımını ve yürütmeyi izin verilen Ethereum hesapları listesiyle sınırlandırır. |

**Tamam'ı**seçin.

### <a name="monitoring"></a>İzleme

İzleme, ağınız için bir günlük kaynağını yapılandırmanıza olanak tanır. İzleme aracı, ağ durumunu veya hata ayıklama sorunlarını hızlı bir şekilde kontrol etme olanağı sağlayan yararlı ölçümleri ve günlükleri anızdakileri toplar ve yüzeyler.

![Azure monitör](./media/ethereum-poa-deployment/azure-monitor.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
İzleme | İzlemeyi etkinleştirme seçeneği | Etkinleştirme
Varolan Azure Monitör günlüklerine bağlanma | Yeni bir Azure Monitörü günlükleri örneği oluşturma veya varolan bir örne katılma seçeneği | Yeni oluştur
Konum | Yeni örneğin dağıtıldığı bölge | Doğu ABD
Varolan günlük analizi çalışma alanı kimliği (Varolan Azure Monitör günlüklerine bağlan = Varolana Katıl)|Varolan Azure Monitor günlükleri örneğinin çalışma alanı kimliği||NA
Varolan günlük analitiği birincil anahtarı (Varolan Azure Monitör günlüklerine bağlan = Varolana Katıl)|Varolan Azure Monitörgünlükleri örneğine bağlanmak için kullanılan birincil anahtar||NA

**Tamam'ı**seçin.

### <a name="summary"></a>Özet

Belirtilen girişleri gözden geçirmek ve temel dağıtım öncesi doğrulamayı çalıştırmak için özeti tıklatın. Dağıtmadan önce şablonu ve parametreleri indirebilirsiniz.

Dağıtmak için **Oluştur'u** seçin.

Dağıtım VNet Ağ Geçitleri içeriyorsa, dağıtım 45 ila 50 dakika sürebilir.

## <a name="deployment-output"></a>Dağıtım çıktısı

Dağıtım tamamlandıktan sonra, Azure portalını kullanarak gerekli parametrelere erişebilirsiniz.

### <a name="confirmation-email"></a>Onay e-postası

Bir e-posta adresi[(Temel Bilgiler Bölümü)](#basics)sağlarsanız, dağıtım bilgilerini ve bu belgelere bağlantılar içeren bir e-posta gönderilir.

![dağıtım e-postası](./media/ethereum-poa-deployment/deployment-email.png)

### <a name="portal"></a>Portal

Dağıtım başarıyla tamamlandıktan ve tüm kaynaklar sağlandıktan sonra, kaynak grubunuzdaki çıktı parametrelerini görüntüleyebilirsiniz.

1. Portaldaki kaynak grubunuza gidin.
1. **Dağıtımlara Genel Bakış >** seçin.

    ![Kaynak grubuna genel bakış](./media/ethereum-poa-deployment/resource-group-overview.png)

1. **microsoft-azure-blockchain.azure-blockchain-eter-...** dağıtımını seçin.
1. **Çıktılar** bölümünü seçin.

    ![Dağıtım çıktıları](./media/ethereum-poa-deployment/deployment-outputs.png)

## <a name="growing-the-consortium"></a>Konsorsiyumun büyümesi

Konsorsiyumunuzu genişletmek için önce fiziksel ağı bağlamanız gerekir. VPN'in arkasına dağıtıyorsanız, [VNet Ağ Geçidi'ni bağlayan](#connecting-vnet-gateways) bölüme bakın, ağ bağlantısını yeni üye dağıtımının bir parçası olarak yapılandırın. Dağıtımınız tamamlandıktan sonra, ağ yöneticisi olmak için [Yönetim DApp'ini](#governance-dapp) kullanın.

### <a name="new-member-deployment"></a>Yeni üye dağıtımı

Aşağıdaki bilgileri katılan üyeyle paylaşın. Bilgiler dağıtım sonrası e-postanızda veya portal dağıtım çıktısında bulunur.

* Konsorsiyum Veri URL'si
* Dağıttığınız düğüm sayısı
* VNet Ağ Geçidi Kaynak Kimliği (VPN kullanıyorsanız)

Dağıtım üyesi, ağ varlıklarını aşağıdaki kılavuzu kullanarak dağıtırken aynı Ethereum Proof-of-Authority konsorsiyum uyrmaci çözüm şablonunu kullanmalıdır:

* **Varolan Join'i** seçin
* Adil temsili sağlamak için ağdaki diğer üyelerle aynı sayıda geçerlilik düğümü seçin
* Aynı Yönetici Ethereum adresini kullanın
* Sağlanan *Konsorsiyum Veri Url'sini* *Ethereum Ayarları'nda* kullanma
* Ağın geri kalanı VPN'in arkasındaysa, gelişmiş bölümün altındaki **Özel VNet'i** seçin

### <a name="connecting-vnet-gateways"></a>VNet ağ geçitlerini bağlama

Bu bölüm yalnızca özel bir VNet kullanarak dağıttıysanız gereklidir. Genel IP adreslerini kullanıyorsanız bu bölümü atlayabilirsiniz.

Özel bir ağ için, farklı üyeler VNet ağ geçidi bağlantıları üzerinden bağlanır. Bir üyenin ağa katılıp işlem trafiğini göremeden önce, varolan bir üyenin bağlantıyı kabul etmek için VPN ağ geçidinde son bir yapılandırma yapması gerekir. Bir bağlantı kurulana kadar birleşin üyenin Ethereum düğümleri çalışmaz. Tek bir hata noktası olasılığını azaltmak için konsorsiyumda gereksiz ağ bağlantıları oluşturun.

Yeni üye dağıtıldıktan sonra, varolan üyenin yeni üyeye bir VNet ağ geçidi bağlantısı kurarak çift yönlü bağlantıyı tamamlaması gerekir. Mevcut üyenin ihtiyaçları:

* Bağlanan üyenin VNet ağ geçidi ResourceID. [Bkz. dağıtım çıktısı.](#deployment-output)
* Paylaşılan bağlantı anahtarı.

Bağlantıyı tamamlamak için varolan üyenin aşağıdaki PowerShell komut dosyasını çalıştırması gerekir. Portaldaki sağ üst gezinti çubuğunda bulunan Azure Bulut Shell'i kullanabilirsiniz.

![bulut kabuğu](./media/ethereum-poa-deployment/cloud-shell.png)

```Powershell
$MyGatewayResourceId = "<EXISTING_MEMBER_RESOURCEID>"
$OtherGatewayResourceId = "<NEW_MEMBER_RESOURCEID]"
$ConnectionName = "Leader2Member"
$SharedKey = "<NEW_MEMBER_KEY>"

## $myGatewayResourceId tells me what subscription I am in, what ResourceGroup and the VNetGatewayName
$splitValue = $MyGatewayResourceId.Split('/')
$MySubscriptionid = $splitValue[2]
$MyResourceGroup = $splitValue[4]
$MyGatewayName = $splitValue[8]

## $otherGatewayResourceid tells me what the subscription and VNet GatewayName are
$OtherGatewayName = $OtherGatewayResourceId.Split('/')[8]
$Subscription=Select-AzSubscription -SubscriptionId $MySubscriptionid

## create a PSVirtualNetworkGateway instance for the gateway I want to connect to
$OtherGateway=New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
$OtherGateway.Name = $OtherGatewayName
$OtherGateway.Id = $OtherGatewayResourceId
$OtherGateway.GatewayType = "Vpn"
$OtherGateway.VpnType = "RouteBased"

## get a PSVirtualNetworkGateway instance for my gateway
$MyGateway = Get-AzVirtualNetworkGateway -Name $MyGatewayName -ResourceGroupName $MyResourceGroup

## create the connection
New-AzVirtualNetworkGatewayConnection -Name $ConnectionName -ResourceGroupName $MyResourceGroup -VirtualNetworkGateway1 $MyGateway -VirtualNetworkGateway2 $OtherGateway -Location $MyGateway.Location -ConnectionType Vnet2Vnet -SharedKey $SharedKey -EnableBgp $True
```

## <a name="service-monitoring"></a>Hizmet izleme

Azure Monitor portalınızı dağıtım e-postasındaki bağlantıyı izleyerek veya dağıtım çıktısındaki parametreyi [OMS_PORTAL_URL] bularak bulabilirsiniz.

Portal ilk olarak üst düzey ağ istatistiklerini ve düğüme genel bakışı görüntüler.

![Kategorileri izleyin](./media/ethereum-poa-deployment/monitor-categories.png)

**DüğümE Genel Bakış'ı** seçmek, düğüm başına altyapı istatistiklerini gösterir.

![Düğüm istatistikleri](./media/ethereum-poa-deployment/node-stats.png)

Ağ **İstatistikleri'ni** seçmek, Ethereum ağ istatistiklerini gösterir.

![Ağ istatistikleri](./media/ethereum-poa-deployment/network-stats.png)

### <a name="sample-kusto-queries"></a>Örnek Kusto sorguları

Hataları veya kurulum eşiği uyarılarını araştırmak için izleme günlüklerini sorgulayabilirsiniz. Aşağıdaki *sorgular, Günlük Arama* aracında çalıştırabileceğiniz örneklerdir:

Birden fazla doğrulayıcı sorgu tarafından bildirilen liste blokları zincir çatalları bulmada yardımcı olmak için yararlı olabilir.

```sql
MinedBlock_CL
| summarize DistinctMiners = dcount(BlockMiner_s) by BlockNumber_d, BlockMiner_s
| where DistinctMiners > 1
```

5 dakikalık kovaların ortalaması olan belirli bir doğrulayıcı düğüm için ortalama eş sayısı alın.

```sql
let PeerCountRegex = @"Syncing with peers: (\d+) active, (\d+) confirmed, (\d+)";
ParityLog_CL
| where Computer == "vl-devn3lgdm-reg1000001"
| project RawData, TimeGenerated
| where RawData matches regex PeerCountRegex
| extend ActivePeers = extract(PeerCountRegex, 1, RawData, typeof(int))
| summarize avg(ActivePeers) by bin(TimeGenerated, 5m)
```

## <a name="ssh-access"></a>SSH erişimi

Güvenlik nedenleriyle, SSH bağlantı noktası erişimi varsayılan olarak bir ağ grubu güvenlik kuralı tarafından reddedilir. PoA ağındaki sanal makine örneklerine erişmek için aşağıdaki güvenlik kuralını değiştirmeniz *gerekir.*

1. Azure portalında dağıtılan kaynak grubunun **Genel Bakış** bölümüne gidin.

    ![ssh genel bakış](./media/ethereum-poa-deployment/ssh-overview.png)

1. Erişmek istediğiniz VM bölgesi için **Ağ Güvenlik Grubu'nu** seçin.

    ![ssh nsg](./media/ethereum-poa-deployment/ssh-nsg.png)

1. **İzin-ssh** kuralını seçin.

    ![ssh-izin](./media/ethereum-poa-deployment/ssh-allow.png)

1. **İzin Vermek** için **Eylemi** Değiştir

    ![ssh izin vermek etkinleştirmek](./media/ethereum-poa-deployment/ssh-enable-allow.png)

1. **Kaydet'i**seçin. Değişikliklerin uygulanması birkaç dakika sürebilir.

Sağlanan yönetici kullanıcı adı ve şifre/SSH tuşu ile SSH üzerinden doğrulama düğümleri için sanal makinelere uzaktan bağlanabilirsiniz. İlk geçerlilik düğümüne erişmek için SSH komutu şablon dağıtım çıktısında listelenir. Örnek:

``` bash
ssh -p 4000 poaadmin\@leader4vb.eastus.cloudapp.azure.com.
```

Ek işlem düğümlerine ulaşmak için bağlantı noktası numarasını bire bir artım.

Birden fazla bölgeye dağıttıysanız, komutu o bölgedeki yük bakiyeleyicisinin DNS adı veya IP adresiolarak değiştirin. Diğer bölgelerin DNS adını veya IP adresini bulmak için, adlandırma kuralı ** \* \* \* \* \*\# -lbpip-reg'i** içeren kaynağı bulun ve DNS adını ve IP adresi özelliklerini görüntüleyin.

## <a name="azure-traffic-manager-load-balancing"></a>Azure Trafik Yöneticisi yük dengeleme

Azure Trafik Yöneticisi, farklı bölgelerdeki birden fazla dağıtımda gelen trafiği yönlendirme yaparak kapalı kalma süresini azaltmaya ve PoA ağının yanıt verme yeteneğini artırmaya yardımcı olabilir. Yerleşik sağlık kontrolleri ve otomatik yönlendirme, RPC uç noktalarının ve Yönetim DApp'inin yüksek kullanılabilirliğini sağlamaya yardımcı olur. Bu özellik, birden çok bölgeye dağıttıysanız ve üretime hazırsanız yararlıdır.

Otomatik arıza yla PoA ağ kullanılabilirliğini iyileştirmek için Trafik Yöneticisi'ni kullanın. Ayrıca, son kullanıcıları en düşük ağ gecikmesiyle Azure konumuna yönlendirme yaparak ağlarınızın yanıt verme yeteneğini artırmak için Trafik Yöneticisi'ni de kullanabilirsiniz.

Bir Trafik Yöneticisi profili oluşturmaya karar verirseniz, ağınıza erişmek için profilin DNS adını kullanabilirsiniz. Diğer konsorsiyum üyeleri ağa eklendikten sonra, Trafik Yöneticisi dağıtılan doğrulayıcıları arasında bakiye yüklemek için de kullanılabilir.

### <a name="creating-a-traffic-manager-profile"></a>Trafik Yöneticisi profili oluşturma

1. Azure [portalında,](https://portal.azure.com)sol üst köşede **kaynak oluştur'u** seçin.
1. Trafik **Yöneticisi profilini**ara.

    ![Azure Trafik Yöneticisi'ni Ara](./media/ethereum-poa-deployment/traffic-manager-search.png)

    Profile benzersiz bir ad verin ve PoA dağıtımı için kullanılan Kaynak Grubu'nu seçin.

1. Dağıtmak için **Oluştur'u** seçin.

    ![Trafik Yöneticisi Oluşturma](./media/ethereum-poa-deployment/traffic-manager-create.png)

1. Dağıtıldıktan sonra, kaynak grubundaki örneği seçin. Trafik yöneticisine erişmek için DNS adı Genel Bakış sekmesinde bulunabilir.

    ![Trafik yöneticisi DNS'yi bulma](./media/ethereum-poa-deployment/traffic-manager-dns.png)

1. Uç **Noktalar** sekmesini seçin ve **Ekle** düğmesini seçin.
1. Bitiş noktasına benzersiz bir ad verin.
1. **Hedef kaynak türü için**Ortak IP **adresini**seçin.
1. İlk bölgenin yük dengeleyicisinin genel IP adresini seçin.

    ![Yönlendirme trafik yöneticisi](./media/ethereum-poa-deployment/traffic-manager-routing.png)

Dağıtılan ağdaki her bölge için yineleyin. Uç noktalar **etkin** duruma geldikten sonra, otomatik olarak yüklenir ve trafik yöneticisinin DNS adına bölge dengelenir. Artık bu DNS adını makalenin diğer adımlarında [CONSORTIUM_DATA_URL] parametresi yerine kullanabilirsiniz.

## <a name="data-api"></a>Veri API'sı

Her konsorsiyum üyesi, diğerlerinin ağa bağlanması için gerekli bilgileri barındırr. Bağlantı kolaylığını etkinleştirmek için, her üye veri API bitiş noktasında bir bağlantı bilgisi kümesi barındırır.

Varolan üye, üyenin göreve başlamasından önce [CONSORTIUM_DATA_URL] sağlar. Dağıtım sırasında, katılan bir üye json arabiriminden aşağıdaki bitiş noktasından bilgi alır:

`<CONSORTIUM_DATA_URL>/networkinfo`

Yanıt, üyelere katılmak için yararlı bilgiler (Genesis bloğu, Validator Set sözleşme ABI, bootnodes) ve mevcut üye (geçerli adresleri) için yararlı bilgiler içerir. Konsorsiyumu bulut sağlayıcıları arasında genişletmek için bu standardizasyonu kullanabilirsiniz. Bu API aşağıdaki yapıile bir JSON biçimlendirilmiş yanıt döndürür:

```json
{
  "$id": "",
  "type": "object",
  "definitions": {},
  "$schema": "https://json-schema.org/draft-07/schema#",
  "properties": {
    "majorVersion": {
      "$id": "/properties/majorVersion",
      "type": "integer",
      "title": "This schema’s major version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "minorVersion": {
      "$id": "/properties/minorVersion",
      "type": "integer",
      "title": "This schema’s minor version",
      "default": 0,
      "examples": [
        0
      ]
    },
    "bootnodes": {
      "$id": "/properties/bootnodes",
      "type": "array",
      "items": {
        "$id": "/properties/bootnodes/items",
        "type": "string",
        "title": "This member’s bootnodes",
        "default": "",
        "examples": [
          "enode://a348586f0fb0516c19de75bf54ca930a08f1594b7202020810b72c5f8d90635189d72d8b96f306f08761d576836a6bfce112cfb6ae6a3330588260f79a3d0ecb@10.1.17.5:30300",
          "enode://2d8474289af0bb38e3600a7a481734b2ab19d4eaf719f698fe885fb239f5d33faf217a860b170e2763b67c2f18d91c41272de37ac67386f80d1de57a3d58ddf2@10.1.17.4:30300"
        ]
      }
    },
    "valSetContract": {
      "$id": "/properties/valSetContract",
      "type": "string",
      "title": "The ValidatorSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\n\nimport \"./SafeMath.sol\";\nimport \"./Utils.sol\";\n\ncontract ValidatorSet …"
      ]
    },
    "adminContract": {
      "$id": "/properties/adminContract",
      "type": "string",
      "title": "The AdminSet Contract Source",
      "default": "",
      "examples": [
        "pragma solidity 0.4.21;\nimport \"./SafeMath.sol\";\nimport \"./SimpleValidatorSet.sol\";\nimport \"./Admin.sol\";\n\ncontract AdminValidatorSet is SimpleValidatorSet { …"
      ]
    },
    "adminContractABI": {
      "$id": "/properties/adminContractABI",
      "type": "string",
      "title": "The Admin Contract ABI",
      "default": "",
      "examples": [
        "[{\"constant\":false,\"inputs\":[{\"name\":\"proposedAdminAddress\",\"type\":\"address\"},…"
      ]
    },
    "paritySpec": {
      "$id": "/properties/paritySpec",
      "type": "string",
      "title": "The Parity client spec file",
      "default": "",
      "examples": [
        "\n{\n \"name\": \"PoA\",\n \"engine\": {\n \"authorityRound\": {\n \"params\": {\n \"stepDuration\": \"2\",\n \"validators\" : {\n \"safeContract\": \"0x0000000000000000000000000000000000000006\"\n },\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\"\n }\n }\n },\n \"params\": {\n \"gasLimitBoundDivisor\": \"0x400\",\n \"maximumExtraDataSize\": \"0x2A\",\n \"minGasLimit\": \"0x2FAF080\",\n \"networkID\" : \"0x9a2112\",\n \"wasmActivationTransition\": \"0x0\"\n },\n \"genesis\": {\n \"seal\": {\n \"authorityRound\": {\n \"step\": \"0x0\",\n \"signature\": \"0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000\"\n }\n },\n \"difficulty\": \"0x20000\",\n \"gasLimit\": \"0x2FAF080\"\n },\n \"accounts\": {\n \"0x0000000000000000000000000000000000000001\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ecrecover\", \"pricing\": { \"linear\": { \"base\": 3000, \"word\": 0 } } } },\n \"0x0000000000000000000000000000000000000002\": { \"balance\": \"1\", \"builtin\": { \"name\": \"sha256\", \"pricing\": { \"linear\": { \"base\": 60, \"word\": 12 } } } },\n \"0x0000000000000000000000000000000000000003\": { \"balance\": \"1\", \"builtin\": { \"name\": \"ripemd160\", \"pricing\": { \"linear\": { \"base\": 600, \"word\": 120 } } } },\n \"0x0000000000000000000000000000000000000004\": { \"balance\": \"1\", \"builtin\": { \"name\": \"identity\", \"pricing\": { \"linear\": { \"base\": 15, \"word\": 3 } } } },\n \"0x0000000000000000000000000000000000000006\": { \"balance\": \"0\", \"constructor\" : \"…\" }\n }\n}"
      ]
    },
    "errorMessage": {
      "$id": "/properties/errorMessage",
      "type": "string",
      "title": "Error message",
      "default": "",
      "examples": [
        ""
      ]
    },
    "addressList": {
      "$id": "/properties/addressList",
      "type": "object",
      "properties": {
        "addresses": {
          "$id": "/properties/addressList/properties/addresses",
          "type": "array",
          "items": {
            "$id": "/properties/addressList/properties/addresses/items",
            "type": "string",
            "title": "This member’s validator addresses",
            "default": "",
            "examples": [
              "0x00a3cff0dccc0ecb6ae0461045e0e467cff4805f",
              "0x009ce13a7b2532cbd89b2d28cecd75f7cc8c0727"
            ]
          }
        }
      }
    }
  }
}

```

## <a name="governance-dapp"></a>Yönetim DApp

Otoritenin kanıtının kalbinde ademi merkeziyetçi yönetim yer almaktadır. Yetki kanıtı, ağı sağlıklı tutmak için izin verilen ağ yetkilileri listesine dayandığından, bu izin listesinde değişiklik yapmak için adil bir mekanizma sağlamak önemlidir. Her dağıtım, bu izin verilen listenin zincirli yönetimi için bir dizi akıllı sözleşme ve portalla birlikte gelir. Önerilen değişiklik konsorsiyum üyeleri tarafından çoğunluk oyu yla gerçekleştiğinde, değişiklik yürürlüğe konur. Oylama, yeni konsensüs katılımcılarının dürüst bir ağı teşvik eden şeffaf bir şekilde eklenmesine veya tehlikeye atılarak katılımcıların çıkarılmasına olanak tanır.

Yönetim DApp önceden dağıtılmış akıllı [sözleşmeler](https://github.com/Azure-Samples/blockchain/tree/master/ledger/template/ethereum-on-azure/permissioning-contracts) ve ağ üzerinde yetkilileri yönetmek için kullanılan bir web uygulaması bir dizi. Yetkililer yönetici kimlikleri ve doğrulayıcı düğümler ayrılmıştır.
Yöneticiler, bir dizi doğrulayıcı düğüme oybirliği yle katılım sağlama yetkisine sahiptir. Yöneticiler ayrıca diğer yöneticileri ağa veya ağa oy verebilir.

![Yönetim DApp](./media/ethereum-poa-deployment/governance-dapp.png)

* **Ademi Merkeziyetçi Yönetim:** Ağ yetkililerindeki değişiklikler, belirli yöneticiler tarafından zincirleme oylama yoluyla yönetilir.
* **Validator Delegasyonu:** Yetkililer, her PoA dağıtımında ayarlanan geçerli düğümlerini yönetebilir.
* **Denetlenebilir Değişiklik Geçmişi:** Her değişiklik şeffaflık ve denetim sağlayan blockchain kaydedilir.

### <a name="getting-started-with-governance"></a>Yönetime başlamak

Governance DApp üzerinden her türlü işlemi gerçekleştirmek için bir Ethereum cüzdan kullanmanız gerekir. En basit yaklaşım [MetaMask](https://metamask.io)gibi tarayıcı içi cüzdan kullanmaktır; ancak, bu akıllı sözleşmeler ağda dağıtıldığı için, Yönetim sözleşmesiyle etkileşimlerinizi de otomatikleştirebilirsiniz.

MetaMask'i yükledikten sonra tarayıcıdaki Governance DApp'e gidin.  Dağıtım çıktısında URL'yi Azure portalı üzerinden bulabilirsiniz.  Tarayıcı içi cüzdanınız yüklü değilse, herhangi bir işlem gerçekleştiremezsinuz; ancak, yönetici durumunu görüntüleyebilirsiniz.  

### <a name="becoming-an-admin"></a>Yönetici olmak

Ağda dağıtılan ilk üyeyseniz, otomatik olarak yönetici olursunuz ve eşlik düğümleriniz doğrulayıcı olarak listelenir. Ağa katılıyorsanız, çoğunluk tarafından yönetici olarak oy almanız gerekir (%50'den büyük). varolan yönetici kümesinin. Yönetici olmamayı seçerseniz, düğümleriniz blockchain'i eşitler ve doğrular; ancak, blok oluşturma işlemine katılmaz. Yönetici olmak için oylama işlemini başlatmak **için, Aday'ı** seçin ve Ethereum adresinizi ve takma adını girin.

![Aday gösterin](./media/ethereum-poa-deployment/governance-dapp-nominate.png)

### <a name="candidates"></a>Aday

**Adaylar** sekmesini seçmek, geçerli aday yönetici kümesini gösterir.  Bir aday mevcut yöneticiler tarafından çoğunluk oyu ulaştığında, aday yöneticiliğe terfi eder.  Bir adayı oylamak için satırı seçin ve **'de Oy verin'i**seçin. Bir oylamada fikrinizi değiştirirseniz, adayı seçin ve **Rescind oy**seçin.

![Aday](./media/ethereum-poa-deployment/governance-dapp-candidates.png)

### <a name="admins"></a>Yöneticiler

**Yöneticiler** sekmesi geçerli yönetici kümesini gösterir ve size alenen oy kullanma olanağı sağlar.  Bir yönetici %50'den fazla desteği kaybettiğinde, ağda yönetici olarak kaldırılır. Yöneticinin sahip olduğu tüm geçerli düğümler geçerlilik durumunu kaybeder ve ağdaki işlem düğümleri haline gelir. Bir yönetici herhangi bir sayıda nedenden dolayı kaldırılabilir; ancak, önceden bir politika üzerinde anlaşma konsorsiyuma kalmış.

![Yöneticiler](./media/ethereum-poa-deployment/governance-dapp-admins.png)

### <a name="validators"></a>Doğrulayıcıları

**Geçerlilik sekmesini** seçmek, örneğin geçerli dağıtılan parite düğümlerini ve geçerli durumlarını (Düğüm türü) görüntüler. Bu görünüm, geçerli dağıtılan konsorsiyum üyesini temsil ettiği için, her konsorsiyum üyesinin bu listede farklı bir doğrulayıcı kümesi vardır. Örnek yeni dağıtıldıysa ve doğrulayıcılarınızı eklemediyseniz, **Geçerlileyici Ekle**seçeneğini elde elabilirsiniz. Doğrulayıcılar eklemek, bölgesel olarak dengeli bir parite düğümleri kümesini otomatik olarak seçer ve bunları doğrulayıcı kümenize atar. İzin verilen kapasiteden daha fazla düğüm dağıttıysanız, kalan düğümler ağda işlem düğümleri olur.

Her bir doğrulayıcının adresi Otomatik olarak Azure'daki [kimlik deposu](#identity-store) üzerinden atanır.  Bir düğüm aşağı iner, kimliğinden vazgeçerek dağıtımınızdaki başka bir düğümün yerini almasına izin verir. Bu süreç, konsensüs katılımınızın yüksek oranda kullanılabilir olmasını sağlar.

![Doğrulayıcıları](./media/ethereum-poa-deployment/governance-dapp-validators.png)

### <a name="consortium-name"></a>Konsorsiyum adı

Herhangi bir yönetici konsorsiyum adını güncelleyebilir.  Konsorsiyum adını güncellemek için sol üstteki vites simgesini seçin.

### <a name="account-menu"></a>Hesap menüsü

Sağ üstte, Ethereum hesap takma adınız ve identicon'uniz var.  Yöneticiyseniz, takma adınızı güncelleştirme olanağınız olur.

![Hesap](./media/ethereum-poa-deployment/governance-dapp-account.png)

## <a name="ethereum-development"></a>Ethereum geliştirme<a id="tutorials"></a>

Akıllı sözleşmeleri derlemek, dağıtmak ve test etmek için, Ethereum geliştirme için göz önünde bulundurabileceğiniz birkaç seçenek şunlardır:
* [Trüf Süiti](https://www.trufflesuite.com/docs/truffle/overview) - Müşteri tabanlı Ethereum geliştirme ortamı
* [Ethereum Remix](https://remix-ide.readthedocs.io/en/latest/index.html ) - Tarayıcı tabanlı ve yerel Ethereum geliştirme ortamı

### <a name="compile-deploy-and-execute-smart-contract"></a>Akıllı sözleşmeyi derleme, dağıtma ve yürütme

Aşağıdaki örnekte, basit bir akıllı sözleşme oluşturursunuz. Akıllı sözleşmeyi blockchain ağınıza derlemek ve dağıtmak için Truffle'ı kullanırsınız. Dağıtıldıktan sonra, bir işlem aracılığıyla akıllı bir sözleşme işlevi çağırırsınız.

#### <a name="prerequisites"></a>Ön koşullar

* [Python 2.7.15'i yükleyin.](https://www.python.org/downloads/release/python-2715/) Python Truffle ve Web3 için gereklidir. Python'u yolunuza eklemek için yükleme seçeneğini seçin.
* Truffle v5.0.5 `npm install -g truffle@v5.0.5`yükleyin. Truffle [Node.js](https://nodejs.org)dahil olmak üzere yüklü olmak üzere çeşitli araçlar gerektirir , [Git](https://git-scm.com/). Daha fazla bilgi için [Trüf belgelerine](https://github.com/trufflesuite/truffle)bakın.

### <a name="create-truffle-project"></a>Trüf mantarı projesi oluşturun

Akıllı bir sözleşme derlemek ve dağıtmak için önce bir Trüf projesi oluşturmanız gerekir.

1. Komut istemini veya kabuğu açın.
1. `HelloWorld` adlı bir klasör oluşturun.
1. Dizinyeni `HelloWorld` klasörü değiştirin.
1. Komutunu `truffle init`kullanarak yeni bir Truffle projesini başlatma.

    ![Yeni bir Trüf projesi oluşturun](./media/ethereum-poa-deployment/create-truffle-project.png)

### <a name="add-a-smart-contract"></a>Akıllı bir sözleşme ekleme

Truffle projenizin **sözleşme** alt dizininde akıllı sözleşmelerinizi oluşturun.

1. Trüf projenizin `postBox.sol` **sözleşme** alt dizininde adı geçen bir dosya oluşturun.
1. **postBox.sol'a**aşağıdaki Katılık kodunu ekleyin.

    ```javascript
    pragma solidity ^0.5.0;
    
    contract postBox {
        string message;
        function postMsg(string memory text) public {
            message = text;
        }
        function getMsg() public view returns (string memory) {
            return message;
        }
    }
    ```

### <a name="deploy-smart-contract-using-truffle"></a>Trüf mantarı kullanarak akıllı sözleşme dağıtın

Trüf projeleri blockchain ağ bağlantısı ayrıntıları için bir yapılandırma dosyası içerir. Yapılandırma dosyasını ağınızın bağlantı bilgilerini içerecek şekilde değiştirin.

> [!WARNING]
> Ethereum özel anahtarınızı asla ağ üzerinden göndermeyin. Her hareketin önce yerel olarak imzalandığından ve imzalanan hareketin ağ üzerinden gönderildiğinden emin olun.

1. [Blockchain ağınızı dağıtırken kullanılan Ethereum yönetici hesabı](#ethereum-settings)için mnemonik ifadeye ihtiyacınız vardır. Hesabı oluşturmak için MetaMask'i kullandıysanız, metamask'dan mnemonic'i alabilirsiniz. MetaMask uzantının sağ üst kısmındaki yönetici hesap simgesini seçin ve **Tohum Sözcükleri'ni Ortaya Çıkarın**> Güvenlik & Gizlilik ayarları >'ı seçin.
1. Truffle `truffle-config.js` projenizdeki içeriği aşağıdaki içerikle değiştirin. Yer tutucu bitiş noktasını ve mnemonik değerleri değiştirin.

    ```javascript
    const HDWalletProvider = require("truffle-hdwallet-provider");
    const rpc_endpoint = "<Ethereum RPC endpoint>";
    const mnemonic = "Twelve words you can find in MetaMask > Security & Privacy > Reveal Seed Words";

    module.exports = {
      networks: {
        development: {
          host: "localhost",
          port: 8545,
          network_id: "*" // Match any network id
        },
        poa: {
          provider: new HDWalletProvider(mnemonic, rpc_endpoint),
          network_id: 10101010,
          gasPrice : 0
        }
      }
    };
    ```

1. Truffle HD Cüzdan sağlayıcısını kullandığımız için, modülü projenizdeki `npm install truffle-hdwallet-provider --save`komutu kullanarak yükleyin.

Trüf mantarı, bir blockchain ağına akıllı sözleşmeler dağıtmak için geçiş komut dosyalarını kullanır. Yeni akıllı sözleşmenizi dağıtmak için bir geçiş komut dosyasına ihtiyacınız var.

1. Yeni sözleşmeyi dağıtmak için yeni bir geçiş ekleyin. Trüf `2_deploy_contracts.js` projesinin **geçişler** alt dizininde dosya oluşturun.

    ``` javascript
    var postBox = artifacts.require("postBox");
    
    module.exports = deployer => {
        deployer.deploy(postBox);
    };
    ```

1. Trüf geçiş komutunu kullanarak PoA ağına dağıtın. Truffle proje dizinindeki komut isteminde çalıştırın:

    ```javascript
    truffle migrate --network poa
    ```

### <a name="call-a-smart-contract-function"></a>Akıllı bir sözleşme işlevi arama

Akıllı sözleşmeniz dağıtıldığına göre, bir işlevi çağırmak için bir hareket gönderebilirsiniz.

1. Trüf proje dizininde, '. `sendtransaction.js`
1. **sendtransaction.js**için aşağıdaki içeriği ekleyin.

    ``` javascript
    var postBox = artifacts.require("postBox");
    
    module.exports = function(done) {
      console.log("Getting the deployed version of the postBox smart contract")
      postBox.deployed().then(function(instance) {
        console.log("Calling postMsg function for contract ", instance.address);
        return instance.postMsg("Hello, blockchain!");
      }).then(function(result) {
        console.log("Transaction hash: ", result.tx);
        console.log("Request complete");
        done();
      }).catch(function(e) {
        console.log(e);
        done();
      });
    };
    ```

1. Truffle execute komutunu kullanarak komut dosyasını çalıştırın.

    ```javascript
    truffle exec sendtransaction.js --network poa
    ```

    ![İşlem yoluyla işlevi çağırmak için komut dosyasını çalıştırma](./media/ethereum-poa-deployment/send-transaction.png)

## <a name="webassembly-wasm-support"></a>WebAssembly (WASM) desteği

Yeni dağıtılan PoA ağlarında WebAssembly desteği zaten sizin için etkinleştirildi. Web-Assembly'e (Pas, C, C++) aktarılan herhangi bir dilde akıllı sözleşme geliştirmeolanağı sağlar. Daha fazla bilgi için bkz: [Parity Tech'ten WebAssembly](https://wiki.parity.io/WebAssembly-Home) ve [Öğretici'ye](https://github.com/paritytech/pwasm-tutorial) Eşlik Genel Bakış

## <a name="faq"></a>SSS

### <a name="i-notice-there-are-many-transactions-on-the-network-that-i-didnt-send-where-are-these-coming-from"></a>Ağda göndermediğim birçok işlem olduğunu fark ettim. Bunlar nereden geliyor?

[Bu kişisel API](https://web3js.readthedocs.io/en/v1.2.0/web3-eth-personal.html)kilidini güvensiz. Botlar kilidi Ethereum hesapları için dinlemek ve fonları boşaltmak için girişimi. Bot bu hesapların gerçek eter içerdiğini varsayar ve bakiyeyi hortumlayailk eforlamaya çalışır. Ağdaki kişisel API'yi etkinleştirme. Bunun yerine metamask gibi bir cüzdan kullanarak veya programlı olarak işlemleri el ile imzalayın.

### <a name="how-to-ssh-onto-a-vm"></a>VM üzerine SSH nasıl?

SSH bağlantı noktası güvenlik nedeniyle açıkta değil. [SSH bağlantı noktasını etkinleştirmek için bu kılavuzu](#ssh-access)izleyin.

### <a name="how-do-i-set-up-an-audit-member-or-transaction-nodes"></a>Denetim üyesi veya işlem düğümlerini nasıl ayarlıyorum?

İşlem düğümleri, ağa bakan ancak konsensüse katılmayan bir parite istemcileri kümesidir. Bu düğümler hala Ethereum işlemleri göndermek ve akıllı sözleşme durumunu okumak için kullanılabilir. Bu mekanizma, ağdaki yetkili olmayan konsorsiyum üyelerine denetlenebilirlik sağlamak için çalışır. Bunu başarmak için, [Konsorsiyum Büyüyen](#growing-the-consortium)adımları izleyin.

### <a name="why-are-metamask-transactions-taking-a-long-time"></a>MetaMask işlemleri neden uzun sürüyor?

Hareketlerin doğru sırada alınmasını sağlamak için, her Ethereum hareketi bir artış nonce ile birlikte gelir. MetaMask'te farklı bir ağda bir hesap kullandıysanız, nonce değerini sıfırlamanız gerekir. Ayarlar simgesine (üç çubuklu), Ayarlar,Hesabı Sıfırla'ya tıklayın. Hareket geçmişi temizlenir ve artık hareketi yeniden gönderebilirsiniz.

### <a name="do-i-need-to-specify-gas-fee-in-metamask"></a>MetaMask'te gaz ücreti belirtmem gerekiyor mu?

Eter otorite kanıtı konsorsiyumunda bir amaca hizmet etmez. Bu nedenle, MetaMask'de işlem gönderirken gaz ücreti belirtmenize gerek yoktur.

### <a name="what-should-i-do-if-my-deployment-fails-due-to-failure-to-provision-azure-oms"></a>Azure OMS'nin sağlanamaması nedeniyle dağıtımım başarısız olursa ne yapmalıyım?

İzleme isteğe bağlı bir özelliktir. Azure Monitor kaynağını başarıyla sağlayamadığı için dağıtımınızın başarısız olduğu bazı nadir durumlarda, Azure Monitor olmadan yeniden dağıtabilirsiniz.

### <a name="are-public-ip-deployments-compatible-with-private-network-deployments"></a>Genel IP dağıtımları özel ağ dağıtımlarıyla uyumlu mu?

Hayır. Tüm ağın herkese açık veya özel olması için iki yönlü iletişim gerekir.

### <a name="what-is-the-expected-transaction-throughput-of-proof-of-authority"></a>Yetki Belgesi'nin beklenen işlem çıktısı nedir?

İşlem inişmi, işlem türlerine ve ağ topolojisi'ne son derece bağlıdır. Basit işlemleri kullanarak, birden fazla bölgede dağıtılan bir ağla saniyede ortalama 400 işlem gerçekleştirdik.

### <a name="how-do-i-subscribe-to-smart-contract-events"></a>Akıllı sözleşme etkinliklerine nasıl abone olurum?

Ethereum Proof-of-Authority artık web yuvalarını destekliyor.  Web soketi URL'sini ve bağlantı noktasını bulmak için dağıtım çıktınızı kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla Azure Blockchain çözümü için [Azure Blockchain belgelerine](https://docs.microsoft.com/azure/blockchain/)bakın.
