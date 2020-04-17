---
title: Azure Otomasyon yönetimi çözümlerine binme de sorun giderme
description: Güncelleştirme Yönetimi, Değişiklik İzleme ve Stok çözümleriyle biniş hatalarını nasıl gidereceğinizi öğrenin
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: ae359e5210a9a11c33dd3ff9b474e28aa2548c57
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536973"
---
# <a name="troubleshoot-errors-when-onboarding-update-management-change-tracking-and-inventory"></a>Güncelleştirme Yönetimi, Değişiklik İzleme ve Envanter'e binerken hata giderme

Güncelleştirme Yönetimi veya İzlemeyi Değiştir ve Envanter gibi çözümlere bindiğinizde hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunları nasıl çözeceğiniaçıklanır.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="scenario-renaming-a-registered-node-requires-unregister--register-again"></a><a name="node-rename"></a>Senaryo: Kayıtlı bir düğümü yeniden adlandırmak için kayıt dışı/ yeniden kaydolun

#### <a name="issue"></a>Sorun

Bir düğüm Azure Automation'a kaydedilir ve ardından işletim sistemi bilgisayar adı değiştirilir.  Düğümden gelen raporlar özgün adla görünmeye devam ediyor.

#### <a name="cause"></a>Nedeni

Kayıtlı düğümleri yeniden adlandırma, Azure Otomasyonu'ndaki düğüm adını güncelleştirmez.

#### <a name="resolution"></a>Çözüm

Düğümü Azure Otomasyon Durumu Yapılandırmasından çıkarın ve yeniden kaydedin.  Bu tarihten önce serviste yayınlanan raporlar artık kullanılamaz.


### <a name="scenario-re-signing-certificates-via-https-proxy-is-not-supported"></a><a name="resigning-cert"></a>Senaryo: https proxy üzerinden sertifikaları yeniden imzalama desteklenmiyor

#### <a name="issue"></a>Sorun

Müşteriler, https trafiğini sona erdiren ve ardından trafiği yeni bir sertifika kullanarak yeniden şifreleyen bir proxy çözümü aracılığıyla bağlanırken, hizmetin bağlantıya izin vermediğini bildirdi.

#### <a name="cause"></a>Nedeni

Azure Otomasyonu, trafiği şifrelemek için kullanılan sertifikaları yeniden imzalamayı desteklemez.

#### <a name="resolution"></a>Çözüm

Bu sorun için geçici çözüm yoktur.

## <a name="general-errors"></a>Genel hatalar

### <a name="scenario-onboarding-fails-with-the-message---the-solution-cannot-be-enabled"></a><a name="missing-write-permissions"></a>Senaryo: Onboarding ileti ile başarısız olur - Çözüm etkinleştirilemez

#### <a name="issue"></a>Sorun

Bir çözüme sanal bir makinede bulunmayı denediğinizde aşağıdaki iletilerden birini alırsınız:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Nedeni

Bu hata, sanal makinede, çalışma alanında veya kullanıcı için yanlış veya eksik izinlerden kaynaklanır.

#### <a name="resolution"></a>Çözüm

