---
title: Azure Güvenlik Merkezi Sorun Giderme Kılavuzu | Microsoft Belgeleri
description: Bu kılavuz, Azure Güvenlik Merkezi ile ilgili sorunları gidermesi gereken BT uzmanları, güvenlik analistleri ve bulut yöneticileri içindir.
services: security-center
author: v-miegge
manager: dcscontentpm
ms.service: security-center
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: b5a85f8ae1564d724b826c809261e56577f4783a
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80435538"
---
# <a name="azure-security-center-troubleshooting-guide"></a>Azure Güvenlik Merkezi Sorun Giderme Kılavuzu

Bu kılavuz, kuruluşları Azure Güvenlik Merkezi'ni kullanmayı planlayan ve Güvenlik Merkezi ile ilgili sorunları gidermeye ihtiyaç duyan bilgi teknolojisi (BT) uzmanları, bilgi güvenlik analizi uzmanları ve bulut yöneticileri içindir.

Güvenlik Merkezi, veri toplamak ve depolamak için Log Analytics aracısını kullanır. Daha fazla bilgi edinmek için [Azure Güvenlik Merkezi Platform Geçişi](security-center-platform-migration.md) makalesine bakın. Bu makaledeki bilgiler, Log Analytics temsilcisine geçtikten sonra Güvenlik Merkezi işlevselliğini temsil eder.

## <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Bu kılavuzda Güvenlik Merkezi ile ilgili sorunların nasıl giderildiği açıklanmaktadır.

Uyarı türleri:

* Sanal Makine Davranış Analizi (VMBA)
* Ağ Analizi
* SQL Veritabanı ve SQL Veri Ambarı Analizi
* Bağlamsal Bilgiler

Müşteriler, uyarı türlerine bağlı olarak aşağıdaki kaynakları kullanarak uyarıyı araştırmak için gerekli bilgileri elde edebilir:

* Windows'da Sanal Makine (VM) olay görüntüleyicisindeki güvenlik günlükleri
* Linux'ta AuditD
* Saldırı kaynağındaki Azure etkinlik günlükleri ve etkin tanılama günlükleri.

Müşteriler, uyarı açıklaması ve ilgi düzeyiyle ilgili geri bildirimde bulunabilir. Uyarıya gidin, **Bu işinize yaradı mı?** düğmesini seçin, nedeni seçin ve geri bildirimi anlatan bir açıklama girin. Uyarılarımızı geliştirmek için bu geri bildirim kanalını düzenli olarak takip ediyoruz.

## <a name="audit-log"></a>Denetim günlüğü

Güvenlik Merkezi’nde yapılan sorun giderme işlemlerinin birçoğu, öncelikle başarısız bileşenlere ilişkin [Denetim Günlüğü](../azure-monitor/platform/platform-logs-overview.md) kayıtlarına bakılarak gerçekleştirilir. Denetim günlükleri ile aşağıdakileri belirleyebilirsiniz:

* Hangi işlemlerin gerçekleştirildiği
* İşlemi kimin başlattığı
* İşlemin ne zaman oluştuğu
* İşlemin durumu
* İşlemi araştırmanıza yardımcı olabilecek diğer özelliklerin değerleri

Denetim günlüğü, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE) içerir, ancak okuma işlemlerini (GET) içermez.

## <a name="log-analytics-agent"></a>Log Analytics aracısı

Security Center, Azure sanal makinelerinizden güvenlik verileri toplamak için Azure Monitor hizmeti tarafından kullanılan aracıyla Log Analytics aracısını kullanır. Veri toplama etkinleştirilip aracı hedef makineye doğru şekilde yüklendikten sonra, aşağıdaki işlem yürütülmelidir:

* HealthService.exe

Hizmet yönetim konsolu (services.msc) açarsanız, aşağıda gösterildiği gibi log Analytics aracısı hizmetini de görürsünüz:

