---
title: Visual Studio'dan Azure VM'ye Web Uygulaması yayınlama
description: Visual Studio'dan Azure Sanal Makinesine ASP.NET Web Uygulaması Yayınlama
services: virtual-machines-windows
author: ghogen
manager: jillfra
tags: azure-service-management
ms.assetid: 70267837-3629-41e0-bb58-2167ac4932b3
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/03/2017
ms.author: ghogen
ms.openlocfilehash: 6e5db9b4e46019aa386057d51d956ff11d90f498
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71970874"
---
# <a name="publish-an-aspnet-web-app-to-an-azure-vm-from-visual-studio"></a>Visual Studio'dan azure vm'de ASP.NET bir Web Uygulaması yayınlama

Bu belge, Visual Studio 2019'daki **Microsoft Azure Sanal Makineler** yayımlama özelliğini kullanarak bir Azure sanal makinesinde (VM) ASP.NET bir web uygulamasının nasıl yayımlandırılabildiğini açıklar.  

## <a name="prerequisites"></a>Ön koşullar
Visual Studio'yu bir Azure VM'de ASP.NET bir proje yayınlamak için kullanabilmek için VM'nin doğru şekilde ayarlanması gerekir.

- Makine, ASP.NET bir web uygulamasını çalıştıracak şekilde yapılandırılmalı ve WebDeploy yüklü olmalıdır.

- VM'nin bir DNS adı yapılandırılmış olması gerekir. Daha fazla bilgi için bkz. Windows [VM için Azure portalında tam nitelikli bir etki alanı adı oluşturun.](portal-create-fqdn.md)

## <a name="publish-your-aspnet-web-app-to-the-azure-vm-using-visual-studio"></a>Visual Studio'yu kullanarak ASP.NET web uygulamanızı Azure VM'de yayımlayın
Aşağıdaki bölümde, varolan bir ASP.NET web uygulamasının Azure sanal makinesinde nasıl yayımlanılabildiğini açıklanmaktadır.

1. Web uygulama çözümünüzü Visual Studio 2019'da açın.
2. Solution Explorer'da projeyi sağ tıklatın ve **Yayımla'yı seçin...**
3. **Microsoft Azure Sanal Makineleri**bulana kadar yayımlama seçenekleri arasında gezinmek için sayfanın sağındaki oku kullanın.  

   ![SayfaYı Yayımla - Sağ ok]

4. Microsoft **Azure Sanal Makineler** simgesini seçin ve **Yayımla'yı**seçin.

   ![SayfaYayımla - Microsoft Azure Sanal Makine simgesi]

5. Uygun hesabı seçin (sanal makinenize bağlı Azure aboneliği ile).  
   - Visual Studio'da oturum açmışsanız, hesap listesi tüm kimlik doğrulaması yaptığınız hesaplarla doldurulur.  
   - Oturum açmadıysanız veya ihtiyacınız olan hesap listede yoksa"Hesap ekle"yi seçin. ve oturum açmak için istemleri izleyin.  
   ![Azure Hesap Seçici]  

6. Varolan Sanal Makineler listesinden uygun VM'yi seçin.

   > [!Note]
   > Bu listeyi doldurmak biraz zaman alabilir.

   ![Azure VM Seçici]

7. Yayımlamayı başlatmak için Tamam'ı tıklatın.

8. Kimlik bilgileri istendiğinde, yayımlama haklarıyla yapılandırılan hedef VM'de bir kullanıcı hesabının kullanıcı adı ve parolasını girin. Bu kimlik bilgileri genellikle VM oluşturulurken kullanılan yönetici kullanıcı adı ve paroladır.  

   ![WebDeploy Giriş]

9. Güvenlik sertifikasını kabul edin.

   ![Sertifika Hatası]

10. Yayımlama işleminin ilerlemesini denetlemek için Çıktı penceresini izleyin.

    ![Çıktı Penceresi]

