---
title: Parola koruma aracısı sürüm geçmişi - Azure Active Directory
description: Belgeler sürüm sürümü ve davranış değişikliği geçmişi
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71fd33388cb1bdf7c87c44fb3273c6850122a0cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74847858"
---
# <a name="azure-ad-password-protection-agent-version-history"></a>Azure AD Parola Koruma aracısı sürüm geçmişi

## <a name="121250"></a>1.2.125.0

Yayın tarihi: 22.03.2019

* Olay günlüğü iletilerinde küçük yazım hatası hatalarını düzeltme
* EULA anlaşmasını son Genel Kullanılabilirlik sürümüyle güncelleştirme

> [!NOTE]
> Build 1.2.125.0 Genel Kullanılabilirlik yapısıdır. Tekrar herkese teşekkür ederiz ürün hakkında geribildirim sağlamıştır!

## <a name="121160"></a>1.2.116.0

Yayın tarihi: 13.03.2019

* Get-AzureADPasswordProtectionProxy ve Get-AzureADPasswordProtectionDCAgent cmdlets şimdi aşağıdaki sınırlamalar ile yazılım sürümü ve geçerli Azure kiracı rapor:
  * Yazılım sürümü ve Azure kiracı verileri yalnızca 1.2.116.0 veya sonraki sürümçalıştıran DC aracıları ve yardımcıları için kullanılabilir.
  * Azure kiracı verileri, proxy veya ormanın yeniden kaydedilmesi (veya yenilenmesi) gerçekleşene kadar bildirilemeyebilir.
* Proxy hizmeti artık .NET 4.7'nin yüklü olduğunu gerektirir.
  * .NET 4.7 zaten tam olarak güncellenmiş bir Windows Server yüklü olmalıdır. Bu durumda, windows [için .NET Framework 4.7 çevrimdışı yükleyicide](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows)bulunan yükleyiciyi indirin ve çalıştırın.
  * Server Core sistemlerinde başarılı olması için /q bayrağını .NET 4.7 yükleyicisine aktarmak gerekebilir.
* Proxy hizmeti artık otomatik yükseltmeyi destekler. Otomatik yükseltme, Proxy hizmetiyle yan yana yüklenen Microsoft Azure AD Connect Agent Updater hizmetini kullanır. Otomatik yükseltme varsayılan olarak açıktır.
* Otomatik yükseltme, Ayar-AzureADPasswordProtectionProxyConfiguration cmdlet kullanılarak etkinleştirilebilir veya devre dışı bırakılabilir. Geçerli ayar Get-AzureADPasswordProtectionProxyConfiguration cmdlet kullanılarak sorgulanabilir.
* DC aracı hizmetinin hizmet ikilisi AzureADPasswordProtectionDCAgent.exe olarak yeniden adlandırılmıştır.
* Proxy hizmetinin hizmet ikilisi AzureADPasswordProtectionProxy.exe olarak yeniden adlandırılmıştır. Bir üçüncü taraf güvenlik duvarı kullanılıyorsa güvenlik duvarı kurallarının buna göre değiştirilmesi gerekebilir.
  * NOT: Bir önceki Proxy yüklemesinde http proxy config dosyası kullanılıyorsa, bu yükseltmeden sonra yeniden adlandırılması gerekir *(proxyservice.exe.config'den* *AzureADPasswordProtectionProxy.exe.config'e).*
* Dc aracısından zaman sınırlı tüm işlevsellik denetimleri kaldırıldı.
* Küçük hatalar giderir ve günlük iyileştirmeleri.

## <a name="12650"></a>1.2.65.0

Yayın tarihi: 2/1/2019

Değişiklik:

* DC aracısı ve proxy hizmeti artık Server Core'da desteklenmiştir. Mininimum işletim sistemi gereksinimleri öncekinden değişmez: DC aracıları için Windows Server 2012 ve yakınlıklar için Windows Server 2012 R2.
* Register-AzureADPasswordProtectionProxy ve Register-AzureADPasswordProtectionForest cmdlets artık aygıt kodu tabanlı Azure kimlik doğrulama modlarını destekler.
* Get-AzureADPasswordProtectionDCAgent cmdlet, karışık ve/veya geçersiz hizmet bağlantı noktalarını yok sayacaktır. Bu, etki alanı denetleyicilerinin bazen çıktıda birden çok kez görüneceği hatayı giderir.
* Get-AzureADPasswordProtectionSummaryReport cmdlet karışık ve/veya geçersiz hizmet bağlantı noktalarını yok sayacaktır. Bu, etki alanı denetleyicilerinin bazen çıktıda birden çok kez görüneceği hatayı giderir.
* Proxy powershell modülü artık %ProgramFiles%\WindowsPowerShell\Modules adresinden kaydedilmiştir. Makinenin PSModulePath ortam değişkeni artık değiştirilmemiştir.
* Bir ormanveya etki alanında kayıtlı proxy'lerin bulunmasına yardımcı olmak için yeni bir Get-AzureADPasswordProtectionProxy cmdlet eklendi.
* DC aracısı, parola ilkelerini ve diğer dosyaları çoğaltmak için sysvol paylaşımında yeni bir klasör kullanır.

   Eski klasör konumu:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Yeni klasör konumu:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   (Bu değişiklik yanlış pozitif "yetim GPO" uyarıları önlemek için yapıldı.)

   > [!NOTE]
   > Eski klasör ve yeni klasör arasında geçiş veya veri paylaşımı yapılmaz. Eski DC aracı sürümleri, bu sürüme yükseltilene veya daha sonra sınana kadar eski konumu kullanmaya devam eder. Tüm DC aracıları sürüm 1.2.65.0 veya sonraki sürümçalıştırdıktan sonra, eski sysvol klasörü el ile silinebilir.

