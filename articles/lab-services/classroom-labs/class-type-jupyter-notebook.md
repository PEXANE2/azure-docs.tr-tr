---
title: Python ve jupi Not defterleri ile veri bilimi öğretmek için laboratuvar kurma | Microsoft Docs
description: Python ve Jupyıter not defterlerini kullanarak veri bilimi öğretmek için laboratuvar ayarlamayı öğrenin.
services: lab-services
documentationcenter: na
author: emaher
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/19/2019
ms.author: enewman
ms.openlocfilehash: 222f5647248d27c3bdfabd0feaeb66dd9f543652
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81257734"
---
# <a name="set-up-a-lab-to-teach-data-science-with-python-and-jupyter-notebooks"></a>Python ve Jupyıter Not defterleri ile veri bilimi öğretmek için laboratuvar ayarlama

Bu makalede, öğrencilerin [Jupyıter not defterlerini](http://jupyter-notebook.readthedocs.io)nasıl kullanacağınızı öğretmek için gereken araçlarla laboratuvar hizmetlerinde bir şablon makinesinin nasıl ayarlanacağı özetlenmektedir.  Jupyter Not defterleri, zengin metin ve yürütülebilir [Python](https://www.python.org/) kaynak kodunu not defteri adlı tek bir tuval üzerinde kolayca birleştirebilmenizi sağlayan açık kaynaklı bir projem.  Bir not defteri çalıştırmak, giriş ve çıkışları doğrusal bir kaydıyla sonuçlanır.  Bu çıktılar metin, bilgi tabloları, dağılım çizimleri ve daha fazlasını içerebilir.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu Laboratuvarı ayarlamak için, başlamak üzere bir Azure aboneliğine ve laboratuvar hesabına sahip olmanız gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. Bir Azure aboneliği aldıktan sonra, Azure Lab Services yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni laboratuvar hesabı oluşturma hakkında daha fazla bilgi için bkz. [Laboratuvar hesabı kurmak için öğretici](tutorial-setup-lab-account.md).  Ayrıca var olan bir laboratuvar hesabı da kullanabilirsiniz.

### <a name="lab-account-settings"></a>Laboratuvar hesabı ayarları

Laboratuvar hesabı için aşağıdaki tabloda açıklanan ayarları etkinleştirin. Market görüntülerinin nasıl etkinleştirileceği hakkında daha fazla bilgi için bkz. [Laboratuvar oluşturucuları için kullanılabilen Market görüntülerini belirtme](specify-marketplace-images.md).

| Laboratuvar hesabı ayarı | Yönergeler |
| ------------------- | ------------ |
| Market görüntüsü | Laboratuvar hesabınızda kullanılmak üzere [veri bilimi sanal makinesi-Windows 2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-win-2019) görüntüsünü etkinleştirin. |

>[!TIP]
>Bu makale, Windows Server işletim sistemini kullanan bir şablon makinesini yapılandırmaya odaklanacaktır.  Ayrıca, Azure Marketi 'nden [Linux (Ubuntu) görüntüleri için veri bilimi sanal makinesi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804) kullanan Python ve jupi Notebook ile veri bilimi sınıfı ayarlamak da mümkündür.

### <a name="lab-settings"></a>Laboratuvar ayarları

Bir sınıf Laboratuvarı ayarlarken aşağıdaki tablodaki ayarları kullanın.  Sınıf Laboratuvarı oluşturma hakkında daha fazla bilgi için bkz. [bir derslik Laboratuvarı ayarlama öğreticisi](tutorial-setup-classroom-lab.md).

| Laboratuvar ayarları | Değer/yönergeler |
| ------------ | ------------------ |
|Sanal makine boyutu| Küçük GPU (Işlem). Bu boyut, yapay zeka ve derin öğrenme gibi yoğun işlem yoğunluğu ve yoğun ağ kullanımı gerektiren uygulamalar için idealdir. |
|Sanal makine görüntüsü| Veri Bilimi Sanal Makinesi-Windows 2016|

## <a name="template-machine"></a>Şablon makinesi

[Veri bilimi sanal makinesi-Windows 2019](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-win-2019) görüntüsü, bu tür bir sınıf için gereken ayrıntılı öğrenme çerçevelerini ve araçları sağlar.  Görüntü Jupyıter not defterlerini ve Visual Studio Code içerir.  [Jupi Not defterleri](http://jupyter-notebook.readthedocs.io) , veri bilimcilerinin ham veri almasına, hesaplamalar çalıştırmasına ve sonuçların tümünü aynı ortamda görüntülemesine olanak tanıyan bir Web uygulamasıdır.  Şablon makinemiz için Web uygulaması yerel olarak çalışır.  [Visual Studio Code](https://code.visualstudio.com/) , bir not defteri yazarken ve test edilirken zengin bir etkileşimli deneyim sağlayan bir IDE 'dir.  Daha fazla bilgi için bkz. [Visual Studio Code jupi Not defterleri Ile çalışma](https://code.visualstudio.com/docs/python/jupyter-support).

Sınıfını ayarlamak için kalan görev, yerel Not defterleri sağlamaktır.  Azure Machine Learning örneklerini kullanma hakkında yönergeler için bkz. [jupi Not defterleri ile ortam yapılandırma](../../machine-learning/how-to-configure-environment.md#jupyter).  Ayrıca, şablon makinesinde kendi not defterlerinizi de sağlayabilirsiniz.  Şablon yayımlandığında Not defterleri tüm öğrenci makinelerine kopyalanır.

## <a name="cost-estimate"></a>Maliyet tahmini

Bu sınıf için olası bir maliyet tahminini ele alalım.  25 öğrencilerden oluşan bir sınıf kullanacağız.  20 saatlik zamanlanan sınıf zamanı vardır.  Ayrıca, her öğrenci, zamanlanan sınıf zamanı dışında ev ödevleri veya atamalar için 10 saatlik kota alır.  Seçtiğimiz sanal makine boyutu, 139 laboratuvar birimi olan küçük GPU (işlem) idi.

Bu sınıf için olası bir maliyet tahmini örneği aşağıda verilmiştir:

25 öğrenci \* (20 zamanlanan saat + 10 kota saati) \* 139 laboratuvar birimi \* 0,01 saat başına ABD Doları = 1042,5 ABD Doları

Fiyatlandırma hakkında daha fazla bilgi için bkz. [Azure Lab Services fiyatlandırması](https://azure.microsoft.com/pricing/details/lab-services/).

## <a name="conclusion"></a>Sonuç

Bu makalede, bir Jupyıter Not defteri sınıfı için Laboratuvar oluşturma adımlarını ele aldık. Diğer makine öğrenimi sınıfları için benzer bir kurulum kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar, herhangi bir laboratuvarı ayarlamak için ortaktır.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrenciler için e-posta kaydı bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
