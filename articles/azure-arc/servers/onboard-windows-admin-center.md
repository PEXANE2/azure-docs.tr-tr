---
title: Windows Yönetim Merkezi 'nden karma makineleri Azure 'a bağlama
description: Bu makalede, Windows Yönetim Merkezi 'nden Azure Arc etkin sunucularını kullanarak aracıyı yüklemeyi ve makineleri Azure 'a bağlamayı öğreneceksiniz.
ms.date: 10/12/2020
ms.topic: conceptual
ms.openlocfilehash: d47e3dd4434d719f890b64e4cdfb12a189c0632a
ms.sourcegitcommit: 857859267e0820d0c555f5438dc415fc861d9a6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93133795"
---
# <a name="connect-hybrid-machines-to-azure-from-windows-admin-center"></a>Windows Yönetim Merkezi 'nden karma makineleri Azure 'a bağlama

Bir adım kümesini el ile gerçekleştirerek ortamınızdaki bir veya daha fazla Windows makinesi için Azure Arc etkin sunucularını etkinleştirebilirsiniz. Ya da [Windows yönetici merkezini](/windows-server/manage/windows-admin-center/understand/what-is) , bağlı makine aracısını dağıtmak ve şirket içi sunucularınızı bu aracın dışında herhangi bir adım gerçekleştirmek zorunda kalmadan kaydetmek için de kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Yay etkin sunucular- [önkoşulları](agent-overview.md#prerequisites) gözden geçirin ve aboneliğinizin, Azure hesabınızın ve kaynaklarınızın gereksinimleri karşıladığını doğrulayın.

* Windows Yönetim Merkezi- [Azure tümleştirmesini ](/windows-server/manage/windows-admin-center/azure/azure-integration)dağıtmak ve yapılandırmak için [ortamınızı hazırlama](/windows-server/manage/windows-admin-center/deploy/prepare-environment) gereksinimlerini gözden geçirin.

* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

* Yönetmek istediğiniz hedef Windows sunucularının Azure 'a erişmek için Internet bağlantısı olması gerekir.

### <a name="security"></a>Güvenlik

Bu dağıtım yöntemi, aracıyı yüklemek ve yapılandırmak için hedef Windows makinesinde veya sunucuda yönetici haklarına sahip olmanızı gerektirir. Ayrıca, [**ağ geçidi kullanıcıları**](/windows-server/manage/windows-admin-center/plan/user-access-options#gateway-access-roles) rolünün bir üyesi olmanız gerekir.

## <a name="deploy"></a>Dağıtma

Windows Server 'ı yay özellikli sunucularla yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Windows yönetici Merkezi ' nde oturum açın.

1. **Genel bakış** sayfasındaki bağlantı listesinden, bağlı Windows Server listesinde, bu sunucuya bağlanmak için listeden bir sunucu seçin.

1. Sol bölmeden **Azure hibrit Hizmetleri** ' ni seçin.

1. **Azure hibrit Hizmetleri** sayfasında **Azure hizmetlerini bul** ' u seçin.

1. Azure **hizmetlerini bul** sayfasında, Azure **Arc ile sunucularınızı yönetmek için Azure ilkeleri ve çözümleri 'nden yararlanın** ' ın altında **Ayarla** ' yı seçin.

1. **Sunucular için Settings\azure Arc** sayfasında, Azure 'da kimlik doğrulaması yaptıysanız ve ardından **kullanmaya başlayın** ' ı seçin.

1. **Azure 'a sunucu bağlantısı** sayfasında, aşağıdakileri sağlayın:

    1. **Azure aboneliği** açılan listesinde Azure aboneliğini seçin.
    1. **Kaynak grubu** için, yeni bir kaynak grubu oluşturmak için **Yeni** ' yi seçin veya **kaynak grubu** açılır listesinde, makineyi kaydetmek ve yönetmek için var olan bir kaynak grubunu seçin.
    1. **Bölge** açılan listesinde, sunucu meta verilerini depolamak için Azure bölgesini seçin.
    1. Makine veya sunucu, internet 'e bağlanmak için bir proxy sunucusu üzerinden iletişim kurduğundan, **proxy sunucusu kullan** seçeneğini belirleyin. Proxy sunucusu IP adresini veya adını ve makinenin proxy sunucusuyla iletişim kurmak için kullanacağı bağlantı noktası numarasını belirtin.

1. Windows Server 'ı Azure Arc etkin sunucularıyla yapılandırmaya devam etmek için **Ayarla** ' yı seçin.

Windows Server Azure 'a bağlanır, bağlı makine aracısını indirir, yükler ve Azure Arc etkin sunucularına kaydeder. İlerlemeyi izlemek için menüdeki **Bildirimler** ' i seçin.

Bağlı makine aracısının yüklenmesini onaylamak için, Windows Yönetim Merkezi 'nde, uygulama olay günlüğündeki *Msiınstaller* olaylarını gözden geçirmek için sol bölmedeki [**olayları**](/windows-server/manage/windows-admin-center/use/manage-servers#events) seçin.

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve Azure Arc etkin sunucularına bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. Makinenizde [Azure Portal](https://portal.azure.com)görüntüleyin.

:::image type="content" source="./learn/media/quick-enable-hybrid-vm/enabled-machine.png" alt-text="Başarılı bir makine bağlantısı" border="false":::

## <a name="next-steps"></a>Sonraki adımlar

* Sorun giderme bilgileri, [bağlı makine Aracısı sorunlarını giderme kılavuzunda](troubleshoot-agent-onboard.md)bulunabilir.

* VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/insights/vminsights-enable-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

* [Log Analytics Aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Windows ve Linux için Log Analytics Aracısı, işletim sistemi ve iş yükü izleme verilerini toplamak, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi özellikleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde gereklidir.