---
title: Azure Lab Services | Windows 'un kapatılma davranışını denetleme Kılavuzu | Microsoft Docs
description: Boşta bir Windows sanal makinesini otomatik olarak kapanmaya yönelik adımlar ve Windows kapatılırken komutunu kaldırma adımları.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.topic: article
ms.date: 3/30/2020
ms.author: spelluru
ms.openlocfilehash: 834338a6c695cf5eed9bcccbe31a703080cb4f6d
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84895811"
---
# <a name="guide-to-controlling-windows-shutdown-behavior"></a>Windows kapanıyor davranışını denetleme Kılavuzu

Azure Lab Services, Windows sanal makinelerinin (VM 'Ler) beklenmedik şekilde çalışmadığını sağlamak için çeşitli ücret denetimleri sağlar:
 - [Zamanlama ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
 - [Kullanıcılar için kota ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users)
 - [Bağlantı kesildiğinde otomatik kapatmayı etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

Bu maliyet denetimlerinde bile, bir Windows VM 'sinin beklenmedik şekilde çalışmaya devam edebildiği durumlar vardır; Sonuç olarak, öğrencinin kotasından düşün:

- **RDP penceresi açık kaldı**
  
    Bir öğrenci, sanal makinesine RDP kullanarak bağlanırsa, yanlışlıkla RDP penceresini açık bırakabilir.  RDP penceresi açık kaldığı sürece, yalnızca RDP oturumunun bağlantısı kesildikten sonra tetiklendiğinden, **bağlantı kesme sırasında otomatik olarak kapatılıyor** ayarı hiçbir zaman etkinleşmez.

- **Windows kapatma komutu VM 'yi kapatmak için kullanılır**
  
    Bir öğrenci Windows kapatma komutunu veya Windows içinde sunulan diğer kapatma mekanizmalarını, [Azure Lab Services ' Durdur düğmesini](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-classroom-lab#start-or-stop-the-vm)kullanmak yerıne, VM 'yi kapatmak için kullanabilir.  Bu durumda, Azure Lab Services perspektifinden VM hala kullanılmaktadır.
    
Bu durumların oluşmasını önlemenize yardımcı olması için bu kılavuzda, boşta olan bir Windows sanal makinesini otomatik olarak kapanmaya yönelik adımlar ve **Başlat** menüsünden Windows oturumu kapatılıyor komutunu kaldırma adımları sunulmaktadır.  

> [!NOTE]
> Ayrıca, öğrenci VM 'leri başlattığında bir VM beklenmedik şekilde kotasından düşmeyebilir, ancak hiçbir zaman RDP kullanarak buna bağlanmamıştır.  Bu kılavuz Şu anda bu senaryoya yönelik *değildir* .  Bunun yerine, öğrenciler başlattıktan sonra RDP kullanarak VM 'lerine hemen bağlanmaları hatırlamalıdır; ya da VM 'leri durdurmalıdır.

## <a name="automatic-rdp-disconnect-and-shutdown-for-idle-vm"></a>Boşta VM için otomatik RDP bağlantısı kesme ve kapatılma

Windows, bir RDP oturumunun boşta kaldığında bağlantısını otomatik olarak kesmek üzere bir zaman sınırı ayarlamak için kullanabileceğiniz **yerel Grup İlkesi** ayarları sağlar.  Bir oturum, herhangi bir mouse\keyboard *girişi olmadığında boşta* olarak belirlenir.  Mouse\keyboard girişi içermeyen uzun süre çalışan etkinlikler VM 'nin boşta durumunda olmasına neden olur.  Bu, uzun bir sorgu yürütme, video akışı, derleme vb. içerir.  Sınıfınızın ihtiyaçlarına bağlı olarak, boşta kalma süresi sınırını, bu tür etkinlikleri işlemek için yeterince uzun olacak şekilde ayarlamayı tercih edebilirsiniz.  Örneğin, boşta kalma süresi sınırını, gerekirse 1 veya daha fazla saat olarak ayarlayabilirsiniz.

Bu, **boş oturum limitini** [**bağlantı kesildiğinde otomatik kapatılma**](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect) ayarı ile birlikte yapılandırdığınızda öğrencinin deneyimidir:
 1. Öğrenci, RDP kullanarak Windows VM 'lerine bağlanır.
 2. Öğrenci, RDP penceresini açık bırakıyor ve sanal makine belirttiğiniz **boş oturum sınırı** (5 dakika) için boşta kaldığında, öğrenci aşağıdaki iletişim kutusunu görür:

    ![Boşta kalma süresi sınırı süresi dolmadı iletişim kutusu](./media/how-to-windows-shutdown/idle-time-expired.png)

1. Öğrenci **Tamam**' a *TıKLAMADıĞıNDAN* , RDP oturumunun süresi 2 dakika sonra otomatik olarak kesilir.
2. RDP oturumunun bağlantısı kesildikten sonra, **bağlantı kesildiğinde otomatik kapatılma** için belirtilen zaman dilimine ulaşıldığında, sanal makine Azure Lab Services tarafından otomatik olarak kapatılıyor.

### <a name="set-rdp-idle-session-time-limit-on-the-template-vm"></a>Şablon VM 'de RDP boşta oturum süresi sınırını ayarlama

RDP oturum boşta kalma süresi sınırını ayarlamak için, şablon VM 'ye bağlanabilir ve aşağıdaki PowerShell betiğini çalıştırabilirsiniz.

```powershell
# The MaxIdleTime is in milliseconds; by default, this script sets MaxIdleTime to 15 minutes.
$maxIdleTime = 15 * 60 * 1000

Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -Name "MaxIdleTime" -Value $maxIdleTime -Force
```
Ya da, şablon VM 'yi kullanarak bu adımları el ile izlemeyi tercih edebilirsiniz:

1. Windows tuşuna basın, **gpedit**yazın ve ardından **Grup Ilkesini Düzenle (Denetim Masası)** seçeneğini belirleyin.

1. **> > Windows bileşenleri > Uzak Masaüstü Hizmetleri > Uzak Masaüstü oturumu ana bilgisayarı oturum süresi sınırlarını Yönetim Şablonları bilgisayar yapılandırması**' na gidin.  

    ![Yerel grup ilkesi düzenleyicisi](./media/how-to-windows-shutdown/group-policy-idle.png)
   
1. **Etkin ancak boşta Uzak Masaüstü Hizmetleri oturumları için zaman sınırını ayarla**öğesine sağ tıklayın ve **Düzenle**' ye tıklayın.

1. Aşağıdaki ayarları girin ve ardından **Tamam**' a tıklayın:
   1. **Etkin**'i seçin.
   1. **Seçenekler**altında, **Boşta oturum sınırı**' nı belirtin.

    ![Boş oturum sınırı](./media/how-to-windows-shutdown/edit-idle-time-limit.png)

1. Son olarak, bu davranışı **bağlantı kesildiğinde otomatik olarak kapatılıyor** ayarı ile birleştirmek için, nasıl yapılır makalesindeki adımları izlemeniz gerekir: [bağlantı kesildiğinde sanal makinelerin otomatik olarak kapatılmasını etkinleştirin](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect).

> [!WARNING]
> PowerShell kullanarak, kayıt defteri ayarını doğrudan veya grup ilkesi düzenleyicisini kullanarak el ile değiştirmek için bu ayarı yapılandırdıktan sonra, ayarların etkili olması için önce VM 'yi yeniden başlatmanız gerekir.  Ayrıca, ayarı kayıt defteri kullanarak yapılandırırsanız grup ilkesi Düzenleyicisi, kayıt defteri ayarında yapılan değişiklikleri yansıtacak şekilde her zaman yenilemez; Ancak, kayıt defteri ayarı yine de beklendiği gibi sürer ve belirttiğiniz süre boyunca boşta kaldığında RDP oturumunun bağlantısını kestiğinizi görürsünüz.

## <a name="remove-windows-shutdown-command-from-start-menu"></a>Windows kapanıyor komutunu Başlat menüsünden kaldır

Windows **yerel Grup İlkesi** ayarları, **Başlat** menüsünden de kapatılıyor komutunu kaldırmanızı sağlar.

Kapanıyor komutunu kaldırmak için, şablon VM 'ye bağlanabilir ve aşağıdaki PowerShell betiğini çalıştırabilirsiniz.

```powershell
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" -Name "HidePowerOptions" -Value 1 -Force
```

Ya da, şablon VM 'yi kullanarak bu adımları el ile izlemeyi tercih edebilirsiniz:

1. Windows tuşuna basın, **gpedit**yazın ve ardından **Grup Ilkesini Düzenle (Denetim Masası)** seçeneğini belirleyin.

1. **> > Başlat menüsü ve görev çubuğu Yönetim Şablonları bilgisayar yapılandırması '** na gidin.  

    ![Yerel grup ilkesi düzenleyicisi](./media/how-to-windows-shutdown/group-policy-shutdown.png)

1. Kaldır ' a sağ tıklayın **ve kapatma, yeniden başlatma, uyku ve hazırda beklet komutlarına erişimi önleyin**ve **Düzenle**' ye tıklayın.

1. **Etkin** ayarı seçin ve ardından **Tamam**' a tıklayın:
 
   ![Kapalı ayarı](./media/how-to-windows-shutdown/edit-shutdown.png)

1. Kapanıyor komutunun artık Windows **Başlat** menüsünde görüntülenmediğine dikkat edin; yalnızca **bağlantıyı kes** komutu görünür.

    ![Komutu kapatır](./media/how-to-windows-shutdown/start-menu.png)

## <a name="next-steps"></a>Sonraki adımlar
Bir Windows şablon VM 'si hazırlama hakkında bilgi için bkz. [Azure Lab Services bir Windows şablon makinesi ayarlama Kılavuzu](how-to-prepare-windows-template.md)