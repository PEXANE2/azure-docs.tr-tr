---
title: Azure ExpressRoute'u Oracle Cloud Altyapısıyla Bağlayın | Microsoft Dokümanlar
description: Azure ExpressRoute'u Oracle Cloud Infrastructure (OCI) FastConnect ile bağlayın ve bulutlar arası Oracle uygulama çözümlerini etkinleştirin
documentationcenter: virtual-machines
author: romitgirdhar
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/02/2019
ms.author: rogirdh
ms.openlocfilehash: 0e2e16ccc04ff6df80597d646a00c40551e4cfd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302058"
---
# <a name="set-up-a-direct-interconnection-between-azure-and-oracle-cloud-infrastructure"></a>Azure ve Oracle Cloud Altyapısı arasında doğrudan bağlantı kurma  

Entegre bir [çoklu bulut deneyimi](oracle-oci-overview.md) (önizleme) oluşturmak için Microsoft ve Oracle, [ExpressRoute](../../../expressroute/expressroute-introduction.md) ve [FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm)üzerinden Azure ve Oracle Bulut Altyapısı (OCI) arasında doğrudan bağlantı sağlar. ExpressRoute ve FastConnect ara bağlantısı sayesinde müşteriler, iki bulut arasında düşük gecikme, yüksek verim ve özel doğrudan bağlantı yaşayabilir.

