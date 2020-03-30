---
title: Windows Azure VM MimarisiNin İş Akışı | Microsoft Dokümanlar
description: Bu makalede, bir hizmeti dağıttığınızda iş akışı işlemlerine genel bakış sağlanmış olur.
services: cloud-services
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: 9f2af8dd-2012-4b36-9dd5-19bf6a67e47d
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/08/2019
ms.author: kwill
ms.openlocfilehash: 5dd57a87658554bf59acf5cee1b6daf67b8692b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71162159"
---
#    <a name="workflow-of-windows-azure-classic-vm-architecture"></a>Windows Azure klasik VM Mimarisi iş akışı 
Bu makalede, sanal makine gibi bir Azure kaynağını dağıttığınızda veya güncellediğinizde oluşan iş akışı işlemlerine genel bir bakış sağlanmaktadır. 

> [!NOTE]
>Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: Resource Manager ve klasik. Bu makale klasik dağıtım modelini incelemektedir.

Aşağıdaki diyagram, Azure kaynaklarının mimarisini sunar.

![Azure iş akışı](./media/cloud-services-workflow-process/workflow.jpg)

## <a name="workflow-basics"></a>İş akışı temelleri
   
**A.** RDFE / FFE, kullanıcıdan kumaşa iletişim yoludur. RDFE (RedDog Front End), Visual Studio, Azure MMC vb. Yönetim Portalı ve Hizmet Yönetimi API'sinin ön ucu olan genel olarak açıklanmış API'dir.  Kullanıcıdan gelen tüm istekler RDFE'den geçer. FFE (Fabric Front End), RDFE'den gelen istekleri kumaş komutlarına çeviren katmandır. RDFE'den gelen tüm istekler ffe'den geçerek kumaş kontrolörlerine ulaşır.

**B**. Kumaş denetleyicisi, veri merkezindeki tüm kaynakların korunmasından ve izlenmesinden sorumludur. Konuk Işletim Sistemi sürümü, servis paketi, hizmet yapılandırması ve hizmet durumu gibi bilgileri gönderen kumaş işletim sistemi üzerindeki kumaş ana bilgisayar temsilcileriyle iletişim kurar.

**C**. Host Agent Host OS'de yaşar ve Konuk İşletim Sistemi'ni kurmakve rolü amaçlanan bir hedef durumuna doğru güncelleştirmek ve Konuk aracıyla kalp atışı kontrolleri yapmak için Konuk Aracı (WindowsAzureGuestAgent) ile iletişim kurmakla sorumludur. Ana Bilgisayar Aracısı 10 dakika boyunca sinyal yanıtı almazsa, Ana Bilgisayar Aracısı Konuk İşletim'i yeniden başlatır.

**C2**. WaAppAgent, WindowsAzureGuestAgent.exe'nin kurulumu, yapılandırılması ve güncellenmesiile sorumludur.

**D**.  WindowsAzureGuestAgent aşağıdakilerden sorumludur:

1. Güvenlik duvarı, ACD'ler, LocalStorage kaynakları, hizmet paketi ve yapılandırma sı dahil olmak üzere Konuk İşletim Sistemi'ni yapılandırma ve sertifikalar.
2. Rolün altında çalışacağı kullanıcı hesabı için SID'yi ayarlama.
3. Rol durumunu kumaşa iletmek.
4. WaHostBootstrapper başlangıç ve rolü hedef durumda olduğundan emin olmak için izleme.

**E**. WaHostBootstrapper sorumludur:

1. Rol yapılandırmasını okuma ve rolü yapılandırmak ve çalıştırmak için tüm uygun görevleri ve süreçleri başlatma.
2. Tüm alt süreçlerini izlemek.
3. Rol barındırma işleminde StatusCheck olayını yükseltme.

**F**. Rol Tam IIS web rolü olarak yapılandırılırsa IISConfigurator çalışır (SDK 1.2 HWC rolleri için çalışmaz). Bu sorumludur:

1. Standart IIS hizmetlerinin başlatılması
2. Web yapılandırmasında yeniden yazma modüllerini yapılandırma
3. Hizmet modelinde yapılandırılan rol için AppPool'u ayarlama
4. IIS günlüğe kaydetmeyi DiagnosticStore LocalStorage klasörüne işaret etmek için ayarlama
5. İzinleri ve ADA'ları yapılandırma
6. Web sitesi %roleroot%:\sitesroot\0'da bulunur ve AppPool IIS'yi çalıştırmak için bu konumu işaret eder. 

**G**. Başlangıç görevleri rol modeli tarafından tanımlanır ve WaHostBootstrapper tarafından başlatılır. Başlangıç görevleri arka planda eşit olarak çalışacak şekilde yapılandırılabilir ve ana bilgisayar bootstrapper başlangıç görevini başlatAcak ve ardından diğer başlangıç görevlerine devam edecek. Başlangıç görevleri, ana bilgisayar bootstrapper'ın başlangıç görevinin çalışmasını tamamlamasını ve bir sonraki başlangıç görevine devam etmeden önce başarı (0) çıkış kodunu döndürmesini bekleyeceği Basit (varsayılan) modda çalışacak şekilde de yapılandırılabilir.

