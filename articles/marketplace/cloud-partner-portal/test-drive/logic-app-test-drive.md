---
title: Mantık Uygulaması Test Sürüşü | Azure Marketi
description: Dynamics AX/CRM örneğine veya azure dışında başka bir kaynağa bağlanan Test Sürüşlerini nasıl oluşturacaklarını açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 766f893d71ca0830fe8b69c50145603c6544cc3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80278391"
---
<a name="logic-app-test-drive"></a>Mantık Uygulaması Test Sürüşü
====================

Bu makale, AppSource'da teklif alan ve Dynamics AX/CRM örneğine veya azure dışında başka bir kaynağa bağlanan Test Sürüşünü oluşturmak isteyen Yayıncılar içindir.

<a name="how-to-build-a-logic-app-test-drive"></a>Mantık Uygulaması Test Sürüşü Nasıl Yapılartırın?
-----------------------------------

Mantık App Test Sürücüleri için Test Drive belgeleri şu anda hala [GitHub Işlemleri](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) ve [Müşteri Katılımı](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app)için, daha fazla bilgi için oraya gidin.

<a name="how-to-publish-a-test-drive"></a>Test Sürüşü nasıl yayımlanır?
---------------------------

Artık Test Sürüşünüzü oluşturduğuna göre, bu bölüm Test Sürüşünüzü başarıyla yayımlamanız için gereken alanların her birinde gezinir.

![Test Sürüşü Özelliğini Etkinleştirme](./media/azure-resource-manager-test-drive/howtopub1.png)

İlk ve en önemli alan, doldurmanız için gerekli tüm alanların sunulduğu formtestini isteyip istemediğinizdir. **Hayır'ı seçtiğinizde,** form devre dışı bırakılır ve Test Sürüşü devre dışı bırakılmışsa, Test Sürücünüz üretimden kaldırılır.

*Not*: Kullanıcılar tarafından etkin olarak kullanılan test sürücüleri varsa, bu Test Sürücüleri oturumları sona erene kadar çalışmaya devam eder.

### <a name="details"></a>Ayrıntılar

Doldurulması gereken bir sonraki bölüm, Test Sürüşü teklifiniz hakkındaki ayrıntılardır.

![Test Sürücüsü ayrıntıları](./media/azure-resource-manager-test-drive/howtopub2.png)

**Açıklama -** *[Gerekli Alan]* Test Sürüşünüzde ne olduğu yla ilgili ana açıklamayı burada yazabilirsiniz. Müşteri, Test Sürüşünüzün ürününizle ilgili hangi senaryoları ele alacağınızı okumak için buraya gelecektir. 

**Kullanım Kılavuzu -** *[Gerekli Alan]* Bu, Test Sürüşü deneyiminizin derinlemesine gözden geçirimidir. Müşteri bunu açar ve Test Sürüşü boyunca tam olarak ne yapmalarını istediğinizi gözden geçirebilirsiniz. Bu içeriğin anlaşılması ve takip edilebilen kolay olması önemlidir! (Bir .pdf dosyası olmalıdır)

**Test Sürüşü Demo Videosu -** \[Önerilen\] Kullanım Kılavuzu'na benzer şekilde, Test Sürüşü deneyiminizi içeren bir video eğitimi eklemek en iyisidir. Müşteri bunu test sürüşünden önce veya sırasında izleyecek ve Test Sürüşü boyunca tam olarak ne yapmalarını istediğinizi gözden geçirebilecektir. Bu içeriğin anlaşılması ve takip edilebilen kolay olması önemlidir!

- **Adı** - Videonuzun Adı
- **Link** - YouTube veya Vimeo'dan gömülü bir URL olmalıdır. Gömülü url'nin nasıl alınabildiğini anlatan örnek aşağıdadır:
- **Küçük resim** - Yüksek kaliteli görüntü (533x324) piksel olmalıdır. Test Sürüşü deneyiminizin bir bölümünün ekran görüntüsünü burada almanız önerilir.

Aşağıda, bu alanların Test Sürüşü deneyimi sırasında müşteriniz için nasıl gösteriş yaptığı aşağıda verilmiştir.

![Test Sürüşü alanları görünüm ve his](./media/azure-resource-manager-test-drive/howtopub4.png)

### <a name="technical-configuration"></a>Teknik Yapılandırma

Doldurulması gereken bir sonraki bölüm, Test Sürüşü Mantık Uygulamanızı yapılandırdığınız ve Test Sürüşü örneklerinizin özel olarak nasıl çalıştığını tanımladığınız bölümdür.

![Test Sürüşü teknik yapılandırması](./media/azure-resource-manager-test-drive/howtopub5_logicapp.png)

