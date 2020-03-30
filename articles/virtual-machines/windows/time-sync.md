---
title: Azure'da Windows VM'leri için zaman eşitleme
description: Windows sanal makineleri için zaman eşitleme.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/17/2018
ms.author: cynthn
ms.openlocfilehash: dd2ae2159c43da6a049d67cae739f111eba682c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74534464"
---
# <a name="time-sync-for-windows-vms-in-azure"></a>Azure'da Windows VM'leri için zaman eşitleme

Zaman eşitleme güvenlik ve olay korelasyon için önemlidir. Bazen dağıtılmış hareketler uygulaması için kullanılır. Birden çok bilgisayar sistemi arasındaki zaman doğruluğu senkronizasyon yoluyla elde edilir. Eşitleme, zaman kaynağı ile zamanı alan bilgisayar arasındaki yeniden başlatma lar ve ağ trafiği de dahil olmak üzere birden çok şeyden etkilenebilir. 

Azure artık Windows Server 2016 çalıştıran altyapı tarafından desteklenen. Windows Server 2016, zamanı düzeltmek ve yerel saati UTC ile eşitlemek üzere koşullandırmak için kullanılan algoritmaları geliştirmiştir.  Windows Server 2016, VM'lerin doğru bir süre için ana bilgisayarla nasıl eşitlediğini yöneten VMICTimeSync hizmetini de geliştirdi. Geliştirmeler, Windows Saati 'ne (W32time) sağlanan örnekler için VM başlatma veya VM geri yüklemesi ve gecikme süresini kesmede daha doğru başlangıç süresini içerir. 


