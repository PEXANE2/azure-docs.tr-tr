---
title: Azure Güvenlik Duvarı için FQDN etiketleri genel bakış
description: FQDN etiketi, iyi bilinen Microsoft hizmetleriyle ilişkili tam nitelikli alan adları (FQDN'ler) grubunu temsil eder.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 6396f8292a4c54f7fce237439f37c3e8156d59e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74169046"
---
# <a name="fqdn-tags-overview"></a>FQDN etiketleri genel bakış

FQDN etiketi, iyi bilinen Microsoft hizmetleriyle ilişkili tam nitelikli alan adları (FQDN'ler) grubunu temsil eder. Güvenlik duvarınızda gerekli giden ağ trafiğine izin vermek için uygulama kurallarında bir FQDN etiketi kullanabilirsiniz.

Örneğin, güvenlik duvarınız üzerinden Windows Update ağ trafiğine el ile izin vermek için Microsoft belgeleri başına birden çok uygulama kuralı oluşturmanız gerekir. FQDN etiketlerini kullanarak, bir uygulama kuralı oluşturabilir, **Windows Güncelleştirmeleri** etiketini ekleyebilirsiniz ve artık Microsoft Windows Update uç noktalarına ağ trafiği güvenlik duvarınızdan akabilir.

Kendi FQDN etiketlerinizi oluşturamaz ve etikete hangi FQDN'lerin eklenmiş olduğunu belirtemezsiniz. Microsoft, FQDN etiketinin kapsadığı FQDN'leri yönetir ve FQDN'ler değiştikçe etiketi güncelleştirir. 

<!--- screenshot of application rule with a FQDN tag.-->

Aşağıdaki tabloda kullanabileceğiniz geçerli FQDN etiketleri gösterilmektedir. Microsoft bu etiketleri korur ve ek etiketlerin düzenli aralıklarla eklenmesini bekleyebilirsiniz.

## <a name="current-fqdn-tags"></a>Geçerli FQDN etiketleri

|FQDN etiketi  |Açıklama  |
|---------|---------|
|Windows Update     |[Yazılım Güncelleştirmeleri için Güvenlik Duvarı Yapılandırma hakkında açıklandığı](https://technet.microsoft.com/library/bb693717.aspx)gibi Microsoft Update'e giden erişime izin verin.|
|Windows Tanılama Özellikleri|Tüm [Windows Diagnostics uç noktalarına](https://docs.microsoft.com/windows/privacy/configure-windows-diagnostic-data-in-your-organization#endpoints)giden erişime izin verin.|
|Microsoft Etkin Koruma Hizmeti (MAPS)|[MAPS'e](https://cloudblogs.microsoft.com/enterprisemobility/2016/05/31/important-changes-to-microsoft-active-protection-service-maps-endpoint/)giden erişime izin verin.|
|Uygulama Hizmet Ortamı (ASE)|ASE platform trafiğine giden erişim sağlar. Bu etiket, ASE tarafından oluşturulan müşteriye özgü Depolama ve SQL uç noktalarını kapsamaz. Bunlar [Hizmet Bitiş Noktaları](../virtual-network/tutorial-restrict-network-access-to-resources.md) aracılığıyla etkinleştirilmeli veya el ile eklenmelidir.<br><br>Azure Güvenlik Duvarı'nı ASE ile tümleştirme hakkında daha fazla bilgi için bir [Uygulama Hizmeti Ortamını Kilitleme'ye](../app-service/environment/firewall-integration.md#configuring-azure-firewall-with-your-ase)bakın.|
|Azure Backup|Azure Yedekleme hizmetlerine giden erişime izin verir.|
|Azure HDInsight|HDInsight platform trafiği için giden erişim sağlar. Bu etiket, HDInsight'ın müşteriye özel Depolama veya SQL trafiğini kapsamaz. [Bunları Hizmet Bitiş Noktalarını](../virtual-network/tutorial-restrict-network-access-to-resources.md) kullanarak etkinleştirin veya el ile ekleyin.|

> [!NOTE]
> Bir uygulama kuralında FQDN Etiketi seçilirken, protokol:bağlantı noktası alanı **https**olarak ayarlanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik Duvarı'nı nasıl dağıtacağımı öğrenmek için [Bkz.](tutorial-firewall-deploy-portal.md)
