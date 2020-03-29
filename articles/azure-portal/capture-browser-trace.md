---
title: Sorun giderme için tarayıcı izleme yakalama | Microsoft Dokümanlar
description: Azure portalındaki sorunları gidermeye yardımcı olmak için bir tarayıcı izlemesinden ağ bilgilerini yakalayın.
services: azure-portal
keywords: ''
author: mgblythe
ms.author: mblythe
ms.date: 01/09/2020
ms.topic: troubleshooting
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 2b506c9d15dafcd23b24207fe15ed0532939209f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76310705"
---
# <a name="capture-a-browser-trace-for-troubleshooting"></a>Sorun giderme için bir tarayıcı izlemesi yakalama

Azure portalıyla ilgili bir sorunu gidermeye devam ediyorsanız ve Microsoft desteğine başvurmanız gerekiyorsa, önce bir tarayıcı izleme ve bazı ek bilgiler yakalamanızı öneririz. Topladığınız bilgiler, sorunun oluştuğu anda portal hakkında önemli ayrıntılar sağlayabilir. Kullandığınız tarayıcıdaki geliştirici araçları için bu makaledeki adımları izleyin: Google Chrome veya Microsoft Edge (Krom), Microsoft Edge (EdgeHTML) veya Apple Safari.

## <a name="google-chrome-and-microsoft-edge-chromium"></a>Google Chrome ve Microsoft Edge (Krom)

