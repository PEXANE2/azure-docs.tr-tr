---
title: Azure Bulut Kabuğu sorun giderme | Microsoft Dokümanlar
description: Azure Bulut Bulut Su Larını Sorun Giderme
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: damaerte
ms.openlocfilehash: 0b1b22095c77344ed71762d3d51b12f19d9f1811
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79458061"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Azure Bulut Bulutu & Sınırlamaları

Azure Bulut Su Bulutu'ndaki sorun giderme sorunları için bilinen çözümler şunlardır:

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="general-troubleshooting"></a>Genel sorun giderme

### <a name="error-running-azuread-cmdlets-in-powershell"></a>PowerShell'de AzureAD cmdlets'te hata çalıştırma

- **Ayrıntılar**: Cloud Shell'deki gibi `Get-AzureADUser` AzureAD cmdlet'lerini `You must call the Connect-AzureAD cmdlet before calling any other cmdlets`çalıştırdığınızda bir hata görebilirsiniz: . 
- **Çözünürlük**: `Connect-AzureAD` Cmdlet çalıştırın. Daha önce Cloud Shell, PowerShell'in başlatılması sırasında bu cmdlet'i otomatik olarak çalıştırılır. Başlangıç saatini hızlandırmak için cmdlet artık otomatik olarak çalışmaz. PowerShell'deki $PROFILE dosyasına `Connect-AzureAD` ekleyerek önceki davranışı geri yüklemeyi seçebilirsiniz.

### <a name="early-timeouts-in-firefox"></a>FireFox erken zaman ları

- **Ayrıntılar**: Cloud Shell, giriş/çıktıyı tarayıcınıza aktarmak için açık bir websocket kullanır. FireFox, Cloud Shell'de erken zaman alabilen websoketini erken kapatabilen önceden ayarlanmış politikalara sahiptir.
- **Çözünürlük**: FireFox'u açın ve URL kutusundaki "about:config" adresine gidin. "network.websocket.timeout.ping.request" araması yapın ve değeri 0'dan 10'a değiştirin.

### <a name="disabling-cloud-shell-in-a-locked-down-network-environment"></a>Bulut Shell'i kilitli ağ ortamında devre dışı bırakma

- **Ayrıntılar**: Yöneticiler kullanıcıları için Bulut Kabuğu'na erişimi devre dışı kılabilir. Cloud Shell, portal.azure.com, `ux.console.azure.com` shell.azure.com, Visual Studio Code Azure Hesabı uzantısı ve docs.microsoft.com dahil olmak üzere Cloud Shell'in giriş noktalarına erişimi durdurarak engellenebilecek etki alanına erişimi kullanır. ABD Hükümeti bulut, giriş noktası; `ux.console.azure.us` karşılık gelen bir shell.azure.us yoktur.
- **Çözünürlük**: Ağ `ux.console.azure.com` `ux.console.azure.us` ayarlarına erişimi veya ağ ayarları üzerinden ortamınıza erişimi kısıtlayın. Bulut Kabuğu simgesi Azure portalında da bulunur, ancak hizmete başarılı bir şekilde bağlanmaz.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Depolama İletişim - Hata: 403 RequestDisallowedByPolicy

- **Ayrıntılar**: Cloud Shell üzerinden bir depolama hesabı oluştururken, yöneticiniz tarafından yerleştirilen bir Azure ilkesi nedeniyle başarısız olur. Hata iletisi şunları içerir:`The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Çözüm**: Depolama oluşturmayı reddeden Azure ilkesini kaldırmak veya güncelleştirmek için Azure yöneticinize başvurun.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Depolama İletişim - Hata: 400 İzin Verilmeyen İşlem

- **Ayrıntılar**: Azure Active Directory aboneliği ni kullanırken depolama alanı oluşturamazsınız.
- **Çözüm**: Depolama kaynakları oluşturabilen bir Azure aboneliği kullanın. Azure AD abonelikleri Azure kaynakları oluşturamaz.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Terminal çıkışı - Hata: Terminali bağlamak için başarısız: websocket kurulamaz. Yeniden `Enter` bağlanmak için basın.
- **Ayrıntılar**: Cloud Shell, Cloud Shell altyapısına bir websocket bağlantısı kurma olanağı gerektirir.
- **Çözünürlük**: *.console.azure.com adresinden etki adlarına https istekleri ve websocket istekleri göndermeolanağı sağlamak için ağ ayarlarınızı yapılandırdığınızı kontrol edin.

### <a name="set-your-cloud-shell-connection-to-support-using-tls-12"></a>Cloud Shell bağlantınızı TLS 1.2 kullanarak destek için ayarlayın
 - **Ayrıntılar**: Cloud Shell bağlantınız için TLS sürümünü tanımlamak için tarayıcıya özel ayarlar ayarlamanız gerekir.
 - **Çözünürlük**: Tarayıcınızın güvenlik ayarlarına gidin ve "TLS 1.2 kullan"ın yanındaki onay kutusunu seçin.

## <a name="bash-troubleshooting"></a>Bash sorun giderme

### <a name="cannot-run-the-docker-daemon"></a>Docker daemon çalıştıramazsınız

- **Ayrıntılar**: Cloud Shell, kabuk ortamınızı barındırmak için bir kapsayıcı kullanır, sonuç olarak daemon çalıştırmasına izin verilmez.
- **Çözünürlük**: Uzak bir Docker ana bilgisayardan docker konteynerleri yönetmek için varsayılan olarak yüklenen [docker-machine'i](https://docs.docker.com/machine/overview/)kullanır.

## <a name="powershell-troubleshooting"></a>PowerShell sorun giderme

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmiyor

- **Ayrıntılar**: Bir kullanıcı gui uygulaması başlatıyorsa, komut istemi geri dönmez. Örneğin, bir klon iki faktör kimlik doğrulaması etkin leştirilmiş özel bir GitHub repo'su etkinleştirildiğinde, iki faktörlü kimlik doğrulaması tamamlamak için bir iletişim kutusu görüntülenir.
- **Çözünürlük**: Kabuğu kapatın ve yeniden açın.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Azure VM'lerin uzaktan yönetimini giderme
> [!NOTE]
> Azure VM'lerin Herkese Açık bir IP adresi olmalıdır.

- **Ayrıntılar**: WinRM için varsayılan Windows Güvenlik Duvarı ayarları nedeniyle kullanıcı aşağıdaki hatayı görebilir:`Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Çözünürlük**: `Enable-AzVMPSRemoting` PowerShell remoting'in tüm yönlerini hedef makinede etkinleştirmek için çalıştırın.