> [!IMPORTANT]
> Microsoft Azure ve OCI arasındaki bağlantı önizleme aşamasındadır. Azure ve OCI arasında düşük gecikme sonu bağlantısı oluşturmak için, bu özellik için öncelikle Azure aboneliğinizin etkinleştirilmesi gerekir. Bu kısa [anket formunu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyzVVsi364tClw522rL9tkpUMVFGVVFWRlhMNUlRQTVWSTEzT0dXMlRUTyQlQCN0PWcu)doldurarak önizlemeye kaydolmalısınız. Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız. Bir onay e-postası alana kadar bu özelliği kullanamazsınız. Bu önizleme için etkinleştirilmek üzere Microsoft temsilcinize de başvurabilirsiniz. Önizleme özelliğine erişim, kullanılabilirlik durumuna bağlıdır ve microsoft tarafından kendi takdirine bağlı olarak kısıtlanır. Anketin tamamlanması erişimi garanti etmez. Bu önizleme, hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için Microsoft Azure Önizlemeleri için [Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

Aşağıdaki resim, ara bağlantının üst düzey bir genel görünümünü gösterir:

![Bulutlar arası ağ bağlantısı](media/configure-azure-oci-networking/azure-oci-connect.png)

## <a name="prerequisites"></a>Ön koşullar

* Azure ve OCI arasında bağlantı kurmak için etkin bir Azure aboneliğiniz ve etkin bir OCI kiranız olması gerekir.

* Bağlantı, yalnızca Bir Azure ExpressRoute bakış konumu OCI FastConnect'e yakın veya aynı bakış konumuna sahipolduğunda mümkündür. [Bkz. Bölge Kullanılabilirliği](oracle-oci-overview.md#region-availability).

* Bu önizleme özelliği için Azure aboneliğinizin etkinleştirilmesi gerekir.

## <a name="configure-direct-connectivity-between-expressroute-and-fastconnect"></a>ExpressRoute ve FastConnect arasında doğrudan bağlantıyı yapılandırma

1. Bir kaynak grubu altında Azure aboneliğinizde standart bir ExpressRoute devresi oluşturun. 
    * ExpressRoute'u oluştururken, hizmet sağlayıcısı olarak **Oracle Cloud FastConnect'i** seçin. ExpressRoute devresi oluşturmak için [adım adım kılavuzuna](../../../expressroute/expressroute-howto-circuit-portal-resource-manager.md)bakın.
    * Azure ExpressRoute devresi parçalı bant genişliği seçenekleri sağlarken FastConnect 1, 2, 5 veya 10 Gbps'yi destekler. Bu nedenle, ExpressRoute altında bu eşleşen bant genişliği seçeneklerinden birini seçmeniz önerilir.

    ![ExpressRoute devresi oluştur](media/configure-azure-oci-networking/exr-create-new.png)
1. ExpressRoute Service **anahtarınızı**not edin. FastConnect devrenizi yapılandırırken anahtarı sağlamanız gerekir.

    ![ExpressRoute Hizmeti anahtarı](media/configure-azure-oci-networking/exr-service-key.png)

    > [!IMPORTANT]
    > ExpressRoute devresi sağlanır karşılanmaz **(Sağlayıcı Durumu** **Sağlanmamış**olsa bile) ExpressRoute ücretleri için faturalandırılırsınız.

1. Azure sanal ağınızla veya OCI sanal bulut ağı IP Adresi alanınızla örtüşmeyen iki özel IP adresi alanı /30'dan oluşan iki özel IP adresi alanı kazıyın. İlk IP adres alanını birincil adres alanı, ikinci IP adres alanı ise ikincil adres alanı olarak adlandıracağız. FastConnect devrenizi yapılandırırken ihtiyacınız olan adresleri not edin.
1. Dinamik Yönlendirme Ağ Geçidi (DRG) oluşturun. FastConnect devrenizi oluştururken buna ihtiyacınız olacaktır. Daha fazla bilgi için [Dinamik Yönlendirme Ağ Geçidi](https://docs.cloud.oracle.com/iaas/Content/Network/Tasks/managingDRGs.htm) belgelerine bakın.
1. Oracle kiracınızın altında bir FastConnect devresi oluşturun. Daha fazla bilgi için [Oracle belgelerine](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/azure.htm)bakın.
  
    * FastConnect yapılandırması altında, sağlayıcı olarak **Microsoft Azure: ExpressRoute'u** seçin.
    * Önceki adımda sağladığınız Dinamik Yönlendirme Ağ Geçidi'ni seçin.
    * Sağlanacak bant genişliğini seçin. En iyi performans için bant genişliği, ExpressRoute devresini oluştururken seçilen bant genişliğiyle eşleşmelidir.
    * **Sağlayıcı Hizmet Anahtarı'nda**ExpressRoute servis anahtarını yapıştırın.
    * **Birincil BGP IP Adresi** için önceki adımda oyulmuş ilk /30 özel IP adresi alanını ve **İkincil BGP IP** Adresi için ikinci /30 özel IP adres alanını kullanın.
        * Oracle BGP IP Adresi (Birincil ve İkincil) için iki aralığın ilk kullanılabilir adresini ve ikinci adresi müşteri BGP IP Adresine (FastConnect perspektifinden) atayın. İlk kullanılabilir IP adresi /30 adres alanındaki ikinci IP adresidir (ilk IP adresi Microsoft tarafından ayrılmıştır).
    * **Oluştur'u**tıklatın.
1. Route Table'ı kullanarak Dynamic Routing Gateway aracılığıyla Oracle kiracınız altında FastConnect'i sanal bulut ağına bağlamayı tamamlayın.
1. Azure'a gidin ve ExpressRoute devreniziçin **Sağlayıcı Durumunun** **Provisioned** olarak değiştirildiğini ve **Azure özel** türünden bir eşleme nin sağlandığını sağlayın. Bu, aşağıdaki adımlar için bir ön koşuldur.

    ![ExpressRoute sağlayıcı durumu](media/configure-azure-oci-networking/exr-provider-status.png)
1. **Azure özel** eşleme'yi tıklatın. FastConnect devrenizi kurarken girdiğiniz bilgilere göre, eşleme ayrıntılarının otomatik olarak yapılandırıldığını göreceksiniz.

    ![Özel akran ayarları](media/configure-azure-oci-networking/exr-private-peering.png)

## <a name="connect-virtual-network-to-expressroute"></a>Sanal ağı ExpressRoute'a bağlayın

1. Henüz yapmadıysanız, sanal ağ ve sanal ağ ağ geçidi oluşturun. Ayrıntılar için [adım adım kılavuzuna](../../../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)bakın.
1. [Terraform komut dosyasını](https://github.com/microsoft/azure-oracle/tree/master/InterConnect-2) çalıştırarak veya [ExpressRoute FastPath'i Yapılandırmak](../../../expressroute/expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath)için PowerShell komutunu çalıştırarak sanal ağ ağ ağ geçidi ile ExpressRoute devreniz arasındaki bağlantıyı kurun.

Ağ yapılandırmasını tamamladıktan sonra, Azure portalındaki ExpressRoute Özel örnek bıçak altında **ARP Records'u al'a** tıklayarak yapılandırmanızın geçerliliğini doğrulayabilir ve **rota tablosunu alabilirsiniz.**

## <a name="automation"></a>Automation

Microsoft, ağ ara bağlantısının otomatik olarak dağıtılmasını sağlamak için Terraform komut dosyaları oluşturmuştur. Terraform komut dosyalarının yürütmeden önce Azure ile kimlik doğrulaması gerekir, çünkü Azure aboneliğinde yeterli izinlere ihtiyaç duyarlar. Kimlik doğrulama, bir [Azure Active Directory hizmet yöneticisi](../../../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) veya Azure CLI kullanılarak gerçekleştirilebilir. Daha fazla bilgi için [Terraform belgelerine](https://www.terraform.io/docs/providers/azurerm/auth/azure_cli.html)bakın.

Bu [GitHub deposunda,](https://aka.ms/azureociinterconnecttf)inter-connect'i dağıtmak için Terraform komut dosyaları ve ilgili belgeler bulunabilir.

## <a name="monitoring"></a>İzleme

Her iki buluta da aracıyükleme, uçtan uca ağın performansını izlemek için Azure Ağ Performans Monitörü'nden [(NPM)](../../../expressroute/how-to-npm.md) yararlanabilirsiniz. NPM, ağ sorunlarını kolayca belirlemenize ve bunların ortadan kaldırılmasına yardımcı olur.

## <a name="delete-the-interconnect-link"></a>Ara bağlantı bağlantısını silme

Ara bağlantının silinmesi için, verilen belirli sırada aşağıdaki adımların izlenmesi gerekir. Aksi takdirde "başarısız durum" ExpressRoute devresi neden olur.

1. ExpressRoute bağlantısını silin. Bağlantınız için sayfadaki **Sil** simgesine tıklayarak bağlantıyı silin. Daha fazla bilgi için [ExpressRoute belgelerine](../../../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#delete-a-connection-to-unlink-a-vnet)bakın.
1. Oracle Cloud Konsolundan Oracle FastConnect'i silin.
1. Oracle FastConnect devresi silindikten sonra Azure ExpressRoute devresini silebilirsiniz.

Bu noktada, silme ve deprovisioning işlemi tamamlandı.

## <a name="next-steps"></a>Sonraki adımlar

* OCI ve Azure arasındaki bulutlar arası bağlantı hakkında daha fazla bilgi için [Oracle belgelerine](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/azure.htm)bakın.
* Hedeflenen Oracle uygulamaları için Azure üzerinden altyapı dağıtmak ve ağ ara bağlantısını yapılandırmak için [Terraform komut dosyalarını](https://aka.ms/azureociinterconnecttf) kullanın. 
