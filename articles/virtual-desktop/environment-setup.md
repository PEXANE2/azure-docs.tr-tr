---
title: Windows Sanal Masaüstü ortamı - Azure
description: Windows Sanal Masaüstü ortamının temel öğeleri.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 33d058f028b7032f296ffcf82f0e5fe2c993e6fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127907"
---
# <a name="windows-virtual-desktop-environment"></a>Windows Sanal Masaüstü ortamı

Windows Sanal Masaüstü, kullanıcıların sanallaştırılmış masaüstü bilgisayarlarına ve RemoteApps'a kolay ve güvenli erişim sağlayan bir hizmettir. Bu konu, Windows Sanal Masaüstü ortamının genel yapısı hakkında biraz daha fazla bilgi verecektir.

## <a name="tenants"></a>Kiracılar

Windows Sanal Masaüstü kiracı, Windows Sanal Masaüstü ortamınızı yönetmek için birincil arabirimdir. Her Windows Sanal Masaüstü kiracısı, ortamda oturum açacak kullanıcıları içeren Azure Etkin Dizini ile ilişkilendirilmelidir. Windows Sanal Masaüstü kiracısından, kullanıcılarınızın iş yüklerini çalıştırmak için ana bilgisayar havuzları oluşturmaya başlayabilirsiniz.

## <a name="host-pools"></a>Ana havuzlar

Ana bilgisayar havuzu, Windows Sanal Masaüstü aracısını çalıştırdığınızda oturum ana bilgisayarları olarak Windows Sanal Masaüstü'ne kaydolan Azure sanal makinelerinin bir koleksiyonudur. Bir ana bilgisayar havuzundaki tüm oturum ana bilgisayar sanal makineleri tutarlı bir kullanıcı deneyimi için aynı görüntüden kaynaklanmalıdır.

Ana bilgisayar havuzu iki türden biri olabilir:

- Her oturum ana bilgisayarının tek tek kullanıcılara atandığı kişisel.
- Oturum ana bilgisayarlarının, ana bilgisayar havuzundaki bir uygulama grubuna yetkili herhangi bir kullanıcıdan bağlantı kabul edebileceği havuzlu.

Yük dengeleme davranışını, her oturum ana bilgisayarının kaç oturum alabileceğini ve kullanıcının Windows Sanal Masaüstü oturumlarında oturum açarken ana bilgisayardaki oturum ana bilgisayarlarına neler yapabileceğini değiştirmek için ana bilgisayar havuzunda ek özellikler ayarlayabilirsiniz. Uygulama grupları aracılığıyla kullanıcılara yayınlanan kaynakları kontrol elersiniz.

## <a name="app-groups"></a>Uygulama grupları

Uygulama grubu, ana bilgisayar havuzundaki oturum ana bilgisayarlarına yüklenen uygulamaların mantıksal bir gruplandırmasıdır. Bir uygulama grubu iki türden biri olabilir:

- Kullanıcıların tek tek seçtiğiniz ve uygulama grubuna yayınladığınız RemoteApps'a eriştiği RemoteApp
- Kullanıcıların tam masaüstüne eriştiği masaüstü

Varsayılan olarak, bir ana bilgisayar havuzu oluşturduğunuzda otomatik olarak bir masaüstü uygulama grubu ("Masaüstü Uygulama Grubu" olarak adlandırılır) oluşturulur. Bu uygulama grubunu istediğiniz zaman kaldırabilirsiniz. Ancak, masaüstü uygulama grubu varken ana bilgisayar havuzunda başka bir masaüstü uygulama grubu oluşturamazsınız. RemoteApps'ı yayınlamak için bir RemoteApp uygulama grubu oluşturmanız gerekir. Farklı çalışan senaryolarına uyum sağlamak için birden çok RemoteApp uygulama grubu oluşturabilirsiniz. Farklı RemoteApp uygulama grupları da çakışan RemoteApps içerebilir.

Kaynakları kullanıcılara yayınlamak için bunları uygulama gruplarına atamanız gerekir. Kullanıcıları uygulama gruplarına atarken aşağıdaki leri göz önünde bulundurun:

- Bir kullanıcı aynı ana bilgisayar havuzunda hem masaüstü uygulama grubuna hem de RemoteApp uygulama grubuna atanamaz.
- Bir kullanıcı aynı ana bilgisayar havuzundaki birden çok uygulama grubuna atanabilir ve özet akışları her iki uygulama grubundan bir birikim olacaktır.

## <a name="tenant-groups"></a>Kiracı grupları

Windows Sanal Masaüstü'nde, Windows Sanal Masaüstü kiracıkurulum ve yapılandırma çoğu nerede olur. Windows Sanal Masaüstü kiracısı ana bilgisayar havuzları, uygulama grupları ve uygulama grubu kullanıcı atamalarını içerir. Ancak, özellikle bulut hizmeti sağlayıcısı (CSP) veya barındırma ortağıysanız, aynı anda birden çok Windows Sanal Masaüstü kiracısını yönetmeniz gereken belirli durumlar olabilir. Bu gibi durumlarda, müşterilerin Windows Sanal Masaüstü kiracılarının her birini yerleştirmek ve erişimi merkezi olarak yönetmek için özel bir Windows Sanal Masaüstü kiracı grubu kullanabilirsiniz. Ancak, yalnızca tek bir Windows Sanal Masaüstü kiracısını yönetiyorsanız, kiracı grubu kavramı geçerli değildir ve varsayılan kiracı grubunda bulunan kiracınızı çalıştırmaya ve yönetmeye devam edebilirsiniz.

## <a name="end-users"></a>Son kullanıcılar

Kullanıcıları uygulama gruplarına atadıktan sonra, Windows Sanal Masaüstü istemcilerinden herhangi biriyle Windows Sanal Masaüstü dağıtımına bağlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Windows [Sanal Masaüstünde Temsilci Erişimi'nde](delegated-access-virtual-desktop.md)kullanıcılara nasıl rol atabildiğiniz ve devredilen erişim hakkında daha fazla bilgi edinin.

Windows Sanal Masaüstü kiracınızı nasıl ayarlayabilirsiniz öğrenmek için windows [sanal masaüstünde kiracı oluşturma](tenant-setup-azure-active-directory.md)'ya bakın.

Windows Sanal Masaüstü'ne nasıl bağlanılaceksiniz öğrenmek için aşağıdaki makalelerden birine bakın:

- [Windows 10 veya Windows 7'den bağlanma](connect-windows-7-and-10.md)
- [Web tarayıcısından bağlanma](connect-web.md)
