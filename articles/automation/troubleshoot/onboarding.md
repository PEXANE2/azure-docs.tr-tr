---
title: Azure Otomasyon yönetimi çözümlerinin üzerinde sorun giderme
description: Onboarding hatalarının çözümüyle nasıl giderilen öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: da5152b459f54cbaae5ec168f103f23a237edebd
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81679225"
---
# <a name="troubleshoot-solution-onboarding"></a>Sorun giderme çözümü onboarding

Güncelleştirme Yönetimi çözümüne veya İzleme ve Envanter'i Değiştir çözümüne binerken hatalar alabilirsiniz. Bu makalede, oluşabilecek çeşitli hatalar ve bunları nasıl çözeceğiniaçıklanır.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="scenario-renaming-a-registered-node-requires-unregister-or-register-again"></a><a name="node-rename"></a>Senaryo: Kayıtlı bir düğümü yeniden adlandırmak için kayıt dışı kaldırma veya yeniden kaydetme gerekir

#### <a name="issue"></a>Sorun

Bir düğüm Azure Automation'a kaydedilir ve ardından işletim sistemi bilgisayar adı değiştirilir. Düğümden gelen raporlar özgün adla görünmeye devam ediyor.

#### <a name="cause"></a>Nedeni

Kayıtlı düğümleri yeniden adlandırma, Azure Otomasyonu'ndaki düğüm adını güncelleştirmez.

#### <a name="resolution"></a>Çözüm

Düğümü Azure Otomasyon Durumu Yapılandırmasından çıkarın ve yeniden kaydedin. Bu tarihten önce serviste yayınlanan raporlar artık kullanılamaz.

### <a name="scenario-re-signing-certificates-via-https-proxy-is-not-supported"></a><a name="resigning-cert"></a>Senaryo: https proxy üzerinden sertifikaları yeniden imzalama desteklenmiyor

#### <a name="issue"></a>Sorun

HTTPS trafiğini sona erdiren ve ardından trafiği yeni bir sertifika kullanarak yeniden şifreleyen bir proxy çözümü aracılığıyla bağlandığımda, hizmet bağlantıya izin vermez.

#### <a name="cause"></a>Nedeni

Azure Otomasyonu, trafiği şifrelemek için kullanılan sertifikaları yeniden imzalamayı desteklemez.

#### <a name="resolution"></a>Çözüm

Şu anda bu sorun için geçici çözüm bulunmamaktadır.

## <a name="general-errors"></a>Genel hatalar

### <a name="scenario-onboarding-fails-with-the-message---the-solution-cannot-be-enabled"></a><a name="missing-write-permissions"></a>Senaryo: Onboarding ileti ile başarısız olur - Çözüm etkinleştirilemez

#### <a name="issue"></a>Sorun

Bir VM'den bir çözüme binmeye çalıştığınızda aşağıdaki iletilerden birini alırsınız:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Nedeni

Bu hata, VM veya çalışma alanında veya kullanıcı için yanlış veya eksik izinlerden kaynaklanır.

#### <a name="resolution"></a>Çözüm

