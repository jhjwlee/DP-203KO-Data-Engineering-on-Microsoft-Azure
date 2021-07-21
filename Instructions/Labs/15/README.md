# 모듈 15 - Event Hubs 및 Azure Databricks를 사용하여 스트림 처리 솔루션 만들기

이 모듈에서는 Azure Databricks의 Spark 구조적 스트리밍 및 Event Hubs를 사용하여 스트리밍 데이터를 대규모로 수집 및 처리하는 방법을 알아봅니다. 그리고 구조적 스트리밍의 주요 기능 및 사용 방식을 알아봅니다. 또한 슬라이딩 윈도우를 사용해 데이터 청크를 집계하고, 워터마크를 적용해 부실 데이터를 제거합니다. 그리고 마지막으로 Event Hubs에 연결해 스트림 읽기와 쓰기를 수행합니다.

이 모듈을 완료하면 다음 작업을 수행할 수 있습니다.

- 구조적 스트리밍의 주요 기능 및 사용 방식 확인
- 파일에서 데이터를 스트리밍한 다음 분산 파일 시스템에 기록
- 슬라이딩 윈도우를 사용하여 모든 데이터가 아닌 데이터 청크 집계
- 워터마크를 적용해 부실 데이터 제거
- Event Hubs에 연결해 스트림 읽기 및 쓰기 수행

## 랩 세부 정보

