---
title: Azure 'da Ethereum yetki kanıtlama Konsorsiyumu çözüm şablonu dağıtma
description: Azure 'da çok siteli bir konsorsiyumum ağını dağıtmak ve yapılandırmak için Ethereum yetkili bir Konsorsiyumu çözümünü kullanın
ms.date: 06/04/2020
ms.topic: how-to
ms.reviewer: ravastra
ms.openlocfilehash: 127aa860fe0c80f4d12a373c00ad2f53447c3497
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85210125"
---
# <a name="deploy-ethereum-proof-of-authority-consortium-solution-template-on-azure"></a>Azure 'da Ethereum yetki kanıtlama Konsorsiyumu çözüm şablonu dağıtma

Çok sayıda Azure ve Ethereum bilgisine sahip çok üyeli bir konsorsiyum yetkilisi olan bir ağ dağıtımı, yapılandırmak ve yönetmek için [Ethereum-Authority Konsorsiyumu önizleme Azure çözüm şablonunu](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-blockchain.azure-blockchain-ethereum) kullanabilirsiniz.

Çözüm şablonu, Microsoft Azure işlem, ağ ve depolama hizmetleri kullanılarak bir blok zinciri ağı kaplama sağlamak için her bir konsorsiyum üyesi tarafından kullanılabilir. Her bir konsorsiyumun ağ izi, bir uygulamanın veya kullanıcının Ethereum işlemlerini göndermek için etkileşime girebileceği yük dengeli bir doğrulayıcı düğümleri kümesinden oluşur.

## <a name="choose-an-azure-blockchain-solution"></a>Bir Azure blok zinciri çözümü seçin

Ethereum yetki kanıtlama Konsorsiyumu çözüm şablonunu kullanmayı seçmeden önce, senaryonuzu, kullanılabilir Azure blok zinciri seçeneklerinin yaygın kullanım durumları ile karşılaştırın.

Seçenek | Hizmet modeli | Yaygın kullanım durumu
-------|---------------|-----------------
Çözüm şablonları | IaaS | Çözüm şablonları, tam olarak yapılandırılmış bir blok zinciri ağ topolojisi sağlamak için kullanabileceğiniz Azure Resource Manager şablonlardır. Şablonlar, belirli bir blok zinciri ağ türü için Microsoft Azure işlem, ağ ve depolama hizmetleri dağıtır ve yapılandırır.
[Azure Blok Zinciri Hizmeti](../service/overview.md) | PaaS | Azure blok zinciri hizmeti önizlemesi, konsorsiyum blok zinciri ağlarının yönetimini, yönetimini ve yönetimini basitleştirir. PaaS, konsorsiyum yönetimi veya sözleşme ve işlem gizliliği gerektiren çözümler için Azure blok zinciri hizmetini kullanın.
[Azure Blockchain Workbench](../workbench/overview.md) | IaaS ve PaaS | Azure blok zinciri çalışma ekranı önizlemesi, iş süreçlerini ve verileri diğer kuruluşlarla paylaşmak üzere blok zinciri uygulamaları oluşturmanıza ve dağıtmanıza yardımcı olmak üzere tasarlanmış bir Azure hizmetleri ve özellikleri koleksiyonudur. Bir blok zinciri çözümünü veya blok zinciri uygulaması kavram kanıtı 'nı prototip için Azure blok zinciri çalışma ekranı 'nı kullanın.

## <a name="solution-architecture"></a>Çözüm mimarisi

Ethereum çözüm şablonunu kullanarak, tek veya çok bölgeli bir çok üyeli bir adet-üye kanıtı Konsorsiyumu ağını dağıtabilirsiniz.

![dağıtım mimarisi](./media/ethereum-poa-deployment/deployment-architecture.png)

Her bir konsorsiyum üye dağıtımı şunları içerir:

* PoA Doğrulayıcıları çalıştırmaya yönelik sanal makineler
* RPC, eşleme ve idare DApp isteklerini dağıtmaya yönelik Azure Load Balancer
* Doğrulayıcı kimliklerini güvenli hale getirmek için Azure Key Vault
* Kalıcı ağ bilgilerini barındırmak ve kiralamayı koordine etmek için Azure depolama
* Günlük ve performans istatistikleri toplamak için Azure Izleyici
* Özel VNET 'lerde VPN bağlantılarına izin vermek için sanal ağ geçidi (isteğe bağlı)