![Hizmetler](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

Aracının hangi sürümüne sahip olduğunuzu görmek **için, İşlemler**sekmesinde Görev Yöneticisi'ni açın, **Log Analytics aracısı Hizmetini**bulun, üzerine sağ tıklayın ve **Özellikler'i**tıklatın. **Processes** **Ayrıntılar** sekmesinde aşağıda gösterildiği gibi dosya sürümüne bakın:

![Dosya](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)

## <a name="log-analytics-agent-installation-scenarios"></a>Log Analytics aracı yükleme senaryoları

Log Analytics aracısını bilgisayarınıza yüklerken farklı sonuçlar üretebilecek iki yükleme senaryosu vardır. Desteklenen senaryolar şunlardır:

* **Aracı Güvenlik Merkezi tarafından otomatik olarak yüklenir**: Bu senaryoda hem Güvenlik Merkezi hem de Günlük aramasında uyarıları görüntüleyebilirsiniz. Kaynağın ait olduğu abonelik için güvenlik ilkesinde yapılandırılan e-posta adresine e-posta bildirimleri alırsınız.

* **Azure'da bulunan bir VM'ye el ile yüklenen temsilci**: Bu senaryoda, Şubat 2017'den önce el ile indirilen ve yüklenen aracıları kullanıyorsanız, yalnızca çalışma alanının ait olduğu abonelikte filtre uygulamanız durumunda Güvenlik Merkezi portalındaki uyarıları görüntüleyebilirsiniz. Kaynağın ait olduğu abonelikte filtre ler ederseniz, herhangi bir uyarı görmezsiniz. Çalışma alanının ait olduğu abonelik için güvenlik ilkesinde yapılandırılan e-posta adresine e-posta bildirimleri alırsınız.

> [!NOTE]
> İkinci senaryoda açıklanan davranışı önlemek için aracının en son sürümünü indirdiğinizden emin olun.

## <a name="monitoring-agent-health-issues"></a>Ajan sağlık sorunlarını izleme<a name="mon-agent"></a>

**İzleme durumu**, Güvenlik Merkezi’nin otomatik sağlama için başlatılmış VM’leri ve bilgisayarları neden başarıyla izleyemediğini tanımlar. Aşağıdaki tabloda **İzleme durumu** değerleri, açıklamaları ve çözüm adımları gösterilmektedir.

| İzleme durumu | Açıklama | Çözüm adımları |
|---|---|---|
| Bekleyen aracı yüklemesi | Log Analytics aracısı yüklemesi hala çalışıyor.  Yükleme birkaç saat sürebilir. | Otomatik yükleme işlemi tamamlanana kadar bekleyin. |
| Güç durumu kapalı | VM durduruldu.  Log Analytics aracısı yalnızca çalışan bir VM'ye yüklenebilir. | VM’yi yeniden başlatın. |
| Eksik veya geçersiz Azure VM aracısı | Log Analytics aracısı henüz yüklenmedi.  Güvenlik Merkezi’nin uzantıyı yüklemesi için geçerli bir Azure VM aracısı gereklidir. | Azure VM aracısını VM’ye yükleyin, yeniden yükleyin veya yükseltin. |
| VM durumu yükleme için hazır değil  | VM yüklemeye hazır olmadığından Log Analytics aracısı henüz yüklenmedi. VM aracısı veya VM sağlama ile ilgili bir sorun nedeniyle VM yükleme için hazır değil. | Sanal makinenizin durumunu denetleyin. Portalda **Sanal Makineler**’e geri dönün ve durum bilgisi için VM’yi seçin. |
|Yükleme başarısız oldu - genel hata | Log Analytics aracısı yüklendi, ancak bir hata nedeniyle başarısız oldu. | [Uzantıyı el ile yükleyin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension) veya Güvenlik Merkezi’nin yüklemeyi yeniden denemesi için uzantıyı kaldırın. |
| Yükleme başarısız oldu - yerel aracı zaten yüklü | Log Analytics aracıyükleme başarısız oldu. Güvenlik Merkezi, VM'de zaten yüklü olan yerel bir aracıyı (Log Analytics veya System Center Operations Manager) tanımladı. VM'nin iki ayrı çalışma alanı için bildirdiği çoklu homing yapılandırmasını önlemek için Log Analytics aracı yüklemesi durduruldu. | Çözümlemek için iki yol vardır: [Uzantıyı el ile yükleyin](../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension) ve istediğiniz çalışma alanına bağlayın. Veya, istediğiniz çalışma alanını varsayılan çalışma alanı olarak ayarlayın ve aracının otomatik sağlamasını etkinleştirin.  Bkz. [otomatik sağlamayı etkinleştirme](security-center-enable-data-collection.md). |
| Aracı çalışma alanına bağlanamıyor | Log Analytics aracısı yüklendi ancak ağ bağlantısı nedeniyle başarısız oldu.  İnternet erişiminin olduğundan veya aracı için geçerli bir HTTP ara sunucusunun yapılandırıldığından emin olun. | Bkz. Monitoring agent ağ gereksinimleri. |
| Aracı eksik veya bilinmeyen çalışma alanına bağlandı | Güvenlik Merkezi, VM'ye yüklenen Log Analytics aracısının erişimi olmayan bir çalışma alanına bağlı olduğunu belirlemiştir. | Bu durum iki koşulda meydana gelebilir. Çalışma alanı silindi ve artık mevcut değil. Aracıyı doğru çalışma alanıyla yeniden yükleyin veya aracıyı kaldırıp Güvenlik Merkezi’nin otomatik sağlama yüklemesini tamamlamasını bekleyin. İkinci koşul ise çalışma alanının, Güvenlik Merkezi’nin izinli olmadığı bir aboneliğe ait olmasıdır. Güvenlik Merkezi, aboneliklerin Microsoft Güvenlik Kaynak Sağlayıcısı'na erişim izni vermesini gerektirir. Etkinleştirmek için aboneliği Microsoft Güvenlik Kaynak Sağlayıcısı’na kaydedin. Bu işlem API, PowerShell, portal veya yalnızca Güvenlik Merkezi'ndeki **Genel Bakış** Panosunda abonelik filtrelenerek yapılabilir. Daha fazla bilgi için bkz. [Kaynak sağlayıcıları ve türleri](../azure-resource-manager/management/resource-providers-and-types.md#azure-portal). |
| Aracı yanıt vermiyor veya kimliği eksik | Güvenlik Merkezi, aracı yüklü olsa bile VM’den taranan güvenlik verilerini alamıyor. | Aracı, sinyal de dahil olmak üzere herhangi bir veri bildirmiyor. Aracı zarar görmüş olabilir veya trafiği engelleyen bir durum vardır. Veya aracı veri bildiriyor, ancak bir Azure kaynak kimliği eksik olduğundan verileri Azure VM ile eşleştirmek imkansız. Linux'un sorun giderme için, [Linux için Log Analytics Agent için Sorun Giderme Kılavuzu'na](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md#im-not-seeing-any-linux-data-in-the-oms-portal)bakın. Windows’da sorun gidermek için bkz. [Windows Sanal Makineleri’nde Sorun Giderme](https://github.com/MicrosoftDocs/azure-docs/blob/8c53ac4371d482eda3d85819a4fb8dac09996a89/articles/log-analytics/log-analytics-azure-vm-extension.md#troubleshooting-windows-virtual-machines). |
| Aracı yüklü değil | Veri toplama devre dışıdır. | Güvenlik ilkesinde veri toplamayı açın veya Log Analytics aracısını el ile yükleyin. |

## <a name="troubleshooting-monitoring-agent-network-requirements"></a>Sorun giderme izleme aracısı ağ gereksinimleri<a name="mon-network-req"></a>

Aracıların Güvenlik Merkezi’ne bağlanması ve kaydolması için, bağlantı noktası numaraları ve etki alanı URL’leri dahil olmak üzere ağ kaynaklarına erişebilmesi gerekir.

* Proxy sunucuları için, aracı ayarlarında uygun proxy sunucusu kaynaklarının yapılandırıldığından emin olmanız gerekir. [Proxy ayarlarını değiştirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents) hakkında daha fazla bilgi için bu makaleyi okuyun.
* İnternete erişimi kısıtlayan güvenlik duvarları için, güvenlik duvarınızı Log Analytics erişimine izin verecek şekilde yapılandırmanız gerekir. Aracı ayarlarında bir işlem yapılması gerekmez.

Aşağıdaki tabloda iletişim için gereken kaynaklar gösterilmektedir.

| Aracı Kaynağı | Bağlantı Noktaları | HTTPS denetlemesini atlama |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Evet |
| *.oms.opinsights.azure.com | 443 | Evet |
| *.blob.core.windows.net | 443 | Evet |
| *.azure-automation.net | 443 | Evet |

Aracıyla ekleme sorunları yaşarsanız, [Operations Management Suite ekleme sorunlarını giderme](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) makalesini okuduğunuzdan emin olun.

## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Uç nokta korumasıyla ilgili sorunları giderme işlemi düzgün çalışmıyor

Konuk aracı, [Microsoft Kötü Amaçlı Yazılımdan Koruma](../security/fundamentals/antimalware.md) uzantısının gerçekleştirdiği tüm işlemlerin üst işlemidir. Konuk aracı işleminin başarısız olması durumunda konuk aracının alt işlemi olarak çalışan Microsoft Kötü Amaçlı Yazılımdan Koruma da başarısız olabilir.  Bu gibi senaryolarda aşağıdaki seçenekleri doğrulamanız önerilir:

* Hedef sanal makine özel bir görüntü mü ve sanal makineyi oluşturan kişi konuk aracısını hiç yüklememiş mi?
* Hedef bir Windows sanal makinesi değil de Linux sanal makinesiyse, kötü amaçlı yazılımdan koruma uzantısının Windows sürümünün Linux sanal makinesine yüklenmesi başarısız olur. Linux konuk aracısının işletim sistemi sürümü ve gerekli paketler açısından belirli gereksinimleri vardır ve bu gereksinimler karşılanmazsa sanal makine aracısı burada da çalışmaz.
* Sanal makine konuk aracısının eski bir sürümüyle mi oluşturulmuş? Bu durumda, bazı eski aracıların kendini otomatik olarak daha yeni bir sürüme güncelleştiremediğini ve bu soruna yol açabileceğini unutmamanız gerekir. Kendi görüntülerinizi oluşturuyorsanız konuk aracısının her zaman en son sürümünü kullanın.
* Bazı üçüncü taraf yönetim yazılımları konuk aracısını devre dışı bırakabilir ya da aracının belirli dosya konumlarına erişmesini engelleyebilir. Sanal makinenizde üçüncü taraf yazılım yüklüyse aracının dışlama listesinde olduğundan emin olun.
* Belirli güvenlik duvarı ayarları ya da Ağ Güvenlik Grubu (NSG), konuk aracısı için giden ve gelen trafiği engelleyebilir.
* Belirli bir Erişim Denetimi Listesi (ACL) disk erişimini engelleyebilir.
* Disk alanının yetersiz olması konuk aracısının düzgün çalışmasını engelleyebilir.

Microsoft Kötü Amaçlı Yazılımdan Koruma Kullanıcı Arabirimi varsayılan olarak devre dışıdır; gerektiğinde bu arabirimi nasıl etkinleştirebileceğiniz hakkında daha fazla bilgi edinmek için [Azure Resource Manager Sanal Makinelerinde Dağıtımdan Sonra Microsoft Kötü Amaçlı Yazılımdan Koruma Kullanıcı Arabirimi’ni Etkinleştirme](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) konusunu okuyun.

## <a name="troubleshooting-problems-loading-the-dashboard"></a>Pano yükleme sorunlarını giderme

Güvenlik Merkezi panosunu yüklemeyle ilgili sorun yaşıyorsanız, aboneliği Güvenlik Merkezi’ne kaydeden kullanıcı (Güvenlik Merkezi’ni bu abonelikle ilk kez açan kullanıcı) ile veri toplamayı etkinleştirmek isteyen kullanıcının abonelikte *Sahip* veya *Katkıda Bulunan* rolüne sahip olduğundan emin olun. O andan itibaren, abonelikte *Okuyucu* rolüne sahip kullanıcılar da pano/uyarılar/öneri/ilke sayfasını görebilir.

## <a name="contacting-microsoft-support"></a>Microsoft Destek ile iletişim kurma

Bazı sorunlar bu makalede verilen yönergeler kullanılarak tanımlanabilirken, bazılarını Güvenlik Merkezi genel [Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)’nda da bulabilirsiniz. Ancak, daha fazla sorun giderme bilgisi gerekirse, **Azure portalında** aşağıda gösterildiği gibi yeni bir destek isteği oluşturabilirsiniz:

![Microsoft Destek](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## <a name="see-also"></a>Ayrıca bkz.

Bu belgede, Azure Güvenlik Merkezi'nde güvenlik ilkelerinin nasıl yapılandırılacağını öğrendiniz. Azure Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi Planlama ve İşlemler Kılavuzu](security-center-planning-and-operations-guide.md) - Azure Güvenlik Merkezi'ni benimsemek için tasarım ile ilgili dikkat edilmesi gerekenleri planlama ve anlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) - Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını anlama](security-center-alerts-type.md)
* [Öğretici: Güvenlik olaylarına yanıt verme](tutorial-security-incident.md)
* [Azure Güvenlik Merkezi'nde Uyarıları Doğrulama](security-center-alert-validation.md)
* [Azure Güvenlik Merkezi'ndeki E-posta Önerileri](security-center-provide-security-contact-details.md)
* [Azure Güvenlik Merkezi’nde Güvenlik Olaylarını İşleme](security-center-incident.md)
* [Azure Güvenlik Merkezi'nde Olayları ve Uyarıları Araştırma](security-center-investigation.md)
* [Azure Güvenlik Merkezi algılama özellikleri](security-center-detection-capabilities.md)
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi SSSS](faq-general.md) - Hizmeti kullanma hakkında sık sorulan soruları bulma
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/) — Azure güvenliği ve uyumluluğu yla ilgili blog gönderilerini bulun
