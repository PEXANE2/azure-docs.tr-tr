---
title: Karma makineleri Azure portal Azure 'a bağlama
description: Bu makalede, Azure portal ' dan sunucular için Azure Arc (Önizleme) kullanarak aracıyı yüklemeyi ve makineleri Azure 'a bağlamayı öğreneceksiniz.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 01/29/2020
ms.custom: mvc
ms.topic: quickstart
ms.openlocfilehash: 165ed8d0aad7011f1ea71ff870ee4629f1f12613
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76898608"
---
# <a name="connect-hybrid-machines-to-azure-from-the-azure-portal"></a>Karma makineleri Azure portal Azure 'a bağlama

Bir adım kümesini el ile gerçekleştirerek veya sağladığımız bir şablon betiği çalıştırarak otomatik bir yöntem kullanarak ortamınızdaki bir veya az sayıda Windows ya da Linux makinesi için sunucu (Önizleme) için Azure yayı 'yi etkinleştirebilirsiniz. Bu betik, her iki aracısının de indirilmesini ve yüklenmesini otomatikleştirir.

Bu yükleme yöntemi, aracıyı yüklemek ve yapılandırmak için makinede yönetici haklarına sahip olmanızı gerektirir. Linux 'ta, kök hesabı ve Windows 'ta, yerel Yöneticiler grubunun üyesi olursunuz.

