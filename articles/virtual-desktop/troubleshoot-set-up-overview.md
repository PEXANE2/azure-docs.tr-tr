---
title: Windows sanal masaüstü sorunlarını gidermeye genel bakış-Azure
description: Windows sanal masaüstü ortamı ayarlanırken sorun gidermeye yönelik genel bakış.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 06/05/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 07857feb2e5552429c445b22c4c4b2f121f29a93
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84976498"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Sorun çözümüne genel bakış, geri bildirim ve destek

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/troubleshoot-set-up-overview-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, bir Windows sanal masaüstü ortamı ayarlarken karşılaşabileceğiniz sorunlara genel bir bakış sağlanır ve sorunları çözmeye yönelik yollar sağlanır.

## <a name="report-issues-during-public-preview"></a>Genel Önizleme sırasında sorunları raporla

Spring 2020 sürümünün genel önizlemesi sırasında sorunları bildirmek veya özellik önermek için [Windows sanal masaüstü teknik topluluğu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop)' nu ziyaret edin. En iyi uygulamaları tartışmak veya yeni özellikleri önermek ve Oylamak için Tech Community 'yi kullanabilirsiniz. Genel önizleme sürümü ile ilgili bir sorun raporlerken, sorun türünü **Spring 2020 Update (Önizleme)** olarak etiketlediğinizden emin olun.

Yardım isteyen bir gönderi yaptığınızda veya yeni bir özellik önerdiğinde, konuyu mümkün olduğunca ayrıntılı olarak tanımladığınızdan emin olun. Ayrıntılı bilgiler, diğer kullanıcıların sorularınızı yanıtlamasını veya oy önerdiğinize ilişkin özelliği anlamanıza yardımcı olabilir.

## <a name="escalation-tracks"></a>Yükseltme izleri

