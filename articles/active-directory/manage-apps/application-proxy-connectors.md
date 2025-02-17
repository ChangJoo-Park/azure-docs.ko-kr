---
title: Azure AD 애플리케이션 프록시 커넥터 이해 | Microsoft Docs
description: Azure AD 애플리케이션 프록시 커넥터에 대한 기본 사항을 제공합니다.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 51ad6ea2abcc18b985e9c45fbfb1ffba98fb2c1f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66113093"
---
# <a name="understand-azure-ad-application-proxy-connectors"></a>Azure AD 애플리케이션 프록시 커넥터 이해

커넥터는 Azure AD 애플리케이션 프록시를 가능하게 만듭니다. 커넥터는 간단하고 배포 및 유지 관리가 쉬우며 기능이 매우 강력합니다. 이 문서에서는 커넥터가 무엇인지, 어떤 방식으로 작동하는지, 어떻게 하면 배포를 최적화할 수 있는지 살펴봅니다. 

## <a name="what-is-an-application-proxy-connector"></a>애플리케이션 프록시 커넥터란?

커넥터는 온-프레미스에서 애플리케이션 프록시 서비스로의 아웃바운드 연결을 용이하게 하는 경량 에이전트입니다. 커넥터는 백 엔드 애플리케이션에 대한 액세스 권한이 있는 Windows Server에 설치해야 합니다. 커넥터를 커넥터 그룹으로 나누고, 각 그룹이 특정 애플리케이션에 대한 트래픽을 처리하게 만들 수 있습니다.

## <a name="requirements-and-deployment"></a>요구 사항 및 배포

애플리케이션 프록시를 성공적으로 배포하려면 커넥터가 하나 이상 필요하지만, 복원력을 높이기 위해 두 개 이상을 사용하는 것이 좋습니다. Windows Server 2012 R2를 실행 하는 컴퓨터에 커넥터를 설치 이상. 커넥터는 애플리케이션 프록시 서비스 및 게시하는 온-프레미스 애플리케이션과 통신해야 합니다. 

### <a name="windows-server"></a>Windows Server
애플리케이션 프록시 커넥터를 설치할 수 있는 Windows Server 2012 R2 이상을 실행하는 서버가 필요합니다. 서버를 Azure의 애플리케이션 프록시 서비스 및 게시 중인 온-프레미스 애플리케이션에 연결해야 합니다.

Windows Server는 TLS 1.2를 사용하도록 설정한 후 애플리케이션 프록시 커넥터를 설치해야 합니다. 버전 1.5.612.0 이하의 기존 커넥터는 추가 공지가 있을 때까지 이전 버전의 TLS에서 계속 작동됩니다. TLS 1.2를 사용하도록 설정하려면:

1. 다음 레지스트리 키를 설정합니다.
    
    ```
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319] "SchUseStrongCrypto"=dword:00000001
    ```

2. 서버 다시 시작


커넥터 서버의 네트워크 요구 사항에 대한 자세한 내용은 [애플리케이션 프록시를 시작하고 커넥터 설치](application-proxy-add-on-premises-application.md)를 참조하세요.

## <a name="maintenance"></a>유지 관리
커넥터와 서비스는 모든 고가용성 작업을 처리합니다. 동적으로 추가하거나 제거할 수 있습니다. 새 요청이 수신될 때마다 현재 사용할 수 있는 커넥터 중 하나로 라우팅됩니다. 일시적으로 커넥터를 사용할 수 없는 경우 커넥터가 이 트래픽에 응답하지 않습니다.

커넥터는 상태 비저장이며 컴퓨터에 구성 데이터가 없습니다. 커넥터가 저장하는 유일한 데이터는 서비스와 인증 인증서를 연결하기 위한 설정입니다. 서비스에 연결할 때 필요한 모든 구성 데이터를 끌어오고 몇 분마다 새로 고쳐집니다.

커넥터는 또한 서버를 풀링하여 최신 버전의 커넥터가 있는지 확인합니다. 최신 버전이 있으면 커넥터가 자체적으로 업데이트합니다.