### <a name="dir-does-not-update-the-result-in-azure-drive"></a>`dir`Azure sürücüsünde sonucu güncelleştirmez

- **Ayrıntılar**: Varsayılan olarak, kullanıcı deneyimi için `dir` optimize etmek için, sonuçları Azure sürücüsünde önbelleğe alır.
- **Çözüm**: Bir Azure kaynağı oluşturduktan, `dir -force` güncelledikten veya kaldırdıktan sonra, Azure sürücüsündeki sonuçları güncelleştirmek için çalıştırın.

## <a name="general-limitations"></a>Genel sınırlamalar

Azure Bulut Kabuğu'nun bilinen sınırlamaları şunlardır:

### <a name="quota-limitations"></a>Kota sınırlamaları

Azure Bulut Shell'in bölge başına kiracı başına 20 eşzamanlı kullanıcı sınırı vardır. Sınırdan daha fazla eşzamanlı oturum açmaya çalışırsanız, "Kota Üzerinden Kiracı Kullanıcı" hatası görürsünüz. Bundan daha fazla oturumun açık olması için yasal bir gereksiniminiz varsa (örneğin eğitim oturumları için), kota artışı istemek için beklenen kullanımöncesinde desteğe başvurun.

Cloud Shell ücretsiz bir hizmet olarak sunulur ve genel amaçlı bir bilgi işlem platformu olarak değil, Azure ortamınızı yapılandırmak için kullanılmak üzere tasarlanmıştır. Aşırı otomatik kullanım, Azure Hizmet Koşullarını ihlal etmiş olarak kabul edilebilir ve Bulut Shell erişiminin engellenmesine neden olabilir.

### <a name="system-state-and-persistence"></a>Sistem durumu ve kalıcılık

Bulut Kabuğu oturumunuzu sağlayan makine geçicidir ve oturumunuz 20 dakika etkin değildikten sonra geri dönüştürülür. Cloud Shell'in monte edilmesi için bir Azure dosya paylaşımı gerekir. Sonuç olarak, aboneliğinizin Cloud Shell'e erişmek için depolama kaynakları ayarlayabilmesi gerekir. Diğer hususlar şunlardır:

