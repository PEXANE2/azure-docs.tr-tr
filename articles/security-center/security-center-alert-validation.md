---
title: Azure Güvenlik Merkezi 'nde uyarı doğrulaması (EICAR test dosyası) | Microsoft Docs
description: Bu belge, Azure Güvenlik Merkezi'nde güvenlik uyarılarını doğrulamanıza yardımcı olur.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/02/2019
ms.author: memildin
ms.openlocfilehash: 32f67fb94b207735e77583a6db62f7c8703dd991
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71202729"
---
# <a name="alert-validation-eicar-test-file-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde uyarı doğrulaması (EICAR test dosyası)
Bu belge, sisteminizin Azure Güvenlik Merkezi uyarıları için doğru yapılandırılıp yapılandırılmadığını doğrulamayı öğrenmenize yardımcı olur.

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Uyarılar, güvenlik merkezi 'nin kaynaklarınızın tehditleri algıladığında oluşturduğu bildirimlerdir. Sorunu hızlı bir şekilde araştırmak için gereken bilgileri ve uyarıları önceliklendirir ve listeler. Güvenlik Merkezi ayrıca bir saldırıyı nasıl düzeltebileceğiniz konusunda öneriler de sağlar.
Daha fazla bilgi için bkz. Azure Güvenlik [Merkezi 'Nde güvenlik uyarıları](security-center-alerts-overview.md) ve [Azure Güvenlik Merkezi 'Nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md)

## <a name="alert-validation"></a>Uyarı doğrulaması

* [Windows](#validate-windows)
* [Linux](#validate-linux)

## Windows VM 'de uyarı doğrulama<a name="validate-windows"></a>

Güvenlik Merkezi Aracısı bilgisayarınıza yüklendikten sonra, uyarının saldırıya uğrayan kaynak olmasını istediğiniz bilgisayarda bu adımları izleyin:

1. Bir yürütülebilir dosyayı (örneğin, **Calc. exe**) bilgisayarın masaüstüne veya sizin rahatınızın diğer dizinine kopyalayın ve **ASC_AlertTest_662jfi039N. exe**olarak yeniden adlandırın.
1. Komut istemi ' ni açın ve bu dosyayı aşağıdaki gibi bir bağımsız değişkenle (yalnızca sahte bir bağımsız değişken adı) yürütün:```ASC_AlertTest_662jfi039N.exe -foo```
1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Aşağıdaki [örneğe](#alert-validate) benzer bir uyarı görüntülenmelidir:

> [!NOTE]
> Windows için bu test uyarısını gözden geçirirken, alan **bağımsız değişkenlerinin denetim etkin** **olduğundan emin olun.** **Yanlış**ise, komut satırı bağımsız değişken denetimini etkinleştirmeniz gerekir. Etkinleştirmek için aşağıdaki komut satırını kullanın:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## Linux VM 'de uyarı doğrulama<a name="validate-linux"></a>

Güvenlik Merkezi Aracısı bilgisayarınıza yüklendikten sonra, uyarının saldırıya uğrayan kaynak olmasını istediğiniz bilgisayarda bu adımları izleyin:
1. Bir yürütülebilir dosyayı uygun bir konuma kopyalayın ve **./asc_alerttest_662jfi039n**olarak yeniden adlandırın, örneğin:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Komut istemi 'ni açın ve bu dosyayı yürütün:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Aşağıdaki [örneğe](#alert-validate) benzer bir uyarı görüntülenmelidir:

### Uyarı örneği<a name="alert-validate"></a>

![Uyarı doğrulama örneği](./media/security-center-alert-validation/security-center-alert-validation-fig2.png) 

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede uyarıları doğrulama işlemine giriş yaptınız. Artık bu doğrulama hakkında bilgi sahibi olduğunuza göre, aşağıdaki makaleleri deneyebilirsiniz:

* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve ele alma](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Güvenlik Merkezi’nde uyarıları yönetme ve güvenlik olaylarına yanıt vermeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumunu izleme](security-center-monitoring.md). Azure kaynaklarınızı durumunu izleme hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Farklı güvenlik uyarısı türleri hakkında bilgi edinin.
* [Azure Güvenlik Merkezi Sorun Giderme Kılavuzu](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Güvenlik Merkezi’nde sık karşılaşılan sorunları gidermeyi öğrenin.
* [Azure Güvenlik Merkezi SSS](security-center-faq.md). Hizmet kullanımı ile ilgili sık sorulan soruları bulun.
* [Azure Güvenlik Blogu](https://blogs.msdn.com/b/azuresecurity/). Azure güvenliği ve uyumluluğu ile ilgili blog yazılarını bulun.