11. Yayımlama başarılı olursa, yeni yayınlanan sitenin URL'sini açmak için bir tarayıcı başlatılır.

**Başarı!**

Artık web uygulamanızı bir Azure sanal makinesinde başarıyla yayınladınız.

## <a name="publish-page-options"></a>Sayfa Seçeneklerini Yayımla

Yayımlama sihirbazını tamamladıktan sonra, belgede yayımlama sayfası yeni yayımlama profili seçilerek açılır.

### <a name="re-publish"></a>Yeniden yayımla

Web uygulamanızda güncelleştirmeleri yayınlamak için Yayımla sayfasındaki **Yayımla** düğmesini seçin.  
- İstenirse, kullanıcı adı ve parola girin.  
- Yayıncılık hemen başlar.

![SayfaYı Yayımla - Yayımla düğmesi]

### <a name="modify-publish-profile-settings"></a>Yayımlama profili ayarlarını değiştirme

Yayımlama profili ayarlarını görüntülemek ve değiştirmek için **Ayarlar...'ı seçin.**  

![Sayfayı Yayımla - Ayarlar düğmesi]

Ayarlarınız şuna benzer:  

![Ayarları Yayımla - Bağlantı sayfası]

#### <a name="save-user-name-and-password"></a>Kullanıcı adını ve Parolayı Kaydet
- Her yayımlamada kimlik doğrulama bilgilerini sağlamaktan kaçının. Bunu yapmak **için, Kullanıcı adı** ve **Parola** alanlarını doldurun ve parola kutusunu **kaydet'i** seçin.
- Doğru bilgileri girdiğinizi doğrulamak için **Bağlantıyı Doğrula** düğmesini kullanın.

#### <a name="deploy-to-clean-web-server"></a>Web sunucusutemizlemek için dağıtın

- Web sunucusunun her yüklemeden sonra web uygulamasının temiz bir kopyasına sahip olduğundan ve önceki dağıtımdan başka dosya kalmadığından emin olmak istiyorsanız, **Ayarlar** sekmesindeki hedef onay **kutusundaek dosyaları kaldır'ı** denetleyebilirsiniz.

- Uyarı: Bu ayarda yayımlama, web sunucusunda bulunan tüm dosyaları (wwwroot dizinini) siler. Bu seçenek etkinken yayımlamadan önce makinenin durumunu bildiğinizden emin olun. 

![Ayarları Yayımla - Ayarlar sayfası]

## <a name="next-steps"></a>Sonraki adımlar

### <a name="set-up-cicd-for-automated-deployment-to-azure-vm"></a>Azure VM'ye otomatik dağıtım için CI/CD ayarlama

Azure Ardışık Hatları ile sürekli bir teslim ardışık alanı ayarlamak için windows [sanal makineye dağıt'a](https://docs.microsoft.com/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups)bakın.

[VM Overview - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSName.png
[IP Address Config - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/IPAddressConfigDNSName.png
[VM Overview - DNS Configured]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSConfigured.png
[SayfaYı Yayımla - Sağ ok]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageRightArrow.png
[SayfaYayımla - Microsoft Azure Sanal Makine simgesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageMicrosoftAzureVirtualMachineIcon.png
[Azure Hesap Seçici]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectAccount.png
[Azure VM Seçici]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectVM.png
[WebDeploy Giriş]: ../../../includes/media/publish-web-app-from-visual-studio/WebDeployLogin.png
[Sertifika Hatası]: ../../../includes/media/publish-web-app-from-visual-studio/CertificateError.png
[Çıkış Penceresi]: ../../../includes/media/publish-web-app-from-visual-studio/OutputWindow.png
[SayfaYı Yayımla - Yayımla düğmesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPagePublishButton.png
[Sayfayı Yayımla - Ayarlar düğmesi]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageSettingsButton.png
[Ayarları Yayımla - Bağlantı sayfası]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsConnectionPage.png
[Ayarları Yayımla - Ayarlar sayfası]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsSettingsPage.png
