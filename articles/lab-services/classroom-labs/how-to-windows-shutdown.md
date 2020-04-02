---
title: Azure Lab Hizmetlerinde Windows kapatma davranışını denetleme kılavuzu | Microsoft Dokümanlar
description: Boşta kalan windows sanal makinesini otomatik olarak kapatma ve Windows kapatma komutunu kaldırma adımları.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.topic: article
ms.date: 3/30/2020
ms.author: spelluru
ms.openlocfilehash: 39ff4f42457451dfa4aae90b281d6b163c56b4cd
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80522244"
---
# <a name="guide-to-controlling-windows-shutdown-behavior"></a>Windows kapatma davranışını denetleme kılavuzu

Azure Lab Hizmetleri, Windows sanal makinelerinin (VM'ler) beklenmeyen şekilde çalışmadığından emin olmak için çeşitli maliyet denetimleri sağlar:
 - [Zamanlama ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/tutorial-setup-classroom-lab#set-a-schedule-for-the-lab)
 - [Kullanıcılar için kota ayarlama](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-configure-student-usage#set-quotas-for-users)
 - [Bağlantı kesildiğinde otomatik kapatmayı etkinleştirme](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

Bu maliyet denetimleriyle bile, Windows VM'nin beklenmedik şekilde çalışmaya devam edebileceği durumlar vardır; ve sonuç olarak, öğrencinin kontenjanından düşüler:

- **RDP penceresi açık bırakılır**
  
    Bir öğrenci RDP kullanarak VM'lerine bağlandığında, istemeden RDP penceresini açık bırakabilir.  RDP penceresi açık kaldığı sürece, **yalnızca** RDP oturumu kesildikten sonra tetiklendirildiğinden, bağlantı kesme ayarındaki otomatik kapatma hiçbir zaman etkili olmaz.

- **Windows kapatma komutu VM'yi kapatmak için kullanılır**
  
    Bir öğrenci, [Azure Lab Hizmetleri'nin durdurma düğmesini](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-use-classroom-lab#start-or-stop-the-vm)kullanmak yerine VM'yi kapatmak için Windows kapatma komutunu veya Windows'da sağlanan diğer kapatma mekanizmalarını kullanabilir.  Bu durumda, Azure Lab Hizmetleri açısından VM hala kullanılıyor.
    
Bu kılavuz, bu durumların olmasını önlemenize yardımcı olmak için boşta kalan bir Windows VM'yi otomatik olarak kapatma ve **Başlat** menüsünden Windows kapatma komutunu kaldırma adımları sağlar.  

> [!NOTE]
> Öğrenci VM'sini başlattığında vm de beklenmedik bir şekilde kotadan düşebilir, ancak rdp kullanarak asla kotaya bağlanmaz.  Bu kılavuz şu anda bu senaryoyu ele *almaz.*  Bunun yerine, öğrencilere başlattıktan sonra RDP kullanarak VM'lerine hemen bağlanmaları hatırlatılmalıdır; veya VM'yi durdurmalıdırlar.

## <a name="automatic-rdp-disconnect-and-shutdown-for-idle-vm"></a>Boşta VM için otomatik RDP bağlantısı nın kesilmesi ve kapatılması

Windows, bir RDP oturumuboş takılan bir OTURUM'un otomatik olarak bağlantısını kesmek için bir zaman sınırı ayarlamak için kullanabileceğiniz **Yerel Grup İlkesi** ayarlarını sağlar.  Herhangi bir fare\klavye girişi *olmadığında* oturum boşta olarak belirlenir.  Fare\klavye girişi içermeyen uzun süren etkinlikler VM'nin boşta duruma sahip olmasını sağlar.  Bu, uzun bir sorgu yürütme, video akışı, derleme, vb içerir.  Sınıfınızın gereksinimlerine bağlı olarak, bu tür etkinlikleri işlemek için yeterince uzun olacak şekilde boşta kalma süresi sınırını ayarlayabilirsiniz.  Örneğin, gerekirse boşta kalma süresi sınırını 1 veya daha fazla saat olarak ayarlayabilirsiniz.

Boşta **oturum sınırını,** bağlantı kesme ayarındaki [**otomatik kapatma**](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect) yla birlikte yapılandırdığınızda öğrencinin deneyimi aşağıda veda edinilir:
 1. Öğrenci RDP kullanarak Windows VM'lerine bağlanır.
 2. Öğrenci RDP penceresini açık bıraktığında ve VM belirttiğiniz **boşta oturum sınırı** için boşta kaldığında (5 dakika gibi), öğrenci aşağıdaki iletişim kutusunu görür:

    ![Boşta zaman sınırı süresi dolmuş iletişim kutusu](../media/how-to-windows-shutdown/idle-time-expired.png)

1. Öğrenci **Tamam'ı** *tıklatmazsa,* RDP oturumu 2 dakika sonra otomatik olarak kesilir.
2. RDP oturumu kesildikten sonra, bağlantı kesme **ayarında otomatik kapatma** için belirtilen zaman çerçevesine ulaşıldıktan sonra, VM Azure Lab Hizmetleri tarafından otomatik olarak kapatılır.

### <a name="set-rdp-idle-session-time-limit-on-the-template-vm"></a>Şablon VM'de RDP boşta oturum süresi sınırını ayarlama

RDP oturumu boşta zaman sınırını ayarlamak için, vm şablonuna bağlanabilir ve aşağıdaki PowerShell komut dosyasını çalıştırabilirsiniz.

```powershell
# The MaxIdleTime is in milliseconds; by default, this script sets MaxIdleTime to 15 minutes.
$maxIdleTime = 15 * 60 * 90

Set-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" -Name "MaxIdleTime" -Value $maxIdleTime -Force
```
Veya, vm şablonu kullanarak bu el ile adımları izlemeyi seçebilirsiniz:

1. Windows tuşuna basın, **gpedit**yazın, ardından **grup ilkesini (Denetim paneli) edin'i**seçin.

1. Uzak **Masaüstü Oturum Ana Bilgisayar > > Oturum Zaman Sınırları > Windows Bileşenleri > Bilgisayar Yapılandırması > Yönetim Şablonlarına**gidin.  

    ![Yerel grup ilkesi düzenleyicisi](../media/how-to-windows-shutdown/group-policy-idle.png)
   
1. Etkin ancak boşta kalan **Uzak Masaüstü Hizmetleri oturumları için zaman sınırını ayarla'yı**sağ tıklatın ve **Edit'i**tıklatın.

1. Aşağıdaki ayarları girin ve sonra **Tamam'ı**tıklatın:
   1. **Etkin**'i seçin.
   1. **Seçenekler**altında, **Boşta oturum sınırını**belirtin.

    ![Boşta oturum sınırı](../media/how-to-windows-shutdown/edit-idle-time-limit.png)

1. Son olarak, bu davranışı **kesme ayarındaki otomatik kapatma** yla birleştirmek için, nasıl yapılabilen makaledeki adımları izlemeniz gerekir: Bağlantıyı [kesmede VM'lerin otomatik olarak kapatılmasını etkinleştirin.](https://docs.microsoft.com/azure/lab-services/classroom-labs/how-to-enable-shutdown-disconnect)

## <a name="remove-windows-shutdown-command-from-start-menu"></a>Başlat menüsünden Windows kapatma komutunu kaldırma

Windows **Yerel Grup İlkesi** ayarları, kapatma komutunu **Başlat** menüsünden kaldırmanızı da sağlar.

Kapatma komutunu kaldırmak için, vm şablonuna bağlanabilir ve aşağıdaki PowerShell komut dosyasını çalıştırabilirsiniz.

```powershell
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer" -Name "HidePowerOptions" -Value 1 -Force
```

Veya, vm şablonu kullanarak bu el ile adımları izlemeyi seçebilirsiniz:

1. Windows tuşuna basın, **gpedit**yazın, ardından **grup ilkesini (Denetim paneli) edin'i**seçin.

1. Başlat **Menüsü ve Görev Çubuğu'> Bilgisayar Yapılandırması > Yönetim Şablonlarına**gidin.  

    ![Yerel grup ilkesi düzenleyicisi](../media/how-to-windows-shutdown/group-policy-shutdown.png)

1. Sağ tıkla Kaldır **ve Kapat, Yeniden Başlat, Uyku ve Hazırda Bekleme komutlarına erişimi engelleyin**ve **Edit'i**tıklatın.

1. **Etkin** ayarını seçin ve **sonra Tamam'ı**tıklatın:
 
   ![Kapatma ayarı](../media/how-to-windows-shutdown/edit-shutdown.png)

1. Kapatma komutu artık Windows **Başlat** menüsü altında görünmüyor dikkat edin; yalnızca **Bağlantıyı Kesme** komutu görüntülenir.

    ![Kapatma komutu](../media/how-to-windows-shutdown/start-menu.png)

## <a name="next-steps"></a>Sonraki adımlar
Windows şablonu VM: Azure Lab [Hizmetlerinde Windows şablon uyrama makinesi kurma kılavuzu](how-to-prepare-windows-template.md) nun nasıl hazırlanacağı hakkındaki makaleye bakın