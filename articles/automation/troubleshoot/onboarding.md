---
title: Azure Otomasyonu yönetim çözümlerini ekleme sorunlarını giderme
description: Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter çözümleriyle ekleme hatalarını giderme hakkında bilgi edinin
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 737b963074a2bec851882bddd78ad0b89f48d1d9
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769906"
---
# <a name="troubleshoot-errors-when-onboarding-update-management-change-tracking-and-inventory"></a>Güncelleştirme Yönetimi, Değişiklik İzleme ve stok ekleme sırasında hata giderme

Güncelleştirme Yönetimi veya Değişiklik İzleme ve envanter gibi çözümleri eklerken hatalarla karşılaşabilirsiniz. Bu makalede oluşabilecek çeşitli hatalar ve bunların nasıl giderileceği açıklanmaktadır.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="node-rename"></a>Senaryo: kayıtlı bir düğümü yeniden adlandırmak için kayıt silme/kaydetme gerekir

#### <a name="issue"></a>Sorun

Bir düğüm Azure Otomasyonu 'na kaydedilir ve işletim sistemi ComputerName değişir.  Düğüm içindeki raporlar özgün adla görünmeye devam eder.

#### <a name="cause"></a>Nedeni

Kayıtlı düğümlerin yeniden adlandırılması, Azure Automation 'da düğüm adını güncelleştirmez.

#### <a name="resolution"></a>Çözünürlük

Azure Otomasyonu durum yapılandırması ' ndan düğümün kaydını kaldırın ve yeniden kaydedin.  Bu süreden önce hizmette yayınlanan raporların artık kullanılamayacak.


### <a name="resigning-cert"></a>Senaryo: https proxy 'si aracılığıyla sertifikaların yeniden imzalanması desteklenmez

#### <a name="issue"></a>Sorun

Müşteriler, HTTPS trafiğini sonlandıran bir ara sunucu çözümüyle bağlantı kurulurken ve ardından yeni bir sertifika kullanarak trafiği yeniden şifrelediği bildirilen hizmet bağlantıya izin vermez.

#### <a name="cause"></a>Nedeni

Azure Otomasyonu, trafiği şifrelemek için kullanılan sertifikaların yeniden imzalanmasını desteklemez.

#### <a name="resolution"></a>Çözünürlük

Bu sorun için geçici çözüm yoktur.

## <a name="general-errors"></a>Genel hatalar

### <a name="missing-write-permissions"></a>Senaryo: ekleme iletiyle başarısız oldu-çözüm etkinleştirilemiyor

#### <a name="issue"></a>Sorun

Bir çözüme sanal makine eklemek istediğinizde aşağıdaki iletilerden birini alırsınız:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Nedeni

Bu hata, sanal makinede, çalışma alanında veya Kullanıcı için hatalı veya eksik izinlere neden olur.

#### <a name="resolution"></a>Çözünürlük

