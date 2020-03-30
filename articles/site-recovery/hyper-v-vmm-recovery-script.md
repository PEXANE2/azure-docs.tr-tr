---
title: Azure Site Kurtarma'da kurtarma planına komut dosyası ekleme
description: VMM bulutlarında Hyper-V VM'lerin olağanüstü kurtarma kurtarma planına vmm komut dosyası eklemeyi öğrenin.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: 6902876e066649ae4dff4134fb8cc462f30dd0b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74084865"
---
# <a name="add-a-vmm-script-to-a-recovery-plan"></a>Kurtarma planına VMM komut dosyası ekleme

Bu makalede, Bir Sistem Merkezi Sanal Makine Yöneticisi (VMM) komut dosyası oluşturmak ve [Azure Site Kurtarma](site-recovery-overview.md)bir kurtarma planına eklemek nasıl açıklanır.

Bu makalenin alt kısmında veya Azure Kurtarma [Hizmetleri forumunda](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)herhangi bir yorum veya soru gönderin.

## <a name="prerequisites"></a>Ön koşullar

Kurtarma planlarınızda PowerShell komut dosyalarını kullanabilirsiniz. Kurtarma planından erişilebilmek için komut dosyasını yazmanız ve komut dosyasını VMM kitaplığına yerleştirmeniz gerekir. Komut dosyasını yazarken aşağıdaki hususları aklınızda bulundurun:

* Özel durumların düzgün bir şekilde işletilebilmesi için komut dosyalarının deneme yakalama blokları kullandığından emin olun.
    - Komut dosyasında bir özel durum oluşursa, komut dosyası çalışmayı durdurur ve görev başarısız olarak gösterir.
    - Bir hata oluşursa, komut dosyasının geri kalanı çalışmaz.
    - Planlanmamış bir başarısızlık çalıştırdığınızda bir hata oluşursa, kurtarma planı devam eder.
    - Planlanmış bir başarısızlık çalıştırdığınızda bir hata oluşursa, kurtarma planı durur. Komut dosyasını düzeltin, beklendiği gibi çalıştığından denetleyin ve ardından kurtarma planını yeniden çalıştırın.
        - Komut `Write-Host` kurtarma planı komutdosyasında çalışmıyor. Komut komutunu `Write-Host` komut dosyasında kullanırsanız, komut dosyası başarısız olur. Çıktı oluşturmak için, ana komut dosyanızı çalıştıran bir proxy komut dosyası oluşturun. Tüm çıktının dışarı aktarılmasını sağlamak ** \> ** için komutu kullanın.
        - 600 saniye içinde dönmezse senaryo zamanları dolacak.
        - STDERR'ye bir şey yazılmışsa, komut dosyası başarısız olarak sınıflandırılır. Bu bilgiler komut dosyası yürütme ayrıntılarında görüntülenir.