Google Chrome ve Microsoft Edge (Krom) her ikisi de [Krom açık kaynak projesi](https://www.chromium.org/Home)dayanmaktadır. Aşağıdaki adımlar, iki tarayıcıda çok benzer olan geliştirici araçlarının nasıl kullanılacağını gösterir. Daha fazla bilgi için [Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools) ve [Microsoft Edge (Krom) Geliştirici Araçları'na](/microsoft-edge/devtools-guide-chromium)bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın. İzlemeye başlamadan _önce_ oturum açmanız önemlidir, böylece izleme oturum açmanızla ilgili hassas bilgiler içermez. 

1. [Steps Recorder'ı](https://support.microsoft.com/help/22878/windows-10-record-steps)kullanarak portalda attığınız adımları kaydetmeye başlayın.

1. Portalda, sorunun oluştuğu yerden hemen önce adıma gidin.

1. F12 tuşuna ![basın veya](media/capture-browser-trace/chromium-icon-settings.png) > tarayıcı ayarları simgesinin Ekran Görüntüsü'nü seçin**Diğer araçlar** > **Geliştirici araçları**.

1. Varsayılan olarak, tarayıcı yalnızca şu anda yüklenen sayfaiçin izleme bilgilerini tutar. Repro'nuz birden fazla sayfaya gitmeyi gerektirse bile, tarayıcının tüm izleme bilgilerini saklayacak şekilde aşağıdaki seçenekleri ayarlayın:

    1. **Ağ** sekmesini seçin, ardından **Günlüğü Koru'yu**seçin.

          !["Günlüğü koru" ekran görüntüsü](media/capture-browser-trace/chromium-network-preserve-log.png)

    1. **Konsol** sekmesini seçin, **Konsol ayarlarını**seçin, ardından **Günlüğü Koru'yu**seçin. Ayarlar bölmesini kapatmak için **Konsol ayarlarını** yeniden seçin.

          !["Günlüğü Koru" ekran görüntüsü](media/capture-browser-trace/chromium-console-preserve-log.png)

1. **Ağ** sekmesini seçin, ardından **ağ günlüğünü durdur'u** ve **Temizle'yi**seçin.

    !["Ağ kaydını durdur" ve "Temizle" ekran görüntüsü](media/capture-browser-trace/chromium-stop-clear-session.png)

1. **Ağ günlüğünü Kaydet'i**seçin ve ardından sorunu portalda yeniden çoğaltın.

    !["Profil oluşturma oturumubaşlat" ekran görüntüsü](media/capture-browser-trace/chromium-start-session.png)

    Aşağıdaki resme benzer oturum çıktısı görürsünüz.

    ![Tarayıcı izleme sonuçlarının ekran görüntüsü](media/capture-browser-trace/chromium-browser-trace-results.png)

1. Beklenmeyen portal davranışını yeniden çoğalttıktan sonra, **ağ günlüğünü Durdur'u**seçin, ardından **HAR'ı Dışa Aktar'ı** seçin ve dosyayı kaydedin.

    !["Export HAR" ekran görüntüsü](media/capture-browser-trace/chromium-network-export-har.png)

1. Adımları Kaydedici'yi Durdurun ve dosyayı kaydedin.

1. Tarayıcı geliştirici araçları bölmesinde, **Konsol** sekmesini seçin. Sağ tıklatın, ardından **kaydet'i seçin...** ve konsol çıktısını bir metin dosyasına kaydedin.

    ![Konsol çıkışının ekran görüntüsü](media/capture-browser-trace/chromium-console-select.png)

1. HAR dosyasını, konsol çıktısını ve ekran kaydını .zip gibi sıkıştırılmış bir biçimde paketleyin ve bunu Microsoft desteğiyle paylaşın.

## <a name="microsoft-edge-edgehtml"></a>Microsoft Edge (EdgeHTML)

Aşağıdaki adımlar, Microsoft Edge'deki geliştirici araçlarının (EdgeHTML) nasıl kullanılacağını gösterir. Daha fazla bilgi için [Microsoft Edge (EdgeHTML) Geliştirici Araçları'na](/microsoft-edge/devtools-guide)bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın. İzlemeye başlamadan _önce_ oturum açmanız önemlidir, böylece izleme oturum açmanızla ilgili hassas bilgiler içermez. 

1. [Steps Recorder'ı](https://support.microsoft.com/help/22878/windows-10-record-steps)kullanarak portalda attığınız adımları kaydetmeye başlayın.

1. Portalda, sorunun oluştuğu yerden hemen önce adıma gidin.

1. F12 tuşuna ![basın veya](media/capture-browser-trace/edge-icon-settings.png) > tarayıcı ayarları simgesinin Ekran Görüntüsü'nü seçin**Diğer araçlar** > **Geliştirici araçları**.

1. Varsayılan olarak, tarayıcı yalnızca şu anda yüklenen sayfaiçin izleme bilgilerini tutar. Repro'nuz birden fazla sayfaya gitmeyi gerektirse bile, tarayıcının tüm izleme bilgilerini saklayacak şekilde aşağıdaki seçenekleri ayarlayın:

    1. **Ağ** sekmesini seçin ve ardından **gezinmede girişleri temizle seçeneğini temizleyin.**

          !["Gezinmedeki girişleri temizle" ekran görüntüsü](media/capture-browser-trace/edge-network-clear-entries.png)

    1. **Konsol** sekmesini seçin, ardından **Günlüğü Koru'yu**seçin.

          !["Günlüğü Koru" ekran görüntüsü](media/capture-browser-trace/edge-console-preserve-log.png)

1. **Ağ** sekmesini seçin, ardından **profil oluşturmayı durdur ve oturumu** **temizle'yi**seçin.

    !["Profil oluşturmayı durdur" ve "Oturumu temizle" ekran görüntüsü](media/capture-browser-trace/edge-stop-clear-session.png)

1. **Profil oluşturma oturumunu başlat'ı**seçin ve ardından sorunu portalda yeniden üretin.

    !["Profil oluşturma oturumubaşlat" ekran görüntüsü](media/capture-browser-trace/edge-start-session.png)

    Aşağıdaki resme benzer oturum çıktısı görürsünüz.

    ![Tarayıcı izleme sonuçlarının ekran görüntüsü](media/capture-browser-trace/edge-browser-trace-results.png)

1. Beklenmeyen portal davranışını yeniden çoğalttınktan sonra **profil oluşturmayı durdur'u**seçin, ardından **HAR olarak Dışa Aktar'ı** seçin ve dosyayı kaydedin.

    !["HAR olarak export" ekran görüntüsü](media/capture-browser-trace/edge-network-export-har.png)

1. Adımları Kaydedici'yi Durdurun ve dosyayı kaydedin.

1. Tarayıcı geliştirici araçları bölmesinde Konsol **sekmesini** seçin ve pencereyi genişletin. İmlecinizi konsol çıkışının başlangıcına yerleştirin ve çıktının tüm içeriğini sürükleyin ve seçin. Sağ tıklatın, sonra **Kopyala'yı**seçin ve konsol çıktısını bir metin dosyasına kaydedin.

    ![Konsol çıkışının ekran görüntüsü](media/capture-browser-trace/edge-console-select.png)

1. HAR dosyasını, konsol çıktısını ve ekran kaydını .zip gibi sıkıştırılmış bir biçimde paketleyin ve bunu Microsoft desteğiyle paylaşın.

## <a name="apple-safari"></a>Apple Safari

Aşağıdaki adımlar, Apple Safari'deki geliştirici araçlarının nasıl kullanılacağını gösterir. Daha fazla bilgi için [Safari Geliştirici Araçlarına genel bakış](https://support.apple.com/guide/safari-developer/safari-developer-tools-overview-dev073038698/11.0/mac)bilgisine bakın.

1. Apple Safari'deki geliştirici araçlarını etkinleştirin:

    1. **Safari'yi**seçin, ardından **Tercihler'i**seçin.

        ![Safari tercihlerinin ekran görüntüsü](media/capture-browser-trace/safari-preferences.png)

    1. **Gelişmiş** sekmesini seçin, ardından **menü çubuğunda Geliştir menüsünü göster'i**seçin.

        ![Safari gelişmiş tercihleri ekran görüntüsü](media/capture-browser-trace/safari-show-develop-menu.png)

1. [Azure portalında](https://portal.azure.com)oturum açın. İzlemeye başlamadan _önce_ oturum açmanız önemlidir, böylece izleme oturum açmanızla ilgili hassas bilgiler içermez. 

1. Portalda attığınız adımları kaydetmeye başlayın. Daha fazla bilgi için, [ekranı Mac'inizde nasıl kaydedebilirsiniz'](https://support.apple.com/HT208721)e bakın.

1. Portalda, sorunun oluştuğu yerden hemen önce adıma gidin.

1. **Geliştir'i**seçin, ardından **Web Denetçisi'ni göster'i**seçin.

    !["Web Denetçisini Göster" ekran görüntüsü](media/capture-browser-trace/safari-show-web-inspector.png)

1. Varsayılan olarak, tarayıcı yalnızca şu anda yüklenen sayfaiçin izleme bilgilerini tutar. Repro'nuz birden fazla sayfaya gitmeyi gerektirse bile, tarayıcının tüm izleme bilgilerini saklayacak şekilde aşağıdaki seçenekleri ayarlayın:

    1. **Ağ** sekmesini seçin ve ardından **Günlüğü Koru'yu**seçin.

          !["Günlüğü Koru" ekran görüntüsü](media/capture-browser-trace/safari-network-preserve-log.png)

    1. **Konsol** sekmesini seçin, ardından **Günlüğü Koru'yu**seçin.

          !["Günlüğü Koru" ekran görüntüsü](media/capture-browser-trace/safari-console-preserve-log.png)

1. **Ağ** sekmesini seçin ve ardından **Ağ Öğelerini Temizle'yi**seçin.

    !["Ağ Öğelerini Temizle" ekran görüntüsü](media/capture-browser-trace/safari-clear-session.png)

1. Sorunu portalda yeniden üretin. Aşağıdaki resme benzer oturum çıktısı görürsünüz.

    ![Tarayıcı izleme sonuçlarının ekran görüntüsü](media/capture-browser-trace/safari-browser-trace-results.png)

1. Beklenmeyen portal davranışını yeniden çoğalttınktan sonra **Dışa Aktar'ı** seçin ve dosyayı kaydedin.

    !["Export" ekran görüntüsü](media/capture-browser-trace/safari-network-export-har.png)

1. Ekran kaydedicisini durdurun ve dosyayı kaydedin.

1. Tarayıcı geliştirici araçları bölmesinde Konsol **sekmesini** seçin ve pencereyi genişletin. İmlecinizi konsol çıkışının başlangıcına yerleştirin ve çıktının tüm içeriğini sürükleyin ve seçin. Çıktıyı kopyalamak ve bir metin dosyasına kaydetmek için Komut-C'yi kullanın.

    ![Konsol çıkışının ekran görüntüsü](media/capture-browser-trace/safari-console-select.png)

1. HAR dosyasını, konsol çıktısını ve ekran kaydını .zip gibi sıkıştırılmış bir biçimde paketleyin ve bunu Microsoft desteğiyle paylaşın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure portal’a genel bakış](azure-portal-overview.md)