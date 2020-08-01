---
title: SSIS tümleştirme çalışma zamanı 'ndaki bağlantıyı Tanıla özelliğini kullanma
description: Tanılama bağlantısı özelliğini kullanarak SSIS tümleştirme çalışma zamanındaki bağlantı sorunlarını giderin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: meiyl
author: meiyl
ms.reviewer: sawinark
manager: yidetu
ms.date: 06/07/2020
ms.openlocfilehash: cf41da685036770144ebf7eb2befd0c3d126362d
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87446020"
---
# <a name="use-the-diagnose-connectivity-feature-in-the-ssis-integration-runtime"></a>SSIS tümleştirme çalışma zamanı 'ndaki bağlantıyı Tanıla özelliğini kullanma

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

SSIS tümleştirme çalışma zamanı 'nda SQL Server Integration Services (SSIS) paketlerini yürütürken bağlantı sorunları bulabilirsiniz. Bu sorunlar özellikle SSIS tümleştirme çalışma zamanı Azure sanal ağını katıldığında oluşur.

Bağlantıları sınamak için bağlantıyı *Tanıla* özelliğini kullanarak bağlantı sorunlarını giderin. Özelliği, Azure Data Factory portalının izleme SSIS tümleştirme çalışma zamanı sayfasında bulunur.

 ![İzleyici sayfası-bağlantıyı Tanıla](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-diagnose-connectivity.png)

 ![İzleme sayfası-test bağlantısı](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-test-connection.png)

Bağlantıları test ederken oluşan en yaygın hatalar hakkında bilgi edinmek için aşağıdaki bölümleri kullanın. Her bölümde şunları açıklar:

- Hata kodu
- Hata iletisi
- Hatanın olası nedeni
- Önerilen çözüm (ler)

## <a name="error-code-invalidinput"></a>Hata kodu: ınvalidınput

- **Hata iletisi**: "Lütfen girişin doğru olduğundan emin olun."
- **Olası neden**: giriş yanlış.
- **Öneri**: Girişinizi denetleyin.

## <a name="error-code-firewallornetworkissue"></a>Hata kodu: Firewallornetworkıssue

- **Hata iletisi**: "Lütfen bu bağlantı noktasının güvenlik duvarınızın/sunucunuzun/NSG 'de açık olduğunu ve ağın kararlı olduğunu doğrulayın."
- **Olası nedenler:**
  - Sunucunuz bağlantı noktasını açmıyor.
  - Ağ güvenlik grubunuzun bağlantı noktasındaki giden trafik reddedildi.
  - NVA/Azure Güvenlik duvarınız/şirket içi güvenlik duvarınız bağlantı noktasını açmıyor.
- **Öneri**
  - Bağlantı noktasını sunucuda açın.
  - Bağlantı noktasında giden trafiğe izin vermek için ağ güvenlik grubunu güncelleştirin.
  - NVA/Azure Güvenlik Duvarı/şirket içi güvenlik duvarında bağlantı noktasını açın.

## <a name="error-code-misconfigureddnssettings"></a>Hata kodu: Hatalı Configureddnssettings

- **Hata iletisi**: "Azure-SSIS IR Ile Birleşik VNET 'TE kendi DNS sunucunuzu kullanıyorsanız, ana bilgisayar adınızı çözümleyebildiğini doğrulayın."
- **Olası nedenler:**
  -  Özel DNS 'niz ile ilgili bir sorun var.
  -  Özel ana bilgisayar adınız için tam etki alanı adı (FQDN) kullanmamış olabilirsiniz.
- **Öneri**
  -  Ana bilgisayar adını çözümleyediğinden emin olmak için özel DNS sorununuzu çözün.
  -  FQDN'yi kullanın. Azure-SSIS IR kendi DNS son ekini otomatik olarak eklememez. Örneğin, **<your_private_server>** yerine **<your_private_server>. contoso.com** kullanın.

## <a name="error-code-servernotallowremoteconnection"></a>Hata kodu: ServerNotAllowRemoteConnection

- **Hata iletisi**: "Lütfen sunucunuzun Bu bağlantı noktası ÜZERINDEN uzak TCP bağlantılarına izin verdiğini doğrulayın."
- **Olası nedenler:**
  -  Sunucu güvenlik duvarınız uzak TCP bağlantılarına izin vermiyor.
  -  Sunucunuz çevrimiçi değil.
- **Öneri**
  -  Sunucu güvenlik duvarında uzak TCP bağlantılarına izin verin.
  -  Sunucuyu başlatın.
   
## <a name="error-code-misconfigurednsgsettings"></a>Hata kodu: Hatalı Configurednsgsettings

- **Hata iletisi**: "Lütfen sanal ağınızın NSG ' sini bu bağlantı noktası üzerinden giden trafiğe izin verdiğinden emin olun. Azure ExpressRoute ve veya UDR kullanıyorsanız, lütfen bu bağlantı noktasının güvenlik duvarınızın/sunucunuzun üzerinde açık olduğunu doğrulayın. "
- **Olası nedenler:**
  -  Ağ güvenlik grubunuzun bağlantı noktasındaki giden trafik reddedildi.
  -  NVA/Azure Güvenlik duvarınız/şirket içi güvenlik duvarınız bağlantı noktasını açmıyor.
- **Önerilen**
  -  Bağlantı noktasında giden trafiğe izin vermek için ağ güvenlik grubunu güncelleştirin.
  -  NVA/Azure Güvenlik Duvarı/şirket içi güvenlik duvarında bağlantı noktasını açın.

## <a name="error-code-genericissues"></a>Hata kodu: Genericsorunlar

- **Hata iletisi**: "test bağlantısı genel sorunlar nedeniyle başarısız oldu."
- **Olası neden**: test bağlantısı genel geçici bir sorunla karşılaştı.
- **Öneri**: test bağlantısını daha sonra yeniden deneyin. Yeniden deneme işlemi yardım vermezse, Azure Data Factory destek ekibine başvurun.

## <a name="error-code-pspingexecutiontimeout"></a>Hata kodu: PSPingExecutionTimeout

- **Hata iletisi**: "test bağlantısı zaman aşımı, lütfen daha sonra yeniden deneyin."
- **Olası neden**: test bağlantısı zaman aşımına uğradı.
- **Öneri**: test bağlantısını daha sonra yeniden deneyin. Yeniden deneme işlemi yardım vermezse, Azure Data Factory destek ekibine başvurun.

## <a name="error-code-networkinstable"></a>Hata kodu: Networkınstable

- **Hata iletisi**: "test bağlantısı, ağ kararsızlık nedeniyle düzenli olarak başarılı oldu."
- **Olası neden**: geçici ağ sorunu.
- **Öneri**: sunucu veya güvenlik duvarı ağının kararlı olup olmadığını denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [SSMS ile Azure 'a bir SSIS projesi dağıtma](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms)
- [SSMS ile Azure 'da SSIS paketlerini çalıştırma](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms)
- [Azure 'da SSIS paketlerini zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15)