- **Bölge** - *[Gerekli Alan]* Seçtiğiniz bölge, Test Sürüşü Mantığı Uygulama kaynaklarınızın dağıtıldığı yeri seçtiğiniz bölgedir.

    *Not:* Mantık Uygulamanızda bir bölgede depolanan özel kaynaklar varsa, bu bölgenin burada seçildiğinden emin olun. Bunu yapmanın en iyi yolu, **Logic App'inizi portaldaki Azure aboneliğinizde yerel olarak tam olarak dağıtmak ve** buraya yazmadan önce çalıştığını doğrulamaktır.

- **Maksimum Eşzamanlı Test Sürücüleri** - *[Gerekli Alan]* Zaten dağıtılan ve seçili bölge başına erişim bekleyen Test Sürüşü örneklerinin sayısı. Müşteriler dağıtım beklemek yerine bu Test Sürücülerine anında erişebilir.

    *Not:* Tüm N öğrenci sayınızın Test Sürüşüne almasını istediğiniz bir web semineri/sınıf çalıştırıyorsanız, N sayısı Hot instance ile yayımlamanız ve sınıf normal Hot örnek sayısına yeniden yayımlamak için bir kez daha önerilir.

- **Test Sürüşü Süresi (saat) -** *[Gerekli Alan]* Test Sürüşünün \# saat içinde etkin kalacağı süre. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer.

- **Azure Kaynak Grubu Adı -** *[Gerekli Alan]* Mantık Uygulama Test Sürücülerinizin kaydedildiği Kaynak Grubu adına yazın.

- **Logic App Name -** *[Gerekli Alan]* Müşteri onu almadan önce Test Sürüşüne bir kullanıcı atamak için kullanılan Mantık Uygulamasına yazın, buradaki Mantık Uygulamasının adına yazın. Bu dosyanın yukarıdaki Kaynak Grubuna kaydolduğundan emin olun.

- **Deprovision Logic App Name -** *[Gerekli Alan]* Test Sürüşü'nde oluşturulan tüm kaynakları sağlamanız için Mantık Uygulaması adına yazın. Bu dosyanın yukarıdaki Kaynak Grubuna kaydolduğundan emin olun.

- **Erişim Bilgileri -** *[Gerekli Alan]* Bir müşteri Test Sürüşü aldıktan sonra erişim bilgileri onlara sunulur. Bu yönergeler, Test Sürücüsü Kaynak Yöneticisi şablonunuzdaki yararlı çıktı parametrelerini paylaşmak içindir. Çıktı parametrelerini eklemek için çift kıvırcık köşeli ayraç (örneğin, **{{outputname}}** kullanın ve bunlar konuma doğru şekilde eklenir. (HTML dize biçimlendirme ön ucunda işlemek için burada önerilir).

### <a name="test-drive-deployment-subscription-details"></a>Test Sürüşü Dağıtım Abonelik Ayrıntıları

Doldurulması gereken son bölüm, Azure Aboneliğinizi ve Azure Etkin Dizininizi (AD) bağlayarak Test Sürücülerini otomatik olarak dağıtabilmektir.

![Test Drive Dağıtım abonelik ayrıntıları](./media/azure-resource-manager-test-drive/subdetails1.png)

**Azure Abonelik Kimliği** *[Gerekli Alan]* Bu, Azure hizmetlerine ve Azure portalına erişim izni verir. Abonelik, kaynak kullanımının raporlandığı ve hizmetlerin faturalandırıldığı yerdir. Yalnızca Test Sürücüleri için **ayrı** bir Azure Aboneliğiniz yoksa, lütfen devam edin ve bir tane yapın. Azure portalına giriş yaparak ve sol taraftaki menüde Aboneliklere gezinerek Azure Abonelik Kimlikleri'ni bulabilirsiniz.
(Örnek: "a83645ac-1234-5ab6-6789-1h234g764ghty")

![Azure Abonelikleri](./media/azure-resource-manager-test-drive/subdetails2.png)

**Azure AD Kiracı Kimliği** *[Gerekli Alan]* Zaten mevcut olan bir Kiracı Kimliğiniz varsa, bu kimliği Aşağıda Özellikler -\> Dizin Kimliği'nde bulabilirsiniz.

![Azure Active Directory](./media/azure-resource-manager-test-drive/subdetails3.png)

Aksi takdirde, Azure Etkin Dizini'nde yeni bir Kiracı oluşturun.

![Azure Etkin Dizin özellikleri ekranı](./media/azure-resource-manager-test-drive/subdetails4.png)

! Azure Etkin Dizin](./media/azure-resource-manager-test-drive/subdetails5.png)

