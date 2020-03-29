---
title: IE için Azure Erişim Masası Uzantısı | Microsoft Dokümanlar
description: Uygulamalarım portalı için Internet Explorer eklentisini dağıtmak için grup ilkesini kullanma.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0269c87572e2a9242a54491103ae0fcc3637518
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67723912"
---
# <a name="troubleshoot-the-access-panel-extension-for-internet-explorer"></a>Internet Explorer için Access Panel Uzantısısorun giderme

Bu makale, aşağıdaki sorunları gidermenize yardımcı olur:

* Internet Explorer'ı kullanırken Uygulamalarım portalı üzerinden uygulamalarınız sayesinde erişemiyorsunuz.
* Yazılımı yüklemiş olsanız bile "Yazılım Yükle" iletisini görürsünüz.

Yöneticiyseniz, [Grup İlkesi'ni kullanarak Internet Explorer için Erişim Masası Uzantısını nasıl dağıtabileceğinize](deploy-access-panel-browser-extension.md)bakın.

## <a name="run-the-diagnostic-tool"></a>Tanılama aracını çalıştırma

Access Paneli tanı lama aracını indirip çalıştırarak Access Panel Uzantısı ile ilgili yükleme sorunlarını tanılayabilirsiniz. 

Tanılama aracını indirmek ve yüklemek için:

1. [Tanılama aracını indirmek için bu bağlantıyı seçin.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
1. Dosyayı açın ve içeriğini bilgisayarınıza ayıklayın.
1. Aracı çalıştırmak için *AccessPanelExtensionDiagnosticTool.js* adlı dosyayı sağ tıklatın ve**Microsoft Windows Tabanlı Komut Dosyası Ana Bilgisayarı**ile >  **Aç'ı**seçin.

    ![Microsoft Windows Tabanlı Komut Dosyası Ana Bilgisayar > ile açık](./media/manage-access-panel-browser-extension/open-access-panel-extension-diagnostic-tool.png)

1. Görünen tanılama sonuçlarını gözden geçirin ve sorunları gidermek için **Evet'i** seçin. **Sonuçları Denetle** iletişim kutusu, uzantı işe yaramazsa ne yapacağına ilişkin bilgilerle birlikte görüntülenir.  
1. İletiyi okuyun ve **Tamam'ı**seçin.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Access Panel Uzantısı'nın etkin olup olmadığını denetleyin

Internet Explorer'da Erişim Masası Uzantısı'nı etkinleştirdiğinizi doğrulamak için:

1. Internet Explorer'da, pencerenin sağ üst köşesindeki **Dişli simgesini** seçin ve **Internet seçeneklerini**seçin.
1. **Programlar** sekmesine gidin ve **eklentileri yönet'i**seçin.
1. **Microsoft Corporation** bölümünde Access **Panel Uzantısı'nı** seçin ve **Etkinleştir'i**seçin.
1. Değişiklikleri kaydetmek için, açık olan tüm Internet Explorer tarayıcı pencerelerini kapatın. Değişiklik, Internet Explorer'ı bir sonraki açtığınızda etkili olur.

## <a name="enable-extensions-for-inprivate-browsing"></a>Özel Tarama için uzantıları etkinleştirme

InPrivate Tarama uzantılarını etkinleştirmek için:

1. Internet Explorer'da, pencerenin sağ üst köşesindeki **Dişli simgesini** seçin ve **Internet seçeneklerini**seçin.
1. **Gizlilik** sekmesine gidin ve **Özel Tarama başlatıldığında araç çubuklarını ve uzantılarını devre dışı** bırak onaykutusunun temiz olduğunu doğrulayın.
1. Değişiklikleri kaydetmek için, açık olan tüm Internet Explorer tarayıcı pencerelerini kapatın. Değişiklik, Internet Explorer'ı bir sonraki açtığınızda etkili olur.

## <a name="uninstall-the-access-panel-extension"></a>Erişim Masası Uzantısını Kaldır

Access Panel Uzantısını bilgisayarınızdan kaldırmak için:

1. Denetim Masası'nda, *kaldır'ı*arayın.
1. Arama sonuçlarında, **programı kaldır'ı**seçin.

    ![Denetim Paneli'nden programı kaldır seçeneğini seçin](./media/manage-access-panel-browser-extension/uninstall-program-control-panel.png)

1. Listeden Access **Panel Uzantısı'nı** seçin ve **Kaldır'ı**seçin.

    ![Erişim Masası Uzantısını Kaldır](./media/manage-access-panel-browser-extension/uninstall-access-panel-extension.png)

1. Daha sonra sorunun çözülüp çözülmediolmadığını görmek için uzantıyı yeniden yüklemeyi deneyebilirsiniz.

Uzantıyı kaldırma sorunlarıyla karşınıza çıkarsa, Microsoft [Fix It](https://go.microsoft.com/?linkid=9779673) aracını kullanarak da kaldırabilirsiniz.

## <a name="related-articles"></a>İlgili makaleler:

* [Azure Active Directory ile uygulama erişimi ve tek oturum açma](what-is-single-sign-on.md)
* [Grup İlkesi'ni kullanarak Internet Explorer için Access Panel Uzantısı nasıl dağıtılır?](deploy-access-panel-browser-extension.md)
