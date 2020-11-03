---
title: Sertifika içeriğini temele dönüştürme-64-Azure HDInsight
description: Azure HDInsight 'ta hizmet sorumlusu sertifika içeriğini Base-64 kodlu dize biçimine dönüştürme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 88faf878d9528b314e59c81dad7a97d4700b608f
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289135"
---
# <a name="converting-service-principal-certificate-contents-to-base-64-encoded-string-format-in-hdinsight"></a>HDInsight 'ta hizmet sorumlusu sertifika içeriğini Base-64 kodlu dize biçimine dönüştürme

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Temel olmayan bir 64 karakteri, ikiden fazla doldurma karakteri veya doldurma karakterleri arasında boşluk olmayan bir karakter içerdiğinden girişin geçerli bir Base-64 dizesi olduğunu belirten bir hata iletisi alırsınız.

## <a name="cause"></a>Nedeni

Birincil veya ek depolama alanı olarak Data Lake sahip kümeler oluşturmak için PowerShell veya Azure şablon dağıtımı kullanılırken, Data Lake depolama hesabına erişmek için belirtilen hizmet sorumlusu sertifika içerikleri Base-64 biçimindedir. PFX Sertifika içeriklerinin Base-64 kodlu dizeye hatalı dönüştürülmesi bu hataya neden olabilir.

## <a name="resolution"></a>Çözüm

Pfx biçiminde hizmet sorumlusu sertifikası aldıktan sonra (örnek hizmet sorumlusu oluşturma adımları için [buraya](https://github.com/Azure/azure-quickstart-templates/tree/master/201-hdinsight-datalake-store-azure-storage) bakın), sertifika içeriğini base-64 biçimine dönüştürmek Için aşağıdaki PowerShell komutunu veya C# kod parçacığını kullanın.

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

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]