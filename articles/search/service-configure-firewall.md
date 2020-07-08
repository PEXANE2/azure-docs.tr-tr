---
title: Azure Bilişsel Arama hizmetiniz için bir IP güvenlik duvarı yapılandırma
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama hizmetinize erişimi kısıtlamak için IP denetim ilkelerini yapılandırın.
manager: nitinme
author: mrcarter8
ms.author: mcarter
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/11/2020
ms.openlocfilehash: 699715e1188616c2d6bda47016ec1ea7b05cef83
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83125613"
---
# <a name="configure-ip-firewall-for-azure-cognitive-search"></a>Azure Bilişsel Arama için IP güvenlik duvarını yapılandırma

Azure Bilişsel Arama, gelen güvenlik duvarı desteği için IP kurallarını destekler. Bu model, arama hizmetiniz için bir Azure sanal ağ güvenlik grubunda bulacağınız IP kurallarına benzer ek bir güvenlik katmanı sağlar. Bu IP kuralları ile, arama hizmetinizi yalnızca onaylanan bir makine ve/veya bulut hizmeti kümesinden erişilebilir olacak şekilde yapılandırabilirsiniz. Bu onaylanmış makine ve hizmet kümelerinden arama hizmetinize depolanan verilere erişim, çağıranın geçerli bir yetkilendirme belirteci sunmasını gerektirir.

> [!Important]
> Azure Bilişsel Arama hizmetinizde IP kuralları Azure portal veya [yönetim REST API 2020-03-13 sürümü](https://docs.microsoft.com/rest/api/searchmanagement/)kullanılarak yapılandırılabilir.

## <a name="configure-an-ip-firewall-using-the-azure-portal"></a><a id="configure-ip-policy"></a>Azure portal kullanarak bir IP güvenlik duvarı yapılandırma

Azure portal IP erişim denetimi ilkesini ayarlamak için Azure Bilişsel Arama hizmet sayfanıza gidin ve gezinti menüsünde **ağ** ' ı seçin. Uç nokta ağ bağlantısı **genel**olmalıdır. Bağlantınız **özel**olarak ayarlandıysa, yalnızca özel bir uç nokta aracılığıyla arama hizmetinize erişebilirsiniz.

![Azure portal IP güvenlik duvarının nasıl yapılandırılacağını gösteren ekran görüntüsü](./media/service-configure-firewall/azure-portal-firewall.png)

Azure portal, CıDR biçiminde IP adreslerini ve IP adresi aralıklarını belirtme olanağı sağlar. CıDR gösterimiyle ilgili bir örnek, 8.8.8.0 ile 8.8.8.255 arasında bir IP 'yi temsil eden 8.8.8.0/24 ' dir.

> [!NOTE]
> Azure Bilişsel Arama hizmetiniz için IP erişim denetimi ilkesini etkinleştirdikten sonra, izin verilen IP adresi aralıkları dışında makinelerden gelen veri düzlemine yapılan tüm istekler reddedilir. IP kuralları yapılandırıldığında Azure portal bazı özellikleri devre dışı bırakılır. Hizmet düzeyi bilgilerini görüntüleyebilir ve yönetebileceksiniz, ancak dizin verilerine ve hizmette Dizin, Dizin Oluşturucu ve beceri tanımları gibi çeşitli bileşenlere erişim sağlamak için Portal erişimi güvenlik nedenleriyle kısıtlıdır.

### <a name="requests-from-your-current-ip"></a>Geçerli IP 'nizden gelen istekler

Azure portal geliştirmeyi basitleştirmek için, istemci makinenizin IP 'sini belirlemenize ve izin verilenler listesine eklemenize yardımcı olur. Makinenizde çalışan uygulamalar, daha sonra Azure Bilişsel Arama hizmetinize erişebilir.

Portal, istemci IP adresinizi otomatik olarak algılar. Makinenizin veya ağ geçidinizin istemci IP adresi olabilir. İş yükünüzü üretime almadan önce bu IP adresini kaldırdığınızdan emin olun.

Geçerli IP 'nizi IP listesine eklemek için, **ISTEMCI IP adresinizi ekleyin**' i işaretleyin. Sonra **Kaydet**'i seçin.

![IP Güvenlik Duvarı ayarlarının geçerli IP 'ye izin vermek için nasıl yapılandırılacağını gösteren ekran görüntüsü](./media/service-configure-firewall/enable-current-ip.png)

## <a name="troubleshoot-issues-with-an-ip-access-control-policy"></a><a id="troubleshoot-ip-firewall"></a>IP erişim denetimi ilkesiyle ilgili sorunları giderme

Aşağıdaki seçenekleri kullanarak bir IP erişim denetimi ilkesiyle ilgili sorunları çözebilirsiniz:

### <a name="azure-portal"></a>Azure portal

Azure Bilişsel Arama hizmetiniz için bir IP erişim denetim ilkesinin etkinleştirilmesi, Azure portal dahil olmak üzere izin verilen IP adresi aralıkları listesinin dışındaki makinelerden gelen tüm istekleri engeller.  Hizmet düzeyi bilgilerini görüntüleyebilir ve yönetebileceksiniz, ancak dizin verilerine ve hizmette Dizin, Dizin Oluşturucu ve beceri tanımları gibi çeşitli bileşenlere erişim sağlamak için Portal erişimi güvenlik nedenleriyle kısıtlıdır. 

### <a name="sdks"></a>SDK’lar

İzin verilenler listesinde olmayan makinelerden SDK 'Yı kullanarak Azure Bilişsel Arama hizmetine eriştiğinizde, ek ayrıntı olmadan genel **403 yasaklanmış** bir yanıt döndürülür. Hesabınız için izin verilen IP listesini doğrulayın ve arama hizmetiniz için doğru yapılandırmanın güncelleştirildiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Özel bağlantı aracılığıyla arama hizmetinize erişme hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

* [Azure Bilişsel Arama güvenli bağlantısı için özel uç nokta oluşturma](service-create-private-endpoint.md)
