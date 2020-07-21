---
title: Kasa tanılama ayarlarını ölçekte yapılandırma
description: Azure Ilkesini kullanarak belirli bir kapsamdaki tüm kasaları için Log Analytics tanılama ayarlarını yapılandırma
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 2400be15dcd46084e9a605076c00cf5c5ac92463
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86498058"
---
# <a name="configure-vault-diagnostics-settings-at-scale"></a>Kasa tanılama ayarlarını ölçekte yapılandırma

Azure Backup tarafından sunulan raporlama çözümü Log Analytics (LA) kullanır. Belirli bir kasadaki verilerin LA 'ya gönderilmesi için bu kasa için bir [Tanılama ayarı](./backup-azure-diagnostic-events.md) oluşturulması gerekir.

Genellikle, kasa başına el ile bir tanılama ayarı eklemek çok bir görev olabilir. Ayrıca, bu kasanın raporlarını görüntüleyebilmek için oluşturulan tüm yeni kasaların tanılama ayarlarının etkinleştirilmiş olması gerekir.

Tanılama ayarlarının ölçeğe göre (hedef olarak LA birlikte) oluşturulmasını basitleştirmek için, Azure Backup yerleşik bir [Azure ilkesi](../governance/policy/index.yml)sağlar. Bu ilke, belirli bir abonelik veya kaynak grubundaki tüm kasaları bir LA tanılama ayarı ekler. Aşağıdaki bölümler, bu ilkenin nasıl kullanılacağına ilişkin yönergeler sağlar.

## <a name="supported-scenarios"></a>Desteklenen Senaryolar

* İlke, belirli bir abonelikteki tüm kurtarma hizmetleri kasalarına (veya aboneliğin içindeki bir kaynak grubuna) tek seferde uygulanabilir. İlkeyi atayan kullanıcının, ilkenin atandığı aboneliğe ' Owner ' erişimi olması gerekir.

* Kullanıcı tarafından belirtilen LA çalışma alanı (Tanılama verilerinin gönderileceği yer), ilkenin atandığı kasalardan farklı bir abonelikte olabilir. Kullanıcının belirtilen LA çalışma alanının bulunduğu aboneliğe ' Reader ', ' katkıda bulunan ' veya ' Owner ' erişimi olması gerekir.

* Yönetim grubu kapsamı şu anda desteklenmiyor.

* Yerleşik ilke şu anda ulusal bulutlarda kullanılamaz.

## <a name="assigning-the-built-in-policy-to-a-scope"></a>Yerleşik ilkeyi bir kapsama atama

Gerekli kapsamdaki kasaların ilkesini atamak için aşağıdaki adımları izleyin:

1. Azure portal oturum açın ve **ilke** panosuna gidin.
2. Azure kaynakları genelinde tüm yerleşik ilkelerin listesini almak için sol menüdeki **tanımlar** ' ı seçin.
3. **Kategori = izleme**için listeyi filtreleyin. **[Önizleme] adlı ilkeyi bulun: kaynağa özgü kategoriler için Log Analytics çalışma alanına kurtarma hizmetleri Kasası Için tanılama ayarlarını dağıtın**.

    ![İlke tanımı dikey penceresi](./media/backup-azure-policy-configure-diagnostics/policy-definition-blade.png)

4. İlkenin adına tıklayın. Bu ilkenin ayrıntılı tanımına yönlendirilirsiniz.

    ![Ayrıntılı Ilke tanımı](./media/backup-azure-policy-configure-diagnostics/detailed-policy-definition.png)

5. Dikey pencerenin üst kısmındaki **ata** düğmesine tıklayın. Bu, sizi **Ilke ata** dikey penceresine yönlendirir.

6. **Temel bilgiler**altında, **kapsam** alanının yanındaki üç noktaya tıklayın. Bu, ilke için uygulanacak aboneliği seçebileceğiniz sağ bağlam dikey penceresini açar. Ayrıca, ilkenin yalnızca belirli bir kaynak grubundaki kasaların uygulanmasını sağlamak için isteğe bağlı olarak bir kaynak grubu seçebilirsiniz.

    ![İlke atama temelleri](./media/backup-azure-policy-configure-diagnostics/policy-assignment-basics.png)

