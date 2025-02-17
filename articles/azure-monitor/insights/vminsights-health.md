---
title: VM용 Azure Monitor(미리 보기)를 사용하여 가상 머신 상태 모니터링| Microsoft Docs
description: 이 문서에서는 VM용 Azure Monitor를 사용하여 가상 머신 및 기본 운영 체제의 상태를 이해하는 방법에 대해 설명합니다.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2019
ms.author: magoedte
ms.openlocfilehash: 9fa76c9637a6dcdca48bf45e8ee2aa9305a4f64f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66130448"
---
# <a name="understand-the-health-of-your-azure-virtual-machines"></a>Azure virtual machines의 상태를 이해 합니다.

Azure는 개별적으로 모니터링 공간에서 특정 역할이 나 태스크를 수행 하지만 Azure virtual machines에서 호스트 운영 체제의 자세한 상태 큐브 뷰를 제공 하지 않은 여러 서비스를 포함 합니다. Azure Monitor를 사용 하 여 다른 조건을 모니터링할 수 있습니다, 있지만 모델 및 핵심 구성 요소 상태 또는 가상 컴퓨터의 전반적인 상태를 나타내는 설계 되지 않은 것입니다. VM용 Azure Monitor 상태 기능을 사용하면 주요 구성 요소와 이들의 관계, 구성 요소의 상태를 측정하는 방법을 지정하는 기준이 되는 모델을 사용하여 Windows 또는 Linux 게스트 OS의 가용성 및 성능을 사전에 모니터링하고 비정상 상태가 감지되면 경고를 표시합니다.  

Azure VM 및 기본 운영 체제의 전반적인 상태를 보려면 VM용 Azure Monitor 상태의 두 가지 관점 보기에서 관찰할 수 있습니다. 가상 머신에서 직접 관찰하거나 Azure Monitor의 리소스 그룹에 있는 모든 VM에서 관찰할 수 있습니다.

이 문서는 감지된 상태 문제를 신속하게 평가하고, 조사하고, 해결하는 방법을 이해하는 데 도움이 됩니다.

VM용 Azure Monitor를 구성하는 방법에 대한 자세한 내용은 [VM용 Azure Monitor 사용하도록 설정](vminsights-enable-overview.md)을 참조하세요.

## <a name="monitoring-configuration-details"></a>모니터링 구성 세부 정보

이 섹션에서는 Azure Windows 및 Linux 가상 머신을 모니터링하도록 정의된 기본 상태 조건을 설명합니다. 모든 상태 조건은 비정상 상태가 충족되면 경고를 표시하도록 미리 구성됩니다. 

### <a name="windows-vms"></a>Windows VM

- 메모리의 사용 가능 메가바이트 
- 쓰기(논리 디스크)당 평균 디스크 시간(초)
- 쓰기(디스크)당 평균 디스크 시간(초)
- 읽기당 평균 논리적 디스크 시간(초)
- 전송당 평균 논리적 디스크 시간(초)
- 읽기당 평균 디스크 시간(초)
- 전송당 평균 디스크 시간(초)
- 현재 디스크 큐 길이(논리 디스크)
- 현재 디스크 큐 길이(디스크)
- 디스크의 유휴 시간 비율
- 파일 시스템 오류 또는 손상
- 논리적 디스크에서 사용 가능한 공간 (%) 낮음
- 논리적 디스크에서 사용 가능한 공간 (MB) 낮음
- 논리적 디스크의 유휴 시간 비율
- 초당 메모리 페이지 수
- 읽기에 사용된 대역폭 비율
- 총 사용된 대역폭 비율
- 쓰기에 사용된 대역폭 비율
- 사용 중인 커밋된 메모리 비율
- 디스크의 유휴 시간 비율
- DHCP 클라이언트 서비스 상태
- DNS 클라이언트 서비스 상태
- RPC 서비스 상태
- 서버 서비스 상태
- 총 CPU 사용 백분율
- Windows 이벤트 로그 서비스 상태
- Windows 방화벽 서비스 상태
- Windows 원격 관리 서비스 상태

### <a name="linux-vms"></a>Linux VM
- 디스크 평균 디스크 초/전송 
- 디스크 평균 디스크 초/읽기 
- 디스크 평균 디스크 초/쓰기 
- 디스크 상태
- 논리적 디스크에서 사용 가능한 공간
- 논리적 디스크에서 사용 가능한 공간 비율
- 논리적 디스크에서 사용 가능한 inode 비율
- 네트워크 어댑터 상태
- 총 프로세서 시간 비율
- 운영 체제에서 사용 가능한 메모리의 메가바이트

## <a name="sign-in-to-the-azure-portal"></a>Azure Portal에 로그인