커넥터가 실행 중인 컴퓨터에서 이벤트 로그 및 성능 카운터를 사용하여 커넥터를 모니터링할 수 있습니다. 또는 Azure Portal의 애플리케이션 프록시 페이지에서 커넥터의 상태를 볼 수 있습니다.

 ![AzureAD 애플리케이션 프록시 커넥터](./media/application-proxy-connectors/app-proxy-connectors.png)

사용하지 않은 커넥터를 수동으로 삭제할 필요가 없습니다. 커넥터가 실행 중인 경우 서비스에 연결됨에 따라 활성 상태가 유지됩니다. 사용되지 않는 커넥터는 _비활성_으로 태그가 지정되고 비활성 상태가 된 지 10일 후에 제거됩니다. 하지만 커넥터를 제거하려면 서버에서 커넥터 서비스와 업데이트 프로그램 서비스를 모두 제거해야 합니다. 서비스를 완전히 제거하려면 컴퓨터를 다시 시작합니다.

## <a name="automatic-updates"></a>자동 업데이트

Azure AD에서는 사용자가 배포하는 모든 커넥터에 자동 업데이트를 제공합니다. 애플리케이션 프록시 커넥터 업데이터 서비스가 실행 중인 동안에는 커넥터가 자동으로 업데이트됩니다. 서버에 커넥터 업데이터 서비스가 표시되지 않는 경우 업데이트를 받으려면 [커넥터를 다시 설치](application-proxy-add-on-premises-application.md)해야 합니다. 