7. **Parametreler**altında aşağıdaki bilgileri girin:

    * **Profil adı** -ilke tarafından oluşturulan tanılama ayarlarına atanacak ad.
    * **Log Analytics çalışma alanı** -tanılama ayarının ilişkilendirilmesi gereken Log Analytics çalışma alanı. Ilke atamasının kapsamındaki tüm kasaların Tanılama verileri belirtilen LA çalışma alanına gönderilir.

    * **Dışlama etiketi adı (isteğe bağlı) ve dışlama etiketi değeri (isteğe bağlı)** -ilke atamasından belirli bir etiket adı ve değeri içeren kasaların dışlanmasını seçebilirsiniz. Örneğin, bir ' isTest ' etiketi ' Yes ' değerine ayarlanmış olan bu kasaları için bir tanılama ayarı **eklenmesini istemiyorsanız,** dışlama etiketi **adı** alanına ' Istest ' ve **dışlama etiketi değeri** alanına ' Yes ' girmelisiniz. Bu iki alanın herhangi biri (veya her ikisi de) boş bırakılırsa, ilke içerdikleri etiketlerin ne olursa olsun tüm ilgili kasaları uygulanır.

    ![İlke atama parametreleri](./media/backup-azure-policy-configure-diagnostics/policy-assignment-parameters.png)

8. **Bir düzeltme görevi oluşturma** -ilke bir kapsama atandıktan sonra, bu kapsamda oluşturulan tüm yeni kasaların yapılandırılmış La tanılama ayarlarını otomatik olarak alması gerekir (kasanın oluşturulması sırasında 30 dakika içinde). Kapsamdaki mevcut kasaları bir tanılama ayarı eklemek için, ilke atama zamanında bir düzeltme görevi tetikleyebilirsiniz. Bir düzeltme görevi tetiklemek için düzeltme onay kutusunu seçin ve **Düzeltme görevi oluşturun**.

    ![İlke atama düzeltmesi](./media/backup-azure-policy-configure-diagnostics/policy-assignment-remediation.png)

9. **Gözden geçir + oluştur** sekmesine gidin ve **Oluştur**' a tıklayın.

## <a name="under-what-conditions-will-the-remediation-task-apply-to-a-vault"></a>Düzeltme görevinin bir kasaya hangi koşullarda uygulanması gerekir?

Düzeltme görevi, ilke tanımına göre uyumlu olmayan kasaların uygulanmasını sağlar. Bir kasa aşağıdaki koşullardan birini karşılıyorsa uyumlu değil:

* Kasa için tanılama ayarı yok.
* Kasa için Tanılama ayarları bulunur, ancak ayarlardan hedefe göre ve geçiş içinde seçilen **kaynağa** özgü **Tüm** kaynağa özgü olaylar etkin değil.

Bu nedenle, bir Kullanıcı AzureDiagnostics modunda etkin AzureBackupReport olayını içeren bir kasaya sahip olsa bile (yedekleme raporları tarafından desteklenir), kaynağa özgü mod tanılama ayarlarını oluşturmak için önerilen yöntem [olduğundan, düzeltme](./backup-azure-diagnostic-events.md#legacy-event)görevi bu kasa için de geçerlidir.

Ayrıca, bir kullanıcının altı kaynağa özgü olayların yalnızca bir alt kümesiyle olan bir Kasası varsa, yedekleme raporları yalnızca altı kaynağa özgü olay etkinleştirildiğinde beklendiği gibi çalışadıklarından bu kasa için düzeltme görevi uygulanır.

> [!NOTE]
>
> Bir kasada **kaynağa özgü kategorilerin bir alt kümesiyle** , belırlı bir La çalışma alanına veri göndermek üzere yapılandırılmış bir tanılama ayarı varsa, ' çalışma alanı x ' deyin ve ilke atamasında sağlanan hedef La çalışma alanı **aynı** ' çalışma alanı x ' ise düzeltme görevi başarısız olur (Bu kasa için).
>
>Bunun nedeni, aynı kaynak üzerinde iki farklı tanılama ayarı tarafından etkinleştirilen olaylar bazı biçimde **çakıştığında** , bu nedenle ayarlar hedefle aynı La çalışma alanına sahip olamaz. İlgili kasada gezinerek ve hedef olarak farklı bir LA çalışma alanıyla bir tanılama ayarı yapılandırarak bu hatayı el ile çözmeniz gerekir.
>
> Varolan tanılama ayarı hedef olarak çalışma alanı X ile yalnızca AzureBackupReport **etkin olduğunda düzeltme görevinin başarısız olmadığını** unutmayın; bu durumda, var olan ayar tarafından etkinleştirilen olaylar ve düzeltme görevi tarafından oluşturulan ayar tarafından etkinleştirilen olaylar arasında çakışma olmayacaktır.

## <a name="next-steps"></a>Sonraki Adımlar

* [Yedekleme raporlarını nasıl kullanacağınızı öğrenin](./configure-reports.md)
* [Azure Ilkesi hakkında daha fazla bilgi edinin](../governance/policy/index.yml)
* [Bir verme kapsamındaki tüm VM 'Lerin yedeklemesini otomatik olarak etkinleştirmek için Azure Ilkesini kullanın](./backup-azure-auto-enable-backup.md)
