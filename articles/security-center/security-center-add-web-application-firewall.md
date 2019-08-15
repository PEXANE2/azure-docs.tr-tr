---
title: Azure Güvenlik Merkezi 'nde Web uygulaması güvenlik duvarı ekleme | Microsoft Docs
description: Bu belgede, Azure Güvenlik Merkezi önerilerini nasıl uygulayacağınızı ve **Web uygulaması güvenlik duvarı ekleme** ve **uygulama korumasını sonlandırma**işlemlerinin nasıl yapılacağı gösterilir.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/13/2018
ms.author: rkarlin
ms.openlocfilehash: 63852ccab842f11f30bcbe695206fedf72931911
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "60706293"
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde Web uygulaması güvenlik duvarı ekleme
Azure Güvenlik Merkezi, Web uygulamalarınızı güvenli hale getirmek için bir Microsoft iş ortağından bir Web uygulaması güvenlik duvarı (WAF) eklemenizi önerebilir. Bu belgede, bu öneriyi nasıl uygulayacağınız konusunda bir örnek gösterilmektedir.

Açık gelen Web bağlantı noktaları (80.443) ile ilişkili bir ağ güvenlik grubuna sahip olan genel kullanıma açık IP (örnek düzeyi IP veya yük dengeli IP) için WAF önerisi gösterilmektedir.

Güvenlik Merkezi, [yalıtılmış](https://azure.microsoft.com/pricing/details/app-service/windows/) hizmet planı altında dağıtılan sanal makinelerde ve dış App Service ortamlarında Web uygulamalarınızı hedefleme saldırılarına karşı savunmaya yardımcı olmak için bir WAF sağlamanızı önerir. Yalıtılmış plan, uygulamalarınızı gizli ve adanmış bir Azure ortamında barındırır. Şirket içi ağınızla güvenli bir bağlantıya ya da daha fazla performans ve ölçeğe gerek duyan uygulamalar için idealdir. Uygulamanızın yalıtılmış bir ortamda olmasının yanı sıra, uygulamanızın bir dış IP adresi yük dengeleyicisine sahip olması gerekir. Ao hakkında daha fazla bilgi edinmek için [App Service ortamı belgelerine](../app-service/environment/intro.md)bakın.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu belge adım adım kılavuz değildir.
>
>

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
1. **Öneriler**altında **Web uygulaması güvenlik duvarı 'nı kullanarak güvenli Web uygulaması**' nı seçin.
   ![Güvenli Web uygulaması][1]
2. Web **uygulaması güvenlik duvarını kullanarak Web uygulamalarınızın güvenliğini sağlama**altında bir Web uygulaması seçin. **Web uygulaması güvenlik duvarı ekleme** açılır.
   ![Web uygulaması güvenlik duvarı ekleme][2]
3. Varsa mevcut bir Web uygulaması güvenlik duvarını kullanmayı seçebilirsiniz veya yeni bir tane oluşturabilirsiniz. Bu örnekte, mevcut bir WAFs yok, bu nedenle WAF oluşturuyoruz.
4. Bir WAF oluşturmak için tümleşik iş ortakları listesinden bir çözüm seçin. Bu örnekte, **barbcuda Web uygulaması güvenlik duvarı**' nı seçeceğiz.
5. **Barkcuda Web uygulaması güvenlik duvarı** açılarak, iş ortağı çözümü hakkında bilgi sağlanır. **Oluştur**’u seçin.

   ![Güvenlik duvarı bilgileri dikey penceresi][3]

6. **Yeni Web uygulaması güvenlik duvarı** açılarak **VM yapılandırma** adımlarını gerçekleştirebilir ve **WAF bilgilerini**sağlayabilirsiniz. **VM yapılandırması**' nı seçin.
7. **VM yapılandırması**altında, WAF 'yi çalıştıran sanal makineyi çalıştırmak için gereken bilgileri girin.

   ![VM yapılandırması][4]
   
8. **Yeni Web uygulaması güvenlik duvarı** 'Na dönüp **WAF bilgileri**' ni seçin. **WAF bilgileri**altında WAF 'yi yapılandırırsınız. 7\. adım, WAF 'nin çalıştığı sanal makineyi yapılandırmanıza ve 8. adımda WAF 'nin kendisini sağlamanıza olanak tanır.

## <a name="finalize-application-protection"></a>Uygulama korumasını sonlandırma
1. **Önerilere**geri dönün. WAF oluşturulduktan sonra, **uygulama korumasını Sonlandır**adlı yeni bir giriş oluşturulmuştur. Bu giriş, uygulamayı koruyabilmesi için Azure sanal ağı içindeki WAF 'yi gerçekten bir şekilde yedeklemeniz gerektiğini bilmenizi sağlar.

   ![Uygulama korumasını sonlandırma][5]

2. **Uygulama korumasını Sonlandır**' ı seçin. Yeni bir dikey pencere açılır. Trafiğinin tekrar yönlendirililmesi gereken bir Web uygulaması olduğunu görebilirsiniz.
3. Web uygulamasını seçin. Web uygulaması güvenlik duvarı kurulumunu sonuçlandırmada size olan adımları sağlayan bir dikey pencere açılır. Adımları tamamladıktan sonra **trafiği kısıtla**' yı seçin. Güvenlik Merkezi daha sonra sizin için bağlantı kurma.

   ![Trafiği kısıtlama][6]

> [!NOTE]
> Bu uygulamaları var olan WAF dağıtımlarınıza ekleyerek güvenlik merkezi 'nde birden çok Web uygulamasını koruyabilirsiniz.
>
>

Bu WAF 'deki Günlükler artık tam olarak tümleşiktir. Güvenlik Merkezi, önemli güvenlik uyarılarını bir araya getirmek için günlükleri otomatik olarak toplayıp analiz edebilir.

## <a name="next-steps"></a>Sonraki adımlar
Bu belge, "bir Web uygulaması ekleme" Güvenlik Merkezi önerisini nasıl uygulayacağınızı gösterdi. Web uygulaması güvenlik duvarını yapılandırma hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [App Service Ortamı için Web Uygulaması Güvenlik Duvarı (WAF) Yapılandırma](../app-service/environment/app-service-app-service-environment-web-application-firewall.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) -Azure güvenliği ve uyumluluğu ile ilgili blog gönderilerini bulun.

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