**H**. Bu görevler SDK'nın bir parçasıdır ve rolün hizmet tanımındaki (.csdef) eklentiler olarak tanımlanır. Başlangıç görevlerine genişletildiğinde, **DiagnosticsAgent** ve **RemoteAccessAgent** her biri biri normal diğeri **/blockBaşlangıç** parametresi olan iki başlangıç görevi tanımlaması açısından benzersizdir. Normal başlangıç görevi, rolün kendisi çalışırken arka planda çalıştırılabilmek için arka plan başlangıç görevi olarak tanımlanır. **/blockStartup** başlangıç görevi, WaHostBootstrapper'ın devam etmeden önce çıkmasını beklemesi için Basit bir başlangıç görevi olarak tanımlanır. **/blockStartup** görevi, normal görevin başlatmayı bitirmesini bekler ve sonra çıkar ve ana bilgisayar bootstrapper'ın devam etmesine izin verir. Bu, rol süreçleri başlamadan önce tanılama ve RDP erişiminin yapılandırılabilmeleri için yapılır (bu /blockStartup görevi aracılığıyla yapılır). Bu aynı zamanda tanılama ve RDP erişiminin, ana bilgisayar bootstrapper başlangıç görevlerini tamamladıktan sonra çalışmaya devam etmesini sağlar (bu Normal görev üzerinden yapılır).

**Ben**. WaWorkerHost normal işçi rolleri için standart ana bilgisayar işlemidir. Bu ana bilgisayar işlemi, OnBaşlat ve Çalıştır gibi tüm rolün DL'lerini ve giriş noktası kodunu barındırıyor.

**J**. WaWebHost, SDK 1.2 uyumlu Barındırılabilir Web Core'u (HWC) kullanacak şekilde yapılandırılırsa, web rolleri için standart ana bilgisayar işlemidir. Roller, öğeyi hizmet tanımından (.csdef) kaldırarak HWC modunu etkinleştirebilir. Bu modda, tüm hizmet kodu ve DL'ler WaWebHost işleminden çalışır. IIS (w3wp) kullanılmaz ve IIS Yöneticisi'nde yapılandırılan AppPools yoktur çünkü IIS WaWebHost.exe içinde barındırılır.

**K**. WaIISHost Tam IIS kullanan web rolleri için rol giriş noktası kodu için ana işlemdir. Bu **işlem, RoleEntryPoint** sınıfını kullanan ve kodu bu sınıftan (OnStart, Run, OnStop) yürüten bulunan ilk DLL'yi yükler. RoleEntryPoint sınıfında oluşturulan tüm **RoleEnvironment** olayları (StatusCheck ve Changed gibi) bu işlemde yükseltilir.

**L**. W3WP, rolün Tam IIS'yi kullanacak şekilde yapılandırılması durumunda kullanılan standart IIS alt işlemidir. Bu, IISConfigurator tarafından yapılandırılan AppPool'u çalıştırZ. Burada oluşturulan tüm RoleEnvironment olayları (StatusCheck ve Changed gibi) bu işlemde yükseltilir. Her iki işlemdeki olaylara abone olursanız RoleEnvironment olaylarının her iki konumda da (WaIISHost ve w3wp.exe) ateş edeceğini unutmayın.

## <a name="workflow-processes"></a>İş akışı süreçleri

