---
title: Azure 'da Hyperledger Fabric Consortium çözüm şablonu dağıtma
description: Azure 'da Hyperledger Fabric Consortium ağ çözüm şablonunu dağıtma ve yapılandırma
ms.date: 05/09/2019
ms.topic: article
ms.reviewer: caleteet
ms.openlocfilehash: 3e7dcd3cdcfa636c0b23ac6643bd7732e7f8ada0
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76029152"
---
# <a name="hyperledger-fabric-consortium-network"></a>Hiper muhasebe doku Consortium ağı

Azure 'da bir hiper muhasebe doku Consortium ağı dağıtmak ve yapılandırmak için Hyperledger Fabric Consortium çözüm şablonunu kullanabilirsiniz.

> [!IMPORTANT]
> [Azure şablonundaki hiper muhasebe dokusunu](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-azure-blockchain.azure-blockchain-hyperledger-fabric) kullanım dışı olacaktır. Bunun yerine [Azure Kubernetes hizmetinde Hyperledger dokusunu](hyperledger-fabric-consortium-azure-kubernetes-service.md) kullanın.  

Bu makaleyi okuduktan sonra şunları yapabilir olacaksınız:

- Blok zinciri, hiper muhasebe dokusu ve daha karmaşık konsorsiyum ağ mimarilerinin çalışma bilgisini edinin
- Azure portal içinden bir hiper muhasebe doku Consortium ağını dağıtmayı ve yapılandırmayı öğrenin

## <a name="about-blockchain"></a>Blok zinciri hakkında

