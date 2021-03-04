---
title: FHıR için Azure API için özel bağlantı
description: Bu makalede, FHıR Hizmetleri için Azure API için özel bir uç noktanın nasıl ayarlanacağı açıklanır
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 03/03/2021
ms.author: zxue
ms.openlocfilehash: 5e24c7666fd2ece7d284b7705bc481866d7604de
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097049"
---
# <a name="configure-private-link"></a>Özel bağlantıyı Yapılandır

Özel bağlantı, sanal ağınızdan özel bir IP adresi kullanarak özel ve güvenli bir şekilde bağlanan bir ağ arabirimi olan özel bir uç nokta üzerinden FHıR için Azure API 'sine erişmenizi sağlar. Özel bağlantıyla, herkese ortak bir DNS aracılığıyla bir ilk taraf hizmeti olarak güvenli bir şekilde erişim sağlayabilirsiniz. Bu makalede, FHıR için Azure API için özel uç noktanızı oluşturma, test etme ve yönetme işlemi adım adım açıklanmaktadır.

>[!Note]
>Bir özel bağlantı etkinleştirildikten sonra, özel bağlantı veya FHıR için Azure API 'SI, bir kaynak grubundan veya abonelikten diğerine taşınabilir. Taşımak için, önce özel bağlantıyı silin, ardından taşıma işlemi tamamlandıktan sonra FHıR için Azure API 'yi taşıyın ve yeni bir özel bağlantı oluşturun. Özel bağlantıyı silmeden önce olası güvenlik kollerini değerlendirin.
>
>Denetim günlüklerini dışarı aktarma ve/ölçümler, FHıR için Azure API 'SI için etkinleştirilmişse, portaldan Tanılama ayarları aracılığıyla dışarı aktarma ayarını güncelleştirin.

## <a name="prerequisites"></a>Önkoşullar

Özel bir uç nokta oluşturmadan önce, öncelikle oluşturmanız gereken bazı Azure kaynakları vardır:

- Kaynak grubu: sanal ağı ve özel uç noktasını içerecek Azure Kaynak grubu.
- FHıR için Azure API 'SI: özel bir uç noktanın arkasına koymak istediğiniz FHıR kaynağı.
- Sanal ağ: istemci hizmetlerinizin ve özel uç noktanızın bağlanacağı VNet.

Daha fazla bilgi için [özel bağlantı belgelerine](../private-link/index.yml)göz atın.

## <a name="disable-public-network-access"></a>Ortak ağ erişimini devre dışı bırak

FHıR kaynağınız için özel bir uç nokta oluşturulması, genel trafiği otomatik olarak devre dışı bırakır. Bunu yapmak için FHıR kaynağınızı "Enabled" kaynağından "devre dışı" olarak yeni bir "ortak erişim" özelliği ayarlamak için güncelleştirmeniz gerekir. Uygun şekilde yapılandırılmış bir özel uç noktadan gelmemiş olan FHıR hizmetinize yapılan tüm istekler reddedilecektir, genel ağ erişimini devre dışı bırakırken dikkatli olun. Yalnızca özel uç noktalarınıza giden trafiğe izin verilir.

![Ortak ağ erişimini devre dışı bırak](media/private-link/private-link-disable.png)

## <a name="create-private-endpoint"></a>Özel uç nokta oluştur

Özel bir uç nokta oluşturmak için, FHıR kaynağında RBAC izinlerine sahip bir geliştirici Azure portal, [Azure PowerShell](../private-link/create-private-endpoint-powershell.md)veya [Azure CLI](../private-link/create-private-endpoint-cli.md)kullanabilir. Bu makale, Azure portal kullanma adımlarında size yol gösterir. Özel DNS bölgesinin oluşturulmasını ve yapılandırılmasını otomatik hale getiren Azure portal kullanmak önerilir. Daha fazla ayrıntı için [özel bağlantı hızlı başlangıç kılavuzlarıyla](../private-link/create-private-endpoint-portal.md) başvurabilirsiniz.

