---
title: PowerShell-Azure ile RDP özelliklerini özelleştirme
description: PowerShell cmdlet 'leriyle Windows sanal masaüstü için RDP özelliklerini özelleştirme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 06/19/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4cbfe03c48c6d971c140dc332c38a47bc9234173
ms.sourcegitcommit: e995f770a0182a93c4e664e60c025e5ba66d6a45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86135112"
---
# <a name="customize-remote-desktop-protocol-properties-for-a-host-pool"></a>Bir konak havuzu için Uzak Masaüstü Protokolü özelliklerini özelleştirme

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/customize-rdp-properties-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Çoklu izleyici deneyimi ve ses yeniden yönlendirme gibi bir konak havuzunun Uzak Masaüstü Protokolü (RDP) özelliklerini özelleştirmek, kullanıcılarınız için gereksinimlerinize göre en iyi deneyimi sunmanızı sağlar. Windows sanal masaüstündeki RDP özelliklerini, Azure portal kullanarak veya **Update-AzWvdHostPool** cmdlet 'inde *-customrdpproperty* parametresini kullanarak özelleştirebilirsiniz.

Desteklenen özelliklerin tam listesi ve bunların varsayılan değerleri için bkz. [desteklenen RDP dosyası ayarları](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/rdp-files?context=/azure/virtual-desktop/context/context) .

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, PowerShell modülünüzü ayarlamak ve Azure 'da oturum açmak için [Windows sanal masaüstü PowerShell modülünü ayarlama](powershell-module.md) bölümündeki yönergeleri izleyin.

## <a name="default-rdp-properties"></a>Varsayılan RDP özellikleri

Varsayılan olarak, yayımlanan RDP dosyaları aşağıdaki özellikleri içerir:

|RDP özellikleri | Masaüstü Bilgisayarlar | RemoteApp uygulamaları |
|---|---| --- |
| Çoklu monitör modu | Etkin | YOK |
| Sürücü yeniden yönlendirmeleri etkin | Sürücüler, pano, yazıcılar, COM bağlantı noktaları, USB cihazları ve akıllı kartlar| Sürücüler, pano ve Yazıcılar |
| Uzak ses modu | Yerel olarak oynat | Yerel olarak oynat |

Konak havuzu için tanımladığınız tüm özel özellikler, bu Varsayılanları geçersiz kılar.

## <a name="configure-rdp-properties-in-the-azure-portal"></a>Azure portal RDP özelliklerini yapılandırın

Azure portal RDP özelliklerini yapılandırmak için:

1. Adresinden Azure 'da oturum açın <https://portal.azure.com> .
2. Arama çubuğuna **Windows sanal masaüstü 'nü** girin.
3. Hizmetler altında **Windows sanal masaüstü**' nü seçin.
4. Windows sanal masaüstü sayfasında, ekranın sol tarafındaki menüden **konak havuzları** ' nı seçin.
5. Güncelleştirmek istediğiniz **konak havuzunun adını** seçin.
6. Ekranın sol tarafındaki menüden **Özellikler** ' i seçin.
7. **Özellikler** sekmesinde, RDP özelliklerini düzenlemeyle başlamak için **RDP ayarları** ' na gidin. Özellikler, PowerShell örnekleri gibi noktalı virgülle ayrılmış bir biçimde olmalıdır.
8. İşiniz bittiğinde, değişikliklerinizi kaydetmek için **Kaydet** ' i seçin.

Sonraki bölümlerde, PowerShell 'de özel RDP özelliklerinin el ile nasıl düzenleneceği açıklanır.

## <a name="add-or-edit-a-single-custom-rdp-property"></a>Tek bir özel RDP özelliği ekleme veya düzenleme

Tek bir özel RDP özelliği eklemek veya düzenlemek için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -CustomRdpProperty <property>
```

Az önce çalıştırdığınız cmdlet 'in özelliği güncelleştirdiğinden emin olmak için şu cmdlet 'i çalıştırın:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, CustomRdpProperty

Name              : <hostpoolname>
CustomRdpProperty : <customRDPpropertystring>
```

Örneğin, 0301HP adlı bir konak havuzunda "audiocapturemode" özelliğini kontrol ediyorsanız şu cmdlet 'i girersiniz:

```powershell
Get-AzWvdHostPool -ResourceGroupName 0301rg -Name 0301hp | format-list Name, CustomRdpProperty

Name              : 0301HP
CustomRdpProperty : audiocapturemode:i:1;
```

## <a name="add-or-edit-multiple-custom-rdp-properties"></a>Birden çok özel RDP özelliği ekleme veya düzenleme

Birden çok özel RDP özelliği eklemek veya düzenlemek için, Özel RDP özelliklerini noktalı virgülle ayrılmış bir dize olarak sağlayarak aşağıdaki PowerShell cmdlet 'lerini çalıştırın:

```powershell
$properties="<property1>;<property2>;<property3>"
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -CustomRdpProperty $properties
```

Aşağıdaki cmdlet 'i çalıştırarak RDP özelliğinin eklendiğinden emin olmak için kontrol edebilirsiniz:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, CustomRdpProperty

Name              : <hostpoolname>
CustomRdpProperty : <customRDPpropertystring>
```

Önceki cmdlet örnekimize göre 0301HP ana bilgisayar havuzunda birden çok RDP özelliği ayarlarsanız, cmdlet 'niz şöyle görünür:

```powershell
Get-AzWvdHostPool -ResourceGroupName 0301rg -Name 0301hp | format-list Name, CustomRdpProperty

Name              : 0301HP
CustomRdpProperty : audiocapturemode:i:1;audiomode:i:0;
```

## <a name="reset-all-custom-rdp-properties"></a>Tüm özel RDP özelliklerini Sıfırla

[Tek bir özel RDP özelliği ekleme veya düzenleme](#add-or-edit-a-single-custom-rdp-property)bölümündeki yönergeleri IZLEYEREK özel RDP özelliklerini varsayılan değerlerine sıfırlayabilirsiniz veya aşağıdaki PowerShell cmdlet 'ini çalıştırarak bir konak havuzu için tüm özel RDP özelliklerini sıfırlayabilirsiniz:

```powershell
Update-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -CustomRdpProperty ""
```

Ayarı başarıyla kaldırmış olduğunuzdan emin olmak için şu cmdlet 'i girin:

```powershell
Get-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> | format-list Name, CustomRdpProperty

Name              : <hostpoolname>
CustomRdpProperty : <CustomRDPpropertystring>
```

## <a name="next-steps"></a>Sonraki adımlar

Artık belirli bir konak havuzu için RDP özelliklerini özelleştirdiğinize göre, bir kullanıcı oturumunun parçası olarak test etmek için bir Windows sanal masaüstü istemcisinde oturum açabilirsiniz. Bu sonraki nasıl yapılır kılavuzlarında, tercih ettiğiniz istemciyi kullanarak bir oturuma nasıl bağlanacağınızı öğreneceksiniz:

- [Windows Masaüstü istemcisine bağlanma](connect-windows-7-and-10.md)
- [Web istemcisiyle bağlanma](connect-web.md)
- [Android istemcisiyle bağlanma](connect-android.md)
- [macOS istemcisiyle bağlanma](connect-macos.md)
- [iOS istemcisiyle bağlanma](connect-ios.md)
