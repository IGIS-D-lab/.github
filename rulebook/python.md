# Style Guide for IGIS Data Lab 
# Python Code

* author -- Sang Il Bae
* version -- 1.0.0
* created -- 2022.06.02
* last edit -- 2022.09.16

## Introduction

<p>

PEP에 의해, 여러 사람들이 파이썬 코드를 짜도, 프로젝트 내의 일관성이 있어서 다른 사람이 코드를 보아도 유지 보수가 가능하게 짜고자 다음 Code Guide를 제작.

하지만 일관성을 지키지 않아야 할 때도 중요. 
- 가이드를 적용했을시, 사람이 읽기 어려울때. 
- 기존 코드 (레거시 코드)들과의 일관성을 맞출 때  - 더더욱 레거시 코드를 잘 짜야 하는 이유

</p>

## Content
#### Line Length

- 한 줄에 최대 79 문자
  - 주석도 줄 당 72 문자 
  - 줄 내 주석(in line comment)를 적을 때는 **스페이스 2 번** 후 작성

```python
# this is wrong
def foo(arg1: int, arg2: int, arg3: int, arg4: str, arg5: set):
    ...

# this is correct
def foo(arg1: int, 
        arg2: int, 
        arg3: int, 
        arg4: str, 
        arg5: set):
    # looks odd but this is correct
    ...
```

- 다음과 같은 경우에 있어서 백슬래시로 코드 줄 나누기 적극 사용

```python
with open('/path/to/some/file/you/want/to/read') as file_1, \
     open('/path/to/some/file/being/written', 'w') as file_2:
    file_2.write(file_1.read())
```

#### Math Operator(연산자)

- 연산자는 다음과 같이 수학적 전통을 따름
```python
income = (gross_wages
          + taxable_interest
          + (dividends - qualified_dividends)  # 괄호는 안쪽에
          - ira_deduction
          - student_loan_interest)
```

#### ln(공백 줄)

- 두 개의 공백 줄
  - 합수 앞 뒤로 두 개의 공백 줄
  - 클래스 정의 앞 뒤로 두 개의 공백 줄
```python
class Foo:
    ...


class Bar:
    ...


def foobar():
    ...
```

- 한 개의 공백 줄
  - 클래스 내 메서드(method)
  - 함수 내 적절하게 구분

#### Import 방법
```python
# correct
from xxx import Yyy, Zzz
```

- 가져오기 순서는 다음과 같이 구분
```python
# 표준 라이브러리
import sys
import os

# 관련 3rd 파티 라이브러리
import requests
import bs4

# 로컬 라이브러리
import IgisOrm
```

#### Comments

- 주석은 코드와 상이한 것이 가장 나쁨. 
- 주석은 완전한 문장으로 쓰여야 함.
- 주석은 **무조건 영어** 사용. **한국어 사용 금지.**
- 첫 글자는 무조건 대문자로 쓰자.
  - 단, 첫 글자가 식별자, 혹은 변수일 경우, 문자 그대로 사용.
- 만약 주석이 여러 문장이면 마침표 뒤에 2칸 공백을 사용하자. 
```python
# this is my comment. this is wrong

# This is my comment.  This is correct, and easy to read.
```

- 주석은 코드와 같은 레벨로 들여쓰기.
- 주석이 여러줄이면 `#` 와 한 칸 공백.
```python
# This is my comment.  This is the correct example.
# Comment must be relevant to the attached code.
def foobar():
    # Write why x is always 1 in this function.
    x = 1
```
- 인라인 주석은 최소화.
- `"""` 사용시
  - 한 줄안에 닫으면 일반 따옴표같이
  - 여러 줄인데 닫으려면, 마지막 줄은 `"""`만 있어야함
  - https://peps.python.org/pep-0257/

**!참고!**

다음은 구글 클라우드 플랫폼 코드 예시이다. 함수에 대한 주석예시를 보고 따르자.
```python
# function without ":param <param>: Explanation
def write_timeseries_value(client, project_resource,
                           custom_metric_type, instance_id, metric_kind):
    """Write the custom metric obtained by get_custom_data_point at a point in
    time."""
    # Specify a new data point for the time series.
    now = get_now()

# function with ":param <param>: Explanation
def read_timeseries(client, project_resource, custom_metric_type):
    """Reads all of the CUSTOM_METRICS that we have written between START_TIME
    and END_TIME
    :param project_resource: Resource of the project to read the timeseries
                             from.
    :param custom_metric_name: The name of the timeseries we want to read.
    """
    request = client.projects().timeSeries().list(
        name=project_resource,
        filter='metric.type="{0}"'.format(custom_metric_type),
        pageSize=3,
        interval_startTime=get_start_time(),
        interval_endTime=get_now(),
    )
    response = request.execute()
    return 
```

#### Naming Scheme(작명 센스)

**!참고!**

다음과 같은 작명 스타일이 존재

- `b`
- `B`
- `lowercase`
- `lowercase_with_underscore`
- `UPPERCASE`
- `UPPERCASE_WITH_UNDERSCORE`
- `CamelCase`
- `mixedCase`


특별한 케이스

- `_single_leading_underscore`
  - 얘는 스크립트 내에서 전역 변수로 사용되지만, import 시에는 private 변수마냥 import 되지 않음
- `single_trailing_underscore_`
  - 얘는 파이썬에 이미 있는 것과 중복되지 않기 위해 뒤에 `_`를 사용.


| Usecase | What to use? |
|---------|--------------|
| 클래스명 | `CamelCase` |
| 타입 변수 | `from typing import TypeVar`로 작성된 `TypeVar`들은, `CamelCase` 사용. `PEP484` |
| 예외(Exception) | `CamelCase` |
| 함수 및 변수 | `lowercase_with_underscore` |
| 퍼블릭(public) 메서드 | `lowercase_with_underscore` |
| 프라이빗(private) 메서드 | `_single_leading_underscore` |
| 상수 | `UPPERCASE_WITH_UNDERSCORE` |


- 네이밍은 기존 프로젝트와 어긋나지 않게 작성.
  - 만약 기존 프로젝트가 한 함수 이름을 `generate_schema()`로 작성하였다면, 다음 함수 이름을 지을 때, `gen_table()` 보단 `generate_table()`을 사용.


#### 상수

<p>

- 전역 상수 변수는 다른 `.py` 파일에 담기. `c.py` 혹은 `constant_.py` 와 같은 파일에 담기.
- 상수 변수는 함수의 계산 결과 등, 변할 수 있는(mutable) 것을 담지 않기

```python
# Constant
MAX_OVERFLOW = 32

# Not constant.
NOT_CONSTANT = 32 + 10
ALSO_NOT_CONSTANT = foo()
```

```python
# c.py
# only stores constant
STO_CONS = 1


# main.py
# import it like so
import c as const

print(const.STO_CONS)
```

</p>

#### 함수 Annotation (Type Hint)
- `PEP484` 는 함수 내 변수 타입 힌트를 소개
  - https://peps.python.org/pep-0484/#generics
  - 제너릭, 타입 힌트 등을 소개



```python
# this is right
def foo(arg1: str):
    pass

def bar(arg1: str) -> str:
    return 'bar'

# this is wrong
def foo(arg1):
    pass

def foo(arg1: str):
    return 'bar'
```


#### 변수 Annotaion. 


- `PEP528` 은 변수 어노테이션을 소개.
  - https://peps.python.org/pep-0008/#variable-annotations
  - 파이썬 3.6 이상부터 지원



