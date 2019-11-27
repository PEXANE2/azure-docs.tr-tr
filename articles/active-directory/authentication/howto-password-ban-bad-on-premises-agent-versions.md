---
title: Parola koruma Aracısı yayın geçmişi-Azure Active Directory
description: Belgeler sürüm sürümü ve davranış değişikliği geçmişi
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6458f31b21d09e54afb080ffc73598903a9831e0
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74381727"
---
# <a name="azure-ad-password-protection-agent-version-history"></a>Azure AD parola koruma Aracısı sürümü geçmişi

## <a name="121250"></a>1.2.125.0

Yayın tarihi: 3/22/2019

* Olay günlüğü iletilerinde küçük yazım hatası hatalarını çözme
* EULA anlaşmasını son genel kullanılabilirlik sürümüne Güncelleştir

> [!NOTE]
> Build 1.2.125.0, genel kullanılabilirlik yapıdır. Herkesin ürünle ilgili geri bildirimde bulunmak için tekrar teşekkür ederiz!

## <a name="121160"></a>1.2.116.0

Yayın tarihi: 3/13/2019

* Get-AzureADPasswordProtectionProxy ve Get-AzureADPasswordProtectionDCAgent cmdlet 'leri artık yazılım sürümünü ve geçerli Azure kiracısını aşağıdaki sınırlamalara bildirir:
  * Yazılım sürümü ve Azure kiracı verileri yalnızca, sürüm 1.2.116.0 veya üstünü çalıştıran DC aracıları ve proxy 'lerde kullanılabilir.
  * Proxy veya ormanın bir yeniden kaydı (veya yenilemesi) gerçekleşene kadar Azure kiracı verileri raporlanmayabilir.
* Proxy hizmeti artık .NET 4,7 'in yüklü olmasını gerektirir.
  * .NET 4,7, tam olarak güncelleştirilmiş bir Windows Server 'a zaten yüklenmiş olmalıdır. Böyle bir durum söz konusu değilse, [Windows için .NET Framework 4,7 çevrimdışı yükleyicisinde](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows)bulunan yükleyiciyi indirip çalıştırın.
  * Sunucu Çekirdeği sistemlerinde, başarılı olması için/q bayrağını .NET 4,7 yükleyicisine geçirmek gerekebilir.