* Kurtarma planındaki komut dosyaları VMM hizmet hesabı bağlamında çalıştırılan. Bu hesabın komut dosyasının bulunduğu uzak paylaşım için izinleri okuduğundan emin olun. Komut dosyasını VMM hizmet hesabıyla aynı kullanıcı hakları düzeyiyle çalıştırmak için test edin.
* VMM cmdlets bir Windows PowerShell modülünde teslim edilir. VMM konsolu yüklediğinizde modül yüklenir. Modülü komut dosyanıza yüklemek için komut dosyasındaki aşağıdaki komutu kullanın: 

    `Import-Module -Name virtualmachinemanager`

    Daha fazla bilgi için [Windows PowerShell ve VMM ile başlayın.](https://technet.microsoft.com/library/hh875013.aspx)
* VMM dağıtımınızda en az bir kitaplık sunucusuolduğundan emin olun. Varsayılan olarak, bir VMM sunucusuiçin kitaplık paylaşım yolu VMM sunucusunda yerel olarak bulunur. Klasör adı MSCVMMLibrary'dir.

  Kitaplık paylaşım yolunuz uzaksa (veya yerelse ancak MSCVMMLibrary ile paylaşılmamışsa), libserver2.contoso.com\share\\\'i örnek olarak kullanarak \\paylaşımı aşağıdaki gibi yapılandırın:
  
  1. Kayıt Defteri Düzenleyicisi'ni açın ve ardından **HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\Azure Site Kurtarma\Kayıt'a**gidin.

  1. **ScriptLibraryPath** değerini ** \\\libserver2.contoso.com\share olarak\\değiştirin.** FQDN'nin tamamını belirtin. Paylaşım konumuna izin ler sağlayın. Bu, paylaşımın kök düğümüdür. VMM'de kök düğümü denetlemek için kitaplıktaki kök düğümüne gidin. Açılan yol yolun köküdür. Bu, değişkende kullanmanız gereken yoldur.

  1. Komut dosyasını, VMM hizmet hesabıyla aynı kullanıcı haklarına sahip bir kullanıcı hesabı kullanarak test edin. Bu kullanıcı haklarını kullanarak, bağımsız, sınanan komut dosyalarıkurtarma planlarında çalıştıkları şekilde çalıştırılan ları doğrular. VMM sunucusunda, yürütme ilkesini aşağıdaki gibi atlayış yapacak şekilde ayarlayın:

     a. **64 bit Windows PowerShell** konsolunu yönetici olarak açın.
     
     b. **Set-execution policy bypass**girin. Daha fazla bilgi için bkz: [Set-ExecutionPolicy cmdlet'i kullanma.](https://technet.microsoft.com/library/ee176961.aspx)

     > [!IMPORTANT]
     > Yalnızca 64 bit PowerShell konsolunda **Set-executionpolicy bypass'ı** ayarlayın. 32 bit PowerShell konsolu için ayarlarsanız, komut dosyaları çalışmaz.

## <a name="add-the-script-to-the-vmm-library"></a>Komut dosyasını VMM kitaplığına ekleme

VMM kaynak siteniz varsa, VMM sunucusunda bir komut dosyası oluşturabilirsiniz. Ardından, komut dosyasını kurtarma planınıza ekleyin.

1. Kitaplık paylaşımında yeni bir klasör oluşturun. Örneğin, \<VMM sunucu adı>\MSSCVMMLibrary\RPScripts. Klasörü kaynak ve hedef VMM sunucuları üzerine yerleştirin.
1. Komut dosyasını oluşturun. Örneğin, komut dosyası RPScript adını. Komut dosyasının beklendiği gibi çalıştığından doğrulayın.
1. Komut dosyasını \<VMM sunucu adı>\MSSCVMMLibrary klasörüne kaynak ve hedef VMM sunucularına yerleştirin.

## <a name="add-the-script-to-a-recovery-plan"></a>Komut dosyasını kurtarma planına ekleme

Kurtarma planına VM'ler veya çoğaltma grupları ekledikten ve planı oluşturduktan sonra, komut dosyasını gruba ekleyebilirsiniz.

1. Kurtarma planını açın.
1. **Adım** listesinde bir öğe seçin. Ardından, **Komut Dosyası** veya El **Ile Eylem'i**seçin.
1. Komut dosyasının veya eylemin seçili öğeden önce mi yoksa sonra mı ekleyin gerektiğini belirtin. Komut dosyasının konumunu yukarı veya aşağı taşımak için **Yukarı Ve** Aşağı **Taşı** düğmelerini seçin.
1. VMM komut dosyası eklerseniz, **VMM komut dosyasına Failover'ı**seçin. **Komut Dosyası Yolunda**, paylaşıma göreli yolu girin. Örneğin, **\RPScripts\RPScript.PS1**girin.
1. Azure Otomasyon runbook'u eklerseniz, runbook'un bulunduğu Otomasyon hesabını belirtin. Ardından, kullanmak istediğiniz Azure runbook komut dosyasını seçin.
1. Komut dosyasının beklendiği gibi çalıştığından emin olmak için kurtarma planının bir test başarısızlığı yapın.


## <a name="next-steps"></a>Sonraki adımlar
* [Failovers çalıştırma](site-recovery-failover.md)hakkında daha fazla bilgi edinin.

