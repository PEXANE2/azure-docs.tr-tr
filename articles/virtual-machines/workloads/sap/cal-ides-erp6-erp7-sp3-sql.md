---
title: SAP IDES EHP7 SP3'u AZURE'da SAP ERP 6.0 için dağıtın | Microsoft Dokümanlar
description: SAP IDES EHP7 SP3'u AZURE'da SAP ERP 6.0 için dağıtın
services: virtual-machines-windows
documentationcenter: ''
author: hermanndms
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 3efd92226b7c69590f3960458ffec49b63b8364f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77616698"
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a>SAP IDES EHP7 SP3'u AZURE'da SAP ERP 6.0 için dağıtın
Bu makalede, SAP Bulut Cihazı Kitaplığı (SAP CAL) 3.0 üzerinden SQL Server ve Windows işletim sistemi ile çalışan bir SAP IDES sisteminin Azure'da nasıl dağıtılanılacak anlatılmaktadır. Ekran görüntüleri adım adım süreci gösterir. Farklı bir çözüm dağıtmak için aynı adımları izleyin.

SAP CAL ile başlamak için [SAP Cloud Appliance Library](https://cal.sap.com/) web sitesine gidin. SAP de yeni SAP [Cloud Appliance Library 3.0](https://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience)hakkında bir blog var. 

> [!NOTE]
> 29 Mayıs 2017 itibariyle, SAP CAL'ı dağıtmak için daha az tercih edilen klasik dağıtım modeline ek olarak Azure Kaynak Yöneticisi dağıtım modelini kullanabilirsiniz. Yeni Kaynak Yöneticisi dağıtım modelini kullanmanızı ve klasik dağıtım modelini göz ardı edersiniz.

Zaten klasik modeli kullanan bir SAP CAL *hesabı oluşturduysanız, başka bir SAP CAL hesabı oluşturmanız gerekir.* Bu hesabın Kaynak Yöneticisi modelini kullanarak yalnızca Azure'da dağıtılması gerekir.

SAP CAL'da oturum açmanızı takiben, ilk sayfa genellikle sizi **Çözümler** sayfasına yönlendirir. SAP CAL'da sunulan çözümler sürekli olarak artmaktadır, bu nedenle istediğiniz çözümü bulmak için biraz kaydırmanız gerekebilir. Yalnızca Azure'da kullanılabilen vurgulanan Windows tabanlı SAP IDES çözümü dağıtım işlemini gösterir:

![SAP CAL Çözümleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-the-sap-cal"></a>SAP CAL'da hesap oluşturma
1. SAP CAL'da ilk kez oturum açabilmek için SAP S-User'ınızı veya SAP'ye kayıtlı diğer kullanıcınızı kullanın. Ardından, cihazları Azure'a dağıtmak için SAP CAL tarafından kullanılan bir SAP CAL hesabı tanımlayın. Hesap tanımında şunları yapmanız gerekir:

    a. Azure'da dağıtım modelini seçin (Kaynak Yöneticisi veya klasik).

    b. Azure aboneliğinizi girin. Bir SAP CAL hesabı yalnızca bir aboneye atanabilir. Birden fazla aboneliğe ihtiyacınız varsa, başka bir SAP CAL hesabı oluşturmanız gerekir.
    
    c. SAP CAL'a Azure aboneliğinize dağıtım izni verin.

   > [!NOTE]
   >  Sonraki adımlar, Kaynak Yöneticisi dağıtımları için sap cal hesabının nasıl oluşturulurolduğunu gösterir. Zaten klasik dağıtım modeline bağlı bir SAP CAL hesabınız varsa, yeni bir SAP CAL hesabı oluşturmak için aşağıdaki adımları izlemeniz *gerekir.* Yeni SAP CAL hesabının Kaynak Yöneticisi modelinde dağıtılması gerekir.

1. Yeni bir SAP CAL hesabı oluşturmak **için, Hesaplar** sayfası Azure için iki seçenek gösterir: 

    a. **Microsoft Azure (klasik)** klasik dağıtım modelidir ve artık tercih edilmez.

    b. **Microsoft Azure,** yeni Kaynak Yöneticisi dağıtım modelidir.

    ![SAP CAL Hesapları](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    Kaynak Yöneticisi modelinde dağıtmak için **Microsoft Azure'u**seçin.

    ![SAP CAL Hesapları](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

1. Azure portalında bulunabilecek Azure **Abonelik Kimliği'ni** girin. 

    ![SAP CAL Abonelik Kimliği](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

1. SAP CAL'ı tanımladığınız Azure aboneliğine dağıtma yetkisi vermek için **Yetkilendir'i**tıklatın. Aşağıdaki sayfa tarayıcı sekmesinde görünür:

    ![Internet Explorer bulut hizmetleri oturum açma](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

1. Birden fazla kullanıcı listelenmişse, seçtiğiniz Azure aboneliğinin yardımcı yöneticisi olmak üzere bağlantılı Microsoft hesabını seçin. Aşağıdaki sayfa tarayıcı sekmesinde görünür:

    ![Internet Explorer bulut hizmetleri onayı](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

1. **Kabul et**'e tıklayın. Yetkilendirme başarılı olursa, SAP CAL hesap tanımı yeniden görüntülenir. Kısa bir süre sonra, bir ileti yetkilendirme işleminin başarılı olduğunu doğrular.

1. Yeni oluşturulan SAP CAL hesabını kullanıcınıza atamak için, sağdaki metin kutusuna **Kullanıcı Kimliğinizi** girin ve **Ekle'yi**tıklatın. 

    ![Hesap-kullanıcı ilişkisi](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

1. Hesabınızı SAP CAL'da oturum açmada kullandığınız kullanıcıyla ilişkilendirmek için **Gözden Geçir'i**tıklatın. 

1. Kullanıcınızla yeni oluşturulan SAP CAL hesabı arasındaki ilişkiyi oluşturmak için **Oluştur'u**tıklatın.

    ![Kullanıcıdan hesap ilişkisine](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

Başarılı bir şekilde şunları yapabiliyor bir SAP CAL hesabı oluşturdunuz:

- Resource Manager dağıtım modelini kullanın.
- SAP sistemlerini Azure aboneliğinize dağıtın.

> [!NOTE]
> SAP IDES çözümlerini Windows ve SQL Server'a göre dağıtmadan önce bir SAP CAL aboneliğine kaydolmanız gerekebilir. Aksi takdirde, çözüm genel bakış sayfasında **Kilitli** olarak görünebilir.

### <a name="deploy-a-solution"></a>Bir çözüm dağıtma
1. Bir SAP CAL hesabı ayarladıktan sonra **Windows ve SQL Server çözümünde SAP IDES çözümünü** seçin. **Örnek Oluştur'u**tıklatın ve kullanım ve şartlar koşullarını onaylayın. 

1. Temel **Modda: Örnek Oluşturma** sayfası, şunları yapmanız gerekir:

    a. Örnek **Adı**girin.

    b. Azure **Bölgesi'ni**seçin. Birden çok Azure bölgesinin sunulması için bir SAP CAL aboneliğine ihtiyacınız olabilir.

    c.  Gösterildiği gibi çözüm için ana **Parola'yı** girin:

    ![SAP CAL Temel Modu: Örnek Oluştur](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

1. **Oluştur'u**tıklatın. Bir süre sonra, çözümün boyutuna ve karmaşıklığına bağlı olarak (SAP CAL bir tahmin sağlar), durum etkin ve kullanıma hazır olarak gösterilir: 

    ![SAP CAL Örnekleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

1. KAYNAK grubunu ve SAP CAL tarafından oluşturulan tüm nesnelerini bulmak için Azure portalına gidin. Sanal makine SAP CAL'da verilen aynı örnek adı ile başlayarak bulunabilir.

    ![Kaynak grubu nesneleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

1. SAP CAL portalında, dağıtılan örneklere gidin ve **Bağlan'ı**tıklatın. Aşağıdaki açılır pencere görüntülenir: 

    ![Örne bağlan](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

1. Dağıtılan sistemlere bağlanmak için seçeneklerden birini kullanmadan önce **Başlangıç Kılavuzu'nu**tıklatın. Belgeler, bağlantı yöntemlerinin her biri için kullanıcıları adlandırır. Bu kullanıcıların parolaları, dağıtım işleminin başında tanımladığınız ana parolaya ayarlanır. Belgelerde, diğer daha işlevsel kullanıcılar, dağıtılan sistemde oturum açmanız için kullanabileceğiniz parolalarıyla birlikte listelenir.

    ![SAP Karşılama belgeleri](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

Birkaç saat içinde Azure'da sağlıklı bir SAP IDES sistemi dağıtılır.

SAP CAL aboneliği satın aldıysanız, SAP Azure'daki SAP CAL aracılığıyla dağıtımları tam olarak destekler. Destek sırası BC-VCM-CAL'dır.

