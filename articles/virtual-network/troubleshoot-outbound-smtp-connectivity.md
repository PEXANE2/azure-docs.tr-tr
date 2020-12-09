---
title: Azure 'da giden SMTP bağlantısı sorunlarını giderme | Microsoft Docs
description: E-posta göndermek için önerilen yöntemi ve Azure 'da giden SMTP bağlantısıyla ilgili sorunları nasıl giderebileceğinizi öğrenin.
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
ms.openlocfilehash: d2e5996da5a1fe3f5b154d57ee509f25e54e30ac
ms.sourcegitcommit: 21c3363797fb4d008fbd54f25ea0d6b24f88af9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96862369"
---
# <a name="troubleshoot-outbound-smtp-connectivity-issues-in-azure"></a>Azure 'da giden SMTP bağlantısı sorunlarını giderme

15 Kasım 2017 tarihinden itibaren, bir sanal makineden (VM) doğrudan dış etki alanlarına (outlook.com ve gmail.com) gönderilen giden e-posta iletileri yalnızca Microsoft Azure içindeki belirli Abonelik türleri için kullanılabilir hale getirilir. TCP bağlantı noktası 25 kullanan giden SMTP bağlantıları engellendi. (Bağlantı noktası 25 öncelikle kimliği doğrulanmamış e-posta teslimi için kullanılır.)

Bu davranış değişikliği, 15 Kasım 2017 ' den beri yalnızca yeni abonelikler ve Yeni dağıtımlar için geçerlidir.

## <a name="recommended-method-of-sending-email"></a>E-posta göndermek için önerilen yöntem

Azure VM 'lerinden veya Azure Uygulama hizmetlerinden e-posta göndermek için kimliği doğrulanmış SMTP geçiş hizmetlerini (genellikle TCP bağlantı noktası 587 veya 443 üzerinden bağlantı, ancak diğer bağlantı noktalarını da destekler) kullanmanızı öneririz. Bu hizmetler, üçüncü taraf e-posta sağlayıcılarının iletiyi reddetme olasılığını en aza indirmek için IP veya etki alanı saygınlığını sürdürmek üzere kullanılır. Bu tür SMTP geçiş Hizmetleri, [SendGrid](https://sendgrid.com/partners/azure/)'e dahil değildir ancak bunlarla sınırlı değildir. Ayrıca, şirket içi olarak çalışan ve kullanabileceğiniz güvenli bir SMTP geçiş hizmetiniz de mümkündür.

Bu e-posta teslim hizmetlerinin kullanılması, abonelik türünden bağımsız olarak Azure 'da kısıtlı değildir.

## <a name="enterprise-agreement"></a>Kurumsal Anlaşma

Kurumsal Anlaşma Azure kullanıcıları için, kimlik doğrulamalı geçiş kullanmadan e-posta gönderme teknik özelliğinin bir değişikliği yoktur. Hem yeni hem de mevcut Kurumsal Anlaşma kullanıcılar Azure platformundan herhangi bir kısıtlama olmadan doğrudan Azure VM 'lerinden giden e-posta teslimini doğrudan dış e-posta sağlayıcılarına deneyebilir. E-posta sağlayıcılarının belirli bir kullanıcıdan gelen e-postayı kabul edeceğini garanti etmese de, teslim denemeleri Kurumsal Anlaşma abonelikleri içindeki VM 'Ler için Azure platformu tarafından engellenmeyecektir. Herhangi bir ileti teslimini veya belirli sağlayıcıları içeren istenmeyen posta filtreleme sorunlarını gidermek için doğrudan e-posta sağlayıcılarıyla çalışmanız gerekir.

## <a name="pay-as-you-go"></a>Kullandıkça Öde

Kullandıkça Öde aboneliği için 15 Kasım 2017 tarihinden önce kaydolduysanız, giden e-posta teslimini deneyebilme konusunda teknik bir değişiklik olmayacaktır. Azure platformundan herhangi bir kısıtlama olmadan bu aboneliklerdeki Azure VM 'lerinden gelen e-posta teslimini doğrudan dış e-posta sağlayıcılarına deneyebileceksiniz. Bu, e-posta sağlayıcılarının belirli bir kullanıcıdan gelen e-postayı kabul edeceğini garanti etmez ve kullanıcıların herhangi bir ileti teslimini veya belirli sağlayıcıları içeren istenmeyen posta filtreleme sorunlarını gidermek için doğrudan e-posta sağlayıcılarıyla çalışması gerekecektir.

