---
title: Sertifika içeriğini base-64'e dönüştürme - Azure HDInsight
description: Azure HDInsight'ta hizmet temel sertifika içeriğini temel-64 kodlanmış dize biçimine dönüştürme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: d6119e4f8c651ba482a24f46b44ff15f870858ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894181"
---
# <a name="converting-service-principal-certificate-contents-to-base-64-encoded-string-format-in-hdinsight"></a>HDInsight'ta hizmet temel sertifika içeriğini base-64 kodlanmış dize biçimine dönüştürme

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Girişin geçerli bir Base-64 dizesi olmadığını belirten bir hata iletisi alırsınız, çünkü temel olmayan 64 karakter, ikiden fazla dolgu karakteri veya dolgu karakterleri arasında beyaz olmayan bir boşluk karakteri içerir.

## <a name="cause"></a>Nedeni

Birincil veya ek depolama alanı olarak Data Lake olan kümeler oluşturmak için PowerShell veya Azure şablon dağıtımı kullanılırken, Veri Gölü depolama hesabına erişmek için sağlanan hizmet temel sertifika içeriği taban-64 biçimindedir. PFX sertifika içeriğinin base-64 kodlanmış dizesine yanlış dönüştürülmesi bu hataya neden olabilir.

## <a name="resolution"></a>Çözüm

Pfx formatında hizmet temel sertifikasını aldıktan sonra (örnek hizmet temel oluşturma adımları için [buraya](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage) bakın), sertifika içeriğini base-64 biçimine dönüştürmek için aşağıdaki PowerShell komutunu veya C# snippet'i kullanın.

```powershell
$servicePrincipalCertificateBase64 = [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(path-to-servicePrincipalCertificatePfxFile))
```

```csharp
using System;
using System.IO;

namespace ConsoleApplication
{
    class Program
    {
        static void Main(string[] args)
        {
            var certContents = File.ReadAllBytes(@"<path to pfx file>");
            string certificateData = Convert.ToBase64String(certContents);
            System.Diagnostics.Debug.WriteLine(certificateData);
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
