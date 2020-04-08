---
title: Bulut hizmeti (portal) nasıl yapılandırılabilen | Microsoft Dokümanlar
description: Azure'da bulut hizmetlerini nasıl yapılandırabileceğinizi öğrenin. Bulut hizmeti yapılandırmasını güncelleştirmeyi ve rol örneklerine uzaktan erişimi yapılandırmayı öğrenin. Bu örnekler Azure portalını kullanır.
services: cloud-services
documentationcenter: ''
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 12/07/2016
ms.author: tagore
ms.openlocfilehash: e862818a4fe2471af574d153d43f0096af7847b8
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811402"
---
# <a name="how-to-configure-cloud-services"></a>Cloud Services’ı Yapılandırma

Azure portalında bir bulut hizmeti için en sık kullanılan ayarları yapılandırabilirsiniz. Veya yapılandırma dosyalarınızı doğrudan güncelleştirmek istiyorsanız, güncelleştirmek için bir hizmet yapılandırma dosyası indirin ve ardından güncelleştirilmiş dosyayı yükleyin ve yapılandırma değişiklikleriyle bulut hizmetini güncelleştirin. Her iki durumda da, yapılandırma güncelleştirmeleri tüm rol örneklerine itilir.

Ayrıca bulut hizmeti rollerinizin örneklerini veya uzak masaüstü nüzü bunlara yönetebilirsiniz.

Azure, yapılandırma güncelleştirmeleri sırasında yalnızca her rol için en az iki rol örneğiniz varsa yüzde 99,95 hizmet kullanılabilirliği sağlayabilir. Bu, bir sanal makinenin diğerinde güncelleştirilirken istemci isteklerini işlemesini sağlar. Daha fazla bilgi için [Hizmet Düzeyi Sözleşmeleri'ne](https://azure.microsoft.com/support/legal/sla/)bakın.

## <a name="change-a-cloud-service"></a>Bulut hizmetini değiştirme

[Azure portalını](https://portal.azure.com/)açtıktan sonra bulut hizmetinize gidin. Buradan, bunun birçok yönünü yönetmek.

![Ayarlar Sayfası](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Ayarlar** veya **Tüm ayarlar** bağlantıları, **Özellikleri**değiştirebileceğiniz, **Yapılandırmayı**değiştirebileceğiniz, **Sertifikaları**yönetebileceğiniz, **Uyarı kurallarını**ayarlayabileceğiniz ve bu bulut hizmetine erişimi olan **Kullanıcıları** yönetebileceğiniz **Ayarlar'ı** açar.

![Azure bulut hizmeti ayarları](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Konuk İşletim Sistemi sürümünü yönetme

Varsayılan olarak Azure, konuk işletim sisteminizi, Windows Server 2016 gibi hizmet yapılandırmanızda (.cscfg) belirttiğiniz işletim sistemi ailesindeki en son desteklenen görüntüyle düzenli olarak güncelleştirir.

Belirli bir işletim sistemi sürümünü hedeflemeniz gerekiyorsa, **yapılandırmada**ayarlayabilirsiniz.

![İşletim sistemi sürümünü ayarlama](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)

>[!IMPORTANT]
> Belirli bir işletim sistemi sürümü seçmek otomatik işletim sistemi güncelleştirmelerini devre dışı kılmış ve sorumluluğunuzu düzeltmenizi sağlar. Rol örneklerinizin güncelleştirmeler aldığından veya uygulamanızı güvenlik açıklarına maruz bırakabileceğinizden emin olmalısınız.

## <a name="monitoring"></a>İzleme

Bulut hizmetinize uyarılar ekleyebilirsiniz. **Ayarlar** > **Uyarı Kuralları'nı** > tıklatın**Uyarı ekleyin.**

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Buradan bir uyarı ayarlayabilirsiniz. **Metrik** açılır kutuyla, aşağıdaki veri türleri için bir uyarı ayarlayabilirsiniz.

* Disk okuma
* Disk yazma
* Ağ içinde
* Ağ çıkış
* CPU yüzdesi

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>İzlemeyi metrik döşemeden yapılandırma

**Ayarlar** > **Uyarı Kuralları'nı**kullanmak yerine, bulut hizmetinin **İzleme** bölümündeki metrik kutucuklardan birini tıklatabilirsiniz.

![Bulut Hizmeti İzleme](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Buradan döşemeyle birlikte kullanılan grafiği özelleştirebilir veya bir uyarı kuralı ekleyebilirsiniz.

## <a name="reboot-reimage-or-remote-desktop"></a>Yeniden başlatma, yeniden görüntüle veya uzak masaüstü

Uzak masaüstünü [Azure portalı (uzak masaüstünü ayarlama)](cloud-services-role-enable-remote-desktop-new-portal.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)veya [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)aracılığıyla ayarlayabilirsiniz.

Bulut Hizmeti'ni yeniden başlatmak, yeniden çizmek veya uzaktan kumanda etmek için bulut hizmeti örneğini seçin.

![Bulut Hizmeti Örneği](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Daha sonra uzak bir masaüstü bağlantısı başlatabilir, örneği uzaktan yeniden başlatabilir veya uzaktan yeniden görüntüleyebilir (yeni bir resimle başlatın) örneğini.

![Bulut Hizmeti Örnek Düğmeleri](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>.cscfg'nizi yeniden yapılandırın

Bulut hizmetinizi [hizmet config (cscfg)](cloud-services-model-and-package.md#cscfg) dosyası üzerinden yeniden yapılandırmanız gerekebilir. Önce .cscfg dosyanızı indirmeniz, değiştirmeniz, sonra da yüklemeniz gerekir.

1. **Ayarlar'ı**açmak için **Ayarlar** simgesine veya **Tüm Ayarlar** bağlantısına tıklayın.

    ![Ayarlar Sayfası](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. **Yapılandırma** öğesine tıklayın.

    ![Yapılandırma Blade](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. **İndir** düğmesine tıklayın.

    ![İndirme](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Hizmet yapılandırma dosyasını güncelleştirdikten sonra yapılandırma güncelleştirmelerini yükleyin ve uygulayın:

    ![Karşıya Yükle](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. .cscfg dosyasını seçin ve **Tamam'ı**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* [Bulut hizmetini](cloud-services-how-to-create-deploy-portal.md)nasıl dağıtılayın öğrenin.
* Özel bir [etki alanı adı](cloud-services-custom-domain-name-portal.md)yapılandırın.
* [Bulut hizmetinizi yönetin.](cloud-services-how-to-manage-portal.md)
* [TLS/SSL sertifikalarını](cloud-services-configure-ssl-certificate-portal.md)yapılandırın.



