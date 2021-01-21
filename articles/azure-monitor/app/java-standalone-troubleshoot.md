---
title: Java için Azure Izleyici Application Insights sorunlarını giderme
description: Azure Izleyici için Java aracısında sorun gidermeyi öğrenin Application Insights
ms.topic: conceptual
ms.date: 11/30/2020
ms.custom: devx-track-java
ms.openlocfilehash: 788eea17cabbea46578d0f59919ae95a59f2223f
ms.sourcegitcommit: a0c1d0d0906585f5fdb2aaabe6f202acf2e22cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98625356"
---
# <a name="troubleshooting-guide-azure-monitor-application-insights-for-java"></a>Sorun giderme kılavuzu: Java için Azure Izleyici Application Insights

Bu makalede, Application Insights için Java aracısını kullanarak bir Java uygulamasını düzenleme sırasında karşılaşabileceğiniz yaygın sorunlardan bazılarını ele aldık. Ayrıca, bu sorunları çözmek için gereken adımları ele alınmaktadır. Application Insights, Azure Izleyici Platform hizmeti 'nin bir özelliğidir.

## <a name="check-the-self-diagnostic-log-file"></a>Kendi kendine tanılama günlük dosyasını denetleme

Varsayılan olarak, Application Insights için Java 3,0 Aracısı, `applicationinsights.log` dosyayı tutan aynı dizinde adlı bir günlük dosyası üretir `applicationinsights-agent-3.0.2.jar` .

Bu günlük dosyası, karşılaşmanız gerekebilecek herhangi bir sorunun ipuçlarını denetlemek için ilk yerdir.

## <a name="jvm-fails-to-start"></a>JVM başlatılamıyor

JVM "ZIP dosyası açma veya JAR bildirimi eksik" hatası ile başlayamazsa, dosya aktarımı sırasında bozulmuş olabileceğinden, aracı jar dosyasını yeniden indirmeyi deneyin.

## <a name="upgrade-from-the-application-insights-java-2x-sdk"></a>Java 2. x SDK Application Insights yükseltme

Uygulamanızda Java 2. x SDK Application Insights zaten kullanıyorsanız, bunu kullanmaya devam edebilirsiniz. Java 3,0 Aracısı tarafından algılanır. Daha fazla bilgi için bkz. [Java 2. x SDK 'Dan yükseltme](./java-standalone-upgrade-from-2x.md).

## <a name="upgrade-from-application-insights-java-30-preview"></a>Application Insights Java 3,0 Preview sürümünden yükseltme

Java 3,0 Önizleme aracılarından yükseltiyorsanız, tüm [yapılandırma seçeneklerini](./java-standalone-config.md) dikkatle gözden geçirin. JSON yapısı, 3,0 genel kullanılabilirlik (GA) sürümünde tamamen değiştirilmiştir.

Bu değişiklikler şunları içerir:

-  Yapılandırma dosyası adı, `ApplicationInsights.json` olarak değiştirildi `applicationinsights.json` .
-  `instrumentationSettings`Düğüm artık yok. İçindeki tüm içerik `instrumentationSettings` kök düzeye taşınır. 
-  ,, Ve gibi yapılandırma düğümleri `sampling` `jmxMetrics` `instrumentation` `heartbeat` `preview` kök düzeye taşınır.

## <a name="some-logging-is-not-auto-collected"></a>Bazı günlük kaydı otomatik olarak toplanmadı

Günlüğe kaydetme işlemi, yalnızca ilk olarak günlüğe kaydetme çerçevelerinin yapılandırılmış eşiğini karşılıyorsa yakalanır ve ikincisi de Application Insights yapılandırılan eşiği karşılar.

Belirli bir günlüğe kaydetme bildiriminin günlüğe kaydetme çerçevelerinin yapılandırılmış eşiğini karşılayıp karşılamadığını öğrenmenin en iyi yolu, normal uygulama günlüğinizde (ör. dosya veya konsol) görüntülendiğini doğrulamadır.

Daha fazla ayrıntı için [Otomatik toplanan günlük yapılandırmasına](./java-standalone-config.md#auto-collected-logging) bakın.

## <a name="import-ssl-certificates"></a>SSL sertifikalarını içeri aktarma

Varsayılan Java keystore kullanıyorsanız, tüm CA kök sertifikalarına zaten sahip olur. Daha fazla SSL sertifikası içeri aktarmanız gerekmez.

Özel bir Java keystore kullanıyorsanız, Application Insights uç nokta SSL sertifikalarını buna aktarmanız gerekebilir.

### <a name="key-terminology"></a>Anahtar terminolojisi
*Anahtar deposu* , sertifikalar, ortak anahtarlar ve özel anahtarların bir deposudur. Genellikle, Java Development Kit dağıtımları bunları yönetmek için çalıştırılabilir dosya sahiptir: `keytool` .

Aşağıdaki örnek, bir SSL sertifikasını keystore 'a aktarmaya yönelik basit bir komuttur:

`keytool -importcert -alias your_ssl_certificate -file "your downloaded SSL certificate name".cer -keystore "Your KeyStore name" -storepass "Your keystore password" -noprompt`

### <a name="steps-to-download-and-add-an-ssl-certificate"></a>SSL sertifikası indirme ve ekleme adımları

1.  En sevdiğiniz tarayıcıyı açın ve `IngestionEndpoint` uygulamanızı işaretlemek için kullanılan bağlantı dizesinde bulunan URL 'ye gidin.

    :::image type="content" source="media/java-ipa/troubleshooting/ingestion-endpoint-url.png" alt-text="Bir Application Insights bağlantı dizesi gösteren ekran görüntüsü.":::

2.  Tarayıcıda **site bilgilerini görüntüle** (kilit) simgesini seçin ve ardından **sertifika** seçeneğini belirleyin.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-icon-capture.png" alt-text="Site bilgilerinde sertifika seçeneğinin ekran görüntüsü.":::

3.  **Ayrıntılar** sekmesine gidin ve **Dosyaya Kopyala**' yı seçin.
4.  **İleri** düğmesini seçin, **Base-64 Encoded X. 509.440 (. CER)** biçimini seçin ve ardından **İleri** ' yi seçin.

    :::image type="content" source="media/java-ipa/troubleshooting/certificate-export-wizard.png" alt-text="Sertifika Dışarı Aktarma Sihirbazı 'nın seçili bir biçim ile ekran görüntüsü.":::

5.  SSL sertifikasını kaydetmek istediğiniz dosyayı belirtin. Ardından **İleri**  >  **son**' u seçin. "Dışarı aktarma başarılı oldu" iletisini görmeniz gerekir.
6.  Sertifikayı aldıktan sonra, sertifikayı bir Java keystore 'a aktarmaya zaman atalım. Sertifikaları içeri aktarmak için [önceki komutu](#key-terminology) kullanın.

> [!WARNING]
> Geçerli sertifikanın süresi dolmadan önce yeni sertifikayı almak için bu adımları yinelemeniz gerekir. Süre sonu bilgilerini **sertifika** Iletişim kutusunun **Ayrıntılar** sekmesinde bulabilirsiniz.
>
> :::image type="content" source="media/java-ipa/troubleshooting/certificate-details.png" alt-text="SSL sertifikası ayrıntılarını gösteren ekran görüntüsü.":::