[Azure Portal](https://portal.azure.com)에 로그인합니다. 

## <a name="introduction-to-health-experience"></a>상태 환경 소개

단일 가상 머신 또는 VM 그룹에 상태 기능을 사용하는 방법을 살펴보기 전에, 정보가 제공되는 방식과 시각화에서 나타내는 의미를 이해할 수 있도록 간략한 소개를 살펴보는 것이 중요합니다.  

### <a name="view-health-directly-from-a-virtual-machine"></a>가상 머신에서 직접 상태 보기 

Azure VM의 상태를 보려면 가상 머신의 왼쪽 창에서 **인사이트(미리 보기)** 를 선택합니다. VM 인사이트 페이지에는 기본적으로 **상태**가 열려 있으며 VM의 상태 보기가 표시됩니다.  

![선택한 Azure 가상 머신에 대한 VM용 Azure Monitor 상태 개요](./media/vminsights-health/vminsights-directvm-health.png)

**게스트 VM 상태** 섹션 아래 **상태** 탭에 있는 테이블에는 가상 머신의 현재 성능 상태 및 비정상적인 구성 요소에서 발생한 VM 상태 경고의 총 수가 표시됩니다. 자세한 내용은 [경고](#alerts) 경고에 대 한 추가 세부 정보에 대 한 환경입니다.  

다음 표에는 VM에 대해 정의된 상태가 나와 있습니다. 

|아이콘 |성능 상태 |의미 |
|-----|-------------|---------------|
| |Healthy |성능 상태는 정의된 상태에 있으면 정상입니다. 이는 VM에 대해 검색된 문제가 없음을 나타내며 필요에 따라 작동합니다. 부모 롤업 모니터를 사용 하 여 상태 롤업 하 고 자식 최상의 또는 최악의 상태를 반영 합니다.|
| |중요 |성능 상태는 정의된 상태 조건 범위 내에 있지 않은 경우 위험입니다. 이 상태는 하나 이상의 위험한 문제가 검색되었으며 정상 기능을 복원하려면 해당 문제를 해결해야 함을 나타냅니다. 부모 롤업 모니터를 사용 하 여 상태 롤업 하 고 자식 최상의 또는 최악의 상태를 반영 합니다.|
| |Warning |성능 상태는 정의된 상태 조건의 두 임계값 사이에 있으면 경고입니다. 두 임계값 중 하나는 *경고* 상태를, 다른 하나는 *위험* 상태를 나타냅니다(세 가지 성능 상태 임계값을 구성할 수 있음). 또는 해결되지 않으면 위험한 문제가 발생할 수 있는 위험하지 않은 문제가 검색된 경우가 있습니다. 부모 롤업 모니터를 하나 이상의 자식에 경고 상태를 부모 반영 *경고* 상태입니다. 하위 항목이 *위험* 상태이고 다른 하위 항목이 *경고* 상태이면 상위 롤업의 성능 상태는 *위험*으로 표시됩니다.|
| |알 수 없음 |성능 상태는 *알 수 없는* 때 여러 가지 이유로 계산할 수 없습니다. 다음의 각주를 참조 하세요 <sup>1</sup> 추가 세부 정보 및 이러한 문제를 해결할 수 솔루션에 대 한 합니다. |

<sup>1</sup> the 알 수 없는 상태는 다음과 같은 문제가 야기 됩니다.

- 에이전트를 다시 구성 하 고 Vm에 대 한 Azure Monitor를 사용 하는 경우 보고서 작업 영역을 지정 하는 더 이상. 작업 영역 참조를 보고 하도록 에이전트를 구성 하려면 [추가 또는 제거 하려면 작업 영역](../platform/agent-manage.md#adding-or-removing-a-workspace)합니다.
- VM 삭제 되었습니다.
- Vm에 대 한 Azure Monitor와 사용 하 여 연결 된 작업 영역 삭제 됩니다. 프리미어 지원 혜택을 사용 하 여 지원 요청을 시작할 수 있는 경우 작업 영역을 복구할 [프리미어](https://premier.microsoft.com/)합니다.
- 솔루션 종속성 삭제 되었습니다. Log Analytics 작업 영역에서은 ServiceMap 및 InfrastructureInsights 솔루션을 다시 설정 하려면 다시 설치할 수 있습니다 사용 하는 [Azure Resource Manager 템플릿을](vminsights-enable-at-scale-powershell.md#install-the-servicemap-and-infrastructureinsights-solutions) 제공 했거나 있는 작업 영역 구성 옵션을 사용 하는 합니다 시작 됨 탭을 가져옵니다.
- VM을 종료 했습니다.
- Azure VM 서비스를 사용할 수 없거나 유지 관리 수행 됨.
- 작업 영역 [일일 데이터 또는 보존 한도](../platform/manage-cost-storage.md) 충족 됩니다.

**상태 진단 보기**를 선택하면 VM의 모든 구성 요소, 연결된 상태 조건, 상태 변경 및 VM과 관련된 구성 요소를 모니터링하여 찾은 기타 중요한 문제가 표시되는 페이지가 열립니다. 자세한 내용은 [상태 진단](#health-diagnostics)을 참조하세요. 

**구성 요소 상태** 섹션 아래 테이블에는 해당 영역 특히, **CPU**, **메모리**, **디스크** 및 **네트워크**의 상태 조건으로 모니터링 되는 기본 성능 범주의 상태 롤업 상태가 표시됩니다.  구성 요소 중 하나를 선택하면 구성 요소의 개별적인 상태 기준 모니터링 측면과 각각의 성능 상태가 나열된 페이지가 열립니다.  

Windows 운영 체제를 실행 하는 Azure VM의 상태에 액세스할 때 위의 상태 5 핵심 Windows 서비스 섹션 아래에 표시 됩니다 **Core services 상태**합니다.  서비스 중 하나를 선택하면 해당 구성 요소 및 성능 상태를 모니터링하는 성능 상태가 나열된 페이지가 열립니다.  상태 조건의 이름을 클릭하면 속성 창이 열리고, 여기에서 상태 조건에 해당하는 구성 세부 정보(예: Azure Monitor 경고가 정의되어 있는지 여부)를 검토할 수 있습니다. 자세한 내용은 [상태 진단 및 상태 조건 사용](#health-diagnostics)을 참조하세요.  

### <a name="aggregate-virtual-machine-perspective"></a>집계 가상 머신 관점

리소스 그룹에 있는 모든 가상 머신의 상태 컬렉션을 보려면 포털의 탐색 목록에서 **Azure Monitor**를 선택한 다음 **Virtual Machines(미리 보기)** 를 선택합니다.  

![Azure Monitor의 VM 인사이트 모니터링 보기](./media/vminsights-health/vminsights-aggregate-health.png)

보고된 성능 상태를 확인하려면 **구독** 및 **리소스 그룹** 드롭다운 목록에서 그룹과 관련된 VM이 포함된 적절한 리소스 그룹을 선택합니다.  선택 항목은 상태 기능에만 적용되며 성능 또는 맵으로 전달되지 않습니다.

**상태** 탭에서 다음 사항을 알아볼 수 있습니다.

* 위험 또는 비정상 상태인 VM 수와 정상 또는 데이터를 제출하지 않음(알 수 없는 상태라고도 함) 상태인 VM 수.
* OS(운영 체제)별로 비정상 상태를 보고하는 VM 및 보고 건수
* 성능 상태로 분류된 프로세서, 디스크, 메모리 또는 네트워크 어댑터를 통해 감지된 문제로 인해 비정상인 VM의 수  
* 성능 상태로 분류된 핵심 운영 체제 서비스를 통해 감지된 문제로 인해 비정상인 VM의 수

여기에서는 VM을 사전에 모니터링하는 상태 조건에 의해 검색된 상위의 위험한 문제를 신속하게 파악할 수 있고, VM 상태 경고 세부 정보 및 문제 진단 및 수정에 도움을 주기 위해 작성된 관련 기술 항목을 검토할 수 있습니다.  아무 심각도나 선택하면 해당 심각도를 기준으로 필터링된 [모든 경고](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) 페이지가 열립니다.

**운영 체제별 VM 배포** 목록에는 Windows 버전 또는 Linux 배포판에 나열된 VM이 해당 버전과 함께 표시됩니다. 운영 체제 범주마다 VM의 상태를 기반으로 VM이 세분화되어 있습니다. 

![VM 인사이트 가상 머신 배포 관점](./media/vminsights-health/vminsights-vmdistribution-by-os.png)

열 항목(**VM 개수**, **위험**, **경고**, **정상** 또는 **알 수 없음**) 중 하나를 클릭하여 **Virtual Machines** 페이지를 드릴다운하면 선택한 열과 일치하는 필터링된 결과 목록을 볼 수 있습니다. 예를 들어 **Red Hat Enterprise Linux 릴리스 7.5**를 실행하는 모든 VM을 검토하려는 경우, 해당 OS에 대한 **VM 개수** 값을 클릭하면 다음과 같은 페이지가 열립니다. 여기에는 해당 필터와 일치하는 가상 머신과 현재 알려진 성능 상태가 나열됩니다.  

![Red Hat Linux VM의 롤업 예제](./media/vminsights-health/vminsights-rollup-vm-rehl-01.png)
 
**Virtual Machines** 페이지에서 **VM 이름** 열 아래에 있는 VM의 이름을 선택하면 VM 인스턴스 페이지가 열립니다. 여기에는 경고에 대한 자세한 내용과 선택한 VM에 영향을 미치는 것으로 파악된 상태 조건 문제가 있습니다.  여기에서 페이지 왼쪽 위 모서리에 있는 **상태** 아이콘을 클릭하면 성능 상태 세부 정보를 필터링하여 어떤 구성 요소가 비정상 상태인지 볼 수 있습니다. 또는 경고 심각도로 분류된 비정상 구성 요소에서 발생한 VM 상태 경고를 볼 수 있습니다.    

VM 목록 보기에서 VM의 이름을 클릭하면 선택한 VM에 해당하는 **상태** 페이지가 열립니다. 이것은 VM에서 **인사이트(미리 보기)** 를 직접 선택하는 것과 비슷합니다.

![선택한 Azure 가상 머신에 대한 VM 인사이트](./media/vminsights-health/vminsights-directvm-health.png)

여기에는 가상 머신에 대한 롤업 **상태**와 심각도별로 분류된 **경고**가 표시됩니다. 이것은 성능 상태의 상태 조건이 정상에서 비정상으로 변경되면 발생하는 VM 상태 경고를 나타냅니다.  **위험 상태에 있는 VM**을 선택하면 위험 상태에 있는 VM이 하나 이상 포함된 목록이 있는 페이지가 열립니다.  목록에 있는 VM 중 하나의 상태를 클릭하면 VM의 **상태 진단** 보기가 표시됩니다.  여기에서 어떤 상태 조건이 상태 문제를 반영하는지 확인할 수 있습니다. **상태 진단** 페이지가 열리면 VM의 모든 구성 요소 및 이 구성 요소와 연결된 상태 조건이 현재 상태와 함께 표시됩니다. 자세한 내용은 [상태 진단](#health-diagnostics)합니다.  

**모든 상태 조건 보기**를 선택하면 이 기능으로 사용할 수 있는 모든 상태 조건의 목록을 보여주는 페이지가 열립니다.  이 정보는 다음과 같은 옵션을 기반으로 추가로 필터링할 수 있습니다.

* **유형** – 조건을 평가하고 모니터링되는 VM의 전반적인 상태를 롤업하는 상태 조건 유형에는 세 가지 종류가 있습니다.  
    a. **단위** – 가상 머신의 특정 양상을 측정합니다. 상태 조건 유형은 성능 카운터를 확인하여 구성 요소의 성능을 판단하고 스크립트를 실행하여 가상 트랜잭션을 수행하거나 오류를 나타내는 이벤트를 감시합니다.  필터는 기본적으로 단위로 설정됩니다.  
    b. **종속성** - 서로 다른 엔터티 간의 상태 롤업을 제공합니다. 이 상태 조건은 엔터티의 상태가 성공적인 작업을 위해 의존하는 다른 종류의 엔터티의 상태에 따라 달라지도록 허용합니다.  
    c. **집계** -  유사한 상태 조건에 대해 결합된 상태를 제공합니다. 단위 및 종속성 상태 조건은 일반적으로 집계 상태 조건 하에 구성됩니다. 엔터티를 대상으로 하는 다양한 상태 조건에 대해 보다 나은 일반적인 구성을 제공하는 것 외에도, 집계 상태 조건은 엔터티의 개별 범주에 대해 고유한 성능 상태를 제공합니다.

* **범주** - 보고를 목적으로 유사한 유형의 조건을 그룹화하는 데 사용되는 상태 조건의 유형입니다.  **가용성** 또는 **성능**이 여기에 해당됩니다.

**비정상 구성 요소** 열 아래에 있는 값을 클릭하면 어떤 인스턴스가 비정상인지 더 자세히 알아볼 수 있습니다.  페이지의 테이블에는 위험 상태에 있는 구성 요소가 나열되어 있습니다.    

## <a name="health-diagnostics"></a>상태 진단

합니다 **상태 진단** 상태 조건, 상태 변경, 연결 된 VM의 모든 구성 요소를 나열, VM의 상태 모델을 시각화 하 고 관련 된 구성 요소를 모니터링 하는 기타 중요 한 문제를 식별 하 여 페이지를 사용 하면 VM.

![VM에 대한 상태 진단 페이지의 예](./media/vminsights-health/health-diagnostics-page-01.png)

다음과 같은 방법으로 상태 진단을 시작할 수 있습니다.

* Azure Monitor에서 집계 VM 관점으로 모든 VM에 대한 성능 상태를 롤업합니다.  **상태** 페이지에서 **게스트 VM 상태** 섹션 아래 **위험**, **경고**, **정상** 또는 **알 수 없음** 성능 상태에 해당하는 아이콘을 클릭하고 필터링된 범주와 일치하는 모든 VM이 나열되어 있는 페이지를 드릴다운합니다.  **성능 상태** 열에 있는 값을 클릭하면 해당하는 특정 VM에 맞게 범위가 지정된 상태 진단이 열립니다.      

* Azure Monitor에서 집계 VM 관점으로 시스템을 운영합니다. **VM 배포** 아래에 있는 열 값 중 하나를 선택하면 **Virtual Machines** 페이지가 열리고 필터링된 범주와 일치하는 테이블에 목록이 반환됩니다.  **성능 상태** 열 아래에 있는 값을 클릭하면 선택한 VM에 해당하는 상태 진단이 열립니다.    
 
* VM용 Azure Monitor **상태** 탭에 있는 게스트 VM에서 **상태 진단 보기**를 선택합니다. 

상태 진단은 상태 정보를 다음 범주로 구성합니다. 

* 가용성
* 성능
 
논리 디스크와 같은 특정 구성 요소에 대해 정의 된 모든 상태 조건 (즉, 모든 조건의 전체 보기를) 두 개의 범주를 필터링 하지 않고 볼 수 있습니다 또는 선택 하는 경우 두 범주별으로 결과 필터링 등 CPU **가용성**  나 **성능** 페이지의 옵션입니다. 또한에서 옆에 있는 조건의 범주를 볼 수 있습니다 합니다 **상태 조건을** 열입니다. 조건에는 선택한 범주와 일치 하지 않으면, 메시지가 표시 됩니다 **선택한 범주에 사용할 수 없는 상태 조건을** 에 **상태 조건을** 열입니다.  

상태 조건의 상태는 네 가지 상태(*위험*, *경고*, *정상*, *알 수 없음*) 중 하나로 정의됩니다. 처음 3 개 상태 조건을 구성 창에서 직접 모니터의 임계값을 수정할 수 있습니다 의미 또는 Azure Monitor REST API를 사용 하 여 구성할 수 있습니다 [업데이트 작업을 모니터](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update)합니다. *알 수 없음*은 특정 시나리오에 맞게 구성 및 예약할 수 없습니다.  

상태 진단 페이지에서 세 가지 주요 섹션이 있습니다.

* 구성 요소 모델 
* 상태 조건
* 상태 변경 

![상태 진단 페이지의 섹션](./media/vminsights-health/health-diagnostics-page-02.png)

### <a name="component-model"></a>구성 요소 모델

상태 진단 페이지의 맨 왼쪽 열이 구성 요소 모델입니다. VM과 연결된 모든 구성 요소가 현재 성능 상태와 함께 이 열에 표시됩니다. 

다음 예제에서 검색된 구성 요소는 디스크, 논리 디스크, 프로세서, 메모리 및 운영 체제입니다. 이러한 구성 요소의 여러 인스턴스가 검색되어 이 열에 표시됩니다. 예를 들어 아래 이미지에 나와 있는 VM에는 정상 상태의 논리 디스크 인스턴스 2개(C: 및 D:)가 있습니다.  

![상태 진단에 제공된 예제 구성 요소 모델](./media/vminsights-health/health-diagnostics-page-component.png)

### <a name="health-criteria"></a>상태 조건

상태 진단 페이지의 가운데 열은 **상태 조건** 열입니다. VM에 대해 정의된 상태 모델이 계층 구조 트리에 표시됩니다. VM의 상태 모델은 단위 및 집계 상태 조건으로 구성됩니다.  

![상태 진단에 제공된 상태 조건 예제](./media/vminsights-health/health-diagnostics-page-healthcriteria.png)

상태 조건은 몇 가지 조건(예: 임계값, 엔터티의 상태 등)을 사용하여 모니터링되는 인스턴스의 상태를 측정합니다. 앞에서 설명한 것처럼 상태 조건에는 구성 가능한 성능 상태 임계값 2~3개가 있습니다. 어느 시점이든 상태 조건은 잠재적인 상태 중 하나에만 해당될 수 있습니다. 

대상의 전반적인 상태는 상태 모델에 정의된 각 상태 조건의 상태에 따라 결정됩니다. 이 상태 조건 대상에서 직접 대상이 되는 집계 상태 조건을 통해 대상에 롤업 하는 구성 요소를 대상으로 하는 상태 조건의 조합입니다. 이 계층 구조는 상태 진단 페이지의 **상태 조건** 섹션에 예시되어 있습니다. 상태 롤업 정책은 집계 상태 조건 구성에 포함되며 기본값은 *최악*으로 설정됩니다. 기본 상태 조건 섹션에서이 기능의 일부로 실행 중인 집합의 목록을 찾을 수 있습니다 [구성 세부 정보를 모니터링](#monitoring-configuration-details), 및 Azure Monitor REST API를 사용 하 여 [리소스에서 목록 인스턴스를 모니터링 합니다. 작업](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitorinstances/listbyresource) 모든 상태 조건 및 Azure VM 리소스에 대해 실행 되는 자세한 구성 목록을 가져오려고 합니다.  

**단위** 상태 조건 유형은 자체 구성을 포함할 수 있습니다. 맨 오른쪽 엤는 줄임표 링크를 클릭하고 **자세한 정보 표시**를 선택하여 구성 창을 열면 이 구성을 수정할 수 있습니다. 

![상태 조건 구성 예제](./media/vminsights-health/health-diagnostics-vm-example-02.png)

선택한 상태 조건의 구성 창에서 예제 **Average Disk Seconds Per Write**를 사용하면 다른 숫자 값으로 해당 임계값을 구성할 수 있습니다. 이 모니터의 상태는 2개뿐입니다. 즉, 상태가 항상 정상과 경고 간에만 변경됩니다. 다른 상태 조건은 세 가지 상태일 수 있으며 이 경우 경고 및 위험 성능 상태 임계값을 구성할 수 있습니다. Azure Monitor REST API를 사용 하 여 임계값을 수정할 수도 있습니다 [업데이트 작업을 모니터](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/monitors/update)합니다.

>[!NOTE]
>하나의 인스턴스에 상태 조건 구성 변경 내용을 적용하면 모니터링되는 모든 인스턴스에 해당 변경 내용이 적용됩니다.  예를 들어 **디스크 - 1 D:** 를 선택하고 **쓰기당 평균 디스크 시간(초)** 임계값을 수정하면 해당 인스턴스에만 적용되지는 것이 아니라 VM에서 검색 및 모니터링되는 다른 모든 디스크 인스턴스에도 적용됩니다.
>

![단위 모니터의 상태 조건 구성 예제](./media/vminsights-health/health-diagnostics-criteria-config-01.png)

상태 표시기에 대해 자세히 알아보려는 경우 포함된 기술 자료 문서에서 문제, 원인 및 해결 방법을 파악할 수 있습니다. 페이지에서 **정보 보기** 링크를 클릭하면 브라우저에서 특정 기술 자료 문서를 보여주는 새 탭이 열립니다. 언제든지 [여기](https://docs.microsoft.com/azure/monitoring/infrastructure-health/)에서 VM용 Azure Monitor 상태 기능에 포함된 상태 조건 기술 문서를 모두 검토할 수 있습니다.
  
### <a name="state-changes"></a>상태 변경

상태 진단 페이지의 맨 오른쪽 열은 **상태 변경**입니다. 여기에는 **상태 조건** 섹션에서 선택한 상태 조건과 연결된 모든 상태 변경 또는 테이블의 **구성 요소 모델** 또는 **상태 조건** 열에 VM이 선택된 경우 VM의 상태 변경이 나열됩니다. 

![상태 진단에 제공된 상태 변경 예제](./media/vminsights-health/health-diagnostics-page-statechanges.png)

이 섹션은 상태 조건 상태 및 연결된 시간(맨 위부터 최신 상태별로 정렬됨)으로 구성됩니다.   

### <a name="association-of-component-model-health-criteria-and-state-change-columns"></a>연결 구성 요소 모델, 상태 조건 및 상태 열 변경 

세 개의 열은 서로 간에 상호 연결되어 있습니다. **구성 요소 모델** 섹션에서 검색된 인스턴스를 선택하면 **상태 조건** 섹션이 필터링되어 해당 구성 요소 보기만 표시되며, 그에 따라 선택한 상태 조건을 기준으로 **상태 변경** 섹션이 업데이트됩니다. 

![모니터링되는 인스턴스 선택 및 결과 예제](./media/vminsights-health/health-diagnostics-vm-example-01.png)

위의 예에서 **디스크 - 1 D:** 를 선택하면 상태 조건 트리가 **디스크 - 1D:** 로 필터링됩니다. **상태 변경** 열에는 **디스크 - 1 D:** 의 가용성에 기반한 상태 변경이 표시됩니다. 

업데이트된 성능 상태를 보려는 경우 **새로 고침** 링크를 클릭하여 상태 진단 페이지를 새로 고치면 됩니다.  미리 정의된 폴링 간격을 기반으로 상태 조건의 성능 상태에 대한 업데이트가 있는 경우, 이 작업을 통해 대기하지 않고 최신 성능 상태를 반영할 수 있습니다.  **상태 조건 상태**는 선택한 성능 상태(*정상*, *경고*, *위험*, *알 수 없음*, *모두*)를 기반으로 결과의 범위를 지정할 수 있는 필터입니다.  오른쪽 위 모서리의 **마지막 업데이트 시간**은 상태 진단 페이지를 마지막으로 새로 고친 시간을 나타냅니다.  

## <a name="alerts"></a>경고

VM용 Azure Monitor 상태 기능은 [Azure 경고](../../azure-monitor/platform/alerts-overview.md)와 통합되어 조건이 감지될 때 미리 정의된 상태 조건이 정상에서 비정상 상태로 바뀌면 경고를 발생시킵니다. 경고는 심각도별로 0에서 4까지 분류됩니다. 심각도 0은 심각도 수준이 가장 높은 것을 나타냅니다. 

경고가 트리거되면 경고 사용자에 게 알려 주기 위해 작업 그룹을 사용 하 여 연결 되지 않습니다. 단계에 따라 알림을 구성 해야 하는 구독 소유자 [이 섹션의 뒷부분에 나오는](#configure-alerts)합니다.   

심각도별로 분류된 VM 상태 경고의 총 수는 **경고** 섹션의 **상태** 대시보드에 제공됩니다. 총 경고 수 또는 심각도 수준에 해당하는 숫자를 선택하면, **경고** 페이지가 열리고 선택 항목과 일치하는 모든 경고가 나열됩니다.  예를 들어 **심각도 수준 1**에 해당하는 행을 선택하면 다음 보기가 표시됩니다.

![모든 심각도 수준 1 경고 예제](./media/vminsights-health/vminsights-sev1-alerts-01.png)

**경고** 페이지에서 선택 사항과 일치하는 경고를 표시하도록 범위가 지정되어 있을 뿐만 아니라 가상 머신 리소스에서 발생한 상태 경고만 표시하도록 **리소스 종류**별로 필터링되어 있습니다.  열에서 경고 목록에 반영 됩니다 **대상 리소스**을 Azure VM에 대 한 경고가 발생 한 특정 상태 조건을 비정상 조건이 충족 된 경우를 보여 줍니다.  

이 보기에 포함 될 다른 리소스 유형 또는 서비스에서 경고 하지 않습니다, 일반적으로 기본 Azure Monitor에서에서 보는 또는 로그 쿼리를 기반으로 한 로그 경고는 메트릭 경고와 같은 [모든 경고](../../azure-monitor/platform/alerts-overview.md#all-alerts-page) 페이지입니다. 

페이지 맨 위에 있는 드롭다운 메뉴에서 값을 선택하여 이 보기를 필터링할 수 있습니다.

|열 |설명 | 
|-------|------------| 
|구독 |Azure 구독을 선택합니다. 선택한 구독의 경고만 보기에 포함됩니다. | 
|리소스 그룹 |단일 리소스 그룹을 선택합니다. 선택한 리소스 그룹의 대상이 있는 경고만 보기에 포함됩니다. | 
|리소스 종류 |리소스 종류를 하나 이상 선택합니다. 기본적으로 대상 **가상 머신**의 경고만 선택되고 이 보기에 포함됩니다. 이 열은 리소스 그룹을 지정한 후에만 사용할 수 있습니다. | 
|리소스 |리소스를 선택합니다. 해당 리소스가 대상으로 지정된 경고만 보기에 포함됩니다. 이 열은 리소스 종류를 지정한 후에만 사용할 수 있습니다. | 
|Severity |경고 심각도를 선택하거나, 심각도에 상관없이 모든 경고를 포함하려면 *모두*를 선택합니다. | 
|조건 모니터링 |조건이 더 이상 활성 상태가 아니고, 시스템에서 *실행*되었거나 *해결*된 경우 경고를 필터링할 모니터 조건을 선택합니다. 또는 *모두*를 선택하여 모든 조건의 경고를 포함합니다. | 
|경고 상태 |경고 상태(*신규*, *승인*, *닫힘*)를 선택하거나, 상태에 상관없이 모든 경고를 포함하려면 *모두*를 선택합니다. | 
|서비스 모니터링 |서비스를 선택하거나, 모든 서비스를 포함하려면 *모두*를 선택합니다. 이 기능에는 *VM Insights*의 경고만 지원됩니다.| 
|시간 범위| 선택한 기간 내에 발생한 경고만 보기에 포함됩니다. 지원되는 값은 지난 1시간, 지난 24시간, 지난 7일 및 지난 30일입니다. | 

**경고 세부 정보** 페이지는 경고를 선택하면 표시되며, 경고의 세부 정보가 제공되고 상태를 변경할 수 있습니다. 경고 관리에 대한 자세한 내용은 [Azure Monitor를 사용하여 경고 만들기, 보기 및 관리](../../azure-monitor/platform/alerts-metric.md)를 참조하세요.  

>[!NOTE]
>현재는 상태 조건을 기준으로 새 경고를 생성하거나 포털에서 Azure Monitor의 기존 상태 경고 규칙을 수정할 수 없습니다.  
>

![선택한 경고에 대한 경고 세부 정보 창](./media/vminsights-health/alert-details-pane-01.png)

경고를 선택한 다음, 왼쪽 상단에 있는 **모든 경고** 페이지에서 **상태 변경**을 선택하면 하나 이상의 경고에 대해서도 경고 상태를 변경할 수 있습니다. **경고 상태 변경** 창에서 상태 중 하나를 선택하고 **설명** 필드에 변경 내용에 대한 설명을 추가한 다음, **확인**을 클릭하여 변경 내용을 적용합니다. 정보가 확인되고 변경 내용이 적용되는 동안 메뉴의 **알림**에서 진행 상황을 추적할 수 있습니다.  

### <a name="configure-alerts"></a>경고 구성
특정 경고 관리 작업을 Azure portal에서 관리할 수 없습니다 및 사용 하 여 수행 해야 합니다 [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components)합니다. 구체적으로 살펴보면 다음과 같습니다.

- 상태 조건에 대 한 경고 사용 여부 설정 
- 상태 조건 경고에 대 한 알림 설정 

각 예제에 사용 되는 접근 방식을 사용 하 여 [ARMClient](https://github.com/projectkudu/armclient) Windows 컴퓨터에 있습니다. 이 메서드를 사용 하 여 잘 모르는 경우 [ARMClient를 사용 하 여](../platform/rest-api-walkthrough.md#use-armclient)입니다.  

#### <a name="enable-or-disable-alert-rule"></a>경고 규칙을 사용할지 설정 합니다.

상태 조건 속성이 특정 성능 조건에 대 한 경고를 사용할지 *alertGeneration* 값을 사용 하 여 수정 해야 **사용 안 함** 또는 **Enabled**. 식별 하는 *monitorId* 는 특정 상태 조건을의 다음 예제에서는 조건에 대 한 해당 값에 대해 쿼리 하는 방법을 표시 됩니다 **LogicalDisk\Avg Disk 초 Per Transfer**합니다.

1. 터미널 창에서 **armclient.exe login**을 입력합니다. 이렇게 Azure에 로그인 하 라는 메시지가 표시 됩니다.

2. 특정 가상 컴퓨터에서 활성 상태 조건을 검색 하 고에 대 한 값을 식별 하려면 다음 명령을 입력 *monitorId* 속성입니다. 

    ```
    armclient GET "subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors?api-version=2018-08-31-preview”
    ```

    다음 예제에서는 해당 명령의 출력을 보여줍니다. 값을 기록해 *MonitorId*합니다. 이 값은 다음 단계에 필요한 상태 조건의 ID를 지정 하 고 경고를 만들려면 해당 속성을 수정 해야 할 경우.

    ```
    "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/monitors/ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "name": "ComponentTypeId='LogicalDisk',MonitorId='Microsoft_LogicalDisk_AvgDiskSecPerRead'",
      "type": "Microsoft.WorkloadMonitor/virtualMachines/monitors"
    },
    {
      "properties": {
        "description": "Monitor the performance counter LogicalDisk\\Avg Disk Sec Per Transfer",
        "monitorId": "Microsoft_LogicalDisk_AvgDiskSecPerTransfer",
        "monitorName": "Microsoft.LogicalDisk.AvgDiskSecPerTransfer",
        "monitorDisplayName": "Average Logical Disk Seconds Per Transfer",
        "parentMonitorName": null,
        "parentMonitorDisplayName": null,
        "monitorType": "Unit",
        "monitorCategory": "PerformanceHealth",
        "componentTypeId": "LogicalDisk",
        "componentTypeName": "LogicalDisk",
        "componentTypeDisplayName": "Logical Disk",
        "monitorState": "Enabled",
        "criteria": [
          {
            "healthState": "Warning",
            "comparisonOperator": "GreaterThan",
            "threshold": 0.1
          }
        ],
        "alertGeneration": "Enabled",
        "frequency": 1,
        "lookbackDuration": 17,
        "documentationURL": "https://aka.ms/Ahcs1r",
        "configurable": true,
        "signalType": "Metrics",
        "signalName": "VMHealth_Avg. Logical Disk sec/Transfer"
      },
      "etag": null,
    ```

3. 수정 하려면 다음 명령을 입력 합니다 *alertGeneration* 속성입니다.

    ```
    armclient patch subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/monitors/Microsoft_LogicalDisk_AvgDiskSecPerTransfer?api-version=2018-08-31-preview "{'properties':{'alertGeneration':'Disabled'}}"
    ```   

4. 명령을 입력 하 여 GET 속성의 값으로 설정 되었는지 확인 하려면 2 단계에서 사용한 **사용 안 함**합니다.  

#### <a name="associate-action-group-with-health-criteria"></a>상태 조건을 사용 하 여 작업 그룹에 연결

경고가 생성 되 면 Vm 상태에 대 한 azure Monitor SMS 및 전자 메일 알림을 지 원하는 상태 조건을 비정상 상태가 되는 경우. 에 알림을 구성 하려면 SMS 또는 전자 메일 알림을 보내도록 구성 된 작업 그룹의 이름을 확인 해야 합니다. 

>[!NOTE]
>이 작업에 대 한 알림을 수신 하려는 리소스 그룹의 모든 Vm에 적용 되지 않습니다 모니터링 하는 각 VM에 대해 수행 해야 합니다.  

1. 터미널 창에서 **armclient.exe login**을 입력합니다. 이렇게 Azure에 로그인 하 라는 메시지가 표시 됩니다.

2. 경고 규칙을 사용 하 여 작업 그룹을 연결 하려면 다음 명령을 입력 합니다.
 
    ```
    $payload = "{'properties':{'ActionGroupResourceIds':['/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/microsoft.insights/actionGroups/actiongroupName']}}" 
    armclient PUT https://management.azure.com/subscriptions/subscriptionId/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings/default?api-version=2018-08-31-preview $payload
    ```

3. 속성의 값을 확인 하려면 **actionGroupResourceIds** 를 성공적으로 업데이트 하 고, 다음 명령을 입력 합니다.

    ```
    armclient GET "subscriptions/subscriptionName/resourceGroups/resourcegroupName/providers/Microsoft.Compute/virtualMachines/vmName/providers/Microsoft.WorkloadMonitor/notificationSettings?api-version=2018-08-31-preview"
    ```

    출력은 다음과 유사합니다.
    
    ```
    {
      "value": [
        {
          "properties": {
            "actionGroupResourceIds": [
              "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourceGroups/Lab/providers/microsoft.insights/actionGroups/Lab-IT%20Ops%20Notify"
            ]
          },
          "etag": null,
          "id": "/subscriptions/a7f23fdb-e626-4f95-89aa-3a360a90861e/resourcegroups/Lab/providers/Microsoft.Compute/virtualMachines/SVR01/providers/Microsoft.WorkloadMonitor/notificationSettings/default",
          "name": "notificationSettings/default",
          "type": "Microsoft.WorkloadMonitor/virtualMachines/notificationSettings"
        }
      ],
      "nextLink": null
    }
    ```

## <a name="next-steps"></a>다음 단계

병목 상태 및 VM 성능에 대한 전반적인 사용률을 확인하려면 [Azure VM 성능 보기](vminsights-performance.md)를 참조하세요. 검색된 애플리케이션 종속성을 보려면 [VM용 Azure Monitor 맵 보기](vminsights-maps.md)를 참조하세요. 
