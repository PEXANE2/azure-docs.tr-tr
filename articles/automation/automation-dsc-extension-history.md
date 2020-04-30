---
description: Azure 'da Istenen durum yapılandırması (DSC) uzantısı için sürüm geçmişi hakkında bilgi edinin.
ms.date: 06/21/2018
keywords: DSC, PowerShell, Azure, uzantı
title: Azure DSC uzantısı sürüm geçmişi
author: mgoedtel
ms.author: magoedte
services: automation
ms.service: automation
ms.subservice: dsc
ms.topic: conceptual
ms.openlocfilehash: 207b8f7467753a8693bbabe6dd3f7a890307cf21
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82120710"
---
# <a name="azure-desired-state-configuration-extension-version-history"></a>Azure Istenen durum yapılandırması uzantısı sürümü geçmişi

Azure Istenen durum yapılandırması (DSC) VM uzantısı, Azure, Windows Server ve Windows PowerShell 'i içeren Windows Management Framework (WMF) tarafından sunulan geliştirmeleri ve yeni özellikleri desteklemek için gereken şekilde güncelleştirilir.

Bu makale, Azure DSC VM uzantısının her sürümü, desteklediği ortamlar ve yeni özellikler ya da değişiklikler hakkında açıklamalar ve açıklamalar hakkında bilgi sağlayacaktır.

## <a name="latest-version"></a>En son sürüm

### <a name="version-276"></a>Sürüm 2,76

- **Yayın tarihi:**
  - 9 Mayıs 2018 (Azure) | 21 Haziran 2018 (Azure Çin, Azure Kamu)
- **İşletim sistemi desteği:**
  - Windows Server 2016
  - Windows Server 2012 R2
  - Windows Server 2012
  - Windows Server 2008 R2 SP1
  - Windows Istemcisi 7/8.1/10
  - Nano Sunucu
- **WMF desteği:**
  - WMF 5.1
  - WMF 5,0 RTM
  - WMF 4,0 güncelleştirmesi
  - WMF 4,0
- **Ortamınızın**
  - Azure
  - Azure Çin
  - Azure Kamu
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - Alt durum ve diğer küçük hata düzeltmeleri için uzantı meta verilerinde geliştirme.

## <a name="supported-versions"></a>Desteklenen sürümler

