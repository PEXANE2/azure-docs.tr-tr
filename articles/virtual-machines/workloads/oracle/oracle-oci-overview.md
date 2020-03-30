---
title: Microsoft Azure'u Oracle Cloud Altyapısıyla tümleştirin | Microsoft Dokümanlar
description: Microsoft Azure'da çalışan Oracle uygulamalarını Oracle Cloud Infrastructure (OCI) veritabanlarıyla bütünleştiren çözümler hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: romitgirdhar
manager: gwallace
tags: ''
ms.assetid: ''
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/04/2019
ms.author: rogirdh
ms.custom: ''
ms.openlocfilehash: e1249913300be532cc6514f1478bbc6f4183c001
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300562"
---
# <a name="oracle-application-solutions-integrating-microsoft-azure-and-oracle-cloud-infrastructure-preview"></a>Microsoft Azure ve Oracle Bulut Altyapılarını entegre eden Oracle uygulama çözümleri (önizleme)

Microsoft ve Oracle, her iki bulutun da en iyi avantajlarından yararlanmanızı sağlayarak düşük gecikme gecikmesi, yüksek bulut lar arası bağlantı sağlamak için ortaklık kurmuştur. 

Bu bulutlar arası bağlantıyı kullanarak, veritabanı katmanınızı Oracle Cloud Infrastructure'da (OCI) ve Microsoft Azure'daki uygulama ve diğer katmanları çalıştırmak için çok katmanlı bir uygulamayı bölümlere kullanabilirsiniz. Deneyim, tüm çözüm yığınını tek bir bulutta çalıştırmaya benzer. 

