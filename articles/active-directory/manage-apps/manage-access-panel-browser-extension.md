---
title: IE için Azure erişim paneli uzantısı sorunlarını giderme | Microsoft Docs
description: Uygulamalarım portalı için Internet Explorer eklentisini dağıtmak üzere Grup İlkesi 'ni kullanma.
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
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "67723912"
---
# <a name="troubleshoot-the-access-panel-extension-for-internet-explorer"></a>Internet Explorer için erişim paneli uzantısı sorunlarını giderme

Bu makale aşağıdaki sorunları gidermenize yardımcı olur:

* Internet Explorer kullanırken uygulamalarınıza uygulamalarım portalından erişemedik.
* Yazılımı zaten yüklemiş olsanız bile "yazılım yükleme" iletisini görürsünüz.

Yönetici değilseniz, [Grup İlkesi kullanarak Internet Explorer Için erişim paneli uzantısını dağıtma](deploy-access-panel-browser-extension.md)bölümüne bakın.

## <a name="run-the-diagnostic-tool"></a>Tanılama aracını çalıştırma

Erişim paneli Tanılama Aracı 'nı indirip çalıştırarak erişim paneli uzantısıyla ilgili yükleme sorunlarını tanılayabilirsiniz. 

Tanılama aracını indirmek ve yüklemek için:

1. [Tanılama aracını indirmek için bu bağlantıyı seçin.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
1. Dosyayı açın ve içeriğini bilgisayarınıza ayıklayın.
1. Aracı çalıştırmak için *accesspanelextensiondiagnostictool. js* adlı dosyaya sağ tıklayın ve > **Microsoft Windows tabanlı betik Konağı** **ile aç**' ı seçin.

    ![> Microsoft Windows tabanlı betik Konağı ile aç](./media/manage-access-panel-browser-extension/open-access-panel-extension-diagnostic-tool.png)

1. Görünen Tanılama sonuçlarını gözden geçirin ve sorunları çözmek için **Evet** ' i seçin. Uzantı çalışmazsa, ne yapmanız gerekdiklerle ilgili bilgiler içeren **Denetim sonuçları** iletişim kutusu görüntülenir.  
1. İletiyi okuyun ve **Tamam**' ı seçin.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Access panel uzantısının etkin olup olmadığını denetleyin

Internet Explorer 'da erişim paneli uzantısını etkinleştirdiğinizi doğrulamak için:

1. Internet Explorer 'da pencerenin sağ üst köşesindeki **dişli simgesini** seçin ve **Internet seçenekleri**' ni seçin.
1. **Programlar** sekmesine gidin ve eklentileri **Yönet**' i seçin.
1. **Microsoft Corporation** bölümünde **erişim paneli uzantısı** ' nı seçin ve **Etkinleştir**' i seçin.
1. Değişiklikleri kaydetmek için, açık olan tüm Internet Explorer tarayıcı pencerelerini kapatın. Değişiklik, Internet Explorer 'ı bir sonraki açışınızda devreye girer.

## <a name="enable-extensions-for-inprivate-browsing"></a>InPrivate gözatma için uzantıları etkinleştir

InPrivate gözatma için uzantıları etkinleştirmek için:

1. Internet Explorer 'da pencerenin sağ üst köşesindeki **dişli simgesini** seçin ve **Internet seçenekleri**' ni seçin.
1. **Gizlilik** sekmesine gidin ve **InPrivate Gözatma başladığında araç çubuklarını ve uzantıları devre dışı bırak** onay kutusunun işaretini kaldırın.
1. Değişiklikleri kaydetmek için, açık olan tüm Internet Explorer tarayıcı pencerelerini kapatın. Değişiklik, Internet Explorer 'ı bir sonraki açışınızda devreye girer.

## <a name="uninstall-the-access-panel-extension"></a>Erişim paneli uzantısını kaldır

Access panel uzantısını bilgisayarınızdan kaldırmak için:

1. Denetim Masası 'nda *Kaldır*' ı arayın.
1. Arama sonuçlarında **Program kaldır**' ı seçin.

    ![Denetim Masası 'ndaki program Kaldır seçeneğini belirleyin](./media/manage-access-panel-browser-extension/uninstall-program-control-panel.png)

1. Listeden **erişim paneli uzantısı** ' nı seçin ve **Kaldır**' ı seçin.

    ![Erişim paneli uzantısını kaldır](./media/manage-access-panel-browser-extension/uninstall-access-panel-extension.png)

1. Daha sonra sorunun çözümlenip çözümlenmediğini görmek için uzantıyı yeniden yüklemeyi deneyebilirsiniz.

Uzantıyı kaldırma sorunlarıyla karşılaşırsanız, [Microsoft çözümü BT](https://go.microsoft.com/?linkid=9779673) aracı 'nı kullanarak da kaldırabilirsiniz.

## <a name="related-articles"></a>İlgili makaleler:

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma](what-is-single-sign-on.md)
* [grup ilkesi kullanarak Internet Explorer için erişim paneli uzantısını dağıtma](deploy-access-panel-browser-extension.md)