Özel uç nokta oluşturmanın iki yolu vardır. Otomatik onay akışı, FHıR kaynağında RBAC izinlerine sahip olan bir kullanıcının onaya gerek duymadan özel bir uç nokta oluşturmasına izin verir. El ile onay akışı, FHıR kaynağında izinleri olmayan bir kullanıcının FHıR kaynağının sahipleri tarafından onaylanabilmesi için özel bir uç nokta istemesine izin verir.

### <a name="auto-approval"></a>Otomatik onay

Yeni özel uç nokta bölgesinin, sanal ağınızın bölgesiyle aynı olduğundan emin olun. FHıR kaynağınızın bölgesi farklı olabilir.

![Azure portal temel bilgiler sekmesi](media/private-link/private-link-portal2.png)

Kaynak türü için "Microsoft. Healthgelişme API 'leri/Hizmetleri" ni arayın ve seçin. Kaynak için FHıR kaynağını seçin. Hedef alt kaynak için "fhır" öğesini seçin.

![Kaynak sekmesine Azure portal](media/private-link/private-link-portal1.png)

Ayarlanmış bir Özel DNS bölgeniz yoksa "(New) Privatelink. azurehealthcareapis. com" seçeneğini belirleyin. Özel DNS bölgeniz zaten yapılandırılmışsa, listeden seçebilirsiniz. "Privatelink.azurehealthcareapis.com" biçiminde olmalıdır.

![Azure portal Yapılandırma sekmesi](media/private-link/private-link-portal3.png)

Dağıtım tamamlandıktan sonra, bağlantı durumu olarak "Onaylandı" göreceğiniz "özel uç nokta bağlantıları" sekmesine dönebilirsiniz.

### <a name="manual-approval"></a>El ile onay

El ile onay için kaynak, "kaynak KIMLIĞINE veya diğer ada göre Azure kaynağına bağlanma" bölümünde ikinci seçeneği belirleyin. Hedef alt kaynak için otomatik onay içinde "fhır" yazın.

![El ile onay](media/private-link/private-link-manual.png)

Dağıtım tamamlandıktan sonra, bağlantısını onaylayabileceğiniz, reddedebileceğiniz veya kaldırabileceğiniz "özel uç nokta bağlantıları" sekmesine dönebilirsiniz.

![Seçenekler](media/private-link/private-link-options.png)

## <a name="test-private-endpoint"></a>Test özel uç noktası

FHıR sunucunuzun ortak ağ erişimini devre dışı bıraktıktan sonra ortak trafik almamakta olduğundan emin olmak için, bilgisayarınızdan sunucunuza yönelik/Metadata uç noktasını vurmaya çalışın. 403 bir yasak almalısınız. Genel trafik engellenmeden önce genel ağ erişim bayrağını güncelleştirdikten 5 dakika kadar sürebilir.

Özel uç noktanızın sunucunuza trafik göndereğinden emin olmak için:

1. Özel uç noktanızın yapılandırıldığı sanal ağa ve alt ağa bağlı bir VM oluşturun. VM 'deki trafiğinizin yalnızca özel ağı kullandığından emin olmak için NSG kuralı aracılığıyla giden internet trafiğini devre dışı bırakabilirsiniz.
2. VM 'ye RDP.
3. VM 'den FHıR sunucunuzun/Metadata uç noktasına vurarak bir yanıt olarak yetenek bildirisini almalısınız.

## <a name="manage-private-endpoint"></a>Özel uç noktayı Yönet

### <a name="view"></a>Görünüm

Özel uç noktalar ve ilişkili NIC, içinde oluşturuldukları kaynak grubundan Azure portal görünür.

![Kaynaklarda görüntüle](media/private-link/private-link-view.png)

### <a name="delete"></a>Sil

Özel uç noktalar yalnızca genel bakış dikey penceresinde (aşağıdaki gibi) veya ağ (Önizleme) "özel uç nokta bağlantıları" sekmesinde Sil seçeneği aracılığıyla Azure portal silinebilir. Sil düğmesine tıkladığınızda özel uç nokta ve ilişkili NIC silinir. Tüm özel uç noktaları FHıR kaynağına silerseniz ve genel ağ erişimi devre dışıysa, hiçbir istek bunu FHıR sunucunuza yapmaz.

![Özel uç noktayı Sil](media/private-link/private-link-delete.png)
