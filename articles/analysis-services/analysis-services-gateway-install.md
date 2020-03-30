---
title: Azure Analiz Hizmetleri için şirket içi veri ağ geçidini yükleme | Microsoft Dokümanlar
description: Azure Analiz Hizmetleri sunucusundan şirket içi veri kaynaklarına bağlanmak için şirket içi veri ağ geçidini nasıl yükleyip yapılandırabileceğinizi öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/17/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f6218b32fb9574adf62384d2a6ee5a62f3788de8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77062158"
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>Şirket içi veri ağ geçidini yükleme ve yapılandırma

Aynı bölgedeki bir veya daha fazla Azure Çözümhizmetleri sunucusu şirket içi veri kaynaklarına bağlandığında şirket içi veri ağ geçidi gereklidir.  Yüklediğiniz ağ geçidi Power BI, Power Apps ve Logic Apps gibi diğer hizmetler tarafından kullanılanla aynı olsa da, Azure Çözümleme Hizmetleri için yükleme yaparken tamamlamanız gereken bazı ek adımlar vardır. Bu yükleme makalesi **Azure Çözümleme Hizmetleri'ne**özgüdür. 

Azure Analiz Hizmetleri ağ geçidinde nasıl çalıştığı hakkında daha fazla bilgi edinmek için şirket [içi veri kaynaklarına bağlanma'ya](analysis-services-gateway.md)bakın. Gelişmiş yükleme senaryoları ve genel olarak ağ geçidi hakkında daha fazla bilgi edinmek için [şirket içi veri ağ geçitleri belgelerine](/data-integration/gateway/service-gateway-onprem)bakın.

## <a name="prerequisites"></a>Ön koşullar

**Minimum Gereksinimler:**

* .NET 4.5 Framework
* Windows 8 / Windows Server 2012 R2 64 bit sürümü (veya daha sonra)

**Önerilen:**

* 8 Çekirdekli CPU
* 8 GB Bellek
* Windows 8 / Windows Server 2012 R2 64 bit sürümü (veya daha sonra)

**Önemli noktalar:**

* Kurulum sırasında, ağ geçidinizi Azure'a kaydederken aboneliğinizin varsayılan bölgesi seçilir. Farklı bir abonelik ve bölge seçebilirsiniz. Birden fazla bölgede sunucunuz varsa, her bölge için bir ağ geçidi yüklemeniz gerekir. 
* Ağ geçidi etki alanı denetleyicisi üzerinde yüklenemez.
* Tek bir bilgisayara yalnızca bir ağ geçidi yüklenebilir.
* Ağ geçidini, üzerinde kalan ve uyku moduna gitmeyen bir bilgisayara yükleyin.
* Ağınıza yalnızca kablosuz bağlantısı olan bir bilgisayara ağ geçidini yüklemeyin. Performans azaltılabilir.
* Ağ geçidini yüklerken, bilgisayarınıza oturum açtığınızda bulunan kullanıcı hesabının hizmet ayrıcalığı olarak Oturum Açması gerekir. Yükleme tamamlandığında, şirket içi veri ağ geçidi hizmeti, hizmet olarak oturum açmak için NT SERVICE\PBIEgwService hesabını kullanır. Kurulum sırasında veya kurulum tamamlandıktan sonra Hizmetler'de farklı bir hesap belirtilebilir. Grup İlkesi ayarlarının hem yükleme yaparken oturum açtığınızda oturum açtığınızda ki hesabına hem de seçtiğiniz hizmet hesabının hizmet ayrıcalığı olarak Oturum Açmasına izin verdiğinden emin olun.
* Ağ geçidini kaydettirdiğiniz abonelikle aynı [kiracı](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) için Azure AD'da bir hesapla Azure'da oturum açın. Azure B2B (konuk) hesapları, ağ geçidi ni yüklerken ve kaydederken desteklenmez.
* Veri kaynakları bir Azure Sanal Ağı'nda (VNet) kullanılıyorsa, [AlwaysUseGateway](analysis-services-vnet-gateway.md) sunucu özelliğini yapılandırmanız gerekir.

## <a name="download"></a><a name="download"></a>Indir

 [Ağ geçidini indirin](https://go.microsoft.com/fwlink/?LinkId=820925&clcid=0x409)

## <a name="install"></a><a name="install"></a>Yükleme

1. Kurulumu çalıştırın.

2. **Şirket içi veri ağ geçidini**seçin.

   ![Şunu seçin:](media/analysis-services-gateway-install/aas-gateway-installer-select.png)

2. Bir konum seçin, koşulları kabul edin ve sonra **Yükle'yi**tıklatın.

   ![Konum ve lisans koşullarını yükleme](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. Azure'da oturum açın. Hesap, kiracınızın Azure Etkin Dizini'nde olmalıdır. Bu hesap ağ geçidi yöneticisi için kullanılır. Azure B2B (konuk) hesapları ağ geçidini yüklerken ve kaydederken desteklenmez.

   ![Azure'da oturum açma](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > Bir etki alanı hesabıyla oturum açarak oturum açarak Azure AD'deki kuruluş hesabınıza eşlenir. Kuruluş hesabınız ağ geçidi yöneticisi olarak kullanılır.

## <a name="register"></a><a name="register"></a>Kaydettir

Azure'da bir ağ geçidi kaynağı oluşturmak için, yüklediğiniz yerel örneği Ağ Geçidi Bulut Hizmeti'ne kaydetmeniz gerekir. 

1.  **Bu bilgisayarda yeni bir ağ geçidi kaydol'u**seçin.

    ![Kaydettir](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. Ağ geçidiniz için bir ad ve kurtarma anahtarı yazın. Varsayılan olarak, ağ geçidi aboneliğinizin varsayılan bölgesini kullanır. Farklı bir bölge seçmeniz gerekiyorsa, **Bölge Değiştir'i**seçin.

    > [!IMPORTANT]
    > Kurtarma anahtarınızı güvenli bir yere kaydedin. Kurtarma anahtarının bir ağ geçidini devralması, geçiş etmesi veya geri yüklemesi gerekir. 

   ![Kaydettir](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-an-azure-gateway-resource"></a><a name="create-resource"></a>Azure ağ geçidi kaynağı oluşturma

Ağ geçidinizi yükledikten ve kaydettikten sonra Azure'da bir ağ geçidi kaynağı oluşturmanız gerekir. Ağ geçidini kaydederken kullandığınız hesapla Azure'da oturum açın.

1. Azure portalında **kaynak oluştur'u**tıklatın, ardından **şirket içi veri ağ geçidini**arayın ve ardından **Oluştur'u**tıklatın.

   ![Ağ geçidi kaynağı oluşturma](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. **Bağlantı ağ geçidi oluştur'da**şu ayarları girin:

   * **Adı**: Ağ geçidi kaynağınızın adını girin. 

   * **Abonelik**: Ağ geçidi kaynağınızla ilişkilendirmek için Azure aboneliğini seçin. 
   
     Varsayılan abonelik, oturum açmak için kullandığınız Azure hesabını temel alır.

   * **Kaynak grubu**: Kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.

   * **Konum**: Ağ geçidinizi kaydettiğiniz bölgeyi seçin.

   * **Yükleme Adı**: Ağ geçidi yüklemeniz henüz seçilmediyse, bilgisayarınıza yüklediğiniz ve kaydolduğunuz ağ geçidini seçin. 

     İşi bittiğinde **Oluştur'u**tıklatın.

## <a name="connect-servers-to-the-gateway-resource"></a><a name="connect-servers"></a>Sunucuları ağ geçidi kaynağına bağlama

1. Azure Analiz Hizmetleri sunucunuza genel bakış, **Şirket İçi Veri Ağ Geçidi'ni**tıklatın.

   ![Sunucuya ağ geçidine bağlanma](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. **Bağlanmak için Şirket İçi Veri Ağ Geçidi'ni Seçin'de**ağ geçidi kaynağınızı seçin ve ardından **seçili ağ geçidini bağla'yı**tıklatın.

   ![Sunucuya ağ geçidi kaynağına bağlanma](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > Ağ geçidiniz listede görünmüyorsa, sunucunuz büyük olasılıkla ağ geçidini kaydederken belirttiğiniz bölgeyle aynı bölgede değildir.

    Sunucunuz ve ağ geçidi kaynağınız arasındaki bağlantı başarılı olduğunda, durum **Bağlı'yı**gösterir.


    ![Sunucuya ağ geçidi kaynak başarısına bağlanma](media/analysis-services-gateway-install/aas-gateway-connect-success.png)

İşte bu kadar. Bağlantı noktalarını açmanız veya herhangi bir sorun giderme yapmanız gerekiyorsa, [şirket içi veri ağ geçidine](analysis-services-gateway.md)göz attığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* [Analysis Services’ı yönetme](analysis-services-manage.md)   
* [Azure Analiz Hizmetleri'nden veri alma](analysis-services-connect.md)   
* [Azure Sanal Ağı üzerindeki veri kaynakları için ağ geçidi kullanma](analysis-services-vnet-gateway.md)
