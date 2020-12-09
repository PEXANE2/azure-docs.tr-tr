---
title: Sorun giderme-Azure Izleyici Application Insights Java
description: Azure Izleyici Application Insights Java 'da sorun giderme
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-java
ms.openlocfilehash: cf27763f857cc1fd1aad5256d0c6cecf91251caf
ms.sourcegitcommit: 48cb2b7d4022a85175309cf3573e72c4e67288f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2020
ms.locfileid: "96855657"
---
# <a name="troubleshooting-azure-monitor-application-insights-java"></a>Azure Izleyici Application Insights Java 'da sorun giderme

Bu makalede, bir kullanıcının Java aracısını kullanarak bu sorunları çözme adımlarıyla birlikte Java uygulamasını düzenleme sırasında karşılaştığı yaygın sorunlardan bazılarını ele aldık.

## <a name="self-diagnostic-log-file"></a>Kendi kendine tanılama günlük dosyası

Varsayılan olarak, Application Insights Java 3,0, dosyanın bulunduğu dizinde adlı bir günlük dosyası oluşturacaktır `applicationinsights.log` `applicationinsights-agent-3.0.0.jar` .

Bu günlük dosyası, karşılaşabileceğiniz sorunların ipuçlarını denetlemek için ilk yerdir.

## <a name="upgrade-from-application-insights-java-2x-sdk"></a>Java 2. x SDK Application Insights yükseltme

Bkz. [2. x SDK 'Dan yükseltme](./java-standalone-upgrade-from-2x.md).

## <a name="upgrade-from-30-preview"></a>3,0 Preview sürümünden yükseltme

3,0 önizlemesinden yükseltiyorsanız, JSON yapısı 3,0 GA sürümünde tamamen değiştiği için lütfen tüm [yapılandırma seçeneklerini](./java-standalone-config.md) dikkatle gözden geçirin.

Bu değişiklikler şunları içerir:

1.  Yapılandırma dosyası adının kendisi `ApplicationInsights.json` olarak değiştirildi `applicationinsights.json` .
2.  `instrumentationSettings`Düğüm artık yok. İçindeki tüm içerik `instrumentationSettings` kök düzeye taşınır. 
3.  `sampling`, Ve gibi yapılandırma `jmxMetrics` düğümleri `instrumentation` `heartbeat` `preview` kök düzeye taşınır.

## <a name="ssl-certificate-issues"></a>SSL sertifikası sorunları

Varsayılan Java keystore kullanıyorsanız, tüm CA kök sertifikalarına zaten sahip olur ve daha fazla SSL sertifikası almanız gerekmez.

Özel bir Java keystore kullanıyorsanız, Application Insights uç nokta SSL sertifikalarını buna aktarmanız gerekebilir.

### <a name="some-key-terminology"></a>Bazı temel terminoloji:
*Keystore* , sertifikalar, ortak ve özel anahtarların bir deposudur. Genellikle JDK dağıtımlarını yönetmek için çalıştırılabilir dosya vardır `keytool` .

Aşağıdaki örnek, bir SSL sertifikasını keystore 'a aktarmaya yönelik basit bir komuttur:

`keytool -importcert -alias your_ssl_certificate -file "your downloaded SSL certificate name".cer -keystore "Your KeyStore name" -storepass "Your keystore password" -noprompt`

### <a name="steps-to-download-and-add-the-ssl-certificate"></a>SSL sertifikasını indirme ve ekleme adımları:

1.  En sevdiğiniz tarayıcıyı açın ve `IngestionEndpoint` aşağıda gösterildiği gibi uygulamanızı işaretlemek için kullanılan bağlantı dizesinde bulunan URL 'ye gidin

    :::image type="content" source="media/java-ipa/troubleshooting/ingestion-endpoint-url.png" alt-text="Application Insights bağlantı dizesi":::

2.  Tarayıcıdaki ' site bilgilerini görüntüle ' (kilit) simgesine tıklayın ve aşağıda göster ' sertifika ' seçeneğine tıklayın

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-icon-capture.png" alt-text="SSL sertifikası yakalama":::

3.  Ayrıntılar sekmesine gidin ve dosyaya Kopyala ' ya tıklayın.
4.  İleri düğmesine tıklayın ve "Base-64 Encoded X. 509.440 (. CER) "biçimini seçin ve ileri ' yi seçin.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-export-wizard.png" alt-text="SSL sertifikası dışarı ExportWizard":::

5.  SSL sertifikasını kaydetmek istediğiniz dosyayı belirtin. Son olarak ileri ve son ' a tıklayın. "Dışarı aktarma başarılı oldu" iletisini görmeniz gerekir.
6.  Sertifikayı bir Java keystore 'a aktarma süresi bittikten sonra. Sertifikaları içeri aktarmak için yukarıdaki [komutu](#some-key-terminology) kullanın.

> [!WARNING]
> Geçerli sertifikanın süresi dolmadan önce yeni sertifikayı almak için bu adımları yinelemeniz gerekecektir. Süre sonu bilgilerini aşağıda gösterildiği gibi sertifika açılan penceresinde "Ayrıntılar" sekmesinde bulabilirsiniz

:::image type="content" source="media/java-ipa/troubleshooting/certificate-details.png" alt-text="SSL sertifikası ayrıntıları":::