> [!WARNING]
> 2,4 ile 5,0 2,13 arasındaki sürümler, oturum açma sertifikalarının tarihinde biten
> 2016. Bu sorun hakkında daha fazla bilgi için bkz. [blog gönderisi](https://devblogs.microsoft.com/powershell/azure-dsc-extension-versions-2-4-up-to-2-13-will-retire-in-august/).

### <a name="version-275"></a>Sürüm 2,75

- **Yayın tarihi:** 5 Mart 2018
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, nano sunucu
- **WMF desteği:** WMF 5,1, WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - GitHub 'ın son TLS 1,2 ' a taşınması sonrasında, Azure Market 'te bulunan DIY Kaynak Yöneticisi şablonlarını kullanarak bir VM 'yi Azure Automation DSC ekleyemezsiniz veya GitHub üzerinde barındırılan herhangi bir yapılandırmayı almak için DSC uzantısını kullanabilirsiniz. Uzantıyı dağıtmaya karşın aşağıdakine benzer bir hata görürsünüz:

    ```json
    {
        "code": "DeploymentFailed",
        "message": "At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.",
        "details": [{
            "code": "Conflict",
            "message": "{
                \"status\": \"Failed\",
                \"error\": {
                    \"code\": \"ResourceDeploymentFailure\",
                    \"message\": \"The resource operation completed with terminal provisioning state 'Failed'.\",
                    \"details\": [ {
                        \"code\": \"VMExtensionProvisioningError\",
                        \"message\": \"VM has reported a failure when processing extension 'Microsoft.Powershell.DSC'.
                        Error message: \\\"The DSC Extension failed to execute: Error downloading
                        https://github.com/Azure/azure-quickstart-templates/raw/master/dsc-extension-azure-automation-pullserver/UpdateLCMforAAPull.zip
                        after 29 attempts: The request was aborted: Could not create SSL/TLS secure channel..\\nMore information about the failure can
                        be found in the logs located under 'C:\\\\WindowsAzure\\\\Logs\\\\Plugins\\\\Microsoft.Powershell.DSC\\\\2.74.0.0' on the VM.\\\".\"
                    } ]
                }
            }"
        }]
    }
    ```

  - Yeni uzantı sürümünde TLS 1,2 artık zorlanır. Kaynak Yöneticisi şablonunda zaten bir oto Upgrademinorversion = true değeri varsa, uzantı dağıtımı, 2,75 'e yeniden yükseltilir. El ile güncelleştirmeler için Kaynak Yöneticisi `TypeHandlerVersion = 2.75` şablonunuzda belirtin.

### <a name="version-270---272"></a>Sürüm 2,70-2,72

- **Yayın tarihi:** 13 Kasım 2017
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, nano sunucu
- **WMF desteği:** WMF 5,1, WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - Hata düzeltmeleri, DSC Azure Otomasyonu 'nu Portal kullanıcı arabiriminden ve Kaynak Yöneticisi şablonuyla kullanmayı kolaylaştıran iyileştirmeler &. Daha fazla bilgi için bkz. DSC Uzantısı belgelerindeki [varsayılan yapılandırma betiği](/azure/virtual-machines/extensions/dsc-overview) .

### <a name="version-226"></a>Sürüm 2,26

- **Yayın tarihi:** 9 Haziran 2017
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, nano sunucu
- **WMF desteği:** WMF 5,1, WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - Telemetri geliştirmeleri.

### <a name="version-225"></a>Sürüm 2,25

- **Yayın tarihi:** 2 Haziran 2017
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, Windows Client 7/8.1/10, nano sunucu
- **WMF desteği:** WMF 5,1, WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - Çeşitli hata düzeltmeleri ve diğer küçük geliştirmeler eklenmiştir.

### <a name="version-224"></a>Sürüm 2,24

- **Yayın tarihi:** 13 Nisan 2017
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, nano Server
- **WMF desteği:** WMF 5,1, WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - Uzantı meta verileri olarak & DSC Aracısı kimliği VM UUID 'sini gösterir. Diğer küçük iyileştirmeler eklendi.

### <a name="version-223"></a>Sürüm 2,23

- **Yayın tarihi:** 15 Mart 2017
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, nano Server
- **WMF desteği:** WMF 5,1, WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - Birçok hata düzeltmesi ve diğer geliştirmeler eklenmiştir.

### <a name="version-222"></a>Sürüm 2,22

- **Yayın tarihi:** 8 Şubat 2017
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, nano Server
- **WMF desteği:** WMF 5,1, WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,1](https://devblogs.microsoft.com/powershell/wmf-5-1-releasing-january-2017/) (WMF yükleme için bir yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - DSC Uzantısı artık WMF 5,1 desteğine sahiptir.
  - Diğer küçük geliştirmeler eklenmiştir.

### <a name="version-221"></a>Sürüm 2,21

- **Yayın tarihi:** 2 Aralık 2016
- **Işletim sistemi desteği:** Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1, nano Server
- **WMF desteği:** WMF 5,1 Preview, WMF 5,0 RTM, WMF 4,0 güncelleştirmesi, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 ' de yer alan DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir. Nano sunucu için, sanal makinede DSC rolü yüklüdür.
- **Yeni Özellikler:**
  - DSC Uzantısı artık nano Server 'da kullanılabilir. Bu sürüm öncelikle, uzantıyı nano sunucu 'da çalıştırmaya yönelik kod değişiklikleri içerir.
  - Diğer küçük geliştirmeler eklenmiştir.

### <a name="version-220"></a>Sürüm 2,20

- **Yayın tarihi:** 2 Ağustos 2016
- **Işletim sistemi desteği:** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF desteği:** WMF 5,1 Preview, WMF 5,0 RTM, WMF 4,0 güncelleştirmesi, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 Technical Preview 'a dahil edilen DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir.
- **Yeni Özellikler:**
  - WMF 5,1 Preview desteği. İlk yayımlandığında, bu sürüm isteğe bağlı bir yükseltiyordu ve WMF 5,1 Preview sürümünü yüklemek için Kaynak Yöneticisi şablonlarda Wmfversion = ' 5.1 PP ' belirtmeniz gerekiyordu.
    Wmfversion = ' Latest ' hala [WMF 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/)'yi yüklüyor.
    WMF 5,1 Preview hakkında daha fazla bilgi için [Bu bloga](https://devblogs.microsoft.com/powershell/announcing-windows-management-framework-wmf-5-1-preview/)bakın.
  - Diğer küçük düzeltmeler ve geliştirmeler eklenmiştir.

### <a name="version--219"></a>Sürüm 2,19

- **Yayın tarihi:** 3 Haziran 2016
- **Işletim sistemi desteği:** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF desteği:** WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Azure, Azure Çin, Azure Kamu
- **Açıklamalar:** Bu sürüm, Windows Server 2016 Technical Preview 'a dahil edilen DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir.
- **Yeni Özellikler:**
  - DSC Uzantısı artık Azure Çin 'ye eklendi. Bu sürüm öncelikle uzantıyı Azure Çin 'de çalıştırmaya yönelik düzeltmeler içerir.

### <a name="version-218"></a>Sürüm 2,18

- **Yayın tarihi:** 3 Haziran 2016
- **Işletim sistemi desteği:** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF desteği:** WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 Technical Preview 'a dahil edilen DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir.
- **Yeni Özellikler:**
  - Telemetri Düzeltme İndirmesi (bilinen Azure DNS sorunu) sırasında veya yükleme sırasında bir hata oluştuğunda telemetri engellenmemiş hale getirin.
  - Yeniden başlatmadan sonra uzantının yapılandırmayı işlemeyi durdurduğu aralıklı soruna yönelik çözüm.
    Bu, DSC uzantısının ' geçiş ' durumunda kalmasına neden oldu.
  - Diğer küçük düzeltmeler ve geliştirmeler eklenmiştir.

### <a name="version-217"></a>Sürüm 2,17

- **Yayın tarihi:** 26 Nisan 2016
- **Işletim sistemi desteği:** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF desteği:** WMF 5,0 RTM, WMF 4,0 güncelleştirme, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 Technical Preview 'a dahil edilen DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir.
- **Yeni Özellikler:**
  - WMF 4,0 güncelleştirmesi desteği. WMF 4,0 güncelleştirmesi hakkında daha fazla bilgi için [Bu bloga](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-4-0-update-now-available-for-windows-server-2012-windows-server-2008-r2-sp1-and-windows-7-sp1/)bakın.
  - DSC Uzantısı yüklemesi sırasında oluşan hatalarda veya bir DSC yapılandırma sonrası yüklemesi uygulanırken oluşan hatalarda yeniden deneme mantığı. Bu değişikliğin bir parçası olarak, önceki yükleme başarısız olursa uzantı yüklemeyi yeniden dener veya daha önce başarısız olan bir DSC yapılandırmasını yeniden harekete (başarı/hata) veya yeni bir istek gelirse, en fazla üç kez daha önceden başarısız olur. Uzantı geçersiz Kullanıcı ayarları/kullanıcı girişi nedeniyle başarısız olursa, yeniden denenmez. Bu durumda, uzantının yeni bir istek ile yeniden çağrılması ve Kullanıcı ayarları doğru olması gerekir. Note: DSC Uzantısı, yeniden denemeler için Azure VM aracısına bağımlıdır. Azure VM Aracısı, bir başarılı veya hata durumuna ulaşıncaya kadar uzantıyı son başarısız istekle çağırır.

### <a name="version-216"></a>Sürüm 2,16

- **Yayın tarihi:** 21 Nisan 2016
- **Işletim sistemi desteği:** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF desteği:** WMF 5,0 RTM, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 Technical Preview 'a dahil edilen DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir.
- **Yeni Özellikler:**
  - Hata işlemede ve diğer küçük hata düzeltmelerde geliştirme.
  - DSC uzantı ayarlarındaki yeni özellik. "ForcePullAndApply", yenileme modu çekme olduğunda (varsayılan gönderim modunun aksine) DSC Uzantısı 'nın DSC yapılandırmalarına olanak tanımak için eklenmiştir. Daha fazla bilgi için lütfen DSC Uzantısı ayarları hakkında daha fazla bilgi edinmek üzere [Bu bloga](https://devblogs.microsoft.com/powershell/arm-dsc-extension-settings/) başvurun.

### <a name="version-215"></a>Sürüm 2,15

- **Yayın tarihi:** 14 Mart 2016
- **Işletim sistemi desteği:** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF desteği:** WMF 5,0 RTM, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 Technical Preview 'a dahil edilen DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir.
- **Yeni Özellikler:**
  - Uzantı sürümü 2,14 ' de, WMF RTM 'yi yüklemek için değişiklikler eklenmiştir. 2.13.2.0 uzantısından 2.14.0.0 sürümüne yükseltirken bazı DSC cmdlet 'lerinin başarısız olduğunu veya yapılandırmanızın bir hata vererek başarısız olduğunu fark edebilirsiniz: ' verilen özellik değerlerine sahip bir örnek bulunamadı '. Daha fazla bilgi için bkz. [DSC sürüm notları](/powershell/scripting/wmf/known-issues/known-issues-dsc). Bu sorunlara yönelik geçici çözümler 2,15 sürümüne eklenmiştir.
  - Ne yazık ki sürüm 2,14 ' ü zaten yüklediyseniz ve yukarıdaki iki sorunlardan birinde çalıştırıyorsanız, bu adımları el ile gerçekleştirmeniz gerekecektir. Yükseltilmiş bir PowerShell oturumunda:
    - `Remove-Item -Path $env:SystemRoot\system32\Configuration\DSCEngineCache.mof`
    - `mofcomp $env:windir\system32\wbem\DscCoreConfProv.mof`

### <a name="version-214"></a>Sürüm 2,14

- **Yayın tarihi:** 25 Şubat 2016
- **Işletim sistemi desteği:** Windows Server 2016 Technical Preview, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1
- **WMF desteği:** WMF 5,0 RTM, WMF 4,0
- **Ortam:** Mavisi
- **Açıklamalar:** Bu sürüm, Windows Server 2016 Technical Preview 'a dahil edilen DSC kullanır; diğer Windows Işletim sistemleri için [Windows Management Framework 5,0 RTM](https://devblogs.microsoft.com/powershell/windows-management-framework-wmf-5-0-rtm-is-now-available-via-the-microsoft-update-catalog/) 'YI (WMF yüklemesi için yeniden başlatma gerekir) yüklenir.
- **Yeni Özellikler:**
  - WMF RTM kullanır.
  - DSC uzantısının kalitesini artırmak için veri toplamayı sağlar. Daha fazla bilgi için bkz. [Blog](https://devblogs.microsoft.com/powershell/azure-dsc-extension-data-collection-2/).
  - Kaynak Yöneticisi şablonunda uzantı için güncelleştirilmiş bir ayar biçimi sağlar. Daha fazla bilgi için bkz. [Blog](https://devblogs.microsoft.com/powershell/arm-dsc-extension-settings/).
  - Hata düzeltmeleri ve diğer geliştirmeler.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell DSC hakkında daha fazla bilgi için [PowerShell belge merkezine](/powershell/scripting/dsc/overview/overview)gidin.
- [DSC uzantısının Kaynak Yöneticisi şablonunu](/azure/virtual-machines/extensions/dsc-template)inceleyin.
- PowerShell DSC 'yi kullanarak yönetebileceğiniz daha fazla işlevsellik ve daha fazla DSC kaynağı için [PowerShell Galerisi](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0)' ne gözatabileceğinizi unutmayın.
- Hassas parametreleri yapılandırmalara geçirme hakkında ayrıntılı bilgi için bkz. [DSC uzantı işleyicisiyle kimlik bilgilerini güvenli bir şekilde yönetme](/azure/virtual-machines/extensions/dsc-credentials).
