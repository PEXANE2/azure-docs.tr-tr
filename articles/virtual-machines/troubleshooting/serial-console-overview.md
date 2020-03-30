---
title: Azure Seri Konsol | Microsoft Dokümanlar
description: Azure Seri Konsolu, SSH veya RDP kullanılamadığında VM'nize bağlanmanızı sağlar.
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: 779bb88d15ea6c52f4399f17223b89916e22653d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267020"
---
# <a name="azure-serial-console"></a>Azure Seri Konsolu

Azure portalındaki Seri Konsol, sanal makineler (VM'ler) ve sanal makine ölçeği için Linux veya Windows çalışan örnekler için metin tabanlı bir konsola erişim sağlar. Bu seri bağlantısı, VM'nin ttyS0 veya COM1 seri bağlantı noktasına veya sanal makine ölçeği kümesi örneğine bağlanarak ağ veya işletim sistemi durumundan bağımsız erişim sağlar. Seri konsola yalnızca Azure portalı kullanılarak erişilebilir ve yalnızca Katılımcı veya VM veya sanal makine ölçeği kümesine erişim rolü olan veya daha yüksek olan kullanıcılar için izin verilir.

Seri Konsol, VM'ler ve sanal makine ölçeği seti örnekleri için aynı şekilde çalışır. Bu dokümanda, aksi belirtilmedikçe, VM'lere yapılan tüm sözlerle sanal makine ölçeği seti örnekleri dolaylı olarak yer alacaktır.

> [!NOTE]
> Seri Konsol genellikle genel Azure bölgelerinde ve Azure Resmi'nde genel önizlemede kullanılabilir. Azure Çin bulutunda henüz kullanıma sunulmadı.

## <a name="prerequisites-to-access-the-azure-serial-console"></a>Azure Seri Konsoluna erişmek için ön koşullar
VM veya sanal makine ölçeği seti örneğiniz deki Seri Konsol'a erişmek için aşağıdakilere ihtiyacınız olacaktır:

- VM için önyükleme tanılama özelliği etkinleştirilmelidir
- Parola kimlik doğrulaması kullanan bir kullanıcı hesabı VM içinde bulunmalıdır. VM erişim uzantısısıfırlama [parola](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) işlevi ile parola tabanlı bir kullanıcı oluşturabilirsiniz. Destek + sorun **giderme** bölümünden **parolayı sıfırla'yı** seçin.
- Seri Konsola erişen Azure hesabı, hem VM hem de [önyükleme tanılama](boot-diagnostics.md) depolama hesabı için [Sanal Makine Katılımcısı rolüne](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) sahip olmalıdır

> [!NOTE]
> Klasik dağıtımlar desteklenmez. VM veya sanal makine ölçeği set örneğiniz Azure Kaynak Yöneticisi dağıtım modelini kullanmalıdır.

## <a name="get-started-with-the-serial-console"></a>Seri Konsol ile başlayın
VM'ler için Seri Konsol'a ve sanal makine ölçeği kümesine yalnızca Azure portalı üzerinden erişilebilir:

### <a name="serial-console-for-virtual-machines"></a>Sanal Makineler için Seri Konsol
VM'ler için Seri Konsol, Azure portalındaki **Destek + sorun giderme** **bölümündeki Seri konsola** tıkladığımız kadar basittir.
  1. Azure [portalını](https://portal.azure.com)açın.

  1. Tüm **kaynaklara** gidin ve bir Sanal Makine seçin. VM için genel bakış sayfası açılır.

  1. Destek + **sorun giderme** bölümüne aşağı kaydırın ve **Seri konsolu**seçin. Seri konsolu ile yeni bir bölme açılır ve bağlantıyı başlatır.

     ![Linux Seri Konsol penceresi](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### <a name="serial-console-for-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Setleri için Seri Konsol
Seri Konsol, ölçek kümesi içinde her örnekte erişilebilir sanal makine ölçek kümeleri için kullanılabilir. **Seri konsol** düğmesini görmeden önce sanal makine ölçeği kümesinin tek tek örneğine gitseniz gerekir. Sanal makine ölçek kümenizde önyükleme tanılama etkin değilse, önyükleme tanılamayı etkinleştirmek için sanal makine ölçeği set modelinizi güncelleştirip seri konsola erişmek için tüm örnekleri yeni modele yükselttiğinizden emin olun.
  1. Azure [portalını](https://portal.azure.com)açın.

  1. Tüm **kaynaklara** gidin ve sanal makine ölçeği kümesini seçin. Sanal makine ölçeği kümesi için genel bakış sayfası açılır.

  1. **Örneklere** Git

  1. Sanal makine ölçeği kümesi örneği seçin

  1. Destek **+ sorun giderme** bölümünden **Seri konsolu'nu**seçin. Seri konsolu ile yeni bir bölme açılır ve bağlantıyı başlatır.

     ![Linux sanal makine ölçek seti Seri Konsol](./media/virtual-machines-serial-console/vmss-start-console.gif)

## <a name="advanced-uses-for-serial-console"></a>Seri Konsol için gelişmiş kullanımlar
VM konsol erişiminin yanı sıra, Azure Seri Konsolu'nu aşağıdakiler için de kullanabilirsiniz:
* [VM'inize sistem istek komutu](./serial-console-nmi-sysrq.md) gönderme
* [VM cihazınıza maskelenebilir olmayan](./serial-console-nmi-sysrq.md) bir kesme gönderme
* [VM'nizi](./serial-console-power-options.md) zarif bir şekilde yeniden başlatıyor veya zorla güç lendirmek


## <a name="next-steps"></a>Sonraki adımlar
Kenar çubuğunda ek Seri Konsol belgeleri mevcuttur.
- [Linux VM'ler için Seri Konsol için](./serial-console-linux.md)daha fazla bilgi mevcuttur.
- [Windows VM'ler için Seri Konsol için](./serial-console-windows.md)daha fazla bilgi mevcuttur.
