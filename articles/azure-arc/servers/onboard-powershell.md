---
title: PowerShell kullanarak karma makineleri Azure 'a bağlama
description: Bu makalede, PowerShell 'i kullanarak Azure Arc etkin sunucularını kullanarak aracıyı yüklemeyi ve bir makineyi Azure 'a bağlamayı öğreneceksiniz.
ms.date: 10/21/2020
ms.topic: conceptual
ms.openlocfilehash: d36fd174606b49b28b1d8343bff6ccc1f62e5194
ms.sourcegitcommit: 28c5fdc3828316f45f7c20fc4de4b2c05a1c5548
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92376300"
---
# <a name="connect-hybrid-machines-to-azure-using-powershell"></a>PowerShell kullanarak karma makineleri Azure 'a bağlama

El ile bir adım kümesi gerçekleştirerek ortamınızdaki bir veya az sayıda Windows veya Linux makinesi için Azure Arc etkin sunucularını etkinleştirebilirsiniz. Ya da [Connect-AzConnectedMachine](/powershell/module/az.connectedmachine/remove-azconnectedmachine)PowerShell cmdlet 'ini kullanabilirsiniz. Bu cmdlet aşağıdaki eylemleri gerçekleştirir:

- Windows aracısını Microsoft Indirme merkezi 'nden ve packages.microsoft.com adresinden Linux aracı paketinden indirmek için konak makineyi yapılandırır.
- Bağlı makine aracısını yükleme.
- Makineyi Azure Arc 'a kaydeder

Bu yöntem, aracıyı yüklemek ve yapılandırmak için makinede yönetici izinlerine sahip olmanızı gerektirir. Linux 'ta, kök hesabı kullanarak ve Windows 'ta, yerel Yöneticiler grubunun üyesi olursunuz.

Başlamadan önce, [önkoşulları](agent-overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun. Desteklenen bölgeler ve diğer ilgili konular hakkında daha fazla bilgi için bkz. [desteklenen Azure bölgeleri](overview.md#supported-regions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Ön koşullar

- Azure PowerShell olan bir bilgisayar. Yönergeler için bkz. [Azure PowerShell yükleyip yapılandırma](/powershell/azure/).

Yay etkin sunucularla yönetilen karma sunucunuzda VM uzantılarını yönetmek için Azure PowerShell kullanmadan önce modülünü yüklemeniz gerekir `Az.ConnectedMachine` . Şu komutu yay etkin sunucunuzda çalıştırın:

```powershell
Install-Module -Name Az.ConnectedMachine
```

Yükleme tamamlandığında, aşağıdaki ileti döndürülür:

`The installed extension ``Az.ConnectedMachine`` is experimental and not covered by customer support. Please use with discretion.`

## <a name="install-and-validate-the-agent-on-windows"></a>Windows 'da aracıyı yükleyip doğrulama

1. Yönetici olarak bir PowerShell konsolu açın.

2. Komutunu çalıştırarak Azure 'da oturum açın `Connect-AzAccount` .

3. Bağlı makine aracısını yüklemek için,, `Connect-AzConnectedMachine` `-Name` `-ResourceGroupName` ve `-Location` parametreleriyle kullanın. `-SubscriptionId`Oturum açtıktan sonra oluşturulan Azure bağlamının bir sonucu olarak varsayılan aboneliği geçersiz kılmak için parametresini kullanın.

    Bağlı makine aracısını doğrudan Azure ile iletişim kurabilen hedef makineye yüklemek için şu komutu çalıştırın::

    ```azurepowershell
    Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e
    ```
    
    Hedef makine bir ara sunucu üzerinden iletişim kuruyorsa, aşağıdaki komutu çalıştırın:
    
    ```azurepowershell
    Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e -proxy http://<proxyURL>:<proxyport>
    ```

    Kurulum tamamlandıktan sonra aracı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. *%ProgramData%\AzureConnectedMachineAgent\Log\himds.log*adresinde ve Linux 'ta */var/seçenek/azcmagent/log/hımds.log*konumunda.

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve Azure Arc etkin sunucularına bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. [Azure portalında](https://portal.azure.com) makinelerinizi görüntüleyin.

![Başarılı bir sunucu bağlantısı](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorun giderme bilgileri, [bağlı makine Aracısı sorunlarını giderme kılavuzunda](troubleshoot-agent-onboard.md)bulunabilir.

* VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/insights/vminsights-enable-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

* [Log Analytics Aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Windows ve Linux için Log Analytics Aracısı, işletim sistemi ve iş yükü izleme verilerini toplamak, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi özellikleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde gereklidir.