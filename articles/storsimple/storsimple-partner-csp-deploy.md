---
title: Bulut Çözüm Sağlayıcı Programı için StorSimple Sanal Dizi dağıtın
description: StorSimple iş ortakları için StorSimple ve CSP hakkında genel bir bakış.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/08/2017
ms.author: alkohli
ms.openlocfilehash: 7f1927a67127766c72be463c283225135b2a2aad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77466918"
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a>Bulut Çözüm Sağlayıcı Programı için StorSimple Sanal Dizi dağıtın

## <a name="overview"></a>Genel Bakış

StorSimple Virtual Array, Bulut Çözüm Sağlayıcısı (CSP) iş ortakları tarafından müşterileri için dağıtılabilir. Bir CSP iş ortağı bir StorSimple Device Manager hizmeti oluşturabilir. Bu hizmet daha sonra StorSimple Virtual Array'i ve ilişkili paylaşımları, birimleri ve yedeklemeleri dağıtmak ve yönetmek için kullanılabilir.

Bu makalede, bir CSP iş ortağının varolan bir müşteriye nasıl müşteri veya yeni bir abonelik ekleyip sonra CSP'de StorSimple Virtual Array dağıtmak için bir hizmet oluşturabileceği açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları emin olun:

- CSP programı kapsamında kayıtlısınız.
- Geçerli [İş Ortağı Merkezi](https://partnercenter.microsoft.com/) giriş kimliğiniz var. Kimlik bilgileri, yeni müşteriler eklemek, müşteri aramak veya İş Ortağı panosundan bir müşteri hesabına gitmek için İş Ortağı portalında oturum açmanızı sağlar. CSP, Azure portalında müşteri adına StorSimple yöneticisi olarak çalışabilir.
                             
## <a name="add-a-customer"></a>Müşteri ekleme

Bir müşteri eklerseniz, otomatik olarak bir abonelik oluşturulur. Müşteri eklemek (ve otomatik olarak abonelik oluşturmak için), İş Ortağı portalında aşağıdaki adımları gerçekleştirin.

1. [İş Ortağı Merkezi'ne](https://partnercenter.microsoft.com/) gidin ve CSP kimlik bilgilerinizi kullanarak oturum açın. **Pano'ya**tıklayın.

     ![İş Ortağı Merkezinde Pano](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. Sol bölmede **Müşteriler'i**tıklatın. Sağ bölmede **müşteri ekle'yi**tıklatın. Müşterinin ayrıntılarını girin. **Sonraki'yi tıklatın:** Müşteri aboneliği oluşturmak için abonelikler.

    ![Müşteri ekle](./media/storsimple-partner-csp-deploy/image2.png)

3.  **Microsoft Azure** teklifini seçin. Sayfanın altına kaydırın ve **Gözden Geçir'i**tıklatın.

    ![Abonelik bilgilerini gözden geçirme](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. Bilgileri gözden geçirin ve **Gönder'e**tıklayın.

    ![Abonelik gönder](./media/storsimple-partner-csp-deploy/image4.png)

5. Onay bilgilerini gelecekteki başvuruiçin kaydedin.

    ![Onayı kaydet](./media/storsimple-partner-csp-deploy/image5.png)

6. Az önce eklediğiniz müşteriyi bulun veya gidin. Ayrıntılara inmek için **Şirket adını** tıklatın.

    ![Müşteriyi ara](./media/storsimple-partner-csp-deploy/image6.png)  

7. Sol bölmede **Hizmet yönetimini**seçin. Sağ bölmede, Yönet **hizmetleri**altında, müşteriniz için Azure yöneticisi olarak oturum açabilmek için **Microsoft Azure Yönetim Portalı'nı** tıklatın.

    ![Azure portalında oturum açın](./media/storsimple-partner-csp-deploy/image9.png)

8. StorSimple Aygıt Yöneticisi oluşturmak için **+ Yeni'yi** tıklatın ve **StorSimple Virtual Device Series'i**arayın veya gidin. Daha fazla bilgi için [Bir StorSimple Device Manager hizmetini dağıt'a](storsimple-virtual-array-manage-service.md)gidin.

    ![StorSimple Device Manager hizmetini oluşturun](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a>Abonelik ekleme

Bazı durumlarda, varolan bir müşteriniz olabilir ve abonelik eklemeniz gerekir. Varolan bir müşteriye abonelik eklemek için İş Ortağı portalında aşağıdaki adımları gerçekleştirin.

1. [İş Ortağı Merkezi'ne](https://partnercenter.microsoft.com/) gidin ve CSP kimlik bilgilerinizi kullanarak oturum açın. **Pano'ya**tıklayın.

     ![İş Ortağı Merkezinde Pano](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. Sol bölmede **Müşteriler'i**tıklatın. Abonelik eklemek istediğiniz müşteriyi bulun veya diğerinde gezinin. Müşterinizin ![şirket](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) adının satırını genişletmek için Denetimi Genişlet simgesini tıklatın. Ayrıntılarda abonelik **ekle'yi**tıklatın.

    ![Müşteriler](./media/storsimple-partner-csp-deploy/image10.png)

3. Abonelikteki En **İyi teklifler** için **Microsoft Azure'u** denetleyin ve **Gönder'i**tıklatın. Bu yeni bir abonelik oluşturur.

    ![Yeni abonelik ekleme](./media/storsimple-partner-csp-deploy/image11.png)

6. Yeni bir abonelik oluşturulduktan sonra, **Müşteriler** sayfasına dönmek için sol bölmedeki **müşteriler<'** yi tıklatın. Aboneliği yeni oluşturduğunuz müşteriyi arayın. Ayrıntılara inmek için **Şirket adını** tıklatın.

    ![Müşteriyi ara](./media/storsimple-partner-csp-deploy/image6.png)  

7. Sol bölmede **Hizmet yönetimini**seçin. Sağ bölmede, Yönet **hizmetleri**altında, müşteriniz için Azure yöneticisi olarak oturum açabilmek için **Microsoft Azure Yönetim Portalı'nı** tıklatın.

    ![Azure portalında oturum açın](./media/storsimple-partner-csp-deploy/image9.png)

8. StorSimple Aygıt Yöneticisi oluşturmak için **+ Yeni'yi** tıklatın ve **StorSimple Virtual Device Series'i**arayın veya gidin. Daha fazla bilgi için [Bir StorSimple Device Manager hizmetini dağıt'a](storsimple-virtual-array-manage-service.md)gidin.

    ![StorSimple Device Manager hizmetini oluşturun](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a>Sonraki adımlar

- CSP'deki StorSimple ile ilgili daha fazla sorunuz varsa, [CSP'deki StorSimple'a gidin: Sık sorulan sorular.](storsimple-partner-csp-faq.md)
- StorSimple'ınızı dağıtmaya hazırsanız, [StorSimple'ınızı CSP'de dağıtın.](storsimple-partner-csp-deploy.md)
