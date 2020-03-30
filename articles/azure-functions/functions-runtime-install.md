---
title: Azure Fonksiyonları Çalışma Zamanı Yükleme
description: Azure İşlevleri Çalışma Zamanı önizlemesi nasıl yüklenir 2
author: apwestgarth
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: anwestg
ms.openlocfilehash: 7ad748aa9a5b45af10121648a668344548484cf7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74226732"
---
# <a name="install-the-azure-functions-runtime-preview-2"></a>Azure İşlevlerini Çalıştırma Zamanı önizlemesini 2'ye yükleme

[!INCLUDE [intro](../../includes/functions-runtime-preview-note.md)]

Azure İşlevleri Çalışma Zamanı önizleme 2'yi yüklemek istiyorsanız aşağıdaki adımları izleyin:

1. Makinenizin minimum gereksinimleri geçtiğinden emin olun.
1. Azure [İşlerini İndirme Zamanı Önizleme Yükleyicisini](https://aka.ms/azafrv2)indirin.
1. Azure İşlevleri Çalışma Süresi önizlemesini kaldırın 1.
1. Azure İşlevleri Çalışma Süresi önizleme2'yi yükleyin.
1. Azure İşlevleri Çalışma Zamanı önizleme2 yapılandırmasını tamamlayın.
1. Azure İşlevler Çalışma Zamanı Önizlemesinde ilk işlevinizi oluşturun

## <a name="prerequisites"></a>Ön koşullar

Azure İşlevleri Çalışma Zamanı önizlemesini yüklemeden önce aşağıdaki kaynaklara sahip olmalısınız:

1. Microsoft Windows Server 2016 veya Microsoft Windows 10 Creators Update (Professional veya Enterprise Edition) çalıştıran bir makine.
1. Abunuz içinde çalışan bir SQL Server örneği.  Gerekli minimum sürüm SQL Server Express'tir.

## <a name="uninstall-previous-version"></a>Önceki Sürümü Kaldır

Azure İşlevleri Çalışma Zamanı önizlemesini daha önce yüklediyseniz, en son sürümü yüklemeden önce kaldırmanız gerekir.  Windows'da Programları Ekle/Kaldır'daki programı kaldırarak Azure İşlerinden Çalışma Zamanı önizlemesini kaldırın.

## <a name="install-the-azure-functions-runtime-preview"></a>Azure İşlerini Çalıştırma Zamanı Önizlemesini Yükleme

Azure İşzamanı Çalışma Zamanı Önizleme Yükleyici, Azure İşzamanı Önizleme Yönetimi ve Çalışan Rolleri'ni yüklemede size yol görür.  Yönetim ve İşçi rolünü aynı makineye yüklemek mümkündür.  Ancak, daha fazla işlev uygulaması ekledikçe, işlevlerinizi birden çok çalışana ölçeklendirebilmek için ek makinelere daha fazla alt rol dağıtmanız gerekir.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Yönetim ve İşçi Rolünü aynı makineye yükleme

1. Azure İşlevler Çalışma Zamanı Önizleme Yükleyicisini çalıştırın.

    ![Azure İşlevler Runtime önizleme yükleyici][1]

1. **İleri**'ye tıklayın.
1. **EULA**şartlarını okuduktan sonra, şartları kabul etmek için **kutuyu işaretleyin** ve ilerlemek için **İleri'ye** tıklayın.
1. Bu makineye yüklemek istediğiniz rolleri seçin **İşlevler Yönetimi Rolü** ve/veya **İşlevler İşçi Rolü** ve **İleri'yi**tıklatın.

    ![Azure İşlevler Runtime önizleme yükleyici - rol seçimi][3]

    > [!NOTE]
    > **İşlevler İşçi Rolü'ni** diğer birçok makineye yükleyebilirsiniz. Bunu yapmak için, bu yönergeleri izleyin ve yalnızca yükleyicideki **İşlevler İşçi Rolü'nü** seçin.

1. **Azure İşlevleri Çalışma Zamanı Kurulum** Sihirbazı'nın makinenizdeki yükleme işlemini başlatması için **İleri'yi** tıklatın.
1. Kurulum sihirbazı tamamlandıktan sonra **Azure İşlevleri Çalışma Zamanı** yapılandırma aracını başlatır.

    ![Azure İşlevler Runtime önizleme yükleyici tamamlandı][6]

    > [!NOTE]
    > **Windows 10'a** yüklüyorsanız ve **Kapsayıcı** özelliği daha önce etkinleştirilmediyse, **Azure İşlevler Çalışma Zamanı Kurulumu** yüklemeyi tamamlamak için makinenizi yeniden başlatmanızı ister.

## <a name="configure-the-azure-functions-runtime"></a>Azure İşlerini Çalışma Süresini Yapılandırma

Azure İşlevler Runtime yüklemesini tamamlamak için yapılandırmayı tamamlamanız gerekir.

1. **Azure İşlevler Çalışma Zamanı** yapılandırma aracı, makinenizde hangi rollerin yüklü olduğunu gösterir.

    ![Azure İşlevler Runtime önizleme yapılandırma aracı][7]

1. **Veritabanı** sekmesini tıklatın, Bir [Veritabanı ana anahtarı](https://docs.microsoft.com/sql/relational-databases/security/encryption/sql-server-and-database-encryption-keys-database-engine)belirtme de dahil olmak üzere SQL Server örneğinizin bağlantı ayrıntılarını girin ve **Uygula'yı**tıklatın.  Azure İşlevler Çalışma Zamanı'nın Çalışma Süresini destekleyecek bir veritabanı oluşturabilmesi için BIR SQL Server örneğine bağlantı gereklidir.

    ![Azure İşlevler Runtime önizleme veritabanı yapılandırması][8]

1. Kimlik **Bilgileri** sekmesini tıklatın.  Burada, tüm işlev uygulamalarınızı barındırmak için bir dosya paylaşımıyla kullanmak üzere iki yeni kimlik belgesi oluşturmanız gerekir.  **Dosya paylaşım sahibi** ve dosya **paylaşım kullanıcısı**için Kullanıcı **adı** ve **Parola** kombinasyonları belirtin, ardından **Uygula'yı**tıklatın.

    ![Azure İşlevler Runtime önizleme kimlik bilgileri][9]

1. Dosya **Paylaş** sekmesini tıklatın.  Burada dosya paylaşım konumunun ayrıntılarını belirtmeniz gerekir.  Dosya paylaşımı sizin için oluşturulabilir veya varolan bir Dosya Paylaşımı'nı kullanabilirsiniz ve **Uygula'yı**tıklatın.  Yeni bir Dosya Paylaşımı konumu seçerseniz, Azure İşlevler Runtime tarafından kullanılmak üzere bir dizin belirtmeniz gerekir.

    ![Azure İşlevler Runtime önizleme dosya paylaşımı][10]

1. **IIS** sekmesini tıklatın.  Bu sekme, Azure İşlevleri Çalışma Zamanı yapılandırma aracının oluşturduğu IIS'deki web sitelerinin ayrıntılarını gösterir.  Azure İşlevler Runtime önizleme portalı için burada özel bir DNS adı belirtebilirsiniz.  Tamamlamak için **Uygula'yı** tıklatın.

    ![Azure İşlevler Runtime önizleme IIS][11]

1. **Hizmetler** sekmesini tıklatın.  Bu sekme, Azure İşlevler Runtime yapılandırma aracınızdaki hizmetlerin durumunu gösterir.  Azure **İş barındırma etkinleştirme hizmeti** ilk yapılandırmadan sonra çalışmıyorsa, **Hizmeti Başlat'ı**tıklatın.

    ![Azure İşlevler Runtime önizleme yapılandırması tamamlandı][12]

1. **Azure İşlevler Çalışma Zamanı Portalına** ' da `https://<machinename>.<domain>/`göz atın.

    ![Azure İşlevler Runtime önizleme portalı][13]

## <a name="create-your-first-function-in-azure-functions-runtime-preview"></a>Azure İşlevler Çalışma Zamanı önizlemesinde ilk işlevinizi oluşturun

Azure İşlevler Çalışma Zamanı önizlemesinde ilk işlevinizi oluşturmak için

1. `https://<machinename>.<domain>` Örneğin `https://mycomputer.mydomain.com`Azure **İşlevler Çalışma Zamanı Portalına** göz atın.

1. Bir etki alanında **dağıtılırsa,** etki alanı hesabınızın kullanıcı adı ve şifresini kullanarak oturum açmanız istenir, aksi takdirde portalda oturum açmak için yerel hesap kullanıcı adınızı ve parolanızı kullanırsınız.

    ![Azure İşlevler Runtime önizleme portalı girişi][14]

1. İşlev uygulamaları oluşturmak için bir Abonelik oluşturmanız gerekir.  Portalın sol üst köşesinde, aboneliklerin **+** yanındaki seçeneği tıklatın.

    ![Azure İşlevler Runtime önizleme portalı abonelikleri][15]

1. **VarsayılanPlan'ı**seçin, Aboneliğiniz için bir ad girin ve **Oluştur'u**tıklatın.

    ![Azure İşlevler Runtime önizleme portalı abonelik planı ve adı][16]

1. Tüm işlev uygulamalarınız portalın sol bölmesinde listelenir.  Yeni bir İşlev Uygulaması oluşturmak için, başlık **+** **İşlev Uygulamaları'nı** seçin ve seçeneği tıklatın.

1. İşlev uygulamanız için bir ad girin, doğru Aboneliği seçin, Azure İşlevleri çalışma zamanının hangi sürümüne karşı program yapmak istediğinizi seçin ve **Oluştur'u** tıklatın

    ![Azure İşlevler Runtime önizleme portalı yeni işlev uygulaması][17]

1. Yeni işlev uygulamanız portalın sol bölmesinde listelenir.  İşlevler'i seçin ve ardından portaldaki merkez bölmenin üst kısmında **Yeni İşlev'i** tıklatın.

    ![Azure İşlevler Runtime önizleme şablonları][18]

1. Zamanlayıcı Tetikleyici işlevini seçin, sağ uçuç adı işlevinizde ve `*/5 * * * * *` Zamanlama'yı değiştirin (bu cron ifadesi zamanlayıcı işlevinizin her beş saniyede bir çalışmasına neden olur) ve **Oluştur'u** tıklatın

    ![Azure İşlevler Runtime önizleme yeni zamanlayıcı işlevi yapılandırma][19]

1. Işleviniz şimdi oluşturuldu.  Portalın altındaki **günlük** bölmesini genişleterek İşlev uygulamanızın yürütme günlüğünü görüntüleyebilirsiniz.

    ![Azure İşlevler Runtime önizleme işlevi yürütme][20]

<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-Progress.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer6-InstallComplete.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[13]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png
[14]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Login.png
[15]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions.png
[16]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_Subscriptions1.png
[17]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewFunctionApp.png
[18]: ./media/functions-runtime-install/AzureFunctionsRuntime_v1FunctionsTemplates.png
[19]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_NewTimerFunction.png
[20]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal_RunningV2Function.png