Sanal makineyi eklemek için doğru izinlere sahip olduğunuzdan emin olun. [Makineleri eklemek için gereken izinleri](../automation-role-based-access-control.md#onboarding) gözden geçirin ve çözümü yeniden çalıştırmayı deneyin. `The solution cannot be enabled on this VM because the permission to read the workspace is missing`hata alırsanız, sanal makinenin bir çalışma alanına eklendi olup olmadığını bulmak için `Microsoft.OperationalInsights/workspaces/read` izninizin olduğundan emin olun.

### <a name="diagnostic-logging"></a>Senaryo: ekleme, ileti ile başarısız olur-Otomasyon hesabını tanılama günlüğü için yapılandırma başarısız oldu

#### <a name="issue"></a>Sorun

Bir çözüme sanal makine yüklemeye çalıştığınızda aşağıdaki iletiyi alırsınız:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Nedeni

Fiyatlandırma Katmanı aboneliğin faturalama modeliyle eşleşmezse bu hataya neden olmuş olabilir. Daha fazla bilgi için bkz. [Azure izleyici 'de kullanımı ve tahmini maliyetleri izleme](https://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Çözünürlük

Log Analytics çalışma alanınızı el ile oluşturun ve oluşturduğunuz çalışma alanını seçmek için ekleme işlemini tekrarlayın.

### <a name="computer-group-query-format-error"></a>Senaryo: ComputerGroupQueryFormatError

#### <a name="issue"></a>Sorun

Bu hata kodu, çözümü hedeflemek için kullanılan kayıtlı arama bilgisayar grubu sorgusunun doğru biçimlendirilmediği anlamına gelir. 

#### <a name="cause"></a>Nedeni

Sorguyu değiştirmiş olabilirsiniz veya sistem tarafından değiştirilmiş olabilir.

#### <a name="resolution"></a>Çözünürlük

Bu çözüm için sorguyu silebilir ve çözümü yeniden ekleyebilirsiniz ve sorguyu yeniden oluşturur. Sorgu, **kayıtlı aramalar**altında çalışma alanınız içinde bulunabilir. Sorgunun adı **Microsoftdefaultcomputergroup**, sorgunun kategorisi ise bu sorguyla ilişkili çözümün adıdır. Birden çok çözüm etkinse, **Microsoftdefaultcomputergroup** **kayıtlı aramalarda**birden çok kez görünür.

### <a name="policy-violation"></a>Senaryo: policyihla

#### <a name="issue"></a>Sorun

Bu hata kodu, bir veya daha fazla ilke ihlalinden dolayı dağıtımın başarısız olduğu anlamına gelir.

#### <a name="cause"></a>Nedeni 

İşlemin tamamlanmasını önleyen bir ilke yerinde.

#### <a name="resolution"></a>Çözünürlük

Çözümü başarıyla dağıtmak için, belirtilen ilkeyi değiştirmeyi göz önünde bulundurmanız gerekir. Tanımlanabilir çok sayıda farklı ilke türü olduğundan, gerekli olan belirli değişiklikler ihlal edilen ilkeye bağlıdır. Örneğin, bir ilke, kaynak grubu içindeki belirli kaynak türlerinin içeriğini değiştirme iznini reddeden bir kaynak grubunda tanımlanmışsa, örneğin, aşağıdakilerden birini yapabilirsiniz:

* İlkeyi tamamen kaldırın.
* Farklı bir kaynak grubuna ekleme yapmayı deneyin.
* İlkeyi, örneğin:
  * İlkeyi belirli bir kaynak için yeniden hedefleme (örneğin, belirli bir Otomasyon hesabı).
  * İlkenin reddedecek şekilde yapılandırıldığı kaynak kümesini düzeltme.

Azure portal, sağ üst köşedeki bildirimleri denetleyin veya Otomasyon hesabınızı içeren kaynak grubuna gidin ve başarısız dağıtımı görüntülemek için **Ayarlar** ' ın altında **dağıtımlar** ' ı seçin. Azure Ilkesi hakkında daha fazla bilgi edinmek için bkz. [Azure Ilkesine genel bakış](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="unlink"></a>Senaryo: bir çalışma alanının bağlantısını kaldırılmaya çalışırken hata oluştu

#### <a name="issue"></a>Sorun

Bir çalışma alanının bağlantısını kaldırılmaya çalışırken şu hatayı alıyorsunuz:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Nedeni

Bu hata, Otomasyon hesabınıza ve bağlantılı Log Analytics çalışma alanına bağlı Log Analytics çalışma alanınızda hala etkin olan çözümlere sahip olduğunuzda oluşur.

### <a name="resolution"></a>Çözünürlük

Bu sorunu çözmek için, bunları kullanıyorsanız, çalışma alanınızdan aşağıdaki çözümleri kaldırmanız gerekir:

* Güncelleştirme Yönetimi
* Değişiklik İzleme
* Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma

Çözümleri kaldırdıktan sonra, çalışma alanınızın bağlantısını kaldırabilirsiniz. Çalışma alanınızda ve otomasyon hesabınızda bulunan bu çözümlerdeki mevcut yapıtları de temizlemek önemlidir.  

* Güncelleştirme Yönetimi
  * Otomasyon hesabınızdan güncelleştirme dağıtımlarını (zamanlamalar) kaldırma
* Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma
  * Otomasyon hesabınızdaki Çözüm bileşenlerinde bulunan kilitleri **ayarlar** > **kilitleri**altında kaldırın.
  * VM'leri çalışma saatleri dışında başlat/durdur çözümü kaldırma hakkında ek adımlar için bkz. [çalışma saatleri dışında sanal makineyi Başlat/Durdur çözümü](../automation-solution-vm-management.md##remove-the-solution).

## <a name="mma-extension-failures"></a>MMA uzantısı sorunları

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Bir çözüm dağıtırken ilgili birçok kaynak dağıtılır. Bu kaynaklardan biri, Linux için Microsoft Monitoring Agent uzantısıdır veya Log Analytics aracısıdır. Bunlar, sanal makinenin Konuk Aracısı tarafından, yapılandırılmış Log Analytics çalışma alanıyla iletişim sağlanmasından sorumlu olan sanal makine uzantılarıdır ve bu, ikili dosyaların ve ekleme yaptığınız çözüm, yürütülmeye başladıktan sonra bağlı olarak değişir.
Genellikle ilk olarak, bildirim merkezinde görüntülenen bir bildirimden Linux yükleme hatalarının MMA veya Log Analytics aracısından haberdar olursunuz. Bu bildirime tıkladığınızda, belirli hata hakkında daha fazla bilgi verilir. Kaynak grupları kaynağına, sonra da içindeki dağıtımlar öğesine gezinti, gerçekleşen dağıtım hatalarıyla ilgili ayrıntılar sağlar.
Linux için MMA veya Log Analytics aracısının yüklenmesi çeşitli nedenlerden dolayı başarısız olabilir ve bu hataların ele geçirilmesiyle ilgili adımlar soruna bağlı olarak farklılık gösterir. Belirli sorun giderme adımları izleyin.

Aşağıdaki bölümde, ekleme sırasında, MMA uzantısının dağıtımında hata oluşmasına neden olabilecek çeşitli sorunlar açıklanmaktadır.

### <a name="webclient-exception"></a>Senaryo: bir WebClient isteği sırasında özel durum oluştu

Sanal makinedeki MMA uzantısı dış kaynaklarla iletişim kuramıyor ve dağıtım başarısız oluyor.

#### <a name="issue"></a>Sorun

Aşağıda döndürülen hata iletilerinin örnekleri verilmiştir:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Nedeni

Bu hatanın olası nedenleri şunlardır:

* VM 'de yapılandırılmış ve yalnızca belirli bağlantı noktalarına izin veren bir ara sunucu vardır.

* Bir güvenlik duvarı ayarı, gerekli bağlantı noktalarına ve adreslere erişimi engelledi.

#### <a name="resolution"></a>Çözünürlük

İletişim için uygun bağlantı noktalarına ve adreslere sahip olduğunuzdan emin olun. Bağlantı noktalarının ve adreslerin listesi için bkz. [ağınızı planlama](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Senaryo: geçici bir ortam sorunları nedeniyle yüklenemedi

Başka bir yükleme veya yüklemeyi engelleyen bir eylem nedeniyle Microsoft Monitoring Agent uzantısının yüklenmesi dağıtım sırasında başarısız oldu

#### <a name="issue"></a>Sorun

Aşağıda, hata iletileri örnekleri gösterilebilir:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Nedeni

Bu hatanın olası nedenleri şunlardır:

* Başka bir yükleme devam ediyor
* Sistem, şablon dağıtımı sırasında yeniden başlatılacak şekilde tetiklenir

#### <a name="resolution"></a>Çözünürlük

Bu hata, doğası gereği geçici bir hatadır. Uzantıyı yüklemek için dağıtımı yeniden deneyin.

### <a name="installation-timeout"></a>Senaryo: yükleme zaman aşımı

Bir zaman aşımı nedeniyle MMA uzantısının yüklenmesi tamamlanmadı.

#### <a name="issue"></a>Sorun

Aşağıdaki örnek, döndürülebilecek bir hata iletisidir:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Nedeni

Bu hata, sanal makinenin yükleme sırasında ağır bir yük altında olması nedeniyle oluşur.

### <a name="resolution"></a>Çözünürlük

VM daha düşük bir yük altında olduğunda MMA uzantısını yükleme girişimi.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyası gönderebilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve **Destek Al**' ı seçin.
