---
title: Azure Lab Services | Windows 'un kapatılma davranışını denetleme Kılavuzu | Microsoft Docs
description: Boşta bir Windows sanal makinesini otomatik olarak kapanmaya yönelik adımlar ve Windows kapatılırken komutunu kaldırma adımları.
ms.topic: article
ms.date: 09/29/2020
ms.openlocfilehash: c6021131787dde4fe23ec4caad107bda2e20158a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91541569"
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