Başlamadan önce, [önkoşulları](overview.md#prerequisites) gözden geçirdiğinizden ve aboneliğinizin ve kaynaklarınızın gereksinimleri karşıladığından emin olun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="generate-install-script-from-the-azure-portal"></a>Azure portal 'den Install betiği oluştur

Azure Arc ile bağlantıyı indirme, yükleme ve oluşturma işlemlerini otomatik hale getirmeye yönelik betik Azure portal bulunabilir. Aşağıdaki adımlarda, bu işlemin nasıl tamamlanacağı açıklanır.

1. Tarayıcınızdan [https://aka.ms/hybridmachineportal](https://aka.ms/hybridmachineportal)başlatın.

2. **Makineler-Azure yay** sayfasında sol üst köşede **+ Ekle** ' yi seçin ya da orta bölmenin altından **makine-Azure yay oluştur** seçeneğini belirleyin. 

3. **Bir yöntem seçin** sayfasında, **etkileşimli betik kutucuğu kullanarak makine Ekle** **betiği oluştur**' u seçin.

4. **Betik oluştur** sayfasında, makinenin Azure 'da yönetilmesini istediğiniz aboneliği ve kaynak grubunu seçin. Makine meta verilerinin depolanacağı Azure konumunu seçin.

    >[!NOTE]
    >Sunucular için Azure Arc (Önizleme), yalnızca aşağıdaki bölgeleri destekler:
    >- WestUS2
    >- WestEurope
    >- Westasıya
    >

5. **Betik oluştur** sayfasında, **işletim sistemi** açılır listesi altında, betiğin üzerinde çalıştırılacağı uygun işletim sistemini seçin.

6. Makine, Internet 'e bağlanmak için bir proxy sunucusu üzerinden iletişim kurduğundan, sonraki seçeneği seçin **: proxy sunucusu >** . **Proxy sunucusu** sekmesinde, makinenin proxy sunucusuyla iletişim kurmak için kullanacağı proxy sunucu IP adresini veya adını ve bağlantı noktası numarasını belirtin. `http://<proxyURL>:<proxyport>`biçimini izleyen değeri girin. Tamamlandığında, **gözden geçir + oluştur**' u seçin.  Aksi takdirde, adımları gerçekleştirmek için **gözden geçir + oluştur** ' u seçin.

7. **Gözden geçir + oluştur** sekmesinde Özet bilgilerini gözden geçirin ve ardından **İndir**' i seçin. Aksi takdirde, değişiklikler yapmanız gerekiyorsa, **önceki**seçeneğini belirleyebilirsiniz.

## <a name="install-and-validate-the-agent-on-windows"></a>Windows 'da aracıyı yükleyip doğrulama

### <a name="install-manually"></a>El ile yükleme

Windows Installer yükleme paketini indirerek ve hedef sunucudaki bir klasöre veya paylaşılan bir ağ klasöründen kopyalayarak `AzureConnectedMachineAgent.msi` bağlı makine aracısını el ile yükleyebilirsiniz. Yükleyici paketini herhangi bir seçenek olmadan çalıştırırsanız, aracı etkileşimli olarak yüklemek için izleyebileceğiniz bir Kurulum Sihirbazı başlatılır.

>[!NOTE]
>*Yönetici* ayrıcalıkları yükleyin veya aracıyı kaldırmak için gereklidir.

Makinenin bir ara sunucu üzerinden hizmete iletişim kurması gerekiyorsa, aracıyı yükledikten sonra, proxy sunucu sistemi ortam değişkenini `https_proxy`ayarlamak için aşağıdaki bölümde açıklanan bir komut çalıştırmanız gerekir.

Aşağıdaki tabloda kurulum tarafından desteklenen komut satırı aracı için parametreleri vurgular.

| Parametre | Açıklama |
|:--|:--|
| /? | Komut satırı seçeneklerinin listesini döndürür. |
| /S | Kullanıcı etkileşimi olmadan Sessiz bir yükleme gerçekleştirir. |

Örneğin, yükleme programını `/?` parametresiyle çalıştırmak için `msiexec.exe /i AzureConnectedMachineAgent.msi /?`girin.

Bağlı makine aracısının dosyaları varsayılan olarak *C:\Program Files\AzureConnectedMachineAgent* 'e yüklenir. Kurulum tamamlandıktan sonra aracı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Günlük dizini *%ProgramFiles%\AzureConnectedMachineAgentAgent\logs*' dir.

### <a name="install-using-scripted-method"></a>Komut dosyalı yöntemi kullanarak Install

1. Sunucuda oturum açın.

2. Yükseltilmiş bir PowerShell komut istemi açın.

3. Betiği kopyaladığınız klasöre veya paylaşıma değiştirin ve komutu `./OnboardingScript.ps1`komutunu çalıştırarak sunucuda yürütün.

### <a name="configure-agent-proxy-setting"></a>Aracı proxy ayarını Yapılandır

Proxy sunucusu ortam değişkenini ayarlamak için aşağıdaki komutu çalıştırın.

```powershell
# If a proxy server is needed, execute these commands with proxy URL and port
[Environment]::SetEnvironmentVariable("https_proxy", "http://{proxy-url}:{proxy-port}", "Machine")
$env:https_proxy = [System.Environment]::GetEnvironmentVariable("https_proxy","Machine")
# The agent service needs to be restarted after the proxy environment variable is set in order for the changes to take effect.
Restart-Service -Name himds
```

>[!NOTE]
>Aracı bu önizlemede proxy kimlik doğrulamasını ayarlamayı desteklemiyor.
>

### <a name="configure-agent-communication"></a>Aracı iletişimini yapılandırma

Aracıyı yükledikten sonra, aşağıdaki komutu çalıştırarak aracıyı Azure Arc hizmeti ile iletişim kuracak şekilde yapılandırmanız gerekir:

`%ProgramFiles%\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="install-and-validate-the-agent-on-linux"></a>Linux 'ta aracıyı yükleyip doğrulama

Linux için bağlı makine Aracısı, dağıtım için tercih edilen paket biçiminde sağlanır (. RPM veya. DEB) Microsoft 'un [paket deposunda](https://packages.microsoft.com/)barınmalıdır. [https://aka.ms/azcmagent](https://aka.ms/azcmagent) konumundaki bir kabuk betik paketi `Install_linux_azcmagent.sh` aşağıdaki eylemleri gerçekleştirir:

- Packages.microsoft.com adresinden Aracı paketini indirmek için konak makineyi yapılandırır.
- Karma kaynak sağlayıcısı paketini kurar.
- İsteğe bağlı olarak, `--proxy "{proxy-url}:{proxy-port}"` parametresini ekleyerek aracıyı proxy bilgilerinizi kullanarak yapılandırın.

Betik Ayrıca, desteklenen ve desteklenmeyen dağıtımları belirlemek için de mantığı ve yüklemeyi gerçekleştirmek için gerekli izinleri doğrulamayı içerir. 

Aşağıdaki örnek, aracı indirir ve koşullu denetimleri gerçekleştirmeden yükler.

```bash
# Download the installation package
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh
```

Aracıyı, proxy sunucunuz üzerinden iletişim kurmak üzere yapılandırmaya yönelik `--proxy` parametresi dahil olmak üzere, aracıyı indirmek ve yüklemek için aşağıdaki komutları çalıştırın:

```bash
# Download the installation package
wget https://aka.ms/azcmagent -O ~/Install_linux_azcmagent.sh

# Install the connected machine agent. 
bash ~/Install_linux_azcmagent.sh --proxy "{proxy-url}:{proxy-port}"
```

### <a name="configure-agent-communication"></a>Aracı iletişimini yapılandırma

Aracıyı yükledikten sonra, aşağıdaki komutu çalıştırarak aracıyı Azure Arc hizmeti ile iletişim kuracak şekilde yapılandırmanız gerekir:

`/opt/azcmagent/bin/azcmagent.exe" connect --resource-group "<resourceGroupName>" --tenant-id "<tenantID>" --location "<regionName>" --subscription-id "<subscriptionID>"`

## <a name="verify-connection-with-azure-arc"></a>Azure Arc ile bağlantıyı doğrulama

Aracıyı yüklemek ve sunucu (Önizleme) için Azure yaya bağlanacak şekilde yapılandırmak için gereken adımları gerçekleştirdikten sonra, sunucunun başarıyla bağlandığını doğrulamak için Azure portal gidin. [https://aka.ms/hybridmachineportal](https://aka.ms/hybridmachineportal)ziyaret ederek makinelerinizi Azure Portal görüntüleyebilirsiniz.

![Başarılı ekleme](./media/quickstart-onboard/arc-for-servers-successful-onboard.png)

## <a name="clean-up"></a>Temizleme

Bir makinenin sunucular için Azure Arc bağlantısını kesmek için (Önizleme), aşağıdaki adımları gerçekleştirmeniz gerekir.

1. [https://aka.ms/hybridmachineportal](https://aka.ms/hybridmachineportal)ziyaret ederek sunucular Için Azure yay (Önizleme) öğesini açın.

2. Listeden makineyi seçin, elipsi (`...`) tıklayın ve **Sil**' i seçin.

3. Windows aracısını makineden kaldırmak için aşağıdakileri yapın:

    1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

    2. **Denetim Masası**'nda **Programlar ve Özellikler**' i seçin.

    3. **Programlar ve Özellikler**' de **Azure bağlı makine Aracısı**' nı seçin, **Kaldır**' ı seçin ve **Evet**' i seçin.

        >[!NOTE]
        >Aracı Kurulum Sihirbazı, **AzureConnectedMachineAgent. msi** yükleyici paketi çift tıklatılarak da çalıştırılabilir.

    Kaldırma işlemini komut dosyasına almak isterseniz, ürün kodunu alan ve Msiexec. exe komut satırı-`msiexec /x {Product Code}`kullanarak aracıyı kaldırmış aşağıdaki örneği kullanabilirsiniz. Kayıt Defteri Düzenleyicisi 'Ni açın ve kayıt defteri anahtarı `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall` bölümüne bakın ve ürün kodu GUID 'sini bulun. Ardından, msiexec kullanarak aracıyı kaldırabilirsiniz.

   Aşağıdaki örnekte aracının kaldırılması gösterilmektedir.

   ```powershell
   Get-ChildItem -Path HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall | `
   Get-ItemProperty | `
   Where-Object {$_.DisplayName -eq "Azure Connected Machine Agent"} | `
   ForEach-Object {MsiExec.exe /x "$($_.PsChildName)" /qn}
   ```

4. Linux aracısını kaldırmak için, aracıyı kaldırmak için aşağıdaki komutu yürütün.

   ```bash
   sudo apt purge hybridagent
   ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bağlı makinelere Ilke atama](../../governance/policy/assign-policy-portal.md)
