---
title: Otomatik ayar e-posta bildirimleri nasıl-nasıl kılavuzu
description: Azure SQL Veritabanı otomatik sorgu ayarı için e-posta bildirimlerini etkinleştirin.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 06/03/2019
ms.openlocfilehash: 1dbcf953ad5f70c6ddf2a73eef2ea712f1e1278c
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632076"
---
# <a name="email-notifications-for-automatic-tuning"></a>Otomatik ayar için e-posta bildirimleri

SQL Veritabanı ayar önerileri Azure SQL Veritabanı [Otomatik ayar](sql-database-automatic-tuning.md)tarafından oluşturulur. Bu çözüm, dizin oluşturma, dizin silme ve sorgu yürütme planlarının optimizasyonu ile ilgili her bir veritabanı için özelleştirilmiş ayar önerileri sağlayan SQL Veritabanlarının iş yüklerini sürekli olarak izler ve çözümler.

SQL Veritabanı Otomatik ayar önerileri, [REST API](https://docs.microsoft.com/rest/api/sql/databaserecommendedactions/listbydatabaseadvisor) çağrılarıyla veya [T-SQL](https://azure.microsoft.com/blog/automatic-tuning-introduces-automatic-plan-correction-and-t-sql-management/) ve [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaserecommendedaction) komutları kullanılarak alınan [Azure portalında](sql-database-advisor-portal.md)görüntülenebilir. Bu makale, otomatik ayar önerilerini almak için bir PowerShell komut dosyası kullanmaya dayanır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

## <a name="automate-email-notifications-for-automatic-tuning-recommendations"></a>Otomatik ayar önerileri için e-posta bildirimlerini otomatikleştirin

Aşağıdaki çözüm, Otomatik ayar önerileri içeren e-posta bildirimlerinin gönderilmesini otomatikleştirir. Açıklanan çözüm, [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro)kullanarak önerilerin ayarını almak için bir PowerShell komut dosyasının yürütülmesini otomatikleştirmek ve [Microsoft Flow'u](https://flow.microsoft.com)kullanarak e-posta teslim işini zamanlama otomasyonundan oluşur.

## <a name="create-azure-automation-account"></a>Azure Otomasyon hesabı oluşturma

Azure Otomasyonu'nu kullanmak için ilk adım, bir otomasyon hesabı oluşturmak ve PowerShell komut dosyasının yürütülmesi nde kullanmak üzere Azure kaynaklarıyla yapılandırmaktır. Azure Otomasyonu ve yetenekleri hakkında daha fazla bilgi edinmek için Azure [otomasyonuna başlarken](https://docs.microsoft.com/azure/automation/automation-offering-get-started)bkz.

Market'ten Otomasyon uygulamasını seçme ve yapılandırma yöntemiyle Azure Otomasyon Hesabı oluşturmak için aşağıdaki adımları izleyin:

- Azure portalına giriş yapın
- Sol üst köşede "**+ Kaynak Oluştur**" düğmesine tıklayın
- "**Automation**" (enter tuşuna bas)
- Arama sonuçlarında Otomasyon uygulamasına tıklayın

![Azure otomasyonu ekleme](./media/sql-database-automatic-tuning-email-notifications/howto-email-01.png)

- "Bir Otomasyon Hesabı Oluştur" bölmesine girdikten sonra "**Oluştur**"
- Gerekli bilgileri doldurma: Bu otomasyon hesabı için bir ad girin, PowerShell komut dosyası yürütmesi için kullanılacak Azure abonelik kimliğinizi ve Azure kaynaklarınızı seçin
- "**Azure Run As hesabı "** seçeneği için, PowerShell komut dosyasının Azure Otomasyonu yardımıyla çalıştığı hesap türünü yapılandırmak için **Evet'i** seçin. Hesap türleri hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/automation/automation-create-runas-account)
- Oluştur'a tıklayarak otomasyon hesabının oluşturulmasını **sonuçlandırın**

> [!TIP]
> Azure Otomasyon hesap adınızı, abonelik kimliğinizi ve kaynaklarınızı (not defterine kopyala-yapıştır gibi) Otomasyon uygulamasını oluştururken tam olarak girilen gibi kaydedin. Bu bilgiye daha sonra ihtiyacın olacak.
>

Aynı otomasyonu oluşturmak istediğiniz birkaç Azure aboneliğiniz varsa, diğer abonelikleriniz için bu işlemi yinelemeniz gerekir.

## <a name="update-azure-automation-modules"></a>Azure Otomasyon modüllerini güncelleştirin

Otomatik ayar önerisini almak için PowerShell komut dosyası, Azure Modülü sürüm 4 ve üzeri için gerekli olan [Get-AzResource](https://docs.microsoft.com/powershell/module/az.Resources/Get-azResource) ve [Get-AzSqlDatabaseRecommendedAction](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlDatabaseRecommendedAction) komutlarını kullanır.

- Azure Modüllerinizin güncellenmesi gerektiğinde Azure [Otomasyonu'nda Az modülü desteğine](../automation/az-modules.md)bakın.

## <a name="create-azure-automation-runbook"></a>Azure Otomasyon Runbook'u Oluşturma

Bir sonraki adım, azure otomasyonu içinde ayar önerileri nin alınması için PowerShell komut dosyasının bulunduğu bir Runbook oluşturmaktır.

Yeni bir Azure Otomasyonu runbook oluşturmak için aşağıdaki adımları izleyin:

- Önceki adımda oluşturduğunuz Azure Otomasyon hesabına erişin
- Otomasyon hesap bölmesine girince, PowerShell komut dosyasıyla yeni bir Azure Otomasyonu runbook'u oluşturmak için sol taraftaki "**Runbook**" menü öğesine tıklayın. Otomasyon runbook'ları oluşturma hakkında daha fazla bilgi edinmek için [bkz.](../automation/manage-runbooks.md#creating-a-runbook)
- Yeni bir runbook eklemek için "**+Runbook ekle**" menüsü seçeneğine tıklayın ve ardından "**Hızlı oluştur – Yeni bir runbook Oluştur**" seçeneğine tıklayın.
- Runbook bölmesinde, runbook'unuzun adını yazın (bu örneğin amacı için "**AutomaticTuningEmailAutomation**" kullanılır), **PowerShell** olarak runbook türünü seçin ve amacını açıklamak için bu runbook'un açıklamasını yazın.
- Yeni bir runbook oluşturmayı bitirmek için **Oluştur** düğmesine tıklayın

![Azure otomasyon runbook ekle](./media/sql-database-automatic-tuning-email-notifications/howto-email-03.png)

Oluşturulan runbook içine bir PowerShell komut dosyası yüklemek için aşağıdaki adımları izleyin:

- "**PowerShell Runbook"u edit"** bölmesinin içinde, menü ağacında "**RUNBOOK"** seçeneğini belirleyin ve runbook'unuzun adını görene kadar görünümü genişletin (bu örnekte "**AutomaticTuningEmailAutomation**"). Bu runbook'u seçin.
- "PowerShell Runbook'u Düzenle" (1 numaradan başlayarak) ilk satırında aşağıdaki PowerShell komut dosyası kodunu kopyalayıp yapıştırın. Bu PowerShell komut dosyası, başlamak için olduğu gibi sağlanır. Gereksinimlerinizi karşılamak için komut dosyasını değiştirin.

Sağlanan PowerShell komut dosyasının üstbilgisinde Azure `<SUBSCRIPTION_ID_WITH_DATABASES>` abonelik kimliğinizle değiştirmeniz gerekir. Azure abonelik kimliğinizi nasıl alabildiğini öğrenmek için Azure [Abonelik GUID'inizi alma](https://blogs.msdn.microsoft.com/mschray/20../../getting-your-azure-subscription-guid-new-portal/)'ya bakın.

Birkaç abonelik olması durumunda, bunları komut dosyasının üstbilgisindeki "$subscriptions" özelliğiyle sınırlı virgülle sınırlandırılmış olarak ekleyebilirsiniz.

```powershell
# PowerShell script to retrieve Azure SQL Database Automatic tuning recommendations.
#
# Provided "as-is" with no implied warranties or support.
# The script is released to the public domain.
#
# Replace <SUBSCRIPTION_ID_WITH_DATABASES> in the header with your Azure subscription ID.
#
# Microsoft Azure SQL Database team, 2018-01-22.

# Set subscriptions : IMPORTANT – REPLACE <SUBSCRIPTION_ID_WITH_DATABASES> WITH YOUR SUBSCRIPTION ID
$subscriptions = ("<SUBSCRIPTION_ID_WITH_DATABASES>", "<SECOND_SUBSCRIPTION_ID_WITH_DATABASES>", "<THIRD_SUBSCRIPTION_ID_WITH_DATABASES>")

# Get credentials
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Define the resource types
$resourceTypes = ("Microsoft.Sql/servers/databases")
$advisors = ("CreateIndex", "DropIndex");
$results = @()

# Loop through all subscriptions
foreach($subscriptionId in $subscriptions) {
    Select-AzSubscription -SubscriptionId $subscriptionId
    $rgs = Get-AzResourceGroup

    # Loop through all resource groups
    foreach($rg in $rgs) {
        $rgname = $rg.ResourceGroupName;

        # Loop through all resource types
        foreach($resourceType in $resourceTypes) {
            $resources = Get-AzResource -ResourceGroupName $rgname -ResourceType $resourceType

            # Loop through all databases
            # Extract resource groups, servers and databases
            foreach ($resource in $resources) {
                $resourceId = $resource.ResourceId
                if ($resourceId -match ".*RESOURCEGROUPS/(?<content>.*)/PROVIDERS.*") {
                    $ResourceGroupName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*SERVERS/(?<content>.*)/DATABASES.*") {
                    $ServerName = $matches['content']
                } else {
                    continue
                }
                if ($resourceId -match ".*/DATABASES/(?<content>.*)") {
                    $DatabaseName = $matches['content']
                } else {
                    continue
                }

                # Skip if master
                if ($DatabaseName -eq "master") {
                    continue
                }

                # Loop through all Automatic tuning recommendation types
                foreach ($advisor in $advisors) {
                    $recs = Get-AzSqlDatabaseRecommendedAction -ResourceGroupName $ResourceGroupName -ServerName $ServerName  -DatabaseName $DatabaseName -AdvisorName $advisor
                    foreach ($r in $recs) {
                        if ($r.State.CurrentValue -eq "Active") {
                            $object = New-Object -TypeName PSObject
                            $object | Add-Member -Name 'SubscriptionId' -MemberType Noteproperty -Value $subscriptionId
                            $object | Add-Member -Name 'ResourceGroupName' -MemberType Noteproperty -Value $r.ResourceGroupName
                            $object | Add-Member -Name 'ServerName' -MemberType Noteproperty -Value $r.ServerName
                            $object | Add-Member -Name 'DatabaseName' -MemberType Noteproperty -Value $r.DatabaseName
                            $object | Add-Member -Name 'Script' -MemberType Noteproperty -Value $r.ImplementationDetails.Script
                            $results += $object
                        }
                    }
                }
            }
        }
    }
}

# Format and output results for the email
$table = $results | Format-List
Write-Output $table
```

Komut dosyasını kaydetmek için sağ üst köşedeki "**Kaydet**" düğmesini tıklatın. Komut dosyasından memnun olduğunuzda, bu runbook'u yayınlamak için "**Yayımla**" düğmesini tıklatın.

Ana runbook bölmesine, komut dosyasını **test** etmek için "**Başlat**" düğmesine tıklamayı seçebilirsiniz. "**Çıktı**" üzerine tıklayarak komut dosyasının sonuçlarını görüntüleyin. Bu çıktı e-postanızın içeriği olacak. Komut dosyasından örnek çıktı aşağıdaki ekran görüntüsünde görülebilir.

![Azure Otomasyonu ile görünüm otomatik ayar önerilerini çalıştırın](./media/sql-database-automatic-tuning-email-notifications/howto-email-04.png)

PowerShell komut dosyasını gereksinimlerinize göre özelleştirerek içeriği ayarladığından emin olun.

Yukarıdaki adımlarla, Otomatik ayar önerilerini almak için PowerShell komut dosyası Azure Otomasyonu'na yüklenir. Bir sonraki adım, e-posta teslim işini otomatikleştirmek ve zamanlamaktır.

## <a name="automate-the-email-jobs-with-microsoft-flow"></a>Microsoft Flow ile e-posta işlerini otomatikleştirin

Çözümü tamamlamak için, son adım olarak, Microsoft Flow'da üç eylemden (iş) oluşan bir otomasyon akışı oluşturun:

1. "**Azure Otomasyonu - İş yarat**" – Azure Otomasyonu runbook'u içinde Otomatik ayar önerilerini almak için PowerShell komut dosyasını çalıştırmak için kullanılır
2. "**Azure Otomasyonu - İş çıktısını al**" – çalıştırılan PowerShell komut dosyasından çıktı almak için kullanılır
3. "**Office 365 Outlook – E-posta gönder**" – e-posta göndermek için kullanılır. E-postalar, akışı oluşturan kişinin Office 365 hesabı kullanılarak gönderilir.

Microsoft Akışı özellikleri hakkında daha fazla bilgi edinmek için Microsoft [Akışı'na başlarken](https://docs.microsoft.com/flow/getting-started)bkz.

Bu adımın ön [koşulu, Microsoft Flow](https://flow.microsoft.com) hesabına kaydolmak ve oturum açmaktır. Çözüme girdikten sonra, yeni bir **akış**ayarlamak için aşağıdaki adımları izleyin:

- Access "**Akışlarım**" menü öğesi
- Akışlarım içinde, sayfanın üst kısmındaki "**+Boştan Oluştur**" bağlantısını seçin
- Sayfanın altındaki "**Yüzlerce bağlayıcı ve tetikleyici ara**" bağlantısına tıklayın
- Arama alanı türünde "**yineleme**" ve çalışacak e-posta teslim işini planlamak için arama sonuçlarından "**Zamanlama - Yineleme**" seçeneğini belirleyin.
- Sıklık alanındaki Yineleme bölmesinde, her Dakika, Saat, Gün, Hafta vb. otomatik e-posta gönderme gibi bu akışın yürütülmesi için zamanlama sıklığını seçin.

Bir sonraki adım, yeni oluşturulan yinelenen akışa üç iş eklemektir (oluşturmak, çıktı almak ve e-posta göndermek). Akışa gerekli işleri eklemeyi başarmak için aşağıdaki adımları izleyin:

1. Atoklama önerilerini almak için PowerShell komut dosyasını çalıştırmak için eylem oluşturma

   - "**+Yeni adım**", ardından " Yineleme akış bölmesinin içinde**eylem ekle**" seçeneğini belirleyin
   - Arama alanı türünde "**otomasyon**" ve arama sonuçlarından "**Azure Otomasyonu – İş Oluşturma**" seçeneğini belirleyin
   - İş Oluştur bölmesinde, iş özelliklerini yapılandırın. Bu yapılandırma için, Daha önce **Otomasyon Hesabı bölmesine** **kaydedilmiş** Olan Azure abonelik kimliğinizin, Kaynak Grubunuzun ve Otomasyon Hesabınızın ayrıntılarına ihtiyacınız olacaktır. Bu bölümde bulunan seçenekler hakkında daha fazla bilgi edinmek için Azure [Otomasyonu - İş Oluştur](https://docs.microsoft.com/connectors/azureautomation/#create-job)bölümüne bakın.
   - "**Akışı kaydet**" düğmesine tıklayarak bu eylemi oluşturmayı tamamlama

2. Çalıştırılan PowerShell komut dosyasından çıktı almak için eylem oluşturma

   - "**+Yeni adım**", ardından " Yineleme akış bölmesinin içinde**eylem ekle**" seçeneğini belirleyin
   - Arama dosyasında "**otomasyon**" yazın ve arama sonuçlarından "**Azure Otomasyonu – İş çıktısı alın**" seçeneğini belirleyin. Bu bölümde bulunan seçenekler hakkında daha fazla bilgi edinmek için Azure [Otomasyonu – İş çıktısını alın.](https://docs.microsoft.com/connectors/azureautomation/#get-job-output)
   - Gereken alanları doldurma (önceki işi oluşturmaya benzer) - Azure abonelik kimliğinizi, Kaynak Grubunuzu ve Otomasyon Hesabınızı doldurma (Otomasyon Hesabı bölmesinde girilen şekilde)
   - "**Dinamik içerik**" menüsünün gösterilmesi için "**İş Kimliği**" alanının içine tıklayın. Bu menü içinden "**İş Kimliği**" seçeneğini belirleyin.
   - "**Akışı kaydet**" düğmesine tıklayarak bu eylemi oluşturmayı tamamlama

3. Office 365 tümleştirmesini kullanarak e-posta göndermek için eylem oluşturma

   - "**+Yeni adım**", ardından " Yineleme akış bölmesinin içinde**eylem ekle**" seçeneğini belirleyin
   - Arama dosyası türünde "**e-posta gönderin**" ve arama sonuçlarından "**Office 365 Outlook – E-posta gönder**" seçeneğini belirleyin
   - Bildirim e-postasını göndermeniz gereken e-posta adresindeki "**To**" alan türünde
   - E-postanızın konusundaki "**Konu**" alan türünde, örneğin "Otomatik ayar önerileri e-posta bildirimi"
   - "**Dinamik içerik**" menüsünün gösterilmesi için "**Gövde**" alanının içine tıklayın. Bu menü içinden , "**İş çıktısı al**", seçin "**İçerik**"
   - "**Akışı kaydet**" düğmesine tıklayarak bu eylemi oluşturmayı tamamlama

> [!TIP]
> Farklı alıcılara otomatik e-postalar göndermek için ayrı akışlar oluşturun. Bu ek akışlarda, "To" alanında alıcı e-posta adresini ve "Konu" alanındaki e-posta konu satırını değiştirin. Azure Otomasyonu'nda özelleştirilmiş PowerShell komut dosyalarıyla (Azure abonelik kimliğinin değiştirilmesi gibi) yeni runbook'lar oluşturmak, otomatik senaryoların daha da özelleştirilmesini sağlar, örneğin ayrı abonelikler için Otomatik tuning önerileri için ayrı alıcılara e-posta gönderebilirsiniz.
>

Yukarıdaki e-posta teslim iş akışını yapılandırmak için gerekli adımları sonuçlandırır. Yapılan üç eylemden oluşan tüm akış aşağıdaki resimde gösterilmiştir.

![Otomatik ayar e-posta bildirimlerini görüntüleme](./media/sql-database-automatic-tuning-email-notifications/howto-email-05.png)

Akışı test etmek için, akış bölmesinin sağ üst köşesindeki "**Şimdi Koş**" düğmesine tıklayın.

Gönderilen e-posta bildirimlerinin başarısını gösteren otomatik işleri çalıştırma istatistikleri Akış analizi bölmesinden görülebilir.

![Otomatik ayar e-posta bildirimleri için akış çalıştırma](./media/sql-database-automatic-tuning-email-notifications/howto-email-06.png)

Akış analizi, iş yürütmelerinin başarısını izlemek ve sorun giderme için gerekirse yardımcı olur.  Sorun giderme durumunda, Azure Otomasyonu uygulaması üzerinden erişilebilen PowerShell komut dosyası yürütme günlüğünü de incelemek isteyebilirsiniz.

Otomatik e-postanın son çıktısı, bu çözümü oluşturma ve çalıştırdıktan sonra alınan aşağıdaki e-postaya benzer:

![Otomatik ayar e-posta bildirimlerinden örnek e-posta çıkışı](./media/sql-database-automatic-tuning-email-notifications/howto-email-07.png)

PowerShell komut dosyasını ayarlayarak, otomatik e-postanın çıktısını ve biçimlendirmesini gereksinimlerinize göre ayarlayabilirsiniz.

Özel senaryolarınıza bağlı olarak, belirli bir tuning olayına ve birden çok alıcıya e-posta bildirimleri oluşturmak için çözümü daha da özelleştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Otomatik ayar oluşturmanın veritabanı performansını geliştirmenize nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin, [Azure SQL Veritabanı'nda Otomatik ayar'a](sql-database-automatic-tuning.md)bakın.
- İş yükünüzü yönetmek için Azure SQL Veritabanı'nda otomatik [ayar](sql-database-automatic-tuning-enable.md)yapmayı etkinleştirmek için bkz.
- Otomatik ayar önerilerini el ile gözden geçirmek [ve](sql-database-advisor-portal.md)uygulamak için bkz.