Sanal makineye binmek için doğru izinlere sahip olduğundan emin olun. Yerleşik [makineler için gereken izinleri](../automation-role-based-access-control.md#onboarding-permissions) gözden geçirin ve çözüme yeniden binmeye çalışın. Hata `The solution cannot be enabled on this VM because the permission to read the workspace is missing`alırsanız, VM'nin `Microsoft.OperationalInsights/workspaces/read` bir çalışma alanına bindirilip bindirilmediğini bulma iznine sahip olduğundan emin olun.

### <a name="scenario-onboarding-fails-with-the-message---failed-to-configure-automation-account-for-diagnostic-logging"></a><a name="diagnostic-logging"></a>Senaryo: Onboarding ileti ile başarısız - Tanılama günlüğe kaydetme için Otomasyon Hesabı yapılandırmak için başarısız oldu

#### <a name="issue"></a>Sorun

Bir çözüme sanal bir makinede binmeye çalıştığınızda aşağıdaki iletiyi alırsınız:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Nedeni

Fiyatlandırma katmanı aboneliğin faturalandırma modeliyle eşleşmiyorsa, bu hata neden olabilir. Daha fazla bilgi için Azure [Monitor'da İzleme kullanımı ve tahmini maliyetler](https://aka.ms/PricingTierWarning)e bakın.

#### <a name="resolution"></a>Çözüm

Log Analytics çalışma alanınızı el ile oluşturun ve oluşturulan çalışma alanını seçmek için biniş işlemini tekrarlayın.

### <a name="scenario-computergroupqueryformaterror"></a><a name="computer-group-query-format-error"></a>Senaryo: ComputerGroupQueryFormatError

#### <a name="issue"></a>Sorun

Bu hata kodu, çözümü hedeflemek için kullanılan kaydedilmiş arama bilgisayarı grup sorgusunun doğru biçimlendirilmemiş olduğu anlamına gelir. 

#### <a name="cause"></a>Nedeni

Sorguyu değiştirmiş olabilirsiniz veya sistem tarafından değiştirilmiş olabilir.

#### <a name="resolution"></a>Çözüm

Bu çözüm için sorguyu silebilir ve sorguyu yeniden oluşturan çözüme yeniden binebilirsiniz. Sorgu, **Kayıtlı aramalar**altında çalışma alanınızda bulunabilir. Sorgunun adı **MicrosoftDefaultComputerGroup'tur**ve sorgukategorisi bu sorguyla ilişkili çözümün adıdır. Birden çok çözüm etkinleştirilmişse, **MicrosoftDefaultComputerGroup** **Kayıtlı Aramalar**altında birden çok kez gösterir.

### <a name="scenario-policyviolation"></a><a name="policy-violation"></a>Senaryo: İlke İhlali

#### <a name="issue"></a>Sorun

Bu hata kodu, bir veya daha fazla ilkenin ihlali nedeniyle dağıtımın başarısız olduğu anlamına gelir.

#### <a name="cause"></a>Nedeni 

İşlemin tamamlanmasını engelleyen bir ilke vardır.

#### <a name="resolution"></a>Çözüm

Çözümü başarıyla dağıtmak için, belirtilen ilkeyi değiştirmeyi düşünmeniz gerekir. Tanımlanabilecek birçok farklı ilke türü olduğundan, gereken belirli değişiklikler ihlal edilen ilkebağlıdır. Örneğin, bu kaynak grubundaki belirli kaynak türlerinin içeriğini değiştirme iznini reddeden bir kaynak grubunda bir ilke tanımlanmışsa, örneğin aşağıdakilerden herhangi birini yapabilirsiniz:

* İlkeyi tamamen kaldırın.
* Farklı bir kaynak grubuna binmeye çalışın.
* Örneğin, politikayı gözden geçirin:
  * İlkenin belirli bir kaynağa (belirli bir Otomasyon hesabına gibi) yeniden hedeflemesi.
  * İlkenin reddedilecek şekilde yapılandırıldığı kaynak kümesini gözden geçirme.

Azure portalının sağ üst köşesindeki bildirimleri kontrol edin veya otomasyon hesabınızı içeren kaynak grubuna gidin ve başarısız dağıtımı görüntülemek için **Ayarlar** altında **Dağıtımlar'ı** seçin. Azure İlkesi hakkında daha fazla bilgi edinmek için ziyaret edin: [Azure İlkesine Genel Bakış.](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json)

### <a name="scenario-errors-trying-to-unlink-a-workspace"></a><a name="unlink"></a>Senaryo: Çalışma alanının bağlantısını kaldırmaya çalışan hatalar

#### <a name="issue"></a>Sorun

Çalışma alanını kaldırmaya çalışırken aşağıdaki hatayı alırsınız:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Nedeni

Bu hata, Log Analytics çalışma alanınızda Otomasyon Hesabınızın ve Log Analytics çalışma alanınızın bağlanmasına bağlı olarak hala etkin çözümler olduğunda oluşur.

### <a name="resolution"></a>Çözüm

Bunu çözmek için, bunları kullanıyorsanız Aşağıdaki çözümleri Çalışma Alanınızdan kaldırmanız gerekir:

* Güncelleştirme Yönetimi
* Değişiklik İzleme
* Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma

Çözümleri kaldırdıktan sonra çalışma alanınızın bağlantısını kaldırabilirsiniz. Mevcut yapıları çalışma alanınızdan ve Otomasyon Hesabınızdan da temizlemek önemlidir.  

* Güncelleştirme Yönetimi
  * Güncelleştirme Dağıtımlarını (Zamanlamaları) Otomasyon Hesabınızdan Kaldırma
* Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma
  * **Ayarlar** > **Kilitleri**altında Otomasyon Hesabınızdaki çözüm bileşenlerindeki kilitleri kaldırın.
  * Mesai saatleri dışında başlat/durdur un VM'lerini kaldırmak [Remove the Start/Stop VM during off-hours solution](../automation-solution-vm-management.md#remove-the-solution)için ek adımlar için bkz.

## <a name="mma-extension-failures"></a><a name="mma-extension-failures"></a>MMA Uzantı hataları

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Bir çözüm dağıtılırken, çeşitli ilgili kaynaklar dağıtılır. Bu kaynaklardan biri, Linux için Microsoft İzleme Aracısı Uzantısı veya Log Analytics aracısıdır. Bunlar, oturum açtığınız çevrimiçi ve diğer dosyaların daha sonra karşıdan yüklenmesi amacıyla, yapılandırılan Log Analytics çalışma alanıyla iletişim kurmaktan sorumlu sanal makinenin Konuk Aracısı tarafından yüklenen Sanal Makine Uzantılarıdır.
Genellikle bildirimler Hub'ında görünen bir bildirimden Linux yükleme hataları için MMA veya Log Analytics aracısından haberdar olursunuz. Bu bildirime tıkladığınızda, belirli hata hakkında daha fazla bilgi verir. Kaynak Grupları kaynağına ve içindeki Dağıtımlar öğesine gezinme, gerçekleşen dağıtım hataları hakkında da ayrıntılar sağlar.
Linux için MMA veya Log Analytics aracısının yüklenmesi çeşitli nedenlerle başarısız olabilir ve bu hataları gidermek için atılacak adımlar soruna bağlı olarak değişir. Belirli sorun giderme adımları izler.

Aşağıdaki bölümde, MMA uzantısı dağıtımında başarısızlığa neden olan uçağa binerken karşılaşabileceğiniz çeşitli sorunlar açıklanmaktadır.

### <a name="scenario-an-exception-occurred-during-a-webclient-request"></a><a name="webclient-exception"></a>Senaryo: Web İstemci isteği sırasında bir özel durum oluştu

Sanal makinedeki MMA uzantısı dış kaynaklarla iletişim kuramıyor ve dağıtım başarısız oluyor.

#### <a name="issue"></a>Sorun

Döndürülen hata iletilerine örnekler aşağıda verilmiştir:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Nedeni

Bu hatanın bazı olası nedenleri şunlardır:

* VM'de yapılandırılan ve yalnızca belirli bağlantı noktalarına izin veren bir proxy var.

* Güvenlik duvarı ayarı, gerekli bağlantı noktalarına ve adreslere erişimi engelledi.

#### <a name="resolution"></a>Çözüm

İletişim için uygun bağlantı noktalarına ve adreslere sahip olduğundan emin olun. Bağlantı noktaları ve adreslerin listesi için [ağınızı planlama](../automation-hybrid-runbook-worker.md#network-planning)bölümüne bakın.

### <a name="scenario-install-failed-because-of-a-transient-environment-issues"></a><a name="transient-environment-issue"></a>Senaryo: Geçici ortam sorunları nedeniyle yüklenilen başarısız

Microsoft İzleme Aracısı uzantısının yüklenmesi, yüklemeyi engelleyen başka bir yükleme veya eylem nedeniyle dağıtım sırasında başarısız oldu

#### <a name="issue"></a>Sorun

Aşağıdaki hata iletileri örnekleri döndürülebilir şunlardır:

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

Bu hatanın bazı olası nedenleri şunlardır:

* Başka bir yükleme devam ediyor
* Sistem şablon dağıtımı sırasında yeniden başlatılması için tetiklenir

#### <a name="resolution"></a>Çözüm

Bu hata doğada geçici bir hatadır. Uzantıyı yüklemek için dağıtımı yeniden deneyin.

### <a name="scenario-installation-timeout"></a><a name="installation-timeout"></a>Senaryo: Yükleme zaman

MMA uzantısı yükleme bir zaman nedeniyle tamamlanmadı.

#### <a name="issue"></a>Sorun

Aşağıdaki örnek, döndürülebilecek bir hata iletisidir:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Nedeni

Bu hata, sanal makinenin yükleme sırasında ağır bir yük altında olması nedeniyle oluşur.

### <a name="resolution"></a>Çözüm

VM daha düşük bir yük altındayken MMA uzantısını yüklemeyi dene.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
