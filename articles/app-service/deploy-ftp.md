---
title: FTP/S kullanarak içeriği dağıtma
description: FTP veya FTPS'yi kullanarak uygulamanızı Azure Uygulama Hizmeti'ne nasıl dağıtılayabilirsiniz öğrenin. Şifrelenmemiş FTP'yi devre dışı bırakarak web sitesi güvenliğini artırın.
ms.assetid: ae78b410-1bc0-4d72-8fc4-ac69801247ae
ms.topic: article
ms.date: 09/18/2019
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: fcc7c5b8fa182cace6e3dae0b1cae4cd41c5dcb9
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81532604"
---
# <a name="deploy-your-app-to-azure-app-service-using-ftps"></a>FTP/S'yi kullanarak uygulamanızı Azure Uygulama Hizmetine dağıtın

Bu makalede, web uygulamanızı, mobil uygulama arka uçunuzu veya API uygulamanızı [Azure Uygulama Hizmetine](https://go.microsoft.com/fwlink/?LinkId=529714)dağıtmak için FTP veya FTPS'yi nasıl kullanacağınızı gösterilmektedir.

Uygulamanızın FTP/S bitiş noktası zaten etkindir. FTP/S dağıtımını etkinleştirmek için yapılandırma gerekmez.

## <a name="open-ftp-dashboard"></a>FTP panosuaçık

1. Azure [portalında,](https://portal.azure.com) **Uygulama Hizmetlerini**arayın ve seçin.

    ![Uygulama hizmetlerini arayın.](media/app-service-continuous-deployment/search-for-app-services.png)

2. Dağıtmak istediğiniz web uygulamasını seçin.

    ![Uygulamanızı seçin.](media/app-service-continuous-deployment/select-your-app.png)

3. **Dağıtım Merkezi** > **FTP** > **Panosu'nü**seçin.

    ![FTP panosuaçık](./media/app-service-deploy-ftp/open-dashboard.png)

## <a name="get-ftp-connection-information"></a>FTP bağlantı bilgilerini alın

FTP panosunda FTPS bitiş noktasını ve uygulama kimlik bilgilerini kopyalamak için **Kopyala'yı** seçin.

![FTP bilgilerini kopyalama](./media/app-service-deploy-ftp/ftp-dashboard.png)

Uygulamanıza dağıtmak için **Uygulama Kimlik Bilgilerini** kullanmanız önerilir, çünkü bu uygulama her uygulamaya özgüdir. Ancak, Kullanıcı **Kimlik Bilgilerini**tıklattığınızda, FTP/S girişi için kullanabileceğiniz kullanıcı düzeyindeki kimlik bilgilerini aboneliğinizdeki tüm Uygulama Hizmeti uygulamalarına ayarlayabilirsiniz.

> [!NOTE]
> Kullanıcı düzeyindekimlik bilgilerini kullanarak FTP/FTPS bitiş noktasına doğrulanması aşağıdaki biçimde bir kullanıcı adı gerektirir: 
>
>`<app-name>\<user-name>`
>
> Kullanıcı düzeyindeki kimlik bilgileri belirli bir kaynağa değil kullanıcıya bağlı olduğundan, oturum açma eylemini doğru uygulama bitiş noktasına yönlendirmek için kullanıcı adı bu biçimde olmalıdır.
>

## <a name="deploy-files-to-azure"></a>Dosyaları Azure'a dağıtma

1. FTP istemcinizden (örneğin, [Visual Studio](https://www.visualstudio.com/vs/community/), [Cyberduck](https://cyberduck.io/)veya [WinSCP),](https://winscp.net/index.php)uygulamanıza bağlanmak için topladığınız bağlantı bilgilerini kullanın.
2. Dosyalarınızı ve ilgili dizin yapısını Azure'daki [ **/site/wwwroot** dizinine](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) (veya Web İşler için **/site/wwwroot/App_Data/İşler/** dizinine kopyalayın).
3. Uygulamanın düzgün çalıştığını doğrulamak için uygulamanızın URL'sine göz atın. 

> [!NOTE] 
> [Git tabanlı dağıtımların](deploy-local-git.md)aksine FTP dağıtımı aşağıdaki dağıtım otomasyonlarını desteklemez: 
>
> - bağımlılık geri yüklenir (NuGet, NPM, PIP ve Composer otomasyonları gibi)
> - .NET ikililerinin derlenmesine
> - web.config nesil (burada bir [Düğüm.js örnek)](https://github.com/projectkudu/kudu/wiki/Using-a-custom-web.config-for-Node-apps)
> 
> Bu gerekli dosyaları yerel makinenizde el ile oluşturun ve ardından uygulamanızla birlikte dağıtın.
>

## <a name="enforce-ftps"></a>FTPS'yi uygulayın

Gelişmiş güvenlik için FTP'ye yalnızca TLS/SSL üzerinden izin vermelisiniz. FTP dağıtımını kullanmazsanız hem FTP hem de FTPS'yi devre dışı kullanabilirsiniz.

Uygulamanızın [Azure portalındaki](https://portal.azure.com)kaynak sayfasında, sol daki gezinmeden **Configuration** > **General ayarlarını** seçin.

Şifrelenmemiş FTP'yi devre dışı kalmak için Yalnızca **FTP durumunda** **FTPS'yi** seçin. Hem FTP'yi hem de FTPS'yi tamamen devre dışı etmek için **Devre Dışı'yı**seçin. İşlemi tamamladıktan sonra **Kaydet**’e tıklayın. Yalnızca **FTPS**kullanıyorsanız, web uygulamanızın **TLS/SSL ayarları** na gözde gezinerek TLS 1.2 veya üzeri uygulamanız gerekir. TLS 1.0 ve 1.1 **sadece FTPS**ile desteklenmez.

![FTP/S'yi devre dışı](./media/app-service-deploy-ftp/disable-ftp.png)

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

[Azure CLI'yi](/cli/azure)kullanarak FTP dağıtımı için [bkz.](./scripts/cli-deploy-ftp.md)

[Azure PowerShell'i](/cli/azure)kullanarak FTP dağıtımı için [FTP (PowerShell) kullanarak dosyaları bir web uygulamasına yükleyin'e](./scripts/powershell-deploy-ftp.md)bakın.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-ftp-deployment"></a>SORUN Giderme FTP dağıtımı

- [FTP dağıtımıyla nasıl sorun giderebilirim?](#how-can-i-troubleshoot-ftp-deployment)
- [FTP'yi yayınlayıp kodumu yayınlayamam. Sorunu nasıl çözebilirim?](#im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue)
- [Azure Uygulama Hizmeti'nde FTP'ye pasif modüzerinden nasıl bağlanabilirim?](#how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode)

### <a name="how-can-i-troubleshoot-ftp-deployment"></a>FTP dağıtımıyla nasıl sorun giderebilirim?

FTP dağıtımında sorun giderme için ilk adım, bir dağıtım sorununu çalışma zamanı uygulama sorunundan yalıtmaişlemidir.

Dağıtım sorunu genellikle uygulamanızda dağıtılan hiçbir dosya veya yanlış dosyayla sonuçlanır. FTP dağıtımınızı araştırarak veya alternatif bir dağıtım yolu (kaynak denetimi gibi) seçerek sorun giderebilirsiniz.

Çalışma zamanı uygulama sorunu genellikle uygulamanıza dağıtılan ancak yanlış uygulama davranışıyla sonuçlanan doğru dosya kümesiyle sonuçlanır. Çalışma zamanında kod davranışına odaklanarak ve belirli hata yollarını araştırarak sorun giderebilirsiniz.

Dağıtım veya çalışma zamanı sorununu belirlemek [için, Dağıtım ve Çalışma Zamanı sorunlarına](https://github.com/projectkudu/kudu/wiki/Deployment-vs-runtime-issues)bakın.

### <a name="im-not-able-to-ftp-and-publish-my-code-how-can-i-resolve-the-issue"></a>FTP kullanamıyorum ve kodumu yayımlayamıyorum. Sorunu nasıl çözebilirim?
Doğru ana bilgisayar adını ve [kimlik bilgilerini](#open-ftp-dashboard)girdiğinizi denetleyin. Makinenizdeki aşağıdaki FTP bağlantı noktalarının bir güvenlik duvarı tarafından engellenmediğini de kontrol edin:

- FTP denetim bağlantı noktası: 21
- FTP veri bağlantı noktası: 989, 10001-10300
 
### <a name="how-can-i-connect-to-ftp-in-azure-app-service-via-passive-mode"></a>Azure Uygulama Hizmeti'nde FTP'ye pasif modüzerinden nasıl bağlanabilirim?
Azure Uygulama Hizmeti, hem Etkin hem de Pasif modu üzerinden bağlanmayı destekler. Dağıtım makineleriniz genellikle bir güvenlik duvarının arkasında (işletim sisteminde veya ev veya iş ağının bir parçası olarak) olduğundan pasif mod tercih edilir. [WinSCP belgelerinden](https://winscp.net/docs/ui_login_connection)bir örneğe bakın. 

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için [Git ile Azure'a dağıtmayı](deploy-local-git.md)deneyin. Azure'a Git tabanlı dağıtım sürüm denetimi, paket geri yükleme, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Diğer kaynaklar

* [Azure Uygulama Hizmeti Dağıtım Kimlik Bilgileri](deploy-configure-credentials.md)