* Proxy hizmeti artık otomatik yükseltmeyi destekliyor. Otomatik yükseltme, proxy hizmeti ile yan yana yüklenen Microsoft Azure AD Connect Agent Güncelleştirici hizmetini kullanır. Otomatik yükseltme varsayılan olarak açık.
* Set-AzureADPasswordProtectionProxyConfiguration cmdlet 'i kullanılarak otomatik yükseltme etkinleştirilebilir veya devre dışı bırakılabilir. Geçerli ayar Get-AzureADPasswordProtectionProxyConfiguration cmdlet 'i kullanılarak sorgulanabilir.
* DC Aracısı hizmeti için hizmet ikilisi, AzureADPasswordProtectionDCAgent. exe olarak yeniden adlandırıldı.
* Proxy hizmeti için hizmet ikilisi, AzureADPasswordProtectionProxy. exe olarak yeniden adlandırıldı. Bir üçüncü taraf güvenlik duvarı kullanımda olduğunda Güvenlik Duvarı kurallarının de değiştirilmesi gerekebilir.
  * NOTE: bir http proxy yapılandırma dosyası önceki bir proxy yüklemede kullanılıyorsa, bu yükseltmeden sonra yeniden adlandırılması gerekir ( *ProxyService. exe. config* 'Den *Azureadpasswordprotectionproxy. exe. config*dosyasına).
* Tüm zaman sınırlı işlevsellik denetimleri DC aracısından kaldırılmıştır.
* Önemsiz hata düzeltmeleri ve günlüğe kaydetme geliştirmeleri.

## <a name="12650"></a>1.2.65.0

Yayın tarihi: 2/1/2019

Değişikliklerine

* DC Aracısı ve proxy hizmeti artık sunucu çekirdeğinde destekleniyor. En düşük işletim sistemi gereksinimleri, daha önce değiştirilmeden önce: DC aracıları için Windows Server 2012 ve proxy 'ler için Windows Server 2012 R2.
* Register-AzureADPasswordProtectionProxy ve Register-AzureADPasswordProtectionForest cmdlet 'leri artık cihaz kodu tabanlı Azure kimlik doğrulama modlarını desteklemektedir.
* Get-AzureADPasswordProtectionDCAgent cmdlet 'i karışmış ve/veya geçersiz hizmet bağlantı noktalarını yoksayacak. Bu, etki alanı denetleyicilerinin çıktıda bazen birden çok kez göstereceği hatayı düzeltir.
* Get-AzureADPasswordProtectionSummaryReport cmdlet, karışmış ve/veya geçersiz hizmet bağlantı noktalarını yoksayar. Bu, etki alanı denetleyicilerinin çıktıda bazen birden çok kez göstereceği hatayı düzeltir.
* Proxy PowerShell modülü artık%ProgramFiles%\WindowsPowerShell\Modules. adresinden kaydedilir Makinenin PSModulePath ortam değişkeni artık değiştirilmez.
* Bir ormanda veya etki alanında kayıtlı proxy 'leri bulmaya yardımcı olmak için yeni bir get-AzureADPasswordProtectionProxy cmdlet 'i eklenmiştir.
* DC Aracısı, parola ilkelerini ve diğer dosyaları çoğaltmak için SYSVOL paylaşımında yeni bir klasör kullanır.

   Eski klasör konumu:

   `\\<domain>\sysvol\<domain fqdn>\Policies\{4A9AB66B-4365-4C2A-996C-58ED9927332D}`

   Yeni klasör konumu:

   `\\<domain>\sysvol\<domain fqdn>\AzureADPasswordProtection`

   (Yanlış pozitif "yalnız bırakılmış GPO" uyarılarını önlemek için bu değişiklik yapılmıştır.)

   > [!NOTE]
   > Eski klasör ve yeni klasör arasında veri geçişi veya paylaşımı yapılmaz. Eski DC Aracısı sürümleri, bu sürüme veya daha yenisine yükseltilene kadar eski konumu kullanmaya devam edecektir. Tüm DC aracıları sürüm 1.2.65.0 veya üzerini çalıştırıyorsa, eski SYSVOL klasörü el ile silinebilir.

* DC Aracısı ve proxy hizmeti artık ilgili hizmet bağlantı noktalarının karıştırılmış kopyalarını algılayacak ve silecek.
* Her DC Aracısı, etki alanındaki hem DC Aracısı hem de proxy hizmeti bağlantı noktaları için karışık ve eski hizmet bağlantı noktalarını düzenli aralıklarla siler. Hem DC Aracısı hem de proxy hizmeti bağlantı noktaları, sinyal zaman damgası yedi günden daha eskiyse eski olarak değerlendirilir.
* DC Aracısı artık gerektiğinde orman sertifikasını yeniler.
* Proxy hizmeti artık gerektiği şekilde proxy sertifikasını yenileyecek.
* Parola Doğrulama algoritmasına yönelik güncelleştirmeler: genel yasaklanmış parola listesi ve müşteriye özgü yasaklanmış parola listesi (yapılandırıldıysa), parola doğrulamaları öncesinde birleştirilir. Yalnızca genel ve müşteriye özgü listeden belirteçler içeriyorsa, belirli bir parola artık reddedilebilir (başarısız veya yalnızca denetim). Olay günlüğü belgeleri bunu yansıtacak şekilde güncelleştirilmiştir; lütfen bkz. [Azure AD parola korumasını izleme](howto-password-ban-bad-on-premises-monitor.md).
* Performans ve sağlamlık düzeltmeleri
* İyileştirilmiş günlük kaydı

> [!WARNING]
> Zamana sınırlı işlevsellik: Bu sürümdeki DC Aracısı hizmeti (1.2.65.0), 1 Eylül 2019 itibariyle parola doğrulama isteklerini işlemeyi durdurur.  Önceki sürümlerde DC Aracısı Hizmetleri (aşağıdaki listeye bakın) 1 Temmuz 2019 itibariyle işlemeyi durdurur. Tüm sürümlerdeki DC Aracısı hizmeti, 10021 olaylarını iki ay içindeki yönetici olay günlüğüne günlüğe kaydeder ve bu son tarihleri önde kalır. Tüm zaman sınırı kısıtlamaları yakında çıkacak GA sürümünde kaldırılacak. Proxy aracısı hizmeti herhangi bir sürümde zaman sınırlı değildir, ancak sonraki tüm hata düzeltmelerinden ve diğer geliştirmelerden faydalanmak için hala en son sürüme yükseltilmelidir.

## <a name="12250"></a>1.2.25.0

Yayın tarihi: 11/01/2018

Düzeltilen

* DC Aracısı ve proxy hizmeti, sertifika güven hatalarından dolayı artık başarısız olmamalıdır.
* DC Aracısı ve proxy hizmeti, FIPS uyumlu makineler için ek düzeltmeler de vardır.
* Proxy hizmeti artık yalnızca TLS 1,2-ağ ortamında düzgün çalışacaktır.
* Düşük performans ve sağlamlık düzeltmeleri
* İyileştirilmiş günlük kaydı

Değişikliklerine

* Proxy hizmeti için gereken en düşük işletim sistemi düzeyi artık Windows Server 2012 R2 'dir. DC Aracısı hizmeti için gerekli en düşük işletim sistemi düzeyi Windows Server 2012 ' de kalır.
* Proxy hizmeti artık .NET sürüm 4.6.2 gerektirir.
* Parola doğrulama algoritması genişletilmiş bir karakter normalleştirme tablosu kullanır. Bu, önceki sürümlerde kabul edilen parolaların reddedilme oluşmasına neden olabilir.

## <a name="12100"></a>1.2.10.0

Yayın tarihi: 8/17/2018

Düzeltilen

* Register-AzureADPasswordProtectionProxy ve Register-AzureADPasswordProtectionForest artık Multi-Factor Authentication 'ı destekliyor
* Register-AzureADPasswordProtectionProxy, şifreleme hatalarını önlemek için etki alanında bir WS2012 veya üzeri etki alanı denetleyicisi gerektirir.
* DC Aracısı hizmeti, başlangıçta Azure 'dan yeni bir parola ilkesi istemek üzere daha güvenilirdir.
* DC Aracısı hizmeti, gerektiğinde Azure 'dan her saat yeni bir parola ilkesi isteyecek, ancak şimdi rastgele seçilen bir başlangıç zamanında bunu yapacaktır.
* DC Aracısı hizmeti, çoğaltma olarak yükseltilmesinden önce bir sunucuya yüklendiğinde yeni DC tanıtımına yönelik sınırsız gecikmeye neden olmaz.
* DC Aracısı hizmeti artık "Windows Server Active Directory 'da parola korumasını etkinleştir" yapılandırma ayarında yer alacak
* Hem DC Aracısı hem de proxy yükleyicileri, gelecekteki sürümlere yükseltirken yerinde yükseltmeyi de destekleyecektir.

> [!WARNING]
> 1\.1.10.3 sürümünden yerinde yükseltme desteklenmez ve yükleme hatasına neden olur. Sürüm 1.2.10 veya sonraki bir sürüme yükseltmek için önce DC Aracısı ve proxy hizmeti yazılımını tamamen kaldırmanız ve sonra yeni sürümü sıfırdan yüklemeniz gerekir. Azure AD parola koruma proxy hizmeti 'nin yeniden kaydı gereklidir.  Ormanın yeniden kaydedilmesi gerekli değildir.

> [!NOTE]
> DC Aracısı yazılımının yerinde yükseltmeleri için yeniden başlatma gerekir.

* DC Aracısı ve proxy hizmeti artık yalnızca FIPS uyumlu algoritmalar kullanmak üzere yapılandırılmış bir sunucuda çalışmayı destekliyor.
* Düşük performans ve sağlamlık düzeltmeleri
* İyileştirilmiş günlük kaydı

## <a name="11103"></a>1.1.10.3

Yayın tarihi: 6/15/2018

İlk genel önizleme sürümü

## <a name="next-steps"></a>Sonraki adımlar

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
