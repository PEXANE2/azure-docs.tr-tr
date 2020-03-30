---
title: Azure'da giden SMTP bağlantısıyla sorun giderme | Microsoft Dokümanlar
description: Azure'da giden SMTP bağlantısı sorunlarını nasıl gidereceklerini öğrenin.
services: virtual-network
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2018
ms.author: genli
ms.openlocfilehash: b8acb50978c5932fe6e6838be86b65c12a0984ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71058947"
---
# <a name="troubleshoot-outbound-smtp-connectivity-issues-in-azure"></a>Azure'da giden SMTP bağlantısı sorunlarını giderme

15 Kasım 2017'den itibaren, sanal bir makineden (VM) doğrudan dış etki adlarına (outlook.com ve gmail.com gibi) gönderilen giden e-posta iletileri yalnızca Microsoft Azure'daki belirli abonelik türlerine sunulur. TCP bağlantı noktası 25 kullanan giden SMTP bağlantıları engellendi. (Bağlantı noktası 25 öncelikle kimliği doğrulanmamış e-posta teslimi için kullanılır.)

Davranıştaki bu değişiklik yalnızca 15 Kasım 2017'den bu yana yeni abonelikler ve yeni dağıtımlar için geçerlidir.

## <a name="recommended-method-of-sending-email"></a>Önerilen e-posta gönderme yöntemi
Azure VM'lerden veya Azure Uygulama Hizmetlerinden e-posta göndermek için kimlik doğrulaması verilen SMTP geçiş hizmetlerini (genellikle TCP port 587 veya 443 üzerinden bağlanan ancak diğer bağlantı noktalarını da destekleyen) kullanmanızı öneririz. Bu hizmetler, üçüncü taraf e-posta sağlayıcılarının iletiyi reddetme olasılığını en aza indirmek için IP veya etki alanı itibarını korumak için kullanılır. Bu tür SMTP röle hizmetleri [SendGrid](https://sendgrid.com/partners/azure/)içerir ancak bunlarla sınırlı değildir. Ayrıca, kullanabileceğiniz şirket içinde çalışan güvenli bir SMTP röle hizmetiniz de olabilir.

Bu e-posta teslim hizmetlerini kullanmak, abonelik türünden bağımsız olarak Azure'da kısıtlanmaz.

## <a name="enterprise-agreement"></a>Kurumsal Anlaşma
Kurumsal Sözleşme Azure kullanıcıları için, kimlik doğrulaması röle kullanmadan e-posta gönderme teknik yeteneğinde bir değişiklik yoktur. Hem yeni hem de mevcut Enterprise Agreement kullanıcıları, Azure platformundan herhangi bir kısıtlama olmaksızın Azure VM'lerinden doğrudan harici e-posta sağlayıcılarına giden e-posta dağıtımını deneyebilir. E-posta sağlayıcılarının herhangi bir kullanıcıdan gelen e-postaları kabul edeceği garanti edilmese de, teslim girişimleri Kurumsal Sözleşme abonelikleri içindeki VM'ler için Azure platformu tarafından engellenmez. Belirli sağlayıcıları içeren ileti teslimini veya SPAM filtreleme sorunlarını gidermek için doğrudan e-posta sağlayıcılarıyla çalışmanız gerekir.

## <a name="pay-as-you-go"></a>Kullandıkça Öde
15 Kasım 2017'den önce You-You-Go Veya Microsoft İş Ortağı Ağı abonelik teklifleri için kaydolduysanız, giden e-posta dağıtımını deneme teknik yeteneğinde bir değişiklik olmayacaktır. Bu abonelikler dahilinde Azure VM'lerinden gelen giden e-posta dağıtımını doğrudan Azure platformundan herhangi bir kısıtlama olmaksızın harici e-posta sağlayıcılarına deneyebilirsiniz. Yine, e-posta sağlayıcılarının herhangi bir kullanıcıdan gelen e-postaları kabul edeceği garanti edilmez ve kullanıcıların belirli sağlayıcıları içeren ileti teslimini veya SPAM filtreleme sorunlarını gidermek için doğrudan e-posta sağlayıcılarıyla çalışmak zorunda kalacaktır.

