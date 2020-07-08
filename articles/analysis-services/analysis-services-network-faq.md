---
title: Analysis Services ağ bağlantısı hakkında sık sorulan sorular | Microsoft Docs
description: Bu makalede, ağ bağlantısıyla Analysis Services ilgili daha yaygın soruların yanıtları sağlanmaktadır.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/05/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: b60cf34e8efed2ed63b6e35cfaf7445edb701610
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82838510"
---
# <a name="frequently-asked-questions-about-analysis-services-network-connectivity"></a>Analysis Services ağ bağlantısı hakkında sık sorulan sorular

Bu makalede depolama hesaplarına, veri kaynaklarına, güvenlik duvarlarını ve IP adreslerine bağlanma hakkında sık sorulan sorulara yanıtlar verilmektedir.

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme

**Soru** -güvenlik duvarının arkasındaki bir depolama hesabını kullanarak yedekleme ve geri yükleme işlemlerinin nasıl yapılacağı nasıl yapabilirim?   
**Yanıtla** -Azure ANALYSIS SERVICES sabit IP adresleri veya hizmet etiketleri kullanmaz. Analysis Services sunucularınızın kullanacağı IP adresi aralığı, *Azure BÖLGESININ*IP adresleri aralığında herhangi bir şey olabilir. Sunucunuzun IP adresleri değişken olduğundan ve zaman içinde değişeceğinden, güvenlik duvarı kurallarınızın, sunucunuzun bulunduğu tüm Azure bölgesi IP adresleri aralığına izin vermeniz gerekir.

**Soru** -Azure Storage hesabım, Analysis Services sunucum tarafından farklı bir bölgede yer alan. Depolama hesabı güvenlik duvarı ayarlarını yapılandırmak Nasıl yaparım??   
**Yanıt** -depolama hesabı farklı bir bölgedeyse, **Seçili ağlardan**erişime izin vermek için depolama hesabı güvenlik duvarı ayarlarını yapılandırın. Güvenlik Duvarı **adres aralığı**' nda, Analysis Services sunucusunun bulunduğu bölge için IP adresi aralığını belirtin. Azure bölgelerinin IP aralıklarını almak için bkz. [Azure IP aralıkları ve hizmet etiketleri – genel bulut](https://www.microsoft.com/download/details.aspx?id=56519). Depolama hesabı güvenlik duvarı ayarlarının tüm ağlardan erişime izin verecek şekilde yapılandırılması desteklenir, ancak seçili ağları seçip bir IP adresi aralığı belirtilerek tercih edilir. 

**Soru** -Azure Storage hesabım, Analysis Services sunucum ile aynı bölgedeyse. Depolama hesabı güvenlik duvarı ayarlarını nasıl yapılandırabilirim?   
**Yanıt** – Analysis Services sunucunuz ve depolama hesabınız aynı bölgede olduğundan, aralarındaki ILETIŞIMLER iç IP adresi aralıkları kullanır, bu nedenle, seçili ağları kullanmak için bir güvenlik duvarı yapılandırmak ve bir IP adresi aralığı belirtmek desteklenmez. Kuruluş ilkelerine bir güvenlik duvarı gerekiyorsa, tüm ağlardan erişime izin verecek şekilde yapılandırılması gerekir.


## <a name="data-source-connections"></a>Veri kaynağı bağlantıları

**Soru** -veri kaynağı sistemim IÇIN bir VNET 'im var. Analysis Services sunuculerimin VNET 'ten veritabanına erişmesine nasıl izin veririm?   
**Yanıt** Azure Analysis Services sanal ağa katılamıyor. Burada en iyi çözüm, VNET 'te şirket içi veri ağ geçidini yükleyip yapılandırmak ve ardından Analysis Services sunucularınızı **Alwaysusegateway** sunucu özelliği ile yapılandırmak için kullanılır. Daha fazla bilgi için bkz. [Azure sanal ağı 'nda (VNet) veri kaynakları için ağ geçidi kullanma](analysis-services-vnet-gateway.md).

**Soru** -bir güvenlik duvarının arkasındaki kaynak Veritabanım var. Güvenlik duvarını Analysis Services sunucum tarafından erişmesine izin verecek şekilde nasıl yapılandırabilirim?   
**Yanıtla** -Azure ANALYSIS SERVICES sabit IP adresleri veya hizmet etiketleri kullanmaz. Analysis Services sunucularınızın kullanacağı IP adresi aralığı, *Azure BÖLGESININ*IP adresleri aralığında herhangi bir şey olabilir. Kaynak veritabanı güvenlik duvarı kurallarında sunucunuzun Azure bölgesinin tam IP adresi *aralığını* sağlamanız gerekir. Diğer bir deyişle, daha güvenli, diğer bir deyişle şirket içi veri ağ geçidi yapılandırmaktır. Daha sonra, Analysis Services sunucularınızı [Alwaysusegateway sunucusu özelliğiyle](analysis-services-vnet-gateway.md#configure-alwaysusegateway-property)yapılandırabilir ve ardından Şirket Içi veri ağ geçidinin, veri kaynağının güvenlik duvarı kuralları tarafından izin VERILEN bir IP adresine sahip olduğundan emin olun.

## <a name="azure-apps-with-ip-address"></a>IP adresi olan Azure uygulamaları

**Soru** -Azure tabanlı bir uygulama (örneğin, Azure işlevleri, Azure Data Factory), anında DEĞIŞEN bir IP adresiyle kullanıyorum. Uygulamamın yürütüldüğü IP adresine dinamik olarak izin vermek için Azure Analysis Services güvenlik duvarı kurallarını nasıl yönetebilirim?   
**Yanıt** Azure Analysis Services özel bağlantıları, VNET 'Leri veya hizmet etiketlerini desteklemez. Bazı açık kaynaklı çözümler bulunur (örneğin, https://github.com/mathwro/Scripts/blob/master/Azure/AllowAzure-AnalysisServer.ps1) istemci UYGULAMANıN IP adresini algılar ve güvenlik duvarı kurallarını otomatik olarak güncelleştirebilir.


## <a name="next-steps"></a>Sonraki adımlar

[Şirket içi veri ağ geçidi yükleyip yapılandırma](analysis-services-gateway-install.md)   
[Şirket içi veri ağ geçidi ile şirket içi veri kaynaklarına bağlanma](analysis-services-gateway.md)   
[Azure sanal ağı (VNet) üzerinde veri kaynakları için ağ geçidi kullanma](analysis-services-vnet-gateway.md)
