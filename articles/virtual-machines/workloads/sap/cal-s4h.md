---
title: SAP S/4HANA veya BW/4HANA'yı Azure VM'de dağıtma | Microsoft Dokümanlar
description: SAP S/4HANA veya BW/4HANA'yı Azure VM'de dağıtma
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: c110a4e0429ba52e01c472097a2241f91d504cf5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616201"
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>SAP S/4HANA veya BW/4HANA'yı Azure'da dağıtma
Bu makalede, SAP Bulut Cihazı Kitaplığı (SAP CAL) 3.0'ı kullanarak Azure'da S/4HANA'nın nasıl dağıtılancayaçalış'ı açıklanmaktadır. BW/4HANA gibi diğer SAP HANA tabanlı çözümleri dağıtmak için aynı adımları izleyin.

> [!NOTE]
> SAP CAL hakkında daha fazla bilgi için [SAP Cloud Appliance Library](https://cal.sap.com/) web sitesine gidin. SAP de [SAP Cloud Appliance Library 3.0](https://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)hakkında bir blog vardır.
> 
> [!NOTE]
> 29 Mayıs 2017 itibariyle, SAP CAL'ı dağıtmak için daha az tercih edilen klasik dağıtım modeline ek olarak Azure Kaynak Yöneticisi dağıtım modelini kullanabilirsiniz. Yeni Kaynak Yöneticisi dağıtım modelini kullanmanızı ve klasik dağıtım modelini göz ardı edersiniz.

## <a name="step-by-step-process-to-deploy-the-solution"></a>Çözümü dağıtmak için adım adım işlem

Aşağıdaki ekran görüntüleri dizisi, SAP CAL'ı kullanarak Azure'da S/4HANA'yı nasıl dağıtabileceğinizi gösterir. Süreç BW/4HANA gibi diğer çözümler için de aynı şekilde çalışır.

**Çözümler** sayfası, Azure'da kullanılabilen SAP CAL HANA tabanlı çözümlerden bazılarını gösterir. **SAP S/4HANA 1610 FPS01, Tam Aktif Cihaz** orta sıradadır:

![SAP CAL Çözümleri](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-the-sap-cal"></a>SAP CAL'da hesap oluşturma
1. SAP CAL'da ilk kez oturum açabilmek için SAP S-User'ınızı veya SAP'ye kayıtlı diğer kullanıcınızı kullanın. Ardından, cihazları Azure'a dağıtmak için SAP CAL tarafından kullanılan bir SAP CAL hesabı tanımlayın. Hesap tanımında şunları yapmanız gerekir:

    a. Azure'da dağıtım modelini seçin (Kaynak Yöneticisi veya klasik).

    b. Azure aboneliğinizi girin. Bir SAP CAL hesabı yalnızca bir aboneye atanabilir. Birden fazla aboneliğe ihtiyacınız varsa, başka bir SAP CAL hesabı oluşturmanız gerekir.

    c. SAP CAL'a Azure aboneliğinize dağıtım izni verin.

   > [!NOTE]
   >  Sonraki adımlar, Kaynak Yöneticisi dağıtımları için sap cal hesabının nasıl oluşturulurolduğunu gösterir. Zaten klasik dağıtım modeline bağlı bir SAP CAL hesabınız varsa, yeni bir SAP CAL hesabı oluşturmak için aşağıdaki adımları izlemeniz *gerekir.* Yeni SAP CAL hesabının Kaynak Yöneticisi modelinde dağıtılması gerekir.

1. Yeni bir SAP CAL hesabı oluşturun. **Hesaplar** sayfası Azure için üç seçenek gösterir: 

    a. **Microsoft Azure (klasik)** klasik dağıtım modelidir ve artık tercih edilmez.

    b. **Microsoft Azure,** yeni Kaynak Yöneticisi dağıtım modelidir.

    c. **Windows Azure 21Vianet tarafından işletilen** klasik dağıtım modeli kullanan Çin'de bir seçenektir.

    Kaynak Yöneticisi modelinde dağıtmak için **Microsoft Azure'u**seçin.

    ![SAP CAL Hesap Detayları](./media/cal-s4h/s4h-pic-2a.png)

1. Azure portalında bulunabilecek Azure **Abonelik Kimliği'ni** girin.

   ![SAP CAL Hesapları](./media/cal-s4h/s4h-pic3c.png)

1. SAP CAL'ı tanımladığınız Azure aboneliğine dağıtma yetkisi vermek için **Yetkilendir'i**tıklatın. Aşağıdaki sayfa tarayıcı sekmesinde görünür:

   ![Internet Explorer bulut hizmetleri oturum açma](./media/cal-s4h/s4h-pic4c.png)

1. Birden fazla kullanıcı listelenmişse, seçtiğiniz Azure aboneliğinin yardımcı yöneticisi olmak üzere bağlantılı Microsoft hesabını seçin. Aşağıdaki sayfa tarayıcı sekmesinde görünür:

   ![Internet Explorer bulut hizmetleri onayı](./media/cal-s4h/s4h-pic5a.png)

1. **Kabul et**'e tıklayın. Yetkilendirme başarılı olursa, SAP CAL hesap tanımı yeniden görüntülenir. Kısa bir süre sonra, bir ileti yetkilendirme işleminin başarılı olduğunu doğrular.

1. Yeni oluşturulan SAP CAL hesabını kullanıcınıza atamak için, sağdaki metin kutusuna **Kullanıcı Kimliğinizi** girin ve **Ekle'yi**tıklatın.

   ![Hesap-kullanıcı ilişkisi](./media/cal-s4h/s4h-pic8a.png)

1. Hesabınızı SAP CAL'da oturum açmada kullandığınız kullanıcıyla ilişkilendirmek için **Gözden Geçir'i**tıklatın. 
 
1. Kullanıcınızla yeni oluşturulan SAP CAL hesabı arasındaki ilişkiyi oluşturmak için **Oluştur'u**tıklatın.

   ![Kullanıcıdan SAP CAL hesap ilişkisine](./media/cal-s4h/s4h-pic9b.png)

Başarılı bir şekilde şunları yapabiliyor bir SAP CAL hesabı oluşturdunuz:

- Resource Manager dağıtım modelini kullanın.
- SAP sistemlerini Azure aboneliğinize dağıtın.

Artık Azure'da kullanıcı aboneliğinize S/4HANA dağıtmaya başlayabilirsiniz.

> [!NOTE]
> Devam etmeden önce, Azure H Serisi VM'ler için Azure vCPU kotanız olup olmadığını belirleyin. Şu anda SAP CAL, SAP HANA tabanlı çözümlerden bazılarını dağıtmak için Azure'un H Serisi VM'lerini kullanır. Azure aboneliğinizde H-Serisi vCPU kotası olmayabilir. Bu nedenle, en az 16 H-Serisi vCPUs kotası almak için Azure desteğine başvurmanız gerekebilir.
> 
> [!NOTE]
> SAP CAL'da Bir çözümü Azure'a dağıttığınızda, yalnızca bir Azure bölgesi seçebileceğinizi görebilirsiniz. SAP CAL tarafından önerilenden başka Azure bölgelerine dağıtmak için SAP'den bir CAL aboneliği satın almanız gerekir. CAL hesabınızın başlangıçta önerilenler dışında Azure bölgelerine teslim edilebilmek için SAP ile bir ileti açmanız da gerekebilir.

### <a name="deploy-a-solution"></a>Bir çözüm dağıtma

SAP CAL'ın **Çözümler** sayfasından bir çözüm dağıtalım. SAP CAL'ın dağıtması gereken iki dizi vardır:

- Dağıtılacak sistemi tanımlamak için tek sayfa kullanan temel bir dizi
- VM boyutları nda belirli seçenekler sağlayan gelişmiş bir dizi 

Burada konuşlanmanın temel yolunu gösteriyoruz.

1. Hesap **Ayrıntıları** sayfasında şunları yapmanız gerekir:

    a. Bir SAP CAL hesabı seçin. (Kaynak Yöneticisi dağıtım modeliyle dağıtmak için ilişkili bir hesap kullanın.)

    b. Örnek **Adı**girin.

    c. Azure **Bölgesi'ni**seçin. SAP CAL bir bölge önerir. Başka bir Azure bölgesine ihtiyacınız varsa ve SAP CAL aboneliğiniz yoksa, SAP ile cal aboneliği sipariş etmeniz gerekir.

    d. Sekiz veya dokuz karakterlik çözüm için ana **Parola** girin. Parola, farklı bileşenlerin yöneticileri için kullanılır.

   ![SAP CAL Temel Modu: Örnek Oluştur](./media/cal-s4h/s4h-pic10a.png)

1. **Oluştur'u**tıklatın ve görünen ileti kutusunda **Tamam'ı**tıklatın.

   ![SAP CAL Destekli VM Boyutları](./media/cal-s4h/s4h-pic10b.png)

1. Özel **Anahtar** iletişim kutusunda, özel anahtarı SAP CAL'da depolamak için **Depola'yı** tıklatın. Özel anahtar için parola korumasını kullanmak için **İndir'i**tıklatın. 

   ![SAP CAL Özel Anahtar](./media/cal-s4h/s4h-pic10c.png)

1. SAP CAL **Uyarı** iletisini okuyun ve **Tamam'ı**tıklatın.

   ![SAP CAL Uyarısı](./media/cal-s4h/s4h-pic10d.png)

    Şimdi dağıtım gerçekleşir. Bir süre sonra, çözümün boyutuna ve karmaşıklığına bağlı olarak (SAP CAL bir tahmin sağlar), durum etkin ve kullanıma hazır olarak gösterilir.

1. Bir kaynak grubunda diğer ilişkili kaynaklarla toplanan sanal makineleri bulmak için Azure portalına gidin: 

   ![Yeni portalda dağıtılan SAP CAL nesneleri](./media/cal-s4h/sapcaldeplyment_portalview.png)

1. SAP CAL portalında durum **Etkin**olarak görünür. Çözüme bağlanmak için **Bağlan'ı**tıklatın. Bu çözüm içinde farklı bileşenlere bağlanmak için farklı seçenekler dağıtılır.

   ![SAP CAL Örnekleri](./media/cal-s4h/active_solution.png)

1. Dağıtılan sistemlere bağlanmak için seçeneklerden birini kullanmadan önce **Başlangıç Kılavuzu'nu**tıklatın. 

   ![Örne bağlan](./media/cal-s4h/connect_to_solution.png)

    Belgeler, bağlantı yöntemlerinin her biri için kullanıcıları adlandırır. Bu kullanıcıların parolaları, dağıtım işleminin başında tanımladığınız ana parolaya ayarlanır. Belgelerde, diğer daha işlevsel kullanıcılar, dağıtılan sistemde oturum açmanız için kullanabileceğiniz parolalarıyla birlikte listelenir. 

    Örneğin, Windows Uzak Masaüstü makinesinde önceden yüklenmiş SAP GUI kullanıyorsanız, S/4 sistemi aşağıdaki gibi görünebilir:

   ![Önceden yüklenmiş SAP GUI'de SM50](./media/cal-s4h/gui_sm50.png)

    Veya DBACockpit kullanıyorsanız, örnek şu şekilde görünebilir:

   ![DBACockpit SAP GUI'de SM50](./media/cal-s4h/dbacockpit.png)

Birkaç saat içinde Azure'da sağlıklı bir SAP S/4 cihazı dağıtılır.

SAP CAL aboneliği satın aldıysanız, SAP Azure'daki SAP CAL aracılığıyla dağıtımları tam olarak destekler. Destek sırası BC-VCM-CAL'dır.







