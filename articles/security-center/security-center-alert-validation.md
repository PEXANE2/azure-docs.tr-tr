---
title: Azure Güvenlik Merkezi'nde uyarı doğrulama (EICAR test dosyası) | Microsoft Dokümanlar
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
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 5146878adf10e452f38fecb115ec40792ffa84f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79140006"
---
# <a name="alert-validation-eicar-test-file-in-azure-security-center"></a>Azure Güvenlik Merkezi’nde uyarı doğrulaması (EICAR test dosyası)
Bu belge, sisteminizin Azure Güvenlik Merkezi uyarıları için doğru yapılandırılıp yapılandırılmadığını doğrulamayı öğrenmenize yardımcı olur.

## <a name="what-are-security-alerts"></a>Güvenlik uyarıları nedir?
Uyarılar, Güvenlik Merkezi’nin kaynaklarınızda tehditler algıladığında oluşturduğu bildirimlerdir. Sorunu hızlı bir şekilde araştırmak için gereken bilgilerle birlikte uyarıları n öncelir ve listeler. Güvenlik Merkezi ayrıca bir saldırıyı nasıl düzeltebileceğiniz konusunda öneriler sağlar.
Daha fazla bilgi için Güvenlik Merkezi ve [Yönetimi'ndeki Güvenlik uyarılarına](security-center-alerts-overview.md) bakın [ve güvenlik uyarılarına yanıt verin](security-center-managing-and-responding-alerts.md)

## <a name="alert-validation"></a>Uyarı doğrulaması

* [Windows](#validate-windows)
* [Linux](#validate-linux)
* [Kubernetes](#validate-kubernetes)

## <a name="validate-alerts-on-windows-vms"></a>Windows VM'lerde uyarıları doğrulama<a name="validate-windows"></a>

Güvenlik Merkezi aracısı bilgisayarınıza yüklendikten sonra, uyarının saldırıya uğrayan kaynağı olmak istediğiniz bilgisayardan aşağıdaki adımları izleyin:

1. Çalıştırılabilir bir çalıştırılabilir (örneğin **calc.exe)** bilgisayarın masaüstüne veya rahatınızın diğer dizinine kopyalayın ve **ASC_AlertTest_662jfi039N.exe**olarak yeniden adlandırın.
1. Komut istemini açın ve bu dosyayı aşağıdakiler gibi bir bağımsız değişkenle (sadece sahte bir bağımsız değişken adı) çalıştırın:```ASC_AlertTest_662jfi039N.exe -foo```
1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Aşağıdaki [örneğe](#alert-validate) benzer bir uyarı görüntülenmelidir:

> [!NOTE]
> Windows için bu test uyarısını gözden geçirirken, **Bağımsız Değişkenleri Denetleme Etkin alanının** **doğru**olduğundan emin olun. **Yanlışsa,** komut satırı bağımsız değişkenlerini denetlemeyi etkinleştirmeniz gerekir. Etkinleştirmek için aşağıdaki komutu kullanın:
>
>```reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"```

## <a name="validate-alerts-on-linux-vms"></a>Linux VM'lerinde uyarıları doğrulama<a name="validate-linux"></a>

Güvenlik Merkezi aracısı bilgisayarınıza yüklendikten sonra, uyarının saldırıya uğrayan kaynağı olmak istediğiniz bilgisayardan aşağıdaki adımları izleyin:
1. Yürütülebilir bir yeri uygun bir konuma kopyalayın ve **./asc_alerttest_662jfi039n**olarak yeniden adlandırın, örneğin:

    ```cp /bin/echo ./asc_alerttest_662jfi039n```

1. Komut istemini açın ve bu dosyayı çalıştırın:

    ```./asc_alerttest_662jfi039n testing eicar pipe```

1. 5-10 dakika bekleyin ve Güvenlik Merkezi Uyarılarını açın. Aşağıdaki [örneğe](#alert-validate) benzer bir uyarı görüntülenmelidir:

### <a name="alert-example"></a>Uyarı örneği<a name="alert-validate"></a>

![Uyarı doğrulama örneği](./media/security-center-alert-validation/security-center-alert-validation-fig2.png) 


## <a name="validate-alerts-on-kubernetes"></a>Kubernetes'teki uyarıları doğrula<a name="validate-kubernetes"></a>

Azure Kubernetes Hizmetini tümleştirmenin Güvenlik Merkezi önizleme özelliğini kullanıyorsanız, uyarılarınızın çalıştığını test etmek için aşağıdaki kubectl komutunu çalıştırın:

```kubectl get pods --namespace=asc-alerttest-662jfi039n```

Azure Kubernetes Hizmeti ve Azure Güvenlik Merkezi'nin tümleştirilmesi hakkında daha fazla bilgi için [bu makaleye](azure-kubernetes-service-integration.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede uyarıları doğrulama işlemine giriş yaptınız. Artık bu doğrulama hakkında bilgi sahibi olduğunuza göre, aşağıdaki makaleleri deneyebilirsiniz:

* [Azure Güvenlik Merkezi'nde Azure Anahtar Kasası Tehdit Algılama'yı doğrulama](https://techcommunity.microsoft.com/t5/azure-security-center/validating-azure-key-vault-threat-detection-in-azure-security/ba-p/1220336)
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtverme](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) - Güvenlik Merkezi'ndeki uyarıları nasıl yöneteceklerini ve güvenlik olaylarını nasıl yanıtlayın öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik durumu izleme](security-center-monitoring.md) - Azure kaynaklarınızın sistem durumunu nasıl izleyeceğinizi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını anlama](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) - Farklı güvenlik uyarıları türleri hakkında bilgi edinin.