Yerleşik makineler için gerekli doğru [izinlere](../automation-role-based-access-control.md#onboarding-permissions) sahip olduğundan emin olun ve ardından çözüme tekrar binmeye çalışın. Hata `The solution cannot be enabled on this VM because the permission to read the workspace is missing`alırsanız, VM'nin `Microsoft.OperationalInsights/workspaces/read` bir çalışma alanına bindirilip bindirilmediğini bulma iznine sahip olduğundan emin olun.

### <a name="scenario-onboarding-fails-with-the-message-failed-to-configure-automation-account-for-diagnostic-logging"></a><a name="diagnostic-logging"></a>Senaryo: Onboarding iletiile başarısız: Tanılama günlüğe kaydetme için otomasyon hesabını yapılandırmak için başarısız oldu

#### <a name="issue"></a>Sorun

Bir VM'den bir çözüme binmeye çalıştığınızda aşağıdaki iletiyi alırsınız:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Nedeni

Fiyatlandırma katmanı aboneliğin faturalandırma modeliyle eşleşmiyorsa, bu hata neden olabilir. Bkz. [Azure Monitor'da Izleme kullanımı ve tahmini maliyetler.](https://aka.ms/PricingTierWarning)

#### <a name="resolution"></a>Çözüm

Log Analytics çalışma alanınızı el ile oluşturun ve oluşturulan çalışma alanını seçmek için biniş işlemini tekrarlayın.

### <a name="scenario-computergroupqueryformaterror"></a><a name="computer-group-query-format-error"></a>Senaryo: ComputerGroupQueryFormatError

#### <a name="issue"></a>Sorun

Bu hata kodu, çözümü hedeflemek için kullanılan kayıtlı arama bilgisayarı grup sorgusunun doğru biçimlendirilmemiş olduğu anlamına gelir. 

#### <a name="cause"></a>Nedeni

Sorguyu değiştirmiş olabilirsiniz veya sistem sorguyu değiştirmiş olabilir.

#### <a name="resolution"></a>Çözüm

Çözüm için sorguyu silebilir ve ardından sorguyu yeniden oluşturan çözüme yeniden binebilirsiniz. Sorgu, **Kayıtlı aramalar**altında çalışma alanınızda bulunabilir. Sorgunun adı **MicrosoftDefaultComputerGroup'tur**ve sorgukategorisi ilişkili çözümün adıdır. Birden çok çözüm etkinleştirilmişse, **MicrosoftDefaultComputerGroup** sorgusu **Kayıtlı Aramalar**altında birden çok kez gösterir.

### <a name="scenario-policyviolation"></a><a name="policy-violation"></a>Senaryo: İlke İhlali

#### <a name="issue"></a>Sorun

Bu hata kodu, bir veya daha fazla ilkenin ihlali nedeniyle dağıtımın başarısız olduğunu gösterir.

#### <a name="cause"></a>Nedeni 

Bir ilke, işlemin tamamlanmasını engelliyor.

#### <a name="resolution"></a>Çözüm

Çözümü başarıyla dağıtmak için, belirtilen ilkeyi değiştirmeyi düşünmelisiniz. Tanımlanabilecek birçok farklı ilke türü olduğundan, gereken değişiklikler ihlal edilen ilkebağlıdır. Örneğin, bazı bulunan kaynakların içeriğini değiştirme iznini reddeden bir kaynak grubunda bir ilke tanımlanırsa, aşağıdaki düzeltmelerden birini seçebilirsiniz:

* İlkeyi tamamen kaldırın.
* Farklı bir kaynak grubuna çözüm onboard deneyin.
* İlkeyi belirli bir kaynağa (örneğin, bir Otomasyon hesabı) yeniden hedefleme.
* İlkenin reddedilecek şekilde yapılandırıldığı kaynak kümesini gözden geçirin.

Azure portalının sağ üst köşesindeki bildirimleri kontrol edin veya Otomasyon hesabınızı içeren kaynak grubuna gidin ve başarısız dağıtımı görüntülemek için **Ayarlar** altında **Dağıtımlar'ı** seçin. Azure İlkesi hakkında daha fazla bilgi edinmek için Azure [İlkesine Genel Bakış](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json)bölümüne bakın.

### <a name="scenario-errors-trying-to-unlink-a-workspace"></a><a name="unlink"></a>Senaryo: Çalışma alanının bağlantısını kaldırmaya çalışan hatalar

#### <a name="issue"></a>Sorun

Çalışma alanını kaldırmaya çalışırken aşağıdaki hatayı alırsınız:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Nedeni

Bu hata, Log Analytics çalışma alanınızda otomasyon hesabınıza ve Log Analytics çalışma alanınızın bağlanmasına bağlı olarak hala etkin çözümler olduğunda oluşur.

### <a name="resolution"></a>Çözüm

Kullanıyorsanız aşağıdaki çözümleri çalışma alanınızdan kaldırın:

* Güncelleştirme Yönetimi
* Değişiklik İzleme ve Stok
* Hizmetin kapalı olduğu saatlerde Sanal Makineleri Başlatma/Durdurma

Çözümleri kaldırdıktan sonra, çalışma alanınızın bağlantısını kaldırabilirsiniz. Bu çözümlerden varolan tüm yapıları çalışma alanınızdan ve Otomasyon hesabınızdan temizlemek önemlidir 

* Güncelleştirme Yönetimi için, Güncelleştirme Dağıtımlarını (Zamanlamaları) Otomasyon hesabınızdan kaldırın.
* Mesai saatleri dışında Başlat/Durdur VM'leri **için, Ayarlar** > **Kilitleri**altında Otomasyon hesabınızdaki çözüm bileşenlerindeki kilitleri kaldırın. Bkz. [Mesai saatleri dışında çözüm sırasında Başlat/Durdur VM'leri kaldırın.](../automation-solution-vm-management.md#remove-the-solution)

## <a name="log-analytics-for-windows-extension-failures"></a><a name="mma-extension-failures"></a>Windows uzantı hataları için Günlük Analizi

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Windows uzantısı için Log Analytics aracısının yüklenmesi çeşitli nedenlerle başarısız olabilir. Aşağıdaki bölümde, Windows uzantısı için Log Analytics aracısının dağıtımı sırasında hatalara neden olabilecek onboarding sorunları açıklanmaktadır.

>[!NOTE]
>Windows için Log Analytics aracısı, Microsoft İzleme Aracısı (MMA) için Azure Otomasyonu'nda şu anda kullanılan addır.

### <a name="scenario-an-exception-occurred-during-a-webclient-request"></a><a name="webclient-exception"></a>Senaryo: Web İstemci isteği sırasında bir özel durum oluştu

VM'deki Windows için Log Analytics dış kaynaklarla iletişim kuramıyor ve dağıtım başarısız oluyor.

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

* VM'de yapılandırılan bir proxy yalnızca belirli bağlantı noktalarına izin verir.
* Güvenlik duvarı ayarı, gerekli bağlantı noktalarına ve adreslere erişimi engelledi.

#### <a name="resolution"></a>Çözüm

İletişim için uygun bağlantı noktalarına ve adreslere sahip olduğundan emin olun. Bağlantı noktaları ve adreslerin listesi için [ağınızı planlama](../automation-hybrid-runbook-worker.md#network-planning)bölümüne bakın.

### <a name="scenario-install-failed-because-of-a-transient-environment-issues"></a><a name="transient-environment-issue"></a>Senaryo: Geçici ortam sorunları nedeniyle yüklenilen başarısız

Windows için Log Analytics'in yüklenmesi, yüklemeyi engelleyen başka bir yükleme veya eylem nedeniyle dağıtım sırasında başarısız oldu

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

* Başka bir yükleme devam ediyor.
* Sistem şablon dağıtımı sırasında yeniden başlatılmak üzere tetiklenir.

#### <a name="resolution"></a>Çözüm

Bu hata doğada geçicidir. Uzantıyı yüklemek için dağıtımı yeniden deneyin.

### <a name="scenario-installation-timeout"></a><a name="installation-timeout"></a>Senaryo: Yükleme zaman

Windows uzantısı için Log Analytics'in yüklenmesi bir zaman ası nedeniyle tamamlanmadı.

#### <a name="issue"></a>Sorun

Döndürülebilecek bir hata iletisi örneği aşağıda verilmiştir:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Nedeni

VM yükleme sırasında ağır bir yük altında olduğundan bu tür bir hata oluşur.

### <a name="resolution"></a>Çözüm

VM daha düşük bir yük altındayken Windows uzantısı için Log Analytics aracısını yüklemeyi deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu yukarıda görmüyorsanız veya sorununuzu çözemiyorsanız, ek destek için aşağıdaki kanallardan birini deneyin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/)aracılığıyla Azure uzmanlarından yanıt alın.
* [@AzureSupport](https://twitter.com/azuresupport)Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.
* Azure destek olayı dosyala. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve Destek **Al'ı**seçin.
