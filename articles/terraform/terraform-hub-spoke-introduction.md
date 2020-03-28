---
title: Öğretici - Terraform'u kullanarak Azure'da hub ve kollu karma ağ topolojisi oluşturun
description: Terraform'u kullanarak Azure'da tüm karma ağ başvuru mimarisinin nasıl oluşturulabildiğini gösteren öğretici
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 6f156dd90b83ceaf5749c8c2acebae35bcb54a92
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77472188"
---
# <a name="tutorial-create-a-hub-and-spoke-hybrid-network-topology-in-azure-using-terraform"></a>Öğretici: Terraform'u kullanarak Azure'da hub ve kollu karma ağ topolojisi oluşturun

Bu öğretici seri, Azure'da bir [hub ve konuşan ağ topolojisini](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)uygulamak için Terraform'un nasıl kullanılacağını gösterir. 

Hub ve konuşan topoloji, ortak hizmetleri paylaşırken iş yüklerini yalıtmanın bir yoludur. Bu hizmetler kimlik ve güvenlik içerir. Hub, şirket içi bir ağa merkezi bağlantı noktası görevi gören bir sanal ağdır (VNet). Uçlar, merkezle eşlenen sanal ağlardır. Paylaşılan hizmetler hub'da dağıtılırken, tek tek iş yükleri konuşan ağlarda dağıtılır.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Hub ve kollu hibrit ağ başvuru mimarisi kaynaklarını düzenlemek için HCL 'yi (HashiCorp Dili) kullanın
> * Hub ağ cihaz kaynakları oluşturmak için Terraform'u kullanın
> * Tüm kaynaklar için ortak nokta olarak hareket etmek için Azure'da hub ağı oluşturmak için Terraform'u kullanın
> * Azure'da vnet olarak ayrı iş yükleri oluşturmak için Terraform'u kullanın
> * Şirket içi ve Azure ağları arasında ağ geçitleri ve bağlantılar kurmak için Terraform'u kullanın
> * Konuşan ağlara VNet eşlemeleri oluşturmak için Terraform'u kullanın

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

- **Terraform'u yükleme ve yapılandırma**: Azure'da VM'leri ve diğer altyapıyı sağlamak için [Terraform'u yükleyin ve yapılandırır](terraform-install-configure.md)

## <a name="hub-and-spoke-topology-architecture"></a>Hub ve topoloji mimarisi konuştu

Hub ve kollu topolojide, hub bir VNet'tir. VNet, şirket içi ağınıza merkezi bir bağlantı noktası görevi görür. Uçlar ise merkezle eşleştirilmiş bir ağdır ve iş yüklerini yalıtmak için kullanılabilir. Şirket içi veri merkezi ile merkez arasındaki trafik akışı bir ExpressRoute veya VPN ağ geçidi bağlantısı üzerinden gerçekleşir. Aşağıdaki resim, bir hub'daki bileşenleri ve konuşan topolojiyi gösterir:

![Azure'da hub ve konuşmalı topoloji mimarisi](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-architecture.png)

## <a name="benefits-of-the-hub-and-spoke-topology"></a>Hub ve kollu topolojinin faydaları

Hub ve konuşan ağ topolojisi, ortak hizmetleri paylaşırken iş yüklerini yalıtmanın bir yoludur. Bu hizmetler kimlik ve güvenlik içerir. Hub, şirket içi bir ağa merkezi bağlantı noktası görevi gören bir VNet'tir. Uçlar, merkezle eşlenen sanal ağlardır. Paylaşılan hizmetler hub'da dağıtılırken, tek tek iş yükleri konuşan ağlarda dağıtılır. Hub ve spoke ağ topolojisinin bazı avantajları şunlardır:

- Hizmetleri birden çok iş yükü tarafından paylaşılabilen tek bir konumda merkezileştirerek **maliyet tasarrufu.** Bu iş yükleri ağ sanal cihazları ve DNS sunucuları içerir.
- **Abonelik sınırlarını aşma**: Farklı aboneliklere ait sanal ağlar merkeze eşlenerek sınırlar aşılabilir.
- **İşlerin ayrılması**: Merkezi BT (SecOps, InfraOps) ve iş yüklerini (DevOps) ilgilendiren konular ayrılır.

## <a name="typical-uses-for-the-hub-and-spoke-architecture"></a>Hub ve spoke mimarisi için tipik kullanımlar

Hub ve spoke mimarisi için tipik kullanımlardan bazıları şunlardır:

- Birçok müşterinin farklı ortamlarda dağıtılan iş yükleri vardır. Bu ortamlar geliştirme, test ve üretimi içerir. Çoğu zaman, bu iş yüklerinin DNS, IDS, NTP veya AD DS gibi hizmetleri paylaşması gerekir. Bu paylaşılan hizmetler VNet hub'ına yerleştirilebilir. Bu şekilde, her ortam yalıtımı korumak için bir konuşma için dağıtılır.
- Birbiriyle bağlantı gerektirmeyen, ancak paylaşılan hizmetlere erişim gerektiren iş yükleri.
- Güvenlik yönleri üzerinde merkezi denetim gerektiren işletmeler.
- Her konuşmadaki iş yükleri için ayrılmış yönetim gerektiren işletmeler.

## <a name="preview-the-demo-components"></a>Demo bileşenlerini önizleyin

Bu serideki her öğretici üzerinde çalışırken, çeşitli bileşenler farklı Terraform komut dosyalarında tanımlanır. Oluşturulan ve dağıtılan demo mimarisi aşağıdaki bileşenlerden oluşur:

- **Şirket içi ağ.** Bir kuruluşla çalışan özel bir yerel alan ağı. Hub ve spoke başvuru mimarisi için Azure'daki bir VNet, şirket içi bir ağı simüle etmek için kullanılır.

- **VPN cihazı**. VPN aygıtı veya hizmeti şirket içi ağa harici bağlantı sağlar. VPN aygıtı bir donanım aygıtı veya yazılım çözümü olabilir. 

- **Merkezi sanal ağ**. Hub, şirket içi ağınıza bağlantının merkezi noktası ve hizmetleri barındırabileceğiniz bir yerdir. Bu hizmetler, konuşan VNets barındırılan farklı iş yükleri tarafından tüketilebilir.

- **Ağ geçidi alt ağı**. VNet ağ geçitleri aynı alt ağda tutulur.

- **Uç sanal ağları**. Uçlar, iş yüklerini diğer uçlardan ayrı olarak yönetilen kendi sanal ağlarında yalıtmak için kullanılabilir. Her iş yükü birden fazla katman içerebilir. Birden fazla alt ağ, Azure yük dengeleyicileri aracılığıyla birbirine bağlanır. 

- **VNet bakan**. İki VNet bir peering bağlantısı kullanılarak bağlanabilir. Eşleme bağlantıları, sanal ağlar arasındaki geçişsiz ve gecikme süresi düşük bağlantılardır. Bir kez bakıldıktan sonra, VNet'ler bir yönlendiriciye gerek kalmadan Azure omurgasını kullanarak trafiği değiştirir. Bir hub ve kollu ağ topolojisinde, VNet eşleme hub'ı her konuştuğa bağlamak için kullanılır. Aynı bölgede veya farklı bölgelerdeVNet'leri eşleyebilirsiniz.

## <a name="create-the-directory-structure"></a>Dizin yapısını oluşturma

Demo için Terraform yapılandırma dosyalarınızı tutan dizini oluşturun.

1. [Azure portalına](https://portal.azure.com)göz atın.

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

1. Bulut Kabuğu'nda, '' `main.tf`adlı yeni bir dosya açın.

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

## <a name="create-the-variables-file"></a>Değişkenler dosyasını oluşturma

Farklı komut dosyaları arasında kullanılan ortak değişkenler için Terraform yapılandırma dosyasını oluşturun.

1. Bulut Kabuğu'nda, '' `variables.tf`adlı yeni bir dosya açın.

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
> [Azure'da Terraform ile şirket içi sanal ağ oluşturun](./terraform-hub-spoke-on-prem.md)