Başka bir şey yapmadan önce, Azure hizmetinizin düzgün çalıştığından emin olmak için Azure [durum sayfasını](https://status.azure.com/status) ve [Azure hizmet durumunu](https://azure.microsoft.com/features/service-health/) kontrol ettiğinizden emin olun.

Uzak Masaüstü istemcisi 'ni kullanarak bir ortam ayarlarken karşılaşabileceğiniz sorunları belirlemek ve çözmek için aşağıdaki tabloyu kullanın. Ortamınız kurulduktan sonra, yaygın senaryolara yönelik sorunları belirlemek için yeni [Tanılama hizmetimizi](diagnostics-role-service.md) kullanabilirsiniz.

| **Konuda**                                                            | **Önerilen çözüm**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Oturum Ana bilgisayar havuzu Azure sanal ağ (VNET) ve hızlı rota ayarları               | [Bir Azure destek Isteği açın](https://azure.microsoft.com/support/create-ticket/)ve uygun hizmeti seçin (ağ kategorisi altında). |
| Windows sanal masaüstü ile birlikte sunulan Azure Resource Manager şablonları kullanılmıyorsa, oturum ana bilgisayar havuzu sanal makinesi (VM) oluşturma | [Bir Azure destek Isteği açın](https://azure.microsoft.com/support/create-ticket/)ve ardından hizmet Için **Windows sanal masaüstü** ' nü seçin. <br> <br> Windows sanal masaüstü ile birlikte sunulan Azure Resource Manager şablonlarıyla ilgili sorunlar için, bkz. [konak havuzu oluşturma](troubleshoot-set-up-issues.md)'nın Azure Resource Manager şablon hataları bölümü. |
| Azure portal Windows sanal masaüstü oturumu ana bilgisayarı ortamını yönetme    | [Bir Azure destek Isteği açın](https://azure.microsoft.com/support/create-ticket/). <br> <br> Uzak Masaüstü Hizmetleri/Windows sanal masaüstü PowerShell 'i kullanırken yönetim sorunları için, bkz. [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md) veya [bir Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/), hizmet için **Windows sanal masaüstü** ' nü seçin, sorun türü için **yapılandırma ve yönetim** ' i seçin, ardından sorun alt türü için **PowerShell 'i kullanarak ortamı yapılandırma sorunları** ' nı seçin. |
| Konak havuzlarına ve uygulama gruplarına bağlı Windows sanal masaüstü yapılandırmasını yönetme (uygulama grupları)      | [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md)'e bakın veya [bir Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/), hizmet Için **Windows sanal masaüstü 'nü** seçip ilgili sorun türünü seçin.|
| FSLogix profil kapsayıcılarını dağıtma ve yönetme | Bkz. [FSLogix ürünleri Için sorun giderme kılavuzu](/fslogix/fslogix-trouble-shooting-ht/) ve bu sorunu çözmezse, [bir Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/), hizmet için **Windows sanal masaüstü** ' nü seçin, sorun türü için **fslogix** ' i seçin ve ardından uygun sorun alt türünü seçin. |
| Başlangıç sırasında uzak masaüstü istemcileri hatalı çalışma                                                 | Bkz. [Uzak Masaüstü Istemcisinde sorun giderme](troubleshoot-client.md) ve bu sorunu çözmezse, [bir Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/), hizmet için **Windows sanal masaüstü** ' nü seçin ve ardından sorun türü için **Uzak Masaüstü istemcileri** ' ni seçin.  <br> <br> Ağ sorunu varsa, kullanıcılarınızın ağ yöneticisiyle iletişim kurabilmesi gerekir. |
| Bağlı ancak akış yok                                                                 | [Windows sanal masaüstü hizmeti bağlantıları](troubleshoot-service-connection.md)'nın Kullanıcı bağlantılarını kullanarak sorun giderme, [ancak hiçbir şey (akış yok) bölümü görüntülenir](troubleshoot-service-connection.md#user-connects-but-nothing-is-displayed-no-feed) . <br> <br> Kullanıcılarınız bir uygulama grubuna atandıysa, [bir Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/), hizmet Için **Windows sanal masaüstü** ' nü seçin ve ardından sorun türü için **Uzak Masaüstü istemcileri** ' ni seçin. |
| Ağ nedeniyle akış bulma sorunları                                            | Kullanıcılarınızın ağ yöneticisiyle iletişim kurabilmesi gerekir. |
| İstemcileri bağlama                                                                    | Bkz. [Windows sanal masaüstü hizmeti bağlantıları](troubleshoot-service-connection.md) ve bu sorunu çözmezse, bkz. [oturum ana bilgisayar sanal makine yapılandırması](troubleshoot-vm-configuration.md). |
| Uzak uygulama veya masaüstü yanıtlama hızı                                      | Sorunlar belirli bir uygulama veya ürüne bağlıysa, bu üründen sorumlu olan ekibe başvurun. |
| Lisanslama iletileri veya hataları                                                          | Sorunlar belirli bir uygulama veya ürüne bağlıysa, bu üründen sorumlu olan ekibe başvurun. |
| Üçüncü taraf kimlik doğrulama yöntemleriyle ilgili sorunlar | Üçüncü taraf sağlayıcınızın Windows sanal masaüstü senaryolarını desteklediğini ve bilinen sorunlarla ilgili olarak bunları yaklaşımını doğrulayın. |
| Windows sanal masaüstü için Log Analytics kullanan sorunlar | Tanılama şeması ile ilgili sorunlar için [bir Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/).<br><br>Log Analytics sorguları, görselleştirme veya diğer sorunlar için Log Analytics altında uygun sorun türünü seçin. |
| M365 uygulamaları kullanma sorunları | [M365 Yönetim Merkezi yardım seçeneklerinden](/microsoft-365/admin/contact-support-for-business-products/)biriyle M365 yönetim merkezine başvurun. |

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ortamında bir konak havuzu oluştururken oluşan sorunları gidermek için bkz. [konak havuzu oluşturma](troubleshoot-set-up-issues.md).
- Windows sanal masaüstündeki bir sanal makineyi (VM) yapılandırırken oluşan sorunları gidermek için bkz. [oturum ana bilgisayarı sanal makine yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için bkz. [Windows sanal masaüstü hizmeti bağlantıları](troubleshoot-service-connection.md).
- Uzak Masaüstü istemcileriyle ilgili sorunları gidermek için bkz [. uzak masaüstü Istemcisinde sorun giderme](troubleshoot-client.md)
- Windows sanal masaüstü ile PowerShell kullanırken karşılaşılan sorunları gidermek için bkz. [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Hizmet hakkında daha fazla bilgi edinmek için bkz. [Windows sanal masaüstü ortamı](environment-setup.md).
- Sorun giderme öğreticisini öğrenmek için bkz. [öğretici: Kaynak Yöneticisi şablonu dağıtımlarının sorunlarını giderme](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Denetim eylemleri hakkında bilgi edinmek için bkz. [Kaynak Yöneticisi Ile denetim işlemleri](../azure-resource-manager/management/view-activity-logs.md).
- Dağıtım sırasında hataları belirleme eylemleri hakkında bilgi edinmek için bkz. [dağıtım Işlemlerini görüntüleme](../azure-resource-manager/templates/deployment-history.md).