![Azure Active Directory kiracıları](./media/azure-resource-manager-test-drive/subdetails6.png)

**Azure AD Uygulama Kimliği** *[Gerekli Alan]* Bir sonraki adım yeni bir uygulama oluşturmak ve kaydetmektir. Bu uygulamayı Test Sürüşü örneğinde işlemleri gerçekleştirmek için kullanacağız.

1. Yeni oluşturulan dizine veya zaten varolan dizine gidin ve filtre bölmesinde Azure Etkin dizinini seçin.
2. "Uygulama kayıtları"nda arama yapın ve "Ekle"ye tıklayın
3. Bir uygulama adı sağlayın.
4. "Web uygulaması / API" olarak Türünü seçin
5. Oturum Aç URL'sinde herhangi bir\'değer sağlayın, bu alanı kullanmayacağız.
6. Oluştur'u tıklatın.
7. Uygulama oluşturulduktan sonra Özellikler' e\> gidin - Uygulamayı çok kiracı olarak ayarlayın ve Kaydet'e çarptı.

Kaydet’e tıklayın. Son adım, bu kayıtlı uygulamanın Uygulama Kimliğini kapmak ve test sürüşü alanına yapıştırmaktır.

![Azure Active Directory uygulama kimliği](./media/azure-resource-manager-test-drive/subdetails7.png)

Uygulamayı aboneliğe dağıtmak için kullandığımızı göz önüne alırsak, uygulamayı aboneliğe katkıda bulunan olarak eklememiz gerekir. Bunlar için talimatlar aşağıdaki gibidir:

1. Abonelikler bıçağına gidin ve yalnızca Test Sürüşü için kullandığınız uygun aboneliği seçin.
1. **Erişim denetimi (IAM)** öğesine tıklayın.
1. Rol **atamaları** sekmesini tıklatın.  ![Azure Active Directory, yeni bir Erişim Denetimi ilkesi ekleme](./media/azure-resource-manager-test-drive/SetupSub7_1.jpg)
1. **Rol Atama ekle'yi**tıklatın.
1. **Katılımcı**olarak rolü ayarlayın.
1. Azure AD uygulamasının adını yazın ve rolü atamak için uygulamayı seçin.
    ![Azure Etkin Dizin izinleri](./media/azure-resource-manager-test-drive/SetupSub7_2.jpg)
1. **Kaydet**'e tıklayın.

**Azure AD Uygulama Anahtarı -** *[Gerekli Alan]* Son alan bir kimlik doğrulama anahtarı oluşturmaktır. Tuşların altında, Bir Anahtar Açıklaması ekleyin, süresi asla sona ermeyecek şekilde ayarlayın ve sonra kaydet'i seçin. Üretimde test sürüşünüzü kıracak, süresi dolmuş bir anahtara sahip olmaktan kaçınmak **önemlidir.** Bu değeri kopyalayın ve gerekli Test Sürüşü alanına yapıştırın.

![Azure Etkin Dizin Anahtarları bölümü](./media/azure-resource-manager-test-drive/subdetails8.png)

> [!CAUTION]
> Şu anda base64 kodlanmış bir anahtar oluşturmadığından Azure Uygulama Kaydı Önizlemesini kullanamazsınız.


<a name="next-steps"></a>Sonraki adımlar
----------

Artık tüm Test Sürüşü alanlarınızın doldurulduğuna göre, teklifinizi gözden geçirin ve **yeniden yayınlayın.** Test Sürüşüniz sertifika sürecini geçtikten sonra, teklifinizin **önizlemesinde** müşteri deneyimini kapsamlı bir şekilde test etmelisiniz. UI'de bir Test Sürüşü başlatın ve Test Sürücülerinizin tam olarak doğru şekilde dağıtıldığını doğrulayın.

Müşterileriniz için sağlanmış olan Test Sürüşü'nün herhangi bir bölümünü silmediğinizi unutmayın, bu nedenle Test Drive hizmeti bu Kaynak Gruplarını müşteriyle birlikte bitirdikten sonra otomatik olarak temizler.

Bir kez Önizleme sunan rahat hissediyorum, şimdi **canlı gitmek**zamanı! Teklif yayımlandıktan sonra Microsoft'tan son bir gözden geçirme işlemi vardır. Teklif inreddedilirse, teklifiniz için mühendislik ilgiliye nelerin düzeltilmesi gerektiğini açıklayan bir bildirim göndeririz.

Daha fazla sorunuz varsa, sorun giderme önerileri arıyorsanız veya Test Sürüşünüzü daha başarılı hale getirmek istiyorsanız, lütfen [SSS, Sorun Giderme & En İyi Uygulamalar'a](./marketing-and-best-practices.md)gidin.