커넥터에 대한 자동 업데이트를 기다리지 않으려면 수동 업그레이드를 수행할 수 있습니다. 커넥터가 있는 서버에서 [커넥터 다운로드 페이지](https://download.msappproxy.net/subscription/d3c8b69d-6bf7-42be-a529-3fe9c2e70c90/connector/download)로 이동하여 **다운로드**를 선택합니다. 그러면 로컬 커넥터의 업그레이드가 시작됩니다. 

다중 커넥터가 있는 테넌트의 경우 자동 업데이트는 각 그룹에서 한 번에 하나의 커넥터를 대상으로 하여 사용자 환경의 가동 중지 시간을 방지합니다. 

다음과 같은 경우 커넥터를 업데이트할 때 가동 중지 시간이 발생할 수 있습니다.  
- 커넥터가 하나만 있으면 두 번째 커넥터를 설치하고 [커넥터 그룹을 만드는](application-proxy-connector-groups.md) 것이 좋습니다. 이렇게 하면 가동 중지 시간을 방지하고 보다 높은 가용성을 제공할 수 있습니다.  
- 업데이트를 시작했을 때 커넥터가 트랜잭션 중에 있었습니다. 초기 트랜잭션이 손실되지만, 브라우저에서 자동으로 작업을 다시 시도하거나 사용자가 페이지를 새로 고칠 수 있습니다. 요청을 다시 보내면 백업 커넥터에 트래픽이 라우팅됩니다.

이전에 릴리스된 버전 및 변경 내용에 대한 정보를 보려면 [애플리케이션 프록시-버전 릴리스 내역](application-proxy-release-version-history.md)을 참조하세요.

## <a name="creating-connector-groups"></a>커넥터 그룹 만들기

커넥터 그룹을 사용하면 특정 애플리케이션을 서비스하기 위해 특정 커넥터를 할당할 수 있습니다. 여러 커넥터를 하나로 그룹화한 다음 각 애플리케이션을 그룹에 할당할 수 있습니다. 

커넥터 그룹을 사용하면 대규모 배포를 관리하기 쉽습니다. 또한 로컬 애플리케이션만 서비스하기 위해 위치 기반 커넥터 그룹을 만들 수 있으므로 서로 다른 지역에서 호스팅되는 애플리케이션이 있는 테넌트의 대기 시간을 낮출 수 있습니다. 

커넥터 그룹에 대한 자세한 내용은 [커넥터 그룹을 사용하여 별도의 네트워크 및 위치에서 애플리케이션 게시](application-proxy-connector-groups.md)를 참조하세요.

## <a name="capacity-planning"></a>용량 계획 

예상된 트래픽 볼륨을 처리하기 위해 커넥터 간에 충분한 용량을 계획했는지 확인하는 것이 중요합니다. 각 커넥터 그룹에 두 개 이상의 커넥터 고가용성 및 확장성을 제공 하는 것이 좋습니다. 세 명의 커넥터 언제 든 지 컴퓨터를 서비스 해야 하는 경우 적합 합니다. 

일반적으로 사용자가 많을수록 더 큰 머신이 필요합니다. 다음은 서로 다른 컴퓨터를 처리할 수는 예상된 대기 시간은 고 볼륨의 개요를 제공 하는 테이블입니다. 사용 패턴이 다양하고 부하를 예측하는 데 사용할 수 없으므로 사용자 기준이 아닌 예상 TPS(초당 트랜잭션)를 기반으로 합니다. 또한 응답의 크기 및 백 엔드 애플리케이션 응답 시간에 따라 약간의 차이가 있습니다. 응답 크기가 크고 응답 시간이 느린 경우 최대 TPS가 낮아집니다. 충분 한 버퍼를 항상 제공 하는 컴퓨터 간에 부하를 분산된 되도록 추가 컴퓨터에 있는 것이 좋습니다. 추가 용량으로 고가용성 및 복원력을 확보할 수 있습니다.

|코어 수|RAM|예상 대기 시간(밀리초)-P99|최대 TPS|
| ----- | ----- | ----- | ----- |
|2|8|325|586|
|4|16|320|1150|
|8|32|270|1190|
|16|64|245|1200*|

\* 권장 설정.NET 이외의 기본 연결 제한 중 일부를 발생 시키는 데 사용자 지정 설정을 사용 하는이 컴퓨터. 지원 서비스에 문의하여 테넌트의 이 제한을 변경하기 전에, 기본 설정으로 테스트를 실행하는 것이 좋습니다.
 
>[!NOTE]
>4, 8, 16개 코어 컴퓨터 간에 최대 TPS는 크게 차이가 없습니다. 주요 차이점은 예상 대기 시간입니다.  

## <a name="security-and-networking"></a>보안 및 네트워킹

커넥터는 애플리케이션 프록시 서비스로 요청을 전송할 수 있는 네트워크 어느 곳에나 설치할 수 있습니다. 중요한 것은 커넥터를 실행 중인 컴퓨터에 앱에 대한 액세스가 있는 것입니다. 회사 네트워크 내부 또는 클라우드에서 실행되는 가상 머신에 커넥터를 설치할 수 있습니다. 커넥터를 완충 영역 (DMZ) 라고도 하는 경계 네트워크 내에서 실행할 수는 있지만 필요 없는 네트워크에 안전 하 게 유지 되므로 모든 트래픽이 아웃 바운드 이므로 합니다.

커넥터는 아웃바운드 요청만 보냅니다. 아웃바운드 트래픽은 애플리케이션 프록시 서비스와 게시된 애플리케이션으로 전송됩니다. 세션이 설정된 후에는 트래픽이 양방향으로 흐르므로 인바운드 포트를 열지 않아도 됩니다. 또한 방화벽을 통해 인바운드 액세스를 구성할 필요가 없습니다. 

아웃바운드 방화벽 규칙 구성에 대한 자세한 내용은 [기존 온-프레미스 프록시 서버 작업](application-proxy-configure-connectors-with-proxy-servers.md)을 참조하세요.


## <a name="performance-and-scalability"></a>성능 및 확장성

애플리케이션 프록시 서비스의 규모가 투명하더라도 규모는 커넥터와 관련된 요소입니다. 최고 트래픽을 처리할 만큼 충분한 커넥터를 마련해야 합니다. 커넥터는 상태 비저장이므로 사용자 또는 세션 수에 영향을 받지 않습니다. 대신, 요청의 수와 페이로드 크기에 반응합니다. 표준 웹 트래픽으로 평균적인 컴퓨터는 초당 몇 천 개의 요청을 처리할 수 있습니다. 특정 용량은 정확한 컴퓨터 특성에 따라 다릅니다. 

커넥터 성능은 CPU 및 네트워킹에 의해 한정됩니다. SSL 암호화 및 암호 해독에는 CPU 성능이 필요한 반면, 애플리케이션 및 Azure의 온라인 서비스에 신속히 연결하기 위해서는 네트워킹이 중요합니다.

반면, 커넥터에서는 메모리가 중요하지 않습니다. 온라인 서비스는 대부분의 프로세싱과 인증되지 않은 모든 트래픽을 처리합니다. 클라우드에서 수행할 수 있는 모든 작업은 클라우드에서 수행됩니다. 

어떤 이유로든 커넥터 또는 컴퓨터를 사용할 수 없는 경우 트래픽이 그룹에 있는 다른 커넥터로 이동하기 시작합니다. 이 복원력은 여러 커넥터를 권장하는 이유이기도 합니다.

성능에 영향을 주는 또 다른 요소는 커넥터 간 네트워킹의 품질이며 다음과 같습니다. 

* **온라인 서비스**: Azure의 애플리케이션 프록시 서비스에 대한 느린 또는 높은 대기 시간 연결은 커넥터 성능에 영향을 줍니다. 최상의 성능을 위해 ExpressRoute를 사용하여 조직을 Azure에 연결합니다. 그렇지 않은 경우 네트워킹 팀에서 Azure 연결을 최대한 효율적으로 처리하도록 합니다. 
* **백 엔드 애플리케이션**: 경우에 따라 커넥터와 백 엔드 애플리케이션 사이에 추가 프록시가 있어 연결이 느려지거나 실패할 수 있습니다. 이 시나리오를 해결하려면 커넥터 서버에서 브라우저를 열고 애플리케이션에 액세스합니다. Azure에서 커넥터를 실행하고 애플리케이션이 온-프레미스 상태이면 사용자가 기대한 환경이 아닐 수 있습니다.
* **도메인 컨트롤러**: 커넥터가 Kerberos 제한된 위임을 사용하여 SSO(Single Sign-On)를 수행하는 경우 커넥터는 백 엔드에 요청을 보내기 전에 도메인 컨트롤러에 연결합니다. 커넥터에는 Kerberos 티켓 캐시가 있지만 사용량이 많은 환경에서는 도메인 컨트롤러의 응답성이 성능에 영향을 줄 수 있습니다. 이 문제는 Azure에서 실행되지만 온-프레미스 도메인 컨트롤러와 통신하는 커넥터에서 좀 더 자주 발생합니다. 

네트워크 최적화에 대한 자세한 내용은 [Azure Active Directory 애플리케이션 프록시를 사용할 때 네트워크 토폴로지 고려 사항](application-proxy-network-topology.md)을 참조하세요.

## <a name="domain-joining"></a>도메인 가입

도메인 가입되지 않은 컴퓨터에서 커넥터를 실행할 수 있습니다. 하지만 Windows 통합 인증(IWA)을 사용하는 애플리케이션에 SSO(Single Sign-On)를 사용하려는 경우에는 도메인 가입된 컴퓨터가 필요합니다. 이 경우 커넥터 컴퓨터는 게시된 애플리케이션에 대한 사용자를 대신하여 [Kerberos](https://web.mit.edu/kerberos) 제한된 위임을 수행할 수 있는 도메인에 가입되어야 합니다.

커넥터는 도메인이나 부분 신뢰하는 포리스트 또는 읽기 전용 도메인 컨트롤러에 가입할 수도 있습니다.

## <a name="connector-deployments-on-hardened-environments"></a>강화된 환경에 커넥터 배포

일반적으로 커넥터 배포는 매우 간단하며 특별한 구성이 필요하지 않습니다. 그러나 고려해야 할 몇 가지 고유한 조건이 있습니다.

* 아웃바운드 트래픽을 제한하는 조직은 [필요한 포트를 열어야](application-proxy-add-on-premises-application.md#prepare-your-on-premises-environment) 합니다.
* 커넥터 프로세스에서 인증서를 생성 및 저장할 수 있도록 구성을 변경하려면 FIPS 규격 컴퓨터가 필요할 수 있습니다.
* 네트워킹 요청을 발행하는 프로세스에 따라 환경을 잠그는 조직은 두 커넥터 서비스가 필요한 모든 포트 및 IP에 액세스할 수 있는지 확인해야 합니다.
* 경우에 따라 아웃바운드 전달 프록시가 양방향 인증서 인증을 중단하여 통신에 실패할 수 있습니다.

## <a name="connector-authentication"></a>커넥터 인증

안전한 서비스를 제공하기 위해 커넥터는 서비스에 대해 인증되어야 하고 서비스는 커넥터에 대해 인증되어야 합니다. 커넥터에서 연결을 시작하면 클라이언트 및 서버 인증서를 사용하여 이 인증이 수행됩니다. 이 경우 관리자의 사용자 이름 및 암호는 커넥터 컴퓨터에 저장되지 않습니다.

사용된 인증서는 애플리케이션 프록시 서비스로 국한됩니다. 인증서는 초기 등록 중에 생성되며 몇 개월마다 커넥터에 의해 자동으로 갱신됩니다. 

몇 달 동안 커넥터가 서비스에 연결되지 않는 경우 인증서가 만료될 수 있습니다. 이 경우 커넥터를 제거 후 다시 설치하여 등록을 트리거합니다. 다음 PowerShell 명령을 실행하면 됩니다.

```
Import-module AppProxyPSModule
Register-AppProxyConnector
```

## <a name="under-the-hood"></a>내부 살펴보기

커넥터는 Windows Server 웹 애플리케이션 프록시를 기반으로 하므로 Windows 이벤트 로그를 포함하여 대부분 동일한 관리 도구를 포함합니다.

 ![이벤트 뷰어로 이벤트 로그 관리](./media/application-proxy-connectors/event-view-window.png)

및 Windows 성능 카운터 

 ![성능 모니터로 커넥터에 카운터 추가](./media/application-proxy-connectors/performance-monitor.png)

커넥터에는 관리 및 세션 로그가 모두 포함됩니다. 관리 로그에는 주요 이벤트와 해당 오류가 포함됩니다. 세션 로그에는 모든 트랜잭션 및 처리 세부 정보가 포함됩니다. 

로그를 보려면 이벤트 뷰어로 이동하고 **보기** 메뉴를 열고 **분석 및 디버그 로그 표시**를 활성화합니다. 그런 다음 활성화하여 이벤트 수집을 시작합니다. 커넥터는 보다 최신 버전을 기반으로 하므로 Windows Server 2012 R2의 웹 애플리케이션 프록시에서는 이러한 로그가 나타나지 않습니다.

서비스 창에서 서비스 상태를 검사할 수 있습니다. 커넥터는 두 개의 Windows 서비스(실제 커넥터와 업데이터)로 구성됩니다. 둘 다 항상 실행되어야 합니다.

 ![AzureAD 서비스 로컬](./media/application-proxy-connectors/aad-connector-services.png)

## <a name="next-steps"></a>다음 단계


* [커넥터 그룹을 사용하여 별도의 네트워크 및 위치에서 애플리케이션 게시](application-proxy-connector-groups.md)
* [기존 온-프레미스 프록시 서버 작업](application-proxy-configure-connectors-with-proxy-servers.md)
* [애플리케이션 프록시 및 커넥터 오류 문제 해결](application-proxy-troubleshoot.md)
* [Azure AD 애플리케이션 프록시 커넥터를 자동으로 설치하는 방법](application-proxy-register-connector-powershell.md)