Varsayılan olarak, RPC ve eşleme uç noktalarına, abonelikler ve bulutlar arasında basitleştirilmiş bağlantı sağlamak için genel IP üzerinden erişilebilir. Uygulama düzeyi erişim denetimleri için, [eşlik 'nın izinleri sözleşmeleri](https://wiki.parity.io/Permissioning)' ni kullanabilirsiniz. Platformlar arası bağlantı için VNet ağ geçitlerinden yararlanan VPN arkasında dağıtılan ağlar desteklenir. VPN ve VNet dağıtımları daha karmaşık olduğundan, bir çözümü prototip oluştururken bir genel IP modeliyle başlamak isteyebilirsiniz.

Docker Kapsayıcıları güvenilirlik ve modülarfor için kullanılır. Azure Container Registry, her dağıtımın parçası olarak sürümlü görüntüleri barındırmak ve barındırmak için kullanılır. Kapsayıcı görüntüleri aşağıdakilerden oluşur:

* Orchestrator-kimlikler ve idare sözleşmeleri oluşturur. Kimlikleri bir kimlik deposunda depolar.
* Eşlik istemcisi-kimlik deposundan kimlik kiralamaları. Eşleri bulur ve eşlere bağlanır.
* Etistats Aracısı-RPC aracılığıyla yerel günlükleri ve istatistikleri toplar ve bilgileri Azure Izleyici 'ye iter.
* İdare sözleşmeleri ile etkileşim kurmak için idare DApp-Web arabirimi.

### <a name="validator-nodes"></a>Doğrulayıcı düğümleri

Yetki kanıtlama protokolünde, doğrulayıcı düğümleri geleneksel Miner düğümlerinin yerini alır. Her doğrulayıcının, blok oluşturma işlemine katılmasına izin veren benzersiz bir Ethereum kimliği vardır. Her konsorsiyum üyesi, coğrafi yedeklilik için beş bölge genelinde iki veya daha fazla Doğrulayıcı düğümü sağlayabilir. Doğrulayıcı düğümleri, temel alınan dağıtılmış defter durumunda konsensus 'a gelmesi için diğer Doğrulayıcı düğümleriyle iletişim kurar. Ağa yönelik dengeli bir katılım sağlamak için, her bir konsorsiyumun, ağdaki ilk Üyeden daha fazla Doğrulayıcıları kullanmasını yasaktır. Örneğin, ilk üye üç Doğrulayıcı dağıttığında, her üyenin en fazla üç doğrulayıcısı olabilir.

### <a name="identity-store"></a>Kimlik deposu

Oluşturulan Ethereum kimliklerini güvenli bir şekilde tutan her üyenin aboneliğine bir kimlik deposu dağıtılır. Her Doğrulayıcı için, düzenleme kapsayıcısı bir Ethereum özel anahtarı oluşturur ve Azure Key Vault depolar.

## <a name="deploy-ethereum-consortium-network"></a>Ethereum Consortium ağını dağıtma

Bu kılavuzda, çok taraflı bir Ethereum Konsorsiyumu ağı oluşturduğunuzu varsayalım. Aşağıdaki akış, çok taraflı bir dağıtıma örnektir:

1. Üç üye, her biri MetaMask kullanarak bir Ethereum hesabı oluşturur
1. *A üyesi bir* Ethereum POA dağıtır ve bu kişilerin Ethereum ortak adresini sağlar
1. *A üyesi* , *B üyesine* ve *C üyesine* yönelik Consortium URL 'sini sağlar
1. *Üye B* ve *üye C* dağıtımı, Ethereum POA, Ethereum ortak adresi ve *ÜYESI A*'nın Konsorsiyumu URL 'si
1. *B üyesi B* 'de yönetici olarak *bir oy üyesi*
1. Üye *A* ve *üye B* üye *C* 'yi yönetici olarak Oylar

Sonraki bölümlerde, ağdaki ilk üyenin parmak izini nasıl yapılandıracağınız gösterilmektedir.

### <a name="create-resource"></a>Kaynak oluşturma

[Azure Portal](https://portal.azure.com)sol üst köşedeki **kaynak oluştur** ' u seçin.

**Blok zinciri**  >  **Ethereum yetki kanıtlama Konsorsiyumu (Önizleme)** öğesini seçin.

### <a name="basics"></a>Temel Bilgiler

**Temel bilgiler**altında herhangi bir dağıtım için standart parametrelerin değerlerini belirtin.

![Temel Bilgiler](./media/ethereum-poa-deployment/basic-blade.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Yeni bir ağ oluşturun veya var olan ağa katılırsanız | Yeni bir konsorsiyum ağı oluşturabilir veya önceden var olan bir konsorsiyum ağına katılabilir. Var olan bir ağa katılmak için ek parametreler gerekir. | Yeni oluştur
E-posta Adresi | Dağıtımınız hakkında bilgi ile dağıtım tamamlandığında bir e-posta bildirimi alırsınız. | Geçerli bir e-posta adresi
VM Kullanıcı adı | Dağıtılan her VM 'nin Yönetici Kullanıcı adı | 1-64 alfasayısal karakter
Kimlik doğrulaması türü | Sanal makinede kimlik doğrulama yöntemi. | Parola
Parola | Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Tüm VM 'Ler başlangıçta aynı parolaya sahiptir. Sağlama sonrasında parolayı değiştirebilirsiniz. | 12-72 karakter 
Abonelik | Consortium ağının dağıtılacağı abonelik |
Kaynak Grubu| Consortium ağının dağıtılacağı kaynak grubu. | myResourceGroup
Konum | Kaynak grubu için Azure bölgesi. | Batı ABD 2

**Tamam**’ı seçin.

### <a name="deployment-regions"></a>Dağıtım bölgeleri

*Dağıtım bölgeleri*' nin altında, her biri için bölgelerin ve konumların sayısını belirtin. En fazla beş bölgede dağıtım yapabilirsiniz. İlk bölge, *temel bilgiler* bölümünden kaynak grubu konumuyla eşleşmelidir. Geliştirme veya test ağları için, her üye için tek bir bölge kullanabilirsiniz. Üretim için, yüksek kullanılabilirlik için iki veya daha fazla bölgeye dağıtın.

![dağıtım bölgeleri](./media/ethereum-poa-deployment/deployment-regions.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Bölge sayısı|Konsorsiyum ağını dağıtmak için bölge sayısı| 2
İlk bölge | Consortium ağını dağıtmaya yönelik ilk bölge | Batı ABD 2
İkinci bölge | Konsorsiyum ağını dağıtmak için ikinci bölge. Bölge sayısı iki veya daha büyükse ek bölgeler görülebilir. | Doğu ABD 2

**Tamam**’ı seçin.

### <a name="network-size-and-performance"></a>Ağ boyutu ve performansı

*Ağ boyutu ve performans*altında, konsorsiyumun ağ boyutu için giriş belirtin. Doğrulayıcı düğümü depolama boyutu, blok zincirinin olası boyutunu belirler. Boyut dağıtımdan sonra değiştirilebilir.

![Ağ boyutu ve performansı](./media/ethereum-poa-deployment/network-size-and-performance.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Yük dengeli Doğrulayıcı düğümlerinin sayısı | Ağın bir parçası olarak sağlanacak Doğrulayıcı düğümlerinin sayısı. | 2
Doğrulayıcı düğümü depolama performansı | Dağıtılan Doğrulayıcı düğümlerinin her biri için yönetilen disk türü. Fiyatlandırma hakkında ayrıntılı bilgi için bkz. [Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/) | Standart SSD
Doğrulayıcı düğümü sanal makine boyutu | Doğrulayıcı düğümleri için kullanılan sanal makine boyutu. Fiyatlandırma hakkında ayrıntılı bilgi için bkz. [sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) | Standart D2 v3

Sanal makine ve depolama katmanı, ağ performansını etkiler.  Maliyet verimliliği ' nı seçmenize yardımcı olması için aşağıdaki tabloyu kullanın:

Sanal makine SKU 'SU|Depolama katmanı|Fiyat|Aktarım hızı|Gecikme süresi
---|---|---|---|---
F1|Standart SSD|zayıf|zayıf|yüksek
D2_v3|Standart SSD|orta|orta|orta
F16s|Premium SSD|yüksek|yüksek|zayıf

**Tamam**’ı seçin.

### <a name="ethereum-settings"></a>Ethereum ayarları

*Ethereum ayarları*altında, Ethereum ile ilgili yapılandırma ayarlarını belirtin.

![Ethereum ayarları](./media/ethereum-poa-deployment/ethereum-settings.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
Konsorsiyum üye KIMLIĞI | Konsorsiyum ağına katılan her üyeyle ilişkili KIMLIK. Çarpışmadan kaçınmak için IP adresi alanlarını yapılandırmak için kullanılır. Özel ağ için, üye KIMLIĞI aynı ağdaki farklı kuruluşlarda benzersiz olmalıdır.  Aynı kuruluş birden çok bölgeye dağıttığında bile benzersiz bir üye KIMLIĞI gereklidir. Bu parametrenin değerini, çakışma olmadığından emin olmak için diğer birleştirme üyeleriyle paylaşmanız gerektiğinden yapın. Geçerli Aralık 0 ile 255 arasındadır. | 0
Ağ KIMLIĞI | Dağıtmakta olan konsorsiyumum ağının ağ KIMLIĞI. Her bir Ethereum ağının kendi ağ KIMLIĞI vardır ve bu, genel ağın KIMLIĞI olan 1 ' dir. Geçerli Aralık 5 ile 999.999.999 arasındadır | 10101010
Yönetici Ethereum adresi | PoA idaresinde katılım için kullanılan Ethereum hesap adresi. Ethereum adresini oluşturmak için MetaMask kullanabilirsiniz. |
Gelişmiş Seçenekler | Ethereum ayarları için Gelişmiş Seçenekler | Etkinleştir
Genel IP kullanarak dağıtma | Özel VNet seçilirse, ağ bir VNet ağ geçidinin arkasında dağıtılır ve eşleme erişimi kaldırılır. Özel VNet için, bağlantının uyumlu olması için tüm üyelerin VNet Gateway kullanması gerekir. | Genel IP
Blok gaz sınırı | Ağın başlangıç blok gaz sınırı. | 50000000
Yeniden mühürlemek süresi (sn) | Ağ üzerinde işlem olmadığında boş blokların oluşturulma sıklığı. Daha yüksek bir sıklık daha hızlı ve daha fazla depolama maliyetine sahip olacaktır. | 15
İşlem Izin sözleşmesi | Işlem Izinleri sözleşmesi için bayt kodu. Akıllı sözleşme dağıtımını ve yürütmeyi, izin verilen Ethereum hesapları listesine kısıtlar. |

**Tamam**’ı seçin.

### <a name="monitoring"></a>İzleme

İzleme, ağınız için bir günlük kaynağı yapılandırmanıza olanak tanır. İzleme Aracısı, ağ durumu veya hata ayıklama sorunlarını hızla denetleyebilme olanağı sunan, ağınızdaki yararlı ölçümleri ve günlükleri toplar ve bunları yüzey halinde sunar.

![Azure izleyici](./media/ethereum-poa-deployment/azure-monitor.png)

Parametre | Açıklama | Örnek değer
----------|-------------|--------------
İzleme | İzlemeyi etkinleştirme seçeneği | Etkinleştir
Mevcut Azure Izleyici günlüklerine bağlanma | Yeni bir Azure Izleyici günlükleri örneği oluşturma veya var olan bir örneğe katma seçeneği | Yeni oluştur
Konum | Yeni örneğin dağıtıldığı bölge | Doğu ABD
Mevcut Log Analytics çalışma alanı KIMLIĞI (mevcut Azure Izleyici günlüklerine Bağlan = var olanı Birleştir)|Mevcut Azure Izleyici günlükleri örneğinin çalışma alanı KIMLIĞI||NA
Mevcut Log Analytics birincil anahtarı (mevcut Azure Izleyici günlüklerine bağlanma = var olanı Birleştir)|Mevcut Azure Izleyici günlükleri örneğine bağlanmak için kullanılan birincil anahtar||NA

**Tamam**’ı seçin.

### <a name="summary"></a>Özet

Belirtilen girişleri gözden geçirmek ve temel dağıtım öncesi doğrulamayı çalıştırmak için Özet ' e tıklayın. Dağıtılmadan önce şablonu ve parametreleri indirebilirsiniz.

Dağıtılacak **Oluştur** ' u seçin.

Dağıtım VNet ağ geçitleri içeriyorsa, dağıtım 50 45 dakika sürebilir.

## <a name="deployment-output"></a>Dağıtım çıkışı

Dağıtım tamamlandıktan sonra, Azure portal kullanarak gerekli parametrelere erişebilirsiniz.

### <a name="confirmation-email"></a>Onay e-postası

Bir e-posta adresi ([temel bilgiler bölümü](#basics)) sağlarsanız, dağıtım bilgilerini ve bu belgelerin bağlantılarını içeren bir e-posta gönderilir.

![Dağıtım e-postası](./media/ethereum-poa-deployment/deployment-email.png)

### <a name="portal"></a>Portal

Dağıtım başarıyla tamamlandıktan ve tüm kaynaklar sağlandıktan sonra, çıkış parametrelerini kaynak grubunuzda görüntüleyebilirsiniz.

1. Portalda kaynak grubunuza gidin.
1. **> dağıtımlarını genel bakış ' ı**seçin.

    ![Kaynak grubuna genel bakış](./media/ethereum-poa-deployment/resource-group-overview.png)

1. **Microsoft-Azure-blockzinciri. Azure-blockzincirini-Ether-...** dağıtımı ' nı seçin.
1. **Çıktılar** bölümünü seçin.

    ![Dağıtım çıkışları](./media/ethereum-poa-deployment/deployment-outputs.png)

## <a name="growing-the-consortium"></a>Konsorsiyumun büyümesi

Konsorsiyumu genişletmek için önce fiziksel ağı bağlamanız gerekir. VPN 'nin arkasında dağıtım yapıyorsanız, [sanal ağ geçidini bağlama](#connecting-vnet-gateways) bölümüne, yeni üye dağıtımının bir parçası olarak ağ bağlantısını yapılandırma konusuna bakın. Dağıtımınız tamamlandıktan sonra, bir ağ yöneticisi olmak için [Idare DApp](#governance-dapp) 'yi kullanın.

### <a name="new-member-deployment"></a>Yeni üye dağıtımı

Aşağıdaki bilgileri katılım üyesiyle paylaşabilirsiniz. Bilgiler, dağıtım sonrası e-postanız veya Portal dağıtım çıkışında bulunur.

* Konsorsiyum verileri URL 'SI
* Dağıttığınız düğümlerin sayısı
* VNet ağ geçidi kaynak KIMLIĞI (VPN kullanılıyorsa)

Dağıtım üyesi, ağ iletişim bilgilerini aşağıdaki kılavuzdan dağıtmada aynı Ethereum yetki kanıtlama Konsorsiyumu çözüm şablonunu kullanmalıdır:

* **Varolanı katılmayı** seçin
* Dengeli gösterimi sağlamak için ağdaki diğer üyelerin geri kalanı ile aynı sayıda Doğrulayıcı düğümünü seçin
* Aynı yönetici Ethereum adresini kullan
* *Ethereum ayarlarında* belirtilen *konsorsiyum veri URL 'sini* kullanın
* Ağın geri kalanı VPN 'in arkasındaysa Gelişmiş bölüm altında **özel VNET** ' i seçin.

### <a name="connecting-vnet-gateways"></a>VNet ağ geçitlerini bağlama

Bu bölüm yalnızca özel bir sanal ağ kullanarak dağıttıysanız gereklidir. Genel IP adresleri kullanıyorsanız, bu bölümü atlayabilirsiniz.

Özel ağ için, farklı Üyeler VNet Gateway bağlantıları aracılığıyla bağlanır. Bir üyenin ağa katılabilmesi ve işlem trafiğini görmeniz için, mevcut bir üyenin VPN ağ geçidinde son bir yapılandırmayı bağlantıyı kabul etmesi gerekir. Birleştirme üyesinin Ethereum düğümleri bağlantı kurulana kadar çalışmaz. Tek bir hata noktası olasılığını azaltmak için, konsorsiyumda yedek ağ bağlantıları oluşturun.

Yeni üye dağıtıldıktan sonra, mevcut üye yeni üyeye VNet ağ geçidi bağlantısı ayarlayarak iki yönlü bağlantıyı tamamlamalıdır. Mevcut üye şunları gerektirir:

* Bağlanan üyenin VNet Gateway RESOURCEID. Bkz. [dağıtım çıktısı](#deployment-output).
* Paylaşılan bağlantı anahtarı.

Mevcut üyenin bağlantıyı tamamlaması için aşağıdaki PowerShell betiğini çalıştırması gerekir. Portalda sağ üst gezinti çubuğunda bulunan Azure Cloud Shell kullanabilirsiniz.

![Cloud Shell](./media/ethereum-poa-deployment/cloud-shell.png)

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

Dağıtım e-postasında bağlantıyı izleyerek veya [OMS_PORTAL_URL] dağıtım çıkışında parametresini bularak Azure Izleyici portalınızı bulabilirsiniz.

Portal önce üst düzey ağ istatistiklerini ve düğüme genel bakış görüntülenir.

![İzleme kategorileri](./media/ethereum-poa-deployment/monitor-categories.png)

**Düğüm genel bakış ' ı** seçmek, düğüm başına altyapı istatistiklerini gösterir.

![Düğüm istatistikleri](./media/ethereum-poa-deployment/node-stats.png)

**Ağ istatistiği** seçildiğinde, Ethereum ağ istatistikleri görüntülenir.

![Ağ İstatistikleri](./media/ethereum-poa-deployment/network-stats.png)

### <a name="sample-kusto-queries"></a>Örnek kusto sorguları

Sorunları araştırmak için izleme günlüklerini sorgulayabilir veya kurulum eşiğini uyarma Uyarısı ' nı kullanabilirsiniz. Aşağıdaki sorgular *günlük arama* aracında çalıştırabileceğiniz örneklerdir:

Birden fazla Doğrulayıcı sorgusu tarafından raporlanan liste blokları, zincir çatalları bulmaya yardımcı olmak için yararlı olabilir.

```sql
MinedBlock_CL
| summarize DistinctMiners = dcount(BlockMiner_s) by BlockNumber_d, BlockMiner_s
| where DistinctMiners > 1
```

5 dakikalık demetlerden ortalama olarak belirtilen bir doğrulayıcı düğümü için Ortalama eşdüzey sayı alın.

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

Güvenlik nedenleriyle, SSH bağlantı noktası erişimi varsayılan olarak bir ağ grubu güvenlik kuralı tarafından reddedilir. PoA ağındaki sanal makine örneklerine erişmek için aşağıdaki güvenlik kuralını *izin*verilecek şekilde değiştirmeniz gerekir.

1. Azure portal dağıtılan kaynak grubunun **genel bakış** bölümüne gidin.

    ![SSH genel bakış](./media/ethereum-poa-deployment/ssh-overview.png)

1. Erişmek istediğiniz VM 'nin bölgesi için **ağ güvenlik grubunu** seçin.

    ![SSH NSG](./media/ethereum-poa-deployment/ssh-nsg.png)

1. **Allow-SSH** kuralını seçin.

    ![SSH-izin ver](./media/ethereum-poa-deployment/ssh-allow.png)

1. **Eylemi** **izin ver** olarak değiştir

    ![SSH etkinleştirme izin ver](./media/ethereum-poa-deployment/ssh-enable-allow.png)

1. **Kaydet**’i seçin. Değişikliklerin uygulanması birkaç dakika sürebilir.

Belirtilen Yönetici Kullanıcı adı ve parola/SSH anahtarı ile SSH aracılığıyla Doğrulayıcı düğümlerine yönelik sanal makinelere uzaktan bağlanabilirsiniz. İlk Doğrulayıcı düğümüne erişmek için SSH komutu, şablon dağıtım çıktısında listelenir. Örneğin:

``` bash
ssh -p 4000 poaadmin\@leader4vb.eastus.cloudapp.azure.com.
```

Ek işlem düğümlerine ulaşmak için, bağlantı noktası numarasını bir artırın.

Birden fazla bölgeye dağıttıysanız, komutu bu bölgedeki yük dengeleyicinin DNS adı veya IP adresi olarak değiştirin. Diğer bölgelerin DNS adını veya IP adresini bulmak için, adlandırma kuralı ** \* \* \* \* \* -lbpıp-reg \# ** adlı kaynağı bulun ve DNS adını ve IP adresi özelliklerini görüntüleyin.

## <a name="azure-traffic-manager-load-balancing"></a>Azure Traffic Manager yük dengelemesi

Azure Traffic Manager, gelen trafiği farklı bölgelerde birden fazla dağıtımda yönlendirerek, kapalı kalma süresini azaltabilir ve ağ yanıt hızını artırır. Yerleşik sistem durumu denetimleri ve otomatik yeniden yönlendirme, RPC uç noktalarının ve Idare DApp 'nin yüksek oranda kullanılabilir olmasını sağlamaya yardımcı olur. Bu özellik, birden fazla bölgeye dağıttığınız ve üretime hazırsanız faydalıdır.

Otomatik yük devretme ile PoA ağ kullanılabilirliğini geliştirmek için Traffic Manager kullanın. Son kullanıcıları en düşük ağ gecikmesi ile Azure konumuna yönlendirerek ağlarınızın yanıt verebilirliğini artırmak için Traffic Manager de kullanabilirsiniz.

Bir Traffic Manager profili oluşturmaya karar verirseniz, ağınıza erişmek için profilin DNS adını kullanabilirsiniz. Ağa diğer konsorsiyum üyeleri eklendikten sonra, dağıtılan doğrulayıcıların yükünü dengelemek için de Traffic Manager kullanılabilir.

### <a name="creating-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

1. [Azure Portal](https://portal.azure.com)sol üst köşedeki **kaynak oluştur** ' u seçin.
1. **Traffic Manager profili**araması yapın.

    ![Azure Traffic Manager ara](./media/ethereum-poa-deployment/traffic-manager-search.png)

    Profile benzersiz bir ad verin ve PoA dağıtımı için kullanılan kaynak grubunu seçin.

1. Dağıtılacak **Oluştur** ' u seçin.

    ![Traffic Manager oluştur](./media/ethereum-poa-deployment/traffic-manager-create.png)

1. Dağıtıldıktan sonra kaynak grubundaki örneği seçin. Traffic Manager 'a erişim için DNS adı genel bakış sekmesinde bulunabilir.

    ![Traffic Manager DNS 'yi bulma](./media/ethereum-poa-deployment/traffic-manager-dns.png)

1. **Uç noktalar** sekmesini seçin ve **Ekle** düğmesini seçin.
1. Uç noktaya benzersiz bir ad verin.
1. **Hedef kaynak türü**IÇIN **genel IP adresi**' ni seçin.
1. İlk bölgenin yük dengeleyicinin genel IP adresini seçin.

    ![Yönlendirme trafiği Yöneticisi](./media/ethereum-poa-deployment/traffic-manager-routing.png)

Dağıtılan ağdaki her bölge için tekrarlayın. Uç noktalar **etkin** duruma getirildikten sonra otomatik olarak yüklenir ve trafık yöneticisinin DNS adına bölge dengelemesi yapılır. Artık bu DNS adını makalenin diğer adımlarında [CONSORTIUM_DATA_URL] parametresi yerine kullanabilirsiniz.

## <a name="data-api"></a>Veri API 'SI

Her bir konsorsiyum üyesi başkalarının ağa bağlanması için gerekli bilgileri barındırır. Bağlantı kolaylığı sağlamak için, her üye veri API uç noktasında bir bağlantı bilgileri kümesi barındırır.

Mevcut üye, üyenin dağıtımından önce [CONSORTIUM_DATA_URL] sağlar. Dağıtım sonrasında, bir katılım üyesi JSON arabiriminden aşağıdaki uç noktada bilgi alacaktır:

`<CONSORTIUM_DATA_URL>/networkinfo`

Yanıt, üyeleri (Genessıs bloğu, doğrulayıcı kümesi sözleşmesi ABı, bootnodes) ve mevcut üye için yararlı olan bilgileri (Doğrulayıcı adresleri) birleştirmek için yararlı bilgiler içerir. Bu standartlaştırma 'yi bulut sağlayıcıları genelinde konsorsiyunuzu genişletmek için kullanabilirsiniz. Bu API aşağıdaki yapıyla JSON biçimli bir yanıt döndürür:

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

## <a name="governance-dapp"></a>İdare DApp

Yetki kanıtlama kalbi, yönetim açısından idare edilir. Yetki kanıtı, ağın sağlıklı kalmasını sağlamak için izin verilen bir ağ yetkilileri listesine bağlı olduğundan, bu izin listesinde değişiklik yapmak için bir yetkili mekanizma sağlanması önemlidir. Her dağıtım, izin verilen bu listenin zinciri için bir dizi akıllı sözleşme ve Portal ile birlikte sunulur. Önerilen bir değişiklik, konsorsiyum üyelerinden oluşan bir büyük oyuna ulaştığında, değişiklik yapılır. Oylama, yeni yarışma katılımcılarının eklenmesine veya tehlikeye giren bir ağı teşvik eden bir saydam şekilde kaldırılmasına olanak sağlar.

İdare DApp, önceden dağıtılan [akıllı sözleşmeleri](https://github.com/Azure-Samples/blockchain/tree/master/ledger/template/ethereum-on-azure/permissioning-contracts) ve ağdaki yetkilileri yönetmek için kullanılan bir Web uygulamasını bir kümesidir. Yetkililer, yönetici kimliklerine ve doğrulayıcı düğümlerine bölünmüştür.
Yöneticiler, konsensus katılımını bir doğrulayıcı düğümleri kümesine devretmek için güce sahiptir. Yöneticiler ayrıca diğer yöneticileri ağ içine veya dışına da oylayabilir.

![İdare DApp](./media/ethereum-poa-deployment/governance-dapp.png)

* Merkezi olmayan **idare:** Ağ yetkililerinde yapılan değişiklikler, select yöneticileri tarafından zincir üzerinde oylama aracılığıyla yönetilir.
* **Doğrulayıcı temsili:** Yetkililer, her bir bir dağıtımda ayarlanan Doğrulayıcı düğümlerini yönetebilir.
* **Denetlenebilir değişiklik geçmişi:** Her değişiklik, saydam ve seslebilirlik sağlayan blok zincirinde kaydedilir.

### <a name="getting-started-with-governance"></a>İdare ile çalışmaya başlama

Idare DApp aracılığıyla herhangi bir türde işlem gerçekleştirmek için bir Ethereum cüzdan kullanmanız gerekir. En kolay yaklaşım, [MetaMask](https://metamask.io)gibi bir tarayıcı içi cüzdan kullanmaktır. Bununla birlikte, bu akıllı sözleşmeler ağ üzerinde dağıtıldığından, Idare sözleşmenize yönelik etkileşimlerinizi de otomatik hale getirebilirsiniz.

MetaMask yükledikten sonra, tarayıcıdaki Idare DApp 'ye gidin.  URL 'YI dağıtım çıkışında Azure portal aracılığıyla bulabilirsiniz.  Tarayıcı içi bir cüzdan yüklü değilse herhangi bir eylem gerçekleştiremezsiniz; Ancak, yönetici durumunu görüntüleyebilirsiniz.  

### <a name="becoming-an-admin"></a>Yönetici olma

Ağa dağıtılan ilk üyesiyse, otomatik olarak yönetici olur ve eşlik düğümleriniz doğrulayıcılar olarak listelenir. Ağa katılırsanız, büyük bir yönetici olarak (%50 ' dan fazla) erişmeniz gerekir var olan yönetici kümesi. Yönetici olmaya devam ederseniz, düğümleriniz blok zincirini eşitler ve doğrular; Ancak, blok oluşturma işlemine katılmaz. Bir yönetici olmak üzere oylama işlemini başlatmak için **aday** ' ı seçin ve Ethereum adresinizi ve diğer adınızı girin.

![Aday gösterin](./media/ethereum-poa-deployment/governance-dapp-nominate.png)

### <a name="candidates"></a>Larınızdan

**Adaylar** sekmesinin seçilmesi, geçerli aday Yöneticiler kümesini gösterir.  Bir aday, geçerli yöneticiler tarafından büyük bir oyuna ulaştığında aday bir yöneticiye yükseltilir.  Bir aday üzerinde oy vermek için satırı seçin ve **oyın**' u seçin. Oyunuzu bir oyda değiştirirseniz, adayı seçin ve **rescind oynı**seçin.

![Larınızdan](./media/ethereum-poa-deployment/governance-dapp-candidates.png)

### <a name="admins"></a>Yöneticiler

**Yöneticiler** sekmesi geçerli yönetici kümesini gösterir ve size bir oy verme özelliği sunar.  Yönetici %50 ' den fazla destek kaybettikten sonra ağ üzerinde yönetici olarak kaldırılır. Yöneticinin sahip olduğu tüm Doğrulayıcı düğümleri, Doğrulayıcı durumunu kaybeder ve ağ üzerinde işlem düğümleri haline gelir. Herhangi bir sayıda nedenden dolayı yönetici kaldırılabilir. Ancak, bir ilkeyi önceden kabul etmek için konsorsiyumun bir üyesi vardır.

![Yöneticiler](./media/ethereum-poa-deployment/governance-dapp-admins.png)

### <a name="validators"></a>Metninin

**Doğrulayıcılar** sekmesini seçtiğinizde, örnek için dağıtılan geçerli eşlik düğümleri ve bunların geçerli durumu (düğüm türü) görüntülenir. Bu görünüm geçerli dağıtılmış Consortium üyesini temsil ettiğinden, her bir konsorsiyumun bu listede farklı bir doğrulayıcılar kümesi vardır. Örnek yeni dağıtılırsa ve Doğrulayıcıları eklemediyseniz, **doğrulayıcılar ekleme**seçeneğini alırsınız. Doğrulayıcılar eklemek, bölgesel olarak dengelenmiş bir eşlik düğümleri kümesini otomatik olarak seçer ve bunları Doğrulayıcı kümesine atar. İzin verilen kapasiteden daha fazla düğüm dağıttıysanız, kalan düğümler ağ üzerinde işlem düğümleri haline gelir.

Her doğrulayıcının adresi, Azure 'daki [kimlik deposu](#identity-store) aracılığıyla otomatik olarak atanır.  Bir düğüm kapanıyorsa, kendi kimliğini yeniden oluşturur, böylece dağıtımınızdaki başka bir düğümün yerini alır. Bu işlem, konsensus katılımınızı yüksek oranda kullanılabilir olmasını sağlar.

![Metninin](./media/ethereum-poa-deployment/governance-dapp-validators.png)

### <a name="consortium-name"></a>Konsorsiyum adı

Herhangi bir yönetici, konsorsiyumun adını güncelleştirebilir.  Konsorsiyumun adını güncelleştirmek için sol üstteki dişli simgesini seçin.

### <a name="account-menu"></a>Hesap menüsü

Sağ üst köşede, Ethereum hesabınızın diğer adı ve identicon.  Yönetici değilseniz, diğer adınızı güncelleştirebilirsiniz.

![Hesap](./media/ethereum-poa-deployment/governance-dapp-account.png)

## <a name="ethereum-development"></a>Ethereum geliştirme<a id="tutorials"></a>

Akıllı sözleşmeleri derlemek, dağıtmak ve test etmek için, Ethereum geliştirmesi için göz önünde bulundurmanız gereken birkaç seçenek vardır:
* [Truffle paketi](https://www.trufflesuite.com/docs/truffle/overview) -Istemci tabanlı Ethereum geliştirme ortamı
* [Ethereum yeniden karışımı](https://remix-ide.readthedocs.io/en/latest/index.html ) -tarayıcı tabanlı ve yerel Ethereum geliştirme ortamı

### <a name="compile-deploy-and-execute-smart-contract"></a>Akıllı sözleşme derleme, dağıtma ve yürütme

Aşağıdaki örnekte basit bir akıllı sözleşme oluşturursunuz. Akıllı sözleşmeyi derlemek ve blok zinciri ağınıza dağıtmak için Truffle kullanırsınız. Dağıtıldıktan sonra bir işlem aracılığıyla akıllı sözleşme işlevini çağırabilirsiniz.

#### <a name="prerequisites"></a>Ön koşullar

* [Python 2.7.15](https://www.python.org/downloads/release/python-2715/)'i yükler. Truffle ve Web3 için Python gereklidir. Yolunuza Python eklemek için Install seçeneğini seçin.
* Truffle v 5.0.5 'i yükler `npm install -g truffle@v5.0.5` . Truffle, [Node.js](https://nodejs.org), [Git](https://git-scm.com/)gibi çeşitli araçların yüklenmesini gerektirir. Daha fazla bilgi için bkz. [Truffle belgeleri](https://github.com/trufflesuite/truffle).

### <a name="create-truffle-project"></a>Truffle projesi oluştur

Akıllı bir sözleşmeyi derleyip dağıtabilmeniz için önce bir truffle projesi oluşturmanız gerekir.

1. Bir komut istemi veya kabuk açın.
1. `HelloWorld` adlı bir klasör oluşturun.
1. Dizini yeni klasör olarak değiştirin `HelloWorld` .
1. Komutunu kullanarak yeni bir truffle projesi başlatın `truffle init` .

    ![Yeni bir truffle projesi oluştur](./media/ethereum-poa-deployment/create-truffle-project.png)

### <a name="add-a-smart-contract"></a>Akıllı sözleşme ekleme

Akıllı sözleşmelerinizi Truffle projenizin **sözleşmeler** alt dizininde oluşturun.

1. `postBox.sol`Truffle projenizin **sözleşmeler** alt dizininde adında bir dosya oluşturun.
1. Aşağıdaki Solidity kodunu **Postbox. Nuevo**öğesine ekleyin.

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

### <a name="deploy-smart-contract-using-truffle"></a>Truffle kullanarak akıllı sözleşme dağıtma

Truffle projeleri blok zinciri ağ bağlantısı ayrıntıları için bir yapılandırma dosyası içerir. Yapılandırma dosyasını, ağınıza yönelik bağlantı bilgilerini içerecek şekilde değiştirin.

> [!WARNING]
> Ethereum özel anahtarınızı hiçbir şekilde ağ üzerinden göndermeyin. Her bir işlemin önce yerel olarak imzalandığından ve imzalı işlemin ağ üzerinden gönderildiğinden emin olun.

1. [Blok zinciri ağınızı dağıtmada kullanılan Ethereum yönetici hesabı](#ethereum-settings)için anımsatıcı ifadesi gerekir. Hesabı oluşturmak için MetaMask kullandıysanız, MetaMask adresinden gelen anımsatıcı alabilirsiniz. MetaMask uzantısının sağ üst köşesindeki yönetici hesabı simgesini seçin ve **ayarlar > güvenlik & gizlilik > tohum sözcüklerini açığa çıkar**' ı seçin.
1. `truffle-config.js`Truffle projenizde içeriğini aşağıdaki içerikle değiştirin. Yer tutucu uç noktasını ve anımsatıcı değerlerini değiştirin.

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

1. Truffle HD cüzdan sağlayıcısını kullandığımızdan, komutunu kullanarak bu modülü projenize yüklersiniz `npm install truffle-hdwallet-provider --save` .

Truffle, bir blok zinciri ağına akıllı sözleşmeleri dağıtmak için geçiş betikleri kullanır. Yeni akıllı sözleşmenizi dağıtmak için bir geçiş betiğinin olması gerekir.

1. Yeni sözleşmeyi dağıtmak için yeni bir geçiş ekleyin. `2_deploy_contracts.js`Truffle projesinin **geçişler** alt dizininde dosya oluşturun.

    ``` javascript
    var postBox = artifacts.require("postBox");
    
    module.exports = deployer => {
        deployer.deploy(postBox);
    };
    ```

1. Truffle Migrate komutunu kullanarak PoA ağına dağıtın. Truffle proje dizinindeki komut isteminde şunu çalıştırın:

    ```javascript
    truffle migrate --network poa
    ```

### <a name="call-a-smart-contract-function"></a>Akıllı sözleşme işlevini çağırma

Akıllı sözleşmeniz dağıtıldığına göre, bir işlevi çağırmak için bir işlem gönderebilirsiniz.

1. Truffle proje dizininde adlı yeni bir dosya oluşturun `sendtransaction.js` .
1. Aşağıdaki içerikleri **sendtransaction.js**ekleyin.

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

1. Truffle Execute komutunu kullanarak betiği yürütün.

    ```javascript
    truffle exec sendtransaction.js --network poa
    ```

    ![İşlem aracılığıyla işlev çağırmak için betiği Yürüt](./media/ethereum-poa-deployment/send-transaction.png)

## <a name="webassembly-wasm-support"></a>WebAssembly (ıSSTREAM) desteği

WebAssembly desteği yeni dağıtılan PoA ağlarında zaten etkin. Web derlemesine (Rust, C, C++) transpiles olan herhangi bir dilde akıllı anlaşma geliştirmesi sağlar. Daha fazla bilgi için bkz.. [Weelsembly 'e eşlik genel bakış](https://wiki.parity.io/WebAssembly-Home) [Tutorial from Parity Tech](https://github.com/paritytech/pwasm-tutorial)

## <a name="faq"></a>SSS

### <a name="i-notice-there-are-many-transactions-on-the-network-that-i-didnt-send-where-are-these-coming-from"></a>Gönderdiğim ağ üzerinde çok sayıda işlem olduğunu fark ediyorum. Bunlar nereden geliyor?

[KIşIsel API](https://web3js.readthedocs.io/en/v1.2.0/web3-eth-personal.html)'nin kilidini açmak güvenli değildir. Botlar, kilitlenmemiş Ethereum hesaplarını dinler ve fonları boşaltmaya çalışır. Bot, bu hesapların gerçek zamanlı olarak olduğunu varsayar ve bakiyedeki ilk olarak bu hesapları yapmaya çalışır. Ağda kişisel API 'yi etkinleştirmeyin. Bunun yerine, MetaMask veya programlı olarak gibi bir cüzdan kullanarak işlemleri önceden imzalayın.

### <a name="how-to-ssh-onto-a-vm"></a>VM üzerinde SSH kullanma

SSH bağlantı noktası güvenlik nedenleriyle gösterilmez. [SSH bağlantı noktasını etkinleştirmek için bu kılavuzu](#ssh-access)izleyin.

### <a name="how-do-i-set-up-an-audit-member-or-transaction-nodes"></a>Nasıl yaparım? bir denetim üyesi veya işlem düğümleri ayarlamak mı istiyorsunuz?

İşlem düğümleri, ağla ilişkilendirilen ancak Consensus 'a katılmayan bir dizi eşlik istemcisi. Bu düğümler, Ethereum işlemlerini göndermek ve akıllı sözleşmenin durumunu okumak için hala kullanılabilir. Bu mekanizma, ağ üzerindeki yetkili olmayan konsorsiyum üyelerine seslebilirlik sağlamak için kullanılır. Bunu başarmak için, [Konsorsiyumu büyümekte](#growing-the-consortium)olan adımları izleyin.

### <a name="why-are-metamask-transactions-taking-a-long-time"></a>Neden MetaMask işlemleri uzun sürüyor?

İşlemlerin doğru sırada alındığından emin olmak için, her bir Ethereum işlemi bir artırma nonce ile gelir. Farklı bir ağda MetaMask içinde bir hesap kullandıysanız, nonce değerini sıfırlamanız gerekir. Ayarlar simgesine (üç çubuk), ayarlar, sıfırlama hesabı ' na tıklayın. İşlem geçmişi silinecek ve artık işlemi yeniden gönderebilirsiniz.

### <a name="do-i-need-to-specify-gas-fee-in-metamask"></a>MetaMask 'te gaz ücreti belirtmem gerekir mi?

Ether, yetkili bir konsorsiyumun bir amacını vermez. Bu nedenle, MetaMask içinde işlem gönderilirken gaz ücreti belirtmeniz gerekmez.

### <a name="what-should-i-do-if-my-deployment-fails-due-to-failure-to-provision-azure-oms"></a>Azure OMS 'yi sağlama hatası nedeniyle dağıtımım başarısız olursa ne yapmam gerekir?

İzleme isteğe bağlı bir özelliktir. Azure Izleyici kaynağını başarıyla sağlayamamasından dolayı dağıtımınızın başarısız olduğu bazı nadir durumlarda Azure Izleyici olmadan yeniden dağıtabilirsiniz.

### <a name="are-public-ip-deployments-compatible-with-private-network-deployments"></a>Özel ağ dağıtımlarıyla uyumlu genel IP dağıtımları mı var?

Hayır. Eşleme iki yönlü iletişim gerektirir, böylece tüm ağ genel veya özel olmalıdır.

### <a name="what-is-the-expected-transaction-throughput-of-proof-of-authority"></a>Yetki kanıtlama için beklenen işlem performansı nedir?

İşlem aktarım hızı, işlem türlerine ve ağ topolojisine bağlı olarak yüksek ölçüde bağımlıdır. Basit işlemler kullanarak, birden çok bölgede dağıtılan bir ağ ile saniye başına ortalama 400 işlem işaretledi.

### <a name="how-do-i-subscribe-to-smart-contract-events"></a>Nasıl yaparım? akıllı sözleşme olaylarına abone misiniz?

Ethereum yetki kanıtı artık Web-Sockets 'i desteklemektedir.  Web yuvası URL 'sini ve bağlantı noktasını bulmak için dağıtım çıktınızdan emin olun.

## <a name="support-and-feedback"></a>Destek ve geri bildirim

Azure blok zinciri haberleri için Azure blok zinciri [blogu](https://azure.microsoft.com/blog/topics/blockchain/) ' nı ziyaret ederek, blok zinciri hizmeti tekliflerini ve Azure blok zinciri Mühendisliği ekibinin bilgilerini güncel tutun.

Ürün geri bildirimi sağlamak veya yeni özellikler istemek için, [blok zinciri Için Azure geri bildirim Forumu](https://aka.ms/blockchainuservoice)aracılığıyla bir fikir gönderin veya oylayın.

### <a name="community-support"></a>Topluluk desteği

Microsoft mühendisleri ve Azure blok zinciri topluluk uzmanlarıyla birlikte katılın.

* [Microsoft Q&Azure blok zinciri hizmeti için soru sayfası](https://docs.microsoft.com/answers/topics/azure-blockchain-workbench.html). Blok zinciri şablonları için mühendislik desteği, dağıtım sorunlarıyla sınırlıdır.
* [Microsoft Teknoloji Topluluğu](https://techcommunity.microsoft.com/t5/Blockchain/bd-p/AzureBlockchain)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-blockchain-workbench)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla Azure blok zinciri çözümü için bkz. [Azure blok zinciri belgeleri](https://docs.microsoft.com/azure/blockchain/).
