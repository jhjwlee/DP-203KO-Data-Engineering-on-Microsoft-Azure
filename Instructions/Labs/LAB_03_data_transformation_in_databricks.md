---
lab:
    title: 'Azure Databricks에서 데이터 탐색 및 변환'
    module: '모듈 3'
---

# 랩 3 - Azure Databricks에서 데이터 탐색 및 변환

이 랩에서는 다양한 Apache Spark DataFrame 메서드를 사용하여 Azure Databricks에서 데이터를 탐색 및 변환하는 방법을 알아봅니다. 구체적으로는 표준 DataFrame 메서드를 수행하여 데이터를 탐색 및 변환하는 방법을 알아봅니다. 그리고 중복 데이터 제거, 날짜/시간 값 조작, 열 이름 바꾸기, 데이터 집계 등의 고급 작업 수행 방법도 배웁니다.

이 랩을 마치면 다음과 같은 역량을 갖추게 됩니다.

- Azure Databricks에서 DataFrames를 사용하여 데이터 탐색 및 필터링
- 후속 쿼리를 더욱 빠르게 실행할 수 있도록 DataFrame 캐시
- 중복 데이터 제거
- 날짜/시간 값 조작
- DataFrame 열 제거 및 이름 바꾸기
- DataFrame에 저장된 데이터 집계

## 랩 설정 및 필수 구성 요소

이 랩을 시작하기 전에 랩 환경을 만드는 설정 단계를 성공적으로 완료했는지 확인하세요. 또한 랩 1에서 만든 Azure Databricks 클러스터가 필요합니다. 랩 1을 완료하지 않은 경우(또는 클러스터를 삭제한 경우)에 대비하여 클러스터를 만드는 단계가 아래 지침에 포함되어 있습니다.

## 연습 1 - 데이터 프레임 사용

이 연습에서는 몇 가지 Databricks Notebook을 사용하여 데이터 프레임을 사용할 때의 기본 개념과 기법에 대해 알아봅니다.

### 작업 1: Databricks 보관 파일 복제

