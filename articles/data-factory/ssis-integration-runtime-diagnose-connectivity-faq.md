---
title: SSIS tümleştirme çalışma zamanı 'nda bağlantı tanılama sorunlarını giderme
description: Bu makale, SSIS tümleştirme çalışma zamanı 'nda bağlantı tanılama için sorun giderme kılavuzu sağlar
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: meiyl
author: meiyl
ms.reviewer: sawinark
manager: yidetu
ms.date: 06/07/2020
ms.openlocfilehash: 8e520048a6067f134e847953f4f4aa0598d9926e
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86172752"
---
# <a name="troubleshoot-diagnose-connectivity-in-the-ssis-integration-runtime"></a>SSIS tümleştirme çalışma zamanı 'nda bağlantı tanılama sorunlarını giderme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

SSIS tümleştirme çalışma zamanı ' nda SSIS paketlerini yürütürken bağlantı sorunlarıyla karşılaşırsanız, özellikle SSIS tümleştirme çalışma zamanı Azure sanal ağına katılırsa. Azure Data Factory portalının izleme SSIS tümleştirme çalışma zamanı sayfasında bu bağlantıyı Tanıla özelliğini kullanarak sorunları kendi kendine tanılamayı deneyebilirsiniz. 

 ![İzleme sayfası-bağlantıyı Tanıla bağlantı ](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-diagnose-connectivity.png) ![ izleyicisi sayfası-test bağlantısı](media/ssis-integration-runtime-diagnose-connectivity-faq/ssis-monitor-test-connection.png)
 
Bu makale, SSIS tümleştirme çalışma zamanı 'nda bağlantıyı test ederken bulabileceğiniz en yaygın hataları içerir. Hataları çözmeye yönelik olası nedenleri ve eylemleri açıklar. 

## <a name="common-errors-potential-causes-and-recommendation-solutions"></a>Yaygın hatalar, olası nedenler ve öneri çözümleri

### <a name="error-code-invalidinput"></a>Hata kodu: ınvalidınput.
* **Hata iletisi**: lütfen girişin doğru olduğundan emin olun.
* **Olası nedenler:** Giriş bilgileriniz yanlış.
* **Öneri:** Lütfen girişinizi denetleyin.

### <a name="error-code-firewallornetworkissue"></a>Hata kodu: Firewallornetworksorun.
* **Hata iletisi**: lütfen bu bağlantı noktasının güvenlik duvarınızın/sunucunuzun/NSG 'de açık olduğunu ve ağın kararlı olduğunu doğrulayın.
* **Olası nedenler:** 
  * Sunucunuz bu bağlantı noktasını açmıyor.
  * Ağ güvenlik grubunuz Bu bağlantı noktasındaki giden trafiği reddetti
  * NVA/Azure Güvenlik duvarınız/şirket içi güvenlik duvarınız bu bağlantı noktasını açmıyor.
* **Önerilen** 
  * Bu bağlantı noktasını sunucuda açın.
  * Bu bağlantı noktasında giden trafiğe izin vermek için ağ güvenlik grubunu güncelleştirin.
  * Bu bağlantı noktasını NVA/Azure Güvenlik Duvarı/şirket içi güvenlik duvarında açın.

### <a name="error-code-misconfigureddnssettings"></a>Hata kodu: Hatalı Configureddnssettings.
* **Hata iletisi**: Azure-SSIS IR Ile Birleşik VNET 'TE kendi DNS sunucunuzu kullanıyorsanız, lütfen ana bilgisayar adınızı çözümleyebildiğini doğrulayın.
* **Olası nedenler:** 
  *  Özel DNS sorunu
  *  Özel ana bilgisayar adınız için tam etki alanı adı (FQDN) kullanmıyor
* **Önerilen** 
  *  Ana bilgisayar adını çözümleyediğinden emin olmak için özel DNS sorununuzu çözün.
  *  Tam etki alanı adını (FQDN) kullanın, örneğin, your_private_server kendi DNS son ekini otomatik olarak eklemeyeceğinden, <> Azure-SSIS IR yerine <your_private_server>. contoso.com kullanın.

### <a name="error-code-servernotallowremoteconenction"></a>Hata kodu: ServerNotAllowRemoteConenction.
* **Hata iletisi**: lütfen sunucunuzun Bu bağlantı noktası ÜZERINDEN uzak TCP bağlantılarına izin verdiğini doğrulayın.
* **Olası nedenler:** 
  *  Sunucu güvenlik duvarınız uzak TCP bağlantılarına izin vermiyor.
  *  Sunucunuz çevrimiçi değil.
* **Önerilen** 
  *  Sunucu güvenlik duvarında uzak TCP bağlantılarına izin verin.
  *  Sunucuyu başlatın.
   
### <a name="error-code-misconfigurednsgsettings"></a>Hata kodu: Hatalı Configurednsgsettings.
* **Hata iletisi**: Lütfen sanal ağınızın NSG ' sini bu bağlantı noktası üzerinden giden trafiğe izin verdiğinden emin olun. Azure ExpressRoute ve veya UDR kullanıyorsanız, lütfen bu bağlantı noktasının güvenlik duvarınızdaki/sunucunuzdaki açık olduğunu doğrulayın.
* **Olası nedenler:** 
  *  Ağ güvenlik grubunuzun Bu bağlantı noktasındaki giden trafiği reddedildi.
  *  NVA/Azure Güvenlik duvarınız/şirket içi güvenlik duvarınız bu bağlantı noktasını açmıyor.
* **Önerilen** 
  *  Bu bağlantı noktasında giden trafiğe izin vermek için ağ güvenlik grubunu güncelleştirin.
  *  Bu bağlantı noktasını NVA/Azure Güvenlik Duvarı/şirket içi güvenlik duvarında açın.

### <a name="error-code-genericissues"></a>Hata kodu: Genericsorunlar.
* **Hata iletisi**: genel sorunlar nedeniyle test bağlantısı başarısız oldu.
* **Olası nedenler:** Sınama bağlantısı genel geçici bir sorunla karşılaştı.
* **Öneri:** Lütfen test bağlantısını daha sonra yeniden deneyin. Yeniden deneme işlemi yardım etmez, lütfen Azure Data Factory destek ekibine başvurun.


### <a name="error-code-pspingexecutiontimeout"></a>Hata kodu: PSPingExecutionTimeout.
* **Hata iletisi**: test bağlantısı zaman aşımı, lütfen daha sonra yeniden deneyin.
* **Olası nedenler:** Sınama bağlantısı zaman aşımına uğradı.
* **Öneri:** Lütfen test bağlantısını daha sonra yeniden deneyin. Yeniden deneme işlemi yardım etmez, lütfen Azure Data Factory destek ekibine başvurun.

### <a name="error-code-networkinstable"></a>Hata kodu: Networkınstable.
* **Hata iletisi**: ağ kararsızlığına bağlı olarak sınama bağlantısı, düzenli olarak başarılı oldu.
* **Olası nedenler:** Geçici ağ sorunu.
* **Öneri:** Lütfen sunucu veya güvenlik duvarı ağının kararlı olup olmadığını kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

- Paketlerinizi dağıtın. Daha fazla bilgi için bkz. [SSMS Ile Azure 'a BIR SSIS projesi dağıtma](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms).
- Paketlerinizi çalıştırın. Daha fazla bilgi için bkz. [SSMS Ile Azure 'DA SSIS paketleri çalıştırma](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms).
- Paketlerinizi zamanlayın. Daha fazla bilgi için bkz. [Azure 'DA SSIS paketlerini zamanlama](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15).