15 Kasım 2017 ' den sonra oluşturulan Kullandıkça Öde abonelikleri için, bu abonelikler içindeki VM 'lerden doğrudan gönderilen e-postayı engelleyen teknik kısıtlamalar olacaktır. Azure VM 'lerinden doğrudan dış e-posta sağlayıcılarına (kimliği doğrulanmış SMTP geçişi kullanmadan) e-posta gönderebilme olanağı istiyorsanız ve bir hesap ödeme geçmişi ile iyi bir şekilde devam ediyorsa, Azure portal bir Azure sanal ağ kaynağı için **Tanılama ve çözme** dikey penceresinin **bağlantı** bölümünde kısıtlamayı kaldırma isteği alabilirsiniz. Nitelikli ise, aboneliğiniz etkinleştirilir veya sonraki adımlarla ilgili yönergeler alırsınız. 

Kullandıkça Öde aboneliği dışarıda bırakılır ve VM 'Ler Azure portal durdurulduğunda ve başlatıldıktan sonra, Bu abonelikteki tüm VM 'Ler ileri doğru tutulur. Muafiyet yalnızca, istenen abonelik ve yalnızca doğrudan internet 'e yönlendirilen VM trafiği için geçerlidir.

> [!NOTE]
> Microsoft, hizmet koşullarının ihlal edildiğini tespit ettiyse bu muafiyeti iptal etme hakkını saklı tutar.

## <a name="msdn-azure-pass-azure-in-open-education-azure-for-students-vistual-studio-and-free-trial"></a>MSDN, Azure Pass, Open ile Azure, eğitim, öğrenciler için Azure, Viziya ve ücretsiz deneme

15 Kasım 2017 ' den sonra bir MSDN, Azure Pass, Open ile Azure, eğitim, Azure öğrenci, ücretsiz deneme veya herhangi bir Visual Studio aboneliği oluşturduysanız, bu aboneliklerdeki VM 'lerden gönderilen e-postayı doğrudan e-posta sağlayıcılarına da engelleyen teknik kısıtlamalara sahip olursunuz. Kötüye kullanımı engellemek için kısıtlamalar yapılır. Bu kısıtlamayı kaldırma isteği verilecek.

Bu abonelik türlerini kullanıyorsanız, bu makalenin önceki kısımlarında açıklandığı gibi SMTP geçiş hizmetlerini kullanmanız veya abonelik türünü değiştirmeniz önerilir.

## <a name="cloud-service-provider-csp"></a>Bulut hizmeti sağlayıcısı (CSP)

Azure kaynaklarını CSP aracılığıyla kullanıyorsanız, Azure portalındaki bir sanal ağ kaynağı için **Tanılama ve çözme** dikey penceresinin **bağlantı** bölümünde kısıtlamayı kaldırmak için bir istek yapabilirsiniz. Nitelikli ise, aboneliğiniz etkinleştirilir veya sonraki adımlarla ilgili yönergeler alırsınız.

## <a name="microsoft-partner-network-mpn-bizspark-plus-or-azure-sponsorship"></a>Microsoft İş Ortağı Ağı (MPN), BizSpark Plus veya Azure Sponsorluğu

15 Kasım 2017 ' den sonra oluşturulan Microsoft İş Ortağı Ağı (MPN), BizSpark Plus veya Azure Sponsorluğu abonelikleri için, bu abonelikler içindeki VM 'lerden doğrudan gönderilen e-postaları engelleyen teknik kısıtlamalar olacaktır. Azure VM 'lerinden doğrudan dış e-posta sağlayıcılarına (kimliği doğrulanmış SMTP geçişi kullanmadan) e-posta gönderebilme olanağı istiyorsanız, aşağıdaki sorun türünü kullanarak bir destek talebi açarak bir istek yapabilirsiniz: **Teknik**  >  **sanal ağ**  >  **bağlantısı**  >  **e-posta gönderemez (SMTP/bağlantı noktası 25)**. Dağıtımınızın neden kimliği doğrulanmış bir geçiş kullanmak yerine doğrudan posta sağlayıcılarına e-posta göndermek zorunda olduğuna ilişkin ayrıntıları eklediğinizden emin olun. İstekler, Microsoft 'un yanı sıra incelenip onaylanacaktır. İstekler yalnızca, ek sahtekarlığı önleme denetimleri tamamlandıktan sonra verilebilir. 

Bir abonelik dışlandıktan ve VM 'Ler Azure portal durdurulduğunda ve başlatıldıktan sonra, Bu abonelikteki tüm VM 'Ler ileri doğru tutulur. Muafiyet yalnızca, istenen abonelik ve yalnızca doğrudan internet 'e yönlendirilen VM trafiği için geçerlidir.

## <a name="restrictions-and-limitations"></a>Kısıtlamalar ve sınırlamalar

- [Azure Güvenlik Duvarı](https://azure.microsoft.com/services/azure-firewall/) gibi Azure PaaS hizmetleri aracılığıyla yönlendirme bağlantı noktası 25 trafiği desteklenmez.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa, aşağıdaki sorun türünü kullanarak sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) : **Teknik**  >  **sanal ağ**  >  **bağlantısı**  >  **e-posta gönderemez (SMTP/bağlantı noktası 25)**.