1. Kullanıcı, ".cspkg" ve ".cscfg" dosyalarını yüklemek, bir kaynağa durdurmasını veya yapılandırma değişikliği yapmasını söyleme gibi bir istekte bulunur. Bu, Azure portalı veya Visual Studio Publish özelliği gibi Hizmet Yönetimi API'sini kullanan bir araç aracılığıyla yapılabilir. Bu istek, abonelikle ilgili tüm işleri yapmak ve isteği FFE'ye iletmek için RDFE'ye gider. Bu iş akışı adımlarının geri kalanı yeni bir paket dağıtmak ve başlatmak için vardır.
2. FFE doğru makine havuzunu bulur (müşteri girişine göre, yakınlık grubu veya coğrafi konum artı makine kullanılabilirliği gibi kumaştan giriş gibi) ve makine havuzundaki ana kumaş denetleyicisi ile iletişim kurar.
3. Kumaş denetleyicisi kullanılabilir CPU çekirdekleri olan bir ana bilgisayar bulur (veya yeni bir ana bilgisayar alabilen). Servis paketi ve yapılandırma sıcağı sıcağı sıcağı sıcağı sıcağı sıcağı sıcağısı kopyalar ve kumaş denetleyicisi, paketi dağıtmak için ana bilgisayar işletim sistemi üzerindeki ana bilgisayar temsilcisiyle iletişim kurar (DIP'leri, bağlantı noktalarını, konuk işletim sistemi vb. yapılandırmayı).
4. Ana bilgisayar aracısı Konuk İşletim Sistemi'ni başlatır ve konuk aracıyla (WindowsAzureGuestAgent) iletişim kurar. Ev sahibi, rolün hedef durumuna doğru çalıştığından emin olmak için konuk oyuncuya kalp atışları gönderir.
5. WindowsAzureGuestAgent konuk işletim sistemi (güvenlik duvarı, ALA'lar, LocalStorage vb.) kurar, c:\Config için yeni bir XML yapılandırma dosyasını kopyalar ve ardından WaHostBootstrapper işlemini başlatır.
6. Tam IIS web rolleri için, WaHostBootstrapper IISConfigurator başlar ve IIS web rolü için herhangi bir mevcut AppPools silmek için söyler.
7. WaHostBootstrapper E:\RoleModel.xml'den **Başlangıç** görevlerini okur ve başlangıç görevlerini yürütmeye başlar. WaHostBootstrapper tüm Basit başlangıç görevleri bitmiş ve bir "başarı" mesajı döndü kadar bekler.
8. Tam IIS web rolleri için WaHostBootstrapper IISConfigurator IIS AppPool yapılandırmak `E:\Sitesroot\<index>`için `<index>` söyler ve puan site `<Sites>` , nerede hizmet için tanımlanan öğelerin sayısına 0 tabanlı dizin.
9. WaHostBootstrapper rol türüne bağlı olarak ana bilgisayar işlemini başlatacaktır:
    1. **İşçi Rolü**: WaWorkerHost.exe başladı. WaHostBootstrapper OnStart() yöntemini yürütür. Döndükten sonra, WaHostBootstrapper Run() yöntemini yürütmeye başlar ve aynı anda rolü Hazır olarak işaretler ve yük dengeleyici döndürmesine koyar (InputEndpoints tanımlanırsa). WaHostBootsrapper sonra rol durumunu kontrol bir döngü gider.
    1. **SDK 1.2 HWC Web Rolü**: WaWebHost başladı. WaHostBootstrapper OnStart() yöntemini yürütür. Döndükten sonra, WaHostBootstrapper Run() yöntemini yürütmeye başlar ve aynı anda Ready rolünü işaretler ve yük dengeleyici rotasyonuna koyar. WaWebHost bir ısınma isteği yayınlar (GET /do.rd_runtime_init). Tüm web istekleri WaWebHost.exe gönderilir. WaHostBootsrapper sonra rol durumunu kontrol bir döngü gider.
    1. **Tam IIS Web Rolü:** aIISHost başlatılır. WaHostBootstrapper OnStart() yöntemini yürütür. Döndükten sonra Çalıştır() yöntemini yürütmeye başlar ve aynı anda Ready rolünü işaretler ve yük dengeleyici döndürmesine koyar. WaHostBootsrapper sonra rol durumunu kontrol bir döngü gider.
10. Tam IIS web rolüne gelen web istekleri, IIS'nin W3WP işlemini başlatmasını ve istekle hizmet etmesini tetikler, aynı şirket içi IIS ortamında olduğu gibi.

## <a name="log-file-locations"></a>Dosya konumlarını günlüğe kaydedin

**WindowsAzureGuestAgent**

- C:\Logs\AppAgentRuntime.Log.  
Bu günlük, başlatmalar, duraklar ve yeni yapılandırmalar dahil olmak üzere hizmette yapılan değişiklikleri içerir. Hizmet değişmezse, bu günlük dosyasında büyük zaman boşlukları görmeyi bekleyebilirsiniz.
- C:\Logs\WaAppAgent.Log.  
Bu günlük durum güncelleştirmeleri ve sinyal bildirimleri içerir ve her 2-3 saniyede bir güncelleştirilir.  Bu günlük, örneğin durumunun tarihi bir görünümünü içerir ve örneğin Hazır durumunda olmadığını size söyler.
 
**WaHostBootstrapper**

`C:\Resources\Directory\<deploymentID>.<role>.DiagnosticStore\WaHostBootstrapper.log`
 
**WaWebHost**

`C:\Resources\Directory\<guid>.<role>\WaWebHost.log`
 
**WaIISHost**

`C:\Resources\Directory\<deploymentID>.<role>\WaIISHost.log`
 
**IISConfigurator**

`C:\Resources\Directory\<deploymentID>.<role>\IISConfigurator.log`
 
**IIS günlükleri**

`C:\Resources\Directory\<guid>.<role>.DiagnosticStore\LogFiles\W3SVC1`
 
**Windows Olay günlükleri**

`D:\Windows\System32\Winevt\Logs`
 



