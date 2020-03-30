---
title: Windows Sanal Masaüstü sorun giderme genel bakış - Azure
description: Windows Sanal Masaüstü kiracı ortamını ayarlarken sorun giderme sorunlarına genel bakış.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 02/07/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2b9dfb2ee7e74f94bfc3e6d2c679cb7da3d6bc66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127408"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Sorun çözümüne genel bakış, geri bildirim ve destek

Bu makalede, Bir Windows Sanal Masaüstü kiracı ortamı kurarken karşılaşabileceğiniz sorunlara genel bir bakış sağlar ve sorunları çözmek için yollar sağlar.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows Sanal Masaüstü Hizmeti'ni ürün ekibi ve etkin topluluk üyeleriyle tartışmak için [Windows Sanal Masaüstü Teknik Topluluğu'nu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) ziyaret edin.

## <a name="escalation-tracks"></a>Yükseltme parçaları

Uzak Masaüstü istemcisini kullanarak kiracı ortamı ayarlarken karşılaşabileceğiniz sorunları tanımlamak ve çözmek için aşağıdaki tabloyu kullanın. Kiracınız kurulduktan sonra, sık karşılaşılan senaryolarla ilgili sorunları belirlemek için yeni [Tanılama hizmetimizi](diagnostics-role-service.md) kullanabilirsiniz.