15 Kasım 2017'den sonra oluşturulan You-You-Go Veya Microsoft İş Ortağı Ağı abonelikleri için, bu abonelikler içinde doğrudan VM'lerden gönderilen e-postaları engelleyen teknik kısıtlamalar olacaktır. Azure VM'lerden doğrudan harici e-posta sağlayıcılarına e-posta gönderme olanağı istiyorsanız (kimlik doğrulamalı Bir SMTP geçişi kullanmıyorsanız), kısıtlamayı kaldırmak için istekte bulunabilirsiniz. İstekler Microsoft'un takdirine bağlı olarak gözden geçirilir ve onaylanır ve bunlara yalnızca ek dolandırıcılıkla mücadele denetimleri yapıldıktan sonra verilir. İstekte bulunmak için aşağıdaki sorun türünü kullanarak bir destek örneği açın: **Teknik** > **Sanal Ağ** > **Bağlantısı** > **e-posta gönderemez (SMTP/Port 25)**. Dağıtımınızın kimlik doğrulaması bir röle kullanmak yerine neden doğrudan posta sağlayıcılarına posta göndermesi gerektiğine ilişkin ayrıntıları eklediğinizden emin olun.

Bir You-As-You-Go veya Microsoft İş Ortağı Ağı aboneliği muaf olduktan ve VM'ler Azure portalından 'Durduruldu' & 'Başlatıldı' sonra, bu abonelikteki tüm VM'ler ileriye dönük olarak muaf tutulur. Bu muafiyet yalnızca talep edilen abonelik için geçerlidir ve yalnızca doğrudan internete yönlendirilen Sanal Makine trafiği için geçerlidir. [Azure Güvenlik Duvarı](https://azure.microsoft.com/services/azure-firewall/) gibi Azure PaaS hizmetleri üzerinden bağlantı noktası 25 trafiğini yönlendirme desteklenmez.

> [!NOTE]
> Microsoft, hizmet şartlarının ihlal edildiğinin tespit edilmesi halinde bu muafiyeti iptal etme hakkını saklı tutar.

## <a name="msdn-azure-pass-azure-in-open-education-bizspark-and-free-trial"></a>MSDN, Azure Pass, Açık, Eğitim, BizSpark ve Ücretsiz Deneme de Azure
15 Kasım 2017'den sonra MSDN, Azure Pass, Azure açık, eğitimde Azure, BizSpark, Azure Sponsorluğunda, Azure Öğrenci, Ücretsiz Deneme veya herhangi bir Visual Studio aboneliği oluşturduysanız, bu kapsamdaki VM'lerden gönderilen e-postaları engelleyen teknik kısıtlamalar alabilirsiniz abonelikleri doğrudan e-posta sağlayıcılarına aktarır. Kısıtlamalar kötüye kullanımı önlemek için yapılır. Bu kısıtlamayı kaldırmak için hiçbir istek kabul edilecektir.

Bu abonelik türlerini kullanıyorsanız, bu makalede daha önce belirtildiği gibi SMTP geçiş hizmetlerini kullanmanız veya abonelik türünüzü değiştirmeniz için teşvik edilirsiniz.

## <a name="cloud-service-provider-csp"></a>Bulut Hizmet Sağlayıcısı (CSP)

Azure kaynaklarını CSP üzerinden kullanıyorsanız, güvenli bir SMTP rölesi kullanılamazsa CSP'den Microsoft ile engellemeyi kaldırma isteği oluşturmasını isteyebilirsiniz.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa, aşağıdaki sorun türünü kullanarak sorununuzu hızlı bir şekilde çözmek için [desteğe başvurun:](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) **Abonelik Yönetimi** Sorunu türü: Bağlantı **Noktası 25 e-posta akışını etkinleştirme isteği.**
