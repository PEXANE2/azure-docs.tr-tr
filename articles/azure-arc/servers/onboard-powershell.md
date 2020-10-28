---
title: PowerShell kullanarak karma makineleri Azure 'a bağlama
description: Bu makalede, PowerShell 'i kullanarak Azure Arc etkin sunucularını kullanarak aracıyı yüklemeyi ve bir makineyi Azure 'a bağlamayı öğreneceksiniz.
ms.date: 10/27/2020
ms.topic: conceptual
ms.openlocfilehash: bb114ec3e279a7ea696d834af8eb7240cb892dc1
ms.sourcegitcommit: 4064234b1b4be79c411ef677569f29ae73e78731
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92891950"
---
# <a name="connect-hybrid-machines-to-azure-using-powershell"></a>PowerShell kullanarak karma makineleri Azure 'a bağlama

Bir el ile adımlar kümesi gerçekleştirerek ortamınızdaki bir veya daha fazla Windows veya Linux makinesi için Azure Arc etkin sunucularını etkinleştirebilirsiniz. Ya da bağlı makine aracısını indirmek, aracıyı yüklemek ve makineyi Azure Arc 'a kaydetmek için [Connect-AzConnectedMachine](/powershell/module/az.connectedmachine/remove-azconnectedmachine) PowerShell cmdlet 'ini kullanabilirsiniz. Cmdlet 'i, Microsoft Indirme merkezi 'nden Windows Agent Windows Installer paketini ve Microsoft 'un paket deposundan Linux Aracısı paketini indirir.

Bu yöntem, aracıyı yüklemek ve yapılandırmak için makinede yönetici izinlerine sahip olmanızı gerektirir. Linux 'ta, kök hesabı kullanarak ve Windows 'ta, yerel Yöneticiler grubunun üyesi olursunuz. [PowerShell uzaktan](/powershell/scripting/learn/ps101/08-powershell-remoting)iletişimini kullanarak bir Windows Server üzerinde bu işlemi etkileşimli veya uzaktan gerçekleştirebilirsiniz.

