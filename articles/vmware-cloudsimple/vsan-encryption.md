---
title: CloudSimple tarafından Azure VMware Çözümü - Özel Bulut için vSAN şifrelemesi yapılandırın
description: CloudSimple Private Cloud'unuzun Azure sanal ağınızda çalışan bir anahtar yönetim sunucusuyla çalışabilmesi için vSAN yazılım şifreleme özelliğini nasıl yapılandırabileceğinizi açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 638b60bd3612fa25350ecef0a738fea75c2f53d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77020650"
---
# <a name="configure-vsan-encryption-for-cloudsimple-private-cloud"></a>CloudSimple Private Cloud için vSAN şifrelemeyi yapılandırma

CloudSimple Private Cloud'unuzun Azure sanal ağınızda çalışan bir anahtar yönetim sunucusuyla çalışabilmesi için vSAN yazılım şifreleme özelliğini yapılandırabilirsiniz.

VMware, vSAN şifrelemesi kullanırken harici bir KMIP 1.1 uyumlu üçüncü taraf anahtar yönetim sunucusu (KMS) aracının kullanılmasını gerektirir. VMware tarafından onaylanan ve Azure için kullanılabilen desteklenen KMS'lerden yararlanabilirsiniz.

Bu kılavuzda, Bir Azure sanal ağında çalışan HyTrust KeyControl KMS'nin nasıl kullanılacağı açıklanmaktadır. Benzer bir yaklaşım vSAN için diğer sertifikalı üçüncü taraf KMS çözümü için kullanılabilir.

Bu KMS çözümü şunları gerektirir:

* Azure sanal ağınızdaki VMware sertifikalı bir üçüncü taraf KMS aracını yükleyin, yapılandırın ve yönetin.
* KMS aracı için kendi lisanslarınızı sağlayın.
* Azure sanal ağınızda çalışan üçüncü taraf KMS aracını kullanarak Özel Bulut'unuzda vSAN şifrelemesini yapılandırın ve yönetin.

## <a name="kms-deployment-scenario"></a>KMS dağıtım senaryosu

KMS sunucu kümesi Azure sanal ağınızda çalışır ve yapılandırılan Azure ExpressRoute bağlantısı üzerinden Özel Bulut vCenter'dan IP'ye erişilebilir.

![.. Azure sanal ağında /media/KMS kümesi](media/vsan-kms-cluster.png)

## <a name="how-to-deploy-the-solution"></a>Çözüm nasıl dağıtılır?

Dağıtım işlemi aşağıdaki adımları vardır:

