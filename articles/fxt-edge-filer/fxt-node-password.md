---
title: 'Öğretici: Donanımı başlatma - Azure FXT Edge Filer'
description: Azure FXT Kenar Filer düğümlerinde ilk parola nasıl ayarlayın?
author: ekpgh
ms.author: rohogue
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.openlocfilehash: 8cb5f639deb0630575c46db30efe70ad967324a8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75550900"
---
# <a name="tutorial-set-hardware-passwords"></a>Öğretici: Donanım parolalarını ayarlama

Azure FXT Kenar Filer düğümünü ilk kez açtığınızda, bir kök parola ayarlamanız gerekir. Donanım düğümleri varsayılan parolayla gönderilmez. 

Ağ bağlantı noktaları, parola ayarlanana ve kök kullanıcı girişi ne kadar devre dışı bırakılır.

Düğümü yükledikten ve kabloladıktan sonra, ancak kümeyi oluşturmaya çalışmadan önce bu adımı yapın. 

Bu öğretici, donanım düğümüne nasıl bağlanıştırılan ve parolayı nasıl ayarlayacaklarını açıklar. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 

> [!div class="checklist"]
> * Bir klavyeyi ve monitörü düğüme bağlayın ve
> * Bu düğümde iDRAC bağlantı noktası ve kök kullanıcı için parolaları ayarlama
> * Kök olarak oturum açma 

Kümenizde kullanacağınız her düğüm için bu adımları yineleyin. 

Bu eğitimin tamamlanması yaklaşık 15 dakika sürer. 

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce şu adımları tamamlayın: 

* Her Azure FXT Kenar Filer düğümlerini bir donanım rafına [yükleyin](fxt-install.md) ve [önceki öğreticide](fxt-network-power.md)açıklandığı gibi güç kabloları ve ağ erişimi takın. 
* Donanım düğümlerine takabileceğiniz USB bağlantılı bir klavye ve VGA'ya bağlı bir monitör bulun. (Parolayı ayarlamadan önce düğümün seri bağlantı noktası etkin değil.)

## <a name="connect-a-keyboard-and-monitor-to-the-node"></a>Bir klavye ve monitörü düğüme bağlayın

Bir monitörü ve klavyeyi Azure FXT Edge Filer düğümüne fiziksel olarak bağlayın. 

* Monitörü VGA bağlantı noktasına bağlayın.
* Klavyeyi USB bağlantı noktalarından birine bağlayın. 

Kasanın arka sideki bağlantı noktalarını bulmak için bu başvuru diyagramını kullanın. 

> [!NOTE]
> Seri bağlantı noktası, parola ayarlanana kadar etkin değil. 

![seri, VGA ve USB bağlantı noktalarını etiketlenmiş Azure FXT Edge Filer'ın arka diyagramı](media/fxt-back-serial-vga-usb.png)

Aynı çevre birimlerine birden fazla düğüm bağlamak istiyorsanız KVM anahtarı kullanabilirsiniz. 

Öndeki güç düğmesine basarak düğümüzerinde güç. 

![Azure FXT Edge Filer'ın ön diyagramı - yuvarlak güç düğmesi sağ üst köşeye yakın etiketlenmiştir](media/fxt-front-annotated.png)

## <a name="set-initial-passwords"></a>İlk parolaları ayarlama 

Azure FXT Kenar Filer düğümü önyükleme sırasında monitöre çeşitli iletiler yazdırır. Birkaç dakika sonra, şu gibi bir başlangıç kurulum ekranı gösterir:

```
------------------------------------------------------
        Microsoft FXT node initial setup
------------------------------------------------------
Password Setup
---------------
Enter a password to set iDRAC and temporary root password.
Minimum password length is 8.
Enter new password:
```

Girdiğiniz parola iki şey için kullanılır: 

* Bu Azure FXT Kenar Filer düğümü için geçici kök paroladır. 

  Bu düğümü kullanarak bir küme oluşturduğunuzda veya bu düğümü kümeye eklediğinizde bu parola değişecektir. Küme yönetimi parolası (kullanıcıyla ``admin``ilişkili) kümedeki tüm düğümlerin kök parolasidir.

* iDRAC/IPMI donanım yönetimi bağlantı noktasının uzun vadeli parolasI.

  Donanım sorununu gidermek için daha sonra IPMI ile oturum açmanız gerektiğinde parolayı hatırladığınızdan emin olun.

Parolayı girin ve onaylayın: 

```
Enter new password:**********
Re-enter password:**********
Loading AvereOS......
```

Parolayı girdikten sonra sistem önyüklemeye devam eder. Bittiğinde, bir ``login:`` istem verir. 

## <a name="sign-in-as-root"></a>Kök olarak oturum açma

Az önce ``root`` belirlediğiniz parolayla oturum açın. 

```
login: root
Password:**********
```

Kök olarak oturum açmadan sonra, ağ bağlantı noktaları etkinolur ve IP adresleri için DHCP sunucusuna başvurur. 

## <a name="next-steps"></a>Sonraki adımlar

Düğüm kümenin bir parçası olmaya hazırdır. Azure FXT Edge Filer kümesini oluşturmak için kullanabilirsiniz veya [varolan bir kümeye ekleyebilirsiniz.](fxt-add-nodes.md) 

> [!div class="nextstepaction"]
> [Küme oluşturma](fxt-cluster-create.md)
