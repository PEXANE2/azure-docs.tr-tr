---
title: Python ve Jupyter Notebook'larla veri bilimi öğretmek için bir laboratuvar kurmak | Microsoft Dokümanlar
description: Python ve Jupyter Notebook'ları kullanarak veri bilimi öğretmek için nasıl bir laboratuvar kurup kurabilirsiniz öğrenin.
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
ms.openlocfilehash: dfb133f9aa3dd9b76f8b4ea4c6188cfaf9a67b75
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444120"
---
# <a name="set-up-a-lab-to-teach-data-science-with-python-and-jupyter-notebooks"></a>Python ve Jupyter Notebook'larla veri bilimi öğretmek için bir laboratuvar ayarlayın

Bu makalede, öğrencilere [Jupyter Notebook'ları](http://jupyter-notebook.readthedocs.io)nasıl kullanacaklarını öğretmek için gereken araçlarla Laboratuvar Hizmetleri'nde bir şablon makinesinin nasıl kurulabildiğini özetlenmiştir.  Jupyter Notebooks, zengin metin ve çalıştırılabilir [Python](https://www.python.org/) kaynak kodunu not defteri adı verilen tek bir tuvalde kolayca birleştirmenizi sağlayan açık kaynaklı bir projedir.  Not defterini çalıştırmak, girdi ve çıktıların doğrusal bir kaydıyla sonuçlanır.  Bu çıktılar metin, bilgi tabloları, dağılım çizimleri ve daha fazlasını içerebilir.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

Bu laboratuarı kurmak için başlamak için bir Azure aboneliğine ve laboratuvar hesabına ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure aboneliği ni aldıktan sonra Azure Lab Hizmetleri'nde yeni bir laboratuvar hesabı oluşturabilirsiniz. Yeni bir laboratuvar hesabı oluşturma hakkında daha fazla bilgi için, [bir laboratuvar hesabı kurma öğreticisine](tutorial-setup-lab-account.md)bakın.  Varolan bir laboratuvar hesabını da kullanabilirsiniz.

### <a name="lab-account-settings"></a>Laboratuvar Hesap Ayarları

Laboratuvar hesabı için aşağıdaki tabloda açıklanan ayarları etkinleştirin. Pazar yeri görüntülerinin nasıl etkinleştirilen hakkında daha fazla bilgi için, [laboratuvar oluşturucuları tarafından kullanılabilen Market görüntülerini belirtin'e](specify-marketplace-images.md)bakın.

| Laboratuvar hesap ayarı | Yönergeler |
| ------------------- | ------------ |
| Pazar yeri görüntüsü | Laboratuvar hesabınızda kullanmak üzere [Veri Bilimi Sanal Makine - Windows 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) görüntüsünü etkinleştirin. |

>[!TIP]
>Bu makalede, Windows Server işletim sistemini kullanan bir şablon makinesi yapılandırma üzerinde durulacak.  Linux [için Veri Bilimi Sanal Makine (CentOS)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm) veya Azure Marketi'nden Linux için Veri [Bilimi Sanal Makine (Ubuntu)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.linux-data-science-vm-ubuntu) görüntülerini kullanarak Python ve Jupyter Notebook'larla veri bilimi sınıfı da oluşturmak mümkündür.

### <a name="lab-settings"></a>Laboratuvar ayarları

Sınıf laboratuarı kurarken aşağıdaki tablodaki ayarları kullanın.  Sınıf laboratuarı oluşturmak için daha fazla bilgi için [bkz.](tutorial-setup-classroom-lab.md)

| Laboratuvar ayarları | Değer/talimatlar |
| ------------ | ------------------ |
|Sanal Makine Boyutu| Küçük GPU (İşlem). Bu boyut, Yapay Zeka ve Derin Öğrenme gibi bilgi işlem yoğun ve ağ yoğun uygulamalar için en uygun uyrmuştur. |
|Sanal Makine Görüntüsü| Veri Bilimi Sanal Makine - Windows 2016|

## <a name="template-machine"></a>Şablon makinesi

[Veri Bilimi Sanal Makine - Windows 2016](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.dsvm-windows) görüntü sınıf bu tür için gerekli derin öğrenme çerçeveleri ve araçları sağlar.  Görüntü Jupyter Notebooks ve Visual Studio Code içerir.  [Jupyter Notebooks,](http://jupyter-notebook.readthedocs.io) veri bilimcilerin ham veri almalarına, hesaplamaları yürütmelerine ve sonuçları aynı ortamda görmelerine olanak tanıyan bir web uygulamasıdır.  Şablon makinemiz için web uygulaması yerel olarak çalışacaktır.  [Visual Studio Code,](https://code.visualstudio.com/) bir dizüstü bilgisayar yazarken ve test ederken zengin bir etkileşimli deneyim sağlayan bir IDE'dir.  Daha fazla bilgi için Visual [Studio Code'da Jupyter Notebook'larla çalışma](https://code.visualstudio.com/docs/python/jupyter-support)bilgisine bakın.

Sınıfı ayarlamak için kalan görev yerel not defterleri sağlamaktır.  Azure Machine Learning örneklerini nasıl kullanacağınız için, [Jupyter Notebook'larla ortamı nasıl yapılandıracağınıza](../../machine-learning/how-to-configure-environment.md#jupyter)bakın.  Ayrıca şablon makinesinde kendi not defterlerinizi de sağlayabilirsiniz.  Şablon yayımlandığında defterler tüm öğrenci makinelerine kopyalanır.

## <a name="cost-estimate"></a>Maliyet tahmini

Bu sınıf için olası bir maliyet tahminini karşılayalım.  25 öğrencilik bir sınıf kullanacağız.  20 saatlik ders saati var.  Ayrıca, her öğrenci planlanan ders saati dışında ödev veya ödevler için 10 saat kontenjan alır.  Seçtiğimiz sanal makine boyutu 139 laboratuvar birimi olan küçük GPU (bilgi işlem) idi.

Aşağıda, bu sınıf için olası bir maliyet tahmini örneği verilmiştir:

25 \* öğrenci (20 planlanan saat + \* 10 kontenjan saati) 139 laboratuvar ünitesi \* 0,01 SAAT başına USD = 1042,5 USD

Fiyatlandırma hakkında daha fazla ayrıntı için Azure [Lab Hizmetleri Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/lab-services/)bakın.

## <a name="conclusion"></a>Sonuç

Bu makalede, bir Jupyter Notebooks sınıf için bir laboratuvar oluşturmak için adımları ile yürüdü. Diğer makine öğrenimi sınıfları için de benzer bir kurulum kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımlar herhangi bir laboratuvar kurmak için ortak.

- [Şablon oluşturma ve yönetme](how-to-create-manage-template.md)
- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab)
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users)
