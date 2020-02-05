---
title: AVS özel bulut ayrıcalıklarını ilerletin-AVS 'ye göre Azure VMware çözümü
description: VCenter 'daki yönetim işlevleri için AVS özel bulutunuzda ayrıcalıkların nasıl ilerletiloluşturulacağını açıklar
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 211960af359e19f93afef58162c5b09ae1d9b23f
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025325"
---
# <a name="escalate-avs-private-cloud-vcenter-privileges-from-the-avs-portal"></a>AVS portalından AVS özel bulut vCenter ayrıcalıklarını ilerlet

AVS özel bulut vCenter 'nize yönetici erişimi için, AVS ayrıcalıklarınızı geçici olarak ilerletebilirsiniz. Yükseltilmiş ayrıcalıkları kullanarak, VMware çözümlerini yükleyebilir, kimlik kaynakları ekleyebilir ve kullanıcıları yönetebilirsiniz.

Yeni kullanıcılar vCenter SSO etki alanında oluşturulabilir ve vCenter erişimi verilebilir. Yeni kullanıcılar oluşturduğunuzda, vCenter 'a erişim için onları AVS yerleşik gruplarına ekleyin. Daha fazla bilgi için bkz. [AVS özel bulut izin modeli VMware vCenter](https://docs.azure.cloudsimple.com/learn-private-cloud-permissions/).

> [!CAUTION]
> Yönetim bileşenleri için herhangi bir yapılandırma değişikliği yapmayın. İlerletilen ayrıcalıklı durum sırasında gerçekleştirilen eylemler sisteminizi olumsuz etkileyebilir veya sisteminizin kullanılamaz hale gelmesine neden olabilir.

## <a name="sign-in-to-azure"></a>Azure'da oturum açın

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="escalate-privileges"></a>Ayrıcalıkları yükseltme

1. [AVS portalına](access-cloudsimple-portal.md)erişin.

2. **Kaynaklar** sayfasını açın, ayrıcalıklarına ILETMEK Istediğiniz AVS özel bulutunu seçin.

3. Özet sayfasının alt kısmındaki **vSphere ayrıcalıklarını Değiştir**altında, **Yükselt**' e tıklayın.

    ![VSphere ayrıcalığını değiştirme](media/escalate-private-cloud-privilege.png)

4. VSphere Kullanıcı türünü seçin. Yalnızca yerel kullanıcı `CloudOwner@cloudsimple.local` ilerletilenebilir.

5. Açılan listeden ilerleme zaman aralığını seçin. Görevi tamamlamanıza olanak sağlayacak en kısa süreyi seçin.

6. Riskleri anladığınızdan emin olmak için onay kutusunu seçin.

    ![Ayrıcalığı ilerlet iletişim kutusu](media/escalate-private-cloud-privilege-dialog.png)

7. **Tamam**’a tıklayın.

8. Yükseltme işlemi birkaç dakika sürebilir. İşlem tamamlandığında **Tamam**’a tıklayın.

Ayrıcalık yükseltme başlar ve seçilen aralığın sonuna kadar sürer. Yönetim görevlerini yapmak için AVS özel bulutu vCenter oturumunuzu açabilirsiniz.

> [!IMPORTANT]
> Yalnızca bir Kullanıcı ilerletilen ayrıcalıklara sahip olabilir. Başka bir kullanıcının ayrıcalıklarından daha fazla kullanıcı için kullanıcı ayrıcalıklarını geçersiz bir şekilde ilerletebilirsiniz.

> [!CAUTION]
> Yeni kullanıcılar yalnızca *bulut sahibi grubu*, *bulut-genel-küme-yönetici-grubu*, *bulut-genel-depolama-yönetici-grubu*, bulut-genel- *Ağ-Yönetici-Grup* veya *bulut-genel-VM-yönetici grubu*için eklenmelidir.  *Yöneticiler* grubuna eklenen kullanıcılar otomatik olarak kaldırılacaktır.  Yalnızca hizmet hesaplarının *Yöneticiler* grubuna eklenmesi gerekir ve hizmet hesapları vSphere Web Kullanıcı arabiriminde oturum açmak için kullanılmamalıdır.

## <a name="extend-privilege-escalation"></a>Ayrıcalık yükseltmeyi uzat

Görevlerinizi tamamlaması için ek süre gerekliyse, ayrıcalık yükseltme dönemini genişletebilirsiniz. Yönetim görevlerini tamamlamanıza izin veren ek ilerleme zaman aralığını seçin.

1. AVS portalındaki **AVS özel bulutları** > **kaynaklarda** , ayrıcalık yükseltme 'Yi genişletmek Istediğiniz AVS özel bulutunu seçin.

2. Özet sekmesinin altındaki **ayrıcalık yükseltmeyi Genişlet**' e tıklayın.

    ![Ayrıcalık yükseltmeyi uzat](media/de-escalate-private-cloud-privilege.png)

3. Açılan listeden bir ilerleme zaman aralığı seçin. Yeni ilerletme bitiş saatini gözden geçirin.

4. Aralığı genişletmek için **Kaydet** ' e tıklayın.

## <a name="de-escalate-privileges"></a>Ön yükseltme ayrıcalıkları

Yönetim görevleriniz tamamlandıktan sonra, ayrıcalıklarınızı geçersiz bir şekilde ilerletin. 

1. AVS portalındaki **AVS özel bulutlarını** > **kaynaklarda** , ayrıcalıklarını yükseltmek Istediğiniz AVS özel bulutunu seçin.

2. **Ön yükseltme**' ye tıklayın.

3. **Tamam**’a tıklayın.

> [!IMPORTANT]
> Herhangi bir hatayı önlemek için vCenter oturumunu kapatın ve yükseltme ayrıcalıklarından sonra tekrar oturum açın.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanılacak vCenter Identity kaynaklarını ayarlayın Active Directory](https://docs.azure.cloudsimple.com/set-vcenter-identity/)
* Yedekleme [iş yükü sanal makinelerini](https://docs.azure.cloudsimple.com/backup-workloads-veeam/) yedekleme çözümünü yükler