1. 현재 Azure Databricks 작업 영역이 열려 있지 않은 경우 Azure Portal(<https://portal.azure.com>)에서 배포된 Azure Databricks 작업 영역으로 이동하고 **작업 영역 시작**을 선택합니다.
1. 왼쪽 창에서 **컴퓨팅**을 선택합니다. 기존 클러스터가 있으면 실행 중인지 확인합니다(필요하면 시작해야 함). 기존 클러스터가 없으면 최신 런타임 및 **Scala 2.12** 이상을 사용하는 단일 노드 클러스터를 만듭니다.
1. 클러스터가 실행 중이면 왼쪽 창에서 **작업 영역** > **사용자**를 선택하고, 사용자 이름(집 모양 아이콘이 있는 항목)을 선택합니다.
1. 표시되는 창에서 이름 옆에 있는 화살표를 선택한 다음 **가져오기**를 선택합니다.

    ![보관 파일을 가져오는 메뉴 옵션](images/import-archive.png)

1. **Notebook 가져오기** 대화 상자에서 URL을 선택하고, 다음 URL에 붙여넣습니다.

    ```
    https://github.com/MicrosoftLearning/DP-203-Data-Engineer/raw/master/Allfiles/microsoft-learning-paths-databricks-notebooks/data-engineering/DBC/04-Working-With-Dataframes.dbc
    ```

1. **가져오기**를 선택합니다.
1. 표시되는 **04-Working-With-Dataframes** 폴더를 선택합니다.

### 작업 2: *Describe a DataFrame* Notebook 실행

1. **1.Describe-a-dataframe** Notebook을 엽니다.
1. 지침에 따라 그 안에 들어 있는 셀을 실행하기 전에 클러스터를 Notebook에 연결합니다. Notebook 내에서 다음을 수행합니다.
  - DataFrame API를 숙지합니다.
  - **SparkSession** 및 **DataFrame**(일명 ***Dataset[Row]***) 클래스를 사용하는 방법을 배웁니다.
  - **count** 작업을 사용하는 방법을 배웁니다.

### 작업 3: *Working with DataFrames* Notebook 실행

1. Azure Databricks 작업 영역의 **04-Working-With-Dataframes** 폴더에서 **2.Use-common-dataframe-methods** Notebook을 엽니다.
1. 지침에 따라 그 안에 있는 셀을 실행하기 전에 클러스터를 Notebook에 연결합니다. Notebook 내에서 다음을 수행합니다.

    - DataFrame API를 숙지합니다.
    - 성능에 일반적인 DataFrame 메서드 사용
    - Spark API 설명서 살펴보기

### 작업 4: *Display Function* Notebook 실행

1. Azure Databricks 작업 영역의 **04-Working-With-Dataframes** 폴더에서 **3.Display-function** Notebook을 엽니다.
1. 지침에 따라 그 안에 있는 셀을 실행하기 전에 클러스터를 Notebook에 연결합니다. Notebook 내에서 다음을 수행합니다.

    - 다음 변환을 사용하는 방법을 알아봅니다.
      - limit(..)
      - select(..)
      - drop(..)
      - distinct()`
      - dropDuplicates(..)
    - 다음 작업을 사용하는 방법을 알아봅니다.
      - show(..)
      - display(..)

### 작업 5: *Distinct Articles* exercise Notebook 완료

1. Azure Databricks 작업 영역의 **04-Working-With-Dataframes** 폴더에서 **4.Exercise: Distinct Articles** Notebook을 엽니다.
1. 지침에 따라 그 안에 있는 셀을 실행하기 전에 클러스터를 Notebook에 연결합니다. 이 Notebook에서는 Parquet 파일을 읽고, 필요한 변환을 적용하며, 총 레코드 수를 수행하고, 모든 데이터가 올바르게 로드되었는지 확인합니다. 데이터와 일치하는 스키마를 정의하고 스키마를 사용하도록 읽기 작업을 업데이트할 수 있습니다.

    > 참고: **Solutions** 하위 폴더 내에서 해당하는 Notebook을 찾을 수 있습니다. 여기에는 연습에 사용할 전체 셀이 포함되어 있습니다. 어려운 점이 있거나 단순히 솔루션만 보려는 경우 Notebook을 참조하세요.

## 연습 2 - 데이터 프레임 고급 메서드 사용

이 연습에서는 이전 랩에서 살펴본 Azure Databricks 데이터 프레임 관련 개념을 추가로 살펴봅니다. 구체적으로는 데이터 엔지니어가 데이터 프레임을 사용하여 데이터 읽기, 쓰기, 변환을 수행할 때 사용할 수 있는 몇 가지 고급 메서드를 살펴봅니다.

### 작업 1: Databricks 보관 파일 복제

1. Databricks 작업 영역의 왼쪽 창에서 **작업 영역**을 선택하고 홈 폴더(집 모양 아이콘이 있는 사용자 이름)로 이동합니다.
1. 이름 옆에 있는 화살표를 선택하고 **가져오기**를 선택합니다.
1. **Notebook 가져오기** 대화 상자에서 **URL**을 선택하고, 다음 URL에 붙여넣습니다.

    ```
    https://github.com/MicrosoftLearning/DP-203-Data-Engineer/raw/master/Allfiles/microsoft-learning-paths-databricks-notebooks/data-engineering/DBC/07-Dataframe-Advanced-Methods.dbc
    ```

1. **가져오기**를 선택합니다.
1. 표시되는 **07-Dataframe-Advanced-Methods** 폴더를 선택합니다.

### 작업 2: *Date and Time Manipulation* Notebook 실행

1. Azure Databricks 작업 영역의 **07-Dataframe-Advanced-Methods** 폴더에서 **1.DateTime-Manipulation** Notebook을 엽니다.
1. 지침에 따라 그 안에 있는 셀을 실행하기 전에 클러스터를 Notebook에 연결합니다. 더 많은 **sql.functions** 작업과 날짜 및 시간 함수를 살펴볼 수 있습니다.

### 작업 3: *Use Aggregate Functions* Notebook 실행

1. Azure Databricks 작업 영역의 **07-Dataframe-Advanced-Methods** 폴더에서 **2.Use-Aggregate-Functions** Notebook을 엽니다.

1. 지침에 따라 그 안에 있는 셀을 실행하기 전에 클러스터를 Notebook에 연결합니다. Notebook에서 다양한 집계 함수를 알아봅니다.

### 작업 4: *De-Duping Data* exercise Notebook 실행

1. Azure Databricks 작업 영역의 **07-Dataframe-Advanced-Methods** 폴더에서 **3.Exercise-Deduplication-of-Data** Notebook을 엽니다.

1. 지침에 따라 그 안에 있는 셀을 실행하기 전에 클러스터를 Notebook에 연결합니다. 이 연습의 목표는 나머지 열을 포함하여 DataFrames를 사용하는 방법에 대해 배운 몇 가지를 연습하는 것입니다. 작업을 수행하는 데 사용할 수 있는 빈 셀과 함께 지침이 Notebook에 제공됩니다. Notebook의 하단에는 작업이 정확한지 확인하는 데 도움이 되는 추가 셀이 있습니다.

## 중요: 클러스터 종료

1. 랩을 완료한 후에 왼쪽 창에서 **컴퓨팅**을 선택하고 클러스터를 선택합니다. 그런 다음에 **종료**를 선택하여 클러스터를 종료합니다.
