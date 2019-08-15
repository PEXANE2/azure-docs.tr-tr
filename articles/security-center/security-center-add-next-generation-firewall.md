---
title: Azure Güvenlik Merkezi 'nde yeni nesil güvenlik duvarı ekleme | Microsoft Docs
description: Bu belgede, Azure Güvenlik Merkezi önerilerini nasıl uygulayacağınızı, **Yeni nesil güvenlik duvarı ekleme** ve **trafiği yalnızca NGFW aracılığıyla yönlendirme**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 48b99015-4db8-4ce8-85e4-b544c0fa203e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 731102037b596091b80fbdfa02a8ff3c111b556e
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "60707083"
---
# <a name="add-a-next-generation-firewall-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde yeni nesil güvenlik duvarı ekleme
Azure Güvenlik Merkezi, güvenlik korumalarını artırmak için bir Microsoft iş ortağından yeni nesil güvenlik duvarı (NGFW) eklemenizi önerebilir. Bu belge, bunun nasıl yapılacağını gösteren bir örnek sağlar.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
1. **Öneriler** dikey penceresinde **Yeni nesil güvenlik duvarı Ekle**' yi seçin.
   ![Yeni Nesil Güvenlik Duvarı ekleme][1]
2. **Yeni nesil güvenlik duvarı Ekle** dikey penceresinde bir uç nokta seçin.
   ![Bir uç nokta seçin][2]
3. İkinci bir **Yeni nesil güvenlik duvarı Ekle** dikey penceresi açılır. Varsa mevcut bir çözümü kullanmayı seçebilirsiniz veya yeni bir çözüm oluşturabilirsiniz. Bu örnekte, bir NGFW oluşturduğumuz için mevcut hiçbir çözüm bulunmamaktadır.
   ![Yeni nesil güvenlik duvarı oluştur][3]
4. Bir NGFW oluşturmak için, tümleşik iş ortakları listesinden bir çözüm seçin. Bu örnekte, **Check Point**' i seçiyoruz.
   ![Yeni nesil güvenlik duvarı çözümünü seçin][4]
5. İş ortağı çözümü hakkında bilgi sağlayan **onay noktası** dikey penceresi açılır. Bilgi dikey penceresinde **Oluştur** ' u seçin.
   ![Güvenlik duvarı bilgileri dikey penceresi][5]
6. **Sanal makine oluştur** dikey penceresi açılır. Burada, NGFW çalıştıran bir sanal makineyi (VM) çalıştırmak için gereken bilgileri girebilirsiniz. Adımları izleyin ve gereken NGFW bilgilerini sağlayın. Uygulamak için Tamam ' ı seçin.
   ![NGFW çalıştırmak için sanal makine oluştur][6]

## <a name="route-traffic-through-ngfw-only"></a>Trafiği yalnızca NGFW aracılığıyla yönlendir
**Öneriler** dikey penceresine dönün. Güvenlik Merkezi aracılığıyla bir NGFW eklendikten sonra, **yalnızca NGFW aracılığıyla yönlendirme trafiği**olarak adlandırılan yeni bir giriş oluşturuldu. Bu öneri yalnızca NGFW 'yi Güvenlik Merkezi aracılığıyla yüklediyseniz oluşturulur. Internet 'e yönelik uç noktalarınız varsa, Güvenlik Merkezi, NGFW aracılığıyla sanal makinenize gelen trafiğe zorlayan ağ güvenlik grubu kurallarını yapılandırmanızı önerir.

1. **Öneriler dikey**penceresinde, **trafiği yalnızca NGFW aracılığıyla yönlendir**' i seçin.
   ![Trafiği yalnızca NGFW aracılığıyla yönlendirme][7]
2. Bu, trafiği yönlendirmenize olanak tanıyan VM 'Leri listeleyen **yalnızca NGFW aracılığıyla dikey pencere yönlendirme trafiğini**açar. Listeden bir VM seçin.
   ![VM seçin][8]
3. İlgili gelen kuralları görüntüleyerek seçili VM için bir dikey pencere açılır. Bir açıklama, olası sonraki adımlar hakkında daha fazla bilgi sağlar. Gelen kuralı düzenlemeye devam etmek için **gelen kurallarını Düzenle** ' yi seçin. Beklentide **kaynak** , NGFW Ile bağlantılı Internet 'e yönelik uç noktalar için **hiçbir** şekilde ayarlanmamış. Gelen kuralının özellikleri hakkında daha fazla bilgi edinmek için bkz. [güvenlik kuralları](../virtual-network/security-overview.md#security-rules).
   ![][9]
   Erişim![düzenleme gelen kuralını sınırlamak için kuralları yapılandırma][10]

## <a name="see-also"></a>Ayrıca bkz.
Bu belge, "yeni nesil güvenlik duvarı ekleme" Güvenlik Merkezi önerisini nasıl uygulayacağınızı gösterdi. NGFWs ve denetim noktası iş ortağı çözümü hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Yeni nesil güvenlik duvarı](https://en.wikipedia.org/wiki/Next-Generation_Firewall)
* [Check Point vSEC](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/checkpoint.vsec)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi 'nde güvenlik Ilkelerini ayarlama](tutorial-security-policy.md) -güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) -Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini bulun.

<!--Image references-->
[1]: ./media/security-center-add-next-gen-firewall/add-next-gen-firewall.png
[2]: ./media/security-center-add-next-gen-firewall/select-an-endpoint.png
[3]: ./media/security-center-add-next-gen-firewall/create-new-next-gen-firewall.png
[4]: ./media/security-center-add-next-gen-firewall/select-next-gen-firewall.png
[5]: ./media/security-center-add-next-gen-firewall/firewall-solution-info-blade.png
[6]: ./media/security-center-add-next-gen-firewall/create-virtual-machine.png
[7]: ./media/security-center-add-next-gen-firewall/route-traffic-through-ngfw.png
[8]: ./media/security-center-add-next-gen-firewall/select-vm.png
[9]: ./media/security-center-add-next-gen-firewall/configure-rules-to-limit-access.png
[10]: ./media/security-center-add-next-gen-firewall/edit-inbound-rule.png
