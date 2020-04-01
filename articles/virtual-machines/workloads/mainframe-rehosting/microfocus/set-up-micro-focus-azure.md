---
title: Azure'da Micro Focus Enterprise Server 4.0 ve Enterprise Developer 4.0'ı yükleyin | Microsoft Dokümanlar
description: Azure sanal makinelerinde (VM) Micro Focus geliştirme ve test ortamını kullanarak IBM z/OS ana bilgisayar iş yüklerinizi yeniden barındırın.
services: virtual-machines-linux
documentationcenter: ''
author: njray
ms.author: edprice
manager: edprice
editor: edprice
ms.topic: conceptual
ms.date: 03/30/2020
tags: ''
keywords: ''
ms.service: multiple
ms.openlocfilehash: 5b3fa5faccf17df17bc4f7cc2d8b023b868fdbe1
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411190"
---
# <a name="install-micro-focus-enterprise-server-40-and-enterprise-developer-40-on-azure"></a>Azure'da Micro Focus Enterprise Server 4.0 ve Enterprise Developer 4.0'ı yükleyin

Bu makalede, [Azure'da Micro Focus Enterprise Server 4.0](https://www.microfocus.com/documentation/enterprise-developer/es30/) ve Micro Focus Enterprise Developer [4.0'ın](https://www.microfocus.com/documentation/enterprise-developer/ed_30/) nasıl kurulabildiğini gösterilmektedir.

Azure'da yaygın bir iş yükü geliştirme ve test ortamıdır. Bu senaryo yaygındır, çünkü çok uygun maliyetlidir ve dağıtılması ve yıkılması kolaydır. Micro Focus, Enterprise Server ile mevcut en büyük ana bilgisayar barındırma platformlarından birini oluşturdu. Windows veya Linux sanal makineleri (VM) kullanarak Azure'da daha ucuz bir x86 platformunda z/OS iş yüklerini çalıştırabilirsiniz.

> [!NOTE]
> Yakında: Azure VM'lerde [Micro Focus Enterprise Server 5.0'ı](https://techcommunity.microsoft.com/t5/azurecat/micro-focus-enterprise-server-5-0-quick-start-template-on-azure/ba-p/1160110) kurma talimatları.

Bu kurulum, Microsoft SQL Server 2017 yüklü olan Azure Marketi'nden Windows Server 2016 görüntüsünü çalıştıran Azure VM'leri kullanır. Bu kurulum Azure Yığını için de geçerlidir.

Enterprise Server için karşılık gelen geliştirme ortamı, Microsoft Visual Studio 2017 veya sonraki saatlerde, Visual Studio Community (ücretsiz indirme) veya Eclipse'de çalışan Enterprise Developer'dır. Bu makalede, Visual Studio 2017 veya daha sonra yüklü bir Windows Server 2016 sanal makine kullanarak nasıl dağıtılan gösterir.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, şu ön koşullara göz atın:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

- Micro Focus yazılımı ve geçerli bir lisans (veya deneme lisansı). Mevcut bir Micro Focus müşterisiyseniz, Micro Focus temsilcinize başvurun. Aksi takdirde, [bir deneme isteyin.](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/)

- Enterprise Server [ve Enterprise Developer](https://www.microfocus.com/documentation/enterprise-developer/#")için belgeleri alın.

> [!NOTE]
> En iyi yöntem, Azure VM'lerine erişimi kontrol edebilmeniz için siteden siteye sanal özel ağ (VPN) tüneli veya atlama kutusu kurmaktır.

## <a name="install-enterprise-server"></a>Enterprise Server’ı yükleme

1. Daha iyi güvenlik ve yönetilebilirlik için, sadece bu proje için yeni bir kaynak grubu oluşturmayı düşünün —örneğin, **RGMicroFocusEntServer**. Listede daha kolay bir şekilde görünmesini sağlamak için kaynak türünü seçmek için Azure'da adın ilk bölümünü kullanın.

2. Sanal makine oluşturur. Azure Marketi'nden istediğiniz sanal makineyi ve işletim sistemini seçin. Önerilen kurulum aşağıda veda edebilirsiniz:

    - **Kurumsal Sunucu**: Windows Server 2016 ve SQL Server 2017 yüklü ES2 v3 VM (2 vCPUs ve 16 GB bellekile) seçeneğini belirleyin. Bu resim Azure Marketi'nden edinilebilir. Kurumsal Sunucu, Azure SQL Veritabanı'nı da kullanabilir.

    - **Kurumsal Geliştirici**: Windows 10 ve Visual Studio yüklü B2ms VM (2 vCPUs ve 8 GB bellek ile) seçin. Bu resim Azure Marketi'nden edinilebilir.

3. Temel **Bilgiler** bölümüne kullanıcı adınızı ve şifrenizi girin. VM'ler için kullanmak istediğiniz **Abonelik** ve **Konum/Bölge'yi** seçin. Kaynak Grubu için **RGMicroFocusEntServer'ı** seçin.

4. Birbirleriyle iletişim kurabilmeleri için her iki VM'yi de aynı sanal ağa koyun.

5. Ayarların geri kalanı için varsayılanları kabul edin. Bu VM'lerin yöneticisi için oluşturduğunuz kullanıcı adını ve parolayı unutmayın.

6. Sanal makineler oluşturulduğunda, Gelen 9003, 86 ve 80'i HTTP için ve 3389 için Enterprise Server makinesinde RDP ve Geliştirici makinesinde 3389'u açın.

7. Azure portalındaki Enterprise Server sanal makinesinde oturum açabilmek için ES2 v3 VM'yi seçin. **Genel Bakış** bölümüne gidin ve bir RDP oturumu başlatmak için **Bağlan'ı** seçin. VM için oluşturduğunuz kimlik bilgilerini kullanarak oturum açın.

8. RDP oturumundan aşağıdaki iki dosyayı yükleyin. Windows kullandığınızdan, dosyaları sürükleyip RDP oturumuna bırakabilirsiniz:

    - **es\_40.exe**, Enterprise Server yükleme dosyası.

    - **mflic**, ilgili lisans dosyası-Enterprise Server onsuz yüklemez.

9. Yüklemeyi başlatmak için dosyayı çift tıklatın. İlk pencerede, yükleme konumunu seçin ve son kullanıcı lisans sözleşmesini kabul edin.

     ![Micro Focus Enterprise Server Kurulum ekranı](media/01-enterprise-server.png)

     Kurulum tamamlandığında aşağıdaki ileti görüntülenir:

     ![Micro Focus Enterprise Server Kurulum ekranı](media/02-enterprise-server.png)

### <a name="check-for-updates"></a>Güncelleştirmeleri denetle

Yüklemeden sonra, Microsoft C++ Yeniden Dağıtılabilir ve .NET Framework gibi bir dizi ön koşul Enterprise Server ile birlikte yüklendiğinden, ek güncelleştirmeleri denetlediğinizden emin olun.

### <a name="upload-the-license"></a>Lisansı yükleyin

1. Mikro Odak Lisans İdaresi'ni başlatın.

2. **Başlat** \> **Mikro Odak Lisans Yöneticisi** \> **Lisans İdaresi'ni**tıklatın ve sonra **Install** sekmesini tıklatın. Yüklemek için lisans biçimi türünü seçin: lisans dosyası veya 16 karakterlik lisans kodu. Örneğin, Bir dosya için, **Lisans dosyasında,** vm daha önce yüklenen **mflic** dosyasına göz atın ve **Lisansları Yükle'yi**seçin.

     ![Mikro Odak Lisans Yönetimi iletişim kutusu](media/03-enterprise-server.png)

3. Enterprise Server'ın yüklerini doğrulayın. Bu URL'yi <http://localhost:86/> kullanarak Enterprise Server Administration sitesini bir tarayıcıdan başlatmayı deneyin. Kurumsal Sunucu Yönetimi sayfası gösterildiği gibi görüntülenir.

     ![Kurumsal Sunucu Yönetimi sayfası](media/04-enterprise-admin.png)

## <a name="install-enterprise-developer-on-the-developer-machine"></a>Geliştirici makinesine Kurumsal Geliştirici'yi yükleyin

1. Daha önce oluşturulan kaynak grubunu seçin (örneğin, **RGMicroFocusEntServer),** ardından geliştirici görüntüsünü seçin.

2. Sanal makinede oturum açabilmek için **Genel Bakış** bölümüne gidin ve **Bağlan'ı**seçin. Bu oturumda bir RDP oturumu başlatıyor. VM için oluşturduğunuz kimlik bilgilerini kullanarak oturum açın.

3. RDP oturumundan, aşağıdaki iki dosyayı yükleyin (isterseniz sürükleyin ve bırakın):

    - **edvs2017.exe**, Enterprise Server yükleme dosyası.

    - **mflic**, ilgili lisans dosyası (Enterprise Developer onsuz yük olmaz).

4. Yüklemeyi başlatmak için **edvs2017.exe** dosyasına çift tıklayın. İlk pencerede, yükleme konumunu seçin ve son kullanıcı lisans sözleşmesini kabul edin. İsterseniz, muhtemelen ihtiyacınız olacak bu terminal emülatörü yüklemek için **Rumba 9.5'i** yükleyin'i seçin.

     ![Visual Studio 2017 Kurulum iletişim kutusu için Micro Focus Enterprise Developer](media/04-enterprise-server.png)

5. Kurulum tamamlandıktan sonra aşağıdaki ileti görüntülenir:

     ![Başarılı ileti kurulumu](media/05-enterprise-server.png)

6. Tıpkı Enterprise Server'da yaptığınız gibi Micro Focus Lisans Yöneticisi'ni başlatın. **Başlat** \> **Mikro Odak Lisans Yöneticisi** \> **Lisans İdaresi'ni**seçin ve **Yükle** sekmesini tıklatın.

7. Yüklemek için lisans biçimi türünü seçin: bir lisans dosyası veya 16 karakterlik lisans kodu. Örneğin, Bir dosya için, **Lisans dosyasında,** vm daha önce yüklenen **mflic** dosyasına göz atın ve **Lisansları Yükle'yi**seçin.

     ![Mikro Odak Lisans Yönetimi iletişim kutusu](media/07-enterprise-server.png)

Kurumsal Geliştirici yüklendiğinde, Azure'da Micro Focus geliştirme ve test ortamı nı dağıtımınız tamamlandı!

## <a name="next-steps"></a>Sonraki adımlar

- [Banka Demo uygulamasını ayarlama](./demo.md)
- [Docker konteynerlerinde Enterprise Server'ı çalıştırın](./run-enterprise-server-container.md)
- [Ana bilgisayar uygulaması geçişi](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)