* DC aracısı ve proxy hizmeti artık kendi servis bağlantı noktalarının parçalanmış kopyalarını algılar ve siler.
* Her DC aracısı, hem DC aracısı hem de proxy servis bağlantı noktaları için etki alanında ezilmiş ve eski hizmet bağlantı noktalarını düzenli aralıklarla siler. Kalp atışı zaman damgası yedi günden eskiyse, hem DC aracısı hem de proxy hizmeti bağlantı noktaları bayat olarak kabul edilir.
* DC ajanı artık orman sertifikasını gerektiği gibi yenileyecek.
* Proxy hizmeti artık gerektiğinde proxy sertifikasını yeniler.
* Parola doğrulama algoritması güncelleştirmeleri: genel yasaklı parola listesi ve müşteriye özel yasaklı parola listesi (yapılandırılırsa) parola doğrulamalarından önce birleştirilir. Belirli bir parola, hem genel hem de müşteriye özel listeden belirteçler içeriyorsa reddedilebilir (yalnızca başarısız veya denetim) olabilir. Olay günlüğü belgeleri bunu yansıtacak şekilde güncelleştirildi; lütfen [Azure AD Parola Korumayı İzle'ye](howto-password-ban-bad-on-premises-monitor.md)bakın.
* Performans ve sağlamlık düzeltmeleri
* Geliştirilmiş günlük

> [!WARNING]
> Zaman sınırlı işlevsellik: Bu sürümdeki DC aracı hizmeti (1.2.65.0) 1 Eylül 2019 itibariyle parola doğrulama isteklerini işlemeyi durdurur.  Önceki sürümlerde DC acente hizmetleri (aşağıdaki listeye bakın) 1 Temmuz 2019 tarihinden itibaren işleme durdurulacaktır. Tüm sürümlerde DC aracı hizmeti, bu son teslim tarihlerine kadar olan iki ay içinde 10021 olaylarını Admin etkinlik günlüğüne kaydeder. Tüm zaman sınırı kısıtlamaları yaklaşan GA sürümünde kaldırılacaktır. Proxy aracısı hizmeti herhangi bir sürümde zaman sınırlı değildir, ancak sonraki tüm hata düzeltmelerinden ve diğer geliştirmelerden yararlanmak için yine de en son sürüme yükseltilmelidir.

## <a name="12250"></a>1.2.25.0

Yayın tarihi: 11.01.2018

Giderir:

* DC aracısı ve proxy hizmeti artık sertifika güven hataları nedeniyle başarısız olmalıdır.
* DC aracısı ve proxy hizmeti, FIPS uyumlu makineler için ek düzeltmelere sahiptir.
* Proxy hizmeti artık TLS 1.2-yalnızca ağ ortamında düzgün çalışacaktır.
* Küçük performans ve sağlamlık düzeltmeleri
* Geliştirilmiş günlük

Değişiklik:

* Proxy hizmeti için gereken minimum işletim sistemi düzeyi artık Windows Server 2012 R2'dir. DC aracı hizmeti için gereken minimum işletim sistemi düzeyi Windows Server 2012'de kalır.
* Proxy hizmeti artık .NET sürüm 4.6.2 gerektirir.
* Parola doğrulama algoritması genişletilmiş bir karakter normalleştirme tablosu kullanır. Bu, önceki sürümlerde kabul edilen parolaların reddedilmesine neden olabilir.

## <a name="12100"></a>1.2.10.0

Yayın tarihi: 17.08.2018

Giderir:

* Register-AzureADPasswordProtectionProxy ve Register-AzureADPasswordProtectionForest artık çok faktörlü kimlik doğrulamasını destekliyor
* Register-AzureADPasswordProtectionProxy şifreleme hatalarını önlemek için etki alanında bir WS2012 veya daha sonra etki alanı denetleyicisi gerektirir.
* DC aracı hizmeti, başlangıçta Azure'dan yeni bir parola ilkesi isteme konusunda daha güvenilirdir.
* DC aracı hizmeti gerekirse her saat için Azure'dan yeni bir parola ilkesi ister, ancak şimdi bunu rasgele seçilen bir başlangıç saatinde yapar.
* DC aracı hizmeti, çoğaltma olarak tanıtılıncaönce bir sunucuya yüklendiğinde yeni DC reklamında artık belirsiz bir gecikmeye neden olmaz.
* DC aracı hizmeti artık "Windows Server Active Directory'de parola korumayı etkinleştir" yapılandırma ayarına uygun olacaktır
* Hem DC aracısı hem de proxy yükleyiciler artık gelecekteki sürümlere yükseltirken yerinde yükseltmeyi destekleyecektir.

> [!WARNING]
> Sürüm 1.1.10.3'ten yerinde yükseltme desteklenmez ve yükleme hatasına neden olur. Sürüm 1.2.10 veya daha sonra yükseltmek için, önce dc aracısı ve proxy hizmet yazılımı tamamen kaldırmak, sonra sıfırdan yeni sürümü yüklemeniz gerekir. Azure AD parola koruma Proxy hizmetinin yeniden kaydedilmesi gereklidir.  Ormanı yeniden kaydettirmek gerekmez.

> [!NOTE]
> DC aracı yazılımının yerinde yükseltmeleri yeniden başlatılmasını gerektirir.

* DC aracısı ve proxy hizmeti artık yalnızca FIPS uyumlu algoritmaları kullanacak şekilde yapılandırılan bir sunucuda çalıştırmayı destekliyor.
* Küçük performans ve sağlamlık düzeltmeleri
* Geliştirilmiş günlük

## <a name="11103"></a>1.1.10.3

Yayın tarihi: 15.06.2018

İlk genel önizleme sürümü

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD Parola Koruması Dağıtma](howto-password-ban-bad-on-premises-deploy.md)