- [모듈 15 - Event Hubs 및 Azure Databricks를 사용하여 스트림 처리 솔루션 만들기](#module-15---create-a-stream-processing-solution-with-event-hubs-and-azure-databricks)
  - [랩 세부 정보](#lab-details)
  - [개념](#concepts)
  - [Event Hubs 및 Spark Structured Streaming](#event-hubs-and-spark-structured-streaming)
  - [스트리밍 개념](#streaming-concepts)
  - [랩](#lab)
    - [실습 랩 시작 전 준비 사항](#before-the-hands-on-lab)
      - [작업 1: Azure Databricks 작업 영역 만들기 및 구성](#task-1-create-and-configure-the-azure-databricks-workspace)
    - [연습 1: Structured Streaming Concepts Notebook 완료](#exercise-1-complete-the-structured-streaming-concepts-notebook)
      - [작업 1: Databricks 보관 파일 복제](#task-1-clone-the-databricks-archive)
      - [작업 2: Notebook 완료](#task-2-complete-the-notebook)
    - [연습 2: Working with Time Windows Notebook 완료](#exercise-2-complete-the-working-with-time-windows-notebook)
    - [연습 3: Structured Streaming with Azure EventHubs Notebook 완료](#exercise-3-complete-the-structured-streaming-with-azure-eventhubs-notebook)

## 개념

Apache Spark Structured Streaming은 빠르고 확장 가능하며 내결함성 스트림 처리 API입니다. 이를 사용하여 스트리밍 데이터에 대한 분석을 거의 실시간으로 수행할 수 있습니다.

Structured Streaming을 사용하면 SQL 쿼리를 통해 정적 데이터를 처리하는 것과 동일한 방식으로 스트리밍 데이터를 처리할 수 있습니다. API는 마지막 데이터를 지속적으로 증분하고 업데이트합니다.

## Event Hubs 및 Spark Structured Streaming

Azure Events Hubs는 수백만 개의 데이터를 단 몇 초 만에 처리하는 확장성 있는 실시간 데이터 수집 서비스입니다. 여러 원본에서 대량의 데이터를 받고 준비된 데이터를 Azure Data Lake 또는 Azure Blob Storage로 스트림할 수 있습니다.

Azure Event Hubs는 Spark Structured Streaming과 통합되어 메시지를 거의 실시간으로 처리할 수 있습니다. Structured Streaming 쿼리 및 Spark SQL을 사용하여 처리된 데이터를 있는 그대로 쿼리하고 분석할 수 있습니다.

## 스트리밍 개념

스트림 처리는 새 데이터를 Data Lake 스토리지에 지속적으로 통합하고 결과를 계산하는 작업입니다. 스트리밍 데이터는 기존 일괄 처리 관련 처리 기법을 사용할 때보다 빠르게 제공됩니다. 데이터 스트림은 지속적으로 데이터를 추가하는 테이블로 취급됩니다. 이러한 데이터의 예로 은행 카드 거래, IoT(사물 인터넷) 디바이스 데이터 및 비디오 게임 재생 이벤트가 있습니다.

스트리밍 시스템을 구성하는 요소는 다음과 같니다.

- 입력 원본(예: Kafka, Azure Event Hubs, IoT Hub, 분산형 시스템의 파일 또는 TCP-IP 소켓)
- 구조적 스트리밍, forEach 싱크, 메모리 싱크 등을 사용하여 스트림 처리

## 랩

이 랩의 연습은 Databricks Notebooks 내에서 완료해야 합니다. 시작하려면 Azure Databricks 작업 영역에 대한 액세스 권한이 있어야 합니다. 사용 가능한 작업 영역이 없으면 아래 지침을 따르세요. 사용 가능한 작업 영역이 있으면 `Databricks 보관 파일 복제` 단계부터 진행하면 됩니다.

### 실습 랩 시작 전 준비 사항

> **참고:** `Before the hands-on lab` 단계는 호스트형 랩 환경이 **아닌**자체 Azure 구독을 사용하는 경우에만 완료하세요. 호스트형 랩 환경을 사용하는 경우에는 연습 1부터 바로 진행하면 됩니다.

이 랩의 연습을 진행하기 전에 사용 가능한 클러스터가 있는 Azure Databricks 작업 영역에 액세스할 수 있는지 확인하세요. 작업 영역을 구성하려면 아래 작업을 수행합니다.

#### 작업 1: Azure Databricks 작업 영역 만들기 및 구성

[랩 15 설정 지침](https://github.com/solliancenet/microsoft-data-engineering-ilt-deploy/blob/main/setup/15/lab-01-setup.md)에 따라 작업 영역을 만들고 구성합니다.

### 연습 1: Structured Streaming Concepts Notebook 완료

#### 작업 1: Databricks 보관 파일 복제

1. 현재 Azure Databricks 작업 영역이 열려 있지 않은 경우 Azure Portal에서 배포된 Azure Databricks 작업 영역으로 이동하고 **작업 영역 시작**을 선택합니다.
1. 왼쪽 창에서 **작업 영역** > **사용자**를 선택하고, 사용자 이름(집 모양 아이콘이 있는 항목)을 선택합니다.
1. 표시되는 창에서 이름 옆에 있는 화살표를 선택한 다음, **가져오기**를 선택합니다.

    ![보관 파일을 가져오는 메뉴 옵션](media/import-archive.png)

1. **Notebook 가져오기** 대화 상자에서 URL을 선택하고, 다음 URL에 붙여넣습니다.

 ```
  https://github.com/solliancenet/microsoft-learning-paths-databricks-notebooks/blob/master/data-engineering/DBC/10-Structured-Streaming.dbc?raw=true
 ```

1. **가져오기**를 선택합니다.
1. 표시되는 **10-Structured-Streaming** 폴더를 선택합니다.

#### 작업 2: Notebook 완료

**1.Structured-Streaming-Concepts** otebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook 내에서 다음을 수행합니다.

- 파일에서 데이터를 스트리밍한 다음 분산 파일 시스템에 기록
- 활성 스트림 나열
- 활성 스트림 중지

Notebook이 완료되면 이 화면으로 돌아와서 다음 단계를 계속 진행하세요.

### 연습 2: Working with Time Windows Notebook 완료

Azure Databricks 작업 영역의 사용자 폴더에서 가져온 **10-Structured-Streaming** 폴더를 엽니다.

**2.Time-Windows** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook 내에서 다음을 수행합니다.

- 슬라이딩 윈도우를 사용하여 모든 데이터가 아닌 데이터 청크 집계
- 보관할 공간이 없는 오래된 이전 데이터를 버리려면 워터마크 처리를 적용합니다.
- `display`를 사용하여 라이브 그래프 그리기

Notebook이 완료되면 이 화면으로 돌아와서 다음 단계를 계속 진행하세요.

### 연습 3: Structured Streaming with Azure EventHubs Notebook 완료

Azure Databricks 작업 영역의 사용자 폴더에서 가져온 **10-Structured-Streaming** 폴더를 엽니다.

**3.Streaming-With-Event-Hubs-Demo** Notebook을 엽니다. 지침에 따라 내부에서 셀을 실행하기 전에 클러스터를 Notebook에 연결했는지 확인합니다.

Notebook 내에서 다음을 수행합니다.

- Event Hubs에 연결하고 이벤트 허브에 스트림 작성
- 이벤트 허브에서 스트림 읽기
- JSON 페이로드의 스키마를 정의하고 데이터를 구문 분석하여 테이블에 표시
