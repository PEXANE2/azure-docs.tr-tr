---
title: Azure Lab Services | sınıf Laboratuvarı VM 'lerinin parolalarını sıfırlama | Microsoft Docs
description: Azure Lab Services derslik laboratuvarlarında sanal makinelerin (VM) parolalarını sıfırlamayı öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: d63d14da8859c12fa797a04fa9a3c3230219adab
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445347"
---
# <a name="set-or-reset-password-for-virtual-machines-in-classroom-labs-students"></a>Sınıf laboratuvarlarında (öğrenciler) sanal makineler için parola ayarlama veya sıfırlama
Bu makalede, öğrencilerin VM 'Leri için parola ayarlama/sıfırlama işlemlerinin nasıl yapılacağı gösterilir. 

## <a name="enable-resetting-of-passwords"></a>Parolaların sıfırlanmasını etkinleştir
Laboratuvarın oluşturulması sırasında, laboratuvar sahibi **tüm sanal makineler için aynı parolayı kullan**seçeneğini etkinleştirebilir veya devre dışı bırakabilir. Bu seçenek etkinleştirilirse, öğrenciler parolayı sıfırlayamaz. Laboratuvardaki tüm VM 'Ler, eğitmen tarafından ayarlanan parolaya sahip olacaktır. 

Bu seçenek devre dışıysa, kullanıcıların VM 'yi ilk kez bağlamaya çalışırken bir parola ayarlaması gerekir. Öğrenciler ayrıca, bu makalenin son bölümünde gösterildiği gibi, parolayı daha sonra istediğiniz zaman sıfırlayabilirler. 

## <a name="reset-password-for-the-first-time"></a>Parolayı ilk kez Sıfırla
**Tüm sanal makineler için aynı parolayı kullan** seçeneği devre dışıysa, kullanıcılar (öğrenciler) **sanal makinelerim** sayfasında laboratuvar kutucuğunda **Bağlan** düğmesini seçtiğinde, Kullanıcı VM 'nin parolasını ayarlamak için aşağıdaki iletişim kutusunu görür: 

![Öğrenci parolasını sıfırlama](./media/how-to-set-virtual-machine-passwords/student-set-password.png)

## <a name="reset-password-later"></a>Parolayı daha sonra Sıfırla
Öğrenci ayrıca laboratuvar kutucuğunda taşma menüsüne (**dikey üç nokta**) tıklayarak ve **Parolayı Sıfırla**' yı seçerek parolayı ayarlayabilir. 

![Parolayı daha sonra Sıfırla](./media/how-to-set-virtual-machine-passwords/student-set-password-2.png)


## <a name="next-steps"></a>Sonraki adımlar
Bir laboratuvar sahibinin yapılandıray, diğer öğrenci kullanım seçenekleri hakkında bilgi edinmek için şu makaleye bakın: [öğrenci kullanımını yapılandırma](how-to-configure-student-usage.md).