> [!IMPORTANT]
> Bu çapraz bulut özelliği şu anda önizlemededir ve [sınırlamalar geçerlidir.](#region-availability) Azure ve OCI arasında düşük gecikme sonu bağlantısı oluşturmak için, bu özellik için öncelikle Azure aboneliğinizin etkinleştirilmesi gerekir. Bu kısa [anket formunu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyzVVsi364tClw522rL9tkpUMVFGVVFWRlhMNUlRQTVWSTEzT0dXMlRUTyQlQCN0PWcu)doldurarak önizlemeye kaydolmalısınız. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız. Bir onay e-postası alana kadar bu özelliği kullanamazsınız. Bu önizleme için etkinleştirilmek üzere Microsoft temsilcinize de başvurabilirsiniz. Önizleme özelliğine erişim, kullanılabilirlik durumuna bağlıdır ve microsoft tarafından kendi takdirine bağlı olarak kısıtlanır. Anketin tamamlanması erişimi garanti etmez. Bu önizleme, hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için Microsoft Azure Önizlemeleri için [Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

Oracle çözümlerini tamamen Azure altyapısına dağıtmak istiyorsanız, [Oracle VM görüntülerine ve bunların Microsoft Azure'da dağıtımına](oracle-vm-solutions.md)bakın.

## <a name="scenario-overview"></a>Senaryoya genel bakış

Çapraz bulut bağlantısı, OCI'da barındırılan veritabanı hizmetlerinin avantajlarından yararlanırken Oracle'ın sektör lideri uygulamalarını ve kendi özel uygulamalarınızı Azure sanal makinelerinde çalıştırmanız için bir çözüm sağlar. 

Bulutlar arası yapılandırmada çalıştırabileceğiniz uygulamalar şunlardır:

* E-İş Paketi
* JD Edwards EnterpriseOne
* Peoplesoft
* Oracle Perakende uygulamaları
* Oracle Hyperion Finansal Yönetimi

Aşağıdaki diyagram, bağlı çözümün üst düzey bir özetidir. Basitlik için diyagram yalnızca bir uygulama katmanı ve veri katmanı nı gösterir. Uygulama mimarisine bağlı olarak, çözümünüz Azure'da web katmanı gibi ek katmanlar içerebilir. Daha fazla bilgi için aşağıdaki bölümlere bakın.

![Azure OCI çözümüne genel bakış](media/oracle-oci-overview/crosscloud.png)

## <a name="region-availability"></a>Bölge Durumu 

Bulutlar arası bağlantı aşağıdaki bölgelerle sınırlıdır:
* Azure Doğu ABD (eastus) & OCI Ashburn (ABD Doğu)
* Azure Uk South (uksouth) & OCI Londra (Uk South)
* Azure Canada Central (canadacentral) & OCI Toronto (Kanada Güneydoğu)
* Azure Batı Avrupa (westeurope) & OCI Amsterdam (Hollanda Kuzeybatı)

## <a name="networking"></a>Ağ Oluşturma

Kurumsal müşteriler genellikle çeşitli iş ve operasyonel nedenlerle iş yüklerini birden çok bulut üzerinde çeşitlendirmeyi ve dağıtmayı seçer. Müşteriler bulut ağlarını internet, IPSec VPN veya şirket içi ağınız üzerinden bulut sağlayıcısının doğrudan bağlantı çözümünü kullanarak çeşitlendirin. Bulut ağlarını birbirine bağlamak zaman, para, tasarım, satın alma, kurulum, test ve işlemlerde önemli yatırımlar gerektirebilir. 

Bu müşteri zorluklarını gidermek için Oracle ve Microsoft entegre bir çoklu bulut deneyimi sağladı. Microsoft Azure'daki bir [ExpressRoute](../../../expressroute/expressroute-introduction.md) devresi ile OCI'daki bir [FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm) devresi bağlanarak çapraz bulut ağı kurulur. Bu bağlantı, Bir Azure ExpressRoute bakış konumu OCI FastConnect'e yakın veya aynı bakış konumuna sahip olduğunda mümkündür. Bu kurulum, bir ara servis sağlayıcısına gerek kalmadan iki bulut arasında güvenli ve hızlı bağlantı sağlar.

ExpressRoute ve FastConnect'i kullanan müşteriler, özel IP adresi alanının çakışması olmaması koşuluyla, Azure'daki sanal ağı OCI'daki sanal bulut ağıyla eşleyebilir. İki ağa bakmak, sanal ağdaki bir kaynağın OCI sanal bulut ağındaki bir kaynağa aynı ağda dır gibi iletişim kurmasına olanak tanır.

## <a name="network-security"></a>Ağ güvenliği

Ağ güvenliği herhangi bir kurumsal uygulamanın önemli bir bileşenidir ve bu çoklu bulut çözümünün merkezinde yer almaktadır. ExpressRoute ve FastConnect üzerinden geçen tüm trafik özel bir ağ üzerinden geçer. Bu yapılandırma, bir Azure sanal ağı ile Oracle sanal bulut ağı arasında güvenli iletişim sağlar. Azure'daki sanal makinelere herkese açık bir IP adresi sağlamanız gerekmez. Benzer şekilde, OCI bir internet ağ geçidi gerekmez. Tüm iletişim makinelerin özel IP adresi üzerinden gerçekleşir.

Ayrıca, OCI sanal bulut ağınızda ve güvenlik kurallarınızda (Azure [ağ güvenlik gruplarına](../../../virtual-network/security-overview.md)bağlı) [güvenlik listeleri](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/securitylists.htm) ayarlayabilirsiniz. Sanal ağlardaki makineler arasında akan trafiği denetlemek için bu kuralları kullanın. Ağ güvenliği kuralları makine düzeyinde, alt ağ düzeyinde ve sanal ağ düzeyinde eklenebilir.
 
## <a name="identity"></a>Kimlik

Kimlik, Microsoft ve Oracle arasındaki ortaklığın temel direklerinden biridir. [Oracle Identity Cloud Service](https://docs.oracle.com/en/cloud/paas/identity-cloud/index.html) 'i (IDCS) [Azure Etkin Dizini](../../../active-directory/index.yml) (Azure AD) ile tümleştirmek için önemli çalışmalar yapıldı. Azure AD, Microsoft'un bulut tabanlı kimlik ve erişim yönetimi hizmetidir. Kullanıcılarınızın oturum açmasını ve çeşitli kaynaklara erişmesine yardımcı olur. Azure AD ayrıca kullanıcılarınızı ve onların izinlerini yönetmenize de olanak tanır.

Şu anda bu tümleştirme, Azure Etkin Dizini olan tek bir merkezi konumda yönetmenize olanak tanır. Azure AD, dizindeki değişiklikleri ilgili Oracle diziniyle senkronize eder ve tek oturum açma ve çapraz bulut Oracle çözümleri için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

Azure ve OCI arasında bir [çapraz bulut ağı](configure-azure-oci-networking.md) yla başlayın. 

OCI hakkında daha fazla bilgi ve teknik incelemeler için [Oracle Cloud](https://docs.cloud.oracle.com/iaas/Content/home.htm) belgelerine bakın.