Blockzincirine yönelik yeni bir topluluk olan bu çözüm şablonu, Azure 'da kolay ve yapılandırılabilir bir şekilde teknoloji hakkında bilgi edinmek için harika bir fırsattır. Blok zinciri, Bitpara arkasındaki temel teknolojidir; Ancak, bir sanal para birimi için yalnızca Etkinleştirici 'tan çok daha fazla. Bu, mevcut veritabanı, Dağıtılmış Sistem ve şifreleme teknolojilerinin bir dağıdır ve bu sayede güvenli çok taraflı hesaplamayı, imlebilirlik, doğrulılığın, auditability ve saldırıya karşı dayanıklı bir şekilde garanti sağlar. Farklı protokoller, bu öznitelikleri sağlamak için farklı mekanizmalar sunar. [Hiper muhasebe dokusunu](https://github.com/hyperledger/fabric) bu tür bir protokoldür.

## <a name="consortium-architecture-on-azure"></a>Azure 'da konsorsiyum mimarisi

Azure 'da hiper muhasebe dokusunu etkinleştirmek için, desteklenen iki birincil dağıtım türü vardır. Bu dağıtımlar, istenen hedefe göre farklı topolojilerle uyum sağlayacak şekilde tasarlanmıştır.

- **Tek sanal makine, geliştirici sunucusu** -bu dağıtım türü, hiper muhasebe dokusunda oluşturulmuş çözümler derlemek ve test etmek için kullanılan bir geliştirme ortamı olarak tasarlanmıştır.
- **Birden çok sanal makine, genişleme dağıtımı** -bu dağıtım türü, paylaşılan bir ortamı kullanan farklı katılımcılar için bir Konsorsiyumu modelleyerek ortamlar için tasarlanmıştır.

Her iki dağıtımda de hiper muhasebe dokusunun çekirdeğini oluşturan yapı taşları aynıdır.  Dağıtımlardaki farklılıklar bu bileşenlerin nasıl ölçeklendiği bir şekilde yapılır.

- **CA düğümleri**: ağdaki kimlikler için kullanılan sertifikalar oluşturmak Için kullanılan sertifika yetkilisini çalıştıran bir düğüm.
- **Orderer düğümleri**: Toplam sipariş yayını veya atomik işlemler gibi bir teslim garantisi uygulayan iletişim hizmetini çalıştıran bir düğüm.
- **Eş düğümler**: işlemleri gerçekleştiren ve dağıtılmış bir genel muhasebe 'nin durumunu ve kopyasını tutan bir düğüm.
- **Couşdb düğümleri**: durum veritabanını tutan ve chaincode verilerinin zengin sorgulanmasına sahip olan couşdb hizmetini çalıştıratabilecek ve basit anahtar/değerden JSON türü depolamaya genişleyen bir düğüm.

### <a name="single-virtual-machine-architecture"></a>Tek sanal makine mimarisi

Daha önce belirtildiği gibi, tek bir sanal makine mimarisi, geliştiricilerin uygulama geliştirmek için kullanılan düşük bir parmak izi sunucusuna sahip olması için oluşturulmuştur. Gösterilen tüm kapsayıcılar tek bir sanal makinede çalışıyor. Sıralama hizmeti bu yapılandırma için [solo](https://github.com/hyperledger/fabric/tree/master/orderer) kullanıyor. Bu yapılandırma, hataya dayanıklı bir sıralama hizmeti *değildir* , ancak geliştirme amacıyla hafif olacak şekilde tasarlanmıştır.

![Tek sanal makine mimarisi](./media/hyperledger-fabric-consortium-blockchain/hlf-single-arch.png)

### <a name="multiple-virtual-machine-architecture"></a>Birden çok sanal makine mimarisi

Birden çok sanal makine, genişleme mimarisi, çekirdek üzerinde her bir bileşen için yüksek kullanılabilirlik ve ölçeklendirme ile oluşturulmuştur. Bu mimari, üretim sınıfı dağıtımları için çok daha uygundur.

![Birden çok sanal makine mimarisi](./media/hyperledger-fabric-consortium-blockchain/hlf-multi-arch.png)

## <a name="getting-started"></a>Başlangıç

Başlamak için birkaç sanal makine ve standart depolama hesabı dağıtımı destekleyebilen bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı oluşturabilirsiniz](https://azure.microsoft.com/free/).

Bir aboneliğiniz olduğunda [Azure Portal](https://portal.azure.com)gidin. **Hyperledger Fabric Consortium > > blok zinciri kaynak oluştur**' u seçin.

![Hiper muhasebe yapısı tek üye blok zinciri Market şablonu](./media/hyperledger-fabric-consortium-blockchain/marketplace-template.png)

## <a name="deployment"></a>Kurulum

**Hiper muhasebe doku Consortium** şablonunda **Oluştur**' u seçin.

Şablon dağıtımı, çok düğümlü [hiper muhasebeye 1,3](https://hyperledger-fabric.readthedocs.io/en/release-1.3/) ağını yapılandırma konusunda size yol gösterecektir. Dağıtım akışı dört adıma bölünmüştür: temel bilgiler, konsorsiyum ağ ayarları, doku yapılandırması ve Isteğe bağlı bileşenler.

### <a name="basics"></a>Temel Bilgiler

**Temel bilgiler**bölümünde, herhangi bir dağıtım için standart parametrelerin değerlerini belirtin. Örneğin, abonelik, kaynak grubu ve temel sanal makine özellikleri.

![Temel Bilgiler](./media/hyperledger-fabric-consortium-blockchain/basics.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Kaynak ön eki** | Dağıtımın bir parçası olarak sağlanan kaynaklar için ad ön eki |6 karakter veya daha az |
**Kullanıcı Adı** | Bu üye için dağıtılan her bir sanal makine için yöneticinin Kullanıcı adı |1-64 karakter |
**Kimlik doğrulama türü** | Sanal makinede kimlik doğrulama yöntemi |Parola veya SSH ortak anahtarı|
**Parola (kimlik doğrulama türü = parola)** |Dağıtılan sanal makinelerin her biri için yönetici hesabının parolası. Parola şu karakter türlerinden üçünü içermelidir: 1 büyük harf, 1 küçük harf, 1 sayı ve 1 özel karakter<br /><br />Tüm VM 'Ler başlangıçta aynı parolaya sahip olsa da, sağlama sonrasında parolayı değiştirebilirsiniz|12-72 karakter|
**SSH anahtarı (kimlik doğrulama türü = SSH ortak anahtarı)** |Uzaktan oturum açma için kullanılan güvenli kabuk anahtarı ||
**Abonelik** |Dağıtım yapılacak abonelik ||
**Kaynak grubu** |Consortium ağının dağıtılacağı kaynak grubu ||
**Konum** |İlk üyenin dağıtılacağı Azure bölgesi ||

**Tamam**’ı seçin.

### <a name="consortium-network-settings"></a>Konsorsiyum ağ ayarları

**Ağ ayarları**' nda, var olan bir konsorsiyum ağı oluşturma veya katılma ve kuruluş ayarlarınızı yapılandırma için giriş belirtin.

![Konsorsiyum ağ ayarları](./media/hyperledger-fabric-consortium-blockchain/network-settings.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Ağ yapılandırması** |Yeni bir ağ oluşturmayı veya var olan bir ağa katılmayı seçebilirsiniz. *Varolanı Birleştir*' i seçerseniz ek değerler sağlamanız gerekir. |Yeni ağ <br/> Mevcut olanı birleştirin |
**HLF CA parolası** |Dağıtım kapsamında oluşturulan sertifika yetkilileri tarafından oluşturulan sertifikalar için kullanılan parola. Parola şu karakter türlerinden üçünü içermelidir: 1 büyük harf, 1 küçük harf, 1 sayı ve 1 özel karakter.<br /><br />Tüm sanal makineler başlangıçta aynı parolaya sahip olsa da, sağlama sonrasında parolayı değiştirebilirsiniz.|1-25 karakter |
**Kuruluş kurulumu** |Kuruluşunuzun adını ve sertifikasını özelleştirebilir veya kullanılacak varsayılan değerlere sahip olabilirsiniz.|Varsayılan <br/> Gelişmiş |
**VPN ağ ayarları** | VM 'Lere erişmek için bir VPN tüneli ağ geçidi sağlama | Evet <br/> Hayır |

**Tamam**’ı seçin.

### <a name="fabric-specific-settings"></a>Yapıya özgü ayarlar

**Doku yapılandırması**' nda Ağ boyutunu ve performansını yapılandırır ve ağın kullanılabilirliği için girişleri belirtirsiniz. Örneğin, numara sırası ve eş düğümleri, her düğüm tarafından kullanılan Kalıcılık altyapısı ve VM boyutu.

![Yapı ayarları](./media/hyperledger-fabric-consortium-blockchain/fabric-specific-settings.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Ölçek türü** |Genişleme modelinde birden çok kapsayıcı veya birden çok sanal makine içeren tek bir sanal makinenin dağıtım türü.|Tek VM veya çoklu VM |
**VM disk türü** |Dağıtılan düğümlerin her birini yedekleyen depolama türü. <br/> Kullanılabilir disk türleri hakkında daha fazla bilgi edinmek için [disk türünü seçin](../../virtual-machines/windows/disks-types.md)' i ziyaret edin.|Standart SSD <br/> Premium SSD |

### <a name="multiple-vm-deployment-additional-settings"></a>Birden çok VM dağıtımı (ek ayarlar)

![Birden çok VM dağıtımı için doku ayarları](./media/hyperledger-fabric-consortium-blockchain/multiple-vm-deployment.png)

| Parametre Adı | Açıklama | İzin verilen değerler |
|---|---|---|
**Sipariş düğümü sayısı** |İşlemleri bir blok halinde sıraya alan (düzenleyen) düğümlerin sayısı. <br />Sipariş hizmeti hakkında daha fazla bilgi için hiper defter [belgelerini](https://hyperledger-fabric.readthedocs.io/en/release-1.1/ordering-service-faq.html) ziyaret edin |1-4 |
**Orderer düğüm sanal makine boyutu** |Ağdaki orderer düğümleri için kullanılan sanal makine boyutu|Standart BS,<br />Standart DS,<br />Standart FS |
**Eş düğüm sayısı** | İşlemleri çalıştıran ve muhasebe 'nin durumunu ve bir kopyasını barındıran konsorsiyum üyelerine ait düğümler.<br />Sipariş hizmeti hakkında daha fazla bilgi için hiper defter [belgelerini](https://hyperledger-fabric.readthedocs.io/en/latest/glossary.html)ziyaret edin.|1-4 |
**Düğüm durumu kalıcılığı** |Eş düğümler tarafından kullanılan Kalıcılık motoru. Bu altyapıyı, eş düğüm başına yapılandırabilirsiniz. Birden çok eş düğüm için aşağıdaki ayrıntılara bakın.|CouchDB <br />LevelDB |
**Eş düğüm sanal makine boyutu** |Ağdaki tüm düğümler için kullanılan sanal makine boyutu|Standart BS,<br />Standart DS,<br />Standart FS |

### <a name="multiple-peer-node-configuration"></a>Birden çok eş düğüm yapılandırması

Bu şablon, eş düğüm başına kalıcı altyapıyı seçmenizi sağlar. Örneğin, üç eş düğümünüz varsa, diğer iki üzerinde de bir ve LevelDB üzerinde Couşdb kullanabilirsiniz.

![Birden çok eş düğüm yapılandırması](./media/hyperledger-fabric-consortium-blockchain/multiple-peer-nodes.png)

**Tamam**’ı seçin.

### <a name="deploy"></a>Kurulum

**Özet**' de, temel dağıtım öncesi doğrulamayı çalıştırmak için ve belirtilen girişleri gözden geçirin.

![Özet](./media/hyperledger-fabric-consortium-blockchain/summary.png)

Yasal ve gizlilik koşullarını gözden geçirin ve dağıtılacak **satın al** ' ı seçin. Sağlanmakta olan sanal makinelerin sayısına bağlı olarak, dağıtım süresi birkaç dakika ile on dakika arasında farklılık gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık hiper muhasebe Consortium blok zinciri ağınıza göre uygulama ve chaincode geliştirmeye odaklanmaya hazırsınız.