>[!NOTE]
> Ürün ekibi ve aktif topluluk üyeleriyle sorunlarınızı tartışmak için ziyaret edebilirsiniz bir Tech Community forumu var. Tartışma başlatmak için [Windows Sanal Masaüstü Teknik Topluluğu'nu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) ziyaret edin.

| **Sorunu**                                                            | **Önerilen Çözüm**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Windows Sanal Masaüstü kiracı oluşturma                                                    | Azure kesintisi varsa, bir [Azure destek isteği açın;](https://azure.microsoft.com/support/create-ticket/) aksi takdirde [bir Azure destek isteği açın,](https://azure.microsoft.com/support/create-ticket/)hizmet için Windows Sanal **Masaüstü'nü** seçin, sorun türü için **Dağıtım'ı** seçin ve ardından sorun alt türü için Windows Sanal **Masaüstü kiracısı oluşturan Sorunlar'ı** seçin.|
| Azure portalında Market şablonlarına erişim       | Azure kesintisi varsa, bir [Azure destek isteği açın.](https://azure.microsoft.com/support/create-ticket/) <br> <br> Azure Marketplace Windows Sanal Masaüstü şablonları serbestçe kullanılabilir.|
| Azure Kaynak Yöneticisi şablonlarına GitHub'dan erişme                                  | Windows Sanal Masaüstü oluşturma [oturumu, Kiracı ve ana bilgisayar havuzu oluşturmanın](troubleshoot-set-up-issues.md) [VM'leri](troubleshoot-set-up-issues.md#creating-windows-virtual-desktop-session-host-vms) barındıran bölüme bakın. Sorun hala çözülmemişse, [GitHub destek ekibine](https://github.com/contact)başvurun. <br> <br> Hata GitHub'da şablona erişinden sonra oluşursa, [Azure Desteği'ne](https://azure.microsoft.com/support/create-ticket/)başvurun.|
| Oturum ana bilgisayar havuzu Azure Sanal Ağ (VNET) ve Ekspres Rota ayarları               | [Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/)ve ardından uygun hizmeti (Ağ kategorisi altında) seçin. |
| Windows Sanal Masaüstü ile sağlanan Azure Kaynak Yöneticisi şablonları kullanılmadığında oturum ana bilgisayar havuzu Sanal Makine (VM) oluşturma | [Azure destek isteği açın](https://azure.microsoft.com/support/create-ticket/)ve ardından hizmet için **Windows çalıştıran Sanal Makine'yi** seçin. <br> <br> Windows Sanal Masaüstü ile sağlanan Azure Kaynak Yöneticisi şablonlarıyla ilgili sorunlar için, [Kiracı ve ana bilgisayar havuzu oluşturmanın](troubleshoot-set-up-issues.md)Windows Sanal Masaüstü kiracı oluşturma bölümüne bakın. |
| Azure portalından Windows Sanal Masaüstü oturum ana bilgisayar ortamını yönetme    | [Azure destek isteği açın.](https://azure.microsoft.com/support/create-ticket/) <br> <br> Uzak Masaüstü Hizmetleri/Windows Sanal Masaüstü PowerShell kullanırken yönetim sorunları için Windows [Virtual Desktop PowerShell'e](troubleshoot-powershell.md) bakın veya [bir Azure destek isteği açın,](https://azure.microsoft.com/support/create-ticket/)hizmet için **Windows Sanal Masaüstü'nü** seçin, sorun türü için Yapılandırma **ve yönetim** seçin ve ardından sorun alt türü için **PowerShell'i kullanarak kiracıyı yapılandıran Sorunları** seçin. |
| Ana bilgisayar havuzlarına ve uygulama gruplarına bağlı Windows Sanal Masaüstü yapılandırmasını yönetme (uygulama grupları)      | [Windows Virtual Desktop PowerShell'e](troubleshoot-powershell.md)bakın veya bir Azure destek isteği [açın,](https://azure.microsoft.com/support/create-ticket/)hizmet için **Windows Sanal Masaüstü'nü** seçin ve ardından uygun sorun türünü seçin.|
| FSLogix Profil Konteynerlerini dağıtma ve yönetme | [FSLogix ürünleri için Sorun Giderme kılavuzuna](/fslogix/fslogix-trouble-shooting-ht/) bakın ve bu sorunu çözmüyorsa, [Azure destek isteği açın,](https://azure.microsoft.com/support/create-ticket/)hizmet için **Windows Sanal Masaüstü'nü** seçin, sorun türü için **FSLogix'i** seçin ve ardından uygun sorun alt türünü seçin. |
| Başlangıçta uzak masaüstü istemcileri arızası                                                 | Bkz. [Uzak Masaüstü istemcisi sorun giderme](troubleshoot-client.md) ve bu sorunu çözmüyorsa, [Azure destek isteği açın,](https://azure.microsoft.com/support/create-ticket/)hizmet için **Windows Sanal Masaüstü'nü** seçin ve sorun türü için Uzak **Masaüstü istemcilerini** seçin.  <br> <br> Bu bir ağ sorunuysa, kullanıcılarınızın ağ yöneticilerine başvurması gerekir. |
| Bağlı ama besleme yok                                                                 | Kullanıcı'yı kullanarak sorun giderme bağlanır, ancak [Windows Sanal Masaüstü hizmet bağlantılarının](troubleshoot-service-connection.md)hiçbir bölümü [görüntülenmez (özet akışı yok).](troubleshoot-service-connection.md#user-connects-but-nothing-is-displayed-no-feed) <br> <br> Kullanıcılarınız bir uygulama grubuna atanmışsa, [bir Azure destek isteği açın,](https://azure.microsoft.com/support/create-ticket/)hizmet için Windows Sanal **Masaüstü'nü** seçin ve sorun türü için Uzak **Masaüstü İstemcileri'ni** seçin. |
| Ağ nedeniyle akış bulma sorunları                                            | Kullanıcılarınızın ağ yöneticilerine başvurması gerekir. |
| Bağlantı istemcileri                                                                    | [Windows Sanal Masaüstü hizmet bağlantılarına](troubleshoot-service-connection.md) bakın ve sorununuzu çözmüyorsa Oturum ana bilgisayar sanal makine [yapılandırması'na](troubleshoot-vm-configuration.md)bakın. |
| Uzak uygulamaların veya masaüstünün yanıt verme                                      | Sorunlar belirli bir uygulamaya veya ürüne bağlıysa, bu üründen sorumlu ekiple iletişime geçin. |
| İletileri veya hataları lisanslama                                                          | Sorunlar belirli bir uygulamaya veya ürüne bağlıysa, bu üründen sorumlu ekiple iletişime geçin. |
| GitHub'da Windows Sanal Masaüstü araçlarını kullanırken sorunlar (Azure Kaynak Yöneticisi şablonları, tanılama aracı, yönetim aracı) | Sorunları bildirmek [için Uzak Masaüstü Hizmetleri için Azure Kaynak Yöneticisi Şablonları'na](https://github.com/Azure/RDS-Templates/blob/master/README.md) bakın. |

## <a name="next-steps"></a>Sonraki adımlar

- Windows Sanal Masaüstü ortamında kiracı ve ana bilgisayar havuzu oluştururken sorunları gidermek için [Bkz. Kiracı ve ana bilgisayar havuzu oluşturma.](troubleshoot-set-up-issues.md)
- Windows Sanal Masaüstü'nde sanal makine (VM) yapılandırırken sorunları gidermek için [Oturum ana bilgisayar sanal makine yapılandırmasına](troubleshoot-vm-configuration.md)bakın.
- Windows Sanal Masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için [Windows Sanal Masaüstü hizmet bağlantılarına](troubleshoot-service-connection.md)bakın.
- Uzak Masaüstü istemcileriyle ilgili sorunları gidermek için Bkz. [Uzak Masaüstü istemcisi sorun giderme](troubleshoot-client.md)
- Windows Virtual Desktop ile PowerShell kullanırken sorunları gidermek için [Windows Virtual Desktop PowerShell'e](troubleshoot-powershell.md)bakın.
- Hizmet hakkında daha fazla bilgi edinmek için [Windows Sanal Masaüstü ortamına](environment-setup.md)bakın.
- Bir sorun giderme öğreticisine geçmek için [Bkz. Öğretici: Kaynak Yöneticisi şablonu dağıtımları.](../azure-resource-manager/templates/template-tutorial-troubleshoot.md)
- Denetim eylemleri hakkında bilgi edinmek için [Kaynak Yöneticisi ile Denetim işlemlerine](../azure-resource-manager/management/view-activity-logs.md)bakın.
- Dağıtım sırasında hataları belirlemek için eylemler hakkında bilgi edinmek için [bkz.](../azure-resource-manager/templates/deployment-history.md)
