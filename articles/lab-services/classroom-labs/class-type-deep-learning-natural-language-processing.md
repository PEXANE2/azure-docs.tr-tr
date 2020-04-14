---
title: Azure Laboratuvar Hizmetlerini kullanarak derin öğrenmeye odaklanan bir laboratuvar ayarlayın | Microsoft Dokümanlar
description: Linux'ta kabuk komut dosyası yazmayı öğretmek için nasıl bir laboratuvar kurup kurup kurmayı öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/30/2019
ms.author: spelluru
ms.openlocfilehash: 889d0d1e98f5c9947588011774d02e54f05edca1
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81257768"
---
# <a name="set-up-a-lab-focused-on-deep-learning-in-natural-language-processing-using-azure-lab-services"></a>Azure Lab Hizmetlerini kullanarak doğal dil işlemede derin öğrenmeye odaklanan bir laboratuvar kurun
Bu makalede, Azure Laboratuvar Hizmetleri'ni kullanarak doğal dil işlemede (NLP) derin öğrenmeye odaklanmış bir laboratuvar nasıl kurulabileceğinizi gösterilmektedir. Doğal dil işleme (NLP), çeviri, konuşma tanıma ve diğer dil anlama özelliklerine sahip bilgisayarlara olanak tanıyan bir yapay zeka (AI) biçimidir.  

NLP sınıfına giren öğrenciler, yazılı insan dilini analiz etmek için kullanılan derin öğrenme modelleri geliştirmek için sinir ağı algoritmalarını nasıl uygulayacaklarını öğrenmek için bir Linux sanal makinesi (VM) alır. 

## <a name="lab-configuration"></a>Laboratuvar yapılandırması
Bu laboratuarı kurmak için başlamak için bir Azure aboneliğine ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. Azure aboneliğiniz olduktan sonra, Azure Laboratuvar Hizmetleri'nde yeni bir laboratuvar hesabı oluşturabilir veya varolan bir laboratuvar hesabını kullanabilirsiniz. Yeni bir laboratuvar hesabı oluşturmak için aşağıdaki [öğreticiye](tutorial-setup-lab-account.md)bakın: Laboratuvar Hesabı Kurma Öğreticisi.
 
Laboratuvar hesabını oluşturduktan sonra, laboratuvar hesabında aşağıdaki ayarları etkinleştirin: 

| Laboratuvar hesap ayarı | Yönergeler |
| ----------- | ------------ |  
| Pazar yeri görüntüleri | Laboratuvar hesabınızda kullanmak üzere Linux için Veri Bilimi Sanal Makine (Ubuntu) görüntüsünü etkinleştirin.  Talimatlar için aşağıdaki makaleye bakın: [Laboratuvar oluşturucuları tarafından kullanılabilen pazar yeri görüntülerini belirtin.](specify-marketplace-images.md) | 

Yeni bir laboratuvar oluşturmak ve aşağıdaki ayarları uygulamak için [bu öğreticiizleyin:](tutorial-setup-classroom-lab.md)

| Laboratuvar ayarları | Değer/talimatlar | 
| ------------ | ------------------ |
| Sanal makine (VM) boyutu | Küçük GPU (İşlem). Bu boyut, Yapay Zeka ve Derin Öğrenme gibi bilgi işlem yoğun ve ağ yoğun uygulamalar için en uygun uyrmuştur. |
| VM görüntü | [Linux (Ubuntu) için Veri Bilimi Sanal Makine.](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-dsvm.ubuntu-1804) Bu görüntü, makine öğrenimi ve veri bilimi için derin öğrenme çerçeveleri ve araçları sağlar. Bu resimdeki yüklü araçların tam listesini görüntülemek için aşağıdaki makaleye bakın: [DSVM'de neler yer alıyor?](../../machine-learning/data-science-virtual-machine/overview.md#whats-included-on-the-dsvm) |
| Uzak masaüstü bağlantısını etkinleştirme | Etkinleştirmek. <p>Bu ayarı etkinleştirmek, öğretmenlerin ve öğrencilerin Uzak Masaüstü (RDP) kullanarak Sanal Makinelerine (VM) bağlanmalarına olanak sağlar.</p><p>**Önemli**: RDP zaten Linux görüntü için Veri Bilimi Sanal Makine yüklü ve yapılandırılmıştır. Sonuç olarak, öğretmenler/öğrenciler herhangi bir ek adım olmadan RDP üzerinden VM'lere bağlanabilirler. Ayrıca, grafik masaüstüne bağlanmanız gerekiyorsa, bu görüntü de sanal makineye [x2Go server](https://wiki.x2go.org/doku.php/doc:newtox2go) yüklü. Öğrenciler x2Go istemcisini yerel makinelerine yüklemeli ve bağlanmak için istemciyi kullanmalıdır. Daha fazla bilgi için aşağıdaki kılavuzlara bakın: <ul><li>[Linux için Veri Bilimi Sanal Makine nasıl erişilir](../../machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine)</li><li>[RDP ve GUI paketlerini yüklemek için şablon VM'ye bağlanın](how-to-enable-remote-desktop-linux.md#connect-to-the-template-vm)</li></ul></p>   |

Linux görüntü için Veri Bilimi Sanal Makine gerekli derin öğrenme çerçeveleri ve araçları sınıf bu tür için gerekli sağlar. Sonuç olarak, şablon makinesi oluşturulduktan sonra, daha fazla özelleştirmeniz gerekmez. Öğrencilerin kullanması için yayınlanabilir. Şablonu laboratuvarda yayınlamak için şablon **sayfasındayayım** düğmesini seçin.  

## <a name="cost"></a>Maliyet
Bu laboratuvarın maliyetini tahmin etmek isterseniz, aşağıdaki örneği kullanabilirsiniz: 

20 saatlik planlanan ders süresi ve ödev ler için 10 saatlik kontenjanı olan 25 öğrenciden oluşan bir sınıf için, laboratuvar fiyatı - 25 öğrenci * (20 + 10) saat * 139 Laboratuvar Birimi * 0,01 USD saat = 1042,5 USD olacaktır

Fiyatlandırma hakkında daha fazla ayrıntı için Azure [Lab Hizmetleri Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/lab-services/)bakın.

## <a name="conclusion"></a>Sonuç
Bu makalede, doğal dil işleme sınıfı için bir laboratuvar oluşturmak için adımlar boyunca yürüdü. Diğer derin öğrenme sınıfları için benzer bir kurulum kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Sonraki adımlar herhangi bir laboratuvar kurmak için ortak:

- [Kullanıcı ekle](tutorial-setup-classroom-lab.md#add-users-to-the-lab)
- [Kota ayarlama](how-to-configure-student-usage.md#set-quotas-for-users)
- [Zamanlama ayarlama](tutorial-setup-classroom-lab.md#set-a-schedule-for-the-lab) 
- [Öğrencilere e-posta kayıt bağlantıları](how-to-configure-student-usage.md#send-invitations-to-users). 