Başlamadan önce, [önkoşulları](agent-overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun. Desteklenen bölgeler ve diğer ilgili konular hakkında daha fazla bilgi için bkz. [desteklenen Azure bölgeleri](overview.md#supported-regions).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

- Azure PowerShell olan bir bilgisayar. Yönergeler için bkz. [Azure PowerShell yükleyip yapılandırma](/powershell/azure/).

Yay etkin sunucularla yönetilen karma sunucunuzda VM uzantılarını yönetmek için Azure PowerShell kullanmadan önce modülünü yüklemeniz gerekir `Az.ConnectedMachine` . Şu komutu yay etkin sunucunuzda çalıştırın:

```powershell
Install-Module -Name Az.ConnectedMachine
```

Yükleme tamamlandığında, aşağıdaki ileti döndürülür:

`The installed extension ``Az.ConnectedMachine`` is experimental and not covered by customer support. Please use with discretion.`

## <a name="install-the-agent-and-connect-to-azure"></a>Aracıyı yükleyip Azure 'a bağlanın

1. Yükseltilmiş ayrıcalıklarla bir PowerShell konsolu açın.

2. Komutunu çalıştırarak Azure 'da oturum açın `Connect-AzAccount` .

3. Bağlı makine aracısını yüklemek için,, `Connect-AzConnectedMachine` `-Name` `-ResourceGroupName` ve `-Location` parametreleriyle kullanın. `-SubscriptionId`Oturum açtıktan sonra oluşturulan Azure bağlamının bir sonucu olarak varsayılan aboneliği geçersiz kılmak için parametresini kullanın. Aşağıdaki komutlardan birini çalıştırın:

    * Bağlı makine aracısını doğrudan Azure ile iletişim kurabilen hedef makineye yüklemek için şunu çalıştırın:

    ```azurepowershell
    Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e
    ```
    
    * Bağlı makine aracısını bir ara sunucu üzerinden iletişim kuran hedef makineye yüklemek için şunu çalıştırın:
    
    ```azurepowershell
    Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -SubscriptionId 978ab182-6cf0-4de3-a58b-53c8d0a3235e -proxy http://<proxyURL>:<proxyport>
    ```

Kurulum tamamlandıktan sonra aracı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. *%ProgramData%\AzureConnectedMachineAgent\Log\himds.log* adresinde ve Linux 'ta */var/seçenek/azcmagent/log/hımds.log* konumunda.

## <a name="install-and-connect-using-powershell-remoting"></a>PowerShell uzaktan iletişimini kullanarak yükleyip bağlanma

Hedef Windows sunucusunu veya makineyi Azure Arc etkin sunucularıyla yapılandırmak için aşağıdaki adımları gerçekleştirin. Uzak bilgisayarda PowerShell uzaktan iletişim özelliğinin etkinleştirilmesi gerekir. `Enable-PSRemoting`PowerShell uzaktan iletişimini etkinleştirmek için cmdlet 'ini kullanın.

1. Yönetici olarak bir PowerShell konsolu açın.

2. Komutunu çalıştırarak Azure 'da oturum açın `Connect-AzAccount` .

3. Bağlı makine aracısını yüklemek için,, `Connect-AzConnectedMachine` `-Name` `-ResourceGroupName` ve `-Location` parametreleriyle kullanın. `-SubscriptionId`Oturum açtıktan sonra oluşturulan Azure bağlamının bir sonucu olarak varsayılan aboneliği geçersiz kılmak için parametresini kullanın.

Bağlı makine aracısını doğrudan Azure ile iletişim kurabilen hedef makineye yüklemek için şu komutu çalıştırın:

```azurepowershell
$session = Connect-PSSession -ComputerName myMachineName
Connect-AzConnectedMachine -ResourceGroupName myResourceGroup -Name myMachineName -Location <region> -PSSession $session
```

Komutun sonuçları aşağıdaki örnekte verilmiştir:

```azurepowershell
time="2020-08-07T13:13:25-07:00" level=info msg="Onboarding Machine. It usually takes a few minutes to complete. Sometimes it may take longer depending on network and server load status."
time="2020-08-07T13:13:25-07:00" level=info msg="Check network connectivity to all endpoints..."
time="2020-08-07T13:13:29-07:00" level=info msg="All endpoints are available... continue onboarding"
time="2020-08-07T13:13:50-07:00" level=info msg="Successfully Onboarded Resource to Azure" VM Id=f65bffc7-4734-483e-b3ca-3164bfa42941

Name           Location OSName   Status     ProvisioningState
----           -------- ------   ------     -----------------
myMachineName  eastus   windows  Connected  Succeeded
```

## <a name="verify-the-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yükledikten ve Azure Arc etkin sunucularına bağlanacak şekilde yapılandırdıktan sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. [Azure portalında](https://portal.azure.com) makinelerinizi görüntüleyin.

![Başarılı bir sunucu bağlantısı](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorun giderme bilgileri, [bağlı makine Aracısı sorunlarını giderme kılavuzunda](troubleshoot-agent-onboard.md)bulunabilir.

* VM [Konuk yapılandırması](../../governance/policy/concepts/guest-configuration.md), makinenin beklenen Log Analytics çalışma alanına rapor olduğunu doğrulama, [VM 'lerle Azure izleyici](../../azure-monitor/insights/vminsights-enable-policy.md)ile izlemeyi etkinleştirme ve çok daha birçok şey için [Azure ilkesi](../../governance/policy/overview.md)'ni kullanarak makinenizi yönetmeyi öğrenin.

* [Log Analytics Aracısı](../../azure-monitor/platform/log-analytics-agent.md)hakkında daha fazla bilgi edinin. Windows ve Linux için Log Analytics Aracısı, işletim sistemi ve iş yükü izleme verilerini toplamak, Otomasyon Runbook 'larını veya Güncelleştirme Yönetimi gibi özellikleri kullanarak yönetmek ya da [Azure Güvenlik Merkezi](../../security-center/security-center-introduction.md)gibi diğer Azure hizmetlerini kullanmak istediğinizde gereklidir.