- Monte edilmiş depolama ile yalnızca `clouddrive` dizin içindeki değişiklikler devam eder. Bash'te `$HOME` diziniz de devam ediyor.
- Azure dosya paylaşımları yalnızca [atanan bölgenizden](persisting-shell-storage.md#mount-a-new-clouddrive)monte edilebilir.
  - Bash'te, `env` bölgenizi ' `ACC_LOCATION`olarak ayarlanmış olarak bulmak için çalıştırın.
- Azure Dosyaları yalnızca yerel olarak yedekli depolama ve coğrafi yedekli depolama hesaplarını destekler.

### <a name="browser-support"></a>Tarayıcı desteği

Cloud Shell aşağıdaki tarayıcıların en son sürümlerini destekler:

- Microsoft Edge
- Microsoft Internet Explorer
- Google Chrome
- Mozilla Firefox
- Apple Safari
  - Özel modda Safari desteklenmez.

### <a name="copy-and-paste"></a>Kopyala ve yapıştır

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="usage-limits"></a>Kullanım sınırları

Cloud Shell, etkileşimli kullanım örnekleri için tasarlanmıştır. Sonuç olarak, uzun süren etkileşimli olmayan oturumlar uyarı yapılmadan sona erdirilir.

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinler sudo erişimi olmayan normal kullanıcılar olarak ayarlanır. Dizininizin `$Home` dışındaki yüklemeler kalıcı değildir.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="editing-bashrc"></a>Düzenleme .bashrc

.bashrc'yi düzenlerken dikkatli olun, bunu yapmak Bulut Kabuğu'nda beklenmeyen hatalara neden olabilir.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="preview-version-of-azuread-module"></a>AzureAD modülünün önizleme sürümü

Şu `AzureAD.Standard.Preview`anda ,.NET Standart tabanlı bir önizleme sürümü, modülü mevcuttur. Bu modül aynı işlevselliği `AzureAD`sağlar.

### <a name="sqlserver-module-functionality"></a>`SqlServer`modül işlevselliği

Cloud `SqlServer` Shell'de yer alan modül, PowerShell Core için yalnızca ön sürüm desteğine sahiptir. Özellikle, `Invoke-SqlCmd` henüz mevcut değildir.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturulduğunda varsayılan dosya konumu

Kullanıcılar PowerShell cmdlets'i kullanarak Azure sürücüsünün altında dosya oluşturamaz. Kullanıcılar vim veya nano gibi diğer araçları kullanarak yeni dosyalar oluşturduklarında, dosyalar varsayılan olarak dosyalara `$HOME` kaydedilir.

### <a name="tab-completion-can-throw-psreadline-exception"></a>Sekme tamamlama PSReadline özel atabilirsiniz

Kullanıcının PSReadline EditMode Emacs ayarlanırsa, kullanıcı sekme tamamlanması yoluyla tüm olasılıkları görüntülemek için çalışır ve pencere boyutu tüm olasılıkları görüntülemek için çok küçük, PSReadline işlenmemiş özel durum atar.

### <a name="large-gap-after-displaying-progress-bar"></a>İlerleme çubuğu görüntülendikten sonra büyük boşluk

Bir komut veya kullanıcı eylemi, `Azure:` sürücüdeyken tamamlanan böyle bir sekme olan bir ilerleme çubuğu görüntülerse, imleç düzgün ayarlanmaz ve ilerleme çubuğunun daha önce bulunduğu yerde bir boşluk belirir.

### <a name="random-characters-appear-inline"></a>Rasgele karakterler satır içinde görünür

İmleç konum sırası kodları, `5;13R`örneğin, kullanıcı girişi görünebilir. Karakterler el ile kaldırılabilir.

## <a name="personal-data-in-cloud-shell"></a>Cloud Shell'deki kişisel veriler

Azure Bulut Shell kişisel verilerinizi ciddiye alır, Azure Bulut BulutU hizmeti tarafından yakalanan ve depolanan veriler, en son kullandığınız kabuk, tercih edilen yazı tipi boyutu, tercih edilen yazı tipi türü ve dosya paylaşım ayrıntıları gibi deneyiminiz için varsayılan değer sağlamak için kullanılır bu arka bulut sürücüsü. Bu verileri dışa aktarmak veya silmek isterseniz, aşağıdaki yönergeleri kullanın.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Dışarı Aktarma
Kullanıcı ayarlarını **dışa aktarmak** için Cloud Shell tercih edilen kabuk, yazı tipi boyutu ve yazı tipi türü gibi sizin için kaydeder aşağıdaki komutları çalıştırın.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Launch Azure Cloud Shell")](https://shell.azure.com)
2. Bash veya PowerShell'de aşağıdaki komutları çalıştırın:

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  ((Invoke-WebRequest -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}).Content | ConvertFrom-Json).properties | Format-List
```

### <a name="delete"></a>Sil
Kullanıcı ayarlarınızı **silmek** için Bulut Shell sizin için tercih edilen kabuk, yazı tipi boyutu ve yazı tipi türü gibi aşağıdaki komutları çalıştırın kaydeder. Cloud Shell'i bir sonraki kez başlattığınızda, bir dosya paylaşımında yeniden bulunmanız istenir. 

>[!Note]
> Kullanıcı ayarlarınızı silerseniz, gerçek Azure Dosyaları paylaşımı silinmez. Bu eylemi tamamlamak için Azure Dosyalarınıza gidin.

1. [![](https://shell.azure.com/images/launchcloudshell.png "Launch Azure Cloud Shell")](https://shell.azure.com)
2. Bash veya PowerShell'de aşağıdaki komutları çalıştırın:

Bash:

  ```
  token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
  curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
  ```

PowerShell:

  ```powershell
  $token= ((Invoke-WebRequest -Uri "$env:MSI_ENDPOINT`?resource=https://management.core.windows.net/" -Headers @{Metadata='true'}).content |  ConvertFrom-Json).access_token
  Invoke-WebRequest -Method Delete -Uri https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -Headers @{Authorization = "Bearer $token"}
  ```
## <a name="azure-government-limitations"></a>Azure Kamu sınırlamaları
Azure Kamu'daki Azure Bulut Bulut Su'ya yalnızca Azure portalı üzerinden erişilebilir.

>[!Note]
> Exchange Online için GCC-High veya Government DoD Clouds'a bağlanmak şu anda desteklenmez.
