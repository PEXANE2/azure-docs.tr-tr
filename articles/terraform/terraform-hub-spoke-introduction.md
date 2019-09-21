---
title: Azure 'da Terrayform ile hub ve bağlı bileşen karma ağ topolojisi oluşturma
description: Terkform kullanarak Azure 'da karma ağ başvuru mimarisinin tamamının nasıl oluşturulacağını gösteren öğretici
services: terraform
ms.service: azure
keywords: terrayform, hub ve bağlı bileşen, ağlar, karma ağlar, DevOps, sanal makine, Azure, VNET eşlemesi, ağ sanal gereci
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 5c2a61dd9da6d233a4b1410042f2125a1c300758
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71173447"
---
# <a name="tutorial-create-a-hub-and-spoke-hybrid-network-topology-with-terraform-in-azure"></a>Öğretici: Azure 'da Terrayform ile hub ve bağlı bileşen karma ağ topolojisi oluşturma

Bu öğretici serisinde, Azure 'da bir [hub ve bağlı ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)Için Terrayform 'un nasıl kullanılacağı gösterilmektedir. 

Hub ve bağlı bileşen topolojisi, ortak hizmetleri paylaşırken iş yüklerini yalıtmak için bir yoldur. Bu hizmetler kimlik ve güvenlik içerir. Hub, şirket içi ağa merkezi bağlantı noktası görevi gören bir sanal ağ (VNet). Uçlar, merkezle eşlenen sanal ağlardır. Paylaşılan hizmetler hub 'a dağıtılır ve tek tek iş yükleri, bağlı olan ağlarda dağıtılır.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hub ve bağlı bileşen karma ağ başvuru mimarisi kaynaklarını düzenlemek için HCL (HashiCorp Language) kullanın
> * Terrayform kullanarak Merkez ağı gereç kaynakları oluşturma
> * Azure 'da tüm kaynaklar için ortak nokta olarak davranacak şekilde Azure 'da Merkez ağı oluşturmak için Terrayform 'u kullanın
> * Azure 'da her iş yükünü bağlı ağ sanal ağları olarak oluşturmak için Terrayform kullanma
> * Şirket içi ve Azure ağları arasında ağ geçitleri ve bağlantılar oluşturmak için Terrayform kullanın
> * Bağlı ağa VNet eşlemeleri oluşturmak için Terrayform kullanma

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Henüz bir Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Terrayform 'U yükleyip yapılandırın**: Azure 'da VM 'Leri ve diğer altyapıyı sağlamak için [Terrayform 'u yükleyip yapılandırın](/azure/virtual-machines/linux/terraform-install-configure)

## <a name="hub-and-spoke-topology-architecture"></a>Hub ve bağlı bileşen topolojisi mimarisi

Hub ve bağlı bileşen topolojisinde, hub bir sanal ağ. VNet, şirket içi ağınıza yönelik merkezi bir bağlantı noktası işlevi görür. Uçlar ise merkezle eşleştirilmiş bir ağdır ve iş yüklerini yalıtmak için kullanılabilir. Şirket içi veri merkezi ile merkez arasındaki trafik akışı bir ExpressRoute veya VPN ağ geçidi bağlantısı üzerinden gerçekleşir. Aşağıdaki görüntüde, bir hub ve bağlı bileşen topolojisinde bileşenler gösterilmektedir:

![Azure 'da hub ve bağlı bileşen topolojisi mimarisi](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-architecture.png)

## <a name="benefits-of-the-hub-and-spoke-topology"></a>Hub ve bağlı bileşen topolojisinin avantajları

Hub ve bağlı bileşen ağ topolojisi, ortak hizmetleri paylaşırken iş yüklerini yalıtmak için bir yoldur. Bu hizmetler kimlik ve güvenlik içerir. Hub, şirket içi ağa merkezi bağlantı noktası görevi gören bir sanal ağ. Uçlar, merkezle eşlenen sanal ağlardır. Paylaşılan hizmetler hub 'a dağıtılır ve tek tek iş yükleri, bağlı olan ağlarda dağıtılır. Hub ve bağlı bileşen ağ topolojisinin bazı avantajları aşağıda verilmiştir:

- Birden çok iş yükü tarafından paylaşılabilen tek bir konumda Hizmetleri merkezileştirerek **maliyet tasarrufu** . Bu iş yükleri, ağ sanal gereçlerini ve DNS sunucularını içerir.
- **Abonelik sınırlarını aşma**: Farklı aboneliklere ait sanal ağlar merkeze eşlenerek sınırlar aşılabilir.
- **İşlerin ayrılması**: Merkezi BT (SecOps, InfraOps) ve iş yüklerini (DevOps) ilgilendiren konular ayrılır.

## <a name="typical-uses-for-the-hub-and-spoke-architecture"></a>Hub ve bağlı bileşen mimarisi için tipik kullanımlar

Hub ve bağlı bileşen mimarisinin bazı yaygın kullanımları şunlardır:

- Birçok müşterinin farklı ortamlarda dağıtılan iş yükleri vardır. Bu ortamlar geliştirme, test ve üretimi içerir. Birçok kez, bu iş yüklerinin DNS, KIMLIKLER, NTP veya AD DS gibi hizmetleri paylaşması gerekir. Bu paylaşılan hizmetler hub VNet 'e yerleştirilebilecek. Bu şekilde, her ortam yalıtımı sürdürmek için bir bağlı bileşene dağıtılır.
- Birbirleriyle bağlantı gerektirmeyen ve paylaşılan hizmetlere erişmesi gereken iş yükleri.
- Güvenlik yönleri üzerinde merkezi denetim gerektiren kuruluşlar.
- Her bir bağlı olan iş yükleri için ayrılmış yönetim gerektiren kuruluşlar.

## <a name="preview-the-demo-components"></a>Tanıtım bileşenlerini önizleyin

Bu serideki her öğreticide çalışırken, farklı bileşenler ayrı Terlarform betiklerinde tanımlanmıştır. Oluşturulan ve dağıtılan tanıtım mimarisi aşağıdaki bileşenlerden oluşur:

- **Şirket içi ağı**. Bir kuruluşla çalışan özel bir yerel alan ağı. Hub ve bağlı bileşen başvuru mimarisi için, Azure 'daki bir sanal ağ, şirket içi bir ağın benzetimini yapmak için kullanılır.

- **VPN cihazı**. VPN cihazı veya hizmeti, şirket içi ağa yönelik dış bağlantı sağlar. VPN cihazı bir donanım gereci veya bir yazılım çözümü olabilir. 

- **Merkezi sanal ağ**. Merkez, şirket içi ağınıza ve ana bilgisayar hizmetlerine yönelik bir yer olan merkezi bağlantı noktasıdır. Bu hizmetler, bağlı olan sanal ağlarda barındırılan farklı iş yükleri tarafından tüketilebilir.

- **Ağ geçidi alt ağı**. VNet ağ geçitleri aynı alt ağda tutulur.

- **Uç sanal ağları**. Uçlar, iş yüklerini diğer uçlardan ayrı olarak yönetilen kendi sanal ağlarında yalıtmak için kullanılabilir. Her iş yükü birden fazla katman içerebilir. Birden fazla alt ağ, Azure yük dengeleyicileri aracılığıyla birbirine bağlanır. 

- **Sanal ağ eşlemesi**. İki sanal ağ, bir eşleme bağlantısı kullanılarak bağlanabilir. Eşleme bağlantıları, sanal ağlar arasındaki geçişsiz ve gecikme süresi düşük bağlantılardır. Eşlendikten sonra, sanal ağlar Azure omurgasını kullanarak bir yönlendirici gerekmeden trafiği Exchange trafiği. Hub ve bağlı ağ topolojisinde VNet eşlemesi, hub 'ı her bir bağlı ağa bağlamak için kullanılır. VNET 'leri aynı bölgede veya farklı bölgelerde eşler.

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturma

Tanıtım için Teraform yapılandırma dosyalarınızı tutan dizini oluşturun.

1. [Azure portala](https://portal.azure.com) gidin.

1. [Azure Cloud Shell](/azure/cloud-shell/overview)'i açın. Önceden bir ortam seçmediyseniz **Bash** ortamını seçin.

    ![Cloud Shell istemi](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. `clouddrive` dizinine geçin.

    ```bash
    cd clouddrive
    ```

1. `hub-spoke` adlı bir dizin oluşturun.

    ```bash
    mkdir hub-spoke
    ```

1. Dizinleri yeni dizinle değiştirin:

    ```bash
    cd hub-spoke
    ```

## <a name="declare-the-azure-provider"></a>Azure sağlayıcısını tanımlama

Azure sağlayıcısını tanımlayan Terraform yapılandırma dosyasını yapılandırın.

1. Cloud Shell ' de adlı `main.tf`yeni bir dosya açın.

    ```bash
    code main.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```hcl
    provider "azurerm" {
        version = "~>1.22"
    }
    ```

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

## <a name="create-the-variables-file"></a>Değişkenler dosyası oluşturma

Farklı betiklerde kullanılan ortak değişkenler için Terrayform yapılandırma dosyasını oluşturun.

1. Cloud Shell ' de adlı `variables.tf`yeni bir dosya açın.

    ```bash
    code variables.tf
    ```

1. Aşağıdaki kodu düzenleyiciye yapıştırın:

    ```hcl
    variable "location" {
      description = "Location of the network"
      default     = "centralus"
    }
    
    variable "username" {
      description = "Username for Virtual Machines"
      default     = "testadmin"
    }
    
    variable "password" {
      description = "Password for Virtual Machines"
      default     = "Password1234!"
    }
    
    variable "vmsize" {
      description = "Size of the VMs"
      default     = "Standard_DS1_v2"
    }
    ```

1. Dosyayı kaydedin ve düzenleyiciden çıkın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"] 
> [Azure 'da Terrayform ile şirket içi sanal ağ oluşturma](./terraform-hub-spoke-on-prem.md)