1. [Ön koşulların karşılandığını doğrulayın](#verify-prerequisites-are-met)
2. [CloudSimple portalı: ExpressRoute Peering Information edinin](#cloudsimple-portal-obtain-expressroute-peering-information)
3. [Azure portalı: Sanal ağınızı Özel Bulut'a bağlayın](#azure-portal-connect-your-virtual-network-to-your-private-cloud)
4. [Azure portalı: Sanal ağınıza bir HyTrust KeyControl Cluster dağıtma](#azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network)
5. [HyTrust WebUI: KMIP sunucusu yapılandırma](#hytrust-webui-configure-the-kmip-server)
6. [vCenter UI: Azure sanal abunuzdaki KMS kümesini kullanacak şekilde vSAN şifrelemesini yapılandırın](#vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network)

### <a name="verify-prerequisites-are-met"></a>Ön koşulların karşılandığını doğrulayın

Dağıtımdan önce aşağıdakileri doğrulayın:

* Seçili KMS satıcısı, aracı ve sürümü vSAN uyumluluk listesindedir.
* Seçili satıcı, aracın Azure'da çalışacak bir sürümünü destekler.
* KMS aracının Azure sürümü KMIP 1.1 uyumludur.
* Azure Kaynak Yöneticisi ve sanal ağ zaten oluşturuldu.
* CloudSimple Private Cloud zaten oluşturuldu.

### <a name="cloudsimple-portal-obtain-expressroute-peering-information"></a>CloudSimple portalı: ExpressRoute eşleme bilgilerini edinin

Kuruluma devam etmek için ExpressRoute için yetkilendirme anahtarı ve eş devresi URI'ye ve Azure Aboneliğinize erişmeye ihtiyacınız vardır. Bu bilgiler CloudSimple portalındaki Sanal Ağ Bağlantısı sayfasında mevcuttur. Yönergeler için [bkz.](virtual-network-connection.md) Bilgileri almakta sorun yaşıyorsanız, bir [destek isteği](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)açın.

### <a name="azure-portal-connect-your-virtual-network-to-your-private-cloud"></a>Azure portalı: Sanal ağınızı Özel Bulut'unuza bağlayın

1. [Azure portalını kullanarak ExpressRoute için sanal ağ ağ ağ geçidini yapılandır'daki](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)yönergeleri izleyerek sanal ağınız için sanal ağ ağ geçidi oluşturun.
2. [Sanal ağınızı portalı kullanarak bir ExpressRoute devresine bağlayarak sanal ağınızı](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)CloudSimple ExpressRoute devresine bağlayın.
3. Sanal ağınızı Azure'daki CloudSimple ExpressRoute devresine bağlamak için CloudSimple'dan gelen karşılama e-postanızda alınan CloudSimple ExpressRoute devre bilgilerini kullanın.
4. Yetkilendirme anahtarını ve eş devresi URI'yi girin, bağlantıya bir ad verin ve **Tamam'ı**tıklatın.

![Sanal ağ oluştururken CS eş devresi URI sağlayın](media/vsan-azureportal01.png) 

### <a name="azure-portal-deploy-a-hytrust-keycontrol-cluster-in-the-azure-resource-manager-in-your-virtual-network"></a>Azure portalı: Sanal ağınızdaki Azure Kaynak Yöneticisi'nde bir HyTrust KeyControl kümesini dağıtma

Sanal ağınızdaki Azure Kaynak Yöneticisi'nde bir HyTrust KeyControl kümesi dağıtmak için aşağıdaki görevleri gerçekleştirin. Ayrıntılar için [HyTrust belgelerine](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0) bakın.

1. HyTrust belgelerindeki yönergeleri izleyerek, belirtilen gelen kuralları içeren bir Azure ağ güvenlik grubu (nsg-hytrust) oluşturun.
2. Azure'da bir SSH anahtar çifti oluşturun.
3. Azure Marketi'ndeki görüntüden ilk KeyControl düğümini dağıtın.  Oluşturulan anahtar çiftinin ortak anahtarını kullanın ve KeyControl düğümü için ağ güvenlik grubu olarak **nsg-hytrust'ı** seçin.
4. KeyControl'in özel IP adresini statik bir IP adresine dönüştürün.
5. Ortak IP adresini ve daha önce bahsedilen anahtar çiftinin özel anahtarını kullanarak KeyControl VM'ye SSH.
6. SSH kabuğunda istendiğinde, `No` düğümü ilk KeyControl düğümü olarak ayarlamayı seçin.
7. Bu yordamın 3-5 adımlarını yineleyerek ve varolan `Yes` bir kümeye ekleme hakkında istendiğinde seçerek ek KeyControl düğümleri ekleyin.

### <a name="hytrust-webui-configure-the-kmip-server"></a>HyTrust WebUI: KMIP sunucusunu yapılandırın

*Public-ip'https://* gidin, *burada public-ip* KeyControl düğümü VM'nin genel IP adresidir. [HyTrust belgelerinden](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0)aşağıdaki adımları izleyin.

1. [KMIP sunucusu yapılandırma](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2)
2. [VMware Şifrelemesi için Sertifika Paketi Oluşturma](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3)

### <a name="vcenter-ui-configure-vsan-encryption-to-use-kms-cluster-in-your-azure-virtual-network"></a>vCenter UI: Azure sanal abunuzdaki KMS kümesini kullanacak şekilde vSAN şifrelemesini yapılandırın

[vCenter'da KMS kümesi oluşturmak](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4)için HyTrust yönergelerini izleyin.

![vCenter'da KMS küme ayrıntıları ekleme](media/vsan-config01.png)

vCenter'da **Cluster > Configure'ye** gidin ve vSAN için **Genel** seçeneği seçin. Şifrelemeyi etkinleştirin ve daha önce vCenter'a eklenen KMS kümesini seçin.

![vCENTER'da vSAN şifrelemesini etkinleştirin ve KMS kümesini yapılandırın](media/vsan-config02.png)

## <a name="references"></a>Başvurular

### <a name="azure"></a>Azure

[Azure portalını kullanarak ExpressRoute için sanal ağ ağ geçidini yapılandırma](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md)

[Portalı kullanarak ExpressRoute bağlantı hattına bir sanal ağı bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)

### <a name="hytrust"></a>HyTrust

[HyTrust DataControl ve Microsoft Azure](https://docs.hytrust.com/DataControl/Admin_Guide-4.0/Default.htm#OLH-Files/Azure.htm%3FTocPath%3DHyTrust%2520DataControl%2520and%2520Microsoft%2520Azure%7C_____0)

[KMPI Sunucusuyapılandırma](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/configuring-kmip-server.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____2)

[VMware Şifrelemesi için Sertifika Paketi Oluşturma](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-user-for-vmcrypt.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____3)

[vSphere'de KMS Kümesioluşturma](https://docs.hytrust.com/DataControl/4.2/Admin_Guide-4.2/index.htm#Books/VMware-vSphere-VSAN-Encryption/creating-KMS-Cluster.htm%3FTocPath%3DHyTrust%2520KeyControl%2520with%2520VSAN%25C2%25A0and%2520VMware%2520vSphere%2520VM%2520Encryption%7C_____4)