>[!NOTE]
>Windows Time hizmetine hızlı bir genel bakış için, bu [üst düzey genel bakış videosuna](https://aka.ms/WS2016TimeVideo)bir göz atın.
>
> Daha fazla bilgi [için Windows Server 2016 için doğru zaman](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)anına bakın. 

## <a name="overview"></a>Genel Bakış

Bilgisayar saatinin doğruluğu, bilgisayar saatinin Eşgüdümlü Evrensel Saat (UTC) zaman standardına ne kadar yakın olduğu ölçülür. UTC, 300 yıl içinde sadece bir saniye kapalı olabilir hassas atom saatleri çokuluslu bir örnek tarafından tanımlanır. Ancak, UTC okuma doğrudan özel donanım gerektirir. Bunun yerine, zaman sunucuları UTC ile senkronize edilir ve ölçeklenebilirlik ve sağlamlık sağlamak için diğer bilgisayarlardan erişilir. Her bilgisayarın, sunucuların ne zaman kullanılacağını bilen ve bilgisayar saatinin düzeltilmesi gerekip gerekmediğini periyodik olarak kontrol eden ve gerekirse zamanı ayarlayan zaman eşitleme hizmeti vardır. 

Azure ana bilgisayarları, Microsoft'a ait Stratum 1 aygıtlarından GPS antenleri ile zaman ayıran dahili Microsoft zaman sunucularıyla senkronize edilir. Azure'daki sanal makineler, doğru zamanı *(ana bilgisayar zamanı)* VM'ye geçirmek için ana bilgisayarlarına güvenebilir veya VM doğrudan bir zaman sunucusundan veya her ikisinin birleşiminden zaman alabilir. 

Ana bilgisayarla sanal makine etkileşimleri de saati etkileyebilir. [Bellek koruma bakımı](../maintenance-and-updates.md#maintenance-that-doesnt-require-a-reboot)sırasında, VM'ler 30 saniyeye kadar duraklatıldı. Örneğin, bakım başlamadan önce VM saati 10:00:00'ı gösterir ve 28 saniye sürer. VM devam ettikten sonra, VM'deki saat hala 28 saniye kapalı olacak 10:00:00'ı gösterir. Bunu düzeltmek için, VMICTimeSync hizmeti ana bilgisayarda neler olduğunu izler ve vm'lerde değişikliklerin telafi edilmesini ister.

VMICTimeSync hizmeti örnek veya eşitleme modunda çalışır ve yalnızca ileri yi etkiler. W32time'ın çalışmasını gerektiren örnek modda, VMICTimeSync hizmeti her 5 saniyede bir ana bilgisayara anket ler verir ve W32time'a zaman örnekleri sağlar. Yaklaşık her 30 saniyede bir, W32time hizmeti en son saat örneğini alır ve konuğun saatini etkilemek için kullanır. Bir konuğun saati devam ettirildiyse veya bir konuğun saati ev sahibinin saatinin 5 saniye gerisinde sürükleniyorsa eşitleme modu etkinleştirilir. W32time hizmetinin düzgün çalıştığı durumlarda, ikinci durumda asla olmamalıdır.

Zaman eşitleme çalışması olmadan, VM'deki saat hata biriktirir. Yalnızca bir VM olduğunda, iş yükü son derece doğru zaman tutma gerektirmedikçe etki önemli olmayabilir. Ancak çoğu durumda, hareketleri izlemek için zaman kullanan ve tüm dağıtım boyunca tutarlı olması gereken birden çok, birbirine bağlı VM'miz vardır. VM'ler arasındaki süre farklı olduğunda, aşağıdaki efektleri görebilirsiniz:

- Kimlik doğrulama başarısız olur. Kerberos veya sertifikaya bağımlı teknoloji gibi güvenlik protokolleri, sistemler arasında tutarlı olan zamana dayanır. 
- Günlükler (veya diğer veriler) zamanında kabul etmiyorsa, bir sistemde neler olduğunu anlamak çok zordur. Aynı olay farklı zamanlarda meydana geldi gibi görünecek, korelasyon zor hale.
- Saat kapalıysa, faturalandırma yanlış hesaplanabilir.

Windows dağıtımları için en iyi sonuçlar, windows server 2016'yı konuk işletim sistemi olarak kullanarak elde edilir ve bu da zaman eşitlemedeki en son geliştirmeleri kullananızaneden.

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Azure'da barındırılan Windows VM'leriniz için zaman eşitleme yapılandırmak için üç seçenek vardır:

- Ev sahibi zaman ve time.windows.com. Bu, Azure Marketi görüntülerinde kullanılan varsayılan yapılandırmadır.
- Sadece ev sahibi.
- Ana bilgisayar süresi olan veya kullanmadan başka bir harici zaman sunucusu kullanın.


### <a name="use-the-default"></a>Varsayılanı kullanma

Varsayılan olarak Windows OS VM görüntüleri w32time için iki kaynaktan eşitlemek için yapılandırılır: 

- time.windows.com bilgi alan NtpClient sağlayıcısı.
- VMICTimeSync hizmeti, vm'lere ana bilgisayar süresini iletmek ve VM bakım için duraklatıldıktan sonra düzeltmeler yapmak için kullanılır. Azure ana bilgisayarları, doğru zamanı korumak için Microsoft'a ait Stratum 1 aygıtlarını kullanır.

w32time öncelik aşağıdaki sırada zaman sağlayıcı tercih ederdim: tabaka düzeyi, kök gecikme, kök dağılımı, zaman dengeleme. Çoğu durumda, w32time time.windows.com daha düşük tabaka raporları, çünkü ana bilgisayar atime.windows.com tercih ederdim. 

Etki alanı birleştirilmiş makineler için etki alanı kendisi zaman eşitleme hiyerarşisi kurar, ancak orman kökü hala bir yerden zaman almak gerekir ve aşağıdaki hususlar hala doğru tutar.


### <a name="host-only"></a>Yalnızca ev sahibi 

time.windows.com ortak bir NTP sunucusu olduğundan, onunla eşitleme süresi internet üzerinden trafik göndermeyi gerektirir, değişen paket gecikmeleri zaman eşitleme kalitesini olumsuz etkileyebilir. Yalnızca ana bilgisayar eşitlemesine geçerek time.windows.com kaldırmak bazen zaman eşitleme sonuçlarınızı artırabilir.

Varsayılan yapılandırmayı kullanarak zaman eşitleme sorunlarıyla karşılaşırsanız, yalnızca ana bilgisayara geçiş yapmak mantıklıdır. VM'de zaman eşitlemeyi iyileştirip iyileştirmeyeceğini görmek için yalnızca ana bilgisayar eşitlemeyi deneyin. 

### <a name="external-time-server"></a>Dış zaman sunucusu

Belirli zaman eşitleme gereksinimleriniz varsa, dış zaman sunucularını kullanma seçeneğiniz de vardır. Dış zaman sunucuları, Microsoft dışındaki veri merkezlerinde barındırılan makinelerle zaman tekdüzeliği sağlayarak veya artık saniyeleri özel bir şekilde işleme sağlayarak test senaryoları için yararlı olabilecek belirli bir zaman sağlayabilir.

Varsayılan yapılandırmaya benzer sonuçlar sağlamak için harici sunucuları VMICTimeSync hizmeti ve VMICTimeProvider ile birleştirebilirsiniz. 

## <a name="check-your-configuration"></a>Yapılandırmanızı kontrol edin


NtpClient zaman sağlayıcısının açık NTP sunucuları (NTP) veya etki alanı zaman eşitlemi (NT5DS) kullanacak şekilde yapılandırıp yapılandırılmadığını denetleyin.

```
w32tm /dumpreg /subkey:Parameters | findstr /i "type"
```

VM NTP kullanıyorsa, aşağıdaki çıktıyı görürsünüz:

```
Value Name                 Value Type          Value Data
Type                       REG_SZ              NTP
```

NtpClient zaman sağlayıcısının hangi zaman sunucusunu kullandığını yüksek bir komut istemi türünde görmek için:

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

VM varsayılan kullanıyorsa, çıktı aşağıdaki gibi görünür:

```
NtpServer                  REG_SZ              time.windows.com,0x8
```


Şu anda hangi zaman sağlayıcısının kullanıldığını görmek için.

```
w32tm /query /source
```


Burada görebiliyordu çıktı ve ne anlama geleceğini:
    
- **time.windows.com** - varsayılan yapılandırmada, w32time time.windows.com zaman alırdı. Zaman eşitleme kalitesi internet bağlantısına bağlıdır ve paket gecikmelerinden etkilenir. Bu, varsayılan kurulumdan olağan çıktıdır.
- **VM IC Zaman Eşitleme Sağlayıcısı** - VM, ana bilgisayardan gelen zamanı eşitlemektedir. Bu genellikle, yalnızca ana bilgisayar zaman eşitlemesi için kabul ederseniz veya NtpServer şu anda kullanılamıyorsa, bu genellikle bir sonucudur. 
- *Etki alanı sunucunuz* - geçerli makine bir etki alanındadır ve etki alanı zaman eşitleme hiyerarşisini tanımlar.
- *Başka bir sunucu* - w32time açıkça başka bir sunucudan zaman almak için yapılandırılmıştır. Zaman eşitleme kalitesi bu zaman sunucu kalitesine bağlıdır.
- **Yerel CMOS Saati** - saat senkronize değil. W32time yeniden başlatmadan sonra başlamak için yeterli zamanı yoksa veya tüm yapılandırılan zaman kaynakları kullanılamıyorsa bu çıktıyı alabilirsiniz.


## <a name="opt-in-for-host-only-time-sync"></a>Yalnızca ana bilgisayar zaman eşitlemi için kabul et

Azure sürekli olarak ana bilgisayarlarda zaman eşitlemesini geliştirmeye çalışır ve her zaman eşitleme altyapısının Microsoft'a ait veri merkezlerinde toplanmış olduğunu garanti edebilir. Birincil zaman kaynağı olarak time.windows.com kullanmayı tercih eden varsayılan kurulumla zaman eşitleme sorunlarınız varsa, yalnızca ana bilgisayara uygun zaman eşitlemeyi için aşağıdaki komutları kullanabilirsiniz.

VMIC sağlayıcısını etkin olarak işaretleyin. 

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\VMICTimeProvider /v Enabled /t REG_DWORD /d 1 /f
```

NTPClient sağlayıcısını devre dışı bırakılmış olarak işaretleyin.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f
```

w32time Hizmetini yeniden başlatın.

```
net stop w32time && net start w32time
```


## <a name="windows-server-2012-and-r2-vms"></a>Windows Server 2012 ve R2 VM'ler 

Windows Server 2012 ve Windows Server 2012 R2 zaman eşitleme için farklı varsayılan ayarları vardır. W32time varsayılan olarak, hizmetin düşük ek yükü yerine kesin zamana göre yapılandırılır. 

Windows Server 2012 ve 2012 R2 dağıtımlarınızı tam zamanı tercih eden yeni varsayılanları kullanmak üzere taşımak istiyorsanız, aşağıdaki ayarları uygulayabilirsiniz.

W32time anketini ve güncelleştirme aralıklarını Windows Server 2016 ayarlarıyla eşleşecek şekilde güncelleştirin.

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MinPollInterval /t REG_DWORD /d 6 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v MaxPollInterval /t REG_DWORD /d 10 /f
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UpdateInterval /t REG_DWORD /d 100 /f
w32tm /config /update
```

W32time'ın yeni anket aralıklarını kullanabilmesi için NtpServers bunları kullanarak işaretlenir. Sunucular 0x1 bitflag maskesi ile açıklamalı ise, bu mekanizmayı geçersiz kılar ve w32time bunun yerine SpecialPollInterval kullanır. Belirtilen NTP sunucularının 0x8 bayrak kullandığından veya hiç bayrak kullanmadığından emin olun:

Kullanılan NTP sunucuları için hangi bayrakların kullanıldığını denetleyin.

```
w32tm /dumpreg /subkey:Parameters | findstr /i "ntpserver"
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıda zaman eşitleme hakkında daha fazla bilgi için bağlantılar şunlardır:

- [Windows Zaman Hizmeti Araçları ve Ayarları](https://docs.microsoft.com/windows-server/networking/windows-time-service/Windows-Time-Service-Tools-and-Settings)
- [Windows Server 2016 Geliştirmeleri](https://docs.microsoft.com/windows-server/networking/windows-time-service/windows-server-2016-improvements)
- [Windows Server 2016 için Doğru Zaman](https://docs.microsoft.com/windows-server/networking/windows-time-service/accurate-time)
- [Yüksek doğruluklu ortamlar için Windows Time hizmetini yapılandırmak için destek sınırı](https://docs.microsoft.com/windows-server/networking/windows-time-service/support-boundary)


